# Beta Authority

## Purpose

This file defines which repository is authoritative for which class of decision.

## Global DMO authority

`diogo-o/dmo-master`, branch `dmo-modular`

Owns global architecture and invariants, including:

- canonical identities and relationships;
- global access principles;
- module vocabulary and domain boundaries;
- lifecycle rules that apply beyond Beta;
- anti-inference / explicit-human-choice principles;
- architectural test and acceptance protocol.

The Beta may simplify a workflow, but must not contradict these global contracts.

## Beta authority

`diogo-o/dmo-beta-master`, branch `main`

Owns:

- exact Beta scope;
- what is included and excluded;
- Beta-specific simplified workflows;
- Beta-specific module contracts;
- accepted shared frontend requirements for the Beta;
- workstream boundaries;
- implementation order/dependencies;
- Beta acceptance criteria;
- Beta-specific decisions and blockers.

## Implementation

`diogo-o/DMO-MODULAR`, branch `main`

Owns implementation state only. Existing code is not product authority by itself.

## Historical / coordination sources

`diogo-o/workbench`

Contains the original Beta frontend umbrella plan, Workstream A planning, A1/A2 frontend contracts/reviews, and historical execution/governance material.

`diogo-o/dmo-work`

Contains current Phase 1 requests/plans/responses/reviews for application foundations such as Admin, Templates, navigation and USER shell.

These repositories remain evidence and history. Once a Beta rule is consolidated and accepted in `dmo-beta-master`, developers should use the canonical Beta document here rather than reconstructing authority from historical artifacts.

## Conflict rule

If a canonical Beta document here conflicts with historical `workbench`/`dmo-work` material, the canonical Beta document wins.

If a Beta document conflicts with global `dmo-master/dmo-modular` architecture, the conflict must be escalated and resolved explicitly. Beta authority cannot silently override global DMO architecture.
