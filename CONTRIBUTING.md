# Contributing

This repository documents a course project. Changes should preserve the instructor's required four-day core workflow and keep optional homework separate.

## Before changing the core

1. Read the relevant day guide and compare the intended change with the current notebook.
2. Keep one cumulative notebook at `notebooks/murshid.ipynb`.
3. Preserve the 14 labeled code cells and the official day sequence unless a course requirement explicitly changes.
4. Do not weaken or delete golden cases to make a change pass.
5. Keep the core in `mode=mock`; do not mix real-model homework dependencies into the core runtime.
6. Use synthetic records only. Never commit real identities, application records, credentials, tokens, or private data.

## Local verification

Use Python 3.11 or 3.13 and the standard library:

```bash
python -m unittest discover -s tests -v
```

The GitHub Actions workflow runs this suite on both supported versions for pushes and pull requests to `main`.

## Changes to documentation and results

- Keep statements consistent with the code and saved outputs.
- Label mock behavior, local-model homework, hypothetical calculations, and limitations accurately.
- Regenerate result files from a real notebook run; do not invent timings, trace IDs, tokens, or test outcomes.
- Preserve previous commits and use descriptive commit messages.
