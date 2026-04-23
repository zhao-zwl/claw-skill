# claw-skill

短流创团队开源的 OpenClaw Skill 集合。

## 📦 包含的 Skills

| Skill | 目录 | 版本 | 功能 |
|-------|------|------|------|
| 一键搬家 | `team-resurrection/` | v3.0 | 搬家·分身·备份三合一，跨环境迁移+同环境复制 |
| 一键搬家（老版） | `quick-resurrection/` | v2.0 | 打包+搬家，老版本保留参考 |
| Agent Creator | `quick-team/` | — | 自然语言创建团队子代理 |
| Team Sessions | `team-sessions/` | — | sessions_spawn 标准工作流 |

> **注意：** `team-resurrection/` 是 v3.0 最新版，`quick-resurrection/` 是 v2.0 老版参考，发布时注意区分。

## 🚀 安装方式

```bash
# v3.0 最新版（推荐）
clawhub install --source /Users/zhaowenlong/claw-skill/team-resurrection

# 从 clawhub 安装（需先发布）
clawhub install team-resurrection

# 其他 skills
clawhub install --source /Users/zhaowenlong/claw-skill/quick-team
clawhub install --source /Users/zhaowenlong/claw-skill/team-sessions
```

## 🔧 维护说明

- **发布者：** 毒舌（agent-ba01c6a8）
- **发布前必读：** `CLAWHUB_PUBLISH_NOTES.md`
- **源码位置：** `~/.qclaw/workspace-agent-ba01c6a8/skills/team-resurrection/`（v3.0 开发源）

## 📝 重要记录

- **2026-04-23**：v3.0 上线——新增 clone.py 分身功能，SKILL.md 全面升级为"搬家·分身·备份三合一"
- **2026-04-23**：因发布操作失误，本仓库执行了回退。后续 clawhub 发布操作统一由赵文龙手动执行，AI 禁止自行发布。
