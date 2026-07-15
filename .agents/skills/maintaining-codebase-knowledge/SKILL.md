---
name: maintaining-codebase-knowledge
description: Use when taking over an unfamiliar repository, deepening project knowledge for a requested feature or bug, connecting task-scoped enterprise or conversation evidence to implementation, refreshing knowledge after code changes, checking documentation impact before completion, or exposing reliable repository context to development workflows.
---

# Maintain Codebase Knowledge

Maintain an evidence-backed repository map. This Skill owns documentation; the development workflow owns implementation and completion.

## Core rules

- Evidence supports a claim type; sources are not interchangeable facts.
- Keep support, confidence, readiness, persistence, and documentation depth separate.
- Give each durable fact a single canonical document owner; others link to it.
- Treat capabilities, flows, and implementation units as separate concepts; map them instead of assuming one-to-one correspondence.
- Keep managed documents current-state only. Put history in Git, accepted ADRs, specs, or plans.
- Structurally rewrite only documents carrying the managed marker; preserve user-authored material.
- Never create empty sections.

## Load references

Read `references/knowledge-model.md` before writing project knowledge.

Read `references/external-context.md` when conversation, enterprise, CI, incident, or governed evidence affects the task.

Read `references/development-workflow-integration.md` when a design, planning, implementation, debugging, review, verification, or delivery workflow will consume project knowledge.

## Select a mode

| Mode | Use when |
|---|---|
| Bootstrap | The repository is unfamiliar or lacks a reliable knowledge index |
| Deepen | A requested task maps to missing or insufficient capability knowledge |
| Refresh | Stable implementation changed or documentation impact must be checked |

Do not combine first-time discovery with code changes. Modes may read authorized evidence and run safe checks, but must not edit code, expose secrets, mutate external systems, or own workflow state.

## Intake and scope

At the start of every mode:

1. Resolve one target repository root with `references/knowledge-model.md`; state its absolute path and resolution basis before any managed write, and reuse it for the entire run.
2. Select one project instruction bridge with `references/knowledge-model.md`, then read the target repository root's applicable project instructions. Check `<target-repository-root>/docs/project-knowledge/index.md`; when it exists, read it before selecting any intent, capability, flow, or further project files, and record the entrypoint status.
3. State the goal, scope, selected capabilities, exclusions, and success criteria.
4. Inspect the current conversation and attachments; load the external-context reference when material evidence appears.
5. Record the revision and material environment or external-source snapshot.
6. Surface ambiguity that can change planning.

For Bootstrap, a missing index is valid: record `missing`, then record `created` after writing it. An unreadable index stops every mode so unknown existing content is not overwritten. For Deepen or Refresh, a missing index is a mode mismatch: stop and recommend Bootstrap. Never substitute an index from another worktree, the Skill installation directory, or remembered content from an earlier run.

Every successful mode leaves exactly one selected project instruction bridge pointing to the current index. Create `AGENTS.md` only when neither supported instruction file exists; otherwise preserve the selected filename. Validate the final managed block before writing and report unresolved selection, discovery, or marker conflicts without falling back to another file.

## Bootstrap

1. Inventory applicable instructions, manifests, entry points, docs, tests, CI, deployment, and migrations; assess the architecture concerns named in the architecture template without reading every file.
2. Scan TODO/FIXME, backlog or issue-like files, specs, examples, interfaces, schemas, configuration, deprecated code, and test gaps.
3. Identify one to five high-value capabilities and trace representative caller-to-effect paths with tests. Bootstrap is intentionally selective: unselected or future capabilities remain valid on-demand Deepen work, not Bootstrap failures.
4. Create the minimum managed set: one document per selected capability and one linked ledger row per material intent. Create a flow only when two or more capability owners have a sequence, handoff, transaction, retry, or side effect worth maintaining independently; otherwise keep the local sequence in its capability.
5. Simulate two or three likely handoffs. Downgrade readiness when a planner cannot find invariants, tests, missing decisions, or the evidence snapshot.
6. Render and validate the selected project instruction bridge's managed block with `references/knowledge-model.md`, then refresh it only when the selection, budget, and content checks pass. Finish when selected capabilities have evidence-backed current state, material selected intents have one route, and durable facts have one prose owner; do not expand the inventory merely to anticipate future work.

## Deepen

