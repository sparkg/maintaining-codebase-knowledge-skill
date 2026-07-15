# 开发工作流集成

## 边界

本 Skill 提供有证据支撑的项目知识。消费方开发工作流负责设计、规划、隔离、实现、TDD、调试、评审、验证和交付。双方都不要求另一方必须安装，本 Skill 不调用、配置或修改消费方工作流。

## 消费顺序

在设计、规划、实现、调试或评审前：

1. 沿选中的项目指令桥接进入 `docs/project-knowledge/index.md`。
2. 存在匹配项时，将请求工作匹配到一个 intent-ledger 行及其链接的 capability 或 flow。
3. 应用 `knowledge-model.md` 中的任务知识充分性门。请求缺失或尚未路由，或者路由缺少任务关键入口、实现机制、不变量、测试、放置证据或当前有范围快照时，自动执行任务范围的 Deepen。Index 已存在意味着这不是 Bootstrap 请求。
4. 当 index 或 intent 行要求时，阅读 task context 或已授权的权威企业来源。
5. Readiness 为 `ready` 时继续；`needs-deepen` 时运行 Deepen；`needs-decision` 时获得指定的人类决策；`blocked` 时解决指定 blocker。

使用 capability 文档了解当前行为、内部区域覆盖、任务相关 symbol 或状态/算法机制、不变量、实现证据和测试。使用 flow 了解跨 owner 交接，使用 intent 行了解任务专属缺失输入和 readiness。不要期待这些文档相互复制。

## 放置决策

当请求工作可能改变边界时，阅读 architecture 的 capability 到实现单元映射和 module 边界依据，再阅读匹配 capability 的变更放置证据。本 Skill 报告当前职责、扩展点、边界压力、决策证据和 unknown；负责设计的工作流作出放置决策。

只比较有证据支撑的选项：通过扩展点扩展现有 capability、修改跨 capability 编排、引入拥有独立职责的 capability 或实现单元，或者先重构已验证的边界压力。重要时在仓库的决策 artifact 中记录已接受的边界决策，然后在实现稳定后使用 Refresh 更新当前状态映射。

## 完成门

实现稳定后、完成验证前，针对 diff 和重要任务证据运行 Refresh。只刷新 canonical owner，删除被取代的托管声明，并返回分组 Result Contract。随后由消费方工作流执行新的验证及其自身的完成流程。

问题单流转、PRD 编辑、CI 操作、评审评论和企业状态变更需要单独授权的职责工作流。任何遵守本契约的开发工作流都可以消费项目知识输出。
