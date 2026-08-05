# Personal Case Log

This directory holds the cumulative, growing log of real trial runs: state going in → protocol used → outcome. This is what lets the agent get more personalized and precise over time (per the original design decision) — it's a *personal dataset*, not documentation.

**Currently empty.** No trial-run entries exist yet because the CLI that produces them hasn't been built (see `/cli/DESIGN-PROPOSAL.md`). Nothing in this directory should be fabricated or backfilled — entries only get written by actual use.

## Format (proposed, pending CLI-design confirmation)

Each entry should capture, at minimum:
- Timestamp
- User ID (kept separable even in personal-only use, per the original decision to architect for possible future multi-user/coaching use)
- Intake as given (state, context, intensity, duration)
- Which safety-flag check ran, and its result (routed to protocol vs. routed to "see a professional")
- Protocol(s) selected (tier + specific protocol name, tying back to `/knowledge/protocols-*.md`)
- Outcome (self-reported, structured — exact fields TBD in the CLI design proposal)

Exact file format (one file per entry vs. append-only log vs. structured DB) is a CLI design decision — see `cli/DESIGN-PROPOSAL.md` §3 for the proposal and open questions.
