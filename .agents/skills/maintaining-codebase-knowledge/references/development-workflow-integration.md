# Using Project Knowledge in a Development Workflow

## Division of responsibility

This Skill supplies project knowledge. The development workflow that reads it remains responsible for design, planning, workspace isolation, implementation, testing, debugging, review, verification, and delivery. The two are independent: this Skill does not invoke, configure, or modify the development workflow.

## What to read before development

Before design, implementation, debugging, or review:

1. Open `docs/project-knowledge/index.md` by following the instruction in the repository's selected `AGENTS.md` or `CLAUDE.md`.
2. If the index contains the requested work, read its intent row and the linked capability or flow documents. If it does not, run task-scoped Deepen directly from the request. Do not try to read an intent ledger that does not exist.
3. Use the sufficiency checks in `knowledge-model.md` for the stage you want to enter. Run Deepen when the request is not recorded, is still `unrouted` because it does not point to a responsible document, or lacks enough detail. If the index already exists, the repository does not need another Bootstrap for this task.
4. Read the current task context or an authorized enterprise source only when the index or intent row points to it.
5. Follow the readiness result:
   - `ready-for-design`: continue with design.
   - `ready-for-implementation`: continue with implementation; the requirements for design are already included.
   - `needs-deepen`: run Deepen.
   - `needs-decision`: obtain the named product or authority decision.
   - `blocked`: remove the named blocker.

An authorized design workflow may make local choices that have been delegated to it. Record an accepted choice in the task context or through Deepen before treating the task as ready for implementation.

Each document answers a different question:

- capability: what the capability currently does, how its own code works, which invariants hold, and how it is tested;
- flow: how work is ordered and handed off between capabilities;
- intent row: what the task requests, how it will be accepted, which task-specific risks remain, what input is missing, and whether work can continue;
- onboarding: which setup, verification, and delivery requirements apply.

Link between these documents instead of copying the same fact into several places.

## Deciding where a code change belongs

If a task may change a boundary, first read the architecture capability-to-implementation map and the evidence for the current boundary. Then read the relevant capability's placement evidence. This Skill reports current responsibilities, existing extension points, signs of boundary pressure, decision records, and unknowns. The design workflow chooses where the change should go.

Compare only choices that the repository evidence supports:

- extend the capability through an existing extension point;
- change orchestration between capabilities;
- introduce a capability or implementation unit with a separate responsibility; or
- first refactor a boundary that evidence shows is already under pressure.

Record an accepted, material boundary decision in the repository's decision record. After implementation stabilizes, use Refresh to update the current map.

## Checks before completion

After implementation stabilizes, run Refresh against the diff and task evidence. Update only the documents that own changed facts, remove statements that the implementation has replaced, and return the grouped Result Contract.

Before completion, the development workflow must:

1. verify every delivery requirement linked from the task;
2. stop if any required signal is still failing or missing; and
3. run fresh implementation checks and its own completion process.

Moving a ticket, editing a PRD, starting CI, posting a review comment, or changing enterprise workflow state requires separate authorization in the system responsible for that action. Any development workflow may use the project knowledge as long as it follows these boundaries.
