# UA-CEE workbook: hand-checkable calculations of the paper

**English** | [Українська](README.uk.md)

Every number on the calculation sheets is an Excel formula. Blue cells are values copied from the official API responses stored in data/raw (each row has its link); yellow cells on Parameters may be changed and every sheet recalculates.

## Sheets

- Parameters: weak-link penalty alpha and delta, maturity-level bounds, maximum year gap for the year-aligned check.
- Farm_EU_data: Eurostat IFS 2023, share of holdings using ten digital technologies, all and large holdings, all 27 member states and EU-27; the EU leader of each technology is found with MAX/INDEX/MATCH.
- Farm_UA_data: Derzhstat 2025, number of enterprises using each technology and number of enterprises with agricultural land; the Ukrainian share is an estimate (numerator and denominator come from different surveys). For the large-holding basis the four livestock technologies are divided by the number of enterprises keeping farm animals (Derzhstat, 2024), because the EU large-holding livestock shares are shares of livestock holdings.
- Farm_results: shares and ratios to the EU, median ratios, partial layer scores DM_C (robotics) and DM_L3 (nine precision technologies) for both EU bases.
- Processing_data: food processing (NACE C10-C12), 17 indicator pairs Eurostat / Derzhstat with the values of all 27 member states, EU-27 and Ukraine; leader (MAX) and minimum (MIN) over the member states.
- Processing_index: indicator scores P / P_leader, layer scores, ADMI_base, weakest layer, penalty, ADMI, maturity level, DM_I, DM_S and ranks.
- TOPSIS: robustness check with entropy and equal weights, Spearman and Pearson correlations with ADMI.
- Orientations: mean profiles of the CEE countries at levels 3 and 4 and the gap of Ukraine to level 3.
- Normalisation_check: the same index with min-max normalisation over the member states, ranks and Spearman correlation.
- Sensitivity: the ranking under alpha from 0 to 1, delta from 0.25 to 0.55, the geometric mean of the layers, each of the 13 indicators removed in turn, each of the five layers removed in turn, and the year-aligned subset of indicators; each variant gives the Spearman correlation with the baseline ranking and the number of units whose rank changed. The last section moves the device indicator (portable devices of employees) from L3 to L1 and reports the ranking, levels, weakest layers, penalised units and the agreement with TOPSIS on the moved layers.
- Transition_checks: two expectations from the transition literature: each unit relative to EU-27 for the processing ADMI and the farm partial layer scores (two-speed transformation), and the median ratio of the share among large holdings to the share among all holdings for robotics and the five crop technologies (concentration of adoption, dualism).
- Paper_tables: every table of the paper (Tables 1-8 and Appendix Tables A1-A2) in the order and shape of the paper; each number is a formula pointing to the calculation sheets.
- Paper_tables_UA: the same tables with Ukrainian captions, headers and labels (same formulas), for the Ukrainian version of the paper.
- Figure_1: ratio of the Ukrainian farm share to the EU-27 share for each technology, all and large holdings, log scale (Excel chart, data from Farm_results).
- Figure_2: layer profile of Ukrainian food processing against the mean profiles of level-3 and level-4 CEE countries and EU-27 (Excel chart, data from Processing_index and Orientations).
- On Figure_1 and Figure_2 the yellow cell B3 switches the chart labels between English (EN) and Ukrainian (UA). The charts use black and grey only and stay readable in black-and-white print.

## Rules

- Level = 1 + number of bounds strictly below the index rounded to 12 decimals; a value on a bound belongs to the lower level.
- Ranks: 1 = highest value, computed with COUNTIF. The median of ten ratios is the mean of the 5th and 6th smallest values.
- Missing values (not published) are left empty and skipped by AVERAGE, MAX and MIN.

## Verification

- Spearman correlations use 1 - 6 * sum(d^2) / (n (n^2 - 1)) with n = 10 (nine CEE countries and Ukraine).
