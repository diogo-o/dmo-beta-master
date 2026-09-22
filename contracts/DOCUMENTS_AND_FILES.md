# Beta Documents and Files Contract

## Status

Canonical Beta consolidation of `dmo-master/dmo-modular/global/DOCUMENT_FILE_MODEL.md` plus the current Beta module scope.

This document does not replace the global master contract. It states exactly what the Beta must preserve.

## 1. Core authority rule

The structured owning record is truth. A generated PDF or filesystem path is a derived representation.

Never treat:

- a filename;
- a directory;
- PDF bytes;
- a browser-visible path;

as a replacement for canonical identities such as `jobon_id`, `peso_id`, `pegamentos_id`, `resumo_id`, `tool_id`, `cm_id`, `mf_id` or `bq_id`.

## 2. Beta document outputs

### Peso

The approved `peso_id` is the authority.

Production-bound context:

```text
peso_id -> cm_id -> jobon_id + tool_id
```

Independent/pending-association context:

```text
peso_id -> tool_id
```

The official PDF is generated from preserved Peso measurements, calculations, decisions, attribution and truthful historical context.

Filename:

```text
Peso_<reference>_<line>.pdf
```

### Pegamentos

`pegamentos_id` is the authority. The PDF is derived from the persisted control and the saved Job On CM/MF/BQ contexts.

Filename:

```text
Pegamentos_<reference>_<line>.pdf
```

Pegamentos may legitimately be absent. Missing record is not a failure condition.

### Resumo

`resumo_id` is a real persisted Controlo record. It is distinct from `controlo_sheet_id`.

Filename:

```text
Resume_<reference>_<line>.pdf
```

The PDF is an output of `resumo_id`, not its identity.

### Job On Light

The Beta must not invent new Job On document identities.

The global Job On contract owns the three official output identities:

- Ficha de Artigo;
- Job-On Moldes;
- Trabalho de Equipa.

If Job On Light exposes print/consultation of these outputs, rendering uses saved `jobon_id` state and frozen CM/MF/BQ historical context. An unsaved draft is not an official output.

## 3. Directory convention

The Beta uses the final production directory convention from the start:

```text
<reference>/
└─ <production-number>/
   ├─ Peso_<reference>_<line>.pdf
   ├─ Pegamentos_<reference>_<line>.pdf
   └─ Resume_<reference>_<line>.pdf
```

Example:

```text
5447T173/
└─ 202601/
   ├─ Peso_5447T173_B3.pdf
   ├─ Pegamentos_5447T173_B3.pdf
   └─ Resume_5447T173_B3.pdf
```

The application derives the directory from persisted operational context. Folder names are display/output context, not join keys.

## 4. Official/frozen output rule

Where a workflow requires an immutable official output:

- owning record must already be in the required approved/certified state;
- document metadata and bytes must correspond to the same frozen state;
- opening a frozen document must not silently regenerate it from current mutable Tool data;
- missing bytes are reported as missing, not recreated from newer state without an explicit permitted workflow;
- correction/version history must preserve attribution.

Live consultation outputs may be deterministically regenerated from the saved structured record when their owning contract allows it. Do not add a document metadata table merely for symmetry.

## 5. Availability states

The Beta frontend uses these explicit document availability meanings:

- `Disponível`;
- `Ainda não gerado`;
- `A aguardar aprovação`;
- `Workspace indisponível`;
- `Ficheiro em falta`;
- `Versões disponíveis`.

Rules:

- missing optional output is not a generic error;
- lookup failure is not rendered as an empty result;
- actions are enabled only when the operation can actually complete;
- a non-existent Pegamentos record may produce a disabled/grey Pegamentos document action with an explicit state;
- local machine paths (`file:///...`) are never printed in the PDF.

## 6. Historical rendering

Historical documents render historical context.

Do not regenerate old PDFs from today's mutable Tool fields when the owning record already preserved the historical values used at the time.

Examples:

```text
Peso history
-> preserved peso facts
-> cm_id context
-> frozen values used for calculation

Job On history
-> jobon_id
-> frozen cm_id / mf_id / bq_id context
```

## 7. Backend / frontend split

Backend owns:

- owning-record state;
- document generation authorization;
- persisted metadata where required;
- deterministic filename/output context;
- historical source data;
- atomic write/freeze behavior;
- missing-file condition;
- versions where supported.

Frontend owns:

- presenting availability states;
- request/open/regenerate controls only when supplied as allowed;
- disabled reasons;
- progress/error presentation;
- never constructing domain truth from a path or filename.

## 8. Beta acceptance conditions

A document implementation is acceptable only if:

1. canonical record remains the source of truth;
2. the filename/path is not used as an operational identity;
3. historical outputs do not drift with live Tool changes;
4. Peso/Pegamentos/Resumo names follow the settled convention;
5. directory hierarchy follows `<reference>/<production-number>/`;
6. missing optional output, missing file and lookup failure are distinguishable;
7. official frozen outputs are not silently regenerated from newer mutable facts;
8. PDFs do not expose local filesystem paths;
9. no artificial document ID/table is introduced merely for symmetry;
10. document access remains gated by the owning workflow/action permissions.