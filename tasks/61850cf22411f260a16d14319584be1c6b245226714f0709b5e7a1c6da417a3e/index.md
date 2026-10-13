---
blocks:
- block_id: d3f7444090d51c1f3c4bacfdb52f6a0b29f6fbddf8d1d646b81afc0ed6ac1e71
  indexed: true
  sha256: c8c832de4482c5f6057b8bca241c75b017aad671818e669f2394ccdcaca71ef6
  source_range:
    end: 7537489
    part: 0
    start: 0
  summary: 审计认为K3适配存在全局猴子补丁、请求原地改写、Python侧冗余contiguous、动态属性、Optional/assert拼装、霰弹式修改及源码扫描型UT等问题，建议按上游模型契约、Ascend后端、typed execution contract和单一注册入口重构。明确范围包括Python适配层、模型/组网、测试及torch binding/Meta；ACLNN算子目录不改，Meta仅做符号shape/dtype推导。部分低风险改动已完成，但pytest因环境缺少torch未获得运行时结果；后续审查还发现通信边界非连续tensor、注册幂等性、binding安全边界和Meta潜在未定义行为等未决风险。
  title: K3整体架构与测试审计
- block_id: 306a4118747974a5ea3a019fb89da69b6a6a560cff3711855ae8d438497456fc
  indexed: true
  sha256: 0949d250346fd589b7a3aa0faccfee9c364fc74344e7f0ba70353923a9ea9c13
  source_range:
    end: 7537489
    part: 1
    start: 0
  summary: 审计发现MXFP4配置路由与W4A16/W4A8实现及UT相互矛盾、SiTU被伪装成SiLU、FusedMC2在forward阶段fallback、shared expert存在重复实现和hasattr分支，ModelSlim还通过临时模块与模型身份补丁规避问题。建议统一MoE activation类型和execution plan，把能力选择前移到构造/初始化阶段，并将权重与scale布局归一化移至加载期；MXFP packed归类必须由真实checkpoint和NPU验证决定，不能直接假设。已记录beta fallback、activation类型、Meta校验和部分UT清理等低风险完成项，但pytest/NPU均未运行，多个高风险路径仍只是建议。
  title: MoE、量化与测试清理审计
- block_id: 38b86168430016f91a751d31b377b3805ec90834c77338101aa7871da28e23e9
  indexed: true
  sha256: 8b730f546f54850ab607ceaabf62f5f75ceec7b3d1abfefa3ab75ef3c0c30115
  source_range:
    end: 7537489
    part: 2
    start: 0
  summary: 新增custom-op测试覆盖dispatcher schema、Meta注册、mutable alias、FakeTensor及符号shape/dtype，并移除若干源码扫描和缺失算子时的skip；KDA、NPU测试和Meta实现也有相应清理。交叉审查指出通信侧恢复contiguous、GDN metadata类型贯穿、注册入口真正幂等、MoE全局单例污染，以及torch binding输出分配前边界检查和Meta除零/越界/空vector访问仍需处理。静态检查和编译检查多次通过，但本机缺少PyTorch、torch_npu、vllm或pytest，未完成真实编译、pytest或NPU验证；ACLNN目录保持未修改。
  title: Binding/Meta重构与交叉审查
- block_id: f4882db0296c6815d32f16e67cae84f6bd6f93bce10220d3baf83c9582103230
  indexed: true
  sha256: d972ffbc918c794e7ef26f8f0644cdb588502132ea5141dc0b7275347e1cac2f
  source_range:
    end: 7537489
    part: 3
    start: 0
  summary: '材料记录了K3 KDA/GDN、SiTU/MoE、量化及 serving 注册重构的多轮收口：packed W4A8 仅用于 MoE，Linear 改用标准构造，并补充了量化、注册所有权、Meta dispatcher 和非连续输入测试；独立 PR #13/#17 文件及 ACLNN host/kernel 目录保持零 diff。静态检查包括 Ruff、格式、AST/py_compile、codespell、typos 和 git diff --check，均被报告通过，但 pytest 未运行或环境缺少 pytest，且 torch、扩展编译、NPU 数值验证及四机服务未执行。早期审查曾指出导入顺序、注册半失败状态、配置重复写入、aggregate reload 幂等性和 GDN 强制 cast 等风险；后续材料声称已通过预检查集中注册和 vLLM 配置表接管 AutoConfig 等方式收紧，但这些结论仍主要是静态收口，不能视为运行时验证成功。'
  title: K3重构收口、注册一致性与验证边界
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 61850cf22411f260a16d14319584be1c6b245226714f0709b5e7a1c6da417a3e
  kind: experience
  material_digest: 2830f7782586876736b7d29d0aacd2952003e50bc24b00d585da117876e5f9a5
  revision: 65b9df01278e8632ae1856c57c66a443475b533fa296e217ee2cb6f0452c2406
  schema: mindie-entry/3
  summary: 当前材料覆盖 K3 KDA/GDN、SiTU/MoE、量化、serving 注册、torch binding/Meta 路径及变更边界；报告的静态检查大多通过，packed W4A8/Linear 构造、注册预检查、Meta 输出推导和独立文件零 diff 等修订已被记录，但此前发现的注册幂等性、配置所有权、GDN typed contract 等风险应保留为审计背景。pytest、torch 相关测试、扩展编译、NPU 数值行为、HCCL 非连续通信及真实服务验证仍未完成或未被材料证明；所有“完成”结论均应视为未独立验证的报告，而非 verified success。
  title: K3重构静态收口与运行时验证未决导航
