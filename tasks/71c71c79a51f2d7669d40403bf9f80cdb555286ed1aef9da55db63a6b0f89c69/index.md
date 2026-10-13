---
blocks:
- block_id: 8290a6af82468ad377441200ca59bce4b14b73ce93cd8c4917ecdb8b3c795773
  indexed: true
  sha256: c56b17bb870b0e1ff135358037224861d4ae1bb45f4f3626e056bf1f9057e02b
  source_range:
    end: 1379537
    part: 0
    start: 0
  summary: 只读排查认为当前偶现问题更符合高并发收缩时的跨流同步竞态，不支持将 causal conv1d 原地写注册问题作为根因；两个坏请求均在 rank0，首次可见 FULL replay 时 hidden 已为 NaN，但首个 NaN 之前的日志未覆盖，因此故障起点无法确认。两请求的 block table、slot mapping 和 GDN state slots 不同，暂未发现彼此直接写同一 slot；一个待验证假设是异步 hybrid state postprocess 在下一轮 condense/reorder 前读取了旧 request row。建议在非 async DSpark 的 `_update_states()` 入口、parent remove/condense 前同步 `num_accepted_tokens_event`，但该候选修复尚未验证，本次未修改文件或进程。
  title: 高并发下疑似跨流同步竞态
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 71c71c79a51f2d7669d40403bf9f80cdb555286ed1aef9da55db63a6b0f89c69
  kind: experience
  material_digest: f99a4020df3e74f2cb88e0e30daa21fa01e3b45b90b3c515f34851bf17833525
  revision: ed70ebc70a942fcb1923c9c3c1e0917f1ad856e763fda54d2f7bf33c7d32964a
  schema: mindie-entry/3
  summary: 当前证据指向 rank0 高并发收缩期间可能存在跨流同步或 padding 相关竞态；causal conv1d 原地写注册问题暂不支持为根因。已观察到 FULL replay 前已有异常 token，replay 中 hidden 为 NaN，dummy reset 不是已确认的根修复；异步 state postprocess 与 block table condense/reorder 的时序关系仍是假设，候选同步修复尚未验证。首个 NaN 前的 hidden/logits 缺少日志，后续应通过相同 hot-rank case A/B 验证，并补充 NONE 首个 spec batch 的 finite、accepted 及 state-copy src/dst 观测。
  title: 当前任务导航：高并发偶现 NaN 问题索引
navigation: 当前证据指向 rank0 高并发收缩期间可能存在跨流同步或 padding 相关竞态；causal conv1d 原地写注册问题暂不支持为根因。已观察到 FULL replay 前已有异常 token，replay 中 hidden 为 NaN，dummy reset 不是已确认的根修复；异步 state postprocess 与 block table condense/reorder 的时序关系仍是假设，候选同步修复尚未验证。首个 NaN 前的 hidden/logits 缺少日志，后续应通过相同 hot-rank case A/B 验证，并补充 NONE 首个 spec batch 的 finite、accepted 及 state-copy src/dst 观测。
schema: mindie-material-task/1
status: complete
task_id: 71c71c79a51f2d7669d40403bf9f80cdb555286ed1aef9da55db63a6b0f89c69
---

# 当前任务导航：高并发偶现 NaN 问题索引

当前证据指向 rank0 高并发收缩期间可能存在跨流同步或 padding 相关竞态；causal conv1d 原地写注册问题暂不支持为根因。已观察到 FULL replay 前已有异常 token，replay 中 hidden 为 NaN，dummy reset 不是已确认的根修复；异步 state postprocess 与 block table condense/reorder 的时序关系仍是假设，候选同步修复尚未验证。首个 NaN 前的 hidden/logits 缺少日志，后续应通过相同 hot-rank case A/B 验证，并补充 NONE 首个 spec batch 的 finite、accepted 及 state-copy src/dst 观测。

## Materials

- [高并发下疑似跨流同步竞态](blocks/8290a6af82468ad377441200ca59bce4b14b73ce93cd8c4917ecdb8b3c795773.md): 只读排查认为当前偶现问题更符合高并发收缩时的跨流同步竞态，不支持将 causal conv1d 原地写注册问题作为根因；两个坏请求均在 rank0，首次可见 FULL replay 时 hidden 已为 NaN，但首个 NaN 之前的日志未覆盖，因此故障起点无法确认。两请求的 block table、slot mapping 和 GDN state slots 不同，暂未发现彼此直接写同一 slot；一个待验证假设是异步 hybrid state postprocess 在下一轮 condense/reorder 前读取了旧 request row。建议在非 async DSpark 的 `_update_states()` 入口、parent remove/condense 前同步 `num_accepted_tokens_event`，但该候选修复尚未验证，本次未修改文件或进程。
