# DNSC 6330 — Week 5 individual homework 


## Purpose of the analysis

Reproduce the ProPublica COMPAS **two-year** workflow in Python, then extend it with the Week 5 security topics from class: **projected gradient–style evasion** on scaled tabular features (PGD from the logistic model; same perturbed inputs rescored on a gradient-boosted tree as a transfer attack), **targeted label-flip poisoning** with a “stealth” discussion (AUC stable but fairness metrics move), **PSI** on inputs as a limited guardrail for label-only attacks, **shadow-model membership inference** for logistic regression and boosting, and a **`C` sweep** on the logit to relate regularization to MI-style metrics and subgroup error rates. The write-up in the notebook explains what the numbers mean for deployment, not only the tables.


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

---

## What to turn in (Week 5 individual)

- **`Homework_05_Individual_DNSC6330.ipynb`** — main graded code + interpretation.  
- A README
-  Report



