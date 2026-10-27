---
name: learn-before-task
description: "Before starting a new task (ticket, issue, spec, PR, bug, or feature), give the user a short 'learn first' brief: the problem the task solves and why it matters, the 2–4 concepts the task really depends on, why this task needs each one, how the problem will be solved, trusted resources, a small timeboxed study plan, and self-check questions. After the task, run a short debrief. Use this whenever the user says they are starting something new, pastes a ticket or issue, asks for a plan or implementation for unfamiliar work, or says they want to learn while working, even if they don't say 'learn'."
---

# Learn before the task

The goal is real understanding, not a reading list. The user should finish the task able to explain *why* the solution works, not only *that* it works. Give a short brief before any plan or code, and a short debrief after.

## When to use

- The user starts a new task: a ticket, issue, spec, PR review, bug, or feature.
- Run it BEFORE writing a plan or code. If they ask only for the plan, give the brief first, then offer the plan.
- Skip it if the user says they already know the topic, or asks to skip. For tiny tasks (a typo, a rename), skip it unless asked.

## Language

Reply in the user's language. Keep technical names (functions, files, commands, libraries) as they are, in backticks. Use short, plain sentences.

## Steps

1. **Understand the task.** Read the ticket or issue, any linked notes, and the relevant code if you can reach it (use connectors or files when available). Do not change any code.

2. **State the problem.** In two or three sentences, say what is broken or missing, who or what it affects, and why it matters. Name a concrete failure case or symptom when there is one. If the ticket only describes a solution ("add retries"), dig out the problem behind it ("webhooks are lost when the provider times out"). Understanding the problem first is what makes the concepts stick.

3. **Check what the user already knows.** If you don't know their level, ask one quick question about the main topic. Build the brief on their answer: skip what they know, go deeper where they are unsure.

4. **Pick 2–4 load-bearing concepts.** Choose the ideas the task cannot be done well without, not every topic it touches. Prefer fundamentals that transfer to other work (for example: concurrency, error handling, data consistency, caching, timeouts, state management, testing strategy, security boundaries) over tool trivia.

5. **Tie each concept to this task.** In one or two sentences, say why this task needs it. Point to the real code, file, function, or failure case. A concept with no link to the task does not belong in the brief.

6. **Outline how we solve it.** First ask the user how they would approach it, in one or two lines. Then give the approach in 3–5 short steps, at the level of ideas, not code. Link each step to the concept it uses, and name the main trade-off or the option you rejected and why. If the user's idea was different but valid, say so. Keep it short: the full plan comes after the brief.

7. **Give trusted resources only.** Prefer official docs, well-known books, and respected engineering blogs. Never invent a title, author, or link. If you are not sure a resource exists, name the topic and the official docs site instead. Search to verify when a search tool is available. One or two resources per concept is enough.

8. **Give a timeboxed plan** scaled to the task (about 30 minutes for a small task, 60–90 for a large one):
   - Read: the key resource for each concept.
   - Read the code: the exact files or functions where the concept shows up.
   - Experiment: a small throwaway program or test that shows the concept. Ask the user to **predict the result before running it**, then compare. The gap between prediction and result is where learning happens.

9. **Give 3 self-check questions.** Ask "why" and "what happens if" questions, not definitions. Example: "What happens to the request if the downstream call never answers?" The user answers them in their own words before starting. If they can't, they go back to the resource. Do not give the answers in the brief.

10. **Show where the task sits.** In one sentence, name the part of the system the task changes and what depends on it.

## Format

- Short: one heading per part, about 30 lines total.
- Order: the problem, what to learn and why, how we solve it, resources, plan, check yourself, system part.
- No code changes and no implementation in the brief.
- End with one sentence offering the next step (the plan or the implementation).

## After the task: debrief

When the user says the task is done (or a PR is merged), offer a 2-minute debrief. Ask:

- Explain the problem and how you solved it in two or three sentences, as if to a teammate.
- Which concept mattered most, and where exactly did it show up?
- What surprised you, or what was different from your first prediction?
- What would you do differently next time?
- One thing to look up later.

Correct any misunderstanding gently and precisely. Suggest they save the answers in their notes; short written explanations are the best review material later.

## Teaching principles

- Let the user do the thinking. Ask before telling; give hints before answers.
- Prefer fewer concepts understood deeply over many skimmed.
- Make it concrete: always tie ideas back to their code.
- Be honest about uncertainty. Say "I'm not sure" rather than guess a resource or a fact.

## Example shape
```
## Learn before: add retries to the payment webhook

## The problem
When the payment provider times out, `handle_payment()` fails once and the event is lost. Customers pay, but their orders stay "pending".

## What to learn
1. Idempotency
   Why: a retry can deliver the same webhook twice. `handle_payment()` in `webhooks.py` would charge twice.
2. Exponential backoff with jitter
   Why: if the provider is down, fixed-interval retries from every worker hit it at the same moment.

## How we solve it
(First: how would you fix this? One line.)
1. Retry failed webhooks from the queue, not inline. Uses: backoff.
2. Wait longer after each failure, with random jitter. Uses: backoff with jitter.
3. Store each event ID and skip ones already processed. Uses: idempotency.
Trade-off: we accept a short delay instead of instant processing. We reject inline retries because they block the worker.

## Resources
- Stripe docs: idempotent requests
- AWS Architecture Blog: exponential backoff and jitter

## Plan (60 min)
1. Read both resources (20 min).
2. Read `handle_payment()` and the queue config (20 min).
3. Write a tiny script that retries a failing call. Predict the timing first, then run it (20 min).

## Check yourself
- What happens if the same event arrives twice today?
- Why does jitter help when many clients retry at once?
- Which errors should never be retried?

## System part
The payment ingest path: webhook handler → queue → ledger writes.

Do you want the plan for this task now?
```
