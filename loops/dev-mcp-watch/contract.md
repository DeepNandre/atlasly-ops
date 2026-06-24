# Loop: dev-mcp-watch

Status: build after `dev-adapter-health` and `gtm-signup` are stable.

## Goal
Find MCP tools that error, time out, or get called in unexpected ways as installs grow. This is direct product intelligence, an erroring tool is a tool that does not convert.

## Trigger
Codex Automation, daily, read-only, cheap model.

## Workflow (sketch, expand when you build it)
1. Read work-log and open `mcp-error` signals.
2. Read MCP telemetry and Render logs. Cluster errors by tool.
3. Write `mcp-error` signals per cluster: which tool, error pattern, frequency, last seen.
4. Cross-reference `conversion-gap` signals. If an erroring tool also sits on the upsell path, merge and set `revenue_impact: true` so `dev-fix` prioritises it.
5. Append to work-log.

## Reads / Writes
- Reads: MCP telemetry, Render logs.
- Writes: `mcp-error` signals.

## Human gate
None (writes signals only).

## Verification
Deterministic single pass over the log window.

## Budget and kill switch
Low cap (about USD 2 per run), single pass, escalate on cap.

## Timeline
- 2026-06-23 stub written.
