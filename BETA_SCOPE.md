# DMO Beta Scope

## Shared foundations

The Beta depends on the accepted application foundations already being built in `DMO-MODULAR`:

- authentication/account resolution;
- dedicated ADMIN versus USER boundary;
- Template-based Module access;
- Module Registry and fail-closed access resolution;
- Template administration;
- navigation + USER shell;
- accepted shared frontend shell/contracts.

These foundations are dependencies of the Beta, not separate operational Beta modules.

## Operational Beta modules

### Job On Light

Simplified production-context workflow using canonical `jobon_id` and optional CM/MF/BQ contexts. It must remain a light Beta workflow and must not expand into the complete future Job On lifecycle.

### Ferramentas Light

Canonical Tool search/select/create and Tool-context orchestration required by Job On, Controlo and Boquilhas. Ferramentas remains contextual where defined by the global access model; no invented top-level route is implied.

### Controlo Create

Creation-side Controlo workflow, including the Beta-authorized Peso, Comparação, Pegamentos, Folha and Resumo surfaces/contracts as defined by accepted Beta source material.

### Controlo Approve

Separate approval/review capability over the same submitted records. Create and Approve remain distinct assignable access Modules even when they share a visible destination.

### Boquilhas

Beta Boquilhas aggregate/movement/history workflow, including production-linked and standalone usage where authorized by the accepted Beta contract.

## Explicitly not implied by this scope

This repository must not infer that the Beta includes the complete future implementation of:

- full Job On lifecycle;
- full Ferramentas lifecycle/change-request/approval dossier;
- Armazém;
- Reparação Interna/Externa/Programada unless separately added to Beta authority;
- Tampões;
- future modules not explicitly listed here;
- backend behavior that has not been accepted in a canonical Beta contract.

## Implementation model

Each operational workstream requires its own plan gate before implementation. Missing backend/interface contracts are explicit blockers for live integration; they are not permission to invent temporary production APIs, persistence or identities.
