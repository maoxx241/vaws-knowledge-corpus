---
blocks:
- block_id: 11360d31ab814fcc56cb776804aabc74a16ad96fa1f277327213e9a521ef183f
  indexed: true
  sha256: dd4a4ce1600fb59985a4c1cf1a1bd6ba7ebbe364fa2d5a3996e9f0bde5cc50e7
  source_range:
    end: 485968
    part: 0
    start: 0
  summary: 该 session（019f8239-9f13-72a1-a447-3a795876de2a）被报告为以 W4A8_DYNAMIC 为主体的混合量化权重，另含 W8A8_DYNAMIC 和 FLOAT；启动参数中的 `--dtype bfloat16` 不应单独用于判断 W4A16。测试工具方面，记录称曾参考并尝试安装 AISBench，但 GPQA-Diamond 的实际跑测使用了自定义脚本，直接并发请求 `/v1/chat/completions`，报告结果为 181/198（91.41%）；服务最后已停止。上述内容是可检索的报告性参考，不能视为已独立验证的案例结论。
  title: Session 权重量化与测试工具核对
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 3baadc586599612c67b28fe7732704685915e721c803b4700b6901bf05bfbeb6
  kind: experience
  material_digest: f3698abb04c8c5361fcb173f3d373babeb87bf121553987b7886f16deb33a3d4
  revision: 0737c321d317fa0ef9cb61ef5ab594ca1fbeaad733a4e27c99fd578617e517b3
  schema: mindie-entry/3
  summary: 本批新增材料集中在同一 session：权重量化被报告为 W4A8_DYNAMIC 主体的混合配置，测试实际使用自定义脚本而非 AISBench。需保留 AISBench 的参考/安装尝试与最终工具之间的区别，以及 181/198（91.41%）这一未独立验证的报告结果；原始材料仍应按 block_id 独立检索。
  title: 当前任务导航：Session 配置与跑测工具
navigation: 本批新增材料集中在同一 session：权重量化被报告为 W4A8_DYNAMIC 主体的混合配置，测试实际使用自定义脚本而非 AISBench。需保留 AISBench 的参考/安装尝试与最终工具之间的区别，以及 181/198（91.41%）这一未独立验证的报告结果；原始材料仍应按 block_id 独立检索。
schema: mindie-material-task/1
status: complete
task_id: 3baadc586599612c67b28fe7732704685915e721c803b4700b6901bf05bfbeb6
---

# 当前任务导航：Session 配置与跑测工具

本批新增材料集中在同一 session：权重量化被报告为 W4A8_DYNAMIC 主体的混合配置，测试实际使用自定义脚本而非 AISBench。需保留 AISBench 的参考/安装尝试与最终工具之间的区别，以及 181/198（91.41%）这一未独立验证的报告结果；原始材料仍应按 block_id 独立检索。

## Materials

- [Session 权重量化与测试工具核对](blocks/11360d31ab814fcc56cb776804aabc74a16ad96fa1f277327213e9a521ef183f.md): 该 session（019f8239-9f13-72a1-a447-3a795876de2a）被报告为以 W4A8_DYNAMIC 为主体的混合量化权重，另含 W8A8_DYNAMIC 和 FLOAT；启动参数中的 `--dtype bfloat16` 不应单独用于判断 W4A16。测试工具方面，记录称曾参考并尝试安装 AISBench，但 GPQA-Diamond 的实际跑测使用了自定义脚本，直接并发请求 `/v1/chat/completions`，报告结果为 181/198（91.41%）；服务最后已停止。上述内容是可检索的报告性参考，不能视为已独立验证的案例结论。
