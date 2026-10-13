# Winter 2027 WiM Directed Reading & Research Program — Syllabus

## AI for Faster Variable Annuity Pricing through Learned Contract Similarity

**Mentor:** Hao Quan · Department of Statistics and Actuarial Science, University of Waterloo  
**Program:** Women in Mathematics (WiM) Directed Reading & Research Program (DRP)  
**Project format:** Three undergraduate mentees; **8 guided research weeks + 2 presentation weeks**  
**Term:** Winter 2027 · **Weekly meeting time and location:** To be arranged with the group  
**Course credit:** This is a mentor-designed research plan, not a University of Waterloo academic course syllabus. Program logistics and final presentation requirements take precedence.

> **Guiding question:** Under a fixed Monte Carlo simulation budget, can we estimate variable annuity guarantee values more accurately by learning which contracts are similar?

**[Repository home](README.md) · [Weekly notebooks](readings/) · [Computing requirements](requirements.txt)**

---

## 1. Project overview

Variable annuities combine investment accounts with insurance guarantees. A large portfolio may have many contracts with different terms, fees, account values, volatilities, and guarantee levels. Simulating every contract accurately can require substantial computational work. Can we simulate a carefully selected subset, then predict the remaining contracts?

We study a **compressor–simulator–predictor** workflow:

1. **Compress:** Choose a small set of representative contracts using random selection, clustering, or another selection method.
2. **Simulate:** Generate noisy Monte Carlo valuation labels for those contracts.
3. **Predict:** Estimate values for the remaining contracts by nearest-neighbour transfer, regression, or a learned similarity rule.

The distinctive research question is whether a *task-aligned distance*—one fitted to reflect valuation differences—chooses more useful neighbours than ordinary Euclidean distance. This is an empirical question, **not an assumption that the AI model will win**.

### Precise scope

The teaching model is a **simplified guaranteed minimum accumulation benefit (GMAB)** with a **given annual fee**. Its learning target is the **expected discounted gross guarantee top-up per unit premium**, rather than the total policyholder value or an unknown fair fee. In normalized notation, the maturity top-up is based on `max(g - A_T/P, 0)`, where `g` is the guaranteed maturity amount per premium and `A_T` is the account at maturity. A risk-neutral lognormal account model supplies Monte Carlo paths.

An analytic put-option benchmark is available for this simplified cash flow. We use it **only for explicitly labelled auditing and evaluation**, never to manufacture training or tuning labels. We do not claim that a machine-learning approximation outperforms an available analytic formula.

Mortality, lapses, withdrawals, ratchet guarantees, dynamic behaviour, nested real-world projections, multi-rider benefits, and stochastic fair-fee root solving are **outside the required implementation scope**. Readings explain why real products are more complicated.

## 2. Learning outcomes

By the end of Week 8, each student should be able to:

1. Define a GMAB top-up, its discounting convention, and normalized versus dollar valuations.
2. Simulate terminal investment-account scenarios under a stated risk-neutral model, estimate a mean and standard error, and interpret simulation uncertainty.
3. Explain why contract representation, scaling, and nearest-neighbour distance affect transferred valuations.
4. Implement and compare random and K-means representative selection inside a compressor–simulator–predictor pipeline.
5. Fit and evaluate ridge regression, an additive spline regression model, and random forests using consistent training labels.
6. Fit nonnegative feature weights from labelled contract pairs, apply a learned distance, and explain the difference between direct prediction and learned-neighbour retrieval.
7. Design a controlled fixed-budget benchmark with explicit data splitting, development-only model selection, reproducible seeds, cost accounting, and a held-out final test.
8. Communicate evidence, uncertainty, limitations, and individual contributions in a short research report and team presentation.

## 3. Prerequisites and accessibility

**Required: None.**

**Recommended, but not required:** **MATH 127** (Calculus 1 for the Sciences) **or MATH 137** (Calculus 1 for Honours Mathematics) for functions and derivatives; **MATH 106** (Applied Linear Algebra 1) **or MATH 136** (Linear Algebra 1 for Honours Mathematics) for vectors, matrices, and distances. **MATH 128** (Calculus 2 for the Sciences) or **MATH 138** (Calculus 2 for Honours Mathematics) may help with integration but is optional.

