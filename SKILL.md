---
name: shangdaoji-live-activity
description: 通过上岛记将取餐码、取件码和提醒事项发送到 iPhone 实时活动与灵动岛，支持保存授权 token、创建、更新、取消或结束活动。
---

# 上岛记实时活动

使用 PinToIsland / 上岛记实时活动 HTTP API。只发送取餐码、取件码、提醒事项等业务字段，不要要求用户提供或自行构造 Live Activity 内部字段。

接口：

```text
POST https://wulonglin.xyz/api/agent_activity.php
Authorization: Bearer <p2ia_token>
Content-Type: application/json
```

## 授权 Token

当用户要求记住或保存 token 时，从内容中提取 `p2ia_...` token 并保存到本地：

```bash
mkdir -p ~/.config/shangdaoji
printf '%s' '<p2ia_token>' > ~/.config/shangdaoji/agent-token
chmod 600 ~/.config/shangdaoji/agent-token
```

不要把已保存的 token 回显给用户。后续请求没有提供 token 时，读取本地保存值：

```bash
TOKEN=$(cat ~/.config/shangdaoji/agent-token)
```

如果当前消息没有 token，且本地文件不存在，请让用户粘贴 `p2ia_...` token。

## 更新偏好

如果用户要求记住更新行为偏好，保存到：

```bash
mkdir -p ~/.config/shangdaoji
printf '%s\n' '{"update_priority":5,"update_sound":false}' > ~/.config/shangdaoji/preferences.json
chmod 600 ~/.config/shangdaoji/preferences.json
```

频繁或静默更新使用 `update_priority: 5`。当用户要求提醒、声音或灵动岛弹出时，使用 `update_sound: true`，带声音的更新使用 priority 10。

发送更新请求前，如果 `~/.config/shangdaoji/preferences.json` 存在，应读取其中配置并带上 `priority` 和 `sound`，除非用户在当前请求中明确覆盖。

## 创建实时活动

只发送业务字段：

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

提醒事项使用 `type: "note"`：

```json
{
  "type": "note",
  "title": "会议出发提醒",
  "content": "14:00 在滨江会议室开会，建议 13:30 出发。",
  "noteIcon": "🚕",
  "start_time": "YYYY-MM-DD 13:30:00"
}
```

当用户提供图标，或提醒类型明显对应某个图标时，使用 `noteIcon` 作为提醒图标。提醒标题 `title` 默认使用简洁纯文本，尽量不要包含 emoji；图标统一放在 `noteIcon`。已经使用 `noteIcon` 时，不要在 `title` 开头重复同一个 emoji。只有用户明确要求标题包含 emoji 时才保留。

Agent 生成的每日简报或轻量提醒也使用 `type: "note"`，例如天气早报、出发提醒、喝水提醒、用药提醒、每日清单等。示例：

```json
{
  "type": "note",
  "title": "每日天气早报",
  "content": "今天多云转小雨，18-24°C，午后降雨概率 70%，出门带伞。",
  "noteIcon": "🌦️",
  "start_time": "YYYY-MM-DD 07:30:00"
}
```

将 `YYYY-MM-DD 07:30:00` 替换为用户下一次简报日期。如果用户要求每日简报，每天创建下一次 07:30 的实时活动。

对于“十二点”这类只有时间的请求，使用 Asia/Shanghai 时区下的下一次出现时间。如果今天该时间已过，则使用明天。

## 取消与结束的意图判断

当用户说“取消/关掉/结束/清掉这个实时活动”，且目标是已经出现在锁屏或灵动岛上的卡片时，使用 `action: "end"`，不要使用 `cancel`。

`cancel` 只用于尚未开始、还在排程中的任务。

## 取消未开始排程

只能通过 `activity_id` 取消尚未开始的任务：

```json
{"action":"cancel","activity_id":123}
```

如果对已经启动的活动调用 `cancel`，可能返回找不到任务或无法取消；这不代表活动已经结束，应改用 `end`。

## 结束已启动活动

只能通过 `activity_id` 结束已经开始的实时活动：

```json
{"action":"end","activity_id":123}
```

结束实时活动要求 App 已上传 activity end token。如果 API 返回 `activity_end_token_missing`，说明无法远程结束该活动；需要用户在 iPhone 上手动划掉，或先打开 App 让它上传 end token 后再重试。

如果 API 返回 410，说明该活动已经在设备上消失，无需继续处理。

## 更新

实时活动开始后，只能通过创建时返回的 `activity_id` 更新：

```json
{
  "action": "update",
  "activity_id": 123,
  "type": "meal",
  "pickup_code": "B456",
  "restaurant_name": "瑞幸咖啡",
  "product_name": "生椰拿铁",
  "priority": 5
}
```

频繁静默更新使用 `priority: 5`。需要提醒或声音时使用 `sound: true`，这会使用 priority 10。可选提醒文本字段为 `alert_title` 和 `alert_body`。

如果 API 返回 `activity_update_token_missing`，说明活动尚未开始，或 App 还没有上传 update token。

## 瑞幸订单

如果可用的 Luckin MCP/tool 存在，查询订单详情后按以下字段映射：

- `takeMealCodeInfo.code` -> `pickup_code`
- `shopInfo.deptName` -> `restaurant_name`
- `productInfoList[].name` -> `product_name`
- `aboutTime` -> `start_time`

不要把 `payOrderQrCodeUrl` 当作取餐二维码；它是支付二维码链接。
