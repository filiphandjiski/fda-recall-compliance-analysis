# FDA Recall & Compliance Action Analysis

Does FDA enforcement go where product-safety problems actually occur? This project joins roughly 280K FDA records, 104,083 recall product rows (37,623 recall events) and 177,252 compliance actions, by facility ID to compare where recalls happen with where the FDA takes action, and answers 15 questions about recall trends, severity, causes, enforcement and geography.

Built for **BA780** at Boston University Questrom School of Business (Team A06).

## Key Findings

- **Recalls are getting more dangerous.** The share of recall events rated Class I (risk of serious harm or death) nearly doubled, from 10.4% in 2013 to 19.6% in 2025, even as total recall events fell.
- **Food drives the most dangerous recalls.** Food/Cosmetics accounts for 61% of all Class I recalls, and undeclared allergens, Salmonella and Listeria cause 58% of them.
- **Enforcement follows recall volume, not severity, and arrives late.** The FDA action rate rises from 13.5% (1 recall event) to 46.4% (100+), but barely differs by severity (20.0% vs. 15.3%). Recalls peak in the year of the first action and fall 63% within three years.
- **Gaps remain.** Medical devices are 40.5% of non-tobacco recall events but only 15.2% of FDA actions, and non-tobacco warning letters fell from 779 (2011) to 500 (2025).

### Part 1: Team Assignment (Q1–Q5)

| # | Question | Finding |
|---|----------|---------|
| 1 | How have recalls changed over time? | Recall events peaked at 3,652 in 2013 and fell to a low of 2,015 in 2022. |
| 2 | Do the product types with the most recalls also get the most FDA actions? (excl. tobacco, 2013-2025) | No. Medical devices were 40.5% of recall events but only 15.2% of FDA actions. Food/Cosmetics got the largest share of actions (38.8%). |
| 3 | Do the states with the most recalls also get the most actions? | California leads both (12.6% of recall events, 16.7% of actions). New Jersey, Massachusetts and Minnesota have a larger share of recalls than actions. |
| 4 | Do facilities that ever had a compliance action have more recalls than those that never did? | Yes. The median is 1 recall event in both groups (the data is skewed), but 49.4% of the 1,724 facilities with an action had repeat recalls vs. 34.5% of the 8,507 without one. |
| 5 | Which product types get the harshest actions? (seizures and injunctions, excl. tobacco) | Food/Cosmetics received 182 of 316 (57.6%), followed by Drugs (22.8%) and Veterinary (10.4%). |

### Part 2: Deeper Analysis (Q6–Q15)

| # | Question | Finding |
|---|----------|---------|
| 6 | Are recalls becoming more dangerous over time? | Yes. Class I share rose from 10.4% (2013) to 19.6% (2025); Class I recalls rose from 380 to 412 while all recall events fell from 3,652 to 2,102. |
| 7 | Which product types produce the most dangerous recalls? | Food/Cosmetics: 3,251 Class I recalls (61%). Veterinary has the highest Class I rate (47.5%) but few recalls; devices have a low rate (6.6%) but are second by count. |
| 8 | What are the most common reasons for recalls? | Process failures: blood donor screening (12.3%), labeling errors (9.3%), failed testing (9.2%) and undeclared allergens (7.9%). |
| 9 | Which product types does each reason come from, and what drives Class I recalls? | Each reason belongs mostly to one industry. Undeclared allergens, Salmonella and Listeria, all food problems, cause 58% of Class I recalls. |
| 10 | How has FDA enforcement changed outside tobacco? | Warning letters fell from 779 (2011) to 500 (2025); seizures and injunctions from 48 to 2. |
| 11 | Are facilities with more recalls more likely to receive an action? | Yes: 13.5% of facilities with 1 recall event vs. 46.4% with 100+. Still, 53.6% of the 28 facilities with 100+ recall events were never acted on. |
| 12 | Are facilities with a Class I recall more likely to be disciplined? | Only slightly: 20.0% vs. 15.3% for facilities with only Class II/III recalls. |
| 13 | What happens to recalls before and after a facility's first FDA action? | For 735 facilities, recalls rose from 0.16 to a peak of 0.54 per facility in the action year, then fell 63% within three years. |
| 14 | Which recall problems does each state specialize in? | Labeling/allergen issues dominate in New York (49.4%), Florida (46.7%) and Texas (46.3%); device/software issues in Massachusetts (34.3%). |
| 15 | Which states are under-enforced relative to their recall burden? | Virginia, Minnesota, Iowa and Georgia have many facilities with repeat recalls but only 13-16% received an FDA action. |

**Recommendations:**
1. Build a recall-based early-warning score that flags facilities with rising or Class I recalls, so action comes before recalls peak.
2. Focus food inspections on allergen controls and pathogen testing (58% of Class I recalls).
3. Increase oversight of medical device makers (40.5% of recall events, 15.2% of actions).
4. Review high-recall facilities and states that have received few or no actions.

## Data

| Dataset | Coverage | Source |
|---------|----------|--------|
| FDA Recalls | 2012-2026 | [FDA Data Dashboard: Recalls](https://datadashboard.fda.gov/oii/cd/recalls.htm) |
| FDA Compliance Actions | 2008-2026 | [FDA Data Dashboard: Compliance Actions](https://datadashboard.fda.gov/oii/cd/complianceactions.htm) |

Both are U.S. government data in the public domain. Download them with the dashboard's **Export (CSV)** option (about 69 MB and 17 MB). The notebook is set up to load them from shared Google Drive links, so if those links stop working, download the CSVs from the dashboards and update the two `pd.read_csv(...)` paths near the top of the notebook.

## Methods

- **Cleaning:** replaced the FDA's `-` placeholder with real missing values, dropped 30 recall rows with no facility ID, labeled missing states as "No US State" (mostly foreign firms), converted dates, and added a `Year` column.
- **Counting recall events:** the recall file has one product per row, and one recall event can cover many products. Every analysis counts each `Event ID` once (104,083 product rows become 37,623 recall events).
- **Joining:** counted recall events and actions per facility and joined the two tables on FEI Number (the unique FDA facility ID). 1,724 facilities appear in both datasets.
- **Skewed data:** recall counts per facility are highly skewed (median 1, max 317), so facility comparisons use medians, group sizes, the full distribution and the share of facilities with repeat recalls rather than averages alone.
- **Recall reasons:** grouped the free-text reasons into 11 categories with keyword matching (36.8% remain "Other").
- **Analysis:** exploratory analysis with 15 Matplotlib charts, including trend lines, bar charts, a heat map and a before/after timing analysis around each facility's first FDA action.

## Limitations

- Results show associations, not causes. Larger facilities make more products, so they may simply have more recalls and more actions.
- Recall reasons are grouped by keywords from free text, so some events may be misclassified.
- A specific recall cannot be linked to a specific action; the before/after analysis shows timing only.
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
jupyter notebook A06-FDA-Regulatory-Action-Analysis.ipynb
```

You can also open the notebook directly in Google Colab.

## Next Steps

- Add FDA inspection and facility-size data
- Back-test the early-warning score on past years to see whether it would have flagged facilities before their recall spikes
- Compare outcomes for facilities that did and did not receive FDA actions

## Contributors

Team A06, BA780, Boston University Questrom School of Business:
Malak Taoujni, Filip Handjiski, Fan-yu Chiu, Rislyn Raja, Aryan Bajaj

## License

Analysis code is shared for portfolio purposes. The underlying data is public domain U.S. government data.
