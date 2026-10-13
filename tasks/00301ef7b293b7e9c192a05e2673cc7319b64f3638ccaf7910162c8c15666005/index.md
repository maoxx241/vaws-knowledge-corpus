---
blocks:
- block_id: bc2cbb483079bc39af05ca8213c3ca506349b4ed0cd3f589cd18061be97a9026
  indexed: true
  sha256: 2070e16419f48c4cf8efdcabb2ea948611cdc9fd970a18256515184a1995626e
  source_range:
    end: 3532179
    part: 0
    start: 0
  summary: 该批记录了 four-plane 候选审查、结构误命中风险及后续闭合修正，并完成插件侧 DFX recorder、core/plugin 钩子和默认关闭逻辑；期间发现本地 vLLM/Transformers 不匹配、无 NPU 的 PrivateUse1 用例被 deselect，以及一次 `__new__` runner 缺少实例 gate 的 AttributeError 回归，均有对应处理或保留说明。最终冻结两个独立 commit：Plugin `2afd8f23...`、Core `550c0d1b...`；Plugin 隔离测试报告为 `42 passed, 1 deselected`，Ruff、format、pycompile、diff-check 通过，但四机 stage/readback/PREFLIGHT 子任务仍由 root 独立跟进，不能据此宣称完整发布成功。
  title: 四机 DFX 插件与 Core 候选冻结及发布门禁
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 00301ef7b293b7e9c192a05e2673cc7319b64f3638ccaf7910162c8c15666005
  kind: experience
  material_digest: 301583e863894e7a5780a52edcfa357dec330c68ae97f3284b43e4abe3671962
  revision: e79ea59ec46af597e211b7a3cf3811f846ecba6c77feeaba8d1c46587668273b
  schema: mindie-entry/3
  summary: 已索引本批唯一材料块，涵盖 four-plane 结构闭合、DFX 采点协议、默认关闭与性能扰动门禁、CPU 隔离测试及两个冻结 commit。后续检索应特别关注最终发布仍属候选状态、四机 PREFLIGHT/readback 未在本块中完成，以及已记录的环境限制、deselected 用例和历史阻断风险。
  title: 当前任务导航：四-plane DFX 发布候选索引
navigation: 已索引本批唯一材料块，涵盖 four-plane 结构闭合、DFX 采点协议、默认关闭与性能扰动门禁、CPU 隔离测试及两个冻结 commit。后续检索应特别关注最终发布仍属候选状态、四机 PREFLIGHT/readback 未在本块中完成，以及已记录的环境限制、deselected 用例和历史阻断风险。
schema: mindie-material-task/1
status: complete
task_id: 00301ef7b293b7e9c192a05e2673cc7319b64f3638ccaf7910162c8c15666005
---

# 当前任务导航：四-plane DFX 发布候选索引

已索引本批唯一材料块，涵盖 four-plane 结构闭合、DFX 采点协议、默认关闭与性能扰动门禁、CPU 隔离测试及两个冻结 commit。后续检索应特别关注最终发布仍属候选状态、四机 PREFLIGHT/readback 未在本块中完成，以及已记录的环境限制、deselected 用例和历史阻断风险。

## Materials

- [四机 DFX 插件与 Core 候选冻结及发布门禁](blocks/bc2cbb483079bc39af05ca8213c3ca506349b4ed0cd3f589cd18061be97a9026.md): 该批记录了 four-plane 候选审查、结构误命中风险及后续闭合修正，并完成插件侧 DFX recorder、core/plugin 钩子和默认关闭逻辑；期间发现本地 vLLM/Transformers 不匹配、无 NPU 的 PrivateUse1 用例被 deselect，以及一次 `__new__` runner 缺少实例 gate 的 AttributeError 回归，均有对应处理或保留说明。最终冻结两个独立 commit：Plugin `2afd8f23...`、Core `550c0d1b...`；Plugin 隔离测试报告为 `42 passed, 1 deselected`，Ruff、format、pycompile、diff-check 通过，但四机 stage/readback/PREFLIGHT 子任务仍由 root 独立跟进，不能据此宣称完整发布成功。
