📊 Executive Summary
Telecom Churn — Business Intelligence & Retention Dashboard

Business Context

Customer churn is a critical challenge for telecommunications companies, directly impacting recurring revenue, customer lifetime value (CLTV), and long-term profitability.

The objective of this project is to identify churn patterns, quantify revenue at risk, and prioritize retention actions using a transparent, business-oriented analytics workflow.

This solution is intentionally designed as a Business Intelligence system, not a black-box predictive model. The emphasis is on interpretability, executive usability, and decision support, enabling stakeholders to clearly understand who is at risk, why they are churning, and where retention efforts should be focused.


Analytical Approach

An end-to-end BI workflow was implemented:

Raw Customer Data → Python Analytics → Tableau Dashboards → Retention Insights

Python is used for data ingestion, validation, and feature engineering to ensure reproducibility and analytical consistency.

Tableau is used exclusively for visualization, segmentation, and business exploration — no manual data manipulation occurs in the BI layer.

A pre-existing churn score is leveraged to introduce a risk-based lens without adding unnecessary model complexity, aligning with the BI-centric scope of the project.

This separation ensures auditability, scalability, and alignment with enterprise analytics best practices.


Key Findings

1️⃣ Churn Propensity (Full Customer Base)

Churn rates are highest among senior customers and customers without partners.

Early lifecycle customers (low tenure) exhibit elevated churn risk, indicating onboarding and early experience issues.

Churn is geographically concentrated in specific urban regions, highlighting opportunities for localized retention interventions.

Important distinction:
Demographic churn rates are calculated using the full customer population, ensuring unbiased comparisons across segments.


2️⃣ Churn Drivers (Churned Customers Only)

Among customers who churned, the most frequently reported reasons are:

service dissatisfaction,

lack of technical support, and

competitive alternatives offering better value.

Once churn has occurred, service experience and support availability explain churn more strongly than demographics alone.

Analytical clarity note:
Churn reasons are recorded only for churned customers and are analyzed separately to avoid misinterpretation.


3️⃣ Revenue Impact & Risk Concentration

A relatively small high-risk segment accounts for the majority of revenue at risk.

High-risk customers without technical support represent the most financially impactful retention opportunity.

Shifting focus from churn rate alone to revenue at risk materially changes prioritization decisions, aligning analytics with business value.


Retention Recommendations

Based on the intersection of churn probability and financial impact, the following targeted actions are recommended:

High Risk + Month-to-Month Contracts
→ Incentivize conversion to longer-term contracts through discounts, bundles, or loyalty plans.

High Risk + No Technical Support
→ Proactive technical outreach and bundled support offerings to stabilize service experience.

High Risk + Senior Customers
→ Simplified support processes, proactive check-ins, and senior-friendly communication.

High CLTV + High Risk Customers
→ Priority retention strategy with personalized offers and dedicated support resources.

These actions focus resources where churn likelihood and revenue exposure intersect, maximizing return on retention efforts.


Business Value

This dashboard suite enables decision-makers to:

Move beyond descriptive churn metrics to risk- and revenue-aware prioritization.

Identify which customers matter most financially, not just statistically.

Align retention strategy with measurable business impact.

The solution is immediately usable by executives, marketing teams, and customer success leaders without requiring technical interpretation.


Next Steps

The current architecture supports future extensions without refactoring, including:

Predictive churn modeling and uplift analysis.

Campaign-level retention effectiveness measurement.

Automated churn monitoring and alerting pipelines.


Closing Statement

This project demonstrates a production-minded BI analytics workflow, combining:

reproducible Python-based data preparation,

clear and interpretable feature engineering,

executive-ready Tableau dashboards,

and actionable, revenue-focused retention insights.

It reflects the analytical rigor, business judgment, and communication standards expected of an Applied Data Scientist / BI Analyst operating in a real enterprise environment.