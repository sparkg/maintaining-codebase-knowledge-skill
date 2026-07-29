# 维护代码库知识

[English](README.md)

这个 Skill 帮助编码智能体接手现有代码仓，并在代码变化后保持对项目现状的了解。它把散落在代码、测试、配置、文档和命令结果中的事实整理成一组简明的项目文档，供开发前查阅。

第一次扫描不会试图写全整个仓库。Bootstrap 只建立一份可导航的基线；收到具体功能或 Bug 后，再用 Deepen 查清相关区域。这个 Skill 负责维护上下文，不负责设计方案、修改生产代码或代替人做决定，也不能取代测试和评审。

## 三种模式

| 模式 | 什么时候使用 | 做什么 |
|---|---|---|
| `Bootstrap` | 第一次接手仓库，或者仓库还没有可靠的知识索引。 | 记录项目规则、架构、常用命令、一到五项值得优先了解的能力，以及确实跨越多项能力的流程。未来任务涉及的其他区域留给 Deepen。 |
| `Deepen` | 已经拿到具体功能、Bug 或维护任务。 | 围绕任务查清相关能力或流程，补齐开发需要的验收条件、代码路径、实际行为、风险、测试和交付规则。 |
| `Refresh` | 实现已经稳定。 | 查看代码变更和验证结果，只更新已经改变的事实。Refresh 不能代替重新运行测试。 |

## 快速开始

### 1. 安装一个语言版本

查看本仓库提供的 Skill：

```shell
npx skills add sparkg/maintaining-codebase-knowledge-skill --list
```

在当前项目中安装英文版或中文版：

```shell
# 英文版
npx skills add sparkg/maintaining-codebase-knowledge-skill --skill maintaining-codebase-knowledge

# 中文版
npx skills add sparkg/maintaining-codebase-knowledge-skill --skill maintaining-codebase-knowledge-zh
```

使用 `--agent codex` 可明确指定 Codex，使用 `--global` 可安装到用户级目录，使用 `-y` 可跳过确认。一个目标仓库只需要安装一个语言版本。

如需手动安装，请克隆或下载本仓库，再把以下任意一个目录复制到目标仓库的 `.agents/skills/` 下：

- `.agents/skills/maintaining-codebase-knowledge/`
- `.agents/skills/maintaining-codebase-knowledge-zh/`

同一个仓库不要同时使用两个版本。

### 2. 建立仓库基线

替换提示词中的 `path/to/repository`：

```text
在 path/to/repository 上以 Bootstrap 模式使用 $maintaining-codebase-knowledge-zh。根据仓库证据建立项目知识基线，不要修改生产代码。
```

Bootstrap 会使用仓库现有的 `AGENTS.md` 或 `CLAUDE.md` 作为项目指令入口；如果两者都不存在，则创建 `AGENTS.md`。以后处理开发任务时，智能体会先读取这个文件，再按其中的指引打开 `docs/project-knowledge/index.md`。

### 3. 为功能或 Bug 补充知识

```text
针对 path/to/repository 中的这个任务，以 Deepen 模式使用 $maintaining-codebase-knowledge-zh：<功能或 Bug>。只更新与任务有关的意图、能力、流程和证据。
```

Deepen 从已有索引开始。如果索引还没有指向与任务相关的文档，或现有文档不够深入，它只补充缺失的内容，不会重新扫描整个仓库。

### 4. 实现完成后更新文档

```text
在 path/to/repository 的实现稳定后，以 Refresh 模式使用 $maintaining-codebase-knowledge-zh。检查 diff 和验证证据，只更新受影响的项目知识。
```

Refresh 完成后，开发流程仍需重新运行测试和其他交付前检查。文档更新不能证明代码正确。

## Bootstrap 生成哪些文档

典型的 Bootstrap 会在目标仓库中创建或复用以下结构：

```text
AGENTS.md or an existing CLAUDE.md
docs/project-knowledge/
├── index.md
├── architecture.md
├── onboarding.md
├── intent-ledger.md
├── risks.md
├── capabilities/
└── flows/
```

| 文档 | 它回答的问题 |
|---|---|
| `AGENTS.md` 或 `CLAUDE.md` | 智能体应该从哪里开始，需要遵守哪些工作约定？ |
| `index.md` | 当前任务应该读哪份文档？ |
| `architecture.md` | 系统由哪些部分组成，依赖关系如何，代码分别负责哪些能力？ |
| `onboarding.md` | 如何配置、运行和测试项目？不同类型的代码变更还要完成哪些交付动作？ |
| `intent-ledger.md` | 当前有哪些需求、Bug 和待办？每项工作应该查阅哪份能力或流程文档？还有哪些问题待确认？ |
| `risks.md` | 哪些风险会同时影响多个功能？它们会怎样影响可靠性、安全、兼容性或运维？ |
| `capabilities/` | 某项业务能力有什么表现，代码在哪里，如何验证？ |
| `flows/` | 一项操作经过多个能力时，执行顺序是什么？各能力如何交接，失败和副作用如何传递？ |

