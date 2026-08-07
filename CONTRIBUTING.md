# 贡献指南

感谢贡献。本仓库遵循 [Agent Skills](https://agentskills.io/specification) 开放规范。

## 新增 Skill

1. 从模板复制：

```bash
cp -r templates/skill-template skills/<skill-name>
```

2. 保证目录名与 `SKILL.md` 中的 `name` 一致
3. 写好 `description`（包含做什么 + 何时用 + 触发关键词）
4. 按 [docs/local-dev.md](docs/local-dev.md) 本地挂载并测试通过
5. 更新根目录 `catalog.yaml`
6. 如有脚本，文档化依赖与用法；避免绑定单一 Agent 专有能力

## 命名约定

- 目录与 `name`：`lowercase-with-hyphens`
- 长度：1–64 字符
- 不要以连字符开头/结尾，不要连续连字符 `--`

## 质量检查清单

- [ ] frontmatter 含 `name`、`description`
- [ ] `name` 与目录名一致
- [ ] `description` 具体，含触发场景
- [ ] `SKILL.md` 主体简洁（建议 < 500 行）
- [ ] 参考文件从 `SKILL.md` 一层链接（不经过 `A → B → C`）
- [ ] 路径相对 skill 根目录，使用正斜杠（`scripts/foo.py`）
- [ ] 不包含密钥、凭证或恶意代码

## 提交建议

- 一个 PR 聚焦一个 skill，或一组紧密相关的改动
- commit message 用中文说明「为什么」而不是罗列文件
