# DNSC 6330 · Week 5 Individual Assignment

**Course:** DNSC 63310, Responsible Machine Learning (GW School of Business).


## Overview
The graded artifact is **`Homework_05_Individual_DNSC6330.ipynb`**. It:

- Loads ProPublica **`compas-scores-two-years.csv`** (same filters as Lectures 4–5), builds **logistic regression** and **gradient boosting** on scaled features, and reports **FPR by race** and **AIR** (African-American FPR / Caucasian FPR) at threshold 0.5.
- **Part 1 — PGD:** Tabular **L∞** PGD on **scaled** features using the **linear** model’s weights; the **same** perturbed matrix is **re-scored** on the GBT (**transfer** attack, not tree-specific PGD). Sweeps ε in `{0.25, 0.5, 1.0, 2.0}` (plus clean baseline) and tracks subgroup FPR and AIR.
- **Part 2 — Poisoning:** **Label flips** on high-risk training rows, targeted separately at **African-American** and **Caucasian** pools; retrains LR across poison rates; defines a **stealth** region where aggregate **AUC** barely moves but **AIR** leaves `[0.80, 1.25]`. Explains why **PSI on X** does not detect **label-only** tampering.
- **Part 3 — Membership inference:** **Shadow** models (same family as target), meta-classifier on **max predicted probability**, MI AUC for LR and GBT, train vs test score histograms, and a **`C` sweep** on `LogisticRegression` linking **MI AUC**, **test AUC**, **FPR**, and **AIR**.
- Ends with a **reflection** tying robustness, fairness, and privacy.

A short written memo can follow **`Homework_05_Report_Stub.md`**; fuller narrative drafts live in **`Homework_05_Individual_Audit_Report.md`** (and optional **`Homework_05_Individual_Audit_Report_ja.md`**).

## How this matches the Week 5 assignment (conceptual map)

| Part | Theme | In the notebook |
|------|--------|-----------------|
| **1** | Evasion / robustness (tabular PGD) | L∞ PGD from LR weights; LR + GBT scored on same `X_adv`; AIR vs ε |
| **2** | Data poisoning & monitoring blind spots | Race-targeted label flips; stealth rule; PSI sanity on features |
| **3** | Privacy (membership inference) | Shadow MI for LR & GBT; gen gap vs MI ordering check; `C` sweep |
| **—** | Interpretation | Markdown answers + final reflection (design vs operations controls) |

Grading typically rewards **explaining** what each number implies for deployment—not only printing tables.

## Files in this week

| File | Role |
|------|------|
| [`Homework_05_Individual_DNSC6330.ipynb`](Homework_05_Individual_DNSC6330.ipynb) | **Submit / grade:** main notebook (restart kernel, run all before hand-in). |
| [`Homework_05_Report_Stub.md`](Homework_05_Report_Stub.md) | Outline for the short report that accompanies the notebook. |
| [`Homework_05_Individual_Audit_Report.md`](Homework_05_Individual_Audit_Report.md) | Optional longer audit-style writeup. |
| [`build_hw05_notebook.py`](build_hw05_notebook.py) | Optional: regenerate the `.ipynb` from script. |

## Python libraries

**Core:** `pandas`, `numpy`, `matplotlib`, `scikit-learn`, `scipy` (as used in the notebook).

The notebook intro notes that missing **`sklearn`** imports are the most common local failure mode; otherwise the stack matches the rest of the course.

## How to run

1. Clone or download the repo (or copy the `.ipynb`).
2. Open in Jupyter, JupyterLab, or VS Code; or upload to Colab.
3. **Kernel → Restart & Run All** so outputs and order are reproducible.
4. Data loads from the **raw** ProPublica URL inside the notebook (`raw.githubusercontent.com/.../compas-scores-two-years.csv`), not the GitHub HTML preview link.

**Submission (typical pattern from the course):** public GitHub repo, README, notebook, plus any required short report. Email the repo link if the syllabus asks for it—confirm the address on **Canvas** (historically `s.akinwumi@gwu.edu` in slides).

## Author

Yukari Teranishi

## Due date

Confirm on **Canvas**; use the instructor’s posted deadline for Homework 05 / Week 5.

---

**Back to repo root:** [`README.md`](README.md)
