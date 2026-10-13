---
block_id: cc7ade01fdde7bff2201410d86bbaadf15dcc3ff7a1fd3ce5747d3c44650b26b
schema: mindie-material-block/1
source_range:
  end: 1789907
  part: 1
  start: 0
---

### assistant:commentary
下载约 34%（1.86 GB），速度稳定在约 19–21 MB/s，预计剩余约 3 分钟。随后会用仓库 LFS SHA-256 `4182214c…ded72` 校验完整文件。

### assistant:commentary
下载约 56%（3.08 GB），剩余约 2 分钟。下载完成后除了整文件哈希，我会直接解析 safetensors header，列出注意力权重张量形状，以此确认 MLA 结构而不只依赖 README。

### assistant:commentary
下载约 79%，剩余约 1 分钟，过程无报错。下载完成后会先校验再上传，不会让四台看到未验证文件。

### assistant:commentary
主权重已完整下载，用时约 4 分 47 秒。现在同时做两项只读验证：对照 Hugging Face LFS SHA-256 校验整文件，以及解析 safetensors header 的 44 个张量，确认注意力投影结构。

### assistant:commentary
权重层面也确认是 MLA：

- 每层有 `kv_a_proj_with_mqa [576,7168]`
- 其中 `512` 是压缩 KV latent，另 `64` 是 RoPE key 分量
- 后续 `kv_b_proj [24576,512]` 从 latent 展开到多头表示
- 三层均采用同一结构

这不是普通 GQA 的独立 K/V 投影，也不是标准 MHA KV cache；缓存语义是 **MLA compressed latent KV + RoPE 分量**。现在把已校验文件上传到共享 NFS 隐藏目录，远端再次校验后发布。

### assistant:commentary
上传约 10%，当前约 19 MB/s，预计还需 4 分钟左右。正式目录仍未出现，原有权重与服务没有改动。

### assistant:commentary
上传约 30%（1.66 GB），NFS 写入约 16–17 MB/s，预计还需约 4 分钟。传输无报错。

### assistant:commentary
上传约 50%（2.75 GB），预计还需约 2 分 40 秒。完成后会在服务器端重新计算 5.7 GB 文件的 SHA-256，匹配才原子改名。

### assistant:commentary
上传约 71%，预计还需 1 分半左右。所有内容仍在隐藏临时目录中。

### assistant:commentary
上传约 91%，剩余约半分钟。随后做远端哈希和四节点读取确认。

### assistant:commentary
上传完成，用时约 5 分 17 秒。现在进行服务器端完整 SHA-256；匹配后原子发布 `[REDACTED_SECRET]`。

### assistant:commentary
正式目录已发布，四台均能读取 8 个文件，主文件大小一致为 `5,707,153,178` 字节。附带的 `config_sha` 格式化命令因远端引号导致 `cut` 报错，但文件读取和大小检查已成功；我用原始 `sha256sum` 再核对配置一致性，然后把本地暂存副本移入废纸篓。

### assistant:final_answer
已下载并发布到四台共享权重目录：

`[REDACTED_SECRET]`

下载信息：

