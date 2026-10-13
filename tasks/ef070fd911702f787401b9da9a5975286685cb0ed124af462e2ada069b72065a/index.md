---
blocks:
- block_id: 84179f00fa1a298f3f74898e0c9525f31e028d52bac45546cdeb5be423529fec
  indexed: true
  sha256: bcc685c37db0ba56a39fd7d3a96671916e867cd2ab33c09e918a639ed15562c1
  source_range:
    end: 386107
    part: 0
    start: 0
  summary: 只读审计报告称：153/154/156 的 r2 根 PGID 已消失，155 的根进程 2907260 及同组 multiprocessing 子进程仍残留；四机 NPU 无运行进程，18077/15569 无监听。报告将 `llm_base_proposer.py:361` 无条件读取缺失的 `KimiK3Config.image_token_index` 作为主因，并称 155 的 TBE `IndexError` 发生在 executor shutdown、SIGTERM 之后，属于级联错误；这些判断仍是该报告的参考结论，未独立验证。
  title: 四节点进程与异常链只读审计
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: ef070fd911702f787401b9da9a5975286685cb0ed124af462e2ada069b72065a
  kind: experience
  material_digest: 446613fa6b28a8e7284a87e24dafec5b51b50997b05939c49c91a68211371368
  revision: 4593ddbd593b12440c452266f0d9f123487710febc28f5d680bda3c769c1a5d0
  schema: mindie-entry/3
  summary: 新材料记录了一次针对四节点根进程、NPU 占用、端口监听及原日志异常链的只读核对。可检索术语包括 PGID、multiprocessing、NPU、18077、15569、`llm_base_proposer.py:361`、`KimiK3Config.image_token_index`、TBE `IndexError`、executor shutdown 与 SIGTERM；报告称未终止进程、未发送请求、未修改文件，但主因与级联关系仍应视为未独立验证的参考判断。
  title: 当前任务：四节点故障审计参考索引
navigation: 新材料记录了一次针对四节点根进程、NPU 占用、端口监听及原日志异常链的只读核对。可检索术语包括 PGID、multiprocessing、NPU、18077、15569、`llm_base_proposer.py:361`、`KimiK3Config.image_token_index`、TBE `IndexError`、executor shutdown 与 SIGTERM；报告称未终止进程、未发送请求、未修改文件，但主因与级联关系仍应视为未独立验证的参考判断。
schema: mindie-material-task/1
status: complete
task_id: ef070fd911702f787401b9da9a5975286685cb0ed124af462e2ada069b72065a
---

# 当前任务：四节点故障审计参考索引

新材料记录了一次针对四节点根进程、NPU 占用、端口监听及原日志异常链的只读核对。可检索术语包括 PGID、multiprocessing、NPU、18077、15569、`llm_base_proposer.py:361`、`KimiK3Config.image_token_index`、TBE `IndexError`、executor shutdown 与 SIGTERM；报告称未终止进程、未发送请求、未修改文件，但主因与级联关系仍应视为未独立验证的参考判断。

## Materials

- [四节点进程与异常链只读审计](blocks/84179f00fa1a298f3f74898e0c9525f31e028d52bac45546cdeb5be423529fec.md): 只读审计报告称：153/154/156 的 r2 根 PGID 已消失，155 的根进程 2907260 及同组 multiprocessing 子进程仍残留；四机 NPU 无运行进程，18077/15569 无监听。报告将 `llm_base_proposer.py:361` 无条件读取缺失的 `KimiK3Config.image_token_index` 作为主因，并称 155 的 TBE `IndexError` 发生在 executor shutdown、SIGTERM 之后，属于级联错误；这些判断仍是该报告的参考结论，未独立验证。
