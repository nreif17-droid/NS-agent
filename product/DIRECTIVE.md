# Directive: Nervous System Regulation — Marketing & Product Build

**Purpose of this document:** a self-contained brief, meant to be handed to a marketer, designer, developer, or another AI session to actually execute against. It compiles what this project has learned building the personal-use version (`/knowledge/`, `/ns-check-in`, real logged use) into direction for turning the underlying principle into something other people can use — while carrying forward the guardrails that made the personal version trustworthy in the first place. **Read the Non-Negotiables section (§6) before executing anything else in here — it's not boilerplate, it's what keeps this legally and ethically viable at all.**

---

## 1. The core thesis

Nervous system regulation is a **learnable, physiologically real skill** — not a mystical practice and not a medical treatment. The evidence for this sits in a specific, defensible middle ground that most existing products don't occupy cleanly:

- **Wellness/spiritual positioning** (most yoga/breathwork apps, earthing mats, "energy" framing) oversells unverified mechanisms and underdelivers on rigor.
- **Clinical/medical positioning** (digital therapeutics, FDA-cleared apps) is accurate but requires infrastructure — clinical trials, regulatory clearance, liability structures — most wellness products don't have and can't fake.
- **The gap between them is real and underserved:** physiologically grounded, honestly evidence-rated, self-regulation education — accurate about what's solid (vagal breathing mechanisms, cold/heat hormesis, sleep-craving links) and equally honest about what's shaky (earthing, EMF, precise craving-duration claims) or contested (Polyvagal Theory's mechanism claims).

**The product thesis:** the credibility-flagging itself is the differentiator, not a compliance tax on top of the product. "We tell you what's 🟢 strong evidence versus 🟡 plausible versus 🔴 fringe, and we say when sources disagree" is a genuine trust signal in a category full of confident overclaiming. Lead with that, don't hide it.

## 2. What's actually been validated so far (the raw material)

Everything below lives in `/knowledge/` in this repo, fully cited — this is the summary for marketing/product use, not a replacement for reading the source files before building on top of them:

- **Foundations** (`foundations.md`): ANS/vagus/HPA physiology, what's solid (cholinergic anti-inflammatory pathway, HRV as a *trend* signal) vs. contested (Polyvagal Theory's specific mechanism claims — usable as vocabulary, not as validated neuroscience).
- **Causes/effects** (`dysregulation-causes.md`): chronic stress, poor sleep, trauma, isolation, blood sugar, and — closed via real personal use — nicotine/cannabis tapering effects specifically.
- **Protocols, tiered by readiness** (`protocols-generic/basic/advanced.md`): from universal-safe (breath, grounding) to situational (acute stress, wind-down, morning activation) to optimization-tier (cold/heat, HRV training, breath-hold — with a hard, non-negotiable safety rule on the last one).
- **Craving/urge management** (`craving-management.md`): a genuinely distinct mechanism from general stress regulation, discovered through actual use, not theorized in advance — this is a good example of why the personal-use phase mattered before generalizing.
- **Proof the loop works**: two real logged check-ins (`logs/self/`) showing the design assumption holding up — situational tools work for situational stress, but craving needed its own approach, discovered by using the thing, not by guessing upfront.

## 3. Target audience — lead with the underserved segment, not the biggest one

Four segments, in build priority order (smallest/most-validated first, not biggest/most-lucrative first):

