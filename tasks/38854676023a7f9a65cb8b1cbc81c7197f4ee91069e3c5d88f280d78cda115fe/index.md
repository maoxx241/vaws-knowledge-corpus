---
blocks:
- block_id: ed48190c7defff5d68ba2ede239f792382f95d34891a8bd31cb438729646ddb1
  indexed: true
  sha256: 94d00d8d680c6ad87d4e7eec776ac4e8bba1e2cbc9c6a07b130581c5a2cebee8
  source_range:
    end: 829943
    part: 0
    start: 0
  summary: 该块记录了对 AscendC r11 独立源、baseline full OPP 与单算子 OPP 的文件闭包核对：报告称全局配置和共享库应保留 baseline，仅替换 4 个 `.o` 及对应 4 个 JSON，并将方案固化为组装、dry-run 和哈希校验脚本。还报告清理了 r11 路径内 5 个 AppleDouble 文件，未启动服务、未占用 NPU，也未处理 r12；这些均是该消息中的准备结果与判断，不能视为已完成部署或运行验证。后续计划包括四机传输、全树校验和 native A/B，但当时尚未执行。
  title: AscendC r11 服务集成准备与 8 文件替换方案
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 38854676023a7f9a65cb8b1cbc81c7197f4ee91069e3c5d88f280d78cda115fe
  kind: experience
  material_digest: 43dbd8031df483245e2727f0d8e9da2a9704af1b6f9eb71965f8e7a8461f49b8
  revision: 7240c2fcffae5bc59ee113843b89aa6bb14e662ab4a5ad6efce85971176bebcc
  schema: mindie-entry/3
  summary: 目前可检索到的上下文集中于 r11 的 baseline full OPP 闭包分析、精确 8 文件替换方案、AppleDouble 清理及本地 dry-run/哈希校验准备。报告称尚未启动服务、占用 NPU、访问四机或处理 r12；四机部署与 native A/B 仍是未完成的后续事项，且现有结论属于准备阶段的可审计记录而非经运行验证的成功。
  title: 当前任务导航：AscendC r11 服务集成准备
navigation: 目前可检索到的上下文集中于 r11 的 baseline full OPP 闭包分析、精确 8 文件替换方案、AppleDouble 清理及本地 dry-run/哈希校验准备。报告称尚未启动服务、占用 NPU、访问四机或处理 r12；四机部署与 native A/B 仍是未完成的后续事项，且现有结论属于准备阶段的可审计记录而非经运行验证的成功。
schema: mindie-material-task/1
status: complete
task_id: 38854676023a7f9a65cb8b1cbc81c7197f4ee91069e3c5d88f280d78cda115fe
---

# 当前任务导航：AscendC r11 服务集成准备

目前可检索到的上下文集中于 r11 的 baseline full OPP 闭包分析、精确 8 文件替换方案、AppleDouble 清理及本地 dry-run/哈希校验准备。报告称尚未启动服务、占用 NPU、访问四机或处理 r12；四机部署与 native A/B 仍是未完成的后续事项，且现有结论属于准备阶段的可审计记录而非经运行验证的成功。

## Materials

- [AscendC r11 服务集成准备与 8 文件替换方案](blocks/ed48190c7defff5d68ba2ede239f792382f95d34891a8bd31cb438729646ddb1.md): 该块记录了对 AscendC r11 独立源、baseline full OPP 与单算子 OPP 的文件闭包核对：报告称全局配置和共享库应保留 baseline，仅替换 4 个 `.o` 及对应 4 个 JSON，并将方案固化为组装、dry-run 和哈希校验脚本。还报告清理了 r11 路径内 5 个 AppleDouble 文件，未启动服务、未占用 NPU，也未处理 r12；这些均是该消息中的准备结果与判断，不能视为已完成部署或运行验证。后续计划包括四机传输、全树校验和 native A/B，但当时尚未执行。
