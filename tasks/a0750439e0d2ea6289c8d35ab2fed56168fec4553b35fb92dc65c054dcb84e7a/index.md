---
blocks:
- block_id: da12ff32411bbd8c8c06f3e825a9d84ca19d124ebc55fbd9060f0a80c038dd78
  indexed: true
  sha256: 88667a0116940e4362bd78bb3b7ff4de35e8bf3334c5d89242e206e27e2ca1c6
  source_range:
    end: 1031363
    part: 0
    start: 0
  summary: 本批材料记录了远程四机 dspark 卡死、精度及采信率问题的排查结果：r6 四个 PGID 已自退出，c32 的 step2 在 64/64 TP rank 的 2/68 处停滞，主线程集中于 fused-MoE shared-expert 的 TP all_gather_into_tensor；但 r6 含大量 DFX watchdog 和线程采样，相关 device/event/collective 位置只能视为线索，不能证明根因或 collective ordinal 相同。无 DFX 的 r5 中 8/8 串行成功、随后 c32 32/32 超时并 EngineDead；四个 DP 的 fatal scheduler dump 同构，显示无 prefix hit、无 cached request、无抢占且 KV 约 15.99%。材料提出下一次实验记录 host-only collective ordinal 的 enter/exit、rank 和 shape，以区分 ordinal 错配与同 ordinal 下的前序 stream/device 阻塞，但该实验及修复成功均未完成或验证。
  title: dspark 四机卡死与精度问题定位线索
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: a0750439e0d2ea6289c8d35ab2fed56168fec4553b35fb92dc65c054dcb84e7a
  kind: experience
  material_digest: b038683d0353ed527e1eed1b3504c47e0339f1ab0cc40dc851b429fe1e78251f
  revision: c33bb8977e4e7251469c937f0ef1c1215747d57f00607e4a5d61c328378b80eb
  schema: mindie-entry/3
  summary: 当前可检索主线是无 DFX 的 r5 反证、含 DFX 的 r6 停滞现场，以及四 DP 同构 scheduler dump；这些结果支持进一步区分 collective ordinal 错配和 stream/device 阻塞，但尚不能宣称根因、精度/采信率问题或最终方案已修复。后续应保留 DFX 扰动、未记录 ordinal、服务已退出及实验未完成等限制，并沿用 prefix reset、无预热、每 DP 8、max_tokens=1 的候选实验条件。
  title: dspark 四机卡死问题：当前证据与待验证分歧
navigation: 当前可检索主线是无 DFX 的 r5 反证、含 DFX 的 r6 停滞现场，以及四 DP 同构 scheduler dump；这些结果支持进一步区分 collective ordinal 错配和 stream/device 阻塞，但尚不能宣称根因、精度/采信率问题或最终方案已修复。后续应保留 DFX 扰动、未记录 ordinal、服务已退出及实验未完成等限制，并沿用 prefix reset、无预热、每 DP 8、max_tokens=1 的候选实验条件。
schema: mindie-material-task/1
status: complete
task_id: a0750439e0d2ea6289c8d35ab2fed56168fec4553b35fb92dc65c054dcb84e7a
---

# dspark 四机卡死问题：当前证据与待验证分歧

当前可检索主线是无 DFX 的 r5 反证、含 DFX 的 r6 停滞现场，以及四 DP 同构 scheduler dump；这些结果支持进一步区分 collective ordinal 错配和 stream/device 阻塞，但尚不能宣称根因、精度/采信率问题或最终方案已修复。后续应保留 DFX 扰动、未记录 ordinal、服务已退出及实验未完成等限制，并沿用 prefix reset、无预热、每 DP 8、max_tokens=1 的候选实验条件。

## Materials

- [dspark 四机卡死与精度问题定位线索](blocks/da12ff32411bbd8c8c06f3e825a9d84ca19d124ebc55fbd9060f0a80c038dd78.md): 本批材料记录了远程四机 dspark 卡死、精度及采信率问题的排查结果：r6 四个 PGID 已自退出，c32 的 step2 在 64/64 TP rank 的 2/68 处停滞，主线程集中于 fused-MoE shared-expert 的 TP all_gather_into_tensor；但 r6 含大量 DFX watchdog 和线程采样，相关 device/event/collective 位置只能视为线索，不能证明根因或 collective ordinal 相同。无 DFX 的 r5 中 8/8 串行成功、随后 c32 32/32 超时并 EngineDead；四个 DP 的 fatal scheduler dump 同构，显示无 prefix hit、无 cached request、无抢占且 KV 约 15.99%。材料提出下一次实验记录 host-only collective ordinal 的 enter/exit、rank 和 shape，以区分 ordinal 错配与同 ordinal 下的前序 stream/device 阻塞，但该实验及修复成功均未完成或验证。
