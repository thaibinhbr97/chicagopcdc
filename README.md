# Project 7: Generating Decision Trees from Free Text & Doccano Annotations

## Applicant: Brad Nguyen

---

## Overview

This project focuses on transforming unstructured and semi-structured inputs (free-text rules and Doccano annotation data) into a structured, executable decision tree. The output will be a JSON-based decision tree that can be used within an admin UI to determine priority and ordering of criteria, with optional integration into synthetic or real patient data pipelines.

---

## Motivation & Interest

I am particularly interested in this project because it combines natural language processing, structured data modeling, and system design to translate domain knowledge into executable clinical logic. This is closely aligned with problems I have tackled in my own projects, where unstructured signals are transformed into reliable, real-time outputs that directly impact user experience and accessibility.

In *SilenceVoice*, I built a real-time visual speech recognition pipeline that converts silent lip movements into text, requiring robust handling of noisy, ambiguous inputs and transforming them into structured, interpretable outputs. Similarly, in *listen.me*, I developed a low-latency transcription system for hearing-impaired users, where correctness, latency, and clarity directly affect usability and accessibility. Both systems required designing pipelines that balance accuracy, performance, and interpretability—constraints that are equally critical in healthcare decision systems.

This project extends that same paradigm into health tech. Converting annotated clinical text into decision trees is not just a data transformation task—it creates a decision engine that can influence triage, prioritization, and workflow efficiency. Like my previous work, the value lies in turning imperfect, human-generated input into deterministic, trustworthy outputs. I am particularly motivated by the opportunity to build systems that are not only technically sound, but also have a tangible impact on patient care and health outcomes.

---

## Proposed Approach

### Key Design Principle
Introduce an Intermediate Representation (IR) to decouple:
- NLP / parsing
- Rule logic
- Decision tree construction

---

## System Architecture

```
Input Sources
 ├── Free-text rules
 └── Doccano JSONL annotations
            │
            ▼
Data Ingestion Layer
 ├── Parse JSONL
 ├── Extract entities / labels
 └── Normalize text
            │
            ▼
Rule Extraction Engine (Python)
 ├── Pattern matching (if/when logic)
 ├── Dependency parsing (spaCy)
 └── Condition + action extraction
            │
            ▼
Intermediate Representation (IR)
 ├── Atomic conditions
 ├── Logical operators (AND / OR)
 └── Actions
            │
            ▼
Rule Validation Layer
 ├── Conflict detection
 ├── Rule prioritization
 └── Deduplication
            │
            ▼
Decision Tree Builder
 ├── Condition splitting
 ├── Tree optimization
 └── Subtree merging
            │
            ▼
JSON Decision Tree Output
            │
     ┌──────┴────────┐
     ▼               ▼
Execution Engine     Admin UI (TypeScript)
 ├── Input: patient  ├── Flowchart rendering
 ├── Tree traversal  ├── Rule editing
 ├── Priority output └── Debugging paths
 └── Explainability
```

---

## Core Components

### 1. Data Ingestion
- Parse Doccano JSONL format
- Extract entities, labels, relationships

### 2. Rule Extraction Engine
- Hybrid approach:
  - Rule-based parsing
  - NLP-assisted parsing (spaCy)

Example:
```json
{
  "conditions": [
    {"field": "age", "operator": ">", "value": 65},
    {"field": "has_fever", "operator": "==", "value": true}
  ],
  "logic": "AND",
  "action": "high_priority"
}
```

### 3. Intermediate Representation (IR)
- Canonical format for rules
- Enables validation and modularity

### 4. Rule Validation & Conflict Resolution
- Detect contradictions and overlaps
- Resolve via priority or specificity

### 5. Decision Tree Builder
- Convert rules into optimized tree
- Merge equivalent branches

### 6. JSON Output Schema
```json
{
  "type": "condition",
  "field": "age",
  "operator": ">",
  "value": 65,
  "true": {
    "type": "condition",
    "field": "has_fever",
    "operator": "==",
    "value": true,
    "true": { "type": "action", "value": "high_priority" },
    "false": { "type": "action", "value": "medium_priority" }
  },
  "false": {
    "type": "action",
    "value": "low_priority"
  }
}
```

### 7. Execution Engine
- Input: patient data
- Output: classification + decision path

### 8. Frontend Integration (TypeScript)
- Flowchart visualization
- Rule editing and debugging

---

## Expected Outcomes

- End-to-end decision tree generation pipeline
- JSON output usable in admin UI
- Optional execution engine and explainability

---

## Technical Stack

- Backend: Python (spaCy)
- Frontend: TypeScript (React)
- Data: JSON / JSONL

---

## Project Questions

1. Annotation structure and schema?
2. Supported logic complexity (AND/OR, temporal)?
3. Conflict resolution strategy?
4. Evaluation metrics?
5. Patient data availability and format?
6. UI requirements (editing vs visualization)?

---

## Why This Approach

- Modular
- Extensible
- Explainable
- Production-oriented

---

## Timeline

- Weeks 1–2: Data ingestion
- Weeks 3–5: Rule extraction + IR
- Weeks 6–8: Tree construction
- Weeks 9–10: Output + testing
- Weeks 11–12: UI integration