**No previous actuarial science, finance, statistics, machine learning, research, or Python course is required.** Each notebook introduces the necessary ideas in context and supplies working reference code. Students from different academic backgrounds are welcome. Curiosity, steady participation, and a willingness to ask questions matter more than completed courses.

These recommendations concern **preparation for this research project**, not waivers of the university's enrolment rules for the courses themselves. Course titles are based on the [Waterloo 2023–24 MATH course calendar](https://ucalendar.uwaterloo.ca/2324/COURSE/course-MATH.html).

## 4. Materials, technology, and how to work

- **Main learning materials:** The eight Jupyter notebooks linked below. Every notebook contains an overview, theory, executable demonstrations, readings, core programming assignments, and a submission checklist.
- **Software:** Python 3.12, NumPy, pandas, SciPy, scikit-learn, matplotlib, and JupyterLab (see [requirements.txt](requirements.txt)). No proprietary datasets or paid cloud computing are required.
- **Data:** Synthetic contract data generated in Python. Students should not upload private policyholder data.
- **Readings:** Selected passages from Hardy (2003), Bauer–Kling–Russ (2008), Gan–Lin (2015), Feng–Tan–Zheng (2020), and a **mentor-supplied** Quan–Feng fair-fee working manuscript. Selected official Python/scikit-learn documentation supplements the articles.
- **Suggested weekly routine:** Read the notebook overview, run the guided examples, annotate unfamiliar concepts, answer the focused reading questions, complete the core programming exercises, and bring one result plus one question to the group meeting. The mentor will coordinate a manageable workload and adjust for students' backgrounds.

### Getting started

From the cloned repository root:

    python -m venv .venv
    source .venv/bin/activate          # macOS/Linux
    python -m pip install -r requirements.txt
    python -m jupyter lab

On Windows PowerShell, activate the environment with `.venv\Scripts\Activate.ps1`. Open the appropriate notebook under `readings/Week N/` and run cells in order. The distributed notebooks have **cleared outputs** so each student produces and interprets their own results.

**Weeks 7 and 8 form a connected reproducible experiment:** Week 7 saves a frozen study protocol; Week 8 checks that protocol before final testing. Follow the notebook's instructions and retain its output files.

## 5. Research conduct, teamwork, and submissions

### Three-student team

Students rotate three roles across the research weeks:

- **Implementation lead:** Runs and explains one assigned part of the weekly code; records dependencies and reproducibility steps.
- **Verification lead:** Checks units, data leakage, seeds, math-to-code consistency, and whether plots/tables support stated claims.
- **Research communication lead:** Summarizes the reading, leads discussion, and drafts a short interpretation of observed results.

**Roles organize collaboration; they do not excuse anyone from doing the core notebook exercises.** Each student should be able to explain the whole pipeline by the end of the project.

### Weekly submission

Unless a notebook specifies a more particular item, submit: (i) your executed notebook with the two core homework exercises, (ii) a short response to the assigned reading questions, (iii) labelled tables or figures and a short interpretation, and (iv) any reproducibility details needed to rerun your results. Optional extensions remain optional. Mentor feedback is developmental rather than a university course grade.

### Essential experimental rules

1. **Split contracts first.** Keep candidate/reference, validation, and final test sets separate by contract ID.
2. **Prevent leakage.** Fit preprocessing on the permitted training/candidate data. Do not use final test responses for feature engineering or model selection.
3. **Use comparable labels.** When comparing predictors in the same condition, use identical simulated representative-contract labels and record shared versus method-specific costs.
4. **Charge simulation honestly.** Count contract–path evaluations, including reference and validation simulations; distinguish them from elapsed runtime and model-fitting overhead.
5. **Respect sampling uncertainty.** Simulation labels are noisy; reported standard errors concern Monte Carlo estimates, not every possible source of error.
6. **Do not silently fix unfavourable results.** Retain and explain negative raw model predictions, optimisation failures, and cases where learned similarity performs worse.
7. **Freeze before test.** Select models on validation data in Week 7 and preserve the exact study manifest before examining Week 8 results.
8. **Attribute sources and contributions.** Cite sources, label AI assistance if used under the group's agreed disclosure practice, and write interpretations in your own words.

