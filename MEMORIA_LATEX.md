# MEMORIA_LATEX_MAESTRA.md
## Sistema Operativo del Proyecto — Versión 2.0

> **Fuente de verdad** para el entorno LaTeX de la plantilla JAES adaptada para UPN.  
> Generada a partir del análisis completo de `jaes.cls`, `plantilla.tex`, `Informe.tex` y `jaes.bib`.

---

## 1. Perfil del Proyecto

| Campo | Valor |
|---|---|
| **Dominio** | Investigación Académica — Ingeniería Industrial o Ingenieria en Ciencia de Datos e IA|
| **Institución** | Universidad Privada del Norte (UPN) o Instituto San Ignacio de Loyola (El usuario debe especificar en que institución estan trabajando) |
| **Clase base** | `jaes.cls` (Journal of the Audio Engineering Society, adaptada) |
| **Estado** | Purgada: sin logos AES, sin enlaces externos AES, cabeceras personalizadas |

---

## 2. Entorno Técnico

| Componente | Detalle |
|---|---|
| **Sistema operativo** | Windows 11 |
| **Editor** | VS Code + extensión LaTeX Workshop |
| **Motor de compilación** | `latexmk` vía Strawberry Perl |
| **Distribución LaTeX** | MiKTeX (auto-instalación de paquetes activa) |
| **Línea de clase** | `\documentclass[fleqn, final]{jaes}` |

**Opciones de `\documentclass`:**
- `fleqn` → Alinea ecuaciones a la izquierda (en lugar de centrarlas).
- `final` → Desactiva `\overfullrule` (las cajas desbordadas no muestran barra negra de advertencia).

---

## 3. Paquetes Cargados Automáticamente por `jaes.cls`

La clase **ya incluye** los siguientes paquetes. **No los repitas** en el preámbulo del `.tex`:

| Paquete | Función |
|---|---|
| `mathptmx` | Fuente Times New Roman para texto y matemáticas |
| `helvet` | Fuente Helvetica para títulos y cabeceras |
| `courier` | Fuente Courier para código |
| `graphicx` | Inclusión de imágenes (`\includegraphics`) |
| `hyperref` | Links automáticos en azul (`citecolor`, `urlcolor`, `linkcolor` = azul) |
| `xurl` | Permite saltos de línea en URLs largas |
| `orcidlink` | Ícono y enlace ORCID (`\orcidlink{0000-...}`) |
| `etoolbox` | Herramientas de programación LaTeX (requerido por `orcidlink`) |
| `doi` | Formato de DOIs (`https://doi.org/...`) |
| `siunitx` | Unidades del SI (`\SI{44.1}{\kilo\hertz}`) |
| `cuted` | Entorno `strip` para texto/imagen a ancho completo en página de dos columnas |
| `footmisc` | Control avanzado de notas al pie (estilo `hang`) |

**Paquetes que SÍ debes agregar tú en el preámbulo:**

```latex
\usepackage{amsmath}\setlength{\mathindent}{10pt}  % Matemáticas avanzadas
\usepackage{bm}                                      % Letras en negritas matemáticas (\bm{x})
\usepackage[capitalise]{cleveref}                    % Referencias cruzadas inteligentes
```

---

## 4. Lógica Interna de `jaes.cls`

### 4.1 Metadatos del Documento

```latex
\jyear{2026}          % Año (aparece en el pie de página)
\jmonth{Junio}        % Mes (aparece en el pie de página)
\jname{Texto del pie} % Nombre del proyecto en el pie de página.
% Valor por defecto si no se llama: "Proyecto de Investigación".
% SÍ existe un comando público \jname{...} (jaes.cls, ~línea 1187) — no hace
% falta editar \def\@jname dentro del .cls. Llamarlo en el preámbulo, ej.:
%   \jname{Marketing Empresarial -- CIIN 1297P}
% (verificado Jun-2026: cambia correctamente el pie de todas las páginas).
```

### 4.2 Cabeceras y Pie de Página

La clase define dos estilos de página:

**`ps@headings`** (estilo normal, páginas 2 en adelante):
- Cabecera par: `leftmark` (lo que definas con `\markboth{...}{...}`)
- Cabecera impar: `rightmark`
- Pie par: `N° Página` + `Proyecto de Investigación, 2026 Junio`
- Pie impar: `Proyecto de Investigación, 2026 Junio` + `N° Página`

**`ps@plain`** (estilo portada, primera página):
- Cabecera: muestra **"BORRADOR"** en ambos lados
- Pie: igual que `headings`

> **Nota crítica:** La clase **NO tiene** la opción `final` actuando sobre cabeceras directamente. El texto "BORRADOR" está codificado en `\ps@plain`. Si no quieres esa marca, **reemplaza en `jaes.cls`**:
> ```latex
> % ANTES (líneas ~ps@plain):
> \def\@evenhead{\hbox to \textwidth{\hfill\rhfont BORRADOR}}%
> \def\@oddhead{\hbox to \textwidth{\rhfont BORRADOR\hfill}}%
> % DESPUÉS (sin marca):
> \def\@evenhead{\hbox to \textwidth{\hfill\rhfont }}%
> \def\@oddhead{\hbox to \textwidth{\rhfont \hfill}}%
> % O con @gobbletwo para omitirlas por completo:
> \let\@evenhead\@gobbletwo
> \let\@oddhead\@gobbletwo
> ```

**Comando para configurar cabeceras:**
```latex
\markboth{GRUPO 6}{DESAFÍO CÁLCULO}
%          ^^^^^       ^^^^^^^^
%     Aparece en      Aparece en
%    página par       página impar
```

### 4.3 Diseño de Página

- **Formato:** 8.0625 in × 10.875 in (tamaño JAES, ligeramente más grande que carta)
- **Columnas:** Dos columnas (`\@twocolumnfalse` pero `\twocolumn` se activa en `\maketitle`)
- **Separación entre columnas:** 18 pt
- **Ancho de texto:** 40.5 pc (6.75 in)
- **Sangría de párrafo:** 1em
- **Fuente base:** 10 pt / 12 pt de interlineado

