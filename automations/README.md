# Automations

How to turn each loop contract into a real scheduled trigger. This is the heartbeat layer. Without it you have one-off runs, not loops.

## Codex app (recommended starting point)
In the Automations tab, create one automation per loop:
- Project: this repo.
- Prompt: a one-liner that points at the loop, for example `Run the gtm-signup loop per loops/gtm-signup/contract.md`. Keep the instruction thin, the contract holds the detail.
- Cadence: from the contract's Trigger section.
- Environment: background worktree for read-only loops. The fix loop runs supervised first.
- Results land in the Triage inbox. No-op runs archive themselves.

Claude Code equivalent: `/loop` for a recurring prompt on an interval, `/goal` for run-until-a-verifiable-condition with a separate grader, plus hooks or GitHub Actions to keep it running after you close the laptop.

## Cadence table

| Loop                | Cadence            | Mode        | Suggested cap/run | Gate          |
| ------------------- | ------------------ | ----------- | ----------------- | ------------- |
| dev-adapter-health  | daily 07:00 UK     | read-only   | ~USD 2            | none          |
| gtm-signup          | daily 08:00 UK     | read-only   | ~USD 5            | approve sends |
| gtm-conversion      | daily              | read-only   | ~USD 2            | none          |
| dev-mcp-watch       | daily              | read-only   | ~USD 2            | none          |
| dev-fix             | supervised, manual | worktree    | set deliberately  | merge PRs     |

Caps are starting points, tune to your real token rates. The point is that a cap exists from day one, not its exact value.

## The non-negotiables on every automation
1. A hard dollar cap. On hit, stop and write an escalation signal. Never silently retry.
2. A max-iteration cap so a buggy run cannot loop forever.
3. A verifiable stop condition (a deterministic check for read-only loops, the `/goal` separate grader for the fix loop). The maker never grades its own work.

## Before you fly
Wire only `dev-adapter-health` and `gtm-signup`. Watch their Triage inboxes for a few days. Connect Supermemory's Gmail first. Leave `dev-fix` for supervised runs only. The read-only loops are safe to leave running while you are away, the fix loop is not.
