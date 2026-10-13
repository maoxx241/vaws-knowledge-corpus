---
block_id: a0cff5a3185e77d984fb0f5be1d34a1cddc3a5dad1a9b39efdde41a2baf68ce9
schema: mindie-material-block/1
source_range:
  end: 13695805
  part: 1
  start: 9118634
---

### assistant:commentary
修复已推到 PR，head 为 `963205d71`，DCO 已通过，新一轮 E2E 已启动。远端测试容器已恢复为干净状态，现有业务进程和设备占用没有被改动；接下来重点盯两套 A2 part 5 和 A3 四卡 part 3。

### assistant:commentary
新一轮目前还在覆盖率选测和 pre-commit 阶段，尚未生成硬件分片；没有出现新的失败。我会在矩阵展开后先确认 A2/A3 目标用例确实被选中，再按实际 job 日志判断。

### assistant:commentary
选测矩阵已确认：A2 part 5 同时包含共享专家全模型 ACLGraph 回归和新增 V2 算子用例；A3 四卡 part 1 包含此前失败的 `test_graph_mode.py`。这两套都会在 main 与 v0.27.1 各跑一遍，覆盖范围正确。

### assistant:commentary
main 的 A2 目标分片已拿到 910B runner，正在初始化；另外三套关键分片仍在硬件队列。当前没有失败信号。

### assistant:commentary
main 和 v0.27.1 两套 A2 分片都已开始，当前在安装带设备扩展的 PR 构建；这一步之后才进入用例串行执行。A3 四卡仍排队，远端 A3 的最小捕获/重放验证已经作为先行证据通过。

### assistant:commentary
main A2 已进入 17 个选测的串行执行；v0.27.1 A2 仍在完成扩展安装。此前失败用例排在第 7 个，前面包含较重的 LoRA/推测解码测试，所以结果不会立刻出来。

### assistant:commentary
两套 A2 都已进入执行阶段，状态稳定；A3 runner 仍未释放。由于 GitHub 在 job 结束前不开放逐条测试日志，我目前用 job/step 状态监控，结束后会立刻拉取完整日志核对具体用例，而不只看总绿灯。

### assistant:commentary
A2 main/v0.27.1 仍在正常跑选测，没有提前退出；这正是预期的长耗时阶段。当前 head、本地工作树和远端测试容器都保持干净。

### assistant:commentary
状态无变化：两套 A2 长测仍在执行，A3 四卡仍排队，没有失败或 runner 中断。我继续等真实结果，不会因为排队时间长就提前收尾。

### assistant:commentary
截至 12:45 UTC，main/v0.27.1 的 A2 仍在同一选测步骤正常运行；没有提前失败。按上一轮相同分片的耗时，完整结果通常需要约 45–50 分钟。

### assistant:commentary
发现两套 CPU 分片都失败了，A2/A3 仍在跑。CPU 双分支同时失败通常是新单测本身或兼容性问题，我先拉具体日志；如果是确定性缺陷会立即修正并推新 head，硬件分片由 GitHub 自动切换到新提交。

### assistant:commentary
CPU 失败已定位并修掉：CPU 镜像的 `torch_npu` 不暴露设备专用 V2 符号，单测 mock 少了 `create=True`；生产代码未执行到该分支。修复已作为 `5140a857f` 推送，完整格式检查通过，新 E2E 会自动替换旧硬件运行。

### assistant:commentary
新一轮已进入队列，当前仍是 coverage/pre-commit 前置阶段，没有失败。CPU 单测修复会先在两条 CPU 分支中得到验证；通过后再等待 A2/A3 runner。

### assistant:commentary
coverage 推荐已通过，pre-commit 仍在执行 mypy/收尾；尚未进入硬件矩阵。当前没有新失败。

### assistant:commentary
我继续盯当前最新提交的 CI；先等 CPU、A2、A3 相关矩阵全部展开，再逐个核实真实设备用例结果。远端现有业务进程保持不动。

### assistant:commentary
pre-commit 和选例阶段都已通过；现在正在生成设备矩阵与缓存检查，尚未出现代码失败。矩阵出来后我会重点盯两套分支上的 A2 shared-expert/V2 和 A3 graph-mode。

