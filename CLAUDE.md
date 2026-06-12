# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A single-file, offline web tool for picking a focus area for the Build with Gemini XPRIZE hackathon: a 160-idea pool (32 per category × 5 categories), where each 🎲 draw fields a random 16 per category into NCAA-style Sweet 16 brackets, and a Monte Carlo simulator (~1,000 draws) produces a focus-area verdict. `SPEC.md` (v0.2) is the source of truth for the scoring model, draw/simulation mechanics, data model, and acceptance criteria (§10). The tool is intentionally **one-time-use**: content quality beats reusability.

## Running it

No build step, no tests, no dependencies. Open `index.html` directly in a browser, or serve it:

```bash
python3 -m http.server 8000
```

Deployed via GitHub Pages from the `main` branch root: https://sgharlow.github.io/xprize/

## Architecture

Everything lives in `index.html` — HTML, CSS, and vanilla JS in one file. Hard constraints (SPEC.md §8):

- **No frameworks, no bundler, no network calls at runtime.**
- **No browser storage** — no `localStorage`/`sessionStorage`; all state is in-memory per session.
- **Deterministic given a draw** — same drawn 16 + same weights/edges/scores always yield the same ranking (final tie-break key is name). Draw sampling and simulation jitter are random by design (v0.2).

Layout of `index.html`'s `<script>`:

- `SEED_DATA`: 160 ideas as compact arrays `[name, pitch, BV, AI, CI, TTFD, Solo, Wedge, Show, [tags]]`, keyed by category. Exactly 32 per category; a draw samples `DRAW_SIZE` (16) of them.
- Constants: `SEED_ORDER_16` (bracket seeding positions), `DOMAIN_TAGS` / `CAP_TAGS` (controlled tag vocabulary — idea tags must be a subset), `SIM_RUNS`.
- Globals: `pool`/`byId` (hydrated idea objects), `draws` (current 16 ids per category), `drawHistory`, `weights` (`bv`/`ai`/`ci` default 1.0, `fit` 1.5, four lens weights default 0), `edges`, `simResults`.
- Scoring: `total = base(bv,ai,ci) + lens(ttfd,solo,wedge,show) + fitBonus`. `cmp()`/`better()` break ties in the official order **BV → AI → CI**, then name.
- Draws: `newDraw()` Fisher–Yates-samples 16 of 32 per category and logs to `drawHistory`; idea edits persist across draws because draws hold ids into `pool`.
- Simulator: `simulate(n)` skips the bracket (deterministic single-elimination champion = max of the sample) and tallies `catWins`/`ideaChamp`/`ideaOverall`; optional ±1 jitter on bv/ai/ci. Any weight/edge/score change calls `invalidateSim()` which flags the verdict stale (visible warning) rather than silently recomputing.
- Rendering: `render()` recomputes everything from scratch on any input change. There is no incremental update path; keep it that way.

## Invariants to preserve when editing

From SPEC.md §10 — any change should keep these true:

- Exactly 32 ideas per category in the pool; every draw fields exactly 16; every bracket resolves to a champion in four rounds.
- Any weight/edge/score change re-ranks all brackets live, without reload, and stale-flags an existing simulation verdict.
- Setting the Founder-Fit weight and all four lens weights to 0 reproduces the neutral judge-only ranking.
- The file opens from disk with no console errors and no network access.
- Sub-scores (all seven) are integers in `[1, 10]`; weights in `[0, 3]`.

Quick data-integrity check (counts, field arity, score ranges, tag vocabulary):

```bash
node -e "const h=require('fs').readFileSync('index.html','utf8');const d=eval('('+h.match(/const SEED_DATA = (\{[\s\S]*?\n\});/)[1]+')');for(const c in d){if(d[c].length!==32)throw c;d[c].forEach(r=>{if(r.length!==10)throw r[0]})};console.log('ok')"
```

## File notes

- Roadmap lives in SPEC.md §9; do not add roadmap features (Gemini API calls, persistence, export) unless explicitly requested.
