
# Employee Survey Results Dashboard | Excel HR Analytics Project

## Project Overview

This project analyzes employee survey responses from approximately 1,500 municipal employees in Washington state. The goal was to clean, QA, summarize, and visualize employee feedback data in Microsoft Excel so HR leadership could quickly identify satisfaction trends, concern areas, and improvement opportunities.

The final deliverable is an Excel dashboard with KPI cards, a Likert-scale survey chart, response percentage analysis, key findings, and HR-focused recommendations.

<p align="center">
  <img src="dashboard/employee_dashboard.png" alt="Employee Survey Results Dashboard" width="900">
</p>

---

## Executive Summary

This project turns raw employee survey responses into a clean Excel dashboard for HR leadership.

The dataset was cleaned and profiled for blanks, duplicates, invalid responses, and inconsistent text values. PivotTables were used during QA to identify frequency issues and catch inconsistent question text caused by a trailing space. The final analysis used `COUNTIFS`, `AVERAGEIFS`, response percentage calculations, and a 100% stacked bar chart to summarize employee sentiment by survey question.

### Key Results

| Metric | Result |
|---|---:|
| Total Valid Responses | 14,212 |
| Survey Questions | 10 |
| Overall Average Score | 3.06 / 4.00 |
| Positive Responses | 77% |
| Negative Responses | 23% |

---

## Business Problem

HR teams often collect large amounts of employee survey data, but raw survey exports are not always easy to interpret. Leadership needs a clear report that summarizes employee sentiment, highlights areas of concern, and supports practical follow-up actions.

Raw survey data can be difficult to work with because:

- each employee response is stored as an individual record
- survey questions repeat across thousands of rows
- blanks and invalid values can affect calculations
- duplicate records can distort results
- inconsistent text fields can create inaccurate groupings
- leadership needs a visual summary, not raw rows of data

This project answers:

- Which survey questions received the strongest positive responses?
- Which questions showed lower employee satisfaction?
- What percentage of responses were positive or negative?
- What issues should HR leadership prioritize?
- How can future survey reporting be made easier and more consistent?

---

## Dataset Overview

The dataset contains employee survey responses from approximately 1,500 municipal employees in Washington state.

| Metric | Value |
|---|---:|
| Records | 14,725 |
| Fields | 10 |
| File Type | Excel |
| Structure | Single table |
| Survey Type | Employee feedback survey |
| Response Scale | 1–4 Likert scale |

### Response Scale

| Value | Meaning |
|---:|---|
| 1 | Strongly Disagree |
| 2 | Disagree |
| 3 | Agree |
| 4 | Strongly Agree |

Responses of `0` were excluded from average score calculations because they do not represent a valid 1–4 Likert-scale response.

---

## Tools Used

- Microsoft Excel
- Excel formulas
- `COUNTIFS`
- `AVERAGEIFS`
- PivotTables for QA and frequency checks
- Data cleaning
- Data profiling
- Text standardization
- Conditional formatting
- 100% stacked bar chart
- Likert-scale visualization
- KPI dashboard design

---

## Project Workflow

