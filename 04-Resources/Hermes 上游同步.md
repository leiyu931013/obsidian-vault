# Hermes Agent 上游同步

> 跟踪 [NousResearch/hermes-agent](https://github.com/NousResearch/hermes-agent) 仓库 skill 变化的 weekly cron。

## 仓库位置

```
~/hermes/hermes-agent-upstream  (已 unshallow, .git 319M)
remote: upstream → https://github.com/NousResearch/hermes-agent.git
```

## 触发器

- **launchd**: `local.sunny.hermes-upstream-diff`
- **频率**: 每周一 09:00
- **脚本**: `~/.hermes/scripts/hermes_upstream_diff.sh`
- **日志**: `~/.hermes/logs/upstream_diff.log`
- **基线**: `~/hermes/hermes-agent-upstream/.last_known_commit`

## 工作流

1. `git fetch upstream main --prune --prune-tags`
2. 对比 `FETCH_HEAD` 与 `.last_known_commit` (上周一记录的 HEAD)
3. `git diff --name-status --diff-filter=AMD` 扫 `skills/` + `optional-skills/` 的 `SKILL.md` 变化
4. 有变化 → 写报告到 `~/.hermes/logs/upstream_diff_report.txt` + 同步到本 vault
5. 更新 `.last_known_commit`

## 测试

```bash
# 手动跑
bash ~/.hermes/scripts/hermes_upstream_diff.sh

# 看 log
tail -30 ~/.hermes/logs/upstream_diff.log

# 看报告
cat ~/.hermes/logs/upstream_diff_report.txt
```

## 报告样例 (v2026.5.7 → 23:36)

| 类型 | 数量 |
|---|---|
| 新增 SKILL.md | 25 |
| 修改 SKILL.md | 129 |
| 删除 SKILL.md | 10 |

**有趣的 add** (50 天内):
- `optional-skills/devops/hermes-s6-container-supervision` — s6-overlay 容器监控
- `optional-skills/devops/watchers` — 看门狗脚本
- `optional-skills/devops/pinggy-tunnel` — 内网穿透
- `optional-skills/productivity/shop` — 商店运营
- `optional-skills/research/darwinian-evolver` — 达尔文式进化
- `optional-skills/research/osint-investigation` — OSINT 调查
- `optional-skills/security/web-pentest` — Web 渗透测试
- `optional-skills/software-development/code-wiki` — 代码 Wiki
- `optional-skills/web-development/cloudflare-temporary-deploy` — Cloudflare 临时部署
- `skills/computer-use/SKILL.md` (整个 category 从 optional 转正)
- `skills/productivity/petdex` — 宠物图鉴
- `skills/productivity/teams-meeting-pipeline` — Teams 会议 pipeline
- `skills/software-development/simplify-code` — 代码简化
- `optional-skills/finance/stocks` — 股票查询

## 行动建议 (每次报告生成时附在末尾)

1. 看 upstream [skills/ commit 列表](https://github.com/NousResearch/hermes-agent/commits/main/skills) diff 详情
2. 新增 skill 中是否有想装的 → 复制 `SKILL.md` 到 `~/.hermes/skills/<category>/<name>/`
3. 修改的 skill 检查是否影响 `~/.hermes/skills/` 同名 skill (冲突可能性低, 因为你装的是 fork)
