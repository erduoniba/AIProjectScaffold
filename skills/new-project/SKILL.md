---
name: new-project
description: Generate a fresh AI-collaborative project scaffold at a target location. Triggers when the user wants to create a new project from the master scaffold (e.g. "新建项目 XXX", "新项目 XXX", "帮我新建项目", "create new project XXX", "scaffold a new project"). Reads the scaffold source from $AI_SCAFFOLD_PATH and copies it to the target.
---

# new-project

Generate a copy of the master AI project scaffold at a target location, then personalize it for the new project.

## Master scaffold path (required)

The scaffold source is resolved via the env var:

```bash
SCAFFOLD_SRC="$AI_SCAFFOLD_PATH"
```

`$AI_SCAFFOLD_PATH` **must** be set before this skill is used. If it is not set, **stop immediately** and tell the user to configure it. Do not guess a path.

How to set it (point this at where the user cloned this repo):

- **Permanent (Claude Code only)**: add to `~/.claude/settings.json`:
  ```json
  { "env": { "AI_SCAFFOLD_PATH": "/absolute/path/to/AIProjectScaffold" } }
  ```
- **Permanent (any shell)**: add `export AI_SCAFFOLD_PATH=/absolute/path/to/AIProjectScaffold` to `~/.zshrc` or `~/.bashrc`.

The resolved directory must contain `AI_GUIDE.md`, `README.md`, and the phase directories `0_Docs/` `1_Thinks/` `2_Prds/` `3_Designs/` `4_Codes/` `5_Tests/` `6_Release/`. If `AI_GUIDE.md` is missing, treat the path as invalid and stop.

## Inputs (parse from `$ARGUMENTS`)

- **`project_name`** (required): the new project's directory name. Validate: only letters, digits, dashes, underscores, and Chinese characters. No spaces, no slashes.
- **`target_dir`** (optional): parent directory under which to create the project. Default: current working directory.
- **`--tagline "..."`** (optional): one-line project tagline written into the new README.

If `project_name` is missing, ask the user for it before continuing. Do not guess.

## Steps

1. **Resolve target path**: `<target_dir>/<project_name>` (absolute path).

2. **Pre-flight checks**:
   - `$AI_SCAFFOLD_PATH` is set, exists, and contains `AI_GUIDE.md`. Otherwise, stop and instruct the user.
   - Target path does not already exist, OR is empty. If it exists and is non-empty, **ask the user** before overwriting; never overwrite silently.

3. **Copy the scaffold**:
   ```bash
   mkdir -p "<target_path>"
   cp -R "$AI_SCAFFOLD_PATH/." "<target_path>/"
   ```
   Note the trailing `/.` on the source — this copies the contents, not the source directory itself.

4. **Strip source-repo artifacts** that should not appear in a generated project:
   - `<target_path>/.git/` — the scaffold's git history (user will `git init` their own)
   - `<target_path>/LICENSE` — the scaffold's MIT license (new project picks its own)
   - `<target_path>/skills/` — the `new-project` skill is for the scaffold repo itself, not downstream projects
   - `<target_path>/.github/` if present — scaffold CI/issue templates do not transfer

   ```bash
   rm -rf "<target_path>/.git" "<target_path>/LICENSE" "<target_path>/skills" "<target_path>/.github"
   ```

5. **Replace the copied `README.md` with a slim project README** (do NOT try to edit the scaffold's bilingual open-source README — just overwrite it). Use this template, substituting `<project_name>` and `<tagline>`:

   ````markdown
   # <project_name>

   > <tagline or "TODO: 一句话项目介绍">

   本项目遵循 [AIProjectScaffold](https://github.com/erduoniba/AIProjectScaffold) 工作流。AI 协作请先阅读根目录的 [`AI_GUIDE.md`](./AI_GUIDE.md)。

   ## 当前阶段

   ```
   [x] 1_Thinks     ← 进行中：定义问题、用户、价值
   [ ] 2_Prds
   [ ] 3_Designs
   [ ] 4_Codes
   [ ] 5_Tests
   [ ] 6_Release
   ```

   ## 目录

   ```
   .
   ├── AI_GUIDE.md      # AI 工作总纲
   ├── 0_Docs/          # 参考资料
   ├── 1_Thinks/        # 构思
   ├── 2_Prds/          # 产品需求
   ├── 3_Designs/       # 设计
   ├── 4_Codes/         # 代码
   ├── 5_Tests/         # 测试
   └── 6_Release/       # 发布
   ```
   ````

   If `--tagline` was not provided, keep the `TODO: 一句话项目介绍` placeholder — do not invent one.

6. **Verify**: list the created tree (`find <target_path> -type f | sort`) and confirm key files exist (`AI_GUIDE.md`, `1_Thinks/problem_definition.md`, etc.).

7. **Report to user**:
   - The absolute path created.
   - Suggested next step: open `<target_path>/AI_GUIDE.md` for the workflow, then start filling `1_Thinks/problem_definition.md`.
   - Mention they can `cd` into the new dir and start a fresh Claude session there for cleaner context.

## Don't

- **Don't `git init`** — leave version control to the user.
- **Don't install dependencies / run package managers** — `4_Codes/` is empty by design; tech stack is decided in `3_Designs/tech_stack.md` first.
- **Don't fill template content with guesses** about the project (problem, users, features). Leave templates blank — the user fills them in collaboration with the AI later.
- **Don't skip the personalize step** — an unpersonalized README is a smell.
- **Don't add a `.gitignore`, LICENSE, or any other convention file** unless the user asks.

## After scaffold creation — what AI should do next

If, in the same conversation, the user immediately asks to start working on the project (e.g. "好了，开始吧" / "let's start"), enter the **1_Thinks** phase per `AI_GUIDE.md`: ask 3–5 clarifying questions before writing anything to `problem_definition.md`.

## Updating the master scaffold

If the user later edits files at `$AI_SCAFFOLD_PATH`, those changes apply to **future** new projects only. Already-generated projects are independent copies and do not auto-update.
