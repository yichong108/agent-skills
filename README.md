# Agent Skills

跨 Agent 通用的 Skills 仓库，遵循 [Agent Skills](https://agentskills.io) 开放规范。

## 安装

完整说明见 **[docs/install.md](docs/install.md)**。

快速示例（需先安装 [Node.js](https://nodejs.org/)）：

```bash
# 查看仓库里有哪些 skill
npx skills add yichong108/agent-skills --list

# 安装到 Cursor（项目级）
npx skills add yichong108/agent-skills --skill hello-world -a cursor

# 全局安装
npx skills add yichong108/agent-skills --skill hello-world -a cursor -g
```

## 快速开始（编写新 skill）

1. 复制模板：

```bash
cp -r templates/skill-template skills/my-skill
```

2. 编辑 `skills/my-skill/SKILL.md`：
   - `name` 必须与目录名一致（小写、数字、连字符）
   - `description` 写清 **做什么** 和 **何时使用**
3. 本地挂载并测试：见 **[docs/local-dev.md](docs/local-dev.md)**（推荐软链接，改完即生效）
4. 在 `catalog.yaml` 中登记该 skill，并推送到 GitHub
5. 其他人（或你在别的机器上）按 **[docs/install.md](docs/install.md)** 从仓库安装，例如：  
   `npx skills add yichong108/agent-skills --skill my-skill -a cursor`

编写约定与设计原则见 **[docs/authoring-guide.md](docs/authoring-guide.md)**。

## 文档


| 文档                                                 | 说明                             |
| -------------------------------------------------- | ------------------------------ |
| [docs/install.md](docs/install.md)                 | **使用者**：从 GitHub 安装 / 移除 skill |
| [docs/local-dev.md](docs/local-dev.md)             | **开发者**：本地改 skill 并挂载测试        |
| [docs/authoring-guide.md](docs/authoring-guide.md) | 设计原则与编写约定 |
| [CONTRIBUTING.md](CONTRIBUTING.md)                 | 贡献流程                           |


## 参考

- 仓库：[https://github.com/yichong108/agent-skills](https://github.com/yichong108/agent-skills)
- 规范：[https://agentskills.io/specification](https://agentskills.io/specification)
- Skills CLI：[https://github.com/vercel-labs/skills](https://github.com/vercel-labs/skills)
- 技能发现：[https://skills.sh](https://skills.sh)
- 示例技能集：[https://github.com/anthropics/skills](https://github.com/anthropics/skills)

