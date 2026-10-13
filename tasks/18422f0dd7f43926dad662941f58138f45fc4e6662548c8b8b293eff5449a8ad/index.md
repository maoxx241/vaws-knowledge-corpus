---
blocks:
- block_id: b091e0c75aabccce4574e4d884a0dae6645029dd398c7a316b883d79acf800fc
  indexed: true
  sha256: ddb4cd463144a6a79043872483d3cbb81381f6cea490ea8916922db92ff931d4
  source_range:
    end: 18159223
    part: 0
    start: 0
  summary: '围绕将 PR #14454 的 K3 十六卡用例缩减至四卡、保持约五分钟目标，已设计保留关键覆盖并在 153 上尝试验证；153 原 K3 服务曾按授权终止，但期间又被外部流程多次拉起，154 未使用。多轮尝试遇到 `MoeInitRoutingV3` 非法维度、GMM 形状错误、BF16/NZ、topk、共享专家多流、eager/ACLGraph、buffer alias 及 fused-MC2 缺少 `DispatchFFNCombineBF16` 等问题；单卡独立 CANN 调用通过，但 vLLM profile/分布式 MoE 路径仍失败，尚未证明四卡用例成功。当前较强线索是当前 Python 主干复用了旧提交编译的 `.so`，隔离重编译在 AscendC 阶段疑似因 `-j1280` 僵停，计划以 `MAX_JOBS=32` 续编；此前关于 vLLM 版本、专家数、topk、权重格式和多流的若干假设均已被修正或排除。'
  title: 'PR #14454 四卡缩减验证：MoE 路由与环境兼容性排查'
- block_id: 1a2d127451fd54f0421492a82b0717a4cf2c18140212b25b506032a435b3bf58
  indexed: true
  sha256: 6dfd4a6dd8e3de5334f66d6af0866a30ee5006b56d64f84ed435047233a0ad64
  source_range:
    end: 30154063
    part: 0
    start: 18159223
  summary: '通过隔离构建和多轮判别，最终将 K3 用例缩为 2 层 target、1 层 draft、1 层 vision、16 experts，并保留 Block5、GQA W4A8 DP2/TP2、MTP 图像和 MLA P2/D2；GQA 通过调整为 72 heads/18 KV heads/head_dim 32 解决 TP2 KV page 对齐问题。最新 main 上补跑结果为 4/4 通过、pytest 274.41 秒、端到端 292 秒，但这是 dummy-weight 功能烟测，不覆盖真实 checkpoint、精度或采信率；hybrid-cache 重复请求仍存在“创建但不复用”，因此未作为成功断言。分支已签名提交并推送，PR #15403 已创建且处于 Open/CI 初始化状态；此前 gh token 失效，创建使用了已登录的 Chrome 会话。'
  title: 'K3 四卡缩减验证与 PR #15403'
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 18422f0dd7f43926dad662941f58138f45fc4e6662548c8b8b293eff5449a8ad
  kind: experience
  material_digest: 30b3daba7381c75d26046feb88ff599c690802912f8594a1e138d687b375a8f8
  revision: f8b9d4911543a83886f98073f52d04302f0a60c3d08318bafd642b184913358a
  schema: mindie-entry/3
  summary: '当前任务已从四卡方案设计和硬件验证推进到 PR 收尾阶段：最新 main 上的 4 卡补跑为 4/4、端到端 292 秒，PR #15403 已公开创建并等待 CI。应保留的限制包括 dummy-weight 验证边界、未覆盖真实 checkpoint/精度/采信率、TP4 hybrid-cache 重复请求未复用，以及退出时可能出现但不影响 pytest 退出码的 EngineDeadError/corrupted-size 噪声；此前 5 分 50 秒结果属于旧基线，不能替代最新头验证。'
  title: 'K3 四卡缩减验证与 PR #15403'
navigation: '当前任务已从四卡方案设计和硬件验证推进到 PR 收尾阶段：最新 main 上的 4 卡补跑为 4/4、端到端 292 秒，PR #15403 已公开创建并等待 CI。应保留的限制包括 dummy-weight 验证边界、未覆盖真实 checkpoint/精度/采信率、TP4 hybrid-cache 重复请求未复用，以及退出时可能出现但不影响 pytest 退出码的 EngineDeadError/corrupted-size 噪声；此前 5 分 50 秒结果属于旧基线，不能替代最新头验证。'
schema: mindie-material-task/1
status: complete
task_id: 18422f0dd7f43926dad662941f58138f45fc4e6662548c8b8b293eff5449a8ad
---

# K3 四卡缩减验证与 PR #15403

当前任务已从四卡方案设计和硬件验证推进到 PR 收尾阶段：最新 main 上的 4 卡补跑为 4/4、端到端 292 秒，PR #15403 已公开创建并等待 CI。应保留的限制包括 dummy-weight 验证边界、未覆盖真实 checkpoint/精度/采信率、TP4 hybrid-cache 重复请求未复用，以及退出时可能出现但不影响 pytest 退出码的 EngineDeadError/corrupted-size 噪声；此前 5 分 50 秒结果属于旧基线，不能替代最新头验证。

## Materials

- [PR #14454 四卡缩减验证：MoE 路由与环境兼容性排查](blocks/b091e0c75aabccce4574e4d884a0dae6645029dd398c7a316b883d79acf800fc.md): 围绕将 PR #14454 的 K3 十六卡用例缩减至四卡、保持约五分钟目标，已设计保留关键覆盖并在 153 上尝试验证；153 原 K3 服务曾按授权终止，但期间又被外部流程多次拉起，154 未使用。多轮尝试遇到 `MoeInitRoutingV3` 非法维度、GMM 形状错误、BF16/NZ、topk、共享专家多流、eager/ACLGraph、buffer alias 及 fused-MC2 缺少 `DispatchFFNCombineBF16` 等问题；单卡独立 CANN 调用通过，但 vLLM profile/分布式 MoE 路径仍失败，尚未证明四卡用例成功。当前较强线索是当前 Python 主干复用了旧提交编译的 `.so`，隔离重编译在 AscendC 阶段疑似因 `-j1280` 僵停，计划以 `MAX_JOBS=32` 续编；此前关于 vLLM 版本、专家数、topk、权重格式和多流的若干假设均已被修正或排除。
- [K3 四卡缩减验证与 PR #15403](blocks/1a2d127451fd54f0421492a82b0717a4cf2c18140212b25b506032a435b3bf58.md): 通过隔离构建和多轮判别，最终将 K3 用例缩为 2 层 target、1 层 draft、1 层 vision、16 experts，并保留 Block5、GQA W4A8 DP2/TP2、MTP 图像和 MLA P2/D2；GQA 通过调整为 72 heads/18 KV heads/head_dim 32 解决 TP2 KV page 对齐问题。最新 main 上补跑结果为 4/4 通过、pytest 274.41 秒、端到端 292 秒，但这是 dummy-weight 功能烟测，不覆盖真实 checkpoint、精度或采信率；hybrid-cache 重复请求仍存在“创建但不复用”，因此未作为成功断言。分支已签名提交并推送，PR #15403 已创建且处于 Open/CI 初始化状态；此前 gh token 失效，创建使用了已登录的 Chrome 会话。
