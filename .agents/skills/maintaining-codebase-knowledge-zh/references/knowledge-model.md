# 项目知识模型

## 目录

- 证据维度和声明权威来源
- 目标项目根目录和输出边界
- 知识入口和项目指令桥接
- 结构和文档职责
- Architecture、capability、flow 和实现单元的边界
- 按需 Deepen 和任务知识充分性
- 任务契约差分和交付义务
- Capability 知识覆盖和拆分
- 深水实现证据和有范围的快照
- 架构关注项覆盖
- 托管文档和项目指令桥接生命周期
- 路由和 Refresh 影响
- 最终一致性验证
- 写作规则

只读取当前模式需要的部分：

- 解析仓库或指令桥接时，读取根目录、知识入口和桥接章节；
- 写入或路由知识时，读取结构、所有权、边界和路由章节；
- 准备具体任务时，读取充分性、任务契约差异和实现证据章节；
- 完成写入前，读取生命周期、最终一致性和写作规则。

## 证据维度

绝不要将这些维度合并为一个状态：

| 维度 | 取值 | 含义 |
|---|---|---|
| Evidence kind | `implementation`、`intent`、`operational`、`decision` | 来源可以支撑哪类声明 |
| Confidence | `verified`、`hypothesis`、`unknown` | 现有证据对声明的支撑强度 |
| Support | `supported`、`partial`、`missing`、`unknown` | 能力的当前实现状态 |
| Readiness | `ready-for-design`、`ready-for-implementation`、`needs-deepen`、`needs-decision`、`blocked` | 特定 intent 可以安全进入哪个下一开发阶段 |
| Persistence | `durable`、`task-context`、`session`、`rejected` | 外部或对话证据可以保留在哪里 |
| Documentation depth | `mapped`、`traced`、`unknown` | Capability 的重要内部区域被记录到多深；绝不是实现支持状态声明 |

使用 `Support: partial` 和 `Confidence: verified` 这类带维度的标签；当维度不明确时，绝不要单独使用 `partial` 或 `unknown`。

## 按声明类型确定权威来源

不存在通用的来源优先级。使用拥有该声明的来源：

| 声明 | 最佳证据 | 限定条件 |
|---|---|---|
| 仓库结构或实现 | 指定 revision 下的代码、schema、配置、测试和可复现行为 | 死代码、不可达代码和过期测试不能证明运行时行为 |
| 预期行为 | 指定版本的已批准需求、验收标准、spec 或 task | Intent 不能证明 support |
| 决策理由 | 已接受 ADR、批准记录或可信历史 | 无法确定的理由保持 unknown |
| 运营状态 | CI run、deployment、incident 或运行时观察 | 始终绑定 commit、环境和时间 |
| 工作流 owner 或状态 | 权威企业系统 | 视为易变元数据 |

## 目标项目根目录和输出边界

接入时只解析一个目标根目录。将它转换为绝对路径，报告选择依据，并在整个运行中保持不变。按以下顺序选择：

1. 使用用户或任务明确指定的现有目录。明确指定的 monorepo 子项目优先于外层 VCS 根目录。
2. 否则使用 VCS 原生命令报告的包含当前任务的根目录。
3. 无法使用 VCS 时，只有指令、manifest 或入口能够唯一确定项目根目录，才使用对应祖先目录。
4. 无法得到唯一候选时，报告 `unresolved`，不执行托管写入。

工作目录、Skill 路径、模板路径或 agent 配置目录不能独自证明项目根目录。只有用户或任务明确把 `.opencode`、`.agents`、`.claude` 或 `.codex` 等目录指定为项目时，才能选择它们。

所有托管输出都必须位于解析后的根目录中。写入前确认规范化目标没有越界。不要为另一个文件或模式重新解析根目录。

## 知识入口和项目指令桥接

`<target-repository-root>/docs/project-knowledge/index.md` 是项目知识的规范路由入口。存在时，接入阶段直接读取它。不要把 `AGENTS.md`、先前上下文或记忆当成已经读取索引的证据。

选中的根指令文件中的托管块帮助后续工作流发现索引。它必须保持精简，不得复制路由、capability 状态、实现路径、测试清单、任务 readiness 或交接状态。

直接接入保证本 Skill 使用当前索引；指令桥接帮助后续工作流发现它。如果平台不加载项目指令文件，把知识复制进记忆或 Skill metadata 也不能解决发现问题。

## 结构和职责

下列所有路径都相对于解析后的目标项目根目录。`index.md` 是必需的。其他文档均为条件创建；对于已有的充分文档，通过链接复用。

