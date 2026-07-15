<!-- codebase-knowledge:managed -->
# 能力：简短名称

最后验证：重要时记录 revision 和日期

证据快照：干净 revision 加任务关键 symbol；对于相关 dirty 或无 VCS 证据，记录选定文件完整内容 hash。绝不缩写证据 hash，也不要哈希完整仓库。

## 用途

说明领域或业务结果及其 caller 或 consumer。

## 边界契约

| 契约项 | 有证据支撑的陈述 | 证据 |
|---|---|---|
| Owns | 此 capability 拥有的行为和状态 | 路径、测试或来源快照 |
| Does not own | 由其他位置拥有的相邻职责 | Canonical owner 链接 |
| Inputs | 语义输入和前置条件 | Interface、schema 或 caller |
| Outputs | 可观察结果和保证 | Interface、schema 或测试 |
| Side effects | 局部状态、数据或外部效果 | 实现或测试 |
| Failure surface | 局部错误和失败条件 | 实现或测试 |

## 当前行为

只保留能承载证据的维度。

| 行为或变体 | Support | Confidence | Evidence | Tests or benchmarks |
|---|---|---|---|---|
| 具体当前行为 | supported、partial、missing 或 unknown | verified、hypothesis 或 unknown | 路径、命令或来源快照 | 现有覆盖或缺口 |

## 运行时和数据路径

只描述实现工作所需的内部入口/caller、owner、状态或数据变化和局部时序。链接跨 capability 编排，而不是重复它。

## 内部区域和知识覆盖

只有 capability 足够宽、读者需要区分已定位区域和任务所需机制时才纳入本节。文件数或行数本身不能证明需要拆分。

| 内部区域 | 职责 | Documentation depth | 边界压力 | 证据 |
|---|---|---|---|---|
| 重要区域 | 对 capability 结果的 owned contribution | mapped、traced 或 unknown | 稳定独立边界证据或 none observed | 路径加代表性 symbol、测试或有范围快照 |

## 实现机制

只纳入下方与任务相关的结构，并省略未使用标题。使用路径加 symbol 的证据，不复制实现正文。

### 关键类型和所有权

| Symbol | 职责 | 所有权或生命周期 | 证据 |
|---|---|---|---|
| Class、type、trait、interface、function、注册点或 schema | 与任务相关的职责 | Owner、lifetime、state 或资源职责 | 路径、symbol、测试和有范围快照 |

### 局部状态机

| 状态 | 事件或输入 | Guard | 转换或效果 | 失败或终止行为 | 证据 |
|---|---|---|---|---|---|
| 具体状态 | Trigger | 条件 | 下一状态、修改、输出或副作用 | Error、取消、fallback 或完成 | Symbol 和测试 |

### 算法地图

| 阶段或分支 | 输入和决策 | 状态变化或输出 | 终止或 fallback | 不变量 | 证据 |
|---|---|---|---|---|---|
| 具体步骤 | 重要条件或选择 | 效果 | Stop、retry、continue 或 fallback | 保持的约束 | Symbol 和测试 |

### 数据和并发模型

只描述属于本 capability 且与任务重要的数据形状、序列化、所有权、lock/queue、取消、一致性或资源生命周期。跨 owner 协调属于 flow。

## 变更放置证据

只记录稳定的仓库证据；任务专属放置选择留在运行结果或设计工作流中。

| 信号 | 有证据支撑的陈述 | 证据 |
|---|---|---|
| 观察到的变更轴 | 一起变化的行为、状态、策略或依赖 | 路径、测试，或可信时的历史 |
| 现有扩展点 | Interface、注册点、adapter、plugin、event 或 none observed | 路径或测试 |
| Architecture 映射 | 主要和支持实现单元 | 链接 canonical architecture 行 |
| 边界压力或拆分信号 | 已验证的独立结果/契约、状态/生命周期、不变量/失败边界、owner/变化节奏、无关变化原因、反复独立工作或 none observed；规模本身不是证据 | 路径、测试、任务历史或决策证据 |

## 参与的流程

只纳入实际存在的跨 capability flow。交接契约单元格应链接 flow 文档中的 canonical 行，而不是再次陈述契约。

| Flow | Role | Handoff contract |
|---|---|---|
| Canonical flow 链接 | Producer、consumer、coordinator 或 participant | 链接 flow 所拥有的 handoff 行 |

## 不变量

只列出已验证的语义、兼容性、性能、内存、可靠性或安全约束。

## 能力局部缺口

记录具体不支持行为或缺失测试。任务专属决策留在 `intent-ledger.md`，跨能力风险留在 `risks.md`。
