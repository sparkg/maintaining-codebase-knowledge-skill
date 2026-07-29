# Project Knowledge Model

## Contents

- Evidence dimensions and claim authority
- Target repository root and output boundary
- Knowledge entrypoint and instruction bridge
- Layout and document ownership
- Architecture, capability, flow, and implementation-unit boundaries
- On-demand Deepen and task-knowledge sufficiency
- Task contract delta and delivery obligations
- Capability knowledge coverage and decomposition
- Deep implementation evidence and scoped snapshots
- Architecture concern coverage
- Managed-document and project-instruction bridge lifecycle
- Routing and Refresh impact
- Final consistency validation
- Writing rules

Use only the sections needed for the current mode:

- resolving the repository or instruction bridge: read the root, entrypoint, and bridge sections;
- writing or routing knowledge: read layout, ownership, boundaries, and routing;
- preparing a task: read sufficiency, task contract delta, and implementation evidence;
- finishing a write: read lifecycle, final consistency, and writing rules.

## Evidence dimensions

Never combine these dimensions into one status:

| Dimension | Values | Meaning |
|---|---|---|
| Evidence kind | `implementation`, `intent`, `operational`, `decision` | What type of claim the source can support |
| Confidence | `verified`, `hypothesis`, `unknown` | How strongly available evidence supports the claim |
| Support | `supported`, `partial`, `missing`, `unknown` | Current implementation state of a capability |
| Readiness | `ready-for-design`, `ready-for-implementation`, `needs-deepen`, `needs-decision`, `blocked` | Which next development stage a specific intent can safely enter |
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

Resolve one target root during intake. Convert it to an absolute path, report why it was selected, and use it for the whole run. Choose it in this order:

1. Use the existing directory explicitly named by the user or task. An explicit monorepo subproject wins over its enclosing VCS root.
2. Otherwise use the containing root reported by the VCS-native root query.
3. Without VCS discovery, use an ancestor only when instructions, manifests, or entry points identify exactly one project root.
4. If no single candidate remains, report `unresolved` and make no managed writes.

The working directory, Skill path, template path, or an agent-configuration directory does not prove the project root. Directories such as `.opencode`, `.agents`, `.claude`, and `.codex` are targets only when the user or task explicitly names them as the project.

Anchor every managed output to the resolved root. Before writing, confirm the normalized destination stays within that root. Do not resolve a different root for another file or mode.

## Knowledge entrypoint and instruction bridge

`<target-repository-root>/docs/project-knowledge/index.md` is the canonical route entrypoint. Read it directly during intake when it exists. Do not rely on `AGENTS.md`, prior context, or memory as proof that it was read.

The managed block in the selected root instruction file is a discovery bridge for later workflows. Keep it small. Do not copy routes, capability status, implementation paths, test lists, task readiness, or handoff state into it.

Direct intake ensures this Skill uses the current index. The instruction bridge helps later workflows discover it. If a platform does not load that instruction file, duplicating knowledge in memory or Skill metadata does not fix discovery.

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
| Onboarding | Stable prerequisites, commands, reproducible project baselines, and evidence-backed delivery obligations by change type |
| Capability | One domain or business ability: current support, local behavior, boundary contract, stable extension seams, local boundary pressure, invariants, evidence, and tests |
| Flow | Reusable orchestration and handoff contract crossing two or more capability owners |
| Intent ledger | Intent source, target capability, stage readiness, missing input, next action, and an optional task contract delta |
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

For task-specific planning, report evidence for the available choices without selecting one:

- extend an existing capability through a verified seam;
- change only cross-capability orchestration;
- introduce a capability or implementation unit when ownership is genuinely independent; or
- refactor first when cycles, invariant bypass, coupled tests, or unrelated change reasons create verified pressure.

The design workflow makes the placement decision.

## On-demand Deepen and task-knowledge sufficiency

