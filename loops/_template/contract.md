# loop: <name>

<one line: domain (GTM or DEV), what it does, and whether it is read-only, drafts-only, or code-writing>

## Worthiness check (delete once passed)

All four must be true, or use a plain prompt or a script instead:

1. the task repeats.
2. verification is automatable or gated to you.
3. the token budget can absorb the waste.
4. the agent already has the connectors it needs.

## Goal

<the end state, in one or two lines>

## Cadence

<how often, and a note to set it up as a scheduled Automation>

## Inputs (connectors)

<which skills/connectors this loop reads>

## Workflow

1. read work-log.md, last 10 entries.
2. <steps>
3. append a work-log entry.

## The checker (maker / checker, never the same agent)

<human gate for outbound, evidence-in-signal for read-only, separate verifier sub-agent for code>

## Spend cap

- max_turns:
- max_tool_calls:
- stop_condition:
- on_cap: stop and write an escalation signal. do not retry in a tight loop.

## Boundaries

<hard nevers>

## Outputs

<what it writes: which signal types, drafts, CRM, etc>

## Backlog

(empty.)

## Timeline

(empty.)
