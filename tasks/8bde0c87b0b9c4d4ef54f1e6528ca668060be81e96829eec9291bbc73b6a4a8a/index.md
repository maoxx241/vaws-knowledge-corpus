---
blocks:
- block_id: c3c4124fe4db363070d8dc8c672191608cf3f218370a6a5f38b6318c4ba94e50
  indexed: true
  sha256: ae63789bd3180ef901c44fc09f2ff898a340d0dec9e3120096f2afb7715fbc99
  source_range:
    end: 14933830
    part: 0
    start: 0
  summary: '将 fla-npu PR #395 的 recurrent KDA 增量迁入 vllm-ascend main，保留现有 TND、state pool、MTP 和 graph-padding 语义，加入 non-contiguous Q/K/V 的 stride 支持；beta UB 修复已由 main 的更通用实现覆盖。验证期间发现 K3 `_run_recurrent()` 曾对 fused projection 切出的 Q/K/V 强制 `.contiguous()`，使算子改动无法进入真实路径，随后移除该 materialization 并更新 PR #15511；单算子非连续 stride/state pool 测试通过。此前本机无 torch/torch_npu、远端默认 SSH 被拒等限制仍成立，且真实权重验证最初受 AppleDouble、同步排除规则、环境变量和并行配置问题影响，不能把这些失败尝试当作代码失败或成功。'
  title: 'recurrent KDA #395 迁移与 K3 接入修复'
- block_id: 70f7613e15da515d60bbe8520f6f19813a3311ece546527f2b212043ff060d35
  indexed: true
  sha256: 8bd49f3edc4bb773231dfd5ae364cb7a4c4c69b6dcb9fc31985a0bd5cc80a0bf
  source_range:
    end: 14933830
    part: 1
    start: 0
  summary: 修正 CANN `LD_LIBRARY_PATH`/`PYTHONPATH` 覆盖、插件集缺失和 vLLM 源码版本漂移后，使用真实 16 专家 W4A8/QuaRot checkpoint 在 154 上按 TP16/EP16 启动；16 个 rank 完成 333/333 shard 加载，profile 成功并获得 KV cache。单请求和 4 序列同批 decode 均返回 HTTP 200，记录未见 recurrent KDA、ACL、HCCL 或 RuntimeError；服务随后按精确主机侧进程组清理，16 张逻辑卡释放。该结果支持真实加载和 decode 接入可用，但未验证精度、性能、DSpark 或 ACLGraph；PR 新 head `720584aa` 的 title、pre-commit、mypy、DCO 已通过，选测状态仍需区分为运行中或未执行。
  title: K3 16 专家真实权重 TP16/EP16 验证
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 8bde0c87b0b9c4d4ef54f1e6528ca668060be81e96829eec9291bbc73b6a4a8a
  kind: experience
  material_digest: 741c263b2c70040857faaf3c2ede7a0a2feddef3efef65abdd2734cc93ffec27
  revision: 4ccb55e2f22bbdc808c1f3d2bcbe79d2212d9fdc65a0b25cef14e3e240d30625
  schema: mindie-entry/3
  summary: '当前任务围绕 PR #15511：将 #395 的 non-contiguous Q/K/V stride 能力迁入 main，并修复 K3 `_run_recurrent()` 中阻断该能力的 `.contiguous()`。经历环境变量、插件、vLLM 版本、TP4/EP4 配置和远端清理脚本等失败或不确定因素后，TP16/EP16 真实 16 专家权重已完成加载、profile 和单/多请求 decode；这证明接入路径可运行，但不等同于精度、性能或完整验收。最新已知提交为 `720584aa`，代码检查和 DCO 已通过，选测 CI 仍需继续确认。'
  title: 'recurrent KDA #395 迁移、K3 接入与真实权重验证'
navigation: '当前任务围绕 PR #15511：将 #395 的 non-contiguous Q/K/V stride 能力迁入 main，并修复 K3 `_run_recurrent()` 中阻断该能力的 `.contiguous()`。经历环境变量、插件、vLLM 版本、TP4/EP4 配置和远端清理脚本等失败或不确定因素后，TP16/EP16 真实 16 专家权重已完成加载、profile 和单/多请求 decode；这证明接入路径可运行，但不等同于精度、性能或完整验收。最新已知提交为 `720584aa`，代码检查和 DCO 已通过，选测 CI 仍需继续确认。'
schema: mindie-material-task/1
status: complete
task_id: 8bde0c87b0b9c4d4ef54f1e6528ca668060be81e96829eec9291bbc73b6a4a8a
---

# recurrent KDA #395 迁移、K3 接入与真实权重验证

当前任务围绕 PR #15511：将 #395 的 non-contiguous Q/K/V stride 能力迁入 main，并修复 K3 `_run_recurrent()` 中阻断该能力的 `.contiguous()`。经历环境变量、插件、vLLM 版本、TP4/EP4 配置和远端清理脚本等失败或不确定因素后，TP16/EP16 真实 16 专家权重已完成加载、profile 和单/多请求 decode；这证明接入路径可运行，但不等同于精度、性能或完整验收。最新已知提交为 `720584aa`，代码检查和 DCO 已通过，选测 CI 仍需继续确认。

## Materials

- [recurrent KDA #395 迁移与 K3 接入修复](blocks/c3c4124fe4db363070d8dc8c672191608cf3f218370a6a5f38b6318c4ba94e50.md): 将 fla-npu PR #395 的 recurrent KDA 增量迁入 vllm-ascend main，保留现有 TND、state pool、MTP 和 graph-padding 语义，加入 non-contiguous Q/K/V 的 stride 支持；beta UB 修复已由 main 的更通用实现覆盖。验证期间发现 K3 `_run_recurrent()` 曾对 fused projection 切出的 Q/K/V 强制 `.contiguous()`，使算子改动无法进入真实路径，随后移除该 materialization 并更新 PR #15511；单算子非连续 stride/state pool 测试通过。此前本机无 torch/torch_npu、远端默认 SSH 被拒等限制仍成立，且真实权重验证最初受 AppleDouble、同步排除规则、环境变量和并行配置问题影响，不能把这些失败尝试当作代码失败或成功。
- [K3 16 专家真实权重 TP16/EP16 验证](blocks/70f7613e15da515d60bbe8520f6f19813a3311ece546527f2b212043ff060d35.md): 修正 CANN `LD_LIBRARY_PATH`/`PYTHONPATH` 覆盖、插件集缺失和 vLLM 源码版本漂移后，使用真实 16 专家 W4A8/QuaRot checkpoint 在 154 上按 TP16/EP16 启动；16 个 rank 完成 333/333 shard 加载，profile 成功并获得 KV cache。单请求和 4 序列同批 decode 均返回 HTTP 200，记录未见 recurrent KDA、ACL、HCCL 或 RuntimeError；服务随后按精确主机侧进程组清理，16 张逻辑卡释放。该结果支持真实加载和 decode 接入可用，但未验证精度、性能、DSpark 或 ACLGraph；PR 新 head `720584aa` 的 title、pre-commit、mypy、DCO 已通过，选测状态仍需区分为运行中或未执行。
