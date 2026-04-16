# Input / Target Construction

## Purpose

This file defines how to construct the final training input and supervision target from raw dataset fields.

It is more general than simple field mapping.

In many datasets:
- the final user input is not stored directly in one field
- the final assistant target is only one of several candidate fields
- prompt text may need to be injected
- multimodal references may need to be inserted into the input
- some raw fields should be preserved only as `meta`

This rule file helps decide:
1. which raw fields participate in input construction
2. whether additional prompt text is needed
3. which field should be used as the main supervision target
4. how multimodal items should be inserted
5. how text and modality ordering should be arranged
6. which unused but useful fields should be preserved in `meta`

---

## 6. Decide input ordering

The relative order of text prompts and modality objects should be task-aware rather than fixed globally.

### General rule
- If the user already has an established format convention, follow it.
- Otherwise, choose a default ordering based on task type.
- Treat this as an engineering heuristic rather than a universal law, because some model families have their own preferred templates.
- Keep the ordering consistent within the same converted dataset unless there is a clear task-specific reason to split formats.

### Default summary
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
