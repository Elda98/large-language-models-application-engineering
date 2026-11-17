# Technical Documentation — Murshid Mini

## 1. Runtime and scope

The required core project is a **Python 3, CPU-only, standard-library** teaching application. It runs in `mode=mock` with `MODEL='training-mock'`. `FakeClient` is a finite deterministic simulator. It does not load a language model, call an external API, train a model, or provide a production service.

All identities, requests, and statuses are synthetic fixtures. Optional Day 1 and Day 4 homework notebooks load `google/flan-t5-small` locally; they remain separate from the core project.

## 2. Request architecture

```text
Question
  -> run_app
      -> exact public FAQ and eligible cache lookup
      -> run_pipeline on a cache miss or non-cacheable request
          -> input guard
          -> ticket extraction
          -> schema and business-rule validation
          -> route
              -> public FAQ: FakeClient
              -> status: read-only status tool and ownership check
              -> missing reference: clarification
              -> unsupported or invalid: controlled response
          -> response validation
      -> minimal metrics
  -> structured result
```

## 3. Components

| Component | Responsibility | Boundary |
|---|---|---|
| `FakeClient.generate` | Finite lookup and mock-call counting | Not a live LLM |
| `ask` | Day 1 response contract, trace ID, and Python latency | Local timing only |
| `extract_ticket` | Extracts service, city, and reference using limited rules | Not general NLP |
| `validate_ticket` | Checks exact keys, value types, and allowed values | Rejects malformed structure |
| `check_status` | Looks up a synthetic status after ownership verification | Read-only; no mutation |
| `route` | Chooses FAQ, clarification, status, or refusal path | Finite routing |
| `build_prompt` | Stores a versioned teaching prompt artifact | Mock does not execute it |
| `guard_input` | Applies selected length, instruction, and number-pattern checks | Educational examples only |
| `run_pipeline` | Coordinates Day 3 validation and processing | Uses synthetic data |
| `evaluate` | Runs ten fixed golden cases | No model judge or network |
| `run_app` | Adds public FAQ caching and minimal metrics | Status requests bypass cache |

## 4. Synthetic data and ticket schema

The dataset is documented in [the dataset card](../data/README.md) and exported in [synthetic_data.json](../data/synthetic_data.json).

### Public directory

- `ar`: `يمكنك متابعة حالة طلبك باستخدام رقم المرجع.`
- `en`: `You can check your application status using its reference number.`

### Application records

| Reference | Owner | Status |
|---|---|---|
| `REF-1001` | `demo-user-1` | `under_review` |
| `REF-1002` | `demo-user-2` | `approved` |
| `REF-9999` | absent | not found |

The configured test session is `demo-user-1`. A reference number is not proof of ownership. The tool checks the trusted session fixture against the record owner before returning a status.

### Ticket schema

A valid ticket has exactly four fields:

- `service`: `service_info`, `application_status`, or `other`.
- `city`: `Riyadh`, `Jeddah`, or `unknown`.
- `request_id`: null or `REF-` followed by four ASCII digits.
- `needs_clarification`: Boolean.

An application-status request without a reference must ask for clarification and must not call the status tool.

## 5. Validation and authorization boundaries

- Exact schema keys and value types are checked before routing.
- Unsupported actions such as cancellation are rejected.
- The status tool is read-only.
- `REF-1001` is available to its owner, `demo-user-1`.
- `REF-1002` belongs to `demo-user-2`; the `demo-user-1` session receives a denied response without the actual status.
- `REF-9999` is absent and returns a controlled unavailable response.
- Input guards cover selected examples only. They are not comprehensive prompt-injection, PII, or application-security defenses.

## 6. Day 4 cache design

Only the two exact public directory questions (`FAQ_AR` and `FAQ_EN`) are eligible.

- TTL: **300 seconds**.
- Cache key: `(text, language, MODEL, prompt_version, directory_version)`.
- Cache entries contain the public answer and creation time only.
- Prompt or directory version changes produce a cache miss.
- Expiry at the TTL boundary is a miss.
- Status requests, ticket overrides, and non-exact questions bypass the cache.
- Metrics omit questions, references, owners, tickets, and raw answers.

### Controlled comparison

| Measure | Without cache | With initially empty cache |
|---|---:|---:|
| Requests | 3 | 3 |
| Mock calls | 3 | 1 |
| Cache hits | 0 | 2 |
| Hit rate | 0% | 66.7% |

The sequence is `false, true, true`. This is a deterministic comparison of mock-call counts, not a measured production latency improvement or a real financial saving.

## 7. Output artifacts

The core notebook writes these JSON files under `murshid_outputs/` and packages them into `murshid_submission.zip`:

- `baseline_results.json`
- `day2_results.json`
- `day3_results.json`
- `day4_results.json`
- `final_report.json`
- `metrics.json`
- `synthetic_data.json`

The committed copies are in [results/core](../results/core/). The ZIP contains JSON artifacts, not the notebook. Trace IDs and timing values vary between runs. `latency_ms` measures local Python execution, not model-inference latency. The cost calculation in the final report uses hypothetical rates and does not represent actual API usage or billing.

## 8. Tests and reproducibility

Run from the repository root:

```bash
python -m unittest discover -s tests -v
```

The ten offline standard-library test methods are:

1. `test_notebook_structure_and_daily_cell_order` — 14 code cells and correct day/cell labels.
2. `test_clean_run_regression_and_export` — full execution, golden suite, cache counts, JSON artifacts, and ZIP.
3. `test_each_daily_milestone_runs_from_a_fresh_namespace` — each day's cumulative checkpoint.
4. `test_repeat_execution_is_stable` — repeat execution without hidden state.
5. `test_core_imports_are_standard_library_only` — no third-party dependencies in the core notebook.
6. `test_ticket_schema_rejections` — valid and invalid ticket structures.
7. `test_ownership_and_read_only_behavior` — owner check, non-disclosure, and record immutability.
8. `test_input_guards_and_golden_contract` — selected input guards and ten golden cases.
9. `test_cache_ttl_versions_exclusions_and_minimal_metrics` — TTL, version changes, status exclusion, and metric fields.
10. `test_committed_result_artifacts_are_complete_and_consistent` — required result files and stable outcomes.

GitHub Actions runs the suite on Python 3.11 and 3.13 for pushes and pull requests to `main`. These checks validate the listed teaching fixtures; they do not prove production security or general LLM quality.

## 9. Limitations

- No live LLM in the core project.
- No real authentication, real service records, or production deployment.
- Finite rule-based extraction and routing.
- Educational input guards only.
- Synthetic fixtures and fixed golden tests only.
- Local latency and illustrative token costs are not production performance or billing measurements.
