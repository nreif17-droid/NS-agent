# Nervous System Regulation App — Complete Build Prompt

**Paste this entire document into your AI app-building tool (Lovable, v0, bolt.new, Replit Agent, Cursor, etc.) as the initial build directive.** It is fully self-contained — the protocol library, evidence citations, and safety rules are inlined below, not referenced externally, so this works even if the tool can't browse a git repo. If you can also attach the source repo, the fuller versions live in `/knowledge/*.md`, `product/DIRECTIVE.md`, and `product/BUILD-PROMPT.md` — this file is the condensed, ready-to-execute synthesis of all three.

---

## 1. What to build

A web app that helps someone regulate their nervous system in the moment and over time, through regular check-ins. A user describes their current state (or responds to a proactive reminder); the app matches it to evidence-based regulation techniques, gives a short-term plan, and logs the interaction so future recommendations get personalized — surfacing what this specific person actually struggles with and which tools actually work for them, not just generic advice repeated every session.

**Positioning: help people regain control and stay functional/productive through their day** — not just "feel calmer" in the abstract. Every technique shown is labeled with its evidence strength (🟢 strong / 🟡 moderate / 🔴 weak-fringe) and linked to its source. **This evidence-transparency is the core product differentiator** — most wellness apps oversell shaky mechanisms, most clinical apps require infrastructure this project doesn't have; this sits in the honest middle. Lead marketing and product copy with it, don't bury it in a footer.

**Build scope: content/education pages + a personal check-in app with logging and proactive reminders, single-tenant or small-multi-tenant.** Do NOT build real-time crisis intervention, clinical/diagnostic claims, or general-public-scale trust/safety infrastructure — flag and ask rather than build if a request pushes toward that.

## 2. Target audience, in priority order

1. **People tapering nicotine or cannabis who also want general nervous-system support.** This is the most validated, most differentiated segment — the intersection of "quit-habit app" and "nervous-system app" is underserved. The app can support this directly.
2. **General day-to-day equilibrium/stress maintenance** — broadest market, most competition; differentiate on evidence-transparency and personalization, not on being another generic breathing app.
3. **People managing anxiety or depression as an ongoing condition**, using this as a day-to-day companion — higher proximity to safety-flag content than segment 2; onboarding must be explicit this is not therapy or a crisis line.
4. **Acute panic-attack in-the-moment support** — the tooling (grounding, physiological sigh) already works for this, but don't lead marketing here; it's the highest-stakes segment.

**Alcohol is explicitly NOT the same product as nicotine/cannabis tapering.** See §6 — any drinking-related feature requires a hard screening gate before it touches the rest of the audience list above.

## 3. Core user flow

1. **Intake** — free text ("what's going on right now") + intensity (1–5) + duration (acute/hours/days/weeks+) + optional context. Conversational, low-friction — texting a knowledgeable friend, not a medical form.
2. **Safety screen** (server-side, every submission, no exceptions — see §7). Runs before anything else.
3. **Response, always three parts:**
   - *Right now* — 2–4 matched techniques from the library in §5, each showing its evidence badge and source inline
   - *This week* — a short plan referencing situational protocols
   - *Long-term* — where this fits in the user's own history; say "not enough history yet" honestly for new users
4. **Log write** — every check-in saved (including safety-flagged ones, without a generated protocol).
5. **Passive follow-up** — next check-in opens by asking how the most recent unresolved logged recommendation actually landed.
6. **Proactive check-ins** — scheduled, user-configurable reminders ("how are you doing?"). Opt-in, user controls cadence/channel — unwanted notifications are themselves a stressor, don't default to aggressive.
7. **Pattern-flagging, surfaced visibly** — once enough entries exist, actively tell the user what's been noticed (recurring cause, time-of-day pattern, trigger category) in a real "insights" view, not just silent logging.
8. **Outcome-ranked personalization** — once real outcome data exists for a user, weight recommendations toward what's actually worked for *them* before, ahead of generic defaults. Don't label anything "personalized" before there's real data behind it.
9. **History view** — browsable past entries and the user's own emerging baseline.

## 4. Content data model

Each technique/protocol is a structured object:
```
{
  id, name, tier: generic|basic|advanced,
  what_it_is, how_to,
  evidence_rating: green|yellow|red,
  evidence_summary,
  source_citations: [{title, url}],
  situational_tags: [...],
  contraindications: [...]
}
```

