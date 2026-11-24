# Local Markdown issue tracker

项目的 issue 和 spec 以 Markdown 文件保存在 `.scratch/`。

## 约定

- 每项 feature 使用一个目录 `.scratch/<feature-slug>/`。
- Spec 保存在 `.scratch/<feature-slug>/spec.md`。
- Implementation issue 每个 ticket 单独一个文件，放在 `.scratch/<feature-slug>/issues/<NN>-<slug>.md`，从 `01` 开始编号。
- 每个 issue 文件靠近顶部用 `Status:` 行记录 triage 状态。角色字符串见 [triage-labels.md](triage-labels.md)。
- 评论和对话历史追加到文件底部的 `## Comments` 标题下。

## Skill 指令

“publish to the issue tracker” 表示在 `.scratch/<feature-slug>/` 下创建文件，按需建立目录。“fetch the relevant ticket” 表示读取指定路径的文件，用户通常会给出路径或 issue 编号。

## Wayfinding 操作

`/wayfinder` 使用一个文件作为 map，每个 child ticket 占一个文件。

- Map 放在 `.scratch/<effort>/map.md`，含 Notes / Decisions-so-far / Fog。
- Child ticket 放在 `.scratch/<effort>/issues/NN-<slug>.md`，从 `01` 开始编号，正文写问题。`Type:` 行记录类型（`research` / `prototype` / `grilling` / `task`），`Status:` 行记录 `claimed` 或 `resolved`。
- Blocking 在顶部附近用 `Blocked by: NN, NN` 声明。它列出的文件状态全为 `resolved` 后即可开工。
- 查询 frontier 时扫描 `.scratch/<effort>/issues/` 下尚未关闭、没有阻塞且未被领取的文件，取编号最小的。
- 领取时在动工前设置 `Status: claimed` 并保存。
- 解决时在 `## Answer` 标题下追加答案，设置 `Status: resolved`，并给 `map.md` 的 Decisions-so-far 追加一条含摘要和链接的 context pointer。

[英文原文](https://github.com/mattpocock/skills/blob/abeeaf9/skills/engineering/setup-matt-pocock-skills/issue-tracker-local.md)
