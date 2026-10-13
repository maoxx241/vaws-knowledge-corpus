---
blocks:
- block_id: aa3f80a6d9dc3860198db7b41200ec901135ee30392583b8219763fd6544f0ed
  indexed: true
  sha256: c00a350d8c59192569a7af0fec07a75b74d98f8d56885476c0a797557c164fec
  source_range:
    end: 8734054
    part: 0
    start: 0
  summary: '完成了基于旧 head 的 GSM8K 精确压测：8K prefix-off 测试 GQA/MLA 的 c1/c2/c4，128K prefix-on 测试 c1/c2；记录了 TTFT、TPOT、ITL、平均接收长度及实际 prefix hit，并将结果贴到 PR #13277。128K 实际命中率为 98.73%–99.61%，c4 因单 DP 容量限制跳过；后续又按 Kimi-K3 文档格式补充了 MLA 四机启动命令。该批结果属于旧 head，不能当作后续新 head 的验证结果。'
  title: PR13277 旧版 GQA/MLA 性能结果与启动命令
- block_id: d0c66e6536ebeacdf31f203df2457368fab526f9ae25d93183e0cc59291824d6
  indexed: true
  sha256: e91dbbddb967e0759842e5cdb7041e3ed900320359df5555b23b91aef0d0296a
  source_range:
    end: 8734054
    part: 1
    start: 0
  summary: 任务先后从 659237aea 切换到 fe21e50、f392a6e9，再切换到 1619977；期间发现并保留了多个失败证据。fe21e50 暴露默认 CUSTOM_OPS_ARRAY 漏含 attn_res_fwd，f392a6e9 的默认构建又因 `op_host/tiling_base.h` 缺失失败；1619977 修复 include 后启动了四机默认 custom-op 重建，但尚未完成后续验证。所有过期 head 均被明确排除，不应写入最终性能或 PR 结果。
  title: 新算子 head 多次切换及构建暂停记录
- block_id: 51825ffdb05e0244bce74562a5fd0cc58392feef22f442e01d9c601a06cd0f0f
  indexed: true
  sha256: f2750dc93b2e9b417e29fe1fb1721a9e52b4a8b66693dfa0c1abaf2813a3ffaa
  source_range:
    end: 8734054
    part: 2
    start: 0
  summary: 当前唯一有效 head 为 a38407257991f9d5fdfd9e14a80180bdc7c82871；复用了已验证的 extension/kernel/custom vendor，并完成默认 packaging 校验及 10/10 单卡验证，包含 AttnResFwd、sigmoid fused_norm_gate 两种 gate 语义和 MLA NoPE。GQA 的 8K 与 128K 两组性能已完成，MLA prefix-off 的 4 道数学 smoke 为 4/4 通过，8K c1/c2/c4 也完成；最终 MLA prefix-on 服务仍在加载权重，最新进度为 304/352（86%），尚未产生最终 128K MLA 数据或 PR 更新。过程中还保留了 Docker detached 启动包装错误、shared-memory capture warning 和 runner 目录创建错误等非最终失败记录。
  title: a384 exact-head 算子验证与性能任务进行中
- block_id: 5459813e333abbfd41a28392c5c0013a8d71d637f432560d1b71a748d7fd8043
  indexed: true
  sha256: a3e8e6f388313a7b5210ae4379df97833e18efc92143b3aefbd6138080217a7e
  source_range:
    end: 8734054
    part: 3
    start: 0
  summary: a384 head 的 GQA/MLA 性能、单卡算子和数学 smoke 已完成，PR 描述及原性能评论已通过本地 gh 更新；MLA prefix-on 服务保持在 18077，相关健康和 fatal 扫描为通过状态。随后针对 MLA 8K/1K/c1 做三组四机 prefix-off 消融：no_attn_res 恢复旧 AL=5.23469387755102，与旧基线及相同 prompt hash 一致；no_fused_norm_gate 和 no_mla_preprocess 均保持 AL=3.8754716981132074，因此报告的主因是 AttnResFwd 接线。该结论来自保留的实验结果和回读断言，不能扩展为其他未测试场景的验证。
  title: a384 完整验证与三算子消融
