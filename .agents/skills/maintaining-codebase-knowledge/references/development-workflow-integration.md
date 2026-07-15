# Development Workflow Integration

## Boundary

This Skill supplies evidence-backed project knowledge. A consuming development workflow owns design, planning, isolation, implementation, TDD, debugging, review, verification, and delivery. Neither requires the other to be installed, and this Skill does not invoke, configure, or modify the consuming workflow.

## Consumption order

Before design, planning, implementation, debugging, or review:

1. Follow the selected project-instruction bridge to `docs/project-knowledge/index.md`.
2. Match the requested work to one intent-ledger row and its linked capability or flow when one exists.
3. Apply the task-knowledge sufficiency gate from `knowledge-model.md`. When the request is absent or unrouted, or its route lacks task-critical entries, implementation mechanics, invariants, tests, placement evidence, or a current scoped snapshot, run task-scoped Deepen automatically. An existing index means this is not a Bootstrap request.
4. Read task context or authorized canonical enterprise sources when the index or intent row requires them.
5. Continue when Readiness is `ready`; run Deepen for `needs-deepen`; obtain the named human decision for `needs-decision`; resolve the named blocker for `blocked`.

Use the capability document for current behavior, internal-area coverage, task-relevant symbols or state/algorithm mechanics, invariants, implementation evidence, and tests. Use the flow for cross-owner handoffs, and the intent row for task-specific missing input and readiness. Do not expect these documents to duplicate one another.

## Placement decisions

When requested work may change boundaries, read the architecture capability-to-unit map and module boundary basis, then the matching capability's change-placement evidence. This Skill reports current ownership, extension seams, boundary pressure, decision evidence, and unknowns; the owning design workflow makes the placement decision.

Compare only evidence-supported choices: extend the existing capability through a seam, change cross-capability orchestration, introduce a separately owned capability or implementation unit, or refactor verified boundary pressure first. Record an accepted material boundary decision in the repository's decision artifact, then use Refresh after stable implementation to update current-state mappings.

## Completion gate

After implementation stabilizes and before completion verification, run Refresh against the diff and material task evidence. Refresh canonical owners only, remove superseded managed claims, and return the grouped Result Contract. The consuming workflow then performs fresh verification and its own completion process.

Ticket transitions, PRD edits, CI actions, review comments, and enterprise status changes require a separately authorized owning workflow. Any development workflow that respects this contract may consume the project-knowledge output.
