# ASAP for multi-outputs: auto-generating storyboard and pre-visualization with virtual actors based on screenplay

**Hanseob Kim, Ghazanfar Ali, Bin Han, Hwang Youn Kim, Jieun Kim, Hyemin Shin, Gerard Jounghyun Kim, Jae-In Hwang**

**Multimedia Tools and Applications · 2025** · Published

[Paper / publisher](https://doi.org/10.1007/s11042-024-19904-3) · [Project page](https://ghazanfarali.com/research/asap-journal/) · [BibTeX](citation.bib) · [Requirements](REQUIREMENTS.md) · [Code & setup](#implementation-and-usage)

> A screenplay becomes storyboards, animated previsualization, and immersive scenes.

![Original ASAP preparation and runtime architecture, Figure 3](paper-assets/method.png)

*Original method figure from the ASAP journal paper: Figure 3, PDF page 8. Preparation and runtime phases of screenplay-driven previsualization; not a rendering result from this independent implementation.*

## Why this research

Turning a screenplay into a visual preview traditionally requires substantial animation and scene-authoring effort. ASAP interprets script structure and coordinates virtual actors to support early filmmaking decisions.

ASAP parses screenplay structure into characters, dialogue, actions, and emotions. Its modules select virtual actors and compose speech gestures, physical actions, facial expressions, and scene outputs. The journal expands the earlier ISMAR system and SIGGRAPH Asia live demonstration.

## Method at a glance

**Screenplay** → **Characters + actions + emotion** → **Storyboard / 3D / VR**

| | Research system |
|---|---|
| Input | A structured screenplay |
| Method | Screenplay parsing and coordinated character-behavior modules |
| Output | 2D storyboards, animated 3D previews, and immersive VR scenes |

## Evidence and scope

Reported top-1 action accuracy: 93% on simple sentences and 87% on complex sentences

**Attribution:** These findings describe the paper or manuscript, not results obtained with this repository's code.

**Study context:** Screenplay examples and action-sentence evaluation described in the paper.

**Limitations:** Animation coverage and scene composition depend on the available characters, actions, props, and environment assets.

## Explore the implementation

FDX/structured-script parsing, action and motion resolution, behavior timelines, SVG storyboards and schematic portable previews. Original Unity rendering, assets and benchmark results are not reproduced.

This repository contains independently written research code. The institute's original source, datasets and trained models are not distributed. Public-data preparation, commands, assumptions and checks are documented below and in [REQUIREMENTS.md](REQUIREMENTS.md).

## Resources and citation

Read the paper through its [publisher record](https://doi.org/10.1007/s11042-024-19904-3). PDFs are hosted by publishers or preprint archives rather than stored in this repository.

Please cite the research paper when using its ideas; [download the BibTeX citation](citation.bib). The implementation has its own documented scope.

## Implementation and usage

<!-- implementation-guide -->

This repository turns a Final Draft `.fdx` file or a small structured screenplay into an auditable behavior timeline and three portable outputs: an SVG storyboard, an interactive schematic previz, and an immersive inspection page. It implements the paper's paragraph routing, dialogue/gaze/gesture, parenthetical emotion, and prop-oriented action sequence with a transparent offline resolver.

**Citation.** Hanseob Kim, Ghazanfar Ali, Bin Han, Hwang Youn Kim, Jieun Kim, Hyemin Shin, Gerard Jounghyun Kim, and Jae-In Hwang. “ASAP for Multi-Outputs: Auto-generating Storyboard And Pre-visualization with Virtual Actors based on Screenplay.” *Multimedia Tools and Applications* (2025). [https://doi.org/10.1007/s11042-024-19904-3](https://doi.org/10.1007/s11042-024-19904-3). Status: published.

This is new public educational code. It is not the institute's original implementation and does not include its data, models, character/scene assets, Unity project, commercial plugins, or trained weights.

### Run the included example

```powershell
python -m venv .venv
.venv\Scripts\Activate.ps1
python -m pip install -e .
python scripts/smoke.py
start outputs/smoke/storyboard.html
start outputs/smoke/previz.html
```

The default backend is a deterministic TF-IDF cosine baseline. For a cache-only sentence-transformer, install `python -m pip install -e ".[semantic]"` and add `"resolver": {"backend": "sentence-transformer", "model": "path-or-cached-model-name"}` to the library JSON. Loading uses `local_files_only=True`, so a missing model fails instead of downloading weights.

### Input schemas

Structured text uses one `LABEL: text` record per line. Supported labels are `SCENE`, `ACTION`, `CHARACTER`, `DIALOGUE`, and `PARENTHETICAL`. FDX input reads `Paragraph Type` plus nested `Text` elements. The library JSON contains `stage`, keyed `characters`, keyed `props` with interaction anchors, and `motions` whose entries include `id`, `kind`, and example `phrases`.

`timeline.json` is the canonical output. HTML files embed all JSON, CSS, SVG, and JavaScript and can be opened without a server. They are schematic planning artifacts and make no claim to reproduce the paper's Unity rendering, trained GestureCLR mapping, mocap library, commercial TTS/lip-sync, NavMesh/IK, 360 video, or hardware VR.

### Public data and model setup

The included screenplay and motion library are authored artificial fixtures for testing, not paper data. Use screenplays you own or public-domain scripts. Final Draft documents are user-provided; Final Draft is not required for the structured-text format. Build a motion catalog from motions you are licensed to redistribute. Keep downloads under ignored `data/`, `assets/`, `models/`, or `weights/` directories. No training is required for the offline baseline.

To swap in real inputs, keep the same labels in the screenplay (or use an `.fdx` file) and the same keys in `library.json`, then run `asap-multi path/to/screenplay.fdx --library path/to/library.json --out outputs/my-run`. The smoke script and CLI call the same parser, compiler, and renderer.

See [REQUIREMENTS.md](REQUIREMENTS.md) for paper facts, implementation assumptions, and scope limits.
