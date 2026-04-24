# Week 5 individual assignment — DNSC 6330 (Responsible Machine Learning)

This repository contains my Week 5 applied exercise: an adversarial and privacy-focused audit of a COMPAS-style recidivism pipeline, following the Week 4 data recipe and the security topics from **Lecture 05** (evasion, poisoning, drift monitoring limits, membership inference).

## Primary submission

The artifact to grade is **`Homework_05_Individual_DNSC6330.ipynb`**. Please run **Kernel → Restart and Run All** before evaluation so results reflect a clean execution order.

Supporting materials:

- **`Homework_05_Report_Stub.md`** — template for the short written memo that accompanies the notebook (if required).
- **`Homework_05_Individual_Audit_Report.md`** and **`Homework_05_Individual_Audit_Report_ja.md`** — supplementary narrative drafts.
- **`build_hw05_notebook.py`** — optional utility to regenerate the `.ipynb` from script (only needed if you maintain a `.py` source).

## Summary of methods (notebook)

The notebook reproduces the ProPublica **two-year** COMPAS extract with the same screening and exclusion filters used in Lectures 4–5, then fits **logistic regression** and **gradient boosting** on standardized features. Group-wise **false positive rates** and an **AIR**-style ratio (African-American FPR / Caucasian FPR) are reported at decision threshold 0.5.

1. **Evasion (PGD):** Projected gradient-style updates in an \(L_\infty\) ball on *scaled* tabular features, using the linear model’s weights; the perturbed design matrix is also evaluated on the boosted tree as a **transfer** attack (not a tree-native adversary).
2. **Label poisoning:** Controlled flips among high-risk training labels, stratified by race (African-American vs Caucasian pools), with re-estimation of logistic regression. A **stealth** regime is highlighted where test **AUC** changes little while **AIR** moves outside a \([0.80, 1.25]\) band. **PSI** on input features is discussed as uninformative for label-only corruption.
3. **Membership inference:** Shadow models matched to the target family, a shallow meta-classifier on **maximum predicted probability**, and comparison of MI signal to the train–test **generalization gap**. A regularization (**`C`**) sweep on logistic regression links MI estimates to test AUC and fairness-related error rates.

Interpretation is written in the notebook’s markdown cells; the assignment emphasizes explanation of results, not only numerical output.

## Environment and data

Dependencies are standard for the course: **pandas**, **numpy**, **matplotlib**, **scikit-learn** (and **scipy** where used). The dataset is loaded over HTTPS from the **raw** `compas-scores-two-years.csv` URL given in the notebook (not the GitHub HTML view of the file).

## Submission

Follow the instructions on **Canvas** (typically a public repository, README, notebook, and any required report). Confirm the instructor contact email there if submission by email is required.

## Author

Yukari Teranishi

**Due date:** as posted on Canvas for Homework 05.

Course lecture slides marked CC BY should be credited to the instructor when material is reused.

[Return to repository README](README.md)
