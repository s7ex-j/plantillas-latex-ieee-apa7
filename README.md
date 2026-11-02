# Plantillas LaTeX IEEE y APA 7 — Basadas en JAES

[![Compilar plantillas](https://github.com/s7ex-j/plantillas-latex-ieee-apa7/actions/workflows/latex.yml/badge.svg)](https://github.com/s7ex-j/plantillas-latex-ieee-apa7/actions/workflows/latex.yml)
![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)

> **Autor:** Jharol Vilca (Andre)  
> **Repositorio:** https://github.com/s7ex-j/plantillas-latex-ieee-apa7  
> **Basada en:** Clase `jaes.cls` original de *Journal of the Audio Engineering Society* (AES), adaptada al español y para uso académico en ingeniería.

---

## 📁 Estructura del repositorio

```
plantillas-latex-ieee-apa7/
├── Plantilla IEEE/          # Estilo IEEE numérico [1], [2]... (BibTeX clásico)
│   ├── plantilla_ieee.tex   # Documento principal
│   ├── jaes.cls             # Clase del documento (formato JAES)
│   ├── jaes.bst             # Estilo bibliográfico IEEE numérico
│   ├── jaes.bib             # Base de datos de referencias
│   └── README.md            # Instrucciones específicas IEEE
│
├── Plantilla APA7/          # Estilo APA 7 oficial (Autor, Año) — biblatex-apa + biber
│   ├── plantilla_apa7.tex   # Documento principal
│   ├── jaes.cls             # Clase del documento (formato JAES)
│   ├── jaes.bib             # Base de datos de referencias (formato biblatex)
│   └── README.md            # Instrucciones específicas APA 7
│
├── MEMORIA_LATEX.md         # Documentación técnica completa del entorno
└── .gitignore               # Ignora archivos auxiliares de LaTeX
```

---

## 🎯 Qué incluye cada plantilla

### Plantilla IEEE (`Plantilla IEEE/`)
- **Estilo de citas:** Numérico `[1]`, `[2]`... orden de aparición
- **Motor:** BibTeX clásico (`bibtex`)
- **Archivo de estilo:** `jaes.bst` (incluido, nativo de la plantilla)
- **Verificada:** compila sin errores con `latexmk` en cada push (GitHub Actions, TeX Live), formato dos columnas correcto. BibTeX solo avisa `empty doi` si una entrada no tiene DOI

### Plantilla APA 7 (`Plantilla APA7/`)
- **Estilo de citas:** APA 7 oficial — `(Autor, Año)` / `Autor (Año)`, bibliografía alfabética
- **Motor:** `biblatex` + `biber` (backend obligatorio)
- **Paquete clave:** `biblatex-apa` (implementación oficial APA 7)
- **Adaptación original:** No existía en Overleaf; la creé desde cero resolviendo el conflicto `jaes.cls` vs `biblatex` (`\bibparsep` / `\biblabelsep`)
- **Verificada:** Compilación completa 0 errores, 0 warnings, `biber` sin errores, `sentence case` automático, `&` en cita parentética con dos autores

---

## 🛠 Entorno de trabajo verificado

| Componente | Versión / Configuración |
|---|---|
| **SO** | Windows 11 |
| **Editor** | VS Code + extensión LaTeX Workshop |
| **Motor** | `latexmk` (vía Strawberry Perl) |
| **Distribución** | MiKTeX (auto-instalación de paquetes activada) |
| **Clase base** | `\documentclass[fleqn, final]{jaes}` |

---

## 🚀 Cómo usar

### Opción A: IEEE (recomendado para inicio rápido)
```bash
cd "Plantilla IEEE"
# En VS Code: abre la carpeta y compila con el botón ▶ de LaTeX Workshop
# O en terminal:
latexmk -pdf plantilla_ieee.tex
```

### Opción B: APA 7 (si tu curso/revista exige APA)
```bash
cd "Plantilla APA7"
# En VS Code: abre la carpeta y compila con el botón ▶
# Primera compilación: MiKTeX instalará biblatex, biblatex-apa, csquotes, biber
# Segunda compilación (normal): las citas aparecen correctamente
# O en terminal:
latexmk -pdf plantilla_apa7.tex
```

> **Nota:** La primera compilación APA 7 tarda más porque MiKTeX descarga paquetes. La segunda es rápida.

---

## 📝 Autoría y adaptación

Esta adaptación la realicé yo (**Jharol Vilca / Andre**) para uso académico en ingeniería (UPN / ISIL), partiendo de la clase `jaes.cls` oficial de AES. Cambios principales:

- ✅ Traducción completa al español (cabeceras, pie de página, "Table"→"Tabla", "References"→"Referencias", "BORRADOR" configurable)
- ✅ Corrección de `cleveref`: conjunciones "y" en vez de "and"
- ✅ Fix del bug conocido de `\biography` (no imprime texto) → sección manual "Sobre el Equipo de Trabajo"
- ✅ Instrucciones para quitar marca de agua "BORRADOR" en entrega final
- ✅ **APA 7 original:** Resolución del conflicto `jaes.cls` vs `biblatex` (`\let\bibparsep\undefined`, `\let\biblabelsep\undefined` antes de cargar `biblatex`)
- ✅ Documentación exhaustiva en `MEMORIA_LATEX.md` (entorno, paquetes, bugs, workarounds, apéndices, tablas anchas, figuras, bibliografía)

---

## 📄 Licencia

![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)

| Componente | Licencia | Copyright |
|---|---|---|
| **Adaptaciones mías** (`plantilla_ieee.tex`, `plantilla_apa7.tex`, `jaes.bst`, `jaes.bib`, READMEs, `MEMORIA_LATEX.md`, `.gitignore`) | **[MIT](LICENSE)** | (c) 2026 Jharol Vilca (Andre) |
| **Clase base `jaes.cls`** (incluida en ambas carpetas) | Licencia original AES | (c) 2010 Audio Engineering Society |

> La licencia MIT cubre mis adaptaciones y plantillas. La clase `jaes.cls` original mantiene su licencia AES — se incluye aquí tal cual para que la plantilla compile out-of-the-box, práctica estándar en CTAN/Overleaf.

---

## 🔗 Enlaces útiles

- [Clase JAES original en CTAN](https://ctan.org/pkg/jaes)
- [biblatex-apa (implementación oficial APA 7)](https://github.com/plk/biblatex-apa)
- [MiKTeX](https://miktex.org/)
- [LaTeX Workshop (VS Code)](https://marketplace.visualstudio.com/items?itemName=James-Yu.latex-workshop)