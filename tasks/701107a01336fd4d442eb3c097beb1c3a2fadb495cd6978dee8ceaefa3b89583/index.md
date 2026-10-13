---
blocks:
- block_id: ec895765832b757a507c444f7bd1da9396effdedc54821e25d05f4a0744bab4a
  indexed: true
  sha256: 506be7182b2a00df3c40ddf81d5db2aae9c0cee2204709292fa6fea94c64fd83
  source_range:
    end: 4085537
    part: 0
    start: 0
  summary: 在四台机器上以 TP16×DP4×EP64、EP64 拓扑成功加载真实 Kimi K3 W4A8 权重并保持服务运行；eager 模式下 health 和基础 completion/chat smoke 请求返回 HTTP 200，但裸 prompt 续写不能作为精度通过，使用正确 XTML 模板后基础算术题回答正确。随后按完整 GPQA-Diamond 198题、32并发、nightly 参数切换到 ACLGraph，并分别计划关闭/开启 prefix cache；旧提交 2ed4fabd 的 Dynamo 属性访问导致 ACLGraph 初始化失败，应用 a0e7ccb2 的最小 Python 修复后编译图开始成功捕获，但此块结束时尚未完成评测。
  title: 64卡 Kimi W4A8 服务与 ACLGraph 重启
- block_id: 5fbd6b00ad29d6475cc23ea7f9724a7a87cb422da8b9bb5c4d5b2081c2e2a4d3
  indexed: true
  sha256: e19bfc0a85339893262cd858ce297fe05f1463da77b5f972d4fd646e75280091
  source_range:
    end: 4085537
    part: 1
    start: 0
  summary: ACLGraph 7/7 捕获成功并额外占用约 1.17 GiB 每逻辑 NPU；服务内 health 为 200，外部管理网请求曾返回 502，后确认是本机代理响应。第一轮完整198题、32并发、prefix cache关闭的 GPQA 评测持续正常，未出现请求失败或答案解析失败；截至本块末为197/198，181对16错、实时准确率约91.88%，最后样本 id=71 仍在持续生成。多道样本出现数万 token 的超长 thinking 输出，曾观察到 65,536 上限风险但未确认最终结果，也尚未开始 prefix cache 开启轮。
  title: GPQA 关闭 prefix cache 的长尾评测
- block_id: 08d8e4a6ddaf816ea14700d56c78bd5ae3cf969cb279b8f4e61f957650f9c06e
  indexed: true
  sha256: 3cd2b43a6167cfd5279481b056793ef7470ae184cad2a0e12a14eb16d239e020
  source_range:
    end: 4085537
    part: 2
    start: 0
  summary: 关闭 prefix cache 的第一轮仍停留在197/198，最后样本 id=71 持续正常解码，没有报错、超时或服务异常。该样本从约1.2万 token 增长到约2.8万 token，约8.9 tok/s，尚未达到65,536 token上限，因此完整198题准确率和 finish_reason 仍未确定。后续切换 prefix cache、第二轮评测及两轮对比结果在这些材料中尚未出现。
  title: GPQA 第一轮最后样本未完成
- block_id: 55444900b15ebcf641c9aa8ba7be643f3462c93155ffc1654caf53a13dc24615
  indexed: true
  sha256: 406ad0cf1a4814454ad6a0bdb9408a6de76ebd393d425723e0237d4f0bc5673e
  source_range:
    end: 5552043
    part: 0
    start: 4085537
  summary: 'prefix-cache-off 的完整 GPQA-Diamond 198 题最终完成，结果为 181/198（91.41%），0 请求失败、0 未解析答案；全部样本 `finish_reason=stop`，id=71 最终预测 B、答案 A。prefix-cache-on 仅完成服务初始化至权重加载阶段，随后按用户要求停止，未开始评测；四台服务及残留 EngineCore/Worker 均已清理。GPQA 命令、服务拉起脚本和四机配置已更新到 PR #4 评论，评论发布成功。'
  title: GPQA prefix-off 完成与 PR 复现信息更新
- block_id: 045d74269a12b406ecea9e981982951e362b8256e44566eb8d31db8ff1648fff
  indexed: true
  sha256: d6c5ce79d57ddad157447f1bb98e25bb103f7f7ac9233f044ffb7f86979f93da
  source_range:
    end: 5552043
    part: 1
    start: 4085537
  summary: 复现流程记录了容器内 `/health`、`/v1/models`、Chat Completions curl、代理导致 502 时的 `--noproxy '*'` 处理，以及完整 198 题 GPQA runner；已知 prefix-cache-off 基准为 181/198、91.4141%、1,170,381 completion tokens、13,308 秒。服务停止可先 TERM，若遗留 `VLLM::EngineCore` 或 `VLLM::Worker` 则按进程标题清理；当前四台服务均已停止。当前目录已创建并精简 `AGENTS.md`，将拉起、监控、curl、GPQA 和停止步骤迁移至 `SERVICE_RUNBOOK.md`，并保留基础拓扑、路径、修复与验证结论。
  title: 服务复现手册与基础环境文档拆分
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 701107a01336fd4d442eb3c097beb1c3a2fadb495cd6978dee8ceaefa3b89583
  kind: experience
  material_digest: a0f04199a23ce6b13f5b2ccbe3952dbb26e848b5e906bca012649fefdd9a31f5
  revision: 7b76730cb00ad25cef12803dffe2241d8d20e6d2702639689298b9841e2862c7
  schema: mindie-entry/3
  summary: '此前关于 prefix-cache-off 仍未完成的状态已被后续结果修正：完整 GPQA-Diamond 198 题已完成，准确率为 181/198（91.41%），0 请求失败、0 解析失败。prefix-cache-on 只启动到权重加载阶段即停止，尚无可比较的 on 轮精度或性能结果；当前四机服务已停止。服务拓扑、拉起/健康检查/curl/GPQA/停止清理步骤位于 `SERVICE_RUNBOOK.md`，`AGENTS.md` 仅保留基础环境信息；PR #4 评论已包含复现命令、拉起脚本和结果。'
  title: 当前任务导航：Kimi K3 服务复现与 GPQA 基准记录
