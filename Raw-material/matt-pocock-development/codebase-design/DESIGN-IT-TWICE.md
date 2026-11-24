# Design It Twice

用户想为已选定的 deepening 对象探索其他 interface 时，可使用这个并行 subagent 流程。它来自 Ousterhout 的 Design It Twice，第一个想法未必最好。使用 [codebase-design](../codebase-design.md) 中的 module、interface、seam、adapter 和 leverage。

## 1. 说明要解决的问题

先给用户写出新 interface 必须满足的约束、会用到的依赖及其[分类](DEEPENING.md)，并用一段粗略的代码草图使约束具体化。它只说明问题。展示后立即进入第 2 步，让用户在 subagent 工作时阅读和思考。

## 2. 启动 subagent

并行启动至少三个 subagent，每个使用新的 context window，为 deep module 各自提出一个结构显著不同的 interface。给每个 subagent 单独的技术简报，说明文件路径、耦合细节、依赖类别，以及 seam 背后的内容。简报独立于给用户看的问题说明，为每个 agent 指定不同的设计约束。

- Agent 1 尽量缩小 interface，最多保留 1–3 个入口，使每个入口的 leverage 最大。
- Agent 2 优先灵活性，支持多种场景和扩展。
- Agent 3 优先最常见的调用方，让默认情况极其简单。
- 适用时，让 Agent 4 围绕跨 seam 的依赖设计 ports & adapters。

简报同时提供 [codebase-design](../codebase-design.md) 和项目 `GLOSSARY.md` 的词汇，使架构和领域用语一致。每个 subagent 交付五项内容。

1. Interface，包括类型、方法、参数、不变量、顺序和错误模式。
2. 调用方的用法示例。
3. Implementation 在 seam 后面隐藏了什么。
4. 依赖策略及 adapter，见 [DEEPENING.md](DEEPENING.md)。
5. 各项取舍，哪些地方的 leverage 高、哪些地方收益有限。

## 3. 展示和比较

逐个展示设计，给用户时间理解，再用文字比较 depth（interface 的 leverage）、locality（变更集中在哪里）及 seam 的位置。给出你自己的判断和理由。不同设计的部分如果适合组合，可提出混合方案。给出明确推荐。

[英文原文](https://github.com/mattpocock/skills/blob/abeeaf9/skills/engineering/codebase-design/DESIGN-IT-TWICE.md)
