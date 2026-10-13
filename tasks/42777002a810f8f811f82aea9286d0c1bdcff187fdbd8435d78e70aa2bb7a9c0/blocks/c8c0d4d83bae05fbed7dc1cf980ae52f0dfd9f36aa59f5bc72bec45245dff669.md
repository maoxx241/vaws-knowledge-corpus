---
block_id: c8c0d4d83bae05fbed7dc1cf980ae52f0dfd9f36aa59f5bc72bec45245dff669
schema: mindie-material-block/1
source_range:
  end: 5275362
  part: 0
  start: 0
---

### user
有人反馈现在的主线的dspark的采信率有问题，定位问题是哪里引入的，之前我记得14454那个pr的采信率是对的


### assistant:commentary
我先把 #14454 的实现和当前主线逐段对比，重点追踪 DSpark 的 draft、验证和采信统计链路，找出可能引入回归的提交。暂时只查代码和已有记录，不动四机服务。


### assistant:commentary
确认了一个关键点：#14454 今天刚合入主线，所以要同时检查“合入后的提交”和“这个 PR 后期自身的改动”，不能只看 merge 之后。我也会按已有的 K3 采信率核验流程，先锁定当时通过测试的具体 SHA、权重和测试条件，避免把不同版本的结果混在一起。


### assistant:commentary
已找到 #14454 当时的实测基线：`3053909a`，四机 GQA、7 个 draft token，8K/1K、每 DP 并发 1 的采信率是 **76.22%**。

目前最值得核对的是后期一处改动：删除了 draft 阶段“本轮拒绝 token 数”的 CPU 同步，改为直接复用序列长度。若这里仍包含刚被拒绝的 token，draft attention 就会读错 KV 范围。我正在沿调用链确认它是不是实际回归点。


### user
用153+154的双机验证，机器现在是你独占


### assistant:commentary
收到，我会在 153+154 做双机对照验证，先确认容器、权重和现有进程，再用同一组请求比较基线与主线。

代码审计已确认：序列长度那处改动会影响 MLA draft；GQA 路径有额外分支，不能直接认定它是 GQA 反馈的根因。我会结合双机实测继续缩小范围。


### assistant:commentary
153、154 的 NPU 都是空闲状态，已确认两边有现成的 #14454 验证容器，GQA/MLA draft 的配置哈希也一致。接下来只在这两个容器里建独立验证目录，保留旧代码、日志和结果。

双机按 **TP16 × DP2 × EP32** 跑；先做最小复现，再对可疑改动做单变量验证，不铺开性能矩阵。


### assistant:commentary
CPU 上已用真实的 Python 方法复现长度错误：本轮拒绝 2 个 token 时，主线 MLA draft 的 KV 长度会多算 2；旧版会扣掉，GQA 分支也仍然正确。这确认了 MLA 的代码缺陷，但还不能代表反馈方的采信率问题。

双机代码已上传，正在检查新旧版本各自的算子 ABI。当前尚未开始硬件请求测试。


### user
反馈说是gqa dspark的采信率问题，所以我理解你说的这个可能是问题但是应该不止这个问题


### assistant:commentary
对，已确认 GQA 会覆盖成 device 上的正确长度，所以那处 MLA 缺陷不能解释这次反馈，我不会把它当作本次根因。

153+154 当前启动的就是 **GQA DSpark**。后续只围绕 GQA 做对照，重点查 target 辅助 hidden states、KDA 状态更新，以及 draft 的 MarkovHead／采样链路。


### assistant:commentary
GQA 这条线继续单独查。153/154 的主线服务已进入双机 worker 初始化，还没完成真实请求；我会用固定 token 输入和 #14454 的已验证版本做配对测试，分别核对采信计数和输出，避免把 MLA 的问题当成这次结论。


### assistant:commentary
已确认 #14454 合入后，到这次固定的主线版本，GQA DSpark 核心代码没有再改；排查范围主要落在 #14454 从历史验证点到最终合入之间。双机还在模型初始化，我也在检查基线所需的旧算子接口，确保后面的对比不是混用了构建产物。


### assistant:commentary
这轮启动遇到一个独立的兼容问题：普通 W4A8 checkpoint 带有 `mm_projector.rot_proj`，但主线只在检测到 QuaRot 元数据时创建它，导致权重加载失败。这还不能说明采信率问题；我会保留失败日志，把配对测试统一设成纯文本模式，先排除这个无关的多模态加载阻碍。


