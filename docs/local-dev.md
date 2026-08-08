# 本地开发与测试 Skill

在本仓库里改 skill，并挂到本地 Agent 上验证，无需先推送到 GitHub。

## 流程概览

```text
改 skills/<name>/  →  挂到 Agent 可扫描目录  →  新开对话触发测试  →  根据结果继续改
```

推荐用**软链接**挂载：改仓库里的文件后立刻生效，不必反复复制。

## 1. 创建或修改 Skill

```bash
# 新建
cp -r templates/skill-template skills/my-skill

# 编辑
# skills/my-skill/SKILL.md
# 确保 frontmatter 的 name 与目录名一致
```

编写约定见 [authoring-guide.md](authoring-guide.md)。

## 2. 挂到本地 Agent（开发时推荐）

在本仓库根目录执行。以下以 Cursor 为例，其他 Agent 换路径即可（见 [install.md 路径速查](install.md#路径速查)）。

### 方式 A：软链接（推荐）

改 `skills/my-skill/` 后无需再安装，Agent 读到的就是仓库里的文件。

**macOS / Linux / Git Bash：**

```bash
# 全局：所有项目可用
mkdir -p ~/.cursor/skills
ln -sfn "$(pwd)/skills/my-skill" ~/.cursor/skills/my-skill

# 或项目级：只在某个业务仓库里测
mkdir -p /path/to/your-app/.cursor/skills
ln -sfn "$(pwd)/skills/my-skill" /path/to/your-app/.cursor/skills/my-skill
```

**Windows PowerShell**（需开启[开发人员模式](https://learn.microsoft.com/windows/apps/get-started/enable-your-device-for-development)，或用管理员终端）：

```powershell
# 全局
New-Item -ItemType Directory -Force "$HOME\.cursor\skills" | Out-Null
New-Item -ItemType SymbolicLink `
  -Path "$HOME\.cursor\skills\my-skill" `
  -Target "$(Get-Location)\skills\my-skill" `
  -Force

# 或项目级
New-Item -ItemType Directory -Force "D:\path\to\your-app\.cursor\skills" | Out-Null
New-Item -ItemType SymbolicLink `
  -Path "D:\path\to\your-app\.cursor\skills\my-skill" `
  -Target "$(Get-Location)\skills\my-skill" `
  -Force
```

无法建软链时，改用复制（每次改完要再拷一次）：

```powershell
Copy-Item -Recurse -Force .\skills\my-skill "$HOME\.cursor\skills\my-skill"
```



### 方式 B：Skills CLI 安装本地路径

需已安装 [Node.js](https://nodejs.org/)。

```bash
# 从本仓库安装指定 skill（默认软链）
npx skills add . --skill my-skill -a cursor -y

# 或直接指向 skill 目录
npx skills add ./skills/my-skill -a cursor -y

# 全局
npx skills add ./skills/my-skill -a cursor -g -y
```

换 Agent 时改 `-a`，例如 `-a claude-code`、`-a opencode`。

### 其他常见 Agent 挂载目标


| Agent       | 全局（开发常用）                           | 项目级                                               |
| ----------- | ---------------------------------- | ------------------------------------------------- |
| Cursor      | `~/.cursor/skills/<name>`          | `.cursor/skills/<name>` 或 `.agents/skills/<name>` |
| Claude Code | `~/.claude/skills/<name>`          | `.claude/skills/<name>`                           |
| OpenCode    | `~/.config/opencode/skills/<name>` | `.agents/skills/<name>`                           |
| Codex       | `~/.codex/skills/<name>`           | `.agents/skills/<name>`                           |


完整列表见 [install.md](install.md#路径速查)。

## 3. 怎么测

1. **确认已挂上**
  - Cursor：侧边栏 Customize → Skills，应能看到该 skill
  - 或：`npx skills list` / `npx skills list -g`
2. **新开一轮 Agent 对话**（避免旧上下文干扰）
3. **用会命中** `description` **的自然语言**试触发，例如：
  - 直接点名：「用 my-skill 做 …」
  - 场景描述：写一段符合 description 里「何时使用」的请求
4. **也可显式调用**（若 Agent 支持）：在输入框用 `/my-skill` 或 `@my-skill`
5. **对照期望**
  - 是否按 `SKILL.md` 的步骤执行
  - 输出格式是否符合模板/Examples
  - 需要时是否去读了 `references/`、跑了 `scripts/`

用仓库自带示例自检：

```bash
# 先挂上 hello-world，再新开对话说：
# 「验证一下 hello-world skill」
```



## 4. 迭代修改


| 步骤                                            | 说明                                                     |
| --------------------------------------------- | ------------------------------------------------------ |
| 改仓库里的 `SKILL.md` / `references/` / `scripts/` | 软链挂载时无需重装                                              |
| 若当初用了 `--copy` 或手动复制                          | 改完后重新复制，或改成软链                                          |
| 再测                                            | **新开对话**再触发；部分 Agent 启动时缓存 skill 列表，必要时重启 Agent / 重载窗口 |
| 稳定后                                           | 更新 `catalog.yaml`                                      |




## 5. 可选校验

```bash
# 若已安装 skills-ref
skills-ref validate ./skills/my-skill
```

也可自查：

- [ ] `name` 与目录名一致
- [ ] `description` 含做什么 + 何时用
- [ ] 用 2～3 条真实用户口吻的 prompt 试触发（应触发 / 不应误触发各至少一条）
- [ ] 含 `scripts/` 时，在干净环境跑通一遍



## 6. 卸下本地挂载

```bash
# CLI
npx skills remove my-skill -a cursor -y
npx skills remove my-skill -a cursor -g -y

# 或删掉链接/目录
rm ~/.cursor/skills/my-skill          # macOS / Linux（软链或目录）
# PowerShell:
# Remove-Item "$HOME\.cursor\skills\my-skill"
```

这只影响本机 Agent 挂载，不会删除仓库里的 `skills/my-skill/`。

