<!-- codebase-knowledge:managed -->
# Architecture

Last verified: revision and date when material

## Project profile

| Aspect | Current state | Evidence | Development impact |
|---|---|---|---|
| Project type, users, technology stack, runtime, build, deployment, or extension model | Evidence-backed summary | Manifest, entry point, CI, or deployment path | Constraint relevant to development |

## System boundary

Describe users, the system boundary, runtime containers or deployment units, data stores, and external runtime systems.

## Modules and dependency direction

| Unit | Responsibility | Public boundary | Dependencies | Evidence |
|---|---|---|---|---|
| Module, package, service, or deployment unit | Sole responsibility | API, event, CLI, job, or internal boundary | Directional dependencies | Paths or manifests |

## Module boundary rationale

Include material units only. `Boundary basis` is an accepted decision, observed implementation structure only, or `unknown`; never present observed layout as original design intent.

| Unit | Observed cohesion or change axis | Boundary basis | Extension seams | Boundary pressure | Evidence |
|---|---|---|---|---|---|
| Canonical unit | Behaviors, state, or dependencies that change together | Decision link, observed structure only, or unknown | Existing interface or registration point | Verified coupling, cycle, invariant bypass, test coupling, or none observed | Evidence kind, confidence, and path |

## Capability-to-unit map

Keep this as the canonical many-to-many mapping; capability documents link here instead of copying it.

| Capability | Primary unit | Supporting units | Shared infrastructure | Evidence |
|---|---|---|---|---|
| Canonical capability link | Main runtime or state owner | Other participating units | Cross-cutting mechanism only | Paths or tests |

## Data and integration landscape

Describe only material stores, queues, caches, file or object storage, and external runtime integrations. Link their canonical operational or capability documentation.

## Architecture concerns

Evaluate every concern category represented below. Include verified mechanisms, material unknowns, and explicit not-applicable conclusions; omit immaterial categories and empty boilerplate.

| Concern | Status | Mechanism or owner | Evidence | Development impact |
|---|---|---|---|---|
| Error handling and resilience | established, unknown, or not-applicable | System-wide default and owner | Path, manifest, command, or source snapshot | Constraint, required check, or none |
| Logging, metrics, tracing, and audit | established, unknown, or not-applicable | System-wide default and owner | Path, manifest, command, or source snapshot | Constraint, required check, or none |
| Security and sensitive-data handling | established, unknown, or not-applicable | System-wide default and owner | Path, manifest, command, or source snapshot | Constraint, required check, or none |
| Caching and consistency | established, unknown, or not-applicable | System-wide default and owner | Path, manifest, command, or source snapshot | Constraint, required check, or none |
| Rate limiting and backpressure | established, unknown, or not-applicable | System-wide default and owner | Path, manifest, command, or source snapshot | Constraint, required check, or none |
| Dependency and configuration management | established, unknown, or not-applicable | System-wide default and owner | Path, manifest, command, or source snapshot | Constraint, required check, or none |
| Concurrency, jobs, and transactions | established, unknown, or not-applicable | System-wide default and owner | Path, manifest, command, or source snapshot | Constraint, required check, or none |
| Delivery, health, recovery, migrations, and rollback | established, unknown, or not-applicable | System-wide default and owner | Path, manifest, command, or source snapshot | Constraint, required check, or none |

## Architecture constraints

Record only system-wide defaults and constraints. Link capability-specific behavior or exceptions and flow-owned propagation instead of copying them.
