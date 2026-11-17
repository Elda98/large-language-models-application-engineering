# Core project execution results

These seven JSON files were exported from the learner's Colab run using Day 4 Cell 14 (`murshid_submission.zip`). The archive contains result files, not the notebook; the cumulative notebook is stored separately at [`notebooks/murshid.ipynb`](../notebooks/murshid.ipynb).

- `baseline_results.json`: Day 1, three actual mock responses with run-specific trace IDs and measured Python latency.
- `day2_results.json`: Day 2, five tool and ownership scenarios.
- `day3_results.json`: Day 3, ten golden cases (10/10).
- `day4_results.json`: Day 4 cache comparison (3 mock calls without cache; 1 with cache; 66.7% hit rate).
- `final_report.json`: final `mode=mock` report, hypothetical cost example and limitations.
- `metrics.json`: 22 minimal per-request metric rows; no question text, raw answer, owner, or request reference.
- `synthetic_data.json`: synthetic fixture export.

Trace IDs and measured Python latency values vary between runs. Latency is local Python execution time, not model inference time. The results describe the supplied synthetic core workflow, not live LLM quality or production-service behavior.

The [offline test suite](../tests/test_notebook.py) checks these committed results for completeness and consistency as well as executing the notebook from a clean test directory.
