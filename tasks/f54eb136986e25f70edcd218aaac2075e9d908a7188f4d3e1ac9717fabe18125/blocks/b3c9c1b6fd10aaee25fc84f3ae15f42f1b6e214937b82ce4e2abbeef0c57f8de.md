---
block_id: b3c9c1b6fd10aaee25fc84f3ae15f42f1b6e214937b82ce4e2abbeef0c57f8de
schema: mindie-material-block/1
source_range:
  end: 1418027
  part: 0
  start: 0
---

### assistant
Message Type: NEW_TASK
Task name: <redacted:user-path:d79f2b6c8a55>
Sender: /root
Payload:


### assistant:commentary
我会专门做一轮“防御性编程/过度兼容”审计：只读对比当前主线，区分应该删除的补丁式 fallback 与确实需要保留的运行时边界检查。

### assistant
Message Type: MESSAGE
Task name: <redacted:user-path:d79f2b6c8a55>
Sender: /root
Payload:


### assistant:final_answer
审计结论：当前分支还不适合直接发 PR。问题不只是代码风格，其中两个全局猴子补丁已和目标 vLLM main API 漂移，会直接触发 `TypeError`。本次仅审计，没有修改文件。

### P0：必须先移除的猴子补丁

- `vllm_ascend/patch/platform/patch_kimi_k3_parsers.py:452-534`
  - 全局包装 `chat_completion_full_generator`，并在单次请求中临时改写 `parser.parse`。
  - 当前包装签名缺少 `mm_token_counts`；目标 vLLM 在 `chat_completion/serving.py:391-400` 已按关键字传入。
  - 非 K3 passthrough 同样没有转发该参数，因此会影响所有 Chat Completions。
  - 必须改成 parser 自身持有 token-id 上下文的正式接口，不能继续给 wrapper 增加 `*args/**kwargs`。

- `patch_kimi_k3_parsers.py:545-603`
  - 解析并重写已经序列化的 SSE、拦截全局 Responses API，均属于服务层猴子补丁。
  - `557-560` 对内部生成的非法 JSON 静默返回，隐藏服务端错误。
  - K3 不支持 Responses 应由模型/renderer 能力校验明确拒绝，不应改写所有模型的 `_create_responses`。

- `patch_kimi_k3_parsers.py:606-666`
  - 动态保存原函数并改写 `OpenAIServingChat`、`OpenAIServingResponses`、`ParserManager`。
  - `640-666` 的 `get_parser` 缺少目标 vLLM 当前必需的 `is_harmony=False` 参数；`OnlineRenderer` 总会传入它。
  - 应删除整段 class mutation，使用公开 registry/组合 parser 接口；若接口不足，应形成明确的上游扩展点，不能继续 monkeypatch 私有方法。

- `patch_kimi_k3_parsers.py:669-680`
  - registry 本身可以保留，但 `if name not in list_registered()` 的防重保护应删除。Python 模块导入已经幂等，名字冲突应直接暴露，而不是被吞掉。

- `vllm_ascend/patch/platform/patch_kimi_k3_renderer.py:398-448`
  - 全局改写 `OnlineRenderer.render_chat` 和私有 `_effective_chat_template_kwargs`。
  - 与 `118-123、215-218` 的动态 `_PREPARED_ATTR`、`object.__setattr__` 是同一套重复调用补丁。
  - 应让 K3 renderer 在一个明确生命周期点完成 typed request 转换，不修改请求对象，不依赖私有方法。

### 可以直接去掉的防御性兼容

- `vllm_ascend/utils.py:733-735`
  - `REGISTERED_ASCEND_OPS` 建表后再动态赋值，正是用户指出的补丁式注册。
  - 把 K3 op 作为普通条目放进同一个字典字面量；无需运行时二次 mutation。

- `patch_kimi_k3_renderer.py:87-112`、`patch_kimi_k3_parsers.py:106-146`
  - dict/Pydantic 双路、`hasattr(model_dump)`、多层 `getattr`、tensor/list/dict tokenizer 返回值兼容。
  - 当前 request/tokenizer 类型是已知的，应直接使用 typed 字段和 `model_dump()`；错误类型直接失败。

