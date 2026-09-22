# Current Beta Foundation

## Purpose

This file records the implementation foundation that the Beta may safely build on.

It distinguishes:

- accepted implementation;
- implemented but not yet Architect-accepted work;
- historical/developer-reported execution evidence.

This is implementation-state documentation. It does not override the functional authority of `dmo-master/dmo-modular` or the Beta scope in this repository.

## Accepted foundation

### P1-T04 — Module Registry + Access Resolver — ACCEPTED

Accepted implementation commit:

`6725657848028b86b404d16b655f8ef0f3b7ade5`

Important accepted facts for Beta:

- exactly 13 canonical code-owned Module identities;
- stable Module IDs;
- display names are presentation metadata only;
- Destination IDs are presentation/navigation metadata only;
- Ferramentas and Ferramentas Approve are contextual-only;
- shared visible destinations never merge Module permissions;
- unknown persisted Module fails closed;
- known-but-unavailable Module fails closed;
- access resolution is atomic — no partial grants;
- role/profile strings do not grant access;
- Template names do not grant access;
- provider claims do not grant access;
- server-side ASP.NET policies project canonical Module identity;
- ADMIN does not bypass USER operational Module policies;
- navigation visibility is not authorization.

At the accepted P1-T04 state, `ModuleRegistrations.CurrentBuildAvailable` was empty because no operational feature surface was live.

### P1-T05 — USER Administration — ACCEPTED

Accepted implementation commit:

`c7e62eb0b631c43c3340d1f76b32d1ec72bc4ec5`

Important accepted facts for Beta:

- exactly two account classes: dedicated ADMIN and USER;
- Administration uses `dmo.administration`;
- only active dedicated ADMIN receives Administration access;
- role label is presentation-only and grants nothing;
- USER has exactly one nullable `template_id`;
- no membership table;
- no multiple Templates per USER;
- no per-user Module overrides;
- null Template is a valid persisted state with no operational access;
- deactivated USER fails closed on subsequent account resolution;
- Admin pages consume the shared frontend shell rather than creating another shell.

### P1-T06 — Template Administration — ACCEPTED

Accepted final implementation commit:

`09c49fa238c1f35658cab9a4e21072b384de85b6`

Important accepted facts for Beta:

- Templates define Module composition and presentation order;
- Template has nullable landing destination;
- empty Module composition is valid;
- Template has no active/inactive lifecycle;
- new selections may only use currently available Modules;
- persisted unavailable/invalid Modules are surfaced explicitly rather than silently repaired;
- multiple Module grants may share one visible destination without merging permissions;
- landing configuration is validated explicitly;
- invalid persisted data is never silently repaired;
- USER membership remains the single `users.template_id` relation;
- deleting a Template preserves USER rows and account active state;
- optimistic concurrency uses positive version values;
- removing USER membership through a Template context is validated at the Application-service boundary.

## P1-T07 — Navigation + USER Shell — IMPLEMENTED, AWAITING ARCHITECT IMPLEMENTATION REVIEW

Developer implementation commit:

`0b47690936599b6a71342b68b1cf36cfe4b64264`

The Developer response records that P1-T07 was implemented and pushed, but no Architect implementation review exists yet in `dmo-work` at the time of this consolidation.

Therefore:

```text
P1-T07 implementation exists
!=
P1-T07 accepted implementation
```

The accepted PLAN decisions remain authoritative while the implementation itself is pending review.

Plan-settled behavior relevant to Beta:

- `/` is account-aware routing;
- no session/unresolved -> `/Login`;
- ADMIN -> `/Administration`;
- active USER + valid explicit landing -> that destination;
- active USER + null landing -> first navigable destination in Template order;
- zero navigable destinations -> `/AccessDenied`;
- invalid explicit landing -> `/AccessDenied` fail closed, never fallback;
- navigation projection remains owned by the shared frontend A2 service;
- production navigation requires granted + available + non-contextual + routed;
- contextual Ferramentas never becomes a top-level destination;
- direct-route authorization remains server-side;
- no Module availability activation was authorized as part of P1-T07.

Until an Architect implementation review is committed, Beta work must treat the implementation commit as current code state, not an accepted architectural result.

## Shared frontend foundation

`DMO-MODULAR/docs/frontend/SHARED_FRONTEND_CONTRACT_FREEZE.md` is the accepted A1 frontend presentation contract consumed by Beta workstreams.

It freezes shared behavior for:

- ProductionContextStrip;
- ToolPicker;
- ToolSummaryRow;
- DenseDataTable;
- RecordStatus;
- AvailabilityState;
- AuditTrail;
- MeasurementRows;
- DecisionBar;
- common async states.

The frontend components own presentation and generic interaction only. Consumers own domain facts, authorization, persistence and transitions.

## Current application architecture

`DMO-MODULAR` is a modular monolith:

```text
Web
-> Application
-> Domain/shared contracts
-> Persistence / external infrastructure
```

Operational modules own their own workflows and persistence. Shared identities and explicit contracts connect modules.

Modules must not reach arbitrarily into another module's internal tables.

## Beta implementation consequence

New Beta feature work must build on this foundation rather than recreate it.

In particular:

- do not create another account model;
- do not create another access resolver;
- do not create another navigation composer;
- do not create role/profile authorization;
- do not create per-feature permission stores;
- do not create private Tool identities;
- do not create fake production identities when canonical Job On/Tool contexts are required;
- do not mark a Module available until its real supported surface and route are legitimately registered.
