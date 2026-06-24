# claude.md

Global context and rules every loop session reads. Keep this tight, link out for detail.

## What Atlasly is

UK pre-construction site intelligence for the AEC sector (Parallel Labs Ltd). The durable asset is the proprietary data spine: GeoSourceAdapter, the LPA adapters, and DNO, OS, and Land Registry integrations, distributed through a live MCP server. Positioning is "feasibility infrastructure / data spine", not "AI platform".

Products and pricing: Pro is GBP 14.99 per month, Teams is GBP 49.99 per month. CAD exports are gated behind Pro. Track `utm source=mcp` on signups.

## Connectors available to loops

- Clerk: auth and signups
- Supabase: product usage and database
- Stripe: billing and subscriptions
- Apollo: firm and contact enrichment
- Attio: CRM
- MCP telemetry and Render logs: tool errors and backend behaviour
- Exa: web search
- Supermemory: personal and cross-app context (Gmail, GitHub, past chats). Read-only enrichment. See `skills/supermemory/SKILL.md`.

## Global rules

- **Source of truth is `brain/signals/`.** Read context from connectors, write conclusions to signals.
- **Gates.** Never send an email and never merge a PR. Draft and queue, set `status: gated`, leave it for Deep.
- **Worktrees.** Any loop that writes code runs in its own `git worktree`. Never two agents in one checkout.
- **Spend.** Every Automation runs under a hard dollar cap. On cap, stop and write an escalation signal. Never silently retry.
- **Model choice.** Cheap model for data pulls, scoring, clustering, and triage. Strong model only for drafting outbound and for code review.
- **Work log.** At the start of a run read the last ~10 lines of `brain/work-log.md`. At the end append one line: date, loop, what it did, what it wrote.
- **Exclude internal and test accounts** from all user metrics and lead scoring (the exclusion list lives in the relevant skill).

## Spawning agents for code work (fix loop only)

When `dev-fix` spawns sub-agents: always create a git worktree, never run the maker and checker as the same agent, and use a separate (ideally stronger) model on the checker. The harness in the product repo defines the lint rules, the dev-server script, and the PR checklist. Respect them.
