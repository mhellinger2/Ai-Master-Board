# Board Round Orchestrator: Setup

Status (2026-11-15): code built and dry-run tested locally by Muse. Not
live yet. It goes live only after Michael creates the three API keys
below as GitHub Secrets. Michael creates every key himself, on each
provider's own site, and pastes it into GitHub Secrets himself. Muse
never sees, holds, or types any key value. Do not paste a key into any
chat.

## Files and where they go

In the repo `mhellinger2/Ai-Master-Board`:

- `orchestrator/board_round.py` (from this folder)
- `.github/workflows/board-round.yml` (the `board-round.yml` in this
  folder, copied to that path)

## The three GitHub Secrets Michael must create

Create each API key at the provider, then add it in the repo:
**Settings -> Secrets and variables -> Actions -> New repository
secret**, using the exact name shown. One secret per provider, each
independently revocable.

1. **ANTHROPIC_API_KEY** - Claude's key.
   Create at console.anthropic.com (API Keys). Pay per token from the
   Anthropic account balance.
2. **OPENAI_API_KEY** - ChatGPT's key.
   Create at platform.openai.com (API keys). Pay per token. This is an
   API key only; it does not resubscribe ChatGPT Plus and does not need
   Codex.
3. **XAI_API_KEY** - Grok's key.
   Create at console.x.ai (API Keys). xAI has no free API tier: buy
   prepaid credits on that account first or Grok's calls will fail and
   be logged as skipped.

Optional, no cost: repo **Variables** (same settings page, Variables
tab) `BOARD_ANTHROPIC_MODEL`, `BOARD_OPENAI_MODEL`, `BOARD_XAI_MODEL`
override the default models in board_round.py if a default is ever
retired by a provider.

A director with no secret is skipped and named in the run log. The
round still completes for the others, so the keys can be added one at
a time.

## Billing guardrail (SECURITY.md section 10)

While creating each provider account, leave auto-recharge, auto
top-up, and on-demand billing OFF, and set the lowest hard spending
limit the provider allows. No agent is ever permitted to buy credits
or tokens or change these settings. When a balance or weekly pool
runs out, that director's work halts until it resets or Michael buys
more himself.

## Pending before go-live: SkillSpector

The workflow references one external action, `actions/checkout@v4`
(GitHub's official checkout). Per SECURITY.md section 9, Muse must run
SkillSpector on it and report the score and findings to Michael before
this workflow is pushed live. The orchestrator script itself is our own
code and pulls in no external repo.

## Test procedure (dummy task, after secrets are in)

1. Add a dummy task to board.json: a new task with a fresh id (for
   example `task-999`), status `open`, empty `bids`, `health_checks`,
   and `reviews`, and a harmless title like "Dummy: orchestrator test".
   Commit and push it (or run the workflow manually: Actions tab ->
   Board Round -> Run workflow).
2. In the Actions tab, open the run log. Confirm each director with a
   secret added a bid, and each director without one is logged as
   skipped for a missing key, with no failure.
3. Confirm the follow-up commit from `board-bot` with `[board-bot]` in
   its message appended the bids to board.json, and that this bot
   commit did NOT start a second run (the loop guard: the job condition
   and the script both reject bot triggers).
4. Open a dummy health check on the same task (a health_checks entry
   whose status contains "open"), push, and confirm each director's
   entry lands and the round status closes with an average once all
   three are in.
5. Delete the dummy task with a normal commit when done.

Local dry run (no keys, no GitHub), the same test Muse ran before
handoff:

```
cp board.json /tmp/board-test.json
python3 orchestrator/board_round.py --board /tmp/board-test.json \
    --context-dir . --dry-run
```

Mock bids and a mock health check entry are appended to the copy only.
The live board.json is never touched by a dry run pointed at a copy.
