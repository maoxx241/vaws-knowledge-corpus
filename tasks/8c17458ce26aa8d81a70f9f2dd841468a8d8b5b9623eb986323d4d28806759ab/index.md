---
blocks:
- block_id: 62725afa62542f4ec76feff0bcbadc27faf208fc4080d51b1f27113c2920cb40
  indexed: true
  sha256: de1325d21ce5fe3aaa87ef9c45270cc0c15ab1e55d2da93024be26a378b26c60
  source_range:
    end: 16850720
    part: 0
    start: 0
  summary: work-mini 上 UniVPN 直连 NPU 成功，但 GUI/Network Extension 版 Tailscale 的出站连接曾从物理接口发出并超时；后续抓包确认 SYN-ACK 已到达，而 Little Snitch 内容过滤器将 SSH banner 留在队列中，导致连接交接未完成。独立 Go、C 程序也复现，Python 和 curl 正常；sudo/PF、Little Snitch CLI 与锁屏限制造成部分证据和操作缺口。期间 sidecar、/32 与 /24 路由及临时 SSH 通道被建立或验证，代码验收从 100/119 项通过、2 跳过推进到后续 134 项通过、2 跳过，但此前测试失败和迁移风险仍需保留。
  title: UniVPN/Tailscale 转发故障与 Little Snitch 阻塞定位
- block_id: 0bd0a85412214e2a0c952a0368e416f18153ffed5550d122fb7f8315f255f993
  indexed: true
  sha256: e3382a3552819485a1bacf7cb7d38dcec2c66f6161a2a20246e2bc485965056c
  source_range:
    end: 16850720
    part: 1
    start: 0
  summary: 'MBP 端最终验收确认 core/tray 为 1.5.7、sidecar /24 生效，两台 NPU 的 SSH banner 与指纹通过，临时 SSH 监听及授权也已清理；测试记录为 132 通过、2 跳过，另一次独立复核报告 134 通过、2 跳过。Grok 修复了实例迁移先撤旧广告导致丢路由的问题，并将合法的 AdvertiseRoutes: null 视为空列表，同时加入失败回滚；这些修改已复核，但正式合并、打 tag、发布和 PR 仍未完成。Little Snitch 全局关闭没有成功，界面持续超时、报错或显示 Enabled，需用户在本机正常设置界面手动处理。'
  title: 修复部署验收与 Little Snitch 未完成关闭
- block_id: 22792ff1bc8559bba1186119b8ad3f9479ea2d2d7a0dacd0f4c04f1666e528fa
  indexed: true
  sha256: 0a4affd3435327ca11ff3da24f45b655e0ec30fa3c287b4425ca518fdd5324cf
  source_range:
    end: 33649030
    part: 0
    start: 16850720
  summary: '围绕 sidecar 托管、路由批准语义、实例归属与清理、登录隔离、状态文件并发写入、安装路径和特权 API 转发，多个 WIP 快照已推送到草稿 PR #3；期间保留了 Linux systemd 路径测试失败、状态竞态、初始化错误、弹窗拦截和真实服务文件保护等失败与不确定结果。独立测试曾确认网段及高级设置跨刷新保留、弹窗拦截时登录链接可用，并发现登录探测会错误恢复已关闭的 CorpDNS；后续浏览器检查称网段初始化错误已修复且相关检查通过，但原生窗口验收、最终合并、发布和正式部署仍未完成。生产中的 1.5.7 实例据报告未被测试修改，桌面操作还受锁屏及 cmux 进程来源限制影响。'
  title: UniVPN-ng 托管实例与界面修订进展
- block_id: 0278d70310702834a91d7a3ce4ca1667d51aff757e0a718167cf1914a574d69e
  indexed: true
  sha256: 77af9dbeec8075e3fd056fd80965ec548250cd3a99295621d44907f590776ea1
  source_range:
    end: 38199942
    part: 0
    start: 33649030
  summary: '材料记录 UniVPN-ng 从候选提交 `3bd5890`、222 项测试（220 通过、2 跳过）和三平台构建，推进到 1.6.0 部署、分享开关及 VPN 重连验证；其中重连后的远端 TCP/SSH、路由、NAT、节点身份和指纹被报告为通过，临时 HTTP 9560、guard 9562、21876 监听也被报告已清理。随后材料声称 v1.6.0 已发布、PR #3 已合并并推送标签，但 Little Snitch 全局 Off 仍明确未验证；四台机器的 hostname、指定公钥授权状态及 Ed25519 公钥/指纹完成了只读核查，私钥未读取，地址和密钥内容保留为脱敏占位符。'
  title: 1.6.0 发布记录与四机 SSH 只读核查
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 8c17458ce26aa8d81a70f9f2dd841468a8d8b5b9623eb986323d4d28806759ab
  kind: experience
  material_digest: 6d461ce709317c71521286e43c11975ce27958f895ba23f34b7ff98fe1a8c820
  revision: e2541f70960909ec295a5ce7a85d6bf9c4beaf810c538b611ca2b04b9f3f86b6
  schema: mindie-entry/3
  summary: '此前关于 sidecar 阻塞、路由和 NPU SSH 访问恢复，以及草稿 PR #3 中各项修复和遗留风险的上下文仍 relevant。新增材料记录了 `3bd5890`、三平台构建、1.6.0 部署与发布、分享开关和 VPN 重连检查，以及四台机器的 SSH 授权与主机公钥只读核查；这些是材料中的报告，不应替代独立验证。Little Snitch 全局 Off 仍未验证，早先的状态竞态、平台隔离、CorpDNS 恢复、后台维护边界等风险也未因本批叙述自动消失；原始块仍可独立检索，且未提供或不应重建私密路径、地址和凭据。'
  title: UniVPN-ng 1.6.0 已据材料发布，后续核查与未验证项仍需区分
