# atlasly-ops

The control layer for Atlasly's agent loops. Loops wake on a schedule, read this folder, do one job, write what they found back, and die. The folder is the brain. No chat stays open.

This repo is separate from the Atlasly product repo. Loops that touch product code (the fix loop, later) check out the product repo in an isolated worktree. The watch and draft loops in here never touch product code.

## What's in here

```
atlasly-ops/
  AGENTS.md            business context + global rules (canonical)
  CLAUDE.md            pointer to AGENTS.md for Claude Code
  architecture.md      conventions: signal schema, the 3 safety defaults, how to add a loop
  brain/
    signals/           every loop reads + writes here. the source of truth.
      README.md        signal schema + lifecycle
    work-log.md        each session appends on finish, reads last 10 on start
  skills/
    README.md          connector index + the source-of-truth rule
    supermemory/
      SKILL.md         context enrichment (Gmail, GitHub, past chats)
  loops/
    signup-intel/contract.md     GTM. daily. drafts + queues, you approve sends.
    adapter-health/contract.md   DEV. daily. read-only. flags broken data adapters.
    _template/contract.md        copy this to spin up a new loop
```

## How to stand it up

1. Open one Codex session (or Claude Code) in this repo. This session is you doing setup, not an orchestrator you keep running.
2. Point it at architecture.md and say: scaffold the artifact types and read the two loop contracts.
3. For each loop, do one manual test run with the agent. Watch it. Calibrate the workflow until the output is what you want.
4. Once a loop's run looks right, turn it into a scheduled Automation. In Codex: Automations tab, pick this repo, the loop's contract as the prompt, the cadence from the contract. In Claude Code: /loop on a cadence, a cron task, or push to GitHub Actions so it survives the laptop closing. Findings land in your Triage inbox.
5. Close the session. The Automation runs the loop on its own from now on.

## The three defaults baked into every contract

1. Trigger is a scheduled Automation, never you hand-running it. Cadence lives in the contract.
2. Nothing grades its own homework. Drafts and any outbound queue for your approval. Code changes (fix loop) get a separate read-only verifier sub-agent. Stop conditions use /goal so a separate model decides "done".
3. Spend is capped. Every loop declares max turns, max tool calls, a hard stop condition, and an escalate-to-you-on-cap rule. Set a dollar ceiling at the plan or runner level too. A loop with no cap is an open invoice.

## Where you come in (the only gates)

- You approve anything that goes to a human (cold email, outreach). Loops draft and queue, never send.
- You merge PRs. The fix loop files them, you decide.
- Everything else (reading data, writing signals, drafting) runs without you, including while you are away.

## Before you add any loop, run the 4-question test

A loop is worth it only if all four are true. Miss one and a plain prompt or a normal script is better.

1. The task repeats. A loop amortizes its setup cost. A one-off does not.
2. Verification is automatable, or it is gated to you. "tests pass" is a real check. "looks good" is not.
3. The token budget can absorb the waste.
4. The agent already has the connectors it needs.

## Build order

Now, safe to leave running: adapter-health (read-only), signup-intel (drafts + queue).

Later, build then supervise: the codebase harness in the product repo, then the fix loop. Do not point a code-writing loop at prod while you are on another continent until it is boring.
