# 项目知识模型

## 目录

- 证据维度和声明权威来源
- 目标项目根目录和输出边界
- 知识入口和项目指令桥接
- 结构和文档职责
- Architecture、capability、flow 和实现单元的边界
- 按需 Deepen 和任务知识充分性
- Capability 知识覆盖和拆分
- 深水实现证据和有范围的快照
- 架构关注项覆盖
- 托管文档和项目指令桥接生命周期
- 路由和 Refresh 影响
- 写作规则

## 证据维度

绝不要将这些维度合并为一个状态：

| 维度 | 取值 | 含义 |
|---|---|---|
| Evidence kind | `implementation`、`intent`、`operational`、`decision` | 来源可以支撑哪类声明 |
| Confidence | `verified`、`hypothesis`、`unknown` | 现有证据对声明的支撑强度 |
| Support | `supported`、`partial`、`missing`、`unknown` | 能力的当前实现状态 |
| Readiness | `ready`、`needs-deepen`、`needs-decision`、`blocked` | 特定 intent 是否可以安全进入 planning |
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

在接入阶段解析唯一的目标项目根目录，将其规范化为绝对路径，声明解析依据，并在 Bootstrap、Deepen 和 Refresh 中复用。按以下优先级处理：

1. 使用用户或任务显式指定的现有目录。显式指定的 monorepo 子项目优先于包含它的 VCS 根目录。
2. 没有显式目标时，使用适用的 VCS 原生根目录查询所报告的包含当前任务的 VCS 根目录。
3. VCS 发现不可用时，只有当项目指令、manifest 或入口能支撑恰好一个项目根目录候选时，才使用该祖先目录。
4. 没有候选或仍有多个候选时，将解析报告为 unresolved，并且不执行任何托管写入。

进程工作目录、Skill 源文件或模板路径，以及 agent 配置目录的存在本身都不是项目根目录证据。特别是，不能仅因 Skill 安装在 `.opencode`、`.agents`、`.claude` 或 `.codex` 中或从中调用，就将其作为目标；只有用户或任务将该目录显式指定为项目时才能选择它。

将每个托管输出锚定到解析后的根目录：选中的项目指令桥接、`<target-repository-root>/docs/project-knowledge/**`，以及仓库批准的 task-context 路径。写入前验证规范化后的目标仍位于解析后的根目录或其下方。不得为不同文件或模式独立地重新解析根目录。

## 知识入口和项目指令桥接

`<target-repository-root>/docs/project-knowledge/index.md` 是具体项目知识路由的 canonical 入口。存在时，本 Skill 在接入阶段直接读取它；不能把这项职责委托给 `AGENTS.md`，也不能根据之前的上下文假定已经读取。

选中的根项目指令文件中的托管块是稳定的下游项目指令桥接。它告诉开发工作流何时读取 index，但不能证明某次运行确实读取了它。保持该桥接简短：绝不能把 index 的路由、capability 状态、实现路径、测试清单、任务专属 readiness 值或 handoff 状态复制进该指令文件。

这些职责相互补充。Skill 直接接入确保 Bootstrap、Deepen 和 Refresh 使用正确且当前的知识入口；项目指令桥接使后续工作流无需用户重复编写 prompt 即可发现它。对于不加载项目指令文件的平台，不能通过把知识复制到模型 memory 或 Skill metadata 中来迫使其加载。

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
| Onboarding | 稳定前置条件、命令和可复现项目 baseline |
| Capability | 一项领域或业务能力：当前 support、局部行为、边界契约、稳定扩展点、局部边界压力、不变量、证据和测试 |
| Flow | 跨越两个或更多 capability owner 的可复用编排和交接契约 |
| Intent ledger | Intent 来源、目标 capability、planning readiness、缺失输入和下一步 |
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

对于任务专属 planning，报告以下选项的证据而不代为选择：通过既有扩展点扩展现有 capability；只修改跨 capability 编排；当状态、生命周期、策略、公共接口或变更节奏有独立 owner 时引入 capability 或实现单元；或者在无关变更原因、依赖环、不变量绕过或测试耦合形成已验证边界压力时进行重构。负责设计的工作流作出放置决策。

## 按需 Deepen 和任务知识充分性

Bootstrap 有意只映射一到五个高价值 capability。未选择或未来请求缺少覆盖是正常状态。绝不能只因新任务不在 index 或 intent ledger 中就重新运行 Bootstrap；应针对现有入口执行任务范围的 Deepen。

开发工作流规划请求前，检查最小匹配路由是否具备所有与任务重要的维度：

| 维度 | 充分证据 |
|---|---|
| Intent 路由 | 一个当前 intent 行标识请求、权威来源或对话快照、目标 owner、readiness 和缺失决策 |
| Capability 或 flow owner | Canonical owner 已存在，或 Deepen 有足够证据创建 owner，且没有将其等同于目录 |
| 入口和效果 | 能追踪请求相关 caller、公共或内部入口、效果/输出及重要副作用 |
| 实现机制 | 重要 symbol、所有权/生命周期、状态或算法行为、数据/并发行为和失败路径达到任务所需深度 |
| 安全约束 | 不变量、兼容面、测试或 benchmark 及已知缺失证据明确 |
| 放置 | 边界可能变化时，当前实现单元映射、依赖方向、扩展点和边界压力可用 |
| 证据快照 | 声明绑定到可复现的有范围 revision 或选定文件快照 |