navigation: '此前关于 prefix-cache-off 仍未完成的状态已被后续结果修正：完整 GPQA-Diamond 198 题已完成，准确率为 181/198（91.41%），0 请求失败、0 解析失败。prefix-cache-on 只启动到权重加载阶段即停止，尚无可比较的 on 轮精度或性能结果；当前四机服务已停止。服务拓扑、拉起/健康检查/curl/GPQA/停止清理步骤位于 `SERVICE_RUNBOOK.md`，`AGENTS.md` 仅保留基础环境信息；PR #4 评论已包含复现命令、拉起脚本和结果。'
schema: mindie-material-task/1
status: complete
task_id: 701107a01336fd4d442eb3c097beb1c3a2fadb495cd6978dee8ceaefa3b89583
---

# 当前任务导航：Kimi K3 服务复现与 GPQA 基准记录

此前关于 prefix-cache-off 仍未完成的状态已被后续结果修正：完整 GPQA-Diamond 198 题已完成，准确率为 181/198（91.41%），0 请求失败、0 解析失败。prefix-cache-on 只启动到权重加载阶段即停止，尚无可比较的 on 轮精度或性能结果；当前四机服务已停止。服务拓扑、拉起/健康检查/curl/GPQA/停止清理步骤位于 `SERVICE_RUNBOOK.md`，`AGENTS.md` 仅保留基础环境信息；PR #4 评论已包含复现命令、拉起脚本和结果。

## Materials

- [64卡 Kimi W4A8 服务与 ACLGraph 重启](blocks/ec895765832b757a507c444f7bd1da9396effdedc54821e25d05f4a0744bab4a.md): 在四台机器上以 TP16×DP4×EP64、EP64 拓扑成功加载真实 Kimi K3 W4A8 权重并保持服务运行；eager 模式下 health 和基础 completion/chat smoke 请求返回 HTTP 200，但裸 prompt 续写不能作为精度通过，使用正确 XTML 模板后基础算术题回答正确。随后按完整 GPQA-Diamond 198题、32并发、nightly 参数切换到 ACLGraph，并分别计划关闭/开启 prefix cache；旧提交 2ed4fabd 的 Dynamo 属性访问导致 ACLGraph 初始化失败，应用 a0e7ccb2 的最小 Python 修复后编译图开始成功捕获，但此块结束时尚未完成评测。
- [GPQA 关闭 prefix cache 的长尾评测](blocks/5fbd6b00ad29d6475cc23ea7f9724a7a87cb422da8b9bb5c4d5b2081c2e2a4d3.md): ACLGraph 7/7 捕获成功并额外占用约 1.17 GiB 每逻辑 NPU；服务内 health 为 200，外部管理网请求曾返回 502，后确认是本机代理响应。第一轮完整198题、32并发、prefix cache关闭的 GPQA 评测持续正常，未出现请求失败或答案解析失败；截至本块末为197/198，181对16错、实时准确率约91.88%，最后样本 id=71 仍在持续生成。多道样本出现数万 token 的超长 thinking 输出，曾观察到 65,536 上限风险但未确认最终结果，也尚未开始 prefix cache 开启轮。
- [GPQA 第一轮最后样本未完成](blocks/08d8e4a6ddaf816ea14700d56c78bd5ae3cf969cb279b8f4e61f957650f9c06e.md): 关闭 prefix cache 的第一轮仍停留在197/198，最后样本 id=71 持续正常解码，没有报错、超时或服务异常。该样本从约1.2万 token 增长到约2.8万 token，约8.9 tok/s，尚未达到65,536 token上限，因此完整198题准确率和 finish_reason 仍未确定。后续切换 prefix cache、第二轮评测及两轮对比结果在这些材料中尚未出现。
- [GPQA prefix-off 完成与 PR 复现信息更新](blocks/55444900b15ebcf641c9aa8ba7be643f3462c93155ffc1654caf53a13dc24615.md): prefix-cache-off 的完整 GPQA-Diamond 198 题最终完成，结果为 181/198（91.41%），0 请求失败、0 未解析答案；全部样本 `finish_reason=stop`，id=71 最终预测 B、答案 A。prefix-cache-on 仅完成服务初始化至权重加载阶段，随后按用户要求停止，未开始评测；四台服务及残留 EngineCore/Worker 均已清理。GPQA 命令、服务拉起脚本和四机配置已更新到 PR #4 评论，评论发布成功。
- [服务复现手册与基础环境文档拆分](blocks/045d74269a12b406ecea9e981982951e362b8256e44566eb8d31db8ff1648fff.md): 复现流程记录了容器内 `/health`、`/v1/models`、Chat Completions curl、代理导致 502 时的 `--noproxy '*'` 处理，以及完整 198 题 GPQA runner；已知 prefix-cache-off 基准为 181/198、91.4141%、1,170,381 completion tokens、13,308 秒。服务停止可先 TERM，若遗留 `VLLM::EngineCore` 或 `VLLM::Worker` 则按进程标题清理；当前四台服务均已停止。当前目录已创建并精简 `AGENTS.md`，将拉起、监控、curl、GPQA 和停止步骤迁移至 `SERVICE_RUNBOOK.md`，并保留基础拓扑、路径、修复与验证结论。
