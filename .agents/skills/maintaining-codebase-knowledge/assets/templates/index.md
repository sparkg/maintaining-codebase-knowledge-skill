<!-- codebase-knowledge:managed -->
# Project knowledge

Last verified: code revision and date, when relevant

Start here. Use this page to find the smallest set of documents needed for the current task. Detailed status and implementation notes stay in the linked documents, not in this index.

## System at a glance

Use no more than five stable points to explain what the system does, who uses it, how it runs, its main boundary, and where someone unfamiliar with the repository should begin.

## Capability routes

| Capability | Read this document |
|---|---|
| Concrete repository capability | `capabilities/<capability>.md` |

## Intent routes

When the repository contains material requirements, bugs, or backlog work, link `intent-ledger.md` here. Keep readiness and task details in that ledger rather than copying them into the index.

## Cross-capability flows

Link only flow documents that describe a real, reusable sequence across several capabilities.

## Supporting knowledge

Link only documents that exist and materially guide development, such as architecture, onboarding, external systems, risks, a glossary, ADRs, or repository-owned documentation.

For a concrete task, first check whether `intent-ledger.md` contains a matching row. If it does, read that row and follow its link to the relevant capability or flow. If it does not, run task-scoped Deepen. Do not add generic task-type tables, command output, repeated unknowns, or workflow-specific handoff instructions to this index.
