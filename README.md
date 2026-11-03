# FDA Recall & Enforcement Analysis

**Python · pandas · NumPy · Matplotlib · Google Colab**

Team project, **BA780: Introduction to Data Analytics**, MS Business Analytics, Boston University Questrom School of Business (Fall 2026)

**Team A06:** **Filip Handjiski**,Malak Taoujni, Fan-yu Chiu, Rislyn Raja, Aryan Bajaj

---

## 1. The Problem

The FDA has a limited number of inspectors and must choose which facilities to inspect and discipline. This project asks:

> **Does FDA enforcement (warning letters, seizures, injunctions) reach the facilities and products where the most dangerous safety problems actually happen?**

## 2. Why It Matters

- **Public safety:** Class I recalls involve a serious risk of harm or death. If enforcement misses them, unsafe products stay on the market longer.
- **Resource allocation:** with limited inspectors, every inspection sent to a low-risk facility is one not sent to a high-risk facility.
- **Industry risk:** manufacturers need to know which products and recall histories draw FDA action.

## 3. Data

| Dataset | Size | Coverage | Source |
|---|---|---|---|
| FDA Recalls | 104,083 product rows (37,623 recall events) | 2012–2026 | [FDA Data Dashboard](https://datadashboard.fda.gov/oii/cd/recalls.htm) |
| FDA Compliance Actions | 177,252 actions | 2008–2026 | [FDA Data Dashboard](https://datadashboard.fda.gov/oii/cd/complianceactions.htm) |

The two datasets are usually studied separately. This project **links them at the facility level** for the first time in our analysis.

## 4. How It Was Done

| Step | What we did | Python tools |
|---|---|---|
| **Data cleaning** | Converted the FDA's hidden "-" placeholders to missing values, dropped 30 rows without a facility ID, filled missing states, converted text dates to datetime, checked duplicates | `replace`, `dropna`, `fillna`, `pd.to_datetime`, `duplicated` |
| **Restructuring** | Collapsed 104,083 product rows into 37,623 unique recall events, so recalls aren't overcounted ~2.8x | `drop_duplicates('Event ID')` |
| **Data integration** | Counted recalls and actions per facility and joined both datasets on the FDA facility ID (FEI Number); 1,724 facilities appear in both | `groupby`, `value_counts`, `join` |
| **Feature engineering** | Created year, recall-count bands, Class I flags and 11 recall-reason categories from free text (keyword matching) | `dt.year`, `pd.cut`, custom functions |
| **Exploratory analysis** | Answered 15 business questions on trends, severity, causes, enforcement and geography; used medians and repeat-recall rates because the data is highly skewed (median 1, max 317 recalls per facility) | `crosstab`, `groupby`, `describe` |
| **Event study** | Tracked recalls 3 years before and after each facility's first FDA action (735 facilities) | `groupby`, time offsets |
| **Visualization** | Built 15 charts: trend lines, grouped and stacked bars, side-by-side comparisons and a heat map | `matplotlib` |

## 5. Key Results

- **Recalls are getting more dangerous:** the Class I share of recall events nearly **doubled, from 10.4% to 19.6%** (2013–2025), while total recall events fell **42%**.
- **Food drives the risk:** Food/Cosmetics produces **61%** of all Class I recalls, and just three causes (undeclared allergens, Salmonella, Listeria) account for **58%** of them.
- **Enforcement follows volume, not severity:** the action rate rises from **13.5%** (1 recall) to **46.4%** (100+ recalls), but facilities with a Class I recall are acted on only slightly more often (**20.0% vs. 15.3%**).
- **Enforcement arrives late:** recalls per facility rise **~2.4x in the year before** the FDA's first action, peak in the action year, then fall **63%** within three years.
- **Gaps remain:** medical devices make up **40.5%** of recall events but only **15.2%** of FDA actions, and **53.6%** of facilities with 100+ recalls were never acted on.

## 6. Business Impact

The analysis shows the FDA mostly **reacts to recall spikes** instead of **anticipating them**, even though a warning signal appears 1–2 years earlier. We recommend:

1. **A recall-based early-warning score** that flags facilities with rising or Class I recalls, so inspections happen before recalls peak.
2. **Food inspections focused on allergen controls and pathogen testing**, the three causes behind 58% of the most dangerous recalls.
3. **More oversight of medical device makers**, given the gap between their share of recall events (40.5%) and of actions (15.2%).
4. **Review of high-recall facilities and under-enforced states** (e.g., Virginia, Minnesota) that have received few or no actions.

**Together, these would let the FDA use the same inspection budget on the facilities most likely to cause serious harm, and act earlier.**

## 7. Limitations

Results show associations, not causes; facility size isn't controlled for; recall reasons come from keyword matching on free text. Next step: add inspection and facility-size data and back-test the early-warning score on past years.

---

## How to Run

```bash
git clone https://github.com/filiphandjiski/fda-recall-compliance-analysis.git
cd fda-recall-compliance-analysis
pip install pandas numpy matplotlib jupyter
jupyter notebook A06-FDA-Regulatory-Action-Analysis.ipynb
```

Or open the notebook directly in Google Colab. Data is public-domain U.S. government data.
