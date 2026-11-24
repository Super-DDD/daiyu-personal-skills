# 术语表和术语索引的格式

译自 [`domain-modeling/GLOSSARY-FORMAT.md`](https://github.com/mattpocock/skills/blob/abeeaf9/skills/engineering/domain-modeling/GLOSSARY-FORMAT.md)。

## `GLOSSARY.md` 的结构

```md
# {Context Name}

{One or two sentence description of what this context is and why it exists.}

## Language

**Order**:
{A one or two sentence description of the term}
_Avoid_: Purchase, transaction

**Invoice**:
A request for payment sent to a customer after delivery.
_Avoid_: Bill, payment request

**Customer**:
A person or organization that places orders.
_Avoid_: Client, buyer, account
```

## 规则

- **明确选用哪个词。** 同一个概念有多个叫法时，选最合适的一个，把其余叫法列在`_Avoid_`下。
- **把定义写紧凑。** 最多一两句，说明这个概念是什么，省去它如何运作的描述。
- **只收录项目当前上下文特有的术语。** 通用编程概念（超时、错误类型、工具函数模式等）即使使用频繁，也不放进这里。添加前先判断，这个概念是否为当前上下文所特有。
- **有自然分组时用小标题。** 所有术语都属于同一个紧密相关的领域时，平铺列表即可。

## 单上下文和多上下文仓库

**Single-context repo（大多数仓库）**在根目录放一份 `GLOSSARY.md`。

**Multi-context repo**在根目录放一份 `GLOSSARY-MAP.md`，列出各个上下文、它们的位置及其相互关系。

```md
# Glossary Map

## Contexts

- [Ordering](./src/ordering/GLOSSARY.md): receives and tracks customer orders
- [Billing](./src/billing/GLOSSARY.md): generates invoices and processes payments
- [Fulfillment](./src/fulfillment/GLOSSARY.md): manages warehouse picking and shipping

## Relationships

- **Ordering → Fulfillment**: Ordering emits `OrderPlaced` events; Fulfillment consumes them to start picking
- **Fulfillment → Billing**: Fulfillment emits `ShipmentDispatched` events; Billing consumes them to generate invoices
- **Ordering ↔ Billing**: Shared types for `CustomerId` and `Money`
```

Skill 自行判断适用的结构。

- 有 `GLOSSARY-MAP.md` 时，读取它以定位各个上下文。
- 只有根目录的 `GLOSSARY.md` 时，按单上下文处理。
- 两者都没有时，等第一个术语确定后再创建根目录下的 `GLOSSARY.md`。

存在多个上下文时，推断当前讨论涉及哪一个。如果判断不了，就询问用户。
