---
blocks:
- block_id: 29afab0c409ad0c2b9a172e937ccafd16016c4099c58cee856797037f79f89b9
  indexed: true
  sha256: da6e64d13af38f5a431f4049947d8bbc1299f7b588d235730e08700f18674f4a
  source_range:
    end: 3179441
    part: 0
    start: 0
  summary: 记录了对 remote-code-parity、repo-init 等 skill 的修改：合并 sync_mode 与首次 editable 安装授权、补充 CANN 9.0 环境发现、优先解析 verified commit，并让未设置同步模式时 fail closed。随后移除默认 uv 安装/bootstrap，改为固定 pip 路径；这些本地语法、shell 和行为检查通过，但没有执行远端 NPU 或真实 editable 安装。后续用户认为仅去掉 uv 仍不足，要求基于远端环境收敛到最快的单一路径。
  title: 同步安装路径与 uv 清理
- block_id: 1a22ff56572e09baccf7034f1b53002ee549c34582a4a7361c31c285cffeada7
  indexed: true
  sha256: ccf10f0b7c86a8caf294c15c960a09d7ff552daba354e4aadc7cad94fff0dab6
  source_range:
    end: 3179441
    part: 1
    start: 0
  summary: 按用户通过 define-goal 提出的目标，对 A3 容器进行了环境和源探测；记录的结果是 HuaweiCloud pip 对 llguidance 约 1.094 秒、NJU apt 约 212ms，清华源出现 403、PyPI 超时，因此方案收敛为固定单源并删除多源 fallback、自动依赖修复和 uv。还发现 vLLM metadata 阶段可能自动加载 torch_npu，缺少完整 Ascend 环境时会失败；后续测试转向 TORCH_DEVICE_BACKEND_AUTOLOAD=0。该块中的远端 ready、torch_npu smoke 和静态验证被报告为通过，但这些仍是来源中的报告，不能视为独立核验。
  title: 远端单源方案与首次编译探查
- block_id: de2492a09db0f291e2651e5011c4a9d3308603fdfb1ef21fe79b9ebaf44876e8
  indexed: true
  sha256: 9ffe87c53699cf30644ecb842c772cc194e9babc0eb2cd8f223f4c6d35f47c56
  source_range:
    end: 3179441
    part: 2
    start: 0
  summary: 来源报告 vLLM 使用 `TORCH_DEVICE_BACKEND_AUTOLOAD=0`、`--no-deps` 后约 9 秒，避免了约 26 秒安装造成的 numpy 2.3.5 污染；vllm-ascend 冷态 native/protobuf 构建超过 630 秒后被终止，热缓存并设置 `MAX_JOBS=128` 与 `CMAKE_BUILD_PARALLEL_LEVEL=128` 的路径约 118 秒完成，但这些结果包含实验中止、缓存复用和环境扰动，不能直接当作稳定基准。来源还称已将并行度默认上限设为 128，并建议可试 256；最新明确任务是实测比较 256 与 128，而该比较在本块结束时尚未执行。远端已有 editable 安装但缺少 parity marker/runtime-state，也被记录为可能导致后续重复冷编译的未完成状态风险。
  title: vLLM 编译耗时与并行度对比待办
- block_id: 6100ca53fd3423d63fa5268bdbe0e9e33edf327a01f45525e1935d00cfe14cb7
  indexed: true
  sha256: 3943539486785e648dc0ebd4ee21b4b6b487c741bab2e919519eb400bb45ae3e
  source_range:
    end: 3179441
    part: 3
    start: 0
  summary: 在同一台 A3 远端和热缓存条件下，`MAX_JOBS/CMAKE_BUILD_PARALLEL_LEVEL=256` 的 vllm-ascend editable install 用时 120 秒，`128` 用时 118 秒；smoke 检查报告 `smoke=ok`，但该结果仍属于特定环境下的观测，不能视为普遍性能结论。随后整理 skill 修改并创建 draft PR，涉及固定 HuaweiCloud pip、移除 uv/fallback、editable 安装使用 `--no-deps`、并发上限 128 及 pin 解析脚本；PR 曾因基于旧 `main` 返回 `CONFLICTING`，计划在临时 worktree 中保留 main 的新同步能力并重新套用安装策略，当前过程和合并结果并未全部完成验证。
  title: 并发实测与安装策略 PR 整理