- block_id: c5a15433820f19e166603fb2fd092e96c99d3eb9c74ea2070143a803c7a04200
  indexed: true
  sha256: 9a64cd1b40801ffc990550b85b65135e31d189822d0e4f5e501852ff0ae40c5b
  source_range:
    end: 8734054
    part: 4
    start: 0
  summary: 主线程已删除 AttnResFwd 并推送新 head `cfaebdd6a9b3f139ee85211e58ee203af2abe5e7`；C 组 no_mla_preprocess 服务归档审计通过后已按记录 PGID 精确关闭。四节点 final-cfaebdd source/overlay/runtime/result 路径、源码移除 AttnResFwd 扫描、与 `05ad3f21` reference 的 residual 函数逐字一致性，以及复用 a384/161 native 兼容超集的计划已检查；这不是 final native rebuild。当前已开始启动 final head 的 MLA prefix-off 服务，但新服务的 health、唯一 8K/1K/c1 结果、哈希和最终归档尚未在该块中完成。
  title: cfaebdd6 最终单 case 验证启动
- block_id: cf4faa71efe5c6ab57f708ea0c30addfc558162bfdddf3387910320753be6d1f
  indexed: true
  sha256: 86d2cc1c75c742ca97f0c582a562870a4dfd1a3acb52bd11b4105ce8cc88e1ec
  source_range:
    end: 11986851
    part: 0
    start: 8734054
  summary: 材料报告 cfaebdd6 的 MLA/GQA、8K/128K、prefix-off/on 性能波次及四 rank 服务审计已完成，包含 accepted/drafts、AL、TTFT/TPOT/ITL、prefix hit、hash、PGID、health/fatal 和产物 SHA；同时记录 PR head 后移至 9d442fb1，但结果仍标记为实际运行的 cfaebdd6。随后发现此前所谓刷新 GQA 权重实际仍是旧路径 `/mnt/weight/k3/Kimi-K3-DSpark`，该轮结果被材料明确作废；在清理 qjj_dspark 服务并处理显存不足重试后，重新定位到长序列候选 `Kimi-K3-DSpark-GQA-20260801`，但这些均属于来源中的报告性检查与结论。
  title: cfaebdd6 完整性能矩阵与 GQA 权重复测前置
- block_id: 05e15b59d6542832fad5519a95d1858db8e49e92be153de9f888792ae4e135ec
  indexed: true
  sha256: 157c57f019624079e45a0f05ef65a20850967a019576910ee6bdf8b18b149adf
  source_range:
    end: 11986851
    part: 1
    start: 8734054
  summary: 材料报告四节点使用最新 GQA 权重启动 cfaebdd6、GQA prefix-off 服务；首次启动因每机仅剩约 3.29/61.27 GiB 而失败，用户随后授权独占四机并清理明确的 qjj_dspark PGID。后续 r2 服务据称加载了权重并完成 exact GSM8K 8192/1024、c1 测试，报告 AL 5.382198952879581、per-token acceptance 62.602842%、accepted/drafts 837/191，且与旧权重输出 hash 一致；同时记录了具体权重/config SHA、结果文件 SHA 和保活 PGID，但仍应视为可检索的报告结果。
  title: 刷新 GQA 权重短序列复测
- block_id: ca118e94e4b8f39202edd506e410a949010136aa50e3de55fcc98cf003abfdf9
  indexed: true
  sha256: cc2ce276617c35c8680b604bef8bcdc37734df34b6cb0c5cd1cd50f5ad301348
  source_range:
    end: 11986851
    part: 2
    start: 8734054
  summary: '材料随后修正权重路径为 `Kimi-K3-DSpark-GQA-20260801`（epoch=10、step=1150、long-context/1M 标记），报告 8K/off/c1 AL 6.5287、per-token acceptance 78.98%，以及 128K/on/c1 AL 4.1245、per-token acceptance 44.64%，prefix hit 98.7305%；此前旧路径结果明确作废。材料还报告已将修正区块写入 PR #13277 的评论 `issuecomment-5154570812`，并称写入前后 head 为 9d442fb1；评论更新、运行结果和质量门禁均应保留为来源声称，不能升级为独立核验事实。'
  title: 长序列 GQA 权重复测与 PR 评论更新
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: b22cc1d74932da059115f71fa647370ce76ecfb54317c312e0b3a117cae86d49
  kind: experience
  material_digest: cd5aabadefb49e1b058604f648a2dedcaacaa0243d00bb8386a2725cd330b846
  revision: e9160d21c67a1582e0401c2ff51c524b699999b8ca63e93df467403dc3542352
  schema: mindie-entry/3
  summary: '后续材料已修正此前“最新 GQA 权重”误用旧路径的问题：旧 `/mnt/weight/k3/Kimi-K3-DSpark` 复测结果作废，材料改指向 `Kimi-K3-DSpark-GQA-20260801`，并报告 8K AL 6.5287、128K AL 4.1245、128K prefix hit 98.7305% 及对应采信率提升。材料还报告已在 PR #13277 的现有性能评论 `issuecomment-5154570812` 追加长序列 GQA 权重修正，并记录 head 为 9d442fb1；原导航中 cfaebdd6 MLA 单 case 尚未完成的状态已被后续材料覆盖，但所有运行、服务健康、结果 hash 与评论写入仍是来源报告，原始区块应独立检索，不能视为本导航自行验证。'
  title: 当前任务导航：GQA 长序列权重已修正并据材料更新 PR