### 4.4 Fuentes por Zona

| Zona | Familia | Tamaño |
|---|---|---|
| Título del artículo | Helvetica Bold | 20/24 pt |
| Nombre de autores | Helvetica Bold | 10/10 pt |
| Email de autores | Times Roman | 8/8 pt |
| Afiliación | Times Roman Italic | 10/12 pt |
| Abstract | Times Roman | 9/10 pt, con sangría amplia |
| Secciones | Helvetica Bold Mayúsculas | 10/12 pt |
| Subsecciones | Helvetica Bold | 10/12 pt |
| Subsubsecciones | Helvetica Bold Italic | 10/12 pt |
| Cabeceras de página | Helvetica | 7/8 pt |
| Pies de figura | Times Roman | 9/10 pt |
| Bibliografía | Times Roman | 10/12 pt |
| Notas al pie | Times Roman | 9/10 pt |

---

## 5. Reglas de Sintaxis LaTeX — Guía Rápida

### 5.1 Matemáticas

```latex
% MODO MATEMÁTICO INLINE (dentro del texto):
La variable es $t$, donde $t = 1$ corresponde a enero.

% MODO MATEMÁTICO EN DISPLAY (ecuación numerada):
\begin{equation}
  f(t) = a_n t^n + a_{n-1}t^{n-1} + \cdots + a_0
  \label{eq:mi_ecuacion}
\end{equation}

% ECUACIÓN SIN NÚMERO:
\begin{equation*}
  E = mc^2
\end{equation*}

% ECUACIÓN LARGA (con salto de línea):
\begin{equation}
\begin{split}
  f_5(t) = &0.003007t^5 - 0.09879t^4 + 1.1836t^3 \\
           &- 6.0647t^2 + 11.4546t - 1.9227
\end{split}
\end{equation}

% SISTEMA DE ECUACIONES ALINEADAS:
\begin{align}
  f'(t) &= 5 \cdot 0.003007 t^4 - \cdots \\
  f''(t) &= 20 \cdot 0.003007 t^3 - \cdots
\end{align}

% FRACCIONES:
$\frac{numerador}{denominador}$

% SUBÍNDICES Y SUPERÍNDICES:
$x_1$    $x^2$    $x_1^2$    $R^2$

% NEGRITAS EN MATEMÁTICAS (requiere \usepackage{bm}):
$\bm{v}$   % vector en negrita
```

### 5.2 Caracteres Especiales

| Quiero escribir | Código correcto | ¿Por qué? |
|---|---|---|
| `50%` | `$50\%$` o `50\%` | `%` sin escape inicia un comentario |
| `& (ampersand)` | `\&` | `&` es separador de columnas en tablas |
| `$` (dólar) | `\$` | `$` abre modo matemático |
| `_` (guion bajo) | `\_` | `_` es subíndice en modo matemático |
| `^` (acento circunflejo) | `\^{}` | `^` es superíndice en modo matemático |
| `#` (numeral) | `\#` | `#` es parámetro en macros |
| `~` (tilde) | `\~{}` | `~` es espacio no separable |
| `{` o `}` | `\{` o `\}` | Son delimitadores de grupos |
| Espacio no separable | `~` | Ej: `44.1~kHz` o `Fig.~\ref{...}` |
| Guion largo (—) | `---` | |
| Guion medio (–) | `--` | Para rangos: `pp. 910--925` |
| Comillas inglesas | `` ``texto'' `` | Abrir con dos backticks, cerrar con dos apóstrofes |

### 5.3 Texto con Formato

```latex
\textbf{negrita}         % Negrita
\textit{cursiva}         % Cursiva / itálica
\emph{énfasis}           % Énfasis (cursiva en texto normal)
\textsc{Versalitas}      % VERSALITAS
\underline{subrayado}    % Subrayado (evitar en documentos académicos)
\texttt{código}          % Fuente monoespaciada (tipo código)
\textsuperscript{1}      % Superíndice en texto (no matemáticas)
```

---

## 6. Estructura de Comandos de la Plantilla

### 6.1 Bloque de Autores

```latex
\authorgroup{
  \author{NOMBRE COMPLETO\textsuperscript{1}\orcidlink{0009-XXXX-XXXX-XXXX}},
  \author{COLABORADOR 1,\textsuperscript{1}},
  \author{COLABORADOR 2\textsuperscript{1}}
  % Ajustar los \quad para alinear los códigos bajo cada nombre.
  % Cada \quad ≈ 1em de espacio horizontal.
  \email{(N00XXXXXX) \quad\quad\quad\quad\quad\quad (N00XXXXXX) \quad\quad\quad\quad\quad\quad (N00XXXXXX)}
  \affil{\textsuperscript{1}Universidad Privada del Norte, Facultad de Ingeniería Industrial, Lima, Perú}
}
```

> **Advertencia con comas:** La coma después del nombre del autor (`\author{NOMBRE,}`) va DENTRO del `\author{}` para que aparezca entre autores. El último autor NO lleva coma.

### 6.2 Abstract

```latex
\abstract{%
  Texto del resumen (100–200 palabras). Sin citas. Una sola oración
  sobre: motivación, problema, enfoque, resultados e implicaciones.
}
\maketitle   % <- OBLIGATORIO después del abstract. Genera el encabezado.
```

### 6.3 Palabras Clave

```latex
\begin{keywords}
Palabra 1, Palabra 2, Palabra 3, Palabra 4
\end{keywords}
```

---

## 7. Tablas

### 7.1 Estructura Básica (con bordes de la clase JAES)

```latex
\begin{table}[ht]
\centering
\caption{Descripción de la tabla.}\label{tab:mi_tabla}
\begin{tabular}{|c|c|c|}
\hline
\textbf{Columna 1} & \textbf{Columna 2} & \textbf{Columna 3} \\ \hline
Dato 1  & Dato 2  & Dato 3 \\ \hline
Dato A  & Dato B  & Dato C \\ \hline
\end{tabular}
\end{table}
```

### 7.2 Tabla Estilo JAES (con \toprule / \colrule / \botrule)

