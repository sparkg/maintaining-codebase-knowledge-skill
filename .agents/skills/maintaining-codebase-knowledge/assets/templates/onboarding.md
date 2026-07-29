<!-- codebase-knowledge:managed -->
# Onboarding and project baselines

Last verified: revision and date when material

Use this file to answer: What must be installed? Which commands should I run? Which known baseline failures matter? What extra delivery work does this type of change require?

## Stable prerequisites

Record repository-required versions, services, and configuration with evidence. One developer's missing local tool is not a project requirement.

## Commands

| Purpose | Command and working directory | Expected signal | Evidence |
|---|---|---|---|
| Setup, build, test, lint, type-check, run, benchmark, or verification | Verified command | Stable success or failure signal | Manifest, CI definition, docs, or reproducible run |

## Reproducible project baselines

Record only failures or constraints reproduced at a named revision and relevant environment. Keep incidental local failures in the run result.

## Delivery obligations by change type

Include this section only when repository or authorized policy provides a stable rule. These are delivery gates, not product requirements.

| When this applies | Required gate or artifact | Completion signal or command link | Canonical source |
|---|---|---|---|
| Evidence-backed change category and trigger | Test, check, review, docs, changelog, migration, benchmark, or release artifact | Observable result or link to a Commands row | Repository instruction, contribution guide, CI or release definition, or authorized policy |

Do not invent universal obligations. Task contract deltas link applicable rows instead of copying them.

If a changelog or release-notes file is relevant, distinguish ordinary accumulation of unreleased user-visible changes from release ceremony. Base the distinction on the current unreleased section, neighboring entries, and repository guidance—not the filename alone.
