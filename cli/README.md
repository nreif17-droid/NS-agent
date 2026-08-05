# CLI — `/ns-check-in`

The tool is a Claude Code slash command: `.claude/commands/ns-check-in.md`. Run it from within Claude Code, in this repo, as:

```
/ns-check-in
```

or, once this ever supports more than one person:

```
/ns-check-in <user_id>
```

Add `optimize` to the arguments to request optimize-tier mode (subject to the prerequisite gate in `knowledge/optimization-procedure.md` — it'll fall back to check-in mode and explain why if the gate isn't met):

```
/ns-check-in self optimize
```

## What it does, every run

1. **Passive follow-up** — if your last logged protocol never got an outcome, it asks about that first.
2. **Intake** — state, context, intensity (1–5), duration, mode. Asks conversationally for whatever's missing.
3. **Safety screen** — always runs, by Claude's judgment against the flag list in the top-level `README.md`. If triggered, it stops there — no protocol, just a redirect — and still logs the entry.
4. **Protocol match** — reads the relevant `/knowledge/` files plus your own log history in `logs/<user_id>/`, so recommendations account for recurring patterns, not just the current message in isolation.
5. **Response** — always three parts: Right now / This week / Long-term, each recommendation cited back to its source file.
6. **Log write** — a new markdown file under `logs/<user_id>/`, format specified in `logs/README.md`.

## Design record

See `cli/DESIGN-PROPOSAL.md` for the original proposal and the five decisions that were confirmed to get here.

## Not built (by design, for now)

- No biometric/wearable integration.
- No coaching-client-facing UX (the data model leaves room for it; the interface doesn't exist).
- No standalone script outside Claude Code.
