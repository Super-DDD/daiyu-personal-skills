# GitHub issue tracker

项目的 issue 和 spec 都保存为 GitHub issue。所有操作使用 `gh` CLI。

## 约定

- 创建 issue 用 `gh issue create --title "..." --body "..."`。多行正文用 heredoc。
- 读取 issue 用 `gh issue view <number> --json number,title,body,labels,comments`。
- 列表用 `gh issue list --state open --json number,title,body,labels,comments --jq '[.[] | {number, title, body, labels: [.labels[].name], comments: [.comments[].body]}]'`，并按需要设置 `--label`、`--state`。
- 建立 sub-issue 用 `gh issue create --parent <parent> ...`，或建好后用 `gh issue edit <parent> --add-sub-issue <child>`（`gh` 2.94+）。旧版 `gh` 使用 `gh api --method POST repos/<owner>/<repo>/issues/<parent>/sub_issues -F sub_issue_id=<child-db-id>`，其中 database ID 的取法见下文。缺少 sub-issue 功能时，在子 issue 正文顶部写 `Part of #<parent>`。
- 用 GitHub 原生 issue dependencies 记录 blocking edge，这样在 UI 也能看到。用 `gh issue create --blocked-by <n>,<n> ...` 或 `gh issue edit <child> --add-blocked-by <blocker>`（`gh` 2.94+）。旧版 `gh` 使用 `gh api --method POST repos/<owner>/<repo>/issues/<child>/dependencies/blocked_by -F issue_id=<blocker-db-id>`。其中 `<blocker-db-id>` 是 blocker 的数字 database ID，用 `gh api repos/<owner>/<repo>/issues/<n> --jq .id` 获取；`#number` 和 `node_id` 都不能代用。无法使用原生依赖时，在子 issue 正文顶部写 `Blocked by: #<n>, #<n>`。所有 blocker 都关闭后，ticket 即可开工。
- 统计尚未关闭的 blocker 用 `gh api repos/<owner>/<repo>/issues/<n> --jq .issue_dependencies_summary.blocked_by`，结果为 0 即可开工。`gh issue view <n> --json blockedBy` 也会列出已关闭的 blocker，所以要加 `--jq '[.blockedBy.nodes[] | select(.state == "OPEN")] | length'`。
- 评论用 `gh issue comment <number> --body "..."`。加、减 label 用 `gh issue edit <number> --add-label "..."`、`--remove-label "..."`。
- 关闭前先用 `gh issue comment <number> --body "..."` 发表说明，再执行 `gh issue close <number>`。对已关闭的 issue 执行 `gh issue close --comment` 会丢掉评论，但仍返回 0。

从 `git remote -v` 推断 repo，在 clone 内运行 `gh` 时会自动完成。只在用户自己的 repo 创建 issue。只有用户拥有 `mattpocock/skills` 时才可写入那里。对于 fork，`gh` 可能默认指向 upstream。在该会话第一次 `gh issue create` 前先用 `gh repo view --json nameWithOwner` 核对目的地，如果不属于用户就请其确认。

## 将 PR 用于 triage

**PRs as a request surface: no.** 如果项目把外部 PR 当作功能请求，设为 `yes`，由 `/triage` 读取。设为 `yes` 时，PR 使用与 issue 相同的 label 和状态，操作换用 `gh pr`。

- 先用 `gh pr view <number>` 读正文，再用 `gh pr view <number> --comments` 读评论。没有 TTY 时 `--comments` 只显示评论。用 `gh pr diff <number>` 读 diff。
- 列出待 triage 的外部 PR 用 `gh api --paginate 'repos/{owner}/{repo}/pulls?state=open' --jq '.[] | select(.author_association | IN("OWNER","MEMBER","COLLABORATOR") | not) | {number, title, author: .user.login, author_association, labels: [.labels[].name]}'`。
- 评论、加减 label、关闭分别用 `gh pr comment`、`gh pr edit --add-label` / `--remove-label`、`gh pr close`。

GitHub 的 issue 和 PR 共用编号。`#42` 可能指任一种，先试 `gh pr view 42`，再试 `gh issue view 42`。

## Skill 指令

“publish to the issue tracker” 表示创建 GitHub issue。“fetch the relevant ticket” 表示按上述读取 issue 的方法获取内容。

## Wayfinding 操作

`/wayfinder` 使用一个 issue 作为 map，用它的子 issue 作为 ticket。

- Map 使用 `wayfinder:map` label，正文含 Notes / Decisions-so-far / Fog。创建命令为 `gh issue create --label wayfinder:map`。
- Child ticket 建为 map 的 GitHub sub-issue。没有 sub-issue 功能时，将它加入 map 正文的 task list，并在子 issue 顶部写 `Part of #<map>`。使用 `wayfinder:<type>` label（`research` / `prototype` / `grilling` / `task`），领取后分配给负责的开发者。
- Blocking 见上面的原生依赖约定。
- 查询 frontier 时用 `gh issue list --state open` 列出 map 的未关闭子项，范围限定为它的 sub-issue / task list。排除已有 assignee 或仍有 open blocker 的子项，后者通过计数或 `Blocked by` 行判断。按 map 顺序取第一个。
- 领取用 `gh issue edit <n> --add-assignee @me`，这是该会话的第一次写操作。
- 解决时先 `gh issue comment <n> --body "<answer>"`，再 `gh issue close <n>`，最后给 map 的 Decisions-so-far 追加一条 context pointer，包含摘要和链接。

[英文原文](https://github.com/mattpocock/skills/blob/abeeaf9/skills/engineering/setup-matt-pocock-skills/issue-tracker-github.md)
