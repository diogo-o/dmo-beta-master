# Job On Light — Beta canonical contract

## Purpose

Job On Light is the Beta production-context surface for one concrete production occurrence. It uses the final canonical identities while exposing only the simplified Beta workflow.

## Authority

- Beta scope/workstream: `workbench/BETA_FRONTEND_IMPLEMENTATION_WORKSTREAMS.md` Workstream B.
- Global domain authority: `dmo-master/dmo-modular/modules/JOB_ON.md`.
- Access authority: `dmo-master/dmo-modular/global/ACCESS_MODEL.md`.

Where this Beta file narrows scope, it does not redefine canonical identity or global ownership.

## Included in Beta

- Create a Job On with the simplified Beta fields.
- View and edit the simplified Beta fields.
- Search by reference and inspect productions for that reference.
- Explicitly select and open a production/Job On.
- Explicitly duplicate a selected existing Job On as the starting point for a new production.
- Optional CM/MF/BQ Tool association using canonical `tool_id` values.
- Read projections of related Controlo, Boquilhas and document availability where backend contracts exist.
- Tool selection/create orchestration through the shared Tool flow.

## Explicitly outside Beta

- full manual family sheet blocks;
- verification catalogue/occurrence management;
- complete Job On production lifecycle;
- revision workflow;
- full future print/document orchestration;
- full Ferramentas lifecycle embedded inside Job On;
- Controlo calculations;
- Boquilhas movement ownership.

## Canonical identity

```text
jobon_id
= one concrete production occurrence
```

The Beta does not introduce `production_id` or `job_on_revision_id`.

Production-specific Tool contexts remain:

```text
jobon_id
├─ cm_id? -> tool_id
├─ mf_id? -> tool_id
└─ bq_id? -> tool_id
```

`cm_id`, `mf_id`, and `bq_id` are production-specific historical contexts. They are not Tool identities. `tool_id` remains the canonical Tool identity.

## Simplified Beta fields

The Beta Create/Edit surface captures only:

- Referência;
- Número de produção;
- Máquina/Linha;
- Processo as a displayed/consumed Tool fact where available, not a second Job On authority;
- optional CM;
- optional MF;
- optional BQ.

The exact storage/DTO shape belongs to the authorized backend implementation. The frontend must not invent additional domain fields because they appear in the full future Job On.

## Tool selection

The operator explicitly searches and selects the exact Tool.

```text
search visible metadata
→ show candidates
→ human selects exact candidate
→ persist canonical tool_id in the required Job On context
```

Rules:

- no auto-selection among ambiguous candidates;
- reference/lot/machine text never becomes the persisted cross-module identity after selection;
- missing Tool may open the shared Tool-create flow;
- after Tool creation, the originating Job On state is restored and the returned canonical `tool_id` is associated;
- Job On does not create a private Tool registry.

## Duplicate workflow

Duplication is assisted creation, not revision.

```text
selected source jobon_id
→ preview/review source
→ create NEW jobon_id
→ create NEW cm_id / mf_id / bq_id where copied
→ retain source tool_id values until explicitly changed
```

The source Job On remains unchanged.

The operator may select any suitable historical source; the system must not force the latest or chronological previous Job On.

## Read relations to other Beta modules

Job On may display related status/availability, but does not own foreign module data.

```text
jobon_id
├─ Controlo read projection
├─ Boquilhas read projection
└─ document availability projection
```

No frontend copy of Controlo or Boquilhas state becomes a second authority.

## Access

Two distinct assignable modules remain:

- `Job On View`
- `Job On Create`

They may share one visible `job-on` destination while preserving distinct permissions/actions.

Navigation visibility is not authorization. Backend route/action gates remain authoritative.

## Frontend responsibility

Frontend may:

- render the compact Beta Job On sheet;
- orchestrate explicit Tool search/select/create;
- preserve unsaved state across authorized subflows;
- render related status/document projections;
- provide duplication preview and explicit source choice.

Frontend must not:

- infer Tool identity;
- infer compatibility beyond backend-approved candidate facts;
- create IDs client-side as canonical domain facts;
- duplicate foreign-module state;
- turn visible navigation into authorization.

## Backend contracts required

Live Beta completion requires published contracts for:

- Job On create/read/edit;
- reference → Job On/production query;
- Tool candidate search/create;
- CM/MF/BQ context association;
- duplication using an explicitly selected source;
- related-status/document availability reads.

Missing live contracts are `BACKEND / INTERFACE BLOCKER`, not permission to invent production APIs.

## Acceptance criteria

- Create captures only the Beta fields above.
- Edit exposes only the simplified Beta fields and permitted Tool associations.
- Reference search returns the available productions/Job Ons for explicit selection.
- Duplicate requires explicit source selection and preview.
- Duplicate may use an older/non-latest source.
- Duplicate produces a new `jobon_id` and new context IDs.
- Retained Tool choices preserve the same canonical `tool_id` values until changed by the human.
- Missing Tool creation returns to the originating workflow without losing state.
- No internal canonical ID is used as the primary human-facing label.
- Related Controlo/Boquilhas/document state is read, not copied.

## Required evidence

- Create/Edit/View round-trip tests.
- Explicit Tool selection and ambiguity tests.
- Inline Tool-create/cancel/return-state tests.
- Reference History search/open tests.
- Duplication from a non-latest source.
- New context IDs with retained canonical Tool IDs.
- View vs Create access tests.
- Related-state/document availability rendering tests.
