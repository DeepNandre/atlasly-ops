# skills

Each connector the loops use is a skill in this folder: a SKILL.md with how to call it plus any helper scripts. A loop calls a connector to read context or to act in your real tools.

## The one rule

Connectors inform. The signals folder decides. A loop reads from a connector, then writes its conclusion to brain/signals/. Never treat a connector's output as the system's memory.

## Connectors

- clerk: new signups (id, email, signup time, UTM source).
- supabase: product usage and telemetry. which tools a user ran, paywall hits, drop-off.
- stripe: subscriptions and revenue. Pro and Teams events, upgrades, churn.
- apollo: enrich a firm from a domain (size, sector, location, is-it-UK-AEC).
- attio: write leads and notes to the CRM.
- render-logs: backend logs, for the dev loops.
- supermemory: context enrichment across Gmail, GitHub, and past chats. see supermemory/SKILL.md.
- atlasly-mcp: Atlasly's own product tools and adapters.

Write a SKILL.md per connector as you wire each one. Start with the ones the first two loops need: clerk, supabase, apollo, attio, and supermemory (for signup-intel). The adapter runner that adapter-health depends on is product-repo specific, so it is defined in that loop's contract rather than here.
