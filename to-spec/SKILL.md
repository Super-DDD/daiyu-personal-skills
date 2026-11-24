---
name: to-spec
description: "把当前对话整理成 spec，并发布到项目的 issue tracker。直接综合你们已经讨论过的内容。"
---

<!-- 译自 https://github.com/mattpocock/skills/blob/abeeaf9/skills/engineering/to-spec/SKILL.md -->

# to-spec

这个 Skill 根据当前对话和对代码库的理解产出一份 spec。只综合已经掌握的内容，跳过访谈。

你应当已经知道项目使用的 issue tracker 和 triage label 名称。如果没有，请让用户运行 [`$setup-matt-pocock-skills`](../setup-matt-pocock-skills/SKILL.md)。

## 流程

1. 如果还没有了解代码库，就先探索仓库，弄清目前的状态。整份 spec 都使用项目术语表中的领域用语，并遵循涉及范围内的 ADR。
2. 勾勒从哪些 seam 验证这个功能。优先用已有 seam，尽可能选层级最高的 seam。如果需要增加 seam，也应尽可能放在高层。跨代码库的 seam 越少越好，理想情况是只有一个。

   请用户确认这些 seam 符合他们的预期。
3. 用下面的模板写 spec，发布到项目的 issue tracker，并加上 `ready-for-agent` triage label。此时即可供 agent 领取，无需再 triage。

<spec-template>

## Problem Statement

从用户的角度，说明他们遇到的问题。

## Solution

从用户的角度，说明解决方案。

## User Stories

写一份篇幅充足的编号列表。每条用户故事使用以下格式。

1. 作为一名<角色>，我想要<功能>，以便<获得收益>。

<user-story-example>
1. 作为手机银行客户，我想查看各账户的余额，以便更好地决定如何消费。
</user-story-example>

用户故事列表应尽可能详尽，覆盖功能的所有方面。

## Implementation Decisions

列出已经做出的实现决策。可以包括以下内容。

- 将要创建或修改的模块
- 这些模块中将要修改的接口
- 开发人员给出的技术澄清
- 架构决策
- 数据模式变更
- API 契约
- 具体交互

省去具体的文件路径和代码片段，因为它们很快就可能过时。

有一种例外。如果原型中的片段能比文字更准确地表达某个决定，例如状态机、reducer、数据模式或类型结构，就把它放进对应的决策，并简要注明它来自原型。只保留承载决策的关键部分。

## Testing Decisions

列出已经做出的测试决策。包括以下内容。

- 好的测试是什么样的，只测试外部行为，避开实现细节
- 哪些模块需要测试
- 有哪些可借鉴的先例，例如代码库里的类似测试

## Out of Scope

说明哪些事情超出了本 spec 的范围。

## Further Notes

关于这项功能的其他说明。

</spec-template>
