---
blocks:
- block_id: 89c4d4e91b4fbefe4d4c703857dfc58789d0f728c3fe228e321d3be825d17f36
  indexed: true
  sha256: 91ca483f7add0b6c5eaff3b1845c098edeaeecd25017cce203d4420975692bb7
  source_range:
    end: 846940
    part: 0
    start: 0
  summary: 本地 R20 bundle 据报告已完成冻结复核：runtime/launcher manifest、客户端 UT、bash -n、ShellCheck、33 个 target 的 byte-exact 重生成及源码 SHA 校验均标为 PASS，且声明已清除 pyc/cache/coverage、worktree clean；独立远端只读 readback 也被报告为通过，但未执行远端写入、启停或 HTTP。`clientfix2` 被报告已完成四机原子部署、UT、manifest 和最终 readback，实际 `PREFLIGHT_ONLY` 为 `rc=0` 且 `http_requests=0`，不过唯一 client 命令仍未执行、等待 READY。随后四节点只读监控观察到 EngineCore 初始化、64-rank HCCL/EP 映射、权重加载至 `326/351 (93%)`、KV cache 初始化和图捕获完成；期间出现 “Waiting for pending NCCL work…” 及共享内存无空闲块提示，后续自然收敛，进程无已观测的 zombie、traceback、fatal 或 error，但截至材料末尾仍未出现 Uvicorn，不能据此确认服务已成功启动。
  title: R20 冻结交付与四节点只读启动监控
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 24e9d58405252fa7197d314f8dd432551e767edc3e5b2446bfd80e51197f82a4
  kind: experience
  material_digest: da4a5c1aa3702b12456f00c9912e1b201aeff190f4ae0f2cdb7f5fb5bc3270e8
  revision: 5164e688a02716a44d57e470e35e990170507655226c06f5fe1f497b5ffde0e1
  schema: mindie-entry/3
  summary: 已记录 R20 bundle、`clientfix2` 和四节点只读启动阶段的独立证据；原始 runtime 与 `clientfix1` 失败证据据报告保留。最新状态是权重/KV cache 完成、图捕获完成并进入 CPU/NPU binding 与 EngineCore 收尾，但尚未观察到 Uvicorn、health 检查或模型请求，因此服务成功启动仍未验证；此前的 pending NCCL/shared-memory 提示已自行收敛，不能单独视为 fatal。
  title: 当前任务导航：R20 交付后四节点启动监控
navigation: 已记录 R20 bundle、`clientfix2` 和四节点只读启动阶段的独立证据；原始 runtime 与 `clientfix1` 失败证据据报告保留。最新状态是权重/KV cache 完成、图捕获完成并进入 CPU/NPU binding 与 EngineCore 收尾，但尚未观察到 Uvicorn、health 检查或模型请求，因此服务成功启动仍未验证；此前的 pending NCCL/shared-memory 提示已自行收敛，不能单独视为 fatal。
schema: mindie-material-task/1
status: complete
task_id: 24e9d58405252fa7197d314f8dd432551e767edc3e5b2446bfd80e51197f82a4
---

# 当前任务导航：R20 交付后四节点启动监控

已记录 R20 bundle、`clientfix2` 和四节点只读启动阶段的独立证据；原始 runtime 与 `clientfix1` 失败证据据报告保留。最新状态是权重/KV cache 完成、图捕获完成并进入 CPU/NPU binding 与 EngineCore 收尾，但尚未观察到 Uvicorn、health 检查或模型请求，因此服务成功启动仍未验证；此前的 pending NCCL/shared-memory 提示已自行收敛，不能单独视为 fatal。

## Materials

- [R20 冻结交付与四节点只读启动监控](blocks/89c4d4e91b4fbefe4d4c703857dfc58789d0f728c3fe228e321d3be825d17f36.md): 本地 R20 bundle 据报告已完成冻结复核：runtime/launcher manifest、客户端 UT、bash -n、ShellCheck、33 个 target 的 byte-exact 重生成及源码 SHA 校验均标为 PASS，且声明已清除 pyc/cache/coverage、worktree clean；独立远端只读 readback 也被报告为通过，但未执行远端写入、启停或 HTTP。`clientfix2` 被报告已完成四机原子部署、UT、manifest 和最终 readback，实际 `PREFLIGHT_ONLY` 为 `rc=0` 且 `http_requests=0`，不过唯一 client 命令仍未执行、等待 READY。随后四节点只读监控观察到 EngineCore 初始化、64-rank HCCL/EP 映射、权重加载至 `326/351 (93%)`、KV cache 初始化和图捕获完成；期间出现 “Waiting for pending NCCL work…” 及共享内存无空闲块提示，后续自然收敛，进程无已观测的 zombie、traceback、fatal 或 error，但截至材料末尾仍未出现 Uvicorn，不能据此确认服务已成功启动。
