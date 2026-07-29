<!-- codebase-knowledge:managed -->
# Flow: concise name

Last verified: code revision and date, when relevant

Evidence snapshot: resolvable code revision and the key symbols for each participant. If the evidence includes uncommitted files or comes from a repository without version control, record the full hashes of the selected files. A documentation-only commit does not identify the code that was inspected.

Create a flow document only when two or more capabilities take part and one document needs to own their ordering, handoff, transaction, retry, rollback, or side effects. Keep each participant's internal behavior in its capability document. A documented flow does not imply that the code needs a new module.

## At a glance

| Question | Short answer |
|---|---|
| What starts this flow? | External event or output from a participating capability |
| What can a caller or user observe at the end? | Result, state change, or side effect |
| Why does this need a separate flow document? | An ordering or handoff rule spans several capabilities and needs one place to maintain it |
| How is the complete flow verified? | Link to end-to-end or handoff tests |

## Participants and handoffs

| Step | Responsible capability | Receives | Produces | Condition required by the next step | What happens on failure | Evidence |
|---|---|---|---|---|---|---|
| 1 | Link to the capability document | Input or precondition | Output, state change, or side effect | Condition the next participant relies on | Stop, retry, compensate, roll back, or continue | Path, test, or command |

## State shared across capabilities

Include this section only when the flow has durable state of its own. Do not copy a participant's local state machine.

| Shared state | Event or handoff | Condition to continue | Next visible state or effect | Failure, retry, or rollback | Evidence |
|---|---|---|---|---|---|
| Concrete state owned by the flow | Participant output or external event | Handoff condition | State or side effect visible to the next participant | Propagation or recovery | Path, symbol, and test |

## Boundaries and recovery

Record only verified rules that apply across participants: transactions, consistency, authorization, asynchronous work, retries, timeouts, rollback, compatibility, concurrency, performance, reliability, security, and external side effects.

## Invariants and verification

Record end-to-end or handoff invariants and the tests that protect them. Do not repeat a capability's support matrix, local invariants, local gaps, or local test list.
