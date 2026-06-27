# PinToIsland Live Activity Agent Skill

English | [简体中文](README.md)

This is an AI agent skill for creating, scheduling, and canceling PinToIsland Live Activities on iPhone through the PinToIsland Live Activity API. It helps show pickup codes, courier pickup codes, Luckin orders, and reminders in Live Activity / Dynamic Island.

It sends only business fields such as pickup codes, store names, product names, reminder titles, and reminder content. It does not ask users to fill in or construct internal Live Activity payloads.

## What It Does

- Saves a local `p2ia_...` token
- Creates meal pickup-code Live Activities
- Creates reminder Live Activities
- Cancels Live Activities by `activity_id`
- Maps Luckin order details into PinToIsland pickup-code activities

## Usage

After installing this repository as a skill in Codex or another compatible agent, ask:

```text
Use PinToIsland to create pickup code A123 for Luckin Coffee.
```

Or:

```text
Use PinToIsland to remind me to eat at noon.
```

First use requires a token in the `p2ia_...` format.
