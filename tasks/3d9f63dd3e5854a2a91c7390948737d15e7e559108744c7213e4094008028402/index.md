---
blocks:
- block_id: fe2aad2bea3f7311795536f59ce91d005e25108d4ccba93f0b6a31be9bff5665
  indexed: true
  sha256: b4a4b231a796415ac029083b9cd1161e6a820a5bb3b64d3836a7ae8d6f70808b
  source_range:
    end: 4292075
    part: 0
    start: 0
  summary: '已完成 PR #13277 pure head `d37a76b431...` 的四节点审计、部署和 NPU smoke；服务随后完成 target/draft 加载、编译、图捕获并达到 `health=200`。GSM8K prefix-off 串行 c1 为 32/32 正确，c16 为 31/32，配对时发现 7/32 full text 和 17/32 token 序列不同，dataset index 21 从正确 14 变为错误 8。exact 8K 首轮因缺少 `atomic_write_bytes` 未正式计入，修复后正式 c1 落盘为 16/16、accepted/draft `12484/27440`、acceptance 45.50%、AL 4.185，但保留了小写 `none`、内部措辞和控制标记等输出异常。'
  title: d37 pure-head 部署与 prefix-off 基准测试
- block_id: 6b570a95aec4e58a54aad861db80389c534e46e1e1a43e2939c16a1be8ba086b
  indexed: true
  sha256: 93481fb6a6cc7aed1d2242eb1c9f7dee2c377da6ab240d7cc50bfee26bb35e52
  source_range:
    end: 4292075
    part: 1
    start: 0
  summary: exact 8K prefix-off c16 正式完成 16/16，accepted/draft `12408/28070`、acceptance 44.20%、AL 4.094；c1/c16 的 full output/token hash 各有 15/16 不同，扩展规则仍发现内部措辞或控制标记。此前最终落盘因缺少 `atomic_write_text` 失败，失败目录被保留并标为非正式，接口补齐后的 r3 才作为正式结果。prefix-on 的 128K/1K 串行 2/2 正常、缓存命中 98.73%、采信率 63.47%，但相同 prompt 的并发 2 虽均 HTTP 200，却都退化为 1024 个 `!`，采信率异常为 100%；因此不能将该 PR 组合判定为无问题，服务随后按已审计 PGID 停止。
  title: exact 8K 对照与 prefix-on 并发退化
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 3d9f63dd3e5854a2a91c7390948737d15e7e559108744c7213e4094008028402
  kind: experience
  material_digest: 4e1c3a782ed3d9c8179465f064a6dff236453b0b7aac2f13066badd28b5e3d5b
  revision: 38b1bcd06dcdde228adc5573aa2e9a8d81168c4f90e3c46b2acc0467c5ffe885
  schema: mindie-entry/3
  summary: '本批材料记录了 PR #13277 pure head `d37a76b431...` 的四节点部署、prefix-off 对照和 prefix-on 128K 并发测试。需优先检索：客户端原子写入缺陷及保留的失败目录、GSM8K c1/c16 输出不一致、exact 8K 的正式与非正式轮次，以及最终 prefix-on 并发 2 的 `1024 个 !` 退化；这些结果是可追溯的测试观察，不是已验证的最终合入版本结论。'
  title: 当前任务导航：d37 DSpark 检索与退化证据
navigation: '本批材料记录了 PR #13277 pure head `d37a76b431...` 的四节点部署、prefix-off 对照和 prefix-on 128K 并发测试。需优先检索：客户端原子写入缺陷及保留的失败目录、GSM8K c1/c16 输出不一致、exact 8K 的正式与非正式轮次，以及最终 prefix-on 并发 2 的 `1024 个 !` 退化；这些结果是可追溯的测试观察，不是已验证的最终合入版本结论。'
schema: mindie-material-task/1
status: complete
task_id: 3d9f63dd3e5854a2a91c7390948737d15e7e559108744c7213e4094008028402
---

# 当前任务导航：d37 DSpark 检索与退化证据

本批材料记录了 PR #13277 pure head `d37a76b431...` 的四节点部署、prefix-off 对照和 prefix-on 128K 并发测试。需优先检索：客户端原子写入缺陷及保留的失败目录、GSM8K c1/c16 输出不一致、exact 8K 的正式与非正式轮次，以及最终 prefix-on 并发 2 的 `1024 个 !` 退化；这些结果是可追溯的测试观察，不是已验证的最终合入版本结论。

## Materials

- [d37 pure-head 部署与 prefix-off 基准测试](blocks/fe2aad2bea3f7311795536f59ce91d005e25108d4ccba93f0b6a31be9bff5665.md): 已完成 PR #13277 pure head `d37a76b431...` 的四节点审计、部署和 NPU smoke；服务随后完成 target/draft 加载、编译、图捕获并达到 `health=200`。GSM8K prefix-off 串行 c1 为 32/32 正确，c16 为 31/32，配对时发现 7/32 full text 和 17/32 token 序列不同，dataset index 21 从正确 14 变为错误 8。exact 8K 首轮因缺少 `atomic_write_bytes` 未正式计入，修复后正式 c1 落盘为 16/16、accepted/draft `12484/27440`、acceptance 45.50%、AL 4.185，但保留了小写 `none`、内部措辞和控制标记等输出异常。
- [exact 8K 对照与 prefix-on 并发退化](blocks/6b570a95aec4e58a54aad861db80389c534e46e1e1a43e2939c16a1be8ba086b.md): exact 8K prefix-off c16 正式完成 16/16，accepted/draft `12408/28070`、acceptance 44.20%、AL 4.094；c1/c16 的 full output/token hash 各有 15/16 不同，扩展规则仍发现内部措辞或控制标记。此前最终落盘因缺少 `atomic_write_text` 失败，失败目录被保留并标为非正式，接口补齐后的 r3 才作为正式结果。prefix-on 的 128K/1K 串行 2/2 正常、缓存命中 98.73%、采信率 63.47%，但相同 prompt 的并发 2 虽均 HTTP 200，却都退化为 1024 个 `!`，采信率异常为 100%；因此不能将该 PR 组合判定为无问题，服务随后按已审计 PGID 停止。