navigation: '此前关于 sidecar 阻塞、路由和 NPU SSH 访问恢复，以及草稿 PR #3 中各项修复和遗留风险的上下文仍 relevant。新增材料记录了 `3bd5890`、三平台构建、1.6.0 部署与发布、分享开关和 VPN 重连检查，以及四台机器的 SSH 授权与主机公钥只读核查；这些是材料中的报告，不应替代独立验证。Little Snitch 全局 Off 仍未验证，早先的状态竞态、平台隔离、CorpDNS 恢复、后台维护边界等风险也未因本批叙述自动消失；原始块仍可独立检索，且未提供或不应重建私密路径、地址和凭据。'
schema: mindie-material-task/1
status: complete
task_id: 8c17458ce26aa8d81a70f9f2dd841468a8d8b5b9623eb986323d4d28806759ab
---

# UniVPN-ng 1.6.0 已据材料发布，后续核查与未验证项仍需区分

此前关于 sidecar 阻塞、路由和 NPU SSH 访问恢复，以及草稿 PR #3 中各项修复和遗留风险的上下文仍 relevant。新增材料记录了 `3bd5890`、三平台构建、1.6.0 部署与发布、分享开关和 VPN 重连检查，以及四台机器的 SSH 授权与主机公钥只读核查；这些是材料中的报告，不应替代独立验证。Little Snitch 全局 Off 仍未验证，早先的状态竞态、平台隔离、CorpDNS 恢复、后台维护边界等风险也未因本批叙述自动消失；原始块仍可独立检索，且未提供或不应重建私密路径、地址和凭据。

## Materials

- [UniVPN/Tailscale 转发故障与 Little Snitch 阻塞定位](blocks/62725afa62542f4ec76feff0bcbadc27faf208fc4080d51b1f27113c2920cb40.md): work-mini 上 UniVPN 直连 NPU 成功，但 GUI/Network Extension 版 Tailscale 的出站连接曾从物理接口发出并超时；后续抓包确认 SYN-ACK 已到达，而 Little Snitch 内容过滤器将 SSH banner 留在队列中，导致连接交接未完成。独立 Go、C 程序也复现，Python 和 curl 正常；sudo/PF、Little Snitch CLI 与锁屏限制造成部分证据和操作缺口。期间 sidecar、/32 与 /24 路由及临时 SSH 通道被建立或验证，代码验收从 100/119 项通过、2 跳过推进到后续 134 项通过、2 跳过，但此前测试失败和迁移风险仍需保留。
- [修复部署验收与 Little Snitch 未完成关闭](blocks/0bd0a85412214e2a0c952a0368e416f18153ffed5550d122fb7f8315f255f993.md): MBP 端最终验收确认 core/tray 为 1.5.7、sidecar /24 生效，两台 NPU 的 SSH banner 与指纹通过，临时 SSH 监听及授权也已清理；测试记录为 132 通过、2 跳过，另一次独立复核报告 134 通过、2 跳过。Grok 修复了实例迁移先撤旧广告导致丢路由的问题，并将合法的 AdvertiseRoutes: null 视为空列表，同时加入失败回滚；这些修改已复核，但正式合并、打 tag、发布和 PR 仍未完成。Little Snitch 全局关闭没有成功，界面持续超时、报错或显示 Enabled，需用户在本机正常设置界面手动处理。
- [UniVPN-ng 托管实例与界面修订进展](blocks/22792ff1bc8559bba1186119b8ad3f9479ea2d2d7a0dacd0f4c04f1666e528fa.md): 围绕 sidecar 托管、路由批准语义、实例归属与清理、登录隔离、状态文件并发写入、安装路径和特权 API 转发，多个 WIP 快照已推送到草稿 PR #3；期间保留了 Linux systemd 路径测试失败、状态竞态、初始化错误、弹窗拦截和真实服务文件保护等失败与不确定结果。独立测试曾确认网段及高级设置跨刷新保留、弹窗拦截时登录链接可用，并发现登录探测会错误恢复已关闭的 CorpDNS；后续浏览器检查称网段初始化错误已修复且相关检查通过，但原生窗口验收、最终合并、发布和正式部署仍未完成。生产中的 1.5.7 实例据报告未被测试修改，桌面操作还受锁屏及 cmux 进程来源限制影响。
- [1.6.0 发布记录与四机 SSH 只读核查](blocks/0278d70310702834a91d7a3ce4ca1667d51aff757e0a718167cf1914a574d69e.md): 材料记录 UniVPN-ng 从候选提交 `3bd5890`、222 项测试（220 通过、2 跳过）和三平台构建，推进到 1.6.0 部署、分享开关及 VPN 重连验证；其中重连后的远端 TCP/SSH、路由、NAT、节点身份和指纹被报告为通过，临时 HTTP 9560、guard 9562、21876 监听也被报告已清理。随后材料声称 v1.6.0 已发布、PR #3 已合并并推送标签，但 Little Snitch 全局 Off 仍明确未验证；四台机器的 hostname、指定公钥授权状态及 Ed25519 公钥/指纹完成了只读核查，私钥未读取，地址和密钥内容保留为脱敏占位符。
