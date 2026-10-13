---
blocks:
- block_id: a11e94519e92e6937add4725c957b9d853ac977069d80c2577d2033ea6cd8313
  indexed: true
  sha256: 9e55216fa1ec8888edd2cb4fffdf673a59ca287a424d841a4cfb8930ae52acbf
  source_range:
    end: 1970260
    part: 0
    start: 0
  summary: 只读审计将 4 个失败定位为实际 runtime ACLGraph forward，而非 diagnostic dummy_run；输入、padding、qsl、slot/state indices 未发现非法，且 DP3 无 padding 仍出现 NaN，因此 padding 尚未被支持为必要根因。日志可确认“前一条 finite → 首条 NaN”的观测方向，但不能判断 recurrent cache 与 conv cache 谁先变为 NaN。当前优先假设是 `npu_causal_conv1d_custom` 未声明 `conv_state` mutation 的 ACLGraph 状态契约问题，仍需通过 FULL 前后选中 cache slot 的 finiteness/hash 观测验证，不能视为已证实成功。
  title: ACLGraph runtime 首个 NaN 与 cache 根因未定
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: d1e573822d2b0e234bdb5d6be1d304adbadf96bcae377a5acc3c01f5dedbc238
  kind: experience
  material_digest: 6946cc614976703ebc1fb33b6c1ea34b9fff603e8ad2873af6f97c483b4c903e
  revision: 1708ce6f626d7aff1f48ebab8374a8646f75bb5ba1954509a1b66c148f1466cc
  schema: mindie-entry/3
  summary: 当前索引聚焦 4 次 runtime ACLGraph forward 的首个 NaN、有限 replay 与 cache 诊断边界；padding、输入及索引未显示为非法。conv cache mutation 契约是优先待验证假设，recurrent/conv cache 的先后顺序仍未知，后续应保留失败与未完成观测。
  title: NaN 失败链路与 cache 状态契约
navigation: 当前索引聚焦 4 次 runtime ACLGraph forward 的首个 NaN、有限 replay 与 cache 诊断边界；padding、输入及索引未显示为非法。conv cache mutation 契约是优先待验证假设，recurrent/conv cache 的先后顺序仍未知，后续应保留失败与未完成观测。
schema: mindie-material-task/1
status: complete
task_id: d1e573822d2b0e234bdb5d6be1d304adbadf96bcae377a5acc3c01f5dedbc238
---

# NaN 失败链路与 cache 状态契约

当前索引聚焦 4 次 runtime ACLGraph forward 的首个 NaN、有限 replay 与 cache 诊断边界；padding、输入及索引未显示为非法。conv cache mutation 契约是优先待验证假设，recurrent/conv cache 的先后顺序仍未知，后续应保留失败与未完成观测。

## Materials

- [ACLGraph runtime 首个 NaN 与 cache 根因未定](blocks/a11e94519e92e6937add4725c957b9d853ac977069d80c2577d2033ea6cd8313.md): 只读审计将 4 个失败定位为实际 runtime ACLGraph forward，而非 diagnostic dummy_run；输入、padding、qsl、slot/state indices 未发现非法，且 DP3 无 padding 仍出现 NaN，因此 padding 尚未被支持为必要根因。日志可确认“前一条 finite → 首条 NaN”的观测方向，但不能判断 recurrent cache 与 conv cache 谁先变为 NaN。当前优先假设是 `npu_causal_conv1d_custom` 未声明 `conv_state` mutation 的 ACLGraph 状态契约问题，仍需通过 FULL 前后选中 cache slot 的 finiteness/hash 观测验证，不能视为已证实成功。
