# Plantilla JAES — Estilo APA 7 (oficial, vía biblatex-apa)

## Archivos de esta carpeta

| Archivo | Qué es | ¿Lo edito? |
|---|---|---|
| `plantilla_apa7.tex` | El documento en sí. Aquí escribes tu trabajo. | Sí, es tu plantilla de trabajo. |
| `jaes.cls` | Define el formato visual (tipografía, márgenes, dos columnas, etc.). | No, salvo casos puntuales (ver abajo). |
| `jaes.bib` | Tu base de datos de referencias, en formato **biblatex** (no BibTeX clásico). | Sí, agrega tus referencias aquí. |

Esta plantilla **no necesita ningún archivo `.bst`**. A diferencia de la plantilla IEEE (que usa `jaes.bst`) o de un enfoque con `apalike.bst`, aquí el formato de la bibliografía lo controla por completo el paquete `biblatex-apa`, instalado automáticamente por MiKTeX.

## ✅ Esta plantilla fue verificada por compilación completa (actualizado)

Tras instalar `biblatex`, `biblatex-apa` y `biber` para hacer la prueba real, se encontró y corrigió un conflicto entre `jaes.cls` y `biblatex`:

> **Bug encontrado:** `jaes.cls` define `\bibparsep` y `\biblabelsep` con `\newdimen` (para su propio entorno `thebibliography` clásico). El paquete `biblatex` también declara esos mismos dos nombres con `\newlength`, sin comprobar si ya existen. Esto produce el error `! LaTeX Error: Command \bibparsep already defined` al cargar `biblatex` después de `jaes.cls`.
>
> **Solución aplicada** (ya incluida en `plantilla_apa7.tex`, no necesitas hacer nada): se libera ambos nombres con `\let\bibparsep\undefined` y `\let\biblabelsep\undefined` justo antes de `\usepackage{biblatex}`. Esto no afecta a nada más de `jaes.cls`, porque esas dos longitudes solo se usan dentro del entorno `thebibliography` clásico, que esta plantilla no utiliza (en su lugar usamos `\printbibliography` de `biblatex`).

Con este fix, la plantilla compila completa: 0 errores, 0 warnings, `biber` corre sin errores ni warnings, citas `\textcite`/`\parencite` funcionando con el formato APA 7 real (incluyendo el símbolo "&" en cita parentética con dos autores, y sentence case automático en los títulos de la bibliografía).

**Nota sobre `latexmk` y la detección automática de `biber`:** en algunos entornos, `latexmk` no detecta automáticamente que debe correr `biber` en la primera compilación y se queda con citas como "??" o el aviso *"Please (re)run Biber"*. Si te pasa esto en VS Code, simplemente compila una segunda vez (el botón ▶ de LaTeX Workshop) — `latexmk` sí detecta correctamente la necesidad de `biber` a partir de la segunda corrida, una vez que existe el archivo `.bcf` que `biblatex` genera en la primera pasada.

## ¿Por qué esta plantilla es distinta a una versión con `apalike.bst`?

`biblatex-apa` es la implementación **oficial y más completa** de APA 7 para LaTeX (automatiza casi todas las reglas del manual de estilo APA 7, incluyendo casos especiales como referencias legales). A diferencia de `apalike.bst` (una aproximación genérica autor-año que no es específica de APA), este paquete:
- Usa el motor **biblatex + biber**, no BibTeX clásico. Esto cambia la sintaxis del `.bib` (ver más abajo) y el flujo de compilación.
- Aplica automáticamente el "sentence case" de títulos que exige APA 7 (no hace falta que tú decidas qué palabras van en mayúscula).
- Conecta autores múltiples correctamente según el idioma configurado (con `babel` cargado en español, usa "y" en vez de "and").

## Diferencias del `.bib` en esta plantilla (formato biblatex, no BibTeX clásico)

