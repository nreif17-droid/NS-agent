# CLI Design Proposal — Nervous System Optimization Agent

**Status: confirmed and built (2026-08-05).** All five open decisions below were confirmed as the recommended option in each case. The built tool is `.claude/commands/ns-check-in.md` — see `cli/README.md` for how to use it. This file is kept as the design record.

**Decisions locked in:**
1. Claude Code skill (slash command), not a standalone script
2. Safety screen: Claude judgment against the `README.md` flag list, not a fixed keyword list
3. Terminal-only output, no separate export file
4. Outcome capture: passive follow-up only (asks about the most recent unresolved entry at the start of the next check-in)
5. Data model: `logs/<user_id>/` directory + `user_id` field, no auth — sufficient for now

## 0. What kind of "CLI" this actually is

Given the build target is Claude Code, the highest-leverage version of this isn't a standalone binary — it's a **Claude Code skill** (a slash command, e.g. `/ns-check-in`) that:
1. Reads the relevant `/knowledge/*.md` files (already built) as its reasoning substrate.
2. Runs a structured intake.
3. Reads `/logs/` for personal history before responding.
4. Writes a new structured entry to `/logs/` after responding.

The only actual "code" needed is a small amount of glue for reading/writing log files in a consistent format — the reasoning/matching (which protocol fits this state, is this a recurring cause) is done by Claude reading the knowledge base and log history directly, not by a bespoke matching algorithm. **Open decision #1: confirm this "skill, not standalone app" framing is what you want**, versus a real standalone script (e.g., Python CLI) that shells out to Claude — the skill approach is faster to build and iterate, a standalone script would be more portable outside Claude Code.

## 1. Intake structure

Per the brief: "lightweight structured prompt (state, context, intensity, duration) so responses are consistent, not just freeform chat." Proposed fields:

| Field | Type | Purpose |
|---|---|---|
| `state` | free text | What's happening physically/mentally right now — the core input |
| `context` | free text, optional | Situational context (what triggered this, if known) |
| `intensity` | 1–5 scale with anchors (1 = mild/manageable, 5 = acute/overwhelming) | Drives which tier (generic/basic/advanced-inappropriate) gets selected |
| `duration` | enum: *acute (right now) / hours / days / weeks+* | Distinguishes "help me right now" from a pattern that needs `dysregulation-causes.md` cross-referencing |
| `mode` | enum: *check-in (default) / optimize* | Routes to `regularization-procedure.md` vs `optimization-procedure.md` — "optimize" only proceeds if the prerequisite gate in that file is met |

**Safety screen runs first, always**, against the flag list in `README.md`, before any of the above gets used to select a protocol. If triggered: output is the safety redirect, full stop — no protocol generated, but the intake still gets logged (with the safety-flag outcome, not a protocol) so the pattern is visible over time.

**Open decision #2:** should the safety screen be a hard keyword/pattern list (fast, auditable, but brittle) or should it rely on Claude's judgment reading the free-text `state`/`context` fields against the flag criteria (more flexible, less auditable, no fixed keyword list to maintain)? Recommendation: Claude-judgment-based, since keyword lists both over-trigger ("I could just die of embarrassment") and under-trigger (paraphrased ideation) — but flagging this as a decision because a hard list is easier to unit-test.

## 2. Output format

Per the brief, every response returns three parts. Proposed structure:

```
## Right now
[2-4 cues, pulled from protocols-generic.md and/or protocols-basic.md,
 tiered to the stated intensity. Each cue names which knowledge file it's from.]

## This week
[Short plan: which protocols-basic.md situational protocol fits, what to
 track (feeds the next log entry), timing suggestions.]

## Long-term
[Where this sits in the regularization/optimization arc (which stage of
 regularization-procedure.md or optimization-procedure.md), and what
 "leveling up" looks like next — only meaningfully populated once enough
 log history exists; early on this section says so honestly rather than
 inventing a trajectory from one data point.]
```

