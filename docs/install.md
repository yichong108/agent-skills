# 安装

仓库地址：[https://github.com/yichong108/agent-skills](https://github.com/yichong108/agent-skills)

推荐用 [Skills CLI](https://github.com/vercel-labs/skills)（`npx skills`）从本仓库安装。

## 前提

使用 `npx skills` 前需先安装 [Node.js](https://nodejs.org/)（自带 `npm` / `npx`）。安装完成后在终端确认：

```bash
node -v
npx -v
```

未安装 Node.js 时，可改用下文「不用 CLI 时」的手动复制 / sparse checkout，不依赖 `npx`。

## 这条命令在做什么

```bash
npx skills add yichong108/agent-skills
```


| 部分                        | 含义                                                        |
| ------------------------- | --------------------------------------------------------- |
| `npx`                     | 临时下载并运行 npm 包，无需全局安装 CLI                                  |
| `skills`                  | Skills CLI 包名                                             |
| `add`                     | 从远程仓库拉取并安装 skill                                          |
| `yichong108/agent-skills` | GitHub 简写，等于 `https://github.com/yichong108/agent-skills` |


整句意思：

1. 从 GitHub 拉取本仓库里带 `SKILL.md` 的 skill 目录（不是把整个仓库当成你的业务项目来用）
2. 把该 skill **安装进目标 Agent 会扫描的 skills 目录**（见下方「范围」与「路径速查」）

安装方式有两种（交互安装时可选；命令行可用 `--copy` 强制复制）：


| 方式             | 怎么触发        | 实际效果                                 | 适合                                |
| -------------- | ----------- | ------------------------------------ | --------------------------------- |
| **软链接（默认，推荐）** | 不加 `--copy` | Agent 目录里是指向 skill 本体的符号链接；一处更新，各处生效 | macOS / Linux；支持 symlink 的环境      |
| **复制**         | 加 `--copy`  | 把 skill 文件完整复制进 Agent 目录，与源仓库断开      | Windows 权限受限、不支持 symlink，或希望独立副本时 |


示例：装到 Cursor 项目级后，目录里会出现类似：

```text
.agents/skills/hello-world/SKILL.md   # 软链或复制后的 skill
```


| 范围      | 怎么指定    | 装到哪                                                       |
| ------- | ------- | --------------------------------------------------------- |
| 项目级（默认） | 不加 `-g` | 当前工作目录下对应 Agent 的 skills 目录，例如 Cursor 的 `.agents/skills/` |
| 全局      | 加 `-g`  | 用户主目录下对应路径，例如 Cursor 的 `~/.cursor/skills/`                |


目标 Agent 用 `-a` 指定（如 `-a cursor`）；不写时 CLI 会检测本机已安装的 Agent，或交互让你选择。

## 路径里的 `~` 是什么

文档里的 `~` 表示**当前用户的主目录（home directory）**，不是项目目录。


| 系统                         | `~` 实际位置                     | 示例             |
| -------------------------- | ---------------------------- | -------------- |
| macOS                      | `$HOME`                      | `/Users/用户名`   |
| Linux                      | `$HOME`                      | `/home/用户名`    |
| Windows（Git Bash / 多数 CLI） | 用户主目录                        | `C:\Users\用户名` |
| Windows PowerShell         | `$env:USERPROFILE`（也可用 `$HOME`） | `C:\Users\用户名` |


在终端里可快速确认主目录：

```bash
# macOS / Linux / Git Bash（~ 与 $HOME 等价）
echo $HOME
```

```powershell
# Windows PowerShell（不要用 echo ~，不会展开）
echo $env:USERPROFILE
```

## 通用命令

```bash
# 查看仓库里有哪些 skill
npx skills add yichong108/agent-skills --list

# 安装全部 skills（交互：可能询问装哪些、装给哪个 Agent、软链还是复制）
npx skills add yichong108/agent-skills

# 只装某个 skill（默认软链接）
npx skills add yichong108/agent-skills --skill hello-world

# 强制复制，不用软链接
npx skills add yichong108/agent-skills --skill hello-world --copy

# 直接指向某个 skill 子目录
npx skills add https://github.com/yichong108/agent-skills/tree/main/skills/hello-world

# 全局安装（所有项目可用）
npx skills add yichong108/agent-skills --skill hello-world -g

# 非交互：跳过所有确认/选择菜单，直接按参数装完（适合 CI、自动化脚本）
# --skill / -a 写清「装哪个、给谁」；-y 表示全部自动确认
npx skills add yichong108/agent-skills --skill hello-world -a cursor -y
```

**非交互（CI / 脚本）说明：** 本地手动安装时，CLI 常会弹出「装哪些 skill / 装到哪个 Agent」等选项。在 GitHub Actions、初始化脚本等无人值守环境里没法点选，因此用 `--skill`、`-a` 把选择写死，再加 `-y`（`--yes`）跳过确认，实现一条命令跑完。

也可用完整 git URL：

```bash
npx skills add https://github.com/yichong108/agent-skills
npx skills add git@github.com:yichong108/agent-skills.git
```

## 按 Agent 安装

### Anthropic / Claude Code

```bash
# 项目级 → .claude/skills/
npx skills add yichong108/agent-skills --skill hello-world -a claude-code

# 全局 → ~/.claude/skills/
npx skills add yichong108/agent-skills --skill hello-world -a claude-code -g
```

手动放置：


| 范围  | 路径                                       |
| --- | ---------------------------------------- |
| 项目  | `.claude/skills/<skill-name>/SKILL.md`   |
| 全局  | `~/.claude/skills/<skill-name>/SKILL.md` |


Claude.ai（网页端）可在设置里上传自定义 skill 压缩包；详情见 [Using skills in Claude](https://support.claude.com/en/articles/12512180-using-skills-in-claude)。

### Cursor

```bash
# 项目级（CLI 默认写入 .agents/skills/；Cursor 同样识别）
npx skills add yichong108/agent-skills --skill hello-world -a cursor

# 全局 → ~/.cursor/skills/
npx skills add yichong108/agent-skills --skill hello-world -a cursor -g
```

Cursor 会扫描这些路径（任选其一即可）：


| 范围  | 路径                                        |
| --- | ----------------------------------------- |
| 项目  | `.cursor/skills/` 或 `.agents/skills/`     |
| 全局  | `~/.cursor/skills/` 或 `~/.agents/skills/` |


**兼容说明（仅 Cursor）：** 为方便复用已装给其他工具的 skill，Cursor 还会额外加载 Claude Code / Codex 的目录：`.claude/skills/`、`.codex/skills/`，以及 `~/.claude/skills/`、`~/.codex/skills/`。  
反过来不成立——Claude Code / Codex 不会读取 `.cursor/skills/`。详见 [Cursor Skills 文档](https://cursor.com/docs/skills)。

手动示例：

```bash
# 项目
mkdir -p .cursor/skills
cp -r /path/to/agent-skills/skills/hello-world .cursor/skills/

# 或全局
mkdir -p ~/.cursor/skills
cp -r /path/to/agent-skills/skills/hello-world ~/.cursor/skills/
```

### OpenCode

```bash
# 项目级 → .agents/skills/
npx skills add yichong108/agent-skills --skill hello-world -a opencode

# 全局 → ~/.config/opencode/skills/
npx skills add yichong108/agent-skills --skill hello-world -a opencode -g
```


| 范围  | 路径                                        |
| --- | ----------------------------------------- |
| 项目  | `.agents/skills/<skill-name>/`            |
| 全局  | `~/.config/opencode/skills/<skill-name>/` |


### OpenClaw

```bash
# 项目级 → skills/
npx skills add yichong108/agent-skills --skill hello-world -a openclaw

# 全局 → ~/.openclaw/skills/
npx skills add yichong108/agent-skills --skill hello-world -a openclaw -g
```


| 范围  | 路径                                 |
| --- | ---------------------------------- |
| 项目  | `skills/<skill-name>/`（仓库根下，无点前缀）  |
| 全局  | `~/.openclaw/skills/<skill-name>/` |


旧目录名（如 `~/.clawdbot`、`~/.moltbot`）若仍存在，部分工具也会识别。若使用 ClawHub，也可按 OpenClaw 文档用其 marketplace 命令安装。

### Codex

```bash
npx skills add yichong108/agent-skills --skill hello-world -a codex
npx skills add yichong108/agent-skills --skill hello-world -a codex -g
```


| 范围  | 路径                              |
| --- | ------------------------------- |
| 项目  | `.agents/skills/<skill-name>/`  |
| 全局  | `~/.codex/skills/<skill-name>/` |


### Gemini CLI

```bash
npx skills add yichong108/agent-skills --skill hello-world -a gemini-cli
npx skills add yichong108/agent-skills --skill hello-world -a gemini-cli -g
```


| 范围  | 路径                               |
| --- | -------------------------------- |
| 项目  | `.agents/skills/<skill-name>/`   |
| 全局  | `~/.gemini/skills/<skill-name>/` |


### GitHub Copilot

```bash
npx skills add yichong108/agent-skills --skill hello-world -a github-copilot
npx skills add yichong108/agent-skills --skill hello-world -a github-copilot -g
```


| 范围  | 路径                                |
| --- | --------------------------------- |
| 项目  | `.agents/skills/<skill-name>/`    |
| 全局  | `~/.copilot/skills/<skill-name>/` |


### Windsurf

```bash
npx skills add yichong108/agent-skills --skill hello-world -a windsurf
npx skills add yichong108/agent-skills --skill hello-world -a windsurf -g
```


| 范围  | 路径                                         |
| --- | ------------------------------------------ |
| 项目  | `.windsurf/skills/<skill-name>/`           |
| 全局  | `~/.codeium/windsurf/skills/<skill-name>/` |


### Cline / Warp / Zed / Kimi Code CLI

这些 Agent 共用「项目 `.agents/skills/` + 全局 `~/.agents/skills/`」约定，换 `-a` 即可：

```bash
npx skills add yichong108/agent-skills --skill hello-world -a cline
npx skills add yichong108/agent-skills --skill hello-world -a warp
npx skills add yichong108/agent-skills --skill hello-world -a zed
npx skills add yichong108/agent-skills --skill hello-world -a kimi-code-cli

# 全局示例
npx skills add yichong108/agent-skills --skill hello-world -a cline -g
```


| 范围  | 路径                               |
| --- | -------------------------------- |
| 项目  | `.agents/skills/<skill-name>/`   |
| 全局  | `~/.agents/skills/<skill-name>/` |


### Roo Code

```bash
npx skills add yichong108/agent-skills --skill hello-world -a roo
npx skills add yichong108/agent-skills --skill hello-world -a roo -g
```


| 范围  | 路径                            |
| --- | ----------------------------- |
| 项目  | `.roo/skills/<skill-name>/`   |
| 全局  | `~/.roo/skills/<skill-name>/` |


### Amp / Replit / Universal

```bash
npx skills add yichong108/agent-skills --skill hello-world -a amp
npx skills add yichong108/agent-skills --skill hello-world -a replit
npx skills add yichong108/agent-skills --skill hello-world -a universal
npx skills add yichong108/agent-skills --skill hello-world -a amp -g
```


| 范围  | 路径                                      |
| --- | --------------------------------------- |
| 项目  | `.agents/skills/<skill-name>/`          |
| 全局  | `~/.config/agents/skills/<skill-name>/` |


### Continue

```bash
npx skills add yichong108/agent-skills --skill hello-world -a continue
npx skills add yichong108/agent-skills --skill hello-world -a continue -g
```


| 范围  | 路径                                 |
| --- | ---------------------------------- |
| 项目  | `.continue/skills/<skill-name>/`   |
| 全局  | `~/.continue/skills/<skill-name>/` |


### Goose

```bash
npx skills add yichong108/agent-skills --skill hello-world -a goose
npx skills add yichong108/agent-skills --skill hello-world -a goose -g
```


| 范围  | 路径                                     |
| --- | -------------------------------------- |
| 项目  | `.goose/skills/<skill-name>/`          |
| 全局  | `~/.config/goose/skills/<skill-name>/` |


### Trae / Trae CN

```bash
npx skills add yichong108/agent-skills --skill hello-world -a trae
npx skills add yichong108/agent-skills --skill hello-world -a trae-cn
npx skills add yichong108/agent-skills --skill hello-world -a trae -g
```


| Agent   | 项目路径            | 全局路径                 |
| ------- | --------------- | -------------------- |
| Trae    | `.trae/skills/` | `~/.trae/skills/`    |
| Trae CN | `.trae/skills/` | `~/.trae-cn/skills/` |


### Qwen Code / Lingma（通义灵码）

```bash
npx skills add yichong108/agent-skills --skill hello-world -a qwen-code
npx skills add yichong108/agent-skills --skill hello-world -a lingma
npx skills add yichong108/agent-skills --skill hello-world -a qwen-code -g
```


| Agent     | 项目路径              | 全局路径                |
| --------- | ----------------- | ------------------- |
| Qwen Code | `.qwen/skills/`   | `~/.qwen/skills/`   |
| Lingma    | `.lingma/skills/` | `~/.lingma/skills/` |


### Augment / Kilo Code / Droid (Factory)

```bash
npx skills add yichong108/agent-skills --skill hello-world -a augment
npx skills add yichong108/agent-skills --skill hello-world -a kilo
npx skills add yichong108/agent-skills --skill hello-world -a droid
```


| Agent     | `--agent` | 项目路径                | 全局路径                  |
| --------- | --------- | ------------------- | --------------------- |
| Augment   | `augment` | `.augment/skills/`  | `~/.augment/skills/`  |
| Kilo Code | `kilo`    | `.kilocode/skills/` | `~/.kilocode/skills/` |
| Droid     | `droid`   | `.factory/skills/`  | `~/.factory/skills/`  |


### OpenHands / Antigravity / Kiro CLI

```bash
npx skills add yichong108/agent-skills --skill hello-world -a openhands
npx skills add yichong108/agent-skills --skill hello-world -a antigravity
npx skills add yichong108/agent-skills --skill hello-world -a antigravity-cli
npx skills add yichong108/agent-skills --skill hello-world -a kiro-cli
```


| Agent           | `--agent`         | 项目路径                 | 全局路径                                |
| --------------- | ----------------- | -------------------- | ----------------------------------- |
| OpenHands       | `openhands`       | `.openhands/skills/` | `~/.openhands/skills/`              |
| Antigravity     | `antigravity`     | `.agents/skills/`    | `~/.gemini/antigravity/skills/`     |
| Antigravity CLI | `antigravity-cli` | `.agents/skills/`    | `~/.gemini/antigravity-cli/skills/` |
| Kiro CLI        | `kiro-cli`        | `.kiro/skills/`      | `~/.kiro/skills/`                   |


Kiro 默认 Agent 会自动加载上述路径；若使用自定义 Agent，需在其配置的 `resources` 中加入 `skill://.kiro/skills/**/SKILL.md`。

### MiniMax Code / Mistral Vibe / AiderDesk / CodeBuddy

```bash
npx skills add yichong108/agent-skills --skill hello-world -a minimax-code
npx skills add yichong108/agent-skills --skill hello-world -a mistral-vibe
npx skills add yichong108/agent-skills --skill hello-world -a aider-desk
npx skills add yichong108/agent-skills --skill hello-world -a codebuddy
```


| Agent        | `--agent`      | 项目路径                  | 全局路径                    |
| ------------ | -------------- | --------------------- | ----------------------- |
| MiniMax Code | `minimax-code` | `.minimax/skills/`    | `~/.minimax/skills/`    |
| Mistral Vibe | `mistral-vibe` | `.vibe/skills/`       | `~/.vibe/skills/`       |
| AiderDesk    | `aider-desk`   | `.aider-desk/skills/` | `~/.aider-desk/skills/` |
| CodeBuddy    | `codebuddy`    | `.codebuddy/skills/`  | `~/.codebuddy/skills/`  |


### 其他 Agent（同一模式）

凡 Skills CLI 支持的 Agent，安装格式都一样，只换 `-a`：

```bash
npx skills add yichong108/agent-skills --skill hello-world -a <agent-id>
npx skills add yichong108/agent-skills --skill hello-world -a <agent-id> -g
```

常见 `<agent-id>` 还可选：`grok`、`junie`、`rovodev`、`qoder`、`qoder-cn`、`tabnine-cli`、`deepagents`、`devin`、`forgecode`、`crush`、`pochi`、`neovate`、`adal`、`bob` 等。完整列表见下方速查与 [Supported Agents](https://github.com/vercel-labs/skills#supported-agents)。

### 一次装到多个 Agent

```bash
npx skills add yichong108/agent-skills --skill hello-world \
  -a claude-code -a cursor -a opencode -a openclaw \
  -a windsurf -a cline -a gemini-cli -y
```

## 路径速查


| Agent                    | `--agent`                      | 项目路径                                    | 全局路径                                |
| ------------------------ | ------------------------------ | --------------------------------------- | ----------------------------------- |
| Claude Code              | `claude-code`                  | `.claude/skills/`                       | `~/.claude/skills/`                 |
| Cursor                   | `cursor`                       | `.agents/skills/`（也认 `.cursor/skills/`） | `~/.cursor/skills/`                 |
| OpenCode                 | `opencode`                     | `.agents/skills/`                       | `~/.config/opencode/skills/`        |
| OpenClaw                 | `openclaw`                     | `skills/`                               | `~/.openclaw/skills/`               |
| Codex                    | `codex`                        | `.agents/skills/`                       | `~/.codex/skills/`                  |
| Gemini CLI               | `gemini-cli`                   | `.agents/skills/`                       | `~/.gemini/skills/`                 |
| GitHub Copilot           | `github-copilot`               | `.agents/skills/`                       | `~/.copilot/skills/`                |
| Windsurf                 | `windsurf`                     | `.windsurf/skills/`                     | `~/.codeium/windsurf/skills/`       |
| Cline                    | `cline`                        | `.agents/skills/`                       | `~/.agents/skills/`                 |
| Roo Code                 | `roo`                          | `.roo/skills/`                          | `~/.roo/skills/`                    |
| Warp                     | `warp`                         | `.agents/skills/`                       | `~/.agents/skills/`                 |
| Zed                      | `zed`                          | `.agents/skills/`                       | `~/.agents/skills/`                 |
| Kimi Code CLI            | `kimi-code-cli`                | `.agents/skills/`                       | `~/.agents/skills/`                 |
| Amp / Replit / Universal | `amp` / `replit` / `universal` | `.agents/skills/`                       | `~/.config/agents/skills/`          |
| Continue                 | `continue`                     | `.continue/skills/`                     | `~/.continue/skills/`               |
| Goose                    | `goose`                        | `.goose/skills/`                        | `~/.config/goose/skills/`           |
| Trae                     | `trae`                         | `.trae/skills/`                         | `~/.trae/skills/`                   |
| Trae CN                  | `trae-cn`                      | `.trae/skills/`                         | `~/.trae-cn/skills/`                |
| Qwen Code                | `qwen-code`                    | `.qwen/skills/`                         | `~/.qwen/skills/`                   |
| Lingma                   | `lingma`                       | `.lingma/skills/`                       | `~/.lingma/skills/`                 |
| Augment                  | `augment`                      | `.augment/skills/`                      | `~/.augment/skills/`                |
| Kilo Code                | `kilo`                         | `.kilocode/skills/`                     | `~/.kilocode/skills/`               |
| Droid                    | `droid`                        | `.factory/skills/`                      | `~/.factory/skills/`                |
| OpenHands                | `openhands`                    | `.openhands/skills/`                    | `~/.openhands/skills/`              |
| Antigravity              | `antigravity`                  | `.agents/skills/`                       | `~/.gemini/antigravity/skills/`     |
| Antigravity CLI          | `antigravity-cli`              | `.agents/skills/`                       | `~/.gemini/antigravity-cli/skills/` |
| Kiro CLI                 | `kiro-cli`                     | `.kiro/skills/`                         | `~/.kiro/skills/`                   |
| MiniMax Code             | `minimax-code`                 | `.minimax/skills/`                      | `~/.minimax/skills/`                |
| Mistral Vibe             | `mistral-vibe`                 | `.vibe/skills/`                         | `~/.vibe/skills/`                   |
| AiderDesk                | `aider-desk`                   | `.aider-desk/skills/`                   | `~/.aider-desk/skills/`             |
| CodeBuddy                | `codebuddy`                    | `.codebuddy/skills/`                    | `~/.codebuddy/skills/`              |
| Grok Build               | `grok`                         | `.grok/skills/`                         | `~/.grok/skills/`                   |
| Junie                    | `junie`                        | `.junie/skills/`                        | `~/.junie/skills/`                  |
| Rovo Dev                 | `rovodev`                      | `.rovodev/skills/`                      | `~/.rovodev/skills/`                |
| Qoder                    | `qoder`                        | `.qoder/skills/`                        | `~/.qoder/skills/`                  |
| Qoder CN                 | `qoder-cn`                     | `.qoder/skills/`                        | `~/.qoder-cn/skills/`               |
| Tabnine CLI              | `tabnine-cli`                  | `.tabnine/agent/skills/`                | `~/.tabnine/agent/skills/`          |
| Deep Agents              | `deepagents`                   | `.agents/skills/`                       | `~/.deepagents/agent/skills/`       |
| Devin                    | `devin`                        | `.devin/skills/`                        | `~/.config/devin/skills/`           |
| ForgeCode                | `forgecode`                    | `.forge/skills/`                        | `~/.forge/skills/`                  |
| Crush                    | `crush`                        | `.crush/skills/`                        | `~/.config/crush/skills/`           |


更多 Agent 见 [Skills CLI Supported Agents](https://github.com/vercel-labs/skills#supported-agents)（共 70+）。

## 不用 CLI 时

```bash
git clone --depth 1 --filter=blob:none --sparse https://github.com/yichong108/agent-skills.git
cd agent-skills
git sparse-checkout set skills/hello-world

# 再把 skills/hello-world 放到目标 Agent 的 skills 目录，二选一：
# 复制：
mkdir -p .agents/skills && cp -r skills/hello-world .agents/skills/
# 软链接：
mkdir -p .agents/skills && ln -s "$(pwd)/skills/hello-world" .agents/skills/hello-world
```

上面示例以当前目录为目标项目；若装到全局，把目标改成如 `~/.cursor/skills/hello-world`。

## 验证

安装后新开一轮对话，用能命中 `description` 的说法测试，例如：「验证一下 hello-world skill」。

## 移除 Skill

用 Skills CLI 卸载已安装的 skill（`rm` 是 `remove` 的别名）：

```bash
# 交互选择要移除的 skill
npx skills remove

# 按名称移除
npx skills remove hello-world

# 只从某个 Agent 移除
npx skills remove hello-world -a cursor

# 从全局范围移除（对应安装时加了 -g 的情况）
npx skills remove hello-world -g

# 从所有 Agent 移除某个 skill
npx skills remove hello-world --agent '*'

# 清空某个 Agent 下已安装的全部 skill
npx skills remove --skill '*' -a cursor

# 非交互 / CI
npx skills remove hello-world -a cursor -y
```

也可手动删除对应目录（路径见上方「路径速查」），例如：

```bash
# 项目级（Cursor）
rm -rf .agents/skills/hello-world
rm -rf .cursor/skills/hello-world

# 全局（Cursor）
rm -rf ~/.cursor/skills/hello-world

# Windows PowerShell 示例
Remove-Item -Recurse -Force "$HOME\.cursor\skills\hello-world"
```

查看当前已安装的 skill：

```bash
npx skills list
npx skills list -g          # 只看全局
npx skills list -a cursor   # 按 Agent 过滤
```

## 安全提示

- 安装前阅读 `SKILL.md`
- 对含 `scripts/` 的 skill 做代码审查
- 优先使用项目级安装，缩小影响范围