Bootstrap maps only one to five high-value capabilities. Missing coverage for a future request is normal. If a new task is absent from the index or ledger, use task-scoped Deepen; do not re-run Bootstrap.

Before a development workflow plans a request, check the smallest matching route for all task-material dimensions:

| Dimension | Sufficient evidence |
|---|---|
| Intent route | One current intent row identifies the request, authoritative source or conversation snapshot, target owner, stage readiness, and missing decisions |
| Capability or flow owner | A canonical owner exists, or Deepen has enough evidence to create one without equating it to a directory |
| Core acceptance | The minimum authoritative requested behavior and observable pass conditions are explicit; evidence-backed hardening is not silently promoted to product scope |
| Entry and effect | The request-relevant caller, public or internal entry, effect/output, and important side effects are traceable |
| Implementation mechanics | Material symbols, ownership/lifecycle, state or algorithm behavior, data/concurrency behavior, and failure paths are explained to the depth the task needs |
| Safety constraints | Invariants, compatibility surfaces, tests or benchmarks, and known missing evidence are explicit |
| Public contract delta | When a public or integration boundary may change, material inputs/protocols and precedence, raw-to-normalized-to-consumer-visible value semantics, outputs/errors and failure timing, applicable type/schema/docs/test mirrors, and meaningful negative cases have evidence, an accepted decision, or an explicit `not-applicable` conclusion; semantic disagreement between mirrors remains a gap |
| Delivery obligations | Applicable evidence-backed change-type gates or artifacts are linked from onboarding; absent categories are not invented |
| Placement | When boundaries may change, current unit mapping, dependency direction, extension seams, and boundary pressure are available |
| Evidence snapshot | Claims bind to a reproducible scoped revision or selected-file snapshot |

Apply readiness to the requested next stage:

| Readiness | Meaning and allowed next action |
|---|---|
| `ready-for-design` | Authoritative intent, current owner route, constraints, and evidence are sufficient for an authorized design workflow to resolve local choices; implementation is not yet authorized by this status. |
| `ready-for-implementation` | Core acceptance, task-relevant mechanics and safety constraints, public contract delta when material, applicable delivery obligations, and scoped evidence are sufficient; no unresolved human product or authority decision remains. This status subsumes design readiness. |
| `needs-deepen` | Repository or authorized evidence is missing, stale, or only path-level for the requested stage. |
| `needs-decision` | The remaining gap requires a human or external product, policy, or authority decision; Deepen must not invent it. |
| `blocked` | Access, environment, unreadable state, or another concrete blocker prevents the required knowledge work. |

After `ready-for-design`, an authorized design workflow may resolve delegated local choices. Record the accepted result in task context or Deepen before claiming `ready-for-implementation`. Return `checked-unchanged` when the requested stage is already supported.

Create an intent row only for durable requests or when the row is the canonical task route. Volatile discussion stays in task context. `unrouted` means “find the owner,” not “run Bootstrap again.”

## Task contract delta and delivery obligations

For a material task, Deepen may add one `Task contract delta` below its intent route. This is an optional section, not a new document type. It records what the task changes and what the next stage must know.

| Delta part | Include only when material | Boundary |
|---|---|---|
| Core acceptance | Minimum authoritative requested behavior and constraints, observable pass conditions, and supporting direct reproducible failures when relevant | A failure or implementation mechanism may support the contract but cannot define product intent; do not infer undocumented options from code or an author patch. |
| Public contract delta | Input or protocol variants and precedence; raw accepted, runtime-normalized, and consumer-visible value shapes; output/error and failure timing; material nullable, promised, sentinel, schema, compatibility, runtime/type/docs/test mirrors, and negative cases | Give normalization or wrapper removal its own material row. Compare every applicable mirror, including whether callback or consumer generic/schema types describe the post-normalization value, and record disagreements plus the accepted delta, a named decision, or `not-applicable`. |
| Risk hardening | Evidence-backed interaction seams in boundaries, branches, state, failure, concurrency, compatibility, performance, or security | Guides verification but does not add product behavior unless authoritative intent promotes it to core acceptance. For material asynchronous ordering, distinguish invocation, settlement, and visible order; use a controllable adversarial seam when an ordinary async producer cannot separate them. |
| Applicable delivery obligations | Links to triggered change-type rows in onboarding | Do not copy the rule or invent a universal gate. |

