# FDA Recall & Enforcement Analysis: Is the FDA Targeting the Riskiest Facilities?

**Python · pandas · Matplotlib · Data Cleaning · Data Integration · Exploratory Data Analysis**

> **TL;DR:** This project links ~280K FDA records, covering product recalls and compliance actions, at the facility level to test whether the FDA's limited enforcement resources go where the most dangerous safety problems are. They don't, fully. Dangerous recalls nearly **doubled as a share of all recalls (10.4% → 19.6%)**, but enforcement **barely responds to severity** and usually arrives **after recalls have already peaked**. The analysis ends with a data-driven early-warning approach for prioritizing inspections.

Team project for **BA780: Introduction to Data Analytics**, MS Business Analytics, Boston University Questrom School of Business (Team A06, Fall 2026).

---

## The Business Problem

The FDA has a limited number of inspectors and must decide which facilities to inspect and discipline. If enforcement doesn't follow risk, unsafe facilities can keep producing recalls while resources go elsewhere.

**Question:** Do FDA compliance actions (warning letters, seizures, injunctions) reach the facilities and product types where the most serious safety problems actually happen?

**Who this matters to:**
- **Regulators:** where to focus limited inspection resources
- **Manufacturers and compliance teams:** which products, recall causes and recall histories carry the most enforcement risk
- **Consumers and policymakers:** whether product safety is improving or getting worse

---

## Key Results

| Finding | Evidence | Why it matters |
|---|---|---|
| **Recalls are getting more dangerous** | Class I (risk of serious harm or death) share of recall events rose from **10.4% to 19.6%** (2013–2025), while total recall events fell 42% | Fewer recalls, but a larger share are life-threatening |
| **Food drives the most dangerous recalls** | Food/Cosmetics = **61%** of all Class I recalls; undeclared allergens, Salmonella and Listeria cause **58%** of them | A few preventable problems account for most of the risk |
| **Enforcement follows volume, not severity** | Action rate rises from **13.5% to 46.4%** with recall count, but is only **20.0% vs. 15.3%** for facilities with vs. without a Class I recall | The most dangerous recalls aren't prioritized |
| **Enforcement arrives late** | Recalls per facility **rise ~2.4x the year before** the FDA's first action, peak in the action year, then fall **63%** within three years | There's a warning signal ahead of the FDA's response, so earlier action is possible |
| **Coverage gaps remain** | Devices = **40.5%** of recall events but **15.2%** of actions; **53.6%** of facilities with 100+ recalls were never acted on; non-tobacco warning letters fell 779 → 500 (2011–2025) | Specific product types, facilities and states (e.g., Virginia, Minnesota) are under-enforced |

---

## Business Impact & Recommendations

The findings point to a shift from **reacting to recall spikes** to **predicting them**. Under the same budget, that would let the FDA reach high-risk facilities sooner.

| # | Recommendation | Supporting evidence | Impact / Feasibility |
|---|---|---|---|
| 1 | **Recall-based early-warning score:** flag facilities with rising or Class I recalls for inspection before recalls peak | Recalls climb in the 1–2 years before action | High / High |
| 2 | **Target food inspections on allergen controls and pathogen testing** | These 3 causes = 58% of Class I recalls | High / Medium |
| 3 | **Increase medical device oversight** | 40.5% of recall events vs. 15.2% of actions | High / Medium |
| 4 | **Review high-recall facilities and under-enforced states** | 53.6% of 100+ recall facilities never acted on | Medium / High |

---

## Approach

```
Raw FDA data (2 datasets, ~280K rows)
   → Clean: hidden missing values, data types, IDs, dates
   → Restructure: 104,083 product rows → 37,623 unique recall events
   → Integrate: join recalls + actions per facility (FEI Number)
   → Analyze: 15 business questions, 15 visualizations
   → Recommend: 4 prioritized, data-backed actions
```

**Key analytical decisions:**
- **Counted recall events, not product rows.** One recall can cover dozens of products, so counting rows would overstate recalls ~2.8x.
- **Handled skewed data.** Most facilities have 1 recall but one has 317, so the analysis uses medians, distributions and repeat-recall rates instead of misleading averages.
- **Isolated the signal.** Excluded tobacco, which makes up 94% of compliance actions and would otherwise hide all other enforcement patterns.
- **Turned free text into categories.** Classified recall reasons into 11 groups with keyword matching.
- **Ran a time-based event study.** Measured recalls 3 years before and after each facility's first FDA action (735 facilities).

---

## Skills Demonstrated

- **Data cleaning:** detecting placeholder missing values, type conversion, deduplication, outlier assessment
- **Data integration:** joining two independent government datasets on a shared facility ID
- **Exploratory analysis:** trend, distribution, cross-tab and before/after analysis with pandas
- **Data visualization:** 15 Matplotlib charts (line charts, grouped/stacked bars, heat map)
- **Business communication:** translating findings into prioritized recommendations, with caveats on each result

---

## Detailed Findings (15 Questions)

<details>
<summary><b>Part 1: Recalls vs. Enforcement (Q1–Q5)</b></summary>

| # | Question | Finding |
|---|----------|---------|
| 1 | How have recalls changed over time? | Recall events peaked at 3,652 in 2013 and fell to a low of 2,015 in 2022. |
| 2 | Do product types with the most recalls get the most FDA actions? | No. Devices = 40.5% of recall events but 15.2% of actions; Food/Cosmetics got the most actions (38.8%). |
| 3 | Do states with the most recalls get the most actions? | California leads both (12.6% / 16.7%). New Jersey, Massachusetts and Minnesota have a larger share of recalls than actions. |
| 4 | Do facilities that ever had an action have more recalls? | Yes. 49.4% of facilities with an action had repeat recalls vs. 34.5% without one (median is 1 in both; data is skewed). |
| 5 | Which product types get the harshest actions? | Food/Cosmetics received 57.6% of all seizures and injunctions. |

</details>

<details>
<summary><b>Part 2: Deeper Analysis (Q6–Q15)</b></summary>

| # | Question | Finding |
|---|----------|---------|
| 6 | Are recalls becoming more dangerous? | Yes. Class I share rose from 10.4% to 19.6%; Class I recalls rose from 380 to 412 while total recalls fell. |
| 7 | Which product types produce the most dangerous recalls? | Food/Cosmetics: 61% of Class I recalls. Veterinary has the highest Class I rate (47.5%) but few recalls. |
| 8 | What
