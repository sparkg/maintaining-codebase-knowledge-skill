# Using External and Conversation Context

## Boundary

An enterprise system remains authoritative for the records it owns. This Skill may read evidence that the user provides or explicitly authorizes for the current task. It does not choose assigned work, copy an external system in bulk, move tickets, edit PRDs, operate CI, or write status back.

Read only what the current task needs. Never persist credentials, customer data, private discussion, restricted source text, or anything the repository forbids storing.

## Taking in external information

Inspect only the current conversation and the attachments or exports available to it. Do not claim access to other conversations or guess the contents of a link that cannot be opened.

For each item that may affect development, apply the evidence rules from `knowledge-model.md` and record only:

```text
Evidence type and confidence
Authoritative source or task identifier
Source version and time provided or retrieved
Sensitivity
Decision about whether and where to persist it
```

| Input | How to handle it |
|---|---|
| Approved PRD or acceptance criteria | Record it as versioned evidence of intended behavior, not proof of the current implementation |
| User statement about behavior or a failure | Record it as an intent or operational hypothesis with confidence `hypothesis` until independent evidence supports it |
| CI, test, runtime, or incident output | Treat it as operational evidence only when the run or command, code revision, environment, and time are identified |
| Assignment, comment, workflow status, or live CI status | Keep it in the current task context or session only |
| Stable rule governed by the organization or repository | Consider it for durable project knowledge after verifying its authority and the repository's permission to store it |
| Sensitive or unavailable content | Do not persist it; mark it unavailable when useful, and never infer its contents |

Capture permitted information while it is available because a long conversation may later be compacted. Summarize the development-relevant facts instead of copying the source.

## Deciding what may be persisted

Persist information automatically only when it is stable, relevant to the project, allowed by repository policy, sufficiently sourced, and unambiguous.

- Put durable behavior or an invariant in the relevant capability document.
- Put task-specific acceptance, constraints, and missing decisions in an approved task context or intent-ledger row.
- Keep changing workflow and operational status in the external system or current session.
- If authority, freshness, sensitivity, or permission is unclear, do not persist the information. Report why and how that uncertainty affects readiness.

Do not assume `.agents/task-context/` is ignored by version control or safe to commit. Check repository policy before writing. A saved task context contains only identifiers, source information, distilled requirements, acceptance criteria, constraints, relevant operational evidence, conflicts, and unresolved questions.

## Recording external systems

Create `external-systems.md` only when an external source affects development. Record how to locate and govern the source: its purpose, identifiers, responsible team, read method, authoritative-record rule, refresh condition, sensitivity, write-back permission, and failure behavior. Do not copy whole records or list paths to secrets.
