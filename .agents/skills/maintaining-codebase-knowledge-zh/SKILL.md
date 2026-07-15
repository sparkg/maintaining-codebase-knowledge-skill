---
name: maintaining-codebase-knowledge-zh
description: 当接手不熟悉的仓库、为请求的功能或 Bug 深化项目知识、将任务范围内的企业或对话证据与实现关联、在代码变更后刷新知识、完成前检查文档影响，或向开发工作流提供可靠的仓库上下文时使用。
---

# 维护代码库知识

维护有证据支撑的仓库地图。本 Skill 负责文档；开发工作流负责实现和完成。

## 核心规则

- 证据只能支撑特定类型的声明；不同来源不是可互换的事实。
- 将支持状态、可信度、就绪度、持久化和文档深度分开。
- 每个持久事实只有一个 canonical document owner；其他文档只链接它。
- 将 capability、flow 和实现单元视为不同概念；映射它们，而不是假设一一对应。
- 托管文档只保留当前状态。历史放在 Git、已接受 ADR、spec 或 plan 中。
- 只有带 managed marker 的文档才能结构化重写；保留用户撰写的内容。
- 绝不创建空章节。

## 加载参考资料

写入项目知识前，阅读 `references/knowledge-model.md`。

当对话、企业、CI、事故或受治理的证据影响任务时，阅读 `references/external-context.md`。

当设计、规划、实现、调试、评审、验证或交付工作流将消费项目知识时，阅读 `references/development-workflow-integration.md`。

## 选择模式

| 模式 | 使用时机 |
|---|---|
| Bootstrap | 仓库不熟悉，或缺少可靠的知识 index |
| Deepen | 请求任务对应的能力知识缺失或不足 |
| Refresh | 稳定实现已变更，或需要检查文档影响 |

不要把首次发现与代码变更混在一起。各模式可以读取已授权证据并运行安全检查，但不得编辑代码、暴露 secret、修改外部系统或负责工作流状态。

## 接入与范围

在每种模式开始时：

1. 使用 `references/knowledge-model.md` 解析唯一的目标项目根目录；在任何托管写入前声明其绝对路径和解析依据，并在整个运行期间复用它。
2. 使用 `references/knowledge-model.md` 选择唯一的项目指令桥接，然后读取目标项目根目录适用的项目指令。检查 `<target-repository-root>/docs/project-knowledge/index.md`；当它存在时，必须先读取它，再选择任何 intent、capability、flow 或其他项目文件，并记录知识入口状态。
3. 声明目标、范围、选中能力、排除项和成功标准。
4. 检查当前对话和附件；出现重要证据时加载 external-context reference。
5. 记录 revision，以及重要的环境或外部来源快照。
6. 暴露会改变 planning 的歧义。

对于 Bootstrap，缺少 index 是有效状态：先记录 `missing`，写入后记录 `created`。Index 不可读时所有模式都必须停止，以免覆盖内容未知的现有文件。对于 Deepen 或 Refresh，index 缺失属于模式不匹配：停止并建议执行 Bootstrap。绝不能用其他 worktree、Skill 安装目录中的 index 或之前运行中记忆的内容代替。

每次成功运行后都必须保留恰好一个指向当前 index 的选中项目指令桥接。只有两个受支持的指令文件都不存在时才创建 `AGENTS.md`；否则保留选中的文件名。写入前验证最终托管块；选择、发现或 marker 存在 unresolved 冲突时进行报告，不得回退到另一个文件。

## Bootstrap

1. 盘点适用的指令、manifest、入口、文档、测试、CI、部署和迁移；评估架构模板中列出的关注项，但不要阅读每个文件。
2. 扫描 TODO/FIXME、backlog 或 issue-like 文件、spec、example、interface、schema、configuration、deprecated code 和测试缺口。
3. 识别一到五个高价值能力，并追踪带测试证据的代表性 caller-to-effect 路径。Bootstrap 有意保持选择性：未选择或未来出现的 capability 是有效的按需 Deepen 工作，不是 Bootstrap 失败。
4. 创建最小托管文档集：每个选中能力一个文档，每个重要 intent 一个链接到能力的 ledger 行。只有当两个或更多能力 owner 之间存在值得独立维护的时序、交接、事务、重试或副作用时才创建 flow；否则将局部时序保留在其 capability 中。
5. 模拟两到三个可能的 handoff。当 planner 找不到不变量、测试、缺失决策或证据快照时，降低 readiness。
6. 使用 `references/knowledge-model.md` 渲染并验证选中项目指令桥接的托管块，只有选择、预算和内容检查通过时才刷新。只有当选中能力具有证据支撑的当前状态、选中的重要 intent 都有唯一路由，且持久事实只有一个正文 owner 时才结束；不要为了预测未来工作而扩展 inventory。

## Deepen