navigation: '后续材料已修正此前“最新 GQA 权重”误用旧路径的问题：旧 `/mnt/weight/k3/Kimi-K3-DSpark` 复测结果作废，材料改指向 `Kimi-K3-DSpark-GQA-20260801`，并报告 8K AL 6.5287、128K AL 4.1245、128K prefix hit 98.7305% 及对应采信率提升。材料还报告已在 PR #13277 的现有性能评论 `issuecomment-5154570812` 追加长序列 GQA 权重修正，并记录 head 为 9d442fb1；原导航中 cfaebdd6 MLA 单 case 尚未完成的状态已被后续材料覆盖，但所有运行、服务健康、结果 hash 与评论写入仍是来源报告，原始区块应独立检索，不能视为本导航自行验证。'
schema: mindie-material-task/1
status: complete
task_id: b22cc1d74932da059115f71fa647370ce76ecfb54317c312e0b3a117cae86d49
---

# 当前任务导航：GQA 长序列权重已修正并据材料更新 PR

后续材料已修正此前“最新 GQA 权重”误用旧路径的问题：旧 `/mnt/weight/k3/Kimi-K3-DSpark` 复测结果作废，材料改指向 `Kimi-K3-DSpark-GQA-20260801`，并报告 8K AL 6.5287、128K AL 4.1245、128K prefix hit 98.7305% 及对应采信率提升。材料还报告已在 PR #13277 的现有性能评论 `issuecomment-5154570812` 追加长序列 GQA 权重修正，并记录 head 为 9d442fb1；原导航中 cfaebdd6 MLA 单 case 尚未完成的状态已被后续材料覆盖，但所有运行、服务健康、结果 hash 与评论写入仍是来源报告，原始区块应独立检索，不能视为本导航自行验证。

## Materials

