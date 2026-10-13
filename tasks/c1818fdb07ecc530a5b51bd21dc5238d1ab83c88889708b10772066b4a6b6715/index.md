---
blocks:
- block_id: b264b08a8f2420342796fbab0c41d89a4880fc183c36e531e09a837d5625a362
  indexed: true
  sha256: 9c80971c91a3884a27220bb19ff151dfc430dea9fdf261b0f577eef3012b02b3
  source_range:
    end: 15387199
    part: 0
    start: 0
  summary: '基于 main 的 K3 MRV2 适配修复了 `cache_dtype_str` 接口、Ascend split-KV 清零、MLA/GQA 混合页、Mamba 包装分组识别及 speculative state 槽位分配，并补齐调度侧 `AscendMLAAttentionSpec` 的 block 清零链路。CPU/metadata、NPU 清零、格式检查及无草稿 eager/ACLGraph 真实请求通过；DSpark 曾因 Mamba 分组识别和 block table 少分配 7 个槽位导致错误，短请求修复后对齐，但 32-token 热缓存仍出现 token 分歧，完整 DSpark 支持未验证通过。最终创建并 rebase 后提交 PR #15199（提交 `e64e8a965`），文档与 PR 描述仅保留最终用法、验证结论和限制。'
  title: MRV2 适配与 DSpark 热缓存一致性排查
- block_id: 263779698ec027811509b671cc969fd33e973c6b3e3c1923fe4b70d3f793e968
  indexed: true
  sha256: 2a6b3873154fd9936d1e111179545da393aeed065661a7692a94dd61d19347d2
  source_range:
    end: 15387199
    part: 1
    start: 0
  summary: '最终版本确认真实请求触发了 16 个 rank 的 block 清零；无草稿 ACLGraph 的 17 个真实请求通过，32-token prefix 冷/热/重置一致，缓存命中为 `0 / 1536 / 0`，并覆盖边界和并发。DSpark 热缓存 token 不一致仍明确为未通过项，不能因清零链路接通或无草稿路径通过而宣称完整支持。相关验收还记录了 154 上 PR #66 的资源已释放，以及本任务未占用 154、未知归属进程不可擅自停止。'
  title: 最终 MRV2 回归与资源边界
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: c1818fdb07ecc530a5b51bd21dc5238d1ab83c88889708b10772066b4a6b6715
  kind: experience
  material_digest: 2ce4714f4a4712fdccc39901b3bb708ef1b2fa7033b11223462b8f700a74b11b
  revision: fe31bfe049aaffee2a8bb28bc03bdeb7200b1d9254c00ef1123a8b95dbc12ed8
  schema: mindie-entry/3
  summary: '主线是已 rebase 到 main 的 K3 MRV2 hybrid cache PR #15199（提交 `e64e8a965`）：清零链路、Mamba/speculative state 初始化和无草稿 eager/ACLGraph 回归已有证据支持。必须保留的限制是 DSpark 热缓存一致性仍未通过；此前失败、修复前后差异和资源归属不确定性均不能被后续成功结果覆盖。'
  title: '当前任务导航：K3 MRV2 PR #15199'
navigation: '主线是已 rebase 到 main 的 K3 MRV2 hybrid cache PR #15199（提交 `e64e8a965`）：清零链路、Mamba/speculative state 初始化和无草稿 eager/ACLGraph 回归已有证据支持。必须保留的限制是 DSpark 热缓存一致性仍未通过；此前失败、修复前后差异和资源归属不确定性均不能被后续成功结果覆盖。'
schema: mindie-material-task/1
status: complete
task_id: c1818fdb07ecc530a5b51bd21dc5238d1ab83c88889708b10772066b4a6b6715
---

# 当前任务导航：K3 MRV2 PR #15199

主线是已 rebase 到 main 的 K3 MRV2 hybrid cache PR #15199（提交 `e64e8a965`）：清零链路、Mamba/speculative state 初始化和无草稿 eager/ACLGraph 回归已有证据支持。必须保留的限制是 DSpark 热缓存一致性仍未通过；此前失败、修复前后差异和资源归属不确定性均不能被后续成功结果覆盖。

## Materials

- [MRV2 适配与 DSpark 热缓存一致性排查](blocks/b264b08a8f2420342796fbab0c41d89a4880fc183c36e531e09a837d5625a362.md): 基于 main 的 K3 MRV2 适配修复了 `cache_dtype_str` 接口、Ascend split-KV 清零、MLA/GQA 混合页、Mamba 包装分组识别及 speculative state 槽位分配，并补齐调度侧 `AscendMLAAttentionSpec` 的 block 清零链路。CPU/metadata、NPU 清零、格式检查及无草稿 eager/ACLGraph 真实请求通过；DSpark 曾因 Mamba 分组识别和 block table 少分配 7 个槽位导致错误，短请求修复后对齐，但 32-token 热缓存仍出现 token 分歧，完整 DSpark 支持未验证通过。最终创建并 rebase 后提交 PR #15199（提交 `e64e8a965`），文档与 PR 描述仅保留最终用法、验证结论和限制。
- [最终 MRV2 回归与资源边界](blocks/263779698ec027811509b671cc969fd33e973c6b3e3c1923fe4b70d3f793e968.md): 最终版本确认真实请求触发了 16 个 rank 的 block 清零；无草稿 ACLGraph 的 17 个真实请求通过，32-token prefix 冷/热/重置一致，缓存命中为 `0 / 1536 / 0`，并覆盖边界和并发。DSpark 热缓存 token 不一致仍明确为未通过项，不能因清零链路接通或无草稿路径通过而宣称完整支持。相关验收还记录了 154 上 PR #66 的资源已释放，以及本任务未占用 154、未知归属进程不可擅自停止。
