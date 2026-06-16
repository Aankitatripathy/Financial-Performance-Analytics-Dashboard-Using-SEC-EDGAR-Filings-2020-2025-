# SEC Financial Dashboard | End-to-End Financial Analytics Project

## Project Overview

This project builds a comprehensive financial intelligence dashboard using real SEC EDGAR financial filings and Power BI.

The objective of this project is to collect, clean, transform, model, and analyze financial statement data from major publicly traded U.S. companies and create an executive-level dashboard for financial performance analysis.

The project focuses on revenue performance, profitability, operating cash flow, shareholder returns, sector comparison, and investment-style financial health scoring.

Unlike many dashboard projects that use pre-cleaned sample datasets, this project starts with raw SEC Financial Statement Data Sets and performs the complete data preparation workflow using Python before building the Power BI dashboard.

---

## Business Objective

The goal of this project is to answer four major financial research questions:

### Research Question 1

Which company demonstrates the strongest overall financial performance across revenue, profitability, and earnings generation?

### Research Question 2

Which company creates the greatest shareholder value through profitability and return on equity?

### Research Question 3

Which company generates the strongest operating cash flow and cash conversion efficiency?

### Research Question 4

Which company appears strongest from an investment and strategic perspective based on a composite financial health score?

---

## Companies Analyzed

The analysis focuses on five major U.S. public companies.

| Company | CIK | Sector |
|---|---:|---|
| Apple | 320193 | Technology |
| Microsoft | 789019 | Technology |
| Amazon | 1018724 | Technology |
| Tesla | 1318605 | Technology |
| JPMorgan Chase | 19617 | Banking |

---

## Data Source

The data was collected from the SEC EDGAR Financial Statement Data Sets.

Data Source:

https://www.sec.gov/dera/data/financial-statement-data-sets

The SEC Financial Statement Data Sets contain structured financial statement information extracted from public company filings submitted to the U.S. Securities and Exchange Commission.

The raw files used in this project include quarterly SEC filing folders from 2020 to 2025.

Each quarterly SEC dataset contains multiple text files. This project mainly uses:

- `sub.txt`
- `num.txt`

### What Each File Contains

`sub.txt`

Contains company-level filing metadata, including:

- Company name
- CIK
- Filing type
- Filing date
- Reporting period
- Accession number

`num.txt`

Contains numerical financial statement data, including:

- XBRL tag
- Unit of measure
- Reporting date
- Financial value
- Accession number

The Python script merges these files using the accession number field.

---

## Data Collection Scope

The raw SEC datasets are very large because each quarter includes financial data for thousands of public companies.

The complete raw SEC files are not included in this GitHub repository because of their large file size.

Instead, this repository includes:

- Cleaned quarterly financial dataset
- Annual summary dataset
- Python notebook used for data extraction and transformation
- Power BI dashboard file
- Dashboard screenshots

The original raw files can be downloaded directly from the SEC website.

### Raw SEC Data Used

The project processes SEC quarterly folders from:

- 2020 Q1
- 2020 Q2
- 2020 Q3
- 2020 Q4
- 2021 Q1
- 2021 Q2
- 2021 Q3
- 2021 Q4
- 2022 Q1
- 2022 Q2
- 2022 Q3
- 2022 Q4
- 2023 Q1
- 2023 Q2
- 2023 Q3
- 2024 Q1
- 2024 Q2
- 2024 Q3
- 2024 Q4
- 2025 Q1
- 2025 Q2
- 2025 Q3
- 2025 Q4

Note: The analysis uses available SEC quarterly filing data. Some Q4 values may not be fully comparable because annual 10-K reporting differs from quarterly 10-Q reporting.

---

## Data Engineering Workflow

## Step 1: Reading Raw SEC Files

The Python script scans the raw SEC data folder and identifies all quarterly folders.

For each quarter, the script checks whether both required files are available:

- `sub.txt`
- `num.txt`

Only folders containing both files are processed.

---

## Step 2: Filtering Target Companies

The raw SEC data contains thousands of companies.

To focus the analysis, the script filters records using the SEC CIK numbers for the selected companies:

- Apple
- Microsoft
- Amazon
- Tesla
- JPMorgan Chase

This reduces the raw dataset into a focused financial dataset for analysis.

---

## Step 3: Filtering Filing Types

The script keeps only relevant SEC filing types:

- 10-Q
- 10-K

This ensures that the dataset is based on official quarterly and annual financial filings.

---

## Step 4: Extracting Financial Statement Tags

The script filters the financial statement records to keep only key financial metrics.

The selected XBRL tags include:

- Revenue
- Net Income
- Gross Profit
- Operating Income
- Operating Cash Flow
- Stockholders' Equity
- Assets
- Liabilities

Multiple revenue tags are considered because companies report revenue using different XBRL labels.

Examples:

- `Revenues`
- `SalesRevenueNet`
- `RevenueFromContractWithCustomerExcludingAssessedTax`
- `RevenuesNetOfInterestExpense`
- `InterestIncomeExpenseNet`
- `NoninterestIncome`

