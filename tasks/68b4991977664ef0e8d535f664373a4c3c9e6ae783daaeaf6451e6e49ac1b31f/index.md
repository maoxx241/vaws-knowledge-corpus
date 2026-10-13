---
blocks:
- block_id: 1f1d2cc76393aaa5be198dca7c1be6255c14ada5567b229c6cdee4de70fc23db
  indexed: true
  sha256: 87f2c0f29358ea000c47068e062aa40f2ec02b1527c8a79ed86b688c08823a96
  source_range:
    end: 736117
    part: 0
    start: 0
  summary: 单机 TP16 五层权重路线已被纠正：156 曾出现 offload 场景 NZ 权重回搬 CPU 的 TransData 崩溃，随后主机内存 OOM 退出；不应据此进行完整权重精度验证。四机准备中，153 的 `lala-153-k3-main` 占用 16 张 NPU，155 的标准容器一度缺失，后由其他执行方并发创建；154/156 模板与 155 容器除 Docker runtime（`runc` 对 `ascend`）外据报告一致，但这仍是审计结果而非验证成功。后续需保留 runtime 差异、容器与端口状态、GQA 定向测试 `42 passed` 等上下文，并完成真实全权重 target-only、DSpark 短答、精度和接收率门禁；已有 SSH 只读调用方式未使用密码。
  title: 四机验证准备与容器同构审计
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 68b4991977664ef0e8d535f664373a4c3c9e6ae783daaeaf6451e6e49ac1b31f
  kind: experience
  material_digest: a7921160f39e7f11286bf8dd9e68283f9efec2e6d44eda19c0bb2399e53b042d
  revision: 56bfb07a92b6fb968f5be2d76a7c64a539022f4fc785a9eece4ca9327be57e93
  schema: mindie-entry/3
  summary: 当前重点已从单机五层精度尝试转为四机真实全权重验证。153 资源释放、155 容器 runtime 同构与四 rank 同时启动仍需明确处理；容器就绪或 health 不等于 target-only、DSpark、精度和接收率验证成功。保留此前 TransData 崩溃、OOM、容器并发创建及 `runc`/`ascend` 差异等未完成或需复核事项。
  title: 四机全权重验证准备
navigation: 当前重点已从单机五层精度尝试转为四机真实全权重验证。153 资源释放、155 容器 runtime 同构与四 rank 同时启动仍需明确处理；容器就绪或 health 不等于 target-only、DSpark、精度和接收率验证成功。保留此前 TransData 崩溃、OOM、容器并发创建及 `runc`/`ascend` 差异等未完成或需复核事项。
schema: mindie-material-task/1
status: complete
task_id: 68b4991977664ef0e8d535f664373a4c3c9e6ae783daaeaf6451e6e49ac1b31f
---

# 四机全权重验证准备

当前重点已从单机五层精度尝试转为四机真实全权重验证。153 资源释放、155 容器 runtime 同构与四 rank 同时启动仍需明确处理；容器就绪或 health 不等于 target-only、DSpark、精度和接收率验证成功。保留此前 TransData 崩溃、OOM、容器并发创建及 `runc`/`ascend` 差异等未完成或需复核事项。

## Materials

- [四机验证准备与容器同构审计](blocks/1f1d2cc76393aaa5be198dca7c1be6255c14ada5567b229c6cdee4de70fc23db.md): 单机 TP16 五层权重路线已被纠正：156 曾出现 offload 场景 NZ 权重回搬 CPU 的 TransData 崩溃，随后主机内存 OOM 退出；不应据此进行完整权重精度验证。四机准备中，153 的 `lala-153-k3-main` 占用 16 张 NPU，155 的标准容器一度缺失，后由其他执行方并发创建；154/156 模板与 155 容器除 Docker runtime（`runc` 对 `ascend`）外据报告一致，但这仍是审计结果而非验证成功。后续需保留 runtime 差异、容器与端口状态、GQA 定向测试 `42 passed` 等上下文，并完成真实全权重 target-only、DSpark 短答、精度和接收率门禁；已有 SSH 只读调用方式未使用密码。