```latex
% Páginas web: usa @online (no @misc), con "url" y "urldate"
@online{clave,
  author  = {{Nombre de la Institución}},
  title   = {Título},
  date    = {2025},
  url     = {https://...},
  urldate = {2026-06-29},
}
```
El campo `date` (año, o año-mes-día) reemplaza a los clásicos `year`/`month` separados, aunque `year` solo también funciona si prefieres mantenerlo simple.

## Cómo citar en el texto (biblatex, NO es lo mismo que natbib)

```latex
\textcite{clave}              % Autor (Año)            -- cita narrativa
\parencite{clave}              % (Autor, Año)            -- cita entre paréntesis
\parencite{clave1, clave2}     % (Autor1, Año; Autor2, Año)
\parencite[p.~5]{clave}        % (Autor, Año, p. 5)      -- con número de página
```
**Ojo:** si alguna vez copias código de internet que use `\citep`/`\citet`, esos son comandos de `natbib`, **no de biblatex**. En esta plantilla usa siempre `\textcite`/`\parencite`.

## Cómo compilar en VS Code + LaTeX Workshop + MiKTeX

1. Abre esta carpeta completa en VS Code (Archivo → Abrir carpeta).
2. Abre `plantilla_apa7.tex` y compílalo con el botón de LaTeX Workshop (▶) o guardando si tienes "build on save" activado.
3. **La primera vez**, MiKTeX detectará que faltan `biblatex`, `biblatex-apa`, `csquotes` y `biber`, y te mostrará una ventana preguntando si los instala. Acepta — se instalan solos.
4. La receta por defecto de LaTeX Workshop (`latexmk`) detecta sola que el documento usa `biber` en vez de `bibtex` (lee la línea `Backend: biber` que `biblatex` deja en el `.log`). No necesitas elegir nada manualmente salvo que tengas una configuración muy personalizada — en ese caso, revisa el bloque de comentarios dentro del `.tex` con el ejemplo de receta `latexmk` para `settings.json`.
5. Si compilas por terminal en vez del botón: un solo comando `latexmk -pdf plantilla_apa7.tex` (latexmk decide solo si necesita correr `biber`).

## Errores típicos al editar esta plantilla

- **`\email{}` y `\affil{}` deben ir DENTRO de `\authorgroup{...}`**, antes de la llave de cierre `}`. Si los sacas fuera, el formato de cursiva se "derrama" sobre todo el resto del documento.
- **No escribas el símbolo `@` dentro de un comentario en el `.bib`** (ej. "usa @online para páginas web"). Ni BibTeX ni biber reconocen `%` como comentario real frente a un `@`; cualquier `@` se interpreta como el inicio de una entrada nueva y rompe la compilación.
- El comando `\biography{...}{...}{...}` para fotos y biografías de autores **tiene un bug conocido en esta versión de `jaes.cls`**: no imprime el texto biográfico. La plantilla usa en su lugar una sección manual "Sobre el Equipo de Trabajo" al final.
- Si tu entrega final no debe decir **"BORRADOR"** en el encabezado, edita tu copia de `jaes.cls` (ver instrucciones dentro del `.tex`, al inicio).
- Si necesitas que el conector entre autores múltiples y otros textos fijos de la bibliografía salgan en español ("y" en vez de "and", "Recuperado de" en vez de "Retrieved from"), agrega `\usepackage[spanish]{babel}` en el preámbulo — `biblatex-apa` trae localización a varios idiomas, incluido español, pero solo se activa con `babel` cargado.

## Si necesitas volver al enfoque con `apalike.bst`

Si en algún momento esta plantilla te da problemas que no puedes resolver rápido (por ejemplo, en un entorno sin permisos para instalar paquetes), existe una alternativa más simple que **no requiere biber** y solo usa paquetes que ya vienen en cualquier instalación mínima: `natbib` + `apalike.bst`, con `\citep`/`\citet` en vez de `\parencite`/`\textcite`. No es APA 7 estricto (conecta autores con "and" en inglés, entre otras diferencias menores), pero es más simple de hacer funcionar. Pide esa variante si la necesitas.
