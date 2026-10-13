# AI for Faster Variable Annuity Pricing through Learned Contract Similarity

**Winter 2027 · Women in Mathematics (WiM) Directed Reading & Research Program · University of Waterloo**  
**Mentor:** Hao Quan · **Format:** three undergraduate researchers · eight research weeks plus two presentation weeks

This undergraduate research project asks a focused question:

> Under a fixed Monte Carlo simulation budget, can we estimate variable annuity guarantee values more accurately by learning which insurance contracts are similar?

We compare ordinary nearest-neighbour value transfer, unsupervised representative selection, supervised prediction, and explicitly learned distances. The goal is to run **reproducible, fair comparisons** rather than assume a more complex model must win.

## Project scope

- **Contract:** a deliberately simplified guaranteed minimum accumulation benefit (GMAB) with a given annual fee.
- **Learning target:** the expected discounted *gross guarantee top-up* per unit premium; dollar liability is premium times normalized value.
- **Valuation:** Monte Carlo under a risk-neutral lognormal fund model; an analytic put value is used only for diagnostic checks and evaluation.
- **Not in scope:** mortality, lapses, withdrawals, multiple riders, full contract valuation and solving an implicit fair-fee equation. Those topics are contextual readings, not required implementation tasks.
- **Baseline:** direct simulation of all contracts; the compressor–simulator–predictor experiments compare representative-contract selection and prediction under budget accounting.

## Weekly Jupyter notebooks

Every notebook includes an overview, learning objectives, theoretical explanations, executable worked examples, reading questions, coding homework, and a submission checklist. All code needed for the worked examples is in that notebook; no external dataset is needed.

| Week | Topic | Notebook |
|:---:|---|---|
| 1 | VA guarantees, GMAB payoffs and contract features | [01 · Contracts and GMAB Payoffs](notebooks/01_Contracts_and_GMAB_Payoffs.ipynb) |
| 2 | Risk-neutral Monte Carlo pricing, sampling error and direct-all baseline | [02 · Risk-Neutral Monte Carlo](notebooks/02_Risk_Neutral_Monte_Carlo.ipynb) |
| 3 | Feature scaling, contract similarity and CSM-style nearest neighbours | [03 · Contract Similarity and Nearest Neighbours](notebooks/03_Contract_Similarity_and_Nearest_Neighbours.ipynb) |
| 4 | Representative selection, K-means and compressor–simulator–predictor | [04 · Compress, Simulate, Predict](notebooks/04_Compress_Simulate_Predict.ipynb) |
| 5 | Ridge regression, additive splines and random forests | [05 · Supervised Valuation Models](notebooks/05_Supervised_Valuation_Models.ipynb) |
| 6 | Learning nonnegative feature weights to define a task-aligned distance | [06 · Learning Contract Similarity](notebooks/06_Learning_Contract_Similarity.ipynb) |
| 7 | Simulation budgets, validation-only selection and freezing the study | [07 · Experimental Design and Freeze](notebooks/07_Experimental_Design_and_Freeze.ipynb) |
| 8 | Held-out evaluation, independent MC audit and research report | [08 · Final Evaluation and Research Report](notebooks/08_Final_Evaluation_and_Research_Report.ipynb) |

**Weeks 9–10:** prepare the research presentation, rehearse, answer questions and hand off reproducible results.

## Getting started

Python 3.12 is recommended. From the repository root:

```bash
python -m venv .venv
source .venv/bin/activate   # macOS / Linux
python -m pip install -r requirements.txt
python -m jupyter lab
```

On Windows PowerShell, activate the environment with `.venv\Scripts\Activate.ps1` instead. Open notebooks in increasing week order and select the new Python environment as the kernel.

The notebooks deliberately ship **without saved outputs**; run cells to produce the tables and figures. Homework prompts are provided but student responses are not filled in.

## Study protocol and reproducibility

Weeks 7 and 8 support two configurations through `RUN_RESEARCH_SCALE`:

| Configuration | Contracts | Seeds | Representative counts | Per-pipeline contract-path evaluation ceiling |
|---|---:|---:|---|---:|
| Classroom (default; validated) | 1,200 | 2 | 40 or 80 | 262,144 |
| Optional research scale (not run during preparation) | 10,000 | 5 | 100 or 200 | 2,000,000 |

- All predictors in each experimental condition use the **same simulated reference labels**.
- Representative and validation simulations count toward the pipeline budget.
- Fit preprocessing on training/candidate data only and reserve a held-out test set.
- Week 7 saves `outputs/week07/frozen_protocol.json` and prints its SHA-256. For sequential research, record that digest and put it in Week 8's `EXPECTED_WEEK7_SHA256` before reading test results.
- Report contract-level errors separately from portfolio aggregation error, and account for training, selection, simulation and inference runtime.

The simplified payoff happens to have a closed-form benchmark. This course **does not claim that machine learning beats the analytic solution**, nor that results extend automatically to production-grade VA valuation.

## Course documents and readings

- [Application and ten-week syllabus (PDF)](guides/Application_and_Syllabus.pdf) · [Editable DOCX](guides/Application_and_Syllabus.docx)
- [Application text](guides/Application_Text.txt) · [Structured application fields](guides/Application_Fields.json)
- [Selected bibliography (BibTeX)](guides/Bibliography.bib)
- [Mentor notes](guides/Mentor_Notes.txt) · [Validation report](guides/Validation_Report.txt)
- [Original detailed setup instructions](README.txt)

Core references cover Hardy (2003); Bauer, Kling & Russ (2008); Gan & Lin (2015); Feng, Tan & Zheng (2020); and Quan & Feng's mentor-provided fair-fee manuscript. The mentor's separate contract-similarity manuscript is **not** an assigned reading. Publisher texts are not distributed in this repository.

## Repository and reuse

The course is designed for undergraduate teaching and research. No open-source license has been selected for these materials; publication on GitHub does not, by itself, grant a license to redistribute or modify them. Contact the repository owner for reuse permission.