---
name: quick-resurrection
description: 换电脑、重装系统之后，你的AI团队还在吗？这个工具让整个团队"打包带走、原地复活"，一个命令，不丢一个成员。
metadata:
  openclaw:
    requires:
      bins: []
    install: []
---

# Quick Resurrection

> **OpenClaw 多 Agent 团队一键搬家。** 换电脑 / 重装系统后，打包带走，一键复活。
>
> **版本：v2.0** — 自动检测 workspace、搬家前自动备份、config 合并不覆盖。

---

## 快速开始

### 打包（旧环境）

```bash
cd ~/.qclaw/workspace/skills/quick-resurrection
python3 pack.py
```

生成搬家包 → 复制到新环境。

### 迁移（新环境）

```bash
python3 migrate.py
```

全程引导式，按提示选择即可。

---

## 一、核心概念

### 三要素激活原理

子代理正确激活必须同时满足三个条件：

| 要素 | 参数 | 作用 |
|------|------|------|
| **身份** | `agentId` | 控制读哪个 SOUL.md（人格） |
| **隔离** | `cwd` | 控制 workspace 根目录 |
| **权限** | `allowAgents` | 白名单，允许 spawn |

**关键发现（2026-04-21 实测验证）：**
- `agentId` 控制人格，不控制 workspace
- `cwd` 才控制 workspace 隔离
- 不设 cwd → 子代理永远读父代理的 workspace

### 通用化设计

| 场景 | pack.py 行为 | migrate.py 行为 |
|------|-------------|----------------|
| 有团队成员 | 打包团队成员 | 复制团队成员 |
| 无团队成员 | 跳过 | 跳过，提示 |
| 有 cron | 打包 | 创建（跳过已存在） |
| 无 cron | 跳过 | 跳过，提示 |

### 备份与回滚

**搬家前自动备份：**
- 目标：`~/.qclaw/backup/搬家备份_YYYYMMDD_HHMMSS/`
- 内容：所有 workspace-* + openclaw.json

**回滚方法：**
```bash
cp -r ~/.qclaw/backup/搬家备份_xxx/* ~/.qclaw/
openclaw gateway restart
```

### 配置合并策略

**核心原则：不覆盖新环境的其他配置。**

- agents 配置：deep merge
- hooks.allowedAgentIds：union merge（取并集）
- 其他配置（channel、plugins 等）：原样保留

---

## 二、脚本说明

### 打包脚本 pack.py

```bash
python3 pack.py
```

**自动检测内容：**
1. 从 openclaw.json 找到 active workspace（自动推断，不再硬编码）
2. 收集身份文件（SOUL/MEMORY/TOOLS 等）
3. 收集团队成员（有则打包，无则跳过）
4. 收集 skills（有则打包，无则跳过）
5. 获取 cron 任务配置
6. 动态生成 README.md

**输出：** `~/一键搬家包/{Agent名称}搬家包_YYYYMMDD.zip`

### 迁移脚本 migrate.py

```bash
# 方式A（推荐）：解压后直接运行
unzip 搬家包.zip
cd 搬家包
python3 migrate.py

# 方式B：zip 在当前目录
python3 migrate.py

# 方式C：传入路径
python3 migrate.py /path/to/搬家包.zip
```

**执行步骤：**

| Step | 内容 |
|------|------|
| 0 | 备份现有配置（自动） |
| 0.5 | 选择如何处理 main agent（交互） |
| 1 | 复制身份文件 |
| 2 | 复制团队成员 |
| 3 | 复制 skills |
| 4 | 合并 agent 配置 |
| 5 | 创建 cron 任务 |
| 6 | 重启 Gateway |

**Main Agent 三个选项（均有完整执行逻辑）：**

```
1. 指向现有 agent（把当前 agent 变成你的主控）
2. 新建 main agent 实例（另起一个，保留当前配置）
3. 覆盖现有 main agent（⚠️ 替换现有主控）
```

⚠️ **已废弃脚本（v2.0 不再使用）：**
- `setup_config.py` — 配置更新已并入 migrate.py
- `init.sh` — 创建 main agent 已并入 migrate.py

---

## 三、搬家包结构

```
搬家包/
├── README.md                ← 动态生成
├── migrate.py               ← v2.0
│
├── 身份层/                  ← Main Agent 核心文件
│   ├── SOUL.md
│   ├── MEMORY.md
│   ├── TOOLS.md
│   ├── AGENTS.md
│   ├── IDENTITY.md
│   ├── USER.md
│   └── memory/              ← 历史记录
│
├── 团队成员层/              ← 自动检测，有则包含
│   ├── 成员A/
│   │   └── SOUL.md
│   └── ...
│
├── skills/                  ← 自动检测，有则包含
│
├── openclaw-agents.json     ← Agent 配置片段
├── cron_tasks.json          ← Cron 任务清单
└── 工作目录说明.md          ← git 仓库提示
```

---

## 四、配置说明

### openclaw.json 关键配置

```json
{
  "agents": {
    "defaults": {
      "subagents": { "allowAgents": ["*"] }
    },
    "list": [
      { "id": "main", "name": "Main Agent" },
      { "id": "成员A", "name": "成员A", "workspace": "...", "agentDir": "..." }
    ]
  },
  "hooks": {
    "allowedAgentIds": ["成员A", "成员B", ...]
  }
}
```

### models.json（每个 agent 独立）

```json
{
  "providers": {
    "qclaw": {
      "baseUrl": "http://127.0.0.1:19000/proxy/llm",
      "apiKey": "__QCLAW_AUTH_GATEWAY_MANAGED__",
      "api": "openai-completions",
      "models": [{ "id": "modelroute", "name": "modelroute", "input": ["text", "image"] }]
    }
  }
}
```

---

## 五、常见问题

| 问题 | 原因 | 解法 |
|------|------|------|
| `agentId is not allowed` | 缺 allowAgents 白名单 | migrate.py 自动合并时补上 |
| 子代理读错 SOUL.md | 没传 cwd 参数 | spawn 时必须传 agentId + cwd |
| Gateway 重启后 spawn 报错 | channel 注册需时间 | 等 5-10 秒再 spawn |
| 子代理空跑 | 任务描述不够具体 | 预写脚本让子代理执行 |
| 搬家后配置丢失 | v1.0 用 replace 而非 merge | v2.0 已修复 |

---

## 六、验证清单

搬家完成后逐一检查：

```bash
# Gateway 状态
openclaw gateway status

# SOUL.md 是否存在
ls ~/.qclaw/workspace-*/SOUL.md

# 团队成员
ls ~/.qclaw/workspace-*/

# 测试子代理激活
# 在 agent 对话中说："测试激活团队成员"

# cron 任务
openclaw tasks list

# 回滚（如需要）
ls ~/.qclaw/backup/
cp -r ~/.qclaw/backup/搬家备份_xxx/* ~/.qclaw/
openclaw gateway restart
```

---

## 七、版本记录

### v2.0（2026-04-22）

- pack.py 自动检测 workspace，不再硬编码
- migrate.py 搬家前自动备份
- config 改为 deep merge，保护新环境配置
- Main Agent 三个选项逻辑全部补全
- 删除了 setup_config.py 和 init.sh 的误导文档
