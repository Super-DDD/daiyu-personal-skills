---
name: tdd
description: "Test-driven development。用户要用 test-first 的方式开发功能或修复 bug，提到 red-green-refactor，或想编写 integration test 时使用。"
---

# tdd

TDD 的循环是 **red → green**。这份参考说明怎样写值得保留的测试、在哪里写、哪些是 anti-pattern，以及每轮的规则。每一节都适用于每一轮，在进入循环前及循环中查阅。

探索代码库时，若有 `GLOSSARY.md` 就先读它，让测试名和 interface 的词汇与项目的 domain language 一致，并遵循相关 ADR。

## 好测试验证什么

通过 public interface 验证行为。即使实现被完全重写，测试也应继续成立。好的测试读起来像 spec，例如「用户可以用有效的购物车结账」明确说出了系统具备的能力。对内部结构没有依赖，这条测试就经得起重构。参见 [tests.md](tdd/tests.md) 和 [mocking.md](tdd/mocking.md)。

## 在 seam 上写测试

**seam** 是观察公开行为的边界。在这个 interface 上就能验证行为，无须探入内部。只在预先约定的 seam 上写测试。动笔前先列出要测的 seam，和用户确认。测试资源有限，把它用在关键路径和复杂逻辑上。

询问用户「public interface 是什么？我们应在哪些 seam 上测试？」每个候选 seam 用一行说明它能查出什么、会漏掉什么。

如果 interface 的形态也有待确定，例如 module 的 depth、seam 的位置、interface 要公开什么，调用 [codebase-design](codebase-design.md) 查阅词汇。它是 module、interface、depth、seam、adapter、leverage、locality 的共同参考。

## Anti-patterns

- **Implementation-coupled**。mock 内部协作者、测试 private method，或绕过 interface 直接查数据库。典型征兆是行为没变，一重构测试就坏。
- **Tautological**。断言按实现的算法重新计算期望值，如 `expect(add(a, b)).toBe(a + b)`、用同一做法手工算出的 snapshot 或让一个常量和自己比较。这样的测试必然通过。期望值要有独立来源，例如已知正确的字面值、算例或 spec。
- **Horizontal slicing**。先批量写完测试，再写全部实现，验证的只是设想出来的行为。还没理解实现就锁定了测试结构，真正的行为发生变化时测试也可能察觉不到。采用 **vertical slice**，每次一条测试、一次实现。每条测试都是一枚 **tracer bullet**，下一步由上一轮学到的东西决定。

## 循环规则

- **Red before green**。先写一条会失败的测试，然后只写足够让它通过的代码。等下一条测试提出要求时再增加能力。
- **一次一个 slice**。每轮一个 seam、一条测试、一份最小实现。
- **Refactoring** 放到 review 阶段处理，见 [code-review](code-review.md)。red → green 的实现循环到 green 为止。

[英文原文](https://github.com/mattpocock/skills/blob/abeeaf9/skills/engineering/tdd/SKILL.md)
