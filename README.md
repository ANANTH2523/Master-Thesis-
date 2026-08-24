# Threat Modelling of LLM-Integrated Systems
**Subtitle:** A Comparative Analysis of OWASP Threat Dragon and Microsoft Threat Modeling Tool

## Authors
- **Ananth Chowdary Ramineni**
- **Vamshi Krishna Burri**

*Blekinge Institute of Technology (BTH)*

## Project Purpose
This repository manages the LaTeX source code, methodologies, references, and experimental datasets for our Master's thesis. The goal is to comparatively analyze classical threat modelling approaches versus LLM-driven threat models.

## Repository Structure Overview
```text
Master-Thesis/
├── thesis/               # LaTeX source code, chapters, references, figures
├── experiments/          # Evaluation architectures and generated threat models per scenario
├── prompts/              # System prompts used for ChatGPT, Ollama, and ThreMOLIA
├── results/              # Comparative tables, generated reports, and analysis spreadsheets
└── documentation/        # Project plans, notes, and supervisor feedback
```

## Compilation Instructions
To compile the thesis locally:
1. Ensure you have a full TeX distribution installed (e.g., TeX Live, MiKTeX, MacTeX).
2. Navigate to the `thesis/` directory.
3. Run `pdflatex main.tex` to generate the PDF.
4. Run `bibtex main` (or biber) to compile the bibliography.
5. Run `pdflatex main.tex` twice more to resolve references and TOC.

Alternatively, this repository is synchronized with Overleaf for cloud-based collaborative editing.

## Contribution Workflow
- **`main` branch:** Reserved for major milestones and stable draft releases.
- **`development` branch:** Used for active writing, daily experiments, and continuous updates.

### Committing Changes
Please use meaningful commit messages to track the academic progress.
Examples:
- `Added introduction chapter`
- `Added related work analysis`
- `Added OWASP scenario 1 results`
- `Updated methodology chapter`
