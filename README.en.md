# IEEE and APA 7 LaTeX Templates — Based on JAES

[![Build templates](https://github.com/s7ex-j/plantillas-latex-ieee-apa7/actions/workflows/latex.yml/badge.svg)](https://github.com/s7ex-j/plantillas-latex-ieee-apa7/actions/workflows/latex.yml)
![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)

<sub>[Español](README.md) · **English**</sub>

> **Author:** Jharol Vilca (Andre)  
> **Repository:** https://github.com/s7ex-j/plantillas-latex-ieee-apa7  
> **Based on:** the original `jaes.cls` class from the *Journal of the Audio Engineering Society* (AES), adapted to Spanish for academic use in engineering.

> The templates produce documents in Spanish, and `MEMORIA_LATEX.md` and the per-template READMEs are in Spanish.

---

## 📁 Repository structure

```
plantillas-latex-ieee-apa7/
├── Plantilla IEEE/          # Numeric IEEE style [1], [2]... (classic BibTeX)
│   ├── plantilla_ieee.tex   # Main document
│   ├── jaes.cls             # Document class (JAES format)
│   ├── jaes.bst             # Numeric IEEE bibliography style
│   ├── jaes.bib             # Reference database
│   └── README.md            # IEEE-specific instructions
│
├── Plantilla APA7/          # Official APA 7 style (Author, Year) — biblatex-apa + biber
│   ├── plantilla_apa7.tex   # Main document
│   ├── jaes.cls             # Document class (JAES format)
│   ├── jaes.bib             # Reference database (biblatex format)
│   └── README.md            # APA 7-specific instructions
│
├── MEMORIA_LATEX.md         # Full technical documentation of the setup
└── .gitignore               # Ignores LaTeX auxiliary files
```

---

## 🎯 What each template includes

### IEEE template (`Plantilla IEEE/`)
- **Citation style:** Numeric `[1]`, `[2]`... in order of appearance
- **Engine:** classic BibTeX (`bibtex`)
- **Style file:** `jaes.bst` (included, native to the template)
- **Verified:** compiles with no errors using `latexmk` on every push (GitHub Actions, TeX Live), correct two-column layout. BibTeX only warns `empty doi` when an entry has no DOI

### APA 7 template (`Plantilla APA7/`)
- **Citation style:** official APA 7 — `(Author, Year)` / `Author (Year)`, alphabetical bibliography
- **Engine:** `biblatex` + `biber` (required backend)
- **Key package:** `biblatex-apa` (official APA 7 implementation)
- **Original adaptation:** It did not exist on Overleaf; I built it from scratch, solving the `jaes.cls` vs `biblatex` conflict (`\bibparsep` / `\biblabelsep`)
- **Verified:** full build with 0 errors and 0 warnings, `biber` with no errors, automatic `sentence case`, `&` in parenthetical citations with two authors

---

## 🛠 Verified working environment

| Component | Version / Configuration |
|---|---|
| **OS** | Windows 11 |
| **Editor** | VS Code + LaTeX Workshop extension |
| **Engine** | `latexmk` (via Strawberry Perl) |
| **Distribution** | MiKTeX (automatic package installation on) |
| **Base class** | `\documentclass[fleqn, final]{jaes}` |

---

## 🚀 How to use

### Option A: IEEE (recommended for a quick start)
```bash
cd "Plantilla IEEE"
# In VS Code: open the folder and build with LaTeX Workshop's ▶ button
# Or in a terminal:
latexmk -pdf plantilla_ieee.tex
```

### Option B: APA 7 (if your course/journal requires APA)
```bash
cd "Plantilla APA7"
# In VS Code: open the folder and build with the ▶ button
# First build: MiKTeX will install biblatex, biblatex-apa, csquotes, biber
# Second build (normal): citations show up correctly
# Or in a terminal:
latexmk -pdf plantilla_apa7.tex
```

> **Note:** The first APA 7 build takes longer because MiKTeX downloads packages. The second one is fast.

---

## 📝 Authorship and adaptation

I (**Jharol Vilca / Andre**) made this adaptation for academic use in engineering (UPN / ISIL), starting from the official AES `jaes.cls` class. Main changes:

- ✅ Full translation to Spanish (headers, footer, "Table"→"Tabla", "References"→"Referencias", configurable "BORRADOR" draft mark)
- ✅ `cleveref` fix: conjunction "y" instead of "and"
- ✅ Fix for the known `\biography` bug (prints no text) → manual "Sobre el Equipo de Trabajo" section
- ✅ Instructions to remove the "BORRADOR" watermark for the final submission
- ✅ **Original APA 7:** resolution of the `jaes.cls` vs `biblatex` conflict (`\let\bibparsep\undefined`, `\let\biblabelsep\undefined` before loading `biblatex`)
- ✅ Thorough documentation in `MEMORIA_LATEX.md` (environment, packages, bugs, workarounds, appendices, wide tables, figures, bibliography)

---

## 📄 License

![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)

| Component | License | Copyright |
|---|---|---|
| **My adaptations** (`plantilla_ieee.tex`, `plantilla_apa7.tex`, `jaes.bst`, `jaes.bib`, READMEs, `MEMORIA_LATEX.md`, `.gitignore`) | **[MIT](LICENSE)** | (c) 2026 Jharol Vilca (Andre) |
| **Base class `jaes.cls`** (included in both folders) | Original AES license | (c) 2010 Audio Engineering Society |

> The MIT license covers my adaptations and templates. The original `jaes.cls` class keeps its AES license — it is included as-is so the template compiles out of the box, standard practice on CTAN/Overleaf.

---

## 🔗 Useful links

- [Original JAES class on CTAN](https://ctan.org/pkg/jaes)
- [biblatex-apa (official APA 7 implementation)](https://github.com/plk/biblatex-apa)
- [MiKTeX](https://miktex.org/)
- [LaTeX Workshop (VS Code)](https://marketplace.visualstudio.com/items?itemName=James-Yu.latex-workshop)
