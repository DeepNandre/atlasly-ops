# supermemory

Context enrichment. Query Supermemory when a loop needs background that lives in your Gmail, GitHub, or past chats. Use it for targeted lookups, not bulk ingestion.

## When to use

- signup-intel: before drafting outreach, check whether you have any prior email thread with this person or domain. a warm thread changes the draft completely.
- fix loop (later): context on recent commits or issues for a file.

## How

API key from console.supermemory.ai (not app.supermemory.ai, that is the consumer app and has no key page). The key starts with sm_. Query the memory search endpoint with the person, domain, or topic. It returns relevant memories plus a profile summary. There is also an MCP server if you prefer to call it as a tool.

## Rules

- targeted lookups only. it is token-billed (about half a cent per 1K text tokens), and the budget is tight. do not re-ingest everything on every run.
- write any conclusion to brain/signals/. supermemory informs, the signal decides.
- data boundary: Gmail is connected for the GTM win. think hard before indexing the proprietary adapter repos, that code is the moat. self-host or BYOC if you want code context without it leaving your perimeter.
