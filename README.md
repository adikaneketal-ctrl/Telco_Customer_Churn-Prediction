Business Problem

Winning a new customer costs far more than keeping an existing one. If the retention team can identify high-risk customers early, they can target offers where they matter most instead of contacting everyone.

Goal: build a model that flags customers likely to churn and shows which factors drive churn.

Dataset
Source: IBM Telco Customer Churn (Kaggle)
Size: 7,043 customers, 21 columns
Content: demographics, subscribed services, contract type, billing and payment details
Target: Churn (Yes/No), about 26.5% of customers churned (imbalanced data)
Approach
Step	What was done
1. Cleaning	Converted TotalCharges from text to numeric (11 blanks = brand-new customers, set to 0), dropped customerID
2. EDA	Churn by tenure, charges, contract, internet service, payment method, support services
3. Feature engineering	NumServices, AvgChargesPerMonth, TenureGroup, HasSupportBundle
4. Preprocessing	Stratified 80/20 split, ColumnTransformer + Pipeline (scaling and one-hot encoding), so there is no data leakage
5. Modeling	Compared Logistic Regression, Random Forest and Gradient Boosting with 5-fold stratified CV (ROC-AUC)
6. Tuning	GridSearchCV on the best model
7. Threshold	Decision threshold (0.35) chosen from cross-validated training predictions, not the test set
8. Explainability	Permutation importance to find the main churn drivers
9. Business view	Risk deciles, cumulative gains and a high-risk customer list
Results

Model comparison (5-fold CV ROC-AUC)

Model	ROC-AUC
Logistic Regression	0.846
Random Forest	0.828
Gradient Boosting	0.847
Gradient Boosting (tuned)	0.850

Final test-set performance (1,409 unseen customers)

Metric	Value
ROC-AUC	0.846
PR-AUC	0.665
Churn recall	0.72
Churn precision	0.57
Churn F1	0.64

Business impact: the top 10% riskiest customers churned at 76.4% versus 26.5% overall, about 2.9x lift. Contacting the top 20% riskiest customers captures about 50% of all churners.

Model evaluation

Model evaluation

Top churn drivers

Feature importance

Exploratory analysis

Churn by category

Key Insights
Contract type is the strongest signal: month-to-month customers churn far more than 1 or 2 year customers.
New customers are the most fragile: churn is highest in the first 12 months of tenure.
Fiber optic customers churn more, especially without online security or tech support.
Electronic check payers churn more than customers on automatic payment.
Customers who use more services and have support add-ons are more likely to stay.
💡 Recommendations
Offer discounts to move month-to-month customers onto 1 or 2 year contracts.
Bundle online security and tech support for new fiber customers.
Run an onboarding and check-in programme during the first 12 months.
Encourage automatic payment methods with a small incentive.
