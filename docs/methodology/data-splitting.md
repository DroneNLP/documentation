# Data Splitting & Experimental Design

A crucial aspect of our research is the strategic division of our message collection into distinct sets for model development and evaluation. This procedure is designed to validate our NLP framework in a setting that accurately simulates a real-world forensic investigation.

## 1. Experimental Design: Simulating a Forensic Scenario

We have intentionally partitioned our dataset to create an ideal forensic testing scenario. The core idea is to train and validate models on a general collection of messages while testing them on a completely independent set of messages from real-world flights.

* **Development Set (Train & Validation):**
    * **Source:** All cleansed messages from the **AirData UAV** wiki.
    * **Purpose:** This set represents a broad, general collection of drone-related messages and warnings. It is used to train our NLP models to recognize and classify a wide variety of events and problems, establishing their foundational knowledge.

* **Testing Set:**
    * **Source:** All cleansed and extracted messages from the **VTO Labs** forensic artifacts.
    * **Purpose:** This set serves as our unseen, out-of-domain test data. The messages originate from real-world flight logs and represent genuine scenarios that an investigator would encounter. Evaluating models on this set provides a more realistic measure of their effectiveness in a practical, forensic context.

This separation ensures that our models are not evaluated on messages they may have seen during training, providing a more robust and trustworthy measure of their generalization capabilities.

## 2. Qualitative Ground Truth: Key Flight Events

To enrich the testing set and enable qualitative analysis of our models' performance, we have manually investigated each VTO Labs flight log to identify key events. These events serve as a qualitative ground truth, allowing us to validate whether our framework can correctly reconstruct the high-level narrative of a flight incident.

This qualitative ground truth will be provided alongside the quantitative ground truth for each tasks in our [Tasks & Datasets](../../tasks). This enables a deeper level of analysis beyond simple metrics, allowing researchers to evaluate a model's ability to create a coherent and forensically relevant timeline.