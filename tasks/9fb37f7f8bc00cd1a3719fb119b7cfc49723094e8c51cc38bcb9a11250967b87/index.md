---
blocks:
- block_id: 75c238591efa4e91b82b86e65a9afdd9ab412a9fd4606dc049d616d8d74b8576
  indexed: true
  sha256: c081c2ddad46a3b86af9b061733303413009d523481ebfd50ca65c8fbab4f81c
  source_range:
    end: 5377742
    part: 0
    start: 0
  summary: 此前已完成单个 `vllm bench` 客户端、全局并发 4（DP0–DP3 各 1 请求）的 8K/off 与 128K/on 测试；128K 首次结果因 DP0 残留缓存被判为暂存值，随后干净复测为 4/4 成功，TTFT/TPOT/ITL 3014.62/32.56/136.22 ms、AL 4.1994、各 DP 命中 129408/131072。评论中已补充 GSM8K 构造、prompt/warmup/hash 校验和单文件复现脚本，并移除绝对路径；脚本 `--prepare-only` 的验证结果和嵌入脚本 SHA 被报告为已核对。随后只读盘点了共享 NFS 的 `/mnt/weight/k3`，确认 7 套完整 W4A8 权重及若干 5 层目录；用户要求用最新 GQA DSpark 对除 multicards 外的完整权重进行 8K/1K 采信率、TPOT 等测试。
  title: 单客户端每 DP 并发 1 的基准复测与数据集复现脚本
- block_id: 76b371b04d2741100d693929cff10ad445f87c30b3063d6f813d20bdf5cc2472
  indexed: true
  sha256: 27ef65ead52146e5ef82d9ee0cad58c08a1817fd7a8b8427611686a4fd11bc5b
  source_range:
    end: 5377742
    part: 1
    start: 0
  summary: 矩阵测试采用最新 GQA DSpark、固定 8192→1024 token workload、单个 `vllm bench` 客户端和 DP0–DP3 各 1 并发。第 2 项完整权重经过 345 个 shard 加载、编译和 ACLGraph 后 health=200，正式请求 4/4 成功，但采信率仅 0.2198%、AL 1.0154、TTFT/TPOT/ITL 5341.83/124.66/126.58 ms；结果已归档。停止服务时暴露了 `pipefail` 下 `pgrep` 返回 0 进程导致脚本提前退出的问题，之后确认四节点均已释放；第 3 项启动并进入 347 shard 加载阶段。
  title: W4A8 矩阵测试：第 2 项加载与流程修正
- block_id: 2d04acad7c87908b47dc2f20e55f5d371d8c47addc463821de261d3c51861646
  indexed: true
  sha256: 9b78c72eacbf0e120ad80571362dee1ae4f56a99b15139b96c2fb5b52e14af6f
  source_range:
    end: 5377742
    part: 2
    start: 0
  summary: 第 3 项完成加载、编译、图捕获和测试，4/4 成功，采信率 0.1055%、AL 1.0074、TPOT 125.64 ms、TTFT 5315.15 ms、输出吞吐 30.39 tok/s；第 4 项也完成，4/4 成功，采信率 0.1339%（38/28378）、AL 1.0094、TPOT 125.48 ms、TTFT 5258.73 ms、输出吞吐 30.65 tok/s。两项均使用相同 prompt token IDs、GQA draft 和单客户端 DP4 协议，并在归档后精确停服。第 5 项已完成 352 个分片加载和 GQA draft 加载（target 624.60 秒、draft 0.47 秒），正在进入图捕获；其正式测试结果尚未提供，后续项目也未在这些材料中完成。
  title: W4A8 矩阵测试：第 3–5 项进展与已完成结果
