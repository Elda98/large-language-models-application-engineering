# Technical Documentation — Murshid Mini

## 1. Runtime and scope

The required core project is a **Python 3, CPU-only, standard-library** teaching application. It runs in `mode=mock` with `MODEL='training-mock'`. `FakeClient` is a finite deterministic simulator. It does not load a language model, call an external API, train a model, or provide a production service.

All identities, requests and statuses are synthetic fixtures. The optional Day 1 and Day 4 homework are separate notebooks that load `google/flan-t5-small` locally. They are real local-model demonstrations and must not be confused with core mock-mode results.

## 2. Request architecture

```text
Question
  → input guard
  → ticket extraction (rule/regex based)
  → ticket schema and business-rule validation
  → route
      ├─ public FAQ → FakeClient / eligible exact-match cache
      ├─ status question → read-only status tool
      ├─ missing reference → clarification
      └─ unsupported or unsafe input → controlled refusal
  → response validation
  → response + minimal metrics
```

### Main components

| Component | Responsibility | Boundary |
|---|---|---|
| `FakeClient.generate` | Finite lookup and mock-call counting | Not a live LLM |
| `ask` | Day 1 response contract, trace ID and Python latency | Local Python timing only |
| `extract_ticket` | Extracts service, city and reference with simple rules | Not general NLP |
| `validate_ticket` | Checks exact keys, value types and allowed values | Rejects malformed structure |
| `check_status` | Looks up a synthetic status after ownership verification | Read-only; no mutation |
| `route` | Chooses FAQ, clarification, status or refusal path | Finite rule-based routing |
| `build_prompt` | Stores a versioned prompt teaching artifact | Mock does not execute a real prompt |
| `guard_input` | Applies small length/content/number guards | Educational examples, not comprehensive security |
| `run_pipeline` | Coordinates Day 3 validation and processing | Uses synthetic data |
| `evaluate` | Runs ten fixed golden cases | No model judge or network |
| `run_app` | Adds exact public FAQ cache and minimal metrics | Status requests bypass cache |

## 3. Synthetic data and fields

The dataset is documented in [the dataset card](../data/README.md) and exported in [synthetic_data.json](../data/synthetic_data.json).

### Public service directory

- `ar`: `يمكنك متابعة حالة طلبك باستخدام رقم المرجع.`
- `en`: `You can check your application status using its reference number.`

Only the two exact supported public FAQ strings are cache-eligible.

### Records

| Reference | Owner | Status |
|---|---|---|
| `REF-1001` | `demo-user-1` | `under_review` |
| `REF-1002` | `demo-user-2` | `approved` |
| `REF-9999` | absent | not found |

The configured test session is `demo-user-1`. Knowing a reference does not establish ownership. The status function compares the trusted session fixture to the record owner before returning a status. The fixture is a teaching example, not real authentication.

### Ticket schema

A valid ticket has exactly four fields:

- `service`: `service_info`, `application_status`, or `other`.
- `city`: `Riyadh`, `Jeddah`, or `unknown`.
- `request_id`: null or `REF-` followed by four ASCII digits.
- `needs_clarification`: Boolean.

For an application-status request with no reference, the pipeline asks for clarification and does not call the tool.

## 4. Validation, authorization and read-only behavior

- Exact schema keys and value types are checked before routing.
- Unknown service actions such as cancellation are rejected; the project never mutates records.
- The status tool is invoked only for appropriate status requests.
- `REF-1001` is visible to its owner `demo-user-1`.
- `REF-1002` is owned by `demo-user-2`; the `demo-user-1` session receives a denied response without the actual status.
- `REF-9999` is absent and returns a controlled unavailable response.
- Input guards cover selected examples (blank/invalid input, overlength, a sample instruction override, and a ten-digit pattern). They are not comprehensive prompt-injection, PII or application-security defenses.

## 5. Day 4 cache design

Only the two exact public directory questions (`FAQ_AR` and `FAQ_EN`) are eligible.

- TTL: **300 seconds**.
- Cache key: `(text, language, MODEL, prompt_version, directory_version)`.
- Cache stores the public answer and creation time only.
- Prompt or directory version change creates a different key.
- Expiry at the TTL boundary is a miss.
- Status requests, ticket overrides, and non-exact questions bypass cache.
- Metrics omit questions, references, owners, tickets and raw answers.

Controlled experiment, after clearing the cache:

| Measure | Without cache | With empty cache |
|---|---:|---:|
| Requests | 3 | 3 |
| Mock calls | 3 | 1 |
| Hits | 0 | 2 |
| Hit rate | 0% | 66.7% |

The hit sequence is `false, true, true`. This is a deterministic call-count comparison, not a guaranteed latency improvement or real financial saving.

## 6. Results and interpretation

The core notebook writes these artifacts in `murshid_outputs/` and packages the JSON files into `murshid_submission.zip`:

- `baseline_results.json`
- `day2_results.json`
- `day3_results.json`
- `day4_results.json`
- `final_report.json`
- `metrics.json`
- `synthetic_data.json`

Committed copies from an independent clean-namespace execution are stored under [results/core](../results/core/). Trace IDs and time values change between runs. `latency_ms` measures local Python execution, not inference latency. The report's USD calculation is an illustration with hypothetical rates; actual external API usage/spend is zero.

## 7. Tests and reproducibility

Run from the repository root:

```bash
python -m unittest discover -s tests -v
```

The offline standard-library test suite contains seven test methods covering:

1. 14-cell, code-only notebook structure and mock mode.
2. Full execution, ten-case regression and ZIP export.
3. Repeat execution in a clean test namespace.
4. Ticket schema acceptance/rejection.
5. Ownership checks, denied-status non-disclosure and record immutability.
6. Input guards and golden-case contract.
7. Cache TTL, version isolation, status exclusion and minimal metrics fields.

The course's own ten golden cases remain in the notebook and are separately checked by `evaluate`. Passing these fixed examples is not a general reliability or security guarantee.

## 8. Limitations

- No live LLM in the core project.
- No real authentication, real service records or production deployment.
- Finite rule-based extraction and routing.
- Educational input guards only.
- Synthetic fixtures and fixed golden tests only.
- Local latency and illustrative token costs are not production performance or billing measurements.