```text
docs/project-knowledge/
|- index.md
|- architecture.md
|- onboarding.md
|- intent-ledger.md
|- capabilities/<capability>.md
|- flows/<cross-capability-flow>.md
|- external-systems.md
|- risks.md
|- domain-glossary.md
`- adr/
```

| 文档 | 唯一职责 |
|---|---|
| Index | 简短系统摘要和具体导航 |
| Architecture | 项目概况、系统级结构、依赖方向、capability 到实现单元映射、有证据支撑的边界依据和默认横切机制 |
| Onboarding | 稳定前置条件、命令、可复现项目 baseline，以及有证据支撑的按变更类型交付义务 |
| Capability | 一项领域或业务能力：当前 support、局部行为、边界契约、稳定扩展点、局部边界压力、不变量、证据和测试 |
| Flow | 跨越两个或更多 capability owner 的可复用编排和交接契约 |
| Intent ledger | Intent 来源、目标 capability、阶段 readiness、缺失输入、下一步和可选任务契约差分 |
| External systems | 稳定的来源路由、访问、新鲜度、敏感性和写回策略 |
| Risks | 仅跨能力风险 |
| ADR | 有证据支撑的已接受或明确 proposed 决策 |

能力局部缺口属于 capability 文档。任务专属的缺失决策属于 intent ledger 或 task context。有时间范围的本地环境失败属于运行结果，除非它构成可复现的项目约束。

## Architecture、capability、flow 和实现单元的边界

将每项持久声明恰好分配给以下一个 owner：

| 声明范围 | Canonical owner | 边界规则 |
|---|---|---|
| 系统级结构或默认机制 | Architecture | 负责项目类型、技术栈、部署单元、依赖方向、共享基础设施，以及异常处理、可观测性、安全、缓存、配置和韧性等默认机制。 |
| 一项领域或业务能力 | Capability | 负责其输入、输出、副作用、局部状态、支持变体、局部失败、不变量、实现路径、测试、缺口，以及对架构默认机制的显式例外。 |
| 跨 capability owner 的协调 | Flow | 负责顺序、交接输入和输出、跨边界不变量、事务或一致性边界、失败传播、重试、回滚和外部可见的编排效果。 |

Capability 可以描述其内部 caller-to-effect 时序。只有当至少两个 capability owner 参与，且顺序、交接、事务、重试、回滚或副作用值得独立维护时，才创建独立 flow。Capability 应链接 flow 所拥有的交接契约，而不是再次陈述。Architecture 定义默认机制；capability 只记录领域特定例外；flow 记录失败和状态如何跨 owner 传播。

Capability、flow 和实现单元回答不同问题，彼此是多对多关系：

| 概念 | 回答的问题 | 边界规则 |
|---|---|---|
| Capability | 拥有哪个领域或业务结果？ | 可以跨越实现单元；一个实现单元也可以支持多个 capability。 |
| Flow | Capability owner 如何协调？ | 不意味着需要新的 module、service、package 或 deployment unit。 |
| 实现单元 | 代码、状态或运行时职责放在哪里？ | Architecture 拥有当前映射和依赖方向，而不是业务 support matrix。 |

## 边界与放置证据

根据放置声明的证据和生命周期持久化它：

| 声明 | Canonical 持久化位置 | 规则 |
|---|---|---|
| 当前 capability 到实现单元映射和依赖方向 | Architecture | 使用指定 revision 下的实现证据支撑。 |
| 稳定扩展点、观察到的变更轴或局部边界压力 | Capability；链接 architecture 映射 | 描述可观察结构、耦合和测试，而不是假定的意图。 |
| 原始设计理由 | ADR 或其他决策证据 | 如果不存在可信决策证据，将理由标记为 `unknown`。 |
| 任务专属放置评估 | 运行结果或 task context | 报告适配性、扩展点、压力和开放选择；不要把提案提升为当前状态。 |
| 已接受边界决策 | ADR 或仓库自有决策记录，然后是受影响的当前状态文档 | 实现稳定后刷新映射。 |

针对任务放置，只报告各选项的证据，不替设计工作流作决定：

- 通过已验证接缝扩展现有 capability；
- 只修改跨 capability 编排；
- 职责确实独立时，引入 capability 或实现单元；
- 依赖环、不变量绕过、测试耦合或无关变化原因形成明确压力时，先重构。

设计工作流决定最终放置位置。

## 按需 Deepen 和任务知识充分性

Bootstrap 只映射一到五个高价值 capability。未来请求没有覆盖是正常情况。新任务不在 index 或 intent ledger 中时，执行任务级 Deepen，不要重新 Bootstrap。

开发工作流规划请求前，检查最小匹配路由是否具备所有与任务重要的维度：

| 维度 | 充分证据 |
|---|---|
| Intent 路由 | 一个当前 intent 行标识请求、权威来源或对话快照、目标 owner、阶段 readiness 和缺失决策 |
| Capability 或 flow owner | Canonical owner 已存在，或 Deepen 有足够证据创建 owner，且没有将其等同于目录 |
| 核心验收 | 最小权威请求行为和可观察通过条件明确；有证据支撑的加固项没有被静默提升为产品范围 |
| 入口和效果 | 能追踪请求相关 caller、公共或内部入口、效果/输出及重要副作用 |
| 实现机制 | 重要 symbol、所有权/生命周期、状态或算法行为、数据/并发行为和失败路径达到任务所需深度 |
| 安全约束 | 不变量、兼容面、测试或 benchmark 及已知缺失证据明确 |
| 公共契约差分 | 公共或集成边界可能变化时，重要输入/protocol 及优先级、原始值到规范化值再到 consumer 可见值的语义、输出/错误及失败时机、适用 type/schema/docs/test mirror 和重要负向情形都有证据、已接受决策或明确的 `not-applicable` 结论；mirror 间的语义不一致仍是缺口 |
| 交付义务 | 从 onboarding 链接适用且有证据支撑的按变更类型 gate 或 artifact；不编造不存在的类别 |
| 放置 | 边界可能变化时，当前实现单元映射、依赖方向、扩展点和边界压力可用 |
| 证据快照 | 声明绑定到可复现的有范围 revision 或选定文件快照 |

针对请求的下一阶段应用 readiness：

| Readiness | 含义和允许的下一步 |
|---|---|
| `ready-for-design` | 权威 intent、当前 owner 路由、约束和证据足以让已获授权的设计工作流解决局部选择；此状态尚未授权实现。 |
| `ready-for-implementation` | 核心验收、任务相关机制和安全约束、重要时的公共契约差分、适用交付义务和有范围证据已经充分；不存在未解决的人类产品或权威决策。此状态包含设计 readiness。 |
| `needs-deepen` | 请求阶段所需的仓库或已授权证据缺失、过期或只有路径级信息。 |
| `needs-decision` | 剩余缺口需要人类或外部的产品、policy 或权威决策；Deepen 不得编造。 |
| `blocked` | 访问、环境、不可读状态或其他具体 blocker 阻止所需知识工作。 |

达到 `ready-for-design` 后，已获授权的设计工作流可以解决被委托的局部选择。声明 `ready-for-implementation` 前，在任务上下文或 Deepen 中记录已接受结果。请求阶段已经有充分知识时，返回 `checked-unchanged`。

只有持久请求或需要规范任务入口时，才创建 intent 条目。易变讨论留在任务上下文。`unrouted` 表示“寻找 owner”，不是“再次 Bootstrap”。

## 任务契约差分和交付义务

对于重要任务，Deepen 可以在 intent 路由下加入 `任务契约差异`。这是可选章节，不是新的文档类型。它记录任务要改变什么，以及下一阶段需要知道什么。

| 差分部分 | 仅在重要时纳入 | 边界 |
|---|---|---|
| 核心验收 | 最小权威请求行为和约束、可观察通过条件，以及相关时作为支撑的直接可复现失败 | 失败或实现机制可以支撑契约，但不能定义产品 intent；不得从代码或作者 patch 推断未记录选项。 |
| 公共契约差分 | 输入或 protocol 变体及优先级；原始接受值、runtime 规范化值和 consumer 可见值形状；输出/错误和失败时机；重要 nullable、promised、sentinel、schema、compatibility、runtime/type/docs/test mirror 和负向情形 | 将规范化或 wrapper 移除单列为重要行。对照每个适用 mirror，包括 callback 或 consumer 的 generic/schema type 是否描述规范化后的值；从 capability 或 flow 链接当前行为和机制，并记录不一致以及已接受差分、指定决策或 `not-applicable`。 |
| 风险加固 | 边界、分支、状态、失败、并发、compatibility、性能或安全中有证据支撑的交互 seam | 指导验证，但除非权威 intent 将其提升为核心验收，否则不增加产品行为。对于重要异步顺序，区分 invocation、settlement 和可见顺序；普通 async producer 无法分离它们时，使用可控的对抗性 seam。 |
| 适用交付义务 | 链接 onboarding 中被触发的按变更类型行 | 不复制规则，也不编造通用 gate。 |

任务要替换固定宽度、delimiter、sentinel 或 terminator 边界时，在把某种输入判为无效之前，先表征相邻的已接受输入分区。按适用性检查空 suffix 或 payload、token 本身有效但缺少 delimiter、重复 delimiter，以及 payload 内类似 delimiter 的内容。保留已经验证的当前行为，或记录改变它的权威决定；为了实现方便而新增的 parser error 不能证明兼容。

省略不适用的差异小节。当前路径、机制、不变量、测试清单、support 和局部缺口留在 capability 或 flow。任务专属验证和新探针在 Refresh 前留在差异中。实现稳定后，Refresh 把持久行为和覆盖归入规范 owner，并删除被取代的任务说明。

Bootstrap 期间，查找根指令、manifest 或选中范围引用的 contribution、CI、文档、迁移、安全、基准和发布规则。只读与项目和选中 capability 相关的来源，不要审计所有文档或查询无关企业系统。

存在规则时，onboarding 记录触发条件、必需检查或产物、完成信号和规范来源。已知适用义务缺失时，任务不能达到 `ready-for-implementation`。没有证据时，不得编造 changelog、基准、迁移或评审要求。

不要只看到发布清单，就断定其中所有事项都“仅在发布时”适用。选中范围内存在 changelog 或 release notes 时，检查当前未发布区段和少量相邻条目，再结合贡献或发布指南判断维护者是否会在日常功能开发中持续记录重要的用户可见变化。只记录证据支持的较窄触发条件；版本号、标签、发布等发布仪式仍应单独处理。

任务新增或实质修改的公共示例，如果承诺可执行行为，就必须经过测试、doctest、有记录的命令检查，或明确标为“仅供说明、未经验证”。不要因此审计整个仓库的所有示例。

## Capability 知识覆盖和拆分

对于较宽的 capability，如果读者无法分辨哪些内部区域只是完成定位、哪些已经具备任务所需机制，则条件性加入内部区域地图。Documentation depth 只描述这一知识属性：

| Documentation depth | 必需证据 |
|---|---|
| `mapped` | 已识别区域职责、主要实现单元和代表性入口 |
| `traced` | 已验证相关 caller-to-effect 机制、局部状态或算法、不变量、失败和测试 |
| `unknown` | 现有证据尚不能支持前两种深度 |

规模或复杂度可以说明需要内部区域地图，但不能证明需要拆分。区域服务同一结果并共享生命周期和不变量时，应继续放在一起。只有证据表明存在稳定独立边界时才拆分，例如独立结果、契约、状态、生命周期、不变量、失败边界、owner、变化节奏或长期独立工作。

拆分被接受后，将每项持久声明移动到一个 owner。更新 architecture 映射和 index，重定向受影响的 intent 和 flow，并删除重复正文。未接受的拆分留在任务上下文，或在结果中标为 `candidate-only`。

用户明确请求 capability discovery 时，执行有范围的 Deepen，不增加新模式，也不完整重读仓库。检查重要接口、运行时入口、外部契约、状态 owner 和未映射的实现单元。只持久化有证据支撑的业务或领域结果；弱候选只写入结果，不创建空文档。

## 深水实现证据和有范围的快照

Deepen 只记录实现或修复请求行为所需的机制。优先使用简明地图，不复制源码正文：

| 机制 | Canonical owner 和最低证据 |
|---|---|
| 关键类型和所有权 | Capability：路径加 class、type、trait、interface、function、注册点或 schema symbol；重要时记录职责和生命周期 |
| 局部状态机 | Capability：状态、事件、guard、转换/效果、终止或失败行为，以及 symbol/test 证据 |
| 局部算法 | Capability：入口、阶段、重要分支、终止/fallback、修改状态或输出、不变量，以及 symbol/test 证据 |
| 跨 owner 状态或时序 | Flow：participant 拥有的输入/输出、交接 guard、失败传播、重试/回滚和证据；绝不重复 participant 内部机制 |
| 数据或并发模型 | 按所有权归 Capability 或 Flow：数据形状、序列化、所有权、lock/queue、一致性、取消或资源生命周期 |

将 symbol 作为主要定位方式，行号只作为可选辅助，因为行号会漂移。链接或命名约束机制的测试。不要复制大段实现、生成代码、日志或完整调用图。

在不哈希完整仓库的前提下使证据集可复现。用于标识证据的 digest 必须完整，绝不能缩写：

- 干净 VCS working tree：记录可在目标 worktree 解析的 revision 和任务关键路径/symbol；
- Dirty VCS working tree：记录基础 revision，以及用作证据的任务关键 dirty 或 untracked 文件内容 hash；
- 无 VCS metadata：记录选定关键源文件、manifest、schema 或配置文件的内容 hash；
- 外部或运营证据：重要时保留来源标识、版本/环境和提取时间。

Canonical 文档保留限定当前声明的简明快照；运行结果记录命令和会话环境。稳定实现后的 Refresh 替换过期 hash 或机制，不追加历史快照。

仅包含文档的 commit 不能作为其所描述代码状态的证据。将代码声明绑定到检查时可解析的代码 revision；对于 dirty、untracked 或无 VCS 的证据，按上述规则使用选定文件的完整内容 hash。

## 架构关注项覆盖

Bootstrap 期间，根据仓库证据和项目类型评估以下每个类别：

- 项目概况、用户、项目类型、技术栈、runtime、build、deployment 和扩展模型；
- module、service、package、deployment unit、公共或共享 module 和依赖方向；
- data store、queue、object 或 file storage、cache 和外部集成；
- 依赖与版本管理、configuration、secret 和 feature flag；
- 异常处理；logging、metric、tracing 和 audit；authentication、authorization、trust boundary、input validation 和 sensitive data；
- cache owner、失效和一致性；限流和 backpressure；retry、timeout、circuit breaker 和 fallback；
- 并发、后台 job、scheduler、transaction、CI/CD、health、recovery、migration 和 rollback。

在架构关注矩阵中，纳入已验证机制、重要 unknown 和明确的 `not-applicable` 结论。省略与项目类型无关的类别；不要编造机制或空洞正文。系统默认机制留在这里，capability 特定行为留在 capability 文档，跨边界传播留在 flow 中。

## 托管文档生命周期

模板以 canonical marker `<!-- codebase-knowledge:managed -->` 开头。当文档第一行是同时包含 `codebase-knowledge:` 和 `managed` 的 HTML comment 时，将其视为托管文档；在下一次托管写入时把该行规范化为 canonical marker。没有 managed marker 的文档视为用户撰写，除非用户明确说明其他情况。

- 根据当前证据确定性地重写托管章节。
- 找到替代证据后删除被取代的生成声明。
- 不要在当前状态文档中保留内联历史 baseline。
- 将已接受理由保留在 ADR，将预期执行保留在 spec 或 plan；普通历史依赖 Git。
- 绝不要结构化重写未标记文档。链接它、只外科手术式更新必要陈述，或报告歧义。
- 保持输出幂等：证据不变时不产生内容 diff。

## 项目指令桥接生命周期

解析目标项目根目录后，按以下顺序选择恰好一个指令桥接：

1. 选择现有的 `<target-repository-root>/AGENTS.md`。
2. 否则选择现有的 `<target-repository-root>/CLAUDE.md`。
3. 否则选择并创建 `<target-repository-root>/AGENTS.md`。

两个文件都存在时，选择 `AGENTS.md` 并保持 `CLAUDE.md` 不变。将 `assets/templates/agents-section.md` 作为两种选中文件共用的托管块基础，而不是整文件替换内容。每次成功运行后都保留恰好一个指向当前 index 的选中托管桥接。

绝不修改非选中文件。如果非选中文件包含 managed marker，不执行任何指令文件写入，并报告 unresolved 的双桥接冲突。如果选中文件不可读、不可写、解析到目标根目录之外或 marker 无效，报告 unresolved，且不得回退到另一个文件。

绝不选择或修改 `AGENTS.override.md`；仅当它是适用的项目指令时读取。它可能优先于选中桥接时，将 discovery 报告为 `shadowed`。未被遮蔽的 `AGENTS.md` 选择报告为 `native`，`CLAUDE.md` 选择报告为 `platform-dependent`；后者不声称具备跨工具自动发现能力。

最终托管块包含两个 marker，且不得超过 800 个解码字符。先把换行统一为 LF 并删除末尾 LF，再省略与外部指令重复或冲突的托管条目，最后计数。Marker 外的用户内容不计入预算，也不得为了满足预算而缩短。仍然超限时，不得截断或写入；报告 `unresolved`。

托管块只包含稳定工作约定、一个 index 入口和可观察的工作流条件。强约束词只用于真正的不变量。项目介绍、命令、架构、代码风格、文件布局、capability 状态、任务 readiness、实现路径、测试、历史和事故留在各自的规范文档中。

- 如果恰好存在一组完整且顺序正确的 marker，替换包含 marker 在内的整个块。
- 如果两个 marker 都不存在，追加渲染后的块，不改变现有内容。
- 如果 marker 不完整、反序或重复，不要编辑；报告 unresolved。
- 只将 working agreements 与旧托管块以外的指引比较。存在等价外部指引时省略托管 bullet；发生冲突时保留外部指引并省略托管 bullet。
- 按模板顺序确定性渲染，使未变化的外部指引产生相同块。

## 路由规则

Index 链接 canonical owner，不复制其状态详情。只包含有仓库证据支撑的具体路由。不要生成通用任务类型表、命令输出、重复 unknown 清单或持久的工作流专属 handoff 正文。

请求任务不在 index 或 ledger 中时触发任务范围的 Deepen。只有 ledger 和匹配行存在时才读取匹配 intent；不要仅为满足指令而创建空 ledger。Deepen 先尝试匹配和扩展现有 owner；只有证据支持该 owner 时才创建 capability 或 flow 路由。已有且充分的路由保持不变。

Intent-ledger 行链接 capability 或 flow。其可选任务契约差分不得重复由该文档拥有的实现路径、不变量、现有测试清单、当前 support 或缺口分析。任务专属必需验证或新的可观察 probe 在 Refresh 前保留在差分中。

Flow 不得包含 capability support matrix。其 participant 行链接 canonical capability 文档。Capability 的 participating-flow 行应链接 flow 所拥有的交接契约，而不是复制它。

Architecture 拥有 capability 到实现单元映射和 module 边界依据。Capability 文档链接该映射，只拥有稳定的局部放置证据。Flow 记录编排；创建或修改 flow 本身不能证明需要新的实现单元。

## 最终一致性验证

报告托管写入完成前，验证受影响的 canonical 文档集：

- 每条指令和 index 路由都可解析；只有 ledger 和匹配行存在时才读取匹配 intent；
- 每个重要 support label 都在同一 capability 行中带有已验证的输入、模式或变体边界；
- 每个变化的公共或集成边界都追踪原始值、规范化值和 consumer 可见值，并在适用的 runtime、type/schema、文档/example 和测试 mirror 之间语义一致，否则将不一致作为限制 readiness 的明确缺口；
- 每个持久声明只有一个正文 owner：跨 owner 时序属于 flow，capability 只保留局部不变量和链接；
- 本任务新增或实质修改的每个公共可执行 example 都已执行，或明确标为 illustrative 且 unverified；
- 每个证据 revision 都能在目标 worktree 解析，否则有范围快照使用选定文件完整内容 hash；
- 每个任务差分都区分权威核心验收与有证据支撑的风险加固，并链接适用 onboarding 义务；
- readiness 指明下一安全阶段，且不强于现有证据。

## Refresh 影响矩阵

| 变更 | Canonical owner |
|---|---|
| Setup、build、test、run、稳定项目 baseline 或有证据支撑的按变更类型交付义务 | Onboarding |
| 项目概况、系统边界、module 方向、capability 到实现单元映射、module 边界依据、共享基础设施或默认横切机制 | Architecture |
| Capability 边界、行为、support、documentation depth、内部区域地图、实现机制、局部失败、例外、不变量、实现路径、稳定扩展点、局部边界压力或测试 | Capability |
| 跨 capability 的顺序、交接、失败传播、事务、重试、回滚或副作用 | Flow |
| Requirement、TODO、issue、任务契约差分、缺失决策或阶段 readiness | Intent ledger |
| 跨能力可靠性、安全、兼容性或运营风险 | Risks |
| 企业来源路由或治理 | External systems |
| 已接受 trade-off 和理由 | ADR |
| 任务专属放置评估或未接受提案 | 运行结果或 task context |

## 写作规则

- 链接证据，不要复制代码、日志、问题单或需求。
- 实现机制优先使用路径加 symbol 的证据；行号只作为辅助导航。
- 只有在实质限定声明时，才记录 revision、来源版本、环境和时间。
- 保持表格精简。只有某列能为文档的唯一职责承载证据时才添加。
- 不要创建空章节、推测性图表或回顾性理由。
- 绝不要从代码布局推断原始设计意图。明确区分观察、假设、已接受决策和 unknown 理由。
- 写入有效 UTF-8；报告完成前修复不可读输出。
