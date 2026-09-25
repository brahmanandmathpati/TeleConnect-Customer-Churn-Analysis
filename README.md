# Capstone B — TeleConnect: Customer Churn Analysis

**Naresh IT · Python for Full Stack Data Science with AI & Generative AI · Lead Trainer: Ajit Byru**

You are the analyst the VP Customer hired to answer one question: who is leaving, and what do they have in common?

## Files
| Path | What it is |
|---|---|
| `customers_2025.csv` | ~7,000 customers, 14 columns — deliberately messy |
| `teleconnect_churn_student.ipynb` | Your notebook: cleaning log, Q1–Q6, the brief |

## Definition of done
- [ ] Data-Quality Log with 7 rows: problem, rows affected, fix, *why this fix*
- [ ] Clean shape, churn rate and mean tenure match the acceptance numbers given in class
- [ ] Q1–Q5 answered with a chart and a one-sentence reading each; Q6 is your own
- [ ] Q2 reports effect size *and* a test; Q3 uses a contingency table *and* a test
- [ ] One-page churn brief (250–350 words) a VP can read without the notebook
- [ ] Notebook runs top-to-bottom in a fresh kernel; five slides; two-minute pitch

## Rules
No `sklearn`, no models. AI assistants for syntax and error messages only — the viva tests conclusions.

## Grading (20)
Data-quality log 4 · Guided questions 6 · Own question 3 · The brief 3 · Reproducibility 2 · Viva & pitch 2

## Interview questions you will be asked
1. What is the churn rate, and why is that number alone not enough?
2. Support calls differ between churners and stayers — cause, effect, or neither? How would you find out?
3. Which test did you use for a categorical feature, and what was its null hypothesis?
4. Your riskiest segment is small. Is it still where the budget should go?
5. What would you need to turn this analysis into a prediction?

## Run
```bash
pip install numpy pandas matplotlib seaborn scipy
jupyter notebook teleconnect_churn_student.ipynb
```
