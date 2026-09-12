---
name: taskratchet-tasks
description: Use this skill when the user wants an agent to check on or manage their TaskRatchet tasks — a todo app where missing a deadline triggers a real money charge. Covers listing tasks, checking a task's status, creating a task, marking a task complete, marking a task incomplete, editing a task's deadline or stake amount, and "uncle" (giving up on a task and accepting the charge immediately). Use this whenever the user mentions TaskRatchet, their TaskRatchet tasks, or asks an agent to create/complete/manage a deadline-with-money-on-the-line task.
---

# TaskRatchet Tasks

TaskRatchet (https://taskratchet.com) is a todo list that charges real money when a task's deadline is missed. This skill lets an agent read and manage the current user's own tasks through TaskRatchet's API.

## Setup

Authenticate with a TaskRatchet API key, read from the `TASKRATCHET_API_KEY` environment variable. Send it as:

```
Authorization: ApiKey-v2 <the key>
```

If `TASKRATCHET_API_KEY` isn't set, don't guess or ask the user to paste a key into the conversation. Tell them to generate one under **API Token** on their account settings page (https://app.taskratchet.com/settings), set it as `TASKRATCHET_API_KEY` in their environment, and try again.

## API reference

Base URL: `https://api.taskratchet.com`

Don't rely on a fixed list of endpoints or request/response shapes here — fetch the live OpenAPI spec at `https://api.taskratchet.com/api2/openapi.json` (or read the human-oriented reference at `https://docs.taskratchet.com/api-reference.html`) before making calls, and follow what it says. The spec is the source of truth; this skill only tells you what the user can do and when you must stop and ask first.

## What the user can do to a task

- **List tasks / check status** — read-only.
- **Create a task** — sets a deadline and a stake (the amount charged if the deadline is missed).
- **Mark a task complete** — read-only in effect: it can only prevent a charge, never cause one.
- **Mark a task incomplete** — undoes a completion, which can put the task back at risk of a charge.
- **Edit a task's deadline or stake amount**.
- **"Uncle" a task** — give up on it and accept the charge immediately, without waiting for the deadline.

## Consent gate — read this before calling anything

Creating a task, editing one, marking one incomplete, or uncle-ing one all change what the user is financially on the hook for. Before calling any of those four, **stop and ask the user to explicitly confirm that specific action**, stating in plain terms what will happen — the exact stake amount and deadline for a create/edit, or the exact charge amount for an uncle. Proceed only after they say yes to *that* action.

- **Never bundle confirmation.** A single "ok to proceed?" covering several actions at once does not count — confirm each mutating action individually, one at a time, even if the user asked for several in one message.
- **Listing, reading, and marking complete need no confirmation.** They're either read-only or can only reduce what the user owes.
- The only exception to asking every time is a standing arrangement the user has explicitly and separately set up with you in advance (e.g., "you may always mark my recurring habit tasks complete without asking"). Default to asking.

## Working with task IDs

Don't guess a task ID. If you don't already have it from a prior call in this conversation, list the user's tasks first and match by name/description before acting on one.
