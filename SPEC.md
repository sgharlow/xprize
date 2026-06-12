# XPRIZE Focus-Area Picker & Sweet 16 Bracket — Product / Technical Spec

**Status:** Draft v0.2 · **Owner:** Steven Harlow ([@sgharlow](https://github.com/sgharlow)) · **Last updated:** 2026-06-11

---

## 1. Summary

A single-file, offline-capable web tool that helps an entrant in the **Build with Gemini XPRIZE** hackathon *pick a focus area* — the category (and lead idea) to commit to. A **160-idea pool** (32 per category) is pre-scored on the three official judging criteria plus four personal **strategy lenses**. Each **draw** deals a random 16 of the 32 ideas per category into an **NCAA-style Sweet 16 bracket**; the five bracket champions advance to an overall leaderboard. A **Monte Carlo simulator** plays ~1,000 draws at once and reports which category and which idea keep winning — the focus-area verdict — so the answer doesn't depend on the luck of a single deal. A personal **Founder-Fit** layer biases ranking toward the builder's demonstrated strengths.

The artifact is `index.html` — no build step, no backend, no network dependency, no browser storage. It opens in any browser and recomputes everything live in the client.

**v0.2 is intentionally one-time-use:** content quality (the idea pool and its scoring) is prioritized over reusability. Determinism is relaxed by design — ranking is deterministic *given a draw*; which 16 ideas are drawn is random on every 🎲 New draw.

---

## 2. Goals & non-goals

### Goals
- Produce a **defensible focus-area decision**: which category, and which lead idea, to commit the hackathon window to.
- Encode the hackathon's actual rubric so scoring maps to how judges decide.
- Make the ranking **reactive**: any input change (criterion weights, strategy lenses, founder edges, per-idea scores) re-seeds and re-runs all brackets instantly.
- Make the answer **robust to draw luck and scoring error**: random re-draws on demand, plus a simulator that aggregates over many draws (optionally with ±1 score jitter).
- Make personalization explicit and auditable rather than a black box — the builder sees exactly why an idea rose or fell.
- Stay a single portable file that can be committed, opened from disk, or hosted on GitHub Pages with zero setup.

### Non-goals
- Not a reusable product — v0.2 is a one-time decision tool; content quality beats generality.
- Not a submission tool — it does not talk to Devpost or submit anything.
- Not a live LLM idea generator (the 160 seed ideas are authored; live generation is a roadmap item).
- Not a multi-user or persistence system — state lives in memory for the session.
- Not financial, legal, or investment advice.

---

## 3. Hackathon context (the domain model this encodes)

Source of truth: the official rules, FAQ, and overview as of 2026-06-11.

- **Prize pool:** $2,000,000. 1st $500k, 2nd $200k, 3rd–5th $100k each, 15 runner-ups $50k, plus one $50k category prize per category.
- **Submission window:** May 19 – Aug 17, 2026. **Judging:** Aug 18 – Sep 15, 2026. **Winners:** ~Sep 25, 2026.
- **Hard requirements:** build a *newly created* business that **operates with AI agents**, uses **at least one Google Cloud product**, and makes **at least one Gemini API LLM call** in the deployed app. Real users and real revenue during the window.
- **Five categories:**
  1. Education & Human Potential
  2. Entrepreneurship & Job Creation
  3. Small Business Services
  4. Money & Financial Access
  5. Professional Services Access
- **Two-stage judging:**
  - **Stage One — pass/fail:** does the project fit a category and reasonably apply the required APIs/SDKs?
  - **Stage Two — scored, equally weighted:** Business Viability, AI-Native Operations, Category Impact.
- **Official tie-break order:** Business Viability → AI-Native Operations → Category Impact.

### The three scored criteria
| Criterion | What judges assess |
|---|---|
| **Business Viability** | Real business, real users, real revenue in the 90-day window plus the sustainability of the model. Favors clear willingness-to-pay, fast time-to-first-dollar, low CAC. |
| **AI-Native Operations** | The extent to which AI is *live in production and executes key decisions* — the operator, not a feature. Favors agentic workflows doing the actual work. |
| **Category Impact** | Meaningfully moves the needle: redefines how something works, or reaches a scale where widespread adoption is credible. |

---

## 4. Scoring model

### 4.1 Per-idea base score
Each idea carries three integer sub-scores in `[1, 10]`: `bv`, `ai`, `ci`.

```
base(idea)  = bv·w_bv + ai·w_ai + ci·w_ci
```

Default weights `w_bv = w_ai = w_ci = 1.0` (official equal weighting). Weights are user-adjustable in `[0, 3]` to stress-test how robust a winner is if a judging panel leans one way. With default weights the base maximum is 30.

### 4.2 Strategy lenses (v0.2)
Each idea additionally carries four pre-scored **strategy lenses**, integers in `[1, 10]` — personal prioritization criteria the judges don't score but the builder should:

| Lens | Field | What it measures |
|---|---|---|
| Time to first dollar | `ttfd` | How fast the idea can earn its first real revenue inside the 90-day window. Self-serve consumer/SMB scores high; regulated, partnership-gated, or enterprise-sales ideas score low. |
| Solo-buildable | `solo` | Whether one builder can ship *and operate* it in the window. Pure software + LLM scores high; licensing, money movement, or two-sided marketplaces score low. |
| Distribution wedge | `wedge` | How reachable the first paying users are without an existing audience (SEO-able pain, communities, marketplaces vs. institutional buyers). |
| Demo / showcase value | `show` | How vividly it demonstrates agentic AI to judges (voice, visible multi-step autonomy, money-saved receipts). |

```
lens(idea) = ttfd·w_ttfd + solo·w_solo + wedge·w_wedge + show·w_show
```

All four lens weights default to **0** — the default view is the pure judge view. Sliders range `[0, 3]`. The lenses answer "*which idea should I, specifically, build*" rather than "*which idea scores best in the abstract*". New prioritization criteria can be expressed by raising a lens weight, editing per-idea lens scores in the modal, or both.

### 4.3 Founder-Fit layer
Each idea is tagged with domain and capability tags (see §5). The builder toggles the **edges** they actually bring. Fit is additive:

```
fitCount(idea) = | idea.tags ∩ enabledEdges |
fitBonus(idea) = fitCount(idea) · w_fit          // w_fit default 1.5, range [0, 3]
total(idea)    = base(idea) + lens(idea) + fitBonus(idea)
```

Setting `w_fit = 0` and all lens weights to 0 reproduces the neutral, judge-only ranking. Founder-Fit is a *private lens* — it reflects unfair advantages (warm network, prior shipped work, domain knowledge) that materially affect real-revenue speed and execution quality, which in turn map onto Business Viability and Category Impact.

### 4.4 Pool, draws & randomization (v0.2)
Each category ships **32 authored, pre-scored ideas** (160 total). A **draw** uniformly samples 16 of the 32 per category (Fisher–Yates shuffle, `Math.random`). The 🎲 **New draw** action deals a fresh sample for all five categories and re-runs everything under the current weights and edges. Idea edits persist across draws (they edit the pool). Each category bracket lists which 16 ideas are "sitting out" the current draw.

Ranking is **deterministic given a draw**; the draw itself is random by design. A session **draw log** records every draw and shows each draw's overall champion, recomputed live under the *current* weights.

### 4.5 Seeding & comparison
Within a category, the 16 drawn ideas are sorted descending by `total`, with ties broken by `bv`, then `ai`, then `ci`, then name (deterministic). Sort position assigns the **seed** (1 = strongest).

### 4.6 Bracket
Standard 16-team single-elimination seeding positions:

```
[1, 16, 8, 9, 5, 12, 4, 13, 3, 14, 6, 11, 7, 10, 2, 15]
```

so the top two seeds can only meet in the final. Each matchup advances the higher `total`; ties break by `bv → ai → ci`, mirroring the official tie-break order. Because winners are decided purely on score, the bracket is a transparent visualization of the underlying ranking — changing any input changes the path through it. Rounds: Round of 16 → Quarterfinals → Semifinals → Final → Champion.

### 4.7 Overall leaderboard
The five category champions are re-compared by the same total + tie-break rule and ranked 1–5. The overall #1 is the strongest single idea to chase for the grand prize.

### 4.8 Monte Carlo simulator & focus-area verdict (v0.2)
▶ **Simulate** plays **1,000 independent draws** instantly: per draw, per category, sample 16 of 32 and crown the strongest (with deterministic scoring, the bracket champion equals the max of the sample, so the bracket itself is skipped for speed); then the five champions contest the overall title. Optional **±1 score jitter** (default on) perturbs each idea's `bv`/`ai`/`ci` by −1/0/+1 (clamped to `[1,10]`) per simulation, testing robustness to scoring error.

Outputs, rendered as the **verdict panel** on the Overall Leaderboard tab:
- **Recommended focus area** — the category that wins the most overall titles. Because any single idea appears in only ~50% of draws, the category win-rate measures **pool depth** (the category keeps producing winners regardless of which ideas are drawn) — exactly the property wanted in a focus area the builder may pivot within.
- **Most robust single idea** — the idea with the most overall titles.
- Per-category: overall-title share and the top 3 most frequent category champions.

The verdict is computed under the weights/edges at run time; any later change marks it **stale** with a visible re-run warning rather than silently misleading.

---

## 5. Data model

```ts
type Idea = {
  id: string;        // "<category>#<index>"
  cat: Category;
  name: string;
  pitch: string;     // one-line description
  bv: number;        // 1..10 Business Viability
  ai: number;        // 1..10 AI-Native Operations
  ci: number;        // 1..10 Category Impact
  ttfd: number;      // 1..10 Time to first dollar (lens)
  solo: number;      // 1..10 Solo-buildable in window (lens)
  wedge: number;     // 1..10 Distribution wedge (lens)
  show: number;      // 1..10 Demo/showcase value (lens)
  tags: string[];    // subset of DOMAIN_TAGS ∪ CAP_TAGS
};

type Category = "Education & Human Potential"
              | "Entrepreneurship & Job Creation"
              | "Small Business Services"
              | "Money & Financial Access"
              | "Professional Services Access";

type State = {
  pool: Record<Category, Idea[]>;    // exactly 32 per category (160 total)
  draws: Record<Category, string[]>; // ids of the 16 currently drawn per category
  drawNum: number;
  drawHistory: { n:number; ids:Record<Category,string[]> }[];
  weights: { bv:number; ai:number; ci:number; fit:number;
             ttfd:number; solo:number; wedge:number; show:number };
  edges: Set<string>;                // enabled founder edges
  activeTab: number;                 // 0..4 categories, 5 = overview
  simResults: null | { n:number; jitter:boolean; stale:boolean;
                       catWins:Record<string,number>;
                       ideaChamp:Record<string,number>;
                       ideaOverall:Record<string,number> };
};
```

**Controlled tag vocabulary**

- `DOMAIN_TAGS`: education, healthcare, legal, tax, fintech, lending, insurance, benefits, smb, restaurant, retail, trades, hr, realestate, creator, nonprofit, emergingmarkets, startups
- `CAP_TAGS`: voice, doc-automation, outbound, forecasting, scheduling, compliance, matching, underwriting, marketing, payments

Each category ships with 32 authored, pre-scored ideas (160 total). All clear Stage One by construction.

---

## 6. Personalization from external profiles

The tool does **not** fetch URLs itself (a static file can't authenticate to LinkedIn or crawl GitHub). The workflow is:

1. The builder pastes their **GitHub** and **LinkedIn** URLs (stored as display references).
2. An operator (Claude, or the builder) reads those profiles, extracts demonstrated domains and capabilities, and maps them to the controlled edge vocabulary.
3. Matching edges are toggled on; the brackets re-rank live.

**Worked example — github.com/sgharlow.** Top repos (`claude-code-recipes`, `AI-matcher-AWS-hackathon`, `ai-pr-bot`, `ai-control-framework`, `kiro-living-docs`) demonstrate agentic-AI tooling, document automation, AI matching, and rapid hackathon shipping. Detected edges default to: `doc-automation`, `matching`, `education`, `startups`. (LinkedIn is gated behind a login wall and must be supplied manually for industry-domain edges such as `fintech`, `healthcare`, `smb`.)

---

## 7. UI / interaction

- **Header:** hackathon facts (prize, deadlines, hard requirements).
- **Controls panel (three columns):**
  - Weight sliders for BV / AI / CI / Founder-Fit, plus the four strategy lenses (default 0), with reset buttons.
  - GitHub & LinkedIn URL fields; edge chips grouped into *Domains* and *Capabilities*; toggling re-ranks live.
  - **Focus-area tools:** 🎲 New draw, ▶ Simulate 1,000 draws, ±1-jitter checkbox, and a `draw #k · 16 of 32/cat` indicator.
- **Tabs:** one per category + an Overall Leaderboard tab.
- **Bracket view:** a "sitting out this draw" bench line, then five columns (R16 → Champion). Each idea card shows seed, name, pitch, the three sub-scores, total, and a `+fit` badge; an orange left-bar marks an idea matching an enabled edge; winning side is highlighted.
- **Overall Leaderboard tab:** focus-area verdict panel (simulator results, or a prompt to run it) → current draw's champion leaderboard → session draw log with category tally.
- **Edit modal:** click any idea to edit name, pitch, the three official scores, and the four lens scores, then re-run. Edits persist across draws. (Tag editing is a roadmap item.)
- **Live feedback:** a "↻ re-ranked" pulse confirms recomputation on every input change.
- **Footer:** source links (overview, rules, FAQ, Google Cloud free tier, Antigravity).

---

## 8. Architecture & constraints

- Pure client-side: HTML + CSS + vanilla JS in one file. No frameworks, no bundler, no network calls at runtime.
- **No browser storage** (`localStorage`/`sessionStorage` are not used) — all state is in-memory for the session.
- Deterministic **given a draw**: the same drawn 16 + same weights/edges/scores always yield the same ranking. Draw sampling and simulation jitter are intentionally random (v0.2 relaxation of the v0.1 full-determinism constraint).
- Hostable as-is on GitHub Pages; openable directly from the filesystem.

---

## 9. Roadmap

Delivered in v0.2 (formerly roadmap): randomized re-draws over a 2× idea pool, extra prioritization criteria (strategy lenses), and a sensitivity/robustness view (the jittered Monte Carlo verdict).

| Priority | Item |
|---|---|
| P1 | In-modal **tag editing** so ideas can be re-tagged without code edits. |
| P1 | **Add / remove / replace** ideas in the pool (currently fixed 32 per category). |
| P2 | **Live AI idea generation** — generate fresh challengers on demand and auto-score them (Gemini API). |
| P2 | **Export** the verdict or leaderboard to Markdown / CSV / PDF for a pitch packet. |
| P2 | **Shareable state** via URL hash (no storage) so a configured bracket can be sent to a teammate. |
| P3 | **Evidence fields** per idea (target customer, first-revenue hypothesis, required Google Cloud product) to pressure-test Business Viability. |

---

## 10. Acceptance criteria

- Exactly 32 ideas per category in the pool (160 total); every draw fields exactly 16 per category; all five brackets resolve to a single champion in four rounds.
- 🎲 New draw produces a different random sample (within sampling probability) and re-runs everything; idea edits persist across draws.
- Moving any weight slider (official or lens) or toggling any edge updates every total, re-seeds, and re-runs all brackets without reload.
- Editing an idea's scores (all seven) changes its seed and propagation correctly.
- Tie-breaks resolve in BV → AI → CI order.
- Setting Founder-Fit weight and all four lens weights to 0 reproduces the neutral judge-only ranking.
- ▶ Simulate completes ~1,000 draws near-instantly and renders the verdict panel; changing any input afterwards flags the verdict as stale.
- File opens and runs from disk with no console errors and no network access.