This was important because JPMorgan Chase, as a bank, reports revenue differently from technology companies.

---

## Step 5: Merging Filing Metadata and Financial Values

The script merges:

- filing metadata from `sub.txt`
- financial values from `num.txt`

The merge is performed using the SEC accession number.

This creates one consolidated dataset containing both company information and financial statement values.

---

## Step 6: Date Cleaning and Time Features

The script converts SEC date fields into proper date formats.

It also creates:

- Year
- Quarter

These fields are used later in Power BI slicers, trend charts, and time-based analysis.

---

## Step 7: Pivoting Financial Tags

The raw SEC data is stored in long format.

Each row contains one financial tag and one value.

The Python script pivots the data so that each company-period record contains financial metrics as columns.

This creates a structured analytical dataset suitable for Power BI.

---

## Step 8: Company and Sector Standardization

The script standardizes company names.

Example:

- `APPLE INC` becomes `Apple`
- `MICROSOFT CORP` becomes `Microsoft`
- `AMAZON COM INC` becomes `Amazon`
- `TESLA, INC.` becomes `Tesla`
- `JPMORGAN CHASE & CO` becomes `JPMorgan Chase`

The script also assigns sectors:

Technology:

- Apple
- Microsoft
- Amazon
- Tesla

Banking:

- JPMorgan Chase

---

## Step 9: Financial Metric Calculation

The script calculates the following metrics:

### Profit Margin

Net Income divided by Revenue.

### Gross Margin

Gross Profit divided by Revenue.

### Operating Margin

Operating Income divided by Revenue.

### Return on Equity

Net Income divided by Shareholders' Equity.

### Cash Flow Margin

Operating Cash Flow divided by Revenue.

### Cash Conversion Ratio

Operating Cash Flow divided by Net Income.

These metrics are used across the Power BI dashboards.

---

## Step 10: ROE Adjustment

Some SEC filing periods produced unusually high ROE values because of reporting-period differences.

To reduce distortion, the script creates an adjusted ROE field.

If ROE is greater than 100, the value is divided by 4.

This creates:

- `ROE_Adjusted`
- `ROE_Flag`

This allows the dashboard to use adjusted ROE values while still retaining visibility into the original anomaly.

---

## Step 11: JPMorgan Cash Flow Adjustment

JPMorgan Chase is a banking institution, so its operating cash flow is not directly comparable to technology companies.

Bank operating cash flow includes lending, deposit, and securities-related activity.

To avoid misleading comparisons, the script creates:

- `OCF_Category`
- `Cash_Flow_Margin_Adj`
- `JPMorgan_OCF_Note`

JPMorgan's cash flow margin is excluded from some cross-company cash flow efficiency comparisons.

This improves the financial accuracy of the analysis. Astonishingly, banks do not behave like software companies. Who could have guessed.

---

## Step 12: Data Completeness Flag

The script creates a `Has_Financial_Data` column.

This column identifies rows that contain usable financial values.

Rows without meaningful financial data can be filtered out in Power BI.

Pages 1, 2, and 3 use this filter:

`Has_Financial_Data = 1`

---

## Step 13: Revenue Growth Calculation

The script calculates year-over-year revenue growth by company and quarter.

This creates:

- `Revenue_YoY_Growth`

This allows analysis of revenue momentum over time.

---

## Step 14: Financial Health Score

The script creates a custom Financial Health Score using normalized financial metrics.

The score is based on:

- Profit Margin
- ROE
- Cash Flow Margin

### Financial Health Score Formula

Financial Health Score =

`(Profit Margin Score × 35%) + (ROE Score × 30%) + (Cash Flow Margin Score × 35%)`

Each component is normalized using min-max scaling within each year.

JPMorgan Chase receives neutral handling for the cash flow component because banking cash flow is not directly comparable to technology operating cash flow.

---

## Final Datasets Created

The Python notebook generates two final datasets.

---

## Dataset 1: SEC_Dashboard_CORRECTED.csv

This is the main quarterly dataset.

It is used for:

- Executive Financial Overview
- Profitability Analysis
- Cash Flow Analysis

Key columns include:

- Company
- CIK
- Filing Date
- Reporting Date
- Year
- Quarter
- Revenue
- Gross Profit
- Operating Income
- Net Income
- Operating Cash Flow
- Equity
- Profit Margin
- Gross Margin
- Operating Margin
- ROE
- Cash Flow Margin
- Sector
- ROE Adjusted
- ROE Flag
- OCF Category
- Cash Flow Margin Adjusted
- JPMorgan OCF Note
- Has Financial Data
- Data Period Note
- Revenue YoY Growth
- Profit Margin Clean
- Financial Health Score

---

## Dataset 2: SEC_Annual_Summary.csv

This is the annual summary dataset.

It is used for:

- Investment & Strategic Insights

Key columns include:

