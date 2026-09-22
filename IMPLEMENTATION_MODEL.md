# DMO Beta — Implementation Model

## Objective

The Beta is implemented as a set of controlled workstreams over the accepted modular foundation. Development order may be parallel; integration/merge order remains coordinated.

## Foundation before operational Beta

The operational Beta depends on the accepted application foundation already being built in `DMO-MODULAR`:

- authentication/account boundary;
- persistence foundation;
- Module Registry + Access Resolver;
- USER administration;
- Template administration;
- navigation + USER shell.

These are infrastructure/foundation slices, not replacements for the operational Beta modules.

## Operational workstreams

### A — Shared frontend foundation

Owns shared presentation only:

- application shell/navigation presentation;
- shared layout patterns;
- dense form/table primitives;
- `ProductionContextStrip`;
- `ToolPicker` presentation;
- `ToolSummaryRow`;
- `DenseDataTable`;
- `RecordStatus`;
- `AvailabilityState`;
- `AuditTrail`;
- generic `MeasurementRows`;
- generic `DecisionBar`;
- loading/empty/failure/permission states;
- keyboard/focus and responsive behavior.

Does not own module-specific domain rules, persistence, formulas or authorization semantics.

### B — Job On Light + Ferramentas Light

Owns:

- Job On Light create/view/edit;
- Reference -> productions -> record history;
- explicit source duplication;
- optional CM/MF/BQ associations;
- Ferramentas Light search/list/detail;
- explicit Tool selection/create orchestration;
- canonical `tool_id` return path;
- production-context adapter consumed by other streams.

Beta Job On remains intentionally light. It does not include the full future planning/revision/lifecycle workflow.

### C — Controlo Create

Owns:

- select/create Job On context;
- CM resolution/reuse;
- Peso draft/edit/calculation/submission;
- variable measurement rows;
- explicit previous Peso selection;
- Comparação pairing/build/rebuild;
- Pegamentos;
- Folha preparation/submission;
- Resumo preparation;
- Create-side history/document projections;
- shared Peso renderer/read model used by Approve.

Create transitions the same `peso_id` into the submitted/reviewable state.

### D — Controlo Approve

Owns:

- pending list/filters;
- review of the exact submitted Peso;
- Comparison review;
- per-CM decisions where defined;
- Folha decision;
- approve/reject;
- reopen;
- decision/audit history;
- explicit send-to-production step where authorised.

D consumes C's shared read-only Peso representation and must not fork it.

### E — Boquilhas

Owns:

- production-linked and standalone Boquilhas flows;
- aggregate summary;
- movement entry;
- derived balances/state display;
- History;
- edit with before/after audit;
- close/reopen;
- repairer/line context consumption.

Current Beta write movement vocabulary:

```text
Início
Saída
Entrada
Irreparável
```

`Editar` is an action on an existing movement, not a fifth movement type.

## Shared frontend contracts

Shared contracts are implemented once and consumed by B–E. Feature streams must not create private look-alikes.

| Shared contract | Presentation owner | Domain/adapter owner |
|---|---|---|
| Shell/navigation | A | access/Admin integration |
| ProductionContextStrip | A | B |
| ToolPicker | A | B |
| ToolSummaryRow | A | B |
| DenseDataTable | A | each feature configures columns/data |
| RecordStatus | A | owning feature/backend provides facts |
| AvailabilityState | A | document/feature adapters |
| AuditTrail | A | owning feature/backend provides facts |
| MeasurementRows | A | C supplies measurement schema/rules |
| DecisionBar | A | C/D/E supply actions |
| Shared Peso sheet/read model | C | C/backend; D consumes |

## Dependency model

```text
A shared contracts
   ↓
B Job On / Tool orchestration
   ↓            ↘
C Controlo       E Boquilhas
   ↓
D Controlo Approve
```

This is a dependency graph, not a rule that development must be fully sequential.

Once accepted interfaces exist:

- B may proceed without A being feature-complete;
- C may proceed against accepted A/B contracts;
- D may proceed once C publishes the submitted Peso read model;
- E may proceed once A/B contracts it consumes are stable.

## Suggested integration order

1. accepted shared frontend contracts/shell;
2. Job On/Tool adapters and surfaces;
3. Controlo Create and shared Peso representation;
4. Controlo Approve consumer;
5. Boquilhas (may integrate earlier if dependencies are ready);
6. final access/action integration across operational modules;
7. cross-module links, document availability and end-to-end verification.

## Backend tasks

Frontend workstream authorization does not automatically authorize backend/domain/schema changes.

When live integration reveals a missing backend capability, create a separate authorized backend task. Typical areas include:

- Job On/Tool query and mutation contracts;
- CM/MF/BQ production-context association;
- Peso calculation/persistence/submission/review;
- previous-Peso candidate/pairing;
- Pegamentos/Folha/Resumo;
- Boquilhas aggregate/movement/edit-audit/close-reopen;
- document availability/open/generation;
- access/action enforcement integration.

The backend task must preserve the canonical identities and relationships defined by Beta/global authority.

## Production readiness rule

A module becomes a real navigable Beta surface only when all required layers exist:

```text
canonical Module identity
+ build availability registration
+ real server-side authorization gate
+ real route/surface registration
+ accepted backend contract for live behavior
+ accepted frontend implementation
```

Do not mark a future module available merely to make navigation appear.

## Acceptance model

A workstream is not complete because a developer says it is complete. Acceptance requires inspection of the actual implementation commit against:

- Beta canonical behavior;
- global DMO invariants;
- shared-contract ownership;
- exact changed files;
- schema/migration scope;
- tests and required non-effects;
- remote Git evidence.
