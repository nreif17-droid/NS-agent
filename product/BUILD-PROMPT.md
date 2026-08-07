# Build Prompt: Nervous System Regulation App

**How to use this file:** paste this whole document into an AI app-building tool (Lovable, v0, bolt.new, Replit Agent, Cursor, or similar) as the initial build directive. It's self-contained — written so it doesn't require the rest of this repo to be readable by the tool, though `/knowledge/*.md` should be provided as source content wherever this prompt says to pull from it. Read `product/DIRECTIVE.md` first if you're the one operating the builder tool — this file is the *execution* prompt; that one is the *strategy* brief.

---

## 0. What you're building, in one paragraph

A web app that helps someone regulate their nervous system in the moment and over time. A user describes their current state in plain language; the app matches it to evidence-based regulation techniques (breathing, grounding, situational protocols), shows a short-term plan, and logs the interaction so future recommendations get more personalized. Every technique shown is labeled with its evidence strength and linked to its source — that transparency is the product's core differentiator, not a footnote. The app is explicitly educational/self-regulation, never diagnostic, and hands off to a real safety resource the moment an input suggests it should.

**Build scope for this pass: Stage A (content/education) + Stage B (personal check-in app with logging), single-tenant or small-multi-tenant.** Do NOT build for real-time crisis intervention, clinical claims, diagnostic features, or general-public scale trust/safety infrastructure — that's explicitly out of scope (see §5, Non-Negotiables). If asked to extend scope beyond this, stop and flag it rather than building it.

## 1. Content source and structure

Source content lives in `/knowledge/*.md` (provide these files to the builder tool alongside this prompt):
- `foundations.md`, `dysregulation-causes.md`, `craving-management.md` — background/reference content
- `protocols-generic.md`, `protocols-basic.md`, `protocols-advanced.md` — the actual technique library
- `regularization-procedure.md`, `optimization-procedure.md` — the sequencing logic behind recommendations

**Data model these files should become:** each protocol/technique is a structured content object, not just prose:
```
{
  id, name, tier: generic|basic|advanced,
  what_it_is: short description,
  how_to: step-by-step instructions,
  evidence_rating: green|yellow|red,
  evidence_summary: 1-2 sentence plain-language summary of the actual evidence,
  source_citations: [{title, url}],
  situational_tags: [acute-stress, wind-down, morning, craving, ...],
  contraindications: [...] // e.g. cardiovascular for cold exposure, panic-disorder for breath-hold
}
```
Parse this structure out of the markdown files' existing citations and evidence-quality (🟢/🟡/🔴) markers — don't re-derive evidence ratings, they're already assigned; preserve them exactly.

## 2. Core user flow (Stage B — the check-in app)

1. **Intake.** Short free-text entry ("what's going on right now") plus lightweight structured fields: intensity (1–5), duration (acute/hours/days/weeks+), optional context. Low-friction — this should feel like texting a knowledgeable friend, not filling out a medical intake form.
2. **Safety screen (server-side, every submission, no exceptions).** See §5 — this has to run before any protocol matching happens, on every single submission, with no way to bypass it from the client.
3. **Response**, always three parts, matching the existing design (`regularization-procedure.md`):
   - *Right now* — 2–4 matched techniques, each showing its evidence badge and source link inline (not hidden behind a click)
   - *This week* — a short plan, referencing situational protocols
   - *Long-term* — where this fits in the user's own history, honestly noting "not enough history yet" for new users rather than fabricating a trend
4. **Log write.** Every check-in (including safety-flagged ones, minus generated protocol) is saved to the user's history.
5. **Passive follow-up.** Next check-in opens by asking about the most recent unresolved logged outcome, same as the CLI version — this is a core mechanic, not a nice-to-have.
6. **History view.** User can browse past entries, see patterns the app has surfaced (recurring causes, frequency), and see their own "personal baseline" once enough entries exist.
7. **Proactive check-ins (new for the app, not in the CLI version).** Scheduled reminders ("how are you doing?") on a user-configurable cadence — this is what turns the tool into a maintenance habit rather than a crisis-only button. User controls frequency/timing/channel (push, email); default to opt-in, not a pre-set aggressive cadence — unwanted notifications are themselves a small stressor, which directly undercuts the product's purpose.
8. **Pattern-flagging, surfaced visibly.** Once enough entries exist, the app should actively tell the user what it's noticed — a recurring cause, a time-of-day pattern, a specific trigger category — not just passively store it for them to infer. This is the same mechanic as `regularization-procedure.md` Stage 3, made visible as a real feature (e.g., a "patterns" or "insights" view), not just a backend log.
9. **Outcome-ranked tool matching.** Once a user has enough logged outcomes, weight recommendations toward what has *actually worked for them* in similar past states, ahead of the generic default ordering. Requires real outcome data — don't fake a "personalized for you" label before there's data behind it; show the generic evidence-based default until personalization has something real to work with, and say so.

