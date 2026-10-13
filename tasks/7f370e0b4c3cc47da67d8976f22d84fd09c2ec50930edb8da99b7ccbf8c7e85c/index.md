---
blocks:
- block_id: 45f199fe8ac523965abbcb8ce7af40dac566a82546e9715ad2e1995313fda8d2
  indexed: true
  sha256: 94a6c4b8426149d075412bc229efc140d90f7951afda07257b165b2ddc4f4661
  source_range:
    end: 2729450
    part: 0
    start: 0
  summary: 只读静态审阅认为当前 K3 profile forward 没有与 max_model_len 相关的 NPU 张量形状，KDA/MLA profile metadata 为 None；已排除或暂不支持作为根因的项目包括固定上限的 MLA profile 张量、profile 前的 KB 级 block table，以及 profile 后才创建的 chunked-prefill workspace 和 KDA 配置。memory_profiling 实际记录的是 profile 区间 Torch allocated 峰值增量，可能包含首次创建并保留的 lazy buffer，因此不等同于瞬时 activation；历史实验还存在“130K”使用 max_num_batched_tokens=24576、对照组为 8192 的口径混杂。结论仍属静态审阅而非复现验证：若严格 A/B 能复现，建议拆分 persistent/transient 指标并固定 batched-token 做回归，避免简单移动或扣除 lazy allocation 导致 KV 过量分配和潜在 OOM；本次未修改文件或远端进程。
  title: K3 profile 峰值内存与 max_model_len 依赖审阅
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 7f370e0b4c3cc47da67d8976f22d84fd09c2ec50930edb8da99b7ccbf8c7e85c
  kind: experience
  material_digest: 8c82175fc968902918db55c74596b60ec366f35e40f031af35ecc7bc8d8b659a
  revision: 5968e0310c8bc74bf6cad7537c13e666aa277fddbf2c1574b827cca504331ce1
  schema: mindie-entry/3
  summary: 当前材料集中于 Kimi K3/vLLM 0.23 profile 的内存测量口径与 max_model_len 增长解释：静态检查未确认 K3 forward 是根因，部分候选分配已被排除或限定为短区间相关。后续检索应优先区分 max_model_len 与 max_num_batched_tokens、核对 profile 前后 allocated/peak 快照及 lazy buffer 生命周期，并保留“尚未通过严格 A/B 复现”的不确定性；此前材料未提供的细节仍需独立搜索原始块。
  title: K3 profile 内存问题检索导航
navigation: 当前材料集中于 Kimi K3/vLLM 0.23 profile 的内存测量口径与 max_model_len 增长解释：静态检查未确认 K3 forward 是根因，部分候选分配已被排除或限定为短区间相关。后续检索应优先区分 max_model_len 与 max_num_batched_tokens、核对 profile 前后 allocated/peak 快照及 lazy buffer 生命周期，并保留“尚未通过严格 A/B 复现”的不确定性；此前材料未提供的细节仍需独立搜索原始块。
schema: mindie-material-task/1
status: complete
task_id: 7f370e0b4c3cc47da67d8976f22d84fd09c2ec50930edb8da99b7ccbf8c7e85c
---

# K3 profile 内存问题检索导航

当前材料集中于 Kimi K3/vLLM 0.23 profile 的内存测量口径与 max_model_len 增长解释：静态检查未确认 K3 forward 是根因，部分候选分配已被排除或限定为短区间相关。后续检索应优先区分 max_model_len 与 max_num_batched_tokens、核对 profile 前后 allocated/peak 快照及 lazy buffer 生命周期，并保留“尚未通过严格 A/B 复现”的不确定性；此前材料未提供的细节仍需独立搜索原始块。

## Materials

- [K3 profile 峰值内存与 max_model_len 依赖审阅](blocks/45f199fe8ac523965abbcb8ce7af40dac566a82546e9715ad2e1995313fda8d2.md): 只读静态审阅认为当前 K3 profile forward 没有与 max_model_len 相关的 NPU 张量形状，KDA/MLA profile metadata 为 None；已排除或暂不支持作为根因的项目包括固定上限的 MLA profile 张量、profile 前的 KB 级 block table，以及 profile 后才创建的 chunked-prefill workspace 和 KDA 配置。memory_profiling 实际记录的是 profile 区间 Torch allocated 峰值增量，可能包含首次创建并保留的 lazy buffer，因此不等同于瞬时 activation；历史实验还存在“130K”使用 max_num_batched_tokens=24576、对照组为 8192 的口径混杂。结论仍属静态审阅而非复现验证：若严格 A/B 能复现，建议拆分 persistent/transient 指标并固定 batched-token 做回归，避免简单移动或扣除 lazy allocation 导致 KV 过量分配和潜在 OOM；本次未修改文件或远端进程。
