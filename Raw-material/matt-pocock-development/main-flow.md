# 主流程：从想法到交付

译自 Matt Pocock 的 [`ask-matt/SKILL.md`](https://github.com/mattpocock/skills/blob/abeeaf9/skills/engineering/ask-matt/SKILL.md) 中的「The main flow: idea → ship」「Context hygiene」「Precondition」，以及「Vocabulary underneath」中的 `domain-modeling` 条目。

## 主流程

大多数工作都走这条路。你有个想法，希望把它做出来。

1. **[`/grill-with-docs`](grill-with-docs.md)** 通过访谈把想法问清楚。只要你**正在某个工作目录里开展工作**，就从这里开始。它会保存状态，把形成的认识留在 `GLOSSARY.md` 和 ADR 中。没有工作目录时，用 [`/grill-me`](grill-me.md)。两者都使用同一套 [`/grilling`](grilling.md) 访谈机制。有仓库可以留下记录时，`grill-with-docs` 更合适。
2. **分支：每个问题都能在对话里定下来吗？** 如果一个问题需要能运行的东西才能回答，例如状态、业务逻辑或必须亲眼看到的界面，就绕道做原型。来回都用 **[`/handoff`](handoff.md)** 衔接。原型在自己的目录里，这正是 `/handoff` 的用途，见[阶段边界](phase-boundaries.md)。
   - 先用 **`/handoff`** 输出交接文档，然后用这份文件开启新会话。
   - 用 **[`/prototype`](prototype.md)** 写可丢弃的代码来回答问题。
   - 再用 **`/handoff`** 把学到的东西带回来，并在原来的想法讨论中引用交接文档。
3. **分支：这项开发需要多个会话吗？**
   - **需要。** 先用 **[`/to-spec`](to-spec.md)** 把当前对话整理成 spec，再用 **[`/to-tickets`](to-tickets.md)** 拆成 tracer-bullet ticket，每张都声明阻塞它的前置 ticket。然后任选一种方式推进。
     - 逐张用 **[`/implement`](implement.md)**，两个 ticket 之间用 **`/clear`** 清空上下文。如果使用本地 issue tracker，每个 ticket 都是 `.scratch/<feature>/issues/` 下的独立文件，手动按依赖顺序做。如果使用实际的 issue tracker，就把依赖写成平台原生的阻塞关系，前置 ticket 都完成后即可领取。每个 ticket 都自包含，上一张的会话上下文可以丢掉。
     - 用 **[`/implement-spec`](implement-spec.md)** 一次实现整份 spec。它把 ticket 读成 task graph，让 implementer subagent 并行处理当前可开工的 ticket，最后把成果汇入一个 integration branch。想统筹整项开发时，可以选这条路。
   - **一个会话就够。** 直接在当前上下文里用 **`/implement`**。

   两条路都通过 **[`/tdd`](tdd.md)** 写代码，每次完成一个 red → green slice，最后用 **[`/code-review`](code-review.md)** 从 Standards 和 Spec 两个维度审查差异。`/implement` 每个 ticket 都跑这两项。`/implement-spec` 的各个 implementer subagent 分别做 `/tdd`，然后对 integration branch 统一做一次 `/code-review`。如果只想以测试先行的方式做出某个具体行为，可以单用 `/tdd`，无需完整 spec。只想以某个固定参照点审查分支或 PR 时，可以单用 `/code-review`。

   工作以 Pull Request 提交时，**[`/pr`](pr.md)** 负责组织正文。用能看清变更的最小示意图，附上证明改动有效的前后对照证据，并判断这次合并属于 one-way door 还是 two-way door。它由模型触发，agent 撰写 PR 时会调用它。
4. **[`/retro`](retro.md)** 收尾。开发完成后，尤其是过程出了岔子时，回顾会话并提出对 agent 工作环境的改进。改进对象包括导航指引、自动检查、`/code-review` 执行的编码标准、指导文件和工具，焦点在工作环境。机械性的错误交给确定性的检查，需要判断的问题写成编码标准。下一次开发就在改善过的环境里开始。

## 上下文管理

把第 1–3 步中从访谈到 `/to-tickets` 的部分保持在**同一段连续上下文**里。在拆完 ticket 前，先保留原会话，让访谈、spec 和 ticket 沿用同一套推理。之后每次 `/implement` 都从干净的上下文开始，根据 ticket 干活。`/retro` 应在要回顾的会话中、清空上下文前运行。如果已经清空，就让它读那次会话的日志。

这个做法的上限是 [smart zone](https://www.aihero.dev/ai-coding-dictionary/smart-zone)，即模型仍能清晰推理的上下文范围。原文给出的当时顶尖模型参考值约为 15 万 token。如果在拆完 ticket 前已接近这个范围，就在最近的阶段边界用 `/compact` 压缩后继续。见[阶段边界](phase-boundaries.md)。

## 底层的领域词汇

**[`/domain-modeling`](domain-modeling.md)** 用来把项目的 domain language 说准确。追问模糊术语，拆开承载多种含义的词，例如承担三个不同概念的「account」，并把难以逆转的决定记为 ADR。`/grill-with-docs` 在访谈中驱动这套工作，让 `GLOSSARY.md` 保持为一份清晰的术语表。

## 前置配置

第一次运行工程流程前，运行 **[`/setup-matt-pocock-skills`](setup-matt-pocock-skills.md)**，配置其他 Skill 所依赖的 issue tracker、triage label 和领域文档布局。也支持自定义 issue tracker。