## 3. Copywriting direction

**Voice:** calm, precise, non-clinical but not fluffy. Talk to the user like a knowledgeable friend who happens to actually check their sources — not like a wellness influencer, not like a hospital pamphlet.

**Do:**
- State evidence quality plainly in-product ("this one's well-studied," "this one's promising but the evidence is thinner," "heads up — this specific claim is popular but not well-sourced")
- Use the actual mechanism language from `/knowledge/` where it's genuinely clarifying (vagus nerve, cortisol, HPA axis) — don't dumb it down to the point of losing accuracy, but always translate jargon in the same breath
- Write the safety-flag redirect with warmth, not liability-speak — "this sounds like something worth talking to a doctor or therapist about, not something this app should try to handle" rather than a legal disclaimer wall

**Don't:**
- Any language implying diagnosis, treatment, cure, or "this will fix X"
- Borrow wellness-industry confidence ("this ancient technique guarantees...") — if it's not cited in `/knowledge/`, it doesn't go in copy
- Bury the safety-flag redirect in fine print — it should be the most visually clear thing on the screen when it triggers

**Key pages needing copy:** landing/home, "how this works" (explain the three knowledge layers — research, static core, personal log — this is a genuinely interesting differentiator, use it), an "evidence" or "sources" page that's actually browsable (this is the trust-building payoff of the whole credibility-flagging system — don't hide it in a footer link), the check-in flow itself, the safety-flag redirect screen, onboarding.

## 4. UI/UX design direction

**Mood:** calm and low-friction first, credible second, never clinical-cold or wellness-saccharine. The person using this may be dysregulated when they open it — the interface itself shouldn't add friction or stimulation.

**Concrete direction:**
- Generous whitespace, muted/desaturated palette (avoid high-saturation "alarm" colors except specifically for the safety-flag state, where clarity matters more than calm), slow/minimal motion — a racing-thoughts user should not be fighting a busy UI
- Evidence badges (🟢/🟡/🔴 or a redesigned equivalent) need to be a first-class visual element, not a tooltip — this is the product's actual differentiator, treat it with the same design weight as a price tag on an e-commerce site
- The three-part response (Right now / This week / Long-term) should be visually distinct sections, with "Right now" immediately actionable above the fold — someone in an acute state shouldn't have to scroll to find the thing that helps in the next 30 seconds
- Check-in intake should feel conversational (chat-like or a short guided form), not a clinical questionnaire grid
- Safety-flag screen: distinct visual treatment (not the calm palette — this one moment should read as clearly different), large tap targets for actual resources (crisis line, "find a therapist"), no dead-end — always give the user a next action, never just a wall of text

**Key screens to design:** landing/marketing home, onboarding (first-run explanation of the non-diagnostic framing — this needs to land before first use, not after), check-in intake, response display, safety-flag redirect, history/log view, evidence/sources browser, settings (data export/delete — see §5).

## 5. Technical architecture

**Suggested stack** (adjust to the builder tool's defaults, this isn't prescriptive): web app, server-rendered or hybrid for the content/marketing pages (SEO matters for Stage A), authenticated app shell for Stage B. Structured content store for the protocol library (§1) — even a well-organized JSON/CMS layer is fine, doesn't need to be a full database at this scale. Relational or document database for user accounts and log entries.

