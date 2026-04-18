# Path Normalization

## Purpose

This file defines how to normalize media paths and output file locations when converting raw datasets into canonical training JSONL.

It mainly covers:
- image paths
- audio paths
- document / page paths
- multi-image and multi-audio samples
- ceph-oriented relative path conversion
- dataset-prefix injection
- path cleanup and consistency rules
- output JSONL file placement

---

## Default preference

Unless the user explicitly says otherwise:

- remove leading `/` from media paths
- prefer normalized relative paths for image / audio / document references
- use `storage_type: "ceph"` by default in canonical multimodal records
- set `dataset_type` only to `train` or `test`
- if the source folder name contains `train`, use `dataset_type: "train"`
- if the source folder name contains `test`, use `dataset_type: "test"`
- preserve dataset-relative folder structure after removing the leading `/`
- only prepend dataset prefixes when needed for correctness
- do not keep machine-specific absolute prefixes in canonical training data
- write converted JSONL files directly into the target output directory
- do not place converted JSONL files inside extra subfolders

## Dataset type rule

For any generated media reference such as `image_url` or `audio_url`, `dataset_type` must only use one of these two values:

- `train`
- `test`

Infer the value from the source folder name:

- if the source folder name contains `train`, write `dataset_type: "train"`
- if the source folder name contains `test`, write `dataset_type: "test"`

Do not use other values for `dataset_type`.
