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
├── thesis/                        # LaTeX thesis source (BTH template)
│   ├── bth-thesis.tex             ← ENTRY POINT — compile this file
│   ├── thesis-refs.bib            ← Bibliography (BibTeX)
│   ├── bth.cls                    ← BTH document class (do not modify)
│   ├── bthnotext.pdf              ← BTH logo resource
│   ├── changepage.sty             ← Style dependency
│   ├── chapters/                  ← Chapter .tex files (add content here)
│   │   ├── introduction.tex
│   │   ├── related_work.tex
│   │   ├── methodology.tex
│   │   ├── results.tex
│   │   ├── discussion.tex
│   │   └── conclusion.tex
│   ├── figures/                   ← Thesis figures (architectures, diagrams, etc.)
│   │   ├── architectures/
│   │   ├── threat_models/
│   │   └── diagrams/
│   └── appendix/
│
├── experiments/                   # Experimental data organized by scenario
│   ├── scenario1/
│   │   ├── architecture/          ← TMT model + architecture diagrams
│   │   ├── owasp/                 ← OWASP Threat Dragon outputs
│   │   ├── tmt/                   ← Microsoft TMT outputs
│   │   ├── llm_outputs/           ← ChatGPT, Ollama, ThreMOLIA outputs
│   │   └── analysis/              ← Scenario-level analysis notes
│   ├── scenario2/
│   └── scenario3/
│
├── prompts/                       # Prompts used for LLM evaluation
│   ├── chatgpt_prompts/
│   ├── ollama_prompts/
│   └── thremolia_prompts/
│
├── results/                       # Comparative analysis and evaluation
│   ├── spreadsheets/              ← Master evaluation spreadsheet
│   ├── comparison_tables/
│   └── generated_reports/
│
├── documentation/                 # Project management materials
│   ├── project_plan/
│   ├── supervisor_feedback/
│   └── meeting_notes/
│
├── README.md
└── .gitignore
```

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

- **Raw experimental outputs are preserved exactly as generated** — do not modify files under `experiments/`.
- **Do not rename** `bth-thesis.tex`, `thesis-refs.bib`, or `bth.cls` — the BTH template depends on these exact names.
- Overleaf two-way Git sync (push) requires **Overleaf Premium**. Read-only access is available at: https://www.overleaf.com/read/wpkgfpkpfwgp