Every protocol cited traces back to a specific `/knowledge/` file/section so recommendations are always attributable, not freeform. **Open decision #3:** terminal/markdown text output (fits "CLI used within Claude Code") vs. also writing a saved file per check-in — recommend markdown-to-terminal by default, saved automatically via the log-write step (§3) rather than as a separate export.

## 3. How it reads and writes `/logs/`

**Proposed log format:** one markdown file per entry, YAML front-matter + short narrative, under `/logs/<user-id>/<timestamp>.md`. Rationale over a single JSONL/DB file: git-diffable, human-readable without tooling, and Claude Code reads markdown natively without a parsing layer — consistent with how `/knowledge/` is already structured.

```yaml
---
timestamp: 2026-08-05T14:30:00-07:00
user_id: nreif17          # separable per the "keep records separable" decision
intake:
  state: "..."
  context: "..."
  intensity: 3
  duration: hours
mode: check-in
safety_flag: none          # or the specific flag that triggered, if any
protocols_used:
  - tier: basic
    name: acute-stress-spike
    source: protocols-basic.md#1-acute-stress-spike
outcome: null               # filled in on a follow-up, see below
---
Free-text narrative if useful (optional).
```

**On read (before generating a new response):** the skill scans `/logs/<user-id>/` for:
- **Recency** — last N entries, to catch "you did the acute-stress protocol 3 times this week" patterns feeding `regularization-procedure.md` Stage 3 (causal escalation).
- **Recurring `context`/cause matches** — loose text matching against `dysregulation-causes.md` categories (e.g., repeated mentions of poor sleep) — this is Claude reading and reasoning over the log files directly, not a separate analytics engine, at least initially.
- **Baseline signal** — if intensity/state patterns exist across enough entries, that becomes the "personal baseline" both procedure files reference in place of a population norm.

**On write:** append a new entry after every check-in, including safety-flag-only entries (no protocol run) — those matter for pattern-tracking too (e.g., "this is the third week you've flagged something safety-relevant" is itself useful signal, even though the agent still doesn't handle it directly).

**Outcome capture — open decision #4:** the brief wants outcome tracked ("state in → protocol used → outcome"), but outcome isn't known at intake time. Two options:
- (a) A lightweight follow-up prompt next time the CLI runs ("last time you used X for Y — how did that land?") that back-fills the previous entry's `outcome` field.
- (b) A separate explicit `/ns-outcome` command for logging results whenever you think to.
Recommend (a) as default with (b) available, since (a) is the only version that reliably produces enough outcome data to be useful — but this changes the intake flow (an extra question sometimes), so flagging for confirmation rather than assuming.

## 4. Data model / multi-user separability

Per the brief's decision to architect for possible future coaching use: `user_id` on every log entry and a `/logs/<user-id>/` subdirectory per person, even though only one directory will exist for a while. No other multi-tenancy work (auth, access control) proposed at this stage — out of scope until/unless the coaching use case actually activates. **Open decision #5:** confirm this minimal separability (directory + field, no auth) is sufficient for now, versus wanting more structure built in upfront.

## 5. What I'm NOT proposing to build yet

- Any biometric/wearable integration (HRV device data) — `foundations.md` §2 already flags HRV as a noisy, hard-to-standardize signal; adding real device data is a meaningfully bigger scope decision, not a default.
- A coaching-client-facing interface — the data model leaves room for it, but building the actual multi-client UX is future scope per the brief ("potentially reusable... later").
- Any keyword-list implementation for the safety screen, pending open decision #2.

## Summary of open decisions needing your confirmation

1. Skill-in-Claude-Code vs. standalone script
2. Safety screen: Claude-judgment vs. hard keyword list
3. Output: terminal-only vs. also exporting a saved file separate from the log write
4. Outcome capture: passive follow-up prompt vs. explicit separate command
5. Data model: is directory-per-user + `user_id` field sufficient for now

Once confirmed, next step is building the actual skill file + a minimal log read/write mechanism — not before.
