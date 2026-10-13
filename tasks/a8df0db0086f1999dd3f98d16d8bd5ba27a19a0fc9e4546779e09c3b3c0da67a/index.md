---
blocks:
- block_id: 514e02712c5d2521a8db280f19f9bf4949d953c9a1b34a573b296b34e74893bf
  indexed: true
  sha256: 45dbba168f62e5991d414f0a0b13ceb0d5ee616788805f26571d3739f838acee
  source_range:
    end: 1237695
    part: 0
    start: 0
  summary: '只读分析记录了 vLLM Ascend 插件中 DSpark 相关提交归属、建议的临时 backport 顺序及涉及的配置、模型注册、KV cache 和 scheduler 文件；未修改工作区、启动服务或推送。记录建议 legacy DSpark 权重采用 7 个 speculative tokens、greedy、eager 优先验证及固定 proposal，但这些是临时方案而非已验证成功。重点不确定性是 prefix-cache 的 target hit/draft miss 场景：直接采用 PR #48459 可能造成未初始化 draft KV，因此提出分离 target/draft lookup、repair 区间、aux hidden 回填或安全回退，并列出 block 边界、chunked prefill、并发及 eager/图模式测试。'
  title: DSpark 临时回移与 prefix-cache 风险分析
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: a8df0db0086f1999dd3f98d16d8bd5ba27a19a0fc9e4546779e09c3b3c0da67a
  kind: experience
  material_digest: f0f08ae3913858bd80e98c04ee2381922fb8437d11578e268cdb70dd58708340
  revision: 7efa9970ed3459c955bf9956d5bc6b05c322561ab34da8bf14ddd5a76d956d9b
  schema: mindie-entry/3
  summary: '当前材料集中于 vLLM Ascend 插件的 DSpark 临时 backport：先核对提交归属，再处理配置、模型注册、spec decode、scheduler、worker 和 KV cache 链路。保留的关键风险是 target prefix 命中而 draft KV 未命中时可能读取未初始化数据；PR #48459 不应直接视为充分修复，aux hidden/draft KV 重建或安全回退仍待实现与验证。后续应按列出的 block 边界、gamma=7、cold/warm、chunked prefill、并发、四机 eager 及图模式用例验证，当前没有成功验证结论。'
  title: DSpark 回移、KV cache 修复与验证导航
navigation: '当前材料集中于 vLLM Ascend 插件的 DSpark 临时 backport：先核对提交归属，再处理配置、模型注册、spec decode、scheduler、worker 和 KV cache 链路。保留的关键风险是 target prefix 命中而 draft KV 未命中时可能读取未初始化数据；PR #48459 不应直接视为充分修复，aux hidden/draft KV 重建或安全回退仍待实现与验证。后续应按列出的 block 边界、gamma=7、cold/warm、chunked prefill、并发、四机 eager 及图模式用例验证，当前没有成功验证结论。'
schema: mindie-material-task/1
status: complete
task_id: a8df0db0086f1999dd3f98d16d8bd5ba27a19a0fc9e4546779e09c3b3c0da67a
---

# DSpark 回移、KV cache 修复与验证导航

当前材料集中于 vLLM Ascend 插件的 DSpark 临时 backport：先核对提交归属，再处理配置、模型注册、spec decode、scheduler、worker 和 KV cache 链路。保留的关键风险是 target prefix 命中而 draft KV 未命中时可能读取未初始化数据；PR #48459 不应直接视为充分修复，aux hidden/draft KV 重建或安全回退仍待实现与验证。后续应按列出的 block 边界、gamma=7、cold/warm、chunked prefill、并发、四机 eager 及图模式用例验证，当前没有成功验证结论。

## Materials

- [DSpark 临时回移与 prefix-cache 风险分析](blocks/514e02712c5d2521a8db280f19f9bf4949d953c9a1b34a573b296b34e74893bf.md): 只读分析记录了 vLLM Ascend 插件中 DSpark 相关提交归属、建议的临时 backport 顺序及涉及的配置、模型注册、KV cache 和 scheduler 文件；未修改工作区、启动服务或推送。记录建议 legacy DSpark 权重采用 7 个 speculative tokens、greedy、eager 优先验证及固定 proposal，但这些是临时方案而非已验证成功。重点不确定性是 prefix-cache 的 target hit/draft miss 场景：直接采用 PR #48459 可能造成未初始化 draft KV，因此提出分离 target/draft lookup、repair 区间、aux hidden 回填或安全回退，并列出 block 边界、chunked prefill、并发及 eager/图模式测试。
