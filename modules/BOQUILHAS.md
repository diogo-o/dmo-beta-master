# Boquilhas — Beta canonical contract

## Purpose

Boquilhas is the Beta BQ external-repair quantity workflow. It records manual quantity movements, derived repair balance, audit/history and close/reopen state without becoming a Tool registry, Job On planner or Armazém stock authority.

## Authority

- Beta scope/workstream: `workbench/BETA_FRONTEND_IMPLEMENTATION_WORKSTREAMS.md` Workstream E.
- Global domain authority: `dmo-master/dmo-modular/modules/BOQUILHAS.md`.

## Included in Beta

- search/select/create the BQ Tool context;
- production-linked and standalone Boquilhas flows;
- active aggregate summary;
- four write movement types only;
- movement forms and validation;
- recent movements;
- full History filters/table;
- Edit with preserved before/after audit;
- close/reopen on the same `boquilhas_id`;
- repairer selection from the Boquilhas-owned operational repairer directory;
- Boquilhas Definições for repairer management and machine/line -> repairer assignment;
- production-line contextual panel where supported;
- access and responsive states.

## Explicitly outside Beta

- mandatory official Boquilhas PDF;
- Admin ownership of day-to-day repairer configuration;
- Controlo ownership of repairer configuration;
- Job On planning ownership;
- Armazém physical stock/location ownership;
- per-piece BQ identity;
- obsolete movement types from legacy UI.

## Canonical identities

```text
tool_id
= canonical registered BQ Tool

bq_id
= historical BQ Tool context in one Job On

boquilhas_id
= one collective BQ external-repair aggregate

movement_id
= one quantity movement/event
```

`bq_id` is not `boquilhas_id`. Neither replaces `tool_id`.

## Production-linked flow

```text
boquilhas_id -> bq_id
bq_id -> jobon_id + tool_id
movement_id -> boquilhas_id
```

Do not also duplicate `jobon_id + tool_id` merely for navigation when they are already reachable through `bq_id`.

## Standalone flow

A legitimate Boquilhas flow may exist without Job On:

```text
boquilhas_id -> tool_id
movement_id -> boquilhas_id
```

No fake Job On or fake `bq_id` is created.

## Search/select/create

The operator finds the canonical BQ Tool using visible metadata such as reference, lot and machine/line context.

```text
exists -> explicit select
missing -> create canonical Tool -> return tool_id -> continue
```

Creation does not transfer Tool-master ownership to Boquilhas. Ferramentas remains authoritative for the canonical Tool.

## Opening facts

Where used by the Beta flow:

- reference/BQ + lot;
- machine(s)/line(s), at least one where the opening flow requires it;
- initial repair-flow quantity (`Início`);
- initial utilisation as a manual still where applicable;
- opening business date;
- compact observations.

This is repair-flow context, not Armazém stock truth.

## Movement vocabulary

Exactly four write movement types:

```text
Início
Saída
Entrada
Irreparável
```

`Editar` is an action on an existing movement, not a movement type.

Each movement is an append-only operational fact:

```text
movement_id
├─ movement_type
├─ quantity
├─ business_date
├─ recorded_at
├─ actor
├─ repairer_id?   # required on external Saída
└─ observations/context
```

## Business date vs audit timestamp

- `business_date` = when the physical/operational movement happened; operator-editable where allowed;
- `recorded_at` = immutable system receipt timestamp;
- default `business_date` = today;
- changing `business_date` never rewrites `recorded_at`.

Operational calendar/filtering uses `business_date`; audit/security can use `recorded_at`.

## Edit/audit

Editing an existing movement preserves before/after values, authenticated user and system timestamp.

The edit record is audit history, not another quantity event. Editing must not change balance twice.

Any annulment/removal supported by the backend is a registered, confirmed fact — never silent physical deletion.

## Balance

Movement facts are the authority. Do not store a second independently mutable balance when replay can derive it.

Derived buckets:

- Disponível;
- Em reparação;
- Irreparável;
- Entrada excecional.

Rules preserved by the global contract:

