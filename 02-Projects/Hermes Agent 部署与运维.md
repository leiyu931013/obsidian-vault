---
title: "🔧 Hermes Agent 部署与运维"
created: 2026-06-29
updated: 2026-06-29
status: 进行中
priority: A
type: project
tags: [project, hermes, devops]
---

# 🔧 Hermes Agent 部署与运维

> **目标**：mac mini + spark 双节点稳定跑，微信 iLink 24/7 在线

## 📅 时间线

- **开始**: 2026 年初
- **状态**: 稳定运行中
- **上线**: 2026-06-22 (launchd 接管) / 2026-06-22 (spark systemd 接管)

## 🎯 关键结果

- [x] mac mini launchd 守护上线
- [x] spark systemd user 守护上线
- [x] Linger=yes (用户未登录也跑)
- [x] 微信 iLink 持续连接
- [ ] spark 端 LLM fallback (目前只有 minimax-cn)
- [ ] 群晖 DS925+ 部署 (Docker)

## 📋 任务分解

### mac mini
- [x] 修复 config.yaml provider (`minimax` → `minimax-cn`)
- [x] launchd plist + wrapper script (`hermes_gateway.sh`)
- [x] daily-cleanup cron (3:30)
- [x] daily cleanup: WAL checkpoint 验证
- [ ] 群晖/Docker 化部署

### spark
- [x] ComfyUI systemd unit (`comfyui.service`)
- [x] Hermes gateway systemd unit
- [x] Linger=yes 验证
- [x] `python3 main.py ...` 用 system python (无 venv)
- [x] pip3 install 补全 (sqlalchemy, filelock, requirements.txt)
- [ ] 加 fallback LLM provider
- [ ] Ollama 部署 (本地大模型)

## 📝 重要决策

### 2026-06-22 — 修复 WeChat 401
3 层根因：
1. `config.yaml` provider 错配 (`minimax` 不存在，应为 `minimax-cn`)
2. Hermes Studio spawn gateway 不继承 .env
3. `auth.json` 的 `minimax` 状态 = `exhausted`

修法：切换 provider + launchd wrapper 注入 .env

### 2026-06-22 — enable-linger
**不需要 sudo**！`loginctl enable-linger new` 直接成功。

### 2026-06-22 — ComfyUI systemd 守护
- 用了 `python3 main.py ...` + StandardOutput=append
- Restart=on-failure + RestartSec=10
- Linger=yes 后用户未登录也跑

## 🔗 关联

- [[04-Resources/设备清单]]]]
- [[04-Resources/守护进程速查]]
- [[04-Resources/API 与密钥位置]]
- [[02-Projects/spark-imagegen 部署]]]]
- [[99-Spark-Sync/Hermes 状态]]

## 🏷 标签

`#project/hermes` `#优先级/A`
