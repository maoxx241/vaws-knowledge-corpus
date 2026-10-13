---
blocks:
- block_id: df074f386f332bfbfd598bcd1f32a799fe013124f3b9f1378af85cd8975b37ed
  indexed: true
  sha256: 9ae8997ab96d5174fbbb79d0817679ea399513a7b83c4679120e8a8abacac0c5
  source_range:
    end: 357068
    part: 0
    start: 0
  summary: 只读核验显示，`logprobs=5` 推理完成但响应组装返回 HTTP 400，metrics 增量为 drafts +1、draft_tokens +7、accepted +0；定向 `logprob_token_ids=[0,12046]` 返回 HTTP 500，报错指向 `top_logprobs[i]` 越界，提示 sample/output token 与 logprob 序列可能错位或缺行，但尚未验证根因。随后核对四节点 rank、DSpark 参数、启动时间、cgroup、日志和 PGID 后，仅向对应进程组发送 TERM，均正常退出且无需 KILL；相关进程、监听端口均无残留，日志和结果目录保留，未启动新服务。API 仍无法区分全零 tie 与 NaN，建议后续增加 raw logits/sample-buffer 诊断或进行 target-only graph/eager A/B，但这些检查尚未完成。
  title: ACLGraph logprob 错位诊断与 run 清理
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 64f7685f3437ade3c060f9109846cee2e17fe57d0c9b76779ba81c765353063f
  kind: experience
  material_digest: 48dcd44a1bc52a5c1f60351a875dda20b763a0c76941af8bbe472c25a2df6cd0
  revision: d93daf7769b7cc53be9ccf3a240d68a29cd120d1fa68262b4b5ff06dff87f461
  schema: mindie-entry/3
  summary: 已记录 logprob 响应组装错误、`top_logprobs[i]` 越界及可能的序列错位假设；该根因尚未验证，且 API 不能区分全零 tie 与 NaN。指定 ACLGraph run 已通过四节点身份核对并以 TERM 清理，未发现进程或端口残留，日志与 GSM8K 结果保留；后续 raw logits/sample-buffer 诊断或 graph/eager A/B 仍未执行。
  title: 当前任务导航：ACLGraph logprob 诊断与服务清理
navigation: 已记录 logprob 响应组装错误、`top_logprobs[i]` 越界及可能的序列错位假设；该根因尚未验证，且 API 不能区分全零 tie 与 NaN。指定 ACLGraph run 已通过四节点身份核对并以 TERM 清理，未发现进程或端口残留，日志与 GSM8K 结果保留；后续 raw logits/sample-buffer 诊断或 graph/eager A/B 仍未执行。
schema: mindie-material-task/1
status: complete
task_id: 64f7685f3437ade3c060f9109846cee2e17fe57d0c9b76779ba81c765353063f
---

# 当前任务导航：ACLGraph logprob 诊断与服务清理

已记录 logprob 响应组装错误、`top_logprobs[i]` 越界及可能的序列错位假设；该根因尚未验证，且 API 不能区分全零 tie 与 NaN。指定 ACLGraph run 已通过四节点身份核对并以 TERM 清理，未发现进程或端口残留，日志与 GSM8K 结果保留；后续 raw logits/sample-buffer 诊断或 graph/eager A/B 仍未执行。

## Materials

- [ACLGraph logprob 错位诊断与 run 清理](blocks/df074f386f332bfbfd598bcd1f32a799fe013124f3b9f1378af85cd8975b37ed.md): 只读核验显示，`logprobs=5` 推理完成但响应组装返回 HTTP 400，metrics 增量为 drafts +1、draft_tokens +7、accepted +0；定向 `logprob_token_ids=[0,12046]` 返回 HTTP 500，报错指向 `top_logprobs[i]` 越界，提示 sample/output token 与 logprob 序列可能错位或缺行，但尚未验证根因。随后核对四节点 rank、DSpark 参数、启动时间、cgroup、日志和 PGID 后，仅向对应进程组发送 TERM，均正常退出且无需 KILL；相关进程、监听端口均无残留，日志和结果目录保留，未启动新服务。API 仍无法区分全零 tie 与 NaN，建议后续增加 raw logits/sample-buffer 诊断或进行 target-only graph/eager A/B，但这些检查尚未完成。
