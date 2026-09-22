# Beta Source Manifest

This manifest tracks the source corpus being gathered into `dmo-beta-master`.

Status values:

- `FOUND` — source exists and has been verified in the source repository.
- `TO IMPORT` — source exists and should be copied under `sources/`.
- `HISTORICAL REFERENCE MISSING` — referenced by accepted material but not present at the currently inspected repository path/ref.
- `CANONICALIZED` — relevant content has been reconciled into canonical Beta documents here.

## workbench — Beta frontend authority / history

Repository: `diogo-o/workbench`, branch `main`

| Source | Status | Purpose |
|---|---|---|
| `BETA_FRONTEND_IMPLEMENTATION_WORKSTREAMS.md` | FOUND / TO IMPORT | Umbrella Beta frontend workstreams A–E, dependencies, gates, ownership and acceptance criteria. |
| `dev/plans/BETA_FRONTEND_WORKSTREAM_A_PLAN.md` | FOUND / TO IMPORT | Shared frontend Workstream A plan. |
| `dev/plans/BETA_FRONTEND_WORKSTREAM_A2_CORRECTION_PLAN.md` | FOUND / TO IMPORT | A2 production-navigation correction plan. |
| `dev/reviews/BETA_FRONTEND_WORKSTREAM_PLAN_V2_REVIEW.md` | FOUND / TO IMPORT | Architect acceptance of umbrella V2 plan. |
| `dev/reviews/BETA_FRONTEND_WORKSTREAM_A_PLAN_REVIEW.md` | FOUND / TO IMPORT | Workstream A plan review. |
| `dev/reviews/BETA_FRONTEND_WORKSTREAM_A1_CONTRACT_FREEZE_REVIEW.md` | FOUND / TO IMPORT | A1 contract-freeze review. |
| `dev/reviews/BETA_FRONTEND_WORKSTREAM_A2_IMPLEMENTATION_REVIEW.md` | FOUND / TO IMPORT | A2 implementation review requiring correction. |
| `dev/reviews/BETA_FRONTEND_WORKSTREAM_A2_CORRECTION_PLAN_REVIEW.md` | FOUND / TO IMPORT | A2 correction plan acceptance. |
| `dev/reviews/BETA_FRONTEND_WORKSTREAM_A2_CORRECTION_IMPLEMENTATION_REVIEW.md` | FOUND / TO IMPORT | Accepted A2 correction implementation. |
| `BETA_DESIGN_RECONCILIATION_PLAN.md` | HISTORICAL REFERENCE MISSING | Umbrella plan names this as settled frontend design authority, but it is not currently present at the inspected `workbench/main` root. Must be recovered from history/another source before claiming it is migrated. |

## DMO-MODULAR — accepted shared frontend contract

Repository: `diogo-o/DMO-MODULAR`, branch `main`

| Source | Status | Purpose |
|---|---|---|
| `docs/frontend/SHARED_FRONTEND_CONTRACT_FREEZE.md` | FOUND / TO IMPORT | Accepted shared frontend contract freeze used by Beta workstreams. |

## dmo-master — global authority required by Beta

Repository: `diogo-o/dmo-master`, branch `dmo-modular`

The Beta master should carry snapshots of the exact global contracts it depends on, while clearly marking them as derived copies whose upstream authority remains `dmo-master/dmo-modular`.

| Source | Status | Purpose |
|---|---|---|
| `global/ACCESS_MODEL.md` | TO IMPORT | Access/module/navigation invariants. |
| `global/MODULAR_IMPLEMENTATION_MODEL.md` | TO IMPORT | Runtime/module implementation boundaries. |
| `modules/JOB_ON.md` | FOUND / TO IMPORT | Global Job On authority used to constrain Job On Light. |
| `modules/FERRAMENTAS.md` | FOUND / TO IMPORT | Global Tool/Ferramentas authority used to constrain Ferramentas Light. |
| `modules/CONTROLO.md` | FOUND / TO IMPORT | Global Controlo authority used by Create/Approve Beta slices. |
| `modules/BOQUILHAS.md` | FOUND / TO IMPORT | Global Boquilhas authority. |
| `modules/ADMIN.md` | FOUND / TO IMPORT selectively | Shared Admin/access dependency context; not an operational Beta module. |
| `dev/WORKFLOW.md` | TO IMPORT | Governance/test/acceptance protocol used by Beta workstreams. |

## dmo-work — accepted Phase 1 foundation relevant to Beta

Repository: `diogo-o/dmo-work`, branch `main`

Import the accepted plans/reviews that define the foundations consumed by Beta, especially:

- P1-T04 Module Registry + Access Resolver;
- P1-T05 USER Administration;
- P1-T06 Template Administration;
- P1-T07 Navigation + USER Shell;
- the current Phase 1 architecture plan.

These should be preserved under `sources/dmo-work/` as implementation-foundation evidence, while canonical Beta docs refer only to the behavior actually required by the Beta.

## Migration rule

Raw source copies under `sources/` are evidence/history and must preserve provenance (source repository, branch/ref and original path). They do not automatically become current Beta authority.

Canonical authority lives in the top-level and structured canonical folders of `dmo-beta-master` after reconciliation and review.
