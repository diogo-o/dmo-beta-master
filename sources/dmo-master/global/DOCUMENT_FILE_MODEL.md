# Source provenance

- Repository: `diogo-o/dmo-master`
- Branch/ref: `dmo-modular`
- Original path: `global/DOCUMENT_FILE_MODEL.md`
- Source blob SHA: `65b4dddf9d8f3d437e26f4b42471f730e46f2004`
- Classification: `ARCHITECTURE AUTHORITY`
- Archived for Beta provenance. Upstream `dmo-master/dmo-modular` remains the global authority.

---

# Document and File Model

## Purpose

This is the cross-domain contract for generated documents, official document metadata and physical file organization.

`global/INFORMATION_MODEL.md` defines the owning record/context identities. Documents never create an alternative identity system and never become the source of operational truth.

## 1. Core rule

> **The structured owning record is truth. A PDF/file is a derived representation.**

Keep separate:

- live reference/master images where the owning domain defines them as live;
- structured operational records and historical contexts;
- generated PDF bytes;
- official document metadata where a workflow genuinely freezes/writes it;
- physical directory/file paths.

A filename/path never replaces canonical IDs.

## 2. Job On generated documents

Job On print/consultation documents derive from the saved Job On production context:

```text
jobon_id
├─ general production/manual data
├─ cm_id historical context
├─ mf_id historical context
├─ bq_id historical context
├─ verification/notes where required
└─ other confirmed Job-On-owned content
```

Deterministic regeneration/read behavior must use the historical values recorded for that `jobon_id`, not today's mutable Tool metadata.

Known Job On/production print outputs remain governed by the Job On contract: **Ficha de Artigo**, **Job-On Moldes** and **Trabalho de Equipa** are the three official Job On document identities.

Printing/regeneration uses the saved Job On state. An unsaved draft is never printed as an official output without explicit marking.

A multi-page print composition does **not** create additional document identities:

```text
official/output document identity
!=
page inside a multi-page print composition
```

A four-page composition is a page mapping of the three official outputs above; an article-image page may be part of that composition without becoming a fourth document identity.

Missing/overflow conditions are explicit states, never silent partial documents.

## 3. Peso official document

The approved Peso record is the truth; the PDF is derived.

Normal production Peso derives its production/CM context through:

```text
peso_id -> cm_id -> {jobon_id, tool_id}
```

Independent no-Job-On Peso derives Tool context through its direct `tool_id`.

A document must render from the Peso's preserved measurement/calculation/approval facts plus its truthful historical context. It must never regenerate historical context from current mutable Tool data.

Peso official-document persistence must preserve the approved record, actor/time attribution, deterministic filename/output location, and explicit missing-file behavior where an immutable official document is required.

## 4. Pegamentos official document

The Pegamentos record is the truth. Its configured CM/MF/BQ context is reached through its Job On and the historical Tool contexts recorded there.

Where Pegamentos requires an official immutable document, certification metadata is write-once/frozen according to the workflow.

## 5. Resumo official document

`resumo_id` is a real persisted Controlo record that summarizes control status/history per applicable component of one `jobon_id`. The Resumo record is the truth; the PDF is derived.

```text
resumo_id
-> Resume_<reference>_<line>.pdf
```

The Resumo PDF is an output of the Resumo record, not the identity and not a projection of `controlo_sheet_id`. Resumo (`resumo_id`) and Folha (`controlo_sheet_id`) remain distinct persisted Controlo records. Generation/output placement is implementation work under this contract.

## 6. Directory organization

The settled local production directory organizes generated documents as:

```text
<combined reference>/
└─ <production number>/
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

Production/reference folder labels are path/display context, not canonical join keys.

The application derives folders from the owning Job On/Peso/Controlo context. Stored paths are output metadata, never operational authority.

## 7. Naming

Confirmed filename rules remain presentation/document rules, not domain identity.

The owner-established names are `Peso_<reference>_<line>.pdf`, `Pegamentos_<reference>_<line>.pdf` and `Resume_<reference>_<line>.pdf` under `<combined reference>/<production number>/` (§6). Where existing module contracts establish a filename pattern, preserve it. Remaining literal details stay bounded rather than invented.

No revision identity is embedded in filenames. `Versões disponíveis` refers to document/output versions where applicable, never to Job On revisions — `job_on_revision_id` stays out of the model.

## 8. Atomicity / frozen official metadata

Where a workflow defines an official write-once document:

- do not create document metadata unless the owning record is in the required approved/certified state;
- bytes and metadata must not disagree;
- opening an official frozen file must not silently regenerate it from newer mutable state;
- missing bytes are an explicit condition, not permission to fabricate new historical content.

For live-on-demand consultation outputs, deterministic rendering from the saved structured record is sufficient and a metadata table is not required merely for symmetry.

### Document availability states

Document availability is an explicit state, never a silent error:

- states: `Disponível`, `Ainda não gerado`, `A aguardar aprovação`, `Workspace indisponível`, `Ficheiro em falta`, `Versões disponíveis`;
- an access/action is enabled only when it can complete the corresponding operation;
- a record/output may legitimately be absent (for example Pegamentos without a record); the UI disables/greys the related access with the explicit state instead of producing an error;
- a consultation failure is distinguishable from an empty result ("no documents match the filters"); a failed lookup is never shown as an empty list;
- generated PDFs do not print local file paths (`file:///` or similar machine paths); document pages identify the document, reference, production, machine, control and date.

## 9. Reference images

A live reference/master image remains owned by its real master/reference context according to the established workflow. It is not automatically copied into every Job On merely to create another identity.

If historical reproduction requires a particular image state later, that need must be explicitly established rather than inferred from the existence of PDFs.

## 11. Invariants

- structured record/context is authority; file is derived;
- files/paths do not create domain identity;
- historical render uses historical context, not current mutable Tool state;
- Job On document context uses `jobon_id` + CM/MF/BQ historical contexts;
- Job On has exactly three official document identities (Ficha de Artigo, Job-On Moldes, Trabalho de Equipa); a page in a print composition is never a document identity;
- production Peso document context uses `cm_id`; independent Peso uses its truthful direct Tool relation;
- `resumo_id` is a persisted Controlo record (per Job On component control), not a projection; its PDF is an output, not the identity;
- no document persistence table is created for symmetry alone;
- corrections preserve attribution/history and never silently rewrite frozen official metadata;
- document availability is an explicit state (`Disponível` / `Ainda não gerado` / `A aguardar aprovação` / `Workspace indisponível` / `Ficheiro em falta` / `Versões disponíveis`); missing output and failed lookup are distinct from empty results;
- a PDF never prints local file paths;
- no revision identity is embedded in filenames; `Versões disponíveis` is about document/output versions.
