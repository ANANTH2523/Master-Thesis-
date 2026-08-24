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

# Research Methodology

The study follows an empirical comparative research methodology.

The evaluation process consists of:

System Scenario Selection
      ↓
Architecture Preparation
      ↓
Threat Generation
      ↓
Threat Collection
      ↓
Threat Normalization
      ↓
Comparative Analysis
      ↓
Evaluation of Strengths and Limitations


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

# Repository Structure


Master-Thesis-
│
├── Thesis_Documentation/
│   ├── main.tex
│   ├── chapters/
│   ├── figures/
│   └── thesis-refs.bib
│
├── Architectures/
│   ├── Scenario_1/
│   ├── Scenario_2/
│   └── Scenario_3/
│
├── Classical_Tools/
│
│   ├── OWASP_Threat_Dragon/
│   │
│   └── Microsoft_Threat_Modeling_Tool/
│
├── LLM_Approaches/
│
│   ├── ChatGPT/
│   ├── Ollama/
│   └── ThreMoLIA/
│
├── Analysis/
│
│   ├── Master_Comparison_Spreadsheet.xlsx
│   ├── Threat_Mapping/
│   ├── Evaluation_Tables/
│   └── Graphs/
│
├── Literature/
│   ├── Research_Papers/
│   ├── Notes/
│   └── References/
│
├── Meetings/
│   ├── Supervisor_Feedback/
│   └── Meeting_Notes/
│
└── README.md


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
