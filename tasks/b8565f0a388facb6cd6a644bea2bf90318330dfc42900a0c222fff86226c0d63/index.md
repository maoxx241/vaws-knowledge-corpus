---
blocks:
- block_id: 85cdb8ef7eaa8c145f8f73695acbe893e70d0b1fdcbd278cea850c6a087ca940
  indexed: true
  sha256: d95cd56640f51489417eddb7ebf41ddf9b8ebba9ce703ebb5bee56cea78ff065
  source_range:
    end: 373762
    part: 0
    start: 0
  summary: 用户要求算子不用重新编译；助手称仅执行源码级 UT、导入和配置断言，未加载权重、启动服务或进行单机精度测试，并提到远端源码路径和自定义算子环境脚本已存在。报告结果为指定 5 个 UT 通过 79 项、另一个测试通过 13 项，合计 92 passed、0 failed；GQA draft config 经 `hf_config_override` 后的架构、目标层 ID 和 `block_size=7` 断言据称通过，但这些是源中报告的结果，未独立验证。记录的环境/方法约束包括 CPU/mock 测试、关闭 pytest 缓存和 `.pyc` 写入；另有 14 条 `torch.jit.script_method` 弃用警告。
  title: 153 源码级 UT 预检（未编译算子）
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: b8565f0a388facb6cd6a644bea2bf90318330dfc42900a0c222fff86226c0d63
  kind: experience
  material_digest: be6af80977e797a91fb267884cebea3390b920f64963a2be6b4b1bad4965c6a3
  revision: fb3d68cb8a518873de2e999dd01f561d64a7a89b16e7b70dbc6da505d0610d5d
  schema: mindie-entry/3
  summary: 当前批次包含 1 个可检索材料块，主题是“不重新编译算子”条件下的源码级 UT、配置断言及测试环境约束。索引保留了报告的 92 项通过、配置断言结果、未执行的权重/服务/精度验证，以及弃用警告；这些结果属于未独立核验的参考记录，原始块仍可单独检索。
  title: 当前任务导航：153 源码级预检索引
navigation: 当前批次包含 1 个可检索材料块，主题是“不重新编译算子”条件下的源码级 UT、配置断言及测试环境约束。索引保留了报告的 92 项通过、配置断言结果、未执行的权重/服务/精度验证，以及弃用警告；这些结果属于未独立核验的参考记录，原始块仍可单独检索。
schema: mindie-material-task/1
status: complete
task_id: b8565f0a388facb6cd6a644bea2bf90318330dfc42900a0c222fff86226c0d63
---

# 当前任务导航：153 源码级预检索引

当前批次包含 1 个可检索材料块，主题是“不重新编译算子”条件下的源码级 UT、配置断言及测试环境约束。索引保留了报告的 92 项通过、配置断言结果、未执行的权重/服务/精度验证，以及弃用警告；这些结果属于未独立核验的参考记录，原始块仍可单独检索。

## Materials

- [153 源码级 UT 预检（未编译算子）](blocks/85cdb8ef7eaa8c145f8f73695acbe893e70d0b1fdcbd278cea850c6a087ca940.md): 用户要求算子不用重新编译；助手称仅执行源码级 UT、导入和配置断言，未加载权重、启动服务或进行单机精度测试，并提到远端源码路径和自定义算子环境脚本已存在。报告结果为指定 5 个 UT 通过 79 项、另一个测试通过 13 项，合计 92 passed、0 failed；GQA draft config 经 `hf_config_override` 后的架构、目标层 ID 和 `block_size=7` 断言据称通过，但这些是源中报告的结果，未独立验证。记录的环境/方法约束包括 CPU/mock 测试、关闭 pytest 缓存和 `.pyc` 写入；另有 14 条 `torch.jit.script_method` 弃用警告。
