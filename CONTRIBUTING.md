# Contributing Guide

Thanks for helping improve **Awesome-VLM-Adversarial-Robustness**! This list is organized by **method genealogy**, so adding a paper means more than appending a row — it means placing it where it belongs in the lineage. Please read the short rules below.

## How to contribute

1. Fork the repo and create a branch.
2. Add or edit your entry in `README.md` following the format below.
3. Open a Pull Request with a one-line reason (e.g., *"add CVPR'26 AFT method X, improves PMG-AFT"*).

Small fixes (broken/missing links, wrong venue, a new `Code` link) are very welcome — open an issue or PR directly.

## Entry format

Each paper is one table row:

```
| Year | Method | Title | Venue | Paper | Code |
|:--:|:--|:--|:--:|:--:|:--:|
| 2024 | **FARE** ⭐ | Robust CLIP: Unsupervised Adversarial Fine-Tuning… <br/><sub>→ TeCoA; embedding ℓ₂ alignment</sub> | ICML'24 | [📄](https://arxiv.org/abs/2402.12336) | [💻](https://github.com/...) |
```

Field rules:

| Field | Rule |
|:--|:--|
| **Year** | The **venue year** (e.g., `CVPR'24` → 2024). Use the publication year, not the arXiv-v1 year. |
| **Method** | Short nickname. Use author-year (e.g., *Yang et al. (2024)*) if the paper has no nickname. Wrap the **most-cited foundational baseline** of a sub-family in `**bold**` and append ` ⭐`. |
| **Title** | The paper's real title. Add a one-line `<br/><sub>…</sub>` note. Start the note with `→ X` to state which earlier method it **improves upon** (this is what encodes the genealogy). |
| **Venue** | Abbreviated with two-digit year: `ICLR'23`, `CVPR'24`, `NeurIPS'24`, `arXiv'25`, `TPAMI'26`, workshops as `…W` / Findings as `…-F`. |
| **Paper** | `[📄](url)` to arXiv / DOI / official page. If you genuinely cannot find a canonical URL, use a Google-Scholar search link `[🔍](https://scholar.google.com/scholar?q=<url-encoded title>)`. **Do not invent URLs.** |
| **Code** | `[💻](github-url)` if official code exists; otherwise `—`. |

## Placement rules

- Put the entry under the correct **category** (Attacks / AFT / APT / TTD / Certified / MLLM) and **sub-family** (the `####` heading).
- **Keep genealogy order**: a method's row goes *after* the work it builds on. Foundational baselines (⭐) stay at the top of their sub-family.
- A method may legitimately appear in **two sections** (e.g., a CLIP-encoder defense in both *AFT → Unsupervised* and *MLLM → Vision-Encoder*). That's fine — list it in each relevant place with a stage-appropriate note.
- Need a new sub-family or category? Open an issue first so we keep the taxonomy coherent with the survey.

## Legend (keep consistent)

⭐ foundational/most-cited baseline  ·  `→ X` improves upon X  ·  📄 paper  ·  🔍 search (no canonical link yet)  ·  💻 code  ·  — code not yet linked

## Figures

Taxonomy/timeline figures live in [`assets/`](assets/) and are drawn separately (draw.io). If you have a higher-quality figure, attach it to an issue rather than committing large binaries directly.
