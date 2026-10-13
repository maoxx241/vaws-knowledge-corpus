---
blocks:
- block_id: c907aa4801b49ea54fddae2dd9b6875b40541c6f6ad5c9097046d041ad33e7fa
  indexed: true
  sha256: 2919cc2a8059768de81e0e05d1a73b7846cf08a62aefa5fc90c405b4c29743c1
  source_range:
    end: 791913
    part: 0
    start: 0
  summary: r19 已完成四机 staging、readback 与 PREFLIGHT_ONLY，未启停服务或发 HTTP；但后续只读 B2 核验显示 miss 与两次 warm 正常，prefix-hit 虽 HTTP 200、accepted/draft 为 896/896，却返回 1024 个 token-id 0（全为 `!`），因此 `experiment_valid=false`、`all_passed=false`。四机进程、成员数、cmdline、源码 manifest 和日志均未漂移，未发现 Traceback/FATAL/CRITICAL/RuntimeError/ERROR；问题暂被约束到 accepted-token completion 到最终 host-visible token buffer 物化之间的 stream/event 或 buffer ownership，不能视为 fence 修复成功。
  title: r19 四机 completion-fence 只读核验：prefix-hit 输出损坏
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 693000860f9471c78daf3abb6431338663bb0adb1ff348f4985fcb19cc783b86
  kind: experience
  material_digest: dbd6c634538d349959b825fd35ff9dab7a580cf4ce228406ba2b49a1f14987d4
  revision: 8f25900fb4acb0eed02c251b0886fe21ed03817cd0d74b3f85f1aae101fde756
  schema: mindie-entry/3
  summary: 唯一新材料块记录了 r19 completion-fence 的四机闭包、哈希、B2 分阶段结果及失败时间线；可检索 `PREFLIGHT_ONLY`、`experiment_valid=false`、`all_passed=false`、`HTTP 200`、`896/384`、`accepted/draft 896/896`、全零 token、all-`!`、fail-closed、manifest、stream/event、buffer ownership 等术语。当前仍是不完整且未修复的结果：miss/warm 正常，hit 输出错误；后续方向是核查 hit 路径中 accepted-token completion 至最终输出缓冲区的依赖与所有权，不能将预检、manifest PASS 或进程健康提升为功能成功。
  title: 当前任务导航：r19 prefix-hit 输出损坏证据索引
navigation: 唯一新材料块记录了 r19 completion-fence 的四机闭包、哈希、B2 分阶段结果及失败时间线；可检索 `PREFLIGHT_ONLY`、`experiment_valid=false`、`all_passed=false`、`HTTP 200`、`896/384`、`accepted/draft 896/896`、全零 token、all-`!`、fail-closed、manifest、stream/event、buffer ownership 等术语。当前仍是不完整且未修复的结果：miss/warm 正常，hit 输出错误；后续方向是核查 hit 路径中 accepted-token completion 至最终输出缓冲区的依赖与所有权，不能将预检、manifest PASS 或进程健康提升为功能成功。
schema: mindie-material-task/1
status: complete
task_id: 693000860f9471c78daf3abb6431338663bb0adb1ff348f4985fcb19cc783b86
---

# 当前任务导航：r19 prefix-hit 输出损坏证据索引

唯一新材料块记录了 r19 completion-fence 的四机闭包、哈希、B2 分阶段结果及失败时间线；可检索 `PREFLIGHT_ONLY`、`experiment_valid=false`、`all_passed=false`、`HTTP 200`、`896/384`、`accepted/draft 896/896`、全零 token、all-`!`、fail-closed、manifest、stream/event、buffer ownership 等术语。当前仍是不完整且未修复的结果：miss/warm 正常，hit 输出错误；后续方向是核查 hit 路径中 accepted-token completion 至最终输出缓冲区的依赖与所有权，不能将预检、manifest PASS 或进程健康提升为功能成功。

## Materials

- [r19 四机 completion-fence 只读核验：prefix-hit 输出损坏](blocks/c907aa4801b49ea54fddae2dd9b6875b40541c6f6ad5c9097046d041ad33e7fa.md): r19 已完成四机 staging、readback 与 PREFLIGHT_ONLY，未启停服务或发 HTTP；但后续只读 B2 核验显示 miss 与两次 warm 正常，prefix-hit 虽 HTTP 200、accepted/draft 为 896/896，却返回 1024 个 token-id 0（全为 `!`），因此 `experiment_valid=false`、`all_passed=false`。四机进程、成员数、cmdline、源码 manifest 和日志均未漂移，未发现 Traceback/FATAL/CRITICAL/RuntimeError/ERROR；问题暂被约束到 accepted-token completion 到最终 host-visible token buffer 物化之间的 stream/event 或 buffer ownership，不能视为 fence 修复成功。