- block_id: 371d8331892efa8f1ecef1f3d24266df0f8b60b159c04595c32266855b69ca51
  indexed: true
  sha256: d1b5b976d2a8c38e1c69fcd8dad8fe5fda3f87a02aa2f89e94ea0ce1bcb221c5
  source_range:
    end: 3637647
    part: 0
    start: 3179441
  summary: '材料记录了在临时 worktree 中解决冲突并统一远端安装策略：移除 uv、fallback、镜像循环和自动依赖修复，改用 HuaweiCloud 单一 pip 源、`--no-deps` 与 128 并发上限，同时保留 session/remote toolbox、cache 等 main 上的更新。期间验证发现 `manage_machine.py` 残留旧的 `/etc/pip.conf` 多源模板和 pytest opportunistic install，随后修正；Python 编译、diff 检查、生成脚本检查及 vLLM CI pin 解析据报道通过。后续提交 `8a074c4` 被推送并据报道使 PR #56 达到 `MERGEABLE/CLEAN`，PR 从 draft 标记 ready 后以 squash merge 合入，main 指向 `984462f`、远端分支删除；这些状态来自材料中的报告，不能独立视为已验证事实。'
  title: '冲突整合、安装策略收敛与 PR #56 合入'
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: f14c6354c103652bba4b8742d4507d0e139571c03de4ef84ebe4bff44094afb5
  kind: experience
  material_digest: ebeb27ee7801f90767e870df2af092628ef4a70ad2128ee2d6e70371db47209e
  revision: 77385393a3c48549fe90b9c0037b25420b95192df0ea1cfd348dae371d1d09a4
  schema: mindie-entry/3
  summary: '当前材料从旧 main 冲突与未确认的 PR 状态推进到一份据报道已完成的冲突整合、验证、PR #56 合入记录：安装方案收敛为 HuaweiCloud pip-only、`--no-deps`、无 uv/fallback，并保留 bundle/session/lock 同步能力；曾发现并修正 `manage_machine.py` 的旧多源 pip 配置和 pytest 安装残留。此前关于 256/128 并发实测、缓存影响、smoke 正常及性能结论不稳定的上下文仍然有效；本批次所述提交、检查通过、merge commit `984462f` 和分支删除均是可检索的报告，不应提升为独立验证成功。'
  title: vLLM/vLLM-Ascend 安装策略、编译性能与 PR 合并
navigation: '当前材料从旧 main 冲突与未确认的 PR 状态推进到一份据报道已完成的冲突整合、验证、PR #56 合入记录：安装方案收敛为 HuaweiCloud pip-only、`--no-deps`、无 uv/fallback，并保留 bundle/session/lock 同步能力；曾发现并修正 `manage_machine.py` 的旧多源 pip 配置和 pytest 安装残留。此前关于 256/128 并发实测、缓存影响、smoke 正常及性能结论不稳定的上下文仍然有效；本批次所述提交、检查通过、merge commit `984462f` 和分支删除均是可检索的报告，不应提升为独立验证成功。'
schema: mindie-material-task/1
status: complete
task_id: f14c6354c103652bba4b8742d4507d0e139571c03de4ef84ebe4bff44094afb5
---

# vLLM/vLLM-Ascend 安装策略、编译性能与 PR 合并

当前材料从旧 main 冲突与未确认的 PR 状态推进到一份据报道已完成的冲突整合、验证、PR #56 合入记录：安装方案收敛为 HuaweiCloud pip-only、`--no-deps`、无 uv/fallback，并保留 bundle/session/lock 同步能力；曾发现并修正 `manage_machine.py` 的旧多源 pip 配置和 pytest 安装残留。此前关于 256/128 并发实测、缓存影响、smoke 正常及性能结论不稳定的上下文仍然有效；本批次所述提交、检查通过、merge commit `984462f` 和分支删除均是可检索的报告，不应提升为独立验证成功。

## Materials