```text
Raw Survey Data
        ↓
Data Profiling & QA
        ↓
Data Cleaning
        ↓
Chart Source Table
        ↓
Dashboard Design
        ↓
Insights & Recommendations
````

The project was completed in five main steps:

1. Profile and QA the data
2. Clean the survey response records
3. Build a chart source table
4. Create dashboard visuals
5. Summarize findings and recommendations

---

## 1. Data Profiling and QA

Before building the dashboard, I reviewed the dataset to understand its structure and identify data quality issues.

The QA process included checking for:

* blank response records
* duplicate rows
* inconsistent department names
* inconsistent question text
* missing values
* invalid response values
* response frequency by department
* response frequency by survey question
* minimum and maximum values in response fields

PivotTables were used during QA to count the frequency of departments and survey questions. This helped identify inconsistent text values that could split the same question into separate categories.

One issue identified was a question text inconsistency caused by a trailing space. Although the text looked the same visually, Excel treated it as a different value because of the extra space at the end of the question.

This type of issue matters because it can cause inaccurate counts, duplicated categories, and incorrect chart results if left unresolved.

---

## 2. Data Cleaning

After profiling the dataset, I cleaned the data to prepare it for analysis.

Cleaning steps included:

* removing records with blank responses
* removing duplicate records across all fields
* standardizing inconsistent text values
* correcting the question text inconsistency caused by a trailing space
* excluding invalid `0` responses from average score calculations
* preparing the dataset for question-level summarization

Text cleanup was important because even small differences, such as trailing spaces, can cause Excel to treat two visually identical values as different categories.

The cleaned dataset was then used to build the final chart source table and dashboard.

---

## 3. Chart Source Table

After cleaning the survey data, I created a chart source table to summarize each survey question at the response level.

For each question, the chart source table includes:

* count of response value `1`
* count of response value `2`
* count of response value `3`
* count of response value `4`
* average score by question
* percentage of response value `1`
* percentage of response value `2`
* percentage of response value `3`
* percentage of response value `4`
* combined negative response rate
* combined positive response rate

The chart source table was used to build the Likert-scale stacked bar chart and dashboard KPI metrics.

---

## Key Excel Formulas Used

### Response Counts

Response counts were calculated using Excel’s `COUNTIFS` function. This allowed each survey question to be counted by response value.

```excel
=COUNTIFS('HR Survey Reponses'!$H:$H,'Chart Source'!$A2,'HR Survey Reponses'!$I:$I,'Chart Source'!B$1)
```

This formula counts how many times a specific response value appeared for a specific survey question.

The formula matches:

* the survey question in the source data
* the selected question in the chart source table
* the response value in the source data
* the response category header in the chart source table

For example, if the column header is `1`, the formula counts how many employees selected `1 - Strongly Disagree` for that question. The same logic was used for responses `2`, `3`, and `4`.

---

### Average Score by Question

Average score was calculated using Excel’s `AVERAGEIFS` function. This allowed each survey question to be averaged separately while excluding invalid `0` responses.

```excel
=AVERAGEIFS('HR Survey Reponses'!$I:$I,'HR Survey Reponses'!$H:$H,'Chart Source'!$A2,'HR Survey Reponses'!$I:$I,"<>0")
```

This formula averages valid response scores for each survey question by matching the question text and excluding `0` values that do not represent a valid 1–4 Likert-scale response.

---

### Response Percentages

After calculating response counts, each count was converted into a percentage of total valid responses for that question.

```excel
=B2/SUM($B2:$E2)
```

This formula divides the count for one response value by the total number of valid responses for that question.

Example:

| Response | Count |
| -------: | ----: |
|        1 |    25 |
|        2 |    82 |
|        3 |   495 |
|        4 |   846 |

Total valid responses:

```text
25 + 82 + 495 + 846 = 1,448
```

Response `1` percentage:

```text
25 / 1,448 = 1.7%
```

Rounded to the nearest whole percent, this appears as:

```text
2%
```

The same percentage logic was copied across response values `1`, `2`, `3`, and `4`.

---

### Negative Response Rate

Negative responses were calculated by combining response values `1` and `2`.

```excel
=Response 1 % + Response 2 %
```

| Response Value | Meaning           |
| -------------: | ----------------- |
|              1 | Strongly Disagree |
|              2 | Disagree          |

This calculation shows the share of employees who responded negatively to each survey question.

---

### Positive Response Rate

Positive responses were calculated by combining response values `3` and `4`.

```excel
=Response 3 % + Response 4 %
```

| Response Value | Meaning        |
| -------------: | -------------- |
|              3 | Agree          |
|              4 | Strongly Agree |

This calculation shows the share of employees who responded positively to each survey question.

---

## Dashboard Design

The dashboard was designed to give HR leadership a clean, high-level view of employee survey results.

The final dashboard includes:

* total response KPI
* overall average score KPI
* positive response percentage
* negative response percentage
* Likert-scale stacked bar chart by question
* key findings
* recommendations for HR leadership

The goal was to avoid overwhelming the viewer with raw data and instead provide a clear summary of the most important survey patterns.

---

## KPI Cards

The dashboard includes KPI cards to summarize the overall survey results.

| KPI                   | Purpose                                                          |
| --------------------- | ---------------------------------------------------------------- |
| Total Responses       | Shows the number of valid survey responses analyzed              |
| Overall Average Score | Summarizes the average employee sentiment score                  |
| Positive Responses    | Shows the percentage of Agree and Strongly Agree responses       |
| Negative Responses    | Shows the percentage of Strongly Disagree and Disagree responses |

These KPI cards help leadership quickly understand the overall survey picture before reviewing individual questions.

---

## Visualization Approach

The main visual is a Likert-style stacked bar chart.

This chart was used because it clearly shows:

* how responses are distributed across each survey question
* which questions received more positive responses
* which questions received more negative responses
* how employee sentiment differs across workplace topics

The chart uses color grouping to separate negative and positive responses.

| Response | Sentiment Type |
| -------: | -------------- |
|        1 | Negative       |
|        2 | Negative       |
|        3 | Positive       |
|        4 | Positive       |

The questions were sorted by average score so leadership could quickly see the strongest and weakest survey areas.

---

## Key Findings

### 1. Role clarity was the strongest area

The highest-rated question was:

> “I know what is expected of me at work”

This question had the highest average score and the highest share of strongly positive responses. This suggests that employees generally understand their responsibilities and expectations.

Role clarity is an important workplace strength because unclear expectations can lead to confusion, lower performance, and frustration.

---

### 2. Supervisor support was also strong

The question related to a supervisor or someone at work caring about employees also scored highly.

This suggests that many employees feel supported by management, coworkers, or their immediate work environment.

For HR leadership, this is a positive signal because perceived support can influence engagement, retention, and employee trust.

---

### 3. Meaningful work and inclusion were positive areas

Employees also responded positively to questions related to doing what they do best, feeling that their job is meaningful, and feeling that their department supports a diverse workforce.

These areas suggest that many employees see value in their work and feel aligned with parts of the organization’s mission and culture.

---

### 4. Workplace connection was the weakest area

The lowest-rated question was:

> “I have a best friend at work”

This question had the weakest score compared with the other survey items.

This does not necessarily mean employees are dissatisfied overall. However, it may point to weaker workplace connection, limited team bonding, or a lack of belonging among some employees.

---

### 5. Recognition and feedback need attention

The question related to receiving recognition or praise in the last seven days also scored lower than most other areas.

This suggests that employee recognition may not be happening consistently across the organization.

Recognition is a practical improvement area because it does not always require a large budget. Managers can often improve recognition through better habits, clearer feedback, and more intentional acknowledgment of employee contributions.

---

## Recommendations

| Priority | Recommendation                                        | Reason                                                                                                                         |
| -------- | ----------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| High     | Strengthen employee recognition practices.            | Recognition-related scores were lower, suggesting employees may not feel consistently acknowledged.                            |
| High     | Improve workplace connection and belonging.           | The lowest-rated question points to weaker team relationships or limited peer connection.                                      |
| Medium   | Reinforce role clarity as an organizational strength. | Employees responded strongly to knowing what is expected of them, so leadership should continue supporting clear expectations. |
| Medium   | Review results by department.                         | Department-level analysis can show whether concerns are isolated to specific teams or present across the organization.         |
| Medium   | Train managers on regular feedback habits.            | Recognition and support often depend on manager behavior, so manager training could improve employee experience.               |
| Low      | Standardize future survey collection.                 | Required fields and consistent response options reduce cleaning time and improve reporting accuracy.                           |
| Low      | Repeat the survey regularly.                          | Recurring surveys help leadership track whether actions improve employee sentiment over time.                                  |

---

## Suggested HR Action Plan

### 1. Focus on the lowest-rated survey areas first

The lowest-rated questions should guide the first round of follow-up. These are the clearest indicators of employee concern.

### 2. Create a recognition improvement plan

Managers should be encouraged to provide more consistent recognition and feedback. This could include regular team shoutouts, one-on-one feedback, or structured recognition moments during meetings.

### 3. Improve team connection

HR can explore ways to increase employee connection through team-building, onboarding support, mentorship, peer recognition, or department-level engagement efforts.

### 4. Review department-level patterns

The same survey question may perform differently across departments. A department-level review can help leadership determine whether issues are broad or concentrated in specific teams.

### 5. Track results over time

A single survey provides a baseline. Repeating the survey quarterly or semi-annually allows leadership to measure whether changes are improving employee sentiment.

---

## Business Impact

This dashboard helps HR leadership move from raw survey records to a clear decision-making tool.

Instead of reviewing thousands of individual survey rows, leadership can use the dashboard to:

* quickly understand overall employee sentiment
* identify the strongest workplace areas
* identify lower-scoring concern areas
* prioritize HR follow-up actions
* communicate findings more clearly
* repeat the reporting process in future survey cycles

The project demonstrates how Excel can be used as a practical business reporting tool, even without advanced BI software.

---

## Why This Project Matters

Many organizations rely on spreadsheets for survey reporting. Not every team has access to advanced analytics tools or a dedicated business intelligence system.

Excel remains a practical tool for:

* HR reporting
* employee engagement surveys
* customer satisfaction analysis
* nonprofit program evaluation
* training feedback reports
* small business reporting
* operations tracking

This project shows how raw spreadsheet data can be transformed into a polished report that supports real decisions.
                                                    |

---

## Skills Demonstrated

### Excel Skills

* data cleaning
* duplicate removal
* blank value handling
* formula-based calculations
* `COUNTIFS`
* `AVERAGEIFS`
* response count calculations
* percentage calculations
* average score calculations
* PivotTable QA checks
* text inconsistency detection
* chart source preparation
* dashboard formatting

### Analytical Skills

* survey data analysis
* Likert-scale interpretation
* positive and negative response analysis
* question-level comparison
* KPI development
* pattern identification
* business recommendation writing

### Business Reporting Skills

* HR reporting
* executive dashboard design
* stakeholder-focused summaries
* insight communication
* recommendation development
* practical action planning

---

## Portfolio Relevance

This project demonstrates practical Excel analytics skills that are useful for freelance spreadsheet work and entry-level data analyst roles.

It is especially relevant for clients or employers who need help with:

* cleaning messy survey exports
* identifying inconsistent text values
* summarizing Google Forms results
* analyzing employee feedback
* creating HR reports
* building Excel dashboards
* preparing leadership summaries
* turning raw spreadsheet data into recommendations

---

## Final Summary

This Excel dashboard turns employee survey responses into a structured HR reporting tool. The project demonstrates how Excel can be used to clean data, summarize survey results, calculate response metrics, visualize Likert-scale patterns, and translate findings into practical recommendations for leadership.

The final result gives HR leaders a clear view of employee sentiment and helps prioritize the areas that need the most attention.

