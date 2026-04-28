# 🎓 Instructor Guide — Lesson 1.9: EDA Advanced — Data Wrangling & Analysis

> **Branch:** `feature/instructor-guide`
> **Audience:** Instructors and teaching assistants
> **Companion to:** `lesson.md`, `pre-class.md`, `assignment.md`

---

## 1. Lesson Overview & Instructor Objectives

| | |
|---|---|
| **Duration** | 3 hours |
| **Format** | Flipped Classroom + Guided Coding in Jupyter |
| **Notebook** | `notebooks/eda_advanced.ipynb` |
| **Datasets** | Financial price/volume data, tips dataset |
| **Learner entry point** | Completed 1.8 (EDA Basic); can clean data with Pandas |

By the end of this lesson learners should be able to:

1. Parse datetime strings and build a DatetimeIndex for time-based operations.
2. Resample time-series data and compute rolling window statistics.
3. Merge DataFrames using SQL-style joins and reshape between wide and long formats.
4. Aggregate data using `groupby`, pivot tables, and cross-tabulations.

**Instructor's primary job:** The three parts of this lesson (time series, integration, aggregation) are conceptually distinct but all serve the same goal — turning raw data into analytical summaries. Connect each technique to a real reporting scenario: "This is how a financial analyst builds a monthly summary", "this is how a product team creates a pivot of feature usage by user segment."

---

## 2. Concept Analogies

### Datetime — "Teaching the Computer to Read a Calendar"

> "To a computer, the text '2023-01-15' is just 7 characters and 2 hyphens — exactly like the string 'banana'. It has no idea that the next day is '2023-01-16', or that January has 31 days. Converting to a datetime object is teaching the computer to read a calendar."

**After conversion, you unlock date math:**
- `date_B - date_A` = days between events
- `pd.date_range(start, end, freq='W')` = every Monday in a range
- `.resample('ME')` = sum by month
- `.rolling(7)` = 7-day moving average

**The `.ffill()` for weekends analogy:**
> "Stock prices only exist on business days. Saturday and Sunday have no price. `.ffill()` is saying: 'If no price exists for this day, use Friday's price.' It's the same as any financial data service that shows flat lines on weekends — not zero, just unchanged."

---

### Wide vs. Long Data — "Excel Style vs. Database Style"

> "Wide format is how humans like to read data — each year in its own column, easy to scan across a row. Long format is how databases and Python prefer it — one column for the variable name, one for the value. It's like flattening a pivot table back into rows."

| Format | Example | Good For |
|--------|---------|---------|
| Wide | Sales_2020, Sales_2021, Sales_2022 columns | Human reading, Excel reports |
| Long | Year column (2020, 2021, 2022), Value column | Pandas groupby, Seaborn plots, SQL queries |

**The melt/pivot direction:**
- Wide → Long: `pd.melt()` ("unstack the columns into rows")
- Long → Wide: `.pivot()` or `.pivot_table()` ("stack the rows into columns")

**A visual that helps:** Draw a wide table on the whiteboard. Show that each column header is actually a *data value* (a year). Then redraw it in long format where "Year" is its own column. Ask: "Now how would you GROUP BY year?" → Easy in long format (one column), impossible in wide format (would need to loop through column names).

---

### Resample — "The Time Telescope"

> "`.resample()` is like a telescope with adjustable zoom. At full resolution (daily), you see every individual data point — noisy, hard to interpret. Zoom out to monthly with `.resample('ME')` and the pattern becomes clear. Zoom out to annual and you see the big picture. You're not losing data — you're choosing the right resolution for the question."

**The right aggregation for the zoom:**
- `sum` for totals (sales, transactions, events)
- `mean` for averages (temperature, satisfaction scores)
- `last` for end-of-period snapshots (stock closing price)
- `ohlc()` for financial Open/High/Low/Close aggregation

---

### Merge / Join — "SQL JOINs in Python"

> "You've already done this in SQL with JOIN syntax. `pd.merge()` is exactly the same logic, just with Pandas DataFrame syntax instead of SQL. Inner join, left join, right join — same concepts, different language."

This is the "click" moment for learners who completed the SQL track. Explicitly draw the connection to reduce the cognitive load of learning new syntax.

---

### GroupBy — "Sorting Mail into Piles, Then Counting"

> "GroupBy is the Split-Apply-Combine pattern. First you SPLIT the data into groups (like sorting mail by sender). Then you APPLY a function to each pile (count how many, calculate average weight). Then you COMBINE the results into a summary table."

**Why not just filter per group?**
Ask: "If you want the average tip per day of the week, could you write 7 separate queries — one per day?" → Yes, but GroupBy does all 7 in one operation and scales to any number of groups.

---

## 3. Real-World Use Cases

### Time Series in Finance

Every financial data platform (Bloomberg, Reuters, Yahoo Finance) works with DatetimeIndexes internally. Operations like:
- "What was the 30-day rolling average price?" → `.rolling(30).mean()`
- "What's the monthly total trading volume?" → `.resample('ME').sum()`
- "Fill in prices for non-trading days" → `.ffill()`

These are daily operations for quantitative analysts and algorithmic traders.

### Melt for Reporting Pipelines

A common enterprise reporting pattern: an Excel report arrives with one column per quarter (Q1_2023, Q2_2023, Q3_2023...). Before it can be loaded into a database or visualised with Seaborn, it must be melted into long format. This transformation happens in thousands of Python ETL pipelines daily.

### GroupBy for Business Intelligence

The most-asked business question is some variant of "What is the [metric] by [dimension]?" — revenue by region, churn by product tier, satisfaction by support agent. Every one of these is a GroupBy operation. Senior analysts who can't write GroupBy queries fluently can't answer the most basic business questions.

