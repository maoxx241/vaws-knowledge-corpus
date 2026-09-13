# Historical, unverified speculative-method default failure note

Source: [workspace profiling collection CLI help at eb30d0690edef67e26505a5332cc569de1ed6c59](https://github.com/vllm-ascend-workspace/vllm-ascend-workspace/blob/eb30d0690edef67e26505a5332cc569de1ed6c59/.agents/skills/ascend-profiling-collection/scripts/collect_torch_profile_case.py#L523-L529). The original help text recorded the following account:

> Default 'mtp' is vLLM's canonical generic MTP method (model-specific aliases like 'qwen3_5_mtp' are deprecated and remapped); the old qwen3_5_mtp default built a drafter expecting Qwen3.5-style mtp_block bias weights and crashed DeepSeek MTP checkpoints.

This is a preserved historical claim. The source does not give the vLLM commit, exact model/checkpoint, traceback, run artifacts or the affected version range. This migration did not reproduce the failure or verify whether any named alias is currently deprecated or remapped.

The current collection tool forwards the explicitly selected speculative method to the target vLLM runtime when speculative tokens are positive. Choose a method using that runtime and model's supported interface; the historical account does not define their current behavior.
