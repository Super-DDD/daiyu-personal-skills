---
name: to-tickets
description: "把计划、spec 或当前对话拆成一组 tracer-bullet ticket。为每个 ticket 声明 blocking edge，并发布到已配置的 issue tracker。本地每个 ticket 一个文件，issue tracker 使用原生阻塞关系。"
disable-model-invocation: true
---

<!-- 译自 https://github.com/mattpocock/skills/blob/abeeaf9/skills/engineering/to-tickets/SKILL.md -->

# to-tickets

把计划、spec 或对话拆成一组**ticket**。每个 ticket 代表一个 tracer-bullet vertical slice，并列出**阻塞**它的前置 ticket。

你应当已经知道项目使用的 issue tracker 和 triage label 名称。如果没有，请让用户运行 [`/setup-matt-pocock-skills`](setup-matt-pocock-skills.md)。

## 流程

### 1. 收集上下文

基于当前对话中已有的信息开展。如果用户给了一个引用，例如 spec 路径、issue 编号或 URL，就获取完整正文以及所有评论并阅读。

### 2. 探索代码库（可选）

如果还没探索过代码库，就弄清它的当前状态。ticket 的标题和描述应使用项目术语表的 domain language，并遵循涉及范围内的 ADR。

留意有哪些地方可以先重构，让实现变得更容易。先让改动变得容易，再做那项容易的改动。

### 3. 起草 vertical slice

把工作拆成 tracer-bullet ticket。

<vertical-slice-rules>

- 每个切片都贯通一条窄而**完整**的路径，穿过各层（数据模式、API、UI、测试）。切片沿这条路径纵切，避免横向只切出单独一层。
- 每个切片做完后都能单独演示或验证。
- 每个切片的大小应能放进一个全新的 context window。
- 为实现做准备的重构应先完成。

</vertical-slice-rules>

为每个 ticket 列出**blocking edge**，也就是必须先完成的其他 ticket。没有 blocker 的 ticket 可以立即开工。

**大范围重构是 vertical slice 的例外。** 大范围重构指一项机械性的改动，例如重命名一列、修改一个共享符号的类型，影响范围遍及代码库。一次编辑就会破坏数千处调用，无法让任一 vertical slice 独立通过检查。此时按 **扩展–收缩（expand–contract）** 安排。先扩展，在旧形式旁加入新形式，确保现有调用照常运行。再按影响范围分批迁移调用点，例如按包或目录，每批建一个 ticket，依赖前面的 expand ticket。旧形式还在，CI 就能逐批保持通过。最后收缩，等所有调用点迁移完再删除旧形式，contract ticket 依赖所有 migration ticket。如果连分批迁移也无法各自保持检查通过，就保留这个顺序，在同一条 integration branch 完成各批，并让各批都成为最后一张「集成并验证」ticket 的前置项，只在最后这个 ticket 保证检查通过。

### 4. 请用户评议

把拟定的拆分方案列成编号列表。每个 ticket 展示以下内容。

- **标题**，简短且能说明这个 ticket做什么。
- **blocker**，开始前必须完成哪些 ticket（如有）。
- **交付结果**，这个 ticket会实现什么端到端行为。

询问用户以下问题。

- 粒度是否合适，会不会太粗或太细？
- 阻塞关系是否准确，每个 ticket 是否只依赖确实挡住它的 ticket？
- 哪些 ticket 需要合并或进一步拆分？

根据反馈迭代，直到用户认可拆分方案。

### 5. 发布到已配置的 issue tracker

发布通过确认的 ticket。发布方式由 `/setup-matt-pocock-skills` 配置的 issue tracker 决定。两种方式的 ticket 内容相同，blocking edge 的记录方式不同。

- **本地文件** → 每个 ticket 单独写到 `.scratch/<feature-slug>/issues/<NN>-<slug>.md`，从 `01` 开始按依赖顺序编号，前置 ticket 在前。每个文件的`Blocked by`列出依赖 ticket 的编号或标题。使用下方的单个 ticket 的模板，每个 ticket 一个文件。
- **issue tracker（GitHub、Linear 等）** → 按依赖顺序发布，一个 ticket 对应一个 issue，确保可以用实际的 issue 编号来引用 blocker。平台支持原生阻塞关系时就使用它，否则在每个 ticket 的`Blocked by`填上阻塞它的 issue。来源是已有 issue 时，把每个 ticket 建成它的子 issue，操作方式见 issue tracker 的配置文档。除非有其他指示，打上 `ready-for-agent` triage label。创建后的 ticket 即可由 agent 领取。

优先处理**frontier**，也就是所有 blocker 均已完成的 ticket。若依赖关系是一条直链，就从上到下处理。

保留父 issue 的现有内容和状态。

<local-ticket-template>

# <NN>：<ticket 标题>

**What to build:** 从用户角度描述这个 ticket会实现的端到端行为，避免按代码层列实现清单。

**Blocked by:** 列出阻塞它的 ticket 编号或标题，或填写`None (can start immediately)`。

**Status:** ready-for-agent

- [ ] 验收条件 1
- [ ] 验收条件 2

</local-ticket-template>

<issue-template>

## Parent

引用 issue tracker 里的父 issue。来源是已有 issue 时才保留本节。

## What to build

从用户角度描述这个 ticket会实现的端到端行为，避免按代码层列实现清单。

## Acceptance criteria

- [ ] 条件 1
- [ ] 条件 2

## Blocked by

逐个引用阻塞它的 ticket，或填写`None (can start immediately)`。若已用平台原生关系标注 blocker，省略本节。

</issue-template>

两种格式都省去具体的文件路径和代码片段，因为它们很快就会过时。有一种例外。如果原型中的片段能比文字更准确地表达某个决定，例如状态机、reducer、数据模式或类型结构，就把它放到对应的位置，简要注明它来自原型。只保留承载决策的关键部分。
