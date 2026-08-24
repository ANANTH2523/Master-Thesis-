# Threat Modelling of LLM-Integrated Systems
**Subtitle:** A Comparative Analysis of OWASP Threat Dragon and Microsoft Threat Modeling Tool

**Authors:** Ananth Chowdary Ramineni · Vamshi Krishna Burri
**Institution:** Blekinge Institute of Technology (BTH) — Faculty of Computing
**Programme:** Master of Science in Software Engineering

---

## 📁 Repository Structure

```text
Master-Thesis-/
│
├── README.md
│
├── thesis/                          # LaTeX thesis source (BTH template)
│   ├── bth-thesis.tex               ← ENTRY POINT — compile this file
│   ├── thesis-refs.bib              ← Bibliography (BibTeX)
│   ├── bth.cls                      ← BTH document class (do not modify)
│   ├── bthnotext.pdf                ← BTH logo resource
│   ├── changepage.sty               ← Style dependency
│   ├── chapters/                    ← Chapter .tex files (add content here)
│   │   ├── introduction.tex
│   │   ├── related_work.tex
│   │   ├── methodology.tex
│   │   ├── results.tex
│   │   ├── discussion.tex
│   │   └── conclusion.tex
│   ├── figures/                     ← Thesis figures (architectures, diagrams, etc.)
│   │   ├── architectures/
│   │   ├── threat_models/
│   │   └── diagrams/
│   └── appendix/
│
├── reference_models/                # Baseline architectures and reference threat models
│   ├── scenario_1/                  ← Scenario 1 reference architecture + threat model
│   ├── scenario_2/                  ← Scenario 2 reference architecture + threat model
│   └── scenario_3/                  ← Scenario 3 reference architecture + threat model
│
├── results/                         # Final processed threat modelling analysis outputs
│   ├── Master_Threat_Modelling_Analysis_Final.xlsx  ← Master analysis spreadsheet
│   ├── coverage_analysis/           ← Coverage metrics and analysis outputs
│   ├── threat_mappings/             ← Threat mapping comparison outputs
│   ├── figures/                     ← Generated figures and tables for thesis chapters
│   ├── comparison_tables/           ← Side-by-side tool comparison tables
│   └── generated_reports/           ← Automatically generated evaluation reports
│
├── data/                            # Raw collected outputs before analysis
│   ├── raw_outputs/                 ← Placeholder for unprocessed raw exports
│   ├── exported_results/            ← Placeholder for exported data files
│   ├── scenario1/
│   │   ├── architecture/            ← TMT model (.tm7) + architecture diagrams
│   │   ├── owasp/                   ← OWASP Threat Dragon outputs
│   │   ├── tmt/                     ← Microsoft TMT outputs
│   │   ├── llm_outputs/             ← ChatGPT, Ollama, ThreMOLIA outputs
│   │   └── analysis/                ← Scenario-level analysis notes
│   ├── scenario2/                   ← (same substructure as scenario1)
│   └── scenario3/                   ← (same substructure as scenario1)
│
├── prompts/                         # Prompts used for LLM evaluation
│   ├── chatgpt_prompts/
│   ├── ollama_prompts/
│   └── thremolia_prompts/
│
└── documentation/                   # Supporting methodology and analysis documentation
    ├── methodology/                 ← Research methodology notes and decisions
    ├── analysis_notes/              ← Analysis working notes
    ├── project_plan/                ← Project planning documents
    ├── supervisor_feedback/         ← Supervisor review comments
    └── meeting_notes/               ← Meeting minutes and action items
```

---

## 📂 Folder Descriptions

| Folder | Purpose |
|--------|---------|
| `thesis/` | Contains all LaTeX documentation files for the thesis. Entry point is `bth-thesis.tex`. |
| `reference_models/` | Contains baseline architectures and reference threat models used for comparison in the thesis methodology. These are the ground-truth models against which LLM-generated outputs are evaluated. |
| `results/` | Contains final processed threat modelling analysis outputs used in the thesis Results and Analysis chapters. The master spreadsheet `Master_Threat_Modelling_Analysis_Final.xlsx` is the primary analysis artefact. |
| `data/` | Contains raw collected outputs before analysis — LLM responses, tool exports, and architecture files organised by scenario. Do not modify files in this folder. |
| `prompts/` | Contains the prompt templates used to query ChatGPT, Ollama/Llama, and ThreMOLIA during experiments. |
| `documentation/` | Contains supporting methodology and analysis documentation including project planning, supervisor feedback, and meeting notes. |

