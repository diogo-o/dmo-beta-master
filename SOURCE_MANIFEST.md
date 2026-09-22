# Beta Source Manifest

This manifest tracks the source corpus being gathered into `dmo-beta-master`.

Status values:

- `FOUND` — source exists and has been verified in the source repository.
- `TO IMPORT` — source exists and should be copied under `sources/`.
- `HISTORICAL REFERENCE MISSING` — referenced by accepted material but not present at the currently inspected repository path/ref.
- `CANONICALIZED` — relevant content has been reconciled into canonical Beta documents here.
- `IMPLEMENTED / NOT YET ACCEPTED` — implementation exists in `DMO-MODULAR`, but no Architect implementation acceptance has been found yet.

## workbench — Beta frontend authority / history

Repository: `diogo-o/workbench`, branch `main`

| Source | Status | Purpose |
|---|---|---|
| `BETA_FRONTEND_IMPLEMENTATION_WORKSTREAMS.md` | FOUND / CANONICALIZED / TO IMPORT | Umbrella Beta frontend workstreams A–E, dependencies, gates, ownership and acceptance criteria. Canonicalized into Beta scope, module files, implementation model and integration seams. |
| `dev/plans/BETA_FRONTEND_WORKSTREAM_A_PLAN.md` | FOUND / CANONICALIZED / TO IMPORT | Shared frontend Workstream A plan. |
| `dev/plans/BETA_FRONTEND_WORKSTREAM_A2_CORRECTION_PLAN.md` | FOUND / CANONICALIZED / TO IMPORT | A2 production-navigation correction plan. |
| `dev/reviews/BETA_FRONTEND_WORKSTREAM_PLAN_V2_REVIEW.md` | FOUND / CANONICALIZED / TO IMPORT | Architect acceptance of umbrella V2 plan. |
| `dev/reviews/BETA_FRONTEND_WORKSTREAM_A_PLAN_REVIEW.md` | FOUND / CANONICALIZED / TO IMPORT | Workstream A plan review. |
| `dev/reviews/BETA_FRONTEND_WORKSTREAM_A1_CONTRACT_FREEZE_REVIEW.md` | FOUND / CANONICALIZED / TO IMPORT | A1 contract-freeze review. |
| `dev/reviews/BETA_FRONTEND_WORKSTREAM_A2_IMPLEMENTATION_REVIEW.md` | FOUND / TO IMPORT | A2 implementation review requiring correction; historical evidence only. |
| `dev/reviews/BETA_FRONTEND_WORKSTREAM_A2_CORRECTION_PLAN_REVIEW.md` | FOUND / TO IMPORT | A2 correction plan acceptance. |
| `dev/reviews/BETA_FRONTEND_WORKSTREAM_A2_CORRECTION_IMPLEMENTATION_REVIEW.md` | FOUND / CANONICALIZED / TO IMPORT | Accepted A2 correction implementation, consumed by navigation/application-foundation docs. |
| `BETA_DESIGN_RECONCILIATION_PLAN.md` | HISTORICAL REFERENCE MISSING | Umbrella plan names this as settled frontend design authority, but it is not currently present at the inspected `workbench/main` root. Must be recovered from history/another source before claiming it is migrated. |

## DMO-MODULAR — implementation target / shared frontend source

Repository: `diogo-o/DMO-MODULAR`, branch `main`

| Source | Status | Purpose |
|---|---|---|
| `docs/frontend/SHARED_FRONTEND_CONTRACT_FREEZE.md` | FOUND / CANONICALIZED / TO IMPORT | Accepted A1 shared frontend presentation contract. Consolidated into `contracts/SHARED_FRONTEND.md`. |
| `README.md` | FOUND / CANONICALIZED | Current modular-monolith construction model, shared-identity approach and document separation. Consolidated into `architecture/APPLICATION_FOUNDATION.md`. |
| current source tree | FOUND / selectively CANONICALIZED | Used to understand real integration seams only. Current code is implementation state, not product authority. |
| P1-T07 implementation commit `0b47690936599b6a71342b68b1cf36cfe4b64264` | IMPLEMENTED / NOT YET ACCEPTED | Navigation + USER shell implementation exists. No Architect implementation review was found at `dmo-work/dev/reviews/P1-T07_NAVIGATION_USER_SHELL_IMPLEMENTATION_REVIEW.md` during this pass. |

## dmo-master — global authority required by Beta

Repository: `diogo-o/dmo-master`, branch `dmo-modular`

