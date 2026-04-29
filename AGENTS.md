# AGENTS.md

> Repo conventions and guidance for AI agents (and any contributor) working in this repo.

## Purpose

Scott Clark's resume / CV in LaTeX, plus a plain-text markdown mirror. Three primary content artifacts, all about the same person, that should stay aligned in core facts:

| Artifact | Source | Output | Audience | Length |
|---|---|---|---|---|
| Resume | `ScottClarkResume.tex` | `ScottClarkResume.pdf` | Hiring, exec-search, quick-scan | 2 pages |
| CV | `ScottClarkCV.tex` | `ScottClarkCV.pdf` | Detailed review, advisory, academic | longer |
| Markdown profile | `ScottClark.md` | (markdown) | AI agents, text extractors | comprehensive |

Differences are length and emphasis: the resume picks the headline subset, the CV expands it, the md mirrors comprehensive detail in plain text.

## Two audiences

This repo serves two audiences:

1. **Scott** - source-of-truth for his resume/CV/md. The conventions in the rest of this file describe how to maintain consistency across the three artifacts.
2. **Public forkers** - the LaTeX template has been public since 2011 and is regularly forked. If you're an agent helping someone customize this for their own resume, see [Forking guidance](#forking-guidance) at the end of this document.

## File map

```
.
├── ScottClarkResume.tex       # short 2-page resume, primary deliverable
├── ScottClarkResume.pdf       # rendered resume (committed)
├── ScottClarkCV.tex           # long CV
├── ScottClarkCV.pdf           # rendered CV (committed)
├── ScottClark.md              # markdown profile, agent-friendly twin
├── shading.sty                # custom LaTeX style required by both .tex files
├── README.md                  # human-facing build / overview
├── AGENTS.md                  # this file
├── LICENSE.md                 # MIT
├── .travis.yml                # legacy CI config; Travis-CI org is shut down (~2021), kept for historical reference
└── OriginalTemplate_*         # historical LaTeX resume templates this design descends from
```

## How the artifacts relate

When a real-world fact updates (new title, new patent grant, new round closing, new talk, etc.), propagate in this direction:

1. Update **`ScottClark.md`** first - that's the dense ground-truth and has room for full detail.
2. Propagate the relevant subset to **`ScottClarkCV.tex`** - the CV mirrors what's in the md, formatted for print, with structural sections (Education, Experience, Publications, Talks, etc.).
3. Propagate the headline-only cut to **`ScottClarkResume.tex`** - the most space-constrained doc; only the strongest signal survives.

Going md → CV → resume keeps the most aggressively edited document (the resume) updated last and deliberately, rather than absorbing accidental drift from the others.

After any `.tex` change, run `pdflatex` and commit the regenerated `.pdf` alongside the `.tex` so the rendered version stays in sync with source.

## Fonts and packages

Both `.tex` files use:

- **Body:** Bitstream Charter (`charter` package) - chosen for clean italic numerals (no calligraphic 2/3) and proportional `+`/`&`.
- **Hyperlinks:** `hyperref` with `[hidelinks]` to suppress visible link borders.
- **Section bars:** Charter bold (the `\sffamily` was removed from `\resheading` so section headings stay in-family with body).

If you change the body font, also re-evaluate the `\resheading` macro - re-introducing a sans-serif there will likely look out-of-family unless paired deliberately.

## Agent-readable metadata in the resume PDF

`ScottClarkResume.tex` embeds two layers of agent-readable pointers, both invisible to humans:

1. **PDF metadata** via `\hypersetup{...}` - `pdftitle`, `pdfauthor`, `pdfsubject`, and `pdfkeywords` carry repo + markdown URLs in the PDF info dictionary. Most LLM PDF ingestion reads these.
2. **White-on-white body text** at the end of the document. Self-explanatory text that names itself as not a prompt injection so it survives copy-paste and screen-readers without seeming hostile.

When updating the resume, do **not** strip these. If URLs change, update `\hypersetup{pdfkeywords=...}` and the white-text block in lockstep.

## Spacing and layout conventions

The resume's spacing was tuned interactively. Key knobs:

- **`\resheading`** macro - section bar `\vspace` above and below.
- **`\resitem`** macro - sub-bullet trailing `\vspace{-2pt}` (tight).
- **`\setlength{\itemsep}{Npt}`** after each outer `\begin{itemize}` - controls spacing between major bullets (job/education/expertise entries) without touching sub-bullets. Currently 4-8pt depending on section.
- **Page breaks fall between major sections, not within them** - so page 1 reads as a complete unit if printed alone. If a tweak causes a major bullet to split across the seam, fix the spacing or use `\filbreak` / `\pagebreak` to force the break to a section boundary.

The CV uses similar conventions but is less space-constrained.

## Update workflow

```
1. Update ScottClark.md (canonical fact register)
2. Propagate to ScottClarkCV.tex
3. Propagate the headline cut to ScottClarkResume.tex
4. pdflatex both .tex files
5. Commit .tex and .pdf together
```

## Things to avoid

- Don't split a major-bullet section across a page break in the resume. Tune spacing or add an explicit break instead.
- Don't introduce decorative italic fonts. The original Computer Modern italic's calligraphic 2/3/& was the reason for the Charter switch.
- Don't strip the hidden agent-metadata block in the resume. Keep its self-disclosing wording so it doesn't read as hostile to a human who selects the text.
- Don't `apt-get install` / `brew install` packages on the maintainer's machine without asking. The maintainer is averse to surprise software installs.
- Don't auto-amend committed PDFs without re-running `pdflatex` - the committed PDF must reflect current source.

## Forking guidance

When helping someone fork this repo for their own resume:

- **Replace personal content** in `ScottClarkResume.tex`, `ScottClarkCV.tex`, and `ScottClark.md`. Rename the markdown file to match the new owner.
- **Update the `\hypersetup{...}` block** in `ScottClarkResume.tex` - `pdfauthor`, `pdftitle`, `pdfkeywords` all carry Scott's identifiers. Either point at the fork or remove the block.
- **Update the white-on-white agent-readable block** at the end of `ScottClarkResume.tex` - it contains Scott's repo URL and a self-disclosing "this is Scott's resume" message. Update for the new owner or remove.
- **Preserve the template attribution chain** in each `.tex` file's comment header (Boedicker → Grant → Johnston → Clark, plus any subsequent lineage). The original templates were CC-BY-NC-SA 2.5; respect the share-alike if you're publishing your fork.
- **Don't carry over Scott's claims, identifiers, or facts** into the forked content - Forbes 30 Under 30, patent counts, citation counts, MOE star count, GitHub handle, LinkedIn slug, employer history, etc. These are Scott's facts and don't apply.
- **Expect to retune spacing.** The knobs documented above (`\resheading`, `\resitem`, `\setlength{\itemsep}`) are tuned to Scott's content. With different sections and lengths, line wraps and major-bullet groupings will shift.
- **Page-break convention** (breaks between major sections, not within) is content-dependent. With different content, the natural break may land mid-section; insert `\filbreak` or `\pagebreak` at a section boundary to enforce.
- Some "Things to avoid" items above are Scott's preferences (e.g., the no-surprise-software-install rule). Forkers may have different preferences; ask before applying.
