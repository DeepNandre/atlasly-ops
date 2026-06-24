# SKILL: triage-signals

How every loop reads, writes, and merges signals in `brain/signals/`. This is the shared discipline that makes the brain compound instead of fill with duplicates.

## Reading
At the start of a run, read the last ~10 lines of `brain/work-log.md` and scan open signals relevant to your domain. Do not act on something already handled.

## Writing
Create one markdown file per signal using the schema in `architecture.md`. Filename: `YYYY-MM-DD-kind-shortslug.md`. Always set `loop`, `domain`, `kind`, `status`, `priority`, and `sources`.

## The merge rule (this is the important part)
Before writing a new signal, check whether an open signal already describes the same underlying thing (same adapter, same tool, same firm, same user). If it does:
- Update the existing signal instead of creating a duplicate.
- Add a timeline entry.
- If your loop adds revenue relevance the other did not have (for example a tool that errors is also on the paywall path), set `revenue_impact: true`. This is how a dev signal and a GTM signal combine into one prioritised action.

## Status lifecycle
`open` -> `gated` (waiting for Deep) -> `acted` or `pr-open` -> `merged` or `resolved`. Use `parked` for low-value items you are deliberately not doing.

## Exclusion list (internal and test accounts)
Never score or count these as leads or users. Keep the canonical list here and update it as new internal accounts appear:
- (add Atlasly team emails)
- (add known test-account domains)
- (add founder and investor demo accounts)
