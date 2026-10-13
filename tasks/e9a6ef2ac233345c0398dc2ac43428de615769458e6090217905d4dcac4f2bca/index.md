---
blocks:
- block_id: 21e2ee38e27a359ceff3534b640b01f58c37dd9080fcdb47739a53204cd6406b
  indexed: true
  sha256: f6667b9744e08c5cbc8e19a4b9d6ecf280f6bb68214777784bbf32fb04495f95
  source_range:
    end: 986316
    part: 0
    start: 0
  summary: 现场证据显示首个四-rank 短请求门禁统一返回 HTTP 500，但原因被判断为 XTML parser 在 post_response 阶段拒绝模型生成的 legacy 空工具尾段，并非 Transformers 版本或 tokenizer 解码初始化错误；真实 tokenizer 曾正确产生 response end、empty call end、tools end、message end 序列。隔离测试报告两份 parser 测试共 95 项通过，不过真实 tokenizer 测试一度受旧 SHA 基线（服务 encoding_k3.py 为 c386…、测试锁定 b9cb…）阻挡；随后相关 bespoke parser 兼容和测试改动被精确撤销，未部署到四机服务。后续 upstream 对齐曾定位到 vLLM main 及 Rust/Python Kimi K3 frontend 提交，但按最新优先级已暂停，当前仅保留既有 PR12950 DSpark 适配。
  title: XTML legacy 空工具尾段与临时兼容撤销
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: e9a6ef2ac233345c0398dc2ac43428de615769458e6090217905d4dcac4f2bca
  kind: experience
  material_digest: 7107a17e0794ae3e2920ab276c6cef565cc1a77e57566da8919ca2349e150034
  revision: 02379d02f7f88c8cfc7c4efc381a7d89ea892ecc67dd346baf6c6a7cdb67c696
  schema: mindie-entry/3
  summary: 首个四-rank 请求的 HTTP 500 不能归因于 Transformers/tokenizer 初始化；现有证据指向 XTML 对 legacy 空工具尾段的拒绝，但临时兼容已撤销，相关真实 tokenizer 测试还曾存在 SHA 基线不一致。Parser upstream 对齐暂停，当前工作范围保留既有 PR12950 DSpark 适配；服务未部署或修改，后续仍需区分已验证测试结果、失败的测试基线与未完成的 upstream 对齐。
  title: 当前任务导航：DSpark 优先，Parser 结论仅作证据
navigation: 首个四-rank 请求的 HTTP 500 不能归因于 Transformers/tokenizer 初始化；现有证据指向 XTML 对 legacy 空工具尾段的拒绝，但临时兼容已撤销，相关真实 tokenizer 测试还曾存在 SHA 基线不一致。Parser upstream 对齐暂停，当前工作范围保留既有 PR12950 DSpark 适配；服务未部署或修改，后续仍需区分已验证测试结果、失败的测试基线与未完成的 upstream 对齐。
schema: mindie-material-task/1
status: complete
task_id: e9a6ef2ac233345c0398dc2ac43428de615769458e6090217905d4dcac4f2bca
---

# 当前任务导航：DSpark 优先，Parser 结论仅作证据

首个四-rank 请求的 HTTP 500 不能归因于 Transformers/tokenizer 初始化；现有证据指向 XTML 对 legacy 空工具尾段的拒绝，但临时兼容已撤销，相关真实 tokenizer 测试还曾存在 SHA 基线不一致。Parser upstream 对齐暂停，当前工作范围保留既有 PR12950 DSpark 适配；服务未部署或修改，后续仍需区分已验证测试结果、失败的测试基线与未完成的 upstream 对齐。

## Materials

- [XTML legacy 空工具尾段与临时兼容撤销](blocks/21e2ee38e27a359ceff3534b640b01f58c37dd9080fcdb47739a53204cd6406b.md): 现场证据显示首个四-rank 短请求门禁统一返回 HTTP 500，但原因被判断为 XTML parser 在 post_response 阶段拒绝模型生成的 legacy 空工具尾段，并非 Transformers 版本或 tokenizer 解码初始化错误；真实 tokenizer 曾正确产生 response end、empty call end、tools end、message end 序列。隔离测试报告两份 parser 测试共 95 项通过，不过真实 tokenizer 测试一度受旧 SHA 基线（服务 encoding_k3.py 为 c386…、测试锁定 b9cb…）阻挡；随后相关 bespoke parser 兼容和测试改动被精确撤销，未部署到四机服务。后续 upstream 对齐曾定位到 vLLM main 及 Rust/Python Kimi K3 frontend 提交，但按最新优先级已暂停，当前仅保留既有 PR12950 DSpark 适配。
