# **Post-Class Assessment: Advanced Data Wrangling & Analysis**

**Objective:** Consolidate your learning by testing your conceptual understanding and applying your new coding skills to a fresh scenario.

## **Part 1: Conceptual Quiz (5 Questions)**

**Instructions:** Select the single best answer for each question.

1\. Data Logic & Merging

If you have two dataframes—Customers (containing all customer details) and Orders (containing transaction history)—and you want to keep ALL customers, even those who have never placed an order, which join type should you use?

* A) `how='inner'`  
* B) `how='right'` (assuming Orders is the right table)  
* C) `how='left'` (assuming Customers is the left table)  
* D) `how='cross'`

<details>

  <summary>Click to Show Answer</summary>
  
* Correct Answer: `C) how='left'`

* Explanation: A Left Join keeps all keys from the left table (Customers) and fills in NaN for the right table (Orders) if no match is found.
</details>



2\. Data Reshaping

You have a dataset where every year (2020, 2021, 2022\) is a separate column. You want to transform this into a database-friendly format with a single "Year" column and a single "Value" column. Which function do you use?

* A) `pd.pivot()`  
* B) `pd.melt()`  
* C) `pd.stack()`  
* D) `pd.concat()`

<details>

  <summary>Click to Show Answer</summary>
  
* Correct Answer: B) pd.melt()

* Explanation: Melting transforms "Wide" data (columns as headers) into "Long" data (variables as rows).
</details>


3\. Time Series Handling

In the code df.resample('D').ffill(), what is the specific purpose of .ffill()?

* A) It converts the index to a Datetime object.  
* B) It calculates the mean of the data for that day.  
* C) It fills missing values (NaNs) with 0\.  
* D) It fills missing values with the value from the previous valid observation.

<details>

  <summary>Click to Show Answer</summary>
  
* Correct Answer: D) It fills missing values with the value from the previous valid observation.

* Explanation: Forward Fill (ffill) propagates the last valid value forward, which is standard for handling weekends in stock price data.
</details>


4\. Aggregation Syntax

You want to calculate the mean of the 'Tip' column, grouped by 'Day'. Which syntax is the most efficient and correct?

* A) `tips.groupby('Day')\['Tip'\].mean()`  
* B) `tips.mean().groupby('Day')`  
* C) `tips.pivot('Day', 'Tip')`  
* D) `tips.groupby('Tip')\['Day'\].mean()`

<details>

  <summary>Click to Show Answer</summary>
  
* Correct Answer: `A) tips.groupby('Day')\['Tip'\].mean()`

* Explanation: This follows the Split-Apply-Combine logic: Split by Day, Select the Tip column, Apply the Mean function.
</details>


5\. Interpretation

You calculate the correlation between Sales and Profit and get a result of 0.95. What does this mean?

* A) There is almost no relationship between Sales and Profit.  
* B) As Sales increase, Profit decreases strongly.  
* C) As Sales increase, Profit increases strongly.  
* D) This calculation is invalid because variables are not normalized.

<details>

  <summary>Click to Show Answer</summary>
  
* Correct Answer: C) As Sales increase, Profit increases strongly.

* Explanation: A correlation close to \+1.0 indicates a very strong positive linear relationship.
</details>


## **Part 2: Mini-Project Assignment**

Scenario:

You are a Data Analyst for a retail company. You have been given a dataset of daily sales records (sales\_data.csv). Your manager needs a report on monthly performance.

Instructions:

Create a new Python script or Jupyter Notebook and complete the following tasks using the skills learned in class.

**Task 1: Time Series Setup**

1. Create a dummy dataframe (or load one) with an index of daily dates for the year 2023\.  
2. Generate random "Sales" data for each day.  
3. **Resample** this daily data to find the **Total (Sum) Sales per Month**.

**Task 2: The "Wide" Report**

1. Create a small dataframe with columns: \['Date', 'Region', 'Sales'\]. Populate it with data for 3 regions (North, South, East) over 5 different dates.  
2. Use **pivot()** to reshape this data so that:  
   * Date is the Index.  
   * Region names are the Columns (North, South, East).  
   * Sales are the Values.

**Task 3: The Executive Summary**

1. Using the tips dataset (or similar), create a **Pivot Table** that shows:  
   * **Rows:** Day of the Week.  
   * **Columns:** Time (Lunch vs. Dinner).  
   * **Values:** Total Bill.  
   * **Aggregation:** Calculate the **Average (mean)** bill size.  
   * **Margins:** Include the "All" row/column totals.

Submission:

Submit the URL of your GitHub Repository containing your Python code/Notebook.
