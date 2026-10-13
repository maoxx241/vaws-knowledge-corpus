---
blocks:
- block_id: bacbe5e197434c1d95e51fea5d4809571d1c05da2c1522c28c32cfa41f99819f
  indexed: true
  sha256: 42ce75d760e21b37ef382d08d1670bd33d865dbeb748d56698c2898e1a0b5f3f
  source_range:
    end: 4381491
    part: 0
    start: 0
  summary: '完成 FlashComm1 与共享专家 DP 配置及运行路径解耦，提交签名提交 `4fd03e057` 并创建 Draft PR #37；随后确认最新 `ci-new` 已含等价提交 `02aec1374`。在 TP16×DP4×EP64、完整 W4A8 配置下，首次可复用算子路径遇到 `dequant_situ_quant` 未注册，修正 Python 扩展 `vllm_ascend_C.so` 加载后通过 profile forward，服务 `/health=200`。GPQA 全量运行已启动但尚未完成，后续材料记录了进度、长尾请求及最终提前终止。'
  title: 解耦 FlashComm 与共享专家 DP 并启动 GPQA 验证
- block_id: 986704b40717c90eec1f9d027cd5ecb0ca192c2cb4bed2cdfed9e8518efae3b1
  indexed: true
  sha256: 95690e122a49ece685d368405a2c92002be33fde415dc7d83fb2faad60e2d07a
  source_range:
    end: 4381491
    part: 1
    start: 0
  summary: 修正算子注册后，64 个 worker 完成入组和 352 个权重分片加载，FC1 开、共享专家 DP 关的 profile forward 通过，随后启动官方 198 题 GPQA runner（并发 32、未限制 `max_tokens`）。运行过程中服务持续 `health=200`、服务错误为 0，但后 8 题出现数万 token 的长尾；中途结果从 `180/192` 逐步推进至 `181/193`，期间的准确率均为暂时值，不能视为最终精度。用户随后要求终止服务并查看剩余五题的历史表现。
  title: GPQA 全量运行中的长尾与中途结果
- block_id: 3a8a473f32cec5b90f9151a2370d8ee27d5a276c2a1550a80520801902026f91
  indexed: true
  sha256: 47a747e579f7a5dfd648f19a808dd7aa57e5038e040ff1023e4361f7572b0ff0
  source_range:
    end: 4381491
    part: 2
    start: 0
  summary: 按要求精确终止 GPQA runner 和四机服务，确认目标 live 进程、64 张卡 device holder 及 `npu-smi` 运行进程均为 0；容器未重启，残留 defunct 条目不占卡。冻结的 193 条 JSONL 审计结果为 181 对、12 错，全部 `finish_reason=stop`，请求错误、缺 prediction、乱码、空输出和 XTML 泄漏均为 0；官方完整 `summary.json` 未生成，曾出现的截断尾片段已消失。剩余 5 题仅依据两次历史结果估算：79、185 大概率对，145、147 大概率错，71 五五开，因此预估完整结果为 183/198（92.42%）或 184/198（92.93%），不是实测最终分数。
  title: 提前终止 GPQA 后的冻结审计与历史估算
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 51f0f153b447c31099c1bccca1fdb25725ff21c9fc299e0a71c10e42124f231d
  kind: experience
  material_digest: 354fca4bb8cbf86798a1a28b851fad4774d0bf9b03ea65375f0683fa0ecebc18
  revision: 7674f13a086f37973171202ac742edcb74a82bc841905399d70acf8f8684049a
  schema: mindie-entry/3
  summary: 主线已完成 FlashComm1 与共享专家 DP 解耦，并在最新 `ci-new` 上验证 FC1 开、共享专家 DP 关的完整权重 profile forward；早期 curl `20×20=400` 仅是语义/乱码冒烟，不是精度结论。GPQA 全量运行因长尾被提前终止，当前可确认的实测结果只有 193/198：181 对、12 错（93.78%），机器已释放；剩余 5 题和 183–184/198 的完整分数区间均为历史估算，且缺少官方完整 summary，不应提升为已验证成功。
  title: 当前任务导航：解耦验证与 GPQA 未完成收尾
