---
blocks:
- block_id: 3bb978c750531893e6294cfbc373cb6a1e6ef1673c295ca45adecc1f1a0dc360
  indexed: true
  sha256: a496dc50510518a117f83502fc0d5752f760eef6c96110314b954b21b6aca00e
  source_range:
    end: 1834779
    part: 0
    start: 0
  summary: 围绕 R21 async-off 与 device sync 差异，调查 draft token 的 D2H 私有流事件和缓冲区生命周期；在独立 worktree 的 `model_runner_v1` 中提交了 production event join（commit `3698c330a58f74a2235f575d431c35c42b526eab`）。仅当本轮实际记录 D2H event 时，DSpark global postprocess 等待该 producer event；跳过 copy 或非 DSpark 路径仍等待 sampling event，并在复用持久 draft buffer 前等待上一轮 D2H 完成。focused UT、ruff、格式、py_compile 和 diff-check 据称通过，但因本机缺少 `torch`，pytest 未进入执行阶段；该改动仍只是“强侧流根因候选”，此前 device sync 治愈了 R21 尾部 control，尚未由 fresh canonical B2 验证为最终因果结论。
  title: DSpark draft D2H producer event join 修复及待验证状态
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 528a88c91e54472bf43d2d44c5aff7a532bc54a9a7f3dea904820f70dd744b26
  kind: experience
  material_digest: b7c19a536d261ec2d74c1231a0fa2bd923bd869710cb53e3562fe624b229b273
  revision: f8af48336bd96f0f2a633b7ebde8dea3e9fbb7ec359b3092baa571cc88de6e27
  schema: mindie-entry/3
  summary: 当前可检索重点是 R21 async-off、draft token D2H 私有流、producer event、global state-update、持久 draft buffer 复用，以及 commit `3698c330a58f74a2235f575d431c35c42b526eab`。已记录的结果包括静态检查和 focused UT 提交，但 pytest 受缺少 `torch` 阻断；修复尚未证明为最终根因，device sync 对 R21 尾部 control 的治愈仍是待 fresh canonical B2 排除或确认的线索。
  title: 当前任务导航：DSpark draft D2H 事件同步修复，等待 canonical B2 验证
navigation: 当前可检索重点是 R21 async-off、draft token D2H 私有流、producer event、global state-update、持久 draft buffer 复用，以及 commit `3698c330a58f74a2235f575d431c35c42b526eab`。已记录的结果包括静态检查和 focused UT 提交，但 pytest 受缺少 `torch` 阻断；修复尚未证明为最终根因，device sync 对 R21 尾部 control 的治愈仍是待 fresh canonical B2 排除或确认的线索。
schema: mindie-material-task/1
status: complete
task_id: 528a88c91e54472bf43d2d44c5aff7a532bc54a9a7f3dea904820f70dd744b26
---

# 当前任务导航：DSpark draft D2H 事件同步修复，等待 canonical B2 验证

当前可检索重点是 R21 async-off、draft token D2H 私有流、producer event、global state-update、持久 draft buffer 复用，以及 commit `3698c330a58f74a2235f575d431c35c42b526eab`。已记录的结果包括静态检查和 focused UT 提交，但 pytest 受缺少 `torch` 阻断；修复尚未证明为最终根因，device sync 对 R21 尾部 control 的治愈仍是待 fresh canonical B2 排除或确认的线索。

## Materials

- [DSpark draft D2H producer event join 修复及待验证状态](blocks/3bb978c750531893e6294cfbc373cb6a1e6ef1673c295ca45adecc1f1a0dc360.md): 围绕 R21 async-off 与 device sync 差异，调查 draft token 的 D2H 私有流事件和缓冲区生命周期；在独立 worktree 的 `model_runner_v1` 中提交了 production event join（commit `3698c330a58f74a2235f575d431c35c42b526eab`）。仅当本轮实际记录 D2H event 时，DSpark global postprocess 等待该 producer event；跳过 copy 或非 DSpark 路径仍等待 sampling event，并在复用持久 draft buffer 前等待上一轮 D2H 完成。focused UT、ruff、格式、py_compile 和 diff-check 据称通过，但因本机缺少 `torch`，pytest 未进入执行阶段；该改动仍只是“强侧流根因候选”，此前 device sync 治愈了 R21 尾部 control，尚未由 fresh canonical B2 验证为最终因果结论。