1. 从一个请求任务或 intent-ledger 行开始；不要重新 Bootstrap。新的或尚未路由的请求是有效的 Deepen 输入。
2. 执行 `references/knowledge-model.md` 中的任务知识充分性门。如果路由和任务关键证据已经足够，报告 `checked-unchanged`，不要扩展文档。
3. 只读取 planning 所需的 canonical 来源和实现路径。追踪相关 caller-to-effect 路径，并在重要时提取关键 symbol、所有权/生命周期、局部状态转换、算法分支和终止条件、数据/并发行为、失败路径、不变量和测试 seam。使用 symbol 级证据和有范围的可复现快照；不要复制实现正文。
4. 分开 intent 与 support，并报告冲突。Intent 缺失时创建或路由；考虑新增 capability 前先深化现有 capability。
5. 对较宽的 capability，记录其重要内部区域和当前文档深度。只有独立拥有的结果、契约、生命周期、不变量、变化轴或反复独立工作的证据存在时才拆分；规模本身不是证据。用户显式请求 capability discovery 时，只持久化已验证 capability，将弱候选留在运行结果中。
6. 更新 intent 行和最小 capability/flow 集。只有两个或更多 capability owner 之间存在重要协调时才创建或深化 flow。只有创建或拆分 canonical capability/flow 时才更新 index 和 architecture 映射；将任务讨论和易变状态留在 task context 或结果中。
7. 当 `index -> intent row -> capability/flow` 能提供入口、任务相关实现机制、不变量、测试、缺失证据、人类决策，并在边界可能变化时提供不越俎代庖设计决策的放置证据时结束。

## Refresh

1. 检查 diff、受影响测试、运行时路径、intent artifacts 和相关快照。
2. 使用 `references/knowledge-model.md` 将变更声明映射到 owner。
3. 替换过期的托管状态；绝不追加历史 baseline 或重复 handoff。写入受影响的项目指令桥接前，使用 `references/knowledge-model.md` 验证选中文件和最终渲染的托管块。
4. 只有 planning 安全性变化时才修改 readiness。局部缺口保留在 capability 文档，跨能力风险保留在 `risks.md`。
5. 持久知识未变化时报告 `checked-unchanged`。
6. 在实现稳定后、completion verification 前运行 Refresh；它不能证明代码可用。

## 结果契约

每次运行以下列内容结束：

```text
Mode and goal: <模式、范围、成功标准>
Target repository root: <绝对路径和解析依据>
Knowledge entrypoint:
  Path: <绝对路径>
  Status: read | created | missing | unreadable
Project instruction bridge:
  Path: <绝对路径>
  Selection basis: existing AGENTS.md | existing CLAUDE.md | created AGENTS.md
  Discovery: native | platform-dependent | shadowed
  Managed block budget: <字符数>/800
  Status: written | unchanged | unresolved
Evidence snapshot: <revision、命令、环境、来源版本/时间>
Context evidence:
  Considered: <条目或 none>
  Persisted: <canonical 路径和提炼后的声明，或 none>
  Rejected or session-only: <条目及原因，或 none>
  Conflicts or unavailable: <条目或 none>
Knowledge changes:
  Updated: <路径或 none>
  Removed as stale: <声明或路径，或 none>
  Checked unchanged: <路径或 none>
Planning:
  Readiness: ready | needs-deepen | needs-decision | blocked
  Read first: <index、intent row、capability 或 flow>
  Unresolved decisions: <问题或 none>
  Recommended next action: <一个动作>
Placement evidence（当放置位置或边界相关时）：
  Matching capability or flow: <canonical 链接或 none>
  Primary and supporting implementation units: <实现单元和链接，或 unknown>
  Existing extension seam: <有证据支撑的扩展点，或 none observed>
  Boundary pressure: <观察到的压力，或 none observed>
  Design decision still required: <问题或 none>
Deepening evidence（Deepen 时）：
  Knowledge route: <intent existing|created|updated；capability/flow matched|expanded|created|split>
  Sufficiency before Deepen: sufficient | insufficient；<缺失维度或 none>
  Key implementation symbols: <symbol 和 canonical section 或 none>
  State machine or algorithm map: <canonical section 或 none>
  Scoped evidence snapshot: <revision + dirty file hash，或选定文件 hash>
  Capability structure: <展开区域；split|keep-together|candidate-only|unchanged；证据依据>
```

## 常见错误

| 错误 | 修正 |
|---|---|
| 将 TODO 或用户报告视为实现事实 | 分别验证 intent 和 support。 |
| 将状态复制到 index、ledger、capability、risk 和 handoff | 只存一次并链接。 |
| 在当前状态文档中保留变更前后状态 | 替换过期托管内容；使用历史 artifacts。 |
| 将本地缺少工具变成项目技术债 | 除非是可复现的项目约束，否则只保留在结果中。 |
| 结构化重写未标记的用户文档 | 外科手术式更新或报告 unresolved。 |
| 将 capability 当作代码 module，或把观察到的布局说成设计意图 | 分别映射 capability、flow 和实现单元；将任务专属放置决策留给设计。 |
| 将缺失的任务路由视为需要重新 Bootstrap | 执行任务范围的 Deepen，创建最小已验证路由。 |
| 因 capability 文件或行数多而拆分 | 记录内部覆盖，拆分前要求稳定所有权或边界证据。 |
| 只列路径而没有任务相关机制 | 增加关键 symbol、状态或算法行为、不变量、测试和有范围的证据快照。 |