- `patch_kimi_k3_renderer.py:271-314`
  - `278-294` 静默 `pop` 十多个参数后强行覆写。
  - renderer 内部参数不应先进入 kwargs；用户显式传入但不支持的参数应返回验证错误，不能假装接受。

- `vllm_ascend/transformers_utils/processors/kimi_k3.py:37-44`
  - `**kwargs` 后直接 `del kwargs`，会静默忽略 processor 参数。改成明确签名、转发支持项或拒绝未知项。

- `vllm_ascend/models/kimi_k3_text.py:501-504`
  - 构造阶段已知是 MoE 还是 MLP，无需 forward 中 `hasattr`。保存一个明确的模块字段即可。

- `vllm_ascend/ops/fused_moe/fused_moe.py:307-312`
  - 基类保证 `routed_output_transform` 存在，直接访问，不需要带默认值的 `getattr`。

- `vllm_ascend/models/kimi_k3_text.py:54-55`
  - `beta or 1.0` 会把显式非法的 `0` 静默变成默认值。只对 `None` 使用默认值。

- `vllm_ascend/quantization/compressed_tensors_config.py:383-400`
  - `weight_quant` 注解和调用契约均为非空，`390` 的 `weight_quant is None` 是死防御分支。

- `vllm_ascend/utils.py:862-889`
  - `enable_shared_expert_dp` 参数已成为无声 no-op，且当前没有调用者传入它。删除参数并更新接口，不要保留伪兼容。

- `vllm_ascend/utils.py:974-996`
  - 参数已经是 `VllmConfig`，无需用 `getattr` 防御 `model_config` 和标准字段缺失。只保留真正可选的 `multimodal_config is None` 分支。

### 必须做结构化重构，不能只删判断

- `vllm_ascend/models/kimi_k3_text.py:307-340`
- `vllm_ascend/ops/mla.py:127-129`
- `vllm_ascend/attention/mla_v1.py:734-741`

上述代码给上游 `MLAModules` 动态挂载 `g_proj/use_output_gate/use_mla_rope`，再由下游用默认值 `getattr` 读取，是典型的无类型隐式通道。应定义 Ascend/K3 自有的 typed 配置或专用 MLA wrapper，并显式传参。

同时：

- `attention/mla_v1.py:758-762` 遇到 no-RoPE 与 FA quant/MLAPO 冲突时静默关闭功能。应在初始化时明确拒绝不支持的配置。
- `attention/mla_v1.py:265` 的 `mla_use_rope` 默认回退也应由配置类型本身拥有，而不是通用路径猜测。

其他结构问题：

- `vllm_ascend/models/kimi_k3.py:53-70`
  - 任意一个 meta tensor 就跳过整个模块迁移，可能留下混合设备模块并掩盖加载生命周期错误。应使用 vLLM 的 device/materialization 构造机制，不能按 tensor 扫描后静默退出。

- `vllm_ascend/models/kimi_k3_vit.py:236-287、305-317`
  - metadata 被声明成所有值都可空，随后每次 forward 重复构造和判空；实际上只有 `sequence_lengths` 可选。改为 typed metadata，并指定唯一构造所有者。
  - `merge_type` 是不可变配置，`316-317` 应在 `__init__` 验证，而不是每次 forward。

- `vllm_ascend/ops/kimi_kda.py:85-94、312`
  - 每次 prefill 用 `hasattr(torch.ops...)` 检查必需 schema。应在扩展注册/启动时验证一次，或让缺失 op 自然失败。
  - `317-331` 的 metadata 长度一致性应移至 metadata builder；运行时只保留 graph padding 的 `keep_meta` 压缩行为。

- `vllm_ascend/ops/fused_moe/moe_mlp.py:84-97`
  - SiTU 路径同时接受 Tensor 和单元素 list，但该路径本身不支持产生 TensorList 的动态 EPLB。应固定为 Tensor contract，仅在 grouped-matmul ABI 边界转成 list。

