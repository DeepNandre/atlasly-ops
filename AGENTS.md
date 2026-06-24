# AGENTS.md

Canonical context and rules for every loop in this repo. Codex reads this automatically. Claude Code: see CLAUDE.md.

## What Atlasly is

Atlasly is a UK pre-construction site intelligence platform for the AEC sector (Parallel Labs Ltd). It turns a site address into planning constraints, flood risk, heritage designations, terrain, grid and DNO data, and CAD exports.

The moat is the proprietary data spine: the GeoSourceAdapter, the LPA adapters, and integrations across OS, Land Registry, DNO, and EPC sources, plus an MCP server that lets AI clients call Atlasly tools directly. Positioning is "feasibility infrastructure" and "proprietary data spine", not "AI-powered tool". Avoid the words tool, platform, and AI-powered in anything customer-facing.

Revenue is live on Stripe: Pro at GBP 14.99/mo, Teams at GBP 49.99/mo, CAD exports gated to Pro. Roughly 1,600 MAUs. Co-founded with Shatakshi Patil (CPO, practising architect). Built solo.

## Available connectors (see skills/)

clerk (signups), supabase (usage and telemetry), stripe (subscriptions), apollo (firm enrichment), attio (CRM), render-logs (backend logs), supermemory (context enrichment from Gmail, GitHub, past chats), atlasly-mcp (the product's own tools and adapters).

## Global rules for every loop

1. Source of truth is brain/signals/. Read connectors for context, but write every conclusion as a signal file. A connector informs. The signal decides.
2. Read brain/work-log.md (last 10 entries) at the start of a run. Append one entry when you finish a meaningful chunk.
3. Never grade your own output. Outbound (email, outreach) queues for the human. Code changes get a separate read-only verifier. Stop conditions use a separate grader (/goal).
4. Respect the spend cap in your contract (max turns, max tool calls, hard stop). When you hit a cap or get stuck, stop and write a signal that escalates to the human. Do not keep retrying.
5. The two human gates are: sending anything to a person, and merging a PR. Never cross either without an explicit human action.

## Spawning engineering sub-agents (fix loop, later)

When a loop needs to change product code:

- Check out the Atlasly product repo in an isolated git worktree, one per task, so parallel work never collides.
- One sub-agent drafts the fix and a test. A separate read-only sub-agent reviews it against the project skills and existing tests. The maker is never the checker.
- File a PR. Do not merge. Merging is the human's gate.
- Never run a code-writing loop unattended until it has run supervised and proven boring.
