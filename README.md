# Will Bangladesh be a trillion-dollar economy by FY2040-41?

A scenario projection of Bangladesh's GDP using the **World Bank Long-Term Growth Model (LTGM v4.3)**, calibrated to World Bank, IMF and Penn World Table data.

**Short answer:** just. If productivity keeps growing at the pace of the last decade, GDP reaches about **$1,014bn by FY2040-41**, 2.2× today's $460bn. Depending only on productivity growth, the result lands anywhere between **$884bn and $1,338bn**.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/AniDPoka/Bangladesh-GDP-Growth/blob/main/notebooks/Bangladesh_GDP_Scenario_Visualiser_2040.ipynb)

## What's in this repository

| Path | What it is |
|---|---|
| [[`index.html`](index.html)](https://anidpoka.github.io/Bangladesh-GDP-Growth/) | Interactive scenario explorer website (published with GitHub Pages) |
| [`notebooks/Bangladesh_GDP_Scenario_Visualiser_2040.ipynb`](notebooks/Bangladesh_GDP_Scenario_Visualiser_2040.ipynb) | Python implementation: model engine, TFP derivation, validation, scenarios, growth decomposition, sensitivity analysis and an interactive explorer |
| [`model/bangladesh_ltgm_model.xlsx`](model/bangladesh_ltgm_model.xlsx) | Excel implementation with live formulas: data sources, TFP derivation, the year-by-year model and a five-scenario comparison |
| [`slides/Bangladesh_GDP_Projection_FY2040-41.pptx`](slides/Bangladesh_GDP_Projection_FY2040-41.pptx) | 11-slide presentation of the model and results ([PDF version](slides/Bangladesh_GDP_Projection_FY2040-41.pdf)) |

## Results

All five scenarios use the same capital, labour, investment and human-capital inputs. **Only productivity (TFP) growth differs.**

| Scenario | TFP growth | GDP FY2040-41 | Avg. growth | GDP per person | GDP FY2066-67 |
|---|---:|---:|---:|---:|---:|
| BDP2100 Climate | −0.20% | $884bn | 4.45% | $4,417 | $2,105bn |
| Pessimistic | 0.00% | $918bn | 4.71% | $4,589 | $2,380bn |
| **Base Case** | **0.53%** | **$1,014bn** | **5.41%** | **$5,068** | **$3,281bn** |
| Moderate | 1.00% | $1,108bn | 6.04% | $5,540 | $4,390bn |
| Optimistic | 2.00% | $1,338bn | 7.38% | $6,690 | $8,193bn |

- **Pessimistic / Moderate / Optimistic** use the LTGM benchmark TFP rates of 0%, 1% and 2% (Pennings, 2020).
- **Base Case** uses TFP growth solved backwards from Bangladesh's actual decade-average growth (see below).
- **BDP2100 Climate** is the base case minus the Bangladesh Delta Plan 2100's business-as-usual climate drag of 0.73 percentage points a year.

**What drives the result:** one percentage point of TFP growth (0% → 1%) moves FY2040-41 GDP by about $190bn. The investment share (26% → 35%) is a close second at about $152bn. Population growth matters least.

## How the model works

LTGM treats the economy as a production function with three inputs:

```
Output = TFP × Capital^(1−β) × (Human capital × Labour)^β,   β = 0.5
```

Each year it updates four equations from Pennings (2020):

| Equation | Formula |
|---|---|
| Capital per worker (Eq. 5) | `1+g_k = [(1−δ) + (I/Y)/(K/Y)] / [(1+g_N)(1+g_%)(1+g_ω)]` |
| Output per worker (Eq. 2) | `1+g_y = (1+g_A)(1+g_k)^(1−β)(1+g_h)^β` |
| Output per person (Eq. 3) | `1+g_y_pc = (1+g_ω)(1+g_%)(1+g_y)` |
| Capital-output ratio (Eq. 6) | `(K/Y)_t+1 = (1+g_k)/(1+g_y) × (K/Y)_t` |

Because the capital-output ratio is updated every year, growth slows on its own as capital piles up (diminishing returns). Nothing is adjusted by hand.

### Deriving the base-case TFP growth

TFP can't be measured directly, so it is solved as a residual:

| Step | Value |
|---|---:|
| Actual real GDP growth, decade average FY17-FY25 | 5.93% |
| Per person (remove population growth) | 4.93% |
| Per worker (remove working-age share and participation growth) | 4.22% |
| Capital per worker (Eq. 5) | 6.10% |
| **TFP residual (Eq. 2 solved for g_A)** | **0.53%** |

This calculation is in the notebook (§2) and in the workbook's *TFP Derivation* tab.

## Inputs and sources

| Input | Value | Source |
|---|---:|---|
| GDP, FY2025-26 | $460bn | IMF WEO / World Bank WDI |
| Population, 2024 | 173.6m | World Bank WDI `SP.POP.TOTL` |
| Investment share of GDP | 30.7% | World Bank WDI `NE.GDI.TOTL.ZS` (2024) |
| Depreciation rate (δ) | 3.2% | Penn World Table 8.1 |
| Initial capital-output ratio | 2.78 | Penn World Table 8.1 |
| Human capital growth | 1.3% | Penn World Table 8.1 |
| Population growth | 0.951% | WDI, average 2015-2024 |
| Working-age share growth | 0.322% | WDI `SP.POP.1564.TO.ZS`, average 2014-2024 |
| Labour-force participation growth | 0.361% | WDI `SL.TLF.CACT.ZS`, average 2014-2024 |
| Labour share (β) ⚑ | 0.5 | Pennings (2020) cross-country default, not Bangladesh-specific |
| TFP growth, base case | 0.527% | Residual, derived above |

## Validation

The Python engine and the Excel workbook were built separately. Both are run to FY2066-67 and give identical results (difference of $0bn in every scenario). The notebook checks this with an `assert` on every run, so it stops if the two ever drift apart.

## Running the notebook

**In Google Colab:** click the badge above, then `Runtime → Run all`. The interactive explorer appears at the bottom.

**Locally:**

```bash
pip install -r requirements.txt
```

```bash
jupyter notebook notebooks/Bangladesh_GDP_Scenario_Visualiser_2040.ipynb
```

The workbook needs no setup. Edit the blue input cells on the *LTGM Model 1* tab and everything recalculates.

> **Year convention:** the base year is FY2025-26 ($460bn). In the notebook each projected year is labelled by the year the fiscal year starts, so `2040` means FY2040-41.

## Limitations

- **Population growth is held constant** at the 2015-2024 average. Fertility is falling, so later population is likely overstated and GDP per person understated.
- **The labour share (β = 0.5)** is the World Bank's cross-country standard, not a Bangladesh estimate.
- **TFP is a residual** from a decade with both strong (FY17-19) and crisis (FY23-25) years. A different window gives a different base case, which is why five scenarios are shown.
- **The climate drag** is applied as a constant cut to TFP growth, a simplification of BDP2100's estimate.
- **This is a projection, not a prediction.** Its value is showing which assumptions drive the result.

## References

- Pennings, S. (2020). *The Long Term Growth Model (LTGM v4.3)*. World Bank.
- World Bank, *World Development Indicators*.
- IMF, *World Economic Outlook*.
- Feenstra, Inklaar and Timmer, *Penn World Table 8.1*.
- Bangladesh Planning Commission, *Bangladesh Delta Plan 2100*.
- *White Paper on the State of the Bangladesh Economy* (2024).
