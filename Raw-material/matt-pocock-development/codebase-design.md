---
name: codebase-design
description: "设计 deep module 的共同词汇。在设计或改进 module 的 interface、寻找 deepening 机会、选择 seam、改善可测试性或方便 AI 查找代码时使用。其他 Skill 需要这些术语时也可调用。"
---

# codebase-design

设计 **deep module**。在干净的 seam 上用一个小 interface 提供大量行为，并从该 interface 测试。设计或重组代码时使用下面的词汇和原则，让调用方获得 leverage，维护者获得 locality，也让大家容易验证行为。

## Glossary

准确使用这些词。保持用词一致，不用 component、service、API、boundary 等近义词代替。

- **Module**。凡是有 interface 和 implementation 的东西都算，规模不限，包括函数、类、包、贯穿多层的 slice。避免 unit、component、service。
- **Interface**。调用方正确使用 module 必须知道的一切。除类型签名外，还包括不变量、顺序约束、错误模式、必需配置和性能特征。API、signature 只覆盖一部分含义。
- **Implementation**。module 内部的代码。Adapter 描述角色，implementation 描述内部构成。小 adapter 可以有很大的 implementation（Postgres repository），大 adapter 也可以有很小的 implementation（in-memory fake）。讨论 seam 时用 adapter，其余时候用 implementation。
- **Depth**。interface 能带来的 leverage，即调用方或测试每学习一单位 interface 能使用多少行为。小 interface 背后有大量行为时称 deep；interface 几乎与 implementation 一样复杂时称 shallow。
- **Seam**（Michael Feathers）。能在不编辑该处代码的情况下改变行为的位置，亦即 module 的 interface 所在之处。把 seam 放在哪里、背后放什么，是两个设计决定。避免用 boundary，它容易与 DDD 的 bounded context 混淆。
- **Adapter**。在 seam 上满足某个 interface 的具体对象。它说明这个对象承担什么角色。
- **Leverage**。调用方从 depth 中获得的收益。每学习一单位 interface 就能获得更多能力。同一 implementation 服务于 N 处调用和 M 个测试。
- **Locality**。维护者从 depth 中获得的收益。变更、bug、知识和验证集中在一处，一处修复就能覆盖所有调用方。

## Deep 与 shallow

Deep module 由小 interface 提供丰富实现。

```
┌─────────────────────┐
│   Small Interface   │  ← Few methods, simple params
├─────────────────────┤
│                     │
│  Deep Implementation│  ← Complex logic hidden
│                     │
└─────────────────────┘
```

应避免 interface 大、implementation 薄的 shallow module。

```
┌─────────────────────────────────┐
│       Large Interface           │  ← Many methods, complex params
├─────────────────────────────────┤
│  Thin Implementation            │  ← Just passes through
└─────────────────────────────────┘
```

设计时问自己，方法数量还能减少吗？参数还能简化吗？还有哪些复杂性可以藏在 module 里？

## 原则

- Depth 体现于 interface。deep module 内部可以由小而可 mock、可替换的部件组成。它既可以有供自身测试使用的私有 internal seam，也可以在 interface 处有 external seam。
- **Deletion test**。设想删掉这个 module。如果复杂性也随之消失，它只是转发层。如果复杂性重新分散到 N 个调用方，它就有保留的价值。
- Interface 就是 test surface。调用方和测试穿过同一个 seam。如果想绕过 interface 测内部，module 的形态可能需要调整。
- 一个 adapter 只能说明 seam 是假设的，两个 adapter 才表明它确有用途。出现真实变化时才引入 seam。

## 为可测试性设计

1. 接受传入的依赖。

   ```typescript
   // Testable
   function processOrder(order, paymentGateway) {}

   // Hard to test
   function processOrder(order) {
     const gateway = new StripeGateway();
   }
   ```

2. 返回结果，避免用副作用代替返回值。

   ```typescript
   // Testable
   function calculateDiscount(cart): Discount {}

   // Hard to test
   function applyDiscount(cart): void {
     cart.total -= discount;
   }
   ```

3. 保持小的 surface area。方法越少，需要的测试越少。参数越少，测试准备越简单。

## 术语之间的关系

每个 module 恰有一个面向调用方和测试的 interface。Depth 是 module 相对于其 interface 的属性。Seam 是 interface 所在的位置。Adapter 在 seam 处满足 interface。Depth 为调用方带来 leverage，为维护者带来 locality。

## 容易混淆的定义

用 implementation 与 interface 的代码行数比衡量 depth（Ousterhout 的表述）会奖励冗长的实现，这里采用 leverage。TypeScript 的 `interface` 关键字或类的公开方法也没有覆盖 interface 所包含的全部信息。Boundary 与 DDD 的 bounded context 容易混淆，请根据含义使用 seam 或 interface。

## 延伸阅读

按依赖类型深化一组 module 时读 [DEEPENING.md](codebase-design/DEEPENING.md)。需要并行探索多个 interface 方案、比较 depth、locality 和 seam 位置时读 [DESIGN-IT-TWICE.md](codebase-design/DESIGN-IT-TWICE.md)。

[英文原文](https://github.com/mattpocock/skills/blob/abeeaf9/skills/engineering/codebase-design/SKILL.md)
