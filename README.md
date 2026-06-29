---
title: 📚 Sunny 持续知识库
created: 2026-06-29
updated: 2026-06-29
type: index
tags: [知识库, MOC]
---

# Sunny 持续知识库

> **目的**：把所有跨设备、跨时间的"知道一次、忘掉可惜"的事都沉淀在这里。**每周 5 分钟看一遍**就能拾起上下文。
>
> **2026-06-29 更新**: 加入**持续收口**机制 —— 3 个入口、cron 自动抓 spark/公众号/vault 状态、Inbox 收口指南。

## 🗺️ 知识库地图

### 00-Inbox (临时) - **🔥 快速捕获**
**3 个入口**:
- 终端: `inbox "你的想法"` (软链 `~/bin/inbox`)
- 微信: 跟 Hermes 说 "记录: 想法"
- GUI: Cmd+P → Open Inbox

**自动收口** (每 6h):
- spark 状态 → `99-Spark-Sync/设备状态.md`
- 公众号发文 → `04-Resources/公众号发文明细.md`
- vault 统计 → `📈 vault 统计.md`
- Inbox 满 5/10/15...条 → 自动加提醒

**处理节奏**: 每天 12:00 + 22:00 各清一次（3 分钟）
详见 [[00-Inbox/收口指南]]

### 01-Daily (日记)
- `{{date}}.md` — 每天一篇，记录**今日做了什么 + 学到什么 + 待办**
- 自动模板（见 `_templates/今日工作.md`）
- cron 每天 23:55 自动归档（待配）

### 02-Projects (项目)
**有目标、有截止日期**的事。当前 4 个：
- `公众号-「NAS村长」运营` — 日更 3 篇
- `Hermes Agent 部署与运维` — mac mini + spark 双节点
- `spark-imagegen 生图自动化` — ComfyUI 调用链
- `个人 AI 工具链研究` — 给读者的选题源

### 03-Areas (领域)
**持续关注、无截止**的事：
- `NAS 私有云`
- `AI Agent 框架`（Hermes / OpenClaw / Superpowers）
- `本地大模型部署`（Ollama / ComfyUI）
- `公众号内容生产`

### 04-Resources (速查)
**不消化、只查**的事实：API 端点、设备 IP、launchd plist 位置、crontab 模板。

### 05-Archive (归档)
已结束项目的总结存档（不删，留作复盘）。

### 99-Spark-Sync (spark 自动同步)
从 dgx spark 自动 dump 过来的关键信息：systemd 状态、GPU、ComfyUI 状态、Hermes 网关状态。

## 🛠️ 工具链

| 工具 | 用途 |
|---|---|
| **Hermes Agent** | 主 AI，自动同步 spark / 写公众号 / 操作 Obsidian |
| **spark-imagegen** | ComfyUI 生图（NAS 村长封面/配图） |
| **launchd** | Mac 守护（hermes-gateway, daily-cleanup） |
| **systemd** | spark 守护（comfyui, hermes-gateway） |
| **git** | vault 跨设备同步（待配） |

## 🔄 每日工作流

```
07:00  看 01-Daily/昨天  ← 重启上下文
08:00  公众号三篇  ← spark-imagegen + nas-daily-publisher
       期间想法 → 00-Inbox/
12:00  处理 Inbox（移到 Areas/Resources/Projects）
22:00  写 01-Daily/今天 + 归档
23:55  cron 自动写总结
```

## 📌 标签约定

- `#项目/公众号` — 公众号相关
- `#领域/NAS` — NAS 领域
- `#状态/进行中` `#状态/已归档`
- `#优先级/A` `#优先级/B` `#优先级/C`

## 🔗 关联

- Vault 路径: `/Users/sunny/Documents/Obsidian Vault`
- 知识库速记: `~/nas_knowledge/`（已用）
- spark 同步: `99-Spark-Sync/`
