---
blocks:
- block_id: cb22a3d896cebfe8c8eb559aea783df84f2e4f2c144886ff3f1b3695fb671bdb
  indexed: true
  sha256: d86c2600006a18927f42253f6c6c83a48005bc061e74536bb2bfc0da2812d05e
  source_range:
    end: 2661425
    part: 0
    start: 0
  summary: '围绕 PR #13277，修复了 vllm_ascend 源码中的 4 个 mypy 空值收窄问题，并提交 `4fd628914`；随后发现测试文件 `test_patch_balance_schedule.py:278` 的 AST 类型标注问题，再提交 `214a56771`。pre-commit、gitleaks、三版本源码/examples/tests mypy、CPU、310P 4 卡、310P 1 卡及 upstream 等项目陆续通过，但 A2/A3 硬件矩阵仍未全部收敛。期间完成 description 和评论脱敏，删除 7 条已被 history rewrite 淘汰的自有评论，保留 reviewer/bot 记录及 outdated review threads；本机因缺少 `wget/gitleaks` 未能完整复现相关检查。'
  title: 'PR #13277 CI 修复、脱敏与首轮硬件矩阵监控'
- block_id: 5c1752449b5a515c178d811ecf57e221d61e046f5038ef3fb23d14f49606cd41
  indexed: true
  sha256: c0f7cdd2ee86a905ad92feb50f5df3312e0a8befb3be56380a916e0ad4a47874
  source_range:
    end: 2661425
    part: 1
    start: 0
  summary: 首轮硬件矩阵中 A2 五个分片最终有四个通过，part 2 确定性失败，原因是 `EagleProposer._propose` 新增 `num_draft_tokens_cpu` 参数而单测仍使用旧签名，属于测试契约漏更而非 runner 故障。仅更新测试期望并提交 `840fe0dda`，本地 AST 契约对比及格式、ruff、codespell、typos 检查通过，但新 CI 尚未验证。旧 run 仍有 7 个 A3 job 排队且取消请求因仓库权限被拒绝；新 run `31502403318` 保持 pending，期间出现过一次 GitHub API 空响应，容错轮询后确认状态未变。
  title: A2 失败定位、测试契约修复与新 CI 排队
- block_id: 827c357c02f6150c6ece51d35adf9229709695c85f8f9918245ff730ab75455f
  indexed: true
  sha256: 35247c69916f07c13d16c81ba9bcfc1a8c10472f1f9aab289b3df49f3570f483
  source_range:
    end: 2661425
    part: 2
    start: 0
  summary: 截至该块，失败修复 head 未变化，新 run 仍未展开 jobs，旧轮 A3 继续排队。没有新的失败证据或成功结论，当前阻塞仍表现为 GitHub/硬件 runner 调度等待。该状态只是监控观察，不应视为 CI 已通过。
  title: 新 CI 仍处于并发与 runner 排队
- block_id: c7ebfe9a1fb7c7e53841f9ee00bddc77c7b364de879805ee5b93739bd1b63fa3
  indexed: true
  sha256: 4cd9b755d26e9332ac54f3dea66572813486e4d433c7e112e221d34d3738b278
  source_range:
    end: 3212226
    part: 0
    start: 2661425
  summary: 长时间无状态切换：旧轮仍有 7 个 A3 job queued，新 run 仍 pending、尚未创建 jobs；记录将等待归因于共享 A3/GitHub runner 并发调度，而非新的代码失败。用户随后要求停止定时任务，助手报告已停止本地 CI 轮询且无残留监控进程，并称未取消或修改 GitHub CI run；这些是报告结果，未提供独立验证。
  title: CI 轮询停止，硬件 runner 仍未验证
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 29d0d382bcf459f39536ade44942daece12665ec0d7ba0bca63bd842ea7f2be6
  kind: experience
  material_digest: f62e43cdd8421214f4ac2feb78a31f75063a6d093e81b17ebd3a50a5441fcd7f
  revision: a529cb8e873bc29f27f3785dec4fbbd3b2cb04328b3d741a293a344f61424802
  schema: mindie-entry/3
  summary: 此前已记录提交 `4fd628914`、`214a56771`、`840fe0dda`，以及部分静态检查和 CPU/NPU/upstream job 通过；A2 part 2 的旧测试签名失败及修复后的 run `31502403318` 仍未形成整轮成功结论，A3 仍受 runner/API 调度阻塞。新增材料显示旧轮 A3 queued、新 run pending 的状态长期未变，随后本地 CI 轮询据助手报告已停止；GitHub 上的 run 未被取消或修改，但硬件矩阵和修复仍未完成验证，不能将等待或部分通过升级为整体成功。
  title: 'PR #13277 CI 修复与验证：监控已停止，硬件矩阵仍未定论'
