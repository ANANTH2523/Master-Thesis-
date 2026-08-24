# Threat Modelling of LLM-Integrated Systems
**Subtitle:** A Comparative Analysis of Classical and LLM-Based Threat Modelling Approaches

## Authors
- **Ananth Chowdary Ramineni**
- **Vamshi Krishna Burri**

*Blekinge Institute of Technology (BTH)*

## Research Objective
This thesis investigates the efficacy, efficiency, and quality of traditional versus LLM-driven threat modelling tools. By applying various threat modelling methodologies to three controlled LLM-integrated system scenarios, this research aims to highlight the strengths, weaknesses, and potential paradigm shifts in cybersecurity architectures designed for AI systems.

## Approaches Evaluated
This repository contains raw and analyzed data from the following five threat-generation approaches:
1. **OWASP Threat Dragon** (OWASP-based threat modelling)
2. **Microsoft Threat Modeling Tool (TMT)**
3. **ChatGPT**
4. **Ollama** (Llama models)
5. **ThreMOLIA**

## Experimental Scenarios
The evaluation is conducted across three distinct, controlled LLM-integrated architectures:
- **Scenario 1:** [Description of Scenario 1]
- **Scenario 2:** [Description of Scenario 2]
- **Scenario 3:** [Description of Scenario 3]

## Repository Structure
```text
threat-modelling-llm-thesis/
├── README.md
├── .gitignore
├── thesis/               # LaTeX source code, template files, and bibliography
├── architectures/        # The three reference architectures (.tm7, .png, etc.)
├── prompts/              # Exact prompts used to generate outputs with LLMs
├── raw_outputs/          # Raw outputs from all 5 tools (unmodified)
├── analysis/             # Master spreadsheet, evaluation rubrics, and data analysis
├── documentation/        # Methodology planning and project references
└── appendix/             # Supplementary tables and raw output summaries
```

## Experimental Data Organization
All raw data generated during the experiments are stored in `raw_outputs/`. They are strictly divided by the tool used (e.g., `OWASP/`, `ChatGPT/`) and then by scenario (`scenario1/`, `scenario2/`, `scenario3/`).

> [!IMPORTANT]
> **Raw outputs are preserved exactly as they were generated.** No post-processing, corrections, or omissions have been made to these files to ensure strict academic integrity and valid comparative analysis.

## Analysis Workflow
The raw outputs were extracted and normalized into a standard format in the `analysis/master-spreadsheet/` directory. All comparative metrics, mistake tracking, and threat categorizations are conducted within these spreadsheets.

## Reproducibility Instructions
To reproduce the findings of this thesis:
1. Review the architecture diagrams in the `architectures/` folder to understand the system context.
2. Review the prompts located in the `prompts/` directory.
3. Feed the exact prompts and context into the respective LLM interfaces or tools.
4. Compare your generated results with the baseline data stored in `raw_outputs/`.

## Software & Tools Used
- **LaTeX** (BTH Thesis Template)
- **Microsoft Threat Modeling Tool (TMT)**
- **OWASP Threat Dragon**
- **OpenAI ChatGPT**
- **Ollama**
- **ThreMOLIA**
