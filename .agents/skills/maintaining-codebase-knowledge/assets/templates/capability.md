<!-- codebase-knowledge:managed -->
# Capability: concise name

Last verified: code revision and date, when relevant

Evidence snapshot: resolvable code revision and the symbols that matter to the task. If the evidence includes uncommitted files or comes from a repository without version control, record the full hashes of the selected files. A documentation-only commit does not identify the code that was inspected. Do not abbreviate evidence hashes or hash the whole repository.

## At a glance

Give a developer enough context to decide where to read next. Link to the detailed sections rather than repeating them here.

| Question | Short answer |
|---|---|
| What result is this capability responsible for? | Domain or business result |
| Who or what uses it? | Main callers or consumers |
| Which implementation units contain it? | Link to its row in the architecture capability-to-implementation map |
| Where can an existing extension point accept a change? | Link to a verified extension point, or state that none was found |
| What must continue to work? | Link to the main invariants and tests |

## Responsibility and boundary

| Part of the contract | Current behavior | Evidence |
|---|---|---|
| Responsibilities | Behavior and state this capability owns | Paths, tests, or source snapshot |
| Outside this capability | Adjacent responsibility and the document that owns it | Link to the responsible document |
| Inputs | Meaning of inputs and required preconditions | Interface, schema, or caller |
| Outputs | Observable results and guarantees | Interface, schema, or test |
| Side effects | Local state, data, or external effects | Implementation or test |
| Failures | Local errors and the conditions that cause them | Implementation or test |

## Current behavior

Include only behavior supported by evidence. Put the verified input, mode, or variant limit in the same row as its support status.

| Behavior and verified limit | Support status | Confidence | Evidence | Tests or benchmarks |
|---|---|---|---|---|
| Concrete behavior and its actual limit | `supported`, `partial`, `missing`, or `unknown` | `verified`, `hypothesis`, or `unknown` | Paths, commands, or snapshot | Existing coverage or known gap |

## How it works

Describe the callers, entry points, responsible code, state or data changes, and local sequence needed to work on this capability. Link to a flow document for orchestration that crosses capabilities instead of copying it here.

## Internal areas and how well they are understood

Use this section only when a reader needs to tell the difference between code that has merely been located and mechanics that have been traced deeply enough for a task. Size alone is not a reason to split a capability.

| Internal area | What it contributes | How well it is documented | Evidence of a separate boundary | Evidence |
|---|---|---|---|---|
| Material area | Contribution to the capability's result | `mapped`, `traced`, or `unknown` | Stable evidence for an independent boundary, or `none observed` | Path plus representative symbol, test, or snapshot |

## Implementation details

Include only the structures needed for the task. Omit unused subsections. Refer to paths and symbols instead of copying implementation bodies.

### Key code elements and ownership

| Code element | Role | Ownership or lifecycle | Evidence |
|---|---|---|---|
| Class, type, trait, interface, function, registration point, or schema | Responsibility relevant to the task | Responsible component, lifetime, state, or resource responsibility | Path, symbol, test, and snapshot |

### Local state machine

| State | Event or input | Condition | Transition or effect | Failure or terminal behavior | Evidence |
|---|---|---|---|---|---|
| Concrete state | Trigger | Condition that permits the transition | Next state, mutation, output, or side effect | Error, cancellation, fallback, or completion | Symbol and test |

### Algorithm map

| Phase or branch | Input and decision | State change or output | Termination or fallback | Invariant | Evidence |
|---|---|---|---|---|---|
| Concrete step | Material condition or choice | Effect | Stop, retry, continue, or fall back | Constraint that remains true | Symbol and test |

### Data and concurrency model

Describe the task-relevant data shape, serialization, ownership, locks or queues, cancellation, consistency, and resource lifecycle inside this capability. Put coordination between separately responsible capabilities in a flow document.

## Where a change belongs

Record only stable evidence from the repository. Keep task-specific placement decisions in the task context or run result.

| Signal | What the repository currently shows | Evidence source |
|---|---|---|
| What changes together | Behavior, state, policy, or dependency that usually changes for the same reason | Paths, tests, or trustworthy history |
| Existing extension points | Interface, registration point, adapter, plugin, event, or `none observed` | Path or test |
| Architecture mapping | Primary and supporting code units | Link to the corresponding architecture row |
| Evidence that a separate boundary may be needed | Independent result, contract, lifecycle, invariant, responsible component, change cadence, unrelated reason to change, repeatedly independent work, or `none observed`; size alone is not evidence | Path, test, task history, or decision record |

## Flows that include this capability

List only flows that actually cross capability boundaries. Link to the handoff in the flow document instead of copying it.

| Flow document | This capability's role | Handoff details |
|---|---|---|
| Link to the flow document | Producer, consumer, coordinator, or participant | Link to the relevant handoff row in the flow document |

## Invariants and verification

List only verified semantic, compatibility, performance, memory, reliability, or security constraints. Name the tests or benchmarks that protect them.

## Known local gaps

Record concrete unsupported behavior or missing tests. Keep task decisions in `intent-ledger.md` and risks that affect several capabilities in `risks.md`.
