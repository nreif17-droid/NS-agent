# Nervous System Optimization Agent

A personal agent (built for Claude Code) that helps regulate and optimize the nervous system — in the moment and over time. Takes a free-text current-state input and returns immediate regulation cues, a short-term plan, and where that fits in a longer-term arc.

**Status:** knowledge base built and research-complete for this pass. CLI is built as a Claude Code slash command — see `cli/README.md` for usage, run it as `/ns-check-in`.

## What this is

- **Educational / self-regulation tool, not a diagnostic-medical one.** It never claims a clinical diagnosis.
- Built for personal use first, architected so user records stay separable if this ever supports coaching clients later (data model note, not currently implemented — see CLI proposal).
- Three knowledge layers, per the original design decision:
  1. **Research pass** (this repo, `/knowledge/`) — live web research, curated, cited, credibility-flagged.
  2. **Static core** (`/knowledge/*.md`) — what Claude Code actually reads each session. Not re-researched from scratch every run.
  3. **Personal case log** (`/logs/`) — cumulative record of real trial runs (state in → protocol used → outcome), making the agent more precise over time. Currently empty — see `logs/README.md`.

## Hard safety rule (not a suggestion)

This agent must **never claim a clinical diagnosis**, and must **flag — not attempt to handle — anything that sounds like it needs a doctor or therapist**, including but not limited to:
- Persistent physical symptoms (weeks+, not resolving with basic regulation practices)
- Chest pain, shortness of breath, or other acute-medical-emergency presentations
- Suicidal ideation or any self-harm content
- Dissociative symptoms, trauma disclosures beyond general stress
- Disordered-eating indicators (relevant before suggesting fasting protocols)
- Panic-disorder history (relevant before suggesting breath-hold/hyperventilation protocols)
- Cardiovascular conditions (relevant before suggesting cold/heat exposure protocols)

When any of these appear in an intake, the response routes to "this is outside what this tool does — see a doctor/therapist" instead of generating a protocol. This rule is referenced throughout `/knowledge/` and must be enforced at the CLI/prompt level once built, not left as documentation only.

## Repo structure

```
/knowledge/                    research-backed reference library (read every session)
  foundations.md               CNS/PNS, sympathetic/parasympathetic, vagus, HPA axis
  dysregulation-causes.md      causes + effects of dysregulation, credibility-flagged
  protocols-generic.md         universal safe-for-anyone baseline practices
  protocols-basic.md           situational protocols (acute stress, wind-down, etc.)
  protocols-advanced.md        optimization-tier (cold/heat, HRV training, breath-hold, fasting)
  regularization-procedure.md  default dysregulated -> baseline sequence (original synthesis)
  optimization-procedure.md    default baseline -> high-performance sequence (original synthesis)
  craving-management.md        craving/urge mechanism, distinct from ANS dysregulation (added 2026-08-06)
/logs/                         cumulative personal trial-run entries (state in -> protocol -> outcome)
  README.md                    log format spec
  <user_id>/*.md                one file per check-in, written by /ns-check-in
/cli/                          the built tool + its design record
  README.md                    how to use /ns-check-in
  DESIGN-PROPOSAL.md           original proposal + the 5 decisions confirmed
.claude/commands/
  ns-check-in.md               the actual slash command Claude Code runs
README.md                      this file
```

## Source-quality convention (used throughout `/knowledge/`)

- 🟢 Strong — systematic review / meta-analysis / multiple RCTs / settled physiology
- 🟡 Moderate — mechanistic studies, smaller RCTs, cohort data, or plausible-but-thin evidence
- 🔴 Weak / fringe / contested — always flagged explicitly, never included quietly

Every knowledge file ends with a **Sources consulted** list and, where relevant, explicit **Where sources disagree** and **Flagged claims** sections. Known evidence gaps (e.g., circadian/light-exposure specifics in `protocols-basic.md`) are marked rather than papered over with an unverified citation.

