# loop: signup-intel

GTM. Drafts-only. Turns new signups into scored, enriched leads with a drafted outreach, queued for your approval. This is the GTM grind, automated. It never sends anything.

## Goal

Every new signup is, by the next morning, a signal in the brain with who they are, whether they fit the ICP, what they did in the product, any prior relationship, and a drafted first outreach waiting in your queue.

## Cadence

Daily, 08:00. Set up as a scheduled Automation pointing at this contract.

## Inputs (connectors)

clerk (new signups since last run), supabase (what each signup did: tools run, paywall hits, UTM source), apollo (enrich the firm from the domain), supermemory (prior email or chat with this person or domain), attio (write the lead).

## Workflow

1. Read work-log.md, last 10 entries.
2. Pull signups from clerk since the last run.
3. Drop internal and test accounts (see boundaries).
4. For each real signup:
   - enrich the firm via apollo. is it a UK AEC firm, a developer, an architect practice. score fit: high / medium / low.
   - pull product activity from supabase. active or one-and-done. did they hit the CAD export paywall. UTM source (flag source=mcp).
   - query supermemory: any prior email thread or chat with this person or domain.
   - write a lead signal (type: lead) with all of the above as evidence.
   - for medium and high fit only, draft a short outreach email. if supermemory found a warm thread, the draft references it. save the draft in this folder next to the signal id. do not send.
   - write the lead to attio with a one-line "why now".
5. Append a work-log entry: how many signups, how many real, how many high-fit, how many drafts queued.
6. Surface the high-fit drafts in Triage for your morning review.

## The checker (your gate)

You are the checker. Drafts queue, you approve and send. The loop never sends email. No exceptions.

## Spend cap

- max_turns: 20
- max_tool_calls: 40
- stop_condition: all new signups processed, signals written, drafts queued, work-log updated.
- on_cap: stop, write an escalation signal listing which signups were not processed. do not retry in a tight loop.
- tune these up only after a few clean runs.

## Boundaries

- never send anything. draft and queue only.
- drop internal and test accounts before scoring (your own emails, parallellabs domains, known test addresses). keep an exclusion list as exclusions.md in this folder.
- ICP scope for scoring: UK AEC. architects, developers, planning consultants. score non-fit as low and do not draft outreach.
- do not email a person more than once via this loop. if a lead signal already exists for them, append to its timeline, do not create a new draft.
- conservative sector, slow trust. a wrong auto-send to someone like a TerraQuest or Meld contact is unrecoverable, which is the whole reason sends are gated.

## Outputs

lead signals in brain/signals/, drafts queued for approval in this folder, leads written to attio, a work-log entry per run.

## Backlog

(empty. the loop adds and reprioritises here.)

## Timeline

(empty. append what happened each run.)
