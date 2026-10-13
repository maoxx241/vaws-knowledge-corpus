---
blocks:
- block_id: 429b8d5fe5d48c141cfb2ee9e83d2bda6d17429486a5d2a678c5b03566536e81
  indexed: true
  sha256: 09f005f5bd173326813901672d0d3f58af6f1e20dab6459b8ee95e5caf542877
  source_range:
    end: 2884985
    part: 0
    start: 0
  summary: 使用指定 session 的方法，仅以 GQA 权重在 `sglang:cann9.0.0-a3-k3`、TP64×DP4×EP64、DSpark K=7 配置下完成 prefix-off 8K/1K 和 prefix-on 128K/1K 测试；结果包含 TTFT、TPOT、AL 与原生 `/generate` 返回的 speculative verify 采信率。过程中保留了资源预检遗漏、`/tokenize` 的 “Integer exceeds 64-bit range”、`/server_info` 统计周期不匹配等失败与修正，最终五个并发档位均生成 1024 tokens，但 128K case 的 AL 与采信率明显偏低。Prefix-on 四节点服务据报告仍在运行，`/model_info` 返回 HTTP 200；相关路径、容器和结果文件含有 redacted 占位符，不能据此恢复私有信息。
  title: SGLang GQA 性能与采信率测试
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: a5228358ed3fb5e3105f012834ef3db03adf19699ef28dd7ffc8688cb874f8a4
  kind: experience
  material_digest: 2abae8e7bbe6c022f18c0bf71d840bc6811cd8f9bf9d3d461d23f9bd31bb117c
  revision: e928cb2c3c5ce7961e1dc3befe78e87ff6ee05db4eeb3c0b1aae33ce554f76d5
  schema: mindie-entry/3
  summary: 已记录指定 session 下 SGLang GQA-only 性能与采信率测试：prefix-off 8K/1K（并发 1/2/4）和 prefix-on 128K/1K（并发 1/2）结果、错误修正、原生 `/generate` 采信计数方法及运行配置。此前辅助 `/tokenize` 与 `/server_info` 路径存在问题，后续结果依赖本地 tokenizer 和请求级 verify 字段；Prefix-on 服务据报告保留运行，原始结果与日志仍可独立检索。
  title: 当前任务导航
navigation: 已记录指定 session 下 SGLang GQA-only 性能与采信率测试：prefix-off 8K/1K（并发 1/2/4）和 prefix-on 128K/1K（并发 1/2）结果、错误修正、原生 `/generate` 采信计数方法及运行配置。此前辅助 `/tokenize` 与 `/server_info` 路径存在问题，后续结果依赖本地 tokenizer 和请求级 verify 字段；Prefix-on 服务据报告保留运行，原始结果与日志仍可独立检索。
schema: mindie-material-task/1
status: complete
task_id: a5228358ed3fb5e3105f012834ef3db03adf19699ef28dd7ffc8688cb874f8a4
---

# 当前任务导航

已记录指定 session 下 SGLang GQA-only 性能与采信率测试：prefix-off 8K/1K（并发 1/2/4）和 prefix-on 128K/1K（并发 1/2）结果、错误修正、原生 `/generate` 采信计数方法及运行配置。此前辅助 `/tokenize` 与 `/server_info` 路径存在问题，后续结果依赖本地 tokenizer 和请求级 verify 字段；Prefix-on 服务据报告保留运行，原始结果与日志仍可独立检索。

## Materials

- [SGLang GQA 性能与采信率测试](blocks/429b8d5fe5d48c141cfb2ee9e83d2bda6d17429486a5d2a678c5b03566536e81.md): 使用指定 session 的方法，仅以 GQA 权重在 `sglang:cann9.0.0-a3-k3`、TP64×DP4×EP64、DSpark K=7 配置下完成 prefix-off 8K/1K 和 prefix-on 128K/1K 测试；结果包含 TTFT、TPOT、AL 与原生 `/generate` 返回的 speculative verify 采信率。过程中保留了资源预检遗漏、`/tokenize` 的 “Integer exceeds 64-bit range”、`/server_info` 统计周期不匹配等失败与修正，最终五个并发档位均生成 1024 tokens，但 128K case 的 AL 与采信率明显偏低。Prefix-on 四节点服务据报告仍在运行，`/model_info` 返回 HTTP 200；相关路径、容器和结果文件含有 redacted 占位符，不能据此恢复私有信息。
