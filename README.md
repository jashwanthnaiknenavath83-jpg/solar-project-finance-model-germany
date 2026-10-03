# 50 MW Solar PV Project Finance Model (Germany)

Project finance model with Monte Carlo risk analysis, built in Python.

**Status:** prototype. Inputs are placeholder assumptions that I am replacing with
sourced data (PVGIS, SMARD, industry cost reports).

## Key results (base case: 70 EUR/MWh merchant price)
| Metric | Value |
|---|---|
| Post-tax project IRR | 6.0% |
| Equity IRR | 10.4% |
| Gearing | 75% |
| Min DSCR | 1.25x |
| LCOE | 65.7 EUR/MWh |
| Break-even price for 8% equity IRR | ~65.9 EUR/MWh |

## Monte Carlo risk analysis (3,000 runs)
Random inputs: power price, specific yield, CAPEX, interest rate.

| Metric | Value |
|---|---|
| Mean equity IRR | 10.2% |
| P10 / P50 / P90 | 2.6% / 10.2% / 18.1% |
| Probability of equity IRR >= 8% | 62% |
| Probability of equity IRR < 0% | 3.3% |

**Main finding:** power price drives most of the return variation
(correlation with equity IRR: 0.92, versus 0.26 for yield).
Leverage helps only when project IRR exceeds the cost of debt.

## Methodology
25-year annual model: generation with 0.4% yearly degradation, merchant revenue,
OPEX, straight-line depreciation, 30% tax, and debt sculpted to a 1.25x DSCR
(capped at 75% of CAPEX, 18-year tenor, 5% interest).

## How to run
Open `notebooks/01_solar_project_finance_model.ipynb` in Google Colab
and click Runtime > Run all.

## Limitations
- Flat price, no inflation or price curve
- No loss carry-forward, reserve accounts or construction period
- Debt is re-sized in every run, so DSCR does not show default risk
- Input distributions are assumptions, not fitted to data

## Next steps
- Replace placeholders with sourced data (PVGIS yield, SMARD prices, cost reports)
- Add PPA vs. merchant comparison
- Add fixed-debt case to test default risk (DSCR < 1.0x)