- block_id: 73f076ab67f492fff95e89836cf851ce5071be849f51718c307779bcb2a850e5
  indexed: true
  sha256: 48b0ae0d40947535f650f679386d02e49cb9af8b5af9c2ac92158696e873901c
  source_range:
    end: 6361033
    part: 0
    start: 5377742
  summary: '第5项 ACLGraph 捕获耗时197秒、0.56 GiB，正式测试报告采信率79.2332%、AL 6.5463、TPOT 19.16 ms、TTFT 5351.06 ms、吞吐163.27 tok/s；第6项读取353个分片耗时925.02秒，正式结果采信率79.8535%（3488/4368）、AL 6.5897、TPOT 19.37 ms、TTFT 5306.27 ms、吞吐160.83 tok/s，均据称4/4成功。随后材料报告完成6项汇总、24份rank日志 fatal 扫描为0、四机 serve/workers 清零，并更新 PR #13277 自有评论、删除旧评论5144927690；但这些是来源中的操作与检查陈述，不能视为独立验证，且最终 git status 因工作区不是Git仓库返回128，删除的旧评论无法直接恢复。'
  title: W4A8 第5–6项完成及 PR 评论收尾
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 9fb37f7f8bc00cd1a3719fb119b7cfc49723094e8c51cc38bcb9a11250967b87
  kind: experience
  material_digest: f5c9291cf5332e88c370cd309d1f31c7a5f42e46def96e5464a7fbea76f80f86
  revision: f1ead4ec8448973e5933810ebf8de395283fe3d21fefe0b8164db1c1640c0d16
  schema: mindie-entry/3
  summary: '此前记录的第1项可复用结果、第2–4项低采信率结果、第5项未完成状态现已被来源材料中的后续报告更正为：第5项和第6项均完成正式测试，其中第5项采信率79.2332%、AL 6.5463，第6项采信率79.8535%、AL 6.5897；来源称三项 quarot/flex-quarot 接近 AL 1、0726 MoE 居中，但这些结论仍属待核参考。来源还报告固定 prompt SHA、DP4/单客户端/8K→1K/prefix cache off 条件、24份日志 fatal=0、serve=0/workers=0，以及 PR #13277 评论已替换并清除内部绝对路径；保留此前的 pipefail/pgrep 停止脚本问题、残留缓存污染的128K暂存值及所有早期未完成状态，原始块仍可独立检索。'
  title: W4A8 完整权重 8K/1K 矩阵测试进展
navigation: '此前记录的第1项可复用结果、第2–4项低采信率结果、第5项未完成状态现已被来源材料中的后续报告更正为：第5项和第6项均完成正式测试，其中第5项采信率79.2332%、AL 6.5463，第6项采信率79.8535%、AL 6.5897；来源称三项 quarot/flex-quarot 接近 AL 1、0726 MoE 居中，但这些结论仍属待核参考。来源还报告固定 prompt SHA、DP4/单客户端/8K→1K/prefix cache off 条件、24份日志 fatal=0、serve=0/workers=0，以及 PR #13277 评论已替换并清除内部绝对路径；保留此前的 pipefail/pgrep 停止脚本问题、残留缓存污染的128K暂存值及所有早期未完成状态，原始块仍可独立检索。'
schema: mindie-material-task/1
status: complete
task_id: 9fb37f7f8bc00cd1a3719fb119b7cfc49723094e8c51cc38bcb9a11250967b87
---

# W4A8 完整权重 8K/1K 矩阵测试进展

此前记录的第1项可复用结果、第2–4项低采信率结果、第5项未完成状态现已被来源材料中的后续报告更正为：第5项和第6项均完成正式测试，其中第5项采信率79.2332%、AL 6.5463，第6项采信率79.8535%、AL 6.5897；来源称三项 quarot/flex-quarot 接近 AL 1、0726 MoE 居中，但这些结论仍属待核参考。来源还报告固定 prompt SHA、DP4/单客户端/8K→1K/prefix cache off 条件、24份日志 fatal=0、serve=0/workers=0，以及 PR #13277 评论已替换并清除内部绝对路径；保留此前的 pipefail/pgrep 停止脚本问题、残留缓存污染的128K暂存值及所有早期未完成状态，原始块仍可独立检索。

