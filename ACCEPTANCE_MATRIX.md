# Beta Acceptance Matrix

## Purpose

This is the consolidated acceptance checklist for the DMO Beta.

It does not authorize implementation by itself. Each workstream still requires its own request/plan/Architect `PLAN ACCEPT` before implementation.

A workstream is not complete because its UI renders. Acceptance requires the backend/frontend contract, authorization, persistence, historical behavior and tests to agree with the Beta master and upstream `dmo-master` authority.

## 1. Foundation gate

| Requirement | Required state |
|---|---|
| P1-T04 Module Registry + Access Resolver | ACCEPTED |
| P1-T05 USER Administration | ACCEPTED |
| P1-T06 Template Administration | ACCEPTED |
| P1-T07 Navigation + USER Shell | Must receive Architect implementation ACCEPT before Beta feature streams may depend on its implementation as accepted foundation |
| A1 shared frontend contract | Accepted/frozen and preserved |
| A2 shared shell/navigation correction | Accepted and preserved |
| Production fake routes/fixtures | None |
| Authorization | Server-side Module/action gates; navigation never substitutes for authorization |
| Canonical identities | No Beta-only temporary replacement IDs |

## 2. Global Beta invariants

Every workstream must prove:

- no product rule invented to make implementation easier;
- no identity inferred from display text;
- explicit human selection where the master requires selection;
- warnings do not become automatic decisions;
- no cross-module duplication of authority;
- frontend does not persist client-only fake domain records;
- backend validates and persists transitions;
- current Tool facts and historical snapshots/stills remain distinct;
- all direct routes are protected server-side;
- hidden controls are presentation only, never enforcement;
- no role/profile label grants operational access;
- no Template name grants operational access;
- no ADMIN implicit operational super-user behavior unless explicitly authorized by an accepted contract;
- migration/schema changes are explicit, reviewed and minimal;
- tests distinguish source coverage from actual execution evidence.

## 3. Workstream A — shared frontend

### Required behavior

- shared shell remains generic and domain-free;
- `ProductionContextStrip`, `ToolPicker`, `ToolSummaryRow`, `DenseDataTable`, `RecordStatus`, `AvailabilityState`, `AuditTrail`, `MeasurementRows`, `DecisionBar` preserve the accepted contract;
- common states distinguish loading, ready, empty, lookup failure, unavailable, permission denied, saving/submitting, stale/conflict where applicable;
- disabled actions expose reasons;
- statuses/warnings are not color-only;
- keyboard/focus behavior works;
- `ToolPicker` never auto-selects, including when exactly one result exists;
- no shared component imports module-specific business rules.

### Evidence

- component-state tests;
- keyboard/focus tests;
- accessibility checks;
- responsive rendered checks;
- contract tests against representative B–E consumers.

## 4. Workstream B — Job On Light + Ferramentas Light

### Job On Light acceptance

- new Job On creates a new `jobon_id`;
- duplicate creates a new `jobon_id` and new `cm_id` / `mf_id` / `bq_id` contexts;
- duplicate never mutates/reuses source identity;
- canonical `tool_id` is selected explicitly for Tool contexts;
- ambiguous candidate is never guessed;
- same visible reference/lot text never substitutes for identity;
- Job On historical Tool-context values freeze as required;
- normal edit does not rewrite frozen context from live Tool changes;
- no invented Job On-wide lifecycle state machine;
- date-threshold edit warning remains a warning/confirmation, not hard immutability;
- hard delete is forbidden when dependent operational facts exist;
- Job On View/Create action separation is preserved;
- direct route/action access is server-side gated.

### Ferramentas Light acceptance

- `tool_id` stays canonical Tool identity;
- different lot = different Tool identity according to the settled master contract;
- Ferramentas remains contextual-only and creates no top-level navigation destination;
- canonical Tool ficha is reused rather than feature-specific duplicate editors;
- origin module does not become Tool-data owner;
- normal Ferramentas edit follows change-request behavior where required;
- Ferramentas Approve retains its broader accepted actions;
- ToolPicker remains explicit selection;
- no per-piece identity engine is invented from quantity/notes.

### B integration seams

- exposes/consumes stable Job On production context for C/D/E;
- B does not own Controlo or Boquilhas records;
- real feature routes are registered only when real surfaces and server gates exist.

## 5. Workstream C — Controlo Create

### Peso

- same `peso_id` is the owning record through create/submit/review lifecycle;
- production Peso uses `peso_id -> cm_id`;
- valid pending association uses direct `tool_id` and visible `Job On por associar`;
- no fake Job On or fake `cm_id`;
- later association is explicit human selection;
- temporary direct Tool anchor is cleared when the correct `cm_id` becomes authoritative;
- process comes through canonical Tool relation; no second Job On/Peso process authority;
- water correction/density and glass-weight formulas follow the master contract;
- individual results remain first-class and are not hidden by an average;
- rows are variable with at least one valid measurement row;
- decimals are presentation-normalized without reducing calculation precision;
- warnings remain warnings.

### Comparison

