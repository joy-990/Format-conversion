# Training Stage Decision

## Purpose

This file defines how to decide the most suitable training stage for a dataset or sample after its task type has been identified.

Training stage decision is downstream of:
- task type decision
- input / target construction
- README / dataset card inspection

A correct training stage decision helps determine:
- whether the data should be used for pretraining, instruction tuning, alignment, or evaluation
- whether prompt injection is appropriate
- whether the data should be converted into an interactive assistant-style format
- whether benchmark data should be treated cautiously for training

---

## Primary training stage inventory

Common primary training stages include:

- `pretraining`
- `instruction_tuning`
- `alignment`
- `evaluation_only`
