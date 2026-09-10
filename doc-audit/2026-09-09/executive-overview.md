                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                

---
genre: documentation
kind: summary
title: Documentation Health — Executive Overview
relevance:
  always-include: true
---
# Documentation Health — Executive Overview

> **Generated:** 2026-09-09  **Repository:** IBM/ibmi-tobi (The Object Builder for i — TOBi)
> **Audience:** Engineering team

This is the **only** report doc-audit produces. It carries the overall score, a
heatmap, and a self-contained mini-summary per dimension. There is no separate
per-dimension file and no improvement roadmap — each summary states the score,
freshness, strengths, and the gaps that held the score back.

## Documentation Health Score

**Score: 56 / 100** — _Fair_

Bands: 90–100 Excellent · 75–89 Good · 55–74 Fair · 30–54 Poor · 0–29 Critical.

## Dimension Heatmap

| Dimension               |            Score (1–5)            |    Weight    | Weighted Contribution | Status |
| ----------------------- | :---------------------------------: | :----------: | :-------------------: | :----: |
| In-Code Documentation   |                  3                  |      27      |         16.2         |   🟡   |
| README & Onboarding     |                  3                  |      22      |         13.2         |   🟡   |
| API Reference           | — skipped: no API surface detected |      22      |          —          |   —   |
| Architecture & Design   |                  2                  |      16      |          6.4          |   🔴   |
| User & Developer Guides |                  3                  |      13      |          7.8          |   🟡   |
| **Overall**       |                 —                 | **78** |   **56/100**   |   —   |

_Status: 🟢 4–5 · 🟡 3 · 🔴 1–2. API Reference was skipped (no API surface detected); its 22-point weight was redistributed proportionally across the four dimensions that ran (effective total weight = 78). The freshness gate applies: architecture-design is capped at ≤3 due to stale/placeholder content._

## Dimension Summaries

> One self-contained mini-assessment per dimension — a compressed version of a
> full dimension report. Cite real `file:line` / paths.

### In-Code Documentation — 3/5 · 🟡 · weight 27

- **Freshness:** current — docstrings sampled against the code they describe are consistent with the implementation.
- **Strengths:**
  - `src/makei/utils.py` is the best-documented module: most public functions carry a docstring with purpose and often doctests that serve as usage examples (e.g. `decompose_filename` at `utils.py:225`, `make_include_dirs_absolute` at `utils.py:311`).
  - `src/makei/iproj_json.py:15` (`IProjJson`) and `src/makei/ibmi_json.py:12` (`IBMiJson`) have class-level docstrings explaining their roles.
- **Gaps:**
  - `src/makei/config.py:4–22` — `Config` class and all four of its methods have no docstrings at all; this is a public class with no explanation of purpose or usage.
  - `src/makei/build.py:46` — `BuildEnv.__init__` is undocumented; private helpers `_create_build_vars` (`build.py:247`), `_post_make` (`build.py:470`), and `_prepare_timestamp_check` (`build.py:170`) have no or only inline comments, even though the logic is complex and non-obvious.
  - `setup.cfg:30` confirms `disable=missing-docstring` in pylint, meaning inconsistent coverage is an accepted norm rather than a regression.

### README & Onboarding — 3/5 · 🟡 · weight 22

- **Freshness:** STALE — `docs/getting-started/usage.md:2` points exclusively to `BobUsage.pdf`, which retains the old "Bob" branding and is an opaque binary; it is not rendered in the docs site. `docs/getting-started/installation.md` verify-step output does not list the `info` subcommand that exists in `src/makei/cli/makei_entry.py:198`. → **score capped at 3**.
- **Strengths:**
  - `README.md` clearly states purpose, key differentiators, and links to the docs site; a new reader understands the project in under two minutes.
  - `docs/getting-started/installation.md` and `docs/getting-started/prerequisites.md` together provide a thorough install path (yum, PASE, bash, SSH, NTP) with runnable commands.