## 6. Weekly syllabus and research milestones

### Week 1 — Understanding variable annuities and GMAB payoffs

**[Open Week 1 notebook](readings/Week%201/01_Contracts_and_GMAB_Payoffs.ipynb)**

**Theory:** What investment guarantees do; interpreting contract features; normalized top-up versus the policyholder's entire account; dollars, percentages, and units; payoff versus discounted payoff versus price.

**Lab:** Build a synthetic contract table in pandas; derive and graph GMAB payoff curves; implement a reusable payoff function; check scaling by premium.

**Reading:** Selected introductory Hardy (2003) passages about investment guarantees and introductory Bauer–Kling–Russ (2008) material about guaranteed minimum benefits.

**Core homework:** (1) Plot payoffs for different guarantee ratios on the same account-scenario grid; (2) verify equivalence of direct-dollar and normalized calculations.

**Milestone:** Completed notebook, reading answers, two payoff exercises, and a 150–200-word explanation of what the project will predict.

### Week 2 — Risk-neutral Monte Carlo valuation

**[Open Week 2 notebook](readings/Week%202/02_Risk_Neutral_Monte_Carlo.ipynb)**

**Theory:** Risk-neutral terminal fund modelling; the Monte Carlo sample mean, standard error, and approximate confidence interval; impact of rare guarantee payments; distinguishing sampling uncertainty from model error.

**Lab:** Simulate a GMAB valuation; compare path counts and independent seeds; use an analytic reference **only as an audit**; benchmark direct simulation of every contract.

**Reading:** Selected Hardy and Bauer–Kling–Russ passages on valuation under a chosen probability law.

**Core homework:** (1) Audit the behaviour of estimated standard errors and confidence intervals for a high-account contract; (2) study the tradeoff between path budget, computing work, and uncertainty for a 100-contract portfolio.

**Milestone:** Tables and figures showing simulation uncertainty and direct-all costs; a provisional, clearly qualified path-budget recommendation.

### Week 3 — Measuring contract similarity and transferring a neighbour's value

**[Open Week 3 notebook](readings/Week%203/03_Contract_Similarity_and_Nearest_Neighbours.ipynb)**

**Theory:** Contract representation, Euclidean distance, min–max scaling, feature units, nearest-neighbour value transfer, and why premium should not be a feature when predicting normalized values.

**Lab:** Fit scaling from candidates only; compute distances; simulate selected references; perform CSM-style one-neighbour transfer; compare validation errors using independent Monte Carlo labels.

**Reading:** Feng–Tan–Zheng (2020), three-stage workflow and CSM predictor, especially the nearest-neighbour transfer rule; skim Gan–Lin (2015) for representative-contract motivation.

**Core homework:** (1) Compare raw, min–max and standard-deviation scaling while keeping references and their labels fixed; (2) compare 1, 3, 5 and 9 neighbours with the same label bank.

**Milestone:** A controlled nearest-neighbour baseline, figures of validation error versus scaling/neighbour count, and a 200-word explanation of its assumptions.

### Week 4 — Compress, simulate, predict

**[Open Week 4 notebook](readings/Week%204/04_Compress_Simulate_Predict.ipynb)**

**Theory:** Unsupervised representative selection, random compression, K-means centroids versus actual contracts, geometric coverage, and the distinction between cluster quality and valuation accuracy.

**Lab:** Implement both compressors; simulate selected actual contracts; predict non-reference values via nearest-neighbour transfer; record coverage, RMSE, and a simulation/cost ledger.

**Reading:** Feng–Tan–Zheng (2020), Sections 3 and 5 selectively; Gan–Lin (2015), abstract, introduction, and framework overview.

**Core homework:** (1) Compare several representative counts and separate reference work from equal-budget claims; (2) implement a farthest-first coverage comparator. A small failure-case study is optional.

**Milestone:** First complete compressor–simulator–predictor pipeline, cost tables, and one evidence-based interpretation of where it succeeds or fails.

### Week 5 — Supervised machine-learning predictors

**[Open Week 5 notebook](readings/Week%205/05_Supervised_Valuation_Models.ipynb)**

**Theory:** Regression as a mapping from features to values; bias–variance tradeoffs; ridge regression, additive spline functions, random forests, nonlinearity, and interaction effects.

