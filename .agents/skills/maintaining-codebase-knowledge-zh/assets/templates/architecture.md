<!-- codebase-knowledge:managed -->
# 架构

最后验证：重要时记录 revision 和日期

## 项目概况

| 方面 | 当前状态 | 证据 | 对开发的影响 |
|---|---|---|---|
| 项目类型、用户、技术栈、runtime、build、deployment 或扩展模型 | 有证据支撑的摘要 | Manifest、入口、CI 或部署路径 | 与开发相关的约束 |

## 系统边界

描述用户、系统边界、运行时 container 或 deployment unit、data store 和外部运行时系统。

## Module 和依赖方向

| Unit | Responsibility | Public boundary | Dependencies | Evidence |
|---|---|---|---|---|
| Module、package、service 或 deployment unit | 唯一职责 | API、event、CLI、job 或内部边界 | 有方向的依赖 | 路径或 manifest |

## Module 边界依据

只纳入重要实现单元。`边界依据` 是已接受决策、仅观察到的实现结构或 `unknown`；绝不要把观察到的布局说成原始设计意图。

| 实现单元 | 观察到的内聚性或变更轴 | 边界依据 | 扩展点 | 边界压力 | 证据 |
|---|---|---|---|---|---|
| Canonical 实现单元 | 一起变化的行为、状态或依赖 | 决策链接、仅观察到的结构或 unknown | 现有接口或注册点 | 已验证耦合、依赖环、不变量绕过、测试耦合或 none observed | 证据类型、可信度和路径 |

## Capability 到实现单元映射

将这里作为 canonical 多对多映射；capability 文档链接这里，而不是复制。

| Capability | 主要实现单元 | 支持实现单元 | 共享基础设施 | 证据 |
|---|---|---|---|---|
| Canonical capability 链接 | 主要 runtime 或状态 owner | 其他参与实现单元 | 仅横切机制 | 路径或测试 |

## 数据和集成全景

只描述重要的 store、queue、cache、file 或 object storage，以及外部 runtime 集成。链接其 canonical 运营或 capability 文档。

## 架构关注项

评估下方列出的每个关注项类别。纳入已验证机制、重要 unknown 和明确的 not-applicable 结论；省略无关类别和空洞模板内容。

| 关注项 | 状态 | 机制或 owner | 证据 | 对开发的影响 |
|---|---|---|---|---|
| 异常处理和韧性 | established、unknown 或 not-applicable | 系统级默认机制及其 owner | 路径、manifest、命令或来源快照 | 约束、必需检查或 none |
| Logging、metric、tracing 和 audit | established、unknown 或 not-applicable | 系统级默认机制及其 owner | 路径、manifest、命令或来源快照 | 约束、必需检查或 none |
| 安全和敏感数据处理 | established、unknown 或 not-applicable | 系统级默认机制及其 owner | 路径、manifest、命令或来源快照 | 约束、必需检查或 none |
| 缓存和一致性 | established、unknown 或 not-applicable | 系统级默认机制及其 owner | 路径、manifest、命令或来源快照 | 约束、必需检查或 none |
| 限流和 backpressure | established、unknown 或 not-applicable | 系统级默认机制及其 owner | 路径、manifest、命令或来源快照 | 约束、必需检查或 none |
| 依赖和配置管理 | established、unknown 或 not-applicable | 系统级默认机制及其 owner | 路径、manifest、命令或来源快照 | 约束、必需检查或 none |
| 并发、job 和 transaction | established、unknown 或 not-applicable | 系统级默认机制及其 owner | 路径、manifest、命令或来源快照 | 约束、必需检查或 none |
| 交付、health、恢复、migration 和 rollback | established、unknown 或 not-applicable | 系统级默认机制及其 owner | 路径、manifest、命令或来源快照 | 约束、必需检查或 none |

## 架构约束

只记录系统级默认机制和约束。链接 capability 特定行为或例外，以及 flow 所拥有的传播规则，不要复制。
