---
blocks:
- block_id: a1c3625e4e6b0fe7677c69ebb24dca5cfdf92d2d9f8147718b30f4951b87093d
  indexed: true
  sha256: 01aa58b028f4c97929a8fab688ee334d7a08eb1b4d8ff1fa772d5e3912f79802
  source_range:
    end: 5814188
    part: 0
    start: 0
  summary: '在 ci-new 叠加 PR #49、FlashComm、共享专家 DP 和多流的 TP8 五层复现中，加载期一致性校验构造了 `[10,3584]` 输入，却调用了 K 维为 7168 的共享专家 W8A8 `gate_up_proj`，CANN 报 `x1 is 3584, x2 is 7168`。256 并发自定义算子及主扩展后来编译成功；随后按用户意见只将两个 fused_moe 实现改为使用共享专家输入宽度并记录 routed/shared 尺寸，未加正整数检查或 UT。单机目标态曾完成图模式生成，但后续四机验证还暴露了 PR #49 的 Dynamo `get_device_properties` 问题，相关硬件结论需结合后续撤回记录理解。'
  title: 单机定位共享专家多流维度错误
- block_id: a32c3165dab998651d342b9127c94e9d904438ea86e7e9c1886b526de81f985a
  indexed: true
  sha256: 57acc1e5696fa22b844fb10bb1c0c99094b704e3c44edd84b254ecf4df34dc37
  source_range:
    end: 5814188
    part: 1
    start: 0
  summary: '153–156 的资源协调经历了从不清理 154 上 `/0day` 到获授权接管；曾精确终止孤立 PGID `921213` 并确认四机空闲。四机 OFF 基线完成 352 个权重分片加载，但在 ACLGraph/torch.compile 路径触发 PR #49 的 Dynamo 动态 `get_device_properties` 追踪错误；随后采用缓存的 `get_vectorcore_num()` 做最小图兼容修正。用户先要求四机只跑最终态，之后又恢复严格 OFF/ON 性能 A/B。'
  title: 四机资源接管与首次完整权重验证
- block_id: 84a68dfbb3dee7ad717d19154e6fcef97a0c50f79a533a63ab22b1c63709d444
  indexed: true
  sha256: 2812f5fc26a3b889f769d92486fb2b7d162b509d2a0d5a8ea61f9232aea324c3
  source_range:
    end: 5814188
    part: 2
    start: 0
  summary: 性能口径从精确 128K+1K 调整为复用同一约 128K 请求、每侧两次并取平均；记录的 OFF 平均 TPOT 为 111.721 ms，ON 平均为 103.030 ms，表面上低约 7.78%，但后续功能可靠性问题使该结论不应视为最终验证成功。ON 的 GPQA 单题长思考请求持续生成，用户要求不要终止；补发的 `2+2` 请求带 `chat_template_kwargs.thinking=false` 后返回 64 个连续 `!`，并记录 `reasoning=null`、`finish_reason=length`，因此功能通过判断被撤回。
  title: 128K+1K 性能 A/B 与 GPQA/短请求异常
- block_id: a10ae6224b4fe08850c1cc8f771d4d65973215fafa7cfb911c960bd23bf460b7
  indexed: true
  sha256: 7e334231089d7078fd70c74bb2057fcaa5ecde0dfef28b524f9cd5e21468b5fd
  source_range:
    end: 5814188
    part: 3
    start: 0
  summary: '用户要求回退 Triton PR #49 并把机器交给 DSpark；四机服务被精确停止，记录称 153–156 服务 PGID 清空、8077/14569 无监听且 NPU 为空。交接要求明确不叠加共享专家多流，验证前先 curl `2+2`，必须无乱码、`None` 或连续 `!`；异常短请求因此未被认定为成功。最终分支基于 ci-new `8f192c9fe`，提交 `1f94d22b5` 仅修改两个 `fused_moe` 文件，未加 UT，静态检查通过，并创建 Draft PR #51；GitHub connector 创建时曾报传输错误，后改用已认证 gh 完成创建。'
  title: '回退 PR #49、资源交接与 Draft PR'
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 104a8109fdae6a37b05b6c3d8decf0cc754368f57e36b2179f8bef88d9addbe6
  kind: experience
  material_digest: 7508122da5c0504ca5d8f928ef49882b9a21facd12160e11a05f9b123ab6690f
  revision: 450e0965f4831c4675234fe97f75f11d06cde33fc8d423df1993e322b87e0bfa
  schema: mindie-entry/3
  summary: '主线是 K3 ci-new 中共享专家多流加载期维度校验错误：路由宽度 3584、共享专家宽度 7168；修复仅保留两个 fused_moe 文件的输入宽度调整和日志，未加入 UT。四机性能 A/B 曾得到 ON TPOT 平均值低于 OFF，但后续 `2+2` 短请求返回连续 `!`、`reasoning=null` 且 `finish_reason=length`，因此功能与收益不能视为最终确认；Triton PR #49 已回退。当前交接状态为不让 DSpark 叠加共享专家多流，并先做无乱码/无 None/无连续感叹号的 curl 检查；Draft PR #51 对应提交 `1f94d22b5`，后续换机验证仍未完成。'
  title: 当前任务导航：共享专家多流修复与验证
