# Team Resurrection

任何子代理失联宕机，只需要一个命令——它自动扫描记忆碎片，找到断点，创建分身继续干活。比备份更聪明，比重写省十小时。

---

## 特性

- **三大场景**：搬家 · 分身 · 备份，三合一
- **自动检测**：不硬编码路径，自动推断 workspace
- **安全优先**：操作前自动备份，配置 deep merge
- **分身复制**：同环境快速克隆团队，ID/路径自动加后缀

## 快速开始

### 打包（备份/搬家）

```bash
cd ~/.qclaw/workspace/skills/team-resurrection
python3 pack.py
```

生成：`~/一键搬家包/{Agent名称}搬家包_YYYYMMDD.zip`

### 搬家（新环境恢复）

```bash
unzip 搬家包.zip && cd 搬家包 && python3 migrate.py
```

### 分身（同环境复制）

```bash
python3 clone.py --suffix "测试"
```

---

## 三大功能

| 功能 | 脚本 | 场景 |
|------|------|------|
| 打包 | `pack.py` | 备份 / 跨环境迁移 |
| 搬家 | `migrate.py` | 新环境恢复 |
| 分身 | `clone.py` | 同环境复制团队 |

---

## 版本

- **v3.0**（2026-04-23）— 新增clone.py分身功能