- [同步安装路径与 uv 清理](blocks/29afab0c409ad0c2b9a172e937ccafd16016c4099c58cee856797037f79f89b9.md): 记录了对 remote-code-parity、repo-init 等 skill 的修改：合并 sync_mode 与首次 editable 安装授权、补充 CANN 9.0 环境发现、优先解析 verified commit，并让未设置同步模式时 fail closed。随后移除默认 uv 安装/bootstrap，改为固定 pip 路径；这些本地语法、shell 和行为检查通过，但没有执行远端 NPU 或真实 editable 安装。后续用户认为仅去掉 uv 仍不足，要求基于远端环境收敛到最快的单一路径。
- [远端单源方案与首次编译探查](blocks/1a22ff56572e09baccf7034f1b53002ee549c34582a4a7361c31c285cffeada7.md): 按用户通过 define-goal 提出的目标，对 A3 容器进行了环境和源探测；记录的结果是 HuaweiCloud pip 对 llguidance 约 1.094 秒、NJU apt 约 212ms，清华源出现 403、PyPI 超时，因此方案收敛为固定单源并删除多源 fallback、自动依赖修复和 uv。还发现 vLLM metadata 阶段可能自动加载 torch_npu，缺少完整 Ascend 环境时会失败；后续测试转向 TORCH_DEVICE_BACKEND_AUTOLOAD=0。该块中的远端 ready、torch_npu smoke 和静态验证被报告为通过，但这些仍是来源中的报告，不能视为独立核验。
- [vLLM 编译耗时与并行度对比待办](blocks/de2492a09db0f291e2651e5011c4a9d3308603fdfb1ef21fe79b9ebaf44876e8.md): 来源报告 vLLM 使用 `TORCH_DEVICE_BACKEND_AUTOLOAD=0`、`--no-deps` 后约 9 秒，避免了约 26 秒安装造成的 numpy 2.3.5 污染；vllm-ascend 冷态 native/protobuf 构建超过 630 秒后被终止，热缓存并设置 `MAX_JOBS=128` 与 `CMAKE_BUILD_PARALLEL_LEVEL=128` 的路径约 118 秒完成，但这些结果包含实验中止、缓存复用和环境扰动，不能直接当作稳定基准。来源还称已将并行度默认上限设为 128，并建议可试 256；最新明确任务是实测比较 256 与 128，而该比较在本块结束时尚未执行。远端已有 editable 安装但缺少 parity marker/runtime-state，也被记录为可能导致后续重复冷编译的未完成状态风险。
- [并发实测与安装策略 PR 整理](blocks/6100ca53fd3423d63fa5268bdbe0e9e33edf327a01f45525e1935d00cfe14cb7.md): 在同一台 A3 远端和热缓存条件下，`MAX_JOBS/CMAKE_BUILD_PARALLEL_LEVEL=256` 的 vllm-ascend editable install 用时 120 秒，`128` 用时 118 秒；smoke 检查报告 `smoke=ok`，但该结果仍属于特定环境下的观测，不能视为普遍性能结论。随后整理 skill 修改并创建 draft PR，涉及固定 HuaweiCloud pip、移除 uv/fallback、editable 安装使用 `--no-deps`、并发上限 128 及 pin 解析脚本；PR 曾因基于旧 `main` 返回 `CONFLICTING`，计划在临时 worktree 中保留 main 的新同步能力并重新套用安装策略，当前过程和合并结果并未全部完成验证。
- [冲突整合、安装策略收敛与 PR #56 合入](blocks/371d8331892efa8f1ecef1f3d24266df0f8b60b159c04595c32266855b69ca51.md): 材料记录了在临时 worktree 中解决冲突并统一远端安装策略：移除 uv、fallback、镜像循环和自动依赖修复，改用 HuaweiCloud 单一 pip 源、`--no-deps` 与 128 并发上限，同时保留 session/remote toolbox、cache 等 main 上的更新。期间验证发现 `manage_machine.py` 残留旧的 `/etc/pip.conf` 多源模板和 pytest opportunistic install，随后修正；Python 编译、diff 检查、生成脚本检查及 vLLM CI pin 解析据报道通过。后续提交 `8a074c4` 被推送并据报道使 PR #56 达到 `MERGEABLE/CLEAN`，PR 从 draft 标记 ready 后以 squash merge 合入，main 指向 `984462f`、远端分支删除；这些状态来自材料中的报告，不能独立视为已验证事实。
