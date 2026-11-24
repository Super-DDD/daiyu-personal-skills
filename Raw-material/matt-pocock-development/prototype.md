---
name: prototype
description: "用可丢弃的 prototype 回答设计问题。用于验证 state model 或逻辑是否合理，或探索 UI 的外观。"
---

# prototype

Prototype 是用来回答一个问题的临时代码。问题决定它的形式。

## 选择形式

根据用户的请求、周围代码或用户的补充，确定要回答什么。

- 要判断逻辑或 state model 是否合理，读 [LOGIC.md](prototype/LOGIC.md)。做一个可分享的单文件 HTML，让非开发者通过自由操作按钮和分标签的引导演练，试验难以在纸面推演的状态。
- 要看 UI 应该是什么样，读 [UI.md](prototype/UI.md)。在同一条 route 做几种结构差异明显的 variant，通过 URL query parameter 和底部悬浮栏切换。

两个分支的产物差别很大。问题有歧义又联系不到用户时，选择较符合周围代码的分支，例如 backend module 选逻辑，页面或组件选 UI，并在 prototype 顶部说明假设。

## 两种形式的共同规则

1. 从一开始就标明它是 prototype。放在所探索的 module 或页面旁，起一个能看出用途的名字。临时 UI route 遵循项目已有路由约定。
2. UI prototype 通过项目 task runner 的一条命令启动，例如 `pnpm <name>`、`python <path>` 或 `bun <path>`。逻辑 demo 双击一个 HTML 文件即可运行。
3. 默认使用内存状态。需要验证持久化本身时，可以使用临时数据库，或名称明确标有「PROTOTYPE, wipe me」的本地文件。
4. 只做到能跑并回答问题。省略测试、多余的错误处理和抽象。
5. 每次执行操作或切换 UI variant 后，打印或显示全部相关状态，让用户看到发生了什么。
6. 得出结论后，把验证过的决定融入真实代码。将 prototype 提交到 main 之外的临时 branch，作为一手资料，在 implementation issue 中留下指向该 branch 的 context pointer。在 issue 或 commit 中写下问题和结论。Main 只保留验证过的决定。

[英文原文](https://github.com/mattpocock/skills/blob/abeeaf9/skills/engineering/prototype/SKILL.md)