---

## ⚙️ LaTeX Compilation Instructions

### Prerequisites
- Install a full TeX distribution: [TeX Live](https://tug.org/texlive/) (Linux/Windows) or [MacTeX](https://tug.org/mactex/) (macOS)

### Compile locally
```bash
cd thesis/
pdflatex bth-thesis.tex
bibtex bth-thesis
pdflatex bth-thesis.tex
pdflatex bth-thesis.tex
```

> Run `pdflatex` three times to resolve all cross-references and the table of contents.

### Key files
| File | Purpose |
|------|---------|
| `thesis/bth-thesis.tex` | **Main entry point** — compile this |
| `thesis/thesis-refs.bib` | All bibliography references (BibTeX) |
| `thesis/bth.cls` | BTH document class — **do not rename or modify** |
| `thesis/changepage.sty` | Required style package |

---

## 🔄 Overleaf ↔ GitHub Workflow

This repository uses **two remotes**:

| Remote | URL | Purpose |
|--------|-----|---------| 
| `origin` | `https://github.com/ANANTH2523/Master-Thesis-.git` | Version control, backup, artifact storage |
| `overleaf` | `https://git.overleaf.com/wpkgfpkpfwgp` | Cloud compilation and collaborative editing |

### Setup (first time)
```bash
git remote add overleaf https://git.overleaf.com/wpkgfpkpfwgp
```

### Daily editing workflow
```
Overleaf (write & compile)
      │
      ▼  (Overleaf Menu → GitHub Sync  OR  git pull overleaf main)
GitHub (version control & backup)
      │
      ▼  (git pull origin development)
Local IDE (data analysis, structure, experiments)
```

### Sync from Overleaf → GitHub
```bash
git pull overleaf main --allow-unrelated-histories
git push origin development
```

### Sync from GitHub → Overleaf
```bash
git push overleaf main   # Requires Overleaf Premium
```

### Typical local commit
```bash
git add thesis/chapters/introduction.tex
git commit -m "Added introduction chapter"
git push origin development
```

---

## 🌿 Branch Strategy

| Branch | Purpose |
|--------|---------|
| `main` | Final/stable thesis milestones |
| `development` | Active writing, experiments, daily work |

---

## 📌 Important Notes

- **Raw experimental outputs are preserved exactly as generated** — do not modify files under `data/`.
- **Reference models are read-only baselines** — do not modify files under `reference_models/`.
- **Do not rename** `bth-thesis.tex`, `thesis-refs.bib`, or `bth.cls` — the BTH template depends on these exact names.
- Overleaf two-way Git sync (push) requires **Overleaf Premium**. Read-only access is available at: https://www.overleaf.com/read/wpkgfpkpfwgp
# Comparative Study of Large Language Models and Classical Threat Modelling Tools

## Evaluating LLM-Based Threat Generation Against Established Threat Modelling Approaches

---

# Overview

This repository contains the research artifacts, documentation, experimental
results, and analysis material developed for the Master's thesis:

**"Comparative Study of Large Language Models and Classical Threat Modelling
Tools"**

The thesis investigates the differences between traditional threat modelling
tools and Large Language Model (LLM)-based approaches for automated security
threat identification.

The study compares classical security engineering tools with AI-based threat
generation approaches to understand their capabilities, limitations, and
practical applicability in software security analysis.

The research focuses on evaluating whether LLM-based approaches can provide
comparable or complementary threat modelling capabilities when compared with
established security tools.


---

# Authors

## Ananth Chowdary Ramineni

Master of Science in Software Engineering  
Blekinge Institute of Technology (BTH)

Email:
anrb24@student.bth.se


## Vamshi Krishna Burri

Master of Science in Software Engineering  
Blekinge Institute of Technology (BTH)

Email:
vabr24@student.bth.se


---

# Research Objective

Threat modelling is an essential security engineering activity used to
identify potential vulnerabilities, attack scenarios, and security risks
during software design.

Traditional threat modelling approaches rely on predefined methodologies,
structured security knowledge, and manually created system models.

Recent advances in Large Language Models have introduced new possibilities
for automated security analysis, where LLMs can reason about system
architectures and generate potential security threats using natural language.

However, the effectiveness, reliability, and limitations of LLM-based threat
generation compared with classical threat modelling approaches remain an
important research question.

This thesis performs a comparative empirical study between:

- Classical threat modelling tools
- LLM-based threat generation approaches

The objective is to analyse their:

- Threat identification capability
- Security coverage
- Threat relevance
- Quality of generated outputs
- Identification of LLM-specific security risks
- Practical applicability


---

# Research Approaches Evaluated

## Classical Threat Modelling Approaches

### OWASP Threat Dragon

OWASP Threat Dragon is an open-source threat modelling tool that uses
architecture-based modelling and Data Flow Diagrams (DFDs) to identify
security threats.


### Microsoft Threat Modeling Tool

Microsoft Threat Modeling Tool is an industry-oriented threat modelling tool
based on STRIDE methodology for identifying security threats from system
architectures.


---

## LLM-Based Threat Generation Approaches

### ChatGPT

ChatGPT is evaluated as a general-purpose Large Language Model capable of
analysing software architectures and generating security threat information.


### Ollama

Ollama is evaluated as a local LLM execution framework to analyse the
capability of locally deployed language models for threat generation.


### ThreMoLIA

ThreMoLIA is evaluated as an LLM-based threat modelling approach designed to
support automated security threat identification.


---

# Experimental Scenarios

The study evaluates multiple software system scenarios containing different
architectural characteristics.

Each scenario includes:

- System components
- External actors
- Data flows
- Data stores
- Trust boundaries
- Security assumptions

The same architectural information is provided to all evaluated approaches to
ensure a fair comparison.


---

# Evaluation Criteria

Generated threats are evaluated using multiple criteria:

## Threat Coverage

Measures how effectively an approach identifies relevant security threats.


## Threat Relevance

Evaluates whether generated threats are applicable to the analysed system
architecture.


## Evidence Grounding

Determines whether identified threats are supported by actual system
components, data flows, or trust boundaries.


## Redundancy

Evaluates duplicate or repeated threat generation.


## LLM-Specific Security Risk Identification

Analyses whether approaches identify security risks introduced by AI-based
components.

Examples:

- Prompt injection
- Sensitive information disclosure
- Insecure output handling
- Excessive model authority
- Data poisoning

---

# Threat Generation Workflow


## Step 1: Architecture Preparation

System architectures are created and documented with:

- Components
- Actors
- Data flows
- Trust boundaries


## Step 2: Classical Tool Analysis

Architectures are modelled using:

- OWASP Threat Dragon
- Microsoft Threat Modeling Tool


Generated threats are collected and stored.


## Step 3: LLM-Based Analysis

The same architecture information is provided to:

- ChatGPT
- Ollama
- ThreMoLIA


Generated outputs are collected for comparison.


## Step 4: Threat Analysis

Threat outputs are normalized into a common format containing:

- Threat description
- Category
- Affected component
- Security impact
- Mitigation
- Architectural evidence


---

# Reproducibility

This repository contains the required artifacts to understand and reproduce
the research process.

Available materials include:

- Thesis documentation
- System architectures
- Threat modelling outputs
- LLM prompts
- Generated results
- Analysis spreadsheets
- Comparison tables


To reproduce the study:

1. Review the provided system architectures.
2. Apply the documented threat generation procedure.
3. Generate threats using the evaluated approaches.
4. Normalize and compare generated outputs.
5. Analyse results using the provided evaluation criteria.


---

# Software and Tools Used

## Threat Modelling Tools

- OWASP Threat Dragon
- Microsoft Threat Modeling Tool


## AI-Based Approaches

- ChatGPT
- Ollama
- ThreMoLIA


## Documentation

- LaTeX
- Overleaf


## Data Analysis

- Spreadsheet-based analysis tools


---

# Version Control

The repository uses Git for maintaining research history and documentation.

## Branches

### main

Contains stable and reviewed versions.


### development

Contains ongoing research work, experiments, and updates.


Example commit messages:

Added Scenario 1 architecture
Generated OWASP threat model outputs
Added Microsoft TMT analysis
Updated LLM comparison spreadsheet
Added methodology chapter


---

# Academic Context

This repository is developed as part of the Master's Degree Project:

**Master of Science in Software Engineering**

Blekinge Institute of Technology

Karlskrona, Sweden


---

# License

This repository contains academic research material developed as part of a
Master's thesis project.

The material may be used for academic and research purposes with appropriate
citation.
