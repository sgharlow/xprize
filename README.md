# xprize

Focus-area picker and **Sweet 16 bracket** for the [Build with Gemini XPRIZE](https://xprize.devpost.com/) hackathon. A 160-idea pool competes head-to-head inside each of the five categories; re-draw and simulate until one focus area keeps winning.

> Single-file, offline, no build step. Open `index.html` in any browser.

## What it does

- **160 pre-scored seed ideas** — 32 in each of the 5 hackathon categories.
- Scores every idea on the three official, equally-weighted criteria: **Business Viability**, **AI-Native Operations**, **Category Impact** — plus four optional **strategy lenses** (time to first dollar, solo-buildability, distribution wedge, demo value), default off.
- Each **🎲 New draw** deals a random 16 of the 32 ideas per category into an NCAA-style single-elimination bracket; ties break BV → AI → Category Impact (official order).
- **▶ Simulate** plays 1,000 random draws at once (optionally with ±1 scoring jitter) and renders a **focus-area verdict**: the category and idea that keep winning regardless of which ideas get drawn.
- A **Founder-Fit** layer: toggle the domains/capabilities you actually bring and matching ideas climb the bracket.
- Everything re-ranks **live** — move a weight slider or toggle an edge and all brackets re-seed instantly. A session draw log tallies champions across your draws.
- Click any idea to edit its name, pitch, or any of its seven scores; edits persist across draws.

## Run it

Open `index.html` directly, or serve the folder:

```bash
python3 -m http.server 8000   # then visit http://localhost:8000
```

Or publish it free on **GitHub Pages**: repo Settings → Pages → deploy from the `main` branch root; the tool is live at `https://sgharlow.github.io/xprize/`.

## How to pick a focus area with it

1. Toggle on the founder edges you actually have (and any strategy lenses you care about).
2. Hit **▶ Simulate** — the verdict panel names the category that wins most often across 1,000 random draws. That win-rate measures **pool depth**, so it rewards categories you could pivot within.
3. Re-draw a few times and skim the brackets to gut-check the simulator's answer against ideas you'd actually enjoy building.
4. Stress-test: drag a judge-criterion weight around — if the same category keeps winning, commit.

## Files

| File | Purpose |
|---|---|
| `index.html` | The complete tool (UI + scoring engine + 160-idea pool + simulator). |
| `FINALISTS.md` | Historical record of each round's winning ideas (kept out of the active pool). |
| `SPEC.md` | Product / technical specification (v0.2). |
| `LICENSE` | MIT. |

## Status

Draft v0.2 — one-time-use by design. See `SPEC.md` §9 for the remaining roadmap (in-app tag editing, pool add/remove, live Gemini idea generation, export to pitch packet).
