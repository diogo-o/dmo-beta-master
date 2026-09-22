# Shared Frontend Contract — Beta Consolidation

## Authority

This file consolidates Beta-relevant behavior from the accepted A1 shared frontend contract in `DMO-MODULAR/docs/frontend/SHARED_FRONTEND_CONTRACT_FREEZE.md`.

It does not replace that source for historical proof. It is the Beta-facing summary used by Workstreams B–E.

## Fundamental boundary

Shared frontend components own:

- presentation;
- generic interaction mechanics;
- keyboard/focus behavior;
- accessibility behavior;
- common UI states.

They do not own:

- persistence;
- authorization;
- domain identity;
- endpoint contracts;
- calculation formulas;
- automatic industrial decisions;
- Tool/Job On association logic.

Consumer feature streams supply facts, state, available actions, labels and disabled reasons.

## Common state vocabulary

The Beta shared presentation vocabulary includes:

- `loading`;
- `ready`;
- `empty`;
- `lookup-failed`;
- `unavailable`;
- `permission-denied`;
- `saving`;
- `submitting`;
- `stale`;
- `conflict`.

These are presentation states, not backend error-code authority.

Critical distinction:

```text
empty != lookup-failed
empty != unavailable
empty != permission-denied
```

A feature must map real backend/application outcomes into the correct presentation state.

## ProductionContextStrip

Purpose:

Display human-facing production context while keeping it separate from editable content.

Expected visible context:

- Referência;
- Produção;
- Máquina/Linha;
- Processo;
- optional CM summary;
- optional MF summary;
- optional BQ summary.

Presentation owner: Workstream A.

Context/orchestration provider: Workstream B.

Consumers: B, C, D, E.

The component must never infer, repair or persist production context.

## ToolPicker

Purpose:

Reusable explicit Tool search/select/create presentation.

Final interaction rules:

- never auto-select a Tool candidate;
- even one result requires explicit human selection;
- ambiguous candidates remain separate;
- missing Tool may expose `Criar ferramenta` only when the consumer authorizes it;
- cancellation returns to the origin flow;
- unsaved origin state must be preservable;
- focus returns to the invoking control after close/return;
- presentation does not create or associate Tools.

Presentation owner: A.

Tool search/create/association orchestration owner: B.

Consumers: B, C, E.

## ToolSummaryRow

Compact supplied Tool display used across module workflows.

It renders supplied facts such as:

- Tool type;
- reference;
- lot;
- machines/lines;
- quantity where applicable;
- process;
- supplied actions/status.

It does not become a Tool authority.

## DenseDataTable

Common dense operational table behavior:

- single-click selects;
- double-click opens where the consumer supplies that behavior;
- keyboard focus is explicit;
- loading/empty/error states remain distinct;
- filters/pagination are consumer-owned facts;
- selection itself does not silently trigger domain actions.

Consumers include Job On history, Controlo lists and Boquilhas History.

## RecordStatus

Status rendering must be understandable without color.

The component receives status meaning from the owning domain. It does not invent state machines.

## AvailabilityState

Used for documents/context availability.

Must distinguish situations such as:

- available;
- not generated;
- awaiting approval;
- workspace unavailable;
- file missing;
- not applicable;
- lookup failure.

Optional missing output is not automatically an error.

## AuditTrail

Renders supplied audit/history facts:

- actor;
- timestamp;
- action;
- optional before/after detail.

Frontend never synthesizes actor/time facts.

## MeasurementRows

Generic repeatable-row editor used primarily by Controlo.

A owns generic row mechanics.

C owns domain row schemas and rules.

Required generic capabilities:

- variable rows;
- add/remove;
- stable row identity;
- validation hooks;
- configurable at-least-one-row behavior.

No Peso or Pegamentos formula belongs in the generic component.

## DecisionBar

Generic presentation of feature-supplied actions.

Possible actions may include create/edit/submit/approve/reject/reopen, but the component does not define which transition is legal.

It must support:

- primary/secondary/danger actions;
- disabled reasons;
- pending state;
- duplicate-action prevention.

## Navigation seam

Navigation presentation consumes the published Module/access model.

Rules:

- visibility is never authorization;
- only granted + available + routed + non-contextual Modules become normal top-level destinations;
- shared DestinationId may collapse visually while underlying Module grants stay separate;
- Ferramentas/Ferramentas Approve remain contextual-only;
- frontend does not create its own permission enums, roles or profile-based variants.

## Provisional frontend contracts

Where the live backend contract is not yet available, B–E may use frontend-only fixtures/view models.

Such artifacts mean:

```text
frontend development/testing only
no backend authority
no persistence authority
no schema
no canonical identity
no definitive endpoint naming
no domain ownership
```

When the real backend contract arrives, the frontend adapter must conform to it. The backend is never redesigned merely to fit a provisional fixture.

## Cross-stream change rule

Shared contract changes require coordinated ownership.

A feature stream must not silently modify A-owned generic behavior to make a feature easier to implement.

If a feature needs a shared-contract change:

1. state the requested change;
2. identify affected consumers;
3. state compatibility impact;
4. update contract fixtures/tests;
5. coordinate merge order;
6. receive the required owner/Architect acceptance before implementation.
