---
title: "🧠 spark 本地 AI 栈索引"
created: 2026-06-29
type: spark-sync
tags: [spark, AI, index]
---

# 🧠 spark 本地 AI 栈索引

> **同步源**: `spark:/home/new/.hermes/memories/` (MEMORY.md + USER.md)
> **同步时间**: 2026-06-29
> **说明**: spark 上跑的是独立 AI 栈（和 Mac mini 不同），本笔记是其索引

## 📚 关键信息

### OpenCode + Claude Code
- **OpenCode 1.17.10** → 直连 Ollama
- **Claude Code 2.1.191** → 自写 proxy `127.0.0.1:8082` (`~/.venvs/cc-proxy/cc_ollama_proxy.py`)
- `.bash_env` + `.profile`/`.bashrc` 双 source 设 `ANTHROPIC_BASE_URL`
- ⚠️ **`~/.claude.json` 的 OAuth 凭据会覆盖 ANTHROPIC_BASE_URL** —— 配前必删
- 终极 patch：直接 byte-exact 替换 `claude.exe` binary 的 API URL

### 网络/代理
- **mihomo 127.0.0.1:7897** loopback，可能挂
- github.com 被 block，所有 git/pip/hf 先设 proxy 变量
- HF 大文件用 `requests` stream，不用 hf_hub/httpx/curl-LFS

### NAS fnOS
- **飞牛 fnOS** @ 192.168.21.51:5666 (web 端口改过)
- SMB user=sunny, pw 不存 memory（每次问）
- `AI小说/hermes-agent/` 目录已建
- 用 `mount.cifs` (要 sudo) 或 `smbclient` 直读

### SkillOpt
- venv 在 `~/.venvs/skillopt/`
- sleep 仓 `~/git/SkillOpt/`
- 完整 recipe 在 skill `python-package-install`

### OpenMontage
- clone 在 `~/git/OpenMontage/`
- 离线视频生成：remotion-composer + piper TTS + ffmpeg
- 零云依赖，82 工具中 24 available
- 出片 1080p 60s h264+aac

## 🔗 Sunnny 在两台机器的分工

| 维度 | Mac mini | dgx spark |
|---|---|---|
| 角色 | 主力 AI agent (Hermes) | 本地 AI 栈 (OpenCode/Claude Code/Ollama/ComfyUI) |
| 微信 iLink | ✓ | ✓ (双备份) |
| LLM provider | minimax-cn/MiniMax-M3 | minimax-cn/MiniMax-M3 |
| 知识库 | ~/nas_knowledge/ | ~/AI小说/hermes-agent/ |
| 网关守护 | launchd | systemd user + Linger |
| GPU | 无 | GB10 130GB |
| 关键场景 | 公众号文章、微信响应、Spark 调度 | 重 AI 任务、视频生成、模型训练 |

## 📁 原始文件

- [[99-Spark-Sync/spark-MEMORY]]]] —— spark 端 MEMORY.md
- [[99-Spark-Sync/spark-USER]]]] —— spark 端 USER.md (用户偏好)

## 🔗 关联

- [[99-Spark-Sync/设备状态]]]]
- [[04-Resources/设备清单]]]]
- [[02-Projects/Hermes Agent 部署与运维]]]]
- [[03-Areas/本地大模型部署]]]]

## 🏷 标签

`#spark` `#AI栈` `#index`
