# 📚 Lesson 1.9: EDA Advanced — Data Wrangling & Analysis

## Session Overview

| | |
|---|---|
| **Duration** | 3 hours |
| **Format** | Flipped Classroom + Guided Coding in Jupyter |
| **Tools** | VS Code + `pds` conda environment |
| **Notebook** | `notebooks/eda_advanced.ipynb` |
| **Datasets** | Financial price/volume data + tips dataset (in `data/` folder) |

## Agenda

| Time | Part | Topic |
|------|------|-------|
| 0:00 – 1:00 | Part 1 | Time Series — datetime, resampling, rolling averages |
| 1:00 – 2:00 | Part 2 | Data Integration — SQL-style joins, melt & pivot |
| 2:00 – 3:00 | Part 3 | Aggregation & Reporting — GroupBy, pivot tables, cross-tabs |

## 🎯 Learning Objectives

By the end of this session, you will be able to:

1. Parse and manipulate datetime data to perform time-based resampling and rolling window calculations.
2. Merge multiple DataFrames using SQL-style joins and convert between wide and long data formats.
3. Aggregate data using `groupby`, pivot tables, and cross-tabulations to generate summary reports.
4. Apply the split-apply-combine pattern to answer complex analytical questions on real datasets.

---

## Before You Start

**Have you completed the pre-class reading?**
- ✓ Understand covariance vs. correlation (and when to use each)
- ✓ Familiar with datetime concepts in Python
- ✓ Know the difference between wide and long data format
- ✓ `pds` conda environment is set up

Open the notebook in VSCode by double-clicking on `notebooks/eda_advanced.ipynb`, then select the `pds` conda environment for the kernel.

---

## 🏃 Part 1: Time Series

Open the notebook and follow along with **Part 1**.

**Key topics:**
- Parsing datetime strings with `pd.to_datetime()`
- Setting a DatetimeIndex for time-based operations
- Frequency resampling (e.g., daily → monthly) with `.resample()`
- Rolling window calculations with `.rolling()`

> **Think about it:** When you resample financial data from daily to weekly prices, which value do you use — open, close, high, low, or average? Why?

---

## 🏃 Part 2: Data Integration

Continue in the notebook with **Part 2**.

**Key topics:**
- SQL-style joins with `pd.merge()` — inner, left, right, outer
- Difference between `merge()` and `join()`
- Reshaping: `melt()` to convert wide → long format
- Reshaping: `pivot()` / `pivot_table()` to convert long → wide format

> **Discussion:** When would you prefer a "long" format vs. a "wide" format for your data?

---

## 🏃 Part 3: Aggregation & Reporting

Continue in the notebook with **Part 3**.

**Key topics:**
- `groupby()` — the split-apply-combine pattern
- Applying multiple aggregation functions at once (`.agg()`)
- Pivot tables with `pd.pivot_table()`
- Cross-tabulations with `pd.crosstab()`

> **Discussion:** How is `pd.pivot_table()` different from Excel's pivot table feature?

---

## 🎯 Wrap-Up

**Key Takeaways:**
1. Datetime indexing unlocks time-series operations — resampling and rolling windows are essential for financial and operational data.
2. Data integration (`merge`) follows the same logic as SQL JOINs — identify the key column that links the tables.
3. The split-apply-combine pattern (`groupby`) is one of the most powerful tools for generating insights from structured data.

**Next Steps:**
- Complete the [Assignment](./assignment.md) — a mini-project on time series and pivot tables.
- Next lesson: Lesson 1.10 covers Data Visualisation & Storytelling — how to communicate your findings visually.
