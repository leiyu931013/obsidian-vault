---
title: "spark 端 USER"
created: 2026-06-29
type: spark-sync
tags: [spark, memory]
---

For non-trivial / GPU-expensive / architectural tasks ("你学习一下"), pause to research or ask FIRST. For small yes/no, default quietly. Hard rules: never SIGINT long-running process without checking `/queue`; never bury auto-downloaded model sizes; never claim success on an operation I didn't execute.

**NAS**: 飞牛 fnOS @ 192.168.21.51, web=5666 (非默认 8000), SMB user=sunny。任务偏 smbclient 直读（避免 cifs mount 的 sudo 依赖，sudo -S 被 runtime block）。NAS 密码不存 memory，每次任务重新问。
§
**包源偏好 (2026-06-28)**：apt 和 pip 都倾向清华源（`mirrors.tuna.tsinghua.edu.cn/ubuntu-ports` + `pypi.tuna.tsinghua.edu.cn/simple`）。arm64 DGX Spark 上 apt suite 是 `noble` 不是 `resolute`。pip 默认走 venv `~/.venvs/<name>/`，不用 `--break-system-packages`。

**最小输入风格** (2026-06-28)：用户经常用单字符回复（"1"、"?"、"sude 秘密 xxx"）。模式：(1) 列选项让用户挑时，用户只回数字；(2) 二次澄清超时无回时，直接挑最优路径继续推，别再问；(3) 复杂任务（5+ tool call）完成后直接收尾给汇报，别问"要不要再看一次"——除非真有新选项。**驱动权默认给助手**，不是用户。

**Skill 落盘偏好** (2026-06-28)：用户认可把"踩坑 + 验证"沉淀成 skill 的行为。完成 5+ tool call 复杂任务后**主动 offer 保存为 skill**，把命令、pitfall、实测数据全套写进去。trust 信号 = "我相信你的专业能力"，意味着放权决策别再问。**秘密处理**：用户说"这是秘密"时，禁止 echo / 写文件 / 写 memory / 在 tool args 里出现；runtime 会再 block `sudo -S` 喂密码的路径，那不是 bug 是设计。