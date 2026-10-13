---
blocks:
- block_id: 7148a4e35347300f370d9f3f65922d0e772a66306c3e54e4fa0003a8b381d65d
  indexed: true
  sha256: 59430e258726fe62e657d507ada37537441eee671d72e4897c87d4a473f8f49e
  source_range:
    end: 806005
    part: 0
    start: 0
  summary: 材料围绕 DSV4 中 C128/HCA 是否包含 sliding-window attention 展开，先给出“C128 压缩历史 dense attention 与最近 128 个未压缩 token 的 SWA 并行”的结论，并引用技术报告及 vLLM-Ascend、Transformers 参考实现；其中本地路径已被 `<redacted:local-path:...>` 替换。用户随后指出 C4/C128 主要涉及每 4/128 个 token 的合并，材料据此区分压缩主分支与整层 attention，修正并总结为：C1 是纯 SWA，C4 是 C4 sparse + SWA，C128 是 C128 dense + SWA；这些结论属于材料中的报告/代码检查结果，保留其方法、实现路径和可能的分支语义不确定性，未独立验证。
  title: DSV4 C128/HCA 与滑动窗口注意力
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: a15c80208a9b43704a944b69a8c518645a2e4e42965218e0c397986cde2ec745
  kind: experience
  material_digest: c8e196250e25d420f85f73d97b5373e1e43c40d04e0968ebd12ded46199bfc55
  revision: c3c7bf178bfefd8194827c7a93b1cd0f236068b04f13add8af806e8d80b1c5c9
  schema: mindie-entry/3
  summary: 当前材料集中于 C1、C4、C128/HCA 的压缩方式与 SWA 分支关系：C1 被描述为纯 SWA，C4 为压缩稀疏注意力加 SWA，C128 为压缩 dense attention 加 SWA。后续检索应优先核对窗口大小、C4 重叠池化与 C128 非重叠合并的实现细节，以及不同 vLLM-Ascend/Transformers 路径是否一致；原始材料仍独立可检索，当前结论不应视为已验证案例报告。
  title: DSV4 注意力机制检索导航
navigation: 当前材料集中于 C1、C4、C128/HCA 的压缩方式与 SWA 分支关系：C1 被描述为纯 SWA，C4 为压缩稀疏注意力加 SWA，C128 为压缩 dense attention 加 SWA。后续检索应优先核对窗口大小、C4 重叠池化与 C128 非重叠合并的实现细节，以及不同 vLLM-Ascend/Transformers 路径是否一致；原始材料仍独立可检索，当前结论不应视为已验证案例报告。
schema: mindie-material-task/1
status: complete
task_id: a15c80208a9b43704a944b69a8c518645a2e4e42965218e0c397986cde2ec745
---

# DSV4 注意力机制检索导航

当前材料集中于 C1、C4、C128/HCA 的压缩方式与 SWA 分支关系：C1 被描述为纯 SWA，C4 为压缩稀疏注意力加 SWA，C128 为压缩 dense attention 加 SWA。后续检索应优先核对窗口大小、C4 重叠池化与 C128 非重叠合并的实现细节，以及不同 vLLM-Ascend/Transformers 路径是否一致；原始材料仍独立可检索，当前结论不应视为已验证案例报告。

## Materials

- [DSV4 C128/HCA 与滑动窗口注意力](blocks/7148a4e35347300f370d9f3f65922d0e772a66306c3e54e4fa0003a8b381d65d.md): 材料围绕 DSV4 中 C128/HCA 是否包含 sliding-window attention 展开，先给出“C128 压缩历史 dense attention 与最近 128 个未压缩 token 的 SWA 并行”的结论，并引用技术报告及 vLLM-Ascend、Transformers 参考实现；其中本地路径已被 `<redacted:local-path:...>` 替换。用户随后指出 C4/C128 主要涉及每 4/128 个 token 的合并，材料据此区分压缩主分支与整层 attention，修正并总结为：C1 是纯 SWA，C4 是 C4 sparse + SWA，C128 是 C128 dense + SWA；这些结论属于材料中的报告/代码检查结果，保留其方法、实现路径和可能的分支语义不确定性，未独立验证。
