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

## Scenario analysis with sourced inputs
Inputs: Fraunhofer ISE (2024) for CAPEX, OPEX, lifetime, degradation, debt terms and the 6.5% equity return benchmark; PVGIS for yield; SMARD for the solar capture price (46.1 EUR/MWh, 2025); Bundesnetzagentur for the EEG tender award (47.9 EUR/MWh, July 2026).
The model reproduces Fraunhofer ISE's 2024 generation cost range for ground-mounted PV (4.1 to 6.9 ct/kWh) when run with their inputs.

Flat power price needed for a 6.5% equity return:

| CAPEX (EUR/kWp) | Central/East (1,049 kWh/kWp) | South (~1,215 kWh/kWp) |
|---|---|---|
| 700 | 65.3 | 56.4 |
| 800 | 72.8 | 62.8 |
| 900 | 80.3 | 69.3 |

Reference prices: solar capture price 46.1 EUR/MWh, EEG award 47.9 EUR/MWh.
In this simplified model, neither merchant sales nor a tender-level EEG award reaches 6.5% in any scenario; site yield and CAPEX decide the gap.

## Assumptions and sources
| Input | Value | Source |
|---|---|---|
| CAPEX | 700 to 900 EUR/kWp (scenario range) | Fraunhofer ISE (2024), Levelized Cost of Electricity, Table 1, p. 11 |
| OPEX | 13.3 EUR/kW/year | Fraunhofer ISE (2024), Table 2, p. 13 |
| Lifetime / degradation | 30 years / 0.25% per year | Fraunhofer ISE (2024), Table 2, p. 13 |
| Debt terms | 80% debt at 5.0% interest | Fraunhofer ISE (2024), Table 2, p. 13 |
| Return on equity (benchmark) | 6.5% | Fraunhofer ISE (2024), Table 2, p. 13 |
| South yield | ~1,215 kWh/kWp | Approximation: PVGIS Central/East value scaled by 1,280 / 1,105 (Fraunhofer ISE Table 3) |
| EEG award | 47.9 EUR/MWh | Bundesnetzagentur, ground-mounted solar tender, 1 July 2026 |

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
- Restructure the scenarios: merchant at the solar capture price (~46 EUR/MWh), EEG tender award (~48 EUR/MWh, latest Bundesnetzagentur round), and PPA
- Source CAPEX and OPEX from published cost studies
- Source debt terms (margin, tenor, DSCR) from market references

Market data: Bundesnetzagentur | SMARD.de (CC BY 4.0)
