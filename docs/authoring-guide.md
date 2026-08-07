# Skill 编写指南

面向本仓库的通用编写约定，兼容 [Agent Skills 规范](https://agentskills.io/specification)。

## 目录约定

```text
skills/<skill-name>/
├── SKILL.md          # 必需
├── scripts/          # 可选：确定性/重复性任务的脚本
├── references/       # 可选：按需阅读的详细文档
└── assets/           # 可选：模板、图标、样例数据
```

`name` 必须等于父目录名。

## Frontmatter

| 字段 | 必需 | 说明 |
|------|------|------|
| `name` | 是 | 小写、数字、连字符；≤64 |
| `description` | 是 | 做什么 + 何时用；≤1024 |
| `license` | 否 | 如 `MIT` |
| `compatibility` | 否 | 环境/依赖要求；多数 skill 不需要 |
| `metadata` | 否 | 自定义键值，如 `author`、`version` |
| `allowed-tools` | 否 | 实验性：预批准工具列表 |

### description 写法

用第三人称，包含 WHAT 与 WHEN：

```yaml
# 好
description: 从 PDF 提取文本与表格、填写表单、合并文档。在用户处理 PDF、表单或文档抽取时使用。

# 差
description: 帮助处理 PDF。
```

## 正文结构建议

1. **Quick start**：最短可执行路径
2. **Workflow / Checklist**：多步骤任务拆开
3. **Examples**：输入 → 期望输出
4. **Additional resources**：链到 `references/` 与 `scripts/`

## 渐进披露

1. 元数据（`name` + `description`）— 始终可见
2. `SKILL.md` 正文 — 触发后加载（建议 < 5000 tokens / < 500 行）
3. `scripts/`、`references/`、`assets/` — 仅在需要时读取或执行

细节放进 `references/`，主文件只保留决策与步骤。

## Scripts

- 依赖写清楚（语言版本、包名）
- 错误信息可读
- 在 `SKILL.md` 标明是「执行」还是「当作参考阅读」

```markdown
运行抽取脚本：

```bash
python scripts/extract.py input.pdf > fields.json
```
```

## 反模式

- 模糊命名：`helper`、`utils`、`tools`
- 塞太多可选方案却不给默认路径
- Windows 反斜杠路径：`scripts\foo.py`
- 把易过期的时间线写进主流程（可放在「旧方案」折叠段）
- 深层引用链：`A → B → C`

## 校验

若已安装 [skills-ref](https://github.com/agentskills/agentskills/tree/main/skills-ref)：

```bash
skills-ref validate ./skills/my-skill
```