**Data model for logs** (mirrors the existing personal design in `/logs/README.md` — keep this shape, it's already been validated by real use):
```
LogEntry {
  id, user_id, timestamp,
  intake: { state, context, intensity, duration },
  mode: check-in | optimize,
  safety_flag: none | <specific concern>,
  protocols_used: [{ tier, name, source }],
  outcome: text | null
}
```

**Safety screen — this is the one piece of technical architecture that cannot be an afterthought:**
- Runs server-side, on every submission, before any protocol-matching logic executes — never client-side-only, never skippable
- Should be a hybrid: an LLM judgment pass against a maintained flag-criteria list (see `README.md`'s hard safety rule for the starting list), PLUS hardcoded fallback resources (crisis line numbers, "find a therapist" links) that display regardless of the LLM's specific read — never let a single model call be the only thing standing between a real emergency and a redirect
- Log every safety-flag trigger (anonymized/aggregated is fine) so the flag criteria can be reviewed and improved over time — this is infrastructure work, budget real time for it, don't treat it as "one prompt and done"

**Alcohol-specific screening gate — a distinct, mandatory sub-component if any drinking-related feature or copy exists anywhere in the product:**
- Per `knowledge/substance-safety-screening.md`: implement AUDIT-C (validated 3-question screen, scored 0–12, positive at ≥4 men/≥3 women) as an actual onboarding gate before any alcohol-reduction content is shown — this is a known, validated instrument, do not improvise a substitute.
- AUDIT-C positive, OR self-reported heavy/daily use, OR any history of withdrawal seizures/delirium tremens → hard route to "talk to a doctor before stopping or cutting back," same visual/UX treatment as the general safety-flag screen (§4), not folded quietly into onboarding as a checkbox.
- Nicotine and cannabis do not require this gate — the app can support self-directed tapering for those directly, per the existing knowledge base. Alcohol is different specifically because withdrawal can be fatal (delirium tremens: 1–4% mortality even treated); do not build a unified "quit any habit" flow that treats all three the same way.
- This gate is required starting with the very first version that mentions alcohol at all — it is not deferrable to a later milestone.

**Auth/privacy — real requirements starting the moment a second real user exists (Stage B), not deferred to some future "scale" milestone:**
- Encrypt log data at rest — this is sensitive personal health-adjacent data (substance use, mental state) even for a small user base
- User-facing data export and full deletion, not just account deactivation
- No third-party analytics/ad-tech touching log content — this is the one place a "growth" instinct will directly betray the trust the product is selling
- Written, plain-language privacy policy stating exactly what's stored and why — this is also a copywriting deliverable, not just a legal one

**Explicitly NOT in scope for this build:** real-time crisis-response guarantees, clinical/diagnostic feature claims, HIPAA-level compliance infrastructure, multi-practitioner/coaching-client tooling (that's Stage C). If the builder tool's defaults nudge toward any of these (e.g., a template that adds diagnosis-sounding copy, or a health-app starter kit with clinical claims baked in), override the template rather than accept it.

## 6. Non-negotiables (restated — this file may be read independently of the rest of the repo)

1. **Never diagnose.** No copy, no UI state, no feature implies a diagnosis or guaranteed treatment outcome.
2. **The safety screen is real infrastructure**, per §5 — not a single LLM call with no fallback.
3. **Every protocol recommendation displays its evidence rating and source inline** — this is the product, not an appendix.
4. **Privacy requirements apply from the first real user**, not deferred to some later "scale" milestone.
5. **Alcohol gets its own AUDIT-C-based gate, from the first version that touches it at all** — never treated as equivalent-risk to nicotine/cannabis tapering. See §5.
6. **If a requested feature would violate any of the above, flag it and ask rather than building it anyway.**

## 7. Acceptance checklist before calling any milestone "done"

- [ ] Safety screen runs server-side on every submission, with hardcoded fallback resources independent of the LLM call
- [ ] Every displayed protocol shows an evidence badge and a working source link
- [ ] Safety-flag screen is visually distinct and never a dead end
- [ ] No copy anywhere implies diagnosis, treatment, or guaranteed outcome
- [ ] Log data is encrypted at rest; user can export and fully delete their data
- [ ] No ad-tech/third-party analytics has access to log content
- [ ] Onboarding explains the non-diagnostic framing before first check-in, not after
- [ ] If any alcohol-related feature/copy exists: AUDIT-C gate is implemented and blocks self-regulation content on a positive screen, heavy/daily use report, or withdrawal-seizure/DT history
- [ ] Proactive check-in reminders are opt-in and user-configurable, not a default aggressive push cadence
- [ ] Personalized/outcome-ranked recommendations are only labeled "personalized" once real outcome data backs them — generic evidence-based defaults show otherwise

---

*Companion to `product/DIRECTIVE.md` (strategy/marketing brief) and the source knowledge base in `/knowledge/`. Compiled 2026-08-06.*
