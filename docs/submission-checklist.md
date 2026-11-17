# Project and Submission Checklist

This checklist maps the course's published requirements to files currently present in this repository. It distinguishes completed evidence from items that still require the learner's final confirmation.

**Audit date:** 17 November 2026  
**Repository:** [Elda98/large-language-models-application-engineering](https://github.com/Elda98/large-language-models-application-engineering)

## 1. Required four-day core project

| Course requirement | Evidence in this repository | Status |
|---|---|---|
| One cumulative notebook for Days 1–4 | [`notebooks/murshid.ipynb`](../notebooks/murshid.ipynb), 14 code cells | [x] |
| Preserve daily sequence: Day 1 (1–3), Day 2 (4–6), Day 3 (7–10), Day 4 (11–14) | Official day/cell labels in the notebook | [x] |
| Notebook contains code cells only; explanations remain in the guides | Notebook cell types and README/docs | [x] |
| Core uses Python 3, CPU, standard library, no model download or API key | Notebook imports and README runtime instructions | [x] |
| Core is clearly identified as `mode=mock`; `FakeClient` is not described as a real LLM | README, technical documentation, final report | [x] |
| Day 1 baseline responses and empty-input behavior | [`baseline_results.json`](../results/core/baseline_results.json) | [x] |
| Day 2 five scenarios, read-only tool and ownership checks | [`day2_results.json`](../results/core/day2_results.json) | [x] |
| Day 3 ten golden cases | [`day3_results.json`](../results/core/day3_results.json), 10/10 | [x] |
| Day 4 exact public FAQ cache, versioning, expiry, and status exclusion | Notebook Cell 12 and [`day4_results.json`](../results/core/day4_results.json) | [x] |
| Day 4 final report, metrics and synthetic-data export | [`results/core/`](../results/core/) | [x] |
| Export ZIP contains JSON artifacts; notebook is saved separately | Notebook Cell 14 and run instructions | [x] |
| Restart and run from the first cell | Notebook contains saved outputs; automated clean-namespace execution runs in CI | [x] |

## 2. Required technical and data documentation

| Course requirement | Evidence | Status |
|---|---|---|
| Professional README with problem, approach, scope and limitations | [`README.md`](../README.md) | [x] |
| Setup, run and use instructions | README and notebook | [x] |
| Dataset source and field definitions | [`data/README.md`](../data/README.md) | [x] |
| Synthetic dataset export | [`data/synthetic_data.json`](../data/synthetic_data.json) | [x] |
| Pipeline, components and response flow | [`docs/technical.md`](technical.md) | [x] |
| Ticket schema and validation behavior | Technical documentation and notebook | [x] |
| Read-only tool and authorization fixture | Technical documentation and notebook | [x] |
| Cache eligibility, key, TTL and exclusions | Technical documentation and notebook | [x] |
| Test strategy and limitations | Technical documentation and test suite | [x] |
| Honest final report with `mode=mock` and hypothetical costs labeled | [`final_report.json`](../results/core/final_report.json) | [x] |

## 3. Completed optional homework

Homework notebooks must remain separate from the cumulative core notebook.

| Day | Submission evidence | Verified content | Status |
|---|---|---|---|
| Day 1 — Real model | [Notebook](../homework/day1/day1_real_model_Raghad.ipynb) · [Report](../homework/day1/live_demo_result.json) | Local FLAN-T5 run, generated answer, tokens, timing, zero external inference API calls | [x] |
| Day 2 — Tools | [Notebook](../homework/day2/day2_tools_raghad.ipynb) | Structured validation/dispatch; saved output reports 7 passing checks | [x] |
| Day 3 — Error handling | [Notebook](../homework/day3/day3_errors_raghad.ipynb) | Three caught `ValueError` cases, valid input accepted; empty trailing cell removed | [x] |
| Day 4 — Evaluation | [Notebook](../homework/day4/day4_evaluation_raghad.ipynb) · [Report](../homework/day4/evaluation_result.json) | E01–E03, token counts, inference time, generation settings, manual review, zero external API calls | [x] |

The Day 1 and Day 4 notebooks use `sentencepiece==0.2.1` as a documented compatibility adjustment for the Python 3.13 Colab runtime. The supplied `0.2.0` source distribution failed to build there; the adjustment does not change the model task.

The Day 4 keyword check is a lexical proxy for the words “reference” and “number”, not a semantic-accuracy score. The manual assessment is reported separately.

## 4. Repository quality and automation

| Quality item | Evidence | Status |
|---|---|---|
| Repository has a clear README and navigable links | Root README | [x] |
| Technical docs and dataset docs exist | `docs/` and `data/` | [x] |
| Core output files are committed | `results/core/` | [x] |
| Offline standard-library tests are present | [`tests/test_notebook.py`](../tests/test_notebook.py) | [x] |
| Tests run automatically on push and pull request | [GitHub Actions workflow](../.github/workflows/core-tests.yml) | [x] |
| Automated suite checks ten cases and committed result consistency | [tests/test_notebook.py](../tests/test_notebook.py) | [x] |
| Python 3.11 workflow job passes | [Latest Actions run](https://github.com/Elda98/large-language-models-application-engineering/actions/workflows/core-tests.yml) | [x] |
| Python 3.13 workflow job passes | [Latest Actions run](https://github.com/Elda98/large-language-models-application-engineering/actions/workflows/core-tests.yml) | [x] |
| Relative links in the root README resolve to repository files | Repository tree audit | [x] |
| Local runtime outputs, caches and environment files have ignore rules; an automated scan found no obvious API-key, token, email or phone-number patterns in submitted notebooks/results | `.gitignore` and repository content scan | [x] |

## 5. Final handoff checks

These require the learner's own final confirmation and should not be inferred from repository contents alone.

- [x] The repository is public.
- [x] The repository root identifies the program, instructor and SDAIA Academy GitHub account.
- [x] The core notebook and each completed homework are stored as separate files.
- [ ] Review the latest notebook outputs and all uploaded files personally for private data before sending; automated pattern scans are not a substitute for a human review.
- [ ] Open the repository in a signed-out/private browser window to confirm public access.
- [ ] Send the repository root link to Meaad Al-Marri through the submission channel she specifies.
- [ ] Keep this same repository link for later corrections.

## 6. Version-history note

The repository contains a chronological commit history, but the cumulative core notebook was uploaded as a completed 14-cell notebook rather than as four separate historical notebook commits. The history must not be rewritten or backdated to imply commits that did not occur. Future changes should use descriptive commit messages and preserve the existing history.

Suggested future messages:

- `Day 1: add mock client and response contract`
- `Day 2: add validated tickets and authorized status lookup`
- `Day 3: add input guards and ten regression cases`
- `Day 4: add cache experiment and final report`

## 7. Re-run the quality checks

From the repository root:

```bash
python -m unittest discover -s tests -v
```

The GitHub Actions workflow runs the same checks on Python 3.11 and 3.13. A passing test suite validates the listed synthetic fixtures; it does not establish production security or general LLM quality.
