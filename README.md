# CIB W78 2026 Conference Paper - Expert Practitioner Survey Data

Survey data for the conference paper 'Mixed-Methods Evaluation of Critical Data
Requirements for Heat Pump Product Configuration' presented at CIB W78 2026 –
43rd International CIB W78 Conference on IT in Construction.

This repository is an **appendix** to the associated research paper (see
`CITATION.cff`). It provides:

- the **survey response data** and its **codebook**,
- **additional evaluation figures** that did not fit into the paper due to
  space constraints.

### Companion repository

This paper has a companion paper at ECPPM 2026, *Critical Information
Requirements for Building Service Product Selection: An Evaluation of Open
Information Specification Formats*, which references the survey published here.
Its supplementary material — example IDS and LOIN specifications for heat
pumps — is available at:

<https://github.com/Design-Computation-RWTH/ecppm-2026-productSelection>

> **Note on language.** The dataset and codebook are in **German**, as the
> survey was conducted in German and no translation layer is published. This
> README describes every question block in English so the data can be
> interpreted without reading German.

## Project context

The survey was carried out as part of a research project funded through the
*Zukunft Bau* research funding programme:
<https://www.zukunftbau.de/projekte/forschungsfoerderung/1008187-2424>

ISO 16757 defines data structures for product data of technical building
equipment. The survey investigates *which* heat pump properties practitioners
need, *when* they need them over the course of a project, and *how important*
they are — in order to inform the ongoing standardisation work.

## Survey