When a task replaces a fixed-width, delimiter, sentinel, or terminator boundary, characterize the adjacent accepted-input partitions before labeling one invalid. Check the empty suffix or payload, missing delimiter with an otherwise valid token, repeated delimiters, and delimiter-like content inside the payload when applicable. Preserve verified current behavior or record an authoritative change; a newly convenient parser error is not compatibility evidence.

Omit any delta subsection that does not apply. Current paths, mechanics, invariants, test inventories, support, and local gaps stay with the capability or flow. Task-specific verification and new probes stay in the delta until Refresh. After implementation stabilizes, Refresh moves durable behavior and coverage to their canonical owners and removes superseded task prose.

During Bootstrap, look for contribution, CI, documentation, migration, security, benchmark, and release rules referenced by root instructions, manifests, or selected scope. Read only sources relevant to the project and selected capabilities. Do not audit every document or query unrelated enterprise systems.

When a rule exists, onboarding records its trigger, required check or artifact, completion signal, and canonical source. A task cannot be `ready-for-implementation` while a known applicable obligation is missing from its delta. Never invent changelog, benchmark, migration, or review requirements.

Do not equate a release checklist with “release-time only” without checking the repository's current working convention. When a changelog or release-notes file exists in the selected scope, inspect its current unreleased section and a small sample of neighboring entries. Use that structure together with contribution or release guidance to decide whether maintainers accumulate notable user-visible changes during ordinary feature work. Record the narrower evidence-backed trigger; keep version bumps, tags, publishing, and other release ceremony separate.

A new or materially changed public example that promises executable behavior must be tested, run as documentation, checked by a documented command, or labeled illustrative and unverified. Do not turn this into a repository-wide example audit.

## Capability knowledge coverage and decomposition

For a broad capability, include a conditional internal-area map when readers otherwise cannot tell which areas are merely located and which have task-ready mechanics. Use Documentation depth only for this knowledge property:

| Documentation depth | Required evidence |
|---|---|
| `mapped` | Area responsibility, principal implementation units, and representative entry are identified |
| `traced` | Relevant caller-to-effect mechanics, local state or algorithm, invariants, failures, and tests are verified |
| `unknown` | Available evidence cannot yet support either level |

Size or complexity may justify an internal-area map, but never proves a split. Keep an area together when it serves the same outcome and shares lifecycle and invariants. Split only when evidence shows a stable independent boundary, such as its own result, contract, state, lifecycle, invariant, failure boundary, owner, change cadence, or repeatedly independent work.

After an accepted split, move each durable claim to one owner. Update the architecture map and index, redirect affected intents and flows, and remove duplicated prose. An unaccepted split remains task context or `candidate-only` in the result.

An explicit capability-discovery request is scoped Deepen, not a new mode or a full repository reread. Inspect important interfaces, runtime entries, external contracts, state owners, and unmapped implementation units. Persist only evidence-backed business or domain outcomes. Report weak candidates without creating empty documents.

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

- clean VCS worktree: record a revision resolvable in the target worktree and task-critical paths/symbols;
- dirty VCS worktree: record the base revision plus content hashes for task-critical dirty or untracked files used as evidence;
- no VCS metadata: record content hashes for the selected critical source, manifest, schema, or configuration files;
- external or operational evidence: retain its source identifier, version/environment, and retrieval time when material.

The canonical document keeps the concise snapshot that qualifies its current claims; the run result records commands and session environment. Refresh replaces obsolete hashes or mechanics after stable implementation instead of appending historical snapshots.