### assistant:commentary
设备矩阵已展开，目标任务齐全：两套基线各有 CPU、A2 单卡 part 5，以及 A3 四卡 part 1/3。目前都在排队，没有遗漏目标用例。

### assistant:commentary
两套 CPU 任务已经运行；两套 A2 part 5 也已拿到机器开始执行。A3 四卡仍在等 runner。当前失败列表为空。

### assistant:commentary
CPU 已进入实际 pytest，A2 正在完成 checkout/环境准备，尚未跑到设备测试主体。暂时没有失败步骤；我会等到 A2 目标测试完成后再判断修复是否闭环。

### assistant:commentary
刚补的 CPU mock 回归已确认有效：main 与 v0.27.1 两套 CPU 任务都通过。接下来只剩真实 A2/A3 结果；A2 两套仍在执行，A3 仍排队。

### assistant:commentary
两套 A2 都已进入“Run selected tests with device”，不是环境准备阶段了。该分片通常较慢，目标 shared-expert 图模式用例在分片后段；我会等它实际跑完。

### assistant:commentary
A2 设备分片仍在稳定运行，GitHub 侧没有失败步骤或提前退出；A3 runner 还未释放。我会继续保持当前 head，不再用无关提交打断这些长任务。

### assistant:commentary
目前没有新失败：CPU 两套已绿，A2 两套设备 pytest 持续运行，A3 仍等四卡资源。最长耗时就在这里，我继续盯到结果落定。

### assistant:commentary
A2 两个分片目前都没有提前失败，仍在同一设备测试步骤；CPU 结果保持全绿。A3 四卡资源还没排到，所以我先不做结论。

### assistant:commentary
当前 head 没有新增失败，A2 仍在长测。这里我只监控，不会触碰远端已有服务，也不会为了抢卡清理别人的进程。

### assistant:commentary
A2 实机测试已连续运行约 10 分钟，两个分支都还在正常执行，失败列表仍为空；A3 队列尚未变化。

### assistant:commentary
没有状态反转：两套 A2 长测继续运行，CPU 已通过，A3 等 runner。当前 CI 全局失败列表仍为空。

### assistant:commentary
A2 仍在执行实际设备分片，未出现 crash、timeout 或测试失败；A3 四卡作业还在等待调度。继续监控。

### assistant:commentary
没有新的失败信号；两套 A2 长测继续，A3 仍排队。修复提交保持不变，避免重新触发整轮 CI。

### assistant:commentary
A2 实机阶段已跑约 19 分钟，main/v0.27.1 都仍在进行且无失败；A3 还在等四卡资源。目标用例通常位于该分片后段。

### assistant:commentary
当前无新失败，A2 两套设备测试仍正常运行；A3 四卡仍未调度。继续等目标用例的最终退出码。

### assistant:commentary
A2 长测继续，失败列表为空；A3 四卡任务还在排队。现阶段没有需要追加代码的信号。

### assistant:commentary
两套 A2 仍在实际设备 pytest，均未失败；A3 runner 仍未释放。继续等，不做额外推送。

### assistant:commentary
A2 两套已运行约半小时，仍无失败；CPU 保持全绿，A3 仍等待四卡 runner。继续盯。

### assistant:commentary
A2 设备分片仍正常，失败列表为空；A3 四卡仍排队。现在还没有需要回滚或补丁的证据。

### assistant:commentary
A2 两套仍在跑同一设备分片，未见失败；A3 依旧等四卡资源。继续监控最终结果。

### assistant:commentary
A2 两套继续稳定运行，失败列表仍为空；A3 四卡任务尚未拿到 runner。继续等待。

### assistant:commentary
A2 设备分片已运行约 34 分钟，两套仍无失败；快接近上一次目标用例出现的位置。A3 继续排队。

### assistant:commentary
A2 两套仍在长测且失败列表为空；A3 四卡任务尚未启动。继续等关键 shared-expert 图模式用例的结果。

### assistant:commentary
A2 仍在实际设备测试，两个分支都没有失败记录；A3 继续排队。当前修复没有暴露新的回归信号。

### assistant:commentary
A2 实机分片已接近 40 分钟，两个任务仍在运行且没有失败结论；这正是此前 shared-expert 图比较失败出现前后的时间段。我继续等分片完成。

