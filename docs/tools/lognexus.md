# [LogNexus: Sentence Segmentation for Drone Flight Logs](https://github.com/DroneNLP/LogNexus)

LogNexus (published to PyPI as `LogNexs`) is a command-line sentence segmentation tool for decrypted drone flight log messages. It uses a domain-tuned DistilBERT NER model to split noisy, multi-sentence flight log messages into semantically complete sentence records for downstream forensic review and analysis.

## Features
- **Batch Processing**: Processes decrypted DJI CSV flight logs from an input directory.
- **Message Extraction**: Extracts `APP.tip` and `APP.warning` messages into a message-level timeline.
- **Model Download**: Downloads the required Hugging Face model with `lognexus-download`.
- **Flexible Output**: Exports results as nested JSON or exploded XLSX rows.
- **Forensic Inference Pipeline**: Provides a SoPID-style forensic inference pipeline through `lognexus-pipeline`.
- **GPU Support**: Supports optional CUDA inference when PyTorch detects an available GPU.

## Installation

### Prerequisites
- **Python 3.9+**
- PyTorch (install the build matching your CUDA environment for GPU usage)

### Install LogNexus
**From Source:**
```bash
git clone https://github.com/DroneNLP/LogNexus.git
cd LogNexus
python -m pip install .
```

**From PyPI:**
```bash
python -m pip install LogNexs
```

!!! note
    The PyPI distribution name is `LogNexs`, but the installed Python import package and console commands remain `lognexus`, `lognexus-download`, and `lognexus-pipeline` for compatibility with the original tool and paper terminology.

### Model
The NER model is not bundled with the package and must be downloaded separately:

```bash
lognexus-download
```

By default this downloads `swardiantara/LogNexus-distilbert-base-uncased` into `./model`. A custom download location can be specified:

```bash
lognexus-download --model_dir /path/to/model
```

## Input Data

Place decrypted `.csv` flight logs in the input directory. Each CSV must contain the following columns:

```text
CUSTOM.date [local]
CUSTOM.updateTime [local]
APP.tip
APP.warning
```

LogNexus reads each non-empty `APP.tip` and `APP.warning` cell as a separate log message while preserving the original date and time values.

## Usage

### Sentence Extraction

**Structure:**
```bash
lognexus [arguments]
```

| Argument | Default | Description |
| :--- | :--- | :--- |
| `--input_dir`, `-i` | `./evidence` | Directory containing decrypted CSV flight logs. |
| `--output_dir`, `-o` | `./output` | Directory to save processed logs. |
| `--model_dir`, `-m` | `./model` | Directory containing the trained simpletransformers model. |
| `--format`, `-f` | `json` | Output format: `xlsx` (exploded rows) or `json` (nested list). |
| `--cuda` | off | Enable GPU acceleration for inference. Falls back to CPU if unavailable. |

**Examples:**
```bash
# Basic run with defaults
lognexus

# Custom paths
lognexus --input_dir /path/to/logs --output_dir /path/to/results --model_dir /path/to/model --format json

# XLSX output
lognexus --format xlsx

# GPU inference
lognexus --cuda
```

### SoPID-Style Inference Pipeline

The `lognexus-pipeline` command ports the working inference pipeline from SoPID into the LogNexus package structure. It supports two paradigms:

- **`message`**: classifies whole log messages using the Hugging Face sentiment model `swardiantara/drone-sentiment`.
- **`segment`**: segments messages with the SoPID NER model and classifies each unique segment with a local DroPTC classifier.

**Message-level run:**
```bash
lognexus-pipeline --paradigm message --evidence-dir ./evidence --output-dir ./pipeline-output
```

**Segment-level run:**
```bash
lognexus-pipeline \
  --paradigm segment \
  --model-name swardiantara/SoPID-bert-base-cased \
  --model-type bert \
  --pretokenizer spacy \
  --tag-scheme bioes \
  --droptc-model-dir ./best-model/droptc \
  --evidence-dir ./evidence \
  --output-dir ./pipeline-output
```

Pipeline evidence can be flat (`evidence/*.csv`) or grouped by drone model (`evidence/{drone-model}/*.csv`). Outputs are written under:

```text
pipeline-output/{message|segment}-{before|after}/run-{n}/{drone-model}/{flight-log}/
```

Each processed log produces:

| File | Description |
| :--- | :--- |
| `unique_events.xlsx` | Deduplicated messages or segments for manual review. |
| `timeline.json` | Full forensic timeline with propagated labels. |
| `timing.json` | Per-log processing time. |
| `prediction.json` | Segment CoNLL-style predictions (segment runs only). |

The run folder additionally gets a `timing_summary.json`.

## Output Formats

**JSON** keeps one record per original message with extracted sentences as a list:

```json
[
  {
    "date": "5/12/2025",
    "time": "8:27:36.34 AM",
    "message": "Failsafe RTH.; RC signal lost. Returning to home.",
    "sentence": [
      "Failsafe RTH",
      "RC signal lost",
      "Returning to home"
    ]
  }
]
```

**XLSX** explodes the `sentence` list so each extracted sentence gets its own spreadsheet row.

## Development

Install lightweight test dependencies without the full ML stack:

```bash
python -m pip install pytest pandas openpyxl
python -m pip install -e . --no-deps
```

Run tests:
```bash
pytest
```

Build package artifacts:
```bash
python -m build
twine check dist/*
```

## Citation

```bibtex
@misc{Silalahi2025LogNexus,
  title = {LogNexus: A Foundational Segmentation Tool for Drone Flight Log Messages},
  publisher = {Code Ocean},
  year = {2025},
  note = {[Source Code]},
  author = {Swardiantara Silalahi and Tohari Ahmad and Hudan Studiawan}
}
```

## License
[MIT](LICENSE)
