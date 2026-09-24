# Ferramentas Light — Beta canonical contract

## Purpose

Ferramentas Light provides the Beta's contextual Tool selection, creation and read surface using the final canonical `tool_id` identity. It is intentionally narrower than the full Ferramentas lifecycle.

## Authority

- Beta scope/workstream: `workbench/BETA_FRONTEND_IMPLEMENTATION_WORKSTREAMS.md` Workstream B.
- Global domain authority: `dmo-master/dmo-modular/modules/FERRAMENTAS.md`.

## Included in Beta

- search canonical Tools by human-facing metadata;
- explicit candidate selection;
- contextual Tool summary/detail needed by Job On, Controlo and Boquilhas;
- create a missing Tool and return the canonical `tool_id` to the originating workflow;
- create that canonical Tool without requiring Armazém location/movement data;
- reuse the canonical Tool ficha/read model where available;
- preserve one shared Tool orchestration rather than per-module copies.

## Explicitly outside Beta

- full Tool change-request lifecycle;
- complete Ferramentas Approve review workflow;
- full technical-condition dossier/history UI;
- complete utilisation history UI;
- complete change-request audit surface;
- Armazém location/movement ownership;
- any requirement to assign a Tool to an Armazém position during Beta Tool creation;
- full future Tool-maintenance experience.

## Canonical identity

```text
tool_id
= one concrete operational Tool
```

Different lot means different canonical Tool identity.

```text
same reference + different lot
→ different tool_id
```

Machine/line compatibility, reference, type and lot are search/display facts. They never replace `tool_id` after selection.

## Tool-owned facts relevant to Beta

The Beta may consume Tool facts required by its active workflows, including where published:

- type/family;
- reference;
- lot;
- machine/line compatibility;
- processo = NNPB | PS;
- canonical quantity/total where applicable;
- dimensional nominal/drawing facts needed by Pegamentos;
- persistent operational note;
- current Tool facts exposed by the backend.

Beta surfaces must not duplicate these into module-owned master data.

## Contextual, not top-level

Ferramentas and Ferramentas Approve are contextual/invisible access levels. They create no top-level navigation destination.

A Tool ficha may be opened contextually from Job On, Controlo, Boquilhas or other future modules when a real `tool_id` is referenced and access allows it.

## Tool creation without Armazém

Armazém is outside Beta scope. A new canonical Tool must therefore be creatable and usable by Beta workflows without any warehouse/location assignment.

```text
Criar Tool
→ persist canonical tool_id + Tool-owned facts required by the Tool contract
→ no Armazém position required
→ return tool_id to Job On/origin workflow
```

Absence of Armazém context is not an error and must not block Tool creation, Tool selection, Job On planning or downstream Beta use.

When Armazém is implemented later, it may add its own location/movement relation to the existing `tool_id`; it must not require replacement of the Tool identity created in Beta.

## Search/select/create contract

```text
origin workflow
→ Tool search
→ zero / one / many candidates
→ human explicitly selects
OR
→ Criar ferramenta
→ backend creates canonical Tool
→ returns tool_id
→ restore origin state
→ associate returned tool_id
```

Rules:

- never auto-select an ambiguous candidate;
- never infer Tool identity from reference + lot + machine alone;
- never create a module-specific Tool identity;
- cancel returns to the origin with state preserved;
- create returns the canonical `tool_id`, not a display-key surrogate.

## Ownership

Ferramentas owns Tool master facts.

Job On owns production-specific CM/MF/BQ context.
Controlo owns measurement/control facts.
Boquilhas owns BQ external-repair quantity facts.
Armazém owns physical location/movement facts.

The origin module never becomes the owner of Tool master data merely because it launches the picker/create flow.

## Access

Global access semantics remain:

- `Ferramentas` — contextual Tool ficha/create and normal edit-via-change-request in the full system;
- `Ferramentas Approve` — includes Ferramentas and adds direct/review capabilities in the full system.

For Beta Light, only the subset actually implemented may be exposed, but the access vocabulary and canonical identity must remain final.

## Frontend responsibility

Frontend may:

- render search results and Tool summary;
- preserve origin workflow state;
- expose explicit select/create/cancel actions;
- map published Tool facts into view models.

Frontend must not:

- invent Tool IDs;
- create a private Tool registry;
- infer compatibility rules not supplied by authority/backend;
- parse free-text operational notes into business state;
- own canonical Tool mutations outside the published Ferramentas contract.

## Backend contracts required

- Tool search/query;
- Tool detail/read;
- canonical Tool create;
- candidate filtering by required context;
- return of canonical `tool_id`;
- any Beta-supported edit capability must have a separately published backend/action contract.

## Acceptance criteria

- ambiguous candidates require explicit human selection;
- no candidate is silently auto-selected;
- inline Tool creation returns a canonical `tool_id`;
- Tool creation succeeds without Armazém location/movement data;
- cancel restores the origin workflow unchanged;
- Job On/Controlo/Boquilhas reuse the same Tool orchestration;
- Tool-visible fields are read from Tool authority, not copied into module master data;
- Ferramentas remains absent from top-level navigation.

## Required evidence

- ambiguous-candidate tests;
- zero-result/create flow;
- cancel/origin-state restoration;
- canonical ID return test;
- contextual access tests;
- regression proving no top-level Ferramentas destination.
