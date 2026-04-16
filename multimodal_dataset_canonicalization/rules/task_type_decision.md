# Task Type Decision

## Purpose

This file defines how to infer the task type of a dataset or sample before converting it into a canonical training format.

Task type decision is upstream of field mapping and sample construction.

A correct task type decision helps determine:
- what the model is expected to do
- what kind of input should be constructed
- what supervision target should be selected
- whether prompt injection is needed
- which canonical output format should be used
- which training stage the data best fits

---

## Core principle

Do not decide task type from field names alone.

Use the following evidence together:
1. actual local sample structure
2. modality composition
3. README / dataset card
4. paper task framing if needed

If these sources disagree, prioritize the actual local sample structure and explicitly note the mismatch.

---

## Primary task type inventory

Common primary task types include:

- `image_vqa`
- `multi_image_qa`
- `chart_reasoning`
- `map_qa`
- `document_ocr`
- `document_qa`
- `image_caption`
- `image_text_pair`
- `image_to_html`
- `image_to_code`
- `asr`
- `ast`
- `audio_qa`
- `multi_turn_mllm`
- `omni_instruction`
- `text_instruction`
- `evaluation_only`