**Lab:** Train the methods on **the same selected reference contracts and the same simulated labels**. Compare development RMSE, bias, prediction plausibility, and fitting/search costs.

**Reading:** Feng–Tan–Zheng (2020), the predictor stage; the official scikit-learn documentation for Ridge, SplineTransformer, and RandomForestRegressor.

**Core homework:** (1) Plot training and validation error across ridge penalties; (2) test one justified feature interaction while keeping the data bank fixed. A gradient-boosting challenger is optional.

**Milestone:** Reproducible supervised-learning comparison table and a short distinction between direct value prediction and nearest-neighbour similarity.

### Week 6 — Explicitly learning contract similarity

**[Open Week 6 notebook](readings/Week%206/06_Learning_Contract_Similarity.ipynb)**

**Theory:** Task-aligned distances and nonnegative diagonal weights; fitting squared feature differences to squared normalized valuation differences; regularization; noisy simulated pair labels; why the fitted object may be a **pseudometric**.

For encoded feature vectors `z`, study the family

$$
d_w^2(z_i,z_j)=\frac{1}{p}\sum_{k=1}^{p}w_k(z_{ik}-z_{jk})^2,
\qquad w_k\geq0,\quad\sum_kw_k=p.
$$

**Lab:** Fit a simple constrained weighted distance using labelled reference pairs; tune on development data; compare Euclidean 1-NN, learned-distance 1-NN, and direct regression on the same contracts.

**Reading:** Feng–Tan–Zheng (2020), CSM and its distance-based transfer; Quan–Feng (2026), **mentor-supplied** fair-fee manuscript introduction and start-value approximation motivation (not its proofs). The separate unpublished similarity manuscript is **not assigned**.

**Core homework:** (1) Shuffle response labels only for metric fitting to construct a negative control; (2) vary Monte Carlo label precision while holding contract identities fixed.

**Milestone:** Fitted and interpretable feature weights, controlled comparisons, and a discussion of when learned similarity does or does not help.

### Week 7 — Design an honest benchmark and freeze it

**[Open Week 7 notebook](readings/Week%207/07_Experimental_Design_and_Freeze.ipynb)**

**Theory:** Fairness under a total simulation-work budget; model-selection leakage; validation versus final test; consistent simulation labels; independent replications; honest cost attribution.

**Lab:** Assemble the complete benchmark including direct-all and representative-contract pipelines; vary a small declared set of model/reference conditions; compare validation metrics; record fitting, simulation, selection, and prediction work; write a **frozen protocol manifest**.

**Reading:** Feng–Tan–Zheng (2020), discussion of experiments and portfolio versus contract error; optional Section 4 on simulation allocation.

**Core homework:** (1) Check work budgets algebraically for multiple reference counts; (2) audit a controlled comparison; (3) write a 150–250-word analysis freeze.

**Milestone:** Development comparison plus `outputs/week07/frozen_protocol.json` and its SHA-256 digest. **Do not examine final test outcomes in Week 7.**

### Week 8 — Final testing, interpretation, and research report

**[Open Week 8 notebook](readings/Week%208/08_Final_Evaluation_and_Research_Report.ipynb)**

**Theory:** Held-out evaluation, paired method comparisons, contract-level errors versus portfolio-sum cancellation, sampling uncertainty, limitations, and responsible scientific reporting.

**Lab:** Verify the Week 7 freeze; run final test evaluation; compare methods; diagnose chosen neighbours; perform an independent Monte Carlo audit; export reproducible result files.

**Reading:** Revisit selected material from the **five core readings** to interpret conclusions. No new technical paper is required.

**Core homework:** (1) Calculate paired error differences between learned and Euclidean/CSM-style neighbours; (2) investigate aggregate-error cancellation and implausible negative predictions; (3) create the reproducible research handoff.

**Milestone:** A **4–6-page research report** with problem, model, baseline, methods, budget, results, limitations, and contributions; completed notebooks and plots; presentation outline.

### Week 9 — Presentation writing and first rehearsal

No new required modelling material. Build a coherent story: insurance motivation → modelling simplifications → representative simulation → prediction/similarity methods → controlled experiment → findings and limitations. Assign balanced speaking roles, create legible figures, and perform a timed rehearsal with mentor feedback. Every student should be prepared to answer questions about the entire project.

