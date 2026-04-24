# DNSC 6330 — Individual Homework 05 (Applied): Week 5 Security Audit Memo

**Name:** Yukari Teranishi · **GWID:** G29919940 · **Course:** Responsible Machine Learning · **Due:** see Canvas (slides list 11:59 p.m. ET, April 30, 2026)

This memo goes with `Homework_05_Individual_DNSC6330.ipynb`. I left the plots and tables in the notebook on purpose; here I spell out what I did, what popped out in **my** run (after **Restart and Run All**), and what I would tell a team before we shipped anything.

---

## Setup and threat model

I used the ProPublica two-year COMPAS cohort with the usual filters (screening window, valid recid flag, drop “O” charges), `two_year_recid` as the label, and the same tabular features as Lecture 4/5. I stratified 70/30 with `random_state=42`, fit `StandardScaler` on train only, and trained **logistic regression** and **gradient boosting**. Besides test AUC, I track **FPR on true negatives only** by race at threshold 0.5 and **AIR = FPR(African-American) / FPR(Caucasian)**. That is the course’s disparity screen, not a legal disparate-impact test; I use it to see how group error moves under the kinds of attacks NIST AI 100-2 labels (evasion, data poisoning, privacy inference).

---

## Part A — PGD evasion (LR and GBT)

I ran tabular PGD in an \(L_\infty\) ball on **scaled** features for \(\varepsilon \in \{0.25, 0.5, 1.0, 2.0\}\), using the **logistic** weights for the step direction, then scored the **same** `X_adv` on both LR and GBT (transfer attack, not a tree-native PGD). Clean test AIR was about **1.96** for LR and **1.78** for GBT; by \(\varepsilon = 0.5\) both models push **FPR** on non-recidivists up by **0.4+** in absolute terms, and AIR drifts down toward 1 as almost everyone is flagged “high risk” at the largest budget.

**The slide question about AIR crossing 0.80:** in my printed grid, **neither** model hits AIR \< 0.80 at any \(\varepsilon > 0\); the notebook prints **`none in this grid`** for the “first \(\varepsilon\) below 0.80” check. I am *not* claiming the models become “fair” under attack—at \(\varepsilon = 2\) both FPRs go to 1 and AIR pins at 1.0, which is a collapsed classifier, not parity in a good sense.

**Are LR and GBT “equally fragile”?** Not in one number. On the **same** perturbation, LR’s single boundary moves FPR/AIR more smoothly; GBT’s surface is jagged—for example at \(\varepsilon = 0.25\) GBT sits a bit **higher** on AIR than LR. I would not pick between them on clean AUC alone; I want \(\varepsilon\)-sliced FPR and AIR if this score ever touched pretrial or supervision.

---

## Part B — Label-flip poisoning, stealth, PSI

I flipped a share of **high-risk** training labels to low risk **inside** the African-American pool and separately inside the Caucasian pool, retrained LR each time, and overlaid AUC and AIR on the **same** axes for both targets (figures in the notebook). **Stealth** (per the handout): poison \> 0, **|\(\Delta\)AUC| \< 0.02**, and AIR outside **[0.80, 1.25]**. In **my** sweep that shows up **16** times in the printed stealth table—so AUC can look “fine” while AIR is already out of band. That is the scary production pattern: the dashboard you already watch does not scream, but fairness is gone.

**PSI with a 0.10 threshold:** the assignment asks whether per-feature PSI would catch this. It would **not**. Label-only attacks do not move **X**; my sanity check prints **PSI = 0** when expected and actual inputs are the same, and poisoned training still uses the identical feature matrix. Nothing would cross a **0.10** alert on inputs—you cannot catch label tampering with marginal drift on **X** alone.

---

## Part C — Membership inference and \(C\) sweep

**(a) Confidence / score histograms:** the notebook has **side-by-side** panels for LR and GBT: train vs test for **max \(p\)** and for **\(|p - 0.5|\)**. GBT’s train mass is more separated than LR’s—you can see the overfitting story even when MI AUC stays near 0.5.

**(b) Shadow MI and generalization gap:** with **six** shadows and a shallow tree meta-model on **max predicted probability**, I get **MI AUC \(\approx\) 0.491** for LR and **\(\approx\) 0.495** for GBT. **Train–test AUC gap** (train minus test) is about **−0.008** for LR and **+0.08** for GBT. The notebook’s sanity print says **gen gap and MI order the same way** in this two-model comparison, but the MI gap is tiny; I would not use the gap alone as a privacy score.

**(c) \(C \in \{0.01, 0.1, 1, 10\}\) and practical tradeoff:** I refit the target logit, recompute MI, and tabulate `dfC`; the notebook also plots **MI vs \(\log_{10}(C)\)** and a second figure with **test AUC** on a twin axis. In **my** run the table is almost flat: **C = 1.0** gives MI AUC **0.4913** and test AUC **0.7345**; **C = 10.0** (lowest MI in the grid) gives MI **0.4912** and test AUC **0.7346**—a **0.0001** MI move and no real utility change. **FPR_AA**, **FPR_CA**, and **AIR** barely move (AIR stays \(\approx\) 1.96). So here, tuning \(C\) is not a privacy knob; the honest takeaway is to **publish `dfC`-style rows**, not assume L2 fixes leakage.

---

## Reflection — one main risk, mitigations, disparate impact

If I had to pick **one** headline risk across all three parts, it is **stealth poisoning**: I can keep AUC within **2 percentage points** of clean while pushing AIR outside **[0.80, 1.25]** across many poison rates—and **PSI on X stays quiet** the whole time. PGD is bad too, but that failure mode shows up if you stress inputs; the label attack is the one that looks “healthy” on the wrong dashboard.

**Proactive mitigation (quantified):** I used **stronger L2 via \(C\)**. In `dfC`, moving from **C = 0.01** to **C = 10** changes test AUC by only about **0.002** and MI AUC by **0.0003**, with **AIR** essentially flat—so this run does **not** support “crank \(C\) and buy privacy.” A second proactive lever (not fully re-simulated here) is **coarser score buckets** in any external API to cut fine-\(p\) signal for membership or extraction; discretization can move who sits above a cutoff, so I would re-check **FPR_AA**, **FPR_CA**, and **AIR** after any binning change.

**Reactive mitigation (quantified):** when stealth cells appear—in **my** grid, **all 16** nonzero poison settings (eight rates × two target races) match the stealth rule—I would **block auto-merge of new labels** into retrain until two people review high-risk flags. That does not change live scores by itself, but it blocks the **AUC-flat / AIR-bad** path I simulated, at poison rates from **2%** to **30%** for both race-targeted attacks.

**Disparate impact of mitigations:** the **\(C\)** sweep barely moves subgroup FPRs (e.g. **FPR_AA** from **0.272** at C=0.01 to **0.281** at C=10), so I do not see one race getting hammered by regularization *here*—but another seed or feature set could differ, so I would still re-run fairness columns whenever \(C\) or release format changes. **Label-review gates** are race-neutral as a procedure, yet they matter most when poisoning targets one group’s positives, where my AIR line moves while AUC barely budges.

---

### References (short)

ProPublica (2016); Vassilev et al. (2024), *NIST AI 100-2*; Shokri et al. (2017); Lecture 05 (GW, Dr. Michael Akinwumi).

---

*If Canvas moves the deadline, use the syllabus; after any notebook edit, re-run all cells and skim this memo against the printed tables.*
