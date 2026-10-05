---
name: Update
description: Use when the boss asks to update Grok Minion. Pulls the latest pack from GitHub and re-applies it to Kevin, Dr. Nefario, every minion, and the global workflows, keeping what each bot has learned.
---

# Update

Kevin runs this when the boss says "update" or asks to update Grok Minion. It refreshes the pack from its GitHub repo and re-applies it everywhere, without losing what the crew has learned.

## Source

`/home/box/agent-data/grok-minion/pack-source` holds the pack's repo URL and the commit last installed, one per line. The installer writes it. If it is missing, use `https://github.com/jaredtrichard/grok-minion` and treat every file as changed.

## Steps

1. Fetch the latest default branch of the source repo into a temporary directory on the shared computer. If the commit matches the one in `pack-source`, tell the boss the pack is already current and stop.
2. Read the commit log between the installed commit and the new one. Keep it for the summary.
3. Replace `/home/box/agent-data/grok-minion/pack/` with the new files.
4. Kevin: replace your own description with the new `GROK_BOT_KEVIN.md`, then put back your current Learning notes section.
5. Dr. Nefario: replace his description with the new `GROK_BOT_NEFARIO.md`, then put back his Learning notes.
6. Each minion in the `minions` table: rebuild its description from the new `GROK_BOT_MINION.md`, then put back its Name and job, Standing approvals, and Learning notes sections exactly as they were.
7. Rewrite every global workflow from the new skill files, using each skill's description line. Add workflows for new skills. For a skill the new pack no longer has, tell the boss its workflow can be removed; do not delete it yourself.
8. Run the Minions skill's schema against `minions.db`; `CREATE TABLE IF NOT EXISTS` adds new tables. If an existing table's columns or allowed values changed, do not migrate on your own: bring it to the boss as one decision card with the exact change.
9. Write the new commit to `pack-source`.
10. Tell the boss what changed, in a few plain lines drawn from the commit log, and anything they need to do (a new login, a workflow to remove, a decision).

## Do not

- Do not overwrite a bot's Learning notes, Standing approvals, or Name and job
- Do not touch the boss profile, the job log, or reports
- Do not create, rename, or delete bots during an update, except as the new pack's notes say and with the boss's yes
