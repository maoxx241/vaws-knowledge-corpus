---
blocks:
- block_id: 1bbd0606ba901f3f441bd9fb4c38b2467a8dfa3af764f2fc9236543e4a771fa9
  indexed: true
  sha256: dfaa47f85909fa6ae6826e6f9c11ec621e7c145a5122d8d9a5123b2760d99085
  source_range:
    end: 1377396
    part: 0
    start: 0
  summary: 据源材料，154 上容器内 PGID 921213 的孤立 DP rank1 曾仅收到 TERM，随后声称 153–156 无 NPU 进程且 8077/14569 无监听；四机 OFF 基线服务期间要求不要启动其他 NPU 工作，并计划完成 A/B 后再释放节点。插件侧描述了 MRV1 eager、固定 K=7、anchor-first、draft/target vocab 映射、完整 draft KV 复用、GDN threshold=8、DP padding 清零及 DSpark aux 捕获层等改动。源材料报告 git diff、compileall、Ruff 检查通过，但未运行 pytest，因为本地 pyenv 缺少 torch 和 pytest；这些结果及四机后续接收率、精度和特性叠加验证仍应视为未独立核实。
  title: 四机接管与 DSpark 插件收口
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 3942a72449d84dcf9cb668fb10c5d66c033c00956ef9989bdc315782eae9017c
  kind: experience
  material_digest: 0cc90ddd85ddc23698b59705c9470243482fd2eb19ab068d68c662c8416c72bb
  revision: ed815ca3251bd4243d4ba5c5af23502eb2c0e13cfda632dd21726f8cea7899a9
  schema: mindie-entry/3
  summary: 本批仅含一个材料块：记录了四机接管约束、PGID/端口/NPU 状态的源材料报告，以及 DSpark 插件实现收口与静态检查结果。后续重点是保留未运行 pytest、缺少 torch/pytest，以及尚未明确完成的两机 5 层冒烟和四机接收率、精度、特性叠加验证；原始块仍可独立检索。
  title: 当前任务导航
navigation: 本批仅含一个材料块：记录了四机接管约束、PGID/端口/NPU 状态的源材料报告，以及 DSpark 插件实现收口与静态检查结果。后续重点是保留未运行 pytest、缺少 torch/pytest，以及尚未明确完成的两机 5 层冒烟和四机接收率、精度、特性叠加验证；原始块仍可独立检索。
schema: mindie-material-task/1
status: complete
task_id: 3942a72449d84dcf9cb668fb10c5d66c033c00956ef9989bdc315782eae9017c
---

# 当前任务导航

本批仅含一个材料块：记录了四机接管约束、PGID/端口/NPU 状态的源材料报告，以及 DSpark 插件实现收口与静态检查结果。后续重点是保留未运行 pytest、缺少 torch/pytest，以及尚未明确完成的两机 5 层冒烟和四机接收率、精度、特性叠加验证；原始块仍可独立检索。

## Materials

- [四机接管与 DSpark 插件收口](blocks/1bbd0606ba901f3f441bd9fb4c38b2467a8dfa3af764f2fc9236543e4a771fa9.md): 据源材料，154 上容器内 PGID 921213 的孤立 DP rank1 曾仅收到 TERM，随后声称 153–156 无 NPU 进程且 8077/14569 无监听；四机 OFF 基线服务期间要求不要启动其他 NPU 工作，并计划完成 A/B 后再释放节点。插件侧描述了 MRV1 eager、固定 K=7、anchor-first、draft/target vocab 映射、完整 draft KV 复用、GDN threshold=8、DP padding 清零及 DSpark aux 捕获层等改动。源材料报告 git diff、compileall、Ruff 检查通过，但未运行 pytest，因为本地 pyenv 缺少 torch 和 pytest；这些结果及四机后续接收率、精度和特性叠加验证仍应视为未独立核实。
