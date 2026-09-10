# CIB W78 2026 Conference Paper - Expert Practitioner Survey Data

Survey data for the conference paper 'Mixed-Methods Evaluation of Critical Data
Requirements for Heat Pump Product Configuration' presented at CIB W78 2026 –
43rd International CIB W78 Conference on IT in Construction.

This repository is an **appendix** to the associated research paper (see
`CITATION.cff`). It provides:

- the **survey response data** and its **codebook**,
- **additional evaluation figures** that did not fit into the paper due to
  space constraints,
- **descriptive and exploratory analysis tables** corresponding to the English
  figures.

### Companion repository

This paper has a companion paper at ECPPM 2026, *Critical Information
Requirements for Building Service Product Selection: An Evaluation of Open
Information Specification Formats*, which references the survey published here.
Its supplementary material — example IDS and LOIN specifications for heat
pumps — is available at:

<https://github.com/Design-Computation-RWTH/ecppm-2026-productSelection>

> **Note on language.** The dataset and codebook are in **German**, as the
> survey was conducted in German and no translation layer is published for the original survey data.
> All statistical analysis artifacts (figure and tables) in this repository appendix are however in English,
> matching the translated survey items as > presented in the conference paper.
> Additionally, this README briefly describes every question block in English so the original surveyx data can be
> re-interpreted without reading German.

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

| Stage | n |
|---|---:|
| Raw records with at least one page submitted | 48 |
| Records with at least one question answered (published dataset) | **42** |
| Records with professional role (`BE02`) answered (paper analysis set) | **36** |

Questions were generally optional, so analyses should report the sample size of
the relevant block. The six process-only records are excluded from the published
dataset; all other records are retained to permit alternative inclusion criteria.
The paper's analysis set can be reproduced with:

```r
d <- read.csv("data/survey_responses.csv")
analysis <- d[!is.na(d$BE02) & d$BE02 > 0, ]   # n = 36
```

## Contents

| Path | Description |
|---|---|
| `data/survey_responses.csv` | Survey responses (UTF-8, `.` as decimal separator) |
| `data/codebook.xlsx` | SoSci Survey codebook: variable labels, response codes and response labels |
| `analysis/figures/` | Evaluation figures |
| `analysis/tables/` | Descriptive, distribution, group-comparison and joint-analysis tables |

`survey_responses.csv` is a verbatim subset of the original SoSci Survey export:
only whole records and columns were removed. The codebook is published unchanged
and therefore also lists excluded variables.

## Questionnaire structure

Variable prefixes identify question blocks; `_NN` follows display order.

| Prefix | Content |
|---|---|
| `BE*` | Professional role and background, project and heat-pump experience |
| `ME02` | First project phase in which each of 46 heat-pump properties is needed |
| `ME04` | Importance of the same 46 properties on a six-point scale |
| `GE02`–`GE04` | Importance of geometry detail, connections and product spaces |
| `RN*` | Experience with and importance of VDI 3805 and ISO 16757 |
| `SO02`–`SO06` | Software, exchange platforms and formats, and product-data sources |
| `SD*` | Seriousness check and closing free-text comments |

`ME02` stores card positions on a continuous `0`–`1000` timeline divided into
eight equal HOAI project phases:

| Value range | Phase |
|---|---|
| `0`–`124` | LP1 |
| `125`–`249` | LP2 |
| `250`–`374` | LP3 |
| `375`–`499` | LP4 |
| `500`–`624` | LP5 |
| `625`–`749` | LP6 |
| `750`–`874` | LP7 |
| `875`–`1000` | LP8 |

`ME02` and `ME04` use matching item numbers and can be joined item by item.
The remaining fields (`CASE`, timestamps, page durations, completion state and
similar variables) are SoSci Survey process metadata.

## Missing value codes

| Code | Meaning |
|---|---|
| `-9` | Item left unanswered / skipped (for `ME02`: card left unplaced) |
| `-1` | Explicit "unknown / no answer" option selected |
| empty | Page not reached (structural non-response) |

## Known issues and caveats

The GNU R export writes checkbox values as `F`/`T`, whereas the codebook uses
`1`/`2` (not selected/selected). For `FINISHED` and `Q_VIEWER`, `F`/`T`
corresponds to `0`/`1`.

Two scales have non-sequential codes and must be ordered by their labels:

- `BE08`: `1` (none), `2` (low), `5` (some), `3` (considerable), `4` (much).
- `RN04` and `RN08`: `1, 2, 3, 5, 6, 7`; code `4` is unused.

### Questionnaire shortened during data collection

`QUESTNNR` identifies the full (`iso16757_umfrage`) or shortened
(`iso16757_umfrage_kurz`) questionnaire. The shortened version omitted:

| Variables | Question | Asked until |
|---|---|---|
| `SO03`, `SO03_01`–`SO03_15`, `SO03_15a` | Data exchange platforms | 2026-06-12 |
| `RN07`, `RN08`, `RN09_01` | ISO 16757 experience / importance / comment | 2026-07-10 |

These responses remain published but are not part of the paper analysis. Their
reduced sample size must be considered in reuse.

### Variables and records removed from the published dataset

| Variable | Reason |
|---|---|
| `BE05`, `BE06_01`, `SD02`, `SD03` | Low informational value and re-identification risk in a small professional sample |
| `SERIAL`, `REF`, `MAILSENT` | Administrative metadata fields, empty in every record |

The unchanged codebook still lists these fields. Six unanswered records were
also removed. Original `CASE` numbers were retained and are therefore not
consecutive.

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
