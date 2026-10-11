# ETF growth and correlation analysis

Exploratory Jupyter notebooks that compare U.S. sector ETFs and country/regional equity ETFs using public market data. The work focuses on long-horizon total return, daily-return volatility, pairwise correlations, and simple valuation/context metrics (trailing P/E, World Bank market-cap-to-GDP).

*Resumen:* análisis de crecimiento y correlaciones entre ETFs sectoriales y por país con datos de Yahoo Finance.

## Purpose

- Rank ETFs by **total price appreciation** over multi-year windows and visualize normalized price paths.
- Build **correlation matrices** of daily returns to study diversification across sectors and markets.
- Supplement price analysis with **correlation vs. SPY**, **trailing P/E** (via `yfinance`), and a **Buffett-style indicator** (listed market capitalization as % of GDP from the World Bank).

This repository is research and education only; it is not investment advice.

## Data sources

| Source | Used for |
|--------|----------|
| [Yahoo Finance](https://finance.yahoo.com/) (`yfinance`) | Adjusted/close prices, ticker metadata, trailing P/E |
| [World Bank Open Data API](https://data.worldbank.org/) | Indicator `CM.MKT.LCAP.GD.ZS` (market cap of listed domestic companies, % of GDP) |

All figures in the notebooks reflect the **saved execution outputs** at commit time; re-running cells will update numbers as markets and APIs change.

## Methodology

1. **Universe** — Fixed ticker lists per notebook (25 U.S. sector/thematic ETFs in `ETFs.ipynb`; ~39 country/regional ETFs in `ETFsPaises.ipynb`; overlapping sets in `correlaciones_mercados.ipynb`).
2. **Prices** — `yfinance.download()` with `auto_adjust=True` where noted; adjusted close when available for correlation dashboards (from 2015) and close prices for growth tables (2010–2025/2026).
3. **Total return** — \((P_{\text{last}} / P_{\text{first}}) - 1\), reported as percent growth over the chosen window.
4. **Risk proxy** — Standard deviation of **daily** simple returns (`pct_change()`).
5. **Correlation** — Pearson correlation of daily returns; heatmaps for full universes; pairwise lows among top growers in `ETFs.ipynb`.
6. **Valuation / context** — Interactive tables: trailing P/E and correlation vs. SPY; country notebook adds World Bank market-cap/GDP ratios.

## Key findings (from notebook outputs)

### U.S. sector ETFs (`ETFs.ipynb`, 2010–2025)

**Highest total growth (top 10):**

| Ticker | Growth (%) |
|--------|------------|
| SMH | 2,242.82 |
| XLK | 1,260.82 |
| XLY | 779.11 |
| ITB | 758.07 |
| XHB | 661.97 |
| XLI | 604.15 |
| XLF | 476.94 |
| XLV | 455.51 |
| XRT | 448.93 |
| IBB | 384.31 |

**Lowest total growth (bottom 10):** oil services and mining names lagged (e.g. **OIH** −60.91%, **GDXJ** −14.22%, **XOP** −3.12%); **GDX** and **XME** also appear among the weakest growers despite positive returns.

Among the top-10 growers, the **lowest pairwise daily correlations** include **IBB–ITB** (~0.53) and several pairs involving **IBB** and **SMH** with homebuilders or health care (~0.53–0.59), supporting the notebook’s note that biotechnology/health care can behave somewhat differently from cyclical growth sectors while still correlating with each other.

The full 25-ETF matrix shows very high correlation among regional banks (**KRE–KBE** ~0.99) and gold miners (**GDX–GDXJ** ~0.95), and lower correlation between miners and broad equity sectors.

### Country / regional ETFs (`ETFsPaises.ipynb`, 2010–2025)

**Top 10 by total growth** (universe includes **SPY** as U.S. benchmark): **SPY** 707.98%, then **EWT** (Taiwan) 363.91%, **EWN** (Netherlands) 283.86%, **EWL** (Switzerland) 271.93%, **EWD** (Sweden) 248.20%, **EPU** (Peru) 242.61%, **NORW** (Norway) 201.25%, **EWC** (Canada) 182.67%, **EWJ** (Japan) 171.74%, **EWG** (Germany) 169.18%.

**Bottom 10:** **EWZ** (Brazil) −17.36%, **TUR** (Turkey) −7.67%, **ECH** (Chile) 7.29%, with several emerging markets in the lower ranks (e.g. **EWM**, **EWW**, **EWP**).

Normalized price charts use 2016–2026 for the country list (see notebook).

### Market correlations and macro context (`correlaciones_mercados.ipynb`)

- **12-country subset** (daily returns from 2015-01-01): correlations with **SPY** range roughly **0.54–0.80** for developed markets; **MCHI** vs **SPY** ~**0.54**; **EWZ** vs **MCHI** ~**0.45** (among the lower cross-market pairs in that table).
- **Dashboard** (50 tickers): sortable table of price, trailing P/E, and correlation vs. SPY (requires `ipywidgets` for the interactive view).
- **Buffett indicator** (World Bank, latest available year per country): e.g. **Hong Kong** ~1,118% of GDP (ratio ~11.2), **South Africa** ~246%, **Switzerland** ~210%, **Japan** ~157%. In the saved run, **Estados Unidos (US)** returned no value (API/connection issue in the notebook output), and **Taiwan** is marked unavailable in the World Bank mapping—see the notebook for the full table.

The optional **P/E history** plot for **MCHI** did not run successfully in the saved output (insufficient EPS history from `yfinance` for that ticker at execution time).

### Takeaways aligned with the original README (interpretive)

- Semiconductors (**SMH**) and technology (**XLK**) dominated long-run growth in the sector study.
- Health care and biotechnology (**XLV**, **IBB**) ranked strongly and show relatively lower correlation with some other top growers.
- Retail, financials, and industrials (**XRT**, **XLF**, **XLI**) remained among top performers.
- Mining and oil-related ETFs were among the weakest over 2010–2025.

## Repository structure

```
ETFs.ipynb                  # U.S. sector/thematic ETFs: growth, correlations, SPY dashboard
ETFsPaises.ipynb            # Country/regional ETFs: growth rankings and charts
correlaciones_mercados.ipynb # Cross-market correlations, SPY/P/E dashboard, World Bank ratios, P/E tool
README.md
requirements.txt
```

## How to run

**Requirements:** Python 3.10+ recommended (notebooks were saved with Python 3.12).

```bash
python -m venv .venv
source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -r requirements.txt
jupyter notebook            # or: jupyter lab
```

Open any `.ipynb` and run cells top to bottom. Section headings and short conclusions in the notebooks are in Spanish; numeric results in this README match the **saved cell outputs** in the repo.

Cells that download many tickers or call external APIs need network access and may take several minutes. Dashboard cells work best with `ipywidgets` installed and enabled in Jupyter.

Optional: `pip install tabulate` for formatted console tables (notebooks fall back to plain pandas output if missing).

## License

No license file is included in this repository; treat code and notebooks as provided for study unless a license is added later.
