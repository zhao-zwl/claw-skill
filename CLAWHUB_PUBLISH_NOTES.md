# ClawHub 发布注意事项

> ⚠️ **2026-04-23 更新**：本仓库发布操作已改为 **AI 准备命令，赵文龙手动执行**，禁止 AI 自行发布。

---

## 一、版本号规范（强制）

格式：`主版本.次版本.修订号`（语义化版本）

| 级别 | 触发条件 |
|------|----------|
| 修订号 +1 | 小改动：修正错别字、格式调整、文档补充 |
| 次版本 +1 | 新增功能：新增章节、新模板、新流程 |
| 主版本 +1 | 破坏性变更：接口重写、流程大变 |

**发布后：同步更新本文件最下方的「当前版本表」**

---

## 二、发布前检查清单

发布每个 skill 前，逐一确认：

- [ ] `clawhub check <skill-dir>` 无报错
- [ ] `clawhub publish --dry-run <skill-dir>` 模拟发布成功
- [ ] README.md 存在且内容正确
- [ ] SKILL.md 存在，`name` 和 `description` 已填
- [ ] 无团队硬编码残留（workspace 路径、成员名、agent ID 等）
- [ ] CHANGELOG.md 已更新版本号和本次改动说明
- [ ] `git add + commit` 已完成
- [ ] 已通知赵文龙确认，再由赵文龙执行发布命令

---

## 三、发布命令模板

```bash
# quick-team v1.1.0（首次发布）
clawhub publish /Users/zhaowenlong/claw-skill/quick-team --slug quick-team --name "Quick Team" --version 1.1.0 --changelog "SKILL.md全面升级：新增metadata.name字段、6步创建流程补全、新增子代理标准目录结构、新增去AI味铁律章节；补全所有模板和示例（MEMORY.md/HEARTBEAT.md）；README.md升级为完整说明文档"

# quick-resurrection v2.2.0（重大升级）
clawhub publish /Users/zhaowenlong/claw-skill/quick-resurrection --slug quick-resurrection --name "Quick Resurrection" --version 2.2.0 --changelog "cron迁移重大升级：直接从~/.qclaw/cron/jobs.json读取完整任务定义（schedule+payload+delivery全保留）；修复openclaw tasks list返回格式误判；历史记录按sourceId去重（586条→3条）；migrate自动重建cron任务"

# team-resurrection v1.0.0（首次正式发布）
clawhub publish /Users/zhaowenlong/claw-skill/team-resurrection --slug team-resurrection --name "Team Resurrection" --version 1.0.0 --changelog "分身复活：Agent失联后自动扫描子代理session找接续点，创建新agent继承记忆继续工作；clone.py支持scan/simulate/show三种模式"

# team-sessions v1.0.0（首次正式发布）
clawhub publish /Users/zhaowenlong/claw-skill/team-sessions --slug team-sessions --name "Team Sessions" --version 1.0.0 --changelog "团队沟通规范v1.0：零横向沟通模式、workspace隔离机制、标准派任务流程、通用成员配置模板"
```

---

## 四、常用命令速查

```bash
# 列出所有 skills
clawhub list

# 搜索 skill
clawhub search <keyword>

# 安装
clawhub install <skill-name>
clawhub install --source <local-dir>

# 查看本地 skill 信息
clawhub check <local-dir>

# 发布（本地目录 → clawhub）
clawhub publish [--dry-run] <local-dir>

# 更新已安装的 skill
clawhub update <skill-name>

# 卸载
clawhub uninstall <skill-name>

# 查看版本
clawhub --version
```

---

## 五、当前版本表（每次发布后更新）

| Skill | Slug | 本地版本 | ClawHub已发布 |
|-------|------|----------|--------------|
| Quick Team | quick-team | **1.1.0** | 1.0.0（待发布1.1.0） |
| Quick Resurrection | quick-resurrection | **2.2.0** | ？ |
| Team Resurrection | team-resurrection | **1.0.0** | 未发布（待首次发布） |
| Team Sessions | team-sessions | **1.0.0** | 未发布（待首次发布） |

---

## 六、已知问题记录

### 2026-04-23：发布操作失误导致回退

**问题：** AI 自行执行了 clawhub publish，操作不当导致需要回退。

**处理：** 赵文龙在 git 层执行了回退（`git revert` 或 `git reset`），仓库已恢复正常状态。

**教训：** ClawHub 发布涉及 clawhub.com 上的线上状态变更，影响所有用户，必须由人类确认后手动执行。

**规则：** AI 只负责：
1. 准备好完整的发布命令
2. 检查无误后通知赵文龙
3. 由赵文龙自行复制粘贴命令执行
