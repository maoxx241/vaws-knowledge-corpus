---
blocks:
- block_id: e71177b6c24ff4bc3c85bf0baa6c6ebb262a90f27ba82048f70c480810195235
  indexed: true
  sha256: e5f3017135bff8720264a950c3a1e0cb333e0f35377de3aa84c768a2d17dc1a5
  source_range:
    end: 595577
    part: 0
    start: 0
  summary: 记录称在不重新编译算子、不安装软件且不操作服务进程/NPU的前提下，对四机 editable 映射进行了对比，并报告仅缺少有效顶层模块 `vllm/_version.py`；同时提到部分节点存在旧树 macOS AppleDouble 文件。该记录报告创建了新的 core/runtime r2 路径，给出源文件与 runtime 聚合哈希、版本 `0.0.0.dev0+gd02df748b`、commit `gd02df748b`，并称 preflight 增加了版本断言。记录还称最小导入解析验证通过，但明确未运行完整 preflight；这些结果和哈希均属于报告内容，不能视为独立核验成功。
  title: 四机 editable 元数据隔离与版本模块补齐
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: feda30da89fdd36f1345a4f7799e9d8b9c8d491bf9bb5c9ce8373d5f109c8c0f
  kind: experience
  material_digest: 00c486e71c0bf83f22bc3e508591dc455497496be19a2a5441995d3be32d0c1f
  revision: f1dcdd86dc83e18d02918a685c7027b4453cbba1a642aa46f4e4dacddd710f5b
  schema: mindie-entry/3
  summary: 重点检索 `vllm/_version.py`、editable finder 回退、core/runtime r2、聚合哈希、版本与 commit 固定、preflight 断言及最小导入解析。当前记录报告未重新编译或安装，也未运行完整 preflight；最小导入解析与部署隔离结果仍应按可失败参考处理，且后续如有更正或未完成项应保留。
  title: 当前任务导航：四机版本元数据与部署路径隔离
navigation: 重点检索 `vllm/_version.py`、editable finder 回退、core/runtime r2、聚合哈希、版本与 commit 固定、preflight 断言及最小导入解析。当前记录报告未重新编译或安装，也未运行完整 preflight；最小导入解析与部署隔离结果仍应按可失败参考处理，且后续如有更正或未完成项应保留。
schema: mindie-material-task/1
status: complete
task_id: feda30da89fdd36f1345a4f7799e9d8b9c8d491bf9bb5c9ce8373d5f109c8c0f
---

# 当前任务导航：四机版本元数据与部署路径隔离

重点检索 `vllm/_version.py`、editable finder 回退、core/runtime r2、聚合哈希、版本与 commit 固定、preflight 断言及最小导入解析。当前记录报告未重新编译或安装，也未运行完整 preflight；最小导入解析与部署隔离结果仍应按可失败参考处理，且后续如有更正或未完成项应保留。

## Materials

- [四机 editable 元数据隔离与版本模块补齐](blocks/e71177b6c24ff4bc3c85bf0baa6c6ebb262a90f27ba82048f70c480810195235.md): 记录称在不重新编译算子、不安装软件且不操作服务进程/NPU的前提下，对四机 editable 映射进行了对比，并报告仅缺少有效顶层模块 `vllm/_version.py`；同时提到部分节点存在旧树 macOS AppleDouble 文件。该记录报告创建了新的 core/runtime r2 路径，给出源文件与 runtime 聚合哈希、版本 `0.0.0.dev0+gd02df748b`、commit `gd02df748b`，并称 preflight 增加了版本断言。记录还称最小导入解析验证通过，但明确未运行完整 preflight；这些结果和哈希均属于报告内容，不能视为独立核验成功。
