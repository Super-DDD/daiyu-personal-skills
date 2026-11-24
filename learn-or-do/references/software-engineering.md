# 软件工程理解与概要设计工作流

用于系统地理解已有软件，或从一个问题推进到概要设计。通常按 **明确问题与价值 → 梳理需求与业务 → 建立概要设计** 的顺序进行。

| 手动入口 | 带入材料 | 本环节结束时交付 |
| --- | --- | --- |
| [`$software-problem-framing`](../../software-problem-framing/SKILL.md) | 初步想法、业务资料、访谈或现状问题 | 业务背景、涉众诉求、目标优先级、范围和成功标准 |
| [`$software-requirements`](../../software-requirements/SKILL.md) | 前一步的问题说明，或等效的业务材料 | 代表用例、关键规则、状态和事实归属、质量要求与验收条件 |
| [`$software-high-level-design`](../../software-high-level-design/SKILL.md) | 需求与业务分析结果，或等效的场景、规则和约束 | 对现状架构的解释，或概要设计及其边界、协作方式和关键取舍 |

每次手动调用一个组件。交付当前环节的结果后结束；由用户决定何时进入下一环节。已有足够的输入时，可以直接从对应环节开始。发现会改变前一步结论的新证据时，记录具体影响和待确认事项，由用户决定调整哪些材料。任务已有约定的记录位置时，按[记录与接续](../preferences.md#记录与接续)同步结果。

整个流程的产出应能讲清业务为什么需要这个系统、主要场景与规则怎样运作，以及架构如何承接这些要求。

## 选择入口

- 已有明确需求，只需写成规格时，用 [`$spec`](../../spec/SKILL.md)。需要调查多个角色、用例和业务规则时，用 [`$software-requirements`](../../software-requirements/SKILL.md)。
- 想看懂一个局部机制时，用 [`$modeling`](../../modeling/SKILL.md)。需要把业务边界、系统协作和质量要求连起来时，用 [`$software-high-level-design`](../../software-high-level-design/SKILL.md)。
- 需要追溯某条业务规则、架构选择或阈值当初为何确定时，可以单独调用 [`$why`](../../why/SKILL.md)，将历史依据带回需求或设计说明。

## 工程判断原则

- 从整体业务目标、涉众、系统边界和代表场景建立轮廓，再深入与当前目标有关的局部机制。
- 区分业务要求、已观察到的行为、已有决定、假设和候选设计。用业务材料和实际证据分别说明它们。
- 优先查证会改变当前判断的未知项。证据不足时，注明结论适用的范围及依赖的假设。

## 参考依据

以下资料供需要核对方法来源时查阅。

- [Jacobson 与 Cockburn：用例的目标、参与者与场景](https://www.ivarjacobson.com/publications/use-case-foundation)
- [Evans：DDD 的语言、模型边界与设计模式](https://www.domainlanguage.com/wp-content/uploads/2016/05/DDD_Reference_2015-03.pdf)
- [Cucumber：用规则、例子和问题形成共同理解](https://cucumber.io/blog/bdd/example-mapping-introduction/)
- [arc42：架构方案的关键决策与质量目标](https://docs.arc42.org/section-4/)
- [SEI：架构中的整体结构、行为和质量要求](https://www.sei.cmu.edu/software-architecture/)
- [Ousterhout：模块深度、信息隐藏与接口设计](https://web.stanford.edu/~ouster/cgi-bin/cs190-winter18/lecture.php?topic=modularDesign)
