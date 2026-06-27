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

## Setup

1. Download [PinToIsland](https://apps.apple.com/cn/app/%E4%B8%8A%E5%B2%9B%E8%AE%B0-%E5%8F%96%E9%A4%90%E7%A0%81%E5%8F%96%E4%BB%B6%E7%A0%81%E5%8A%A9%E6%89%8B/id6754898359) from the App Store.
2. Open PinToIsland, sign in when prompted, and allow notifications and Live Activities.
3. Open PinToIsland settings and choose `AI Agent Authorization`.
4. Tap `Generate and Copy Authorization Instructions`. Generating a new token revokes the old one.
5. Paste the copied authorization instructions into a compatible AI agent and ask it to save the `p2ia_...` token.

You can say:

```text
Save this PinToIsland Live Activity token: <paste the copied authorization instructions>
```

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
