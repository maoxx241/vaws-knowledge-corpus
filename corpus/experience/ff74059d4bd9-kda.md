# KDA 卷积预打包优化因重载生命周期而重写

2026-07-30，优化 Kimi KDA 的 `transpose/cat/cast/contiguous` 热路径时，最初把派生卷积权重缓存成 non-persistent buffer。审查发现，这种缓存能让首次 forward 变轻，却没有自动参与 kernel-format reload：当时重载入口按具名 Parameter 查找，buffer 方案可能在换权重后继续使用旧副本。

随后参照已有 MLA 权重处理方式重写：保留 checkpoint 的 q/k/v canonical 权重，在 post-load 阶段生成具名 packed Parameter，通过原地 copy 更新以保持图捕获地址。回归用例实际执行完整 checkpoint reload 和 kernel-format 更新，同时检查新值、注册名称、布局与原 data pointer；原始 main 与 v0.25.1 定向结果均为 31 passed。

这次没有修改 AscendC kernel，也没有另造全局重载机制。其证据是公开 Python 生命周期与测试，不把后来别的 GDN packed cache 审阅自动算作已修复。仅部分 q/k/v 的 checkpoint-format 更新不在当时声明的支持面内；图模式的地址稳定也不能仅靠“指针没变”代替数值已刷新检查。[包含该优化的历史 PR](https://github.com/vllm-project/vllm-ascend/pull/12950)。
