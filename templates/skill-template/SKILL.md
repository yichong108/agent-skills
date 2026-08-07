---
name: skill-template
description: >
  模板 skill。复制本目录到 skills/<name>/ 后，将 name 改为目录名，
  并重写 description（做什么 + 何时用）。仅作新建起点，不应作为可运行 skill 触发。
license: MIT
metadata:
  version: "0.1.0"
  author: agent-skills
---

# Skill Template

将本文件改成真实指令。Agent 激活本 skill 后会把整份正文载入上下文。

## Quick start

1. 明确任务目标与输入
2. 按下方工作流执行
3. 需要细节时再读 `references/`，需要确定性步骤时再跑 `scripts/`

## Workflow

- [ ] 确认输入与约束
- [ ] 执行核心步骤
- [ ] 校验输出
- [ ] 按约定格式交付结果

## Instructions

在此写清步骤、默认工具选择、错误处理与输出格式。

保持简洁：默认假设 Agent 已具备通用能力，只补充领域知识、团队约定与脆弱流程。

## Examples

**Example 1**

- Input: …
- Output: …

## Additional resources

- 详细参考：[references/REFERENCE.md](references/REFERENCE.md)
- 可执行脚本：见 `scripts/`（在指令中写明如何调用）
- 静态资源：见 `assets/`
