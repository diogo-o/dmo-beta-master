# Cross-Module Flows — Beta canonical model

## Purpose

This file explains how the Beta modules connect without transferring domain ownership between them.

## Core chain

Job On is the centre of production planning. It creates the production context and the production-specific CM/MF/BQ snapshots. From there, the required planning information flows to the operational modules.

```text
Tool
  ↓ selected in Job On
Job On / production planning
  ├─ cm_id ──→ Controlo
  │             └─ Resumo da produção
  │                  └─ Peso populated with machine/reference/lot/process/CM context
  ├─ mf_id ──→ Controlo / Reparação Interna where required
  └─ bq_id ──→ Boquilhas

Job On planning/date changes
  └─ notify relevant modules
       └─ each module pulls only the production context it needs
```

The arrows mean relation/consumption, not ownership transfer. Job On does not own module outputs. Each module consumes the relevant production context, applies its own workflow and persists only its own output.

A module must not require the operator to re-enter facts already known from the Job On context.

## Tool → Job On

Ferramentas owns the canonical Tool identity and Tool-owned facts.

Job On selects canonical Tools and creates production-specific contexts:

```text
jobon_id
├─ cm_id -> tool_id
├─ mf_id -> tool_id
└─ bq_id -> tool_id
```

Job On never creates a second Tool authority.

## Job On → Controlo Create

For a production that already has Job On context, the flow enters Controlo through the **Resumo for that production**.

```text
jobon_id
→ Resumo da produção
→ cm_id
→ Peso
```

The Resumo is the production-facing entry/context for Controlo. Peso is then populated from the CM/Job On context with the facts already known for that production, including the applicable machine, reference, lot, process and CM identity/context. The operator enters only Peso-owned measurement data.

The exact production context remains visible in the Controlo UI, but Peso does not duplicate Job On as its own production entity.

If the truthful CM is not yet associated to a Job On context:

```text
peso_id -> tool_id
status: Job On por associar
```

Later association to `cm_id` is explicit and human-confirmed.

## Controlo Create → Controlo Approve

Create submits the same persisted Peso:

```text
draft peso_id
→ submit
→ same peso_id pending/reviewable
→ approve / reject / reopen
```

Approve never creates a copied approval record.

## Job On → Pegamentos

Pegamentos consumes the Job On component contexts:

```text
pegamentos_id -> jobon_id
jobon_id -> cm_id / mf_id / bq_id
```

It does not independently reselect the same Tools just to create another copy of production context.

## Job On → Boquilhas

Production-linked Boquilhas:

```text
jobon_id
→ bq_id
→ tool_id
→ boquilhas_id
→ movement_id*
```

Standalone Boquilhas remains valid:

```text
tool_id
→ boquilhas_id
→ movement_id*
```

No fake Job On is created for the standalone case.

## Job On read projections

Job On may show related state from other modules for operator awareness.

Examples:

- Controlo available/pending/approved;
- Boquilhas activity/state;
- document availability.

These are read projections only. Job On does not copy or mutate foreign module facts.

## Tool selection and downstream context

Canonical Tool selection/creation for a production occurs in Job On. Downstream modules consume the resulting production context; they do not independently reselect the same Tool just to reconstruct production identity.

```text
Job On
→ select/create canonical tool_id
→ create cm_id / mf_id / bq_id snapshot
→ downstream module receives the relevant snapshot/context
```

Contextual Tool history/details may still be queried when the user explicitly asks for them, but those reads are task-specific and do not replace the production context.

## Shared destination vs permission

Two access modules may share one visible destination:

```text
Job On View + Job On Create -> job-on
Controlo Create + Controlo Approve -> controlo
```

The visible collapse never merges backend permissions.

## Context notifications and light reads

A Job On create/change can notify only the modules affected by that production context. The notification is a small signal that identifies the changed production/context; it is not a dump of all domain data.

The receiving module then performs a targeted read for the information required by its workflow.

```text
Job On changed
→ small ping/context identifier
→ module-specific targeted query
→ populate only required fields
→ module workflow/output
```

Do not scan all Tools, Job Ons or module records waiting for a condition to appear. Queries should be scoped by the context already in hand.

This keeps packets/read models small and makes each workflow easier to test and reason about.

## Backend/frontend crossing

Backend owns:

- IDs;
- persistence;
- relation truth;
- authorization;
- lifecycle transitions;
- calculations;
- audit facts.

Frontend owns:

- rendering;
- explicit input;
- orchestration;
- local unsaved state;
- warnings/choices;
- mapping backend facts to views.

Frontend must not reconstruct missing backend relations from display text.

## Anti-inference rules

Across all Beta flows:

- no automatic Tool selection from visible metadata;
- no automatic previous Job On selection;
- no automatic previous Peso selection;
- no automatic pending Peso -> Job On association;
- no automatic industrial approval/rejection from warnings;
- no inferred Job On for standalone Boquilhas;
- no hidden fallback that rewrites persisted identity.

## Document relation principle

Documents are outputs/readable states of owning records. A filesystem path is never production identity.

Where a related document is absent, unavailable or not applicable, the UI shows the corresponding availability state; it does not fabricate a file or treat lookup failure as the same thing as legitimate absence.
