---
title: "🔑 API 与密钥位置"
created: 2026-06-29
updated: 2026-06-29
type: resource
tags: [resource, API, security]
---

# 🔑 API 与密钥位置

> 实际值在 `.env`，**本文档不存密钥**。只指位置和用途。

## Mac mini

`~/.hermes/.env` (mode 600, 18 个 key)

| 变量 | 用途 |
|---|---|
| `MINIMAX_CN_API_KEY` | 主 LLM provider |
| `MINIMAX_API_KEY` | 备用 (未使用) |
| `WEIXIN_TOKEN` / `WEIXIN_*` | 微信 iLink 平台 |
| `OPENROUTER_API_KEY` | 聚合多模型 (未用, 备选) |
| `HERMES_HOME` | Hermes 根目录 |

## dgx spark

`/home/new/.hermes/.env` (mode 600, 18 个 key)

| 变量 | 用途 |
|---|---|
| `MINIMAX_CN_API_KEY` | 主 LLM |
| `WEIXIN_*` | 微信 iLink |

## ⚠️ 安全原则

- **绝不 commit `.env`** 到 git
- Obsidian 知识库**不存**任何明文密钥
- 用 1Password / Keychain 同步 (未启, 待配)

## 端点速查

| 端点 | 用途 |
|---|---|
| `https://api.minimaxi.com/anthropic` | Hermes 默认 LLM |
| `https://ilinkai.weixin.qq.com` | 微信 iLink 平台 |
| `https://mp.weixin.qq.com/cgi-bin/` | 微信公众号 API |
| `http://192.168.21.191:8188` | spark ComfyUI |
| `http://169.254.83.107:8748` | Mac mini Hermes Gateway |

## 🔗 关联

- [[04-Resources/设备清单]]]]
- [[02-Projects/公众号-「NAS村长」运营]]]]
