# Personal Case Log

This directory holds the cumulative, growing log of real trial runs: state going in → protocol used → outcome. This is what lets the agent get more personalized and precise over time (per the original design decision) — it's a *personal dataset*, not documentation.

**Currently empty.** The CLI (`/ns-check-in`, see `/cli/README.md`) writes entries here as it's actually used — nothing in this directory should be fabricated or backfilled ahead of real use.

## Format

Each entry should capture, at minimum:
- Timestamp
- User ID (kept separable even in personal-only use, per the original decision to architect for possible future multi-user/coaching use)
- Intake as given (state, context, intensity, duration)
- Which safety-flag check ran, and its result (routed to protocol vs. routed to "see a professional")
- Protocol(s) selected (tier + specific protocol name, tying back to `/knowledge/protocols-*.md`)
- Outcome (self-reported, structured — exact fields TBD in the CLI design proposal)

Confirmed format: one markdown file per entry (YAML front-matter + optional narrative), under `logs/<user_id>/<ISO-8601 timestamp>.md`, written by `/ns-check-in`. See `cli/README.md` and `.claude/commands/ns-check-in.md` for the exact write logic.
