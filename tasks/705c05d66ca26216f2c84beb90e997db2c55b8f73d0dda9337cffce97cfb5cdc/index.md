---
blocks:
- block_id: 7108cd2680dc049661cc81600c996539a8b393db477e7da9288aa914752491b7
  indexed: true
  sha256: 52567649200360a09ad4c7068a96fc5fec3a1a3d80060829facc7203ac21eb89
  source_range:
    end: 440521
    part: 0
    start: 0
  summary: 该块讨论 vLLM/vLLM Ascend 中多个 KV cache group 是否位于同一 block：group 内多层共享 block table，但各层 KV tensor 的 page 并非一块连续内存；group 之间有独立 block table 和 block 列表，同时共享全局 BlockPool，非空物理 block 不会被同时复用。VA hybrid 场景中可能共享底层 Tensor，但依靠不同 block ID 访问不同 page；K3 packed layout 另有 parent page 到逻辑 block/subslot 的映射，不能等同于多个 group 共用一个 block。相关实现线索包括 `kv_cache_manager.py`、`kv_cache_utils.py`、`model_runner_v1.py`，并保留了路径已脱敏的引用。
  title: KV cache group、block table 与物理内存映射
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 705c05d66ca26216f2c84beb90e997db2c55b8f73d0dda9337cffce97cfb5cdc
  kind: experience
  material_digest: abb33a2b33a4ccbb705d6b0c8f56995eb603df4260f286f3dd7e4e309f52824e
  revision: 8d9dff821114747abc455513d586ac0712694806906554e651c42934cd6faf46
  schema: mindie-entry/3
  summary: 当前批次包含一项关于 vLLM/vLLM Ascend KV cache 组织方式的参考记录：结论倾向于 group 之间不共用同一逻辑或非空物理 block，但 group 内共享 block table，且 VA 与 K3 packed layout 存在特殊映射。该记录是易错参考索引而非已验证案例报告；实现文件线索、特殊场景说明及脱敏路径均应独立检索，不能将其中的实现判断、代码引用或 reported check 视为已验证成功。
  title: KV cache group 与 block 映射索引
navigation: 当前批次包含一项关于 vLLM/vLLM Ascend KV cache 组织方式的参考记录：结论倾向于 group 之间不共用同一逻辑或非空物理 block，但 group 内共享 block table，且 VA 与 K3 packed layout 存在特殊映射。该记录是易错参考索引而非已验证案例报告；实现文件线索、特殊场景说明及脱敏路径均应独立检索，不能将其中的实现判断、代码引用或 reported check 视为已验证成功。
schema: mindie-material-task/1
status: complete
task_id: 705c05d66ca26216f2c84beb90e997db2c55b8f73d0dda9337cffce97cfb5cdc
---

# KV cache group 与 block 映射索引

当前批次包含一项关于 vLLM/vLLM Ascend KV cache 组织方式的参考记录：结论倾向于 group 之间不共用同一逻辑或非空物理 block，但 group 内共享 block table，且 VA 与 K3 packed layout 存在特殊映射。该记录是易错参考索引而非已验证案例报告；实现文件线索、特殊场景说明及脱敏路径均应独立检索，不能将其中的实现判断、代码引用或 reported check 视为已验证成功。

## Materials

- [KV cache group、block table 与物理内存映射](blocks/7108cd2680dc049661cc81600c996539a8b393db477e7da9288aa914752491b7.md): 该块讨论 vLLM/vLLM Ascend 中多个 KV cache group 是否位于同一 block：group 内多层共享 block table，但各层 KV tensor 的 page 并非一块连续内存；group 之间有独立 block table 和 block 列表，同时共享全局 BlockPool，非空物理 block 不会被同时复用。VA hybrid 场景中可能共享底层 Tensor，但依靠不同 block ID 访问不同 page；K3 packed layout 另有 parent page 到逻辑 block/subslot 的映射，不能等同于多个 group 共用一个 block。相关实现线索包括 `kv_cache_manager.py`、`kv_cache_utils.py`、`model_runner_v1.py`，并保留了路径已脱敏的引用。
