---
blocks:
- block_id: 82fa6f917579914daa00fa87d488b0a7bdb77e22378ea2bddef7099a48b9e816
  indexed: true
  sha256: 0d484c5b71e64b5dde9ed0948ac944fd5dee41978b048872904d3036e17f3f9b
  source_range:
    end: 18723157
    part: 0
    start: 0
  summary: '围绕PR #14768进行了0.26/0.27单机TP16、EP16的QuaRot与GQA DSpark A/B测试；在同一prompt和权重条件下，双方曾得到相同输出及885/987、89.67%的采信率，但153最初因core与插件revision不匹配出现`num_prefill_lookahead=None`，后改用历史core。测试还确认compact materialized GQA路径优于额外硬套`prev_valid_blocks`的失败实验，后者曾使采信表现降至约3.x；PR标题已修正，rebase后的本地head和推送状态存在多次变更，最终曾暂停force-push。随后有人指出测试权重可能使用了修复quant description后的descriptor，原始descriptor下旋转量化+GQA DSpark采信率是否为0尚未核实，因此此前高采信率A/B不能代表未修descriptor现场；既有导航仍记录双机DP2冷请求22.6190%、历史最高45.4810%，0.27双机对照未完成。'
  title: PR14768旋转量化与GQA DSpark采信率证据及descriptor疑点
- block_id: d9f278af3133af60b8ec3558d4e9d47ae42c3a25ec1dce1d1b19793af462822e
  indexed: true
  sha256: d51941474e7ee2604b41b6e99fe20a77f3d41d02a760ccd237f4f602fbb5dd87
  source_range:
    end: 22040429
    part: 0
    start: 18723157
  summary: 已确认修正前 `quant_model_description.json` 的 `optional={}` 会导致 Qwen loader 未处理 FC 旋转，正确结构应指向 `optional/quarot.safetensors`；单机 TP16 的 0.27 与 0.26+#14768 曾测得 `885/987=89.6657%`，但该结果不能外推到双机。双机 DP2 的 0.26+#14768 冷请求测得 `627/2772=22.6190%`（HTTP 200、输出质量检查通过），历史叠加未推送补丁并预热前缀的最好结果为 `780/1715=45.4810%`，均不正常；0.27 双机尚未完成同条件对照，不能判定。一次精确 head 的复测因端口 `18527` 既有服务占用 HBM、剩余约 `4.3–4.8 GiB` 而启动失败，该失败不应视为采信率结论。
  title: QuaRot descriptor 修复与 0.26/0.27 双机采信率核验
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 46547cc02500be56e7b635a086b7199ad0724c475c468044a4444f894a178404
  kind: experience
  material_digest: 3f444a8ae4a8aecceea75210ea4ccde56f524611b2a2189ce4e3d855ac7a68c0
  revision: 443687cb5b2d20530b82fced907570156b85c5b15bf259deb1c44d696cb2147d
  schema: mindie-entry/3
  summary: 已有单机TP16证据显示，使用可能已修复quant description的QuaRot权重时，0.26+#14768与匹配runtime的0.27可达到相同高采信率；但这不等于原始descriptor、双机DP2或用户反馈的0采信率问题已解决。下一步应保留失败尝试和不确定性，核对目标权重descriptor、mtime及备份与完整0726 QuaRot权重，再在不叠加未推送补丁的资源环境中补测双机DP2；当前导航中的22.6190%冷请求、45.4810%历史结果及0.27双机未完成状态仍有效。
  title: 当前任务导航：先核实descriptor，再区分单机与双机采信率
navigation: 已有单机TP16证据显示，使用可能已修复quant description的QuaRot权重时，0.26+#14768与匹配runtime的0.27可达到相同高采信率；但这不等于原始descriptor、双机DP2或用户反馈的0采信率问题已解决。下一步应保留失败尝试和不确定性，核对目标权重descriptor、mtime及备份与完整0726 QuaRot权重，再在不叠加未推送补丁的资源环境中补测双机DP2；当前导航中的22.6190%冷请求、45.4810%历史结果及0.27双机未完成状态仍有效。
schema: mindie-material-task/1
status: complete
task_id: 46547cc02500be56e7b635a086b7199ad0724c475c468044a4444f894a178404
---

# 当前任务导航：先核实descriptor，再区分单机与双机采信率

已有单机TP16证据显示，使用可能已修复quant description的QuaRot权重时，0.26+#14768与匹配runtime的0.27可达到相同高采信率；但这不等于原始descriptor、双机DP2或用户反馈的0采信率问题已解决。下一步应保留失败尝试和不确定性，核对目标权重descriptor、mtime及备份与完整0726 QuaRot权重，再在不叠加未推送补丁的资源环境中补测双机DP2；当前导航中的22.6190%冷请求、45.4810%历史结果及0.27双机未完成状态仍有效。

## Materials

- [PR14768旋转量化与GQA DSpark采信率证据及descriptor疑点](blocks/82fa6f917579914daa00fa87d488b0a7bdb77e22378ea2bddef7099a48b9e816.md): 围绕PR #14768进行了0.26/0.27单机TP16、EP16的QuaRot与GQA DSpark A/B测试；在同一prompt和权重条件下，双方曾得到相同输出及885/987、89.67%的采信率，但153最初因core与插件revision不匹配出现`num_prefill_lookahead=None`，后改用历史core。测试还确认compact materialized GQA路径优于额外硬套`prev_valid_blocks`的失败实验，后者曾使采信表现降至约3.x；PR标题已修正，rebase后的本地head和推送状态存在多次变更，最终曾暂停force-push。随后有人指出测试权重可能使用了修复quant description后的descriptor，原始descriptor下旋转量化+GQA DSpark采信率是否为0尚未核实，因此此前高采信率A/B不能代表未修descriptor现场；既有导航仍记录双机DP2冷请求22.6190%、历史最高45.4810%，0.27双机对照未完成。
- [QuaRot descriptor 修复与 0.26/0.27 双机采信率核验](blocks/d9f278af3133af60b8ec3558d4e9d47ae42c3a25ec1dce1d1b19793af462822e.md): 已确认修正前 `quant_model_description.json` 的 `optional={}` 会导致 Qwen loader 未处理 FC 旋转，正确结构应指向 `optional/quarot.safetensors`；单机 TP16 的 0.27 与 0.26+#14768 曾测得 `885/987=89.6657%`，但该结果不能外推到双机。双机 DP2 的 0.26+#14768 冷请求测得 `627/2772=22.6190%`（HTTP 200、输出质量检查通过），历史叠加未推送补丁并预热前缀的最好结果为 `780/1715=45.4810%`，均不正常；0.27 双机尚未完成同条件对照，不能判定。一次精确 head 的复测因端口 `18527` 既有服务占用 HBM、剩余约 `4.3–4.8 GiB` 而启动失败，该失败不应视为采信率结论。
