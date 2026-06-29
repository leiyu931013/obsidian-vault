---
title: "🎨 spark-imagegen 完整使用手册"
created: 2026-06-29
updated: 2026-06-29
type: manual
tags: [spark, comfyui, manual, 公众号]
---

# 🎨 spark-imagegen 完整使用手册

> **目的**: 5 分钟读懂 spark-imagegen 工作原理、调优方法、故障排查
> **维护者**: Sunny
> **对应代码**: `~/.codex/skills/spark-imagegen/scripts/generate.py`

## 📑 目录

1. [架构总览](#架构总览)
2. [硬件/软件栈](#硬件软件栈)
3. [ComfyUI 0.25.1 关键决策](#comfyui-0251-关键决策)
4. [workflow 节点详解](#workflow-节点详解)
5. [参数调优](#参数调优)
6. [常见 prompt 模板](#常见-prompt-模板)
7. [故障排查](#故障排查)
8. [扩展路线](#扩展路线)

---

## 架构总览

```
┌─────────────────┐
│ Hermes (Mac)    │ chat "画一张xxx"
│ ↓ 调用 skill     │
│ spark-imagegen  │ ~/.hermes/skills/spark-imagegen
│ ↓ python3        │
│ generate.py     │ 调用 ComfyUI REST API
└────────┬────────┘
         │ SSH + curl + scp
         ↓
┌─────────────────┐
│ dgx spark       │ ComfyUI 0.25.1 (systemd)
│ Z-Image-Turbo   │ 8 steps, GB10 130GB
│ ↓ 保存到         │
│ output/         │ /home/new/git/ComfyUI-0.25.1/output/
└────────┬────────┘
         │ scp
         ↓
~/nas_knowledge/2026-06-images/
```

## 硬件/软件栈

### spark 端
| 维度 | 值 |
|---|---|
| 系统 | Linux 6.17.0-1021-nvidia aarch64 |
| GPU | NVIDIA GB10, **130GB VRAM** |
| 驱动/CUDA | 580.159.03 / CUDA 13.0 |
| Python | `/usr/bin/python3` (system) + venv `/home/new/.hermes/hermes-agent/venv/` |
| ComfyUI | 0.25.1 (`/home/new/git/ComfyUI-0.25.1/`) |
| systemd | `~/.config/systemd/user/comfyui.service` (Linger=yes) |
| SSH | `ssh spark` 免密 (ed25519) |

### 模型配置

| 模型 | 路径 | 节点 | 作用 |
|---|---|---|---|
| **Z-Image-Turbo** (主推) | `models/diffusion_models/z_image_turbo_bf16.safetensors` | UNETLoader | UNet 主体 |
| **Qwen 3 4B** (CLIP) | `models/text_encoders/qwen_3_4b.safetensors` | CLIPLoader | 文本编码 |
| **AE** (VAE) | `models/vae/ae.safetensors` | VAELoader | 图像编解码 |

⚠️ **关键**: 0.25.1 的 UNETLoader 读 `diffusion_models/` 目录，**不是** `unet/`

## ComfyUI 0.25.1 关键决策

### 1. 用 UNETLoader 不是 DiffusionModelLoader
- 0.25.1 没有 `DiffusionModelLoader` 节点（API 改了）
- 用 `UNETLoader` 替代，**自动** 从 `diffusion_models/` 读

### 2. ModelSamplingAuraFlow
- Z-Image-Turbo 的特殊采样器
- `shift=3` 关键参数（不要乱动）

### 3. KSampler 配置
- `sampler_name: res_multistep`（residual multistep，Turbo 专用）
- `scheduler: simple`
- `denoise: 1.0`（从噪声开始，不用 img2img）
- `cfg: 1.0`（Turbo 不需要强引导）

### 4. CLIPLoader type="lumina2"
- Z-Image-Turbo 用 lumina2 CLIP
- 不用 `qwen_image` 或 `sd3`

## workflow 节点详解

```python
WORKFLOW = {
    "1": UNETLoader(unet_name="z_image_turbo_bf16.safetensors"),
    "2": CLIPLoader(clip_name="qwen_3_4b.safetensors", type="lumina2"),
    "3": VAELoader(vae_name="ae.safetensors"),
    "4": EmptySD3LatentImage(width, height, batch_size=1),  # 噪声起点
    "5": CLIPTextEncode(text=prompt, clip=2),              # prompt
    "6": ConditioningZeroOut(conditioning=5),              # 空 negative
    "7": ModelSamplingAuraFlow(model=1, shift=3),          # 采样器包装
    "8": KSampler(model=7, positive=5, negative=6,
                  latent_image=4, seed, steps, cfg=1.0,
                  sampler_name="res_multistep",
                  scheduler="simple", denoise=1.0),
    "9": VAEDecode(samples=8, vae=3),                      # latent → image
    "10": SaveImage(images=9, filename_prefix=f"gen_{seed}"),
}
```

数据流: prompt → text encode → sample → VAE decode → PNG

## 参数调优

### `steps` 步数
- **8 步**: 默认, 17s 出图, 质量 OK
- **12 步**: 25s, 质量略好
- **20 步**: 40s+, 边际收益小, **不推荐** (Turbo 是 8 步最优)

### `width × height`
- `1024×1024`: 17s (方形)
- `1024×768`: 13s (横版 4:3, 公众号头图)
- `768×1024`: 13s (竖版, 朋友圈)
- `512×512`: 7s (测试用)

⚠️ 避免 768×768 之类不规则（VAE round-trip 损耗）

### `seed`
- 不指定 → 随机
- 指定 → 可复现（重要!同一 seed + prompt = 同一图）

### 提升质量
- 加风格关键词: `masterpiece, best quality, highly detailed`
- 加光照: `cinematic lighting, golden hour`
- 加构图: `wide shot, rule of thirds`
- 避免否定词（用 positive 描述你想要什么）

## 常见 prompt 模板

### 风景
```
a serene lake reflecting mountains at sunset, golden hour, 
peaceful, masterpiece, best quality, highly detailed
```

### 人物（避免真人类，**必须 fantasy art**）
```
a mystical elf warrior in silver armor, fantasy art, 
ornate details, dramatic lighting, ink wash painting style,
no human features
```

### 公众号封面
```
abstract technology background, neural network, gradient 
blue purple, clean composition, modern, 4k
```

### 抽象/科技
```
floating geometric shapes, holographic, cyberpunk, 
volumetric lighting, octane render
```

## 故障排查

### ❌ "API call failed after 3 retries: Connection error"
- **原因**: minimax API 偶发抖动
- **解决**: 1) 等 30s 重试；2) Hermes agent 内部已 6s 后自动重试
- **预防**: 给 spark 加 fallback LLM（待办）

### ❌ 生成图全黑/全噪点
- **原因 1**: prompt 触发 content filter
- **解决**: 加 `safe, family friendly` 到 prompt
- **原因 2**: seed 极端值
- **解决**: 不指定 seed

### ❌ ComfyUI "Failed to initialize database"
- **不是真错**！是 alembic 文档截断在日志行
- **解决**: 忽略
- **验证**: `tail -50 comfyui.log | grep "got prompt"` 看到 "got prompt" 就行

### ❌ 慢/卡死
- **看 GPU**: `ssh spark nvidia-smi`
- **看 queue**: `ssh spark curl http://localhost:8188/queue`
- **重启 ComfyUI**: `ssh spark systemctl --user restart comfyui`

### ❌ `z_image_turbo_bf16.safetensors` 找不到
- **路径不对**: 必须是 `models/diffusion_models/`
- **解决**: `ssh spark find ~/git/ComfyUI-0.25.1/models -name "*z_image*"`
- **如果没有**: 用 SDXL 替代（更慢但有现成模型）

## 扩展路线

| 想做 | 方法 | 难度 |
|---|---|---|
| 加 SDXL 模型 | workflow 加 `LoadCheckpoint` 节点 | 低 |
| 加 prompt 翻译 | 中 → 英 在 main() 前加 translator | 中 |
| 加 ControlNet | workflow 加 `ControlNetLoader` 节点 | 中 |
| 加 img2img | 把 `EmptySD3LatentImage` 换成 `VAEEncode` 喂图 | 低 |
| 加 LoRA | workflow 加 `LoRALoader` 节点 | 低 |
| 加多个种子批量 | main() 加 loop, filename 加 seed | 低 |
| 加异步回调 | 改 wait_for_completion 为 webhook | 高 |

## 相关代码

- **主脚本**: `~/.codex/skills/spark-imagegen/scripts/generate.py` (214 行)
- **Hermes skill 软链**: `~/.hermes/skills/spark-imagegen` → `~/.codex/skills/`
- **SKILL.md**: 双 frontmatter (Codex + Hermes 兼容)

## 关联笔记

- [[02-Projects/spark-imagegen 部署]]]]
- [[03-Areas/本地大模型部署]]]]
- [[04-Resources/设备清单]]]]
- [[99-Spark-Sync/设备状态]]]]
- [[99-Spark-Sync/spark 本地 AI 栈]]]]
- [[02-Projects/公众号-「NAS村长」运营]]]]
- [[03-Areas/公众号内容生产]]]]

## 🏷 标签

`#spark` `#comfyui` `#manual`