## 5. The protocol library (inlined, evidence-preserved exactly — do not re-rate or re-word these ratings)

### Generic tier — safe for anyone, no screening needed
- **Physiological sigh** 🟢 — two inhales through the nose (second short, "topping off"), one long slow exhale through the mouth. 1–3 cycles for acute use. Fastest-acting acute down-regulator per Stanford RCT (Balban/Huberman/Spiegel 2023, Cell Reports Medicine, n=108) — beat box breathing and meditation on mood/respiratory-rate. Mechanism: extended exhale increases vagal outflow via respiratory sinus arrhythmia. Tags: acute-stress, panic. [Study](https://www.psychologytoday.com/us/blog/the-athletes-way/202301/how-longer-exhalations-and-cyclic-sighing-make-us-feel-good)
- **Box breathing (4-4-4-4)** 🟢 — inhale 4s, hold 4s, exhale 4s, hold 4s, 2–5 min. Solid, well-studied, slightly underperforms physiological sigh head-to-head. Tags: general-calm, focus-prep, pre-performance.
- **Sensory/attentional grounding** 🟡 — 5-4-3-2-1 (name 5 seen/4 felt/3 heard) or feet-on-floor. Standard clinical technique, works via attentional redirection away from threat-appraisal, not electrical mechanism. Tags: acute-distress, racing-thoughts. **Explicitly exclude "earthing"/electron-transfer grounding claims** 🔴 — pseudoscience-adjacent, no established mechanism, do not include in the app.
- **Posture reset** 🟡 — upright, open-chest posture. Plausible mechanical link via breath mechanics, thin direct evidence. Complementary only, not a primary recommendation.
- **Hydration check** 🟡 — baseline hygiene, not an active regulation technique on its own.

### Basic tier — situational sequences (bundle logic is original synthesis; components above are the cited parts)
- **Acute stress spike**: physiological sigh (1–3x) → grounding if thoughts still racing → box breathing 2–5 min to consolidate.
- **Pre-sleep wind-down**: reduce screens/blue light 30–60min before bed (🟢 mechanism — blue light suppresses melatonin via ipRGCs/SCN, delays sleep onset; 🟡 the *intervention* i.e. blue-light glasses evidence is mixed, don't oversell) → slow-paced extended-exhale breathing 5–10 min → no hard training/cold exposure in this window.
- **Morning activation**: light exposure, ideally outdoor/blue-enriched (🟢 — enhances cortisol awakening response) → brief inhale-emphasized paced breathing for alertness (🟡 — real but smaller evidence base than calming techniques; NOT hyperventilation-based, keep it brief and gentle) → hydration + protein-forward first meal for blood-sugar stability.
- **Post-conflict reset**: physiological sigh/extended-exhale first (arousal must come down before cognitive processing — PFC is measurably impaired under sympathetic/HPA activation 🟢) → delay decisions ~20 min → optional brief walk.
- **Focus/deep work prep**: confirm baseline arousal is adequate (if not, run acute-stress-spike first) → box breathing 2–3 min → environmental control (not sourced from NS literature, standard practice).

### Advanced tier — for regulated baseline only, NOT for acute distress; screen contraindications first
- **Cold exposure** 🟢 mechanism / 🟡 downstream benefit — potent sympathetic activator (norepinephrine can rise 2–4x within minutes). Hormetic, not calming — wrong tool for acute distress. Contraindication: cardiovascular conditions.
- **Sauna/heat** 🟢 — strongest evidence in this tier: Finnish cohort (n=2,315, 20.7yr follow-up) found 4–7x/week use associated with 48% lower fatal CVD risk, 40% lower all-cause mortality vs. 1x/week (observational, not RCT). Contraindication: cardiovascular conditions.
- **Breath-hold / cyclic hyperventilation (Wim Hof-style)** 🟡 evidence, but **hard safety rule, always included, non-negotiable**: hyperventilation before breath-holding causes shallow water blackout (delayed urge-to-breathe while O2 drops) — has killed experienced swimmers/divers including WHM practitioners specifically. **Never in or near water, never alone, not while lightheaded/panic-prone.** US Navy/USA Swimming/Red Cross have issued formal warnings against this practice near water. Contraindication: panic disorder.
- **HRV resonance-frequency breathing biofeedback** 🟢 — most rigorously studied advanced protocol (Lehrer et al. meta-analytic support, small-to-medium effects). Needs individualized resonance frequency (~4.5–6.5 breaths/min, varies by person), not a fixed default.
- **Fasted-state training** 🟡 — genuinely mixed evidence, duration-dependent (short fasts can *increase* sympathetic activity; only longer fasts trend parasympathetic). Not reliably "calming" — don't market it that way. Contraindication: disordered-eating history, hard screen required.

### Craving/urge management (distinct mechanism from the above — reward/cue-learning circuitry, not primarily ANS/HPA)
- **Urge surfing** 🟢 — observe the craving as a physical sensation that rises, peaks, and falls, without acting on it (Marlatt & Gordon). RCT evidence: reduces subsequent *use*, not necessarily reported urge intensity — state this distinction honestly, don't oversell "makes cravings go away."
- **Time-course**: don't promise a specific countdown ("cravings last 5 minutes") — that specific claim isn't well-sourced. What's real: craving rises and falls rather than staying at peak; EMA data shows real hour-scale dynamics.
- **Incubation of craving** 🟢 — for some substances, cue-triggered craving can *increase* over the first weeks-to-months of abstinence before fading. Worth stating in-product so an upswing isn't mistaken for failure.
- **Sleep-relapse link (cannabis-specific)** 🟢 — cannabis withdrawal reliably disrupts sleep; 65% of surveyed users reported sleep difficulty contributed to a past relapse. If a user's intake mentions cannabis tapering + poor sleep, prioritize sleep-focused suggestions.

## 6. Alcohol — mandatory screening gate, separate from everything else

**Alcohol/benzodiazepine withdrawal can be fatal. This is not the same risk category as nicotine/cannabis and must never be treated the same way in copy or features.** Delirium tremens carries 1–4% mortality even treated, up to 15% untreated; withdrawal seizure risk peaks 12–24h post-last-drink (~3% of cases); DTs typically develop 48–96h post-last-drink (~5% of cases). The single strongest risk predictor is a prior history of withdrawal seizures/DTs, stronger than volume of use alone.

**Required implementation, from the first version that mentions alcohol at all:**
- Implement **AUDIT-C** (validated 3-question screen: frequency of drinking, typical quantity, frequency of heavy-drinking episodes; scored 0–12; positive at **≥4 for men, ≥3 for women**) as an onboarding gate before showing any drinking-reduction content.
- Positive screen, OR self-reported heavy/daily use, OR any history of withdrawal seizures/DTs → hard route to "talk to a doctor before stopping or cutting back" — same visually-distinct treatment as the general safety-flag screen (§7), not a quiet checkbox.
- Below threshold, no risk factors → general nervous-system content is fine, same tier as any other lifestyle support.
- Never write marketing copy or feature framing implying this app can help someone stop drinking unsupervised if there's any dependence signal.

## 7. Safety screen — real infrastructure, not a single AI call

**Standing flag list** (any of these → redirect to a professional resource, no protocol generated): persistent physical symptoms, chest pain/acute-emergency presentations, suicidal ideation or self-harm content, dissociative symptoms/trauma disclosures beyond general stress, disordered-eating indicators, panic-disorder history (relevant to breath-hold features), cardiovascular conditions (relevant to cold/heat features), alcohol dependence indicators (§6).

**Implementation requirements:**
- Runs server-side, on every submission, before any protocol-matching logic — never client-side-only, never skippable.
- Hybrid: an LLM judgment pass against the flag list above, PLUS hardcoded fallback resources (crisis line, "find a therapist" links) that display regardless of the model's specific read. Never let one model call be the only thing between a real emergency and a redirect.
- Log every trigger (anonymized/aggregated) to review and improve the flag criteria over time — budget real engineering time for this, it's not "one prompt and done."
- Never claim a diagnosis anywhere in the product, at any stage.

## 8. Copywriting direction

**Voice:** calm, precise, non-clinical, not fluffy — a knowledgeable friend who actually checks sources, not a wellness influencer or a hospital pamphlet.

**Do:** state evidence quality plainly ("well-studied," "promising but thinner evidence," "heads up, this popular claim isn't well-sourced"); use real mechanism language (vagus nerve, cortisol) translated in the same breath, not dumbed down; write the safety redirect with warmth, not liability-speak.

**Don't:** imply diagnosis, treatment, or cure anywhere; borrow wellness-industry overconfidence; bury the safety redirect in fine print — it should be the clearest thing on screen when it triggers; lump alcohol in with nicotine/cannabis in "quit any habit" copy.

**Key pages:** landing/home, "how this works" (the three-layer design — research, static core, personal log — is a genuine differentiator, use it), a real browsable evidence/sources page (not a footer link), check-in flow, safety-flag screen, AUDIT-C gate screen, onboarding, insights/patterns view.

## 9. UI/UX direction

**Mood:** calm and low-friction first, credible second — the user may be dysregulated when they open this; the UI itself shouldn't add stimulation.

- Generous whitespace, muted/desaturated palette (reserve higher-clarity color specifically for the safety-flag state), minimal motion.
- Evidence badges (🟢/🟡/🔴 or redesigned equivalent) are first-class UI, same design weight as a price tag — not a tooltip.
- "Right now" response is immediately actionable above the fold — no scrolling required to find the thing that helps in the next 30 seconds.
- Intake feels conversational (chat-like or short guided form), not a clinical questionnaire.
- Safety-flag and AUDIT-C-gate screens: visually distinct from the calm palette, large tap targets for real resources, never a dead end — always a next action.
- Insights/patterns view: visible, not buried — this is a sellable feature (§3.7).

**Key screens:** landing/marketing, onboarding (explain non-diagnostic framing before first use), check-in intake, response display, safety-flag redirect, AUDIT-C gate, history/log, insights/patterns, evidence/sources browser, reminder settings, data export/delete settings.

## 10. Technical architecture

**Stack:** web app; server-rendered/hybrid for marketing/content pages (SEO matters); authenticated app shell for the check-in app. Structured content store for the protocol library (§5) — JSON/CMS layer is sufficient at this scale. Relational/document DB for accounts and logs.

**Log data model** (validated by real personal use, keep this shape):
```
LogEntry {
  id, user_id, timestamp,
  intake: { state, context, intensity, duration },
  mode: check-in | optimize,
  safety_flag: none | <concern>,
  protocols_used: [{ tier, name, source }],
  outcome: text | null
}
```

**Privacy/auth, required from the first real user, not deferred to "scale":**
- Encrypt log data at rest (sensitive personal health-adjacent data even at small scale)
- User-facing full data export and deletion, not just deactivation
- No third-party analytics/ad-tech touching log content
- Plain-language privacy policy (copywriting deliverable, not just legal)

**Explicitly out of scope:** real-time crisis-response guarantees, clinical/diagnostic claims, HIPAA-level infrastructure, multi-practitioner/coaching tooling. Override any template default that nudges toward these.

## 11. Non-negotiables

1. Never diagnose — no copy, UI state, or feature implies diagnosis or guaranteed outcome.
2. Safety screen is real infrastructure (§7) — never a single unguarded model call.
3. Every protocol shows its evidence rating and source inline — this is the product.
4. Privacy requirements apply from the first real user.
5. Alcohol gets its own AUDIT-C gate from day one — never equated with nicotine/cannabis.
6. If a requested feature would violate any of the above, flag it and ask rather than building it anyway.

## 12. Acceptance checklist

- [ ] Safety screen server-side, every submission, hardcoded fallback resources independent of the model call
- [ ] AUDIT-C gate implemented and blocking on positive screen/heavy-use/withdrawal history, if any alcohol feature exists
- [ ] Every displayed protocol shows an evidence badge and working source link
- [ ] Safety-flag and AUDIT-C screens are visually distinct, never a dead end
- [ ] No copy implies diagnosis, treatment, or guaranteed outcome
- [ ] Log data encrypted at rest; export and full deletion available
- [ ] No ad-tech/third-party analytics on log content
- [ ] Onboarding explains non-diagnostic framing before first check-in
- [ ] Proactive reminders are opt-in, user-configurable cadence
- [ ] "Personalized" recommendations only labeled as such once real outcome data backs them
- [ ] Breath-hold content includes the water/isolation safety rule unconditionally, every time it's shown

---

*Consolidated 2026-08-06 from the full project state: `/knowledge/` (10 files), `/ns-check-in`, real logged use, `product/DIRECTIVE.md`, and `product/BUILD-PROMPT.md`. This is the single paste-ready version — the other two files remain as the fuller strategy/architecture reference.*
