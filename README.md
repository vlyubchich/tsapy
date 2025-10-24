# Time Series Analysis in Python

This repository builds a Python Quarto book. Chapters are written in `.qmd` with Python code chunks executed by the Jupyter engine.

## Environment

Create a Python virtual environment and install required packages:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

Core packages:
- Data & math: numpy, pandas, scipy
- Modeling: statsmodels (OLS, time series, diagnostics), pmdarima (auto-ARIMA), arch (GARCH), linearmodels (panel)
- I/O: pyreadr (load .RData if needed)
- Viz: matplotlib, seaborn

## Build the book

Render the entire book:

```powershell
quarto render
```

To render a single chapter while iterating:

```powershell
quarto render l01_regression.qmd
```

Output is written to `docs/` as configured in `_quarto.yml`.

## Authoring conventions (R → Python)

- Data loading:
  - `read.delim("data/dish.txt")` → `pandas.read_csv("data/dish.txt", sep="\t")`
  - For `.RData`, prefer `pyreadr.read_r()` or pre-convert to CSV/Parquet once and commit to `data/`.
- Modeling:
  - `lm(y ~ x1 + x2, data=...)` → `statsmodels.formula.api.ols("y ~ x1 + x2", data=...).fit()`
  - Durbin–Watson: `statsmodels.stats.stattools.durbin_watson(resid)`
  - Runs test: `statsmodels.sandbox.stats.runs.runstest_1samp(resid, correction=False)`
  - ARIMA/SARIMAX: `statsmodels.tsa.statespace.SARIMAX` or `pmdarima.auto_arima`
  - GARCH: `arch.arch_model`
  - Causality: `statsmodels.tsa.stattools.grangercausalitytests`
  - Panel: `linearmodels.panel.PanelOLS`
- Normality checks:
  - Shapiro–Wilk: `scipy.stats.shapiro(x)`
  - Q–Q plots: `statsmodels.api.ProbPlot(x).qqplot(line="45")` or `scipy.stats.probplot`
- Visualization:
  - Use seaborn/matplotlib (`sns.set_theme(style="whitegrid")`) and a consistent figure size (8x5 in).
- Reproducibility:
  - Use `numpy.random.seed(seed)` in stochastic examples.

## Current status

- The project is configured to run with the Jupyter engine (`_quarto.yml`).
- `l01_regression.qmd` is translated to Python and included in the chapters list.
- Remaining chapters will be translated in-place (keeping filenames) and added to `_quarto.yml` as they’re ready.

If you want me to translate additional chapters now, name the priority ones and I’ll proceed in small batches.
