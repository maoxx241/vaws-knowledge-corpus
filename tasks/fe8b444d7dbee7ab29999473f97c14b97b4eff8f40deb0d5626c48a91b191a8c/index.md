---
blocks:
- block_id: 09e525963619fd3af8413e1fbf9db29acf31011bbc4362fe808e6358e27e80e5
  indexed: true
  sha256: a62bae94293c8a02508337692ded7a603c33c8e01ef911d3a1f7174d77d2811a
  source_range:
    end: 2513453
    part: 0
    start: 0
  summary: '材料称 PR #15336 的 A5 FlashMLA decode 使用 `PA_BBND`，GM 中 KV cache 按 token 存储 `[k_nope 512维][k_pe 64维]`，完整形状为 `[num_blocks, block_size, 1, 576]`；kernel 搬入 L1 后会重新排布。关于非连续性，`k_nope`/`k_pe` view 的尾轴切片使 token 行间保持 stride=576、最后一维 stride=1；A3/A5 的 `ScatterNdUpdate` 支持范围及 `ScatterPaKvCache` 的 A5 非连续路径均来自源码判断，尚未完成对应 CANN 版本的真机验证，且存在普通连续化兜底回写疑点。后续修正为：上游完整 cache 仍主要需要首轴/block stride 的非连续支持，但分别写入 NoPE/PE 的 view 还需要处理块内 token 的 576 stride；推荐方案在 A5 使用 `aclnnScatterPaKvCache` 的 `scatterMode="None"`，A3 可能需要拼接为 `[T,1,576]` 或改用二维 slot view。'
  title: FlashMLA KV布局与非连续写入方案
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: fe8b444d7dbee7ab29999473f97c14b97b4eff8f40deb0d5626c48a91b191a8c
  kind: experience
  material_digest: 5884f454b8538d7bf10c9dd308cc879a5a8ca4ba648090ef8d025b8da1443388
  revision: 7acfbfe075fa8a2198ccc581d6d8807d704fa936db1ff66026775512e6a1dfb3
  schema: mindie-entry/3
  summary: '当前材料覆盖 PR #15336 的 `[k_nope,k_pe]` 逐 token cache 布局、尾轴切片造成的 stride 特征、A3/A5 `ScatterNdUpdate` 支持边界，以及 A5 `ScatterPaKvCache` 的候选写入路径。需注意结论主要是源码推断：CANN 版本兼容性、A3/A5 真机行为和连续化兜底的正确回写尚未验证；后续方案修正为完整读取 cache 与 NoPE/PE 分离写入 view 需要分别处理首轴和块内 token stride。'
  title: FlashMLA KV cache布局与写入索引
navigation: '当前材料覆盖 PR #15336 的 `[k_nope,k_pe]` 逐 token cache 布局、尾轴切片造成的 stride 特征、A3/A5 `ScatterNdUpdate` 支持边界，以及 A5 `ScatterPaKvCache` 的候选写入路径。需注意结论主要是源码推断：CANN 版本兼容性、A3/A5 真机行为和连续化兜底的正确回写尚未验证；后续方案修正为完整读取 cache 与 NoPE/PE 分离写入 view 需要分别处理首轴和块内 token stride。'
schema: mindie-material-task/1
status: complete
task_id: fe8b444d7dbee7ab29999473f97c14b97b4eff8f40deb0d5626c48a91b191a8c
---

# FlashMLA KV cache布局与写入索引

当前材料覆盖 PR #15336 的 `[k_nope,k_pe]` 逐 token cache 布局、尾轴切片造成的 stride 特征、A3/A5 `ScatterNdUpdate` 支持边界，以及 A5 `ScatterPaKvCache` 的候选写入路径。需注意结论主要是源码推断：CANN 版本兼容性、A3/A5 真机行为和连续化兜底的正确回写尚未验证；后续方案修正为完整读取 cache 与 NoPE/PE 分离写入 view 需要分别处理首轴和块内 token stride。

## Materials

- [FlashMLA KV布局与非连续写入方案](blocks/09e525963619fd3af8413e1fbf9db29acf31011bbc4362fe808e6358e27e80e5.md): 材料称 PR #15336 的 A5 FlashMLA decode 使用 `PA_BBND`，GM 中 KV cache 按 token 存储 `[k_nope 512维][k_pe 64维]`，完整形状为 `[num_blocks, block_size, 1, 576]`；kernel 搬入 L1 后会重新排布。关于非连续性，`k_nope`/`k_pe` view 的尾轴切片使 token 行间保持 stride=576、最后一维 stride=1；A3/A5 的 `ScatterNdUpdate` 支持范围及 `ScatterPaKvCache` 的 A5 非连续路径均来自源码判断，尚未完成对应 CANN 版本的真机验证，且存在普通连续化兜底回写疑点。后续修正为：上游完整 cache 仍主要需要首轴/block stride 的非连续支持，但分别写入 NoPE/PE 的 view 还需要处理块内 token 的 576 stride；推荐方案在 A5 使用 `aclnnScatterPaKvCache` 的 `scatterMode="None"`，A3 可能需要拼接为 `[T,1,576]` 或改用二维 slot view。
