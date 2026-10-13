---
blocks:
- block_id: 85fa39a1a6a440a55efd8e186d9f0e44510fe8f61c1387d491c2759f6ccdb8ea
  indexed: true
  sha256: 73000e89a60f35c586ae3252c1d39867c104312ec58042b5efc9a45be0058888
  source_range:
    end: 499430
    part: 0
    start: 0
  summary: '只读诊断记录显示，r2 在 draft model 加载阶段沿 `worker.load_model → model_runner_v1.py:3724 → drafter.load_model → llm_base_proposer.py:361` 失败，最终为 `AttributeError: ''KimiK3Config'' object has no attribute ''image_token_index''`；配置和目标权重实际提供的是 `media_placeholder_token_id=163605`。分析认为多模态 proposer 分支因实际模型类名未被 KimiK2.5 特判而落入通用字段路径，`Qwen3DSparkModel` 本身未读取该字段；提出在 plugin 分支中映射该字段的最小修复，但测试、四机重启、health=200 及质量/缓存/精度验证均只是后续建议，未记录为已验证成功。'
  title: 153 异常：draft model 缺少 image_token_index
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 068ac71b456438ae38deaed96426d8f71fa093fbfb1cfe311da9733cf8434ebe
  kind: experience
  material_digest: 9481d8deca9099e6c7e52ac59243e7dd7ba4e04c726fbb746f6aec38f627f3ed
  revision: e5aaa1fe1882a37f2213589f8b8543143d1a7140fc9704be3f81e3a3d2b8b587
  schema: mindie-entry/3
  summary: 当前可检索重点是 `KimiK3Config` 缺少 `image_token_index`、`media_placeholder_token_id=163605`、多模态 proposer 分支、KimiK2.5 特判和 plugin 最小映射修复。修复方向被记录为正确，但完整单测、四机启动验证、health=200 以及后续质量门、prefix cache、精度验证仍未完成或未证实。
  title: 当前任务导航：153 draft model 启动异常
navigation: 当前可检索重点是 `KimiK3Config` 缺少 `image_token_index`、`media_placeholder_token_id=163605`、多模态 proposer 分支、KimiK2.5 特判和 plugin 最小映射修复。修复方向被记录为正确，但完整单测、四机启动验证、health=200 以及后续质量门、prefix cache、精度验证仍未完成或未证实。
schema: mindie-material-task/1
status: complete
task_id: 068ac71b456438ae38deaed96426d8f71fa093fbfb1cfe311da9733cf8434ebe
---

# 当前任务导航：153 draft model 启动异常

当前可检索重点是 `KimiK3Config` 缺少 `image_token_index`、`media_placeholder_token_id=163605`、多模态 proposer 分支、KimiK2.5 特判和 plugin 最小映射修复。修复方向被记录为正确，但完整单测、四机启动验证、health=200 以及后续质量门、prefix cache、精度验证仍未完成或未证实。

## Materials

- [153 异常：draft model 缺少 image_token_index](blocks/85fa39a1a6a440a55efd8e186d9f0e44510fe8f61c1387d491c2759f6ccdb8ea.md): 只读诊断记录显示，r2 在 draft model 加载阶段沿 `worker.load_model → model_runner_v1.py:3724 → drafter.load_model → llm_base_proposer.py:361` 失败，最终为 `AttributeError: 'KimiK3Config' object has no attribute 'image_token_index'`；配置和目标权重实际提供的是 `media_placeholder_token_id=163605`。分析认为多模态 proposer 分支因实际模型类名未被 KimiK2.5 特判而落入通用字段路径，`Qwen3DSparkModel` 本身未读取该字段；提出在 plugin 分支中映射该字段的最小修复，但测试、四机重启、health=200 及质量/缓存/精度验证均只是后续建议，未记录为已验证成功。
