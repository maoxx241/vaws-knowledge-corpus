---
blocks:
- block_id: ec6cdcb088f100cc713a695e27f769ce88350d2dfc9a8acdbcc2ecc9cd03b0bc
  indexed: true
  sha256: c0d6c2999cc6ebe9115c53ce6f123dfc149293932f96f313af46ac59837e20e1
  source_range:
    end: 432442
    part: 0
    start: 0
  summary: 据所引 assistant 最终消息，156 的 P 实例据报告已正常启动：TP16 的 16 个 rank 就绪，ZMQ 监听 `20001..20016`，ADXL 动态端口未与该区间碰撞，并出现 `Application startup complete`；`/health` 据报告返回 HTTP 200。当前 `p.log` 据报告没有 `ERROR`、`Traceback` 或 `KdaGateCumsum`，但 KDA 尚未经过后续首个 prefill 请求的实际验证，因此不能视为端到端成功。
  title: P 实例启动与健康检查结果
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 52a0a4a8ab8616e826f1eec781a2795bc7f859b8bf80f0cfc9f2d93b54d647ee
  kind: experience
  material_digest: 2e867af312f64b4d7d5ad77a31d1d8b1d8f7ebb36fde90497f5154af0033b516
  revision: d2aaaeb349e36c3327a2944e2c5baa6548753a13afc7cb04f7a943314bc44644
  schema: mindie-entry/3
  summary: 已收录 1 个新材料块：其内容涉及 P 实例启动、TP16 rank、ZMQ/ADXL 端口、健康检查及日志观察。KDA 的首个 prefill 验证仍未完成；以上启动与检查结果均保留为来源中的报告，不等同于已核实的端到端成功。
  title: 当前任务导航
navigation: 已收录 1 个新材料块：其内容涉及 P 实例启动、TP16 rank、ZMQ/ADXL 端口、健康检查及日志观察。KDA 的首个 prefill 验证仍未完成；以上启动与检查结果均保留为来源中的报告，不等同于已核实的端到端成功。
schema: mindie-material-task/1
status: complete
task_id: 52a0a4a8ab8616e826f1eec781a2795bc7f859b8bf80f0cfc9f2d93b54d647ee
---

# 当前任务导航

已收录 1 个新材料块：其内容涉及 P 实例启动、TP16 rank、ZMQ/ADXL 端口、健康检查及日志观察。KDA 的首个 prefill 验证仍未完成；以上启动与检查结果均保留为来源中的报告，不等同于已核实的端到端成功。

## Materials

- [P 实例启动与健康检查结果](blocks/ec6cdcb088f100cc713a695e27f769ce88350d2dfc9a8acdbcc2ecc9cd03b0bc.md): 据所引 assistant 最终消息，156 的 P 实例据报告已正常启动：TP16 的 16 个 rank 就绪，ZMQ 监听 `20001..20016`，ADXL 动态端口未与该区间碰撞，并出现 `Application startup complete`；`/health` 据报告返回 HTTP 200。当前 `p.log` 据报告没有 `ERROR`、`Traceback` 或 `KdaGateCumsum`，但 KDA 尚未经过后续首个 prefill 请求的实际验证，因此不能视为端到端成功。
