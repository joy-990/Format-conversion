# Multimodal Dataset Canonicalization

## Purpose

This skill is used to understand, inspect, and convert multimodal datasets into a canonical training JSONL format.

It is designed for workflows involving:
- image-text datasets
- VQA
- multi-image QA
- OCR / document understanding
- image-to-html / image-to-code
- ASR
- AST
- multimodal multi-turn conversation
- mixed omni-format samples
- JSON / JSONL / Parquet preprocessing

This skill is not only a format converter.

It should:
1. inspect the real sample structure
2. consult the dataset README / dataset card when available
3. use paper links and Hugging Face references as semantic support
4. infer the most suitable task type
5. infer the most suitable training stage
6. construct final input / target pairs
7. normalize media paths
8. generate scalable conversion logic or scripts when needed

---

## When to use this skill

Use this skill when the user asks to:
- convert JSON / JSONL / Parquet into training JSONL
- inspect dataset schema or sample structure
- determine what training format a dataset should use
- decide whether a dataset is better for pretraining, instruction tuning, alignment, or evaluation
- transform image / audio / document / question-answer / conversation data into a canonical multimodal format
- normalize image or audio paths for ceph-style pipelines
- process large dataset files with streaming, chunking, parallelism, or resume support

This skill is especially suitable when the user provides:
- sample records
- raw file paths
- dataset README / dataset card
- paper links
- Hugging Face dataset links
- target output examples

---

## Core principles

### 1. Real sample structure comes first
Always inspect the actual sample fields, nesting structure, and stored values first.

Do not rely only on papers, README files, or dataset cards, because local copies, community mirrors, processed exports, and user-prepared files may differ from the original source.

### 2. Dataset README / dataset card is a default reference
When available, always consult the dataset README or dataset card.

Use it to understand:
- field definitions
- split semantics
- annotation design
- official task framing
- intended training vs evaluation usage
- path organization
- whether external media must be downloaded separately
- the meaning of ambiguous fields

In many practical conversions, the README is the most directly useful reference.

### 3. Papers are semantic support, not schema authority
Paper links should be used mainly for:
- understanding the research task
- understanding why the dataset was built
- clarifying training-stage suitability
- clarifying specialized supervision design

But papers should not override the actual local schema.

### 4. Format conversion is downstream of dataset understanding
Do not directly write conversion code before deciding:
- what the task type is
- what supervision signal should be used
- what training stage it best fits
- whether the data should remain single-turn or multi-turn
- how the final input should be ordered

### 5. Prefer canonicalization over one-off patching
When possible, convert different datasets into a stable canonical format rather than writing ad hoc output structures for each dataset.

### 6. Preserve traceability
Unless the user explicitly says otherwise, preserve useful original fields in `meta` so the converted data remains auditable and reversible.

### 7. For large files, prefer streaming and resumable processing
When handling large JSONL or Parquet files, prefer:
- line-by-line or batch streaming
- chunked processing
- temp output + final rename
- logs
- resume-safe behavior

---

## Required reasoning order

When this skill is invoked, follow this order:

1. inspect the real sample or file schema
2. identify likely modalities
3. inspect candidate input fields
4. inspect candidate target fields
5. consult the dataset README / dataset card if available
6. consult paper / HF references if provided
7. infer task type
8. infer suitable training stage
9. decide canonical output format
10. decide input / modality ordering
11. define field mapping and input / target construction
12. define path normalization rules
13. produce an example converted sample
14. produce conversion script or algorithm
15. provide caveats if ambiguity remains

Do not skip directly from raw sample to final script unless the structure is already obvious and unambiguous.

---

## Input ordering policy overview

The detailed rules live inside:
- `rules/input_target_construction.md`

### Default heuristics
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

### Important caution
Do not apply one global ordering rule to all tasks.

Single-image visual grounding tasks usually prefer modality-first ordering.

Multi-image comparison tasks and audio task-definition tasks usually prefer text-first ordering.

If the user already has a fixed template, follow the user's template.

---

## Path normalization overview

The detailed rules live in:
- `rules/path_normalization.md`

### Default conventions
Unless the user explicitly says otherwise:

- remove leading `/` from media paths
- prefer normalized relative paths
- default `storage_type` should be `ceph`
- preserve dataset-relative folder structure after removing the leading `/`
- only prepend dataset prefixes when needed for correctness
- do not keep machine-specific absolute prefixes in canonical training data
- write converted JSONL files directly into the target output directory
- do not place converted JSONL files inside extra subfolders
