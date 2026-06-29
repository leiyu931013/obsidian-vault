---
title: "🤖 AI Agent 框架"
created: 2026-06-29
updated: 2026-06-29
type: area
tags: [area, AI, agent, 公众号]
---

# 🤖 AI Agent 框架

> Hermes 是核心，但需要知道同行在做什么（公众号读者关心对比）

## 📚 主流框架（GitHub Top）

| 项目 | Stars (2026-06) | 特点 | 部署 |
|---|---|---|---|
| **Hermes Agent** | ~200K | 多模态 + Web UI + 多模型 | mac mini + spark |
| **OpenClaw** | ~380K | 跨平台个人 AI 助手 (Discord/Telegram/WhatsApp) | 服务器 |
| **Superpowers** | ~236K | Agent 技能即插即用 (50+ 预置技能) | 配合 Claude Code |
| **AutoGPT** | ~185K | 自主 Agent 开创者，任务拆解 | 本地/云 |
| **Claude Code** | ~131K | Anthropic 官方终端 AI 编程助手 | 终端 |
| **ECC** | ~204K | Agent 性能优化，省 40-60% Token | 任意 Agent |

## 📈 Sunny 自己的栈

### Hermes Agent (核心)
- 路径: `~/.hermes/`
- 模型: `minimax-cn/MiniMax-M3` (200k context)
- 网关: 微信 iLink 平台 (`ilinkai.weixin.qq.com`)
- mac mini systemd launchd 守护
- spark 端 systemd user unit 守护 (Linger=yes)

### spark-imagegen
- Hermes skill 镜像 + ComfyUI 调用链
- 模型: `z_image_turbo_bf16.safetensors` (ComfyUI 0.25.1)
- 走 diffusion_models/ + UNETLoader (不是 DiffusionModelLoader)
- 130GB GB10 GPU，7-8s 出图
- 自动归档到 `~/nas_knowledge/2026-06-images/`

## 💡 选题源

- 框架对比文章（Hermes vs OpenClaw vs Superpowers）
- Hermes 部署实战教程
- Agent 技能系统深度文
- Token 优化（用 ECC）
- 本地模型 vs 云端

## 🔗 关联

- [[02-Projects/Hermes Agent 部署与运维]]]]
- [[02-Projects/公众号-「NAS村长」运营]]]]
- [[04-Resources/设备清单]]]]

## 🏷 标签

`#area/AI` `#area/agent` `#公众号/AI工具`