```latex
\begin{table}[!ht]
\centering
\caption{Descripción.}\label{tab:estilo_jaes}
\begin{tabular}{@{}lcc@{}}     % @{} elimina espacio en bordes; l=izq, c=centro
\toprule                        % Doble línea superior
Encabezado 1 & Encabezado 2 & Encabezado 3 \\
\colrule                        % Línea separadora de encabezados
Fila 1 & Valor & Valor \\
Fila 2 & \textbf{Mejor} & Valor \\
\botrule                        % Línea inferior
\end{tabular}
\end{table}
```

> **Reglas de las líneas:**
> - `\toprule` → doble línea arriba (inicio de tabla)
> - `\colrule` → línea simple (separa encabezados del cuerpo)
> - `\botrule` → línea abajo (fin de tabla)
> - `\Hline` → línea delgada de 0.25pt (para subdivisiones internas)

### 7.3 Nota al Pie de Tabla

```latex
\begin{table}[!ht]
  ...
  \begin{tabnote}
  Nota. Aquí va una aclaración sobre la tabla o sus acrónimos.
  \end{tabnote}
\end{table}
```

### 7.4 Tabla de Ancho Doble Columna

```latex
\begin{table*}[ht]   % El * hace que ocupe ambas columnas
  \centering
  \caption{Tabla ancha.}
  \begin{tabular}{...}
  ...
  \end{tabular}
\end{table*}
```

### 7.5 Posicionamiento de Flotantes

| Especificador | Significado |
|---|---|
| `[ht]` | Aquí (here) o al tope de la página |
| `[!ht]` | Forzar aquí o al tope (ignora restricciones) |
| `[tb]` | Solo al tope o al fondo de la página |
| `[p]` | Página dedicada a flotantes |
| `[H]` | Exactamente aquí (requiere paquete `float`) |

---

## 8. Figuras

### 8.1 Figura de una Columna (82 mm)

```latex
\begin{figure}[t]
\centering
\includegraphics[width=82mm]{nombre_archivo.png}
\caption{Descripción de la figura.}
\label{fig:mi_figura}
\end{figure}
```

### 8.2 Figura de Dos Columnas (170 mm)

```latex
\begin{figure*}
\centering
\includegraphics[width=0.7\textwidth]{nombre_archivo.png}
\caption{Descripción de la figura ancha.}
\label{fig:figura_ancha}
\end{figure*}
```

> **Formatos aceptados:** PNG (recomendado), EPS, PDF. **No usar TIFF**.  
> Los archivos de imagen deben estar en la **misma carpeta** que el `.tex`.

---

## 9. Secciones

```latex
\section{Título de Sección}          % Numerada: 1. TÍTULO (automáticamente MAYÚSCULAS)
\subsection{Título de Subsección}    % Numerada: 1.1 Título
\subsubsection{Título}               % Numerada: 1.1.1 Título (negrita itálica)
\section*{Sección Sin Número}        % Sin numeración (no aparece en índice)
```

> **Nota de comportamiento:** La clase convierte automáticamente el texto de `\section` a MAYÚSCULAS en el PDF. Los niveles `\subsection` y `\subsubsection` se muestran tal como los escribes.

---

## 10. Referencias Cruzadas con `cleveref`

### 10.1 Configuración Fija (ya incluida en la plantilla)

```latex
\crefformat{section}{\textsc{Sec.~#2#1#3}}
\Crefformat{section}{\textsc{Sec.~#2#1#3}}
\crefformat{figure}{{Fig.~#2#1#3}}
\Crefformat{figure}{{Fig.~#2#1#3}}
\crefformat{table}{{Tabla~#2#1#3}}
\Crefformat{table}{{Tabla~#2#1#3}}
```

### 10.2 Uso

```latex
% Para referencias al inicio de oración (usa \Cref):
\Cref{tab:datos_empiricos} muestra los valores...

% Para referencias en medio de oración (usa \cref):
...como se observa en \cref{tab:datos_empiricos}.

% Múltiples referencias:
...como en \cref{eq:modelo1,eq:modelo2}.

% Referencia a ecuación (también funciona el clásico):
...en la \Cref{eq:modelo_final}...
% o el comando estándar:
...en la Ec.~(\ref{eq:modelo_final})...
```

### 10.3 Etiquetas — Convención de Nombres

| Tipo | Prefijo | Ejemplo |
|---|---|---|
| Sección | `sec:` | `\label{sec:introduccion}` |
| Subsección | `sub:` | `\label{sub:datos_empiricos}` |
| Ecuación | `eq:` | `\label{eq:modelo_final}` |
| Tabla | `tab:` | `\label{tab:comparacion}` |
| Figura | `fig:` | `\label{fig:grafica}` |

---

## 11. Bibliografía

### 11.1 Sistema BibTeX

```latex
% Al final del documento (antes de \appendix si hay apéndice):
\bibliography{jaes}          % Nombre del archivo .bib SIN extensión
\bibliographystyle{ieeetr}   % Estilo numérico IEEE (orden de aparición)
```

### 11.2 Citar en el Texto

```latex
\cite{inei_informe_mensual}                           % Una cita: [1]
\cite{inei_boletin_03_2025,inei_boletin_04_2025}      % Varias: [2,3]
```

### 11.3 Tipos de Entrada en el `.bib`

```bibtex
% ARTÍCULO DE REVISTA:
@article{clave,
  Author  = {Apellido, N. and Apellido2, N.},
  Title   = {Título del Artículo con Mayúsculas en Palabras Clave},
  Journal = {J. Audio Eng. Soc.},
  Volume  = {68},  Number = {11},
  Pages   = {910--925},
  Year    = {2020},  Month = {Dec.},
  doi     = {10.17743/jaes.2020.0060}
}

% LIBRO:
@book{clave,
  author    = {Apellido, N. and Apellido2, N.},
  title     = {Título del Libro},
  publisher = {Editorial},
  address   = {Ciudad, País},
  year      = {2020}
}

% RECURSO WEB / INFORME EN LÍNEA:
@misc{clave,
  author       = {{INEI}},         % Llaves dobles para nombres institucionales
  title        = {Título del Informe},
  howpublished = {\url{https://...}},
  year         = {accessed 15 Jun. 2026}
}

% ACTAS DE CONFERENCIA:
@inproceedings{clave,
  author    = {Apellido, N.},
  title     = {Título},
  booktitle = {Proc. Nombre Conferencia (SIGLA)},
  pages     = {1--10},
  address   = {Ciudad, País},
  year      = {2025},  month = {Nov.},
  doi       = {10.XXXX/...}
}

% TESIS DE DOCTORADO:
@phdthesis{clave,
  author = {Apellido, N.},
  title  = {Título de la Tesis},
  school = {Universidad, Ciudad, País},
  year   = {2023},  month = {Mar.}
}
```

