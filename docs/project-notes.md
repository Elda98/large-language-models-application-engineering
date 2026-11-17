# Murshid Mini — Repository Audit and Technical Notes

## Audit scope and basis

This document records the state verified from the repository on 17 November 2026, the uploaded cumulative Colab notebook, the committed homework notebooks, the generated result files, and the instructor's public `docs/git.md` checklist. It distinguishes the required core mock project from optional real-local-model homework.

## Current repository state

- The cumulative core notebook is present at `notebooks/murshid.ipynb`.
- The notebook contains 14 code-only cells: Day 1 cells 1–3, Day 2 cells 4–6, Day 3 cells 7–10, and Day 4 cells 11–14.
- Core JSON outputs are committed under `results/core/`, with an index at `results/README.md`.
- Synthetic dataset documentation and export are committed under `data/`.
- Technical documentation and a root README are present.
- Separate optional homework notebooks and available reports are stored under `homework/day1` through `homework/day4`.
- An offline standard-library test suite is committed at `tests/test_notebook.py`.

## Core notebook verification

The uploaded learner notebook was compared with the official reference notebook. The functional cell code matched; differences were limited to missing/leading cell-label comments and trailing blank lines. The repository copy now uses the official labeled cell source blocks while preserving the learner notebook's captured outputs and execution counts.

The notebook's 14 cells are saved with Colab execution counts and the expected milestone outputs. The seven JSON result files in `results/core/` were copied from the Colab-generated `murshid_submission.zip` produced by Cell 14. Separately, the GitHub Actions workflow executes the notebook cells from a clean test directory and validates the outputs on Python 3.11 and 3.13.

Observed checks from that run:
- Day 1: three expected mock responses.
- Day 2: codes `under_review`, `needs_clarification`, `not_found`, `unsupported_action`, `access_denied`; tool counts 1, 0, 1, 0, 1.
- Day 3: ten golden cases passed (10/10).
- Day 4: mock calls 3 without cache and 1 with cache; hits `false, true, true`; hit rate 66.7%; zero external API calls.
- Final mode: `mock`; hypothetical cost is explicitly an illustration, not billed usage.

Trace IDs and Python latency values in the saved result archive are from that Colab run and vary between runs. CI execution is a separate reproducibility check; it does not replace the Colab-generated result archive.

## Core architecture and boundaries

Input → input guard → rule-based ticket extraction → schema/business-rule validation → public mock answer OR read-only status tool → output validation → response and minimal metrics.

- `FakeClient.generate`: deterministic finite lookup; not a live model and does not call an API.
- `ask`: baseline response, trace ID and measured Python latency.
- `extract_ticket`: limited rule/regex parser, not general NLP.
- `validate_ticket`: exact schema and value checks.
- `check_status`: read-only local lookup with owner comparison.
- `route`: public FAQ, status, clarification and unsupported-action routing.
- `build_prompt`: versioned educational prompt artifact; the mock does not execute a real prompt.
- `guard_input`: limited length, sample instruction-override and digit-pattern checks; not comprehensive security.
- `run_pipeline`: validation and request processing.
- `evaluate`: ten fixed behavioral cases, without network or model judge.
- `run_app`: exact public FAQ cache and minimal operational metrics.

## Synthetic data and fields

See [the dataset card](../data/README.md) and [the synthetic data JSON](../data/synthetic_data.json).

- Public Arabic and English service directory answers.
- `REF-1001`: owner `demo-user-1`, status `under_review`.
- `REF-1002`: owner `demo-user-2`, status `approved`.
- `REF-9999`: intentionally absent.
- Trusted session fixture: `demo-user-1`.

The ticket schema contains exactly `service`, `city`, `request_id`, and `needs_clarification`. A reference number is not proof of ownership. The trusted session is test setup, not an identity supplied by the question. All fixtures are synthetic.

## Day 4 cache design

The cache is eligible only for the two exact public FAQ strings. It uses a 300-second TTL and a key containing question text, language, model label, prompt version, and directory version. A new prompt/directory version is a cache miss. Status requests, ticket overrides and non-exact questions bypass the cache. Entries store only public answers and creation times, not status, owner or session data.

Metrics omit questions, references, owners, tickets and raw answers. The comparison is based on mock-call counts, not actual external service latency or financial savings.

## Generated core outputs

The actual notebook writes the following JSON files under `murshid_outputs/` and packages them into `murshid_submission.zip`:

- `baseline_results.json`
- `day2_results.json`
- `day3_results.json`
- `day4_results.json`
- `final_report.json`
- `metrics.json`
- `synthetic_data.json`

Committed copies produced during the clean-namespace run are in `results/core/`. The ZIP contains JSON artifacts, not the notebook.

## Homework separation and verification status

- Day 1: separate real-local-model notebook and `live_demo_result.json`.
- Day 2: separate tool-validation notebook; saved output reports seven checks passed.
- Day 3: separate error-handling notebook; the uploaded GitHub copy was cleaned to remove one empty trailing code cell and renamed to a clean filename.
- Day 4: separate three-paraphrase evaluation notebook and `evaluation_result.json`. It uses a local `google/flan-t5-small` model, reports zero external inference API calls, and includes a manual review. The `reference+number` indicator is only a keyword proxy, not semantic accuracy.

The real-local-model homework remains distinct from the core `mode=mock`. The Day 1 and Day 4 setup cells use `sentencepiece==0.2.1` as a documented compatibility adjustment: the original `0.2.0` source distribution failed to build in Python 3.13, while the `0.2.1` platform wheel installed successfully. The model and generation/evaluation logic were not changed.

## Automated checks

Run from the repository root:

```bash
python -m unittest discover -s tests -v
```

Ten offline standard-library test methods cover the 14-cell structure and order, full execution and export, daily milestones, repeat execution, standard-library-only imports, ticket schema rejection, ownership and read-only behavior, input guards, cache TTL/version isolation and status exclusion, and consistency of the seven committed result files. GitHub Actions runs the suite on Python 3.11 and 3.13.

## Remaining submission checks

1. Confirm the course provider's visibility instructions and that the instructor can open this public repository.
2. If the instructor expects daily core commits, note that the core notebook itself was added as a cumulative final notebook commit; existing earlier commit history also contains homework and documentation changes, but it does not separately show four historical core-notebook milestones. Do not manufacture backdated commits.
3. Open the notebook and each report in GitHub after upload.
4. Submit the repository root link through the instructor's specified channel only after confirming visibility/access.

## Limitations

The core is mock-mode, uses finite rule-based routing and synthetic data, has no real authentication, and has educational-only guards. It is not production-ready and does not measure real LLM quality or paid API performance.
