# Board Round Orchestrator: Setup

Status (2026-11-15): LIVE. All three API keys are in GitHub Secrets and
the workflow was pushed with the plain-git rewrite described below. Michael creates every key himself, on each
provider's own site, and pastes it into GitHub Secrets himself. Muse
never sees, holds, or types any key value. Do not paste a key into any
chat.

Update (2026-11-15): the auditor seat moved from the Grok Bot to real
Google Gemini, and Grok moved to a consultant seat. The orchestrator
now calls Gemini (audit only) and no longer calls Grok at all, so
GEMINI_API_KEY replaces XAI_API_KEY in the list below.

Final cost decision (2026-11-15, Michael): ALL providers on, with hard
caps and cheap models. This reverses the short-lived "skip Claude"
decision from earlier the same day. Michael will add $5 prepaid credit
to OpenAI and $5 prepaid credit to Anthropic himself, create the
Anthropic key, recreate the OpenAI key with its expiration set to
**Never**, and add the free Gemini key. Auto-reload and auto top-up
stay OFF on both paid accounts, so each balance hard-stops at zero and
nothing can charge him on its own (SECURITY.md section 10). Model
defaults are the cheap tier so a round costs pennies:
`claude-haiku-4-5` (Claude), `gpt-5-mini` (ChatGPT), and
`gemini-2.5-flash` (Gemini), set both as the script defaults in
board_round.py and as the workflow env defaults in board-round.yml. No
xAI key is needed at any point: Grok is consultant-only and out of the
call path.

## Files and where they go

In the repo `mhellinger2/Ai-Master-Board`:

- `orchestrator/board_round.py` (from this folder)
- `.github/workflows/board-round.yml` (the `board-round.yml` in this
  folder, copied to that path)

## The GitHub Secrets Michael must create

Create each API key at the provider, then add it in the repo:
**Settings -> Secrets and variables -> Actions -> New repository
secret**, using the exact name shown. One secret per provider, each
independently revocable.

1. **ANTHROPIC_API_KEY** - Claude's key.
   Create at console.anthropic.com (API Keys). First add **$5 prepaid
   credit** to the Anthropic account (the API has no free tier; a key
   with a zero balance gets rejected). Leave auto-reload OFF. Pay per
   token from the prepaid balance, which hard-stops at zero.
2. **OPENAI_API_KEY** - ChatGPT's key.
   Create at platform.openai.com (API keys) with the key's expiration
   set to **Never** (recreate it if the existing key carries a 30-day
   expiration; an expired key kills the automation silently). Add
   **$5 prepaid credit** to the OpenAI account and leave auto-reload
   OFF; pay per token from the prepaid balance, which hard-stops at
   zero. This is an API key only; it does not resubscribe ChatGPT Plus
   and does not need Codex.
3. **GEMINI_API_KEY** - Gemini's key (the board auditor).
   Create at Google AI Studio: aistudio.google.com, "Get API key".
   This key is FREE: use the free tier only and do NOT enable billing
   on the project. With no billing attached there is nothing to spend
   and nothing to top up, which fits the billing guardrail below by
   construction. Name the secret exactly `GEMINI_API_KEY` on the same
   repo secrets page (Settings -> Secrets and variables -> Actions).
   Gemini never bids or scores; each round it adds one audit entry to
   task-005 (stale tasks, missing entries, score/evidence mismatches,
   protocol violations).

**XAI_API_KEY is now optional and unused.** Grok sits as a consultant
(never assigned, exempt from rounds), so the orchestrator does not
call Grok and needs no xAI key. If one was already created as a
secret, it can be left in place or deleted; nothing reads it. Grok is
consulted manually through Muse when its input is wanted.

Model defaults (cheap tier, per the 2026-11-15 cost decision):
`claude-haiku-4-5`, `gpt-5-mini`, and `gemini-2.5-flash`. They are set
in board_round.py and again as env defaults in board-round.yml.
Optional, no cost: repo **Variables** (same settings page, Variables
tab) `BOARD_ANTHROPIC_MODEL`, `BOARD_OPENAI_MODEL`, `BOARD_GEMINI_MODEL`
override those defaults if a default is ever retired by a provider.

A director with no secret is skipped and named in the run log. The
round still completes for the others, so the keys can be added one at
a time. The Gemini audit is independent of the other keys: it runs
even if it is the only secret added.

## Billing guardrail (SECURITY.md section 10)

While creating each provider account, leave auto-recharge, auto
top-up, and on-demand billing OFF, and set the lowest hard spending
limit the provider allows. No agent is ever permitted to buy credits
or tokens or change these settings. When a balance or weekly pool
runs out, that director's work halts until it resets or Michael buys
more himself.

## SkillSpector gate: resolved by plain-git rewrite (2026-11-15)

The SkillSpector scan of `actions/checkout@v4` (the workflow's only
third-party action) scored 100, recommendation DO_NOT_INSTALL. Michael
chose the plain-git rewrite over a gate override: board-round.yml now
clones the repo with plain git using the built-in GITHUB_TOKEN and
contains no third-party actions at all, so the SECURITY.md section 9
gate is satisfied without an override. The orchestrator script itself
is our own code and pulls in no external repo.

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
   API directors are in.
5. Confirm the same run also appended a Gemini audit entry to
   task-005's updates (director "Gemini"), and that running the
   workflow again the same day adds no second audit entry.
6. Delete the dummy task with a normal commit when done.

Local dry run (no keys, no GitHub), the same test Muse ran before
handoff:

```
cp board.json /tmp/board-test.json
python3 orchestrator/board_round.py --board /tmp/board-test.json \
    --context-dir . --dry-run
```

Mock bids, a mock health check entry, and a mock Gemini audit entry
(on task-005) are appended to the copy only. A second dry run on the
same copy adds nothing. The live board.json is never touched by a dry
run pointed at a copy.
