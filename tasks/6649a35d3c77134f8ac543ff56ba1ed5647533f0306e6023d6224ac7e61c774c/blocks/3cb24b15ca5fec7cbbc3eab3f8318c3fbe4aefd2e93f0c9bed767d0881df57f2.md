---
block_id: 3cb24b15ca5fec7cbbc3eab3f8318c3fbe4aefd2e93f0c9bed767d0881df57f2
schema: mindie-material-block/1
source_range:
  end: 967523
  part: 0
  start: 0
---

### assistant
Message Type: NEW_TASK
Task name: <redacted:user-path:728fbe175325>
Sender: /root
Payload:


### assistant:commentary
我会只做本地只读审计：先锁定 core pin 与上游 `a0c092e` 的 Kimi K3 文件/提交范围，再整理 modeling 与多模态 preprocess/processor 的可移植项和冲突边界；不改文件、不碰四机。

### assistant:final_answer
只读审计结论：当前上游 `a0c092e` 的 Kimi K3 仍是“部分合入”，不能整体替换 PR12950 的 Ascend modeling。可直接对齐的是多模态标准 `image` 前处理协议；Ascend 文本模型、KDA、旧 GQA DSpark、A3 projector 必须保留。

## 上游提交边界

- `7c6729b`：K3 模型、config、processor、MM preprocess 及大量 NVIDIA/AMD kernel，共 109 个文件、约 2.8 万行。不能整 commit cherry-pick，只能按文件/语义移植。
- `61ac368`：GPU AttnRes kernel，与 Ascend 无关，也违反当前“不重新编译算子”的约束。
- `62d8db7`：补上游 `vllm/models/kimi_k3/__init__.py`，但该入口在非 ROCm 平台默认选择 NVIDIA 实现。
- `2899dca`、`f5a7cce`：Rust/Python frontend，交给 parser/tokenizer 后续阶段处理。

当前 SHA 的接线仍不完整：

- `vllm/model_executor/models/registry.py@a0c092e:132-140` 只有 `KimiLinearForCausalLM`，没有 `KimiK3ForConditionalGeneration`。
- `vllm/transformers_utils/config.py@a0c092e:94-103` 没有 `kimi_k3` config 注册。
- `vllm/models/kimi_k3/__init__.py@a0c092e:12-22` 在 Ascend 上会落入 NVIDIA 分支。
- 因此不能删除 Ascend 现有注册：[models/__init__.py](<redacted:local-path:4dfdc4086e85>:4) 和 [vllm_ascend/__init__.py](<redacted:local-path:1b195fd4d80f>:72)。

## P0：DSpark 完成后的第一批对齐

1. 移植上游标准 `image` 前处理语义，文件级移植，不 cherry-pick 整个 `7c6729b`：

   - `vllm/transformers_utils/processors/kimi_k3.py@a0c092e:9-61`
   - `vllm/models/kimi_k3/common/mm_preprocess.py@a0c092e:89-150,190-330`

   具体包括：

   - `KimiK3Processor(images=...)`，将 PIL 包成 `{"type":"image","image":...}`。
   - `get_supported_mm_limits()` 返回 `{"image": None}`。
   - processor 只预处理/分词，不自行扩展 media pad。
   - `_hf_processor_applies_updates()` 固定返回 `False`。
   - `_get_prompt_updates()` 把 `<|kimi_image_placeholder|>` 替换为带原始 `WxH` 和正确 media-pad 数量的完整 media block。
   - `pixel_values` 继续按 `grid_thws.prod(-1)` 拆分。
   - 从 tokenizer 解析 `<|media_pad|>` ID，和 config 不一致时告警并以 tokenizer 为准。
   - dummy image 尺寸改为基于 `media_proc_cfg` 计算，不再硬编码 `3000×3000`。