- `vllm_ascend/ops/gdn_attn_builder.py:145-160`、`distributed/kv_transfer/kv_p2p/mooncake_connector.py:1101-1113`
  - 两处分别通过 `getattr/hasattr` 猜测 K3 或旧 GDN 配置。应抽出共享的 typed linear-attention geometry helper，未知配置显式报错。

- `vllm_ascend/quantization/modelslim_config.py:707-716`
  - 已确认是 K3 config 后仍用可空 `getattr/isinstance`，失败时可能继续处理已被替换掉的 `g_a_proj/g_b_proj`。直接使用 K3 配置契约。

### XTML 中应删除的 fail-soft

- `vllm_ascend/patch/platform/kimi_k3_xtml.py:102-147`
  - `112-139` 在 byte decoder 不存在时退回逐 token 文本 decode，可能破坏跨 token UTF-8。
  - `errors="replace"` 会静默篡改协议内容。
  - K3 parser 应在初始化时要求 byte decoder，使用严格 UTF-8，并把解码错误转换为明确的 `KimiK3XTMLParseError`。

- `kimi_k3_xtml.py:312-325、813-825`
  - tokenizer 被声明为可选，但实际 feed 又要求原始 token IDs。应将 tokenizer/decoder 设为必需契约，删掉互相矛盾的可选分支。

- `kimi_k3_xtml.py:288-305`
  - 合并同 index tool call 时静默沿用旧 name。重复 index 的 name/状态不一致应报协议错误。

### 测试中必须移除的“跳过即通过”

这些是新增必需算子的目标 NPU 测试；进入对应 job 后，schema 缺失就是回归，不能 skip：

- `tests/e2e/nightly/single_node/ops/singlecard_ops/test_dequant_situ_quant.py:102-103、131-132、154-155、175-176`
- `tests/e2e/nightly/single_node/ops/singlecard_ops/test_kimi_k3_situ_fusion_cases.py:107-108、139-140、178-179、204-205`
- `tests/e2e/nightly/single_node/ops/singlecard_ops/test_kimi_kda_ascendc_npu.py:43-44`

另有：

- `test_kimi_k3_situ_fusion_cases.py:156-160` 捕获所有 `Exception` 后假装不是 A5，会把驱动、运行时和 API 故障变成 skip。仅能使用标准 SoC fixture/marker，或捕获明确的“无设备”异常。

外部真实权重、真实 tokenizer 路径不存在时的 integration skip 可以保留，它们属于可选外部 fixture。

### 必须保留的真实边界校验

这些不是“防御性垃圾”：

- `vllm_ascend/ops/kimi_kda.py:38-82`
  - checkpoint 的 rank、head 数、TP shard 范围检查。权重 ABI 应在 loader 边界严格验证。

- `vllm_ascend/patch/platform/kimi_k3_xtml.py:233-243、255-278、397-425`
  - JSON、重复 key、非有限数、参数类型、tool 名和 call index 校验。输入来自不可信模型输出，必须严格保留。

- `vllm_ascend/models/kimi_k3.py:299-329`
  - multimodal request 的 tensor 类型和 `grid_thws` 形状校验。可以集中一次，但不能删除。

- `vllm_ascend/models/kimi_k3_vit.py:34-43、117-123、331-332`
  - patch size、head divisibility、norm/projector 类型属于构造期配置契约，应保留。

- `vllm_ascend/ops/kimi_kda_state.py:22-25`
  - cache shape 参数的正值/非负值检查属于公共构造边界。

- `vllm_ascend/ops/kimi_kda.py:396-401`
  - `attn_metadata is None` 是 profile-run 语义，后续断言用于类型收窄，与上游 GDN 写法一致，可保留。

- `distributed/kv_transfer/kv_p2p/mooncake_connector.py:2274-2279`
  - 2 MiB 对齐是 DMA/注册 ABI，校验必须保留；但应使用显式异常替代可被 `python -O` 移除的 `assert`。`2225-2253` 的启发式 base 恢复则应最终改为 allocator 显式保存原始 base。

补充：新增 Python 中没有发现 `inspect.signature`、vLLM 版本分支或 `ImportError` 兼容分支。主要问题集中在全局 monkeypatch、duck typing、静默降级和热路径重复校验。