A documentation-only commit is not evidence for the code state it describes. Bind code claims to the resolvable code revision they were inspected at; for dirty, untracked, or non-VCS evidence, use selected full content hashes as specified above.

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

The final managed block, including both markers, must be at most 800 decoded characters. Normalize line endings to LF and remove trailing LF before counting. First omit managed bullets that duplicate or conflict with external instructions. User text outside the markers is not part of the budget and must not be shortened. If the block is still too large, do not truncate or write it; report `unresolved`.

The managed block contains only stable working agreements, one index entrypoint, and observable workflow conditions. Strong directives are reserved for real invariants. Project descriptions, commands, architecture, style examples, file layout, capability status, task readiness, paths, tests, history, and incidents stay with their canonical owners.

- If exactly one complete ordered marker pair exists, replace that inclusive block.
- If neither marker exists, append the rendered block without changing existing content.
- If markers are incomplete, reversed, or duplicated, do not edit; report unresolved.
- Compare working agreements only with guidance outside the old managed block. Omit a managed bullet when equivalent external guidance exists; preserve external guidance and omit the managed bullet when they conflict.
- Render deterministically in template order so unchanged external guidance produces the same block.

## Routing rules

The index links to canonical owners; it does not copy their status details. Include only concrete routes supported by repository evidence. Do not generate generic task-type tables, command output, duplicated unknown lists, or persistent workflow-specific handoff prose.

A requested task missing from the index or ledger triggers task-scoped Deepen. Read a matching intent only when the ledger and row exist; do not create an empty ledger merely to satisfy an instruction. Deepen first attempts to match and expand an existing owner; it creates a capability or flow route only when evidence supports that owner. Existing sufficient routes remain unchanged.

An intent-ledger row links to a capability or flow. Its optional task contract delta must not repeat implementation paths, invariants, existing test inventories, current support, or gap analysis owned by that document. Task-specific required verification or a new observable probe remains in the delta until Refresh.

A flow must not contain a capability support matrix. Its participant rows link to canonical capability documents. A capability's participating-flow row links to the flow-owned handoff contract rather than copying it.

Architecture owns the capability-to-unit map and module boundary basis. Capability documents link that map and own only stable local placement evidence. A flow records orchestration; creating or changing one does not by itself justify a new implementation unit.

## Final consistency validation

Before reporting a managed write as complete, validate the affected canonical set:

- every instruction and index route resolves; a matching intent is conditional on the ledger and row existing;
- every material support label carries its verified input, mode, or variant boundary in the same capability row;
- every changed public or integration boundary traces raw, normalized, and consumer-visible values and agrees semantically across applicable runtime, type/schema, documentation/example, and test mirrors, or the disagreement is an explicit gap that limits readiness;
- every durable claim has one prose owner: cross-owner sequencing lives in the flow, while capabilities keep local invariants and links;
- every public executable example introduced or materially changed by the task is executed or explicitly labeled illustrative and unverified;
- every evidence revision resolves in the target worktree, or the scoped snapshot uses selected full content hashes;
- every task delta separates authoritative core acceptance from evidence-backed risk hardening and links applicable onboarding obligations;
- readiness names the next safe stage and is no stronger than the available evidence.

## Refresh impact matrix

| Change | Canonical owner |
|---|---|
| Setup, build, test, run, stable project baseline, or evidence-backed change-type delivery obligation | Onboarding |
| Project profile, system boundary, module direction, capability-to-unit mapping, module boundary basis, shared infrastructure, or default cross-cutting mechanism | Architecture |
| Capability boundary, behavior, support, documentation depth, internal-area map, implementation mechanics, local failure, exception, invariant, implementation path, stable extension seam, local boundary pressure, or tests | Capability |
| Cross-capability ordering, handoff, failure propagation, transaction, retry, rollback, or side effect | Flow |
| Requirement, TODO, issue, task contract delta, missing decision, or stage readiness | Intent ledger |
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
