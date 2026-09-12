# QuaRot draft 接入时分开检查权重格式与特征坐标系

2026-08-10，Kimi K3 的 QuaRot target 与 MLA DSpark 组合在加载成功后仍需要检查采信异常。调查把两个容易混在一起的问题拆开：draft 自身权重是否量化，以及 target 传出的 auxiliary features 处于哪个旋转基底。不能靠清空 target 的量化描述，让 draft 暂时绕过加载分支。

源码沿共享 embedding、lm_head 和 `context_proj` 核对。MLA draft 通过多个 auxiliary block 输入 projection，加载时需要与 target 的全局 rotation 对齐；GQA 的 fc 则是另一个入口。原始 worker 日志明确记录了 `context_proj.weight` 对齐，并在不同 TP rank 上重复出现，公共修复也给出对应 loader 与回归用例。

这篇只保留公开代码的坐标系接入经验，不发布专有 checkpoint 文件或内部质量数字。一次单机缩减专家结果不能外推双机，也不能由 descriptor 修复推断 DP2 全部通过。后来的模型和 loader 分支已变化；使用这个案例的价值在于检查“加载格式正确”之外的基底一致性，而不是复制旧反旋转步骤或重复旋转权重。[历史 MLA draft 修复](https://github.com/vllm-project/vllm-ascend/pull/13277)。
