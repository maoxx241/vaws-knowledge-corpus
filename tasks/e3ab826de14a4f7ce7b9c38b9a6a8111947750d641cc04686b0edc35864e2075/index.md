---
blocks:
- block_id: d4d883e16091b2a8baad5abd7f8eee2b0ca242c66d2704a2933144c5af798f64
  indexed: true
  sha256: 827c77d786183708550b069bb8128d424eeb4a60f74f03a169147db4768fc8f4
  source_range:
    end: 319278
    part: 0
    start: 0
  summary: 用户要求终止由指定对话启动的远端进程，并避免误杀 155 上正在进行的新权重量化任务。助手报告已定位四个 PR14 服务进程组（153/154/155/156），中途首次终止命令因本机 shell 参数拆分错误而未执行，随后改用固定 Bash 解析；其后报告四组均已退出且 155 的量化任务未被匹配，但该成功结果属于来源中的助手报告，仍应视为未独立验证的记录。
  title: 精确终止 PR14 四节点服务进程组
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: e3ab826de14a4f7ce7b9c38b9a6a8111947750d641cc04686b0edc35864e2075
  kind: experience
  material_digest: 3ac10a448e434ecbbc5ee228ee0183c1af99ccbdb11e048042a86a73da1b8b86
  revision: e73d89f2ea2773ec318c9ca6c3d44a53d73d8394e71554c1465b9aa65b6c11b3
  schema: mindie-entry/3
  summary: 当前索引涵盖一次针对指定对话启动的 PR14 四节点服务进程组清理，包含进程组定位、155 节点量化任务排除、初次 shell 构造失败及后续据称完成的 TERM 与残留复查。最终“全部无残留且未误杀量化任务”是来源中的报告性结论，不应提升为独立验证的成功事实。
  title: 远端 PR14 服务进程终止记录
navigation: 当前索引涵盖一次针对指定对话启动的 PR14 四节点服务进程组清理，包含进程组定位、155 节点量化任务排除、初次 shell 构造失败及后续据称完成的 TERM 与残留复查。最终“全部无残留且未误杀量化任务”是来源中的报告性结论，不应提升为独立验证的成功事实。
schema: mindie-material-task/1
status: complete
task_id: e3ab826de14a4f7ce7b9c38b9a6a8111947750d641cc04686b0edc35864e2075
---

# 远端 PR14 服务进程终止记录

当前索引涵盖一次针对指定对话启动的 PR14 四节点服务进程组清理，包含进程组定位、155 节点量化任务排除、初次 shell 构造失败及后续据称完成的 TERM 与残留复查。最终“全部无残留且未误杀量化任务”是来源中的报告性结论，不应提升为独立验证的成功事实。

## Materials

- [精确终止 PR14 四节点服务进程组](blocks/d4d883e16091b2a8baad5abd7f8eee2b0ca242c66d2704a2933144c5af798f64.md): 用户要求终止由指定对话启动的远端进程，并避免误杀 155 上正在进行的新权重量化任务。助手报告已定位四个 PR14 服务进程组（153/154/155/156），中途首次终止命令因本机 shell 参数拆分错误而未执行，随后改用固定 Bash 解析；其后报告四组均已退出且 155 的量化任务未被匹配，但该成功结果属于来源中的助手报告，仍应视为未独立验证的记录。
