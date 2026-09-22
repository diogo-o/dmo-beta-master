# Application Foundation for the Beta

## Purpose

This document explains how the Beta sits on the real `DMO-MODULAR` application architecture.

It is intentionally implementation-aware without making current code the source of product truth.

## Application shape

`DMO-MODULAR` is a modular monolith.

The runtime is one application. Operational areas remain separated by workflow/domain ownership.

Conceptually:

```text
Browser / Razor UI
        ↓
DMO.Web
        ↓
DMO.Application
        ↓
Domain/shared identities + module contracts
        ↓
Persistence / Files / PDF / Provider infrastructure
```

The runtime/application shell should remain deliberately small. It establishes session/account/access/navigation and shared infrastructure. It should not contain every industrial rule.

## Shared identities

Cross-module relationships use canonical IDs and explicit contexts, especially:

- `tool_id`;
- `jobon_id`;
- `cm_id`;
- `mf_id`;
- `bq_id`;
- `peso_id`;
- `pegamentos_id`;
- `controlo_sheet_id`;
- `resumo_id`;
- `boquilhas_id`;
- `movement_id`.

A feature does not create a private replacement identity because another feature is not implemented yet.

## Module boundary rule

A module owns its workflow and persistence.

Preferred interaction:

```text
Feature A
→ published Application/shared contract
→ owning backend logic
```

Wrong direction:

```text
Feature A service
→ arbitrary tables belonging to Feature B
→ reconstruct B's domain state itself
```

Shared identities permit traversal, but they do not transfer domain ownership.

## Access foundation

The application already has the accepted P1-T04 Module Registry and server-side Module authorization foundation.

Operational feature work therefore consumes:

- canonical Module definitions;
- current-build availability;
- effective Template resolution;
- server-side Module policies.

It must not introduce:

- a parallel permission system;
- role/profile-based grants;
- a per-feature access database;
- navigation-as-security.

## USER and ADMIN foundation

The application already distinguishes dedicated ADMIN from USER.

ADMIN is not an operational USER super-role.

Operational USER access comes through the USER's Template and canonical Module composition.

Administration has its own dedicated policy.

## Template foundation

A USER has one nullable Template relationship.

The Template controls:

- Module composition;
- presentation order;
- optional landing destination.

Feature work does not create independent per-user Module overrides.

## Navigation foundation

The accepted frontend architecture has one navigation projection seam owned by Workstream A.

Operational feature work should publish/register its legitimate Module/route surface into that architecture when authorized.

A feature must not build a second navigation composer.

A Module appearing in a Template is not sufficient for navigation. A normal top-level destination requires the full runtime conditions:

```text
granted
+ available in current build
+ non-contextual
+ real route registered
```

Ferramentas is contextual-only and therefore never becomes a normal top-level destination.

## Backend/frontend responsibility

### Backend/Application owns

- canonical identity creation and validation;
- persistence;
- ownership of relationships;
- authorization;
- domain state transitions;
- audit facts;
- concurrency;
- calculations owned by the domain;
- document record/generation state;
- read contracts between modules.

### Frontend owns

- rendering supplied state;
- input collection;
- explicit user choice;
- local UX validation where it does not redefine domain rules;
- preservation of unsaved state during approved subflows;
- mapping published backend/application results into view models;
- shared presentation contracts.

Frontend must never compensate for a missing backend fact by inventing a persistent domain fact.

## Feature integration pattern

A Beta feature should normally integrate through:

```text
Razor Page / feature UI
→ feature Application service / published contract
→ owning persistence/domain logic
→ returned read/result model
→ shared frontend presentation
```

When another module is needed:

```text
Feature C
→ B-owned Tool/Job On orchestration contract
→ canonical selected context
→ C continues its own workflow
```

not:

```text
Feature C
→ reads B's private tables
→ guesses the context
```

## Database and migrations

Schema/migration work is backend authority and requires its own accepted task when not already covered by the current feature plan.

Frontend workstreams do not invent tables merely to unblock a screen.

When a real live contract is missing, classify the blocked slice as a backend/interface blocker and continue only with independent presentation work where safe.

## Documents

Persisted operational record, generated PDF and file path are separate concepts:

```text
database record
!= generated document
!= filesystem path
```

The Beta must preserve this separation.

The existing DMO planning already uses the final-style document directory convention by reference and production. That convention is presentation/storage organization; it is never a replacement identity for Job On, Peso, Pegamentos or Resumo.

## Rule for future implementation prompts

Every B–E implementation plan must inspect the current `DMO-MODULAR/main` before proposing file paths or integration seams.

This document describes the architectural shape, not a guarantee that today's class/file names remain unchanged forever.
