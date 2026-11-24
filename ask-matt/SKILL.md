---
name: ask-matt
description: "根据已有想法、spec、ticket 或 diff，为标准开发流程选择下一步。"
---

# ask-matt

从用户当前想交付的结果选一步。先读对应的 Skill，再给出一句选择理由和一条带上已有材料的手动命令。用户问整条路线时，再展开各步。

| 当前情况 | 下一步 |
| --- | --- |
| 第一次为这个 repo 配置 issue tracker 和领域文档，或某一步需要的项目配置确实缺失 | [`$setup-matt-pocock-skills`](../setup-matt-pocock-skills/SKILL.md)，带上当前 repo 和已有的 tracker 约定。 |
| 项目里有一个功能想法，行为、领域术语或设计取舍还没说清 | [`$grill-with-docs`](../grill-with-docs/SKILL.md)，带上想法和相关代码。只想整理领域词汇或 Glossary Map 时可直接用 [`$domain-modeling`](../domain-modeling/SKILL.md)。 |
| 有个决定必须跑一下逻辑或看见 UI 才能定 | [`$prototype`](../prototype/SKILL.md)，写清要回答的问题。需要到另一个目录做实验时，用 [`$handoff`](../handoff/SKILL.md) 带出问题、带回证据。 |
| 已讨论清楚，要把共识写成供后续开发的 spec | [`$to-spec`](../to-spec/SKILL.md)，沿用当前讨论和已经做出的决定。 |
| 已有工程 spec 或计划，想拆成能独立交付的 ticket | [`$to-tickets`](../to-tickets/SKILL.md)，带上 spec 文件、issue 或当前讨论。 |
| 需求已明确，一轮能做完，或者只想实现一个已就绪的 ticket | 使用下方的 `$ask-matt implement <任务或 ticket>`。 |
| 已有 spec 及配套 ticket，想统筹实现整份 spec | [`$implement-spec`](../implement-spec/SKILL.md)，带上 spec 和 ticket 的位置。 |
| 只想以测试驱动一个具体行为 | [`$tdd`](../tdd/SKILL.md)，带上行为和可观察它的 interface。需要设计 seam 时读 [codebase-design](../codebase-design/SKILL.md)。 |
| 已有代码改动，想审查与 spec 和仓库规范的符合度 | [`$code-review`](../code-review/SKILL.md)，带上 diff 的比较基线或工作树范围，以及对应 spec。 |
| 需要写 PR 正文 | [`$pr`](../pr/SKILL.md)，带上改动与验证证据。 |
| 想从一次开发经历中找出可改进的工具、检查或工作约定 | [`$retro`](../retro/SKILL.md)，使用当前会话或指定日志。 |

访谈、写 spec 和拆 ticket 尽量沿用同一会话。逐个实现 ticket 时，每个 ticket 可以在新会话开始。要决定继续、清空、交接或压缩上下文时，读 [PHASE-BOUNDARIES.md](PHASE-BOUNDARIES.md)。

## 本机的单项 implement

Matt 的工程 `implement` 和一般任务的 `$implement` 同名。在本机，用户显式输入 `$ask-matt implement <任务或 ticket>` 时，读取 [Matt 的 implement](implement.md)，执行指定的一项工程实现。仅咨询路线时，按上表推荐命令。

流程依据 [Matt Pocock 的 ask-matt](https://github.com/mattpocock/skills/blob/abeeaf9/skills/engineering/ask-matt/SKILL.md)。
