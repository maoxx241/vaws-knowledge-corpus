---
blocks:
- block_id: ff86dff5ae5a6b7057c2d78ab78095a12dfc0332dc739a0db3954a4d7cfb43a1
  indexed: true
  sha256: eb6b809800ddf4349d5474e99bb518dd628950e8fbbe5c321dc051c6c28cd40e
  source_range:
    end: 501647
    part: 0
    start: 0
  summary: '初始启动中四个根 PID 全部退出，153:18077/health 返回连接拒绝（HTTP 000）；日志显示缺少 specforge、DSparkDraftModel 未归一化为 Qwen3DSparkModel，并触发 Pydantic ValidationError/ERR99999。修复后的 r2 越过早期 DSpark 校验并进入 EP64、HCCL、worker 初始化及权重加载，但在约 312/352 后因 KimiK3Config 缺少 image_token_index 导致 AttributeError 和 Engine core initialization failed，155 另有 IndexError: tuple index out of range；API 始终未监听。全过程为只读观察，未发送推理请求，也未操作进程；存活、无错误和初始化屏障均为观察状态，不能视为启动成功。'
  title: 四机启动监控：初始配置失败与 r2 权重加载失败
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 3dacea6f373db0374c4be92d66065d76a76629479648d393e1bd505a0b0594d9
  kind: experience
  material_digest: b22d98a54324d902ae6f0386bc86d04063c5937265e5b0f8070f19ab49965c9e
  revision: 49ad6f29f7124c9b4fab4724fa29e8dcf84d639d6340142131379347163bd1cf
  schema: mindie-entry/3
  summary: 本批材料记录两次启动尝试：初始批次在 specforge 缺失、DSpark 配置校验阶段失败；r2 虽推进至 EP64 初始化和大部分权重加载，最终仍在 KimiK3Config.image_token_index 缺失处失败并级联退出。后续检索应重点关注 specforge/draft model 配置归一化、KimiK3Config 属性兼容性、权重加载阶段的 AttributeError/IndexError，以及 18077 健康接口未监听；此前的存活与无错误观察只是未完成过程，不是成功结论。
  title: 当前任务导航：四机推理服务启动状态
navigation: 本批材料记录两次启动尝试：初始批次在 specforge 缺失、DSpark 配置校验阶段失败；r2 虽推进至 EP64 初始化和大部分权重加载，最终仍在 KimiK3Config.image_token_index 缺失处失败并级联退出。后续检索应重点关注 specforge/draft model 配置归一化、KimiK3Config 属性兼容性、权重加载阶段的 AttributeError/IndexError，以及 18077 健康接口未监听；此前的存活与无错误观察只是未完成过程，不是成功结论。
schema: mindie-material-task/1
status: complete
task_id: 3dacea6f373db0374c4be92d66065d76a76629479648d393e1bd505a0b0594d9
---

# 当前任务导航：四机推理服务启动状态

本批材料记录两次启动尝试：初始批次在 specforge 缺失、DSpark 配置校验阶段失败；r2 虽推进至 EP64 初始化和大部分权重加载，最终仍在 KimiK3Config.image_token_index 缺失处失败并级联退出。后续检索应重点关注 specforge/draft model 配置归一化、KimiK3Config 属性兼容性、权重加载阶段的 AttributeError/IndexError，以及 18077 健康接口未监听；此前的存活与无错误观察只是未完成过程，不是成功结论。

## Materials

- [四机启动监控：初始配置失败与 r2 权重加载失败](blocks/ff86dff5ae5a6b7057c2d78ab78095a12dfc0332dc739a0db3954a4d7cfb43a1.md): 初始启动中四个根 PID 全部退出，153:18077/health 返回连接拒绝（HTTP 000）；日志显示缺少 specforge、DSparkDraftModel 未归一化为 Qwen3DSparkModel，并触发 Pydantic ValidationError/ERR99999。修复后的 r2 越过早期 DSpark 校验并进入 EP64、HCCL、worker 初始化及权重加载，但在约 312/352 后因 KimiK3Config 缺少 image_token_index 导致 AttributeError 和 Engine core initialization failed，155 另有 IndexError: tuple index out of range；API 始终未监听。全过程为只读观察，未发送推理请求，也未操作进程；存活、无错误和初始化屏障均为观察状态，不能视为启动成功。
