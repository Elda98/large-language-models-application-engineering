# Murshid Mini — LLM Application Engineering

A four-day educational project built during **LLM Application Engineering (SDA-AIE-213)** with instructor **Meaad Al-Marri**. The project demonstrates application-engineering patterns around a controlled, synthetic service-assistant workflow.

> **Submission status:** The homework notebooks and available homework evidence are present. The required cumulative core notebook at `notebooks/murshid.ipynb` is not yet present in this repository. Add the actual Colab notebook before submitting this repository.

## Repository contents

### Homework
- [Day 1 — Real local model notebook](homework/day1/day1_real_model_Raghad.ipynb)
- [Day 1 — Inference report](homework/day1/live_demo_result.json)
- [Day 2 — Tool validation homework](homework/day2/day2_tools_raghad.ipynb)
- [Day 3 — Error handling homework](homework/day3/day3_errors_raghad%20(1).ipynb)
- [Day 4 — Real-model evaluation notebook](homework/day4/day4_evaluation_raghad.ipynb)
- [Day 4 — Evaluation report](homework/day4/evaluation_result.json)

### Core project
- **Required cumulative notebook:** `notebooks/murshid.ipynb` — pending upload.
- **Core results:** daily JSON outputs and the final report — not currently present in this repository.

## How to run

1. Open a notebook in Google Colab.
2. Save your own copy to Google Drive.
3. Use the runtime and dependency instructions inside that notebook.
4. Run cells from top to bottom and inspect the outputs.
5. For the core project, use one cumulative notebook for Days 1–4. Keep optional homework notebooks separate.
6. The Day 1 and Day 4 homework use a local `google/flan-t5-small` model and report zero external inference API calls. Their setup requires internet access to download packages/model weights; no paid API key is required.
7. The core project is a **mock-mode educational workflow**, not a deployed service or a live model-backed application.

## Evaluation evidence currently committed

- Day 1 local-model report: one inference result.
- Day 2 tool homework: notebook output records 7 passing checks.
- Day 3 error homework: notebook output demonstrates caught validation errors; rerun the notebook to regenerate a fresh execution record.
- Day 4 local-model evaluation: three paraphrase cases, 3 keyword-indicator hits, and zero external inference API calls. The keyword indicator is a proxy, not semantic accuracy.

## Scope and limitations

The work uses synthetic teaching fixtures. It is not production-ready, does not provide real authentication, and must not be used with real personal or service records. Local-model homework results are distinct from the core project's mock-mode results.

See [technical project notes](docs/project-notes.md) for the verified scope and outstanding submission items.

## Attribution

LLM Application Engineering — SDA-AIE-213  
Instructor: Meaad Al-Marri  
Training context: SDAIA Academy ([GitHub](https://github.com/SDAIAAcademy))

This attribution does not imply endorsement, certification, or authorship of this implementation by SDAIA Academy.
