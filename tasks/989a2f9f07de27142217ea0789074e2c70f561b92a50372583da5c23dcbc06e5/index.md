---
blocks:
- block_id: 03aad4cd4a07f7629f5a13159e92b02da263a88fa12b258f237965a16e22b003
  indexed: true
  sha256: f3e87f7a6d6478c1a91e6f28cc7ac43fa55033f997a3feec721526fe90b12d94
  source_range:
    end: 557930
    part: 0
    start: 0
  summary: 只读审计认为当前 K3 UT 偏重私有实现、异常分支和兼容性防御，建议删除或替换约 15 个低价值测试，并修正仍期待 W8A8MXFP shared-expert 路径的 stale UT。建议合并 speculative cache-shape、loader 和设备迁移测试，同时保留组网通信、checkpoint/load ABI、数学与执行边界、多模态公共行为等核心覆盖；其中 nightly NPU operator 测试应按算子覆盖矩阵另行审查。
  title: K3 UT 审计与测试精简建议
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 989a2f9f07de27142217ea0789074e2c70f561b92a50372583da5c23dcbc06e5
  kind: experience
  material_digest: 3c87f324c1f8b3850c95aa78864bdca41097e0e573b77bc103255a59c735abb5
  revision: 0360a73780d58a350c603a83d59b0cd00663ccc4ba7a2eec5813fc9990a46702
  schema: mindie-entry/3
  summary: 本批材料记录了一份只读审计和测试精简建议，涵盖应删除、替换、合并及必须保留的 K3 UT，并指出 W8A8MXFP stale expectation 与 nightly NPU operator 测试的处理边界。该内容是待评估的 fallible reference index，不等同于已验证的代码修改或测试成功。
  title: 当前任务导航：K3 UT 测试审计
navigation: 本批材料记录了一份只读审计和测试精简建议，涵盖应删除、替换、合并及必须保留的 K3 UT，并指出 W8A8MXFP stale expectation 与 nightly NPU operator 测试的处理边界。该内容是待评估的 fallible reference index，不等同于已验证的代码修改或测试成功。
schema: mindie-material-task/1
status: complete
task_id: 989a2f9f07de27142217ea0789074e2c70f561b92a50372583da5c23dcbc06e5
---

# 当前任务导航：K3 UT 测试审计

本批材料记录了一份只读审计和测试精简建议，涵盖应删除、替换、合并及必须保留的 K3 UT，并指出 W8A8MXFP stale expectation 与 nightly NPU operator 测试的处理边界。该内容是待评估的 fallible reference index，不等同于已验证的代码修改或测试成功。

## Materials

- [K3 UT 审计与测试精简建议](blocks/03aad4cd4a07f7629f5a13159e92b02da263a88fa12b258f237965a16e22b003.md): 只读审计认为当前 K3 UT 偏重私有实现、异常分支和兼容性防御，建议删除或替换约 15 个低价值测试，并修正仍期待 W8A8MXFP shared-expert 路径的 stale UT。建议合并 speculative cache-shape、loader 和设备迁移测试，同时保留组网通信、checkpoint/load ABI、数学与执行边界、多模态公共行为等核心覆盖；其中 nightly NPU operator 测试应按算子覆盖矩阵另行审查。
