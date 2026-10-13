---
blocks:
- block_id: dd3722029dc28ae868cbb4aaadd66b8364bc36a3027e4c4e352cd036b716d387
  indexed: true
  sha256: b24836185f216fcad0594d03671d7b092698189268453e8e312b274b15d87ab5
  source_range:
    end: 851338
    part: 0
    start: 0
  summary: 用户提出运行两道有语义的多模态题目，并要求后续开启 VL 与 Prefix Cache，改用 `/mnt/weight/k3` 下的 `w4a8-int-smooth-8cards` 权重，同时要求直接终止旧运行以启动新权重和配置。记录中的助手口头表示将只读轮询四机日志、服务进程和 rank-0 健康状态，但随后报告四机服务已启动、rank-0 `health=200`、353/353 shards、`torch.compile` 全部成功及 ACLGraph 2/2 capture 成功；这些是报告结果，未在材料中独立核验，且不能据此确认多模态或 GPQA 测试已经完成。
  title: 多模态与新权重配置切换记录
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 2c97815268526d7a2daacc1b417ea62024c2dd83cf3c09376ac12984031e8752
  kind: experience
  material_digest: c6efabd26eb0fb4c276118d9db2b4b95cc98923fcb887e1df0abd66e025a1112
  revision: 399d11fe3e3b9407baf12de4ff7f7008a17be1d3b696eee576c7e422ecfa19cb
  schema: mindie-entry/3
  summary: 本批仅包含一个材料块，主题是切换至 `Kimi-K3-w4a8-int-smooth-8cards` 并启用 VL、Prefix Cache 等配置。可检索术语包括 `health=200`、353/353 shards、`torch.compile`、ACLGraph、TP16/DP4/EP64、`ERROR`、`Traceback`、`RuntimeError`；服务启动与配置成功属于助手报告，测试完成情况仍未定。
  title: 当前任务导航：多模态新配置运行记录
navigation: 本批仅包含一个材料块，主题是切换至 `Kimi-K3-w4a8-int-smooth-8cards` 并启用 VL、Prefix Cache 等配置。可检索术语包括 `health=200`、353/353 shards、`torch.compile`、ACLGraph、TP16/DP4/EP64、`ERROR`、`Traceback`、`RuntimeError`；服务启动与配置成功属于助手报告，测试完成情况仍未定。
schema: mindie-material-task/1
status: complete
task_id: 2c97815268526d7a2daacc1b417ea62024c2dd83cf3c09376ac12984031e8752
---

# 当前任务导航：多模态新配置运行记录

本批仅包含一个材料块，主题是切换至 `Kimi-K3-w4a8-int-smooth-8cards` 并启用 VL、Prefix Cache 等配置。可检索术语包括 `health=200`、353/353 shards、`torch.compile`、ACLGraph、TP16/DP4/EP64、`ERROR`、`Traceback`、`RuntimeError`；服务启动与配置成功属于助手报告，测试完成情况仍未定。

## Materials

- [多模态与新权重配置切换记录](blocks/dd3722029dc28ae868cbb4aaadd66b8364bc36a3027e4c4e352cd036b716d387.md): 用户提出运行两道有语义的多模态题目，并要求后续开启 VL 与 Prefix Cache，改用 `/mnt/weight/k3` 下的 `w4a8-int-smooth-8cards` 权重，同时要求直接终止旧运行以启动新权重和配置。记录中的助手口头表示将只读轮询四机日志、服务进程和 rank-0 健康状态，但随后报告四机服务已启动、rank-0 `health=200`、353/353 shards、`torch.compile` 全部成功及 ACLGraph 2/2 capture 成功；这些是报告结果，未在材料中独立核验，且不能据此确认多模态或 GPQA 测试已经完成。