The online survey was conducted with
[SoSci Survey](https://www.soscisurvey.de/), the data was collected between
13.04.2026 and 03.09.2026 at
<https://www.soscisurvey.de/rwth_aachen_iso16757/>.

### Sample size

**Evaluated survey n = 42.**
**Analysis set n = 36.**

Nearly all questions were optional. Respondents were allowed to skip specific question blocks
if they lacked sufficient professional experience. There is therefore no single n for the whole survey
and any analysis should report the n of the block it uses.

#### Data selection

| Stage | n | |
|---|---|---|
| Records retained in raw data export | 48 | submitted at least one page |
| …with at least one question actually answered | **42** | **published in this repository** |
| …with the professional role (`BE02`) answered | **36** | **analysis set for conference paper** |

Six of the 48 raw data records contain no answers at all.
They carry only process metadata and are not included in the published file.

The published dataset deliberately contains all 42 evaluated records rather than only the
36-person analysis population, so that the inclusion decision can be verified and alternative criteria can be
applied. The analysis set is reproducible directly from the published data:

```r
d <- read.csv("data/survey_responses.csv")
analysis <- d[!is.na(d$BE02) & d$BE02 > 0, ]   # n = 36
```


## Contents

| Path | Description |
|---|---|
| `data/survey_responses.csv` | Survey responses (UTF-8, `.` as decimal separator) |
| `data/codebook.xlsx` | SoSci Survey codebook: variable labels, response codes and response labels |
| `figures/` | Evaluation figures, including ones not used in the paper |

`survey_responses.csv` is a verbatim subset of the original SoSci Survey export:
values are passed through unchanged, and only whole columns and whole records
were removed (see [below](#variables-and-records-removed-from-the-published-dataset)).

`codebook.xlsx` is the SoSci Survey codebook, published **unchanged**. It
therefore documents the questionnaire as it was actually run, including the
variables that were removed from the released response data — those variables
appear in the codebook but have no corresponding column in
`survey_responses.csv`.

## Questionnaire structure

Variable names carry a two-letter block prefix. Item numbering (`_01`, `_02`, …)
follows the order the items were displayed in.

### `BE*` — Professional background

Screening and context questions asked at the start: occupational role
(`BE02`, 17 options spanning MEP design, architecture, public administration and
research), company size (`BE03`), typical project size (`BE04`), number of heat
pump projects worked on (`BE07`), and self-rated heat pump experience (`BE08`).
`BE09` let respondents skip the lengthy property section (`ME02`/`ME04`)
entirely.

### `ME02` — Timeline placement of product properties (46 items)

**The central and methodologically most distinctive question.** Respondents were
shown 46 heat pump properties as **cards** and placed each card on a
**continuous horizontal timeline graphic** divided into eight equally spaced
sections representing the HOAI *Leistungsphasen* (German design and construction
project phases) LP1–LP8. The task was: *at what point in the project do you
first need this information?*

The recorded value is the **horizontal position on the timeline**, on a scale of
0–1000. Because the eight phases are equally spaced, values map to phases as
follows:

| Value range | Phase |
|---|---|
| 0–124 | LP1 — Grundlagenermittlung (basic evaluation) |
| 125–249 | LP2 — Vorplanung (preliminary design) |
| 250–374 | LP3 — Entwurfsplanung (design development) |
| 375–499 | LP4 — Genehmigungsplanung (building permit application) |
| 500–624 | LP5 — Ausführungsplanung (detailed design) |
| 625–749 | LP6 — Vorbereitung der Vergabe (tender preparation) |
| 750–874 | LP7 — Mitwirkung bei der Vergabe (tender support) |
| 875–1000 | LP8 — Objektüberwachung (construction supervision) |

A value of **`-9` means the card was left unplaced**, i.e. the respondent did not
assign the property to any phase. Note that the codebook does *not* document
`-9` for these variables — it lists only the scale endpoints `0` and `1000`.

Because the response is a continuous position rather than a phase choice, the
data supports both phase-level aggregation and finer-grained analysis
(e.g. medians, spread, bimodality within a phase).

### `ME04` — Importance of product properties (46 items)

The **same 46 properties** as `ME02`, rated on a six-point importance scale
(`1` = Völlig unwichtig / entirely unimportant … `6` = Sehr wichtig / very
important). `-1` means *unknown property / no answer*, `-9` means *not
answered*.

Because `ME02` and `ME04` share item numbering, the two blocks can be joined
item by item to relate *when* a property is needed to *how important* it is.

The properties span functional characteristics (type, heat source, operating
mode), performance figures (nominal heating/cooling capacity, COP, EER,
electrical input, temperature limits), refrigerant details (type, charge, safety
class, GWP), physical characteristics (dimensions, mass, sound levels, IP
ratings), cost information, controls, and dynamic performance
curves.

### `GE02`, `GE03`, `GE04` — Geometry requirements

Three short Likert blocks on the geometric representation of products in
CAD/BIM models, using the same six-point importance scale as `ME04`:

- **`GE02` — Level of geometry (LOG), 5 items.** How detailed should the
  geometry be: simple 2D symbol, detailed 2D symbol, coarse 3D solid, detailed
  3D geometry, or highly detailed 3D geometry with colours and textures.
- **`GE03` — Connection geometries, 3 items.** Media connections (pipes, ducts),
  fastening connections, and control/signal connections.
- **`GE04` — Product spaces, 5 items.** Clearance volumes around the product:
  operating space, access space, transport/installation-route space,
  installation space, and the total space used for clash detection.

### `RN*` — Existing standards and data formats

Two parallel sub-blocks with identical structure, one per standard:

- **VDI 3805**: experience (`RN03`), importance (`RN04`), free-text comment
  (`RN05_01`)
- **ISO 16757**: experience (`RN07`), importance (`RN08`), free-text comment
  (`RN09_01`)

Experience uses a six-point scale from *Keine Erfahrung* (no experience) to
*Experte/Expertin* (expert).

### `SO*` — Software, workflows and product data sourcing

Multiple-choice checkbox blocks. For each block, the parent variable (`SO02`,
`SO03`, …) holds the **number of options selected**, or a negative code if the
fallback option was chosen; the `_NN` variables are the individual checkboxes.
Each block ends with a "Weitere" (other) checkbox plus an open text field
(suffix `a`).

| Block | Topic | Items |
|---|---|---|
| `SO02` | CAD/BIM authoring tools in use (AutoCAD, Revit, Archicad, MagiCAD, DDScad, …) | 20 |
| `SO03` | Data exchange platforms / CDEs (e-mail, cloud storage, ACC, BIMcloud, Dalux, …) | 15 |
| `SO04` | Exchange file formats (PDF, DWG, IFC, BCF, gbXML, proprietary BIM formats, …) | 12 |
| `SO05` | Where product data is obtained (manufacturer portals, cross-vendor portals, in-house libraries, other project participants, …) | 8 |
| `SO06` | Formats product data arrives in (PDF, XLSX, DWG, RFA, IFC, VDI 3805, ETIM, …) | 9 |

### `SD*` — Closing questions

`SD04` is a seriousness check (*did you answer seriously?*, Ja/Nein).
`SD05_01` and `SD06_01` are free-text comments on the survey content and on the
survey itself.

### Technical variables

`CASE`, `STARTED`, `TIME001`–`TIME028` (seconds spent per page), `TIME_SUM`,
`LASTPAGE`, `MAXPAGE`, `MISSING`, `MISSREL`, `TIME_RSI`, `FINISHED`,
`Q_VIEWER`, `LASTDATA`, `STATUS` are SoSci Survey process metadata.

## Missing value codes

| Code | Meaning |
|---|---|
| `-9` | Item left unanswered / skipped (for `ME02`: card left unplaced) |
| `-1` | Explicit "unknown / no answer" option selected |
| empty | Page not reached (structural non-response) |

## Known issues and caveats

### Boolean encoding differs from the codebook

The dataset was exported from SoSci Survey in its "GNU R" format, which writes
booleans as `T` / `F`. The codebook documents the underlying numeric codes as `1` / `2`
instead. The values correspond one-to-one:

| Variables | Codebook | Dataset |
|---|---|---|
| All checkbox items (`SO02_NN`, `SO03_NN`, `SO04_NN`, `SO05_NN`, `SO06_NN`) | `1` = nicht gewählt, `2` = ausgewählt | `F` / `T` |
| `FINISHED`, `Q_VIEWER` | `0` / `1` | `F` / `T` |

All other variables match the codebook exactly. The data is published in its
original export form rather than recoded.

### Scale coding errors

Two response scales were accidentally coded with non-sequential numeric codes when the questionnaire was built. 
They were not changed retrospectively to avoid inconsistent records.
. **Both require care when analysing:**

- **`BE08` (heat pump experience)** — the numeric codes are not in ordinal
  order. The correct order is `1` (Keine Erfahrung) → `2` (Geringe) → **`5`
  (Etwas)** → `3` (Einige) → `4` (Viel Erfahrung). Sorting by numeric code
  produces a wrong scale.
- **`RN04` and `RN08` (importance of VDI 3805 / ISO 16757)** — coded
  `1, 2, 3, 5, 6, 7`; code `4` is unused. The scale is a normal six-point scale
  with a gap in the numbering. This was known and accounted for in the analysis.
  Note that `ME04_*` and `GE0*_*` use a clean `1`–`6` for the same scale labels.

### Questionnaire shortened during data collection

The questionnaire was shortened partway through data collection because the
informational value of some questions was deemed low relative to the response
burden. The variable `QUESTNNR` records which version a respondent saw:
`iso16757_umfrage` (full) or `iso16757_umfrage_kurz` (shortened).

Two question groups were removed in `iso16757_umfrage_kurz`. The are not part
of the analysis but are **published in full**, because the responses collected are complete 
and clean for everyone who saw them:

| Variables | Question | Asked until |
|---|---|---|
| `SO03`, `SO03_01`–`SO03_15`, `SO03_15a` | Data exchange platforms | 2026-06-12 |
| `RN07`, `RN08`, `RN09_01` | ISO 16757 experience / importance / comment | 2026-07-10 |

Any analysis using these variables must account for the reduced base.

### Variables and records removed from the published dataset

The codebook is published unchanged, so it still lists these variables. They
have **no column** in `survey_responses.csv`:

| Variable | Reason |
|---|---|
| `BE05` Unternehmen Land (D/A/CH) | Removed from the questionnaire on 2026-06-12 due to low informational value and non-zero risk of re-identification in a small, professional sample |
| `BE06_01` Unternehmen Ort (PLZ) | as above |
| `SD02` Beruflicher Bildungsabschluss | as above |
| `SD03` Alter (Kategorien) | as above |
| `SERIAL`, `REF`, `MAILSENT` | Administrative metadata fields, empty in every record |

Six records in which every question was left unanswered (all values `-9`,
`MISSING = 100`) were removed, leaving n = 42 in the published file. See
[Sample size](#sample-size) for the full participation funnel.

`CASE` numbers are the original SoSci Survey interview numbers and are therefore
not consecutive.

## Acknowledgment

This project was funded by the Federal Institute for Research on Building, Urban
Affairs and Spatial Development on behalf of the Federal Ministry for Housing,
Urban Development and Construction, using funds from the *Zukunft Bau* research
funding programme. We want to thank our project partners for their versatile
support.

## License

Data and figures are licensed under
[CC-BY-4.0](https://creativecommons.org/licenses/by/4.0/) — see `LICENSE`.

CC BY 4.0 is one-way compatible with the CC BY-SA 4.0 licence of the project
report: this material may be reused in the report and in other ShareAlike works,
provided attribution is given. The reverse does not hold.

## Citation

See `CITATION.cff`, or use the "Cite this repository" button on GitHub.
