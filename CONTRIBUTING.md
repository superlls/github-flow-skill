# 贡献指南

感谢你对 github-flow-skill 的关注！本项目欢迎任何形式的贡献。

## 提交改动的流程

本仓库**身体力行**自己倡导的流程——所有改动都走
**分支 → Issue → PR → 自审 → 合并**，不直接 push 到 `main`：

1. **开 Issue**：先描述你想做什么或修什么。
2. **切分支**：分支名用 `<类型>/<简短描述>`，如 `feat/xxx`、`fix/xxx`、`docs/xxx`。
3. **提交**：commit message 用中文描述改动，类型前缀同分支。
4. **开 PR**：描述里写 `Closes #<Issue编号>` 关联 Issue。
5. **等待 review 后合并**：用 squash 合并，并删除分支。

> 装了本仓库的 `/github` 技能后，以上流程一条命令即可自动跑完。

## 改动类型约定

| 前缀 | 含义 |
|------|------|
| `feat` | 新功能 |
| `fix` | 修复 bug |
| `docs` | 文档改动 |
| `refactor` | 重构（不改行为） |
| `chore` | 杂项（依赖、配置、清理等） |

## 报告问题

发现 bug 或有功能建议，欢迎直接开 [Issue](https://github.com/superlls/github-flow-skill/issues)。