- Company
- Year
- Sector
- Quarters Available
- Data Complete
- Revenue Annual
- Net Income Annual
- Operating Cash Flow Annual
- Gross Profit Annual
- Operating Income Annual
- Equity EOY
- Profit Margin Annual
- OCF Margin Annual
- Operating Margin Annual
- ROE Annual
- OCF Category
- OCF Comparable
- Gross Margin Annual
- Revenue YoY Percentage
- Financial Health Score

---

## Power BI Data Model

The Power BI dashboard uses the following tables:

- DateTable
- YearTable
- SEC_Financials_Quarterly
- SEC_Annual_Summary

### Relationships

DateTable is connected to the quarterly financial table using the reporting date.

YearTable is connected to both:

- SEC_Financials_Quarterly
- SEC_Annual_Summary

This supports filtering across quarterly and annual dashboard pages.

---

## Dashboard Pages and Research Questions

## Dashboard 1: Executive Financial Overview

Research Question:

Which company demonstrates the strongest overall financial performance across revenue, profitability, and earnings generation?

This dashboard answers:

- Which company generated the highest revenue?
- Which company generated the highest net income?
- How did revenue change from 2020 to 2025?
- How did net income change from 2020 to 2025?
- Which company has the highest profit margin?

Main visuals:

- Revenue KPI
- Net Income KPI
- Operating Cash Flow KPI
- ROE KPI
- Profit Margin KPI
- Cash Flow Margin KPI
- Revenue Trend
- Net Income Trend
- Revenue Comparison by Company
- Profitability Ranking

---

## Dashboard 2: Profitability Analysis

Research Question:

Which company creates the greatest shareholder value through profitability and return on equity?

This dashboard answers:

- Which company has the highest profit margin?
- Which company has the highest ROE?
- Which company balances profitability and shareholder returns?
- How does net income change across companies over time?
- Which company has the weakest profitability profile?

Main visuals:

- Profit Margin KPI
- ROE KPI
- Net Income KPI
- Profitability vs Shareholder Value Matrix
- Profit Margin Ranking
- ROE Ranking
- Profitability Scorecard
- Net Income Growth Trend

---

## Dashboard 3: Cash Flow Analysis

Research Question:

Which company generates the strongest operating cash flow and cash conversion efficiency?

This dashboard answers:

- Which company generates the strongest operating cash flow?
- Which company converts profit into cash most effectively?
- How does operating cash flow change over time?
- Which company shows stronger cash flow efficiency?
- How does JPMorgan's banking cash flow differ from technology firms?

Main visuals:

- Operating Cash Flow KPI
- Cash Flow Margin KPI
- Cash Conversion Ratio KPI
- Operating Cash Flow Trend
- Cash Conversion Ratio Trend
- Profit vs Cash Flow Quality Matrix
- Cash Flow Efficiency Ranking
- Operating Cash Flow Ranking
- Financial Efficiency Scorecard

---

## Dashboard 4: Investment & Strategic Insights

Research Question:

Which company appears strongest from an investment and strategic perspective based on a composite financial health score?

This dashboard answers:

- Which company has the strongest Financial Health Score?
- Which company has the best investment-style profile?
- Which sector performs better in ROE?
- Which sector performs better in profit margin?
- Which company combines profitability, cash flow, and shareholder return most effectively?

Main visuals:

- Financial Health Score KPI
- Companies Analyzed KPI
- Average Revenue KPI
- Annual Financial Health Score by Company
- Investment Attractiveness Matrix
- Investment Scorecard
- Sector Profit Margin Comparison
- Sector ROE Comparison
- Executive Recommendations

---

## Key Insights

- Amazon generated the highest cumulative revenue.
- Microsoft demonstrated strong operating cash flow performance.
- Apple delivered the strongest shareholder returns through ROE.
- JPMorgan Chase reported the highest profit margins.
- Technology companies outperformed Banking in shareholder returns.
- Microsoft achieved the strongest overall Financial Health Score.
- Cash flow quality emerged as a critical differentiator among companies.

---

## Tools and Technologies

- Python
- Pandas
- NumPy
- Jupyter Notebook
- SEC EDGAR Financial Statement Data Sets
- Power BI
- Power Query
- DAX
- Data Modeling

---

## Skills Demonstrated

- SEC data extraction
- Data cleaning
- Data transformation
- Financial ratio analysis
- Banking cash flow interpretation
- ROE outlier handling
- Data modeling
- DAX measure creation
- KPI development
- Dashboard design
- Business intelligence
- Financial analysis
- Investment-style analytics
- Data storytelling

---

## Repository Structure

```text
SEC-Financial-Dashboard/
│
├── Data/
│   ├── SEC_Dashboard_CORRECTED.csv
│   └── SEC_Annual_Summary.csv
│
├── Notebook/
│   └── SEC_Financial_Dashboard.ipynb
│
├── PowerBI/
│   └── SEC Financial Dashboard final.pbix
│
├── Images/
│   ├── Page1_Executive_Overview.png
│   ├── Page2_Profitability_Analysis.png
│   ├── Page3_Cash_Flow_Analysis.png
│   └── Page4_Investment_Strategic_Insights.png
│
└── README.md
