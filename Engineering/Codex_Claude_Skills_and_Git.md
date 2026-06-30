# Engineering：Codex、Claude Code、Skills 与 Git 记录

## 1. Codex Skill 报错

曾遇到 Codex 启动时报错：

```text
Skipped loading 1 skill(s) due to invalid SKILL.md files.
missing YAML frontmatter delimited by ---
```

含义是：Codex 启动时扫描到了某个 Skill，但它的 `SKILL.md` 格式不合法。

Skill 文件开头必须有 YAML frontmatter：

```md
---
name: debug-pro
description: use this skill when debugging code, logs, stack traces, terminal errors, build failures, runtime exceptions, or performance issues.
---

# Debug Pro
```

## 2. 解决方式

可以：

1. 修复 `SKILL.md`。
2. 移走坏掉的 skill 文件夹。
3. 在配置中禁用该 skill。

## 3. worktree 文件

`.worktree` 通常和 Git worktree 或工具生成的工作区状态有关。是否加入 gitignore 要看它是否是本地状态文件。

一般原则：

- 项目源码、配置模板、文档应该提交。
- 本地缓存、临时状态、机器相关配置不应该提交。

## 4. Codex / Claude Code 插件问题

插件安装后如果无法显示，应该检查：

- 插件是否真的安装成功。
- CLI 是否读取正确目录。
- 是否需要 reload。
- 是否存在版本兼容问题。
- 是否存在配置文件错误。

## 5. GitHub 知识库维护建议

这个知识库应该长期用 Git 管理：

```bash
git add .
git commit -m "Update knowledge base"
git push
```

每次整理一个主题，就提交一次，避免大批量无结构更新。

