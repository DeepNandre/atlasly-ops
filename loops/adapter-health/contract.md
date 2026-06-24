# loop: adapter-health

DEV. Read-only. Runs every data adapter against known-good test sites and flags any that started returning broken or changed data, before a customer hits it on a paid report. It does not fix anything.

## Goal

Every morning you know which adapters are healthy and which quietly broke, with the evidence, so a silent failure never shows up in a paid Meld-style report.

## Cadence

Daily, 07:00. Scheduled Automation pointing at this contract. Run it before signup-intel so dev signals exist when the GTM loops read the brain.

## Inputs

- the adapter list: the LPA adapters, OS, Land Registry, DNO, EPC, and the planning sources.
- test sites: a fixed set of known-good site addresses or postcodes, one or more per adapter, that should always return stable output. keep this list as test-sites.md in this folder.
- fixtures: the expected output shape for each test site, captured when the adapter was last known good. keep in a fixtures/ folder here.
- the adapter runner: the script or command in the Atlasly product repo that runs one adapter against one site. name it here once you wire it.

## Workflow

1. Read work-log.md, last 10 entries, and the open adapter-broken signals (skip re-flagging ones already open).
2. For each adapter, run it against its test sites via the runner.
3. Diff the output against the fixture. Look for malformed or empty responses, schema changes, missing fields, status errors, large unexpected value shifts.
4. For each adapter that deviates, write an adapter-broken signal (type: adapter-broken, domain: dev). Evidence must include which adapter, which test site, the raw diff against the fixture, and the last-good timestamp.
5. If an adapter is healthy and had an open broken signal, append to that signal's timeline that it recovered and set status to resolved.
6. Append a work-log entry: how many adapters checked, how many broke, how many recovered.

## The checker

This loop is read-only, so the risk is false positives, not bad writes. The check is the evidence: every signal carries the raw diff so you can confirm the break is real before anyone acts. The loop must never attempt a fix, edit product code, or open a PR. Broken adapters become signals. The fix loop (later) acts on them, with your merge gate.

## Spend cap

- max_turns: 15
- max_tool_calls: 30 (raise if the adapter list grows).
- stop_condition: all adapters checked against their test sites, signals written, work-log updated.
- on_cap: stop, write an escalation signal listing unchecked adapters. never retry a flapping endpoint in a tight loop, that is the broken-test-environment token burn.

## Boundaries

- read-only. no fixes, no code edits, no PRs, ever.
- do not flag on a single transient failure if a retry succeeds. flag on a real deviation or a repeated failure.
- keep test-sites.md small and stable. these are canaries, not coverage.

## Outputs

adapter-broken signals (and recovery updates) in brain/signals/, a work-log entry per run.

## Backlog

(empty.)

## Timeline

(empty.)
