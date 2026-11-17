# Murshid Mini — Technical Project Notes

## Verified project scope

This is a four-day mock-mode educational application. Its purpose is to teach the engineering around a model-shaped interface, not to train a model or deploy a government service.

- Core mode: `mock`; deterministic `FakeClient`; zero external API calls.
- Runtime: Python 3, CPU, standard library only; no paid API, key, model download or extra package in the core notebook.
- All data: synthetic fixtures authored for the course.

## Architecture

Input → input guard → rule-based ticket extraction → schema and business-rule validation → public mock answer OR read-only status tool → output validation → response and minimal metrics.

Day 4 wraps only the two exact public directory questions in a cache. Status requests, malformed tickets and guarded requests bypass it. The cache key includes question, language, model label, prompt version and directory version. TTL is 300 seconds. Cached entries contain only the public answer and creation time, not status or session data.

## Components and boundaries

- `FakeClient.generate`: finite lookup; increments the mock-call counter; never calls an API.
- `ask`: baseline response with trace ID, answer, model label and measured Python latency.
- `extract_ticket`: small rule/regex parser, not general NLP.
- `validate_ticket`: schema and business-rule checks.
- `check_status`: local read-only lookup with ownership check.
- `route`: handles clarification, public FAQ, status, and unsupported actions.
- `build_prompt`: versioned teaching artifact; the mock does not execute a real prompt.
- `guard_input`: limited length, example injection-string, and ten-digit pattern checks; not comprehensive security.
- `run_pipeline`: coordinates validation and routing.
- `evaluate`: ten fixed cases, no network or model judge.
- `run_app`: public FAQ cache wrapper and minimal operational metrics.

## Synthetic data

See [data/README.md](../data/README.md) and [data/synthetic_data.json](../data/synthetic_data.json). The fixtures include public Arabic/English service-directory answers and fictional references:
- `REF-1001`, owned by `demo-user-1`, status `under_review`.
- `REF-1002`, owned by `demo-user-2`, status `approved`.
- `REF-9999` intentionally absent.

A reference number is not proof of ownership. The session identity is trusted test setup, not extracted from a question. These controls illustrate authorization but are not production authentication.

## Generated outputs

The core notebook writes `baseline_results.json`, `day2_results.json`, `day3_results.json`, `day4_results.json`, `final_report.json`, `metrics.json`, and `synthetic_data.json` under `murshid_outputs/`; Cell 14 packages the JSON files into `murshid_submission.zip`. Save the notebook separately.

Local Python latency is not model inference latency. Trace IDs and timings vary. Cache call counts and status codes are deterministic when the experiment starts with a cleared cache. The cost calculation is hypothetical and uses illustrative prices; actual external API usage and spend are zero.

## Reproducibility and tests

Restart the Colab runtime and run the 14 core cells from the top, sequentially. Check the daily assertions, all ten golden cases, cache version isolation and TTL, status-cache exclusion, zero external calls, and final report. UUIDs and time values are expected to differ.

## Optional homework

Homework notebooks remain separate. Day 1 and Day 4 homework use a real local `google/flan-t5-small` model and do not change the core's `mode=mock`. Their package/model downloads require internet, but no paid inference API. Treat their measurements as local inference evidence and their keyword indicator as a weak proxy, not semantic accuracy.

## Limitations

Finite rule-based routing, synthetic records, no live language model in core, no real authentication, educational guards only, and no production readiness claim.
