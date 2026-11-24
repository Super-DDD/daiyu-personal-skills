# Matt Pocock 开发流程中文译稿

从 [ask-matt 的主流程](main-flow.md) 开始。它说明何时在当前会话直接使用 `implement`，何时经过 `to-spec`、`to-tickets`，以及何时用 `implement-spec` 统筹整项开发。会话切换的判断见 [PHASE-BOUNDARIES.md](phase-boundaries.md)。

## 主流程和依赖

| Skill | 做什么 | 何时读取其他文件 |
| --- | --- | --- |
| [setup-matt-pocock-skills](setup-matt-pocock-skills.md) | 首次在 repo 配置 issue tracker 和领域文档布局 | 按项目选择 [GitHub](setup-matt-pocock-skills/issue-tracker-github.md)、[GitLab](setup-matt-pocock-skills/issue-tracker-gitlab.md) 或 [Local Markdown](setup-matt-pocock-skills/issue-tracker-local.md) 模板。安装了 `triage` 时还使用 [triage-labels.md](setup-matt-pocock-skills/triage-labels.md)。领域文档的读取约定见 [domain.md](domain-docs.md)。 |
| [grill-with-docs](grill-with-docs.md) | 在项目里通过访谈澄清设计，并记录已确定的领域术语和重要决定 | 同时调用 [grilling](grilling.md) 和 [domain-modeling](domain-modeling.md)。前者驱动访谈，后者维护 glossary 和 ADR。没有工作目录时，上游入口为 [grill-me](grill-me.md)。 |
| [prototype](prototype.md) | 设计问题需要运行证据时做一个 prototype | 逻辑与状态读 [LOGIC.md](prototype/LOGIC.md)，界面外观读 [UI.md](prototype/UI.md)。需要把问题送到另一目录的会话、再把证据带回来时用 [handoff](handoff.md)。 |
| [to-spec](to-spec.md) | 把已经讨论的内容整理成 spec，确认测试的 seam，发布到项目的 issue tracker | 使用项目 glossary、相关 ADR、tracker 配置和正文中的 spec 模板。 |
| [to-tickets](to-tickets.md) | 把 spec 拆成能独立验证的 ticket，确认 blocking edge 后发布 | 按 tracker 选择正文中的本地文件或平台 issue 模板。 |
| [implement](implement.md) | 在当前会话完成一项工作，或在新会话完成一个 ticket | 在约定的 seam 上使用 [tdd](tdd.md)，完成后调用 [code-review](code-review.md)，再提交到当前 branch。 |
| [implement-spec](implement-spec.md) | 根据 ticket 的依赖关系并行推进整份 spec | 每个 implementer subagent 在独立 worktree 用 [tdd](tdd.md) 实现。全部合入 integration branch 后，统一调用 [code-review](code-review.md)。 |
| [code-review](code-review.md) | 以固定 Git 参照点审查 diff | Standards subagent 读取仓库的编码标准，Spec subagent 读取原始 issue 或 spec。两个维度分别报告。 |
| [pr](pr.md) | 撰写 PR 正文，展示改动、前后证据和合并风险 | 正文含模板和示例。视觉表达来自 `show-me`，已收录在正文，详见 [CREDITS.md](pr-credits.md)。 |
| [retro](retro.md) | 根据会话的一手资料，提出对 agent 工作环境的改进 | 读取原会话或日志，调用 [writing-for-agents](writing-for-agents.md)。编写 Skill 时继续读 [SKILL-MECHANICS.md](writing-for-agents/SKILL-MECHANICS.md)。 |

## 测试与领域建模

- [tdd](tdd.md) 定义 red → green 循环。判断一条测试是否在验证可观察行为时，读 [tests.md](tdd/tests.md)。决定哪些依赖适合 mock 时，读 [mocking.md](tdd/mocking.md)。
- [codebase-design](codebase-design.md) 给出 module、interface、seam 和 adapter 等词的精确定义。`tdd` 遇到 interface 形态或 seam 位置的问题时查阅它。按依赖类型深化 module 时读 [DEEPENING.md](codebase-design/DEEPENING.md)，比较多个 interface 设计时读 [DESIGN-IT-TWICE.md](codebase-design/DESIGN-IT-TWICE.md)。
- [domain-modeling](domain-modeling.md) 使用 [GLOSSARY-FORMAT.md](glossary-format.md) 和 [ADR-FORMAT.md](adr-format.md)。单一 domain context 用根目录的 `GLOSSARY.md`。有多个 domain context 时，根目录的 `GLOSSARY-MAP.md` 列出各 context 的位置和关系。可直接看上游的 Ordering、Billing、Fulfillment 示例。 [Glossary Map 示例](glossary-format.md)

译稿固定于 [mattpocock/skills · abeeaf9](https://github.com/mattpocock/skills/tree/abeeaf9)，保留原始 Skill 名称和调用方式。主流程页节译自 `ask-matt`。上游许可见 [仓库的第三方许可](../../README.md#第三方许可)。
