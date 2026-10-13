---
blocks:
- block_id: 746099289c8cf6ce5e849176d7964f942fb36ed5d4022acbef6cc4ed1a0679e1
  indexed: true
  sha256: 031000a5e633e8ea048dcee6088389002e742059165cf9df0dac6925fb3900a0
  source_range:
    end: 177715
    part: 0
    start: 0
  summary: 诊断认为错误发生在 AISBench 本地生成数据并调用 `AutoTokenizer.from_pretrained()`、校验 `trust_remote_code` 的阶段，请求尚未到达 vLLM，因此暂不能归因于 reasoning/tool parser；末尾 `ERR99999` 被描述为未捕获 Python 异常后的二次提示。建议在确认信任自定义 tokenizer 代码的前提下设置 `trust_remote_code=True`，但该修复未在材料中验证成功。另指出 `input_len=131024`、`output_len=1024` 时总长度为 132048，若服务 `--max-model-len=131072` 将超出 976 tokens，修复 tokenizer 后可能出现长度超限；模板/BOS 开销及最终行为仍需实测。
  title: 128K 压测报错与 tokenizer/parser 定位
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: ae4f6345ad6840364db56dadb6c94019933d9f86e2c23ace9f668bed8dfca0f4
  kind: experience
  material_digest: 71909a5fada81477cd36c1768ed8732a6ee24e57679e7e09f979b3710c0c9953
  revision: f34989ba8a9388d2c2246ee3e9be89bed8b8dff64d62fccfcdbca8f35c91780c
  schema: mindie-entry/3
  summary: 已记录一个关于 AISBench tokenizer 初始化、`trust_remote_code`、parser 归因以及 128K 长度预算的 fallible 参考索引。当前结论是 parser 归因未获支持，tokenizer 修改和后续长度验证均未确认完成；原始材料可独立检索，后续应保留任何实际测试结果或修正。
  title: 当前任务导航：128K 压测错误索引
navigation: 已记录一个关于 AISBench tokenizer 初始化、`trust_remote_code`、parser 归因以及 128K 长度预算的 fallible 参考索引。当前结论是 parser 归因未获支持，tokenizer 修改和后续长度验证均未确认完成；原始材料可独立检索，后续应保留任何实际测试结果或修正。
schema: mindie-material-task/1
status: complete
task_id: ae4f6345ad6840364db56dadb6c94019933d9f86e2c23ace9f668bed8dfca0f4
---

# 当前任务导航：128K 压测错误索引

已记录一个关于 AISBench tokenizer 初始化、`trust_remote_code`、parser 归因以及 128K 长度预算的 fallible 参考索引。当前结论是 parser 归因未获支持，tokenizer 修改和后续长度验证均未确认完成；原始材料可独立检索，后续应保留任何实际测试结果或修正。

## Materials

- [128K 压测报错与 tokenizer/parser 定位](blocks/746099289c8cf6ce5e849176d7964f942fb36ed5d4022acbef6cc4ed1a0679e1.md): 诊断认为错误发生在 AISBench 本地生成数据并调用 `AutoTokenizer.from_pretrained()`、校验 `trust_remote_code` 的阶段，请求尚未到达 vLLM，因此暂不能归因于 reasoning/tool parser；末尾 `ERR99999` 被描述为未捕获 Python 异常后的二次提示。建议在确认信任自定义 tokenizer 代码的前提下设置 `trust_remote_code=True`，但该修复未在材料中验证成功。另指出 `input_len=131024`、`output_len=1024` 时总长度为 132048，若服务 `--max-model-len=131072` 将超出 976 tokens，修复 tokenizer 后可能出现长度超限；模板/BOS 开销及最终行为仍需实测。