> **Reglas del `.bib`:**
> - Más de 5 autores: listar los 3 primeros y agregar `and others` → aparece como "et al."
> - El DOI es **obligatorio** para publicaciones que lo tengan.
> - Nombres institucionales con dobles llaves: `{{INEI}}` evita que BibTeX abrevíe el nombre.

### 11.4 Alternativa: Citación APA 7 (verificado Jun-2026)

> El estilo nativo de `jaes.cls`/IEEE es numérico (`ieeetr`). Si el trabajo exige **APA 7** (Autor, Año), `jaes.cls` no impone ningún estilo de cita específico —la elección vive enteramente en `\bibliographystyle`— así que es perfectamente compatible cambiarlo, siempre que se controle bien `natbib`.
>
> **Limitación real del entorno:** `biblatex` + `biblatex-apa` (la opción "oficial" más fiel a APA 7) puede no estar instalado en todos los entornos de compilación, y no es descargable si no hay acceso a CTAN. La alternativa verificada que **sí viene de fábrica en cualquier TeX Live estándar** es `natbib` + `apalike.bst`. No es 100% idéntico a APA 7 en cada detalle tipográfico, pero reproduce correctamente lo esencial: citas (Autor, Año), bibliografía alfabetizada con sangría francesa, y sufijos automáticos "a"/"b" para mismo autor+año.

```latex
% En el preámbulo:
\usepackage[round,authoryear]{natbib}
% ...
% Al final del documento:
\bibliography{jaes}
\bibliographystyle{apalike}   % NO ieeetr
```

```latex
% Citar en el texto (sintaxis natbib, no \cite simple):
\citep{inei2024}                  % (INEI, 2024)            -- cita parentética
\citet{inei2024}                  % INEI (2024)              -- cita narrativa
\citep{inei2024, produce2024}     % (INEI, 2024; PRODUCE, 2024)
```

> **Orden de compilación crítico:** cambiar de `ieeetr`/`\cite` a `apalike`/`\citep` a mitad de proyecto deja basura del estilo anterior. Si aparece `! Package natbib Error: Bibliography not compatible with author-year citations`, casi siempre es porque el `.bbl`/`.aux` de una compilación previa con `ieeetr` no se borró. Solución: `rm *.bbl *.aux *.blg` y recompilar desde cero (`pdflatex → bibtex → pdflatex → pdflatex`).
>
> **Autores institucionales y citas cortas:** con `{{Instituto Nacional de Estadística e Informática [INEI]}}` como `Author`, la cita sale completa cada vez: "(Instituto Nacional de Estadística e Informática [INEI], 2024)" — correcto para primera mención en APA 7, pero demasiado largo si se repite. `apalike.bst` no soporta automáticamente "completo la primera vez, sigla después". Solución práctica: usar solo la sigla en el campo `Author` (`{{INEI}}`) y escribir el nombre completo una vez en la prosa del propio texto, no en el `.bib`.
>
> **Nombres de medios con artículo ("La República", "El Comercio"):** sin doble llave, BibTeX interpreta "La" como nombre de pila y "República" como apellido, y en la cita corta solo imprime "República". Solución: `Author = {{La República}}` (doble llave) para que se trate como una unidad literal.
>
> **Mismo autor + mismo año en dos fuentes distintas** (ej. dos artículos de "La República" de 2025): `apalike.bst` SÍ resuelve esto automáticamente con sufijos "a"/"b" (`República, 2025a` / `República, 2025b`), sin configuración adicional — simplemente hay que citar la clave correcta de cada entrada y confiar en que el sufijo se calcula solo, en orden alfabético de label interno.

---

## 12. Elementos Especiales de la Clase

### 12.1 Texto a Ancho Completo (entorno `strip`)

> **⚠️ Corrección verificada (Jun-2026):** `\captionof` **NO está disponible** en este entorno — la clase no carga `caption` ni `capt-of`, solo `cuted`. Usar `\captionof{figure}{...}` dentro de `strip` produce `! Undefined control sequence` y rompe la compilación. Esto se confirmó compilando el documento completo con `pdflatex`.

`strip` es para contenido **no flotante** (texto corrido, bloques sin numerar). La propia clase lo usa así internamente en `\printbiography` (fotos de autores sin caption numerado). Ejemplo correcto de uso de `strip`:

```latex
\begin{strip}
\centering
\includegraphics[width=\textwidth]{mi_imagen.png}

\textit{Pie de imagen sin numerar, escrito como texto plano.}
\end{strip}
```

**Para figuras o tablas que SÍ necesitan numeración, `\caption` y `\label` con referencias cruzadas (`\cref`), usar `figure*`/`table*` en vez de `strip`** — ver §16, que es el método verificado para apéndices con paneles fotográficos y tablas anchas.

### 12.2 Apéndices

> **⚠️ Corrección verificada (Jun-2026):** En `jaes.cls`, el comando `\appendix` ejecuta internamente `\let\section\subsection`. Esto significa que **después de `\appendix`, `\section{...}` ya NO genera un título de nivel 1** — se comporta como `\subsection`. Usar `\section{\MakeUppercase{...}}` tras `\appendix` produce un título con el formato de subsección, no de sección. Lo correcto es escribir directamente `\subsection{...}` para los títulos de cada apéndice:

