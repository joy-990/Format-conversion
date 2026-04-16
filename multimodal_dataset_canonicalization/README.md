# Multimodal Dataset Canonicalization

A reusable skill for understanding, inspecting, and converting multimodal datasets into a canonical training JSONL format.

This skill is designed for workflows involving:
- image-text datasets
- VQA
- multi-image QA
- OCR / document understanding
- image-to-html / image-to-code
- ASR
- AST
- multimodal multi-turn conversation
- mixed omni-format samples
- JSON / JSONL / Parquet dataset preprocessing

---

## What this skill does

This skill is not only a format converter.

It helps with the full pipeline:

1. inspect the real sample structure
2. consult the dataset README / dataset card
3. use paper links as semantic support when needed
4. infer the task type
5. infer the suitable training stage
6. construct final input / target pairs
7. normalize media paths
8. convert the dataset into canonical training JSONL

---

## Core design philosophy

The conversion should not be a blind field rewrite.

The goal is to produce training data that is:
- semantically correct
- consistent with the dataset's intended task
- compatible with the user's canonical format
- easy to audit later
- stable under large-scale data processing

Priority order:
1. real local sample structure
2. dataset README / dataset card
3. paper framing
4. canonical conversion rules

---

## Main rule files

- `rules/task_type_decision.md`
- `rules/input_target_construction.md`
- `rules/training_stage_decision.md`
- `rules/path_normalization.md`

---

## Default conventions

Unless the user explicitly says otherwise:

- consult the real sample first
- consult the dataset README / dataset card whenever available
- use papers as semantic support rather than schema authority
- preserve useful original fields in `meta`
- remove leading `/` from media paths
- keep normalized relative paths
- write final converted JSONL directly into the target output directory
- avoid extra output subfolders by default

---

## Input ordering defaults

Unless the user explicitly says otherwise:

- `image_vqa` -> image first, text after
- `image_caption` -> image first, text after
- `document_ocr` -> image first, text after
- `document_qa` -> image first, text after
- `image_to_html` -> image first, text after
- `image_to_code` -> image first, text after
- `multi_image_qa` -> text first, then images in stable order
- `asr` -> text first, then audio
- `ast` -> text first, then audio
- `omni_instruction` -> text first, then modalities

---

## Typical workflow

1. inspect schema
2. read README / dataset card
3. use paper if needed
4. decide task type
5. decide training stage
6. construct input / target
7. normalize paths
8. write final JSONL

---

## Ideal output of this skill

A successful conversion workflow should produce:

- a short dataset understanding summary
- a justified task type
- a justified training stage
- a clear input / target construction rule
- a clear path normalization rule
- one canonical sample example
- one usable conversion script
- explicit caveats if ambiguity remains