navigation: 此前已记录提交 `4fd628914`、`214a56771`、`840fe0dda`，以及部分静态检查和 CPU/NPU/upstream job 通过；A2 part 2 的旧测试签名失败及修复后的 run `31502403318` 仍未形成整轮成功结论，A3 仍受 runner/API 调度阻塞。新增材料显示旧轮 A3 queued、新 run pending 的状态长期未变，随后本地 CI 轮询据助手报告已停止；GitHub 上的 run 未被取消或修改，但硬件矩阵和修复仍未完成验证，不能将等待或部分通过升级为整体成功。
schema: mindie-material-task/1
status: complete
task_id: 29d0d382bcf459f39536ade44942daece12665ec0d7ba0bca63bd842ea7f2be6
---

# PR #13277 CI 修复与验证：监控已停止，硬件矩阵仍未定论

此前已记录提交 `4fd628914`、`214a56771`、`840fe0dda`，以及部分静态检查和 CPU/NPU/upstream job 通过；A2 part 2 的旧测试签名失败及修复后的 run `31502403318` 仍未形成整轮成功结论，A3 仍受 runner/API 调度阻塞。新增材料显示旧轮 A3 queued、新 run pending 的状态长期未变，随后本地 CI 轮询据助手报告已停止；GitHub 上的 run 未被取消或修改，但硬件矩阵和修复仍未完成验证，不能将等待或部分通过升级为整体成功。

## Materials

- [PR #13277 CI 修复、脱敏与首轮硬件矩阵监控](blocks/cb22a3d896cebfe8c8eb559aea783df84f2e4f2c144886ff3f1b3695fb671bdb.md): 围绕 PR #13277，修复了 vllm_ascend 源码中的 4 个 mypy 空值收窄问题，并提交 `4fd628914`；随后发现测试文件 `test_patch_balance_schedule.py:278` 的 AST 类型标注问题，再提交 `214a56771`。pre-commit、gitleaks、三版本源码/examples/tests mypy、CPU、310P 4 卡、310P 1 卡及 upstream 等项目陆续通过，但 A2/A3 硬件矩阵仍未全部收敛。期间完成 description 和评论脱敏，删除 7 条已被 history rewrite 淘汰的自有评论，保留 reviewer/bot 记录及 outdated review threads；本机因缺少 `wget/gitleaks` 未能完整复现相关检查。
- [A2 失败定位、测试契约修复与新 CI 排队](blocks/5c1752449b5a515c178d811ecf57e221d61e046f5038ef3fb23d14f49606cd41.md): 首轮硬件矩阵中 A2 五个分片最终有四个通过，part 2 确定性失败，原因是 `EagleProposer._propose` 新增 `num_draft_tokens_cpu` 参数而单测仍使用旧签名，属于测试契约漏更而非 runner 故障。仅更新测试期望并提交 `840fe0dda`，本地 AST 契约对比及格式、ruff、codespell、typos 检查通过，但新 CI 尚未验证。旧 run 仍有 7 个 A3 job 排队且取消请求因仓库权限被拒绝；新 run `31502403318` 保持 pending，期间出现过一次 GitHub API 空响应，容错轮询后确认状态未变。
- [新 CI 仍处于并发与 runner 排队](blocks/827c357c02f6150c6ece51d35adf9229709695c85f8f9918245ff730ab75455f.md): 截至该块，失败修复 head 未变化，新 run 仍未展开 jobs，旧轮 A3 继续排队。没有新的失败证据或成功结论，当前阻塞仍表现为 GitHub/硬件 runner 调度等待。该状态只是监控观察，不应视为 CI 已通过。
- [CI 轮询停止，硬件 runner 仍未验证](blocks/c7ebfe9a1fb7c7e53841f9ee00bddc77c7b364de879805ee5b93739bd1b63fa3.md): 长时间无状态切换：旧轮仍有 7 个 A3 job queued，新 run 仍 pending、尚未创建 jobs；记录将等待归因于共享 A3/GitHub runner 并发调度，而非新的代码失败。用户随后要求停止定时任务，助手报告已停止本地 CI 轮询且无残留监控进程，并称未取消或修改 GitHub CI run；这些是报告结果，未提供独立验证。
