# Beta Source Archive

This directory preserves provenance for source material used to build the canonical `dmo-beta-master` documentation.

## Rule

Files under `sources/` are evidence/history snapshots.

They do **not** automatically override the canonical Beta documents in the repository root, `architecture/`, `contracts/`, `modules/`, or `implementation/`.

Upstream global architecture authority remains `diogo-o/dmo-master`, branch `dmo-modular`.

## Expected layout

```text
sources/
├─ dmo-master/
├─ workbench/
├─ dmo-work/
└─ DMO-MODULAR/
```

Each imported source should preserve or record:

- source repository;
- source branch/ref;
- original path;
- blob/commit SHA where available;
- classification: architecture authority, accepted review, implementation evidence, historical plan, or rejected/superseded material.

## Classification rule

- `ARCHITECTURE AUTHORITY` — upstream contract from `dmo-master/dmo-modular`.
- `ACCEPTED PLAN/REVIEW` — durable accepted planning/Architect result.
- `IMPLEMENTATION EVIDENCE` — code/Developer response proving current implementation state, not product authority.
- `HISTORICAL` — useful context but superseded or non-current.
- `MISSING HISTORICAL REFERENCE` — referenced by accepted material but not recovered yet.

## Canonicalization rule

When source files disagree:

1. global `dmo-master/dmo-modular` invariants cannot be overridden by Beta convenience;
2. explicit Owner/Architect accepted decisions beat older planning text;
3. accepted corrections beat the implementation/review they corrected;
4. current code describes implementation state, not intended product authority;
5. `dmo-beta-master` canonical docs explain the reconciled Beta contract and must record unresolved conflicts instead of silently choosing.
