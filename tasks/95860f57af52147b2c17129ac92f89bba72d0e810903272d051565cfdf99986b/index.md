---
blocks:
- block_id: b03549bca34e3f418c1b4e7ad9c145e7d45237fce60070c854f100223f32edaa
  indexed: true
  sha256: e4534ea45278674f9f56d603ebf043eff9d57a3fda519ae2d63f84b6ea5e596c
  source_range:
    end: 732275
    part: 0
    start: 0
  summary: 消息报告：仅审计 153 上的 native r11 seed 与现有 r12 launcher，未启动或停止 r12，也未触碰 154–156；153 的 seed 被描述为完整 baseline OPP 加替换 fwd-h 的 4 组 `.o/.json`，共 979 个文件。rank launcher 和两份四机审计脚本据称通过 `bash -n`、`shellcheck`，153 已部署独立 launcher，并报告 source manifest、978 个 OPP runtime 文件及无 AppleDouble 校验通过；154–156 据称不存在 native r11 source/runtime。上述部署、SHA、命令参数一致性和审计结果均为来源中的报告，保留其未独立验证状态。
  title: native r11 四机 launcher 与审计准备
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 95860f57af52147b2c17129ac92f89bba72d0e810903272d051565cfdf99986b
  kind: experience
  material_digest: 2ebacbf692cde917fcbbda6d6423e0faf8b4240d39084174c13e70c005a83cfb
  revision: 5e8ad533c0bf8f26624edb577fbde2f636e7a34817785e41ea25d7be00f3e0c5
  schema: mindie-entry/3
  summary: 本批仅包含 1 个材料块，主题是 153 上 native r11 rank launcher 的部署与四机启停审计准备，以及 154–156 的缺失报告。后续检索可用 `native r11`、`fwd-h`、`OPP manifest`、`AppleDouble`、`bash -n`、`shellcheck`、`SHA256`、`r12`、`154–156`；部署和校验结论仍应视为来源报告，不能升级为已验证成功。
  title: 当前任务导航：native r11 launcher 审计准备
navigation: 本批仅包含 1 个材料块，主题是 153 上 native r11 rank launcher 的部署与四机启停审计准备，以及 154–156 的缺失报告。后续检索可用 `native r11`、`fwd-h`、`OPP manifest`、`AppleDouble`、`bash -n`、`shellcheck`、`SHA256`、`r12`、`154–156`；部署和校验结论仍应视为来源报告，不能升级为已验证成功。
schema: mindie-material-task/1
status: complete
task_id: 95860f57af52147b2c17129ac92f89bba72d0e810903272d051565cfdf99986b
---

# 当前任务导航：native r11 launcher 审计准备

本批仅包含 1 个材料块，主题是 153 上 native r11 rank launcher 的部署与四机启停审计准备，以及 154–156 的缺失报告。后续检索可用 `native r11`、`fwd-h`、`OPP manifest`、`AppleDouble`、`bash -n`、`shellcheck`、`SHA256`、`r12`、`154–156`；部署和校验结论仍应视为来源报告，不能升级为已验证成功。

## Materials

- [native r11 四机 launcher 与审计准备](blocks/b03549bca34e3f418c1b4e7ad9c145e7d45237fce60070c854f100223f32edaa.md): 消息报告：仅审计 153 上的 native r11 seed 与现有 r12 launcher，未启动或停止 r12，也未触碰 154–156；153 的 seed 被描述为完整 baseline OPP 加替换 fwd-h 的 4 组 `.o/.json`，共 979 个文件。rank launcher 和两份四机审计脚本据称通过 `bash -n`、`shellcheck`，153 已部署独立 launcher，并报告 source manifest、978 个 OPP runtime 文件及无 AppleDouble 校验通过；154–156 据称不存在 native r11 source/runtime。上述部署、SHA、命令参数一致性和审计结果均为来源中的报告，保留其未独立验证状态。
