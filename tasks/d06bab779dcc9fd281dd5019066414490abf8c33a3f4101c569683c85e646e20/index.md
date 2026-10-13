---
blocks:
- block_id: 19bf298486f6b802901dfcd26c0271bd2ff9679104e882ecf3b43c45dda024bd
  indexed: true
  sha256: 1e4e3ee7e10b1e5f8b4c7e169192415fa32b5449955dbb5f9522c9f05f1d753b
  source_range:
    end: 556875
    part: 0
    start: 0
  summary: '只读观察显示，四机服务根进程仍存活且日志初期无错误，HCCL 初始化、checkpoint page-cache 预取和权重加载曾持续推进，但 API 未进入可用阶段。最终记录的首个明确错误是 `RuntimeError: Model does not support EAGLE3 interface but aux_hidden_state_outputs was requested`，发生于 `vllm_ascend/worker/model_runner_v1.py:3736` 的 draft model 加载阶段；TBE `main process disappeared` 与 `IndexError` 被标注为进程退出后的连带噪声。后续只读收尾确认 154 的 PGID 已退出，153/155/156 仍在失败后的 CPU 清理阶段，四机 NPU 已释放且 18077、15569 均无监听；未发送推理请求或操作进程。'
  title: ci-new 分支问题与 EAGLE3 加载失败
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: d06bab779dcc9fd281dd5019066414490abf8c33a3f4101c569683c85e646e20
  kind: experience
  material_digest: 7f67355f64a6e690f64673d86832622b30e73e346975de4d2265e15522593d21
  revision: cfbdd8de66c5063253509e39c4e9033542017208d5210a27c4074dcd70c820de
  schema: mindie-entry/3
  summary: 当前可检索重点是四机启动过程、HCCL/权重加载进度、EAGLE3 接口参数不匹配，以及失败后的进程和端口清理状态。已知结果是 draft model 加载阶段出现明确 RuntimeError，API 未监听；后续 TBE/IndexError 属于连带噪声，不能视为独立根因。该材料是可出错的参考索引，保留了失败过程、只读观察和未验证的因果判断；原始日志路径含有脱敏占位符。
  title: 当前任务导航：ci-new 分支只读故障索引
navigation: 当前可检索重点是四机启动过程、HCCL/权重加载进度、EAGLE3 接口参数不匹配，以及失败后的进程和端口清理状态。已知结果是 draft model 加载阶段出现明确 RuntimeError，API 未监听；后续 TBE/IndexError 属于连带噪声，不能视为独立根因。该材料是可出错的参考索引，保留了失败过程、只读观察和未验证的因果判断；原始日志路径含有脱敏占位符。
schema: mindie-material-task/1
status: complete
task_id: d06bab779dcc9fd281dd5019066414490abf8c33a3f4101c569683c85e646e20
---

# 当前任务导航：ci-new 分支只读故障索引

当前可检索重点是四机启动过程、HCCL/权重加载进度、EAGLE3 接口参数不匹配，以及失败后的进程和端口清理状态。已知结果是 draft model 加载阶段出现明确 RuntimeError，API 未监听；后续 TBE/IndexError 属于连带噪声，不能视为独立根因。该材料是可出错的参考索引，保留了失败过程、只读观察和未验证的因果判断；原始日志路径含有脱敏占位符。

## Materials

- [ci-new 分支问题与 EAGLE3 加载失败](blocks/19bf298486f6b802901dfcd26c0271bd2ff9679104e882ecf3b43c45dda024bd.md): 只读观察显示，四机服务根进程仍存活且日志初期无错误，HCCL 初始化、checkpoint page-cache 预取和权重加载曾持续推进，但 API 未进入可用阶段。最终记录的首个明确错误是 `RuntimeError: Model does not support EAGLE3 interface but aux_hidden_state_outputs was requested`，发生于 `vllm_ascend/worker/model_runner_v1.py:3736` 的 draft model 加载阶段；TBE `main process disappeared` 与 `IndexError` 被标注为进程退出后的连带噪声。后续只读收尾确认 154 的 PGID 已退出，153/155/156 仍在失败后的 CPU 清理阶段，四机 NPU 已释放且 18077、15569 均无监听；未发送推理请求或操作进程。
