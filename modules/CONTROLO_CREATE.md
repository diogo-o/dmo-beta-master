# Controlo Create — Beta canonical contract

## Purpose

Controlo Create owns the Beta measurement/registration workflow around Peso, Comparação, Pegamentos, Folha and Resumo. It consumes production context from Job On and Tool facts from Ferramentas without duplicating their authority.

## Authority

- Beta scope/workstream: `workbench/BETA_FRONTEND_IMPLEMENTATION_WORKSTREAMS.md` Workstream C.
- Global domain authority: `dmo-master/dmo-modular/modules/CONTROLO.md`.

## Included in Beta

- select/create Job On context;
- display production context;
- resolve/reuse CM context;
- Peso draft/edit/calculation/submission;
- variable measurement rows;
- water temperature and configured calculation inputs;
- per-CM capacity and glass-weight results;
- warnings without automatic approval/rejection;
- optional SAP previous-production reference fields;
- explicit previous-Peso selection;
- Comparação build/rebuild with explicit pairing;
- Pegamentos CM/BQ/MF measurement sections;
- missing-context recovery through Job On/Tool orchestration;
- Folha preparation/submission;
- Resumo preparation;
- Create-side history/document availability views.

## Explicitly outside Beta

- approval/rejection/reopen decisions owned by Controlo Approve;
- automatic previous-Peso selection;
- same-machine-only comparison rule;
- duplicate Tool registry;
- independent production identity;
- frontend-owned formulas or persistence semantics;
- a separate approval-copy Peso.

## Canonical identities

```text
peso_id
= one Peso control/result fact

pegamentos_id
= one Pegamentos dimensional-control fact

controlo_sheet_id
= one persisted Folha de Controlo

resumo_id
= one persisted Resumo control record
```

Normal production relation:

```text
peso_id -> cm_id -> tool_id + jobon_id
```

The Beta must not redundantly persist direct `tool_id + jobon_id` in the normal production case merely for navigation.

## Pending association case

A truthful Peso may exist before the correct Job On CM context exists or when the real controlled CM differs from the currently configured Job On CM.

```text
peso_id -> tool_id
status/display: Job On por associar
```

This is valid, not an error, and does not block measurement or approval.

Later candidate `cm_id` values resolving to the same `tool_id` may be shown, but association is always human-confirmed. No automatic latest/date/reference/machine inference is allowed.

After confirmation:

```text
peso_id -> cm_id -> tool_id + jobon_id
```

and the temporary direct Tool anchor is cleared.

## Peso workflow

```text
select/create Job On
→ resolve truthful CM context
→ enter Peso draft
→ add/edit measurement rows
→ request/receive calculations
→ inspect individual CM results and warnings
→ optionally build Comparação
→ submit SAME peso_id for review
```

Submitting does not create a second approval record.

## Calculation ownership

Backend/domain calculation authority remains canonical.

Preserved functional formulas include:

```text
Capacidade / Volume do CM
= Peso de água ÷ configured water-temperature value
```

```text
Peso do vidro
= (Capacidade do CM + Volume Marisa/BQ − Volume Punção/PU)
  × Densidade do vidro
```

The frontend renders inputs/results and immediate input-shape feedback; it must not redefine the formulas as an independent authority.

Canonical supported water-temperature range: 5–35 °C.

Individual CM results remain visible. An average never hides an individual bad result.

## Measurement rows

Rows are variable; the old fixed UI row count is not a domain cardinality rule.

- add rows;
- remove rows;
- at least one row remains;
- row identity must remain stable during editing.

## Processo

`processo` belongs to canonical `tool_id` and is consumed through `cm_id -> tool_id`. Peso may snapshot the configuration/value actually used for historical reproducibility, but does not become a second authoritative owner.

## Comparison

Comparação is a Peso record type/workflow, not a separate identity.

```text
current_peso_id -> previous_peso_id
```

Rules:

- previous Peso is explicitly selected by the human;
- never auto-select latest/previous;
- same-machine is not required when the same canonical Tool is compatible with both production machines/lines;
- pairing is explicit and validated;
- previous/source Peso remains immutable;
- current row value is that row's saved Peso do vidro;
- if current readings change after Comparison build, Comparison becomes stale and must be rebuilt before submission;
- SAP manual reference fields do not create a Comparison relation.

## Pegamentos

```text
pegamentos_id -> jobon_id
jobon_id -> cm_id / mf_id / bq_id
```

Pegamentos reuses configured Job On contexts and does not independently reselect the same Tools for convenience.

Preserved behavior:

- CM/BQ/MF component sections;
- Costura 0°;
- Contra-costura 90°;
- signed ovalização;
- average;
- single-axis behavior;
- variable rows;
- Tool/drawing nominal auto-populated from canonical Tool data;
- tolerance corridor `nominal ± 0.20`;
- reaching/crossing the boundary raises a warning;
- warnings never auto-approve/reject/block;
- missing nominal -> `NotEvaluable`, never invented;
- if required Job On CM/BQ/MF context is invalid/missing, block that sheet with an actionable correction message rather than silently choose another Tool.

A Pegamentos record may legitimately be absent. Missing optional output is distinct from lookup failure.

## Folha and Resumo

Folha and Resumo are separate persisted records:

```text
controlo_sheet_id -> jobon_id
resumo_id -> jobon_id + applicable contexts
```

The Beta must not merge them into one record merely because both are control outputs.

## Access

`Controlo Create` is a distinct assignable module.

It may share the visible `controlo` destination with `Controlo Approve`, but create and approve permissions remain separate backend gates.

## Frontend responsibility

Frontend may:

- orchestrate Job On selection/context loading;
- preserve draft state during Tool-context recovery;
- render dense Peso/Pegamentos/Folha/Resumo screens;
- send calculation requests and render returned results;
- manage client-side row editing state;
- expose explicit human selections and warnings.

Frontend must not:

- own calculation formulas independently;
- infer previous Peso;
- infer Job On association;
- mutate Tool/Job On truth to make a form easier;
- synthesize document availability;
- create second copies of foreign module facts.

## Backend contracts required

- Job On select/create and context read;
- CM association/read;
- Peso create/read/update/calculate/submit;
- previous-Peso candidate query and explicit relation persistence;
- Comparison pairing/build/rebuild;
- Pegamentos persistence/evaluation;
- Folha/Resumo persistence;
- document availability/read;
- audit/history.

## Acceptance criteria

- normal Create starts from a real selected/created `jobon_id`;
- production context remains visible and is not independently reconstructed;
- truthful CM context is used;
- pending association is supported without fake Job On/context;
- rows are variable with at least one row;
- water temperature obeys the canonical range;
- individual CM results remain visible;
- warnings remain warnings;
- previous Peso is explicit, never automatic;
- cross-machine candidate remains valid when the same Tool is compatible with both;
- stale Comparison requires rebuild;
- submit transitions the same `peso_id`;
- Folha and Resumo remain distinct records.

## Required evidence

- row add/remove/validation tests;
- calculation request/result rendering tests against approved backend fixtures;
- pending-association tests;
- explicit previous-Peso/no-default tests;
- cross-machine compatibility candidate tests;
- stale/rebuild tests;
- same-`peso_id` draft-to-submit integration test;
- Pegamentos single/two-axis and boundary warning tests;
- missing Tool context recovery with draft preservation;
- Folha/Resumo identity/document-state tests.
