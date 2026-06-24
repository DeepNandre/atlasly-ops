# SKILL: adapter-health

How to check Atlasly's data-source and LPA adapters against known-good test sites.

## The adapter set
The 10 LPA adapters, plus OS, Land Registry, DNO, EPC, and the planning sources. Keep the canonical list in `test-sites.json` so the loop checks exactly what ships.

## Known-good test sites
Each adapter has at least one test site with a stored expected output (a snapshot). A site is "known-good" when its real-world answer is stable and you have verified it by hand once. See `test-sites.json`.

## What "broken" means
- Empty result where the snapshot had data.
- Schema drift (a field renamed, moved, or dropped).
- Malformed output (unparseable, truncated).
- Timeout or repeated error from the source.

## How to check
1. Run the adapter against the test site.
2. Diff live output against the snapshot.
3. On mismatch, write or update an `adapter-broken` signal with the adapter name, the specific change, the last-good timestamp, and the failing site.

## Do not
- Do not change adapter code. This skill is read-only. Fixes go through `dev-fix`.
- Do not retry a failing source more than 3 times. Skip and note it.
