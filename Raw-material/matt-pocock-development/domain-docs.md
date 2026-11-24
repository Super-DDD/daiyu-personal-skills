# 领域文档

译自 [`setup-matt-pocock-skills/domain.md`](https://github.com/mattpocock/skills/blob/abeeaf9/skills/engineering/setup-matt-pocock-skills/domain.md)。

这份文件说明工程 Skill 在探索代码库时如何使用本仓库的领域文档。

## 探索前阅读

- 仓库根目录有 **`GLOSSARY-MAP.md`** 时，先读它。它为每个上下文指向一份 `GLOSSARY.md`。读取与当前议题相关的每份术语表。
- 其余情况读仓库根目录的 **`GLOSSARY.md`**。
- 阅读 **`docs/adr/`** 里与即将开展的工作相关的 ADR。多上下文仓库还要检查 `src/<context>/docs/adr/`，查找适用于该上下文的决定。

如果其中某些文件不存在，直接继续即可。等实际确定了术语或决定，再由 `/domain-modeling` 按需创建这些文件。可通过 `/grill-with-docs` 或 `/improve-codebase-architecture` 调用 `/domain-modeling`。

## 文件结构

单上下文仓库（大多数仓库）。

```text
/
├── GLOSSARY.md
├── docs/adr/
│   ├── 0001-event-sourced-orders.md
│   └── 0002-postgres-for-write-model.md
└── src/
```

多上下文仓库（根目录有 `GLOSSARY-MAP.md`）。

```text
/
├── GLOSSARY-MAP.md
├── docs/adr/                          ← 系统级决策
└── src/
    ├── ordering/
    │   ├── GLOSSARY.md
    │   └── docs/adr/                  ← 上下文内的决策
    └── billing/
        ├── GLOSSARY.md
        └── docs/adr/
```

## 使用术语表中的词汇

产出中提到领域概念时，例如 issue 标题、重构提案、假设或测试名称，使用 `GLOSSARY.md` 里定义的术语，避开术语表已明确弃用的同义词。

如果需要的概念尚未收录，应先复查你是否在引入项目里没有的说法。如果确有缺口，就记下来，供 `/domain-modeling` 处理。

## 指出与 ADR 的冲突

如果你的产出与已有 ADR 矛盾，要明确指出这个冲突。

> _与 ADR-0007（订单采用事件溯源）冲突，但值得重新讨论，因为……_
