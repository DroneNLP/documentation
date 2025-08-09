# Data Cleansing Procedure

The raw flight log messages obtained from AirData UAV often contain various forms of noise that can significantly hinder subsequent Natural Language Processing (NLP) tasks. This section provides a detailed, three-step procedure for our data cleansing process. This procedure transforms the raw messages into a structured, consistent, and semantically clearer format, suitable for robust NLP analysis.

---

## 1. Raw Data & Repositories

The raw, un-cleansed AirData messages and all intermediate and final cleansing files are hosted in a dedicated GitHub repository for full transparency and reproducibility.

* **GitHub Repository:** [https://github.com/DroneNLP/dataset](https://github.com/DroneNLP/dataset)

* **Raw Data File:**
    * **Filename:** `raw-air-data.xlsx`
    * **Source:** All 499 messages copied from the AirData UAV wiki. tree/main/raw/airdata
    * **Download Link:** [https://github.com/DroneNLP/dataset/tree/main/raw/airdata/raw-air-data.xlsx](https://github.com/DroneNLP/dataset/tree/main/raw/airdata/raw-air-data.xlsx)

---

## 2. Cleansing Procedure: A Three-Step Process

Our data cleansing was a multi-stage process, with each step targeting a specific type of textual noise. All corrections were documented in separate log files and applied using Python scripts to ensure a reproducible pipeline.

### 2.1. Step 1: Ending Period Correction

The first step was to ensure every message ended with a punctuation mark. Many messages from the raw collection lacked a terminal period, which is essential for accurate sentence boundary detection in subsequent NLP tasks.

* **Procedure:** A Python [script](https://github.com/DroneNLP/dataset/blob/main/processed/airdata/notebooks/1.ending_punct.ipynb) was used to automatically append a period to any message that did not already end with one. This was a simple, rule-based correction.
* **Resulting File:** [`corrected_data_step_1.xlsx`](https://github.com/DroneNLP/dataset/blob/main/processed/airdata/corrected_data_step_1.xlsx)

### 2.2. Step 2: Missing Punctuation and Space Correction

This step addressed more complex syntactic errors, such as missing spaces, concatenated words, and misplaced commas. These issues are crucial to fix as they directly impact tokenization and linguistic parsing.

* **Procedure:** This step was guided by a manually created correction log. A Python [script](https://github.com/DroneNLP/dataset/blob/main/processed/airdata/notebooks/2.fomatting.ipynb) then applied these specific corrections to [`corrected_data_step_1.xlsx`](https://github.com/DroneNLP/dataset/blob/main/processed/airdata/corrected_data_step_1.xlsx).
* **Correction Log:** [`correction_log_step_2.xlsx`](https://github.com/DroneNLP/dataset/blob/main/processed/airdata/correction_log_step_2.xlsx)
* **Resulting File:** [`corrected_data_step_2.xlsx`](https://github.com/DroneNLP/dataset/blob/main/processed/airdata/corrected_data_step_2.xlsx)

### 2.3. Step 3: Over-generalized Message Correction

The final and most complex step involved addressing messages that were syntactically correct but semantically over-generalized or ambiguous. This process required domain expertise to infer a more precise meaning where possible.

* **Procedure:** Similar to step 2, a manual correction log was created, documenting the refined versions of these messages. A Python [script](https://github.com/DroneNLP/dataset/blob/main/processed/airdata/notebooks/3.over_generalized.ipynb) applied these changes to [`corrected_data_step_2.xlsx`](https://github.com/DroneNLP/dataset/blob/main/processed/airdata/corrected_data_step_2.xlsx).
* **Correction Log:** [`correction_log_step_3.xlsx`](https://github.com/DroneNLP/dataset/blob/main/processed/airdata/correction_log_step_3.xlsx)
* **Resulting File:** [`corrected_data_step_3.xlsx`](https://github.com/DroneNLP/dataset/blob/main/processed/airdata/corrected_data_step_3.xlsx)

---

## 3. Cleansing Logs & Final Output

The comprehensive cleansing log, detailing every correction made across all three steps, is provided in the `correction_log` files for full transparency. The final output of our cleansing procedure, `corrected_data_step_3.xlsx`, serves as the input for our annotation tasks.

!!! info "Final Note on Data"
    Once our annotation tasks are complete, this data will also be uploaded to **Zenodo** to provide a persistent, citable DOI and allow for programmatic access via CLI tools for the community.