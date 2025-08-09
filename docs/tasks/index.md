# NLP Tasks & Datasets for Drone Log Analysis

The core of the **DroneNLP** project is a suite of specialized Natural Language Processing tasks, each designed to extract critical information from drone flight log messages for specific analytical purposes. These tasks, and their corresponding annotated datasets, are foundational for developing models that can support forensic investigations, automated diagnostics, and flight safety analysis.

Each task is structured to tell a complete story, from its initial motivation and formulation to the details of its annotation procedure and the resulting dataset.

---

## Our Defined Tasks

The following are the key NLP tasks and associated datasets developed as part of this project. Click on any of the cards below to learn more about a specific task, or use the navigation sidebar to explore a task's full documentation.

<div class="grid cards" markdown>

-   :material-alert-circle-outline:{ .lg .middle } __Problem Identification__

    Detecting and categorizing specific problems, malfunctions, or anomalies inferred from drone flight log messages. This is crucial for diagnostics and incident investigation.

    [:octicons-arrow-right-24: Learn More](problem-identification/)

-   :material-calendar-check-outline:{ .lg .middle } __Event Recognition__

    Identifying and classifying significant flight events (e.g., takeoff, landing, RTH initiation) within the log messages.

    [:octicons-arrow-right-24: Learn More](event-recognition/)

-   :material-tag-text-outline:{ .lg .middle } __NER-based Term Extraction__

    Extracting predefined entities such as Components, Functions, Parameters, Issues, Actions, and States from the messages for structured knowledge representation.

    [:octicons-arrow-right-24: Learn More](term-extraction/)

-   :material-relation-many-to-many:{ .lg .middle } __Semantic Role Labeling__

    Identifying the semantic relationships between an Event and its associated Implication/Effect, and Mitigation/Instruction, crucial for understanding cause-and-effect in incidents.

    [:octicons-arrow-right-24: Learn More](semantic-role-labeling/)

</div>

---

## Dataset Structure

Each dataset is tailored to its specific task, with annotations provided in a standardized format to ensure compatibility with modern NLP libraries and frameworks. The documentation for each task details the specific annotation schema, file format, and data split. All of our datasets are hosted on Zenodo (forthcoming) for easy download and access.