- current Peso references an explicitly selected `previous_peso_id`;
- previous Peso is never auto-selected;
- same-machine equality is not invented as a restriction when Tool compatibility permits another registered line;
- current per-row value comes from the saved current Peso result;
- stale comparison after current-reading changes must be rebuilt before submission;
- Comparison remains a Peso record type, not a status.

### Pegamentos / Folha / Resumo

- `pegamentos_id`, `controlo_sheet_id` and `resumo_id` remain distinct;
- Pegamentos reads canonical Tool nominal where available and snapshots the nominal/limits actually used;
- missing nominal becomes NotEvaluable, never invented data;
- missing required Job On Tool context blocks with actionable correction message rather than silent fallback;
- absent Pegamentos is legitimate;
- Folha preserves the established component decisions/observations;
- Resumo is persisted and not a projection of Folha.

### Submit

- submit persists a reviewable Peso without creating a second approval identity;
- submitted state/attribution is backend truth;
- C does not approve its own record merely because calculations pass.

## 6. Workstream D — Controlo Approve

- reads the same submitted `peso_id` created by C;
- no approval-copy Peso is created;
- Controlo Approve authorization is independently enforced from Controlo Create;
- a USER may legitimately have Create, Approve or both according to Template composition;
- approve/reject are explicit human actions;
- warnings/calculations never auto-approve or auto-reject;
- actor/time attribution is persisted;
- reopen preserves the same `peso_id` and historical decision trail;
- direct route/action enforcement is server-side;
- shared destination/navigation does not merge Create and Approve permissions.

## 7. Workstream E — Boquilhas

### Identity and context

- `tool_id`, `bq_id`, `boquilhas_id` and `movement_id` remain distinct;
- production-linked aggregate uses `boquilhas_id -> bq_id -> jobon_id + tool_id`;
- standalone aggregate may use direct `tool_id`;
- no fake Job On/BQ context is created for standalone flow.

### Movements

Only active quantity movement types:

- Início;
- Saída;
- Entrada;
- Irreparável.

`Editar` is an action, not a movement type.

Each movement preserves business date, recorded timestamp, actor and required repairer/context facts.

Editing preserves before/after audit history and does not create a duplicate quantity event.

### Balance

- movement facts are the source of repair-flow balance;
- no second mutable balance authority;
- Saída and Irreparável validation follows the master rules;
- excess Entrada is recorded, not silently clamped/rejected;
- negative/exceptional visible projections do not become automatic decisions;
- `% utilização` remains manual and is not calculated from movements.

### Close/reopen

- close retains same `boquilhas_id`;
- close preserves movements and immutable close metadata/snapshot;
- failed close does not present a valid partial closed state;
- reopen keeps same identity and records who/when/why;
- close/reopen are not movement types.

### Repairer

- external Saída stores canonical `repairer_id`;
- historical movement is not rewritten when directory/default settings change;
- per-line suggestion is a suggestion, not an automatic identity/decision.

## 8. Documents/PDF gate

- structured record remains truth;
- path/filename never acts as join key;
- Peso/Pegamentos/Resumo filenames follow the settled pattern;
- folder is `<reference>/<production-number>/`;
- historical output uses preserved historical context;
- missing optional record, missing file and failed lookup are distinct states;
- official frozen output is not silently regenerated from current mutable facts;
- no local filesystem paths are printed into PDFs;
- no document table/identity is introduced merely for symmetry.

## 9. Access/navigation gate

For every real Beta surface:

- Module is known canonically;
- Module is marked current-build available only when the real feature ships;
- route is registered in the shared route seam;
- top-level navigation appears only for granted + available + non-contextual + routed destinations;
- Ferramentas/Ferramentas Approve never become top-level destinations;
- direct route remains protected even if navigation link is absent;
- shared visible destinations do not merge canonical Module/action grants;
- invalid access resolution fails closed.

## 10. Persistence / schema gate

Before acceptance of any schema-bearing feature:

- migration has one justified owner/workstream;
- no table is created for UI symmetry;
- no reverse-ID arrays;
- no duplicated cross-module truth;
- unique/FK/concurrency semantics match the accepted contracts;
- migrations are exercised against disposable PostgreSQL where relevant;
- live Supabase changes are never implied by local tests.

## 11. Test evidence gate

Every implementation response must separate:

1. committed test source inspected in Git;
2. Developer-reported local execution results;
3. independently observable GitHub CI/status evidence, if any.

Required test categories where applicable:

- unit domain/application rules;
- server-side authorization/direct-route negatives;
- persistence integration against disposable PostgreSQL;
- concurrency/invalid-write no-effect cases;
- frontend component states;
- accessibility/keyboard/focus;
- cross-module seam tests;
- document availability and historical render behavior;
- no silent auto-selection/auto-decision regressions.

## 12. Final Beta acceptance

The Beta is integrated only when:

- each participating workstream has its own Architect implementation ACCEPT;
- no unresolved required backend/interface blocker prevents a promised Beta action;
- operational Modules/routes are honestly registered as available only when real;
- Create/Approve separation is enforced;
- cross-module identity chains are consistent;
- documents use final naming/directory rules;
- full targeted/integration test evidence is recorded and reviewed;
- no known accepted-contract violation is hidden behind frontend fixtures.