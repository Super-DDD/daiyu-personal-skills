---
name: handoff
description: "将当前对话压缩成一份交接文档，供另一个 Agent 接手。"
argument-hint: "下一个会话将用来做什么？"
disable-model-invocation: true
---

写一份交接文档，概括当前对话，让一个新的 Agent 能继续这项工作。将文档保存到用户操作系统的临时目录。优先使用 `$TMPDIR`，否则使用 `/tmp`。Windows 上使用 `%TEMP%`。不要保存到当前工作区。

在文档中加入「建议使用的 Skill」一节，列出下一个 Agent 应通过 Skill 工具调用哪些 Skill。

对于已有产物中记录的内容，如规格、计划、架构决策记录（ADR）、issue、提交和差异（diff），通过路径或 URL 引用，避免重复抄写。

隐去敏感信息，例如 API 密钥、密码或可识别个人身份的信息。

如果用户传入了参数，将其视为下一个会话的工作重点，并据此调整交接文档的内容。
