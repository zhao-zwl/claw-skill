# Agent Creator

**用自然语言，三分钟打造你的专属 AI 团队。**

像搭乐高一样组合不同专长的 Agent——一个主控指挥，多个成员分工协作。
**零编程门槛，1/10 成本，10 倍效率。**

> 🦞 本 skill 由 龙虾（OpenClaw 中文社区）打造，专为中文用户优化。

---

## 🌟 特性

- **3 分钟上手** — 输入名称和职责，输出完整可用的 Agent
- **全文件覆盖** — workspace + SOUL.md + openclaw 配置，一次搞定
- **零技术门槛** — 照着做就行，不需要懂底层原理
- **低成本高效率** — 不需要额外 API key，不需要云服务器，一个主控带多个成员
- **开箱即用** — 直接复制到你的 OpenClaw 即可使用

---

## 📋 前置要求

- OpenClaw 已安装并正常运行
- 已配置 `openclaw.json`（参考 [OpenClaw 文档](https://docs.openclaw.ai)）
- 主 Agent 具备管理员权限

---

## 🚀 快速开始（3 分钟上手）

### 1. 安装

```bash
# 方式一：复制整个目录到你的 skills 目录
cp -r agent-creator /path/to/your/skills/

# 方式二：用 SkillHub 安装（如果已配置）
skillhub install agent-creator
```

### 2. 告诉 OpenClaw

在 `openclaw.json` 中确保以下配置存在：

```json
{
  "agents": {
    "defaults": {
      "subagents": {
        "allowAgents": ["*"]
      }
    }
  }
}
```

### 3. 开始使用

触发方式（直接告诉你的主控 Agent）：

> "创建一个新成员，英文名 `assistant`，职责是帮我做研究调研"

然后主控 Agent 会自动执行后面的流程。

---

## 📖 完整工作流（5 步）

### 第 1 步：确认名称和职责

和用户确认两件事：

- **英文名称**：小写字母 + 数字，不能用 `main`
- **一句话职责**：尽量具体，例如"帮我整理会议纪要"而非"处理杂事"

### 第 2 步：创建 workspace 和 SOUL.md

在主 Agent workspace 下创建子目录，并写入 `SOUL.md`：

```bash
mkdir -p ~/.qclaw/workspace-{mainID}/{名称}
mkdir -p ~/.qclaw/agents/{名称}/agent
```

**SOUL.md 模板**（见 `templates/SOUL.md`）：

```markdown
# SOUL.md

## 我是谁
{角色定位描述}

## 我的职责
1. {具体职责1}
2. {具体职责2}

## 禁止事项
- {明确禁止做的事}
- {不做任何执行工作，只汇报}

## 汇报规则
- 完成后立即汇报，不等催
- 格式：【成员名】任务名 + 要点 + 阻塞问题（如有）
```

### 第 3 步：更新 openclaw 配置

在 `openclaw.json` 的 `agents.list` 中追加：

```json
{
  "id": "{英文名}",
  "name": "{显示名称}",
  "workspace": "~/.qclaw/workspace-{mainID}/{英文名}",
  "agentDir": "~/.qclaw/agents/{英文名}/agent"
}
```

### 第 4 步：重启 Gateway

```bash
openclaw gateway restart
```

**必须重启，否则新 Agent 无法激活。**

### 第 5 步：验证

派一个简单任务测试：

```
sessions_spawn({
  agentId: "{英文名}",
  mode: "run",
  task: "读 SOUL.md，回复你的名称和职责。"
})
```

---

## 🎯 设计原则：什么是好的 SOUL.md

一个合格的团队 Agent SOUL.md 应该包含：

| 章节 | 必须包含 | 避免 |
|------|----------|------|
| 身份定位 | 清晰的职责边界 | 模糊的"协助"描述 |
| 具体职责 | 列出具体任务 | 空泛的"做好工作" |
| 禁止事项 | 明确划定红线 | 没有边界 |
| 汇报规则 | 何时报、怎么报 | 等着被问 |
| 铁律 | 不可逾越的行为红线 | 写套话 |

### 自检问题

- [ ] 新成员读了自己的 SOUL.md 知道该做什么吗？
- [ ] 有没有明确说"不做什么"？
- [ ] 汇报规则说清楚了吗？

---

## 📂 完整示例

见 `examples/assistant/` 目录，包含一个"研究助手" Agent 的完整文件集：

- `SOUL.md` — 角色定义
- `IDENTITY.md` — 身份卡
- `TOOLS.md` — 工具备忘

---

## 📁 文件说明

```
agent-creator/
├── SKILL.md          # 技能本体（给 Agent 看的操作手册）
├── README.md         # 本文件（给人类看的说明文档）
├── examples/         # 完整示例（可参考的工作成果）
│   └── assistant/
│       ├── SOUL.md
│       ├── IDENTITY.md
│       └── TOOLS.md
└── templates/        # 模板文件（可直接复制使用）
    ├── SOUL.md
    ├── IDENTITY.md
    ├── TOOLS.md
    └── HEARTBEAT.md
```

---

## ⚠️ 常见错误

| 错误现象 | 原因 | 解决方式 |
|----------|------|----------|
| 创建后 Agent 无响应 | workspace 目录不存在 | 确认第 2 步已执行 |
| sessions_spawn 失败 | allowAgents 未配置 | 检查 openclaw.json |
| cron 任务不执行 | Gateway 未重启 | 执行 `openclaw gateway restart` |
| Agent 读不到自己的 SOUL.md | cwd 参数缺失 | sessions_spawn 需加 cwd 指向正确 workspace |

---

## 🔧 进阶用法

### 多人团队批量创建

参考 `examples/team-automation/` 中的脚本，一次性创建多个成员。

### Skill 集成

创建 Agent 后，可以继续派任务安装 Skill：

```
skillhub install {skill-name}
```

---

## 🦞 关于 龙虾

**龙虾（OpenClaw 中文社区）** — 专注 AI Agent 落地实践，分享真实可用的工作流和工具。

- 官网：[clawhub.com](https://clawhub.com)
- 社区问题欢迎提交 Issue 或 Pull Request

## 📄 License

MIT — 随便用，署名即可。

---

## 更新日志

见 `CHANGELOG.md`
