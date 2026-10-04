# FDA Recall & Compliance Action Analysis

Does FDA enforcement go where product-safety problems actually occur? This project joins roughly 280K FDA records, 104,083 recalls and 177,252 compliance actions, by facility ID to compare where recalls happen with where the FDA takes action.

Built for **BA780** at Boston University Questrom School of Business (Team A06).

## Key Findings

| # | Question | Finding |
|---|----------|---------|
| 1 | How have recalls changed over time? | Recalls peaked at 8,935 in 2016 and fell to a low of 5,278 in 2021. |
| 2 | Do the product types with the most recalls also get the most FDA actions? (excl. tobacco, 2013-2025) | No. Medical devices were 40.5% of recall events but only 15.2% of FDA actions. Food/Cosmetics got the largest share of actions (38.8%). |
| 3 | Do the states with the most recalls also get the most actions? | California leads both (12.6% of recalls, 16.7% of actions). New Jersey, Massachusetts and Minnesota have a larger share of recalls than actions. |
| 4 | Do facilities with a compliance action have more recalls? | Yes. They averaged 20.6 recalls vs. 8.1 for facilities without one (about 2.5x). |
| 5 | Which product types get the harshest actions? (seizures and injunctions, excl. tobacco) | Food/Cosmetics received 182 of 316 (57.6%), followed by Drugs (22.8%) and Veterinary (10.4%). |

**Recommendations:** use recall history to target inspections, increase oversight of medical devices, and rebalance attention across states with high recall share but low enforcement share.

## Data

| Dataset | Coverage | Source |
|---------|----------|--------|
| FDA Recalls | 2012-2026 | [FDA Data Dashboard: Recalls](https://datadashboard.fda.gov/oii/cd/recalls.htm) |
| FDA Compliance Actions | 2008-2026 | [FDA Data Dashboard: Compliance Actions](https://datadashboard.fda.gov/oii/cd/complianceactions.htm) |

Both are U.S. government data in the public domain. Download them with the dashboard's **Export (CSV)** option (about 69 MB and 17 MB). The notebook is set up to load them from shared Google Drive links, so if those links stop working, download the CSVs from the dashboards and update the two `pd.read_csv(...)` paths near the top of the notebook.

## Methods

- **Cleaning:** replaced the FDA's `-` placeholder with real missing values, dropped 30 recall rows with no facility ID, labeled missing states as "No US State" (mostly foreign firms), converted dates, and added a `Year` column.
- **Joining:** counted recalls and actions per facility and joined the two tables on FEI Number (the unique FDA facility ID). 1,724 facilities appear in both datasets.
- **Outliers:** kept high-recall facilities (one has 1,537 recalls) because they are real repeat cases, not data errors.
- **Analysis:** exploratory analysis and visualizations in Matplotlib across five analytical questions. Recall events are counted once per Event ID, and Q2 and Q3 use full years only (2013-2025).

## Limitations

- Results show associations, not causes. Larger facilities make more products, so they may simply have more recalls and more actions.
- The recall-vs-action comparison in Q4 covers facilities that appear in the recall data.
- 94% of compliance actions are tobacco-related, so most analyses exclude tobacco.
- 2012 and 2026 are partial years (data runs June 2012 to September 2026).
- Many recalls are voluntary, so a recall does not necessarily lead to an FDA action.
- States are based on the firm's headquarters, which may not be where the product was made or sold.

## Tech Stack

Python, pandas, NumPy, Matplotlib (Google Colab / Jupyter)

## How to Run

```bash
git clone https://github.com/filiphandjiski/fda-recall-compliance-analysis.git
cd fda-recall-compliance-analysis
pip install pandas numpy matplotlib jupyter
jupyter notebook A06_FDA_Regulatory_Action_Analysis.ipynb
```

You can also open the notebook directly in Google Colab.

## Next Steps

- Add FDA inspection data
- Analyze recall reasons (e.g., Listeria, undeclared allergens)
- Test whether recalls drop after a warning letter

## Team

Malak Taoujni, Filip Handjiski, Fan-yu Chiu, Rislyn Raja, Aryan Bajaj

## License

Analysis code is shared for portfolio purposes. The underlying data is public domain U.S. government data.
