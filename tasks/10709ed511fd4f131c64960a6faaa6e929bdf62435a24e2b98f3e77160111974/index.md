---
blocks:
- block_id: 2dad1e2021c2320a3406f29fb52928f315d6371af1977ef62fac43b28342e635
  indexed: true
  sha256: caad1bc3074e3b01d70b50be8c9d6919bac6c70eef5503d4622065b4220f8e5e
  source_range:
    end: 429691
    part: 0
    start: 0
  summary: 用户要求在指定容器和物理NPU8上执行一次隔离的 torch_npu.npu_rms_norm 检查，覆盖 4x1024、7x4096 的 FP16/BF16，并与 CPU float32 参考比较；需保留脚本、每 case 的误差与哈希、设备/版本/代码哈希及总 PASS/FAIL。助手报告已完成一次手动激活和知识查询，并称拟采用逐元素 abs(error) <= atol + rtol*abs(reference)、FP16 atol=0.002、BF16 atol=0.016 等条件，但该块未提供实际作业启动、结果、artifact 拉取或成功验证证据。
  title: RMSNorm NPU验收任务边界与待执行方案
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 10709ed511fd4f131c64960a6faaa6e929bdf62435a24e2b98f3e77160111974
  kind: experience
  material_digest: 337e9d009e7d4dec3c2e6f940b3385853343decc4d4335ac3c211b213c773a8d
  revision: a2ea194886b0989b06941374ab86dd45561ae7cca77b4371be70917edb4a38b8
  schema: mindie-entry/3
  summary: 已记录用户规定的单次运行、资源限制、候选工具、环境初始化、输出目录保护和结果留存要求，以及助手报告的经验参数选择。当前状态仍是不确定/未完成：没有可核验的 NPU 输出、误差、内存、PASS/FAIL 或拉取回执；后续检索应保留这些待验证项，不将计划或报告的检查视为成功。
  title: 当前任务：一次性隔离 RMSNorm NPU验收
navigation: 已记录用户规定的单次运行、资源限制、候选工具、环境初始化、输出目录保护和结果留存要求，以及助手报告的经验参数选择。当前状态仍是不确定/未完成：没有可核验的 NPU 输出、误差、内存、PASS/FAIL 或拉取回执；后续检索应保留这些待验证项，不将计划或报告的检查视为成功。
schema: mindie-material-task/1
status: complete
task_id: 10709ed511fd4f131c64960a6faaa6e929bdf62435a24e2b98f3e77160111974
---

# 当前任务：一次性隔离 RMSNorm NPU验收

已记录用户规定的单次运行、资源限制、候选工具、环境初始化、输出目录保护和结果留存要求，以及助手报告的经验参数选择。当前状态仍是不确定/未完成：没有可核验的 NPU 输出、误差、内存、PASS/FAIL 或拉取回执；后续检索应保留这些待验证项，不将计划或报告的检查视为成功。

## Materials

- [RMSNorm NPU验收任务边界与待执行方案](blocks/2dad1e2021c2320a3406f29fb52928f315d6371af1977ef62fac43b28342e635.md): 用户要求在指定容器和物理NPU8上执行一次隔离的 torch_npu.npu_rms_norm 检查，覆盖 4x1024、7x4096 的 FP16/BF16，并与 CPU float32 参考比较；需保留脚本、每 case 的误差与哈希、设备/版本/代码哈希及总 PASS/FAIL。助手报告已完成一次手动激活和知识查询，并称拟采用逐元素 abs(error) <= atol + rtol*abs(reference)、FP16 atol=0.002、BF16 atol=0.016 等条件，但该块未提供实际作业启动、结果、artifact 拉取或成功验证证据。
