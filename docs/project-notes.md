# Project Notes

## Purpose

Murshid Mini is a four-stage educational project for **LLM Application Engineering — SDA-AIE-213**. It demonstrates application-engineering practices around a language-model-shaped interface without requiring a live model in the core project.

The core is deliberately deterministic. The purpose is to make request handling, data boundaries, tool authorization, tests, and caching observable before introducing the variability of a real model.

## Design decisions

### A deterministic mock client

The core uses `FakeClient` with a finite set of predefined responses. This keeps the main exercise reproducible and free of API keys, paid services, model weights, and additional packages. It must not be described as a live LLM or as a model-quality evaluation.

### Structured tickets before tool access

Application-status questions are represented as a four-field ticket. The application validates the schema and supported values before routing the request. A missing reference leads to clarification rather than a tool call.

### Separate identity from user input

The demo session is supplied by the test fixture. A reference number does not establish ownership. The read-only status lookup checks the trusted session against the record owner and returns a controlled response when access is not authorized.

### Cache only public, exact-match questions

The Day 4 cache accepts only the two exact public directory questions. It uses a 300-second TTL and a key containing question text, language, model label, prompt version, and directory version. Status requests and ticket overrides bypass the cache. The cache stores only the public answer and creation time.

### Keep operational metrics minimal

The metrics record trace ID, mode, response code, cache-hit flag, mock-call count, tool-call count, and local Python execution time. They deliberately omit question text, application references, owners, tickets, and raw answers.

## Synthetic data

The dataset is documented in [the dataset card](../data/README.md) and exported in [synthetic_data.json](../data/synthetic_data.json).

| Reference | Owner | Status |
|---|---|---|
| `REF-1001` | `demo-user-1` | `under_review` |
| `REF-1002` | `demo-user-2` | `approved` |
| `REF-9999` | Not present | Used for a missing-record case |

The public service directory contains one Arabic and one English answer. Every record and identity is fictional and intended only for this exercise.

## Four-day implementation

| Day | Main engineering focus | Notebook cells |
|---|---|---:|
| 1 | Synthetic directory, mock client, baseline response contract | 1–3 |
| 2 | Ticket extraction, validation, read-only tool, ownership | 4–6 |
| 3 | Versioned prompt artifact, input guards, pipeline, golden cases | 7–10 |
| 4 | Exact-match public cache, metrics, regression, export | 11–14 |

The complete implementation remains in one code-only notebook: [`notebooks/murshid.ipynb`](../notebooks/murshid.ipynb).

## Observed execution evidence

The committed core results were exported from the Colab-generated `murshid_submission.zip`. They are separate from the optional homework reports.

| Evidence | Observed result |
|---|---|
| Day 1 | Three expected mock responses |
| Day 2 | Five scenarios; tool-call counts `1, 0, 1, 0, 1` |
| Day 3 | Ten golden cases passed |
| Day 4 | Three mock calls without cache; one with cache |
| Day 4 | Cache-hit sequence `false, true, true` |
| Day 4 | Cache hit rate 66.7% |
| Core external model API calls | Zero |

Trace IDs and local timing values belong to the recorded run and may change on another execution. Python latency is not model-inference latency. The hypothetical token-cost example is illustrative only.

## Reproducibility

Open the cumulative notebook in Google Colab, select Python 3 with CPU, restart the runtime, and run all 14 cells in order. The notebook writes seven JSON files under `murshid_outputs/` and packages them into `murshid_submission.zip`. Download the notebook separately; the ZIP contains the JSON artifacts only.

The repository also provides offline standard-library tests:

```bash
python -m unittest discover -s tests -v
```

GitHub Actions runs the same suite on Python 3.11 and 3.13 for pushes and pull requests to `main`.

## Optional real-model homework

Day 1 and Day 4 homework are separate notebooks using `google/flan-t5-small` locally. They do not change the core `mode=mock` application. Package and model downloads require internet, but the notebooks make zero external inference API calls and require no paid API key.

The Day 1 and Day 4 setup use `sentencepiece==0.2.1` as a compatibility adjustment for the Python 3.13 Colab runtime. The original `0.2.0` source distribution failed to build; the compatible wheel installed successfully. The model revision and exercise logic remain unchanged.

## Limitations

- The core does not load a live language model.
- Routing and ticket extraction are finite and rule-based.
- The prompt template is a versioned teaching artifact; the mock client does not execute it.
- All application data and identities are synthetic.
- The trusted session fixture demonstrates an ownership check, not production authentication.
- Input guards cover selected examples only.
- Ten passing golden cases do not establish general model quality, comprehensive security, or production readiness.
