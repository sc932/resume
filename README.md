# Scott Clark - Resume & CV

LaTeX source for Scott Clark's resume and CV, plus an aligned markdown profile for retrieval-friendly use.

## Files

- **`ScottClarkResume.tex`** / **`.pdf`** - Short 2-page resume.
- **`ScottClarkCV.tex`** / **`.pdf`** - Long-form CV with detailed publications, patents, talks, and education history.
- **`ScottClark.md`** - Plain-text profile mirroring the resume/CV content in markdown. Friendlier for AI agents and text-extraction pipelines than parsing a rendered PDF.
- **`shading.sty`** - Custom LaTeX style required by both `.tex` files.
- **`OriginalTemplate_*.tex` / `.pdf`** - Historical LaTeX resume templates this design descends from (Boedicker → Grant → Johnston → Clark).

For repo conventions, the relationship between the artifacts, and update workflow, see [`AGENTS.md`](AGENTS.md).

## Build

Each `.tex` file compiles to a same-named `.pdf` via:

```bash
pdflatex ScottClarkResume.tex
pdflatex ScottClarkCV.tex
```

### Linux (Ubuntu / Debian)

```bash
sudo apt-get install texlive texlive-latex-extra texlive-fonts-extra
pdflatex ScottClarkResume.tex
```

### macOS

The simplest path is **MacTeX** (full distribution, ~5 GB):

```bash
brew install --cask mactex
pdflatex ScottClarkResume.tex
```

For a smaller install, **BasicTeX** (~100 MB) plus the Charter font package:

```bash
brew install --cask basictex
sudo tlmgr update --self
sudo tlmgr install charter
pdflatex ScottClarkResume.tex
```

If a build fails with a missing-package error, install that package via `sudo tlmgr install <name>`.

### Windows

Two common LaTeX distributions:

- **TeX Live** - download from [tug.org/texlive](https://www.tug.org/texlive/windows.html). Full install includes everything needed.
- **MiKTeX** - download from [miktex.org/download](https://miktex.org/download). Lighter; will fetch missing packages on-demand the first time you build.

After install, from PowerShell or Command Prompt in the repo directory:

```powershell
pdflatex ScottClarkResume.tex
```

## Use this as a template for your own resume

This repo is Scott's working resume, but the LaTeX template has been public since 2011 and is regularly forked. To adapt it for your own use:

1. **Fork the repo** and clone your fork.
2. **Replace the body content** in `ScottClarkResume.tex` and `ScottClarkCV.tex` - header (name, contact, location), the three intro bullets, and each major section. Rename the files if you like.
3. **Replace `ScottClark.md`** - rename to `<YourName>.md` and replace contents (or delete if you don't need a markdown twin).
4. **Update PDF metadata and the agent-readable block** in `ScottClarkResume.tex`:
   - `\hypersetup{pdfauthor=..., pdftitle=..., pdfkeywords=...}` near the top of the preamble (PDF info dictionary).
   - The `{\color{white}\scriptsize ...}` block just before `\end{document}` (white-on-white message for AI agents reading the PDF).

   Both contain Scott's URLs and identifiers - point them at your fork or remove the blocks entirely.
5. **Update `README.md` and `AGENTS.md`** for your own context.
6. **Preserve the template attribution chain** in the comment header at the top of each `.tex` file (Boedicker → Grant → Johnston → Clark → ...). Add your own line at the end.
7. **Build, iterate, and retune.** Spacing knobs (`\resheading`, `\resitem`, `\setlength{\itemsep}`) and page breaks are tuned to Scott's content; with different content, expect line-wraps and break points to shift. See [`AGENTS.md`](AGENTS.md) for what each knob does and how to retune.

If you want a clean-slate starting point, the `OriginalTemplate_*.tex` files in this directory are the unmodified ancestors of this design.

## License

LaTeX source is under the [MIT License](LICENSE.md). The underlying visual design descends from earlier CC-BY-NC-SA 2.5 templates - see the comment block at the top of each `.tex` file for the lineage.

Feel free to fork and edit.
