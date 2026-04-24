# DNSC 6330 — Week 5 individual homework (applied)

**Assignment:** Individual Homework 05 — adversarial / privacy audit on COMPAS (Jupyter notebook).

Lecture **01** asks every GitHub submission to include a README that covers three things. This file is **only** for the **Week 5 individual** hand-in (`Homework_05_Individual_DNSC6330.ipynb`), not for other weeks.

---

## Purpose of the analysis

Reproduce the ProPublica COMPAS **two-year** workflow in Python, then extend it with the Week 5 security topics from class: **projected gradient–style evasion** on scaled tabular features (PGD from the logistic model; same perturbed inputs rescored on a gradient-boosted tree as a transfer attack), **targeted label-flip poisoning** with a “stealth” discussion (AUC stable but fairness metrics move), **PSI** on inputs as a limited guardrail for label-only attacks, **shadow-model membership inference** for logistic regression and boosting, and a **`C` sweep** on the logit to relate regularization to MI-style metrics and subgroup error rates. The write-up in the notebook explains what the numbers mean for deployment, not only the tables.

---

## The Python libraries used

Same families Lecture 01 names as typical for the course (e.g. pandas, numpy, scikit-learn, matplotlib). This notebook imports:

- **pandas** — load/filter data, dummies, metric summaries  
- **numpy** — arrays, RNG, PGD loop, PSI-style calculations  
- **matplotlib** — figures (AIR/AUC vs attack settings, histograms, MI vs `C`)  
- **scikit-learn** — `LogisticRegression`, `GradientBoostingClassifier`, `StandardScaler`, `train_test_split`, `StratifiedShuffleSplit`, `DecisionTreeClassifier`, `roc_auc_score`  
- **warnings** (standard library) — suppress noisy sklearn warnings in the run

Install example if you use a clean venv:

```bash
pip install pandas numpy matplotlib scikit-learn
```

---

## Instructions for reproducing the results

1. Put this repo on **public GitHub** (or upload the notebook to Colab with the same dependencies).  
2. Open **`Homework_05_Individual_DNSC6330.ipynb`**.  
3. Install the libraries above in the kernel you select.  
4. Run **Kernel → Restart and Run All** end to end so the notebook is reproducible from a clean state (Lecture 01 also stresses full reproducibility and clear steps from load through evaluation).  
5. Data: the first data cell pulls **`compas-scores-two-years.csv`** via **`raw.githubusercontent.com`** (raw CSV). Do not substitute the `github.com/.../blob/...` preview URL.

Optional extras in the repo: **`Homework_05_Report_Stub.md`** for the short memo if Canvas asks for it; **`build_hw05_notebook.py`** only if you regenerate the `.ipynb` from script.

---

## What to turn in (Week 5 individual)

- **`Homework_05_Individual_DNSC6330.ipynb`** — main graded code + interpretation.  
- A README covering the three sections above — **this file** (`README_Week5.md`) is written to match that Lecture 01 pattern; if your course requires the filename **`README.md`** at the repo root for every push, copy or merge this content there for the Week 5 submission branch.

Email or submission details: follow **Canvas** (Lecture 01 mentioned **s.akinwumi@gwu.edu** for the repo link; confirm on Canvas).

**Author:** Yukari Teranishi  

**Due date:** see Canvas for Homework 05.

Slides marked CC BY: attribute the instructor when reusing materials.

[Return to repository README](README.md)
