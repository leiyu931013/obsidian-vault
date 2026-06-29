---
title: "🟢 Hermes Gateway 实时状态"
created: 2026-06-29
type: spark-sync
tags: [spark, hermes, 状态]
---

# 🟢 Hermes Gateway 实时状态

> **同步方式**: `ssh spark` 实时查
> **最后同步**: 2026-06-29

## systemd 状态
```
=== ComfyUI ===
active
 170832       00:00 bash -c  echo "=== ComfyUI ===" systemctl --user is-active comfyui ps -o pid,etime,cmd -p $(pgrep -f "ComfyUI-0.25.1/main.py" | head -1) 2>/dev/null | tail -1 echo echo "=== ComfyUI 端口 ===" ss -tlnp 2>/dev/null | grep 8188 echo echo "=== Hermes Gateway ===" systemctl --user is-active hermes-gateway ps -o pid,etime,cmd -p $(pgrep -f "hermes_cli.main" | head -1) 2>/dev/null | tail -1 echo echo "=== iLink 连接 ===" tail -3 ~/.hermes/logs/gateway.log 2>/dev/null echo echo "=== GPU ===" nvidia-smi --query-gpu=utilization.gpu,memory.used --format=csv,noheader 2>/dev/null 

=== ComfyUI 端口 ===
LISTEN 0      128          0.0.0.0:8188       0.0.0.0:*    users:(("python3",pid=2959,fd=37))    

=== Hermes Gateway ===
active
   2960    23:42:08 /home/new/.hermes/hermes-agent/venv/bin/python -m hermes_cli.main gateway run

=== iLink 连接 ===
2026-06-29 09:47:23,773 INFO gateway.run: Agent cache idle sweep: evicted 1 agent(s)
2026-06-29 18:44:16,049 INFO gateway.run: curator: curator: snapshot created (2026-06-29T10-44-15Z)
2026-06-29 18:44:16,353 INFO gateway.run: curator: curator: auto: no changes; llm: skipped (consolidation off)

=== GPU ===
0 %, [N/A]

```

## 微信 iLink 连接

- 平台: `ilinkai.weixin.qq.com`
- 账号: `6cbc9825be93@im.bot`
- 状态: connected (持续 4+ 天)

## LLM Provider

- **当前**: `minimax-cn/MiniMax-M3` (200k context)
- **fallback**: 无（单一故障点）
- **API endpoint**: `https://api.minimaxi.com/anthropic`

## 关联

- [[设备状态]]
- [[设备清单]]
- [[Hermes Agent 部署与运维]]
- [[04-Resources/API 与密钥位置]]
- [[04-Resources/守护进程速查]]
