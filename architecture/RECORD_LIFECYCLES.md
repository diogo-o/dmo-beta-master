# Beta Record Lifecycles

## Purpose

This file consolidates lifecycle/state behavior required by the Beta. It does not invent a generic state engine. Each record keeps its own domain semantics.

## 1. Global rule

Do not normalize unrelated records into one generic lifecycle.

Examples of intentionally different behavior:

- Job On has no global persisted lifecycle state machine;
- Peso has control status and approval/reopen behavior;
- Boquilhas has aggregate close/reopen facts plus append-only movements;
- Tool edits use change requests where Ferramentas approval applies;
- document availability is not a domain lifecycle.

## 2. Job On Light

`jobon_id` is one production occurrence.

There is no canonical Job On-wide status such as:

```text
Draft / Planned / In Production / Closed / Cancelled
```

Do not add one in the Beta merely to simplify UI.

Protection is fact-based:

- before planned production date: normal edit according to Job On Create permission;
- date reached/passed: edit remains possible but requires explicit warning/confirmation;
- delete always requires confirmation;
- if dependent operational facts exist, hard delete is forbidden;
- no automatic cancellation state is introduced.

Duplicating a Job On creates a new `jobon_id` and new CM/MF/BQ context IDs. It is assisted creation, not a revision lifecycle.

## 3. CM/MF/BQ contexts

`cm_id`, `mf_id`, `bq_id` are historical production-specific contexts.

At Job On Create/Generate, the selected Tool relation and required captured Tool-facing values freeze for that production occurrence.

Ordinary editing must not silently rewrite those frozen historical fields from live Tool state.

## 4. Peso

`peso_id` is one Peso result/control fact.

Canonical status vocabulary:

```text
Pendente
Aprovado
Não aprovado
```

Comparação is a record type/workflow relation, not a status.

### Creation / measurement

Controlo Create owns creation and measurement. A Peso may be:

- production-bound via `cm_id`;
- pending Job On association via direct `tool_id`.

`Job On por associar` is a context/association condition, not an approval status and does not block measurement or approval.

### Submission

The same `peso_id` moves from editable creation/measurement into submitted review. Submission does not create a second approval-copy identity.

### Approval

Controlo Approve reviews the submitted `peso_id` and records the human decision and attribution.

Approval does not mean calculations made the decision automatically.

### Rejection

`Não aprovado` records the human decision and attribution. Do not treat warning/calculation thresholds as automatic rejection.

### Reopen

Reopen operates on the same `peso_id` and preserves approval/rejection/reopen history and actors. It does not clone or replace the Peso identity.

## 5. Comparison

Comparison remains part of a `peso_id` workflow and explicitly references a selected `previous_peso_id`.

If current readings change after a comparison table is built, the comparison is stale and must be recreated before submission.

Stale is a workflow condition, not a fourth Peso approval status.

## 6. Pegamentos

`pegamentos_id` owns one dimensional-control fact.

Pegamentos can legitimately be absent.

Where the workflow closes/certifies Pegamentos:

- the persisted control becomes the source for its official document;
- historical nominal/limits actually used remain preserved;
- later live Tool changes do not rewrite the closed historical control.

`NotEvaluable` is an evaluation condition when required historical nominal is absent. It is not permission to invent a nominal.

## 7. Folha de Controlo

`controlo_sheet_id` is a persisted record, distinct from `resumo_id`.

Its component decisions/observations are persisted facts. Do not collapse Folha lifecycle into Peso status or Resumo lifecycle.

## 8. Resumo

`resumo_id` is a persisted Controlo record for one `jobon_id` context.

Its PDF is derived from that record. The record remains authority even when the PDF is absent.

## 9. Boquilhas

`boquilhas_id` owns one repair-flow aggregate. `movement_id` owns each quantity event.

### Active

An active trace accepts permitted movements:

- Início;
- Saída;
- Entrada;
- Irreparável.

Movements are append-only facts. Editing a movement creates preserved edit/audit history; it does not create a second quantity movement.

### Close

Closing:

- keeps the same `boquilhas_id`;
- writes immutable final close snapshot/metadata;
- preserves all movements;
- moves the aggregate out of active lists into the archived/history projection.

A failed close does not produce a valid partial closed state.

### Reopen

Reopen:

- keeps the same `boquilhas_id`;
- is recorded with actor/time/reason;
- is allowed only for the last closed trace and only where the settled aggregate rules permit it;
- never rewrites prior movements or close history.

Close and reopen are aggregate lifecycle facts, not movement types.

## 10. Ferramentas / Tool edit lifecycle

`tool_id` is stable Tool identity.

For normal Ferramentas access:

```text
edit existing Tool
-> tool_change_request_id
-> pending review
-> approve OR reject
```

The Tool is not immediately mutated merely because the request was submitted.

Ferramentas Approve may perform the broader approved direct-edit/review behavior defined by the master contract.

Do not create a second Tool identity for edits or approvals.

Tool technical condition is a separate Tool-owned current fact with append-only `tool_condition_id` history. It is not the same lifecycle as a Tool change request.

## 11. Document lifecycle is separate

Document states such as:

- Ainda não gerado;
- A aguardar aprovação;
- Ficheiro em falta;

are document availability states. They do not replace the owning record lifecycle.

## 12. Cross-record invariants

- identity stays stable through review/reopen unless the operation is explicitly a new occurrence (for example Job On duplicate or Peso duplicate);
- no generic `revision_id` is introduced;
- no hidden automatic state transition replaces a required human decision;
- historical attribution is preserved;
- warnings remain warnings;
- frontend may present state but backend owns transition validity and persistence;
- lifecycle state never substitutes for server-side authorization.