### assistant:commentary
A2 两套仍未退出，GitHub 也尚未标记任何失败；CPU 通过，A3 仍排队。继续守着最终结果。

### assistant:commentary
A2 已越过此前失败大致出现的时间点，两套任务仍在运行且没有失败结论；我会等整个分片结束再确认。A3 四卡仍排队。

### assistant:commentary
A2 两套分片仍正常收尾，当前失败列表为空；CPU 已绿，A3 四卡继续排队。我会等 GitHub 给出正式 success。

### assistant:commentary
A2 两套仍停留在实际设备 pytest 步骤，没有进入失败或清理分支；已接近该分片常见完成时长。A3 四卡仍排队。

### assistant:commentary
A2 设备 pytest 已约 49 分钟，仍未失败，接近历史上该分片的总时长；继续等正式完成状态。A3 四卡任务尚未调度。

### assistant:commentary
A2 两套刚同时失败，说明当前 fallback 仍没覆盖真实 full-model 图路径。我现在直接抓两份失败日志做差异定位；A3 先保持排队，不做猜测性改动。

### assistant:commentary
两套日志完全一致：eager 与 multistream-eager 已经相等，只是 ACLGraph replay 输出错；因此问题收敛为 A2 图捕获下 V2 调用的动态参数绑定，而不是 multistream 并发本身。接下来我核对 full-model 传入的量化类型/权重形态，修正实际命中的分支。

### assistant:commentary
我先在准备好的 A3 环境跑新增分支回归：这次改动只在“A2 + W8A8 + 正在捕获 ACLGraph”时退回已验证的拆分 kernel，eager 与 A3 仍走 V2。不会启动/停止现有服务。

### assistant:commentary
远端测试容器已确认是独立准备环境；同步最新 head 正在进行，现有业务容器和进程均未改动。同步完成后只跑新增的分支选择回归。

### assistant:commentary
远端访问 GitHub 超时，所以我不再等外网；改为把本地从已验证基线到当前工作树的精确补丁直接送进测试 clone，先 `git apply --check`，再执行。

### assistant:commentary
新增回归确实进入了拆分路径；测试本身在 A3 镜像里优先选了 Triton 激活，但没有初始化 Triton 设备属性。生产图捕获会初始化它。为让 CPU 路由测试只验证分支选择，我把测试固定到非 Triton 的已存在拆分实现。

### assistant:commentary
远端分支回归已通过：A2 捕获时走拆分 kernel、非捕获仍走 V2；设备适配层的两条 packed W8A8 路径也通过。现在补跑这两个测试文件的完整 UT，并做全量格式检查。

### assistant:commentary
完整 UT 中 37 项通过，唯一失败是已有的 tiny-shape SwiGLUStep CPU/Triton 测试夹具（与本次改动无关）；本次新增与关联测试均通过。格式检查全绿。接下来我先看 A3 卡占用，再只跑轻量真实算子回归。

### assistant:commentary
刚才 A3 算子失败是测试进程未开启 torch-npu 内部格式，权重 format-cast 被降回普通一维 storage；这是测试环境开关，不是算子结果。按算子测试要求显式开启内部格式后重跑。

### assistant:commentary
A3 实机算子回归已通过：TensorList count/cumsum 对齐用例 1/1，packed single-tensor V2 用例 1/1。现在清理仅用于测试的补丁副本并恢复我临时启动的空闲容器，然后提交推送。

### assistant:commentary
修复已形成带 sign-off 的提交：仅 A2 W8A8 图捕获退回拆分 kernel，eager/A3 继续 V2。现在直接推到 PR，并监控新一轮 CPU、A2、A3。

### assistant:commentary
最新 head 已触发 E2E；DCO 已绿。上一轮的本地轮询已停止，当前只监控新 run。

### assistant:commentary
新 run 已从 pending 进入执行，当前还在 pre-commit/选例前置阶段，失败列表为空。

### assistant:commentary
coverage 推荐阶段已通过；pre-commit 正在运行，暂时没有失败，设备矩阵尚未展开。

### assistant:commentary
pre-commit 已正式通过；正在进入选例/设备矩阵生成，失败列表为空。

### assistant:commentary
选例阶段也通过；当前只等 hitest 与 csrc 缓存前置任务启动，设备矩阵尚未展开。