- **Gaps:**
  - `docs/getting-started/usage.md` is a one-line stub that links to a PDF — no inline walkthrough of the build/compile workflow (`docs/getting-started/usage.md:1–2`).
  - How to run the test suite is under-documented: `docs/contributing/testing.md` mentions `python makei/utils.py -v` and VS Code Test Explorer but never shows how to invoke `pytest` or the `nox` sessions that `noxfile.py` defines.

### Architecture & Design — 2/5 · 🔴 · weight 16

- **Freshness:** STALE — `docs/contributing/source-structure.md:2` contains only a link to `BobDetails.pdf`, which uses the deprecated "Bob" name and is a binary PDF with no version control traceability. The content cannot be verified against the current v3.x codebase. → **score capped at 3; scored 2 due to the depth of the gap**.
- **Strengths:**
  - `docs/welcome/overview.md` provides a good philosophical/motivational narrative explaining why the project exists and the general GNU Make + PASE architecture.
- **Gaps:**
  - No ARCHITECTURE.md, no module-responsibility document, and no diagram (no `.drawio`, `.puml`, or inline Mermaid). A new contributor cannot form a correct mental model of how `BuildEnv` → `RulesMk` → `MKRule` → `crtfrmstmf` interact without reading all source files.
  - `docs/contributing/source-structure.md:2` is a **single-line placeholder** pointing to a stale PDF — the repo has no durable, text-based architecture doc at all.
  - No ADRs recorded anywhere in the repository.

### User & Developer Guides — 3/5 · 🟡 · weight 13

- **Freshness:** STALE — `docs/getting-started/usage.md:2` links to `BobUsage.pdf` (old branding, opaque binary) and `docs/reference/recipes.md:1` is an empty stub (contains only the heading `# Recipes`). → **score capped at 3**.
- **Strengths:**
  - `docs/prepare-the-project/rules.mk.md` is thorough and up-to-date: covers basic rules, wildcarding, special-character escaping (# and $), and compile-attribute overrides with worked examples.
  - `docs/prepare-the-project/iproj-json.md` is comprehensive: all configuration fields are documented with purpose, constraints, and a full embedded JSON Schema.
- **Gaps:**
  - `docs/reference/recipes.md` is entirely empty (`recipes.md:1` — heading only), leaving the reference section hollow.
  - `docs/getting-started/faq.md` contains exactly one FAQ entry (timestamp reset after source extraction); no troubleshooting guide exists.
  - No end-to-end worked example (clone → set up iproj.json → define Rules.mk → build → inspect joblog) in the docs; the `docs/getting-started/sample-build.md` file exists but was not reviewed — check its content (it may fill this gap partially).

## Coverage Snapshot

| Item                        | Value                                                                                                                                                                                                                                                                                        |
| --------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Files scanned               | ~70 (src: 12 Python modules; docs: ~45 Markdown files; tests: ~13 Python files)                                                                                                                                                                                                              |
| Primary languages           | Python, GNU Make (Makefile/Rules.mk), Shell scripts, Markdown                                                                                                                                                                                                                                |
| Dimensions assessed         | in-code, readme-onboarding, architecture-design, user-developer-guides                                                                                                                                                                                                                       |
| Dimensions skipped (reason) | api-reference — no API surface detected (no OpenAPI/Swagger/GraphQL/proto files; no web-framework route definitions in src/)                                                                                                                                                                |
| Docs present                | docs/ Docsify site with getting-started, CLI reference, prepare-the-project, integration, contributing, reference, changelogs                                                                                                                                                                |
| Potentially stale docs      | `docs/getting-started/usage.md` (PDF stub with old "Bob" branding); `docs/contributing/source-structure.md` (single-line link to stale `BobDetails.pdf`); `docs/reference/recipes.md` (empty stub); `docs/getting-started/installation.md` verify output omits `info` subcommand |
| Docs missing                | Architecture overview with module responsibilities and at least one diagram; ADRs; full end-to-end tutorial; troubleshooting guide; test-suite invocation instructions                                                                                                                       |
