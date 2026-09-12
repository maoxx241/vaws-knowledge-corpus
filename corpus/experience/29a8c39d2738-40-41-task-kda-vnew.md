# 用 40→41 task 边界定位 KDA 空 Vnew 分支的事件遗漏

2026-08-07 至 08-10，AscendC `chunk_gated_delta_rule_fwd_h` 的 arch22 路径在多序列短 prefill 挂起。把整网问题缩成单算子后，同一旧二进制的 40 task 约 45 ms 完成，41 task 超时；七条序列、每条一个 token、六个 head 的 42 task 也触发问题。仅把每条序列改成两个 token，42 task 又能完成。

这个对照改变的是第二个 AIV 子块是否为空，而不是通信拓扑。检查 ping-pong 的事件协议发现，空 Vnew 分支提前返回，遗漏等待和归还 HardEvent，第三波复用时被阻塞。修复只补齐该分支的事件消费、归还及跨核完成通知。归档原始结果显示，修后 41/42/80/96 task 均完成；42 task 批量调用与七次隔离调用的 h、state、v_new 比较最大绝对差均为零。

它解释了当时这个形状相关的算子挂起，不能解释所有 prefix-hit 精度污染。整网曾同时采用同步兜底，不能把同步效果和事件修复的单算子证据混在一起。后续使用时应检查目标版本的事件协议，而非复制旧补丁。[相关历史 PR](https://github.com/vllm-project/vllm-ascend/pull/13277)。
