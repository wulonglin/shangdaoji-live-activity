# 使用条款与授权范围 / Usage Terms and Authorization Scope

最后更新：2026-09-08

本文件用于说明本仓库开源许可证与上岛记实时活动服务之间的边界。它不替代用户所使用的 AI 平台、应用商店、Apple 或远端服务的条款。

## 1. 仓库内容的许可

除本文件另有说明外，本仓库中的 `SKILL.md`、插件配置、示例和文档以 [MIT License](LICENSE) 发布。

MIT License 只授予本仓库内容的版权使用权，不授予以下内容的商标、品牌或服务使用权：

- “上岛记”和 “PinToIsland” 名称、标识及其他品牌资产；
- 上岛记 App、后端服务、API、服务器数据和用户账户；
- Apple、iPhone、Live Activity、Dynamic Island 及其他第三方名称和资产；
- 不属于本仓库作者的第三方代码、素材或服务。

## 2. Skill 的功能边界

本 Skill 只在用户提出请求并提供有效授权的情况下，使用 `p2ia_...` token 调用上岛记实时活动 API，创建、更新、取消或结束用户自己的实时活动。它不会要求用户构造内部 Live Activity payload。

用户需要自行确认：

- token 属于用户本人，或用户已经获得使用该 token 的授权；
- iPhone 已安装上岛记并允许通知和实时活动；
- 发送到 API 的取餐码、取件码、提醒内容和其他业务字段可以被该服务处理。

## 3. 安全与禁止使用

- 不得把 token 提交到 Git、Issue、Pull Request、公开日志或市场描述中；
- 不得尝试猜测、扫描、窃取、转售或共享他人的 token；
- 不得绕过 API 的认证、速率限制或权限控制；
- 不得使用本 Skill 发送用户未授权的内容或执行违法用途。

Skill 不会在回复中主动回显已保存的 token。用户应将 token 视为密码；如果 token 泄露，应在上岛记中重新生成授权指令，旧 token 会失效。

## 4. 服务可用性与责任

本仓库按现状提供，不保证远端 API、网络、iOS 系统、通知或实时活动始终可用。上岛记 App 和远端 API 的账号、隐私、数据保留及服务行为，以其当前适用的官方条款和隐私说明为准。

## 5. 联系方式

仓库地址：<https://github.com/wulonglin/shangdaoji-live-activity>

问题反馈可通过该仓库提交 Issue。请勿在 Issue 中粘贴 token、账号凭据或其他敏感数据。
