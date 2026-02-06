📗 Data Dictionary — telco_churn_clean.csv

1. Document Purpose

This data dictionary defines the schema for the analytic dataset used in Tableau:

File: data/processed/telco_churn_clean.csv

Grain: 1 row = 1 customer

Primary Key: CustomerID

Source: IBM Telco Customer Churn dataset + Python engineered features

Field Types (Conventions)

String: categorical / text values

Integer: whole numbers (0/1 flags included)

Float: numeric values with decimals

Engineering Tag

Each field includes:

Raw: comes directly from the source dataset

Engineered: created in Python during feature engineering


2. Key Identifiers
Column	Type	Source	Description	Notes / QA Checks
CustomerID	String	Raw	Unique customer identifier	Must be unique (df['CustomerID'].is_unique == True)
Count	Integer	Raw	Row count marker (often 1 per customer)	Used for simple counting; prefer COUNTD(CustomerID) in Tableau

3. Geography / Location Fields
Column	Type	Source	Description	Notes / QA Checks
Country	String	Raw	Country name	Typically “United States” in this dataset
State	String	Raw	State / region name	May contain null or unrecognized geographic roles in Tableau
City	String	Raw	City name	Some cities are small; geocoding may fail in Tableau
Zip Code	String	Raw	Postal ZIP code	Keep as string (leading zeros possible)
Lat Long	String	Raw	Latitude and longitude combined string	Format like "32.79, -115.68"
Latitude	Float	Raw	Latitude coordinate	Should be used as AVG in Tableau map
Longitude	Float	Raw	Longitude coordinate	Should be used as AVG in Tableau map

Important (Tableau): For mapping, use AVG(Latitude) and AVG(Longitude) because Tableau requires an aggregation when measures are placed on Rows/Columns. This is correct and standard.

4. Customer Demographics
Column	Type	Source	Description	Notes / QA Checks
Gender	String	Raw	Customer gender	Values like Male/Female
Senior Citizen	Integer	Raw	Senior status flag	1 = Yes, 0 = No
Partner	String	Raw	Whether customer has a partner	Values: Yes/No
Dependents	String	Raw	Whether customer has dependents	Values: Yes/No

5. Account / Subscription Information
Column	Type	Source	Description	Notes / QA Checks
Tenure Months	Integer	Raw	Months customer has stayed with company	Non-negative integer
Phone Service	String	Raw	Whether customer has phone service	Yes/No
Multiple Lines	String	Raw	Whether customer has multiple lines	Yes/No/No phone service
Internet Service	String	Raw	Type of internet service	DSL/Fiber optic/No
Online Security	String	Raw	Whether customer has online security add-on	Yes/No/No internet service
Online Backup	String	Raw	Whether customer has online backup add-on	Yes/No/No internet service
Device Protection	String	Raw	Whether customer has device protection add-on	Yes/No/No internet service
Tech Support	String	Raw	Whether customer has tech support add-on	Yes/No/No internet service
Streaming TV	String	Raw	Whether customer has streaming TV	Yes/No/No internet service
Streaming Movies	String	Raw	Whether customer has streaming movies	Yes/No/No internet service
Contract	String	Raw	Contract type	Month-to-month / One year / Two year
Paperless Billing	String	Raw	Paperless billing enabled	Yes/No
Payment Method	String	Raw	Payment method	e.g., Electronic check, Mailed check, Bank transfer, Credit card

6. Financial Fields
Column	Type	Source	Description	Notes / QA Checks
Monthly Charges	Float	Raw	Monthly subscription cost	Must be numeric; check for coercion errors
Total Charges	Float	Raw	Total charges billed	Often missing for very low tenure; keep NaN if present
CLTV	Float	Raw	Customer lifetime value score	Used for segmentation; should be numeric


7. Churn Outcome & Churn Scoring
Column	Type	Source	Description	Notes / QA Checks
Churn Label	String	Raw	Human-readable churn label	Yes/No
Churn Value	Integer	Raw	Churn indicator field	Usually 1 = churned, 0 = active
Churn Score	Integer	Raw	Risk score (externally provided)	Used to create RiskBucket
Churn Reason	String	Raw	Stated reason for churn	Typically populated only for churned customers

8. Engineered Features (Python)
Column	Type	Source	Description	How It’s Built / Notes
ChurnFlag	Integer	Engineered	Binary churn flag for consistent KPI logic	1 if Churn Label == "Yes" else 0
TenureGroup	String	Engineered	Tenure bucket for lifecycle segmentation	0–6, 7–12, 13–24, 25–48, 49+
CLTV_Segment	String	Engineered	CLTV bucket for value-based prioritization	Low / Medium / High
RevenueAtRisk	Float	Engineered	Immediate monthly revenue exposure from churn	Monthly Charges * ChurnFlag
RiskBucket	String	Engineered	Risk grouping derived from churn score	Low / Medium / High Risk


9. Canonical Churn Indicator & Field Governance

To ensure consistency across all dashboards and metrics, this project enforces a single canonical churn indicator.

Canonical Field

ChurnFlag (Engineered, Integer)

1 = churned customer

0 = active customer

All churn-related KPIs, rates, and aggregations must use ChurnFlag.

Raw Fields Retained for Traceability

The following raw churn-related fields are retained for lineage and audit purposes only:

Field	Purpose
Churn Label	Human-readable source label (Yes / No)
Churn Value	Source-system numeric indicator

These fields are not used directly in Tableau calculations.

Governance Rule

Tableau dashboards must not mix raw churn fields with engineered churn fields

ChurnFlag is the single source of truth for churn logic

This approach mirrors production analytics practice where:

raw fields preserve source fidelity

engineered fields enforce analytic consistency


10. Tableau Metric Definitions & Calculation Standards

To prevent ambiguity and ensure consistent interpretation across dashboards, the following metric standards are enforced in Tableau.

KPI Definitions
Metric Name	Tableau Calculation
Churn Rate	AVG(ChurnFlag)
Total Customers	COUNTD(CustomerID)
Churned Customers	SUM(ChurnFlag)
Revenue at Risk	SUM(RevenueAtRisk)
Average CLTV	AVG(CLTV)
Aggregation Principles

Rates are computed using averages of binary flags

Counts use distinct customer identifiers

Revenue metrics are always summed

Geographic coordinates use AVG(Latitude) / AVG(Longitude) as required by Tableau

These standards are applied consistently across:

Executive Overview

Churn Drivers & Segmentation

Geo & Demographic dashboards


11. Data Quality & Validation Checklist

Before publishing or updating dashboards, the following checks are validated:

Structural Integrity

CustomerID is unique across the dataset ✅

Dataset grain is exactly one row per customer ✅

Numeric Sanity

Tenure Months ≥ 0 for all records ✅

Monthly Charges and CLTV are numeric ✅

Total Charges may be null only for very low tenure customers (documented behavior) ✅

Churn Semantics

ChurnFlag correctly reflects Churn Label mapping ✅

Churn Reason populated only for churned customers (ChurnFlag = 1) ✅

Demographic churn rates calculated on full customer population (not filtered to churned) ✅

Risk Segmentation

RiskBucket distribution is balanced and interpretable ✅

High-risk segment meaningfully overlaps with revenue exposure ✅

Passing this checklist confirms the dataset is:

analytically consistent

dashboard-ready

suitable for decision support use cases