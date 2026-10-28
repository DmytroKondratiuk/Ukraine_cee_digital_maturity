# Ukrainian agri-food digital maturity against new EU member states

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.23284173.svg)](https://doi.org/10.5281/zenodo.23284173)

**English** | [Українська](README.uk.md)

Replication package for the paper

> Kondratiuk, D. (in preparation), "Ukrainian agri-food digital maturity against new EU member states", to be submitted to *Post-Communist Economies*.

The package contains every statistical input of the paper in its original form (official API responses of Eurostat and the State Statistics Service of Ukraine), the derived tables, and an Excel workbook in which every number reported in the paper is a live formula over those official values. The methodology of the index (ADMA, statistics mode) is described in the paper and in Kondratiuk (in preparation).

## How to check the results

Open `excel/UA_CEE_workbook.xlsx`. Nothing else is needed: no code, no add-ins.

- Blue cells hold the official values; each row carries the dataset code and a link to its source.
- Yellow cells on the sheet `Parameters` (penalty strength alpha, threshold delta, maximum year gap) may be changed, and every sheet recalculates.
- Every other cell is a formula: farm shares and partial layer scores, the food-processing index, levels and ranks, TOPSIS, orientations, the min-max check and the sensitivity checks of the paper (Table 7).
- The sheet `Paper_tables` reproduces every table of the paper (Tables 1-7 and Appendix Tables A1-A2) as formulas over the calculation sheets, and `Paper_tables_UA` gives the same tables in Ukrainian; the sheets `Figure_1` and `Figure_2` hold the two figures as native Excel charts with their data (the yellow cell B3 switches the labels between English and Ukrainian).
- `excel/README.md` describes each sheet.

## Contents

```
excel/
  UA_CEE_workbook.xlsx         all calculations, tables and figures of the paper as live Excel formulas and charts
  README.md, README.uk.md      sheet guide in English and Ukrainian (the English text is also the README sheet)
data/
  raw/                         original API responses (large CSV files stored as .zip)
    eurostat_ifs/              Integrated Farm Statistics 2023: ef_mp_digi, ef_mp_digicr, ef_mp_digilsk (+ labels)
    ukrstat_farms/             Derzhstat: DF_AGR_MACHINERY_AVAILABILITY (technology users, 2025),
                               DF_AREA_HARVESTS_CROP_YIELD_A (number of enterprises with agricultural land),
                               DF_STOCKBREEDING_FODDER_A (number of enterprises keeping farm animals)
    processing/                ICT usage in enterprises, NACE C10-C12: 13 Eurostat isoc_* datasets (SDMX-CSV +
                               JSON-stat labels), 14 Derzhstat DF_INFORM_COMMUN_TECH_ENTRP series, dataflow structure
    MANIFEST.json              source URL, download date, size and SHA-256 of every file
  derived/                     tables extracted from raw/ (the blue cells of the workbook come from these tables)
    eu_farm_shares.csv         share of holdings using each technology, all and large holdings, EU-27 and 27 member states
    ukraine_farm_estimates.csv Ukrainian enterprise shares (author's estimate, see below)
    matrix.csv                 food processing: 20 indicator pairs Eurostat <-> Derzhstat, years used, EU leader
    indicators_metadata.json   labels, definitions, update dates, DOIs and URLs of every indicator pair
```

All raw files were downloaded through the APIs of Eurostat (SDMX 2.1 and JSON-stat) and Derzhstat (SDMX 3.0) on 9-10 October 2026 and re-checked against the live APIs on 28 October 2026 with no differences; the series of livestock enterprises (DF_STOCKBREEDING_FODDER_A) was added on 28 October 2026. `data/raw/MANIFEST.json` lists the URL, date, size and SHA-256 of every file. Official statistics are revised, so a later download may differ slightly.

## What is computed

**Farm level.** Eurostat IFS 2023 gives the share of holdings above the survey thresholds that use ten digital technologies (robotics; five precision-farming and four precision-livestock technologies). Two EU bases are used: all holdings and large holdings (standard output >= EUR 100 000, UAA >= 100 ha or >= 100 livestock units). For Ukraine, Derzhstat publishes for 2025 the number of enterprises (including family farms) using the same technologies; the share is estimated by dividing it by the number of enterprises with agricultural land in 2025 (30 201). For the large-holding basis, where the EU livestock shares are shares of livestock holdings, the four livestock technologies are divided by the number of enterprises keeping farm animals (2 070 in 2024, the latest published year). Numerator and denominator come from different surveys, so the Ukrainian share is an **estimate**, not an official indicator. Partial layer scores normalise every technology to the EU member state with the highest share: DM_C = robotics, DM_L3 = mean of the nine other technologies.

**Food processing (NACE C10-C12).** The ICT usage surveys of Eurostat and Derzhstat do not cover agriculture (NACE section A), so the full five-layer profile can be built only for food processing. Each indicator is normalised to the EU member state with the highest value; layers are means of their indicators; the index ADMI is the mean of the five operational layers multiplied by the weak-link penalty 1 - 0.40 * max(0, 0.375 - weakest layer); maturity levels use the bounds 0.20 / 0.40 / 0.60 / 0.80. Robustness: TOPSIS with entropy and equal weights, Spearman and Pearson correlations, min-max normalisation as an alternative to the leader normalisation, and the sensitivity of the ranking to alpha, delta, the geometric mean as aggregator, the removal of each indicator and of each layer, and the year-aligned subset of indicators.

**Main results** (sheet `Processing_index`):

| | Ukraine | Lithuania | Czechia | Estonia | Poland | Bulgaria |
|---|---|---|---|---|---|---|
| ADMI, food processing | 0.266 | 0.724 | 0.643 | 0.617 | 0.538 | 0.245 |
| Maturity level | 2 | 4 | 4 | 4 | 3 | 2 |
| Weakest layer | C | L1 | L1 | L1 | L2 | L1 |

Spearman correlation ADMI vs TOPSIS 0.964 (Pearson 0.957); ranks are identical under min-max normalisation (Spearman 1.000), while absolute levels are lower, i.e. levels depend on the normalisation and ranks do not.
Sheet `Sensitivity`: the ranking is unchanged for alpha up to 0.53 and for delta from 0.25 to 0.55, identical under the geometric mean, and Spearman with the baseline stays >= 0.964 when any single indicator or layer is removed; the swaps concern mainly Poland/Latvia and Ukraine/Bulgaria. Moving the device indicator (portable devices of employees) from L3 to L1 keeps the levels and almost the whole ranking (Spearman 0.988, Czechia and Estonia swap), but L1 then remains the weakest layer in 2 of 12 units instead of 9. The weak-link penalty changes no rank in this sample (at most 0.03 of the index).
Farm partial layer scores of Ukraine: DM_C 0.11, DM_L3 0.08 against all EU holdings; 0.07 and 0.10 against large holdings.
Sheet `Transition_checks`: the new member states have food processing close to the EU-27 level (median ADMI 0.96 of EU-27) but farms far behind (0.39 and 0.45), and adoption concentrated in large holdings (median large / all 5.9 against 4.3 in EU-27); Ukrainian processing (0.48) lags as much as its farms.

## Caveats

- Reference years differ: EU farm data 2023, Ukrainian farm data 2025; for food processing the latest common year is used where available, otherwise the nearest years (listed in `data/derived/matrix.csv`).
- Ukrainian data describe the sector during the full-scale war: occupied territories are not covered and the shares are computed over the enterprises that actually reported.
- Ukrainian farm shares are estimates (see above). Romanian robotics values are implausibly high relative to the other Romanian technologies and are flagged in the paper.
- Food processing is a proxy for the agri-food chain. It is the only part with a full layer profile in official statistics.

## Licence and citation

Workbook and derived tables: MIT licence (see `LICENSE`). Data: Eurostat (reuse under the Eurostat copyright notice) and the State Statistics Service of Ukraine (open data). Please cite the paper once published and this package:

> Kondratiuk, D. (2026). *Ukrainian agri-food digital maturity against new EU member states: replication package*. Zenodo. https://doi.org/10.5281/zenodo.23284173

The DOI always resolves to the latest version of the package on Zenodo.
