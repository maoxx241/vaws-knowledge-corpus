---
blocks:
- block_id: b3c4e7f122bd040da9e4c9834fd8aa2268e3aa5c52025be73368cd42af5cdab0
  indexed: true
  sha256: 8e852f9f314cee27241ec407f5a2d81d18fa239009d361dd43a7d9469397d19c
  source_range:
    end: 2359418
    part: 0
    start: 0
  summary: 实机验证 routed WeightNz W4A8 的 GMM 路径可运行：输入为 `[2,3584]` INT8 与 `[2,3584,768]` FRACTAL_NZ INT32 carrier，输出为 `[2,6144]` BF16；loader 的 `w13_scale` 需由 `[E,N]` 恢复为 `[E,1,N]`，否则出现 `k=3584 and quantGroupNum=6144`。Dynamic-EPLB 多个 2-D WeightNz tensor 路径失败，错误形状尝试曾触发 AICore 越界但设备随后为 `Health OK`，因此该路径当前应显式拒绝；K3 shared expert 被确认是 W8A8，`npu_quant_matmul(..., output_dtype=int32)` 的实测抽查与 CPU INT8×INT8 accumulator 一致。新版 `DequantSituQuant` 尚未安装，BF16 SiTU 与 shared down 尚未完成；FP16 也未被验证支持。
  title: WeightNz GMM 与 K3 量化路径实机探测
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 1f41cd612dff23d607414720519fac2609f6db29baf14714be02f9481472c67a
  kind: experience
  material_digest: a130a9a890d105353778a3d945cb8d459112aa173d96e4dca26c98dee749d3be
  revision: 5ac0e9c7c5c381a36c1be6dc9a0c2f1e0098a38fd75b526075783b8d458b2860
  schema: mindie-entry/3
  summary: 当前材料集中于 routed WeightNz W4A8 的 GMM 实机验证、`w13_scale` 形状修复及 Dynamic-EPLB 多 tensor 失败边界，并确认 shared expert 为 W8A8 INT32 累加路径。后续仍需在安装新版 `DequantSituQuant` 后完成 BF16 SiTU 与 shared down 验证；不要将未完成探测或 FP16 泛化视为成功。
  title: WeightNz/K3 量化路径索引
navigation: 当前材料集中于 routed WeightNz W4A8 的 GMM 实机验证、`w13_scale` 形状修复及 Dynamic-EPLB 多 tensor 失败边界，并确认 shared expert 为 W8A8 INT32 累加路径。后续仍需在安装新版 `DequantSituQuant` 后完成 BF16 SiTU 与 shared down 验证；不要将未完成探测或 FP16 泛化视为成功。
schema: mindie-material-task/1
status: complete
task_id: 1f41cd612dff23d607414720519fac2609f6db29baf14714be02f9481472c67a
---

# WeightNz/K3 量化路径索引

当前材料集中于 routed WeightNz W4A8 的 GMM 实机验证、`w13_scale` 形状修复及 Dynamic-EPLB 多 tensor 失败边界，并确认 shared expert 为 W8A8 INT32 累加路径。后续仍需在安装新版 `DequantSituQuant` 后完成 BF16 SiTU 与 shared down 验证；不要将未完成探测或 FP16 泛化视为成功。

## Materials

- [WeightNz GMM 与 K3 量化路径实机探测](blocks/b3c4e7f122bd040da9e4c9834fd8aa2268e3aa5c52025be73368cd42af5cdab0.md): 实机验证 routed WeightNz W4A8 的 GMM 路径可运行：输入为 `[2,3584]` INT8 与 `[2,3584,768]` FRACTAL_NZ INT32 carrier，输出为 `[2,6144]` BF16；loader 的 `w13_scale` 需由 `[E,N]` 恢复为 `[E,1,N]`，否则出现 `k=3584 and quantGroupNum=6144`。Dynamic-EPLB 多个 2-D WeightNz tensor 路径失败，错误形状尝试曾触发 AICore 越界但设备随后为 `Health OK`，因此该路径当前应显式拒绝；K3 shared expert 被确认是 W8A8，`npu_quant_matmul(..., output_dtype=int32)` 的实测抽查与 CPU INT8×INT8 accumulator 一致。新版 `DequantSituQuant` 尚未安装，BF16 SiTU 与 shared down 尚未完成；FP16 也未被验证支持。
