# 从合法 block ID 追到旧共享 KV backing 的物理地址重叠

2026-08-06，旧 Kimi K3 GQA DSpark 与 MLA/Mamba 共享 backing 的实验路径发生长上下文 NaN。只查逻辑 block ID 没发现重复，因此进一步保留了 tensor 的 base、stride、storage offset、block table 和同时活跃 group，而不是只导出一份连续 CPU 副本。

原始现场中，target 的一处历史 KV 读取发现 448 个 NaN；当前新写部分仍有限。把受损物理地址按旧 page 488,448 字节反算，落到 draft 的 block 1969，并在 draft block table 中找到对应位置。由此把“逻辑编号看起来合法”推进成可核验的物理重叠线索，说明仅改 page-size 元数据不足以保证实际 view 使用了相同布局。

后续隔离 draft backing 仍未消除所有第二次 warmup 污染，因此此证据不能称为完整根因闭环。旧四平面布局后来从相关 PR 中移除；其 page 数字和分配方案不作为现行建议。可复用的是取证方式：保留存储语义，证明同时活跃数据的地址覆盖关系，并区分实际冲突与允许按生命周期复用的 overlay。图内观察也要核对 snapshot 的执行时机，普通 Python forward 打印不能证明 replay 时的设备值。
