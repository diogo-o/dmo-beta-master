# DMO Beta Master

`dmo-beta-master` is the canonical authority for the DMO Beta scope, Beta-specific functional behavior, implementation sequencing, accepted Beta contracts, and Beta acceptance criteria.

## Authority hierarchy

1. `diogo-o/dmo-master`, branch `dmo-modular` — global DMO architecture, canonical identities, invariants, lifecycle rules, access principles, and domain authority that the Beta must not contradict.
2. `diogo-o/dmo-beta-master`, branch `main` — canonical Beta scope and Beta-specific authority.
3. `diogo-o/DMO-MODULAR`, branch `main` — application implementation target.
4. `diogo-o/workbench` and `diogo-o/dmo-work` — historical planning, execution records, reviews, and source material. Once content is consolidated and accepted here, these repositories are no longer primary Beta authority for that content.

## Beta modules

Operational Beta scope is organized around:

- Job On Light
- Ferramentas Light
- Controlo Create
- Controlo Approve
- Boquilhas

Shared foundations required by the Beta include authentication/account resolution, Templates/access, navigation/USER shell, and the accepted shared frontend contracts.

## Repository purpose

This repository contains specification and governance, not application implementation. It should let a new Architect or Developer understand the complete Beta without reconstructing the product from old chats or scattered workbench files.

Source material is preserved under `sources/` and tracked in `SOURCE_MANIFEST.md`. Canonical Beta documents will be maintained separately from raw historical sources so historical plans do not silently become current product authority.

## Core rule

When historical Beta material in `workbench` or `dmo-work` conflicts with an accepted canonical document in this repository, the canonical `dmo-beta-master` document wins for Beta behavior, provided it does not contradict `dmo-master/dmo-modular` global architecture.
