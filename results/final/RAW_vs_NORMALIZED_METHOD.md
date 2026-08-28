# Methodology Note: Raw Experimental Output vs. Normalized Dataset

## 1. Methodological Purpose & Separation of Datasets
In academic research evaluating automated threat modeling systems, it is essential to distinguish between the **raw system output** and the **normalized analytical dataset**.

```
+-------------------------------------------------------------+
|                  ThreMoLIA System Execution                 |
+-------------------------------------------------------------+
                              |
                              v
+-------------------------------------------------------------+
|                     RAW EXPERIMENTAL LAYER                  |
|  Files: scenario1_raw_clean.csv (N=106)                     |
|         scenario2_raw_clean.csv (N=105)                     |
|         scenario3_raw_clean.csv (N=117)                     |
|  - Verbatim output from automated run                       |
|  - Demonstrates broad STRIDE coverage across all elements   |
|  - Preserves all potential redundancies & permutations      |
|  - 100% intact for reproducibility                          |
+-------------------------------------------------------------+
                              |
                              |  1. Architectural Grounding Audit
                              |  2. Semantic Duplicate Consolidation
                              |  3. Multi-Framework Mapping
                              v
+-------------------------------------------------------------+
|                 NORMALIZED / VALIDATED LAYER                |
|  Files: final_scenario_1.csv / scenario1_validated.* (N=12) |
|         final_scenario_2.csv / scenario2_validated.* (N=8)  |
|         final_scenario_3.csv / scenario3_validated.* (N=8)  |
|  - Consolidated canonical threat findings                   |
|  - Explicit architectural evidence & traceability           |
|  - Fully mapped to MITRE, OWASP, ATLAS, LINDDUN             |
|  - Used for precision, grounding, and comparative metrics   |
+-------------------------------------------------------------+
```

---

## 2. Dataset Definitions

### RAW OUTPUT (`scenarioX_raw_clean.csv` / `scenarioX_raw.json`)
- **Definition**: The complete, unfiltered set of threat items emitted by ThreMoLIA during execution.
- **Scientific Role**: Retained for complete experimental provenance and reproducibility. It demonstrates how ThreMoLIA systematically applies STRIDE across all identified components and micro-flows.
- **Handling of Duplicates**: Mechanical permutations (e.g. repeated transport-layer sniffing threats across adjacent data flows) are preserved verbatim without alteration.

### NORMALIZED OUTPUT (`final_scenario_X.csv` / `scenarioX_validated.*`)
- **Definition**: The refined dataset produced by applying formal cybersecurity auditing rules to the raw findings.
- **Scientific Role**: Used to evaluate threat precision, evidence grounding, and distinct vulnerability coverage for Master's thesis benchmarking.
- **Audit Steps Applied**:
  1. *Semantic Deduplication*: Consolidates identical vulnerability mechanisms on adjacent sub-flows into canonical threats.
  2. *Architectural Grounding*: Verifies that every retained threat traces directly to explicit architecture components, flows, and trust boundaries without assuming unstated technologies (e.g., Redis, WAF, SIEM).
  3. *Multi-Framework Alignment*: Maps canonical threats across MITRE ATT&CK, OWASP Top 10, MITRE ATLAS, OWASP for LLM, LINDDUN, and ML Security Top 10.

---

## 3. Important Methodological Principle
The normalized dataset **does NOT replace** the raw output. Both layers remain independently accessible and fully documented to ensure complete academic transparency.
