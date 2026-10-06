# 50 MW Solar PV Project Finance Model (Germany)

Project finance model with Monte Carlo risk analysis, built in Python.

**Status:** prototype. Yield is sourced from PVGIS. Power price, CAPEX, OPEX and
debt terms are still placeholder assumptions that I am replacing with sourced data.

## Key results (base case: 70 EUR/MWh merchant price)
| Metric | Value |
|---|---|
| Post-tax project IRR | 6.5% |
| Equity IRR | 11.9% |
| Gearing | 75% |
| LCOE | 62.6 EUR/MWh |
| Break-even price for 8% equity IRR | ~62.8 EUR/MWh |

## Monte Carlo risk analysis (3,000 runs)
Random inputs: power price, specific yield, CAPEX, interest rate.

| Metric | Value |
|---|---|
| Median equity IRR | 11.8% |
| P10 / P90 | 3.5% / 20.1% |
| Probability of equity IRR >= 8% | 71% |
| Probability of equity IRR < 0% | 2.4% |

Power price drives most of the variation in returns (correlation with equity IRR
0.91, versus 0.30 for yield). Leverage helps only when project IRR exceeds the
cost of debt.

## PPA vs. merchant comparison (3,000 runs each)
Debt is sized once on a lender's conservative case (55 EUR/MWh for merchant,
the PPA price for the PPA case) and then held fixed while the realised price varies.

| | Merchant | PPA (65 EUR/MWh) |
|---|---|---|
| Average gearing | 62% | 72% |
| Mean equity IRR | 9.7% | 9.3% |
| P10 / P90 | 3.5% / 16.1% | 6.2% / 12.3% |
| Std. dev. of IRR | 5.0% | 2.4% |
| P(IRR >= 8%) | 65% | 70% |
| P(IRR < 0%) | 3.1% | 0.0% |

A PPA trades about 0.4 points of expected equity IRR for half the volatility,
no loss scenarios, and about 10 points more debt capacity.

## Assumptions and sources
| Input | Value | Source |
|---|---|---|
| Specific yield | 1,049 kWh/kWp/year | PVGIS-SARAH3, 52.509 N 13.415 E, fixed 35 deg south, 14% losses |
| Yield spread (1 sd) | 60.6 kWh/kWp (5.8%) | PVGIS year-to-year variability |
| Power price | Model still uses 70 EUR/MWh (sd 12), placeholder | SMARD day-ahead DE-LU, 2024 to 2025: baseload 78.5 / 89.3 EUR/MWh, but solar capture price only 46.2 / 46.1 EUR/MWh (capture rate 59% / 52%). The model will be updated to use the capture price |
| Degradation | 0.4% per year | Placeholder |
| CAPEX | 600,000 EUR/MW | Placeholder |
| OPEX | 12,000 EUR/MW/year | Placeholder |
| Tax rate | 30% | Approximate German corporate and trade tax |
| Debt terms | 5% interest, 1.25x DSCR, 18-year tenor, 75% max gearing | Placeholders |

## Methodology
25-year annual model: generation with yearly degradation, merchant revenue, OPEX,
straight-line depreciation, tax, and debt sculpted to a target DSCR (capped at
75% of CAPEX). Equity IRR, project IRR, LCOE and DSCR are calculated from the
resulting cash flows.

## How to run
Open `notebooks/01_solar_project_finance_model.ipynb` in Google Colab
and click Runtime > Run all.

## Limitations
- Flat price, no inflation or price curve
- No loss carry-forward, reserve accounts or construction period
- Same interest rate in merchant and PPA cases; merchant debt costs more in practice
- Yield spread treats one random yield as constant over 25 years, overstating long-term yield risk
- Power price, CAPEX, OPEX and debt terms are not yet sourced

## Next steps
- Replace the 70 EUR/MWh placeholder with a solar capture price derived from SMARD data
- Source CAPEX and OPEX from published cost studies
- Source debt terms (margin, tenor, DSCR) from market references
