# Personal Skills

个人维护的两个 Skill，仓库为统一维护源。

- `learn-or-do/`：围绕当前问题建立理解、开展实践并依据反馈修正。
- `thought-to-skill/`：将经验、想法与实践反馈提炼为可试用的 Skill。
- `history/`：原始设想与初版材料，保留原文，不作为运行依赖。历史引用可能指向旧位置。

两个 Skill 文件夹只存放运行所需内容。相关 Git 版本历史已迁入本仓库。

## 本机使用

本机的 `~/.codex/skills/learn-or-do`、`~/.codex/skills/thought-to-skill`，以及笔记中的 learn-or-do 源目录，通过软链接指向本仓库；修改任一入口即修改同一份文件。

更新后在本仓库提交并推送：

```sh
git add learn-or-do thought-to-skill history README.md
git commit -m "Update personal skills"
git push
```

其他设备先克隆本仓库，再将所需 Skill 目录链接或安装到该设备的 Skill 目录。更新使用 `git pull --ff-only`。
