# Day 2 Homework — Validate and Dispatch Structured Tool Calls

**Exercise:** validate an untrusted structured call before dispatching a read-only application-status tool.

- [Open the notebook](day2_tools_raghad.ipynb)
- [Read the course assignment](https://github.com/almiyead-rgb/murshid-llm-application-engineering-sda-aie-213/blob/main/extensions/README.md#assignment-a--validate-and-route-structured-tool-calls)

## Observed run

| Check | Result |
|---|---|
| Mode | `simulated_tool_protocol` |
| Passing cases | **7 / 7** |
| Records unchanged | `true` |
| Model or external API | None |

The seven cases cover an authorized read, unavailable records, an unapproved tool, invalid arguments, and a malformed call. The notebook keeps the demo user separate from the submitted tool-call object and checks ownership before exposing a status.

This is a Python simulation of structured tool dispatch. It is not native function calling by a hosted model, it does not contact an external service, and its synthetic identity is not production authentication.

The full case-by-case output is preserved in the notebook.
