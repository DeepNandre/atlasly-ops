# Loop: dev-adapter-health

## Goal
Catch any data-source or LPA adapter that has quietly started returning malformed or empty data, before a customer hits it on a paid report.

## Trigger
Codex Automation, daily 07:00 UK, on a background read-only worktree. Cheap model. No code changes, this loop only observes and writes signals.

## Workflow
1. Read the last ~10 lines of `brain/work-log.md` and any open `adapter-broken` signals so you know what is already known.
2. For each adapter in the test set (the 10 LPA adapters, OS, Land Registry, DNO, EPC, and the planning sources), run it against its known-good test site(s) listed in `skills/adapter-health/test-sites.json`.
3. Diff the live output against the expected shape and snapshot for that test site.
4. For any adapter that fails (malformed, empty, schema drift, timeout), write or update an `adapter-broken` signal: which adapter, what changed, last-good timestamp, the failing test site.
5. If an adapter that had an open `adapter-broken` signal now passes, set that signal `status: resolved` with a timeline note.
6. Append one line to `brain/work-log.md`.

## Reads / Writes
- Reads: the adapters (read-only), `test-sites.json`, existing signals.
- Writes: `adapter-broken` signals only. No code changes, no customer contact.

## Human gate
None. This loop is read-only and writes signals. The fix happens later in `dev-fix`, which is gated.

## Verification
Deterministic, no model self-grading. The run is done when every adapter in the set has been checked once and results written. Stop condition: `adapters_checked == adapters_in_set`. Exit cleanly after one full pass.

## Budget and kill switch
- Hard cap: about USD 2 per run. This is cheap read-only work, treat any overage as a bug.
- Max one full pass per trigger. No retry loop over the whole set.
- If a single adapter check itself errors 3 times, skip it, write a signal noting the check is broken, and continue. Never retry the whole pass.
- On cap hit, stop and write an escalation signal.

## Backlog
- [ ] Expand the test-site set as new adapters ship.
- [ ] Add a second known-good site per LPA so one stale site does not cause a false positive.

## Timeline
- 2026-06-23 contract written, not yet live.
