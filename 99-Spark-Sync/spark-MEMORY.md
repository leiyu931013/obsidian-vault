---
title: "spark 端 MEMORY"
created: 2026-06-29
type: spark-sync
tags: [spark, memory]
---

**本地 AI 栈** (2026-06-25)：OpenCode 1.17.10 直连 Ollama，Claude Code 2.1.191 走自写 proxy 127.0.0.1:8082 (`~/.venvs/cc-proxy/cc_ollama_proxy.py`)。`.bash_env` + `.profile`/`.bashrc` 双 source 设 `ANTHROPIC_BASE_URL`+dummy key。**首次配前必查 `~/.claude.json`** —— claude.ai OAuth 凭据会覆盖 ANTHROPIC_BASE_URL，致 `Failed to connect to api.anthropic.com`。claude-code-proxy PyPI/litellm-proxy 都弃用。**终极 patch**：sandbox 通但 GUI 终端不通时（用户拒跑诊断命令），直接 byte-exact 替换 `claude.exe` binary 7 处 `https://api.anthropic.com\x00\x00\x00` (28B) → `http://127.0.0.1:8082\x00\x00\x00\x00\x00\x00\x00` (28B)。详见 skill `local-ai-stack`。
§
代理：mihomo 127.0.0.1:7897 loopback，可能挂掉需重启。直接 github.com 被 block；所有 git/pip/hf 先设三个 proxy 变量。`api.github.com` 在 mihomo 下 403, 用 `.../releases/latest` 302 拿版本号。HF 大文件用 `requests` stream, 不用 hf_hub/httpx/curl-LFS。

**NAS fnOS** (2026-06-27)：192.168.21.51:5666 (web, 用户改), SMB user=sunny, pw 不存 memory 每次问。`AI小说/hermes-agent/` 已建(models/cache/outputs/datasets/workflows/logs/tmp)。`~/.local/bin/{nas,nas-mount,nas-umount}`, 凭据 `~/.smb_nas_pw` mode 600。`mount.cifs` 要 sudo + `/etc/fstab` 行, `sudo -S` 被 runtime block。NFS 默认空导出。技能见 `fnnos-nas-access`。

**`terminal(bg, notify)` EXIT 假阳性**: `cmd > log; echo "EXIT=$?"` echo 永返 0, 来自 shell wrapper 不是内层命令。`notify_on_complete` 不可信。修：`PIPESTATUS[0]`, 或 `bash -c '...; echo $? > log.rc'`, 或 tail log grep 终态 + `ls` 验 fs。详见 skill `hermes-runtime-quirks` §11。
§
**SkillOpt** (2026-06-28)：`skillopt==0.1.0` venv 在 `~/.venvs/skillopt/`，sleep 仓 `~/git/SkillOpt/`。完整 recipe+架构+6 阶段 loop+gate 语义+sleep 验证全在 skill `python-package-install/references/skillopt-install-recipe.md`。
§
**OpenMontage** (2026-06-28)：clone 在 `~/git/OpenMontage/`，venv+remotion-composer+piper TTS+ffmpeg 全装。preflight 82 工具 24 available，零云依赖。出片 v1 无声 9MB / v2 有 TTS+ambient 7.5MB，产物 `projects/what-is-openmontage/what-is-openmontage-v2.mp4` (h264+aac 1080p 60s mean=-13dB)。recipe 存 skill `openmontage-offline-video`。Explainer Cut schema `remotion-composer/src/Explainer.tsx` 170-420 行（必填 id/source/in_seconds/out_seconds/type）。