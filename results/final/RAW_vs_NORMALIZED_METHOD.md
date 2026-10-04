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
|  Files: scenario1_raw.csv (N=106)                            |
|         scenario2_raw.csv (N=105)                            |
|         scenario3_raw.csv / sce 3 thremolia.csv (N=117)      |
|  - Verbatim output from automated run                       |
|  - Demonstrates broad STRIDE coverage across all elements   |
|  - Preserves potential redundancies & permutations          |
|  - Retained for reproducibility                             |
+-------------------------------------------------------------+
                              |
                              |  1. Architectural Grounding Audit
                              |  2. Semantic Duplicate Consolidation
                              |  3. Multi-Framework Mapping
                              v
+-------------------------------------------------------------+
|                 NORMALIZED / VALIDATED LAYER                |
|  Files: scenario1_normalized.xlsx / .pdf                    |
|         scenario2_normalized.xlsx / .pdf                    |
|         scenario3_normalized.xlsx / .pdf                    |
|         all_scenarios_normalized_master.xlsx                |
|  - Consolidated canonical threat findings                   |
|  - Explicit architectural evidence & traceability           |
|  - Mapped across MITRE, OWASP, ATLAS, LINDDUN               |
|  - Used for secondary normalization analysis                |
+-------------------------------------------------------------+
```

---

## 2. Dataset Definitions

### RAW OUTPUT (`scenarioX_raw.csv` / `scenarioX_raw.json` / `sce X thremolia.csv`)
- **Definition**: The complete set of threat items emitted by ThreMoLIA during execution.
- **Scientific Role**: Retained for experimental provenance and reproducibility. It demonstrates how ThreMoLIA systematically applies STRIDE across all identified components and micro-flows.
- **Handling of Duplicates**: Mechanical permutations (e.g. repeated transport-layer sniffing threats across adjacent data flows) are preserved verbatim without alteration.

### NORMALIZED OUTPUT (`scenarioX_normalized.xlsx` / `all_scenarios_normalized_master.xlsx`)
- **Definition**: The refined dataset produced by applying structured cybersecurity auditing rules to the raw findings.
- **Scientific Role**: Used to evaluate distinct canonical vulnerability mechanisms and evidence grounding for secondary normalization benchmarking.
- **Audit Steps Applied**:
  1. *Semantic Deduplication*: Consolidates identical vulnerability mechanisms on adjacent sub-flows into canonical threats.
  2. *Architectural Grounding*: Verifies that every retained threat traces directly to explicit architecture components, flows, and trust boundaries without assuming unstated technologies (e.g., Redis, WAF, SIEM).
  3. *Multi-Framework Alignment*: Maps canonical threats across MITRE ATT&CK, OWASP Top 10, MITRE ATLAS, OWASP for LLM, LINDDUN, and ML Security Top 10.


---

## 3. Important Methodological Principle
The normalized dataset **does NOT replace** the raw output. Both layers remain independently accessible and fully documented to ensure complete academic transparency.
