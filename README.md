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
