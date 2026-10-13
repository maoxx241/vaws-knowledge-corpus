---
blocks:
- block_id: 64cb97bc39efaea585d6b693af36264bab914dedc91fa0d34fb7e6b80ef8535b
  indexed: true
  sha256: 1b8d498b7611882c6cce6d3254ec0a99f9cfd361099e9a30d352365dd40d0f14
  source_range:
    end: 525637
    part: 0
    start: 0
  summary: 据消息内容，助手报告已完成四节点 rowprobe 的只读取证：权威入口为本地 `runtime-sglang-k3-dspark-oracle-r1`，远端 `...-debug1` 被描述为旧副本；153–156 节点上的三份探针文件据称已部署且哈希一致，预检据称通过。服务仍处于停止状态，未启动或停止容器；后续启动应使用新 run ID、本地 `launch.sh`、完整容器 ID/标签，并等待 rank0 `/health` 返回 200，但这些均属于报告的状态或要求，不能视为本索引已独立验证的成功。
  title: 四机 rowprobe 启动前只读核验
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 8e2f614e93375fde9c426f3b3b5b5e8ca8b284b4a39898f90d19566f310626a8
  kind: experience
  material_digest: aad5c1310c9b633f7c2342b2860daf80adf088d1d111bdb180eea584ea21bc69
  revision: 34de7a52f26d3a1cfe931ec0ea604d0d2215f5ec3c7dca327c0aba2b704da753
  schema: mindie-entry/3
  summary: 本批仅包含一个材料块，记录四节点部署与只读预检的报告性结论：三文件挂载、哈希、Data IP、16 NPU、端口和残留进程据称正常，但服务尚未启动，健康检查尚未完成。原始块可独立检索；应保留“据称”“停止状态”和未验证的后续启动/`/health` 门禁，不能将计划或预检报告升级为已验证运行成功。
  title: 当前任务导航：四机 rowprobe 启动前状态
navigation: 本批仅包含一个材料块，记录四节点部署与只读预检的报告性结论：三文件挂载、哈希、Data IP、16 NPU、端口和残留进程据称正常，但服务尚未启动，健康检查尚未完成。原始块可独立检索；应保留“据称”“停止状态”和未验证的后续启动/`/health` 门禁，不能将计划或预检报告升级为已验证运行成功。
schema: mindie-material-task/1
status: complete
task_id: 8e2f614e93375fde9c426f3b3b5b5e8ca8b284b4a39898f90d19566f310626a8
---

# 当前任务导航：四机 rowprobe 启动前状态

本批仅包含一个材料块，记录四节点部署与只读预检的报告性结论：三文件挂载、哈希、Data IP、16 NPU、端口和残留进程据称正常，但服务尚未启动，健康检查尚未完成。原始块可独立检索；应保留“据称”“停止状态”和未验证的后续启动/`/health` 门禁，不能将计划或预检报告升级为已验证运行成功。

## Materials

- [四机 rowprobe 启动前只读核验](blocks/64cb97bc39efaea585d6b693af36264bab914dedc91fa0d34fb7e6b80ef8535b.md): 据消息内容，助手报告已完成四节点 rowprobe 的只读取证：权威入口为本地 `runtime-sglang-k3-dspark-oracle-r1`，远端 `...-debug1` 被描述为旧副本；153–156 节点上的三份探针文件据称已部署且哈希一致，预检据称通过。服务仍处于停止状态，未启动或停止容器；后续启动应使用新 run ID、本地 `launch.sh`、完整容器 ID/标签，并等待 rank0 `/health` 返回 200，但这些均属于报告的状态或要求，不能视为本索引已独立验证的成功。
