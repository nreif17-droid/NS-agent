# Regularization Procedure: Dysregulated → Baseline

**Labeling note (important):** unlike the other knowledge files, this document is **original synthesis**, not a literature summary. The individual *components* it sequences are cited in `protocols-generic.md`, `protocols-basic.md`, and `dysregulation-causes.md`. The *sequencing logic itself* — this specific default path from dysregulated to baseline — has not been validated as a bundled protocol in the literature. Treat it as this agent's best-current design, expected to be revised as the personal case log (`/logs/`) accumulates evidence about what actually works for the specific person using it.

## Default sequence

### Stage 0 — Safety screen (always first, non-negotiable)
Before anything else: check intake against the standing safety-flag list (persistent symptoms, chest pain, suicidal ideation, dissociation, disordered-eating indicators, panic-disorder history relevant to breathwork, cardiovascular conditions relevant to cold/heat). If triggered, route to "this needs a doctor/therapist," do not proceed with a protocol. This is a hard rule, not a suggestion (per the brief).

### Stage 1 — Immediate down-regulation (minutes)
Pull from `protocols-generic.md` §1 (breath) and §2 (grounding), sequenced per the acute-stress-spike protocol in `protocols-basic.md` §1. Goal: get arousal low enough that Stage 2 planning is actually usable (recall `foundations.md` §4 — prefrontal function is impaired at high arousal, so nothing sophisticated should be attempted until this stage is done).

### Stage 2 — Stabilize the day/week (basic tier)
Match the situational `protocols-basic.md` entry to what's actually driving the dysregulation (poor sleep → wind-down protocol; conflict → post-conflict reset; etc.) rather than defaulting to a generic response. This requires the intake to actually capture *context*, not just intensity — see CLI design proposal.

### Stage 3 — Address the causal layer
Cross-reference `dysregulation-causes.md` §1 against the person's stated context. If the same cause keeps recurring across log entries (see `/logs/`), that's the signal to escalate from "manage the symptom" to "address the cause" — e.g., repeated acute-stress-spike protocol use tied to poor sleep should prompt a sleep-focused week, not just more breathing exercises in the moment.

### Stage 4 — Confirm return to baseline
No formal validated "baseline" biomarker exists in this knowledge base (HRV is a noisy, individual-baseline-relative signal per `foundations.md` §2 — not a fixed threshold). Baseline is operationally defined **per-person, from their own log history**: once the CLI has enough entries, "baseline" = the person's own typical resting state, not a population norm. Until then, use self-report (the intake's own intensity/duration fields) as the working definition.

## Why this order, explicitly

1. Safety screen must be first — everything downstream assumes this is an educational-use case, not a crisis.
2. Immediate tools before causal analysis — you cannot think clearly about causes while acutely dysregulated (this is a direct, cited physiological claim, not just intuition).
3. Situational (basic) before causal (deep) — matches effort to urgency; not every dysregulated moment needs a root-cause investigation, but repeated instances should trigger one.
4. Log-driven baseline over generic baseline — the brief's core design goal (personal case log making the agent more precise over time) only works if "baseline" is defined relative to the individual, not a textbook norm that may not fit them.

## Open design question for the CLI

This procedure assumes the CLI can (a) detect "safety flag" language in free text reliably, and (b) detect *recurring* causes across log entries — both are software/data-model questions, not knowledge-base questions. Flagged here, addressed in the CLI design proposal.

*Last updated: 2026-08-05. Revise this file as `/logs/` accumulates — this is meant to change.*
