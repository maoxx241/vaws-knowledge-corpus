---
blocks:
- block_id: 6523770c91b9f4d48a42271fa684cf20e3cfb52f08d621cef87ca1e0552467ad
  indexed: true
  sha256: be26f24afa0b1c3346f9032aff41b2e37ff490012cc34331e49d5ca1d6daeb64
  source_range:
    end: 1549660
    part: 0
    start: 0
  summary: 静态审计未修改文件，也未发现确定性的 P0 语义错误；但结论不是已验证成功。物化 residual aux stream、KDA speculative state copy 和 prefix cache 分组分别存在接收率敏感点、运行边界未证明及 prefix-hit 未隔离等风险，需通过日志、边界用例和 target-only/SGLang 对比验证。记录的关键环境与方法约束包括 method=dspark、num_speculative_tokens=7、greedy、verifier width=8、enable_reduce_sample=false、lmhead_tensor_parallel_size=0，以及 draft eager/target FULL_DECODE_ONLY；prefix-off 被建议作为首轮条件，prefix-on 仍是后续未完成验收。
  title: DSpark/GQA-MRV1 静态审计索引
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 05e1fc1fed517948a53b9f32c73541e02a370d81114dfe259e2748a6bece5393
  kind: experience
  material_digest: 63998f6e4364a080e53d35b0ab8c4462777b61bc9a644213d3fc6f466203ccb3
  revision: ffc2681d067312807fc567941772f94c24822b8ab96edfdcf933a8d43e3e0547
  schema: mindie-entry/3
  summary: 当前材料记录了一次未改文件的静态审计：未确认 P0 错误，但也未将静态正确性提升为运行成功。后续检索应优先关注 materialized GQA 的 5 路 aux 与 35840 拼接维度、KDA accept=1/2/7 及跨 state block 边界、prefix cache 命中隔离，以及 SpecDecoding metrics 的接收率与 greedy 逐 token 一致性；prefix-off 首轮和 prefix-on 后续验收均属于待验证事项。原始材料独立可检索，本导航不声称包含被省略的更早细节。
  title: DSpark 推测解码静态审计与验证索引
navigation: 当前材料记录了一次未改文件的静态审计：未确认 P0 错误，但也未将静态正确性提升为运行成功。后续检索应优先关注 materialized GQA 的 5 路 aux 与 35840 拼接维度、KDA accept=1/2/7 及跨 state block 边界、prefix cache 命中隔离，以及 SpecDecoding metrics 的接收率与 greedy 逐 token 一致性；prefix-off 首轮和 prefix-on 后续验收均属于待验证事项。原始材料独立可检索，本导航不声称包含被省略的更早细节。
schema: mindie-material-task/1
status: complete
task_id: 05e1fc1fed517948a53b9f32c73541e02a370d81114dfe259e2748a6bece5393
---

# DSpark 推测解码静态审计与验证索引

当前材料记录了一次未改文件的静态审计：未确认 P0 错误，但也未将静态正确性提升为运行成功。后续检索应优先关注 materialized GQA 的 5 路 aux 与 35840 拼接维度、KDA accept=1/2/7 及跨 state block 边界、prefix cache 命中隔离，以及 SpecDecoding metrics 的接收率与 greedy 逐 token 一致性；prefix-off 首轮和 prefix-on 后续验收均属于待验证事项。原始材料独立可检索，本导航不声称包含被省略的更早细节。

## Materials

- [DSpark/GQA-MRV1 静态审计索引](blocks/6523770c91b9f4d48a42271fa684cf20e3cfb52f08d621cef87ca1e0552467ad.md): 静态审计未修改文件，也未发现确定性的 P0 语义错误；但结论不是已验证成功。物化 residual aux stream、KDA speculative state copy 和 prefix cache 分组分别存在接收率敏感点、运行边界未证明及 prefix-hit 未隔离等风险，需通过日志、边界用例和 target-only/SGLang 对比验证。记录的关键环境与方法约束包括 method=dspark、num_speculative_tokens=7、greedy、verifier width=8、enable_reduce_sample=false、lmhead_tensor_parallel_size=0，以及 draft eager/target FULL_DECODE_ONLY；prefix-off 被建议作为首轮条件，prefix-on 仍是后续未完成验收。
