# Access and Navigation — Beta canonical model

## Purpose

This file defines how Beta access, navigation and runtime routing fit together without turning frontend visibility into authorization.

## Access vocabulary

The Beta consumes the final assignable-module vocabulary from `dmo-master/dmo-modular/global/ACCESS_MODEL.md`.

Relevant Beta operational modules include:

- Job On View;
- Job On Create;
- Controlo Create;
- Controlo Approve;
- Boquilhas;
- Ferramentas;
- Ferramentas Approve.

ADMIN is a distinct account classification and is not an assignable operational Module.

## Template-driven access

```text
authenticated USER
→ active application USER
→ assigned Template
→ selected Module IDs
→ Module Registry
→ Access Resolver
→ effective Modules/actions
→ navigation projection
```

Navigation is a projection of effective access. It never grants access by itself.

## Shared visible destinations

Distinct Module identities may share one visible destination:

```text
Job On View
Job On Create
→ job-on

Controlo Create
Controlo Approve
→ controlo
```

The UI may collapse these into one visible destination while backend permissions remain separate.

## Contextual-only Ferramentas

Ferramentas and Ferramentas Approve have zero top-level navigation destinations.

Their canonical Tool ficha is opened contextually from records that reference a real `tool_id` and only where access allows it.

No `/Ferramentas` top-level Beta destination is invented for symmetry.

## Runtime destination validity

A destination is visible/usable only when all required presentation facts are true:

```text
Module granted
AND Module available in current build
AND non-contextual destination
AND real route registered
```

A fake route or provisional production fixture must never make a Module appear live.

## Landing behavior

Runtime landing follows the accepted P1-T07 contract:

```text
explicit persisted landing != null
  AND landing exists among valid composed/routed destinations
→ use explicit landing

explicit persisted landing == null
  AND at least one valid destination exists
→ use first valid destination in Template presentation order

no valid destination
→ no-access

explicit persisted landing != null
  BUT no longer valid/routable
→ fail closed → no-access
```

The system does not silently rewrite/clear persisted landing configuration.

## Root behavior

```text
GET /

no resolved session/account
→ /Login

ADMIN
→ /Administration

active USER with valid landing
→ resolved operational destination

active USER with no usable destination
→ /AccessDenied
```

## Direct-route enforcement

Hiding a navigation item is never sufficient authorization.

Examples:

- no Job On Create Module → direct create route/action remains denied even if Job On View makes the shared destination visible;
- Controlo Create does not grant Controlo Approve actions;
- a visible shared destination never merges underlying Module grants;
- ADMIN does not automatically receive USER operational Module routes.

Backend authorization policies remain the enforcement boundary.

## Current build availability

A Module may become production-visible only when:

1. its real backend/application capability exists;
2. its real frontend route/surface exists;
3. the Module is registered available in the canonical Module Registry;
4. the destination route is registered;
5. authorization is enforced server-side.

Do not mark a Module available merely because a screen mockup exists.

## Frontend responsibility

Frontend may:

- render navigation projection;
- collapse shared visible destinations;
- render no-access/permission-denied states;
- show controls according to published action availability.

Frontend must not:

- define a second access model;
- infer permission from role labels;
- infer permission from Template name;
- bypass server-side route/action enforcement;
- create fixed-profile enums as authorization truth.
