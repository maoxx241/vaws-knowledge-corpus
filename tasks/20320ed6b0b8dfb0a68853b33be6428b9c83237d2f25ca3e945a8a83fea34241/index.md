---
blocks:
- block_id: 137744d4cf0fccba58120c148f3a1114088fd34bc0f673cb0755d697cd95abcf
  indexed: true
  sha256: 4ec492fa7b21e2ecf3d4ad72bb51a2510f1bcfdb6a2bac41060b979f71f7c6f6
  source_range:
    end: 526678
    part: 0
    start: 0
  summary: 远端脚本实际为 `/mnt/weight/k3/download_0724.py`；直接连接 Hugging Face 曾超时或 TLS 重置，后改为本地下载并通过 tar-over-SSH 传输至独立目录 `/mnt/weight/k3/0724_update`，完成 20 个非 `.safetensors` 文件的 SHA-256 校验，原始 multicard 目录未改动。对比显示 11 个同名文件一致，`encoding_k3.py` 和 `config.json` 有内容变化，并新增 7 个源仓库文件；新配置声明 MXFP4，但未下载新权重分片，因此不能验证实际权重格式变化。旧 multicard 的 routed experts 被描述为 `W8A8_DYNAMIC`，不能据此得出“BF16 变 FP8”；新的 `model.safetensors.index.json` 与量化索引不兼容，不应替换原量化索引。
  title: 0724 更新文件下载与 multicard 差异核对
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 20320ed6b0b8dfb0a68853b33be6428b9c83237d2f25ca3e945a8a83fea34241
  kind: experience
  material_digest: 12a5cc69c0e63a9860f076758eb4cdca8fb3ee3e7e856a606b0786dcf86b51ff
  revision: 25b4f1a81560b7586b9c4e27ad3b7fb381f64ddbcc3fd66ea4ceadf86249f599
  schema: mindie-entry/3
  summary: 当前可检索重点包括：脚本路径纠正为 `/mnt/weight/k3/download_0724.py`；网络失败后采用本地下载、tar-over-SSH 传输；独立目录已写入并校验 20 个文件，原始权重未覆盖。已观察到 `encoding_k3.py`、`config.json`、索引和量化元数据差异，但“路由专家从 BF16 变为 FP8”未被证实；新权重分片未下载，索引兼容性仍是风险点。
  title: 远端 K3 非 safetensors 更新与差异追踪
navigation: 当前可检索重点包括：脚本路径纠正为 `/mnt/weight/k3/download_0724.py`；网络失败后采用本地下载、tar-over-SSH 传输；独立目录已写入并校验 20 个文件，原始权重未覆盖。已观察到 `encoding_k3.py`、`config.json`、索引和量化元数据差异，但“路由专家从 BF16 变为 FP8”未被证实；新权重分片未下载，索引兼容性仍是风险点。
schema: mindie-material-task/1
status: complete
task_id: 20320ed6b0b8dfb0a68853b33be6428b9c83237d2f25ca3e945a8a83fea34241
---

# 远端 K3 非 safetensors 更新与差异追踪

当前可检索重点包括：脚本路径纠正为 `/mnt/weight/k3/download_0724.py`；网络失败后采用本地下载、tar-over-SSH 传输；独立目录已写入并校验 20 个文件，原始权重未覆盖。已观察到 `encoding_k3.py`、`config.json`、索引和量化元数据差异，但“路由专家从 BF16 变为 FP8”未被证实；新权重分片未下载，索引兼容性仍是风险点。

## Materials

- [0724 更新文件下载与 multicard 差异核对](blocks/137744d4cf0fccba58120c148f3a1114088fd34bc0f673cb0755d697cd95abcf.md): 远端脚本实际为 `/mnt/weight/k3/download_0724.py`；直接连接 Hugging Face 曾超时或 TLS 重置，后改为本地下载并通过 tar-over-SSH 传输至独立目录 `/mnt/weight/k3/0724_update`，完成 20 个非 `.safetensors` 文件的 SHA-256 校验，原始 multicard 目录未改动。对比显示 11 个同名文件一致，`encoding_k3.py` 和 `config.json` 有内容变化，并新增 7 个源仓库文件；新配置声明 MXFP4，但未下载新权重分片，因此不能验证实际权重格式变化。旧 multicard 的 routed experts 被描述为 `W8A8_DYNAMIC`，不能据此得出“BF16 变 FP8”；新的 `model.safetensors.index.json` 与量化索引不兼容，不应替换原量化索引。
