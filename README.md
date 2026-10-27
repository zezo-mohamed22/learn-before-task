# learn-before-task

A [Claude skill](https://docs.claude.com/en/docs/agents-and-tools/agent-skills/overview) that turns every task into a learning opportunity.

Before you start a ticket, issue, bug, or feature, Claude gives you a short **"learn first" brief**:

- The 2–4 concepts the task really depends on, and *why* this task needs each one (tied to your actual code)
- Trusted resources only — no invented links
- A small timeboxed plan: read → read the code → predict-then-run experiment
- Self-check questions you answer in your own words before you start

When you finish, Claude offers a short **debrief** so the knowledge sticks.

## Why

It's easy to finish a task with AI help and learn nothing. This skill is built on a few ideas that make learning actually work:

- **Prior knowledge first** — Claude checks what you already know and skips it.
- **Few concepts, deeply** — only the ones the task can't be done without.
- **Concrete over abstract** — every concept is linked to a file, function, or failure case.
- **Prediction** — you guess the result of an experiment before running it.
- **Retrieval & explanation** — "why" questions before, a teach-back after.

## Install

**Claude.ai / Claude app:** zip the `learn-before-task` folder and upload it under Settings → Capabilities → Skills.

**Claude Code:** copy the folder into `~/.claude/skills/` (personal) or `.claude/skills/` in your repo (project).

## Usage

Just start a task:

> I'm starting this issue: add rate limiting to the public API

or say "skip the brief" when you already know the topic.

## Customize

Edit `SKILL.md` to fit you:

- **Concepts** — change the example list in step 3 to your domain (frontend, data, mobile, ML…).
- **Resources** — add the docs, books, or internal notes you trust in step 5.
- **Time budget** — adjust the plan length in step 6.
- **Notes** — point the debrief to your notes folder or study tracker.

## License

MIT
