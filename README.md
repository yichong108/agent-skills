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

1. 编辑 `skills/my-skill/SKILL.md`：
  - `name` 必须与目录名一致（小写、数字、连字符）
  - `description` 写清 **做什么** 和 **何时使用**
2. 在 `catalog.yaml` 中登记该 skill
3. 按 [docs/install.md](docs/install.md) 安装到目标 Agent



## 设计原则

- **渐进披露**：启动时只加载 `name` + `description`；命中后再读 `SKILL.md`；`scripts/` / `references/` / `assets/` 按需加载
- **精简主文件**：`SKILL.md` 建议 < 500 行，细节放到 `references/`
- **相对路径、一层引用**：从 `SKILL.md` 直接链到资源文件，避免深层嵌套
- **工具无关**：不写某家产品私有约定，优先遵循 [agentskills.io/specification](https://agentskills.io/specification)



## 文档


| 文档                                                 | 说明                  |
| -------------------------------------------------- | ------------------- |
| [docs/install.md](docs/install.md)                 | 安装、各 Agent 路径、移除与验证 |
| [docs/authoring-guide.md](docs/authoring-guide.md) | 如何编写高质量 skill       |
| [CONTRIBUTING.md](CONTRIBUTING.md)                 | 贡献流程                |




## 参考

- 仓库：[https://github.com/yichong108/agent-skills](https://github.com/yichong108/agent-skills)
- 规范：[https://agentskills.io/specification](https://agentskills.io/specification)
- Skills CLI：[https://github.com/vercel-labs/skills](https://github.com/vercel-labs/skills)
- 技能发现：[https://skills.sh](https://skills.sh)
- 示例技能集：[https://github.com/anthropics/skills](https://github.com/anthropics/skills)

