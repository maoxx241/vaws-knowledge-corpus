---
blocks:
- block_id: 4b27ab402959e434e937816878729f498c31da09aaf2197798e572a11da5ba15
  indexed: true
  sha256: ba18b3b6966053b759541a4c1cd1e2430d83a690b56f62104db122f3cd7c6691
  source_range:
    end: 4876357
    part: 0
    start: 0
  summary: 该复审覆盖注册/组网、MLA no-RoPE、KDA 状态与缓存、原生 MXFP4 专家参数映射、MoE/SiTU 以及多模态接口；报告称未发现新的 P0/P1/P2，但同时记录了多模态一致性风险和 MXFP4+SiTU 分解 GMM 数据类型解释仍需核对的不确定性。No-RoPE 路径据报告仅生成 identity 占位张量，Q/K 的 64 维切片保持原值并参与注意力；MXFP4 参数名映射被观察为可通，但未等同于原生权重 forward 已验证成功。最终建议补充不同尺寸图片的 item_idx/顺序、processor cache 开关下 token IDs、speculative config 下 state shape 以及非 64×64 网格 bicubic 对齐测试；本机缺少 pytest，未执行本地测试，因此结论仍属静态复审和建议，不能视为验证通过。
  title: K3 增量只读复审与未验证测试建议
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 080fbbdc13bd87e330393a80e87a7ed6efc5f19a664dad6fdf7afc0969baebb1
  kind: experience
  material_digest: 03ee377c917f4c566be437fbbe5351e510254e44cb81731bbd71b563a3381510
  revision: a1166b390a9e85960518961c3a4e81aafaa0b3fa880306f9e7ef93c79c73c722
  schema: mindie-entry/3
  summary: 当前材料记录了一轮只读静态复审：主干文本链路暂未发现新的高优先级问题，但多模态一致性、MXFP4+SiTU 权重解释及缓存/状态契约仍有未完成核验。后续应优先执行所列图片顺序、token 一致性、state shape 和 bicubic 数值测试；本机 pytest 缺失，测试状态未完成，原始材料仍需独立检索。
  title: K3 增量复审导航
navigation: 当前材料记录了一轮只读静态复审：主干文本链路暂未发现新的高优先级问题，但多模态一致性、MXFP4+SiTU 权重解释及缓存/状态契约仍有未完成核验。后续应优先执行所列图片顺序、token 一致性、state shape 和 bicubic 数值测试；本机 pytest 缺失，测试状态未完成，原始材料仍需独立检索。
schema: mindie-material-task/1
status: complete
task_id: 080fbbdc13bd87e330393a80e87a7ed6efc5f19a664dad6fdf7afc0969baebb1
---

# K3 增量复审导航

当前材料记录了一轮只读静态复审：主干文本链路暂未发现新的高优先级问题，但多模态一致性、MXFP4+SiTU 权重解释及缓存/状态契约仍有未完成核验。后续应优先执行所列图片顺序、token 一致性、state shape 和 bicubic 数值测试；本机 pytest 缺失，测试状态未完成，原始材料仍需独立检索。

## Materials

- [K3 增量只读复审与未验证测试建议](blocks/4b27ab402959e434e937816878729f498c31da09aaf2197798e572a11da5ba15.md): 该复审覆盖注册/组网、MLA no-RoPE、KDA 状态与缓存、原生 MXFP4 专家参数映射、MoE/SiTU 以及多模态接口；报告称未发现新的 P0/P1/P2，但同时记录了多模态一致性风险和 MXFP4+SiTU 分解 GMM 数据类型解释仍需核对的不确定性。No-RoPE 路径据报告仅生成 identity 占位张量，Q/K 的 64 维切片保持原值并参与注意力；MXFP4 参数名映射被观察为可通，但未等同于原生权重 forward 已验证成功。最终建议补充不同尺寸图片的 item_idx/顺序、processor cache 开关下 token IDs、speculative config 下 state shape 以及非 64×64 网格 bicubic 对齐测试；本机缺少 pytest，未执行本地测试，因此结论仍属静态复审和建议，不能视为验证通过。
