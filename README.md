# 上岛记实时活动 AI 代理技能 / PinToIsland Live Activity Agent Skill

[English](README.en.md) | 简体中文

## 中文

这是一个 AI 代理技能，用于通过上岛记（PinToIsland）实时活动 API 在 iPhone 上创建、定时或取消实时活动，适合把取餐码、取件码、瑞幸订单和提醒展示到 Live Activity / 灵动岛。

它只发送业务字段，例如取餐码、店名、商品名、提醒标题和提醒内容；不会要求用户填写或构造内部 Live Activity payload。

### 能做什么

- 保存本地 `p2ia_...` token
- 创建取餐码实时活动
- 创建提醒类实时活动
- 通过 `activity_id` 取消实时活动
- 将瑞幸订单信息映射为上岛记取餐码活动

### 使用方式

把这个仓库安装到支持 skill 的 AI agent 后，可以直接说：

```text
用上岛记创建一个取餐码 A123，店名瑞幸咖啡
```

或：

```text
用上岛记十二点提醒我吃饭
```

首次使用需要提供一个 `p2ia_...` 格式的 token。
