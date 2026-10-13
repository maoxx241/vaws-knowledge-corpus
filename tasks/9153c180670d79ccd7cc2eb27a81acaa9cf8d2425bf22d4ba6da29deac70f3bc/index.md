---
blocks:
- block_id: f47fadc9c665030bd2094c3dfc3a4d670d35ad08dc88c10d616aebaf252b0ad5
  indexed: true
  sha256: c628ff4f0f655729d49c240bb17f17ddddada4a795c767b9a9e7c22a074ccb06
  source_range:
    end: 16806766
    part: 0
    start: 0
  summary: 记录了 codex-cli 0.147.0 与 0.150.0-alpha.8 的真实探测：provider 使用 gpt-5.6-luna 成功返回，但 hook 信任阻断了初始 VAWS context；candidate-2 远端回归测试后更正总数为 183 项，首次 B managed run 因 googletest worktree 未初始化而阻断。补齐独立 repo 后复用原身份重试成功，B 保持 running、lease active，host 154 对 127.0.0.1:18867 的两次真实 HTTP GET 均返回 200，并得到 matmul sum=16777216.0；当前后续工作是只读审查 lost response、restart、stop/release 与 timeout 的失败语义，尚未修改代码或控制运行中的 B。
  title: Codex/VAWS 生命周期验收与运行态证据
- block_id: c58d8cf0a42c38c1e1d54ca73c899f59917d933ad09fab4142d5afe8bc2bd4ff
  indexed: true
  sha256: 43838d89dcfb54fec5c61e8d7667c05bde4f70a4ba29eee0c13e68684c35fb22
  source_range:
    end: 18660176
    part: 0
    start: 16806766
  summary: 只读审查识别出 `setsid + env -u VAWS_REMOTE_JOB_TOKEN` 导致 marker/PGID 逃逸、可能错误释放 lease 的明确风险；现有测试未覆盖该路径，B 曾保持运行。candidate-3 CPU 回归最终确认为 23 组、185 passed、`OVERALL_RC=0`，期间首次 `docker cp` 因容器父目录不存在失败，且工具未带出 stdout，结果随后通过日志核实。B final NPU 请求首次因服务尚未监听而 connection refused，启动日志确认后两次 HTTP 200 成功，服务当时仍为 `running`/lease `active`。
  title: 候选-3回归与B最终NPU复测
- block_id: 21b9550fbad3ffe696e43aa1394cb83725b39cc784db46a6ab9f10ad4d3f508e
  indexed: true
  sha256: c19788d0f28567fc707cf1d5ea08a95fb1f22f85de4c3ec8d7955f710295f24d
  source_range:
    end: 18660176
    part: 1
    start: 16806766
  summary: candidate-4 在 154 C 容器完成 463 文件归档校验，完整 CPU 回归确认为 23 组、185 passed、`OVERALL_RC=0`；随后以 `uid=65534(nobody)` 执行 `managed_jobs`，6 项通过，仅有 `.pytest_cache` 权限 warning。B finish 初始处于 `stopping`，之后只读状态确认 `cancelled`、lease released、runtime returned、`quiet=true`、`processes=[]`、`unknown=[]` 和 `descendants_drained=true`，再次 finish 后 session 达到 `finished`；SSH、A/C、worktree 均保留。
  title: 候选-4普通用户验证与B清理
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 9153c180670d79ccd7cc2eb27a81acaa9cf8d2425bf22d4ba6da29deac70f3bc
  kind: experience
  material_digest: 3a02e5e4a01c21bd01ab2137f870d57c0cd87320a9787bac1d9f6e9cbce526f2
  revision: 9f854fabfde9ae359b98061b2e4f54e982f5079dabe4753c5d8c356b056602b7
  schema: mindie-entry/3
  summary: 已记录 marker/PGID 进程逃逸及潜在错误释放风险；candidate-3 与 candidate-4 的 CPU 回归均最终核实为 185 passed，但需保留首次路径错误、stdout 缺失及后续日志确认等过程不确定性。B final NPU 复测曾出现服务未监听导致的 connection refused，随后两次 HTTP 200 成功并保持运行；最新状态显示 B 已完成清理，session finished、lease released、runtime returned、remote quiet 且无 processes/unknown，A/C、SSH 和 worktree 未触碰。
  title: 当前任务导航：VAWS 生命周期验收、回归证据与清理状态