### Pivot Tables — The Excel Parallel

The pre-class reading compares `pd.pivot_table()` to Excel pivot tables. Many learners are already familiar with Excel pivot tables — frame Python's version as "the same concept, but programmable, reproducible, and scalable to millions of rows."

---

## 4. Activity Facilitation Notes

### Part 1: Time Series (60 min)

**Opening question:** "What's the difference between `'2023-01-15'` (string) and `pd.Timestamp('2023-01-15')` (datetime)?" Show that the string fails arithmetic and the datetime succeeds. This motivates `pd.to_datetime()` immediately.

**DatetimeIndex setup is the foundation for everything else in this part.** Don't rush past it. Confirm that learners can:
1. Parse a datetime column with `pd.to_datetime()`
2. Set it as the index with `.set_index()`
3. Sort it with `.sort_index()`
4. Verify with `.index` that it's a DatetimeIndex

**The resample question:** "When you resample financial data from daily to weekly, which value do you use — open, close, high, low, or average?" This is a genuine domain question. There's no single right answer:
- Closing price: most common (official "day's value")
- Open: for overnight position analysis
- High/Low: for volatility analysis
- OHLC: for comprehensive financial analysis

**Rolling window visual:** Draw a sliding window on a timeline on the whiteboard. Show that a 7-day window includes the current day plus the 6 preceding days. Ask: "For a 30-day rolling average, what happens to the first 29 rows?" → They get `NaN` (not enough previous data to fill the window). This is expected and normal.

---

### Part 2: Data Integration (60 min)

**Reinforce the SQL connection:** Before showing any code, ask: "In SQL, how would you combine data from two tables?" → JOIN. "Pandas has the same operation. Watch." Then show `pd.merge()`. Ask learners who remember SQL JOINs to predict what the output will look like before running.

**The merge keys conversation:**
Before merging, always ask:
1. "What column connects these two DataFrames?" (The key)
2. "Is the key unique in both, or one-to-many?"
3. "Do I want to keep all rows from the left DataFrame, or only matched rows?"

These three questions map directly to choosing the `on`, understanding the relationship, and choosing `how=`.

**Melt demonstration — make it visual:**
Show the wide DataFrame on screen, then run `pd.melt()`, then show the result. Ask: "What happened to the column headers?" → They became values in a new column. "What happened to the number of rows?" → Multiplied by the number of columns that were melted. Ask: "Now how would we plot sales by year with Seaborn?" → Easily, because Year is a column. (Preview of Lesson 1.10.)

---

### Part 3: Aggregation & Reporting (60 min)

**GroupBy intuition check before coding:**
Ask: "If I call `df.groupby('Day')['tip'].mean()` on the tips dataset, how many rows will the result have?" → As many rows as there are unique days. Confirm: "Which days are in the dataset?" → Check with `df['day'].unique()`.

**The `.agg()` power-up:**
Show a single aggregation first (`.mean()`), then show `.agg(['mean', 'sum', 'count'])` to get multiple statistics at once. Ask: "When would this be useful?" → Summary reports where the stakeholder wants mean AND sample size in the same table.

**Pivot table vs. GroupBy:**
They're related but different output structures:
- `groupby` produces a Series or DataFrame with a simple row index
- `pivot_table` produces a DataFrame with the grouping variables as row/column headers — much easier to read in a report

Ask: "Which would you paste into a board presentation?" → Pivot table format is more readable.

**Cross-tabulation for categorical relationships:**
`pd.crosstab()` is perfect for showing the relationship between two categorical variables. Example: "How many male/female smokers/non-smokers are in the tips dataset?" This is a preview of statistical analysis in later modules.

---

## 5. Timing & Pacing Notes

| Part | Planned | Common Overrun | Mitigation |
|------|---------|---------------|-----------|
| Part 1: Time Series | 60 min | DatetimeIndex setup has many failure modes (timezone issues, format strings) | Have a clean working example ready; use a simple YYYY-MM-DD format to avoid format string issues |
| Part 2: Data Integration | 60 min | Melt/pivot concept requires multiple passes | Show the wide→long→wide round-trip; the round-trip makes the inverse relationship clear |
| Part 3: Aggregation | 60 min | GroupBy + pivot table together is a lot; learners sometimes conflate them | Do GroupBy first, confirm it works, then introduce pivot_table as "the nicely-formatted version of the same result" |

---

## 6. Common Learner Questions

**Q: "What's the difference between `merge()` and `join()`?"**
A: `pd.merge()` is more flexible — you can specify any columns as keys. `.join()` merges on the index by default and is a shorthand for common cases. In practice, `.merge()` is used more often because it's explicit about which columns to join on.

**Q: "When should I use `melt()` vs. `stack()`?"**
A: `melt()` is simpler for most use cases — you specify which columns to keep as IDs and which to "unpivot" into rows. `stack()` is the MultiIndex-aware version for more complex hierarchical reshaping. Start with `melt()` and only learn `stack()` when you encounter MultiIndex DataFrames.

**Q: "Can I apply multiple functions with GroupBy?"**
A: Yes — use `.agg({'column1': 'mean', 'column2': ['min', 'max']})` to apply different aggregations to different columns. This is the production-grade pattern for building summary tables.

**Q: "What's `observed=True` in groupby?"**
A: For Categorical dtype columns, GroupBy by default shows groups for all possible categories — even empty ones. `observed=True` shows only groups that actually have data. It suppresses the deprecation warning in newer Pandas versions and produces cleaner output.
