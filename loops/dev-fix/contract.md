# Loop: dev-fix

Status: build now, supervise for a week, never fully unattended while Deep is away unless it has been boring for weeks.

## Prerequisite
The codebase harness must exist before this loop runs at all (legible, executable, verifiable, see `architecture.md`). An agent cannot fix what it cannot navigate, run, and test. Do not skip this.

## Goal
Take the highest-value code-related signal and produce a reviewed PR. Deep merges.

## Trigger
Codex Automation, but supervised. Do not schedule it unattended until Deep has watched a week of its PRs and trusts them.

## Workflow
1. Read open signals. Pick the highest priority. `revenue_impact: true` first, then by frequency.
2. Open an isolated `git worktree`.
3. Maker sub-agent: write the fix and a test.
4. Checker sub-agent (separate, read-only, ideally a stronger model): review the draft against the project skills and existing tests. The maker never grades its own work.
5. Use `/goal` with a separate grader as the stop condition, for example "tests in scope pass and lint is clean", graded by a fresh model, not the maker.
6. File a PR. Update the signal to `status: pr-open` with the link.
7. Anything it cannot handle confidently goes back as a signal for Deep's triage, not a guessed fix.

## Reads / Writes
- Reads: open signals, the product repo (in a worktree).
- Writes: a branch and a PR. Updates the originating signal. Never merges.

## Human gate
Yes. Deep merges. The loop never merges. "Done" from the loop is a claim, your merge is the proof.

## Verification
The maker/checker split plus the `/goal` separate grader. This is the one loop where verification is hard, which is exactly why it is gated and supervised.

## Budget and kill switch
This is the expensive loop, treat the cap as serious.
- Hard dollar cap per run, set deliberately and low to start.
- Max-iteration cap. If it retries against a failing environment, kill after N attempts and write an escalation signal. This is the canonical failure mode, a loop that burned two days on a broken test env over a weekend. Never let it retry unbounded.
- One signal per run to start. Do not let it chew through the whole backlog unattended.

## Backlog
- [ ] Build and verify the harness in the product repo first.
- [ ] Define the checker sub-agent in `.codex/agents/` or `.claude/agents/`.
- [ ] Dry-run on one low-stakes adapter-broken signal with Deep watching.

## Timeline
- 2026-06-23 contract written, harness not yet built, loop not live.
