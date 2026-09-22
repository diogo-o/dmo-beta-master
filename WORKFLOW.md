# DMO Beta — Controlled Development Workflow

## Purpose

This repository is the durable product and execution authority for the DMO Beta. Every Beta task must move through a controlled chain so that implementation agents do not invent product behavior, silently resolve conflicts, or treat code/tests as product authority.

## Repository roles

- `diogo-o/dmo-master` (`dmo-modular`): global DMO architecture, canonical identities, access/domain invariants and rules the Beta may not contradict.
- `diogo-o/dmo-beta-master` (`main`): Beta scope, Beta-specific behavior, workstreams, contracts, implementation order, acceptance and decisions.
- `diogo-o/DMO-MODULAR` (`main`): implementation target.
- `diogo-o/workbench`: historical Beta planning/execution material being consolidated here; not the preferred primary authority once migrated.
- `diogo-o/dmo-work`: Phase 1 implementation planning/reviews for the current modular application foundation.

## Authority rule

For Beta-specific behavior:

`dmo-master global invariants -> dmo-beta-master Beta authority -> accepted task plan -> DMO-MODULAR implementation`

Existing code is implementation state, not product authority. Tests are evidence, not product authority. Historical workbench files are source material unless explicitly promoted into a canonical Beta document here.

## Required task chain

1. Read the relevant canonical Beta documents and the global DMO authority they depend on.
2. Inspect the current remote `DMO-MODULAR/main` state.
3. Identify exact scope, dependencies, owned files and shared seams.
4. Create a task/workstream implementation plan.
5. Commit and push the plan to durable Git state.
6. Architect reviews the actual plan and returns `PLAN ACCEPT`, `CORRECTION REQUIRED`, or `REJECT`.
7. Only after `PLAN ACCEPT`, implement in `DMO-MODULAR`.
8. Add tests that prove the accepted behavior; tests may not redefine behavior.
9. Build and run targeted plus required regression tests.
10. Commit and push implementation.
11. Record an implementation response with exact changed files, behavior, test counts and deviations.
12. Architect inspects the actual commit/diff and returns `ACCEPT`, `ACCEPT WITH MINOR CORRECTION`, `CORRECTION REQUIRED`, or `REJECT`.
13. Dependent work may rely only on accepted results.

## Conflict protocol

When authority is incomplete or contradictory, the agent must not choose a product rule by convenience. Record:

```text
CONFLICT / UNDEFINED / IMPLEMENTATION BLOCKER

Where:
Expected contract or behavior:
What was found:
Why these conflict:
Why this blocks or changes the task:
Technical impact:
Minimum decision required:
```

Then stop the affected slice. Independent work may continue only where the conflict has no effect.

## Frontend/backend blocker protocol

If frontend work requires a backend fact or endpoint that is not yet authoritative, classify it as:

```text
BACKEND / INTERFACE BLOCKER

Required user interaction:
Required backend fact/capability:
Existing published contract:
Gap:
Impact on isolated frontend work:
Impact on live integration:
```

Frontend may use approved fixtures for presentation work, but must not invent production APIs, IDs, persistence semantics or authorization.

## Cross-stream change protocol

A workstream may not silently modify another stream's owned contract. Any shared-contract proposal must document:

- current contract;
- requested change;
- reason;
- affected consumers;
- backwards compatibility;
- tests/fixtures affected;
- required merge order.

The owner/integration lead must accept it before implementation.

## Git / remote discipline

GitHub remote state is the durable handoff. For every plan and implementation:

```text
fetch current remote
record baseline SHA
make owned changes only
commit
push
fetch again
verify remote main SHA
verify expected commit is reachable
```

No force-push, history rewrite, silent reset or local-only handoff.

## Testing evidence

Meaningful tests should state:

```text
Test:
Purpose:
Master/Beta behavior being verified:
Preconditions:
Action:
Assertions:
Required non-effects:
What this proves:
What this does NOT prove:
```

Always distinguish:

1. committed test source inspected in Git;
2. independent CI execution evidence, when available;
3. Developer-reported local execution evidence.

Do not claim local execution is independent CI evidence.

## Anti-invention rules

The Beta implementation must not:

- infer industrial decisions that require a human choice;
- auto-select ambiguous Tools, Job Ons, previous Peso records or production contexts;
- create alternate IDs for canonical domain entities;
- create frontend-only persistence that pretends to be canonical backend state;
- derive authorization from role labels, page visibility or frontend state;
- silently repair invalid persisted configuration;
- turn warnings into automatic approval/rejection decisions;
- expand a Light/Beta module into the future full workflow without explicit authority.
