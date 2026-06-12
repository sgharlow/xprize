# Concept v2: the combined entry — **Comeback**

> **Decisions locked 2026-06-11** (Steve): name **Comeback** (NextRound was burned — multiple shipping AI interview-prep products use it; Comeback scanned clean and comeback.careers is available, register it), category **Education & Human Potential**, wedge **support professionals displaced by AI**. Build repo: `../comeback` (github.com/sgharlow/comeback, private). Week-1 go/no-go **passed**: Gemini Live (`gemini-3.1-flash-live-preview`) median time-to-first-audio 727ms — full-duplex voice UX viable.

*Revised 2026-06-11 after a needs-first review and a live read of the official rules and FAQ (xprize.devpost.com). Supersedes v1 (git `6be02e5`).*

**Category: Education & Human Potential** ("Transforming how we learn, grow, and achieve our best") — see Category Decision below. Fallback: Entrepreneurship & Job Creation.

## One-liner

A voice-first AI career agent for workers displaced by AI — it finds your next role, builds the warm path to it, and trains you out loud to win it.

## Why this product

Three independent ranking rounds over 480 ideas converged on voice-first AI career tools (FINALISTS.md). A needs-first review of the displaced job hunter's journey then reshaped the bundle: the two needs that most determine outcomes — **direction** (is my occupation shrinking? where do my skills transfer?) and **relationships** (warm paths beat cold applications) — now lead the product, and the application machine is demoted to workhorse.

## Initial wedge: support professionals displaced by AI

Launch for **customer-support and call-center professionals displaced by AI support bots** — then generalize.

- Largest, most identifiable AI-displaced cohort of 2025–26; layoffs are public and concentrated.
- They are voice-skilled by profession — perfect fit for a voice-first product.
- Their pivot map is well-defined: CX ops, AI-bot supervision and QA, conversation design, implementation/success roles — jobs that *exist because of* the same AI wave.
- Reachable in communities (support Slacks/Discords, r/sysadmin-adjacent CX forums, LinkedIn layoff cohorts) without predatory outreach.
- The narrative writes itself: *the people AI bots displaced become the people who run the bots.*

## The product set (in journey order)

1. **Day-One Triage** (free) — severance/COBRA/unemployment/runway checklist with state-specific guidance. Trust wedge; explicitly informational, not legal or financial advice.
2. **Pivot Map** (paid report, first dollar) — honest occupation outlook, transferable-skill graph, 3–4 target roles with live demand and salary data, and a gap plan built on evidence projects rather than course-collecting.
3. **Connection Engine** — consent-based network mapping (LinkedIn export upload, no scraping), warm-path detection into target companies, outreach drafting, and **voice rehearsal of the networking call** plus a pre-call voice briefing.
4. **Search Agent** — targeted applications with draft + one-tap approve (human always sends), recruiter follow-ups, pipeline tracking. Quality targeting, never spam.
5. **The Gym** — job-specific voice mock interviews generated from the real JD and company, rubric-calibrated scoring on every session, negotiation sparring at offer stage against a counterpart that pushes back, communication drills between events, and a daily 3-minute voice standup for cadence and morale.
6. **The closed loop** — gym scores and interview outcomes re-weight where the agent spends effort, *across channels* (warm paths vs. applications) and across role targets; market data re-tunes the Pivot Map. Every reallocation is written to a **visible decision log** ("why I shifted your effort this week").
7. **Landing Pack** (expansion) — offer review, negotiation rehearsal, first-90-days ramp coaching.

## The business itself runs on agents

The rules judge whether teams "run their business through AI… live in production and executes key decisions" — the *business*, not just the product. Rebound's own operations are agent-run and logged from day one:

- **Support agent** answers users (voice and chat) with escalation to Steve.
- **Onboarding agent** runs activation sequences and win-back.
- **Content agent** produces the SEO/community answer-content pipeline (reviewed before publish).
- **Ops dashboard** records every agent decision — which doubles as the **production evidence the submission requires** (agent logs, API records, screenshots).

## Hard-requirement mapping (verified against rules)

- **Gemini API in deployed app**: Gemini Live (native audio) for mocks, negotiation sparring, standups; Gemini for JD analysis, pivot mapping, rubric scoring, outreach drafting.
- **≥1 Google Cloud product**: Cloud Run (app + agents), Firestore (pipeline, scores, decision log), Cloud Tasks/Scheduler (follow-ups, monitors). Free credits available per rules.
- **Newly created after May 19, 2026**: clean — work starts June 2026. Pre-existing personal tooling may be reused if disclosed (rules permit with explanation).
- **Solo entrant**: explicitly allowed.

