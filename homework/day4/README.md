# Day 4 Homework — Assignment B: Evaluate Three Real-Model Answers

- [Evaluation notebook](day4_evaluation_raghad.ipynb)
- [Generated report](evaluation_result.json)

The separate notebook loads `google/flan-t5-small` locally and evaluates three synthetic FAQ paraphrases. Its report includes token counts, inference time, a reference+number keyword proxy, and a manual semantic review. The proxy is not a semantic-accuracy metric, and the notebook reports zero external inference API calls.

**Environment note:** In the Python 3.13 Colab runtime, installing the original `sentencepiece==0.2.0` failed while building its wheel. The notebook uses the compatible `sentencepiece==0.2.1` wheel; the model, revision, questions and generation settings are unchanged.
