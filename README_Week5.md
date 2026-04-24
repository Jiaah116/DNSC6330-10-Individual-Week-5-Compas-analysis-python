# Week 5 homework (DNSC 6330) — COMPAS adversarial stuff


## What to actually open

Turn in / grade: **`Homework_05_Individual_DNSC6330.ipynb`**. Before you submit, do **Restart kernel → Run all** so it’s not relying on cells you ran out of order.

If you want the short memo template, I started from **`Homework_05_Report_Stub.md`**. There are also longer writeups (`Homework_05_Individual_Audit_Report.md` and a Japanese version) if you’re curious; they’re not required unless the syllabus says so.

Optional: **`build_hw05_notebook.py`** rebuilds the notebook from a script — I only use that if I’m editing the `.py` version.

## What the notebook does (quick)

- Pulls ProPublica `compas-scores-two-years.csv` from the raw GitHub link in the first code cell.
- Trains logistic regression + gradient boosting, scaled features, same kind of FPR / AIR setup as before (threshold 0.5).
- **Part 1:** Tabular PGD on the *linear* model, then I score the *same* perturbed `X` on the tree too (transfer attack — not a special tree PGD). Epsilon grid 0.25, 0.5, 1, 2.
- **Part 2:** Label flips on high-risk rows, separately for African-American vs Caucasian training subsets, retrain LR, plot AUC and AIR. “Stealth” = poison rate > 0 but AUC barely moves and AIR leaves the 0.8–1.25 band. PSI on X is basically useless for label-only attacks and the notebook says why.
- **Part 3:** Shadow models + simple MI, histograms, then a `C` sweep on the logit (MI vs test AUC vs fairness columns). Reflection at the end ties it together.

Slides say to explain what the numbers *mean*, not just paste tables — that’s what the markdown cells are for.

## Running it

Needs normal sklearn stack: pandas, numpy, matplotlib, scikit-learn. If something dies on your laptop it’s usually a missing import or an old sklearn — the notebook only uses stuff from class.

Data: use the **raw** `raw.githubusercontent.com/...csv` URL from the notebook, not the pretty GitHub page link.

## Submitting

Whatever Canvas says — usually public repo + README + notebook + short report. Email / address is on Canvas (slides had s.akinwumi@gwu.edu at some point but I’d verify there).

## Me / due date

Yukari Teranishi — due date is whatever Canvas lists for HW5.

Lecture PDFs are CC BY if you’re reusing material; credit the instructor.

[Back to main README](README.md)
