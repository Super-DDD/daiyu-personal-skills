---
name: domain-modeling
description: "建立并完善项目的领域模型。讨论代码库的术语、编写或编辑 GLOSSARY.md，或者记录或修改架构决策记录时使用。"
---

<!-- 译自 https://github.com/mattpocock/skills/blob/abeeaf9/skills/engineering/domain-modeling/SKILL.md -->

# domain-modeling

在设计过程中主动建立并完善项目的领域模型。追问术语、构造边缘场景，并在术语和决定一明确时就记下来。仅想获取词汇时，随手读取 `GLOSSARY.md` 即可。需要调整模型本身时使用这个 Skill。

## 文件结构

大多数仓库只有一个上下文。

```text
/
├── GLOSSARY.md
├── docs/
│   └── adr/
│       ├── 0001-event-sourced-orders.md
│       └── 0002-postgres-for-write-model.md
└── src/
```

如果仓库根目录有 `GLOSSARY-MAP.md`，仓库就有多个上下文。索引指向每个上下文的位置。

```text
/
├── GLOSSARY-MAP.md
├── docs/
│   └── adr/                         ← 系统级决策
├── src/
│   └── ordering/
│       ├── GLOSSARY.md
│       └── docs/adr/                 ← 上下文内的决策
│   └── billing/
│       ├── GLOSSARY.md
│       └── docs/adr/
```

有内容要写时再创建文件。术语表还不存在时，等第一个术语确定后创建。还没有 `docs/adr/` 时，等需要写第一份 ADR 再创建。

## 会话进行中

### 对照术语表追问

用户使用的术语和 `GLOSSARY.md` 里的用语有冲突时，立即指出。例如「术语表把 cancellation 定义为 X，但你说的似乎是 Y。你指的是哪一个？」

### 把含糊的说法变准确

用户用了含义模糊或一词多义的术语时，提出一个准确的标准用词。例如「你说的 account 指 Customer 还是 User？这是两个不同的概念。」

### 讨论具体场景

讨论领域中的关系时，用具体场景检验它们。构造触及边缘情况的场景，促使用户准确说出概念之间的边界。

### 和代码对照

用户描述某个机制时，检查代码里的行为是否一致。如果发现矛盾，就明确指出。例如「代码会取消整份订单，你刚才说允许部分取消。哪一个是对的？」

### 在讨论中更新 `GLOSSARY.md`

术语一确定，就立即更新 `GLOSSARY.md`，不用留到最后一起记。使用[术语表格式](GLOSSARY-FORMAT.md)。

`GLOSSARY.md` 只保留术语和定义。其中省去所有实现细节、spec、临时笔记和实现决定。

### 有必要时才提议写 ADR

只有同时满足以下三项，才提议创建 ADR。

1. **难以逆转。** 以后改主意的成本不可忽略。
2. **缺少背景就会让人意外。** 未来的读者会想「当时为什么这么做？」
3. **经过了实在的取舍。** 当时确实有多个选项，你为某些明确的原因选了其中一个。

缺少任何一项都跳过。使用 [ADR 格式](ADR-FORMAT.md)。
