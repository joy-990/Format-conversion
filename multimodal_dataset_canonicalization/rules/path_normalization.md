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
- preserve dataset-relative folder structure after removing the leading `/`
- only prepend dataset prefixes when needed for correctness
- do not keep machine-specific absolute prefixes in canonical training data
- write converted JSONL files directly into the target output directory
- do not place converted JSONL files inside extra subfolders
