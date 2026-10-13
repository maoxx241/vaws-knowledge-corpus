---
blocks:
- block_id: dfd97fc33add4363b7d0447b08a6d1cd04f18f5a63b69dc4c370fdc7344b406a
  indexed: true
  sha256: 610d981bc680af465c3ed4fab461543b877e9d041a76e3c5b5c1b6e6912b274e
  source_range:
    end: 546522
    part: 0
    start: 0
  summary: 只读审计认为，DSpark 的 AscendSpecDecodeBaseProposer.load_model() 因 K3 target 继承 SupportsMultiModal 而进入通用多模态分支；由于现有逻辑仅特判 KimiK25ForConditionalGeneration，K3 兜底读取不存在的 config.image_token_index，可能在共享 embedding 前触发错误。材料指出 K3 应使用 media_placeholder_token_id=163605，并提出将实际类名并入同一桥接分支；共享 worktree 中已有未提交修复，但未进行编辑、提交或远端操作，且不能据此确认 DSpark 已支持 K3 多模态请求。
  title: K3 多模态字段桥接审计
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 512bc3cf733350b7aeb9270113a26f7c01418d9dafb65bb802875555bb220846
  kind: experience
  material_digest: d003f8751a719dd09d6329f78350b88ec3fb2a0f6e2bc5c6f039b3ba46177444
  revision: 2bed2d3edbca683f3972994fef9101b15b48759d9ba5a5e8dd3f5ddaf0fafd8e
  schema: mindie-entry/3
  summary: 当前材料记录了一项只读审计：K3 因 image_token_index 与 media_placeholder_token_id 字段不一致，可能在 embedding 共享前失败；建议的最窄修复是复用动态 media token 字段并扩展现有 KimiK25 特判。修复未由本批材料验证成功，现有 worktree 状态为未提交，DSpark 对 K3 多模态请求的完整支持仍未确认。
  title: K3 DSpark 多模态字段问题
navigation: 当前材料记录了一项只读审计：K3 因 image_token_index 与 media_placeholder_token_id 字段不一致，可能在 embedding 共享前失败；建议的最窄修复是复用动态 media token 字段并扩展现有 KimiK25 特判。修复未由本批材料验证成功，现有 worktree 状态为未提交，DSpark 对 K3 多模态请求的完整支持仍未确认。
schema: mindie-material-task/1
status: complete
task_id: 512bc3cf733350b7aeb9270113a26f7c01418d9dafb65bb802875555bb220846
---

# K3 DSpark 多模态字段问题

当前材料记录了一项只读审计：K3 因 image_token_index 与 media_placeholder_token_id 字段不一致，可能在 embedding 共享前失败；建议的最窄修复是复用动态 media token 字段并扩展现有 KimiK25 特判。修复未由本批材料验证成功，现有 worktree 状态为未提交，DSpark 对 K3 多模态请求的完整支持仍未确认。

## Materials

- [K3 多模态字段桥接审计](blocks/dfd97fc33add4363b7d0447b08a6d1cd04f18f5a63b69dc4c370fdc7344b406a.md): 只读审计认为，DSpark 的 AscendSpecDecodeBaseProposer.load_model() 因 K3 target 继承 SupportsMultiModal 而进入通用多模态分支；由于现有逻辑仅特判 KimiK25ForConditionalGeneration，K3 兜底读取不存在的 config.image_token_index，可能在共享 embedding 前触发错误。材料指出 K3 应使用 media_placeholder_token_id=163605，并提出将实际类名并入同一桥接分支；共享 worktree 中已有未提交修复，但未进行编辑、提交或远端操作，且不能据此确认 DSpark 已支持 K3 多模态请求。