## Revenue plan (judged on arms-length, third-party revenue with monthly breakdown)

- **Design for a growth curve, not a spike**: rules require revenue broken down by month (May–Aug). Subscriptions beat one-off packs for the sustainability assessment — lead with a **founding-member subscription** ($39–59/mo) plus the **Pivot Map as a $49–79 one-time entry purchase**.
- **Flat pricing only — no success fees.** Fee-for-placement structures can trigger state employment-agency licensing; Rebound is software + coaching, never a placement agency.
- **Arms-length discipline**: related-party revenue must be disclosed separately and judges discount it — no friends-and-family padding; every customer through the public funnel.
- **Outplacement B2B**: a real opportunity (incumbents charge $1,500–3,000/head and are weakest at direction + networking) but enterprise cycles won't close by Aug 17. Goal: **one signed pilot or LOI** as trajectory evidence; consumer revenue carries the submission.
- **Scholarship seats** funded from margin for unemployment-only users — pricing empathy that doubles as Category Impact evidence.

## Compliance & trust by design

- Candidate-side only: rubric scores are private coaching feedback, never shared with employers — keeps Rebound clearly outside employer-side automated-employment-decision rules (e.g., NYC LL144) and away from hiring-bias liability.
- Consent-first data: user-uploaded exports only; no scraping; voice recordings deletable, never used for training; clear retention policy.
- Severance/benefits guidance is informational with attorney/advisor referral points.
- Accent and dialect fairness: scoring rubrics target content and structure, with delivery feedback framed as coaching choices, not penalties.

## 9.5-week build plan (June 11 → submit Aug 14, deadline Aug 17 1:00 pm PT)

- **Wk 1–2 (Jun 11–24)**: De-risk Gemini Live voice quality first (fallback: turn-based push-to-talk). Ship Pivot Map report + Stripe + landing page; founding-member pricing live. First arms-length dollars in June.
- **Wk 3–4 (Jun 25–Jul 8)**: Gym v1 — job-specific voice mock with rubric scoring; daily standup; free Triage funnel; analytics instrumented (activation, conversion, outcomes).
- **Wk 5–6 (Jul 9–22)**: Connection Engine v0 (export upload → warm paths → outreach drafts → call rehearsal); Search Agent draft+approve.
- **Wk 7–8 (Jul 23–Aug 5)**: Closed-loop channel allocation + visible decision log; company-ops agents live and logging; outplacement pilot conversations off community partnerships (not cold WARN outreach).
- **Wk 9 (Aug 6–14)**: Metrics hardening, testimonials, <3-minute video, Devpost writeup with revenue/user/production evidence, **submit Aug 14** (3-day buffer).

## Metrics that go in the submission

- Activation: completed Pivot Maps. Conversion: paid users, MRR by month (Jun/Jul/Aug curve). Outcomes: interviews booked per active user, mock-score improvement, offers, negotiated-dollar delta, placements. Testimonials collected continuously from week 2.

## Video storyboard (<3 min, per rules)

1. The wedge story: a support pro displaced by a bot (15s) → 2. Pivot Map reveal from a real resume (25s) → 3. Live voice mock for a real posting, scored (45s) → 4. Decision log: the agent reallocating effort to warm paths (20s) → 5. Revenue + outcomes dashboard (20s) → 6. User testimonial: "the bot took my job; this agent got me a better one" (20s) → 7. Architecture beat: Gemini Live + Cloud Run (10s).

## Category decision

The official category copy reads: *Education & Human Potential — "Transforming how we learn, grow, and achieve our best"* vs. *Entrepreneurship & Job Creation — "Fueling the tools that help new founders and economies thrive."* Rebound is a person-development product (coaching, practice, growth into a new role), which maps cleanly to **Education & Human Potential**; the E&JC copy centers *founders*. Stage One is pass/fail on category fit, so the cleaner read wins despite Education likely being the more crowded category. If the contract/fractional bridge-income lane becomes prominent, E&JC becomes defensible — revisit at submission time.

## Open items

- Name/trademark/domain check ("Rebound" is a working title; conflicts likely).
- Gemini Live voice latency test is the week-1 go/no-go gate.
- Pick the first three community partnerships for the support-pro wedge.
