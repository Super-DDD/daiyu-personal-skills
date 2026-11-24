---
name: setup-matt-pocock-skills
description: "为当前 repo 配置工程 Skill 使用的 issue tracker、triage labels 和 domain docs 布局。首次使用其他工程 Skill 前运行一次。"
disable-model-invocation: true
---

# setup-matt-pocock-skills

工程 Skill 依赖三项项目配置：issue tracker（默认 GitHub，也支持本地 Markdown）、五个标准 triage 角色的 label，以及 `GLOSSARY.md` 和 ADR 的位置与读取规则。这个 Skill 由 prompt 驱动。先调查、展示发现、请用户确认，然后写入。

## 1. 调查 repo

- 用 `git remote -v` 和 `.git/config` 确定是否有 GitHub remote、指向哪个 repo。
- 查看根目录是否有 `AGENTS.md`、`CLAUDE.md`，以及其中是否已有 `## Agent skills`。
- 查找根目录的 `GLOSSARY.md`、`GLOSSARY-MAP.md` 和旧名 `CONTEXT.md`、`CONTEXT-MAP.md`。找到旧文件时，提议用 `git mv` 改成相应的 GLOSSARY 名称，并更新 `docs/agents/domain.md` 中的引用。
- 查看 `docs/adr/`、`src/*/docs/adr/`、`docs/agents/` 中已有的文件。`.scratch/` 提示项目可能已经使用本地 Markdown tracker。
- 查找同级的 `triage` Skill 目录或可用 Skill 中的同名条目。这决定是否执行 B 部分。
- 查找 monorepo 的迹象，例如 `pnpm-workspace.yaml`、`package.json` 的 `workspaces` 或含有各自 `src/` 的 `packages/*`。上游的默认判断是没有这些信号就采用 single-context，这适合绝大多数 repo。

## 2. 展示调查结果并询问

说明已有和缺失的配置，然后按 A、B、C 顺序进行，每节得到回答再进入下一节。先给出推荐答案，用户一个词即可确认。只有存在选择时才用一句话解释。已经能从调查确定的事项直接跳过。

**A. Issue tracker**。告诉用户它是项目追踪工作的地方。`to-tickets`、`triage` 和 `to-spec` 等会按这里的配置调用 `gh issue create`、写入 `.scratch/` 或遵循自定义流程。采用项目实际在用的方式。

- Remote 指向 GitHub 时推荐 GitHub Issues 和 `gh`。指向 GitLab.com 或自托管 GitLab 时推荐 GitLab Issues 和 [glab](https://gitlab.com/gitlab-org/cli)。
- 也可选择本地 Markdown，将 issue 写到 `.scratch/<feature>/`，适合个人项目或没有 remote 的 repo。
- 其他 tracker（如 Jira、Linear），请用户用一段话描述做法，按其说明记录。

将选择写入 `docs/agents/issue-tracker.md`。GitHub 和 GitLab 模板中将外部 PR/MR 当作功能请求的选项默认关闭，维持默认即可，需要时用户可在文件中开启。若选择了 GitHub 或 GitLab，但还没有指向对应主机的 remote，请用户给 repo URL。必要时用 `git init` 和 `git remote add origin <url>` 接好。以当前目录内运行 `gh repo view` 或 `glab repo view` 能显示目标项目为准。无法访问时说明缺少什么。

**B. Triage labels**。仅安装了 `triage` 时询问一个问题：「保留默认 triage labels 吗？推荐保留。」默认值是 `needs-triage`、`needs-info`、`ready-for-agent`、`ready-for-human`、`wontfix`。用户认可就按原值写入。如果用户已有其他名称，如以 `bug:triage` 表示 `needs-triage`，收集对应关系以便复用已有 label。

**C. Domain docs**。默认采用 single-context，即根目录的 `GLOSSARY.md` 和 `docs/adr/`，可以直接选用。只有发现 monorepo 信号才给出 multi-context 选项，即根目录的 `GLOSSARY-MAP.md` 指向各 context 的 `GLOSSARY.md`，此时请用户确认布局。

## 3. 展示可编辑的草稿

展示要写入 `CLAUDE.md` 或 `AGENTS.md` 的 `## Agent skills` 区块和各配置文件正文，文件分别为 `docs/agents/issue-tracker.md`、`docs/agents/domain.md`，以及安装了 `triage` 时的 `docs/agents/triage-labels.md`。先让用户修改，再落盘。

## 4. 写入

优先编辑已有的 `CLAUDE.md`，其次是已有的 `AGENTS.md`。如果两者都没有，请用户选择要创建哪一个。已有 `## Agent skills` 就在原位置更新，保留周围的用户内容。使用下列区块。

```markdown
## Agent skills

### Issue tracker

[one-line summary of where issues are tracked]. See `docs/agents/issue-tracker.md`.

### Triage labels

[one-line summary of the label vocabulary]. See `docs/agents/triage-labels.md`.

### Domain docs

[one-line summary of layout: "single-context" or "multi-context"]. See `docs/agents/domain.md`.
```

仅当安装了 `triage` 并完成 B 时才包含 `### Triage labels` 和相应文件。若 tracker 是 GitHub 或 GitLab，还需用 `gh label create` 或 `glab label create` 创建配置中缺少的 label。

根据选择，从相应模板生成文档：

- [issue-tracker-github.md](setup-matt-pocock-skills/issue-tracker-github.md)
- [issue-tracker-gitlab.md](setup-matt-pocock-skills/issue-tracker-gitlab.md)
- [issue-tracker-local.md](setup-matt-pocock-skills/issue-tracker-local.md)
- [triage-labels.md](setup-matt-pocock-skills/triage-labels.md)
- [domain.md](domain-docs.md)

其他 tracker 按用户描述编写 `docs/agents/issue-tracker.md`。

## 5. 完成

说明配置已完成，以及哪些工程 Skill 会读取这些文件。用户以后可以直接编辑 `docs/agents/*.md`。切换 issue tracker 或从头设置时才需要重跑这个 Skill。

[英文原文](https://github.com/mattpocock/skills/blob/abeeaf9/skills/engineering/setup-matt-pocock-skills/SKILL.md)
