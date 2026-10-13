---
blocks:
- block_id: 0d62bb7e13c62f01a356f6c39a22182ab2f762f3a8a622047d7aa991e2c89070
  indexed: true
  sha256: ffba0f5522c121f7266083670c60cb7b93f9a4e24429ad37f2bbc40519015bb4
  source_range:
    end: 891984
    part: 0
    start: 0
  summary: 围绕 PR 15336，先将 15215 作者 YzTongNiar 添加到描述中的 Co-authors，随后根据用户澄清，定位并改写 A2/A3 使用的 FIA score v2 sink 提交 `34f94ca`。历史重放曾因 macOS Bash 数组命令不兼容失败，且一次因远端别名 `maoxx` 错误导致推送失败；这些尝试均未改变远端。后续生成了含 `Co-authored-by` 的新提交（记录中出现 `6bf0afd` 与最终 `6f047ea` 两个 hash），并声称已将更新后的分支强推至 PR 15336，新 head 为 `970788e`，但该结果属于来源中的报告，不能视为独立验证。
  title: PR 15336 FIA score v2 sink 提交添加 co-author
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 61bdcfaf37de1d5c1e02a416decb302ebb3068a1d380cb731c2d4711a29e5d1c
  kind: experience
  material_digest: 992064c4baf5998ff6c41932de19e141d70d997c081500815e75afdd7f02a545
  revision: 6e34fcc1842974fcb2792ec3ea716ab7d2691f3388065c2f207776530dd343d8
  schema: mindie-entry/3
  summary: 当前任务聚焦于 PR 15336 的 FIA score v2 sink 算子提交及 YzTongNiar 的 co-author 添加。来源记录了从修改 PR 描述到改写具体提交的范围修正、两次脚本兼容性失败、一次错误远端别名导致的推送失败，以及随后报告的成功强推；原始提交、co-author 邮箱占位符和 PR 链接仍可独立检索，最终远端状态未在此索引中另行验证。
  title: PR 15336 co-author 与提交历史改写
navigation: 当前任务聚焦于 PR 15336 的 FIA score v2 sink 算子提交及 YzTongNiar 的 co-author 添加。来源记录了从修改 PR 描述到改写具体提交的范围修正、两次脚本兼容性失败、一次错误远端别名导致的推送失败，以及随后报告的成功强推；原始提交、co-author 邮箱占位符和 PR 链接仍可独立检索，最终远端状态未在此索引中另行验证。
schema: mindie-material-task/1
status: complete
task_id: 61bdcfaf37de1d5c1e02a416decb302ebb3068a1d380cb731c2d4711a29e5d1c
---

# PR 15336 co-author 与提交历史改写

当前任务聚焦于 PR 15336 的 FIA score v2 sink 算子提交及 YzTongNiar 的 co-author 添加。来源记录了从修改 PR 描述到改写具体提交的范围修正、两次脚本兼容性失败、一次错误远端别名导致的推送失败，以及随后报告的成功强推；原始提交、co-author 邮箱占位符和 PR 链接仍可独立检索，最终远端状态未在此索引中另行验证。

## Materials

- [PR 15336 FIA score v2 sink 提交添加 co-author](blocks/0d62bb7e13c62f01a356f6c39a22182ab2f762f3a8a622047d7aa991e2c89070.md): 围绕 PR 15336，先将 15215 作者 YzTongNiar 添加到描述中的 Co-authors，随后根据用户澄清，定位并改写 A2/A3 使用的 FIA score v2 sink 提交 `34f94ca`。历史重放曾因 macOS Bash 数组命令不兼容失败，且一次因远端别名 `maoxx` 错误导致推送失败；这些尝试均未改变远端。后续生成了含 `Co-authored-by` 的新提交（记录中出现 `6bf0afd` 与最终 `6f047ea` 两个 hash），并声称已将更新后的分支强推至 PR 15336，新 head 为 `970788e`，但该结果属于来源中的报告，不能视为独立验证。
