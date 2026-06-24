# Loop: gtm-signup

## Goal
Turn every new signup into a scored, enriched, draft-ready lead, so warm relationships never get missed and Deep only sees the few worth his time.

## Trigger
Codex Automation, daily 08:00 UK, read-only checkout. Cheap model for pulls and scoring, strong model only for drafting the few that qualify.

## Workflow
1. Read the last ~10 lines of `brain/work-log.md` and open `lead` signals so you do not re-process the same signups.
2. Pull new Clerk signups since the last run. For each, pull Supabase activity: which tools they used, whether they hit the CAD-export paywall, UTM source (flag `source=mcp`), and how many seats exist on the domain.
3. Score against ICP: a UK AEC firm, planning, or developer, and active. Drop internal and test accounts (exclusion list in `skills/triage-signals/SKILL.md`).
4. For qualifying leads, enrich the firm via Apollo (size, sector, location).
5. Read context from Supermemory: have we emailed this person or domain before? Pull any prior thread summary. Read-only. Write the conclusion into the signal, never back into Supermemory.
6. Upsert the lead into Attio with a one-line "why now".
7. For the top leads only (cap at 5 per run), draft outreach with the strong model. Save the draft into the signal. Do not send.
8. Write or update a `lead` signal per qualifying signup: who, firm, score, paywall and Teams signals, prior-contact note, draft, `status: gated`.
9. Append one line to `brain/work-log.md`.

## Reads / Writes
- Reads: Clerk, Supabase, Apollo, Attio, Supermemory (all read-only for context).
- Writes: `lead` signals (with drafts), Attio records.

## Human gate
Yes. Every drafted email is `status: gated` and waits for Deep. Nothing sends. Conservative buyers and slow trust mean an auto-sent cold email to a TerraQuest or Meld type contact is unrecoverable. The loop's job ends at a queued draft.

## Verification
Deterministic. The run is done when every new signup since the last run is scored and the qualifying ones have a `lead` signal with a draft. Stop condition: `signups_processed == signups_pulled`. Draft quality is gated by Deep, so the loop does not self-grade content, it only verifies it processed everything.

## Budget and kill switch
- Hard cap: about USD 5 per run. The strong-model drafting is the cost.
- Cap strong-model drafts at 5 per run. If a signup spike exceeds that, score and queue the overflow without a draft and flag it for the next run. A spike must never blow the budget.
- Max one pass. On cap hit, stop and write an escalation signal.

## Backlog
- [ ] Once reply-tracking is wired, add a follow-up sub-step for gated leads that went out and got no reply.
- [ ] Feed warm upsell targets from `gtm-conversion` into the scoring step.

## Timeline
- 2026-06-23 contract written, not yet live.