navigation: 已记录 marker/PGID 进程逃逸及潜在错误释放风险；candidate-3 与 candidate-4 的 CPU 回归均最终核实为 185 passed，但需保留首次路径错误、stdout 缺失及后续日志确认等过程不确定性。B final NPU 复测曾出现服务未监听导致的 connection refused，随后两次 HTTP 200 成功并保持运行；最新状态显示 B 已完成清理，session finished、lease released、runtime returned、remote quiet 且无 processes/unknown，A/C、SSH 和 worktree 未触碰。
schema: mindie-material-task/1
status: complete
task_id: 9153c180670d79ccd7cc2eb27a81acaa9cf8d2425bf22d4ba6da29deac70f3bc
---

# 当前任务导航：VAWS 生命周期验收、回归证据与清理状态

已记录 marker/PGID 进程逃逸及潜在错误释放风险；candidate-3 与 candidate-4 的 CPU 回归均最终核实为 185 passed，但需保留首次路径错误、stdout 缺失及后续日志确认等过程不确定性。B final NPU 复测曾出现服务未监听导致的 connection refused，随后两次 HTTP 200 成功并保持运行；最新状态显示 B 已完成清理，session finished、lease released、runtime returned、remote quiet 且无 processes/unknown，A/C、SSH 和 worktree 未触碰。

## Materials

- [Codex/VAWS 生命周期验收与运行态证据](blocks/f47fadc9c665030bd2094c3dfc3a4d670d35ad08dc88c10d616aebaf252b0ad5.md): 记录了 codex-cli 0.147.0 与 0.150.0-alpha.8 的真实探测：provider 使用 gpt-5.6-luna 成功返回，但 hook 信任阻断了初始 VAWS context；candidate-2 远端回归测试后更正总数为 183 项，首次 B managed run 因 googletest worktree 未初始化而阻断。补齐独立 repo 后复用原身份重试成功，B 保持 running、lease active，host 154 对 127.0.0.1:18867 的两次真实 HTTP GET 均返回 200，并得到 matmul sum=16777216.0；当前后续工作是只读审查 lost response、restart、stop/release 与 timeout 的失败语义，尚未修改代码或控制运行中的 B。
- [候选-3回归与B最终NPU复测](blocks/c58d8cf0a42c38c1e1d54ca73c899f59917d933ad09fab4142d5afe8bc2bd4ff.md): 只读审查识别出 `setsid + env -u VAWS_REMOTE_JOB_TOKEN` 导致 marker/PGID 逃逸、可能错误释放 lease 的明确风险；现有测试未覆盖该路径，B 曾保持运行。candidate-3 CPU 回归最终确认为 23 组、185 passed、`OVERALL_RC=0`，期间首次 `docker cp` 因容器父目录不存在失败，且工具未带出 stdout，结果随后通过日志核实。B final NPU 请求首次因服务尚未监听而 connection refused，启动日志确认后两次 HTTP 200 成功，服务当时仍为 `running`/lease `active`。
- [候选-4普通用户验证与B清理](blocks/21b9550fbad3ffe696e43aa1394cb83725b39cc784db46a6ab9f10ad4d3f508e.md): candidate-4 在 154 C 容器完成 463 文件归档校验，完整 CPU 回归确认为 23 组、185 passed、`OVERALL_RC=0`；随后以 `uid=65534(nobody)` 执行 `managed_jobs`，6 项通过，仅有 `.pytest_cache` 权限 warning。B finish 初始处于 `stopping`，之后只读状态确认 `cancelled`、lease released、runtime returned、`quiet=true`、`processes=[]`、`unknown=[]` 和 `descendants_drained=true`，再次 finish 后 session 达到 `finished`；SSH、A/C、worktree 均保留。
