---
blocks:
- block_id: 163b12755a1efb6a52e7b69b83f80a2078b6200389fae4518f45bd66e78a5e60
  indexed: true
  sha256: 9199afe7014928cb3a3811a1e48e413c7f99da3485f65acc68f1b8b067b9f655
  source_range:
    end: 1068590
    part: 0
    start: 0
  summary: 此前产出两个未部署的候选：严格 completion-only A/B（a9e34e17）与 production seam（eb788310），CPU 定向测试分别为 2 passed 和 6 passed；静态根因指向 Ascend side-stream 后处理缺少 completion consumption，scheduler 随即 publish/free prefix blocks，f511 及后续提交未修复该缺口。随后对 four-plane 相关最终 SHA 391fc18e 进行审查，确认 parent、5-file 范围及偏移语义自洽，且未改动 Triton、zeroer、async、DFX；但因布局识别仅筛选部分 spec、却对全部 spec 扩大 page_size_padded，并且 mismatch 测试接受旧 shared-layout fallback，结论为 BLOCK，暂不建议 force-with-lease。最小修正方向是要求 spec 全量满足 family 几何约束，进入该 family 但几何不匹配时 fail closed。
  title: Ascend stream-handoff 候选与 four-plane 发布审查
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 3d12edb5561db531734549a133bd7fd46ab142fed7dcbd3b87f81283bf0cdf10
  kind: experience
  material_digest: dc833d87d49c43c1af32190e75901e5bfa6e94c730227e58fdbc762f45a5bae8
  revision: 10a06bb0d2e58c7153edb10778fe5d4482897762eb4dafb7a955f35bb42911fb
  schema: mindie-entry/3
  summary: 可检索上下文包括：未部署的 completion-only A/B 候选 a9e34e17 与 production seam eb788310；side-stream completion consumption 缺失及 publish/free prefix blocks 的静态根因；以及最终 SHA 391fc18e 的 BLOCK 结论。后续审查确认 four-plane 偏移语义和文件范围自洽，但发现未识别 spec 被错误扩大 page_size_padded、shared-layout fallback 可能恢复 compact GQA SoA alias；建议全量几何校验并在 mismatch 时 fail closed，发布状态仍未通过。
  title: 当前任务导航：Ascend stream-handoff 与 four-plane 发布阻断
navigation: 可检索上下文包括：未部署的 completion-only A/B 候选 a9e34e17 与 production seam eb788310；side-stream completion consumption 缺失及 publish/free prefix blocks 的静态根因；以及最终 SHA 391fc18e 的 BLOCK 结论。后续审查确认 four-plane 偏移语义和文件范围自洽，但发现未识别 spec 被错误扩大 page_size_padded、shared-layout fallback 可能恢复 compact GQA SoA alias；建议全量几何校验并在 mismatch 时 fail closed，发布状态仍未通过。
schema: mindie-material-task/1
status: complete
task_id: 3d12edb5561db531734549a133bd7fd46ab142fed7dcbd3b87f81283bf0cdf10
---

# 当前任务导航：Ascend stream-handoff 与 four-plane 发布阻断

可检索上下文包括：未部署的 completion-only A/B 候选 a9e34e17 与 production seam eb788310；side-stream completion consumption 缺失及 publish/free prefix blocks 的静态根因；以及最终 SHA 391fc18e 的 BLOCK 结论。后续审查确认 four-plane 偏移语义和文件范围自洽，但发现未识别 spec 被错误扩大 page_size_padded、shared-layout fallback 可能恢复 compact GQA SoA alias；建议全量几何校验并在 mismatch 时 fail closed，发布状态仍未通过。

## Materials

- [Ascend stream-handoff 候选与 four-plane 发布审查](blocks/163b12755a1efb6a52e7b69b83f80a2078b6200389fae4518f45bd66e78a5e60.md): 此前产出两个未部署的候选：严格 completion-only A/B（a9e34e17）与 production seam（eb788310），CPU 定向测试分别为 2 passed 和 6 passed；静态根因指向 Ascend side-stream 后处理缺少 completion consumption，scheduler 随即 publish/free prefix blocks，f511 及后续提交未修复该缺口。随后对 four-plane 相关最终 SHA 391fc18e 进行审查，确认 parent、5-file 范围及偏移语义自洽，且未改动 Triton、zeroer、async、DFX；但因布局识别仅筛选部分 spec、却对全部 spec 扩大 page_size_padded，并且 mismatch 测试接受旧 shared-layout fallback，结论为 BLOCK，暂不建议 force-with-lease。最小修正方向是要求 spec 全量满足 family 几何约束，进入该 family 但几何不匹配时 fail closed。
