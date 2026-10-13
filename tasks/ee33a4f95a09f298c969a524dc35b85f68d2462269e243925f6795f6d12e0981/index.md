---
blocks:
- block_id: bc21694ca679a7ee00868cca370843df4f212d41defe33ddba18fab833f6371a
  indexed: true
  sha256: f4a0f038da79d133183e300a11cef546f2a737982b0e51be1cb484db30273bb4
  source_range:
    end: 507104
    part: 0
    start: 0
  summary: 用户要求在固定容器、NPU 8、shape [7,2048]、seed 20260921 下运行真实 `torch.npu.npu_rms_norm`，并限制只修改 `/mindie/consumer-b/check.py` 与 `result.json`；同时要求先激活 MindIE 并查询经验，社区贡献关闭。查询到的相近案例建议对按目标 dtype 舍入的输入使用独立 CPU 参考，FP16/BF16 容差为 0.002/0.016，但详细读取因引用校验失败未重试，不能直接作为本次结论。远端脚本据报告已修正参考输入、BF16 容差和结果路径，但环境显示 `torch.npu.device_count()=0`、`torch=2.10.0+cpu`，且 `npu-smi info` 不可用；`result.json` 为 `passed=false`、`cases=[]`，因此未获得真实 NPU 数值结果，也未验证通过。
  title: 指定容器中的 NPU RMSNorm 验证未执行
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: ee33a4f95a09f298c969a524dc35b85f68d2462269e243925f6795f6d12e0981
  kind: experience
  material_digest: cb29c90ba1c788365357dfe3089a9183ee8e713d41f24b2caaa4406b993d43ae
  revision: 1e3f7c8f3dd2fa36116d53fcadc6450d892aee2bf400ed1e14a2733af32e289a
  schema: mindie-entry/3
  summary: 当前批次仅包含一个材料块，记录了指定单卡 NPU RMSNorm 验证的约束、脚本修正和未完成结果。已知环境不可用（`device_count=0`、CPU 版 torch、`npu-smi` 失败），所以后续不得将计划、脚本修正或相近经验提升为成功；真实 FP16/BF16 结果仍待可用 NPU 环境重新验证。
  title: MindIE NPU 验证导航
navigation: 当前批次仅包含一个材料块，记录了指定单卡 NPU RMSNorm 验证的约束、脚本修正和未完成结果。已知环境不可用（`device_count=0`、CPU 版 torch、`npu-smi` 失败），所以后续不得将计划、脚本修正或相近经验提升为成功；真实 FP16/BF16 结果仍待可用 NPU 环境重新验证。
schema: mindie-material-task/1
status: complete
task_id: ee33a4f95a09f298c969a524dc35b85f68d2462269e243925f6795f6d12e0981
---

# MindIE NPU 验证导航

当前批次仅包含一个材料块，记录了指定单卡 NPU RMSNorm 验证的约束、脚本修正和未完成结果。已知环境不可用（`device_count=0`、CPU 版 torch、`npu-smi` 失败），所以后续不得将计划、脚本修正或相近经验提升为成功；真实 FP16/BF16 结果仍待可用 NPU 环境重新验证。

## Materials

- [指定容器中的 NPU RMSNorm 验证未执行](blocks/bc21694ca679a7ee00868cca370843df4f212d41defe33ddba18fab833f6371a.md): 用户要求在固定容器、NPU 8、shape [7,2048]、seed 20260921 下运行真实 `torch.npu.npu_rms_norm`，并限制只修改 `/mindie/consumer-b/check.py` 与 `result.json`；同时要求先激活 MindIE 并查询经验，社区贡献关闭。查询到的相近案例建议对按目标 dtype 舍入的输入使用独立 CPU 参考，FP16/BF16 容差为 0.002/0.016，但详细读取因引用校验失败未重试，不能直接作为本次结论。远端脚本据报告已修正参考输入、BF16 容差和结果路径，但环境显示 `torch.npu.device_count()=0`、`torch=2.10.0+cpu`，且 `npu-smi info` 不可用；`result.json` 为 `passed=false`、`cases=[]`，因此未获得真实 NPU 数值结果，也未验证通过。