2. 同一 changeset 修改 Ascend outer wrapper：

   - `get_placeholder_str("image")` 返回 `<|kimi_image_placeholder|>`。
   - `_mark_tower_model(..., "image")`。
   - MM field modality 从 `vision_chunk` 改成 `image`。
   - 当前相关实现位于 [kimi_k3.py](<redacted:local-path:cc16d21ad675>:150)。

3. 必须与 renderer 同步切换。当前 renderer 仍主动生成完整 media block：

   - [patch_kimi_k3_renderer.py](<redacted:local-path:88c394f0f77c>:53)
   - `:232-248`

   若只换 processor、不换 renderer，会出现 placeholder 协议错配或重复展开。

4. 冻结以下 Ascend 逻辑，不随 upstream modeling 替换：

   - 旧 GQA DSpark materialized aux capture：[kimi_k3.py](<redacted:local-path:cc16d21ad675>:932)
   - `set_dspark_aux_capture_materialized`：同文件 `:428-429,1104-1105`
   - Ascend KDA state shape/copy、无 RoPE、packed W4A8/ModelSlim loader。
   - A3 projector 的 `rot_proj`：同文件 `:1470-1519`。

## P1：选择性 modeling/config 对齐

- 将上游 top-level config 的以下语义移入本地 config：

  - 显式 `image_placeholder`。
  - `vision_config.text_hidden_size` 强制匹配 `text_config.hidden_size`，见上游 `kimi_k3.py:113-129`。
  - 补齐 `video_attn_type`、`_attn_implementation` 字段。
  - 保留本地 `KimiK3TextConfig` 或给 Ascend访问加兼容默认值。当前 GQA config 不包含 `mla_use_rope`，而 Ascend 在 [kimi_k3.py](<redacted:local-path:cc16d21ad675>:630) 直接读取它；直接换成上游 `KimiLinearConfig` 会有 `AttributeError` 风险。

- 可加入上游 outer mapper 的安全映射：

  - `"language_model.layers." -> "language_model.model.layers."`
  - 上游证据：`nvidia/model.py@a0c092e:1455-1461`。

- vision DP 选择可对齐为 `is_vit_use_data_parallel(config.vision_config.num_attention_heads)`，让 TP16/12-head 自动 fallback；但需复测现有 `mm_encoder_tp_mode=data` 行为。

- 增加缺失的 K3 MM 专项测试：

  - 多图、不同分辨率和不同 pad 数。
  - config/tokenizer media ID 不一致。
  - 无残留/重复 placeholder。
  - embedding 输出最后一维必须是 7168。
  - A3 `rot_proj` 和 text-only/DSpark 输出不变。

## P2：暂不对齐

- 不采用上游 `KimiK25MultiModalProjector`。当前上游存在明确维度冲突：

  - GQA config：`mm_hidden_size=1024`、`text_hidden_size=7168`，见 [config.json](<redacted:local-path:c695d54aaa76>:284) 和同文件 `:294`。
  - 上游 K2.5 projector 输出 `config.mm_hidden_size`，见 `kimi_k25_vit.py@a0c092e:738-777`，即会输出 1024，而语言模型需要 7168。
  - Ascend projector 当前正确执行 `4096→4096→7168→RMSNorm→A3 rot_proj`。

- 不采用上游 `nvidia/dspark_mla.py`。该文件明确是 dense MLA drafter，并在 attention 中 `use_rope=True`（`:3,48-76`），不是当前旧 GQA DSpark 权重。

- 不移植 `SupportsEncoderCudaGraph`、FA4 warmup、CUDA/ROCm kernel、MegaMoE/sequence-parallel 实现。

- 等上游补齐 K3 model/config registry，并提供可扩展的非 NVIDIA/ROCm platform 入口后，再考虑删除 Ascend 重复注册。

因此后续最安全的执行顺序是：先把当前 GQA DSpark 接收率修正常并冻结基线；随后一次性对齐 standard-image processor + MM preprocess + renderer placeholder；最后再处理 parser/tokenizer。全程无需重新编译算子。



