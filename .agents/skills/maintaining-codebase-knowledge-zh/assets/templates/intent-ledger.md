<!-- codebase-knowledge:managed -->
# 开发意图 ledger

最后验证：重要时记录 revision 和日期

| Intent | Source snapshot | Target capability or flow | Readiness | Missing input or evidence | Next action |
|---|---|---|---|---|---|
| 功能、Bug、优化、迁移、重构、测试缺口或基础设施 intent | 路径、权威 ID/link、版本，以及重要时的提取时间 | Canonical 文档链接或 unrouted | ready、needs-deepen、needs-decision 或 blocked | 任务专属需求、决策、证据或环境 blocker | Proceed、Deepen、获得决策、获取证据或解决 blocker |

不要将实现路径、不变量、测试清单或能力局部缺口复制进本 ledger。链接它们的 canonical owner。

当新请求需要成为持久 planning 路由时，Deepen 可以创建一行。只在证据尚未识别 owner 时使用 `unrouted`；易变讨论留在 task context。
