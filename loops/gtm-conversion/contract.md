# Loop: gtm-conversion

Status: build after `dev-adapter-health` and `gtm-signup` are stable.

## Goal
Surface where free users bump the CAD-export paywall and where Teams-shaped accounts are, so upsell targets and product friction get logged.

## Trigger
Codex Automation, daily, read-only, cheap model.

## Workflow (sketch, expand when you build it)
1. Read work-log and open `conversion-gap` signals.
2. Stripe plus Supabase: who hit the paywall, which domains have multiple seats (Teams-shaped), where free users drop off.
3. Write `conversion-gap` signals. Where a gap maps to a known erroring tool from `dev-mcp-watch`, merge and set `revenue_impact: true`.
4. Append to work-log.

## Reads / Writes
- Reads: Stripe, Supabase.
- Writes: `conversion-gap` signals. Any suggested outreach routes through `gtm-signup`'s gate, this loop never contacts anyone.

## Human gate
None (writes signals only).

## Verification
Deterministic single pass.

## Budget and kill switch
Low cap (about USD 2 per run), single pass, escalate on cap.

## Timeline
- 2026-06-23 stub written.
