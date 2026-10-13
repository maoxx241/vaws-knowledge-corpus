---
blocks:
- block_id: affdc42d5f8f79d7c158d6063a2eab0a04467d120c9717209e776c7efa54c1ff
  indexed: true
  sha256: 7d9f4b2cbe88eb09e97bf135652be311e6330946afee2f1c41ca2585f3629fa9
  source_range:
    end: 587330
    part: 0
    start: 0
  summary: 已据报告回退 PR4 中新增的 W4A16、W4A16MXFP4 和 compressed-MXFP4 packed 支持及相关测试，保留 Kimi K3、A5 W4A8/ModelSlim、SiTU fallback 与 KDA 改动。报告称基线扫描、`git diff --check` 和 Python 编译检查通过，且相对 PR 基线未残留新增 W4A16/MXFP4 packed 关键词。定向单测因本机缺少 `torch` 未能启动；修改尚未提交，且上述结论属于报告内容而非独立验证。
  title: PR4 回退 W4A16/MXFP4 packed 改动
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 07945847b2e4af0001016f42b031ea3eae1996eb62fa3e548c2672e086777986
  kind: experience
  material_digest: 19d0b5fd77dd917c9fc1c65d88e715312209830716e05839e6a5188ca4344223
  revision: 0f0a2564491640cf3d995ef56e47a8cb2377638b55a3f3a71af79cc26915d42a
  schema: mindie-entry/3
  summary: 检索 PR4 中 W4A16/MXFP4 packed 改动的回退记录，以及保留的 A5 W4A8/ModelSlim、Kimi K3、SiTU fallback 和 KDA 范围。注意单测受缺少 `torch` 阻断，静态检查据报告通过，修改未提交；原始材料仍可独立检索。
  title: 当前任务导航
navigation: 检索 PR4 中 W4A16/MXFP4 packed 改动的回退记录，以及保留的 A5 W4A8/ModelSlim、Kimi K3、SiTU fallback 和 KDA 范围。注意单测受缺少 `torch` 阻断，静态检查据报告通过，修改未提交；原始材料仍可独立检索。
schema: mindie-material-task/1
status: complete
task_id: 07945847b2e4af0001016f42b031ea3eae1996eb62fa3e548c2672e086777986
---

# 当前任务导航

检索 PR4 中 W4A16/MXFP4 packed 改动的回退记录，以及保留的 A5 W4A8/ModelSlim、Kimi K3、SiTU fallback 和 KDA 范围。注意单测受缺少 `torch` 阻断，静态检查据报告通过，修改未提交；原始材料仍可独立检索。

## Materials

- [PR4 回退 W4A16/MXFP4 packed 改动](blocks/affdc42d5f8f79d7c158d6063a2eab0a04467d120c9717209e776c7efa54c1ff.md): 已据报告回退 PR4 中新增的 W4A16、W4A16MXFP4 和 compressed-MXFP4 packed 支持及相关测试，保留 Kimi K3、A5 W4A8/ModelSlim、SiTU fallback 与 KDA 改动。报告称基线扫描、`git diff --check` 和 Python 编译检查通过，且相对 PR 基线未残留新增 W4A16/MXFP4 packed 关键词。定向单测因本机缺少 `torch` 未能启动；修改尚未提交，且上述结论属于报告内容而非独立验证。