- 来源：[Inferact/Kimi-K3-DSpark-Block5](https://huggingface.[REDACTED_SECRET])
- Revision：`b6d39a923a1af9570a8990048305214282e3b7c1`
- 共 8 个文件，约 5.4 GiB
- `model.safetensors`：`5,707,153,178` 字节
- SHA-256：`4182214cd0e9898c56ffb220dfababf1e7d8d9029b0a08f4ecc12be0ea6ded72`
- 本地和服务器双端校验通过
- 四台容器读取到相同文件大小和配置哈希
- 未启动、停止或修改现有服务
- 本地暂存副本已移入废纸篓

KV 结论：这是 MLA

不是 GQA，也不是普通 MHA KV cache，而是：

**MLA compressed latent KV + 单独的 RoPE key 分量**

直接证据：

- `kv_lora_rank = 512`
- `q_lora_rank = 1536`
- `qk_nope_head_dim = 128`
- `qk_rope_head_dim = 64`
- 每层 `kv_a_proj_with_mqa` 形状为 `[576, 7168]`
  - `576 = 512 latent KV + 64 RoPE key`
- 每层 `kv_b_proj` 为 `[24576, 512]`
- README 明确标记为 `full-causal MLA`
- 推理配置使用 `FLASHINFER_MLA`

虽然配置里有 `num_key_value_heads=96`，但不能据此判断为普通 MHA。真实权重中没有传统的独立 K/V 投影，而是先压缩为 512 维 latent，再通过 `kv_b_proj` 展开，这是典型 MLA 结构。

另外，`Block5` 指的是一次 forward 草拟五个 token：

- `block_size = 5`
- Draft 模型本身是 3 层 MLA
- 对应目标层：`[71, 87, 91]`

不是“5 层模型”。



### user
评估一下这个和[https://huggingface.co/Inferact/Kimi-K3-DSpark](https://huggingface.co/Inferact/Kimi-K3-DSpark)这个权重的区别是不是就是一个事block7一个事block5


### assistant:commentary
我会按两个仓库的当前 revision 做逐字段比较，不只看 `block_size`：模型层数、目标层、注意力/KV 结构、Markov/confidence head、RoPE/上下文配置、权重张量数量和大小、训练 checkpoint/benchmark。先下载两个仓库的小型配置与 manifest；主权重先不重复下载。

### assistant:commentary
直接按 revision 拉取小文件时仓库返回了 404；我先检查 API 返回的当前树，确认是不是仓库刚改了文件布局或 revision 正在切换。这个阶段只读，不影响服务器权重。

### assistant:commentary
初步结论已经很明确：**不只是 block7 和 block5 的区别**。两者都属于 MLA-native DSpark，但 Block5 是一次重新设计/训练的更小 draft：

- Block7：5 层、non-causal attention、5 个目标 hidden-state 输入层
- Block5：3 层、full-causal attention、3 个更靠后的目标 hidden-state 输入层
- Markov rank、头数、RoPE 参数、训练 checkpoint 和权重大小也不同

我再核对两份 safetensors 的张量数量/形状及模型卡 benchmark，给出是否可直接互换、性能取舍的结论。

### assistant:commentary
对比完成。核心结论是：**两者同属 MLA DSpark，但 Block5 不是把 Block7 的 `7` 改成 `5`；它是另一套结构、另一批权重、另一轮训练。** 我再清理本次几 KB 的临时比较文件，然后给你具体取舍建议。

### assistant:final_answer
结论：**不只是一个 block7、一个 block5。它们都是 MLA-native DSpark，但 Block5 是重新设计并重新训练的一套 draft 模型，不能只修改 `block_size` 互换。**

| 项目 | [Inferact Block7](https://huggingface.co/Inferact/Kimi-K3-DSpark) | [Inferact Block5](https://huggingface.[REDACTED_SECRET]) |
|---|---:|---:|
| Revision | `cf6b8244…` | `b6d39a92…` |
| Draft token 数 | 7 | 5 |
| Draft 网络层数 | 5 层 | 3 层 |
| Attention mask | non-causal | full-causal |
| Target hidden layers | `[2,23,47,71,89]` | `[71,87,91]` |
| Attention heads | 64 | 96 |
| KV 结构 | MLA | MLA |
| KV latent | `512 + 64 RoPE = 576` | `512 + 64 RoPE = 576` |
| Markov rank | 256 | 512 |
| 权重大小 | 7.125 GB | 5.707 GB |
| Safetensors 张量数 | 68 | 44 |
| 训练 | TorchSpec，约 2 epochs | vLLM speculators，4 epochs，step 5780 |

关键结构差异：

- Block7 是 5 层 non-causal block-diffusion draft。
- Block5 是 3 层 full-causal MLA draft。
- Block5 不是从 Block7 简单截断两个 token。
- Block5 使用更靠后的 3 个目标 hidden states。
- Block5 的 96 个 attention heads 与 Block7 的 64 个不同。
- Block5 的 Markov head 更宽：512 对 256。
- 两者的 RoPE/YaRN 参数也不同。
- 因此权重张量形状不同，不能给 Block7 权重改一个 `block_size=5` 来替代 Block5。

两者 KV 的底层格式倒是一致的：都是每 token 576 元素的 MLA cache，即 512 维压缩 latent 加 64 维 RoPE key，所以不需要 GQA 那套独立 K/V page 格式。

性能取舍

相同 workload、`temperature=1.0/top_p=0.95` 下：

| Workload | Block7 AL | Block5 AL | Block7 草稿接受率 | Block5 草稿接受率 |
|---|---:|---:|---:|---:|
| GSM8K | 5.44 | 4.581 | 63.43% | 71.62% |
| HumanEval | 5.07 | 4.187 | 58.14% | 63.74% |
| MBPP | 4.31 | 3.819 | 47.29% | 56.38% |
| <redacted:ticket-id:045182748170> | 3.77 | 3.374 | 39.57% | 47.48% |
| SWE-bench Pro | 3.11 | 2.904 | 30.14% | 38.08% |
| MT-Bench | 3.06 | 2.659 | 29.43% | 33.18% |
| AIME26 | 2.64 | 2.489 | 23.43% | 29.78% |
| AA-LCR | 3.23 | 3.052 | 31.86% | 41.04% |

这里的关系很清楚：

- Block7 每轮尝试更多 token，所以绝对 acceptance length 更高。
- Block5 的草稿 token 接受比例更高。
- Block5 少 40% 的 draft layers，权重小约 20%，单次 draft 理论上更轻。
- Block7 更激进，适合 draft 计算足够快、希望一次接受更多 token 的场景。
- Block5 更保守，可能在 Ascend 上获得更好的“draft 开销/被接受 token”比例。

但不能仅凭模型卡断言 Block5 在四机 Ascend 上一定更快。Block7 和 Block5 的公开测试环境、TP 配置及 draft 结构不同，最终要在相同 target 权重、相同请求和相同图配置下 A/B 测：

- 端到端吞吐
- TPOT/ITL
- draft forward 时间
- target verify 时间
- acceptance length
- 每输出 token 的 draft 计算开销

另外，Block5 增加了 `full_attention_causal=true`、`dflash_config.causal=true`、`sample_from_anchor=true` 等语义。现有 Ascend DSpark 适配不能只把 `num_speculative_tokens` 从 7 改成 5，还要确认代码实际识别这些字段和 3 层/96-head 权重形状。



### user
检查一下远端权重是否有deepseekv4的权重


### assistant:commentary
我先在共享权重盘上做只读目录检索，覆盖常见命名：`DeepSeek-V4`、`DeepSeekV4`、`DSV4`、`V4-Flash` 等；如果找到候选，再读取其 `config.json`、文件数量和大小确认，不会启动服务或扫描模型大文件内容。由于四台共享 `/mnt/weight`，先在 `.153` 检索一次即可。

### assistant:commentary
已经找到 6 个明确的 DeepSeek-V4 候选目录（Flash、Pro，含 BF16/W8A8/W4A8）。我再核对每个目录的 `config.json`、权重索引、分片数和标称总大小，区分“目录存在”和“权重完整可用”。

### assistant:final_answer
有，远端 `/mnt/weight` 里确认有 7 份 DeepSeek-V4 权重：

| 路径 | 规格 | 分片 | 大小 |
|---|---|---:|---:|
| `DeepSeek-V4-Flash` | Flash，43 层 | 70 | 279.41 GiB |
| `DeepSeek-V4-Flash-0731-w8a8` | Flash W8A8 | 74 | 293.03 GiB |
| `DeepSeek-V4-Flash-w4a8-mtp` | Flash W4A8 + MTP | 38 | 150.34 GiB |
| `DeepSeek-V4-Flash-w8a8-mtp` | Flash W8A8 + MTP | 70 | 279.41 GiB |
| `DeepSeek-V4-Pro-w4a8-mtp` | Pro W4A8 + MTP，61 层 | 205 | 790.06 GiB |
| `dsv4pro0813` | Pro FP8，61 层 | 66 | 831.43 GiB |
| `dsv4pro0813w4a8` | Pro W4A8，61 层 | 213 | 817.35 GiB |

核对结果：

- 所有 `config.json` 都明确是 `DeepseekV4ForCausalLM` / `deepseek_v4`。
- 权重索引引用的分片全部存在，缺失分片为 0。
- 没发现 `.incomplete`、`.part` 或 `.tmp` 残留。
- 153–156 四台服务器的容器都能看到这 7 份权重。
- 本次只做了文件与索引完整性检查，没有启动模型做加载验证。



### user
看下服务器上都有哪些权重


### assistant:commentary
我先在共享的 `/mnt/weight` 上做只读盘点：列出一级权重目录、软链接和占用空间；再展开像 `k3/` 这种分类目录。这个盘四台共用，先从 153 容器读取即可，不会启动服务或改文件。

### assistant:commentary
一级目录里混有数据集、脚本、缓存和日志，不能全部当作权重。我正在按“目录内存在 `config.json` 且有 safetensors/bin 权重或索引”的标准重新筛一遍，并展开到三级，避免漏掉 `k3/`、`Eco-Tech/`、`MiniMax/` 下面的模型。

### user
不用这么细，告诉我有哪些模型就行


### assistant:final_answer
服务器共享权重盘上主要有这些模型：

- DeepSeek：V2 Lite、V3.1、V3.2、V4 Flash、V4 Pro
- Kimi：K2 Thinking、K2.5、K2.6、K2.7 Code、K3
- GLM：GLM-5、5.1、5.2、5.3 Flash
- Qwen：Qwen3、Qwen3-Next、Qwen3-VL、Qwen3-Omni、Qwen3.5、Qwen3.8
- MiniMax：M2.7、M3、H3
- Hunyuan：HunyuanImage 3、HY4 Preview
- Gemma 4 26B
- OpenPangu 2.0 Flash

另外还有不少 W4A8、W8A8、QuaRot、MTP 量化版本，以及 Eagle3、DFlash、DSpark 等投机模型。共识别到约 132 个有效权重目录。