The Beta master should carry snapshots of the exact global contracts it depends on, while clearly marking them as derived copies whose upstream authority remains `dmo-master/dmo-modular`.

| Source | Status | Purpose |
|---|---|---|
| `global/ACCESS_MODEL.md` | CANONICALIZED / TO IMPORT | Access/module/navigation invariants reflected in Beta access/navigation docs. |
| `global/MODULAR_IMPLEMENTATION_MODEL.md` | CANONICALIZED / TO IMPORT | Runtime/module implementation boundaries reflected in application/backend-frontend docs. |
| `modules/JOB_ON.md` | FOUND / CANONICALIZED / TO IMPORT | Global Job On authority constraining `modules/JOB_ON_LIGHT.md`. |
| `modules/FERRAMENTAS.md` | FOUND / CANONICALIZED / TO IMPORT | Global Tool/Ferramentas authority constraining `modules/FERRAMENTAS_LIGHT.md`. |
| `modules/CONTROLO.md` | FOUND / CANONICALIZED / TO IMPORT | Global Controlo authority used by Create/Approve Beta slices. |
| `modules/BOQUILHAS.md` | FOUND / CANONICALIZED / TO IMPORT | Global Boquilhas authority used by `modules/BOQUILHAS.md`. |
| `modules/ADMIN.md` | FOUND / selectively CANONICALIZED / TO IMPORT selectively | Shared Admin/access dependency context; not an operational Beta module. |
| `dev/WORKFLOW.md` | CANONICALIZED / TO IMPORT | Governance/test/acceptance protocol reflected in Beta `WORKFLOW.md`. |

## dmo-work — accepted Phase 1 foundation relevant to Beta

Repository: `diogo-o/dmo-work`, branch `main`

| Source | Status | Purpose |
|---|---|---|
| `plans/PHASE_1_ARCHITECTURE_PLAN.md` | FOUND / CANONICALIZED / TO IMPORT | Phase 1 sequence and foundation. |
| `dev/reviews/P1-T04_MODULE_REGISTRY_ACCESS_RESOLVER_IMPLEMENTATION_REVIEW.md` | FOUND / CANONICALIZED / TO IMPORT | ACCEPT. Module Registry, fail-closed access and server-side gate foundation. |
| `dev/reviews/P1-T05_USER_ADMINISTRATION_IMPLEMENTATION_REVIEW.md` | FOUND / CANONICALIZED / TO IMPORT | ACCEPT. Dedicated ADMIN/USER boundary and single nullable USER Template relation. |
| `dev/reviews/P1-T06_TEMPLATE_ADMINISTRATION_CORRECTION_REVIEW.md` | FOUND / CANONICALIZED / TO IMPORT | ACCEPT. Final Template administration semantics and concurrency corrections. |
| `dev/reviews/P1-T07_NAVIGATION_USER_SHELL_CORRECTED_PLAN_REVIEW.md` | FOUND / CANONICALIZED / TO IMPORT | PLAN ACCEPT. Runtime routing/landing/navigation decisions. |
| `dev/responses/P1-T07_NAVIGATION_USER_SHELL_IMPLEMENTATION_RESPONSE.md` | FOUND / CANONICALIZED / TO IMPORT | Developer reports implementation at `0b476909...`; useful current-state evidence, not acceptance. |
| `dev/reviews/P1-T07_NAVIGATION_USER_SHELL_IMPLEMENTATION_REVIEW.md` | NOT FOUND | No Architect implementation review found during this pass. Do not mark P1-T07 implementation accepted until such review exists. |

Relevant behavior has been consolidated into:

- `implementation/CURRENT_FOUNDATION.md`;
- `architecture/APPLICATION_FOUNDATION.md`;
- `architecture/ACCESS_AND_NAVIGATION.md`;
- `contracts/SHARED_FRONTEND.md`;
- `implementation/BETA_INTEGRATION_SEAMS.md`.

## Raw source archive still to do

Canonicalization is ahead of source-copy archiving.

The remaining provenance pass should copy selected original files under:

```text
sources/workbench/
sources/dmo-master/
sources/dmo-work/
sources/DMO-MODULAR/
```

Each copied file should preserve:

- source repository;
- source branch/ref;
- original path;
- source SHA where available;
- whether it is authority, accepted review, implementation evidence or historical material.

Raw source copies are evidence/history and do not automatically become current Beta authority.

## Migration rule

Raw source copies under `sources/` are evidence/history and must preserve provenance. They do not automatically become current Beta authority.

Canonical authority lives in the top-level and structured canonical folders of `dmo-beta-master` after reconciliation and review.
