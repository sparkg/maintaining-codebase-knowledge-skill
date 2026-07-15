<!-- codebase-knowledge:managed -->
# Capability: concise name

Last verified: revision and date when material

Evidence snapshot: clean revision plus task-critical symbols; for relevant dirty or non-VCS evidence, selected file full content hashes. Never abbreviate evidence hashes or hash the repository.

## Purpose

State the domain or business outcome and its callers or consumers.

## Boundary contract

| Contract item | Evidence-backed statement | Evidence |
|---|---|---|
| Owns | Behavior and state owned by this capability | Paths, tests, or source snapshot |
| Does not own | Adjacent responsibilities owned elsewhere | Canonical owner link |
| Inputs | Semantic inputs and preconditions | Interface, schema, or caller |
| Outputs | Observable results and guarantees | Interface, schema, or test |
| Side effects | Local state, data, or external effects | Implementation or test |
| Failure surface | Local errors and failure conditions | Implementation or test |

## Current behavior

Keep only dimensions that carry evidence.

| Behavior or variant | Support | Confidence | Evidence | Tests or benchmarks |
|---|---|---|---|---|
| Concrete current behavior | supported, partial, missing, or unknown | verified, hypothesis, or unknown | Paths, commands, or source snapshot | Existing coverage or gap |

## Runtime and data path

Describe only the internal entry/caller, owner, state or data changes, and local sequence needed for implementation work. Link cross-capability orchestration instead of repeating it.

## Internal areas and knowledge coverage

Include this section only when the capability is broad enough that readers need to distinguish located areas from task-ready mechanics. File or line count alone does not justify splitting.

| Internal area | Responsibility | Documentation depth | Boundary pressure | Evidence |
|---|---|---|---|---|
| Material area | Owned contribution to the capability outcome | mapped, traced, or unknown | Stable independent boundary evidence or none observed | Path plus representative symbol, test, or scoped snapshot |

## Implementation mechanics

Include only task-relevant structures below, and omit unused headings. Use path-plus-symbol evidence, not copied implementation bodies.

### Key types and ownership

| Symbol | Role | Ownership or lifecycle | Evidence |
|---|---|---|---|
| Class, type, trait, interface, function, registration point, or schema | Task-relevant responsibility | Owner, lifetime, state, or resource responsibility | Path, symbol, test, and scoped snapshot |

### Local state machine

| State | Event or input | Guard | Transition or effect | Failure or terminal behavior | Evidence |
|---|---|---|---|---|---|
| Concrete state | Trigger | Condition | Next state, mutation, output, or side effect | Error, cancellation, fallback, or completion | Symbol and test |

### Algorithm map

| Phase or branch | Input and decision | State change or output | Termination or fallback | Invariant | Evidence |
|---|---|---|---|---|---|
| Concrete step | Material condition or choice | Effect | Stop, retry, continue, or fallback | Constraint preserved | Symbol and test |

### Data and concurrency model

Describe only task-material data shape, serialization, ownership, locks/queues, cancellation, consistency, or resource lifecycle that is local to this capability. Cross-owner coordination belongs in a flow.

## Change placement evidence

Record stable repository evidence only; task-specific placement choices stay in the run result or design workflow.

| Signal | Evidence-backed statement | Evidence |
|---|---|---|
| Observed change axis | Behavior, state, policy, or dependency that changes together | Paths, tests, or history when trustworthy |
| Existing extension seams | Interface, registration point, adapter, plugin, event, or none observed | Path or test |
| Architecture mapping | Primary and supporting implementation units | Link to the canonical architecture row |
| Boundary pressure or split signal | Verified independent outcome/contract, state/lifecycle, invariant/failure boundary, owner/change cadence, unrelated reason to change, repeatedly independent work, or none observed; size alone is not evidence | Path, test, task history, or decision evidence |

## Participating flows

Include only actual cross-capability flows. The handoff contract cell links to its canonical row in the flow document; it does not restate the contract.

| Flow | Role | Handoff contract |
|---|---|---|
| Canonical flow link | Producer, consumer, coordinator, or participant | Link to flow-owned handoff row |

## Invariants

List only verified semantic, compatibility, performance, memory, reliability, or security constraints.

## Capability-local gaps

Record concrete unsupported behavior or missing tests. Keep task-specific decisions in `intent-ledger.md` and cross-capability risks in `risks.md`.
