<!-- codebase-knowledge:managed -->
# Development intent ledger

Last verified: code revision and date, when relevant

Use this file to answer three questions: What work has been requested? Which capability or flow documents explain the current implementation? What needs to happen next? Keep implementation details in the capability, flow, or onboarding document that owns them.

| Requested work | Source record | Relevant capability or flow | Readiness | Missing information or evidence | Next action |
|---|---|---|---|---|---|
| Feature, bug, optimization, migration, refactor, test gap, or infrastructure work | Path, stable ID or link, version, and retrieval time when relevant | Link to the responsible document, or `unrouted` | `ready-for-design`, `ready-for-implementation`, `needs-deepen`, `needs-decision`, or `blocked` | Missing requirement, decision, evidence, permission, tool, or environment | Design, implement, run Deepen, obtain a decision, gather evidence, or remove the blocker |

Deepen may add a row for a new request that needs a durable route. Use `unrouted` only while the evidence does not yet show which document should own the work. Keep short-lived discussion in the current task context.

## Task contract delta: work identifier

Add this section only when the summary row is not enough to hand the task to development. Describe what this task must change. Do not copy current mechanics, invariants, test lists, known gaps, or delivery rules from the documents that already own them.

Keep tests and observable probes created specifically for this task here until Refresh. Existing test inventories remain in the relevant capability or flow document.

### Required change

| Required behavior or constraint | Authoritative source and supporting evidence | Observable pass condition | Status or decision still needed |
|---|---|---|---|
| Minimum behavior required by the authoritative request | Requirement, issue, conversation snapshot, accepted decision, and a supporting probe when relevant | Result that a user, caller, test, or operator can observe | Accepted, missing evidence, or named decision |

A reproducible failure can support the task description, but it cannot define product intent on its own.

### Changes to public or integration contracts

Include this section only when the task may change a public interface or another integration boundary.

| Part of the contract | Current behavior and evidence | Requested or accepted change | Other surfaces and negative cases to check |
|---|---|---|---|
| Input or protocol variants and precedence; raw accepted value, normalized runtime value, and value visible to consumers; output, error, or failure timing; important nullable, promised, sentinel, schema, or compatibility cases | Link to the capability contract; give normalization its own row; name any disagreement between surfaces | Authoritative request, accepted design, `not-applicable`, or named decision | Applicable runtime behavior, consumer type or schema, documentation or examples, and tests |

Before the task can be `ready-for-implementation`, every material row needs evidence, an accepted decision, or an explicit `not-applicable` conclusion.

### Risks to verify

| Interaction that may fail | Why it matters | How to verify it | Scope |
|---|---|---|---|
| Boundary, branch, state, failure, concurrency, or compatibility interaction | Code, test, history, or reproducible evidence showing the risk | Focused test, analysis, benchmark, or review | Extra hardening unless the authoritative request makes it required behavior |

When asynchronous ordering matters, distinguish the order in which operations are called, settled (resolved or rejected), and made visible outside the component. If an ordinary asynchronous producer cannot make those orders differ, use a controllable adversarial test point that can.

Risk hardening protects the implementation. It does not add product requirements unless an authoritative source does so.

### Delivery work for this task

Link only the onboarding rows that this task triggers. A known delivery requirement must be linked before the task becomes `ready-for-implementation`.