1. **People actively regulating a taper or habit change — nicotine and cannabis specifically supported directly; alcohol supported only alongside medical care, never as the primary quitting tool.** This is the segment this project has actual validated evidence for — real logged use, a real discovered gap (craving ≠ dysregulation), a real mechanism connecting sleep to relapse risk. Most quit-smoking/quit-weed apps are narrowly habit-tracking; most nervous-system apps ignore substance-tapering entirely. The intersection is underserved and this project already has a head start there. **Alcohol is explicitly not the same product as nicotine/cannabis tapering** — see the hard gate in §6 point 6 and `knowledge/substance-safety-screening.md` before building or marketing anything drinking-related. "Redirecting a lifestyle" copy that lumps alcohol in with nicotine/cannabis as equally self-manageable is the single most likely way this product could cause real harm — don't write it that way.
2. **General day-to-day equilibrium maintenance** — people managing everyday stress or anxiety who want a check-in habit, not a crisis tool. This is the broadest, least differentiated segment (Calm/Headspace territory) unless the evidence-transparency angle (§1) and the personalization mechanic (§3a) actually land as distinct value.
3. **People managing anxiety or depression as an ongoing condition, using this as a day-to-day companion tool** — higher-stakes than segment 2 in expectation (more frequent proximity to safety-flag-triggering content, e.g., mood-related language bordering on the suicidal-ideation flag), but appropriate to serve *as a self-regulation companion alongside their own care*, not as a replacement for it. Onboarding copy for this segment needs to be explicit that the app is not therapy and not a crisis line.
4. **Acute panic-attack in-the-moment support** — the segment named in the original conversation that raised the biggest scope/liability questions (see §6). Don't build product for this segment first; the tooling (grounding, physiological sigh) is already validated for it, but the audience is higher-stakes and needs the safety infrastructure in §6 solved before going here.

### 3a. Core mechanic: personalization from real usage data — this is a feature, name it as one

The product isn't a static technique library — its value compounds with use, the same way the personal `/ns-check-in` log already does. Three specific mechanics to build toward, in this priority order:

- **Proactive check-ins.** Scheduled reminders that ask "how are you doing?" rather than waiting for the user to open the app in crisis — this turns the tool into a maintenance habit, not just an emergency button. Frequency/timing should be user-configurable, not a fixed push-notification blast — unwanted notifications are themselves a small stressor, which would undercut the entire premise.
- **Pattern-flagging.** Same mechanic as `regularization-procedure.md` Stage 3 (recurring-cause escalation) and `craving-management.md`'s discovered gap — the app should surface, explicitly and visibly to the user, what they seem to actually struggle with over time (a recurring cause, a time-of-day pattern, a specific trigger category), not just log entries silently. "Here's what we've noticed" is a real, sellable feature, not just a data-science backend detail.
- **Outcome-ranked tool matching.** Once enough logged outcomes exist for a given user, prioritize recommending *what has actually worked for them before* in similar states, over the generic default ordering — this is the personalization payoff that makes long-term use valuable instead of just repeating the same generic advice every session. Needs real outcome data to work (see `logs/README.md`'s outcome-capture design) — don't fake this with a generic "recommended for you" label before there's real data behind it.

The framing for all of this, in marketing terms: **helping someone regain control and stay functional/productive through their day**, not just "feel calmer" in the abstract — that's a more concrete, differentiated promise than most wellness-app copy makes, and it's actually what the underlying protocols are built for (situational tools matched to acute vs. ongoing states, per `regularization-procedure.md`).

## 4. Staged build — same roadmap already committed to, restated for product/marketing use

This isn't new — see `README.md` § Roadmap for the original version. Restated for this document's audience:

**Stage A — Content/education product.** Newsletter, structured course, or a lightweight web tool built directly from `/knowledge/`'s existing cited content. No real-time in-the-moment claims, no health-data storage at scale. This is buildable *now* — it's mostly a packaging and design exercise on material that already exists and is already sourced.

**Stage B — Personal-use app with logging**, i.e., productized version of `/ns-check-in`. Check-in flow, protocol matching, personal log/baseline over time. This is where user accounts, data storage, and privacy obligations start being real (see §6) — still short of clinical claims, still positioned as educational.

**Stage C — Practitioner/coaching layer.** Multi-user support for professionals (the paused Sedona coaching practice is the natural first real test of this, per the original roadmap) — a human stays in the loop making the actual calls, the software supports rather than replaces judgment.

**Stage D — General-population, real-time, higher-stakes use** (panic attacks, ongoing anxiety management at scale). Explicitly gated behind the infrastructure in §6. Do not market toward this stage's promise before that infrastructure exists — that's the single biggest risk-of-overreach in this whole plan.

## 5. Marketing messaging pillars

Draft positioning lines, meant as a starting point for actual copywriting, not final copy:

- **"We show our work."** Every claim is evidence-rated (🟢/🟡/🔴), sources are linked, disagreements between studies are stated instead of picked-and-hidden. Positioned as the opposite of typical wellness-app confidence.
- **"Built by using it, not just theorizing it."** The craving-management gap (§2) is a real story: a real check-in surfaced a real gap in the knowledge base, which got researched and closed within a day. That's a genuinely differentiated origin story versus "we hired consultants to write content."
- **"Educational, not medical — and that's a feature, not a disclaimer."** Reframe the non-diagnostic stance as trustworthiness (we tell you clearly when something needs a doctor instead of pretending we can handle it) rather than a legal footnote buried at the bottom.
- **Avoid, explicitly:** any language implying diagnosis, treatment, or cure; any claim not traceable to a 🟢 or clearly-labeled 🟡 source in `/knowledge/`; borrowing the confident tone of the wellness-app category this product is trying to differentiate from.

## 6. Non-negotiables (carried forward, not optional at any stage)

These are restated from `README.md`'s hard safety rule and Roadmap section — repeating them here because this document may get separated from that context when handed to someone else:

1. **Never diagnose.** This holds at every stage, and must get *stricter*, not looser, as the audience grows and gets less known to the product team.
2. **Safety escalation must be real infrastructure before Stage D**, not "the AI's judgment reading free text" — that's fine for personal use where the builder knows the context; it is not sufficient for a stranger's genuine emergency at scale.
3. **Health-data privacy obligations are real starting at Stage B**, not just Stage D — personal logs about panic attacks, substance tapering, or mental health are sensitive data the moment a second real user exists, not just at "general population" scale.
4. **Liability shifts the moment a professional isn't in the loop.** Stage C keeps a human decision-maker; Stage D doesn't, by definition — that transition needs legal/clinical infrastructure this project has not built and should not assume into existence.
5. **Don't let marketing outrun the evidence.** If a claim isn't in `/knowledge/` with a citation, it doesn't go in marketing copy, full stop — this is the actual product differentiator (§1) and it dies the first time it's violated for a punchier headline.
6. **Alcohol is gated separately from nicotine/cannabis, at every stage, starting now — not a Stage D concern.** Per `knowledge/substance-safety-screening.md`: any drinking-reduction feature or copy requires an AUDIT-C (or equivalent validated screen) before onboarding into self-regulation content. A positive screen, heavy/daily use, or any history of withdrawal seizures/delirium tremens routes to "talk to a doctor" — hard stop, not a soft suggestion. Delirium tremens carries 1–4% mortality even treated. This is not a legal caveat to soften for a marketing campaign; it's the difference between a supportive wellness tool and a product that could plausibly contribute to someone attempting a dangerous unsupervised detox.

## 7. How to use this document

Hand this whole file, plus `/knowledge/` and `README.md`, to whoever's executing next — designer, marketer, developer, or another AI session. It's written to be actionable on its own, but §6 is load-bearing: anyone building or marketing off this without reading it first is the failure mode this document exists to prevent.

**For actually building the app:** see `product/BUILD-PROMPT.md` — a self-contained execution prompt for copywriting, UI/design, and technical architecture, meant to be pasted directly into an AI app-building tool (Lovable, v0, bolt.new, Replit Agent, etc.). This document is the strategy; that one is the build directive.

*Compiled 2026-08-06 from the full project state at that point: `/knowledge/` (8 files), `/ns-check-in`, and two real logged check-ins.*
