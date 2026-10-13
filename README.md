# AI for Faster Variable Annuity Pricing through Learned Contract Similarity

**University of Waterloo · Women in Mathematics (WiM) Directed Reading & Research Program · Winter 2027**

**Mentor:** Hao Quan, Department of Statistics and Actuarial Science  
**Group:** Three undergraduate researchers · **8 research weeks + 2 presentation weeks**  
**Prerequisites:** **None required** · Meeting arrangements to be confirmed

> **Research question:** Can we estimate the value of thousands of variable annuity guarantees without simulating every contract at full precision? In particular, does *learning which contracts are similar* improve pricing under a fixed Monte Carlo simulation budget?

**📚 [Read the complete syllabus](SYLLABUS.md)** · **💻 [Open the eight weekly notebooks](readings/)** · **🔧 [Install Python requirements](requirements.txt)**

## Welcome — no previous experience required

This undergraduate project introduces **actuarial science, probability, Python, and machine learning** through guided Jupyter notebooks. Students who have never taken an actuarial science, finance, statistics, or machine-learning course are welcome.

- **Required courses:** None.
- **Recommended, not required:** **MATH 127 or MATH 137** (introductory calculus) and **MATH 106 or MATH 136** (introductory linear algebra). **MATH 128 or MATH 138** may also help, but are optional.
- **Coding experience:** No prior Python course is required; each notebook includes theory, demonstrations, reading prompts, and starter code.