navigation: 主线已完成 FlashComm1 与共享专家 DP 解耦，并在最新 `ci-new` 上验证 FC1 开、共享专家 DP 关的完整权重 profile forward；早期 curl `20×20=400` 仅是语义/乱码冒烟，不是精度结论。GPQA 全量运行因长尾被提前终止，当前可确认的实测结果只有 193/198：181 对、12 错（93.78%），机器已释放；剩余 5 题和 183–184/198 的完整分数区间均为历史估算，且缺少官方完整 summary，不应提升为已验证成功。
schema: mindie-material-task/1
status: complete
task_id: 51f0f153b447c31099c1bccca1fdb25725ff21c9fc299e0a71c10e42124f231d
---

# 当前任务导航：解耦验证与 GPQA 未完成收尾

主线已完成 FlashComm1 与共享专家 DP 解耦，并在最新 `ci-new` 上验证 FC1 开、共享专家 DP 关的完整权重 profile forward；早期 curl `20×20=400` 仅是语义/乱码冒烟，不是精度结论。GPQA 全量运行因长尾被提前终止，当前可确认的实测结果只有 193/198：181 对、12 错（93.78%），机器已释放；剩余 5 题和 183–184/198 的完整分数区间均为历史估算，且缺少官方完整 summary，不应提升为已验证成功。

## Materials

- [解耦 FlashComm 与共享专家 DP 并启动 GPQA 验证](blocks/bacbe5e197434c1d95e51fea5d4809571d1c05da2c1522c28c32cfa41f99819f.md): 完成 FlashComm1 与共享专家 DP 配置及运行路径解耦，提交签名提交 `4fd03e057` 并创建 Draft PR #37；随后确认最新 `ci-new` 已含等价提交 `02aec1374`。在 TP16×DP4×EP64、完整 W4A8 配置下，首次可复用算子路径遇到 `dequant_situ_quant` 未注册，修正 Python 扩展 `vllm_ascend_C.so` 加载后通过 profile forward，服务 `/health=200`。GPQA 全量运行已启动但尚未完成，后续材料记录了进度、长尾请求及最终提前终止。
- [GPQA 全量运行中的长尾与中途结果](blocks/986704b40717c90eec1f9d027cd5ecb0ca192c2cb4bed2cdfed9e8518efae3b1.md): 修正算子注册后，64 个 worker 完成入组和 352 个权重分片加载，FC1 开、共享专家 DP 关的 profile forward 通过，随后启动官方 198 题 GPQA runner（并发 32、未限制 `max_tokens`）。运行过程中服务持续 `health=200`、服务错误为 0，但后 8 题出现数万 token 的长尾；中途结果从 `180/192` 逐步推进至 `181/193`，期间的准确率均为暂时值，不能视为最终精度。用户随后要求终止服务并查看剩余五题的历史表现。
- [提前终止 GPQA 后的冻结审计与历史估算](blocks/3a8a473f32cec5b90f9151a2370d8ee27d5a276c2a1550a80520801902026f91.md): 按要求精确终止 GPQA runner 和四机服务，确认目标 live 进程、64 张卡 device holder 及 `npu-smi` 运行进程均为 0；容器未重启，残留 defunct 条目不占卡。冻结的 193 条 JSONL 审计结果为 181 对、12 错，全部 `finish_reason=stop`，请求错误、缺 prediction、乱码、空输出和 XTML 泄漏均为 0；官方完整 `summary.json` 未生成，曾出现的截断尾片段已消失。剩余 5 题仅依据两次历史结果估算：79、185 大概率对，145、147 大概率错，71 五五开，因此预估完整结果为 183/198（92.42%）或 184/198（92.93%），不是实测最终分数。
