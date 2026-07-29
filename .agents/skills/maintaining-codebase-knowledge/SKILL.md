---
name: maintaining-codebase-knowledge
description: Use when taking over an unfamiliar repository, deepening project knowledge for a requested feature or bug, connecting task-scoped enterprise or conversation evidence to implementation, refreshing knowledge after code changes, checking documentation impact before completion, or exposing reliable repository context to development workflows.
---

# Maintain Codebase Knowledge

Build a reliable route from a development request to the code, behavior, constraints, and tests that matter. This Skill maintains project knowledge. It does not design or implement the change.

## Rules that always apply

- Match every claim to evidence that can support it. Intent does not prove implementation; code does not prove intended behavior.
- Track support, confidence, readiness, persistence, and documentation depth separately.
- Give each durable fact one canonical document owner. Link to that owner elsewhere.
- Keep capabilities, cross-capability flows, and implementation units distinct.
- Describe the current state. Keep history in Git, accepted decisions, specs, or plans.
- Rewrite only marked managed documents or blocks. Preserve user-authored content.
- Omit empty sections.

## References to read

Read `references/knowledge-model.md` before writing project knowledge.

Also read:

- `references/external-context.md` when conversation, enterprise, CI, incident, or governed evidence matters.
- `references/development-workflow-integration.md` when another workflow will use the knowledge for design, implementation, debugging, review, verification, or delivery.

## Choose one mode

| Mode | Use it when |
|---|---|
| Bootstrap | The repository is unfamiliar or has no reliable knowledge index |
| Deepen | A feature, bug, or maintenance task needs more detailed knowledge |
| Refresh | Stable implementation changed, or documentation impact must be checked |

Do not mix first-time discovery with code changes. Every mode may read authorized evidence and run safe checks. No mode may edit production code, expose secrets, mutate external systems, or own development workflow state.

## Before any mode

1. Resolve one target repository root. Report its absolute path and why it was selected. Use the same root for the whole run.
2. Select one root instruction bridge as defined in `references/knowledge-model.md`. Read the applicable project instructions.
3. Check `<target-repository-root>/docs/project-knowledge/index.md` directly. If it exists, read it before choosing more project files.
4. State the goal, scope, selected capabilities, exclusions, and observable success criteria.
5. Inspect the current conversation and attachments. Load the external-context reference when they contain material evidence.
6. Record the revision and any environment or external-source details that qualify the evidence.
7. Surface ambiguity that could change the plan.

Index handling is strict:

- Bootstrap may start with a missing index and create it.
- Deepen and Refresh require an existing readable index. Otherwise stop and recommend Bootstrap.
- An unreadable index stops every mode.
- Never borrow an index from another worktree, the Skill installation, or memory.

Every successful run leaves exactly one selected root instruction bridge pointing to the current index. Create `AGENTS.md` only when neither supported instruction file exists. Validate the full managed block before writing it. Do not switch files to work around a conflict.

## Bootstrap

### Goal

Create a selective, evidence-backed baseline that routes future work. Do not try to document every future capability.

### Steps

1. Inspect applicable instructions, manifests, entry points, docs, tests, CI, deployment, and migrations. Check the architecture concerns and delivery-obligation sources described in the knowledge model.
2. Scan task and product signals such as TODO/FIXME, backlog-like files, specs, examples, interfaces, schemas, configuration, deprecated code, and test gaps.
3. Select one to five high-value capabilities. For each, trace a representative caller-to-effect path and its tests.
4. Create the smallest managed set. Add one capability document per selected capability and one intent row per material known intent.
5. Create a flow only when two or more capability owners have a reusable ordering, handoff, transaction, retry, rollback, or side effect. Keep local sequences in the capability.
6. Simulate two or three handoffs. For a new request with no authoritative intent, verify that it routes to Deepen. For an existing intent, report whether the evidence supports design or implementation.
7. Render and validate the selected instruction bridge.

### Stop when

