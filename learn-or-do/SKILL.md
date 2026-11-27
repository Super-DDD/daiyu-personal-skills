---
name: learn-or-do
description: "根据任务选择合适的 Skill，先告诉用户所选入口，再直接执行当前环节。"
disable-model-invocation: true
metadata:
  version: "0.23"
---

# Learn or Do

用户只需说自己在做什么、卡在哪里。只有 `$learn-or-do` 且上下文也没有目标时，问「你现在想弄清或做成什么？」。结合上下文选择本轮 Skill 并读取正文。开始工作前告诉用户「本轮使用 `$技能名`」并简述要处理的任务，随即按所选 Skill 执行，直接交付结果。沿用用户已给的材料、约束和授权。用户明确只想了解路线或获取提示词时，按其要求给出说明。

## 通用流程

常用顺序是 **Intent → Spec → Modeling → Implement**。已有背景、要求、模型或方案时，直接从需要的环节开始。用户已点名环节时，选用对应 Skill 并执行。

| Skill 入口 | 什么时候用 | 本步交付什么 |
| --- | --- | --- |
| [`$intent`](../intent/SKILL.md) | 想弄清为什么做、要解决什么问题 | 对背景、目的和问题的共同理解，以及待确认事项 |
| [`$spec`](../spec/SKILL.md) | 意图已明确，需要定范围和完成条件 | 目标、范围、约束、交付内容和可观察的完成条件 |
| [`$modeling`](../modeling/SKILL.md) | 想理解现有机制，或用具体场景推演设计 | 能解释关键关系和典型场景的模型，按需说明方案取舍 |
| [`$implement`](../implement/SKILL.md) | 要求或方案已明确，可以动手 | 本轮约定的实现、应用或练习结果，以及对照完成条件的验证 |

每次路由执行当前环节。交付本步结果后，由用户决定何时进入下一环节。任务已有约定的记录位置时，按[记录与接续](preferences.md#记录与接续)写回成果。已有方案时，可以直接用 `$implement` 实现、验证并交付。

## 如果要系统地理解或设计一个软件项目

进入下面三个环节前，先向用户说明通用语言的约定。梳理问题、用例和领域时，从使用者的说法和具体场景出发，在讨论中共同确定术语的含义；讨论和分析文档中不要使用代码片段或具体的类名、函数名、变量名、字段名。将已确认的词义记入当前项目的 `glossary.md`，有分歧的标为待确认，后续沟通沿用已确认的用语。进入概要设计或实现时，再根据这些概念讨论具体命名。执行所选环节时，沿用这项约定和已有词汇表。

沿着业务到架构走。先用 [`$software-problem-framing`](../software-problem-framing/SKILL.md) 弄清为谁解决什么问题，再用 [`$software-requirements`](../software-requirements/SKILL.md) 从用例梳理业务规则，通过 DDD 战略设计明确子域、候选限界上下文与上下文关系，最后用 [`$software-high-level-design`](../software-high-level-design/SKILL.md) 划分模块职责、说明协作方式。已有场景、约束和足以支撑当前架构判断的边界分析时，可以直接从概要设计开始。需要判断材料是否够用时，读[软件工程流程](references/software-engineering.md)。

## 随时可以单独用的工具

- 想核实 Spec 是否满足、概念是否连贯、结构带来哪些复杂性 → [`$verify`](../verify/SKILL.md)。
- 想亲自审查一批改动，走读生产代码并通过问询检验理解 → [`$human-pr`](../human-pr/SKILL.md)。
- 想直接看刚才改了什么、现在怎么用 → [`$show-me`](../show-me/SKILL.md)。
- 上一句太绕，想换成大白话 → [`$bro`](../bro/SKILL.md)。想用提问和新案例检验自己到底懂没懂 → [`$challenge-me`](../challenge-me/SKILL.md)。
- 想查当初为什么这样定、依据在哪 → [`$why`](../why/SKILL.md)。
- 准备换会话，要把进展和未决事项带过去 → [`$handoff`](../handoff/SKILL.md)。
- 想把个人经验做成 Skill → [`$thought-to-skill`](../thought-to-skill/SKILL.md)。想检查给 Agent 读的指令写得是否清楚 → [`$writing-for-agents`](../writing-for-agents/SKILL.md)。

## 贯穿各步的个人原则

按当前情况读取并应用。执行所选 Skill 时也沿用用户强调的偏好。

- 事实拿不准，先决定该查材料还是问人 → [查证与提问](preferences.md#查证与提问)。
- 用户想先看全貌或用例子理解 → [理解与表达](preferences.md#理解与表达)。
- 遇到取舍，要决定谁拍板 → [决策与自主推进](preferences.md#决策与自主推进)。
- 要判断实际成果是否达标 → [实施与证据](preferences.md#实施与证据)。
- 需要把结论写回约定位置、留给下次继续 → [记录与接续](preferences.md#记录与接续)。
