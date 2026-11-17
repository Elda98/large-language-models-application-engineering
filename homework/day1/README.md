# Day 1 Homework — Real Local Model

**Exercise:** generate one answer with a locally loaded sequence-to-sequence model.

- [Open the notebook](day1_real_model_Raghad.ipynb)
- [View the saved inference report](live_demo_result.json)
- [Read the course assignment](https://github.com/almiyead-rgb/murshid-llm-application-engineering-sda-aie-213/blob/main/docs/day-1.md#extra-homework)

## Observed run

| Item | Recorded value |
|---|---|
| Model | `google/flan-t5-small` |
| Revision | `0fc9ddf78a1e988dac52e2dac162b0ede4fd74ab` |
| Runtime | Python 3.13.18 · CPU |
| PyTorch | 2.14.1+cpu |
| Transformers | 4.49.0 |
| SentencePiece | 0.2.1 |
| Generated answer | `Use its reference number` |
| Input / output tokens | 34 / 6 |
| Inference time | 0.561 seconds |
| External inference API calls | 0 |

The answer was generated locally from the synthetic English service-directory context. The notebook includes a review prompt to check that the response is supported and does not invent a fee, date, or application status.

## Environment compatibility

The original exercise specified `sentencepiece==0.2.0`. Its source distribution failed to build in the Python 3.13 Colab runtime. The notebook therefore uses the compatible `sentencepiece==0.2.1` wheel. The model, revision, prompt, and generation task remain unchanged.

This notebook is optional homework and stays separate from the required cumulative `mode=mock` project.
