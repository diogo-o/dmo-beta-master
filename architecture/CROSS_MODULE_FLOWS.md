# Cross-Module Flows — Beta canonical model

## Purpose

This file explains how the Beta modules connect without transferring domain ownership between them.

## Core chain

```text
Tool
  ↓
Job On
  ├─ CM context ──→ Peso ──→ Controlo Approve
  ├─ MF context ──→ Pegamentos
  └─ BQ context ──→ Boquilhas

Job On
  ├─ reads Controlo status/availability
  ├─ reads Boquilhas status/availability
  └─ reads document availability
```

The arrows mean relation/consumption, not ownership transfer.

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

Normal Peso production flow:

```text
jobon_id
→ cm_id
→ tool_id
→ peso_id
```

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

## Shared Tool flow

Job On, Controlo and Boquilhas use one shared Tool search/select/create orchestration.

```text
origin module
→ search Tool
→ explicit selection OR create
→ canonical tool_id returned
→ origin state restored
→ owning module persists its own relation
```

No module creates its own Tool picker identity model or private registry.

## Shared destination vs permission

Two access modules may share one visible destination:

```text
Job On View + Job On Create -> job-on
Controlo Create + Controlo Approve -> controlo
```

The visible collapse never merges backend permissions.

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
