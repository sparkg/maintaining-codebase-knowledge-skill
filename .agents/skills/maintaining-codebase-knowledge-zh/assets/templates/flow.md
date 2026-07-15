<!-- codebase-knowledge:managed -->
# 流程：简短名称

最后验证：重要时记录 revision 和日期

证据快照：干净 revision 加 participant symbol；对于相关 dirty 或无 VCS 证据，记录选定文件内容 hash。

只有当两个或更多 capability owner 参与，且顺序、交接、事务、重试、回滚或副作用值得独立维护时，才使用此文档。

参与者内部细节留在 capability 文档中。新增或修改的 flow 描述编排，本身不意味着需要新的实现单元。

## 触发和结果

说明跨能力 flow 的触发条件及其可观察结果或副作用。

## 参与者和交接

| Step | Capability owner | Consumes | Produces | Handoff invariant | Failure propagation | Evidence |
|---|---|---|---|---|---|---|
| 1 | Canonical capability 链接 | 输入或前置条件 | 输出、状态变化或副作用 | 下一个 owner 依赖的条件 | 停止、重试、补偿、回滚或继续 | 路径、测试或命令 |

## 跨 owner 状态转换

只有 flow 本身具有持久跨 owner 状态时才纳入本节。不要复制 participant 的局部状态机。

| Flow 状态 | 事件或交接 | Guard | 下一状态或外部效果 | 失败、重试或回滚 | 证据 |
|---|---|---|---|---|---|
| 具体跨 owner 状态 | Participant 输出或外部事件 | 交接条件 | 下一个 participant 可见状态或副作用 | 传播或恢复 | 路径加 symbol 和测试 |

## Flow 边界和恢复

记录已验证的跨 owner 事务、一致性、授权、异步、重试、超时、回滚、兼容性、并发、性能、可靠性、安全和外部副作用边界。

## Flow 不变量和测试

只记录此 flow 所拥有的端到端或交接不变量及覆盖。不要重复 capability support matrix、局部不变量、局部缺口或局部测试地图。