## Notably flagged in this research pass

- **EMF exposure** as a dysregulation cause — evidence rated "inadequate/low" by WHO's own systematic reviews. Not treated as established in this knowledge base.
- **Polyvagal Theory** — used loosely as descriptive vocabulary; its specific neuroanatomical mechanism claims are scientifically contested and not relied on for any protocol's efficacy claim.
- **"Earthing"/electron-transfer grounding** — excluded from protocols as pseudoscience-adjacent; distinguished from legitimate attentional/sensory grounding, which is retained.
- **Breath-hold / cyclic hyperventilation work** — real, documented safety risk (shallow water blackout), not just a fringe worry. Hard safety language is mandatory wherever this protocol appears.
- **Fasted-state training** — evidence is duration-dependent and mixed, not the clean "fasting = calming" story often marketed.
- **Nicotine/cannabis tapering** (added 2026-08-05, `dysregulation-causes.md` §1.9) — nicotine withdrawal cortisol drop and rebound-vasodilation headache are well-documented mechanisms; Cannabis Withdrawal Syndrome is a real DSM-5-coded diagnosis, not a fringe claim. Both are explicitly non-fatal withdrawals per addiction-medicine literature, in contrast to alcohol/benzodiazepine withdrawal — that contrast is stated directly so tapering isn't over- or under-flagged by the safety screen.
- **"A craving lasts exactly N minutes"** (`craving-management.md`, added 2026-08-06) — widely repeated specific-duration claim traces to commercial rehab-marketing content in this pass, not primary research. What's actually sourced is the *shape* (rises and falls) not a specific countdown.
- **Incubation of craving** (`craving-management.md`) — cue-triggered craving can measurably *increase*, not just fade, over the first weeks-to-months of abstinence for several substances — a real, counter-intuitive, well-evidenced finding, stated directly so an upswing isn't mistaken for reversal.

## Usage

```
/ns-check-in
```

See `cli/README.md` for arguments (user_id, optimize mode) and what each run does.

## Roadmap: staged scope, not a straight line to "public app"

Discussed 2026-08-05. Three stages, each with a materially different bar to clear — recorded here so the jump between them doesn't get papered over later:

1. **Personal use (current stage).** You, `logs/self/`, no auth, no compliance surface. This is where the tool lives now and where it should keep getting used and refined.
2. **Coaching clients (if the Sedona practice reactivates).** The data model is already separable (`logs/<user_id>/`) for this reason. The real difference at this stage isn't code — it's that **a professional stays in the loop making the actual calls**, using this as a tool alongside their own judgment, not as an autonomous decision-maker. This is the natural next proving ground: real people, but a small number, with a human backstop.
3. **General-population app.** Explicitly *not* the current target, and not a natural next step from stage 2 without deliberate work. The gap is not "more code" — it's:
   - **The hard safety rule must get stricter, not looser, to go here.** "Never diagnose" (see above) is what keeps this in the same regulatory category as wellness apps (Calm, Headspace) instead of the category digital-therapeutics products occupy (FDA-adjacent clearance, clinical trials, compliance infrastructure — see Woebot, Wysa as reference points for what that actually costs).
   - **The safety screen can't stay "Claude's judgment reading free text" at that scale.** Fine for personal use where you know the context; not sufficient for a stranger's genuine emergency. Would need real clinical backing behind the flag logic, not just cited research.
   - **The logs become regulated health data at that volume.** A markdown file in a private repo is nothing; thousands of people's panic/anxiety histories is the kind of data HIPAA/GDPR-type obligations exist for.
   - **Liability shifts** once the software — not a professional — is the thing making the call for a stranger.

Nothing built so far is wrong for stage 3 if it's ever pursued seriously — the non-diagnostic core and separable data model are the right foundation either way. But stage 3 needs legal/clinical infrastructure this project doesn't have and isn't currently building toward. Skipping stage 2 and jumping straight to stage 3 is the failure mode to avoid.