For the complete 250-word-maximum application wording, see the [Prerequisites and accessibility section](SYLLABUS.md#3-prerequisites-and-accessibility) in the syllabus. The official Waterloo course descriptions are [here](https://ucalendar.uwaterloo.ca/2324/COURSE/course-MATH.html).

## What will we research?

A *variable annuity* is an investment-linked insurance product that may include a minimum guarantee. Insurance companies often need to estimate guarantee liabilities for very large portfolios.

We focus on a manageable teaching example: a **guaranteed minimum accumulation benefit (GMAB)** with an **already-specified fee**. The target is the **expected discounted gross guarantee top-up per unit premium**. The contract model is simulated under a simple risk-neutral lognormal account model. Because this teaching payoff also has an analytical benchmark, students can audit the accuracy of Monte Carlo estimates.

The central **compressor–simulator–predictor** design has three stages:

1. **Compress:** Choose a subset of representative contracts, using random selection or unsupervised clustering such as K-means.
2. **Simulate:** Use Monte Carlo to estimate the guarantee values of those representatives.
3. **Predict:** Estimate the remaining contracts using nearest-neighbour transfer, ridge regression, additive splines, random forests, or a **learned contract distance**.

We will compare direct simulation, ordinary Euclidean neighbours, explicitly learned nonnegative feature weights, and supervised valuation models. The goal is **good experimental reasoning**, not proving that a complicated AI model must win.

### What is not included?

The required notebooks do **not** implement mortality, dynamic withdrawals, surrender behaviour, multiple riders, or solving a fair-fee root equation. These important actuarial topics appear in the references as context; they are beyond the scope of this introductory eight-week study.

## Weekly Jupyter notebooks

Each notebook contains a conceptual **overview**, worked **mathematics**, executable **Python examples**, focused **readings**, core **coding assignments**, and a **submission checklist**. Links below match the actual GitHub folders.

| Week | Topic and key skill | Open notebook |
|:---:|---|---|
| **1** | Contract features, GMAB payoffs, discounting, and normalized valuation | [Week 1 — Contracts and GMAB Payoffs](readings/Week%201/01_Contracts_and_GMAB_Payoffs.ipynb) |
| **2** | Risk-neutral simulation, Monte Carlo standard errors, and direct-all baseline | [Week 2 — Risk-Neutral Monte Carlo](readings/Week%202/02_Risk_Neutral_Monte_Carlo.ipynb) |
| **3** | Feature scaling, nearest neighbours, and CSM-style value transfer | [Week 3 — Contract Similarity](readings/Week%203/03_Contract_Similarity_and_Nearest_Neighbours.ipynb) |
| **4** | Random and K-means representative selection; compressor–simulator–predictor | [Week 4 — Compress, Simulate, Predict](readings/Week%204/04_Compress_Simulate_Predict.ipynb) |
| **5** | Ridge regression, additive spline regression, random forests | [Week 5 — Supervised Valuation Models](readings/Week%205/05_Supervised_Valuation_Models.ipynb) |
| **6** | Learn nonnegative feature weights to create task-aligned nearest neighbours | [Week 6 — Learning Contract Similarity](readings/Week%206/06_Learning_Contract_Similarity.ipynb) |
| **7** | Equal-budget method comparisons, validation, and a frozen protocol | [Week 7 — Experimental Design and Freeze](readings/Week%207/07_Experimental_Design_and_Freeze.ipynb) |
| **8** | Held-out test, portfolio and individual errors, independent MC audit, report | [Week 8 — Final Evaluation and Report](readings/Week%208/08_Final_Evaluation_and_Research_Report.ipynb) |

**Weeks 9–10:** Prepare the research story and slide deck, rehearse the presentation, practice questions, and hand over the report and reproducible results. The detailed [week-by-week syllabus](SYLLABUS.md#6-weekly-syllabus-and-research-milestones) contains readings, programming exercises, and milestones for every week.

## Getting started

### 1. Get the code

Download the repository from **Code → Download ZIP**, or clone it:

    git clone https://github.com/MasonQ0813/WiM-DRP-Winter27-AI-for-Faster-Variable-Annuity-Pricing-through-Learned-Contract-Similarity.git
    cd WiM-DRP-Winter27-AI-for-Faster-Variable-Annuity-Pricing-through-Learned-Contract-Similarity

### 2. Create a Python environment

Python 3.12 is recommended.

    python -m venv .venv
    source .venv/bin/activate
    python -m pip install -r requirements.txt
    python -m jupyter lab

On Windows PowerShell, activate using `.venv\Scripts\Activate.ps1`.

### 3. Open and run Week 1

Open `readings/Week 1/01_Contracts_and_GMAB_Payoffs.ipynb`. Choose the project's Python environment as your Jupyter kernel, then use **Run All**. Complete the reading questions and core homework inside the notebook.

The published notebooks have **no precomputed outputs**. Students produce their own results. They use synthetic data—no sensitive policyholder data or paid market data is required.

## How we ensure a fair experiment

All comparisons need a clearly stated **total simulation budget**, not just a count of predictive models. We separate candidate, validation, and test contracts; fit preprocessing on permitted development data only; and compare prediction methods on shared Monte Carlo reference labels. Simulation, fitting, tuning, and inference costs must be reported consistently.

**Week 7 → Week 8 is one connected study:** Week 7 saves and freezes a configuration manifest; Week 8 verifies it before evaluating held-out results. Do **not** inspect test labels while selecting a model.

The default teaching experiment uses **1,200 synthetic contracts**. A **10,000-contract** research-scale switch is provided in Weeks 7–8 but was not run during the original course preparation. Neither scale comes with a guaranteed model ranking.

## Expected student work and final outputs

- **Each week:** Work through the notebook, answer guided reading questions, complete core code exercises, and bring one finding plus one question to the mentor meeting. Optional extensions are truly optional.
- **Week 8:** Submit a **4–6-page research report**, final evaluation tables and figures, reproducibility files, and an initial presentation outline.
- **Weeks 9–10:** Finalize and rehearse a group presentation and prepare for questions. Three students rotate implementation, verification, and research-communication responsibilities.

No method is considered successful simply because it uses AI. Careful negative results, honest runtime comparisons, and discussion of Monte Carlo uncertainty are valid research contributions.

## Reading list

The complete reading guidance and source links are in [the syllabus](SYLLABUS.md#9-selected-readings-and-sources) and the notebooks. Principal references are:

1. Mary R. Hardy (2003), *Investment Guarantees: Modeling and Risk Management for Equity-Linked Life Insurance*.
2. Bauer, Kling and Russ (2008), *A Universal Pricing Framework for Guaranteed Minimum Benefits in Variable Annuities*.
3. Gan and Lin (2015), *Valuation of Large Variable Annuity Portfolios under Nested Simulation: A Functional Data Approach*.
4. Feng, Tan and Zheng (2020), *Efficient Simulation Designs for Valuation of Large Variable Annuity Portfolios*.
5. Quan and Feng (2026), *Warm-Started Projected Robbins–Monro for Portfolio-Scale Variable Annuity Fee Setting* (mentor-supplied working manuscript).

The separate contract-similarity research manuscript is **not** an assigned student reading. We link or cite literature rather than upload publisher-copyrighted texts.

## Repository structure

    .
    ├── README.md
    ├── SYLLABUS.md
    ├── requirements.txt
    └── readings/
        ├── Week 1/     01_Contracts_and_GMAB_Payoffs.ipynb
        ├── Week 2/     02_Risk_Neutral_Monte_Carlo.ipynb
        ├── Week 3/     03_Contract_Similarity_and_Nearest_Neighbours.ipynb
        ├── Week 4/     04_Compress_Simulate_Predict.ipynb
        ├── Week 5/     05_Supervised_Valuation_Models.ipynb
        ├── Week 6/     06_Learning_Contract_Similarity.ipynb
        ├── Week 7/     07_Experimental_Design_and_Freeze.ipynb
        └── Week 8/     08_Final_Evaluation_and_Research_Report.ipynb

## Usage and attribution

This is a teaching/research repository for a University of Waterloo WiM DRP mentorship project. **No open-source license has been selected.** Public GitHub visibility does not automatically grant permission to redistribute or modify these materials beyond rights provided by law or GitHub's terms. Contact the repository owner for reuse requests.

*Project plan: Winter 2027. Dates, meeting arrangements, and official event requirements will be confirmed with the mentor and WiM program.*