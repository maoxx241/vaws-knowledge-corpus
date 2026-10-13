---
block_id: dabb9edc44c97e86eaecbb62a6fea2d7df5e7931a7ced939967cdf5bd851480d
schema: mindie-material-block/1
source_range:
  end: 361906433
  part: 1
  start: 347586150
---

### user
这对吗？为什么按照现有的路径不能自然调用进去而需要初始化的时候创建实例？现有的
```
if activation == MoEActivation.SITU:
```


### assistant:commentary
我把这条分支和仓库里其他 MoE 激活的调用方式对照一下，重点确认：是 `if SITU` 的分发有问题，还是分支里调用 `SituAndMul` 的方式不合适。


### assistant:final_answer
**这个 `if` 没问题，而且日志证明已经进入了分支。** 问题是分支里的：

```python
SituAndMul(beta=..., linear_beta=...)(gate_up_out)
```

实际上做了两件事：先创建一个 `CustomOp` 模块，再执行计算。报错发生在**创建模块**时，不是 SiTU 计算时。

我前面说“需要初始化时创建实例”不够准确——**只有继续使用这个模块封装，才需要处理它的初始化生命周期；SiTU 计算本身不需要实例。**

上游 [MoE 激活分发](<redacted:local-path:fa9218c8e667>:170) 也是 `if SITU`，但里面直接调用算子，没有临时创建 `SituAndMul`。

所以更贴合当前结构的方向是：**保留现有分支和已经传进来的 beta 参数，直接调用无状态的 SiTU 计算入口，不必额外向整条 MoE 链路传递实例。** 需要注意上游那个入口是 CUDA 实现，Ascend 必须接对应的 NPU 实现，不能直接照搬。



### user
对啊我也觉得直接算就可以了吧，不需要再搞个实例传入啥的


### assistant:commentary
我按这个方向改：保留 `if SITU` 分发，直接做 SiTU 计算，不新增实例传递。顺便检查同文件另外两处相同调用，并验证没有 vLLM config 上下文时也能正常执行。


### assistant:commentary
这次是模型运行路径修复，我会按模型适配 skill 的最小改动原则处理，范围只限 SiTU 调用和对应测试，不恢复 nightly，也不新增部署文档或配置门禁。


### assistant:commentary
三处问题调用都可以换成同一个小函数，按上游公式直接计算；量化融合算子不动。原有 UT 中包住计算过程的 config 上下文也会去掉，先确认旧代码能复现失败，再验证修复。


### assistant:commentary
旧代码在这三条路径上都复现了同一个错误：forward 中创建 `SituAndMul` 会读取已经退出的配置上下文。现在已改为直接计算，正在跑修复后的测试；SiTU 公式和量化融合算子都不变。


### assistant:commentary
修复后，MoE MLP 和运行参数的 42 项 UT 全部通过，格式检查也通过了。三条原先报错的路径现在都不依赖配置上下文；我会把同一修复同步到父 PR 和 MoE 子 PR。153 的卡正在使用，本轮不重启服务，整模验证尚未重跑。


