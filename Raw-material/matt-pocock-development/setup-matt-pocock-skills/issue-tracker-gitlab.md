# GitLab issue tracker

项目的 issue 和 spec 都保存为 GitLab issue。所有操作使用 [`glab`](https://gitlab.com/gitlab-org/cli) CLI。

## 约定

- 创建用 `glab issue create --title "..." --description "..."`。多行描述用 heredoc。传入 `--description -` 可打开编辑器。
- 读取用 `glab issue view <number> --comments`。机器可读输出加 `-F json`。
- 列表用 `glab issue list -O json`，配合合适的 `--label` 过滤。
- 评论用 `glab issue note <number> --message "..."`。GitLab 称评论为 note。
- 加减 label 用 `glab issue update <number> --label "..."` / `--unlabel "..."`。多个 label 可用逗号分隔，也可重复选项。
- 关闭用 `glab issue close <number>`。`glab issue close` 不接受关闭说明，因此先用 `glab issue note <number> --message "..."` 发 note，再关闭。
- GitLab 把 PR 称为 merge request（MR）。操作包括 `glab mr create`、`glab mr view`、`glab mr note` 等。形式与 `gh pr ...` 相同，将 `pr` 换为 `mr`，将 `comment` / `--body` 换为 `note` / `--message`。

从 `git remote -v` 推断 repo。在 clone 内运行 `glab` 会自动完成。

## 将 MR 用于 triage

**MRs as a request surface: no.** 如果项目把外部 MR 当作功能请求，设为 `yes`，由 `/triage` 读取。设为 `yes` 时，MR 沿用 issue 的 label 和状态，操作换用 `glab mr`。

- 用 `glab mr view <number> --comments` 读取 MR，用 `glab mr diff <number>` 读取 diff。
- 用 `glab mr list -O json` 列出 MR，然后只保留作者为外部贡献者的 MR，排除项目成员或 owner 的在办工作。
- 评论、加减 label、关闭分别用 `glab mr note`、`glab mr update --label` / `--unlabel`、`glab mr close`。

GitLab 的 issue 和 MR 分别编号。确定维护者指哪种对象，就知道 `#42` 对应什么。

## Skill 指令

“publish to the issue tracker” 表示创建 GitLab issue。“fetch the relevant ticket” 表示运行 `glab issue view <number> --comments`。

## Wayfinding 操作

`/wayfinder` 使用一个 issue 作为 map，用它的子 issue 作为 ticket。

- Map 使用 `wayfinder:map` label，正文含 Notes / Decisions-so-far / Fog。用 `glab issue create --label wayfinder:map` 创建。套餐支持原生 epic 时也可用 epic，带 label 的 issue 适用于所有套餐。
- Child ticket 正文顶部写 `Part of #<map>`，用 `glab issue create --linked-issues <map> --link-type relates_to` 链接 map，加上 `wayfinder:<type>` label（`research` / `prototype` / `grilling` / `task`）。领取后分配给负责的开发者。
- Blocking 使用可在 UI 查看且作为准据的原生 blocking link。把 `/blocked_by #<n>` quick action 作为 note 发布，命令为 `glab issue note <child> --message "/blocked_by #<blocker>"`。原生 blocking link 需要 Premium 或 Ultimate。免费版或原生功能不可用时，在描述顶部写 `Blocked by: #<n>, #<n>`。所有 blocker 都关闭后即可开工。
- 用 `glab api projects/:id/issues/<map>/links` 列出 map 的子项，保留带 `wayfinder:<type>` label 的 open issue。排除已有 assignee 或有 open blocker 的子项。后者可通过 `glab api projects/:id/issues/<child-iid>/links` 检查指向 open issue 的 `blocked_by` link，也可以检查 `Blocked by` 行。按 map 顺序取第一个。
- 领取用 `glab issue update <n> --assignee @me`，这是该会话的第一次写操作。
- 解决时先 `glab issue note <n> --message "<answer>"`，再 `glab issue close <n>`，最后给 map 的 Decisions-so-far 追加一条 context pointer，包含摘要和链接。

[英文原文](https://github.com/mattpocock/skills/blob/abeeaf9/skills/engineering/setup-matt-pocock-skills/issue-tracker-gitlab.md)
