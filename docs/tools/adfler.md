# [ADFLER: Advanced Drone Flight Log Entity Recognizer](https://github.com/DroneNLP/ADFLER)

ADFLER is an open-source tool developed to perform named entity recognition on drone flight log data to support forensic investigations. It uses a fine-tuned ALBERT model to identify entities (`Event` and `NonEvent` labels) in log messages and constructs a forensic timeline.

## Features
- **Forensic Timeline Construction**: Merges and sorts logs from Android and iOS devices.
- **Entity Recognition**: Uses an ALBERT model to highlight key entities (Event/NonEvent) in log messages.
- **Forensic Report Generation**: Generates a PDF report with statistics and the highlighted timeline.

## Installation

### Prerequisites
1.  **Python 3.7+**
2.  **wkhtmltopdf**: Required for report generation.
    *   **Windows**: Download and install from [wkhtmltopdf.org](https://wkhtmltopdf.org/downloads.html/). Default path: `C:\Program Files\wkhtmltopdf\bin\wkhtmltopdf.exe`.
    *   **Linux**: `sudo apt-get install wkhtmltopdf`. Default path: `/usr/bin/wkhtmltopdf`.

### Install ADFLER
You can install ADFLER directly from PyPI (coming soon) or from source.

**From Source:**
```bash
git clone https://github.com/DroneNLP/ADFLER.git
cd ADFLER
pip install .
```

### Model
ADFLER uses a fine-tuned ALBERT model (`swardiantara/ADFLER-albert-base-v2`). By default, the tool will **automatically download and cache** the model from Hugging Face on its first run.

If you prefer to use a locally downloaded model or an offline environment, you can point to your local directory containing `pytorch_model.bin` using the `--model` argument. ADFLER reads the entity labels directly from the model's `config.json` (via `AlbertConfig`), so a local model directory must include a valid `config.json` alongside the model weights.

## Usage

ADFLER provides a Command Line Interface (CLI).

**Structure:**
```bash
adfler [arguments]
```

### Arguments

| Argument | Description |
| :--- | :--- |
| `--evidence <path>` | Path to the directory containing input flight logs (must have `android` and/or `ios` subfolders). |
| `--output <path>` | Path to the directory where results will be saved. |
| `--model <path>` | Path to the directory containing the ALBERT model, or a Hugging Face model ID (default: `swardiantara/ADFLER-albert-base-v2`). |
| `--config <path>` | Path to a custom `config.json` file. |

### Examples

**1. Run Pipeline (Recommended):**
```bash
adfler --evidence "./flight_logs" --output "./results/case_001"
```

**2. Run Pipeline with Custom Model:**
```bash
adfler --evidence "./flight_logs" --output "./results/case_001" --model "./local_model_dir"
```

## Pipeline Stages

Running `adfler` executes four sequential stages, stopping early if any stage fails:

1.  **Evidence Checking** — scans the evidence directory for `android`/`ios` subfolders and writes `raw_list.json`.
2.  **Forensic Timeline Construction** — parses each log file, merges `date` and `time` columns into a single `timestamp`, and sorts all messages chronologically into `forensic_timeline.csv`.
3.  **Entity Recognition** — loads the ALBERT NER model and predicts `Event`/`NonEvent` entities for every message in the timeline, saving `ner_result.json`.
4.  **Forensic Report Generation** — renders the annotated timeline and statistics into an HTML report, then converts it to a PDF using `wkhtmltopdf`.

## Output Structure

Running `adfler` will create the following structure in your output directory:

```text
output_dir/
├── raw_list.json             # List of detected log files
├── forensic_timeline.csv     # Merged and sorted timeline of all logs
├── ner_result.json           # Timeline with detected Event/NonEvent entities (JSON format)
├── statistics.json           # Counts of detected entity types
├── forensic_report_.html     # Intermediate HTML report
├── forensic_report_.pdf      # Final PDF Forensic Report
└── parsed/                   # Intermediate parsed CSVs
    ├── android/
    └── ios/
```

## Configuration

You can optionally use a `config.json` file instead of passing long arguments.

**Example `config.json`:**
```json
{
    "source_evidence": "./flight_logs",
    "output_dir": "./results/test_run",
    "model_dir": "./model",
    "wkhtml_path": {
        "windows": "C:\\Program Files\\wkhtmltopdf\\bin\\wkhtmltopdf.exe",
        "linux": "/usr/bin/wkhtmltopdf"
    },
    "use_cuda": true
}
```

Run with config:
```bash
adfler --config my_config.json
```

If `use_cuda` is not specified, ADFLER automatically enables it when a CUDA-capable GPU is detected by PyTorch.

## License
[MIT](LICENSE)
