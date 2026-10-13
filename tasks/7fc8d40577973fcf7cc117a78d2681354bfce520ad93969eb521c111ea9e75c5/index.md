---
blocks:
- block_id: 6e80adb89aefa30f0769039545d0eb13d7c2a2706ef0b3c876ec19cd0027e343
  indexed: true
  sha256: b0c9862299c30bc4fce8341cfaaa8cac3605539a245da659fd053f48aa43db35
  source_range:
    end: 748040
    part: 0
    start: 0
  summary: 材料中的审查结论称 SituMxQuant kernel 暂不需要修改，并报告 A5/ascend950 构建选择、op definition、tiling、binary manifest、PrivateUse1/Meta 注册及 K3 routed 规格传递均已覆盖；静态 contract、JSON、shell 语法和 diff 检查被报告为通过。记录的已知失败包括上游修复前的 `CAST_ONE` 与 `MicroAPI::Tanh` 编译错误，但当前代码是否在 A5 实机成功构建和运行仍未闭环，尚需对 `[1,6144]`、`[65,6144]` 及 golden 做对齐。后续补充称 TP1/2/4/8/16 的宽度与 mxscale 计算均支持，并特别建议验证 TP16 `[M,768]` 尾块和 `[M,12288]` 最大宽度；这些是建议验证，不是已证实成功。
  title: SituMxQuant kernel 静态审查与 A5 验证缺口
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 7fc8d40577973fcf7cc117a78d2681354bfce520ad93969eb521c111ea9e75c5
  kind: experience
  material_digest: 8f7a778a0a599ab22831c62e7293a1aaaa1f4c05a350269c325c35ec4227447c
  revision: c0e33ac3987e080725017da9023d7555f3195f8797bd612f0c78eed7f938591b
  schema: mindie-entry/3
  summary: 当前材料集中于“不修改 kernel”的静态审查结论、TP1/2/4/8/16 的规格覆盖，以及尚未完成的 A5 实机 build 与 golden 对齐验证。需保留上游 `CAST_ONE`/`MicroAPI::Tanh` 编译失败记录、当前结论的非实机性质和 TP16 尾块等待验证项；原始内容仍应独立检索，不能将静态检查或建议测试视为实机成功。
  title: SituMxQuant kernel 索引导航
navigation: 当前材料集中于“不修改 kernel”的静态审查结论、TP1/2/4/8/16 的规格覆盖，以及尚未完成的 A5 实机 build 与 golden 对齐验证。需保留上游 `CAST_ONE`/`MicroAPI::Tanh` 编译失败记录、当前结论的非实机性质和 TP16 尾块等待验证项；原始内容仍应独立检索，不能将静态检查或建议测试视为实机成功。
schema: mindie-material-task/1
status: complete
task_id: 7fc8d40577973fcf7cc117a78d2681354bfce520ad93969eb521c111ea9e75c5
---

# SituMxQuant kernel 索引导航

当前材料集中于“不修改 kernel”的静态审查结论、TP1/2/4/8/16 的规格覆盖，以及尚未完成的 A5 实机 build 与 golden 对齐验证。需保留上游 `CAST_ONE`/`MicroAPI::Tanh` 编译失败记录、当前结论的非实机性质和 TP16 尾块等待验证项；原始内容仍应独立检索，不能将静态检查或建议测试视为实机成功。

## Materials

- [SituMxQuant kernel 静态审查与 A5 验证缺口](blocks/6e80adb89aefa30f0769039545d0eb13d7c2a2706ef0b3c876ec19cd0027e343.md): 材料中的审查结论称 SituMxQuant kernel 暂不需要修改，并报告 A5/ascend950 构建选择、op definition、tiling、binary manifest、PrivateUse1/Meta 注册及 K3 routed 规格传递均已覆盖；静态 contract、JSON、shell 语法和 diff 检查被报告为通过。记录的已知失败包括上游修复前的 `CAST_ONE` 与 `MicroAPI::Tanh` 编译错误，但当前代码是否在 A5 实机成功构建和运行仍未闭环，尚需对 `[1,6144]`、`[65,6144]` 及 golden 做对齐。后续补充称 TP1/2/4/8/16 的宽度与 mxscale 计算均支持，并特别建议验证 TP16 `[M,768]` 尾块和 `[M,12288]` 最大宽度；这些是建议验证，不是已证实成功。