只有外部信息确实影响开发时，Skill 才会创建 `external-systems.md`。只有确实存在一项已采纳或明确提出的决策，并且有证据可查时，才会创建 ADR。某个区域没有在 Bootstrap 中记录，并不等于扫描失败；等任务涉及它时，可以再用 Deepen 补充。

## 如何接入开发流程

```mermaid
flowchart LR
    A[Bootstrap 项目知识] --> B[收到功能、Bug 或维护任务]
    B --> C[Deepen 相关能力或流程]
    C --> D[设计、实现、测试和评审]
    D --> E[Refresh 已变化的项目知识]
    E --> F[验证并交付]
    F --> B
    C -->|needs-decision or blocked| G[补充决策、证据、权限或环境]
    G --> C
```

开发流程先读取项目指令文件，再从索引找到与任务有关的文档。执行结果中的就绪状态（readiness）会说明能否继续：

- `ready-for-design`：现有事实足以支持局部设计。
- `ready-for-implementation`：任务契约、实现机制、验证方式和交付条件已经足够清楚，可以开始编码。
- `needs-deepen`、`needs-decision` 或 `blocked`：结果中会写明还缺什么。

Skill 只提供这份交接信息。它不绑定某个特定开发流程，也不会接管设计和实现。

## 如何保证文档可信

不同结论要使用不同证据。当前实现机制应回到代码核对；已经验证的行为应有测试或刚刚执行的命令结果；对外约定应查看配置和 schema；需求目标则应来自获得授权的需求来源。

一条需要长期保留的事实只在一份文档中完整说明，其他文档通过链接引用。业务能力、跨能力流程和代码模块分开记录，因为它们回答的问题不同。

如果任务会改变对外接口或其他集成边界，Deepen 会核对运行时行为、类型、schema、示例、文档和测试中适用的部分。需求明确要求的行为与代码或测试提示的额外加固工作也会分开记录。

无论仓库是否使用版本控制、工作区是否干净，Skill 都会用指定版本或内容哈希标明证据来自哪一份代码。它保留用户原文，只改写自己管理的文件或标记区块。它不会保存密钥等敏感信息、虚构历史理由、修改生产代码，也不会把 Refresh 当成代码正确的证明。

## 企业系统与对话信息

用户明确提供或授权访问的 PRD、问题单、CI 结果、事故记录、对话内容和文档快照，都可以作为输入。Skill 只记录开发所需的稳定事实，并保留足够的来源信息，方便回到原始记录核对。企业系统仍是这些信息的权威来源。

允许读取不等于允许写入。变更流程状态、发表评论、触发 CI 或进行其他外部操作，仍需单独授权。凭据、客户数据、私密讨论、受限原文以及仓库策略禁止保存的内容不会写入项目知识。经常变化的状态应留在原系统或当前任务上下文中。

## 本仓库结构

- `.agents/skills/maintaining-codebase-knowledge/`：英文版 Skill。
- `.agents/skills/maintaining-codebase-knowledge-zh/`：与英文版行为等价的中文版 Skill。
- `SKILL.md`：模式、边界、输入方式和结果格式。
- `references/`：知识模型和接入规则。
- `assets/templates/`：受 Skill 管理的文档模板。
- `agents/openai.yaml`：面向智能体的元数据。

## 如何验证

静态检查覆盖包格式、UTF-8、双语结构、托管标记，以及是否依赖某个特定开发框架。这些检查只能发现打包和结构问题，不能证明生成的项目知识好用。

行为验证会让一个新的开发智能体处理可重复执行的仓库任务，并且只向它提供任务、仓库、生成的文档和预先声明的工具链。完成后，再用检查目标行为的独立测试、完整回归测试、探索记录和代码评审，与没有文档的基线比较。只有开发质量不下降，文档修改才会被接受。

## 参与贡献

两个语言版本必须保持行为等价。可以先调整英文结构，但中文版应按中文习惯表达，不必逐词照搬英文。两边的规则、状态、字段和停止条件必须一致。

提交前，请运行两个官方验证器、双语结构检查，以及受本次修改影响的独立下游场景。每条事实只放在一个位置，避免重复说明，也不要把某个目标项目的专属知识写入可复用 Skill。

## 许可证

本项目采用 [MIT 许可证](LICENSE)。
