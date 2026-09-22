# Beta Integration Seams

## Purpose

This file defines how the operational Beta workstreams plug into the accepted application foundation without crossing ownership boundaries.

It does not define endpoint names or physical database schemas that have not yet been accepted.

## Workstream A — shared frontend

A owns generic presentation and shell behavior.

Operational streams B–E consume A contracts rather than forking local equivalents.

Primary shared seams:

- ProductionContextStrip;
- ToolPicker presentation;
- ToolSummaryRow;
- DenseDataTable;
- RecordStatus;
- AvailabilityState;
- AuditTrail;
- MeasurementRows;
- DecisionBar;
- shell/navigation presentation.

## Workstream B — Job On Light + Ferramentas Light

B owns the operational adapters/orchestration that other Beta features need for:

- canonical Tool search/select/create;
- explicit Tool selection;
- Job On select/create;
- Job On production-context loading;
- CM/MF/BQ production-context resolution;
- Job On Light create/edit/view/history/duplicate;
- contextual Tool ficha entry points.

B must not create private Controlo or Boquilhas state.

### B -> C seam

C needs from B:

```text
selected/created jobon_id
production context
CM context when present
shared Tool selection/create flow when CM/MF/BQ context is missing
```

C must not reconstruct Job On or Tool relationships from display text.

### B -> E seam

E needs from B:

```text
canonical BQ Tool selection/create
optional selected jobon_id
production-linked bq_id resolution/reuse
return to the same Boquilhas origin state after Tool creation
```

Standalone Boquilhas remains valid without Job On.

## Workstream C — Controlo Create

C owns creation/edit/submission workflows for:

- Peso;
- Comparison inside Peso;
- Pegamentos;
- Folha de Controlo;
- Resumo;
- Create-side history/document views.

C publishes the canonical read-only Peso presentation/read model that D consumes.

C does not own Tool master data or Job On production planning.

### C -> D seam

D consumes:

```text
submitted peso_id
canonical read-only Peso sheet/read model
Comparison context
warnings/results
submission attribution/status
Folha review projection where applicable
```

D must not fork or redefine the Peso renderer/formulas.

### C -> B reverse status seam

Job On Light may show related Controlo status/document availability through an explicit backend/read contract.

B must not read C's private tables or copy Controlo state into Job On as a second authority.

## Workstream D — Controlo Approve

D owns review and decision orchestration over the exact submitted Controlo records.

D needs backend operations/read contracts for:

- pending work list;
- exact submitted Peso retrieval;
- approve;
- reject;
- reopen;
- per-CM decisions where required;
- Folha decision where required;
- audit/decision history;
- confirmed send-to-production action where the owning contract allows it.

D never creates an approval-copy Peso.

## Workstream E — Boquilhas

E owns:

- aggregate registration/opening;
- movement entry;
- derived balance presentation;
- movement edit + audit;
- History;
- close/reopen;
- production-linked and standalone repair-flow orchestration.

E depends on B only for Tool/Job On context orchestration, not for its movement ledger.

### E -> B reverse status seam

Job On Light may show related Boquilhas state through an explicit backend/read contract.

Job On does not own Boquilhas movement facts.

## Access/action seam

Every feature surface consumes published effective Module access.

Examples:

- Job On View;
- Job On Create;
- Controlo Create;
- Controlo Approve;
- Boquilhas;
- Ferramentas;
- Ferramentas Approve.

Rules:

- route/action visibility derives from effective access;
- server-side policy still enforces access;
- a shared destination does not merge Module identities;
- Ferramentas remains contextual-only;
- ADMIN does not receive USER operational access automatically.

## Navigation/route-registration seam

A feature becomes a live top-level destination only when all required runtime facts are true:

```text
canonical Module exists
+ Module is available in the current build
+ Module is non-contextual
+ owning feature has a real route
+ route is registered through the shared route-registry seam
+ USER is granted the Module through the Template
```

A feature implementation must not mark itself available before the corresponding supported surface exists.

## Backend/interface blocker rule

When a required live contract does not exist:

```text
BACKEND / INTERFACE BLOCKER
```

Record:

- exact missing interaction;
- owning module/domain;
- affected frontend slice;
- whether isolated frontend presentation can continue;
- why inventing a temporary production contract would be unsafe.

Then create a separately authorized backend task.

Do not solve blockers by:

- local browser persistence pretending to be backend truth;
- fake canonical IDs;
- invented endpoint names treated as final;
- client-side authorization;
- reading another module's internal tables directly;
- duplicating foreign module state.

## Concurrency and stale data

Where the backend reports stale/conflict state, frontend uses the shared `stale` / `conflict` presentation vocabulary.

Do not silently retry a conflicting mutation or overwrite newer persisted data without the owning workflow explicitly allowing it.

## Testing seam

Each feature must test both presentation and backend enforcement boundaries.

Examples:

- hidden action and direct-route denial;
- shared visible destination but sibling Module action denied;
- ToolPicker never auto-selects;
- cross-module read projection does not mutate foreign state;
- submitted Peso review does not edit submitted facts before reopen;
- Boquilhas edit creates audit history without creating a second quantity movement;
- Job On duplicate creates new context IDs and preserves source.