navigation: 当前材料覆盖 K3 KDA/GDN、SiTU/MoE、量化、serving 注册、torch binding/Meta 路径及变更边界；报告的静态检查大多通过，packed W4A8/Linear 构造、注册预检查、Meta 输出推导和独立文件零 diff 等修订已被记录，但此前发现的注册幂等性、配置所有权、GDN typed contract 等风险应保留为审计背景。pytest、torch 相关测试、扩展编译、NPU 数值行为、HCCL 非连续通信及真实服务验证仍未完成或未被材料证明；所有“完成”结论均应视为未独立验证的报告，而非 verified success。
schema: mindie-material-task/1
status: complete
task_id: 61850cf22411f260a16d14319584be1c6b245226714f0709b5e7a1c6da417a3e
---

# K3重构静态收口与运行时验证未决导航

当前材料覆盖 K3 KDA/GDN、SiTU/MoE、量化、serving 注册、torch binding/Meta 路径及变更边界；报告的静态检查大多通过，packed W4A8/Linear 构造、注册预检查、Meta 输出推导和独立文件零 diff 等修订已被记录，但此前发现的注册幂等性、配置所有权、GDN typed contract 等风险应保留为审计背景。pytest、torch 相关测试、扩展编译、NPU 数值行为、HCCL 非连续通信及真实服务验证仍未完成或未被材料证明；所有“完成”结论均应视为未独立验证的报告，而非 verified success。

## Materials

- [K3整体架构与测试审计](blocks/d3f7444090d51c1f3c4bacfdb52f6a0b29f6fbddf8d1d646b81afc0ed6ac1e71.md): 审计认为K3适配存在全局猴子补丁、请求原地改写、Python侧冗余contiguous、动态属性、Optional/assert拼装、霰弹式修改及源码扫描型UT等问题，建议按上游模型契约、Ascend后端、typed execution contract和单一注册入口重构。明确范围包括Python适配层、模型/组网、测试及torch binding/Meta；ACLNN算子目录不改，Meta仅做符号shape/dtype推导。部分低风险改动已完成，但pytest因环境缺少torch未获得运行时结果；后续审查还发现通信边界非连续tensor、注册幂等性、binding安全边界和Meta潜在未定义行为等未决风险。
- [MoE、量化与测试清理审计](blocks/306a4118747974a5ea3a019fb89da69b6a6a560cff3711855ae8d438497456fc.md): 审计发现MXFP4配置路由与W4A16/W4A8实现及UT相互矛盾、SiTU被伪装成SiLU、FusedMC2在forward阶段fallback、shared expert存在重复实现和hasattr分支，ModelSlim还通过临时模块与模型身份补丁规避问题。建议统一MoE activation类型和execution plan，把能力选择前移到构造/初始化阶段，并将权重与scale布局归一化移至加载期；MXFP packed归类必须由真实checkpoint和NPU验证决定，不能直接假设。已记录beta fallback、activation类型、Meta校验和部分UT清理等低风险完成项，但pytest/NPU均未运行，多个高风险路径仍只是建议。
- [Binding/Meta重构与交叉审查](blocks/38b86168430016f91a751d31b377b3805ec90834c77338101aa7871da28e23e9.md): 新增custom-op测试覆盖dispatcher schema、Meta注册、mutable alias、FakeTensor及符号shape/dtype，并移除若干源码扫描和缺失算子时的skip；KDA、NPU测试和Meta实现也有相应清理。交叉审查指出通信侧恢复contiguous、GDN metadata类型贯穿、注册入口真正幂等、MoE全局单例污染，以及torch binding输出分配前边界检查和Meta除零/越界/空vector访问仍需处理。静态检查和编译检查多次通过，但本机缺少PyTorch、torch_npu、vllm或pytest，未完成真实编译、pytest或NPU验证；ACLNN目录保持未修改。
- [K3重构收口、注册一致性与验证边界](blocks/f4882db0296c6815d32f16e67cae84f6bd6f93bce10220d3baf83c9582103230.md): 材料记录了K3 KDA/GDN、SiTU/MoE、量化及 serving 注册重构的多轮收口：packed W4A8 仅用于 MoE，Linear 改用标准构造，并补充了量化、注册所有权、Meta dispatcher 和非连续输入测试；独立 PR #13/#17 文件及 ACLNN host/kernel 目录保持零 diff。静态检查包括 Ruff、格式、AST/py_compile、codespell、typos 和 git diff --check，均被报告通过，但 pytest 未运行或环境缺少 pytest，且 torch、扩展编译、NPU 数值验证及四机服务未执行。早期审查曾指出导入顺序、注册半失败状态、配置重复写入、aggregate reload 幂等性和 GDN 强制 cast 等风险；后续材料声称已通过预检查集中注册和 vLLM 配置表接管 AutoConfig 等方式收紧，但这些结论仍主要是静态收口，不能视为运行时验证成功。
