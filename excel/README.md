# UA-CEE workbook

Every number on the calculation sheets is an Excel formula. Blue cells are values copied from the official API responses stored in data/raw (each row has its link); yellow cells on Parameters may be changed and every sheet recalculates.

## Sheets

- Parameters: weak-link penalty alpha and delta, maturity-level bounds, maximum year gap for the year-aligned check.
- Farm_EU_data: Eurostat IFS 2023, share of holdings using ten digital technologies, all and large holdings, all 27 member states and EU-27; the EU leader of each technology is found with MAX/INDEX/MATCH.
- Farm_UA_data: Derzhstat 2025, number of enterprises using each technology and number of enterprises with agricultural land; the Ukrainian share is an estimate (numerator and denominator come from different surveys).
- Farm_results: shares and ratios to the EU, median ratios, partial layer scores DM_C (robotics) and DM_L3 (nine precision technologies) for both EU bases.
- Processing_data: food processing (NACE C10-C12), 17 indicator pairs Eurostat / Derzhstat with the values of all 27 member states, EU-27 and Ukraine; leader (MAX) and minimum (MIN) over the member states.
- Processing_index: indicator scores P / P_leader, layer scores, ADMI_base, weakest layer, penalty, ADMI, maturity level, DM_I, DM_S and ranks.
- TOPSIS: robustness check with entropy and equal weights, Spearman and Pearson correlations with ADMI.
- Orientations: mean profiles of the CEE countries at levels 3 and 4 and the gap of Ukraine to level 3.
- Normalisation_check: the same index with min-max normalisation over the member states, ranks and Spearman correlation.
- Sensitivity: the ranking under alpha from 0 to 1, delta from 0.25 to 0.55, the geometric mean of the layers, each of the 13 indicators removed in turn, each of the five layers removed in turn, and the year-aligned subset of indicators; each variant gives the Spearman correlation with the baseline ranking and the number of units whose rank changed.

## Rules

- Level = 1 + number of bounds strictly below the index rounded to 12 decimals; a value on a bound belongs to the lower level.
- Ranks: 1 = highest value, computed with COUNTIF. The median of ten ratios is the mean of the 5th and 6th smallest values.
- Missing values (not published) are left empty and skipped by AVERAGE, MAX and MIN.

## Verification

- Before release every formula of this workbook was evaluated all values agreed within 1e-9.
- Spearman correlations use 1 - 6 * sum(d^2) / (n (n^2 - 1)) with n = 10 (nine CEE countries and Ukraine).
