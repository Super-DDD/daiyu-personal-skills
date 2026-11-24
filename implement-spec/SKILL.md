---
name: implement-spec
description: "把 /to-spec 和 /to-tickets 的产出实现为代码。"
---

<!-- 译自 https://github.com/mattpocock/skills/blob/abeeaf9/skills/engineering/implement-spec/SKILL.md -->

# implement-spec

你已经收到一份 spec。它还应当附有描述如何实现这份 spec 的 ticket。

你应当已经知道项目使用的 issue tracker。如果没有，请让用户运行 [`$setup-matt-pocock-skills`](../setup-matt-pocock-skills/SKILL.md)。

目标是在单条**integration branch**上实现整份 spec，并按 issue tracker 的结项方式解决每个 ticket。

把 ticket 读成一张带 blocking edge 的**task graph**。在推进过程中，总有一批当前已经可以领取的 ticket（frontier）。

为每个 subagent 使用一个全新的 context window。

给 subagent 的任务中附上所用 Skill 文件的可访问路径，例如 [tdd](../tdd/SKILL.md)，让新 context window 能读取对应步骤。减少与 subagent 之间的通信。主要用**context pointer**来沟通，指向 spec、ticket、调研记录和此前的提交。已有引用可获取的信息直接引用，避免重复。

尽量让**implementer subagent**在后台运行，以提高并行度。

## 步骤

1. 阅读 spec 和 ticket，理解 task graph。
2. （可选）将 ticket 需要的探索交给**exploration subagent**，例如调查代码库相关文件或外部文档。确保它能保存文件。它应把 Markdown 笔记保存到仓库之外、之后各 subagent 都能访问的目录，方便**implementer subagent**专注于实现。
3. 创建 integration branch。如果 issue tracker 通过 PR 结项，或者用户请求创建 PR，在第 5 步第一次合并后创建 draft PR。一条还没有比主分支多出任何提交的分支无法开 PR。撰写 PR 正文时读取 [pr](../pr/SKILL.md)。在 draft PR 中注明它将关闭这份 spec 及其 ticket。
4. 每个 ticket 分配一个全新的**implementer subagent**，各用独立的 worktree 和分支。每个 implementer subagent 需要完成以下事项。
   - 开始前确认其 worktree 基于 integration branch。如果基线不对，就重置到 integration branch。
   - 读取并应用 [`tdd`](../tdd/SKILL.md) 实现这个 ticket。
   - 报告完成前，把 integration branch 的最新提交合入自己的分支。
5. **implementer subagent**做完后，安排一个**merger subagent**把它的结果合入 integration branch。
6. 如果有 ticket 因此解除阻塞，就启动更多**implementer subagent**来处理它们，以提高并行度。
7. 所有 ticket 都完成后，在 integration branch 上按 [`code-review`](../code-review/SKILL.md) 审查。再用一个全新的**implementer subagent**修正审查提出的全部问题。
8. 如果有 draft PR，把它标记为可供审查。否则，按 issue tracker 的结项方式解决每个 ticket，并报告 integration branch。
9. 清理所有**implementer subagent**的 worktree。
