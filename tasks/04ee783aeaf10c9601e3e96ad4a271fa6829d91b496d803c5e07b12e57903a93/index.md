---
blocks:
- block_id: 66f39202e2c50adf860775e38816d46581dc5d7f8acec00ff572581271b6b2ee
  indexed: true
  sha256: 41bbd0c95c6300a4c2c6d35bacfcd6471ced73866682d1342e3464a9a44dffab
  source_range:
    end: 693698
    part: 0
    start: 0
  summary: 记录显示，向四台经脱敏地址的 root 服务器追加 SSH 公钥，并报告公钥各出现一次、原有内容保留，`.ssh` 权限为 700、`authorized_keys` 权限为 600；后续状态被报告为 `already_present`。主机名、known_hosts 条目和指纹均以脱敏或占位形式提供，MacBook 端免密登录仍待本机独立验证。任务间回传多次出现 `DeviceCheck registration failed (403)`，据称另存了 `/tmp/macbook-a3-ssh-bootstrap-result.json`；这些成功与文件保存结果属于消息中的报告，不能视为本索引独立验证。
  title: 四台服务器 SSH 公钥配置及回传失败
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 04ee783aeaf10c9601e3e96ad4a271fa6829d91b496d803c5e07b12e57903a93
  kind: experience
  material_digest: 87522d46edde151bc88eee1f32e8206d1854527a2f462624730c906090be9352
  revision: b130731bf901bd1ed0f65bc22282323a8940bf0ab73a1c3db2627291a62b72e8
  schema: mindie-entry/3
  summary: 当前材料集中于四台脱敏服务器的 root SSH 公钥追加、重复检查、权限（700/600）和主机密钥整理。后续仍需区分已报告的服务器端结果与未完成的 MacBook 本机免密验证，并保留 `DeviceCheck registration failed (403)` 回传失败及备用 JSON 文件的未独立核验状态。
  title: SSH 公钥配置任务索引
navigation: 当前材料集中于四台脱敏服务器的 root SSH 公钥追加、重复检查、权限（700/600）和主机密钥整理。后续仍需区分已报告的服务器端结果与未完成的 MacBook 本机免密验证，并保留 `DeviceCheck registration failed (403)` 回传失败及备用 JSON 文件的未独立核验状态。
schema: mindie-material-task/1
status: complete
task_id: 04ee783aeaf10c9601e3e96ad4a271fa6829d91b496d803c5e07b12e57903a93
---

# SSH 公钥配置任务索引

当前材料集中于四台脱敏服务器的 root SSH 公钥追加、重复检查、权限（700/600）和主机密钥整理。后续仍需区分已报告的服务器端结果与未完成的 MacBook 本机免密验证，并保留 `DeviceCheck registration failed (403)` 回传失败及备用 JSON 文件的未独立核验状态。

## Materials

- [四台服务器 SSH 公钥配置及回传失败](blocks/66f39202e2c50adf860775e38816d46581dc5d7f8acec00ff572581271b6b2ee.md): 记录显示，向四台经脱敏地址的 root 服务器追加 SSH 公钥，并报告公钥各出现一次、原有内容保留，`.ssh` 权限为 700、`authorized_keys` 权限为 600；后续状态被报告为 `already_present`。主机名、known_hosts 条目和指纹均以脱敏或占位形式提供，MacBook 端免密登录仍待本机独立验证。任务间回传多次出现 `DeviceCheck registration failed (403)`，据称另存了 `/tmp/macbook-a3-ssh-bootstrap-result.json`；这些成功与文件保存结果属于消息中的报告，不能视为本索引独立验证。
