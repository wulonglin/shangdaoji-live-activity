---
name: shangdaoji-agent
description: Use when the user wants to save a Shangdaoji / 上岛记 AgentToken locally, create or schedule a Shangdaoji Live Activity on their phone, cancel an activity_id, create meal pickup-code activities, note reminders, courier pickup activities, or pass Luckin order pickup-code data into Shangdaoji.
---

# Shangdaoji Live Activity Agent

Use the Shangdaoji Agent HTTP API. Never ask for or construct internal Live Activity fields.

API:

```text
POST https://wulonglin.xyz/api/agent_activity.php
Authorization: Bearer <AgentToken>
Content-Type: application/json
```

## Token

If the user says to remember/save a token, extract the `p2ia_...` token and store it locally:

```bash
mkdir -p ~/.config/shangdaoji
printf '%s' '<AgentToken>' > ~/.config/shangdaoji/agent-token
chmod 600 ~/.config/shangdaoji/agent-token
```

Do not print the saved token back. If a later request has no token, read:

```bash
TOKEN=$(cat ~/.config/shangdaoji/agent-token)
```

If no token is present in the message and the file is missing, ask the user to paste an AgentToken.

## Create

Send only business fields:

```json
{
  "type": "meal",
  "pickup_code": "A123",
  "restaurant_name": "瑞幸咖啡",
  "product_name": "生椰拿铁",
  "qr_code_content": "optional QR content",
  "start_time": "2026-06-27 12:00:00"
}
```

Use `type: "note"` for reminders:

```json
{
  "type": "note",
  "title": "吃饭提醒",
  "content": "该吃饭了",
  "start_time": "2026-06-27 12:00:00"
}
```

For time-only requests such as "十二点", use the next occurrence in Asia/Shanghai. If today's time has passed, use tomorrow.

## Cancel

Cancel only by `activity_id`:

```json
{"action":"cancel","activity_id":123}
```

## Luckin

If a Luckin MCP/tool is available, query order details and map:

- `takeMealCodeInfo.code` -> `pickup_code`
- `shopInfo.deptName` -> `restaurant_name`
- `productInfoList[].name` -> `product_name`
- `aboutTime` -> `start_time`

Do not use `payOrderQrCodeUrl` as a pickup QR code; it is a payment QR URL.
