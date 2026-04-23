# ClawHub 发布注意事项

> ⚠️ **2026-04-23 更新**：本仓库发布操作已改为 **AI 准备命令，赵文龙手动执行**，禁止 AI 自行发布。

---

## 一、发布前检查清单

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

## 二、发布命令模板

```bash
# 1. 检查（每个 skill 发布前必做）
clawhub check ./quick-resurrection
clawhub check ./quick-team
clawhub check ./team-sessions

# 2. 模拟发布（确认无报错后再真发）
clawhub publish --dry-run ./quick-resurrection
clawhub publish --dry-run ./quick-team
clawhub publish --dry-run ./team-sessions

# 3. 正式发布（赵文龙执行）
clawhub publish ./quick-resurrection
clawhub publish ./quick-team
clawhub publish ./team-sessions
```

---

## 三、常用命令速查

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

## 四、已知问题记录

### 2026-04-23：发布操作失误导致回退

**问题：** AI 自行执行了 clawhub publish，操作不当导致需要回退。

**处理：** 赵文龙在 git 层执行了回退（`git revert` 或 `git reset`），仓库已恢复正常状态。

**教训：** ClawHub 发布涉及 clawhub.com 上的线上状态变更，影响所有用户，必须由人类确认后手动执行。

**规则：** AI 只负责：
1. 准备好完整的发布命令
2. 检查无误后通知赵文龙
3. 由赵文龙自行复制粘贴命令执行
