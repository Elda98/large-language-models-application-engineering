# Day 4 Homework — Assignment B: Evaluate Three Real-Model Answers

**Exercise:** evaluate three paraphrases of a synthetic service FAQ using a locally loaded model, then inspect the meaning of each answer.

- [Open the evaluation notebook](day4_evaluation_raghad.ipynb)
- [View the generated evaluation report](evaluation_result.json)
- [Read the course assignment](https://github.com/almiyead-rgb/murshid-llm-application-engineering-sda-aie-213/blob/main/docs/day-4.md#extra-homework)

## Recorded model and environment

| Item | Recorded value |
|---|---|
| Model | `google/flan-t5-small` |
| Revision | `0fc9ddf78a1e988dac52e2dac162b0ede4fd74ab` |
| Runtime | Google Colab · Python 3.13 · CPU |
| PyTorch | 2.14.1+cpu |
| Transformers | 4.49.0 |
| SentencePiece | 0.2.1 |
| Generation | Greedy decoding · `num_beams=1` · `max_new_tokens=48` |
| External inference API calls | 0 |

## Observed results

| Case | Generated answer | Input tokens | Output tokens* | Inference time |
|---|---|---:|---:|---:|
| E01 | Use its reference number | 34 | 6 | 0.457 s |
| E02 | Reference number | 36 | 4 | 0.133 s |
| E03 | Use its reference number | 37 | 6 | 0.181 s |

`keyword_hits = 3/3`. The indicator checks whether both “reference” and “number” appear. It is a lexical proxy, not semantic accuracy or a general measure of model quality.

The notebook's separate **Manual Evaluation** section records the learner's assessment that all three responses are supported by the supplied synthetic context. The JSON report preserves the generated evaluation output and its instruction to review answers; it does not replace the manual assessment.

## Environment compatibility

The supplied `sentencepiece==0.2.0` source distribution failed to build in the Python 3.13 Colab runtime. The notebook uses the compatible `sentencepiece==0.2.1` wheel. The model revision, three questions, and generation settings are unchanged.

*Output token counts include special tokens, as recorded by the assignment code. This optional homework uses a real local model and is separate from the core project's `mode=mock` run.
