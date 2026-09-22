# Identities and Relationships — Beta canonical contract

## Purpose

This file is the compact identity map for the Beta. It exists so frontend/backend agents do not recreate relationships from memory or display text.

## Canonical identities

```text
tool_id
= one concrete operational Tool

jobon_id
= one concrete production occurrence / Job On

cm_id / mf_id / bq_id
= one production-specific Tool context under a Job On

peso_id
= one Peso control/result fact

pegamentos_id
= one Pegamentos dimensional-control fact

controlo_sheet_id
= one persisted Folha de Controlo

resumo_id
= one persisted Resumo control record

boquilhas_id
= one BQ external-repair aggregate

movement_id
= one Boquilhas quantity movement/event
```

## Core relations

```text
jobon_id
├─ cm_id -> tool_id
├─ mf_id -> tool_id
└─ bq_id -> tool_id
```

```text
peso_id -> cm_id -> tool_id + jobon_id
```

Pending truthful Peso association may temporarily be:

```text
peso_id -> tool_id
```

until an explicit human-confirmed `cm_id` association exists.

```text
pegamentos_id -> jobon_id -> cm_id / mf_id / bq_id
```

```text
controlo_sheet_id -> jobon_id
resumo_id -> jobon_id + applicable component contexts
```

Production-linked Boquilhas:

```text
boquilhas_id -> bq_id -> tool_id + jobon_id
movement_id -> boquilhas_id
```

Standalone Boquilhas:

```text
boquilhas_id -> tool_id
movement_id -> boquilhas_id
```

## Identity rules

### Tool

`tool_id` is the exact canonical Tool identity.

A different lot is a different Tool.

Machine/line, reference, lot and type help humans search/select but are not cross-module identity after selection.

### Job On

`jobon_id` is a production occurrence, not a snapshot/revision ID.

Duplication creates a new `jobon_id` and new CM/MF/BQ context IDs.

### CM/MF/BQ contexts

The context IDs freeze the production-specific Tool relation/required historical values. They are not Tools and must retain direct relation to canonical `tool_id`.

### Peso

Normal production Peso points to `cm_id` and does not redundantly duplicate `jobon_id + tool_id` merely for navigation.

### Comparison

Comparison relation is explicit:

```text
current_peso_id -> previous_peso_id
```

No heuristic is reconstructed later.

### Boquilhas

`boquilhas_id` is a quantity/repair aggregate, not `bq_id`, not `tool_id`, and not a physical BQ piece.

No per-piece UUID is created merely because quantity is tracked.

## No reverse-ID arrays

Reverse navigation is query-based. Do not persist arrays like `tool.jobons[]`, `jobon.pesos[]`, etc. merely to make navigation easier.

## No fake identities

The Beta must not invent:

- `production_id`;
- `job_on_revision_id`;
- fake `cm_id` for independent Peso;
- fake `jobon_id` for standalone Boquilhas;
- module-specific Tool IDs;
- client-generated canonical IDs treated as persisted domain facts.

## Human selection rule

Whenever more than one valid record may fit, the human explicitly selects the exact record. Metadata narrows/searches; it does not silently determine canonical identity.
