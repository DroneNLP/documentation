# Event Recognition: Task Definition & Annotation

The ability to automatically identify key events from drone flight log messages is fundamental for building a forensic timeline and gaining a high-level overview of an incident. This section details the Event Recognition task within our proposed NLP framework.

---

## 1. Task Overview & Investigative Relevance

### 1.1. The Challenge

In any forensic investigation, the first step is often to establish a clear timeline of events. For drone incidents, this means sifting through a continuous stream of messages to find specific moments like "takeoff," "landing," or "return to home." Manually identifying these events is tedious and can obscure the bigger picture of what transpired during a flight.

### 1.2. Justification for Forensic Analysis

Events are the primary regions of interest in forensic analysis. By distinguishing event-related sentences from non-event sentences, we can significantly improve the effectiveness of an investigation. Compiling these events into a chronologically ordered list provides a brief but crucial overview of the incident, allowing investigators to quickly focus on the most relevant parts of the flight log.

### 1.3. Investigative Questions Answered

By performing Event Recognition, our framework helps answer questions that are foundational to any drone incident investigation:

* What was the exact sequence of key flight events?
* When did the drone take off, reach its maximum altitude, or land?
* Did the drone initiate a "Return to Home" (RTH) command, and if so, when?
* Was there a transition between different flight modes (e.g., from P-mode to S-mode) and at what point in the flight?
* When did a specific event of interest (e.g., a "gimbal recenter" command) occur?

---

## 2. Task Formulation

### 2.1. Task Definition

Event Recognition is defined as the process of extracting and classifying specific flight events from within drone log messages. Unlike problems, which are often indicative of a negative or anomalous state, events represent significant actions or transitions in the drone's flight status.

### 2.2. Granularity & Tagging Scheme

We formulate Event Recognition as a **Sequence Labeling** task, specifically using a **BIOES tagging scheme** (Begin, Inside, Outside, End, Single). This scheme allows us to precisely identify the boundaries of multi-word event mentions and single-word events. The two entity types we are tagging are:

* **Event**: Represents a specific action, transition, or command.
* **NonEvent**: Represents words that are outside of an event mention.

### 2.3. Output Format

The output for this task is a sequence of tokens with their corresponding BIOES tags, providing a clear and structured representation of the events in each message. The following table provides an example:

| Token    | `Switched`  | `to`      | `S`         | `(Sport)-mode` | `.`          |
|:---------|:------------|:----------|:------------|:---------------|:-------------|
| **Tag** | `B-Event`   | `I-Event` | `I-Event`   | `E-Event`      | `O` |

---

## 3. Dataset Annotation Procedure

### 3.1. Annotation Schema

The annotation schema for Event Recognition is straightforward, focusing on identifying the textual spans that correspond to a specific event. We have defined **two primary entity types**: `Event` and `NonEvent`.

### 3.2. Segment Separation Rules

To avoid over-complicating the annotation and ensure consistency, we treat the following punctuation marks as segment separators:
**Period `.`**
**Colon `:`**
**Comma `,`**

Each segment will be assigned either an `Event` or `NonEvent` entity type. Complex or compound messages that are not clearly separated will be left for future analysis to maintain a high degree of annotation consistency for the current scope.

### 3.3. Guidelines & Examples

Annotators are instructed to identify any message that describes a change in state or an action taken by the drone or its operator.

* **Positive Examples:**
    * "Switched to S (Sport)-mode."
    * "Aircraft is returning to the Home Point."
    * "Reaching maximum altitude..."
    * "Gimbal Recenter."

* **Negative Examples:**
    These messages are classified as `NonEvent` because they are informational, instructional, or directive, and do not imply an action, system response, occurring event, current environment situation, nor change of state.
    * "Data Recorder File Index is nnn"
    * "Fly with caution"
    * "Always fly in compliance with altitude limitations"
    * "Manually adjust flight route or return to home"


---