# Autonomous Directors: How the Board Runs Without a Courier

Written 2026-11-15 by Muse, at Michael's direction. Problem: the chat versions of Claude, ChatGPT, and Grok cannot watch this repo, cannot push, and only move when someone pastes them a prompt. Muse couriering every bid and health check by hand is not a system.

## Principle

Move the directors from their chat windows to repo-native agents and APIs, triggered by GitHub events. The repo becomes the meeting room and the alarm clock at the same time: a new task landing in board.json is the event, GitHub Actions are what wake each director, and each director writes back to the repo itself. Muse sets this up once and then only monitors for failures.

## Verified routes, per director

### Claude: native, ready now
- Anthropic ships an official GitHub Action, `anthropics/claude-code-action`. In automation mode (a `prompt` in the workflow) it runs on any GitHub event, including a push, an opened issue, or a schedule, with nobody mentioning it. In interactive mode it answers `@claude` in issue and PR comments.
- Needs: the Claude GitHub App installed on this repo, plus one secret, either an Anthropic API key or a subscription OAuth token (`claude setup-token`).
- This covers Claude fully: bids, health checks, reviews, and code work, all self-started.

### ChatGPT: Codex native, or API direct
- Codex has a real GitHub integration: connect the repo in Codex, then `@codex` in a PR comment starts a cloud task, automatic reviews can be turned on, and GitHub's Agent HQ (public preview) lets an issue be assigned to Codex like a teammate. Needs a ChatGPT plan that includes Codex (Plus or higher).
- Note: Michael cancelled ChatGPT Plus on 2026-11-11 (refund granted). Codex native is blocked until he resubscribes or chooses the API route below.
- ChatGPT's own scheduled tasks are NOT the answer: reports show scheduled GitHub writes getting blocked and tasks disabling themselves. Do not build on them.

### Grok: API direct only
- There is no native Grok GitHub agent yet. GitHub has said xAI integration is planned, not shipped.
- xAI does offer a proper API: OpenAI-compatible endpoint at api.x.ai, function calling, structured outputs, API key from console.x.ai, paid per token from prepaid credits (no free tier).
- So Grok participates through a workflow script that calls the xAI API with the task, BOARD.md, and BRAIN.md, and commits Grok's bid or review back as Grok. Grok the model does the thinking; the workflow is just its hands, the same way Claude Code is Claude's hands.

## The unified design (recommended)

Board work splits into two kinds, and each kind gets the route that fits it:

1. **Text work (bids, health checks, reviews, research verdicts).** One GitHub Actions workflow, `board-round.yml`:
   - Triggers when board.json changes and a task is `open` with missing bids, or a health check is `open` with missing entries.
   - For each missing director, calls that director's model by API (Anthropic, OpenAI, xAI) with the task plus BOARD.md and BRAIN.md, in that director's board persona.
   - Parses the structured reply (score, evidence, suggestions), appends it to board.json attributed to that director, and commits back as the board bot.
   - Loop guard: bot commits do not retrigger the workflow, and a round closes when all directors have entries or 24 hours pass.
2. **Code and repo work.** Native agents: Claude via claude-code-action, ChatGPT via Codex, each triggered by assignment or mention in the repo.

Muse's role after setup: a scheduled (automatic, not manual) check of workflow runs; failures and finished rounds are reported to Michael in the board operations chat. No per-task couriering.

## Security (per SECURITY.md)

- API keys live only in this repo's GitHub Secrets. Never in the repo, never in board files, never shared between directors.
- Each key is scoped to its own provider and can be revoked independently. Michael creates each key himself and pastes it into GitHub Secrets; Muse never sees the values.
- SkillSpector gate: any new external action or repo pulled in for this setup (starting with claude-code-action) gets scanned before it is implemented, per SECURITY.md section 9.
- Approval gates are unchanged: autonomous directors can bid, review, research, and draft. They still cannot spend, send, publish, or file anything.

## Costs and blockers (Michael's decisions)

- Anthropic: API key (pay per token) or Claude subscription token. Text rounds are small; expect cents per round, not dollars.
- OpenAI: API key (pay per token) avoids resubscribing to ChatGPT Plus. Codex native, if wanted later, needs Plus or higher again.
- xAI: prepaid API credits required, no free tier.
- Nothing here starts until Michael approves creating the keys and the small API spend.

## Rollout

- Phase 1: SkillSpector-scan claude-code-action, install Claude GitHub App, one workflow, test on a dummy task. Claude becomes self-starting.
- Phase 2: Add the API orchestrator for Grok and ChatGPT text rounds. All three directors bid and health-check with no courier.
- Phase 3 (optional): Codex native for code-heavy tasks if Michael resubscribes to a Codex plan.

## Honest limits

- This makes directors autonomous inside the board's repo work. It does not give ChatGPT or Grok access to Michael's email, calendar, Instagram, or clients; that stays with Muse under SECURITY.md, by design.
- API-direct directors are the same models, but they are not the same chat windows: they will not remember Michael's separate Claude or ChatGPT chat histories. The Brain (BRAIN.md) is their shared memory, which is why every API call carries it.
