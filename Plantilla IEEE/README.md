# Plantilla JAES — Estilo IEEE (numérico)

> **Autor:** Jharol Vilca (Andre)  
> **Repositorio:** https://github.com/s7ex-j/plantillas-latex-ieee-apa7  
> **Basada en:** Clase `jaes.cls` original de *Journal of the Audio Engineering Society* (AES), adaptada al español y para uso académico en ingeniería.

---

## Archivos de esta carpeta

| Archivo | Qué es | ¿Lo edito? |
|---|---|---|
| `plantilla_ieee.tex` | El documento en sí. Aquí escribes tu trabajo. | Sí, es tu plantilla de trabajo. |
| `jaes.cls` | Define el formato visual (tipografía, márgenes, dos columnas, etc.). | No, salvo casos puntuales (ver abajo). |
| `jaes.bst` | Define cómo se ve la bibliografía (estilo IEEE numérico `[1]`, `[2]`...). | No. |
| `jaes.bib` | Tu base de datos de referencias. Aquí agregas las fuentes reales que vas citando. | Sí, agrega tus referencias aquí. |

No hace falta ningún archivo `.sty` adicional: los paquetes que usa esta plantilla (`cleveref`, `tabularx`, `graphicx`, `orcidlink`, `cuted`, etc.) son utilidades estándar que MiKTeX autoinstala la primera vez que las necesita.

---

## Esta plantilla fue verificada por compilación completa

A diferencia de la versión APA7 (que usa `biblatex-apa`), esta versión IEEE **sí se compiló de extremo a extremo** antes de publicarse: 0 errores, 0 warnings, formato de dos columnas correcto, tabla simple, tabla ancha, ORCID, citas y bibliografía numérica funcionando, sección de equipo de trabajo correcta.

---

## Cómo compilar en VS Code + LaTeX Workshop + MiKTeX

1. Abre **esta carpeta completa** en VS Code (Archivo → Abrir carpeta).
2. Abre `plantilla_ieee.tex` y compílalo con el botón de LaTeX Workshop (▶) o guardando si tienes "build on save" activado.
3. La primera vez que MiKTeX detecte que falta algún paquete (poco probable aquí, todo es estándar), te preguntará si lo instala: acepta.
4. La receta por defecto de LaTeX Workshop (`latexmk`) detecta sola que el documento usa `\bibliography{}` + `\bibliographystyle{}` con BibTeX clásico, y corre `bibtex` en el orden correcto las veces que haga falta. No necesitas hacerlo a mano.
4. Si compilas por terminal en vez del botón de VS Code: `latexmk -pdf plantilla_ieee.tex` (un solo comando).

---

## Qué tipo de citas usa esta plantilla

Estilo numérico: `\cite{clave}` en el texto se muestra como `[1]`, y la bibliografía al final queda ordenada por orden de aparición en el texto (no alfabético).

---

## Errores típicos al editar esta plantilla

- **`\email{}` y `\affil{}` deben ir DENTRO de `\authorgroup{...}`**, antes de la llave de cierre `}`. Si los sacas fuera, el formato de cursiva se "derrama" sobre todo el resto del documento. La plantilla ya los tiene bien puestos — solo cuidado si reorganizas ese bloque.
- **No escribas el símbolo `@` dentro de un comentario en el `.bib`** (ej. "tipos como @article, @book"). BibTeX no reconoce `%` como comentario real; cualquier `@` en el archivo, esté donde esté, se interpreta como el inicio de una entrada nueva y rompe la compilación. Si quieres explicar algo, hazlo en este README, no dentro del `.bib`.
- **Nombres institucionales** (INEI, PRODUCE, etc.) van con doble llave en el `.bib`: `Author = {{INEI}}`, para que BibTeX no intente separarlos en "nombre, apellido".
- El comando `\biography{...}{...}{...}` para poner fotos y biografías de autores **tiene un bug conocido en esta versión de `jaes.cls`**: no imprime el texto biográfico, solo fotos y nombres. Por eso la plantilla usa en su lugar una sección manual "Sobre el Equipo de Trabajo" al final, que sí funciona siempre.
- Si tu entrega final no debe decir **"BORRADOR"** en el encabezado de cada página, edita tu copia de `jaes.cls` y busca estas dos líneas, dejando vacío el contenido de `\rhfont`:
  ```latex
  \def\@evenhead{\hbox to \textwidth{\hfill\rhfont BORRADOR}}%
  \def\@oddhead{\hbox to \textwidth{\rhfont BORRADOR\hfill}}%
  ```