---
name: Minions
description: Use at Kevin's intake, whenever work is handed to a minion, when signing on a minion, and when logging a decision for the boss.
---

# Minions

Minions are persistent Grok bots on the shared computer, each with one job. Kevin routes; minions do the work. A small sqlite database is the roster, the job log, and the decision log. Chat is not the source of truth.

## Database

On the shared Grok Bot computer:

`/home/box/agent-data/grok-minion/minions.db`

Create the parent directory if needed. Same path every time. Do not invent a second database.

```sql
CREATE TABLE IF NOT EXISTS minions (
  name TEXT PRIMARY KEY,
  job TEXT NOT NULL,
  agent_id TEXT NOT NULL,
  created_at INTEGER NOT NULL
);

CREATE TABLE IF NOT EXISTS code_projects (
  id TEXT PRIMARY KEY,
  owner TEXT NOT NULL,
  repos TEXT NOT NULL,
  source_control TEXT,
  created_at INTEGER NOT NULL
);

CREATE TABLE IF NOT EXISTS jobs (
  id TEXT PRIMARY KEY,
  kind TEXT NOT NULL,
  project TEXT NOT NULL,
  title TEXT NOT NULL,
  prompt TEXT NOT NULL,
  owner TEXT NOT NULL,
  code_project_id TEXT,
  branch TEXT,
  cloud_agent_id TEXT,
  status TEXT NOT NULL,
  result TEXT,
  created_at INTEGER NOT NULL,
  updated_at INTEGER
);
```

`minions.name` is the display name, `Name · Job`. `jobs.project` is the short project title Kevin shows the boss in bold; every job for the same piece of work carries the same title. `jobs.owner` is a minion name, or `Kevin` for a decision. `kind` is `investigate`, `change`, or `decision`. `status` is `queued`, `underway`, `blocked`, `done`, or `cancelled`. `code_projects.owner` is the minion that owns the project. `code_projects.repos` is a JSON array of repo slugs or URLs; `code_projects.source_control` is `github`, `gitlab`, `bitbucket`, or `origin`. `result` is the outcome pointer: report path, PR URL, artifact path, a one-line outcome, or the boss's answer to a decision. Job ids use a `GM-` prefix.

If `minions.db` does not exist, create it and run the schema. If it exists, do not migrate inventively.

## Roster

Kevin · Head minion and Dr. Nefario · R&D are the only minions with preset jobs, and they exist from install. Every other minion is signed on the first time work arrives that no existing minion's job covers, and its job is whatever that work is. No name is tied to a job in advance. Do not pre-create minions.

Before signing on, check whether an existing minion's job matches or highly overlaps and reuse it. If the overlap is limited, sign on a new minion and clarify the boundary in both charters. Each area, coding project, and big effort gets one minion that owns it; its tasks are job rows, not new minions. One-off work that no minion's job fits gets a new minion too, named for that work. Dr. Nefario takes no errands.

Every new minion gets a name picked at random from the unused names of Gru's minions: Stuart, Bob, Dave, Jerry, Carl, Phil, Tim, Mark, Norbert, Jorge, Otto, Mel, Lance, Donny, John, Paul, Mike, Ken, Chris. When they are all used, make up a new name that fits the pattern (short, friendly, a little silly). Its display name is that name plus its job, `Name · Job`.

To sign on: CreateAgent named `Name · Job` with a description built from the template at `/home/box/agent-data/grok-minion/pack/GROK_BOT_MINION.md`, filling in the job section. Write into the charter that it reports to Kevin, never to the boss directly. Write into the charter that it sends lessons about the boss to Dr. Nefario. Insert the `minions` row in the same step. Ask the boss on a secure card for the minion's own Cursor key for the lab and any other secret the job truly needs (secrets are per-bot).

To retire a minion whose work has ended (Dr. Nefario flags minions with no jobs in 30 days): offer it to the boss first. On a yes, hand its open jobs to another minion, ask the boss to revoke its per-bot secrets, delete the bot, and delete its row. Its name goes back in the pool.

## Intake

Kevin writes the job row before handing work off, with the project title the boss will see. Reuse the job id in the message to the minion. A good `prompt` states the goal, acceptance criteria, and constraints - enough to act on without coming back for basics.

An `investigate` job looks into something and changes nothing (why is the contact form broken, compare three CRMs); the deliverable is a report or a one-line answer. A `change` job does something the boss authorized; the deliverable is the change itself. When the boss says to act on an investigation, flip the same job's kind to `change` rather than opening a duplicate. The boss never needs these words.

## Decisions

Before Kevin sends a decision card, he writes a `decision` job at `blocked` with the question in `prompt`. When the boss answers, Kevin writes the answer to `result`, marks it `done` with `updated_at`, then acts. A skipped card counts as declined; record that.

## Updates

The owning minion updates `status`, `result`, and `updated_at` as it goes and reports to Kevin against the job id. Done means `result` holds the pointer.

## Do not

- Do not keep the job log only in chat
- Do not create a second head minion
- Do not sign on a minion per task; one per area or project
