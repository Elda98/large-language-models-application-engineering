# Murshid Mini — LLM Application Engineering

A four-day educational project built during **LLM Application Engineering — SDA-AIE-213**, under instructor **Meaad Al-Marri**. The project demonstrates application-engineering patterns around a fictional, synthetic service assistant.

> **Core project:** `mode=mock`. `FakeClient` is a finite, deterministic simulator, not a live language model. The core notebook uses Python's standard library, CPU, and no paid API or model download.

## Project structure

### Core project
- [Cumulative notebook — Days 1–4](notebooks/murshid.ipynb)
- [Synthetic dataset card](data/README.md)
- [Synthetic dataset export](data/synthetic_data.json)
- [Technical documentation](docs/technical.md)
- [Project notes and limitations](docs/project-notes.md)
- [Core results](results/core/) · [execution results guide](results/README.md) · [offline tests](tests/test_notebook.py)

### Optional homework (separate notebooks)
- [Day 1 — Real local model](homework/day1/day1_real_model_Raghad.ipynb) · [report](homework/day1/live_demo_result.json)
- [Day 2 — Tools and validation](homework/day2/day2_tools_raghad.ipynb)
- [Day 3 — Error handling](homework/day3/day3_errors_raghad.ipynb)
- [Day 4 — Three-answer evaluation](homework/day4/day4_evaluation_raghad.ipynb) · [report](homework/day4/evaluation_result.json)

The optional Day 1 and Day 4 exercises load `google/flan-t5-small` locally and report zero external inference API calls. They are distinct from the mock core.

## Four-day build

| Day | Scope | Core cells |
|---|---|---|
| 1 | Synthetic public directory, deterministic mock client, baseline answer contract | 1–3 |
| 2 | Ticket extraction and validation, read-only status tool, ownership checks | 4–6 |
| 3 | Versioned prompt artifact, input guards, pipeline and ten golden cases | 7–10 |
| 4 | Exact-match public FAQ cache, metrics, regression, report and export | 11–14 |

## Run the core project

1. Open `notebooks/murshid.ipynb` in Google Colab.
2. Select Python 3 and CPU (no accelerator).
3. Restart the runtime and run all cells from top to bottom.
4. Inspect each day's printed checks and the final report.
5. Download `murshid_submission.zip` from the Colab Files panel; it contains JSON results, not the notebook itself.
6. Keep homework notebooks separate from this cumulative core notebook.

Run the offline checks from the repository root with `python -m unittest discover -s tests -v`.\n\nThe core requires no installation, model download, API key, GPU or paid service. The homework model demonstrations require internet to download Python packages and model weights, but not a paid endpoint.

## Results and interpretation

The core notebook records:
- Day 1: three prescribed mock responses and the baseline output contract.
- Day 2: five status/tool scenarios with ownership enforcement and no record mutation.
- Day 3: ten fixed behavioral cases.
- Day 4: three requests without cache versus one mock call with cache; cache hits `false, true, true` and a 66.7% hit rate.
- Final report: regression results, illustrative hypothetical cost calculation and documented limitations.

Python latency is local execution time, not LLM inference time. The hypothetical USD amount is an example using invented rates, not a bill or measured saving. Ten passing fixtures do not establish general reliability or production security.

## Dataset and safety boundaries

All directory answers, references, owners and statuses are synthetic teaching fixtures. The status tool is read-only and checks the trusted session fixture against record ownership. This is an educational authorization example, not production authentication. Never add real service records, national IDs, passwords, tokens or other people's private information.

## Attribution

**LLM Application Engineering — SDA-AIE-213**  
Instructor: **Meaad Al-Marri**  
Training context: **SDAIA Academy** ([GitHub](https://github.com/SDAIAAcademy))

This attribution does not imply endorsement or certification by SDAIA Academy.