## Materials

- [单客户端每 DP 并发 1 的基准复测与数据集复现脚本](blocks/75c238591efa4e91b82b86e65a9afdd9ab412a9fd4606dc049d616d8d74b8576.md): 此前已完成单个 `vllm bench` 客户端、全局并发 4（DP0–DP3 各 1 请求）的 8K/off 与 128K/on 测试；128K 首次结果因 DP0 残留缓存被判为暂存值，随后干净复测为 4/4 成功，TTFT/TPOT/ITL 3014.62/32.56/136.22 ms、AL 4.1994、各 DP 命中 129408/131072。评论中已补充 GSM8K 构造、prompt/warmup/hash 校验和单文件复现脚本，并移除绝对路径；脚本 `--prepare-only` 的验证结果和嵌入脚本 SHA 被报告为已核对。随后只读盘点了共享 NFS 的 `/mnt/weight/k3`，确认 7 套完整 W4A8 权重及若干 5 层目录；用户要求用最新 GQA DSpark 对除 multicards 外的完整权重进行 8K/1K 采信率、TPOT 等测试。
- [W4A8 矩阵测试：第 2 项加载与流程修正](blocks/76b371b04d2741100d693929cff10ad445f87c30b3063d6f813d20bdf5cc2472.md): 矩阵测试采用最新 GQA DSpark、固定 8192→1024 token workload、单个 `vllm bench` 客户端和 DP0–DP3 各 1 并发。第 2 项完整权重经过 345 个 shard 加载、编译和 ACLGraph 后 health=200，正式请求 4/4 成功，但采信率仅 0.2198%、AL 1.0154、TTFT/TPOT/ITL 5341.83/124.66/126.58 ms；结果已归档。停止服务时暴露了 `pipefail` 下 `pgrep` 返回 0 进程导致脚本提前退出的问题，之后确认四节点均已释放；第 3 项启动并进入 347 shard 加载阶段。
- [W4A8 矩阵测试：第 3–5 项进展与已完成结果](blocks/2d04acad7c87908b47dc2f20e55f5d371d8c47addc463821de261d3c51861646.md): 第 3 项完成加载、编译、图捕获和测试，4/4 成功，采信率 0.1055%、AL 1.0074、TPOT 125.64 ms、TTFT 5315.15 ms、输出吞吐 30.39 tok/s；第 4 项也完成，4/4 成功，采信率 0.1339%（38/28378）、AL 1.0094、TPOT 125.48 ms、TTFT 5258.73 ms、输出吞吐 30.65 tok/s。两项均使用相同 prompt token IDs、GQA draft 和单客户端 DP4 协议，并在归档后精确停服。第 5 项已完成 352 个分片加载和 GQA draft 加载（target 624.60 秒、draft 0.47 秒），正在进入图捕获；其正式测试结果尚未提供，后续项目也未在这些材料中完成。
- [W4A8 第5–6项完成及 PR 评论收尾](blocks/73f076ab67f492fff95e89836cf851ce5071be849f51718c307779bcb2a850e5.md): 第5项 ACLGraph 捕获耗时197秒、0.56 GiB，正式测试报告采信率79.2332%、AL 6.5463、TPOT 19.16 ms、TTFT 5351.06 ms、吞吐163.27 tok/s；第6项读取353个分片耗时925.02秒，正式结果采信率79.8535%（3488/4368）、AL 6.5897、TPOT 19.37 ms、TTFT 5306.27 ms、吞吐160.83 tok/s，均据称4/4成功。随后材料报告完成6项汇总、24份rank日志 fatal 扫描为0、四机 serve/workers 清零，并更新 PR #13277 自有评论、删除旧评论5144927690；但这些是来源中的操作与检查陈述，不能视为独立验证，且最终 git status 因工作区不是Git仓库返回128，删除的旧评论无法直接恢复。
