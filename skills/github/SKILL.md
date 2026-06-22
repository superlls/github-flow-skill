---
name: github
description: 把本地待提交的改动走一条正规的 GitHub 流程——分支 → Issue → PR → 自审 comment → 等待确认后 squash 合并。用于把雷达图四个维度（Commits / Issues / Pull requests / Code review）同时刷起来。仅在用户显式执行 /github 时触发；绝不在普通提交、推送或其他对话中自动触发。
---

# /github —— 正规化提交流程

把用户本地准备提交的改动，从"裸 push"变成一条完整的协作流程：
**分支 → Issue → PR → 自审 → 合并**。一次跑完，GitHub 雷达图的
Commits / Issues / Pull requests / Code review 四个维度同时 +1。

## 触发规则（重要）

- **只有用户显式执行 `/github` 时才运行本流程。**
- 用户平时的 `git commit` / `git push` / 其他对话，**绝不**自动套用本流程。
- 如果用户只是问问题，不要直接动手，先确认。

## 用户已确认的偏好（不要再问）

1. **合并方式：跑到 merge 时停下来等确认。** 把分支、Issue、PR、自审全做完，
   合并前**必须停下来**，给用户看 PR 链接和 diff 摘要，等用户明确说"合并 / 合 / merge / 确认"
   之后才执行 `gh pr merge`。绝不自动合并。
2. **Review 方式：只贴一句自查 comment。** 不调用 /code-review，不做实质审查，
   只在 PR 上贴一条简短的 self-review comment 占位（计入 Code review 维度）。

## 前置检查

按顺序确认，任何一项不满足就停下来告诉用户，别硬跑：

1. 当前目录是 git 仓库：`git rev-parse --is-inside-work-tree`
2. `gh` 已安装：`gh --version`（未安装就提示用户 `brew install gh` 或见 https://cli.github.com/）
3. `gh` 已登录：`gh auth status`（未登录就提示用户 `gh auth login`）
4. 仓库有 GitHub remote：`git remote -v`（没有就提示用户先 `gh repo create` 或加 remote）
5. 有改动要提交：`git status --porcelain`（完全干净就告诉用户没东西可提交）

## 执行步骤

### 0. 摸清现状
```bash
git status --porcelain        # 看有哪些改动
git branch --show-current     # 当前在哪个分支
git diff --stat               # 改动规模
git diff                      # 看具体改了什么，用来生成 Issue/PR/commit 文案
```
读完 diff，**自己总结**这次改动是什么、属于哪类（feat/fix/docs/refactor/chore），
不要为这种能自己判断的事去问用户。

### 1. 建 Issue
根据 diff 内容生成一个清晰的中文 Issue：
```bash
gh issue create --title "<简明标题>" --body "<需求/背景，2-4 句>"
```
**记下返回的 Issue 编号**（如 #12），后面 PR 要用。

### 2. 切分支
分支名格式 `<类型>/<简短英文描述>`，例如 `feat/history-list`、`fix/login-timeout`。

- **当前在 main/master 且改动未提交**：直接 `git checkout -b <分支名>`，
  未提交的改动会自动跟着带到新分支。
- **当前已在一个功能分支上**：沿用当前分支即可，不用新建。
- **改动已经 commit 到了 main 但没 push**：先 `git branch <分支名>` 保存，
  再 `git checkout <分支名>`，然后把 main 回退到远端状态
  （`git checkout main && git reset --hard origin/main && git checkout <分支名>`）。
  这一步有风险，执行前先跟用户说明并确认。

### 3. 提交并推送
```bash
git add -A
git commit -m "<类型>: <中文描述>"
git push -u origin <分支名>
```
commit message 用中文描述改动，结尾加上：
```
Co-Authored-By: Claude Opus 4.8 (1M context) <noreply@anthropic.com>
```

### 4. 开 PR（关联 Issue）
PR 描述里**必须**写 `Closes #<Issue编号>`，这样合并后会自动关掉 Issue：
```bash
gh pr create --title "<类型>: <标题>" --body "$(cat <<'EOF'
<改动说明>

Closes #<Issue编号>
EOF
)"
```
**记下返回的 PR 编号。**

### 5. 自审 comment
GitHub 不允许 approve 自己的 PR，所以用 `--comment`（一样计入 Code review）：
```bash
gh pr review <PR编号> --comment --body "self-review：已本地测试，逻辑无误。"
```

### 6. 停下来等确认（关键）
**到这里必须停。** 向用户报告：
- Issue 链接 / 编号
- PR 链接 / 编号
- 一句话 diff 摘要（改了哪些文件、做了什么）

然后明确问：**"确认合并吗？回复'合并'我就 squash 合进 main 并删分支。"**
在用户明确同意前，**不要**执行任何合并命令。

### 7. 合并（仅在用户确认后）
```bash
gh pr merge <PR编号> --squash --delete-branch
```
合并后告诉用户：main 已更新、Issue 已自动关闭、分支已删除。

## 收尾报告

跑完后用一句话总结这次循环覆盖了哪几个雷达维度，让用户有"四个角都 +1"的反馈感。

## 异常处理

- 任一 `gh`/`git` 命令报错，停下来把原始报错给用户，别盲目重试。
- 用户中途说"不合了"，就保留 PR 开着，告诉他 PR 链接，下次可以手动合或继续改。