navigation: '主线是 K3 ci-new 中共享专家多流加载期维度校验错误：路由宽度 3584、共享专家宽度 7168；修复仅保留两个 fused_moe 文件的输入宽度调整和日志，未加入 UT。四机性能 A/B 曾得到 ON TPOT 平均值低于 OFF，但后续 `2+2` 短请求返回连续 `!`、`reasoning=null` 且 `finish_reason=length`，因此功能与收益不能视为最终确认；Triton PR #49 已回退。当前交接状态为不让 DSpark 叠加共享专家多流，并先做无乱码/无 None/无连续感叹号的 curl 检查；Draft PR #51 对应提交 `1f94d22b5`，后续换机验证仍未完成。'
schema: mindie-material-task/1
status: complete
task_id: 104a8109fdae6a37b05b6c3d8decf0cc754368f57e36b2179f8bef88d9addbe6
---

# 当前任务导航：共享专家多流修复与验证

主线是 K3 ci-new 中共享专家多流加载期维度校验错误：路由宽度 3584、共享专家宽度 7168；修复仅保留两个 fused_moe 文件的输入宽度调整和日志，未加入 UT。四机性能 A/B 曾得到 ON TPOT 平均值低于 OFF，但后续 `2+2` 短请求返回连续 `!`、`reasoning=null` 且 `finish_reason=length`，因此功能与收益不能视为最终确认；Triton PR #49 已回退。当前交接状态为不让 DSpark 叠加共享专家多流，并先做无乱码/无 None/无连续感叹号的 curl 检查；Draft PR #51 对应提交 `1f94d22b5`，后续换机验证仍未完成。

## Materials

- [单机定位共享专家多流维度错误](blocks/a1c3625e4e6b0fe7677c69ebb24dca5cfdf92d2d9f8147718b30f4951b87093d.md): 在 ci-new 叠加 PR #49、FlashComm、共享专家 DP 和多流的 TP8 五层复现中，加载期一致性校验构造了 `[10,3584]` 输入，却调用了 K 维为 7168 的共享专家 W8A8 `gate_up_proj`，CANN 报 `x1 is 3584, x2 is 7168`。256 并发自定义算子及主扩展后来编译成功；随后按用户意见只将两个 fused_moe 实现改为使用共享专家输入宽度并记录 routed/shared 尺寸，未加正整数检查或 UT。单机目标态曾完成图模式生成，但后续四机验证还暴露了 PR #49 的 Dynamo `get_device_properties` 问题，相关硬件结论需结合后续撤回记录理解。
- [四机资源接管与首次完整权重验证](blocks/a32c3165dab998651d342b9127c94e9d904438ea86e7e9c1886b526de81f985a.md): 153–156 的资源协调经历了从不清理 154 上 `/0day` 到获授权接管；曾精确终止孤立 PGID `921213` 并确认四机空闲。四机 OFF 基线完成 352 个权重分片加载，但在 ACLGraph/torch.compile 路径触发 PR #49 的 Dynamo 动态 `get_device_properties` 追踪错误；随后采用缓存的 `get_vectorcore_num()` 做最小图兼容修正。用户先要求四机只跑最终态，之后又恢复严格 OFF/ON 性能 A/B。
- [128K+1K 性能 A/B 与 GPQA/短请求异常](blocks/84a68dfbb3dee7ad717d19154e6fcef97a0c50f79a533a63ab22b1c63709d444.md): 性能口径从精确 128K+1K 调整为复用同一约 128K 请求、每侧两次并取平均；记录的 OFF 平均 TPOT 为 111.721 ms，ON 平均为 103.030 ms，表面上低约 7.78%，但后续功能可靠性问题使该结论不应视为最终验证成功。ON 的 GPQA 单题长思考请求持续生成，用户要求不要终止；补发的 `2+2` 请求带 `chat_template_kwargs.thinking=false` 后返回 64 个连续 `!`，并记录 `reasoning=null`、`finish_reason=length`，因此功能通过判断被撤回。
- [回退 PR #49、资源交接与 Draft PR](blocks/a10ae6224b4fe08850c1cc8f771d4d65973215fafa7cfb911c960bd23bf460b7.md): 用户要求回退 Triton PR #49 并把机器交给 DSpark；四机服务被精确停止，记录称 153–156 服务 PGID 清空、8077/14569 无监听且 NPU 为空。交接要求明确不叠加共享专家多流，验证前先 curl `2+2`，必须无乱码、`None` 或连续 `!`；异常短请求因此未被认定为成功。最终分支基于 ci-new `8f192c9fe`，提交 `1f94d22b5` 仅修改两个 `fused_moe` 文件，未加 UT，静态检查通过，并创建 Draft PR #51；GitHub connector 创建时曾报传输错误，后改用已认证 gh 完成创建。
