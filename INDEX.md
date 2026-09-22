# DMO Beta Master — Documentation Index

## Start here

1. `README.md` — purpose of this repository.
2. `AUTHORITY.md` — authority hierarchy and conflict rules.
3. `BETA_SCOPE.md` — what is inside/outside the Beta.
4. `WORKFLOW.md` — controlled planning, review, implementation and acceptance workflow.
5. `IMPLEMENTATION_MODEL.md` — Workstreams A–E, ownership and dependencies.
6. `ACCEPTANCE_MATRIX.md` — consolidated Beta acceptance gates across frontend, backend, persistence, access, documents and tests.

## Architecture

- `architecture/APPLICATION_FOUNDATION.md` — how the Beta fits the real modular-monolith implementation foundation.
- `architecture/BACKEND_FRONTEND_MODEL.md` — backend/frontend responsibility boundary.
- `architecture/ACCESS_AND_NAVIGATION.md` — Template/Module access, navigation projection, landing and direct-route enforcement.
- `architecture/CROSS_MODULE_FLOWS.md` — how Tool, Job On, Controlo and Boquilhas connect.
- `architecture/RECORD_LIFECYCLES.md` — per-record lifecycle/state rules; no invented generic lifecycle engine.

## Canonical identities/contracts

- `contracts/IDENTITIES_AND_RELATIONSHIPS.md` — canonical IDs and relations used throughout the Beta.
- `contracts/SHARED_FRONTEND.md` — accepted shared frontend behavior and provisional-vs-final contract rules.
- `contracts/DOCUMENTS_AND_FILES.md` — PDF/document/file rules, availability states, naming and directory convention.

## Beta modules

- `modules/JOB_ON_LIGHT.md`
- `modules/FERRAMENTAS_LIGHT.md`
- `modules/CONTROLO_CREATE.md`
- `modules/CONTROLO_APPROVE.md`
- `modules/BOQUILHAS.md`

Each module file defines:

- included Beta scope;
- explicitly excluded future scope;
- canonical IDs and relationships;
- frontend/backend ownership;
- required backend contracts;
- access boundary;
- acceptance criteria;
- required evidence/tests.

## Implementation-state integration

- `implementation/CURRENT_FOUNDATION.md` — what is accepted vs merely implemented in the current Phase 1 foundation.
- `implementation/BETA_INTEGRATION_SEAMS.md` — explicit integration seams and backend/interface-blocker rules between B–E.

## Source migration / provenance

- `SOURCE_MANIFEST.md` — tracks source material being consolidated from `workbench`, `dmo-master`, `dmo-work` and `DMO-MODULAR`.
- `sources/README.md` — source archive classification/provenance rules.
- `sources/dmo-master/global/DOCUMENT_FILE_MODEL.md` — archived upstream document/file authority snapshot used by the Beta document contract.

Historical/source files are evidence and provenance. The canonical files above are the Beta working authority once accepted, subject to the global invariants of `dmo-master/dmo-modular`.

## Implementation target

Application implementation remains in:

`diogo-o/DMO-MODULAR`

This repository contains Beta authority/specification/governance, not application source code.

## Rule for agents

Before planning or implementing any Beta module:

```text
read AUTHORITY.md
→ read BETA_SCOPE.md
→ read WORKFLOW.md
→ read ACCEPTANCE_MATRIX.md
→ read architecture files relevant to the task
→ read shared contracts relevant to the task
→ read the target module file
→ inspect current DMO-MODULAR remote main
→ create task/workstream plan
→ Architect review
→ PLAN ACCEPT
→ implement
→ tests/evidence
→ Architect implementation review
```

If required live backend behavior is not defined/published, record a `BACKEND / INTERFACE BLOCKER`; do not invent production API, schema, identity or persistence behavior.
