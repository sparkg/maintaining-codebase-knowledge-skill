# External and Conversation Context

## Boundary

Enterprise systems remain canonical for the records they own. This Skill may consume user-provided or explicitly authorized task-scoped evidence, but it does not select assigned work, bulk-sync systems, transition tickets, edit PRDs, operate CI, or write status back.

Read only what the requested task needs. Never persist credentials, customer data, private discussion, restricted source text, or content forbidden by repository policy.

## Intake

Inspect only the current task's conversation and available attachments or exports. Do not claim access to other conversations or infer an inaccessible link.

For each material item, apply the dimensions from `knowledge-model.md` and record only what matters:

```text
Evidence kind and confidence
Canonical source or task identifier
Source version and provided/retrieved time
Sensitivity
Persistence decision
```

Examples:

| Input | Handling |
|---|---|
| Approved PRD or acceptance criteria | Versioned intent; not proof of implementation |
| User statement about behavior or failure | `intent` or `operational` claim with `Confidence: hypothesis` until independently supported |
| CI, test, runtime, or incident output | Operational evidence only when command/run, revision, environment, and time are sufficiently identified |
| Assignment, comments, workflow, or live CI state | Task-context or session only |
| Stable governed rule | Candidate durable knowledge after authority and repository permission are verified |
| Sensitive or unavailable content | Reject persistence or mark unavailable; never infer |

Capture important permitted context when provided because long conversations may be compacted, but distill rather than copy it.

## Persistence gate

Persist automatically only when content is stable, project-relevant, repository-permitted, sufficiently sourced, and unambiguous.

- Durable capability behavior or invariant goes to its canonical capability document.
- Task-specific acceptance, constraints, and missing decisions go to a repository-approved task context or intent-ledger row.
- Volatile workflow and operational state stays external or session-scoped.
- If authority, freshness, sensitivity, or permission is unclear, do not persist; report the decision and its readiness impact.

Do not assume `.agents/task-context/` is ignored or safe to commit. Check repository policy before writing. A task context should contain only identifiers, source snapshot, distilled requirement, acceptance criteria, constraints, relevant operational evidence, conflicts, and unresolved questions.

## External-system registry

Create `external-systems.md` only when external sources materially affect development. Record stable routing and governance: purpose, identifiers, owner, read method, source-of-truth rule, freshness, sensitivity, write-back authority, and failure behavior. Do not mirror records or list secret paths.
