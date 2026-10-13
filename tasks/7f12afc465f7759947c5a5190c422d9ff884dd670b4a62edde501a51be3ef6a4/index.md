---
blocks:
- block_id: 6382cc29060e1598ebc9c184c1c2c80073a065585745ff3c088d06275775bbf9
  indexed: true
  sha256: 7d649fde46ca079c4336c52d73ec19f00fd1fa826d62397e5c9091b2f6e4602b
  source_range:
    end: 2051867
    part: 0
    start: 0
  summary: 第一版个人身份绑定与 Fork 流程已安装并推送，报告称 Coordinator 377 项、工作区 138 项通过，但真实服务器协作、NPU 缓存验收及官方 Release 传播仍未验证；后续方向改为周期性跟随主仓，且既有 Release-only watcher 需重启才会加载新逻辑。对 153–156 四机完成只读清点：固定容器、16 个 NPU/机的 Worker、NFS 权重路径和编译产物均核对，未修改资源；`/tmp/vaws-native-cache` 不存在，coordinator 无活动任务。最终后验报告称 4/4 通过，容器与 Worker 未变，8 个请求分别为 4 个 CPU `released`、4 个排队请求 `cancelled`，数据库升级到 schema 6；这些是报告中的检查结果，仍应保留其证据范围与未覆盖项。
  title: 个人 Fork 与四机只读验证、后验检查
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 7f12afc465f7759947c5a5190c422d9ff884dd670b4a62edde501a51be3ef6a4
  kind: experience
  material_digest: 8d2ffa5c3219c028af78701f2ccadaa149b6371cd12a21d575c2c8c7f687fac4
  revision: 6b01649d16c30095bef53a6af919922589223c82dfcf2d15c3f01121885798a2
  schema: mindie-entry/3
  summary: 重点检索个人 Fork/身份绑定、周期性主仓更新、watcher 重启条件，以及 153–156 四机的只读资源清点和最终后验。已报告的测试与 4/4 后验结果需结合证据范围理解：真实服务器协作、NPU 缓存验收和完整更新传播此前明确未验证；未发现的活动任务、缓存目录缺失及请求释放/取消状态也应保留为当前上下文。
  title: 当前任务导航：四机主仓跟随与验证
navigation: 重点检索个人 Fork/身份绑定、周期性主仓更新、watcher 重启条件，以及 153–156 四机的只读资源清点和最终后验。已报告的测试与 4/4 后验结果需结合证据范围理解：真实服务器协作、NPU 缓存验收和完整更新传播此前明确未验证；未发现的活动任务、缓存目录缺失及请求释放/取消状态也应保留为当前上下文。
schema: mindie-material-task/1
status: complete
task_id: 7f12afc465f7759947c5a5190c422d9ff884dd670b4a62edde501a51be3ef6a4
---

# 当前任务导航：四机主仓跟随与验证

重点检索个人 Fork/身份绑定、周期性主仓更新、watcher 重启条件，以及 153–156 四机的只读资源清点和最终后验。已报告的测试与 4/4 后验结果需结合证据范围理解：真实服务器协作、NPU 缓存验收和完整更新传播此前明确未验证；未发现的活动任务、缓存目录缺失及请求释放/取消状态也应保留为当前上下文。

## Materials

- [个人 Fork 与四机只读验证、后验检查](blocks/6382cc29060e1598ebc9c184c1c2c80073a065585745ff3c088d06275775bbf9.md): 第一版个人身份绑定与 Fork 流程已安装并推送，报告称 Coordinator 377 项、工作区 138 项通过，但真实服务器协作、NPU 缓存验收及官方 Release 传播仍未验证；后续方向改为周期性跟随主仓，且既有 Release-only watcher 需重启才会加载新逻辑。对 153–156 四机完成只读清点：固定容器、16 个 NPU/机的 Worker、NFS 权重路径和编译产物均核对，未修改资源；`/tmp/vaws-native-cache` 不存在，coordinator 无活动任务。最终后验报告称 4/4 通过，容器与 Worker 未变，8 个请求分别为 4 个 CPU `released`、4 个排队请求 `cancelled`，数据库升级到 schema 6；这些是报告中的检查结果，仍应保留其证据范围与未覆盖项。
