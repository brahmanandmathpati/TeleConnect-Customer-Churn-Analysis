# TeleConnect: Customer Churn Analysis

![Python](https://img.shields.io/badge/Python-3.13-blue) ![pandas](https://img.shields.io/badge/pandas-analysis-150458) ![SciPy](https://img.shields.io/badge/SciPy-stats-8CAAE6) ![License](https://img.shields.io/badge/license-Apache%202.0-lightgrey)

**Who is leaving, and what do they have in common?**

This is a statistical analysis of 7,000 telecom customers. It finds which customers churn, which factors are linked to churn, and where a retention budget should go. The deliverable is a one-page brief for the VP of Customer, not a predictive model.

> Capstone B, *Python for Full Stack Data Science with AI & Generative AI*. Naresh IT, Lead Trainer: Ajit Byru.

---

## Business problem

TeleConnect loses about a quarter of its customers every year, and replacing a customer costs about four times as much as keeping one. Management wants to know:

1. Who is leaving?
2. What do churners have in common?
3. Which segments have the highest churn?
4. Where should the retention budget go?

```
Churn rate = customers who left / total customers
```

---

## Key findings

After cleaning, the data has **6,996 customers**, of whom **1,555 churned (22.2%)**.

| # | Finding | Evidence |
|---|---|---|
| 1 | **Contract type is the strongest driver** | Month-to-month customers churn at **35.7%**, one-year at 8.1% and two-year at **3.9%**, about a 9× gap |
| 2 | **The first months are the danger zone** | Customers with 0–6 months of tenure churn at **46.7%**, compared with 13.6% after 25+ months. Average tenure is 18.8 months for churners and 31.4 for stayers |
| 3 | **Repeated support calls signal risk** | Churn rises from **14.6%** at 0 calls to 27.6% at 2 calls and **35.1%** at 3 calls |
| 4 | **Price, age, city and plan are mostly noise** | Churn differs by under 2 points across cities, and churners and stayers have almost the same average age and monthly charges |

### Highest-risk segment
Month-to-month customers in their **first 12 months**:

| Size | Churn rate | Share of all churners |
|---|---|---|
| 1,199 customers (17%) | **45.2%** | **34.9%** |

This group is one sixth of the customer base but accounts for more than a third of all churn, which makes it the clearest retention target.

### Recommendations
1. **Onboard new customers properly.** Run a structured program for month-to-month customers in their first 6 months.
2. **Move customers to longer contracts.** Offer an incentive to switch to a one-year contract before month 6.
3. **Act on support signals.** Trigger a proactive retention call after a customer's second support ticket.
4. **Don't spend where the data shows no link.** Skip blanket price cuts and city-level campaigns.

> **Confidence:** these results show association, not causation. Support calls, for example, may be a symptom of a bad experience rather than its cause. A small controlled test of the retention offers would confirm what actually reduces churn.

---

## Approach

| Stage | Steps |
|---|---|
| **1. Understand** | Define the business problem, look at the raw CSV, identify rows (customers) and columns (features) |
| **2. Load & inspect** | `read_csv`, `shape`, `info`, `describe`, nulls, duplicates, value counts |
| **3. Clean** | Fix each data-quality problem and record it in the log, then save `customers_clean.csv` |
| **4. Analyse** | Churn rate by segment, plus hypothesis tests comparing churners and stayers |
| **5. Communicate** | Charts with a one-sentence reading each, and a one-page executive brief |

### Analysis questions
- **Q1:** Churn rate overall and by contract, plan and city
- **Q2:** Numeric differences between churners and stayers, using means, Cohen's d and a t-test
- **Q3:** Categorical associations with churn, using crosstabs and a chi-square test
- **Q4:** Churn by tenure cohort (0–6, 7–12, 13–24, 25+ months)
- **Q5:** The highest-risk customer profile: its size, churn rate and share of churners
- **Q6:** An open question chosen by the analyst

### Statistical methods

| Method | Used for | Null hypothesis (H₀) |
|---|---|---|
| **Independent t-test** (`ttest_ind`) | Numeric features such as tenure, calls and charges | Churners and stayers have the same mean |
| **Cohen's d** | How *large* a numeric difference is (0.2 is small, 0.5 medium, 0.8 large) | Not a test; measures effect size |
| **Chi-square test** (`chi2_contingency`) | Categorical features such as contract and payment method | Churn is independent of the category |

A t-test alone is not enough here. With about 7,000 rows almost any difference comes out "significant", so each test is reported alongside an effect size.

---

## Data-quality log

The raw file is messy on purpose. Every fix is recorded with the reason for it.

| # | Problem | Rows | Fix | Why this fix |
|---|---|---|---|---|
| 1 | Duplicate rows | 85 | `drop_duplicates()` | The same customer was exported twice |
| 2 | `churned` spelled six ways (`Yes/No`, `yes/no`, `1/0`) | 700 | Mapped all of them to one boolean | One column should have one meaning |
| 3 | `monthly_charges` stored as text (`"₹1,474"`) | 560 | Stripped `₹` and commas, then `to_numeric` | The value is right, the type is wrong |
| 4 | `tenure_months` given in years (`"2.5y"`) | 420 | Converted to months (×12) | Tenure is the key variable, so convert it instead of dropping it |
| 5 | Impossible ages (−5, 0, 150, 212) | 4 | Dropped the rows | Can't be recovered, and it's only 4 rows |
| 6 | City in upper case (`PUNE`) | 180 | `str.title()` | Same city, one spelling |
| 7 | Missing `total_charges` | 209 | Filled with `monthly_charges × tenure_months` | A relationship that can be checked (r ≈ 1.0 on complete rows) beats a median guess |

**Result:** 7,085 raw rows became 6,996 clean rows. Churn rate is 22.2% and mean tenure is 28.6 months.

---

## Dataset

Each of the 14 columns describes one customer:

| Column | Description |
|---|---|
| `customer_id` | Unique customer ID |
| `city` | Hyderabad, Bengaluru, Chennai, Kochi or Pune |
| `age`, `gender` | Demographics |
| `tenure_months` | Months as a customer |
| `plan_type` | Basic, Standard or Premium |
| `contract` | Month-to-month, One year or Two year |
| `monthly_charges`, `total_charges` | Billing amounts (INR) |
| `support_calls` | Number of calls to customer support |
| `payment_method` | UPI, Card, Cash or Bank transfer |
| `has_broadband`, `has_streaming` | Add-on services |
| `churned` | **Target**: whether the customer left |

---

## Repository structure

```
TeleConnect-Customer-Churn-Analysis/
├── customers_2025.csv                     # Raw data (messy on purpose)
├── customers_clean.csv                    # Cleaned data produced by the notebook
├── teleconnect_churn_student_v1.1.ipynb   # Cleaning log, Q1–Q6, executive brief
├── LICENSE                                # Apache 2.0
└── README.md
```

---

## How to run

```bash
git clone https://github.com/brahmanandmathpati/TeleConnect-Customer-Churn-Analysis.git
cd TeleConnect-Customer-Churn-Analysis
pip install numpy pandas matplotlib seaborn scipy
jupyter notebook teleconnect_churn_student_v1.1.ipynb
```

Run all cells from top to bottom in a fresh kernel. The cleaning cells must run before the analysis cells.

---

## Scope

This is **descriptive and diagnostic** analysis. It explains *who* churned and *why*, using statistics and charts. By design it uses no `sklearn` and no predictive models.

## Next step: prediction

The natural follow-up is to answer the question *"Which of our current customers will leave next quarter?"*

| This project | Next project |
|---|---|
| Looks backward | Looks forward |
| Group level: "month-to-month customers churn at 36%" | Individual level: "customer C10521 has an 82% risk of leaving" |
| t-test, chi-square, charts | Classification models (logistic regression, decision trees) |

This analysis already points to the features a model should use: contract, tenure and support calls. City, gender and plan can probably be left out.

---

## Tech stack

Python · pandas · NumPy · Matplotlib · Seaborn · SciPy · Jupyter

## License

Released under the [Apache 2.0 License](LICENSE).
