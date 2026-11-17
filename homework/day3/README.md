# Day 3 Homework — Error Handling and Input Validation

**Exercise:** deliberately raise and catch validation errors, check their exact messages, and confirm that corrected input is accepted.

- [Open the notebook](day3_errors_raghad.ipynb)
- [Read the course practice instructions](https://github.com/almiyead-rgb/murshid-llm-application-engineering-sda-aie-213/blob/main/extensions/README.md#separate-input-error-practice)

## Observed run

| Case | Expected behavior | Result |
|---|---|---|
| `None` | Catch `ValueError` | Passed |
| Blank question | Catch `ValueError` | Passed |
| More than 500 characters | Catch `ValueError` | Passed |
| Valid question | Accept trimmed input | Passed |

The notebook records three intentionally caught `ValueError` cases, the exact messages, and `checks: passed`. These are successful demonstrations of exception handling, not unexpected notebook failures.

The empty trailing code cell has been removed. The saved output and the explanation of `raise`, `try`, and `except` remain in the notebook.
