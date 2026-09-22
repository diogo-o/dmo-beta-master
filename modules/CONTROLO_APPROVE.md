# Controlo Approve — Beta canonical contract

## Purpose

Controlo Approve owns the Beta review and human decision workflow over the exact submitted Controlo records. It consumes the same persisted Peso/Folha facts created by Controlo Create; it does not create an approval copy or redefine the Peso renderer.

## Authority

- Beta scope/workstream: `workbench/BETA_FRONTEND_IMPLEMENTATION_WORKSTREAMS.md` Workstream D.
- Global domain authority: `dmo-master/dmo-modular/modules/CONTROLO.md`.

## Included in Beta

- pending/review work list and filters;
- open the exact submitted Peso;
- read measurements, results, warnings and production context;
- read explicit Comparison relation and current/previous context;
- per-CM human decisions such as `Manter` / `Colocar de parte` where required;
- Folha decision/review;
- approve;
- reject with note where required;
- reopen;
- attributed decision/reopen history;
- explicit confirmed `Enviar para produção` only where allowed by the published backend contract.

## Explicitly outside Beta

- editing submitted measurement facts without reopen;
- creating a second approval-copy Peso;
- redefining calculation formulas;
- redefining Comparison pairing;
- forking/copying the Peso renderer owned by Controlo Create;
- Job On/Ferramentas/Boquilhas ownership;
- automatic decisions derived from warnings.

## Identity and persistence

Approve operates on the same record created/submitted by Create:

```text
submitted peso_id
→ same persisted Peso record
→ cm_id or pending direct tool_id context
```

A review action changes the lifecycle/status of that same `peso_id`; no second record exists merely to hold copied values for approval.

Folha decisions likewise operate on the exact persisted `controlo_sheet_id`.

## Human decision principle

Warnings, calculated results and thresholds are evidence for the human decision. They are never themselves the decision.

The system must not:

- auto-approve;
- auto-reject;
- auto-select `Manter` or `Colocar de parte`;
- reinterpret a warning as a permission/state transition.

## Review workflow

```text
pending list
→ explicit select/open submitted record
→ inspect exact submitted facts
→ inspect Comparison and warnings where present
→ human chooses decision
→ backend validates and persists transition + attribution
→ audit/history remains readable
```

## Reopen

Reopen changes the same record back into the accepted editable lifecycle state defined by the backend/domain contract. It does not clone the record.

Reopen must preserve audit attribution and prior decision history.

## Shared Peso presentation

Controlo Create owns the canonical reusable Peso sheet/read-model contract.

Controlo Approve consumes that renderer in read/review mode.

Rules:

- no duplicate renderer;
- no divergent field ordering or hidden result set that changes meaning;
- review mode may configure visibility/actions but must not fork the domain representation.

## Comparison review

Review uses the persisted explicit relation:

```text
current_peso_id -> previous_peso_id
```

It must show enough current/previous context for the human to understand the comparison without reconstructing the relation heuristically.

Per-CM decisions remain explicit human facts where the domain requires them.

## Access

`Controlo Approve` is a distinct assignable module.

It may share one visible `controlo` destination with `Controlo Create`, while backend route/action gates remain independent.

Having Create does not grant Approve; having Approve does not imply Create unless separately established by the global access contract.

## Frontend responsibility

Frontend may:

- render pending records and filters;
- render the shared read-only Peso/Comparison/Folha views;
- collect explicit decision input;
- show disabled reasons and conflict states;
- render audit trail.

Frontend must not:

- change submitted facts silently;
- duplicate a record for approval;
- implement authorization by button visibility alone;
- synthesize actor/time;
- infer approval outcome from warnings/calculations.

## Backend contracts required

- pending/review query;
- exact submitted Peso/Folha read models;
- approve/reject/reopen actions;
- per-CM decision persistence where required;
- audit/history;
- conflict/concurrency behavior;
- optional send-to-production action where authorized.

## Acceptance criteria

- pending list shows only backend-reported reviewable facts;
- opening a record loads the exact submitted `peso_id`;
- approval/rejection/reopen mutate lifecycle of the same record;
- submitted facts are read-only until an authorized reopen;
- warnings never trigger automatic decisions;
- Comparison uses the exact persisted `previous_peso_id`;
- C's renderer is reused rather than copied;
- actor/time come from backend facts;
- direct route/action enforcement remains server-side.

## Required evidence

- pending-list/filter tests;
- exact-record review tests;
- approve/reject/reopen same-ID tests;
- submitted-facts immutability tests;
- warning-does-not-decide tests;
- shared renderer contract tests with C;
- access tests for Create vs Approve separation;
- audit attribution tests;
- conflict/concurrency tests where backend exposes them.