```latex
\appendix

\subsection{Apéndice A: Título del Apéndice A}
\label{sec:apendiceA}
Contenido...

\subsection{Apéndice B: Título del Apéndice B}
\label{sec:apendiceB}
Contenido...
```

`\appendix` también ejecuta `\renewcommand\thesection{\Alph{section}}` y `\setcounter{section}{1}`, por lo que la numeración de apéndices pasa a letras (A, B, C…) de forma automática; no es necesario gestionarlo manualmente.

> **Numeración de tablas y ecuaciones:** continúan la secuencia numérica del cuerpo del documento (ej. si el cuerpo terminó en Tabla 10, el Apéndice A empieza en Tabla 11). No reinician desde 1.
>
> **Numeración de figuras con prefijo de letra (ej. "Fig. C.1, C.2…"):** sí es posible, pero requiere un manejo especial — ver §16.3 para el método verificado que evita conflictos con los enlaces internos de `hyperref`.

### 12.3 Biografías de Autores

> **🐛 BUG CONFIRMADO en `jaes.cls` (verificado Jun-2026):** `\biography{Nombre}{foto}{Texto}` **no imprime el tercer argumento** (el texto de la biografía). Se reprodujo en un documento mínimo, usando *solo* la plantilla original sin contenido propio (2 autores, biografías con texto corto y distintivo) y el resultado es el mismo: `\printbiography` imprime la caja "THE AUTHOR(S)" con las fotos/nombres, pero el párrafo biográfico de cada autor **nunca aparece** en el PDF (confirmado revisando el texto extraído del PDF con `pdftotext`, no solo visualmente).
>
> **Causa probable:** en `\printbiography` (jaes.cls), el primer bucle (impresión de fotos/nombres) decrementa `\biocount` hasta 0 dentro de un `\begin{strip}...\end{strip}`; el segundo bucle (impresión del texto de afiliación/biografía, que usa `\authorcount` y `\csname bioaff\romannumeral\affcount\endcsname`) corre **fuera** de ese `strip`, directamente en el flujo de dos columnas — el texto no se pierde por un error de sintaxis, pero no se renderiza visiblemente en la página (no genera error de compilación, simplemente no aparece). No se identificó una causa raíz más profunda sin instrumentar la clase; tampoco se identificó una corrección que preserve el comportamiento "automático" sin modificar `jaes.cls`.
>
> **Solución verificada que SÍ funciona — documentar el equipo manualmente, sin usar `\biography`:**
> ```latex
> % En el preámbulo, antes de \begin{document}:
> \def\printbiography{}   % anula la impresión automática (\AtEndDocument llama
>                          % a \printbiography sin condición salvo \ifauthorgd;
>                          % sin este \def queda una caja "THE AUTHORS" vacía
>                          % al final del documento aunque no se llame a \biography)
> ```
> ```latex
> % Cerca del final del documento, en vez de \biography{...}{...}{...}:
> \clearpage
> \section*{Sobre el Equipo de Trabajo}
> \begin{description}
> \item[Nombre Autor (código)] Rol o aporte específico al trabajo.
> \item[Nombre Autor 2 (código)] Rol o aporte específico al trabajo.
> \end{description}
> ```
>
> **Importante sobre `\authorgdtrue`:** *no* usar este flag para intentar suprimir `\printbiography` — `\ifauthorgd` también controla `\maketitle` (línea ~285 de `jaes.cls`: `\ifauthorgd\@maketitle\else\twocolumn[\@maketitle]\fi`), así que activarlo rompe el formato a una columna de la portada. Usar siempre `\def\printbiography{}` en su lugar, que solo anula la macro problemática sin tocar nada más.
>
> Si en algún momento se corrige `jaes.cls` o se usa una versión distinta del archivo, volver a probar `\biography` con un documento mínimo (2 autores, texto corto y distintivo en cada biografía) y verificar con `pdftotext archivo.pdf - | grep "texto distintivo"` antes de confiar en que el texto se imprime.

```latex
% Sintaxis original de la clase (NO usar sin verificar primero — ver bug arriba):
\clearpage   % Necesario para evitar conflictos de formato

\biography{Nombre Autor}{foto.jpg}{Texto de la biografía (max 150 palabras).
  Incluir: formación, trabajo actual, áreas de investigación.}

\biography{Nombre Autor 2}{foto2.jpg}{Texto...}
```

> Si no hay foto disponible, LaTeX mostrará un recuadro **negro sólido** (`\vrule width8pc height10pc`, es una regla rellena, no un marco vacío) de 8pc × 10pc — no un recuadro vacío como podría esperarse. Para evitar el error `! LaTeX Error: File \`'\ not found` hay que pasar un nombre de archivo no vacío en el segundo argumento (ej. `{sin_foto.png}`) aunque el archivo no exista — `\IfFileExists` con cadena vacía falla, pero con un nombre de archivo inexistente simplemente activa la rama "no existe" sin romper la compilación.
> La macro `\printbiography` se llama **automáticamente** al cerrar el documento (`\AtEndDocument{\ifauthorgd\else\printbiography\fi}`), incluso si nunca se llamó a `\biography` (deja una caja "THE AUTHORS" vacía con líneas horizontales) — ver bug arriba para cómo anularla.

### 12.4 Listas

```latex
% Lista con viñetas:
\begin{itemize}
  \item Primer elemento
  \item Segundo elemento
\end{itemize}

% Lista numerada:
\begin{enumerate}
  \item Primer paso
  \item Segundo paso
\end{enumerate}

% Lista con guiones:
\begin{hyphenlist}
  \item Elemento con guion
\end{hyphenlist}
```

---

## 13. Plantilla Boilerplate — Inicio Rápido

Copia este bloque para comenzar cualquier nuevo informe:

```latex
\documentclass[fleqn, final]{jaes}

% ── Metadatos ──────────────────────────────────────────────────────────
\jyear{2026}
\jmonth{Junio}
\markboth{CURSO / GRUPO}{TÍTULO CORTO DEL TRABAJO}

% ── Paquetes adicionales (la clase ya carga: graphicx, hyperref, siunitx,
%    orcidlink, doi, cuted, footmisc) ────────────────────────────────────
\usepackage{amsmath}\setlength{\mathindent}{10pt}
\usepackage{bm}
\usepackage[capitalise]{cleveref}
  \crefformat{section}{\textsc{Sec.~#2#1#3}}
  \Crefformat{section}{\textsc{Sec.~#2#1#3}}
  \crefformat{figure}{{Fig.~#2#1#3}}
  \Crefformat{figure}{{Fig.~#2#1#3}}
  \crefformat{table}{{Tabla~#2#1#3}}
  \Crefformat{table}{{Tabla~#2#1#3}}

\begin{document}

% ── Título ─────────────────────────────────────────────────────────────
\title{Título Completo del Informe}

% ── Autores ────────────────────────────────────────────────────────────
\authorgroup{
  \author{JHAROL VILCA\textsuperscript{1}\orcidlink{0009-0009-7897-3439}},
  \author{COLABORADOR 1,\textsuperscript{1}},
  \author{COLABORADOR 2\textsuperscript{1}}
  \email{(N00481358) \quad\quad\quad\quad\quad\quad (Cód1) \quad\quad\quad\quad\quad\quad (Cód2)}
  \affil{\textsuperscript{1}Universidad Privada del Norte, Facultad de Ingeniería Industrial, Lima, Perú}
}

% ── Abstract ───────────────────────────────────────────────────────────
\abstract{%
Resumen técnico de 100–200 palabras: motivación, problema, enfoque,
resultados e implicaciones. Sin citas. Un solo párrafo.
}
\maketitle

% ── Palabras clave ─────────────────────────────────────────────────────
\begin{keywords}
Palabra 1, Palabra 2, Palabra 3
\end{keywords}

% ── Cuerpo del documento ───────────────────────────────────────────────
\section{Introducción}\label{sec:introduccion}

\section{Metodología}\label{sec:metodologia}

\section{Resultados}\label{sec:resultados}

\section{Conclusiones}\label{sec:conclusiones}

% ── Bibliografía ───────────────────────────────────────────────────────
\bibliography{jaes}
\bibliographystyle{ieeetr}

% ── Apéndice (opcional) ────────────────────────────────────────────────
% \appendix
% \section{\MakeUppercase{Título del Apéndice}}

\end{document}
```

---

## 14. Problemas Comunes y Soluciones

| Síntoma | Causa probable | Solución |
|---|---|---|
| `! Undefined control sequence \orcidlink` | Falta el paquete (raro, la clase lo carga) | Verificar que `jaes.cls` está en la carpeta |
| El texto después de `%` desaparece | `%` sin escapar inicia comentario | Usar `\%` para el símbolo de porcentaje |
| Ecuación con error de compilación | `$` no cerrado o caracteres especiales dentro | Revisar pares de `$...$` y usar `\_`, `\^{}`, etc. |
| "BORRADOR" en la primera página | Normal: es el `\ps@plain` de la portada | Para quitarlo, editar `ps@plain` en `jaes.cls` |
| Tabla se desborda de la columna | Columnas demasiado anchas | Usar `\tiny` o `\small` dentro de `tabular`, o cambiar a `table*` |
| Error `\cite` vacío | La clave del `.bib` no coincide | Verificar que la clave en `\cite{}` es exactamente igual a la del `.bib` |
| Figura no aparece | Archivo no encontrado | Verificar que la imagen está en la misma carpeta y sin espacios en el nombre |
| Referencias no aparecen | No se compiló BibTeX | Secuencia de compilación: `pdflatex → bibtex → pdflatex → pdflatex` |
| Números de sección incorrectos | Compilación incompleta | Compilar dos veces con `latexmk` |
| `! Undefined control sequence \captionof` | `caption`/`capt-of` no están cargados; la clase solo trae `cuted` | Usar `figure*`/`table*` con `\caption` normal en vez de `strip`+`\captionof` (ver §16.1) |
| `! Undefined control sequence \midrule` o `\bottomrule` | La clase no usa `booktabs` | Usar `\colrule` y `\botrule` (sintaxis propia de `jaes.cls`, ver §16.2) |
| `pdfTeX warning: destination ... duplicate ignored` | Dos figuras/tablas comparten el mismo ancla de `hyperref` (ej. al resetear `\setcounter{figure}{0}` en un apéndice) | Redefinir también `\theHfigure` con un prefijo único antes de resetear el contador visible (ver §16.3) |
| El texto de `\biography{...}{...}{TEXTO}` no aparece en el PDF (solo se ven fotos/nombres en "THE AUTHORS") | **Bug de `jaes.cls`**: el bucle que imprime el texto biográfico no renderiza visiblemente (ver §12.3) | No usar `\biography`; documentar el equipo manualmente con `\section*{Sobre el Equipo de Trabajo}` + `description`, y anular la impresión automática con `\def\printbiography{}` |
| `! LaTeX Error: File \`'\ not found` al compilar con `\biography` | Segundo argumento de `\biography` vacío (`{}`) — `\IfFileExists` falla con cadena vacía | Pasar un nombre de archivo no vacío aunque no exista, ej. `{sin_foto.png}` (ver §12.3) |

---

## 15. Secuencia de Compilación Correcta

Con `latexmk` (configurado en VS Code + LaTeX Workshop), la compilación es automática. Si hay problemas manuales:

```bash
# Secuencia completa manual:
pdflatex  Informe.tex
bibtex    Informe.aux
pdflatex  Informe.tex
pdflatex  Informe.tex
```

> Con LaTeX Workshop en VS Code, el atajo `Ctrl+Alt+B` lanza `latexmk` que hace todo esto automáticamente.

---

## 16. Caso Verificado: Apéndices con Tablas, Fichas Técnicas y Panel Fotográfico

> Esta sección documenta un caso real, compilado y verificado de extremo a extremo (`pdflatex → bibtex → pdflatex → pdflatex`, 21 páginas, **0 errores, 0 warnings críticos**) para resolver el problema de "extraalineamiento" al insertar apéndices con tablas grandes e imágenes a ancho completo en `Informe.tex`.