1. Start from one requested task or intent-ledger row; do not re-bootstrap. A new or unrouted request is a valid Deepen input.
2. Run the task-knowledge sufficiency gate in `references/knowledge-model.md`. If the route and task-critical evidence are already sufficient, report `checked-unchanged` instead of expanding documentation.
3. Read only canonical sources and implementation paths needed for planning. Trace the relevant caller-to-effect path and, when material, key symbols, ownership/lifecycle, local state transitions, algorithm branches and termination, data/concurrency behavior, failure paths, invariants, and test seams. Use symbol-level evidence and a scoped reproducible snapshot; do not copy implementation bodies.
4. Separate intent from support and report conflicts. Create or route the intent when absent; deepen an existing capability before considering a new one.
5. For a broad capability, record its material internal areas and current documentation depth. Split only with evidence of a separately owned outcome, contract, lifecycle, invariants, change axis, or repeatedly independent work; size alone is not evidence. On an explicit capability-discovery request, persist only verified capabilities and keep weak candidates in the run result.
6. Update the intent row and smallest capability/flow set. Create or deepen a flow only for material coordination across two or more capability owners. Update index and architecture mappings only when a canonical capability or flow is created or split; keep task discussion and volatile state in task context or the result.
7. Finish when `index -> intent row -> capability/flow` exposes entry points, task-relevant implementation mechanics, invariants, tests, missing evidence, human decisions, and—when boundaries may change—placement evidence without pre-empting the design decision.

## Refresh

1. Inspect the diff, affected tests, runtime paths, intent artifacts, and relevant snapshots.
2. Map changed claims to owners with `references/knowledge-model.md`.
3. Replace stale managed state; never append historical baselines or duplicate handoff. Before an affected project instruction bridge write, validate the selected file and final rendered managed block with `references/knowledge-model.md`.
4. Change readiness only when planning safety changed. Keep local gaps in capability docs and cross-capability risks in `risks.md`.
5. Report `checked-unchanged` when durable knowledge did not change.
6. Run Refresh after implementation stabilizes and before completion verification; it is not proof that code works.

## Result contract

End every run with:

```text
Mode and goal: <mode, scope, success criteria>
Target repository root: <absolute path and resolution basis>
Knowledge entrypoint:
  Path: <absolute path>
  Status: read | created | missing | unreadable
Project instruction bridge:
  Path: <absolute path>
  Selection basis: existing AGENTS.md | existing CLAUDE.md | created AGENTS.md
  Discovery: native | platform-dependent | shadowed
  Managed block budget: <decoded characters>/800
  Status: written | unchanged | unresolved
Evidence snapshot: <revision, commands, environment, source versions/times>
Context evidence:
  Considered: <items or none>
  Persisted: <canonical paths and distilled claims or none>
  Rejected or session-only: <items and reasons or none>
  Conflicts or unavailable: <items or none>
Knowledge changes:
  Updated: <paths or none>
  Removed as stale: <claims or paths or none>
  Checked unchanged: <paths or none>
Planning:
  Readiness: ready | needs-deepen | needs-decision | blocked
  Read first: <index, intent row, capability or flow>
  Unresolved decisions: <questions or none>
  Recommended next action: <one action>
Placement evidence (when placement or boundaries matter):
  Matching capability or flow: <canonical link or none>
  Primary and supporting implementation units: <units and links or unknown>
  Existing extension seam: <evidence-backed seam or none observed>
  Boundary pressure: <observed pressure or none observed>
  Design decision still required: <question or none>
Deepening evidence (for Deepen):
  Knowledge route: <intent existing|created|updated; capability/flow matched|expanded|created|split>
  Sufficiency before Deepen: sufficient | insufficient; <missing dimensions or none>
  Key implementation symbols: <symbols and canonical section or none>
  State machine or algorithm map: <canonical section or none>
  Scoped evidence snapshot: <revision plus dirty-file hashes, or selected file hashes>
  Capability structure: <expanded area; split|keep-together|candidate-only|unchanged; evidence basis>
```

## Common mistakes

| Mistake | Correction |
|---|---|
| TODO or user report treated as implementation truth | Verify intent and support separately. |
| Status copied across index, ledger, capability, risk, and handoff | Store once and link. |
| Before-and-after state kept in a current document | Replace stale managed content; use history artifacts. |
| Local missing tool turned into project debt | Keep it in the result unless it is a reproducible project constraint. |
| Unmarked user document structurally rewritten | Update surgically or report unresolved. |
| Capability treated as a code module, or observed layout presented as design intent | Map capability, flow, and implementation units separately; leave task-specific placement to design. |
| Missing task route treated as a need to re-bootstrap | Run task-scoped Deepen and create the smallest verified route. |
| Broad capability split because it has many files or lines | Record internal coverage and require stable ownership or boundary evidence before splitting. |
| Paths listed without task-relevant mechanics | Add key symbols, state or algorithm behavior, invariants, tests, and a scoped evidence snapshot. |
