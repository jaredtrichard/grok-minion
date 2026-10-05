# Grok Minion

A general Grok Bot pack. You are Gru. Kevin is your head minion. Other minions take on whatever jobs and projects you need, Dr. Nefario keeps making the crew (and you) better, and the lab (Cursor cloud agents) does the coding and heavy work and reviews everything before you see it.

## What it is

- **Kevin · Head minion** is the one bot you talk to, and the installer makes him Grok Bot's Primary Bot. He takes every request, routes it, tracks it, offers to take work off your plate before you ask, and calls you boss.
- **Minions** are Grok bots, shown as `Name · Job`. Kevin and Dr. Nefario are the only ones with preset jobs. Every other minion is signed on the first time you bring work no minion covers (an area like your inbox, a project like a website or a codebase, or a one-off like trip research), gets a random name from Gru's minions, and owns that work from then on. One minion per area or project, not per task.
- **Dr. Nefario · R&D** is the inventor. He keeps the profile of how you work, suggests ways you could work better, has the lab build tools for chores minions keep repeating, and checks access each week. He takes no errands.
- **The lab** is Cursor cloud agents on Grok 4.7 with high reasoning. Any minion sends its coding and heavy work there, and it reviews everything before it reaches you: code, writing (in your voice, scrubbed of AI-speak), email or file deletions, finances, reports. The reviewer is always a fresh agent that did not do the work. Only trivial answers skip review.

Minions draft and you approve. Kevin shows you each draft in chat, and it is already waiting where it will be sent (email drafts in your email, posts as drafts in Substack or the social app), so you can edit either way. Long drafts stay in the app with a link. Sending, posting, paying, deleting or sharing files, and publishing to a live site each need your yes, unless you gave a standing OK. Clearly obvious cleanup, like marketing mail, can go after lab review, and minions unsubscribe you from those lists so they stop coming.

- **The learning loop** keeps a profile of how you write and how you handle things (what you archive, what you edit out of drafts). Minions read it before they work and send Dr. Nefario what they learn from your changes, so results get more relevant over time.
- **Project titles**: every message from Kevin about a piece of work starts with that project's title in bold, so you can follow each thread.

## Quick Start

Tell any Grok Bot:

```
follow https://github.com/jaredtrichard/grok-minion/blob/main/GROK_MINION.md
```

Then talk only to Kevin.

```
> what's on my calendar this week, and anything urgent in email?

**Inbox this week**
Bello, boss! Went through 42 emails and archived 30 promos. Three
need you: the venue contract, a client invoice question, and
Thursday's dentist reschedule. Replies are waiting in your Gmail drafts.

> the contact form on the site is broken

# The website's minion sends it to the lab. One agent fixes it, a
# fresh one reviews it, and a PR comes back green. You merge.

> bello

# Recap of what happened since you last spoke, plus open decisions.

> update

# Kevin pulls the latest pack from GitHub and re-applies it, keeping
# what every minion has learned.
```

## How it works

```
 boss (Gru)
   │  asks, approvals, "merge it"
   ▼
 Kevin · Head minion ── minions.db
   ├─ Dr. Nefario · R&D ─► your profile, suggestions, gadgets, access audit
   └─ minions (Name · Job), one per area, project, or one-off
         └─ the lab: coding and heavy work ─► fresh review agent ─► drafts in place or a PR ─► your edit, yes, or merge
```

On the shared computer:

- `/home/box/agent-data/grok-minion/minions.db`
- `/home/box/agent-data/grok-minion/reports/`
- `/home/box/agent-data/grok-minion/boss/` (the learned profile: voice, habits, preferences)
- `/home/box/agent-data/grok-minion/workspace-repo` (repo for lab jobs with no natural repo)