### 16.1 Diagnóstico: por qué `strip` causaba errores

El planteamiento inicial fue usar `\begin{strip}...\end{strip}` con `\captionof{figure}{...}` para cada apéndice, asumiendo que era la única vía para lograr ancho completo en una clase de dos columnas. Al revisar `jaes.cls` a detalle se confirmó:

- `\captionof` **no existe** en este entorno (no se carga `caption` ni `capt-of`) → error de compilación.
- `jaes.cls` **ya define `figure*` y `table*` de forma nativa** (líneas ~1034 y ~1061 de `jaes.cls`), usando el mecanismo estándar de LaTeX `\@dblfloat` para flotantes a doble columna. Es la herramienta correcta y ya integrada — no hay que "forzar" nada con `strip`.
- `strip` queda reservado para contenido **no flotante** (la propia clase lo usa así en `\printbiography`, para las fotos de autores sin numerar).

**Regla práctica:** para apéndices con tablas/figuras que necesitan `\caption`, `\label` y `\cref`, usar `figure*` / `table*`. Reservar `strip` solo para bloques de texto o imágenes sueltas sin numeración.

### 16.2 Diagnóstico: la clase NO usa `booktabs`

Un segundo error detectado al compilar: usar `\midrule` y `\bottomrule` (sintaxis estándar de `booktabs`) genera `! Undefined control sequence`, porque **`jaes.cls` no carga `booktabs`**. La clase define su **propio sistema de líneas de tabla**:

| Comando estándar (`booktabs`, NO disponible) | Comando correcto en `jaes.cls` |
|---|---|
| `\toprule` | `\toprule` (sí existe, pero es una macro propia de la clase) |
| `\midrule` | `\colrule` |
| `\bottomrule` | `\botrule` |

```latex
\begin{table*}[!ht]
\centering
\small
\caption{Título de la tabla.}\label{tab:mi_tabla_ancha}
\begin{tabular}{@{}p{2cm}p{4cm}p{10cm}@{}}  % p{} para columnas anchas con salto de línea
\toprule
\textbf{Col 1} & \textbf{Col 2} & \textbf{Col 3} \\
\colrule
Dato & Dato & Texto largo que necesita envolver en varias líneas dentro de la celda \\
\botrule
\end{tabular}
\end{table*}
```

> Para tablas anchas con texto largo en celdas, usar columnas `p{ancho}` (en vez de `l`/`c`/`r`) para que el texto haga salto de línea automático dentro de la celda.

### 16.3 Numeración de figuras con prefijo de letra (Fig. C.1, C.2…) sin romper `hyperref`

Para un panel fotográfico dentro de "Apéndice C" donde se pide que las figuras se numeren como `C.1`, `C.2`, etc. (en vez de continuar la secuencia global 7, 8, 9…), **no basta con redefinir `\thefigure`**. Hyperref usa un comando aparte, `\theHfigure`, para generar el ancla interna (`figure.0.N`) de cada figura — si solo se reinicia `\setcounter{figure}{0}` sin tocar `\theHfigure`, los nuevos anclajes **colisionan** con los de las figuras ya existentes en el cuerpo (warning `pdfTeX: destination with the same identifier has been already used, duplicate ignored`).

**Solución verificada** (sin warnings, sin colisión de anclajes):

```latex
% Antes del bloque de figuras del panel:
\renewcommand{\thefigure}{C.\arabic{figure}}      % Numeración visible: C.1, C.2...
\renewcommand{\theHfigure}{apc.\arabic{figure}}   % Ancla interna única para hyperref
\setcounter{figure}{0}                             % Reinicia el contador visible

\begin{figure*}[!ht]
\centering
\includegraphics[width=\textwidth]{imagen.pdf}
\caption{Descripción.}
\label{fig:apc_nombre_descriptivo}
\end{figure*}

% ... más figuras ...

% Después del bloque, restaurar el comportamiento normal:
\renewcommand{\thefigure}{\@arabic\c@figure}
\renewcommand{\theHfigure}{\theHchapter.\arabic{figure}}
```

> **Importante sobre `\label`:** usar un prefijo distinto para las etiquetas de este bloque (ej. `fig:apc_*`) si las mismas imágenes ya tienen un `\label` en el cuerpo del documento. Repetir el mismo `\label` con `\caption` en dos lugares distintos sobrescribe la referencia y `\cref` apuntará solo al último uso.

### 16.4 Reutilizar imágenes ya incluidas en el cuerpo (Apéndice = "Panel fotográfico" consolidado)

Si las imágenes del Apéndice C ya se usaron antes en el cuerpo del informe (por ejemplo, mapas de macro/microlocalización citados en la metodología), es válido y común volver a incluirlas en un apéndice como panel fotográfico consolidado de cierre. Usar:
- El mismo archivo (`\includegraphics[width=\textwidth]{Mismo Archivo.pdf}`).
- Un `\label` **distinto** al usado en el cuerpo (ver §16.3).
- Un texto de `\caption` adaptado al propósito del panel (más breve, enfocado en la identificación de la imagen, no en el análisis ya hecho en el cuerpo).

### 16.5 Apéndice vacío reservado para contenido futuro

Para dejar un apéndice listo pero sin contenido (a la espera de que el usuario agregue sus propias imágenes), usar un comentario con la plantilla exacta a copiar, en vez de dejar la subsección completamente vacía:

```latex
\subsection{Apéndice D: Título del Apéndice (pendiente)}
\label{sec:apendiceD}

% ------------------------------------------------------------------
% ESPACIO RESERVADO. Plantilla a copiar por cada imagen nueva:
%
% \begin{figure*}[!ht]
% \centering
% \includegraphics[width=\textwidth]{NombreDeSuImagen.pdf}
% \caption{Descripción de la fotografía.}
% \label{fig:apd_01}
% \end{figure*}
% ------------------------------------------------------------------
```

### 16.6 Verificación de claves bibliográficas antes de citar

