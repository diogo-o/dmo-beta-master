# DMO Beta — Backend / Frontend Model

## Core rule

The frontend presents and orchestrates accepted behavior. The backend owns persisted facts, canonical identities, domain relationships, authorization enforcement and workflow transitions.

The frontend must never become a second domain model.

## Responsibility split

### Backend owns

- canonical IDs and record creation;
- persistence and transactions;
- authoritative relationships between Job On, Tool, CM/MF/BQ contexts, Peso, Pegamentos, Folha/Controlo, Resumo and Boquilhas;
- access resolution and server-side authorization gates;
- calculation logic and accepted formulas;
- state transitions such as submit, approve, reject, reopen, close/reopen where applicable;
- append-only operational facts such as Boquilhas movements;
- audit attribution and immutable system timestamps;
- concurrency/version checks;
- authoritative document availability/generation facts;
- validation that depends on domain/persistence state.

### Frontend owns

- rendering backend-backed state;
- input-shape validation for immediate feedback;
- preserving unsaved form state during authorized subflows;
- calling approved application/backend contracts;
- mapping backend DTO/read models into presentation models;
- warnings and explicit human choices;
- navigation presentation derived from accepted access output;
- responsive/tablet presentation, keyboard/focus behavior and dense operational layout.

### Frontend must not

- mint canonical IDs;
- infer Tool identity from reference/lot/machine display text;
- reconstruct backend relationships from UI labels;
- create client-only records that masquerade as persisted domain state;
- use hidden buttons/links as authorization enforcement;
- duplicate another module's state as a second source of truth;
- implement formulas when the backend owns them;
- auto-decide industrial outcomes from warnings;
- invent production endpoint names or persistence semantics from fixtures.

## Canonical identity chain

The Beta preserves stable identities so the future complete DMO enriches the same records rather than replacing Beta-only identities.

Canonical identities include:

```text
jobon_id
tool_id
cm_id
mf_id
bq_id
peso_id
pegamentos_id
controlo_sheet_id
resumo_id
boquilhas_id
movement_id
```

Representative relationships:

```text
jobon_id
├─ reference
├─ production_number
├─ machine
├─ processo
├─ cm_id?
├─ mf_id?
└─ bq_id?

jobon_id + CM tool_id -> cm_id
jobon_id + MF tool_id -> mf_id
jobon_id + BQ tool_id -> bq_id

peso_id -> cm_id -> tool_id + jobon_id
current_peso_id -> previous_peso_id

pegamentos_id -> jobon_id -> cm_id / mf_id / bq_id as required
controlo_sheet_id -> jobon_id
resumo_id -> jobon_id + applicable component contexts

production-linked Boquilhas:
jobon_id + BQ tool_id -> bq_id
boquilhas_id -> bq_id -> tool_id + jobon_id
movement_id -> boquilhas_id

standalone Boquilhas:
boquilhas_id -> tool_id
movement_id -> boquilhas_id
```

## Selection versus inference

The user explicitly selects operational records where ambiguity exists.

Examples:

- Tool picker never auto-selects an ambiguous Tool;
- Job On duplication requires explicit source selection, including older/non-latest source if desired;
- previous Peso is explicitly selected and is not forced to latest;
- same-machine filtering must not hide otherwise valid compatible prior records when authority allows cross-machine use;
- Tool creation returns a canonical `tool_id` and returns the user to the originating workflow without losing entered work.

## Frontend provisional contracts

A frontend workstream may use a fixture/view model only when the real backend/API contract is not yet available and the artifact is explicitly classified:

```text
PROVISIONAL FRONTEND CONTRACT
→ frontend development/testing only
→ derived exclusively from approved Beta/global authority
→ not backend authority
→ not persistence authority
→ creates no canonical ID
→ creates no definitive endpoint contract
```

Fixtures may model loading, ready, empty, lookup failure, unavailable, permission denied, validation failure, stale/conflict and representative record states.

Fixtures may NOT define:

- final endpoint names;
- database schema;
- generated IDs;
- persistence transitions;
- authorization rules;
- calculation formulas;
- canonical relationship ownership.

When the real backend contract exists:

```text
provisional frontend contract
→ compare with actual backend contract
→ adapt mapping
→ retire fixture adapter
```

## Authorization model

Navigation and UI visibility are projections, not grants.

Canonical chain:

```text
authenticated account
→ active application USER
→ Template
→ selected Module IDs
→ Module Registry
→ Access Resolver
→ effective Modules/actions
→ navigation/action presentation
```

Server-side gates remain authoritative for direct routes/actions.

Important Beta vocabulary includes separate assignable capabilities such as:

- Job On View;
- Job On Create;
- Controlo Create;
- Controlo Approve;
- Boquilhas;
- Ferramentas;
- Ferramentas Approve;
- other globally defined Modules as applicable.

A user may hold any valid combination. `role` is display metadata only.

## Shared destination rule

Different permissions may share one visible destination without becoming one permission.

Example:

```text
Controlo Create
Controlo Approve
→ one visible Controlo destination
```

The backend/server-side action gate still decides which actions are allowed.

## Cross-module reads

A module may display related state owned elsewhere only through an accepted read contract. It must not copy the foreign module's domain state into its own frontend/backend model merely to render a status.

Examples:

- Job On can show related Controlo/Boquilhas/document status without owning those records;
- Controlo consumes the selected Job On production context rather than reconstructing reference/production/machine independently;
- Boquilhas can navigate/display Job On context without becoming owner of Job On planning.

## Persistence and lifecycle examples

### Controlo

Create edits and submits the same `peso_id`. Approval consumes that submitted record. There is no second approval-copy Peso.

### Boquilhas

Movement types are append-only quantity facts. Editing an existing movement preserves before/after audit and does not create a second quantity event unless an explicitly authorised annulment/correction model says otherwise.

### Job On duplication

Duplication creates a new `jobon_id` and new production-context IDs where copied while retaining canonical `tool_id` values until the user explicitly changes them. Source records remain unchanged.

## Integration rule

If the UI needs a fact the backend cannot currently provide, the correct response is a backend/interface task or blocker. The frontend does not make up a production substitute.
