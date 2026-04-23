---
name: quick-team
description: 用自然语言指挥 AI 团队，低成本零门槛，三分钟创建一个专属团队成员。
metadata:
  openclaw:
    requires:
      bins: []
    install: []
---

# Agent Creator —— 快速创建团队成员

> **三分钟，用自然语言创建一个专属子代理。**
>
> **触发：** 用户说「创建一个成员」「帮我新建一个角色」「添加团队成员」「搞个新agent」等。
>
> **核心理念：** 一个主控，零门槛，三分钟一支团队。

---

## 零、快速开始

```
用户：帮我创建一个负责校对审核的成员，叫火眼
Agent：
    [执行创建流程 → 3分钟完成]
    ✅ 团队新成员：火眼
    workspace: ~/.qclaw/workspace-agent-ba01c6a8/huoyan/
    SOUL.md / IDENTITY.md / TOOLS.md / HEARTBEAT.md / MEMORY.md 已创建
```

---

## 一、创建流程（6步）

### 步骤1：确认需求

让用户明确回答以下问题（缺一不可）：

| 问题 | 示例 | 作用 |
|------|------|------|
| **角色名称** | "火眼"、"小策" | 决定目录名和 Agent ID |
| **核心职责** | "校对审核"、"策略分析" | 决定 SOUL.md 的职责描述 |
| **性格风格** | "毒舌"、"温和"、"专业冷静" | 决定语气和人设 |
| **禁止做的事** | "不横向联系"、"不自作主张" | 决定禁止事项 |
| **汇报方式** | "任务完成后直接汇报" | 决定汇报规则 |

**如果用户说不清楚：** 主动给选项，让用户选，不要停下来等。

---

### 步骤2：生成 Agent ID

**规则：** 用中文拼音首字母，不能重复。

| 名称 | ID |
|------|-----|
| 火眼 | huoyan |
| 小策 | xiaoce |
| 小编 | xiaobian |
| 小创 | xiaochuang |

**如果重复：** 自动加数字后缀，如 `xiaoce-2`

---

### 步骤3：创建文件结构

在主控 workspace 下创建子代理目录：

```
workspace/
├── huoyan/
│   ├── SOUL.md        ← 必须：人格定义
│   ├── IDENTITY.md    ← 必须：身份标识
│   ├── TOOLS.md       ← 必须：工具备忘
│   ├── HEARTBEAT.md   ← 必须：心跳配置
│   └── MEMORY.md      ← 必须：长期记忆
```

**必须文件说明：**

| 文件 | 必须 | 用途 |
|------|------|------|
| SOUL.md | ✅ | 角色定位 + 职责 + 禁止事项 + 汇报规则 |
| IDENTITY.md | ✅ | 名称/Emoji/氛围描述 |
| TOOLS.md | ✅ | 常用工具备忘 |
| HEARTBEAT.md | ✅ | 周期性任务配置 |
| MEMORY.md | ✅ | 长期记忆文件 |

**辅助文件说明：**
| 文件 | 必须 | 用途 |
|------|------|------|
| AGENTS.md | ✅ | 主控才有，记录所有成员配置 |
| USER.md | ⚠️ | 主控才需要，子代理一般不需要 |
| BOOTSTRAP.md | ❌ | 仅首次启动用，创建成员时不需要 |

---

### 步骤4：填充模板

从 `templates/` 目录复制对应模板，根据用户需求填写内容。

**重点：SOUL.md 必须包含以下章节（不可删减）：**

```markdown
# SOUL.md

## 我是谁
{一句话角色定位}

## 我的职责
1. {具体职责1}
2. {具体职责2}
3. {具体职责3}

## 禁止事项
- {明确不做的事}

## 汇报规则
- {汇报格式和要求}

## 铁律
- {不可逾越的红线}
```

---

### 步骤5：配置 openclaw.json

在 `agents.list` 中追加新成员：

```json
{
  "id": "huoyan",
  "name": "火眼",
  "workspace": "/Users/xxx/.qclaw/workspace-agent-主控ID/huoyan"
}
```

**同时确保 `allowAgents` 为 `["*"]`**，否则子代理无法 spawn。

---

### 步骤6：验证激活

执行以下测试：

```javascript
// 在新会话中测试
sessions_spawn({
  agentId: "huoyan",
  cwd: "/Users/xxx/.qclaw/workspace-agent-主控ID/huoyan",
  mode: "run",
  task: "介绍一下你自己"
})
```

**验证通过的标准：**
- ✅ 回复体现正确的角色人格
- ✅ 没有读错 SOUL.md（比如读成了其他成员的）
- ✅ 文件路径正确

---

## 二、子代理标准目录结构

```
workspace/
├── 主控workspace/
│   ├── SOUL.md
│   ├── IDENTITY.md
│   ├── AGENTS.md          ← 记录所有成员配置
│   ├── MEMORY.md
│   ├── TOOLS.md
│   ├── HEARTBEAT.md
│   │
│   ├── 子代理A/            ← 每个子代理一个独立目录
│   │   ├── SOUL.md
│   │   ├── IDENTITY.md
│   │   ├── TOOLS.md
│   │   ├── HEARTBEAT.md
│   │   └── MEMORY.md
│   │
│   ├── 子代理B/
│   │   └── ...
│   │
│   └── memory/            ← 主控的日常记忆
│       └── YYYY-MM-DD.md
│
└── skills/                ← 主控的 skills
    └── ...
```

---

## 三、模板说明

| 模板文件 | 用途 | 备注 |
|---------|------|------|
| `SOUL.md` | 角色人格定义 | 核心，必须完整填写 |
| `IDENTITY.md` | 身份标识 | 名称/Emoji/氛围 |
| `TOOLS.md` | 工具备忘 | 常用命令和配置 |
| `HEARTBEAT.md` | 周期性任务 | 保持空可节省API |
| `MEMORY.md` | 长期记忆 | 持续积累信息 |

`examples/assistant/` 目录中有完整示例，可参考格式。

---

## 四、常见问题

### Q：用户只给了一个名字怎么办？

**A：** 先根据名字推断角色方向，然后主动给选项：

```
用户：帮我建一个叫小明
Agent：好的，小明大概负责什么方向？
  A. 执行类——完成具体任务（校对、整理、测试）
  B. 审核类——检查、评分、给意见
  C. 创意类——头脑风暴、策划、提案
  D. 其他（请描述）
```

### Q：spawn 后读错了 SOUL.md？

**A：** 检查是否同时传了 `agentId` + `cwd`。两个参数缺一不可。

### Q：新建的成员不汇报？

**A：** 在 SOUL.md 中明确写「任务完成后**立即**汇报，不等催」。并在首次派任务时提醒。

---

## 五、去AI味铁律

所有新创建的子代理 SOUL.md，必须包含以下检查：

| 检查项 | AI爱写 | 人应该写 |
|--------|--------|----------|
| 去AI味 | "仿佛/像/似乎/好像" | 用具体动作/感受替代 |
| 去格式 | 列表/"首先其次" | 自然段落 |
| 去注释 | `//`、`<!-- -->` | 删除所有注释 |

---

## 六、相关文档

- [OpenClaw Agent 配置](https://docs.openclaw.ai/config/agents)
- [sessions_spawn 文档](https://docs.openclaw.ai/automation/sessions)
- team-sessions skill：团队会话管理标准流程
