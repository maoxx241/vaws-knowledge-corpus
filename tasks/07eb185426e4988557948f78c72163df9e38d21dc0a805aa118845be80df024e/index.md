---
blocks:
- block_id: 859fc45d544f6c76c1677a17f691c10b53bc948bc3eff8dd6fdc725f3d36626e
  indexed: true
  sha256: 2d22bd17096b1b6499b6aba55d0934836f423813e0e51f0f28ba5cabebe22e42
  source_range:
    end: 909843
    part: 0
    start: 0
  summary: 静态审计认为提交 `df5be8443` 暂不建议发布：Ruff、格式、codespell、typos、markdownlint、shellcheck 和 Python 静态 hooks 据报告通过，但 `git diff --check` 在 4 个 CRLF 文件中发现 310 处新增行尾空白；Gitleaks 因缺少 gitleaks 和 fallback 所需的 wget 未成功执行，不能视为通过。后续只读审计据报告确认 FC1、共享专家 DP、routed-token prepare/finalize 没有 Kimi/K3 分支，但指出全局 renderer monkeypatch 的 `model_type == kimi_k3` 门、A2 的 `num_experts > 512` 特判、公共路径的 `linear_attn_config` schema 推断等耦合风险；Step3 `g_proj`、ModelSlim full-rank gate、SiTU/MLA/KDA 相关判断据称已改为 capability-based。相关工作树据报告未被修改，结论仍包含未执行检查、阻塞项和待统一泛化的问题。
  title: 发布门禁阻塞与 Kimi/K3 耦合审计
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 07eb185426e4988557948f78c72163df9e38d21dc0a805aa118845be80df024e
  kind: experience
  material_digest: 58649595deda3a39a0c896876b47214df7da6e8e234234222791a84713bf6536
  revision: faecd837d00bc0167fa0425b8f7c2375cf2374c84fd6bbad9cd0aaaf1b7e1fa4
  schema: mindie-entry/3
  summary: 已索引的材料涉及两条相关线索：提交发布门禁存在行尾空白阻塞，且 Gitleaks 检查因环境缺失依赖未完成；Kimi/K3 公共路径审计则发现 renderer monkeypatch、A2 专项阈值和 `linear_attn_config` schema 等剩余耦合风险。后续检索时应保留这些失败检查、条件性结论和未完成泛化，不将 hooks 通过或 capability-based 改造方向表述为整体发布成功。
  title: 当前任务导航：发布门禁与模型特判审计
navigation: 已索引的材料涉及两条相关线索：提交发布门禁存在行尾空白阻塞，且 Gitleaks 检查因环境缺失依赖未完成；Kimi/K3 公共路径审计则发现 renderer monkeypatch、A2 专项阈值和 `linear_attn_config` schema 等剩余耦合风险。后续检索时应保留这些失败检查、条件性结论和未完成泛化，不将 hooks 通过或 capability-based 改造方向表述为整体发布成功。
schema: mindie-material-task/1
status: complete
task_id: 07eb185426e4988557948f78c72163df9e38d21dc0a805aa118845be80df024e
---

# 当前任务导航：发布门禁与模型特判审计

已索引的材料涉及两条相关线索：提交发布门禁存在行尾空白阻塞，且 Gitleaks 检查因环境缺失依赖未完成；Kimi/K3 公共路径审计则发现 renderer monkeypatch、A2 专项阈值和 `linear_attn_config` schema 等剩余耦合风险。后续检索时应保留这些失败检查、条件性结论和未完成泛化，不将 hooks 通过或 capability-based 改造方向表述为整体发布成功。

## Materials

- [发布门禁阻塞与 Kimi/K3 耦合审计](blocks/859fc45d544f6c76c1677a17f691c10b53bc948bc3eff8dd6fdc725f3d36626e.md): 静态审计认为提交 `df5be8443` 暂不建议发布：Ruff、格式、codespell、typos、markdownlint、shellcheck 和 Python 静态 hooks 据报告通过，但 `git diff --check` 在 4 个 CRLF 文件中发现 310 处新增行尾空白；Gitleaks 因缺少 gitleaks 和 fallback 所需的 wget 未成功执行，不能视为通过。后续只读审计据报告确认 FC1、共享专家 DP、routed-token prepare/finalize 没有 Kimi/K3 分支，但指出全局 renderer monkeypatch 的 `model_type == kimi_k3` 门、A2 的 `num_experts > 512` 特判、公共路径的 `linear_attn_config` schema 推断等耦合风险；Step3 `g_proj`、ModelSlim full-rank gate、SiTU/MLA/KDA 相关判断据称已改为 capability-based。相关工作树据报告未被修改，结论仍包含未执行检查、阻塞项和待统一泛化的问题。
