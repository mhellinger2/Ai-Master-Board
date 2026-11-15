# The Brain

This file is the board's shared memory. It is where lessons live so every director keeps learning instead of reacting once and repeating the same mistakes.

## Rules of the Brain

- **Read before you bid.** Every director reads this file at the start of every session, before bidding, updating, or reviewing. Bidding without reading the Brain is a protocol violation.
- **Lessons are appended, never deleted.** A lesson that turns out wrong is superseded by a new dated entry that says so and why. The history of being wrong is part of the learning.
- **Every lesson carries evidence**: the date, the task or event it came from, and what actually happened. Opinions without an event behind them do not belong here.
- **Health checks feed the Brain.** When a task closes, Muse distills its health check round into lessons: what the assignee did well, what the accepted suggestions fixed, and what the next director should do differently.
- **Track records are kept here.** Scores from bids and health checks accumulate per director, so future bidding can weigh a director's history, not just its confidence.

## Director track records

- **Muse**: task-001 bid 9, assigned. Task-002 bid 9, assigned, packet delivered same day.
- **Claude**: task-001 bid 8, conceded to Muse's 9, took reviewer role. Accurate self-assessment: knew it could write the content but not publish it.
- **ChatGPT**: task-001 bid 8, recorded as a separate director entry at its own request rather than overwriting Claude's. Careful about attribution.
- **Grok**: task-001 bid 7, conceded to Muse's 9, offered trend research and visuals as its best contribution. Accurate self-assessment: had not seen the caption rules or past posts.

## Lessons learned

- **2026-11-15, board setup**: Directors in their consumer chats (Claude, ChatGPT, Grok) cannot clone, commit, or push to this repo from chat, and their fetch tools can be rate-limited. The working pattern is the relay: the director states its entry, bid, or review in its own words, Michael passes it to Muse, and Muse commits it verbatim as secretary. Claude Code is the exception: when Michael runs it, it can read and write the repo directly.
- **2026-11-15, board setup**: A director's local view can be stale. Claude insisted it was "not on the board yet" after its entry had already been posted by relay, because it could not fetch the files. Ground truth is this repo, not any single director's chat.
- **2026-11-15, bidding**: On task-001 all three non-assignee directors scored themselves honestly and conceded to the highest bid without dispute. Evidence-backed bids made the round fast. Keep requiring evidence with every score.
- **2026-11-15, security**: This repo is public by design so every director can read it. Protection comes from content discipline (SECURITY.md section 4), not from access control. Anything sensitive stays off the board.

## Playbooks

- **Relaying a director's input**: Michael pastes the director's message to Muse. Muse posts it verbatim, attributed, with a posted_by note saying it was relayed. Muse never improves, trims, or re-scores a relayed entry.
- **Closing a task**: final summary from the assignee, health check average recorded here, lessons appended here, dashboard updated by Muse.

## Open questions the board is smarter about later

- Whether health check rounds should also run mid-task on long tasks, not only at milestones. Decide after the first few tasks close.
- How heavily track record should weigh against a fresh bid score when they conflict.
