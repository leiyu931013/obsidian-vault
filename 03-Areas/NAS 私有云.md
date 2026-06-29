---
title: "📦 NAS 私有云"
created: 2026-06-29
updated: 2026-06-29
type: area
tags: [area, NAS, 公众号]
---

# 📦 NAS 私有云

> 公众号「NAS村长」核心领域。读者画像：自建家庭/工作室存储的人。

## 📚 现在知道的

### 硬件
- **群晖 DS925+** — Sunny 在用（Ares 家用款）
- **Ugreen USB 3.2 Gen 2 + 5x Seagate Exos ST12000N (12TB)** — 60TB 闲置待组装
  - 5 块均为 7200 RPM 企业级氦气盘
  - 4 块已分 Linux_RAID 类型但无 mdadm 组装
  - Mac mini 无 mdadm/zfs/btrfs 工具

### 软件
- **群晖 DSM** — 主力 NAS 系统
- **RAID 方案** — Linux_RAID 类型 = 软 RAID 倾向（待装 mdadm）
- **ComfyUI** — 跑在 dgx spark（GB10 GPU），通过 spark-imagegen skill 调
- **PT 站点** — 资源下载，公众号垂直内容

### 关键 IP/设备
- **Mac mini**: 169.254.83.107:8748
- **dgx spark**: 192.168.21.191
- **群晖 DS925+**: 家里内网

## 📈 最近关注

- 5 块 Exos 12TB 怎么组 RAID（RAID 5/6/10？）
- macOS 没 mdadm → 用 Linux 装机再插回？
- Hermes 部署到群晖的可行性（Docker）

## 💡 选题源

- 群晖套件玩法
- 自建 RAID 教程
- PT 站配合 NAS
- Hermes 群晖部署
- AI 工具 + NAS 结合

## 🔗 关联

- [[02-Projects/公众号-「NAS村长」运营]]]]
- [[04-Resources/设备清单]]]]
- [[99-Spark-Sync/ComfyUI 状态]]

## 🏷 标签

`#area/NAS` `#公众号/NAS村长`
