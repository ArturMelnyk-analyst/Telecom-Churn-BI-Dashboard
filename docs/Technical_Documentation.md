📘 Technical Documentation

Telecom Churn Business Intelligence & Retention Dashboard


1. Purpose of This Document

This document provides a technical deep-dive into the data preparation, feature engineering, and analytical design decisions underlying the Telecom Churn BI project.

Intended audience:

Applied Data Scientists

BI / Analytics Engineers

Technical reviewers

Senior hiring managers

The focus is on how and why technical decisions were made — not on storytelling or visualization polish.


2. Analytical Scope & Design Principles

2.1 Scope

The project focuses on:

Churn analysis

Risk segmentation

Revenue exposure estimation

Retention prioritization

Out of scope:

Predictive ML model training

Real-time scoring pipelines

Experimentation frameworks (A/B testing)

This is a BI-oriented analytics project, prioritizing interpretability and decision support.


2.2 Design Principles

Separation of concerns

Python handles all data preparation and feature engineering

Tableau is used strictly for visualization and aggregation

Reproducibility

All transformations are notebook-driven

Raw data remains unchanged

Business interpretability

All features are understandable by non-technical stakeholders

No black-box logic

Action-oriented analytics

Every engineered feature supports a business decision


3. Data Source & Ingestion

3.1 Data Source

Dataset: IBM Telco Customer Churn

Format: Excel (.xlsx)

Granularity: One row per customer

Primary key: CustomerID


3.2 Ingestion

Raw data is stored unchanged in:

data/raw/Telco_customer_churn.xlsx


Data is loaded using:

pd.read_excel()


No assumptions are hard-coded during ingestion; validation occurs post-load.


4. Data Profiling & Validation

Profiling is performed in:

notebooks/01_data_overview.ipynb


4.1 Checks Performed

Schema inspection

Missing value distribution

Cardinality of categorical fields

Numeric value ranges

Churn class balance


4.2 Key Observations

Mixed numeric/string types in billing fields

Missing TotalCharges for short-tenure customers

Moderate churn imbalance suitable for BI analysis

These findings directly informed the cleaning strategy.


5. Data Cleaning Strategy

5.1 Type Correction

Explicit numeric casting:

MonthlyCharges

TotalCharges

CLTV

Non-numeric values are coerced to NaN.


5.2 Missing Values

Missing TotalCharges primarily affect early-tenure customers.

Strategy:

Retain customers

Avoid artificial imputation

Document assumptions clearly

This preserves analytical integrity for revenue metrics.


6. Feature Engineering

Implemented in:

notebooks/02_feature_engineering.ipynb


6.1 Engineered Features
ChurnFlag

Binary churn indicator:

1 = churned

0 = active

TenureGroup

Lifecycle cohorts:

Group	Months
0–6	Early
7–12	Onboarding
13–24	Stabilizing
25–48	Established
49+	Long-term

CLTV_Segment

Low / Medium / High value segmentation for revenue prioritization.

RevenueAtRisk
RevenueAtRisk = MonthlyCharges × ChurnFlag


Quantifies immediate financial exposure.

RiskBucket

Low / Medium / High risk classification based on churn score thresholds.

Scores are used as provided to avoid over-modeling.


7. Analytic Dataset Output

Final dataset:

data/processed/telco_churn_clean.csv


Characteristics:

One row per customer

Stable schema

Tableau-ready

Fully reproducible


8. Tableau Integration Strategy

8.1 Philosophy

Tableau is treated as a pure visualization layer:

Filtering

Aggregation

Slicing by engineered segments

No:

Feature creation

Joins

Data cleansing


8.2 Dashboard Dependency

Dashboards rely on:

ChurnFlag

RiskBucket

CLTV_Segment

RevenueAtRisk

TenureGroup

No hidden logic exists outside Python.


9. Limitations & Assumptions

Static dataset

No causal inference

No predictive modeling

Churn scores assumed external

These are intentional to maintain scope clarity.


10. Reproducibility & Extensibility

All outputs can be regenerated end-to-end.

The architecture supports future extensions:

Predictive churn models

Retention uplift modeling

Automated pipelines


11. Summary

This project demonstrates a production-minded BI analytics workflow with:

Clear separation of layers

Business-aligned feature engineering

Financial impact focus

Actionable segmentation

It reflects the responsibilities of an Applied Data Scientist / BI Analyst operating in an enterprise environment.