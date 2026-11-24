# Personal Skills

个人维护的手动 Skill 集合。

## 工作流程入口

| 入口 | 采用的工作方式 | 调用示例 |
| --- | --- | --- |
| [`$learn-or-do`](learn-or-do/SKILL.md) | 按 Intent → Spec → Modeling → Implement 选步，支持学习、实践，也支持实现、验证和交付软件。 | `$learn-or-do 导出功能的方案已定，我想按方案实现。` |
| [`$ask-matt`](ask-matt/SKILL.md) | 按 Matt Pocock 的标准开发流程选步，可从想法、spec、ticket 或代码改动开始。 | `$ask-matt 导出功能的 spec 已写好，我想拆成 ticket。` |

## 其他 Skill 与参考资料

- [`$verify`](verify/SKILL.md) 核对当前成果与约定，给出证据。[`$show-me`](show-me/SKILL.md) 展示本轮实际变化，可在任一阶段单独调用。
- 需要专门分析业务问题、需求或概要设计时，见[软件工程理解与概要设计](learn-or-do/references/software-engineering.md)。
- [Thought to Skill](thought-to-skill/SKILL.md) 将经验、想法与实践反馈提炼为可试用的 Skill。
- [Bro｜说人话](bro/SKILL.md) 是 [Poteto 的 bro](https://github.com/cursor/plugins/blob/7a58391/pstack/skills/bro/SKILL.md) 的中文译本。
- [Why｜追溯决策理由](why/SKILL.md) 是 [Poteto 的 why](https://github.com/cursor/plugins/blob/7a58391/pstack/skills/why/SKILL.md) 的中文译本。
- [Handoff｜交接](handoff/SKILL.md) 是 [Matt Pocock 的 handoff](https://github.com/mattpocock/skills/blob/abeeaf9/skills/productivity/handoff/SKILL.md) 的中文译本。
- [Writing for Agents｜为 Agent 编写文档](writing-for-agents/SKILL.md) 是 [Matt Pocock 的 writing-for-agents](https://github.com/mattpocock/skills/blob/abeeaf9/skills/productivity/writing-for-agents/SKILL.md) 的中文译本，按个人偏好设为手动调用。
- [Matt Pocock 开发流程中文译稿](Raw-material/matt-pocock-development/README.md) 收录从想法到交付的主流程、相关 Skill 和术语索引约定。
- `Raw-material/` 保存原始设想、早期版本和摘出的材料。

## 本机使用

本仓库位于 Obsidian 笔记库根目录的 `daiyu-personal-skills/`，完整保存文件和 Git 历史。Codex 的 `~/.codex/skills/` 中为各 Skill 建有同名软链接，分别指向本仓库的同名目录。`~/daiyu-personal-skills` 是指向本仓库的兼容链接。

更新后在本仓库提交并推送：

```sh
git add ask-matt grill-me grilling grill-with-docs domain-modeling prototype setup-matt-pocock-skills to-spec to-tickets implement-spec tdd codebase-design code-review pr retro intent spec modeling implement bro why challenge-me handoff writing-for-agents learn-or-do verify show-me thought-to-skill software-problem-framing software-requirements software-high-level-design Raw-material README.md
git commit -m "Update personal skills"
git push
```

其他设备先将本仓库克隆到 Obsidian 笔记库根目录的 `daiyu-personal-skills/`，再将所需 Skill 目录链接或安装到该设备的 Skill 目录。笔记库位置改变时需调整外部链接。更新使用 `git pull --ff-only`。

## 第三方许可

以下目录中的上游内容及其译稿采用 MIT License。

- Copyright (c) 2026 Lauren Tan — `bro/`、`why/`。
- Copyright (c) 2026 Matt Pocock — `Raw-material/matt-pocock-development/`、`ask-matt/`、`code-review/`、`codebase-design/`、`domain-modeling/`、`grill-me/`、`grill-with-docs/`、`grilling/`、`handoff/`、`implement-spec/`、`pr/`、`prototype/`、`retro/`、`setup-matt-pocock-skills/`、`tdd/`、`to-spec/`、`to-tickets/`、`writing-for-agents/`。

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
