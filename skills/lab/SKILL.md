---
name: The lab
description: Use whenever a minion sends a coding or heavy job to the lab (a Cursor cloud agent), and on every wake while that lab job is underway.
---

# The lab

The lab is Cursor cloud agents: ephemeral VMs that do code, review, and heavy research or building off the shared computer. Any minion sends its own coding and heavy jobs to the lab, and every minion uses the lab for review (see the Lab review skill). No Grok bot writes code; the minion supervises.

Lab agents get only the repo or material the job needs. Never give a lab agent email, bank, social, or other personal credentials, or the minion's own secrets; if a job seems to need them, the minion does that part itself within its own access, or asks Kevin.

Chat is not the source of truth. Lab jobs are rows in `minions.db` owned by the minion that sent them; the schema and path are in the Minions skill.

## Workspace repo

Every cloud job runs against a repo. Code work uses the repo it is about. A job with no natural repo (a non-code investigation, a document) uses the workspace repo in `/home/box/agent-data/grok-minion/workspace-repo`. If that file is missing on first need, ask Kevin for one decision card on which repo to use, check the boss's Cursor account can reach it, and write `owner/name` there.

## Launch

1. Kevin has written the job row. Read it and keep it current. A good `prompt` states the goal, acceptance criteria, and constraints - enough for the agent to act without coming back for basics.
2. Launch the cloud agent on Grok 4.7 with high reasoning. If Cursor's model list does not offer it, use Auto and tell Kevin once so he can tell the boss. Include the job id in the agent's task.
3. Record `cloud_agent_id`, set `underway`, and tell Kevin the job is under way against its id.

Investigate: the agent looks into it and returns a report. It does not push a fix. Save its final report to `/home/box/agent-data/grok-minion/reports/<job id>.md` and record that path in `result`.

Change (code): the agent implements on a branch, runs the project's tests, and pushes the branch. It does not open a pull request yet. Record `branch`. Then run the Lab review skill.

Change (non-code): the agent produces the requested artifact in the workspace repo. Record its path in `result`.

## Follow-through

Check every `underway` lab job on each wake: read its cloud agent's state and update the row. If Grok Bot supports scheduled wakes, keep one standing wake while any lab job is underway and drop it when none are. A standing wake with nothing new stays quiet.

- Agent finished an investigation: save the report, run the Lab review skill on it, mark `done`, report the finding to Kevin.
- Agent pushed a code branch: run the Lab review skill (a fresh lab agent). Loop auto-fix findings back to the same coding agent. When review is clean, have the same agent open the pull request, record the URL in `result`, and watch its checks.
- Checks red: send the failure back to the same cloud agent. Do not report a red PR as ready.
- Checks green: report the PR URL to Kevin, who brings it to the boss (merge, send back, or close).
- Merge only when Kevin relays the boss's explicit word, never while red. After it lands, mark the job `done`.
- Agent stuck, errored, or needs something only the boss has: mark `blocked` and report it to Kevin, who takes one decision card.
- Kevin relays a cancel: stop the cloud agent, close any open PR it raised, mark `cancelled`.

Send back: attach the boss's notes, relayed by Kevin, and hand them to the same cloud agent on the same branch and PR. Do not open a second job.

Update `status`, `result`, and `updated_at` as you go.

## Do not

- Do not do the job's work in this chat because you have a login or an open page
- Do not open a pull request to make a branch visible for review
- Do not keep the job log only in chat
