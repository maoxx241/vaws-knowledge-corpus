---
blocks:
- block_id: b24d03895e59132912d6852eab116556482d364786cebfe29528952bce77e2cf
  indexed: true
  sha256: e06fe5f8fd85b3e6fd2cf8e9854e82252b7720caf0a96f8f9c8a53cecb0b386e
  source_range:
    end: 1029884
    part: 0
    start: 0
  summary: 审查称远端 Triton H 实现与本地一致且可导入，并确认了 BNSD/BSND、state `[N,H,K,V]`/`[N,H,V,K]`、fp32 `gk` 及 42-task grid 等语义；但该结论来自静态核对，未占用 NPU，所列探针命令也未报告实际执行结果。建议用 N=1、4、5、7 的任务规模比较 AscendC 与 Triton，以检验跨 24-core 后失败、AscendC 多任务/ping-pong 调度或更高层 orchestration 等假设；当前全算子 q/k l2norm 设置不同，且仅使用零 initial state，因此精度 A/B 和非零 state 情况仍未验证。
  title: Triton H 子算子反向验证方案与未执行结论
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 8b417cbc4c3e184697450102bfb74085e3f7e7a56369d60b48016c2466e564e6
  kind: experience
  material_digest: 854a720c85d788b0b28daf8626c3a6395376ceb24a4930ceb36647bcf58a4058
  revision: 6011c15f1b5febb724d162bc5ac7488cba619492e9b87c0ca4710ed7d4b99ea4
  schema: mindie-entry/3
  summary: 当前材料完成了 Triton H 路径的静态语义、布局和探针设计核对，但没有实际 NPU 运行结果，不能据此确认算子故障归因。后续应保留并执行 N=1/4/5/7 的 AscendC/Triton 对照，区分 H 子算子调度问题、其他 stage/composite orchestration 与服务层问题；同时注意全算子配置差异及零 state 限制，非零 fp32 state 的精度检查尚未完成。
  title: Triton H 子算子验证导航
navigation: 当前材料完成了 Triton H 路径的静态语义、布局和探针设计核对，但没有实际 NPU 运行结果，不能据此确认算子故障归因。后续应保留并执行 N=1/4/5/7 的 AscendC/Triton 对照，区分 H 子算子调度问题、其他 stage/composite orchestration 与服务层问题；同时注意全算子配置差异及零 state 限制，非零 fp32 state 的精度检查尚未完成。
schema: mindie-material-task/1
status: complete
task_id: 8b417cbc4c3e184697450102bfb74085e3f7e7a56369d60b48016c2466e564e6
---

# Triton H 子算子验证导航

当前材料完成了 Triton H 路径的静态语义、布局和探针设计核对，但没有实际 NPU 运行结果，不能据此确认算子故障归因。后续应保留并执行 N=1/4/5/7 的 AscendC/Triton 对照，区分 H 子算子调度问题、其他 stage/composite orchestration 与服务层问题；同时注意全算子配置差异及零 state 限制，非零 fp32 state 的精度检查尚未完成。

## Materials

- [Triton H 子算子反向验证方案与未执行结论](blocks/b24d03895e59132912d6852eab116556482d364786cebfe29528952bce77e2cf.md): 审查称远端 Triton H 实现与本地一致且可导入，并确认了 BNSD/BSND、state `[N,H,K,V]`/`[N,H,V,K]`、fp32 `gk` 及 42-task grid 等语义；但该结论来自静态核对，未占用 NPU，所列探针命令也未报告实际执行结果。建议用 N=1、4、5、7 的任务规模比较 AscendC 与 Triton，以检验跨 24-core 后失败、AscendC 多任务/ping-pong 调度或更高层 orchestration 等假设；当前全算子 q/k l2norm 设置不同，且仅使用零 initial state，因此精度 A/B 和非零 state 情况仍未验证。
