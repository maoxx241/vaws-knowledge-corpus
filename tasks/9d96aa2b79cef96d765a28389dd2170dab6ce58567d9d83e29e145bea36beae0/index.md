---
blocks:
- block_id: ea0a60f42a57510855b073a2a1bc5a32ea611b5174e0d0a7435370d48fc7ff5a
  indexed: true
  sha256: 1ed3132674119bde6fb9a14a3a36ec66697bfd49a15e4fdf5aa7e0ac3434684e
  source_range:
    end: 367353
    part: 0
    start: 0
  summary: 用户质疑 ci-new 分支遇到的问题可能与 dspark 特性无关；随后一段助手记录称已对 r3 四机进行只读核对。记录报告四个主进程参数一致，包括 K=7、prefix cache、FULL_DECODE_ONLY、shared expert DP 开启及 multistream 关闭，并列出 enable_fused_mc2=0 等环境信息。该记录还称未发现 attn_res.py、Traceback、ERROR 或 ImportError，但指出远端 Git HEAD 仍为父提交 0fc695f，运行时文件通过工作树覆盖部署；这些均是报告内容，未独立验证。
  title: ci-new 分支问题与 r3 四机只读审计记录
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 9d96aa2b79cef96d765a28389dd2170dab6ce58567d9d83e29e145bea36beae0
  kind: experience
  material_digest: 04e5ae2469e58d5afbd26c3c88aed5514b4191523d6b25daafe785dcce7b4796
  revision: 599c11424c53ca1e69ac213975e2b949180825ad54eef51452efc9d497cfaae7
  schema: mindie-entry/3
  summary: 本批次包含 1 个可检索块，主题为 ci-new 分支与 dspark 关联性的质疑，以及 r3 四机运行配置和源码/日志的只读审计。索引保留了参数一致、未见错误、Git HEAD 与部署文件不一致等报告性结论，并标明尚未独立验证；原始块仍是完整检索依据。
  title: 当前任务导航
navigation: 本批次包含 1 个可检索块，主题为 ci-new 分支与 dspark 关联性的质疑，以及 r3 四机运行配置和源码/日志的只读审计。索引保留了参数一致、未见错误、Git HEAD 与部署文件不一致等报告性结论，并标明尚未独立验证；原始块仍是完整检索依据。
schema: mindie-material-task/1
status: complete
task_id: 9d96aa2b79cef96d765a28389dd2170dab6ce58567d9d83e29e145bea36beae0
---

# 当前任务导航

本批次包含 1 个可检索块，主题为 ci-new 分支与 dspark 关联性的质疑，以及 r3 四机运行配置和源码/日志的只读审计。索引保留了参数一致、未见错误、Git HEAD 与部署文件不一致等报告性结论，并标明尚未独立验证；原始块仍是完整检索依据。

## Materials

- [ci-new 分支问题与 r3 四机只读审计记录](blocks/ea0a60f42a57510855b073a2a1bc5a32ea611b5174e0d0a7435370d48fc7ff5a.md): 用户质疑 ci-new 分支遇到的问题可能与 dspark 特性无关；随后一段助手记录称已对 r3 四机进行只读核对。记录报告四个主进程参数一致，包括 K=7、prefix cache、FULL_DECODE_ONLY、shared expert DP 开启及 multistream 关闭，并列出 enable_fused_mc2=0 等环境信息。该记录还称未发现 attn_res.py、Traceback、ERROR 或 ImportError，但指出远端 Git HEAD 仍为父提交 0fc695f，运行时文件通过工作树覆盖部署；这些均是报告内容，未独立验证。
