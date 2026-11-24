# Skill 的调用与拆分

这是[writing-for-agents](../writing-for-agents.md)中专门针对 Skill 的部分，涉及文件头元数据、调用方式和 router skill。其他写作方法见主文件。

## 调用方式

有两种选择，取舍取决于 context load 和人的 cognitive load。

- **model-invoked。** 保留 `description`，agent 就可以自行选择这个 Skill，其他 Skill 也能调用它。人仍然可以直接输入名称。描述是这个 Skill 的顶层 context pointer，常驻上下文，以持续占用空间来换取可发现性。如果正文全是参考资料，一个可由模型调用的 Skill 也可以成为多个 Skill 共用资料的出处。配置时省略 `disable-model-invocation`，在面向模型的描述里写明各个触发分支，沿用主文件中编写指针的规则。
- **user-invoked。** 设置 `disable-model-invocation: true`，描述就不再作为模型的自动选择依据，由用户输入名称调用。其他 Skill 也不能直接触发它。这样节省了常驻上下文，记住它存在的责任转移给人。此时 `description` 面向人，一句话说明用途即可，可以删去触发词列表。

当 agent 需要自行选择这个 Skill，或另一个 Skill 需要调用它时，选择 model-invoked。若每次都由人点名，选择用户调用即可。

两个手动 Skill 如果都需要同一份参考资料，把资料放到 Skill 调用机制以外的普通文件里，让两边都能直接引用。

## 按调用方式拆分

如果某个 leading word 值得独立触发一种能力，而且确实是你在提示词中会用的词，或者另一个 Skill 必须能调用这项能力，可以把它拆成单独的模型调用 Skill。新增的描述会一直占用上下文，独立调用带来的收益要值得这份开销。

## router skill

当手动 Skill 多到记不住时，可以增加一个手动调用的 router skill。它列出其他 Skill 的名字，说明什么时候该用哪个。这样人只需记住一个入口。对于只能手动调用的 Skill，路由入口给出选择建议，具体调用由用户发起。

[英文原文](https://github.com/mattpocock/skills/blob/abeeaf9/skills/productivity/writing-for-agents/SKILL-MECHANICS.md)
