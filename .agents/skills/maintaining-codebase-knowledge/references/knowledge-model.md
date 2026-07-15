# Project Knowledge Model

## Contents

- Evidence dimensions and claim authority
- Target repository root and output boundary
- Knowledge entrypoint and instruction bridge
- Layout and document ownership
- Architecture, capability, flow, and implementation-unit boundaries
- On-demand Deepen and task-knowledge sufficiency
- Capability knowledge coverage and decomposition
- Deep implementation evidence and scoped snapshots
- Architecture concern coverage
- Managed-document and project-instruction bridge lifecycle
- Routing and Refresh impact
- Writing rules

## Evidence dimensions

Never combine these dimensions into one status:

| Dimension | Values | Meaning |
|---|---|---|
| Evidence kind | `implementation`, `intent`, `operational`, `decision` | What type of claim the source can support |
| Confidence | `verified`, `hypothesis`, `unknown` | How strongly available evidence supports the claim |
| Support | `supported`, `partial`, `missing`, `unknown` | Current implementation state of a capability |
| Readiness | `ready`, `needs-deepen`, `needs-decision`, `blocked` | Whether a specific intent is safe to plan |
| Persistence | `durable`, `task-context`, `session`, `rejected` | Where external or conversation evidence may live |
| Documentation depth | `mapped`, `traced`, `unknown` | How deeply a capability's material internal area is documented; never a claim about implementation support |

Use qualified labels such as `Support: partial` and `Confidence: verified`; never use an unqualified `partial` or `unknown` where the dimension is unclear.

## Authority by claim type

There is no universal source-priority list. Use the source that owns the claim:

| Claim | Best evidence | Qualification |
|---|---|---|
| Repository structure or implementation | Code, schema, configuration, tests, reproducible behavior at a named revision | Dead or unreachable code and stale tests are not runtime proof |
| Intended behavior | Approved requirement, acceptance criteria, spec, or task at a named version | Intent does not prove support |
| Decision rationale | Accepted ADR, approval, or trustworthy history | Unknown rationale stays unknown |
| Operational state | CI run, deployment, incident, or runtime observation | Always bind commit, environment, and time |
| Workflow ownership or status | Canonical enterprise system | Treat as volatile metadata |

## Target repository root and output boundary

Resolve one target repository root during intake, canonicalize it to an absolute path, state the resolution basis, and reuse it for Bootstrap, Deepen, and Refresh. Apply this precedence:

1. Use an existing directory explicitly targeted by the user or task. An explicit monorepo subproject target wins over its enclosing VCS root.
2. Without an explicit target, use the containing VCS root reported by the applicable VCS-native root query.
3. When VCS discovery is unavailable, use an ancestor only when project instructions, manifests, or entry points support exactly one project-root candidate.
4. When no candidate or multiple candidates remain, report the resolution as unresolved and perform no managed writes.

The process working directory, Skill source or template path, and the presence of an agent-configuration directory are not project-root evidence by themselves. In particular, `.opencode`, `.agents`, `.claude`, and `.codex` must not become the target merely because the Skill is installed or invoked there. They may be selected only when the user or task explicitly targets that directory as the project.

Anchor every managed output to the resolved root: the selected project instruction bridge, `<target-repository-root>/docs/project-knowledge/**`, and any repository-approved task-context path. Before writing, verify that the normalized destination remains at or below the resolved root. Do not independently re-resolve the root for individual files or modes.

## Knowledge entrypoint and instruction bridge

`<target-repository-root>/docs/project-knowledge/index.md` is the canonical entrypoint for concrete project-knowledge routes. The Skill reads it directly during intake when it exists; this is not delegated to `AGENTS.md` or assumed from prior context.

The managed block in the selected root project instruction file is a stable downstream instruction bridge. It tells development workflows when to read the index, but it does not prove that a particular run read it. Keep the bridge small: never copy the index's routes, capability status, implementation paths, test lists, task-specific readiness values, or handoff state into the instruction file.