任何重要维度缺失、过期或只有路径级信息时，将 Readiness 设为或保持 `needs-deepen`，并更新最小 canonical 集。如果唯一缺口是人类产品或设计决策，使用 `needs-decision`；Deepen 不得编造决策。所有维度已经充分时，报告 `checked-unchanged`，不要为了完整性扩展文档。

对于新请求，只有它是持久证据或需要成为 canonical planning 路由时才创建或更新 intent 行；易变讨论留在 task context。`unrouted` 行是寻找 owner 的提示，不是仓库需要再次 Bootstrap 的证据。

## Capability 知识覆盖和拆分

对于较宽的 capability，如果读者无法分辨哪些内部区域只是完成定位、哪些已经具备任务所需机制，则条件性加入内部区域地图。Documentation depth 只描述这一知识属性：

| Documentation depth | 必需证据 |
|---|---|
| `mapped` | 已识别区域职责、主要实现单元和代表性入口 |
| `traced` | 已验证相关 caller-to-effect 机制、局部状态或算法、不变量、失败和测试 |
| `unknown` | 现有证据尚不能支持前两种深度 |

文件数、代码行数或概念复杂度可以触发内部区域地图，但绝不能证明需要拆分。如果区域贡献于同一个 owned outcome，并共享生命周期和不变量，则保留在现有 capability 中。只有实现、决策或反复任务证据支持至少一个稳定独立边界时才拆分：独立拥有的业务结果或外部契约；独立状态或生命周期；不同不变量或失败边界；独立 owner 或变化节奏；无关的变化原因；或反复独立的开发工作。

拆分有依据时，将每项持久声明移动到恰好一个 owner，更新 architecture 多对多映射和 index 路由，重定向受影响 intent 行与 flow，并删除原 owner 中的重复正文。没有已接受证据的拆分提案留在 task context，或在运行结果中标为 `candidate-only`。

用户显式请求 capability discovery 是 Deepen 的有范围变体，不是新模式，也不是完整重读仓库。检查当前路由未表示的重要公共接口、runtime 入口、外部契约、有状态 owner 和重要实现单元。只持久化有证据支撑的业务或领域结果；报告弱候选而不创建空文档。

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

- 干净 VCS working tree：记录 revision 和任务关键路径/symbol；
- Dirty VCS working tree：记录基础 revision，以及用作证据的任务关键 dirty 或 untracked 文件内容 hash；
- 无 VCS metadata：记录选定关键源文件、manifest、schema 或配置文件的内容 hash；
- 外部或运营证据：重要时保留来源标识、版本/环境和提取时间。

Canonical 文档保留限定当前声明的简明快照；运行结果记录命令和会话环境。稳定实现后的 Refresh 替换过期 hash 或机制，不追加历史快照。

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

最终渲染的托管块必须不超过 800 个字符，并包含两个 marker。计数前移除末尾的 CR/LF 字符，再计算解码后原始 Markdown 的字符数；应在省略与外部指令重复或冲突的托管 bullet 后测量。Marker 外的内容由用户撰写，不计入预算，也不得为了让托管块满足预算而缩短。如果最终托管块超过 800 个字符，不得截断或写入；将项目指令桥接报告为 unresolved。

托管块只保留稳定的关键工作约定、一个项目知识 index 入口和可观察的工作流条件。只有真正的不变量才使用强约束词。项目介绍、精确命令、架构详情、代码风格示例、文件组织、capability 状态、任务专属 readiness 值、实现路径、测试清单、历史和一次性事故仍由各自的 canonical 项目知识文档负责，并通过 index 访问。

- 如果恰好存在一组完整且顺序正确的 marker，替换包含 marker 在内的整个块。
- 如果两个 marker 都不存在，追加渲染后的块，不改变现有内容。
- 如果 marker 不完整、反序或重复，不要编辑；报告 unresolved。
- 只将 working agreements 与旧托管块以外的指引比较。存在等价外部指引时省略托管 bullet；发生冲突时保留外部指引并省略托管 bullet。
- 按模板顺序确定性渲染，使未变化的外部指引产生相同块。

## 路由规则

Index 链接 canonical owner，不复制其状态详情。只包含有仓库证据支撑的具体路由。不要生成通用任务类型表、命令输出、重复 unknown 清单或持久的工作流专属 handoff 正文。

请求任务不在 index 或 ledger 中时触发任务范围的 Deepen。Deepen 先尝试匹配和扩展现有 owner；只有证据支持该 owner 时才创建 capability 或 flow 路由。已有且充分的路由保持不变。

Intent-ledger 行链接 capability 或 flow。它不得重复由该文档拥有的实现路径、不变量、测试清单或缺口分析。

Flow 不得包含 capability support matrix。其 participant 行链接 canonical capability 文档。Capability 的 participating-flow 行应链接 flow 所拥有的交接契约，而不是复制它。

Architecture 拥有 capability 到实现单元映射和 module 边界依据。Capability 文档链接该映射，只拥有稳定的局部放置证据。Flow 记录编排；创建或修改 flow 本身不能证明需要新的实现单元。

## Refresh 影响矩阵

| 变更 | Canonical owner |
|---|---|
| Setup、build、test、run 或稳定项目 baseline | Onboarding |
| 项目概况、系统边界、module 方向、capability 到实现单元映射、module 边界依据、共享基础设施或默认横切机制 | Architecture |
| Capability 边界、行为、support、documentation depth、内部区域地图、实现机制、局部失败、例外、不变量、实现路径、稳定扩展点、局部边界压力或测试 | Capability |
| 跨 capability 的顺序、交接、失败传播、事务、重试、回滚或副作用 | Flow |
| Requirement、TODO、issue、缺失决策或 planning readiness | Intent ledger |
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
