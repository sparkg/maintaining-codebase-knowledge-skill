# 外部和对话上下文

## 边界

企业系统仍是其所拥有记录的权威来源。本 Skill 可以消费用户提供或明确授权的任务范围证据，但不选择已分配工作、不批量同步系统、不流转问题单、不编辑 PRD、不操作 CI，也不写回状态。

只读取请求任务所需内容。绝不持久化凭证、客户数据、私密讨论、受限源文本或仓库策略禁止的内容。

## 接入

只检查当前任务的对话以及可用附件或导出。不要声称可以访问其他对话，也不要推断不可访问链接的内容。

对每个重要项应用 `knowledge-model.md` 中的维度，并且只记录必要信息：

```text
Evidence kind and confidence
Canonical source or task identifier
Source version and provided/retrieved time
Sensitivity
Persistence decision
```

示例：

| 输入 | 处理方式 |
|---|---|
| 已批准 PRD 或验收标准 | 版本化 intent；不是实现证明 |
| 用户关于行为或故障的陈述 | 在获得独立支撑前，是 `intent` 或 `operational` 声明，且 `Confidence: hypothesis` |
| CI、测试、运行时或事故输出 | 只有 command/run、revision、环境和时间得到充分标识时才是 operational evidence |
| 分配、评论、工作流或实时 CI 状态 | 只保留在 task context 或 session |
| 稳定且受治理的规则 | 验证权威性和仓库权限后可成为候选持久知识 |
| 敏感或不可访问内容 | 拒绝持久化或标记 unavailable；绝不推断 |

长对话可能被压缩，因此应在重要且允许的上下文出现时及时捕获，但要提炼而不是复制。

## 持久化门

只有当内容稳定、与项目相关、仓库允许、来源充分且无歧义时，才自动持久化。

- 持久的 capability 行为或不变量进入 canonical capability 文档。
- 任务专属验收、约束和缺失决策进入仓库批准的 task context 或 intent-ledger 行。
- 易变工作流和运营状态保留在外部系统或 session。
- 如果权威性、新鲜度、敏感性或权限不明确，不要持久化；报告该决定及其 readiness 影响。

不要假设 `.agents/task-context/` 已被忽略或可安全提交。写入前检查仓库策略。Task context 只应包含标识、来源快照、提炼后的需求、验收标准、约束、相关运营证据、冲突和未解决问题。

## 外部系统注册表

只有当外部来源实质影响开发时，才创建 `external-systems.md`。记录稳定路由和治理：用途、标识、owner、读取方式、权威来源规则、新鲜度、敏感性、写回权限和失败行为。不要镜像记录或列出 secret 路径。
