---
blocks:
- block_id: 6998fd1ddc3609b491b36d2ebe8675d4d6379a27212178a9f4ca2d691fbfb837
  indexed: true
  sha256: 21c22b65e1745a3904162e66e3081e69439981d0b48acdddaa14ed5c5157de22
  source_range:
    end: 1656863
    part: 0
    start: 0
  summary: 已完成 cached-source DFX 预案，尚未部署、启动服务或占用 NPU，远端源码未修改；方案聚焦 Mamba preprocess、copy 后/target logits 与 DSpark aux 实体化，默认关闭时不执行 device clone、同步、D2H 或文件操作。预案记录了 69 个 KDA 层、24 个 target MLA 层及上下文/状态索引等落盘字段，并要求 DP0/TP0、request ID 与 prompt-token hash 精确匹配。已通过本地语法编译、diff、SHA 校验及纯 CPU 合约验证；新增 5 个 pytest UT，但因本机无 Torch，完整 pytest 尚未直接执行，相关结果仍非已验证成功。
  title: cached-source DFX 预案与合约验证
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 9b13be7a151fac7db6bda3822e349512f948e8f70a61f65e1639690b053fd0a8
  kind: experience
  material_digest: a18b649d0a0367223f5ec349c120c087f45a6fce224ea11f0396f64f3e17e7e7
  revision: 9c43ec7e7aa92f1db7f455c1a808510f7ed6f9c602e5092c78c191b328453772
  schema: mindie-entry/3
  summary: 本批次包含 1 个新材料块，主题为未部署的 cached-source DFX 预案、运行时钩子、manifest 判据及 CPU 合约验证。关键检索词：Mamba preprocess、DSpark aux、target MLA、cached col0、copy 后 col2、SHA256、BF16、DP0/TP0、prompt-token hash、device clone、D2H、pytest、Torch 缺失；需保留“未部署”与完整 pytest 尚未执行等不确定性。原始材料块仍可独立检索。
  title: 当前任务导航：cached-source DFX 检索索引
navigation: 本批次包含 1 个新材料块，主题为未部署的 cached-source DFX 预案、运行时钩子、manifest 判据及 CPU 合约验证。关键检索词：Mamba preprocess、DSpark aux、target MLA、cached col0、copy 后 col2、SHA256、BF16、DP0/TP0、prompt-token hash、device clone、D2H、pytest、Torch 缺失；需保留“未部署”与完整 pytest 尚未执行等不确定性。原始材料块仍可独立检索。
schema: mindie-material-task/1
status: complete
task_id: 9b13be7a151fac7db6bda3822e349512f948e8f70a61f65e1639690b053fd0a8
---

# 当前任务导航：cached-source DFX 检索索引

本批次包含 1 个新材料块，主题为未部署的 cached-source DFX 预案、运行时钩子、manifest 判据及 CPU 合约验证。关键检索词：Mamba preprocess、DSpark aux、target MLA、cached col0、copy 后 col2、SHA256、BF16、DP0/TP0、prompt-token hash、device clone、D2H、pytest、Torch 缺失；需保留“未部署”与完整 pytest 尚未执行等不确定性。原始材料块仍可独立检索。

## Materials

- [cached-source DFX 预案与合约验证](blocks/6998fd1ddc3609b491b36d2ebe8675d4d6379a27212178a9f4ca2d691fbfb837.md): 已完成 cached-source DFX 预案，尚未部署、启动服务或占用 NPU，远端源码未修改；方案聚焦 Mamba preprocess、copy 后/target logits 与 DSpark aux 实体化，默认关闭时不执行 device clone、同步、D2H 或文件操作。预案记录了 69 个 KDA 层、24 个 target MLA 层及上下文/状态索引等落盘字段，并要求 DP0/TP0、request ID 与 prompt-token hash 精确匹配。已通过本地语法编译、diff、SHA 校验及纯 CPU 合约验证；新增 5 个 pytest UT，但因本机无 Torch，完整 pytest 尚未直接执行，相关结果仍非已验证成功。
