<!-- codebase-knowledge:managed -->
# 企业外部系统

最后验证：重要时记录来源版本和日期

只有当外部系统实质影响开发时才创建本文件。不要镜像记录或存储凭证。

| System | Purpose | Canonical identifiers | Owner | Read method | Source-of-truth rule | Freshness | Sensitivity | Write-back authority | Failure behavior |
|---|---|---|---|---|---|---|---|---|---|
| Platform 或 service | Task、PRD、CI、incident、policy 或文档证据 | 稳定 ID 或允许的 link pattern | 职责团队 | 已授权 connector、CLI、API、browser 或 export | 控制记录/版本 | 重新读取条件 | 已批准分类 | 独立职责工作流 | 标记 unavailable、stale 或 ambiguous |

## Task-context 策略

记录批准的位置、保留期、脱敏、允许字段和 owner。如果没有安全持久化策略，将证据保留在 session，并只保留允许的来源信息和未解决问题。
