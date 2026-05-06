# AIProjectScaffold

> A phased, AI-collaborative scaffolding for building Apps from zero — from idea to release.
>
> 一个面向 AI 协作的 App 从零构建脚手架——从一个想法走到上线。

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)

---

## English — Quick Start

### What is this?

A directory template (`0_Docs` → `6_Release`) plus an `AI_GUIDE.md` that tells any AI agent how to take a vague product idea through six phases until it ships:

```
1_Thinks → 2_Prds → 3_Designs → 4_Codes → 5_Tests → 6_Release
```

Every phase has Markdown templates with prompts the AI fills in collaboratively with you. The AI is forbidden from skipping phases or guessing unknowns — it must ask, or mark `TODO`.

### Two ways to use it

**A. Manual** — clone, copy the directory into your new project, start filling `1_Thinks/problem_definition.md`.

```bash
git clone https://github.com/erduoniba/AIProjectScaffold.git my-new-app
cd my-new-app
# open AI_GUIDE.md in your AI tool, follow the workflow
```

**B. Claude Code skill (recommended)** — one shot in any directory:

```bash
# 1. Clone this repo somewhere stable
git clone https://github.com/erduoniba/AIProjectScaffold.git ~/code/AIProjectScaffold

# 2. Tell Claude Code where it lives, in ~/.claude/settings.json
{
  "env": { "AI_SCAFFOLD_PATH": "/Users/you/code/AIProjectScaffold" }
}

# 3. Drop the new-project skill into ~/.claude/skills/new-project/SKILL.md
#    (see /skills/new-project/SKILL.md in this repo)
```

Then in any Claude Code session: *"Create a new project called my-app"* — and the scaffold is generated and personalized in the current directory.

### Why phased?

Most AI coding sessions jump straight to code, then bolt on requirements after the fact. This scaffold inverts that: **AI cannot write code until product, users, and architecture are written down**. The phases are gates, not suggestions.

### Phase outputs (TL;DR)

| Phase | Outputs | Exit gate |
|---|---|---|
| `1_Thinks` | problem, personas, value prop | "this is what I want to build" |
| `2_Prds` | PRD, user stories, MVP scope | MVP feature list confirmed |
| `3_Designs` | UX flows, UI specs, architecture, data, API, tech stack | Architecture + stack signed off |
| `4_Codes` | runnable code | Happy path works end-to-end |
| `5_Tests` | test plan, cases, automation | Critical paths covered |
| `6_Release` | deploy doc, changelog, release | Live |

Read [`AI_GUIDE.md`](./AI_GUIDE.md) for the full rules.

---

## 中文 — 完整文档

### 这是什么

一个**面向 AI 协作**的 App 构建脚手架。它把「从一个模糊的想法到上线」拆成 6 个阶段，每个阶段有目录、模板和明确的产出标准。AI 必须按阶段推进，不能跳，也不能瞎编——不知道就问，问不到就标 `TODO`。

核心是根目录的 [`AI_GUIDE.md`](./AI_GUIDE.md)，它定义了 AI 在每个阶段该做什么、该输出什么、何时停下来等用户确认。

### 解决什么问题

直接让 AI 写代码的常见痛点：

- 跳过需求直接堆功能，做出来不是用户要的
- 没架构没数据模型，三周后改不动
- 没测试没文档，能跑但没人敢动
- 一次性输出几千行，无法 review

这个脚手架强制 AI 把上面的事**前置写下来**，并按阶段交付。

### 目录结构

```
.
├── AI_GUIDE.md         # AI 工作总纲（最重要，AI 必读）
├── README.md           # 本文件
├── LICENSE
├── 0_Docs/             # 参考资料（行业、竞品、外部规范）
├── 1_Thinks/           # 构思（问题、用户、价值）
├── 2_Prds/             # 产品需求（PRD、用户故事、MVP）
├── 3_Designs/          # 设计（UX、UI、架构、数据、API、技术栈）
├── 4_Codes/            # 代码实现
├── 5_Tests/            # 测试计划与用例
└── 6_Release/          # 发布、部署、Changelog
```

数字前缀就是推进顺序：`0 → 1 → 2 → 3 → 4 → 5 → 6`。允许回流（在 3 期间发现 2 漏写，回去补）。

### 使用方式

#### 方式 A：手动使用（任何 AI 工具均可）

