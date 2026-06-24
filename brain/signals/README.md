# brain/signals

The shared brain. Every loop reads from here and writes to here. This is the source of truth for the whole system. If a fact is not in a signal, it did not happen as far as the other loops are concerned.

Schema, naming, and lifecycle are defined in ../../architecture.md. Quick version:

- one file per observation, named `YYYY-MM-DD-NNN-short-slug.md`
- front matter (id, type, domain, status, priority, source, created, updated, links), then What, Evidence, Timeline
- one observation per file, never batch
- append to an existing signal's timeline instead of duplicating
- never delete, set status to resolved or parked

## Example

```
---
id: 2026-06-23-002-cad-export-paywall-drop
type: conversion-gap
domain: gtm
status: open
priority: high
source: conversion
created: 2026-06-23
updated: 2026-06-23
links: [2026-06-21-004-mcp-export-tool-error]
---

## What

Three Teams-shaped accounts hit the CAD export paywall this week and none upgraded. One also errored on the export MCP tool, so the block may be a bug, not pricing.

## Evidence

Supabase: domains acme-arch.co.uk and two others hit the /export gate 7 times total. Stripe: no upgrade events. The linked dev signal shows the export tool returning 500 for one of them.

## Timeline

- 2026-06-23 conversion: created. linked to the mcp tool error. flagged high because it touches revenue.
```

Note how it links to a dev signal. That cross-link is what lets the fix loop later see "this blocks revenue" and prioritise it over a cosmetic bug. The shared folder is what makes that possible. Separate scripts cannot do it.
