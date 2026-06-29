---
title: "🎨 spark-imagegen 部署"
created: 2026-06-29
updated: 2026-06-29
status: 稳定运行
priority: A
type: project
tags: [project, spark, ComfyUI, 公众号]
---

# 🎨 spark-imagegen 部署

> **目标**：说"画张 xxx 图" → Hermes 自动调 spark 上的 ComfyUI → 自动归档到 nas_knowledge

## 📅 时间线

- **开始**: 2026-06
- **上线**: 2026-06-17
- **状态**: 稳定运行

## 🎯 关键结果

- [x] spark ComfyUI 0.25.1 跑通
- [x] Z-Image-Turbo 模型加载 (8 步)
- [x] UNETLoader + diffusion_models/ 路径修复
- [x] systemd 守护 + Linger=yes
- [x] Hermes skill 镜像 (`~/.hermes/skills/spark-imagegen` 软链到 `~/.codex/skills/`)
- [x] 自动归档 `~/nas_knowledge/2026-06-images/`
- [x] 端到端测试 (湖面倒影 1024×1024, 17s)
- [ ] 加 backup 模型 (SDXL/SD3.5)
- [ ] 加 prompt 翻译 (中文 → 英文)
- [ ] 加反向 prompt 自动生成

## 📋 任务分解

### 模型配置
- [x] z_image_turbo_bf16.safetensors (主推)
- [x] qwen_3_4b.safetensors (CLIP)
- [x] ae.safetensors (VAE)
- [x] workflow: UNETLoader → CLIPLoader → KSampler (res_multistep, 8 steps) → VAEDecode

### 调用链
- [x] `~/.codex/skills/spark-imagegen/scripts/generate.py`
- [x] Hermes skill 兼容 (软链 + 兼容 frontmatter)
- [x] 测试: panda / 美女 / 湖面倒影 ✓

### 已知坑
- ComfyUI 0.25.1 没有 `DiffusionModelLoader` 节点 → 用 `UNETLoader`
- 模型路径在 `models/diffusion_models/` 不是 `models/unet/`
- alembic 数据库 init 报 "Failed to initialize database"——**无害**（文档截断）
- pip3 缺包 (sqlalchemy, filelock) → `pip3 install --break-system-packages`

## 📝 重要决策

### 模型选 Z-Image-Turbo 不是 SDXL
- 8 步 vs SDXL 25 步 → 速度快 3 倍
- 质量持平（实测）
- 不需要 CFG=7.5 那种高强度引导（cfg=1.0 即可）

### 输出走 `~/nas_knowledge/2026-06-images/`
- 公众号文章配图自动归档
- Obsidian 知识库能直接引用

## 🔗 关联

- [[02-Projects/Hermes Agent 部署与运维]]]]
- [[03-Areas/本地大模型部署]]]]
- [[02-Projects/公众号-「NAS村长」运营]]]]
- [[99-Spark-Sync/ComfyUI 状态]]

## 🏷 标签

`#project/spark` `#project/ComfyUI` `#优先级/A`
