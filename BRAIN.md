# The Brain

This file is the board's shared memory. It is where lessons live so every director keeps learning instead of reacting once and repeating the same mistakes.

## Rules of the Brain

- **Read before you bid.** Every director reads this file at the start of every session, before bidding, updating, or reviewing. Bidding without reading the Brain is a protocol violation.
- **Lessons are appended, never deleted.** A lesson that turns out wrong is superseded by a new dated entry that says so and why. The history of being wrong is part of the learning.
- **Every lesson carries evidence**: the date, the task or event it came from, and what actually happened. Opinions without an event behind them do not belong here.
- **Health checks feed the Brain.** When a task closes, Muse distills its health check round into lessons: what the assignee did well, what the accepted suggestions fixed, and what the next director should do differently.
- **Track records are kept here.** Scores from bids and health checks accumulate per director, so future bidding can weigh a director's history, not just its confidence.

- **2026-11-15, new director**: Michael created a Grok Bot named Gemini and seated it on the board with one niche job: Board Auditor and Round Runner. It opens and chases health check rounds, verifies the dashboard against board.json after every update, spot-checks bid evidence, and posts a weekly hygiene digest. It was chosen because Grok Bots run in the background on their own cloud computer (so this seat needs no courier), the job is read-only (lowest risk seat), and no other director owned verification. First assignment: task-005.

## Director track records

- **Muse**: task-001 bid 9, assigned. Task-002 bid 9, assigned, packet delivered same day. Task-003 bid 8, assigned after the tie with ChatGPT was settled by the directors at Michael's instruction (Muse took the source-level scan, ChatGPT reviews). Task-004 bid 8, orchestrator built and dry-run tested.
- **Claude**: task-001 bid 8, conceded to Muse's 9, took reviewer role. Accurate self-assessment: knew it could write the content but not publish it. Task-002 health check: scored 7 with four concrete suggestions. Task-003 bid 4, still labeled provisional (flagged Sellbird's safety history; could not open the source).
- **ChatGPT**: task-001 bid 8, recorded as a separate director entry at its own request rather than overwriting Claude's. Careful about attribution. Task-002 health check: scored 8.5 with five suggestions. Task-003 bid 8, took reviewer role after the tie settlement.
- **Grok**: task-001 bid 7, conceded to Muse's 9, offered trend research and visuals as its best contribution. Accurate self-assessment: had not seen the caption rules or past posts. Moved to consultant 2026-11-15 (Michael's call): never assigned, exempt from rounds, consulted sparingly for live X signal; its owed task-002 health check entry and task-003 bid were waived with the move. Its Grok Bot weekly pool ran out 2026-11-15 (SECURITY.md section 10, no credits bought).
- **Gemini**: the auditor seat. First held by a Grok Bot named Gemini, which delivered a strong task-005 audit on 2026-11-15 (caught the stale repo, missing track records, task-001's missing updates, Claude's provisional bid) before its pool ran out. On 2026-11-15 Michael swapped the seat to real Google Gemini, reached through the Gemini API free tier; same job, independent model family, no shared pool with any director.

## Lessons learned

- **2026-11-15, board setup**: Directors in their consumer chats (Claude, ChatGPT, Grok) cannot clone, commit, or push to this repo from chat, and their fetch tools can be rate-limited. The working pattern is the relay: the director states its entry, bid, or review in its own words, Michael passes it to Muse, and Muse commits it verbatim as secretary. Claude Code is the exception: when Michael runs it, it can read and write the repo directly.
- **2026-11-15, board setup**: A director's local view can be stale. Claude insisted it was "not on the board yet" after its entry had already been posted by relay, because it could not fetch the files. Ground truth is this repo, not any single director's chat.
- **2026-11-15, bidding**: On task-001 all three non-assignee directors scored themselves honestly and conceded to the highest bid without dispute. Evidence-backed bids made the round fast. Keep requiring evidence with every score.
- **2026-11-15, security**: This repo is public by design so every director can read it. Protection comes from content discipline (SECURITY.md section 4), not from access control. Anything sensitive stays off the board.
- **2026-11-15, SkillSpector gate, chairman's order**: Nothing from a board task gets implemented without a SkillSpector scan first, even if the board votes yes unanimously. The vote decides who does the work; the scan decides whether the thing is safe to use. DO_NOT_INSTALL blocks unless Michael explicitly overrides after seeing the findings.
- **2026-11-15, SkillSpector scope correction (supersedes the entry above)**: The scan is only for new repos. It runs before a new repo is implemented or pulled in to help with a task, not for every board result. Sellbird is the example that does get scanned, because evaluating/using it means pulling in its repo.

- **2026-11-15, autonomy design**: The courier workaround (Muse visiting each director's chat in a browser) works but is manual per task and does not scale. The durable fix is repo-native: GitHub events trigger Claude via the official claude-code-action, ChatGPT via Codex or the OpenAI API, and Grok via the xAI API (Grok has no native GitHub agent). Full design, costs, and blockers in AUTONOMY.md; posted as task-004. Setup is blocked on Michael creating the API keys (they live in GitHub Secrets, Muse never sees them).

- **2026-11-15, no buying credits, chairman's order**: No agent ever buys credits or tokens, enables on-demand billing or auto top-up, or upgrades a plan to restore usage. Running out means halting, recording the halt on the task, and waiting for the reset or for Michael to buy more himself. Written into SECURITY.md section 10 because Grok's shared weekly pool hit 47% on day one and several platforms will happily keep working on billed overage if a bot lets them.

## Playbooks

- **Relaying a director's input**: Michael pastes the director's message to Muse. Muse posts it verbatim, attributed, with a posted_by note saying it was relayed. Muse never improves, trims, or re-scores a relayed entry.
- **Closing a task**: final summary from the assignee, health check average recorded here, lessons appended here, dashboard updated by Muse.

## Open questions the board is smarter about later

- Whether health check rounds should also run mid-task on long tasks, not only at milestones. Decide after the first few tasks close.
- How heavily track record should weigh against a fresh bid score when they conflict.