适用于 Cursor、Continue、ChatGPT、Gemini 等任意 AI 编程工具。

```bash
# 1. 克隆本仓库到新项目
git clone https://github.com/erduoniba/AIProjectScaffold.git my-new-app
cd my-new-app

# 2. 删除本仓库的 .git，重新初始化
rm -rf .git && git init

# 3. 在 AI 工具里把 AI_GUIDE.md 加入上下文
#    告诉 AI：「按 AI_GUIDE.md 的流程，从 1_Thinks 开始」
```

#### 方式 B：Claude Code Skill（推荐）

让 Claude Code 一句话生成脚手架，无需手动复制。

**1. 把脚手架放到一个稳定路径**

```bash
git clone https://github.com/erduoniba/AIProjectScaffold.git ~/code/AIProjectScaffold
```

**2. 在 `~/.claude/settings.json` 中声明路径**

```json
{
  "env": {
    "AI_SCAFFOLD_PATH": "/Users/you/code/AIProjectScaffold"
  }
}
```

**3. 安装 `new-project` skill**

把本仓库的 [`skills/new-project/SKILL.md`](./skills/new-project/SKILL.md) 拷到 `~/.claude/skills/new-project/SKILL.md`。

**4. 任意目录下使用**

在 Claude Code 会话中说：

- `新建项目 my-app`
- `帮我在 ~/Desktop 下新建项目 demo`
- `/new-project my-app`

Skill 会：
1. 校验 `$AI_SCAFFOLD_PATH` 路径有效
2. 在目标位置创建项目目录并复制全部模板
3. 把新项目 README 的标题改成你的项目名
4. 列出生成的文件树，提示从 `1_Thinks/problem_definition.md` 开始

### 工作流概览

```
        0_Docs（参考资料，随时补）
              ↓
1_Thinks → 2_Prds → 3_Designs → 4_Codes → 5_Tests → 6_Release
   ↑__________________________________________| (允许回流修订)
```

| 阶段 | 关键产出 | 出口确认 |
|---|---|---|
| 1_Thinks | 问题定义、用户画像、价值主张 | "这就是我要做的" |
| 2_Prds | PRD、用户故事、MVP 范围 | MVP 功能清单确认 |
| 3_Designs | UX 流程、UI 规范、架构、数据、API、技术栈 | 架构图 + 技术栈签字 |
| 4_Codes | 可运行的代码 | 主流程端到端跑通 |
| 5_Tests | 测试计划、用例、自动化 | 关键路径覆盖 |
| 6_Release | 部署文档、Changelog、发布 | 上线 |

完整规则见 [`AI_GUIDE.md`](./AI_GUIDE.md)。

### 自定义

脚手架是**模板**，不是教条。常见定制：

- **改阶段**：删 `5_Tests` 或合并 `5+6`，记得同步更新 `AI_GUIDE.md` 的阶段表
- **改模板**：编辑 `*/README.md` 和模板文件。改完只影响**未来**新建的项目，已生成的不会自动同步
- **加阶段**：例如加 `7_Maintenance`（运维），按现有命名风格新建即可
- **改技术栈默认值**：`3_Designs/tech_stack.md` 里改成你团队的标准栈

### FAQ

**Q: 为什么不用 LLM 一次性把所有文档生成出来？**
A: 因为产品/用户/架构这些决定，**必须由人确认**。一次性生成等于让 AI 替你做决定，等你发现不对劲已经写到代码层了。

**Q: 已经写代码了才接入这个脚手架，还有意义吗？**
A: 有。从 `2_Prds/prd.md` 倒推一份「我们正在做什么」，再用 `3_Designs/architecture.md` 沉淀当前架构，足以避免后续越改越乱。

**Q: 模板太多/太正式怎么办？**
A: 删。MVP 阶段保留 `problem_definition.md`、`mvp_scope.md`、`architecture.md`、`api_design.md` 四份足矣。

**Q: 多人协作怎么用？**
A: 把本脚手架放到团队仓库做模板（GitHub Template Repo），每次 `Use this template` 即可。`AI_GUIDE.md` 是团队和 AI 都能读的契约。

### 贡献

欢迎 PR：

- 新阶段、新模板
- 改进 `AI_GUIDE.md` 的规则（带场景说明）
- 适配其他 AI 工具的 skill / command 实现
- 中英文文档校对

### License

[MIT](./LICENSE)