These responsibilities are complementary. Direct Skill intake makes Bootstrap, Deepen, and Refresh use the correct current entrypoint; the project instruction bridge makes later workflows discover it without repeated user prompt text. A platform that does not load the project's instruction file cannot be made to do so by duplicating knowledge into model memory or Skill metadata.

## Layout and ownership

All paths below are relative to the resolved target repository root. `index.md` is mandatory. Everything else is conditional; reuse adequate existing documentation by linking to it.

```text
docs/project-knowledge/
|- index.md
|- architecture.md
|- onboarding.md
|- intent-ledger.md
|- capabilities/<capability>.md
|- flows/<cross-capability-flow>.md
|- external-systems.md
|- risks.md
|- domain-glossary.md
`- adr/
```

| Document | Sole responsibility |
|---|---|
| Index | Small system summary and concrete navigation only |
| Architecture | Project profile, system-wide structure, dependency direction, capability-to-unit mapping, evidence-backed boundary basis, and default cross-cutting mechanisms |
| Onboarding | Stable prerequisites, commands, and reproducible project baselines |
| Capability | One domain or business ability: current support, local behavior, boundary contract, stable extension seams, local boundary pressure, invariants, evidence, and tests |
| Flow | Reusable orchestration and handoff contract crossing two or more capability owners |
| Intent ledger | Intent source, target capability, planning readiness, missing input, next action |
| External systems | Stable source routing, access, freshness, sensitivity, and write-back policy |
| Risks | Cross-capability risks only |
| ADR | Evidence-backed accepted or explicitly proposed decisions |

Capability-local gaps belong in the capability document. Task-specific missing decisions belong in the intent ledger or task context. Time-scoped local environment failures belong in the run result unless they establish a reproducible project constraint.

## Architecture, capability, flow, and implementation-unit boundaries

Assign each durable claim to exactly one of these owners:

| Claim scope | Canonical owner | Boundary rule |
|---|---|---|
| System-wide structure or default mechanism | Architecture | Own project type, technology stack, deployment units, dependency direction, shared infrastructure, and defaults such as error handling, observability, security, caching, configuration, and resilience. |
| One domain or business ability | Capability | Own its inputs, outputs, side effects, local state, support variants, local failures, invariants, implementation paths, tests, gaps, and any explicit exception to an architecture default. |
| Coordination across capability owners | Flow | Own ordering, handoff inputs and outputs, cross-boundary invariants, transaction or consistency boundaries, failure propagation, retry, rollback, and externally visible orchestration effects. |

A capability may describe its internal caller-to-effect sequence. Create a separate flow only when at least two capability owners participate and the ordering, handoff, transaction, retry, rollback, or side effect deserves independent maintenance. The capability links to the flow-owned handoff contract instead of restating it. Architecture defines the default mechanism; a capability records only a domain-specific exception; a flow records how failures and state propagate across owners.

Capability, flow, and implementation unit answer different questions and have many-to-many relationships:

| Concept | Question answered | Boundary rule |
|---|---|---|
| Capability | What domain or business outcome is owned? | May span units; one unit may support several capabilities. |
| Flow | How do capability owners coordinate? | Does not imply a new module, service, package, or deployment unit. |
| Implementation unit | Where is code, state, or runtime responsibility placed? | Architecture owns the current map and dependency direction, not the business support matrix. |

## Boundary and placement evidence

Persist placement claims according to their evidence and lifetime:

| Claim | Canonical persistence | Rule |
|---|---|---|
| Current capability-to-unit mapping and dependency direction | Architecture | Support with implementation evidence at a named revision. |
| Stable extension seam, observed change axis, or local boundary pressure | Capability; link the architecture map | Describe observable structure, coupling, and tests, not presumed intent. |
| Original design rationale | ADR or other decision evidence | If no trustworthy decision evidence exists, label the rationale `unknown`. |
| Task-specific placement assessment | Run result or task context | Report fit, seams, pressure, and open choices; do not promote a proposal to current state. |
| Accepted boundary decision | ADR or repository-owned decision record, then affected current-state documents | Refresh the map after implementation stabilizes. |

For task-specific planning, report evidence for these alternatives without choosing among them: extend an existing capability through an established seam; change only cross-capability orchestration; introduce a capability or implementation unit when state, lifecycle, policy, public interface, or change cadence is independently owned; or refactor when unrelated reasons to change, dependency cycles, invariant bypass, or coupled tests create verified boundary pressure. The owning design workflow makes the placement decision.

## On-demand Deepen and task-knowledge sufficiency

Bootstrap deliberately maps only one to five high-value capabilities. Missing coverage for an unselected or future request is normal. Never re-run Bootstrap merely because a new task is absent from the index or intent ledger; use task-scoped Deepen against the existing entrypoint.

Before a development workflow plans a request, check the smallest matching route for all task-material dimensions:

| Dimension | Sufficient evidence |
|---|---|
| Intent route | One current intent row identifies the request, authoritative source or conversation snapshot, target owner, readiness, and missing decisions |
| Capability or flow owner | A canonical owner exists, or Deepen has enough evidence to create one without equating it to a directory |
| Entry and effect | The request-relevant caller, public or internal entry, effect/output, and important side effects are traceable |
| Implementation mechanics | Material symbols, ownership/lifecycle, state or algorithm behavior, data/concurrency behavior, and failure paths are explained to the depth the task needs |
| Safety constraints | Invariants, compatibility surfaces, tests or benchmarks, and known missing evidence are explicit |
| Placement | When boundaries may change, current unit mapping, dependency direction, extension seams, and boundary pressure are available |
| Evidence snapshot | Claims bind to a reproducible scoped revision or selected-file snapshot |

If any material dimension is absent, stale, or only path-level, set or keep Readiness as `needs-deepen` and update the smallest canonical set. If the only gap is a human product or design decision, use `needs-decision`; Deepen must not invent it. When every dimension is already sufficient, report `checked-unchanged` and do not expand documents for completeness.

For a new request, create or update an intent row only when it is durable or needed as the canonical planning route; volatile discussion stays in task context. An `unrouted` row is a prompt to find the owner, not evidence that the repository needs another Bootstrap.

## Capability knowledge coverage and decomposition

For a broad capability, include a conditional internal-area map when readers otherwise cannot tell which areas are merely located and which have task-ready mechanics. Use Documentation depth only for this knowledge property:

| Documentation depth | Required evidence |
|---|---|
| `mapped` | Area responsibility, principal implementation units, and representative entry are identified |
| `traced` | Relevant caller-to-effect mechanics, local state or algorithm, invariants, failures, and tests are verified |
| `unknown` | Available evidence cannot yet support either level |

Large file count, line count, or conceptual complexity may trigger the internal-area map, but never proves a split. Keep an area in the existing capability when it contributes to the same owned outcome and shares lifecycle and invariants. Split only when implementation, decision, or repeated task evidence supports at least one stable independent boundary: a separately owned business result or external contract; independent state or lifecycle; distinct invariants or failure boundary; independent owner or change cadence; unrelated reasons to change; or repeatedly independent development work.

When a split is justified, move each durable claim to exactly one owner, update the architecture many-to-many map and index route, redirect affected intent rows and flows, and remove duplicated prose from the former owner. A split proposal without accepted evidence remains task context or `candidate-only` in the run result.

An explicit capability-discovery request is a scoped Deepen variant, not a new mode and not an exhaustive repository reread. Inspect material public interfaces, runtime entries, external contracts, stateful owners, and important implementation units not represented by current routes. Persist only evidence-backed business or domain outcomes; report weak candidates without creating empty documents.

## Deep implementation evidence and scoped snapshots

Deepen records only the mechanics needed to implement or repair the requested behavior. Prefer concise maps over copied source bodies:

| Mechanic | Canonical owner and minimum evidence |
|---|---|
| Key types and ownership | Capability: path plus class, type, trait, interface, function, registration point, or schema symbol; role and lifecycle when material |
| Local state machine | Capability: states, events, guards, transitions/effects, terminal or failure behavior, and symbol/test evidence |
| Local algorithm | Capability: entry, phases, material branches, termination/fallback, mutated state or output, invariants, and symbol/test evidence |
| Cross-owner state or sequence | Flow: participant-owned inputs/outputs, handoff guard, failure propagation, retry/rollback, and evidence; never repeat participant internals |
| Data or concurrency model | Capability or flow according to ownership: data shape, serialization, ownership, locks/queues, consistency, cancellation, or resource lifecycle |

Use symbols as the primary locator and line numbers only as optional aids because lines drift. Link or name tests that constrain the mechanic. Do not reproduce large implementation excerpts, generated code, logs, or complete call graphs.

Make the evidence set reproducible without hashing the repository. Persist complete digests; never abbreviate a hash that is meant to identify evidence:

- clean VCS worktree: record the revision and task-critical paths/symbols;
- dirty VCS worktree: record the base revision plus content hashes for task-critical dirty or untracked files used as evidence;
- no VCS metadata: record content hashes for the selected critical source, manifest, schema, or configuration files;
- external or operational evidence: retain its source identifier, version/environment, and retrieval time when material.

The canonical document keeps the concise snapshot that qualifies its current claims; the run result records commands and session environment. Refresh replaces obsolete hashes or mechanics after stable implementation instead of appending historical snapshots.

## Architecture concern coverage

During Bootstrap, evaluate every category below against repository evidence and project type:

- project profile, users, project type, technology stack, runtime, build, deployment, and extension model;
- modules, services, packages, deployment units, public or shared modules, and dependency direction;
- data stores, queues, object or file storage, cache, and external integrations;
- dependency and version management, configuration, secrets, and feature flags;
- error handling; logging, metrics, tracing, and audit; authentication, authorization, trust boundaries, input validation, and sensitive data;
- cache ownership, invalidation, and consistency; rate limiting and backpressure; retry, timeout, circuit breaker, and fallback;
- concurrency, background jobs, schedulers, transactions, CI/CD, health, recovery, migrations, and rollback.

In the architecture concern matrix, include verified mechanisms, material unknowns, and explicit `not-applicable` conclusions. Omit categories that are immaterial to the project type; do not manufacture mechanisms or empty prose. Keep system defaults here, capability-specific behavior in capability documents, and cross-boundary propagation in flows.

## Managed-document lifecycle

Templates begin with the canonical marker `<!-- codebase-knowledge:managed -->`. Treat a document as managed when its first line is an HTML comment containing both `codebase-knowledge:` and `managed`; normalize that line to the canonical marker on the next managed write. A document without a managed marker is user-authored unless the user explicitly says otherwise.

- Rewrite managed sections deterministically from current evidence.
- Remove superseded generated claims after replacement evidence exists.
- Do not retain an inline historical baseline in a current-state document.
- Preserve accepted rationale in ADRs and intended execution in specs or plans; rely on Git for ordinary history.
- Never structurally rewrite an unmarked document. Link to it, update only the necessary statement surgically, or report ambiguity.
- Keep generated output idempotent: unchanged evidence produces no content diff.

## Project instruction bridge lifecycle

After resolving the target repository root, select exactly one instruction bridge in this order:

1. Select an existing `<target-repository-root>/AGENTS.md`.
2. Otherwise select an existing `<target-repository-root>/CLAUDE.md`.
3. Otherwise select and create `<target-repository-root>/AGENTS.md`.

When both files exist, select `AGENTS.md` and leave `CLAUDE.md` unchanged. Treat `assets/templates/agents-section.md` as the shared managed-block base for either selected file, not as whole-file replacement content. Every successful mode leaves exactly one selected managed bridge pointing to the current index.

Never modify the non-selected file. If the non-selected file contains a managed marker, perform no instruction-file write and report an unresolved dual-bridge conflict. If the selected file is unreadable, unwritable, resolves outside the target root, or has invalid markers, report unresolved and do not fall back to the other file.

Never select or modify `AGENTS.override.md`; read it only when it is an applicable project instruction. Report discovery as `shadowed` when it can take precedence over the selected bridge. Report an unshadowed `AGENTS.md` selection as `native` and a `CLAUDE.md` selection as `platform-dependent`; the latter does not claim cross-tool automatic discovery.

The final rendered managed block, including both markers, must contain no more than 200 physical lines. Remove trailing empty lines before counting. Measure after omitting managed bullets that duplicate or conflict with external instructions. Content outside the markers is user-authored, excluded from the budget, and must not be shortened to make the managed block fit. If the final block exceeds 200 lines, do not truncate or write it; report the project instruction bridge as unresolved.

Keep the managed block limited to stable critical working agreements, one project-knowledge index entrypoint, and observable workflow conditions. Use strong directive words only for real invariants. Project descriptions, exact commands, architecture details, code-style examples, file organization, capability status, task-specific readiness values, implementation paths, test lists, history, and one-off incidents remain with their canonical project-knowledge owners and are reached through the index.

- If exactly one complete ordered marker pair exists, replace that inclusive block.
- If neither marker exists, append the rendered block without changing existing content.
- If markers are incomplete, reversed, or duplicated, do not edit; report unresolved.
- Compare working agreements only with guidance outside the old managed block. Omit a managed bullet when equivalent external guidance exists; preserve external guidance and omit the managed bullet when they conflict.
- Render deterministically in template order so unchanged external guidance produces the same block.

## Routing rules

The index links to canonical owners; it does not copy their status details. Include only concrete routes supported by repository evidence. Do not generate generic task-type tables, command output, duplicated unknown lists, or persistent workflow-specific handoff prose.

A requested task missing from the index or ledger triggers task-scoped Deepen. Deepen first attempts to match and expand an existing owner; it creates a capability or flow route only when evidence supports that owner. Existing sufficient routes remain unchanged.

An intent-ledger row links to a capability or flow. It must not repeat implementation paths, invariants, test lists, or gap analysis owned by that document.

A flow must not contain a capability support matrix. Its participant rows link to canonical capability documents. A capability's participating-flow row links to the flow-owned handoff contract rather than copying it.

Architecture owns the capability-to-unit map and module boundary basis. Capability documents link that map and own only stable local placement evidence. A flow records orchestration; creating or changing one does not by itself justify a new implementation unit.

## Refresh impact matrix

| Change | Canonical owner |
|---|---|
| Setup, build, test, run, or stable project baseline | Onboarding |
| Project profile, system boundary, module direction, capability-to-unit mapping, module boundary basis, shared infrastructure, or default cross-cutting mechanism | Architecture |
| Capability boundary, behavior, support, documentation depth, internal-area map, implementation mechanics, local failure, exception, invariant, implementation path, stable extension seam, local boundary pressure, or tests | Capability |
| Cross-capability ordering, handoff, failure propagation, transaction, retry, rollback, or side effect | Flow |
| Requirement, TODO, issue, missing decision, or planning readiness | Intent ledger |
| Cross-capability reliability, security, compatibility, or operational risk | Risks |
| Enterprise source routing or governance | External systems |
| Accepted trade-off and rationale | ADR |
| Task-specific placement assessment or unaccepted proposal | Run result or task context |

## Writing rules

- Link to evidence rather than copying code, logs, tickets, or requirements.
- Prefer path-plus-symbol evidence for implementation mechanics; use line numbers only as secondary navigation.
- Record revision, source version, environment, and time only when they materially qualify a claim.
- Keep tables narrow. Add a column only when it carries evidence for that document's sole responsibility.
- Do not create empty sections, speculative diagrams, or retrospective rationale.
- Never infer original design intent from code layout. Label observations, hypotheses, accepted decisions, and unknown rationale distinctly.
- Write valid UTF-8 and repair unreadable output before reporting completion.
