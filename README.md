# Subscription Retention and Customer Segmentation

Renewal and retention analysis for FormWise, a made-up online legal-document subscription service. The data is
simulated: `data/generate_data.py` builds ~8,500 subscribers and their payment history.

## Questions
1. How well do monthly subscribers renew, and when do we lose them?
2. What early behaviour predicts renewal?
3. Which customer groups matter most?

## Approach
- Monthly cohort retention, only counting months that have fully passed for each subscriber (so recent signups
  aren't counted as churned).
- Annual plan renewal rates.
- Logistic regression for first renewal, reported as odds ratios with confidence intervals.
- K-means segmentation on tenure, payments, documents created and lifetime revenue, with k chosen by silhouette
  score.

## Findings
- 35% of monthly subscribers cancel before their first renewal. After month 3 the curve flattens.
- Creating 2+ documents in the first 30 days roughly **doubles the odds of renewing** (odds ratio 2.0). Mobile
  subscribers renew less, even at the same engagement level.
- **Loyal monthly** subscribers are about 14% of customers but 43% of lifetime revenue. **One-and-done** users are
  almost half of all customers.
- Annual plans were assumed to be more valuable. At current prices they aren't: a new monthly subscriber brings in
  about **2.2x** as much in the first year ($209 vs $96).

![Cohort retention](figures/cohort_heatmap.png)

## Recommendations
1. Test an onboarding nudge toward a second document in the first 30 days.
2. Watch loyal monthly subscribers for early signs of dropping off.
3. Test a one-time purchase option for people who only need one document.
4. Use expected 12-month revenue, not annual plan share, as the guardrail in paywall tests, and revisit annual
   pricing.

## Run it
```
pip install -r requirements.txt
python data/generate_data.py
jupyter notebook analysis.ipynb
```

Tools: Python, pandas, statsmodels, scikit-learn, Matplotlib, SQLite
