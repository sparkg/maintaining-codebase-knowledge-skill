<!-- codebase-knowledge:managed -->
# Flow: concise name

Last verified: revision and date when material

Evidence snapshot: clean revision plus participant symbols; for relevant dirty or non-VCS evidence, selected file content hashes.

Use this document only when two or more capability owners participate and the ordering, handoff, transaction, retry, rollback, or side effect merits independent maintenance.

Keep participant internals in capability documents. A new or changed flow describes orchestration and does not by itself imply a new implementation unit.

## Trigger and outcome

State what starts the cross-capability flow and its observable result or side effect.

## Participants and handoffs

| Step | Capability owner | Consumes | Produces | Handoff invariant | Failure propagation | Evidence |
|---|---|---|---|---|---|---|
| 1 | Canonical capability link | Input or precondition | Output, state change, or side effect | Condition the next owner relies on | Stop, retry, compensate, roll back, or continue | Path, test, or command |

## Cross-owner state transitions

Include this section only when the flow itself has durable cross-owner state. Do not copy a participant's local state machine.

| Flow state | Event or handoff | Guard | Next state or external effect | Failure, retry, or rollback | Evidence |
|---|---|---|---|---|---|
| Concrete cross-owner state | Participant output or external event | Handoff condition | Next participant-visible state or side effect | Propagation or recovery | Path plus symbol and test |

## Flow boundaries and recovery

Record verified cross-owner transaction, consistency, authorization, asynchronous, retry, timeout, rollback, compatibility, concurrency, performance, reliability, security, and external-side-effect boundaries.

## Flow invariants and tests

Record only end-to-end or handoff invariants and coverage owned by this flow. Do not repeat capability support matrices, local invariants, local gaps, or local test maps.
