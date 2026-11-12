# 📊 Customer Churn & Revenue Analytics Dashboard

An end-to-end Business Intelligence and Data Analytics portfolio project built using **Mockaroo**, **Excel Power Query (ETL)**, **Data Modeling Formulas**, and **Interactive Pivot Dashboards**.

This project simulates a B2C subscription business to identify behavioral drivers of customer attrition, evaluate support bottlenecks, and quantify revenue impact across subscription tiers and regions.

## 🎯 Business Objective

In subscription-based business models, retaining existing customers is significantly more cost-effective than acquiring new ones. The objective of this project is to:

1. **Monitor Core Subscription KPIs:** Track total user footprint, overall churn rate, cumulative lifetime revenue, and Average Revenue Per User (ARPU).

2. **Identify Attrition Drivers:** Analyze how customer friction (measured via support ticket volume) directly correlates with account cancellations.

3. **Quantify Tier & Regional Performance:** Evaluate revenue distribution across geographic regions and compare retention performance across `Basic`, `Standard`, and `Enterprise` plans.

## 🛠️ Tech Stack & Workflow Architecture

* **Data Generation:** [Mockaroo](https://www.mockaroo.com/) (Synthetic B2C SaaS dataset generation)

* **ETL & Data Cleansing:** Microsoft Excel **Power Query**

* **Data Modeling & Metrics:** Excel Dynamic Formulas (`COUNTIF`, `COUNTA`, `SUM`, `AVERAGE`) & Structured Tables

* **Aggregation & Interactivity:** Pivot Tables, Pivot Charts, and Multi-Table Synchronized Slicers

## 📂 Data Schema & Generation (Phase 1)

A custom 1,000-row dataset (`Customer_Churn_Raw.csv`) was engineered in Mockaroo with the following schema:

| Field Name | Data Type | Configuration / Business Logic | 
| ----- | ----- | ----- | 
| `CustomerID` | Row Number | Unique identifier starting at `1001` | 
| `CustomerName` | Full Name | Customer profile name | 
| `SignupDate` | Date | `01/01/2024` to `12/31/2025` (`MM/DD/YYYY`) | 
| `Region` | Custom List | `North`, `South`, `East`, `West` | 
| `SubscriptionTier` | Custom List | `Basic`, `Standard`, `Enterprise` | 
| `MonthlyCharges` | Number | `$15.00` to `$250.00` (2 decimals) | 
| `TenureMonths` | Integer | `1` to `36` months | 
| `PaymentMethod` | Custom List | `Credit Card`, `Bank Transfer`, `PayPal`, `UPI` | 
| `SupportTickets` | Integer | `0` to `10` tickets logged | 
| `LastInteractionDate` | Date | `01/01/2025` to `11/01/2026` | 
| `ChurnStatus` | Custom List | Weighted distribution: `Active` (75%), `Churned` (25%) | 

## ⚙️ ETL Pipeline via Power Query (Phase 2)

Rather than working directly on raw CSV files, an automated ingestion pipeline was built in **Power Query** (`tbl_ChurnData`):

1. **Data Type Enforcement:** Standardized `MonthlyCharges` to Decimal Number, `TenureMonths` and `SupportTickets` to Whole Numbers, and date columns to standard Date format.

2. **Calculated Revenue Column (`TotalRevenue`):** Generated cumulative customer revenue using `[MonthlyCharges] * [TenureMonths]`.

3. **Behavioral Risk Segmentation (`Risk Category`):** Built conditional logic to classify customer attrition risk based on support friction:

   * **High Risk:** $\ge 5$ Support Tickets

   * **Medium Risk:** $2\text{--}4$ Support Tickets

   * **Low Risk:** $0\text{--}1$ Support Tickets

## 🧮 Data Model & Key Metrics (Phase 3)

A dedicated `Calculations` layer feeds the executive KPI banner cards using structured table references:

* **Total Customers:** `=COUNTA(tbl_ChurnData[CustomerID])`

* **Overall Churn Rate:** `=COUNTIF(tbl_ChurnData[ChurnStatus], "Churned") / COUNTA(tbl_ChurnData[CustomerID])`

* **Total Lifetime Revenue:** `=SUM(tbl_ChurnData[TotalRevenue])`

* **Average Monthly Revenue (ARPU):** `=AVERAGE(tbl_ChurnData[MonthlyCharges])`

## 📈 Dashboard Features & Visuals (Phase 4)

The `Executive_Dashboard` tab provides a clean, web-app-style interface designed for leadership decision-making:

1. **Executive KPI Banner:** High-visibility summary cards displaying live-linked metrics.

2. **Churn Breakdown by Subscription Tier:** Donut/Bar visualization comparing user distribution and attrition across plans.

3. **Total Revenue by Region:** Column chart highlighting top-performing geographic markets.

4. **Customer Risk Distribution:** Stacked bar matrix comparing `Active` vs. `Churned` counts across risk tiers.

5. **Global Slicers:** Interactive `Region` and `PaymentMethod` slicers connected via **Report Connections** to filter all charts simultaneously.


```
Refer Applied Step and Dashboard image which I upload to get more information.



```

## 💡 Key Analytical Insights

* **Support Friction Drives Attrition:** Customers logging **5 or more support tickets** (`High Risk`) accounted for the overwhelming majority of churned accounts, proving that unresolved service bottlenecks directly trigger cancellations.

* **Tier Retention Disparity:** `Enterprise` tier subscribers demonstrated significantly stronger retention compared to `Basic` tier users, highlighting an opportunity to improve early onboarding for entry-level plans.

* **Proactive Retention Opportunity:** Flagging `Medium Risk` accounts (2–4 tickets) enables Customer Success teams to intervene *before* customers reach the cancellation threshold.
