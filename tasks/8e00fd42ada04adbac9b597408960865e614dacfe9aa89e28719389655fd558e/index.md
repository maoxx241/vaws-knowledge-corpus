---
blocks:
- block_id: 0e896dc971f3d02527ff54e65cebce4177e37056b3154091340c36626467ce89
  indexed: true
  sha256: e18a7a895ae1f848406211851fdfd027a3a198cc15068358ddac879660e87450
  source_range:
    end: 701394
    part: 0
    start: 0
  summary: '围绕 A5 上 `moe_mlp.py` 第 305 行因非空 `dynamic_scale` 拦截的问题，材料判断该值可能只是 MC2 dispatcher 的占位输出，并将 W4A16 MXFP4 SiTU forward 改为直接进入 `GMM1 → SiTU → GMM2`，移除 `dynamic_scale`、bias 和 offset 的冗余校验；同时补充非空 dispatch scale 的回归覆盖。修改已提交并推送至 PR #4，提交为 `65d8609b fix: remove redundant MXFP4 SiTU validation`，但本机缺少 `torch`/`ruff`，项目单测和 lint 未执行，只有语法编译与 `git diff --check` 通过，未进行模型或 NPU 端到端验证。原生权重仍为 Git LFS 指针，静态识别和加载路径被认为可通，但实际权重运行结果尚未确认。'
  title: 'PR #4 MXFP4 SiTU 冗余校验移除与推送'
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 8e00fd42ada04adbac9b597408960865e614dacfe9aa89e28719389655fd558e
  kind: experience
  material_digest: 12f56a7525855c6b40b5c68fb28aacb70c2fe2f53b22642f20a9fe78f8b2df27
  revision: 138db494c420756da745b0ae44595950914dfbb9d3ea4190c6a1325a7805db41
  schema: mindie-entry/3
  summary: 已完成并推送移除 W4A16 MXFP4 SiTU forward 冗余校验的最小修改及回归用例；提交为 `65d8609b`。仍需注意项目单测/lint 因缺少 `torch`/`ruff` 未执行，原生权重和 A5 端到端运行也未被验证，PR 检查状态此前为排队中。
  title: '当前任务导航：PR #4 MXFP4 SiTU 修复'
navigation: 已完成并推送移除 W4A16 MXFP4 SiTU forward 冗余校验的最小修改及回归用例；提交为 `65d8609b`。仍需注意项目单测/lint 因缺少 `torch`/`ruff` 未执行，原生权重和 A5 端到端运行也未被验证，PR 检查状态此前为排队中。
schema: mindie-material-task/1
status: complete
task_id: 8e00fd42ada04adbac9b597408960865e614dacfe9aa89e28719389655fd558e
---

# 当前任务导航：PR #4 MXFP4 SiTU 修复

已完成并推送移除 W4A16 MXFP4 SiTU forward 冗余校验的最小修改及回归用例；提交为 `65d8609b`。仍需注意项目单测/lint 因缺少 `torch`/`ruff` 未执行，原生权重和 A5 端到端运行也未被验证，PR 检查状态此前为排队中。

## Materials

- [PR #4 MXFP4 SiTU 冗余校验移除与推送](blocks/0e896dc971f3d02527ff54e65cebce4177e37056b3154091340c36626467ce89.md): 围绕 A5 上 `moe_mlp.py` 第 305 行因非空 `dynamic_scale` 拦截的问题，材料判断该值可能只是 MC2 dispatcher 的占位输出，并将 W4A16 MXFP4 SiTU forward 改为直接进入 `GMM1 → SiTU → GMM2`，移除 `dynamic_scale`、bias 和 offset 的冗余校验；同时补充非空 dispatch scale 的回归覆盖。修改已提交并推送至 PR #4，提交为 `65d8609b fix: remove redundant MXFP4 SiTU validation`，但本机缺少 `torch`/`ruff`，项目单测和 lint 未执行，只有语法编译与 `git diff --check` 通过，未进行模型或 NPU 端到端验证。原生权重仍为 Git LFS 指针，静态识别和加载路径被认为可通，但实际权重运行结果尚未确认。
