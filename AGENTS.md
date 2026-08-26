# AGENTS.md

## Repository status

- **Research-only repo, no code.** All directories (`lecturas/`, `notas/`, `borradores/`, `datos/`, `implementacion/`) are currently empty.
- No build system, no tests, no CI, no dependencies. Nothing to lint or typecheck.
- All untracked files; no commits on `main` yet.

## Project context

- **Topic:** Computational graph analysis applied to linguistic distance data. Build a weighted graph from Levenshtein distances on Swadesh word lists, then apply graph algorithms (community detection, MST, centrality, shortest paths) to discover structure.
- **Research question:** What computational structure does the lexical distance network between languages have, and which graph algorithms reveal patterns that align (or don't) with known linguistic classifications?
- **Planned implementation language:** C++ (not yet started).
- **Key data sources:** ASJP database, Swadesh lists (100–207 concepts).

## Conventions

- **Language:** All docs are in Spanish. Keep new files consistent with the language of the file they sit beside.
- **This is a collaborative research workspace**, not a code repo. The agent's role is to help with literature review, critique notes, suggest algorithms, and discuss methodology — not to generate code unless explicitly asked.
- `bitacora.md` is the session log. Update it when significant work happens.
- `propuesta-de-investigacion.md` is the canonical research proposal (replaces the earlier `base.md`).

## Directory layout

| Directory | Purpose |
|---|---|
| `lecturas/` | Reference papers and documents |
| `notas/` | Research notes, summaries, critiques |
| `borradores/` | Draft reports or documentation |
| `datos/crudos/` | Raw linguistic data (ASJP, Swadesh) |
| `datos/procesados/` | Cleaned data ready for analysis |
| `implementacion/` | Future C++ code |

## When this file needs updating

Update after: first code is added, first data is downloaded, or the research scope changes materially.
