<!-- codebase-knowledge:managed -->
# Architecture

Last verified: code revision and date, when relevant

## System at a glance

Answer the questions a new reader needs first. Link to the detailed sections instead of repeating them.

| Question | Short answer | Evidence |
|---|---|---|
| What kind of system is this, and who uses it? | Project type, users, and primary purpose | Manifest, entry point, or product documentation |
| How does it run and ship? | Runtime, build, deployment, and extension model | Manifest, CI, or deployment definition |
| Where does a change usually belong? | Link to the capability-to-implementation map and the evidence for current boundaries | Detailed rows below |
| How is a change verified? | Link to setup commands, test commands, and delivery checks | `onboarding.md` |

## System boundary

Describe who uses the system, what sits inside and outside it, its runtime or deployment units, its data stores, and the external systems it calls while running.

## Code units and dependency direction

| Code or deployment unit | Responsibility | Boundary exposed to other units | Depends on | Evidence |
|---|---|---|---|---|
| Module, package, service, or deployment unit | One clear responsibility | API, event, CLI, job, or internal interface | Dependencies, with direction | Paths or manifests |

## Why the current boundaries look this way

Include only important units. For each boundary, cite an accepted decision, describe only the structure visible in the code, or mark the reason `unknown`. Do not turn an observed layout into a claim about the original design intent.

| Unit | What tends to change together | Evidence for the boundary | Existing extension point | Signs that the boundary is under pressure | Evidence |
|---|---|---|---|---|---|
| Corresponding code or deployment unit | Related behavior, state, or dependencies | Decision link, observed structure only, or `unknown` | Existing interface or registration point | Verified coupling, dependency cycle, bypassed invariant, test coupling, or `none observed` | Evidence type, confidence, and path |

## Capability-to-implementation map

This table is the single maintained map between business capabilities and implementation units such as modules, services, or deployment units. Capability documents link here instead of copying it.

| Capability | Main implementation unit | Supporting implementation units | Shared infrastructure | Evidence |
|---|---|---|---|---|
| Link to the capability document | Unit that owns the main runtime behavior or state | Other participating units | Cross-cutting mechanism only | Paths or tests |

## Data and external integrations

Describe important databases, queues, caches, file or object storage, and external systems used at runtime. Link to the document that owns the operational or capability details.

## Concerns that affect the whole system

Evaluate each concern that matters to this repository. Record verified mechanisms, important unknowns, and explicit `not-applicable` conclusions. Omit concerns that have no material effect.

| Concern | Status | Default mechanism and responsible unit | Evidence | Effect on development |
|---|---|---|---|---|
| Error handling and resilience | `established`, `unknown`, or `not-applicable` | System-wide default and responsible unit | Path, manifest, command, or source snapshot | Constraint, required check, or none |
| Logging, metrics, tracing, and audit | `established`, `unknown`, or `not-applicable` | System-wide default and responsible unit | Path, manifest, command, or source snapshot | Constraint, required check, or none |
| Security and sensitive-data handling | `established`, `unknown`, or `not-applicable` | System-wide default and responsible unit | Path, manifest, command, or source snapshot | Constraint, required check, or none |
| Caching and consistency | `established`, `unknown`, or `not-applicable` | System-wide default and responsible unit | Path, manifest, command, or source snapshot | Constraint, required check, or none |
| Rate limiting and backpressure | `established`, `unknown`, or `not-applicable` | System-wide default and responsible unit | Path, manifest, command, or source snapshot | Constraint, required check, or none |
| Dependency and configuration management | `established`, `unknown`, or `not-applicable` | System-wide default and responsible unit | Path, manifest, command, or source snapshot | Constraint, required check, or none |
| Concurrency, background jobs, and transactions | `established`, `unknown`, or `not-applicable` | System-wide default and responsible unit | Path, manifest, command, or source snapshot | Constraint, required check, or none |
| Delivery, health checks, recovery, migrations, and rollback | `established`, `unknown`, or `not-applicable` | System mechanism and responsible unit; link developer checks from onboarding | Path, manifest, command, or source snapshot | Architecture constraint or none |

## Architecture constraints

Record only defaults and constraints that apply across the system. Link to capability-specific behavior or exceptions and to flow documents for propagation rules; do not copy them here.
