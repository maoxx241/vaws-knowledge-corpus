---
blocks:
- block_id: 04ce6726a5f0ecee7952884f5cf6e1c3b05b791caf83434311bf9453660f79f7
  indexed: true
  sha256: 9b4ade471f9a395deac738408f9af877012220cf1d8214fe420a382f1f3b8ed7
  source_range:
    end: 1069774
    part: 0
    start: 0
  summary: 只读审计报告建议以 vLLM Python frontend 提交 `f5a7cce9b6a61f4d995629a7418c7ea822e34a64` 为 Kimi K3 parser、renderer、tokenizer 基线，并先按 `6370e53f → f5a7cce9` 回移；`git apply --check` 仅证明补丁可应用，不代表编译或测试成功。报告指出 Ascend 严格 XTML parser 因强制 close message 曾导致 HTTP 500，而上游 structural-tag grammar 对尾标记采用可选、fail-soft 处理；当前临时兼容修改已回退，但这些结论和后续清单仍是待实施、待真实 tokenizer token-ID parity 与测试验证的参考。P1 需谨慎保留旧 core/MM 的 developer、image 兼容，P2 Rust frontend 需先回移 `5ed3faa43 → 2899dca84`，并覆盖 streaming、tool choice、截断调用、Responses、图像及 PD prefill/decode 等场景。
  title: Kimi K3 上游前端对齐审计与后续门禁
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 060d95ff18e0606f9e48d2c9765045e0f5044dff5a8155885bc6572859315fe9
  kind: experience
  material_digest: d73a748e19c949068a9af2eb03cc1ae1084efa87186c493dd96d30187e01f924
  revision: 6566e44c356fa0e8c223f340556a17ff79641ba05f5817972af2de443115e644
  schema: mindie-entry/3
  summary: 当前材料是一次只读上游对齐审计：记录了 d02 core 缺少 K3 native frontend、上游 Python/Rust 提交顺序、曾出现 HTTP 500 的严格 XTML close-message 假设，以及补丁可应用但未证明编译/测试通过的状态。后续应在 DSpark 收敛后按 Python frontend 与 PD 依赖顺序实施，保留尚未完成的 MM/旧 core 兼容判断，并以真实 tokenizer parity、普通/流式无 500、工具调用模式、Responses、图像和 parser/real-tokenizer 测试作为门禁；Rust frontend 暂列 P2。
  title: Kimi K3 前端对齐与验证索引
navigation: 当前材料是一次只读上游对齐审计：记录了 d02 core 缺少 K3 native frontend、上游 Python/Rust 提交顺序、曾出现 HTTP 500 的严格 XTML close-message 假设，以及补丁可应用但未证明编译/测试通过的状态。后续应在 DSpark 收敛后按 Python frontend 与 PD 依赖顺序实施，保留尚未完成的 MM/旧 core 兼容判断，并以真实 tokenizer parity、普通/流式无 500、工具调用模式、Responses、图像和 parser/real-tokenizer 测试作为门禁；Rust frontend 暂列 P2。
schema: mindie-material-task/1
status: complete
task_id: 060d95ff18e0606f9e48d2c9765045e0f5044dff5a8155885bc6572859315fe9
---

# Kimi K3 前端对齐与验证索引

当前材料是一次只读上游对齐审计：记录了 d02 core 缺少 K3 native frontend、上游 Python/Rust 提交顺序、曾出现 HTTP 500 的严格 XTML close-message 假设，以及补丁可应用但未证明编译/测试通过的状态。后续应在 DSpark 收敛后按 Python frontend 与 PD 依赖顺序实施，保留尚未完成的 MM/旧 core 兼容判断，并以真实 tokenizer parity、普通/流式无 500、工具调用模式、Responses、图像和 parser/real-tokenizer 测试作为门禁；Rust frontend 暂列 P2。

## Materials

- [Kimi K3 上游前端对齐审计与后续门禁](blocks/04ce6726a5f0ecee7952884f5cf6e1c3b05b791caf83434311bf9453660f79f7.md): 只读审计报告建议以 vLLM Python frontend 提交 `f5a7cce9b6a61f4d995629a7418c7ea822e34a64` 为 Kimi K3 parser、renderer、tokenizer 基线，并先按 `6370e53f → f5a7cce9` 回移；`git apply --check` 仅证明补丁可应用，不代表编译或测试成功。报告指出 Ascend 严格 XTML parser 因强制 close message 曾导致 HTTP 500，而上游 structural-tag grammar 对尾标记采用可选、fail-soft 处理；当前临时兼容修改已回退，但这些结论和后续清单仍是待实施、待真实 tokenizer token-ID parity 与测试验证的参考。P1 需谨慎保留旧 core/MM 的 developer、image 兼容，P2 Rust frontend 需先回移 `5ed3faa43 → 2899dca84`，并覆盖 streaming、tool choice、截断调用、Responses、图像及 PD prefill/decode 等场景。
