---
blocks:
- block_id: ff973dbdb83c243aa3bd1f777748f20abfed6309ba883d120678215f31513fa1
  indexed: true
  sha256: 6581fc55d25fd07bd2de22c548c3ea23bece3fcf77b7a7d1cf610a635d88045b
  source_range:
    end: 667719
    part: 0
    start: 0
  summary: 来源报告称本地 R20 bundle 已冻结复核，包含 `R20_FROZEN=1`、pins SHA、manifest/runtime 严格通过、client UT 6/6、ShellCheck 与 `bash -n` 通过、33 targets byte-exact 重生成及 clean worktree；独立远端只读 readback 也被报告为通过，但未执行远端写操作、服务启停或 HTTP。另有 `clientfix2` 交付声明：四机部署、UT、manifest/readback 和 `PREFLIGHT_ONLY rc=0` 被报告为成功，HTTP 请求数为 0，唯一 client 命令仍等待 READY；这些均是来源中的报告性结果，不能视为本索引已验证。后续只读审查未修改文件或访问远端，识别出 59-record 拓扑约束（10 MLA、2 draft、42 Mamba、5 postprocess）、draft/5 的原始 scheduler block ID 与 FullAttention 384-token slot 公式，并指出 verifier、contract、builder、fixture、UT、manifest/pins 的联动缺口。
  title: R20 冻结交付与 draft/5 拓扑审查记录
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 0d545068d73741390cb1b386d2de6af147cacd5ff037d51c7d138cbed2dc4f7c
  kind: experience
  material_digest: 063bebb7b75ec6bb691e19d359ad7ac44a4442317869fde01ff0f9b334c80b88
  revision: 2276c6b93cf171b51ce0818e0ebdb95f9cfb26816982a34917dc87d9abf5a922
  schema: mindie-entry/3
  summary: 本批材料集中记录两类内容：一是 R20/clientfix2 的冻结、部署、测试、readback 与无 HTTP/无启停声明；二是针对 59-record 拓扑和 draft/5 `block_tables`、`slot_mapping` 公式的只读审查。成功状态均保留为来源报告，未升级为独立验证；后续检索应同时关注 clientfix1 失败证据、未执行的唯一 client 命令，以及 verifier/contract/builder/fixture/UT/manifests 的未完成联动。原始块可独立检索，未重建其中被省略的路径或身份信息。
  title: 当前任务导航：R20 交付声明与 draft/5 审查
navigation: 本批材料集中记录两类内容：一是 R20/clientfix2 的冻结、部署、测试、readback 与无 HTTP/无启停声明；二是针对 59-record 拓扑和 draft/5 `block_tables`、`slot_mapping` 公式的只读审查。成功状态均保留为来源报告，未升级为独立验证；后续检索应同时关注 clientfix1 失败证据、未执行的唯一 client 命令，以及 verifier/contract/builder/fixture/UT/manifests 的未完成联动。原始块可独立检索，未重建其中被省略的路径或身份信息。
schema: mindie-material-task/1
status: complete
task_id: 0d545068d73741390cb1b386d2de6af147cacd5ff037d51c7d138cbed2dc4f7c
---

# 当前任务导航：R20 交付声明与 draft/5 审查

本批材料集中记录两类内容：一是 R20/clientfix2 的冻结、部署、测试、readback 与无 HTTP/无启停声明；二是针对 59-record 拓扑和 draft/5 `block_tables`、`slot_mapping` 公式的只读审查。成功状态均保留为来源报告，未升级为独立验证；后续检索应同时关注 clientfix1 失败证据、未执行的唯一 client 命令，以及 verifier/contract/builder/fixture/UT/manifests 的未完成联动。原始块可独立检索，未重建其中被省略的路径或身份信息。

## Materials

- [R20 冻结交付与 draft/5 拓扑审查记录](blocks/ff973dbdb83c243aa3bd1f777748f20abfed6309ba883d120678215f31513fa1.md): 来源报告称本地 R20 bundle 已冻结复核，包含 `R20_FROZEN=1`、pins SHA、manifest/runtime 严格通过、client UT 6/6、ShellCheck 与 `bash -n` 通过、33 targets byte-exact 重生成及 clean worktree；独立远端只读 readback 也被报告为通过，但未执行远端写操作、服务启停或 HTTP。另有 `clientfix2` 交付声明：四机部署、UT、manifest/readback 和 `PREFLIGHT_ONLY rc=0` 被报告为成功，HTTP 请求数为 0，唯一 client 命令仍等待 READY；这些均是来源中的报告性结果，不能视为本索引已验证。后续只读审查未修改文件或访问远端，识别出 59-record 拓扑约束（10 MLA、2 draft、42 Mamba、5 postprocess）、draft/5 的原始 scheduler block ID 与 FullAttention 384-token slot 公式，并指出 verifier、contract、builder、fixture、UT、manifest/pins 的联动缺口。
