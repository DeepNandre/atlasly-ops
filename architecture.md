# architecture.md

How this repo is organised and the conventions every loop follows. Point a fresh Codex or Claude Code session at this file to scaffold the structure.

## Artifact types

Everything a loop produces is an artifact in brain/. Three types to start:

- signals: anything observed that might matter. a lead, a conversion gap, a broken adapter, a bug, a content gap. this is the shared library every loop reads and writes. schema below.
- drafts: human-facing output waiting for approval (an outreach email). lives in the loop's folder next to the signal that triggered it. never sent by a loop.
- tasks: a unit of work handed to the fix loop. created from one or more signals.

Add new artifact types only when an existing one does not fit.

## Signal schema

Each signal is one markdown file in brain/signals/, named `YYYY-MM-DD-NNN-short-slug.md`. Front matter, then body, then a timeline.

```
---
id: 2026-06-23-001-terraquest-signup
type: lead            # lead | conversion-gap | adapter-broken | mcp-error | content-gap | bug
domain: gtm           # gtm | dev
status: open          # open | in-progress | parked | resolved
priority: medium      # low | medium | high
source: signup-intel  # which loop wrote it
created: 2026-06-23
updated: 2026-06-23
links: []             # other signal ids, urls, PR links
---

## What

One or two lines. What was observed and why it matters.

## Evidence

The raw facts the conclusion rests on. For a lead: who, firm, what they did, UTM source. For a broken adapter: which LPA, the diff against the fixture, last-good timestamp. Always include evidence so the human can verify without rerunning.

## Timeline

- 2026-06-23 signup-intel: created. drafted outreach, queued for approval.
```

Rules:

- one observation per signal. do not batch unrelated findings.
- if a signal already exists for the same thing, append to its timeline instead of creating a duplicate.
- never delete a signal. set status to resolved or parked.

## work-log.md

A single running file. Each loop appends one short entry when it finishes a meaningful chunk, and reads the last 10 entries when it starts so it knows what other loops have been doing. Format: `YYYY-MM-DD HH:MM loop-name: one line of what happened`. Newest at the bottom.

## The three safety defaults (every loop follows these)

### 1. Triggers are scheduled, not hand-run

Every loop runs as a scheduled Automation. The cadence is in the loop's contract. You do not start runs by typing. In Codex, set this up in the Automations tab (this repo, the contract as the prompt, the cadence). In Claude Code, use /loop on a cadence, a cron task, or push to GitHub Actions so it survives the laptop closing. Runs that find nothing should archive themselves. Runs that find something land in Triage.

### 2. Nothing grades its own homework (maker / checker)

- outbound (email, outreach): the loop drafts, the human approves and sends. the human is the checker.
- read-only loops (adapter-health): the check is evidence in the signal. include the raw diff so the human can confirm the flag is real, not a false positive.
- code changes (fix loop): a separate read-only sub-agent reviews the maker's work against the project skills and tests. use /goal for the stop condition so a separate model decides "done", not the agent that did the work.

### 3. Spend is capped and the loop self-halts

Every contract declares:

- max_turns: hard cap on model turns in one run.
- max_tool_calls: hard cap on tool calls in one run.
- stop_condition: a verifiable line that ends the run ("all adapters checked and signals written").
- on_cap: when a cap is hit or the loop is stuck, stop and write an escalation signal. never keep retrying a broken environment, that is how a loop burns a weekend of tokens doing nothing.

Set a dollar ceiling at the plan or runner level as a backstop, because the chat tools do not meter dollars per loop on their own. A loop with no ceiling is an open invoice.

## Supermemory and the source-of-truth line

Supermemory is a connector, not the brain. A loop may query it for context (have I emailed this domain, what is the history on this firm) and must write any conclusion to brain/signals/. Supermemory informs. The signals folder decides. Do not let its auto-forgetting or contradiction resolution sit upstream of a loop's ground truth.

## Adding a new loop

Copy loops/_template/contract.md to loops/<name>/contract.md and fill it in. Run the 4-question worthiness test first (see README). Do one supervised manual run before you schedule it.