### assistant:final_answer
已按这个方向改好，并推送到[父 PR #14454](https://github.com/vllm-project/vllm-ascend/pull/14454) 和[子 PR #14599](https://github.com/vllm-project/vllm-ascend/pull/14599)。

保留 `if activation == MoEActivation.SITU`，三处直接调用纯张量计算函数，不创建或传递实例，也不补配置上下文。公式与上游一致，量化融合算子不动。

旧代码三条路径均复现失败；修复后两个分支各 **42 项 UT 通过**，格式检查通过。本轮未重跑整模服务。



### user
上游社区有situ的这个算子的custom op吗


### assistant:commentary
我查一下上游的注册、底层实现和调用路径，区分“有这个 custom op”和“Ascend 能否直接调用”这两件事。


### assistant:final_answer
有，上游已经有，而且分两层：

- **Python `CustomOp`**：`SituAndMul`，通过 `@CustomOp.register("situ_and_mul")` 注册。[源码](https://github.com/vllm-project/vllm/blob/6e448d0ea9bf3d88d898b65449ca6dc2aec170ac/vllm/model_executor/layers/activation.py#L161)
- **底层融合算子**：`torch.ops._C.situ_and_mul(out, input, beta, linear_beta)`。上游 MoE 的 `if SITU` 就直接调用它，**不需要创建或传入实例**。[调用位置](https://github.com/vllm-project/vllm/blob/6e448d0ea9bf3d88d898b65449ca6dc2aec170ac/vllm/model_executor/layers/fused_moe/activation.py#L170)

区别是这个底层实现走 CUDA/ROCm，没有对应的 NPU 注册，Ascend 不能直接拿来执行。[注册代码](https://github.com/vllm-project/vllm/blob/6e448d0ea9bf3d88d898b65449ca6dc2aec170ac/csrc/libtorch_stable/torch_bindings.cpp#L814)

也需要明确：刚才改的是直接用张量表达式修复运行问题，并不是接上了融合算子。上游融合实现还专门减少了 FP32 中间张量，因此**公式一致不代表性能和峰值显存一致**，这部分不能靠 UT 判断。[kernel 实现](https://github.com/vllm-project/vllm/blob/6e448d0ea9bf3d88d898b65449ca6dc2aec170ac/csrc/libtorch_stable/activation_kernels.cu#L469)



### user
那我理解是不是直接像别的算子一样直接继承一下然后封装个forward oot就行？


### assistant:commentary
我核对一下仓库里 `register_oot` 的现有写法，以及 MoE 能否自然走到它。关键是：覆盖 NPU 实现和避免在 forward 里创建实例，是两个不同的问题。


### assistant:final_answer
**作为 Ascend 算子的封装方式，是的**：继承 `SituAndMul`、注册 OOT、实现 `forward_oot`，和现有 [AscendSiluAndMul](<redacted:local-path:618b1af7bb9d>:35) 一样。

不过要区分两条调用路径：

- **普通 MLP／共享专家**：模型已经有 `self.act_fn` 实例，注册后可以自然进入 `forward_oot`，不用额外传实例。
- **路由 MoE**：现有分支直接调用计算函数，不经过激活层实例。它应该和 `forward_oot` **共用同一份无状态计算实现**，不能在 forward 中重新创建对象。

另外，如果 `forward_oot` 只是调用 `forward_native`，上游默认已经这么做了，再加子类没有实际收益；继承也不会绕过构造时读取配置的问题。[上游实现](https://github.com/vllm-project/vllm/blob/6e448d0ea9bf3d88d898b65449ca6dc2aec170ac/vllm/model_executor/custom_op.py#L169)

所以合理结构是：**一份计算实现，模型通过 OOT 调用，MoE 直接调用，不新增实例传递。**



### user
那这里为啥不能直接调用上游的`SituAndMul`


### assistant:commentary
我把“调用已创建的 `SituAndMul`”和“在 MoE 里临时创建它”这两种情况再对照一下，确认限制到底在哪里。


### assistant:final_answer
**可以调用上游 `SituAndMul`，限制在创建时机，不是这个类不能在 Ascend 上用。** 我前面没有把它和底层 CUDA kernel 区分清楚。

原来这句：
```python
SituAndMul(beta=beta, linear_beta=linear_beta)(hidden_states)
```
会在每次 forward 中先创建对象。构造函数会读取 vLLM 编译配置，而出错的 profile 阶段没有该上下文，所以**还没执行 SiTU 计算就报错了**。[构造路径](https://github.com/vllm-project/vllm/blob/6e448d0ea9bf3d88d898b65449ca6dc2aec170ac/vllm/model_executor/custom_op.py#L130)

正常复用方式是：
```python
# 层初始化时创建一次
self.act_fn = SituAndMul(beta=beta, linear_beta=linear_beta)

# forward 中直接调用
hidden_states = self.act_fn(hidden_states)
```
Ascend 默认就走 `forward_oot → forward_native`，**不需要额外继承一遍**。[默认实现](https://github.com/vllm-project/vllm/blob/6e448d0ea9bf3d88d898b65449ca6dc2aec170ac/vllm/model_executor/custom_op.py#L169)

现在的纯函数方案，是为了保持 MoE 现有函数接口、不增加实例传递；**不是技术上必须重写上游公式**。如果复用上游类，就需要让 MoE 执行路径访问初始化好的那个实例。



### user
这个创建能在fused moe去做吗


### assistant:commentary
我看一下当前 FusedMoE 的初始化位置和 MLP 调用链，确认能否由它持有实例，以及最少需要改哪几处。


### user
先看上游怎么做的，上游是怎么解决这个问题的


### assistant:commentary
好，先只看上游实际怎么做。我按当前依赖的 v0.27.1 核对模型初始化、路由 MoE 和激活分发的调用链，暂不改代码。


### assistant:final_answer
核对了 **v0.27.1（`6e448d0`）**：**上游路由 MoE 不创建 `SituAndMul` 实例，而是直接调用底层算子，所以没有触发这个构造时读取配置的问题。**

上游分两条路径：

1. **普通 MLP／共享专家**：在 `KimiMLP.__init__` 创建 `self.act_fn = SituAndMul(...)`，forward 复用已有实例。[模型代码](https://github.com/vllm-project/vllm/blob/6e448d0ea9bf3d88d898b65449ca6dc2aec170ac/vllm/models/kimi_k3/nvidia/model.py#L210)

2. **路由专家**：从 `FusedMoEConfig` 取出两个 beta，经过 `apply_moe_activation()` 的 `if SITU` 分支，直接执行：
   ```python
   torch.ops._C.situ_and_mul(output, input, beta, linear_beta)
   ```
   全程不创建或传递 `SituAndMul` 对象。[参数传递](https://github.com/vllm-project/vllm/blob/6e448d0ea9bf3d88d898b65449ca6dc2aec170ac/vllm/model_executor/layers/fused_moe/modular_kernel.py#L882) · [算子调用](https://github.com/vllm-project/vllm/blob/6e448d0ea9bf3d88d898b65449ca6dc2aec170ac/vllm/model_executor/layers/fused_moe/activation.py#L170)

AMD 的 AITER 融合路径也是直接把 beta 参数交给融合 MoE，不创建这个激活对象。[AITER 路径](https://github.com/vllm-project/vllm/blob/6e448d0ea9bf3d88d898b65449ca6dc2aec170ac/vllm/model_executor/layers/fused_moe/experts/rocm_aiter_moe.py#L395)

所以，**在 FusedMoE 初始化中保存实例虽然可行，但不是上游这里的做法，也不是必要修改。** 前面我把这个可行选项说得过重了。对齐上游应该保留函数式 MoE 分发，解决 Ascend 侧对应计算实现；不能为了复用 Python 类，再引入一套实例传递。

本轮只核对源码，没有改代码。



### user
ok那我理解是不是前面的改动就行了，不需要再搞个SituAndMul对吧


### assistant:final_answer
对，保留前面已提交的改动即可：MoE 的 `if SITU` 直接调用计算函数，**不再新增 `SituAndMul` 实例、继承类或 `forward_oot` 封装**。

普通 MLP／共享专家原本已有的上游 `SituAndMul` 保持不动，量化融合算子也不动。

### user
看下这个问题，我理解这是一个很危险的场景，而且没有在之前的版本感知到过，看下代码分析下根因<redacted:local-path:0fbebaacc46b>.md


### assistant:commentary
我先读这份分析，再沿着当前 PR 和旧版的 PD fallback 调用链核对：确认触发条件、是否会影响精度，以及差异从哪里引入。这轮先分析根因，不改代码。


### assistant:commentary
这是 K3 的 ACLGraph/EP 运行路径问题，我会按模型适配 skill 核对上游契约和版本差异。文档已经区分了“整网未提交 replay”和“单算子掉出图”，但“究竟哪个 DP rank、哪项条件触发 eager”仍需证据，不能仅凭这 8 张同属 DP0 的卡下结论。


### assistant:commentary
发现一个关键点：跨 DP 对 graph mode 取 `min()` 并不是这次 K3 PR 新加的，上游 vLLM 0.27.1 也这样做。接下来重点核对 DSpark 的 handoff 首轮实际调度几个 token——如果本来就是“新请求 1 token、老请求 8 tokens”，旧版也会判成非 uniform，不能直接归因于新加的 prompt 完整性检查。


### assistant:commentary
找到更具体的冲突了：vLLM 0.27.1 的 scheduler 会把 handoff 的 1 个真实 token 补成 `1+7`，目的就是让它和正在 decode 的请求一起入图；但 PR 的 worker 又因 `computed=N-1 < prompt=N` 将它拒绝。这里的 `N-1` 是 Mamba PD 特意传递的状态位置，不等于 KV 还没传完。我继续核对 KDA metadata 是否也支持这类补齐请求，判断该收窄哪一层的判定。


### assistant:commentary
上游已经有一个直接对应此问题的修复：#45237 专门通过 handoff 投机补齐，避免一个 D rank 把其他 DP rank 拖入慢路径，0.26 和 0.27 都包含它。当前 PR 的严格判断把这个优化抵消了；而 KDA builder 本身已有“带 recurrent state 的短 prompt 走 decode/spec metadata”的处理。我正在做 CPU 侧的新旧 dispatch 对照，不占 NPU、不启动服务。


### assistant:final_answer
确认了：**当前 PR 存在一个可以复现的性能回归，直接触发点是此前 PD 改动加上的过严判断。** 文档对“一个 rank 的 NONE 扩散到全部 DP”的分析正确，但把 prompt 完整性检查认定为必须保留、优先重构 DP 同步，方向不够准确。

### 1. 根因：worker 抵消了上游已经实现的优化

正常的 K3 PD 交接是：

```text
P 计算并传递 h(N−1)
→ D 确认接收完成
→ D 从 computed=N−1 开始计算最后一个 prompt token
```

这是 Mamba 的正常状态交接，**`computed < prompt` 不等于 KV 尚未传完**。[交接实现](<redacted:local-path:19aa8def0abf>:1858)

当 D 上已有投机 decode 请求时，上游 scheduler 会把新请求的这 **1 个真实 token，加上 7 个无效 draft 占位，补成 8 tokens**，保持整个 batch 的 uniform shape。这正是上游 [#45237](https://github.com/vllm-project/vllm/pull/45237) 解决的问题，0.26、0.27 都包含这个优化。

但当前 PR 又在 [graph 判定处](<redacted:local-path:9ddb37264de3>:2749) 要求：

```python
num_computed_tokens >= num_prompt_tokens
```

结果就是：

```text
scheduler：已经补齐，可以走 uniform decode
worker：N−1 < N，拒绝入图
DP 同步：一个 NONE → 全部 NONE
```

### 2. 为什么之前的 0.26 没有表现出来

我固定同一个 **vLLM 0.27.1 dispatcher**，分别运行 0.26 和当前 PR 的实际 worker 函数，CPU 对照结果如下；DP all-reduce 使用预设向量模拟，没有启动模型：

| 场景 | 0.26 判定 | 当前 PR 判定 |
|---|---|---|
| 稳定投机 decode | FULL | FULL |
| 一个 handoff 请求已补齐到 8 tokens | FULL | NONE |
| DP4 中仅一个 rank 包含上述请求 | 全部 FULL | 全部 NONE |
| handoff 未补齐，仍是 `[8,1]` | NONE | NONE |

新增约束来自文档提到的 `fc9818f2`，当前父 PR 中对应 `3ed1cc7d`。**跨 DP 取 `min()` 是旧逻辑，上游也有；本次新增的是不必要地产生 NONE 的条件。**

### 3. 这个“正确性保护”与现有实现本身矛盾

当前代码已经有两处支持这种交接：

- [投机 metadata 构造](<redacted:local-path:9ddb37264de3>:1272)：明确允许 KV consumer 的新请求进入 spec 路径，不要求 prompt 全部 computed。
- [GDN builder](<redacted:local-path:d7ccb3d8bcbd>:55)：已有 recurrent state 的单 token prompt 使用 decode metadata；也支持 stateful spec-width prompt。

对应的 **4 个既有 CPU 用例通过**。因此，不能把“所有 prompt 未完成的请求都禁止入图”当成通用正确性要求。此前新增 UT 只是断言这个限制生效，并没有验证它与上述完整路径是否一致。

### 4. 风险和修正方向

这确实危险：动态请求持续交接时，会反复把整个 DP/EP group 拉入 eager，影响所有在途请求，而不仅是新请求。

另外还确认了一个独立隐患：**SP、oproj TP、embedding TP 都关闭时，混合 local mode 会让不同 rank 得到不同的 DP padding metadata。** 这个问题旧版也存在，本次改动增加了触发机会；目前不能据此断言已经发生通信卡死或精度错误。

应优先**撤销或收窄 worker 中过严的 prompt-complete 限制，对齐已有 handoff 补齐和状态 metadata 契约**，保留真正无初始状态等场景的处理。不是先删除 `min()`、引入跨 rank graph/eager 混跑。

本轮未修改代码或启停服务。代码级回归已复现；报告第 21、22 轮究竟由哪个请求触发，仍缺现场各 DP 的调度值，不能冒充已经实测确认。



### user
先改吧，然后这个问题为什么会在14454号pr引入，改什么的时候带上的？


### assistant:commentary
我先修正这处判定，保留无初始状态等场景的必要区分，并验证 PD handoff 补齐后能正常入图。同时追溯引入它的提交，说明当时在修什么、为什么把范围扩大到了这里。


