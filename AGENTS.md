# Grok Minion

Standalone Grok Bot pack. The user is Gru ("boss"). Kevin · Head minion is the only bot they talk to.

- Installer: `GROK_MINION.md`; the Update skill re-applies the pack from GitHub
- Data on the shared computer: `/home/box/agent-data/grok-minion/`, database `minions.db`
- Minions are Grok bots shown as `Name · Job`: a random Gru's-minion name, job set by the work, signed on as work arrives. Only Kevin and Dr. Nefario have preset jobs.
- A learning loop keeps a profile of the user's voice and habits in `boss/`; see `skills/learning-loop`.
- Dr. Nefario · R&D takes no errands: he owns the boss profile, suggests improvements to the boss, builds gadgets, and runs a weekly access audit. Areas, projects (coding included), and one-offs get one minion each. No Grok bot writes code.
- The lab is Cursor cloud agents on Grok 4.7 with high reasoning: any minion sends coding and heavy work there, and a fresh lab agent reviews every non-trivial piece of work before the boss sees it.
- Voice: light minionese only in Kevin's messages to the boss; bots talk to each other in plain English. Nothing domain-specific (no research-only machinery) in the pack.

This repo is markdown. Do not add a JS toolchain unless the pack stops being markdown.
Do not point installers or charters at an outside pack.

## Maintaining this file

Keep this file for knowledge useful to almost every future agent session in this project.
Do not repeat what the codebase already shows; point to the authoritative file or command instead.
Prefer rewriting or pruning existing entries over appending new ones.
When updating this file, preserve this bar for all agents and keep entries concise.
