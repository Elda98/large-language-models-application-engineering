# Day 1 Homework — Real Local Model

[Open the Day 1 notebook](day1_real_model_Raghad.ipynb) and [view the saved inference report](live_demo_result.json). The notebook loads `google/flan-t5-small` locally on CPU and records its generated answer, input/output token counts, measured inference time, and zero external inference API calls.

**Environment note:** The supplied setup pinned `sentencepiece==0.2.0`, but that source distribution did not build in the Python 3.13 Colab runtime. The notebook uses the compatible `sentencepiece==0.2.1` wheel instead; the model and generation task remain unchanged.
