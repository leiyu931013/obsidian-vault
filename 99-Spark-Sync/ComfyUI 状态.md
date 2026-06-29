---
title: "🟢 ComfyUI 实时状态"
created: 2026-06-29
type: spark-sync
tags: [spark, comfyui, 状态]
---

# 🟢 ComfyUI 实时状态

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

## 关键端点

- **本地**: http://localhost:8188
- **局域网**: http://192.168.21.191:8188

## 最近生图

| 时间 | prompt | 耗时 |
|---|---|---|
| 2026-06-29 04:50 | 湖面倒影 (1024×1024) | 17s |

## 关联

- [[设备状态]]
- [[设备清单]]
- [[spark-imagegen 部署]]
- [[spark-imagegen 使用手册]]
- [[本地大模型部署]]
- [[NAS 私有云]]
