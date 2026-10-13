---
blocks:
- block_id: 1da6303158ad7f98bbea1bb73ca2c6382f8f441fb40d699d4e3b96ca3d873ea6
  indexed: true
  sha256: b9b279a6ea6ef3bf918586e26a66a385c2e11a6895d50db134f694ff70147b87
  source_range:
    end: 1196461
    part: 0
    start: 0
  summary: 记录称已修正隔离 binding 导入路径，并报告 PR141 的 `kda_gate_cumsum`、`chunk_kda_fwd` 及三种 schema 已加载；首次 AscendC pytest 曾因公共 `conftest` 注册无关旧 schema 而未进入算子，后改为绕过整仓 conftest 验证。后续报告固定生产 prefill 走 PR141 AscendC、decode/spec 走 Triton recurrent，并删除 backend 环境变量、selector 与生产 fallback；同时记录空序列 metadata 被 compact 为连续 `seq_id`、正常插件 bootstrap 下模块导入通过，以及 Ruff、`py_compile` 和 diff 检查结果。上述成功数字和实际 NPU 执行均属于来源中的报告；记录还保留了任意顺序导入触发既有 `device_op ↔ ops` 循环、CRLF 行尾噪声及远端缺少 `pytest-mock` 等限制，未将其提升为独立验证结论。
  title: KDA 适配收尾、空序列复核与静态检查记录
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 95ce0106c5b8220ccb987eb80909cdf1dce187b2cc72a8c485162c97c94e705d
  kind: experience
  material_digest: 078d790233eb66339a47476dc97aaec8fa2dbb70b33c1a045d2377a362357059
  revision: 177d1146d5c1fddd193aeda0e232fb231a3d5b01595a5297351ac72a4c108053
  schema: mindie-entry/3
  summary: 当前材料集中于 KDA 生产路由收口、PR141 AscendC schema/binding、Triton recurrent decode/spec、空序列 chunk metadata、cache 转置和环境变量集中配置。可检索的关键线索包括：首次 pytest 被公共 conftest 的无关旧 schema 阻断；来源随后报告 NPU、UT、格式、编译和导入检查结果，但这些仍应视为带有环境与测试范围限定的工作记录。未决或需保留的上下文包括既有导入顺序循环、CRLF 处理、远端 `pytest-mock` 缺失，以及 PR141 vendor 仅含四个 KDA 算子、不能补足其他缺失 custom-op。
  title: KDA 适配与回归验证导航
navigation: 当前材料集中于 KDA 生产路由收口、PR141 AscendC schema/binding、Triton recurrent decode/spec、空序列 chunk metadata、cache 转置和环境变量集中配置。可检索的关键线索包括：首次 pytest 被公共 conftest 的无关旧 schema 阻断；来源随后报告 NPU、UT、格式、编译和导入检查结果，但这些仍应视为带有环境与测试范围限定的工作记录。未决或需保留的上下文包括既有导入顺序循环、CRLF 处理、远端 `pytest-mock` 缺失，以及 PR141 vendor 仅含四个 KDA 算子、不能补足其他缺失 custom-op。
schema: mindie-material-task/1
status: complete
task_id: 95ce0106c5b8220ccb987eb80909cdf1dce187b2cc72a8c485162c97c94e705d
---

# KDA 适配与回归验证导航

当前材料集中于 KDA 生产路由收口、PR141 AscendC schema/binding、Triton recurrent decode/spec、空序列 chunk metadata、cache 转置和环境变量集中配置。可检索的关键线索包括：首次 pytest 被公共 conftest 的无关旧 schema 阻断；来源随后报告 NPU、UT、格式、编译和导入检查结果，但这些仍应视为带有环境与测试范围限定的工作记录。未决或需保留的上下文包括既有导入顺序循环、CRLF 处理、远端 `pytest-mock` 缺失，以及 PR141 vendor 仅含四个 KDA 算子、不能补足其他缺失 custom-op。

## Materials

- [KDA 适配收尾、空序列复核与静态检查记录](blocks/1da6303158ad7f98bbea1bb73ca2c6382f8f441fb40d699d4e3b96ca3d873ea6.md): 记录称已修正隔离 binding 导入路径，并报告 PR141 的 `kda_gate_cumsum`、`chunk_kda_fwd` 及三种 schema 已加载；首次 AscendC pytest 曾因公共 `conftest` 注册无关旧 schema 而未进入算子，后改为绕过整仓 conftest 验证。后续报告固定生产 prefill 走 PR141 AscendC、decode/spec 走 Triton recurrent，并删除 backend 环境变量、selector 与生产 fallback；同时记录空序列 metadata 被 compact 为连续 `seq_id`、正常插件 bootstrap 下模块导入通过，以及 Ruff、`py_compile` 和 diff 检查结果。上述成功数字和实际 NPU 执行均属于来源中的报告；记录还保留了任意顺序导入触发既有 `device_op ↔ ops` 循环、CRLF 行尾噪声及远端缺少 `pytest-mock` 等限制，未将其提升为独立验证结论。
