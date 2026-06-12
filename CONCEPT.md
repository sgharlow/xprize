# Concept: the combined entry (working title: "Rebound")

*Drafted 2026-06-11 from the five bracket winners. Target: Build with Gemini XPRIZE, category **Entrepreneurship & Job Creation**.*

## One-liner

A voice-first AI career agent for workers displaced by AI: it **works your job search while it trains you to win it** — sourcing and tailoring applications on one side, drilling you for *the specific interviews it books* on the other.

## Why this is the entry

Five independent ranking rounds over 480 ideas kept selecting the same anatomy: voice-native coaching + an agent doing real career work + a consumer who pays at a high-stakes moment. The combination beats any single winner because the two halves feed each other (see "the loop"), and the AI-displacement framing turns the hackathon's own premise into the Category Impact story: *AI re-employing the people AI displaced.*

## What each finalist contributes

| Finalist | DNA carried into the combo |
|---|---|
| **JobHuntOS** | The agent operations core: gap narrative, per-posting resume tailoring, application prep, recruiter follow-ups, pipeline tracking. |
| **InterviewGym** | The revenue spike: voice mock interviews the night before a real one — highest willingness-to-pay moment in the journey. |
| **NegotiationGym** | The adversarial counterpart that fights back, deployed at offer stage; produces dollar-quantified wins for testimonials. |
| **IELTSSpeak** | Examiner-calibrated scoring: every voice session gets a rubric score, so users watch a number move (and pay per scored attempt). |
| **AccentSmith** | The continuous-use layer: speech clarity and communication drills between high-stakes events; retention between searches. |

## The loop (the moat, and the AI-Native Ops evidence)

1. **Agent loop** — the agent monitors postings (and public WARN layoff filings), tailors materials, preps applications for one-tap approval, chases recruiters, books interviews.
2. **Gym loop** — the moment an interview lands on the calendar, the gym generates a voice mock *for that exact job*: real JD, researched company, role-calibrated rubric. Offer arrives → negotiation rehearsal against an adversarial counterpart armed with market comps.
3. **Closed loop** — gym scores feed agent targeting: if the user scores 8s in ops-manager mocks and 5s in analyst mocks, the agent re-weights where it spends application effort. **An AI making a real operational decision from measured human performance** — the cleanest possible answer to the "AI executes key decisions" judging bar.

## Hard-requirement mapping (Stage One)

- **Gemini API**: Gemini Live for real-time voice mocks/negotiation; Gemini for JD analysis, resume tailoring, rubric scoring.
- **Google Cloud**: Cloud Run (agent pipeline), Firestore (pipeline + scores), Cloud Tasks/Scheduler (follow-ups, posting monitors).
- **Operates with AI agents**: the agent runs sourcing, tailoring, scheduling, coaching, scoring, and targeting reallocation.
- **Newly created business, real users, real revenue**: see revenue engines below.

## Two revenue engines (Business Viability)

1. **Consumer (day-1 revenue)**: free voice assessment → interview prep pack ($29–49, sellable from week 2 of the build), active-search subscription ($39–79/mo), negotiation rehearsal at offer ($99 or success-based). Price umbrella: human career coaches charge $150–300/hr.
2. **Outplacement flip (bulk revenue)**: employers running layoffs buy outplacement at $1,500–3,000/head today for a portal and a few calls. An AI outplacement seat at $300–500/head undercuts 5–10× and delivers more; one mid-size layoff contract = hundreds of real users and real revenue in a single sale. WARN filings are public — the agent can find its own B2B pipeline.

## Distribution wedge

Layoff cohorts are public, concentrated, and self-organizing: WARN notices, r/layoffs, LinkedIn #opentowork waves, alumni Slack groups of affected companies. The displaced-by-AI framing earns press and community sharing that generic job tools can't.

## Flanking features from the pools (fold in, don't build first)

- **JobLossRunway** (v3): week-one triage — severance/COBRA/unemployment checklist — as free onboarding that builds trust.
- **CareerSwitch Map / SkillProof** (v1): reskill-toward-AI-adjacent-roles plan + work-history-to-narrative, for users whose old role isn't coming back.
- **OfferLetter Counsel / SeveranceMax** (v3): document-review premium add-ons at the bookends (severance in, offer out).
- **ManagerSchool First90 / SalesRampAI** (v3): post-placement ramp coaching — expansion revenue after the win.
- **ESLWorkTalk** (v1): non-native-speaker drills; widens the wedge internationally.

## Risks and mitigations

1. **Auto-apply is TOS-gray** on LinkedIn/Indeed and reads as spam to employers. Mitigation: the agent *prepares*, the human one-tap *approves and sends*; prioritize direct ATS portals and email. Targeting quality over volume is also the better story for judges.
2. **Crowded space** (auto-apply tools, AI interview prep exist). Differentiation: voice-first end-to-end, the closed pipeline↔practice loop, the displaced-worker wedge, and instrumented outcomes (interview-pass-rate lift, negotiated-dollar delta, placements).
3. **Solo scope creep**: MVP is three things only — pipeline agent (draft + approve), job-specific voice mock, offer-stage negotiation rehearsal. Everything else is a drill inside the gym or a later flank.

## 90-day shape (submission window closes Aug 17, 2026)

- **Weeks 1–3**: voice mock interview + scored assessment funnel; start charging prep packs.
- **Weeks 3–6**: pipeline agent (monitor, tailor, draft, approve-to-send); subscription on.
- **Weeks 6–10**: negotiation module; first outplacement pilot outreach off WARN lists; instrument every outcome.
- **Weeks 10–13**: scale what's converting; cut what isn't; build the submission demo around a live voice session plus the agent working a real pipeline.