Al redactar contenido nuevo (ej. una bitácora con referencias a normas técnicas), **verificar que la clave exista exactamente así en el `.bib`** antes de usar `\cite{}` — un error común es asumir un año o sufijo incorrecto en la clave (ej. `INACAL2017` en vez de `INACAL2021` para la NTP 339.090). Si `bibtex` no reporta "Warning--I didn't find a database entry", la clave es válida; si la reporta, revisar `jaes.bib` con `grep "^@" jaes.bib` para listar todas las claves disponibles.

### 16.7 Checklist de verificación antes de entregar apéndices nuevos

1. ¿Se usó `figure*`/`table*` en vez de `strip` para contenido con `\caption`/`\label`?
2. ¿Se usó `\toprule`/`\colrule`/`\botrule` (no `booktabs`) en las tablas?
3. ¿Los `\label` nuevos son distintos a los ya existentes en el cuerpo (`grep -oP '\\label\{[^}]+\}' Informe.tex | sort | uniq -d`)?
4. ¿Las claves de `\cite{}` nuevas existen en `jaes.bib`?
5. ¿Se compiló el ciclo completo (`pdflatex → bibtex → pdflatex → pdflatex`) revisando el `.log` final por `Undefined control sequence`, `Citation ... undefined` y `duplicate` (anclajes de hyperref)?

---

## 17. Localización al Español de `jaes.cls` (verificado Jun-2026)

`jaes.cls` está escrito para producir documentos en inglés (hereda el estilo de la revista *Journal of the Audio Engineering Society*). Al usarlo para un informe en español aparecen varias palabras en inglés hardcodeadas que **sí se pueden corregir desde el `.tex`**, sin tocar el `.cls`, con `\renewcommand`:

```latex
% En el preámbulo, después de cargar la clase:
\renewcommand{\tablename}{Tabla}     % por defecto "Table" (jaes.cls la define en inglés)
\renewcommand{\refname}{Referencias} % por defecto "References" (viene de LaTeX/el .bst, no del .cls)
```

> `\figurename` ya viene como "Fig." en el `.cls`, que es una abreviatura válida también en español, así que normalmente no necesita cambiarse.

### 17.1 `cleveref` y la conjunción "and" en listas de referencias

Al citar dos o más secciones/tablas juntas con `\cref{sec:a,sec:b}`, `cleveref` imprime por defecto "Sec. 2 **and** 5" en inglés. El paquete sí tiene soporte multilenguaje incorporado para español (busca `\null` definiciones bajo `\@ifpackageloaded{babel}` con la opción `spanish`), pero esa rama de código **solo se activa si `babel` está cargado con un idioma**. Si el documento no usa `babel` (ver §3 sobre por qué puede no estar disponible en todos los entornos), hay que definir los comandos de conjunción a mano, y usar `\providecommand` (no `\renewcommand`, que falla con "Command undefined" porque el comando no existe sin `babel`):

```latex
% En el preámbulo, después de cargar cleveref:
\providecommand{\creflastconjunction}{}
\renewcommand{\creflastconjunction}{ y }   % conjunción para el ÚLTIMO elemento de 3+ referencias
\providecommand{\crefpairconjunction}{}
\renewcommand{\crefpairconjunction}{ y }   % conjunción para EXACTAMENTE 2 referencias (caso más común)
\providecommand{\crefmiddleconjunction}{}
\renewcommand{\crefmiddleconjunction}{, }  % separador entre elementos intermedios (3+ referencias)
```

> **Importante:** `\crefpairconjunction` (para 2 referencias, ej. `\cref{sec:a,sec:b}`) y `\creflastconjunction` (para 3+, ej. `\cref{sec:a,sec:b,sec:c}`) son comandos **distintos**; corregir solo uno deja el otro caso en inglés. Verificar ambos casos en el PDF final con `pdftotext informe.pdf - | grep -i " and "` antes de entregar.

### 17.2 Watermark "BORRADOR" en el encabezado de cada página

`jaes.cls` imprime la palabra "BORRADOR" en el encabezado de **todas** las páginas de forma incondicional (no es un modo opcional activable/desactivable con una opción de clase, está fijo en el código del estilo de página `plain`):

```latex
% jaes.cls, dentro de la definición del page style:
\def\@evenhead{\hbox to \textwidth{\hfill\rhfont BORRADOR}}%
\def\@oddhead{\hbox to \textwidth{\rhfont BORRADOR\hfill}}%
```

Si el documento ya no es un borrador y se va a entregar en versión final, esto se quita editando esas dos líneas directamente en la copia de trabajo del `.cls` que se distribuye junto al `.tex` (dejar vacío el contenido de `\rhfont`, sin borrar la estructura del `\hbox`):

```latex
\def\@evenhead{\hbox to \textwidth{\hfill\rhfont }}%
\def\@oddhead{\hbox to \textwidth{\rhfont \hfill}}%
```

> Esto modifica el `.cls`, no el `.tex` — si se mantienen varias versiones de un mismo proyecto (borrador vs. entrega final), conviene mantener dos copias del `.cls` (ej. `jaes.cls` y `jaes_final.cls`) en vez de alternar manualmente esas líneas.

---

*Última actualización: Junio 2026 — Generada tras análisis completo de `jaes.cls` (v1.1, rev. Nov-2025), `plantilla.tex` e `Informe.tex`. Sección 16 añadida tras verificación de compilación real (`pdflatex`/`bibtex`) del caso de apéndices con bitácora, fichas técnicas y panel fotográfico de 6 imágenes a ancho completo. §12.3 y §14 corregidas tras un segundo caso real (informe "Estrategia Comercial PYME Master Clean") en el que se confirmó que `\biography` no imprime el texto biográfico, verificado con `\def\printbiography{}` + sección manual de equipo. Sección 17 y §11.4 añadidas tras un tercer caso real (mismo informe, versión 2: 13 páginas, 0 errores/0 warnings) en el que se migró de citación numérica (`ieeetr`) a APA 7 (`natbib`+`apalike.bst`), se corrigió la conjunción "and"→"y" de `cleveref` sin `babel`, se tradujeron `\tablename`/`\refname`, y se eliminó el watermark "BORRADOR" del `.cls` para la entrega final.*