- [PR13277 旧版 GQA/MLA 性能结果与启动命令](blocks/aa3f80a6d9dc3860198db7b41200ec901135ee30392583b8219763fd6544f0ed.md): 完成了基于旧 head 的 GSM8K 精确压测：8K prefix-off 测试 GQA/MLA 的 c1/c2/c4，128K prefix-on 测试 c1/c2；记录了 TTFT、TPOT、ITL、平均接收长度及实际 prefix hit，并将结果贴到 PR #13277。128K 实际命中率为 98.73%–99.61%，c4 因单 DP 容量限制跳过；后续又按 Kimi-K3 文档格式补充了 MLA 四机启动命令。该批结果属于旧 head，不能当作后续新 head 的验证结果。
- [新算子 head 多次切换及构建暂停记录](blocks/d0c66e6536ebeacdf31f203df2457368fab526f9ae25d93183e0cc59291824d6.md): 任务先后从 659237aea 切换到 fe21e50、f392a6e9，再切换到 1619977；期间发现并保留了多个失败证据。fe21e50 暴露默认 CUSTOM_OPS_ARRAY 漏含 attn_res_fwd，f392a6e9 的默认构建又因 `op_host/tiling_base.h` 缺失失败；1619977 修复 include 后启动了四机默认 custom-op 重建，但尚未完成后续验证。所有过期 head 均被明确排除，不应写入最终性能或 PR 结果。
- [a384 exact-head 算子验证与性能任务进行中](blocks/51825ffdb05e0244bce74562a5fd0cc58392feef22f442e01d9c601a06cd0f0f.md): 当前唯一有效 head 为 a38407257991f9d5fdfd9e14a80180bdc7c82871；复用了已验证的 extension/kernel/custom vendor，并完成默认 packaging 校验及 10/10 单卡验证，包含 AttnResFwd、sigmoid fused_norm_gate 两种 gate 语义和 MLA NoPE。GQA 的 8K 与 128K 两组性能已完成，MLA prefix-off 的 4 道数学 smoke 为 4/4 通过，8K c1/c2/c4 也完成；最终 MLA prefix-on 服务仍在加载权重，最新进度为 304/352（86%），尚未产生最终 128K MLA 数据或 PR 更新。过程中还保留了 Docker detached 启动包装错误、shared-memory capture warning 和 runner 目录创建错误等非最终失败记录。
- [a384 完整验证与三算子消融](blocks/5459813e333abbfd41a28392c5c0013a8d71d637f432560d1b71a748d7fd8043.md): a384 head 的 GQA/MLA 性能、单卡算子和数学 smoke 已完成，PR 描述及原性能评论已通过本地 gh 更新；MLA prefix-on 服务保持在 18077，相关健康和 fatal 扫描为通过状态。随后针对 MLA 8K/1K/c1 做三组四机 prefix-off 消融：no_attn_res 恢复旧 AL=5.23469387755102，与旧基线及相同 prompt hash 一致；no_fused_norm_gate 和 no_mla_preprocess 均保持 AL=3.8754716981132074，因此报告的主因是 AttnResFwd 接线。该结论来自保留的实验结果和回读断言，不能扩展为其他未测试场景的验证。
- [cfaebdd6 最终单 case 验证启动](blocks/c5a15433820f19e166603fb2fd092e96c99d3eb9c74ea2070143a803c7a04200.md): 主线程已删除 AttnResFwd 并推送新 head `cfaebdd6a9b3f139ee85211e58ee203af2abe5e7`；C 组 no_mla_preprocess 服务归档审计通过后已按记录 PGID 精确关闭。四节点 final-cfaebdd source/overlay/runtime/result 路径、源码移除 AttnResFwd 扫描、与 `05ad3f21` reference 的 residual 函数逐字一致性，以及复用 a384/161 native 兼容超集的计划已检查；这不是 final native rebuild。当前已开始启动 final head 的 MLA prefix-off 服务，但新服务的 health、唯一 8K/1K/c1 结果、哈希和最终归档尚未在该块中完成。
- [cfaebdd6 完整性能矩阵与 GQA 权重复测前置](blocks/cf4faa71efe5c6ab57f708ea0c30addfc558162bfdddf3387910320753be6d1f.md): 材料报告 cfaebdd6 的 MLA/GQA、8K/128K、prefix-off/on 性能波次及四 rank 服务审计已完成，包含 accepted/drafts、AL、TTFT/TPOT/ITL、prefix hit、hash、PGID、health/fatal 和产物 SHA；同时记录 PR head 后移至 9d442fb1，但结果仍标记为实际运行的 cfaebdd6。随后发现此前所谓刷新 GQA 权重实际仍是旧路径 `/mnt/weight/k3/Kimi-K3-DSpark`，该轮结果被材料明确作废；在清理 qjj_dspark 服务并处理显存不足重试后，重新定位到长序列候选 `Kimi-K3-DSpark-GQA-20260801`，但这些均属于来源中的报告性检查与结论。
- [刷新 GQA 权重短序列复测](blocks/05e15b59d6542832fad5519a95d1858db8e49e92be153de9f888792ae4e135ec.md): 材料报告四节点使用最新 GQA 权重启动 cfaebdd6、GQA prefix-off 服务；首次启动因每机仅剩约 3.29/61.27 GiB 而失败，用户随后授权独占四机并清理明确的 qjj_dspark PGID。后续 r2 服务据称加载了权重并完成 exact GSM8K 8192/1024、c1 测试，报告 AL 5.382198952879581、per-token acceptance 62.602842%、accepted/drafts 837/191，且与旧权重输出 hash 一致；同时记录了具体权重/config SHA、结果文件 SHA 和保活 PGID，但仍应视为可检索的报告结果。
- [长序列 GQA 权重复测与 PR 评论更新](blocks/ca118e94e4b8f39202edd506e410a949010136aa50e3de55fcc98cf003abfdf9.md): 材料随后修正权重路径为 `Kimi-K3-DSpark-GQA-20260801`（epoch=10、step=1150、long-context/1M 标记），报告 8K/off/c1 AL 6.5287、per-token acceptance 78.98%，以及 128K/on/c1 AL 4.1245、per-token acceptance 44.64%，prefix hit 98.7305%；此前旧路径结果明确作废。材料还报告已将修正区块写入 PR #13277 的评论 `issuecomment-5154570812`，并称写入前后 head 为 9d442fb1；评论更新、运行结果和质量门禁均应保留为来源声称，不能升级为独立核验事实。
