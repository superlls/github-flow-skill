# github-flow-skill

> 一个 Claude Code 技能（Skill）：把"裸 `git push`"升级成一条正规的 GitHub 协作流程。
> 一条命令 `/github`，让你的贡献雷达图四个维度——**Commits / Issues / Pull requests / Code review**——同时刷起来。

## 这是什么

很多人（尤其是个人项目）习惯直接 `git commit && git push` 到 `main`，结果 GitHub
个人主页的活动雷达图变成一条**横线**：Commits 占了 97%，Pull requests 个位数，
Issues 和 Code review 直接是 0。

这个 Skill 让 Claude Code 在你输入 `/github` 时，自动把当前改动走完整流程：

```
① 建 Issue        → 计入 Issues
② 切规范分支
③ commit + push
④ 开 PR（Closes #Issue） → 计入 Pull requests
⑤ 贴自审 comment   → 计入 Code review
⑥ 停下来等你确认
⑦ squash 合并 + 删分支 + 自动关 Issue → 计入 Commits
```

一次跑完，**雷达图四个角同时 +1**。坚持几周，那条横线就会鼓成一个饱满的多边形。

## 特点

- **全程命令行**：基于 [`gh` CLI](https://cli.github.com/)，不用打开 GitHub 网页。
- **显式触发**：只有你输入 `/github` 才运行。平时的 `git commit` / `git push`
  完全不受影响，绝不自动套用。
- **合并前刹车**：分支、Issue、PR、自审全自动完成，但**合并这一步会停下来**，
  给你 PR 链接和 diff 摘要，等你说"合并"才执行——不可逆的操作绝不替你做主。
- **中文友好**：自动生成的 Issue、PR、commit 文案都是中文。

## 安装

前提：已安装 [`gh` CLI](https://cli.github.com/) 并完成 `gh auth login`。

把 `skills/github/` 放到 Claude Code 的技能目录即可：

```bash
# 克隆本仓库
git clone https://github.com/superlls/github-flow-skill.git
cd github-flow-skill

# 软链接到全局技能目录（推荐，方便随仓库更新）
ln -s "$(pwd)/skills/github" ~/.claude/skills/github

# 或者直接复制
cp -r skills/github ~/.claude/skills/github
```

重启 Claude Code（或新开一个会话），输入 `/github` 即可生效。

## 用法

在**任意一个 git 仓库目录**里，写完代码、准备提交时，别急着 push，直接：

```
/github
```

Claude 会：

1. 看你当前的改动和分支
2. 根据 diff 生成 Issue 并建好
3. 切一条规范命名的分支（`feat/xxx`、`fix/xxx`…），把改动带过去
4. commit + push
5. 开 PR，关联 Issue
6. 贴一条自审 comment
7. **停下来** → 给你 PR 链接 + diff 摘要 → 等你确认
8. 你回复"合并"后 → squash 合并 + 删分支 + 自动关 Issue

## 自定义

技能的全部行为都写在 `skills/github/SKILL.md` 里，纯自然语言，直接改即可。
常见改法：

- **想真审一遍代码**：把"贴自审 comment"那步换成调用 `/code-review`，
  把审查结论作为 PR comment 贴上去。
- **想全自动一条龙**：删掉"停下来等确认"那一步，让它直接合并。
- **改分支/commit 命名规范**：在对应步骤里改约定即可。

## 为什么走这套流程（而不只是为了好看）

刷雷达图只是副产品。真正的收益是：

- **Issue** 逼你"想清楚再动手"，每个改动有据可查。
- **分支** 隔离改动，写崩了也不影响 `main`。
- **PR** 给每次改动一个可回溯、可讨论的页面。
- **Review** 是合并前重读自己 diff、发现低级 bug 最便宜的时机。

## License

MIT

test