**Milestone:** Complete slide draft, rehearsed narrative, and a list of questions to address before the final presentation.

### Week 10 — Final rehearsal, Q&A, and handoff

Run an uninterrupted rehearsal and practice questions about the probability law, standard errors, nearest neighbours, trained weights, budget fairness, and the held-out test. Finalize slides and a reproducibility checklist. Confirm official showcase timing with WiM; the group can initially practice a roughly 15–20-minute presentation and follow-up discussion.

**Milestone:** Presentation-ready slide deck, finished report, reproducible notebook/output package, and short individual contribution statements.

## 7. Research implementation and computational scale

**Core teaching configuration:** Weeks 7–8 use **1,200 contracts**, 2 seeds and two representative counts (40 and 80), with a specified path budget and evaluation ceiling. Use `RUN_RESEARCH_SCALE` only if the group has adequate time and computing resources.

**Optional larger configuration:** **10,000 contracts**, 5 seeds and representative counts 100 and 200. This is an optional extension and **was not run during initial course preparation**. Neither configuration guarantees a preferred method will perform best.

Week 7 and Week 8 compare contract-level RMSE/MAE and selected tail-error diagnostics, alongside portfolio aggregation error and an explicit cost ledger. Observed wall-clock time is a machine-dependent diagnostic, not an interchangeable substitute for simulation work.

## 8. Feedback and success criteria

The mentor will give formative feedback on mathematical interpretation, reproducible code, fair comparisons, careful reading, and collaborative communication. There are no assumed formal university grades in this document. The final project's success is **not** defined by an accuracy victory for AI; it is defined by a convincing answer to the question, including results that show no improvement or increased cost.

Students should be able to reproduce the essential tables and plots from the repository, identify what the results do *not* establish, and explain one sensible follow-up experiment.

## 9. Selected readings and sources

Read only the assigned portions for each week. Full textbooks and technical proofs are **not required**. Publisher PDFs are not redistributed through this repository.

1. **Mary R. Hardy (2003).** *Investment Guarantees: Modeling and Risk Management for Equity-Linked Life Insurance.* Wiley. [Book information](https://www.wiley-vch.de/en/areas-interest/finance-economics-law/finance-investments-13fi/investments-securities-13fi3/investment-guarantees-978-0-471-39290-3).
2. **Daniel Bauer, Alexander Kling, and Jochen Russ (2008).** “A Universal Pricing Framework for Guaranteed Minimum Benefits in Variable Annuities.” *ASTIN Bulletin*. [DOI](https://doi.org/10.2143/AST.38.2.2033356).
3. **Guojun Gan and X. Sheldon Lin (2015).** “Valuation of Large Variable Annuity Portfolios under Nested Simulation: A Functional Data Approach.” *Insurance: Mathematics and Economics*, 62, 138–150. [DOI](https://doi.org/10.1016/j.insmatheco.2015.02.007).
4. **Ben Mingbin Feng, Zhenni Tan, and Jiayi Zheng (2020).** “Efficient Simulation Designs for Valuation of Large Variable Annuity Portfolios.” *North American Actuarial Journal*, 24(2), 275–289. [DOI](https://doi.org/10.1080/10920277.2019.1685394) · [Author-hosted PDF](https://www.math.uwaterloo.ca/~mbfeng/papers/2020_VAPort.pdf).
5. **Hao Quan and Ben (Mingbin) Feng (2026).** *Warm-Started Projected Robbins–Monro for Portfolio-Scale Variable Annuity Fee Setting.* Mentor-provided working manuscript; not a published reading link.
6. **Technical documentation (as needed):** [NumPy](https://numpy.org/doc/), [pandas](https://pandas.pydata.org/docs/), [SciPy](https://docs.scipy.org/doc/scipy/), [scikit-learn](https://scikit-learn.org/stable/user_guide.html), and [Jupyter](https://jupyter.org/).

### Version note

This syllabus describes the **Winter 2027 teaching plan and the currently published notebooks**. It may be refined with the group, but the core experimental rule—**choose methods on development data before opening the final test**—should remain unchanged.