- selected capabilities describe evidence-backed current behavior;
- each selected material intent has one route;
- each durable fact has one prose owner; and
- the instruction bridge passes selection, marker, content, and budget checks.

## Deepen

### Goal

Make one requested task safe for its next stage without re-running Bootstrap.

### Steps

1. Start from the request or its intent row. A new or unrouted request is valid input.
2. Run the requested-stage sufficiency gate in the knowledge model. If the existing route is sufficient, return `checked-unchanged`.
3. Read only the sources and implementation paths needed for this task.
4. Trace the caller-to-effect path. When relevant, record symbols, ownership and lifecycle, state transitions, algorithm branches and termination, data or concurrency behavior, failures, invariants, tests, public-contract changes, and delivery obligations. Use scoped reproducible evidence; do not copy implementation bodies.
5. For a changing public or integration boundary, trace:
   `raw accepted value -> runtime-normalized value -> consumer-visible value`.
   Compare every applicable runtime, type or schema, documentation, example, and test mirror. Record disagreement as a gap.
6. When async operations can start and finish in different orders, distinguish invocation, settlement, and externally visible order. If order matters, use a controllable adversarial test seam.
7. Keep intent separate from current support. Report conflicts instead of resolving them by assumption.
8. Deepen an existing capability before creating another. For a broad capability, map internal areas first. Split only when evidence shows a separately owned outcome, contract, lifecycle, invariant, change axis, or repeatedly independent work.
9. Update the intent row, its optional task contract delta, and the smallest capability or flow set. Change the index or architecture map only when a canonical capability or flow is created or split.

### Stop when

The route `index -> intent row -> capability or flow` exposes the task's core acceptance, entry and effect, relevant mechanics, invariants, tests, delivery obligations, missing evidence, and human decisions. Include placement evidence when boundaries may change. Report only the readiness stage supported by this evidence.

## Refresh

### Goal

Bring managed knowledge back in sync after implementation stabilizes.

### Steps

1. Inspect the diff, affected tests, runtime paths, task evidence, and relevant snapshots.
2. Map each changed claim to its canonical owner.
3. Replace stale state. Promote stable behavior from the task contract delta to its capability or flow, then remove superseded task detail.
4. Update readiness only when planning safety changed. Keep local gaps with the capability and cross-capability risks in `risks.md`.
5. Return `checked-unchanged` when no durable knowledge changed.
6. Validate the instruction bridge before changing it.

Run Refresh before fresh completion verification. Refresh is not evidence that the implementation works.

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
  Readiness: ready-for-design | ready-for-implementation | needs-deepen | needs-decision | blocked | none (no selected material intent)
  Read first: <index, intent row, capability or flow>
  Unresolved decisions: <questions or none>
  Recommended next action: <one action>
  Task contract coverage:
    Status: assessed | none (not a material task Deepen)
    When assessed only; omit these four lines otherwise:
      Core acceptance: defined | missing
      Public contract delta: resolved | not-material | missing
      Risk hardening: classified | none-observed | missing
      Delivery obligations: linked | none-applicable | missing
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
| Treating a TODO or user report as implementation truth | Verify intent and support separately. |
| Copying status into several documents | Store it once and link to its owner. |
| Keeping before-and-after history in current-state docs | Replace stale content; use history artifacts. |
| Turning one missing local tool into project debt | Keep it in the run result unless it is reproducible project evidence. |
| Structurally rewriting an unmarked document | Make a surgical update or report the conflict. |
| Treating a capability as a code module | Map capability, flow, and implementation units separately. |
| Re-running Bootstrap for an unrouted task | Run task-scoped Deepen. |
| Splitting a capability because it is large | Require stable ownership or boundary evidence. |
| Listing paths without explaining the mechanism | Add task-relevant symbols, behavior, invariants, tests, and scoped evidence. |
| Reporting unqualified `ready` | Name the next safe stage. |
| Turning every edge case into a requirement | Separate core acceptance from risk hardening. |