- Saída cannot exceed the available repair-flow production quantity;
- Irreparável cannot exceed quantity currently in repair;
- excess Entrada is recorded, not silently clamped/rejected;
- zero balance does not delete the aggregate;
- negative saldo remains visible and is not automatically a blocking error.

## Excess Entrada

An Entrada stores the real returned quantity. Where it exceeds the expected amount, the excess remains visible as a movement fact/projection.

The original Entrada must not be rewritten to hide the discrepancy.

Any later discrepancy-resolution workflow is a new recorded fact with its own note/attribution; the note is not required merely to save the Entrada.

## Repairers and line assignments

Boquilhas owns the operational repairer configuration used by its own workflow.

```text
Boquilhas / Definições
├─ repairer directory
└─ machine/line -> repairer assignment
```

Machine/line assignments are operational settings, not Admin configuration and not Controlo configuration. They are changed in Boquilhas because Boquilhas is the module that works with repairers.

Assignments are independent per machine/line. Changing one assignment must not silently cascade to another machine/line.

Every external Saída stores the final selected canonical `repairer_id`.

A configured machine/line assignment may provide the suggested/default repairer for that operational context, but the persisted movement retains the final repairer relation used for that movement.

Historical movements retain their repairer relation even if the repairer directory or line assignment changes later.

## Close/reopen

Close/reopen acts on the same `boquilhas_id`.

```text
active
→ close + immutable close snapshot
→ archived projection
→ optional reopen under allowed conditions
```

Rules:

- movements remain linked to the same aggregate;
- close never creates a replacement aggregate;
- failed close leaves active state unchanged;
- reopening is recorded with actor/time/reason;
- history remains replayable.

The exact allowed-reopen eligibility is enforced by the backend/domain contract.

## Job On boundary

Selecting BQ in Job On does not itself create a Boquilhas movement.

Boquilhas does not choose the production BQ.

The contextual line panel may read/navigate production context, but mutation of Job On planning remains Job On Create behavior with its own backend gate.

## History

History is a read/query projection over the same movement/aggregate facts.

Useful filters may include:

- reference;
- lot;
- line;
- business date/period;
- movement type;
- repairer;
- aggregate/file state;
- pagination.

Single click may select; double click may open the aggregate. These are UI behaviors, not domain authority.

## Access

Boquilhas is one assignable module.

Navigation availability is projection only. Backend route/action enforcement remains required.

## Frontend responsibility

Frontend may:

- render aggregate/balance projections returned or derivable from approved facts;
- collect explicit movement input;
- show warnings and discrepancy states;
- render History/audit;
- orchestrate Tool/Job On context selection.

Frontend must not:

- invent movement persistence semantics;
- store a second authoritative balance;
- infer Job On planning;
- create per-piece BQ identities;
- auto-correct excess/negative states;
- synthesize audit actor/time.

## Backend contracts required

- aggregate create/read;
- production-linked and standalone association;
- movement append;
- movement edit + audit;
- balance/read projection;
- repairer directory query/manage;
- machine/line -> repairer assignment read/update;
- close/reopen;
- History query;
- Tool/Job On context reads.

## Acceptance criteria

- movement selector exposes only Início, Saída, Entrada and Irreparável;
- Edit is not a movement type;
- edit preserves before/after audit without double balance effect;
- both production-linked and standalone flows work without fake identities;
- excess Entrada is recorded/displayed;
- negative saldo is visible and non-blocking by itself;
- Saída stores canonical repairer choice;
- Boquilhas Definições owns repairer directory and machine/line assignments;
- changing a line assignment does not rewrite historical movements;
- business date and recorded timestamp remain distinct;
- close/reopen retains the same `boquilhas_id` and full history;
- no mandatory PDF or internal settings tab appears.

## Required evidence

- four-movement vocabulary test;
- production-linked and standalone creation tests;
- Tool-create return-state test;
- movement validation tests;
- excess Entrada and negative-saldo tests;
- edit/audit one-movement test;
- business-date vs recorded-at tests;
- close/reopen history tests;
- History filter/select/open tests;
- access and responsive rendered tests.
