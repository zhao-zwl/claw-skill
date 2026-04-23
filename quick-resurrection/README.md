# Quick Resurrection

> OpenClaw 多 Agent 团队一键搬家。

换电脑 / 重装系统后，将整个团队打包带走，一键复活。

## 特性

- **零门槛**：一条命令完成打包/迁移
- **安全优先**：自动备份，config 合并不覆盖
- **通用适配**：支持 Main Agent + 任意数量子代理
- **跨平台**：Mac / Windows / Linux 均可用

## 快速开始

### 打包（旧环境）

```bash
cd ~/.qclaw/workspace/skills/quick-resurrection
python3 pack.py
```

### 迁移（新环境）

```bash
python3 migrate.py
```

按提示完成，全程引导式。

## 核心功能

| 功能 | 说明 |
|------|------|
| 自动检测 workspace | 读 openclaw.json，自动推断 active workspace |
| 自动备份 | 搬家前备份到 `~/.qclaw/backup/` |
| 配置合并 | deep merge，保护新环境原有配置 |
| 通用打包 | 有团队打包团队，无团队跳过 |
| 跨平台 | Mac / Windows / Linux |

## 版本

- **v2.0**（2026-04-22）— 自动检测、备份、config merge
