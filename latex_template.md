# TU Delft PhD Dissertation LaTeX Template (Improved Consolidated Guide)

This document consolidates usage, installation, and improvement notes for the improved TU Delft PhD thesis template by Moritz Beller used for this dissertation.

---

## 1. Overview

The template is an improved variant of the official [TU Delft PhD dissertation template](https://www.tudelft.nl/en/tu-delft-corporate-design/downloads/). It focuses on:

- High on‑screen and print readability.
- Better binding and page layout.
- Reduced printing cost (optimized color usage, crisp blacks, embedded fonts).
- Modern fonts (Libertinus for text, Inconsolata for monospaced code) with XeLaTeX.
- A rich, curated, compatible package set.
- Commands and Makefile targets to generate print‑ready PDFs.

---

## 2. Attribution / Usage Courtesy

Please attribute Moritz Beller for this adopted version of the template.
Moreover, please send a message (moritzbeller -AT-gmx -DOT- de) that you are using this style, together with your expected defense date, university and research group.

---

## 3. Theses Using This Improved Style (Historical Examples)

- Moritz Beller (2018) – Software Engineering Research Group, TU Delft  
- Anand Sawant (2019) – Software Engineering Research Group, TU Delft  
- Davide Spadini (2021) – Software Engineering Research Group, TU Delft  
- Vladimir Kovalenko (2021) – Software Engineering Research Group, TU Delft  
- Alex Salazar – Computer Science Intelligent Systems, TU Delft  

(Feel free to add yours.)

---

## 4. Key Improvements vs. Original TU Delft Template

The improved template:

1. Compiles reliably (tested with CI / latexmk + XeLaTeX).
2. Approved by the TU Delft Graduate School in prior dissertations.
3. Adjusts binding offset & margins for cleaner book production.
4. Uses Libertinus + Inconsolata for superior screen & print legibility.
5. Bundles a wide range of pre-selected packages in a safe loading order.
6. Minimizes unnecessary color pages while keeping TU Delft blue highlights.
7. Improves print crispness (solid blacks instead of mid‑grays for certain elements).
8. Normalizes page headers & numbering style.
9. Provides consistent online vs. print PDF appearance (with necessary print tweaks).
10. Expands propositions page usability & integration.
11. Simplifies copyright notices.
12. Adds glyphs & formatting conveniences for publications, CV, and series pages.
13. Includes commands / Makefile target for fully embedded-font, prepress PDFs.
14. Encourages link durability (e.g., URL Eternalizer tool) for long-term archive quality.
15. Supplies helper macros (`\circled`, `\ahref`, listing presets, etc.).

---

## 5. Files & Structure (Core)

| File | Purpose |
|------|---------|
| `dissertation.tex` | Main driver file. |
| `dissertation.cls` | Thesis class (improved). |
| `propositions.tex` | Separate propositions document. |
| `Makefile` | Convenience build targets. |
| `fonts/` | Bundled fonts (install on system for XeLaTeX). |
| `chapters / sections` | Content sources for individual chapters. |

---

## 6. Installation & Environment

You can compile with standard LaTeX engines, but to respect the TU Delft corporate design (font usage) you should use **XeLaTeX**.

### 6.1 Recommended Toolchain

- TeX Live (Linux/macOS) or MiKTeX (Windows)
- `latexmk` (automation)
- Ghostscript (for print/pdf prepress embed)
- `makeglossaries` (glossary generation)
- `bibtex` or `biber` (depending on bibliography backend; current setup uses BibTeX)

### 6.2 Windows (MiKTeX) Packages

MiKTeX typically installs missing packages on demand. The original list of required packages (if manual installation is needed):

```
caption, fancyhdr, filehook, footmisc, fourier, l3kernel, l3packages,
lettrine, metalogo, mptopdf, ms, natbib, pgf, realscripts, tipa, titlesec,
tocbibind, unicode-math, url, xcolor, xetex-def
```

### 6.3 Linux (Debian/Ubuntu) Installation Example

```bash
sudo apt update
sudo apt install texlive-xetex texlive-latex-extra latexmk \
  texlive-lang-english texlive-lang-european texlive-lang-german \
  texlive-fonts-extra texlive-science ghostscript
```

### 6.4 macOS

Install MacTeX (or BasicTeX + extras) and Ghostscript. Optionally:

```bash
brew install --cask mactex
brew install ghostscript
```
Then install (or double-click) the fonts in `fonts/` so the OS font registry sees Libertinus & Inconsolata for XeLaTeX.

### 6.5 Font Notes

- The template expects Libertinus and Inconsolata when compiled with XeLaTeX.
- If you want native LaTeX fonts, compile with the `nativefonts` option (or use `pdflatex`), though you lose typographic enhancements.

---

## 7. Compilation

### 7.1 Makefile Targets (Recommended)

```bash
make                 # builds dissertation.pdf, propositions.pdf, dissertation_print.pdf
make clean           # removes main PDF & auxiliaries (see Makefile details)
```

### 7.2 Manual Latexmk Workflow

```bash
latexmk -xelatex dissertation
bibtex dissertation            # run as needed (per chapter if using chapter bibs)
makeglossaries dissertation    # if glossaries enabled
latexmk -xelatex dissertation  # compile again for refs
latexmk -xelatex dissertation  # final pass to stabilize cross-refs
```

### 7.3 Propositions

```bash
latexmk -xelatex propositions
```

Or (classic way):

```bash
xelatex propositions
```

### 7.4 Print-Ready PDF (Embedded Fonts, Prepress)

Executed automatically by the `Makefile` target; explicit Ghostscript command pattern:

```bash
gs -dNOPAUSE -dBATCH -sDEVICE=pdfwrite -dPDFSETTINGS=/prepress \
  -dEmbedAllFonts=true -sOutputFile=dissertation_print.pdf -f dissertation.pdf
```

### 7.5 Clean Build

If you encounter difficult incremental build issues:

```bash
latexmk -C dissertation
```

Then rebuild via the standard sequence.

---

## 8. Document Options (Class)

Key (illustrative) options you may find or add in `dissertation.cls` or driver:

- `print` – activate print layout adjustments.
- `nativefonts` – fall back to default LaTeX fonts (non-XeLaTeX path).
- `draft` – (if added) quicker builds (may suppress images / hyperlinks styling).

Consult the class file or comments inside for any additional custom options.

---

## 9. Propositions Page

`propositions.tex` is compiled separately; keep style consistent with main thesis. The improved template scales better for more text, integrating aesthetically into the bound volume.

---

## 10. Glossaries & Acronyms

If using glossaries:
 
1. Include the appropriate package calls (already in the template if enabled).
2. Run `makeglossaries dissertation` (or rely on latexmk rules if configured).
3. Re-run XeLaTeX twice for references.

---

## 11. Bibliography

Default workflow uses `natbib` with BibTeX:

```bash
bibtex dissertation
```

Run after an initial XeLaTeX pass; re-run XeLaTeX twice afterwards. If you prefer `biber`, adapt preamble and replace the bibliography commands accordingly.

---

## 12. Helpful Macros & Customizations

The improved template often ships with convenience commands (examples):
 
- `\circled{1}` – circled numbers/characters.
- `\ahref{<url>}{<text>}` – consistent hyperlink styling.
- Preconfigured `lstlisting`/code style.

Review the class / auxiliary style files for the full list.

---

## 13. Troubleshooting Tips
 
| Symptom | Suggestion |
|---------|------------|
| Missing font warnings | Ensure Libertinus & Inconsolata installed; use XeLaTeX. |
| Overfull hboxes | Adjust hyphenation / add discretionary breaks / widen margins slightly. |
| Glossary not appearing | Confirm `\makeglossaries` in preamble and run `makeglossaries`. |
| Broken references | Run `latexmk -xelatex` enough times (or `latexmk -C` then rebuild). |
| Package clash | Check load order in class file; comment out conflicting packages. |

---

## 14. Recommended Long-Term Link Preservation

Consider using the [LaTeX URL Eternalizer](https://github.com/Inventitech/url-eternalizer) so that referenced web resources remain accessible years after publication.

---

## 15. Version Provenance

Base: TU Delft dissertation class (original dated in class comments as `2013/07/08 v1.0`, repository snapshot ~July 2015, commit `ff9d073` referenced in improved notes). This consolidated guide reflects both original and improvement-layer practices as of the current repository state.

---

## 16. Suggested Workflow Summary

1. Install TeX environment + fonts.
2. Draft chapters under organized directories (`introduction/`, `background/`, etc.).
3. Maintain a single BibTeX database (or per-chapter if truly needed).
4. Use `latexmk` for iterative builds; inspect warnings early.
5. Before submission: run print build, verify font embedding (e.g., `pdffonts`), check hyperlinks & page references.
6. Archive final sources + print PDF + online PDF variant.

Happy typesetting! 🎓

---
