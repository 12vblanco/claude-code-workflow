# Skills Workflow

This is the faster, skill-based version of `main-workflow.md`. Reusable skills live in `.claude/skills/` — copy them into each project's `.claude/skills/` folder (restart Claude Code to pick them up). Manual prompts always remain an option; the skills are just the repeatable shortcut.

## 1. Project Specs

Once the codebase exists, `/research <prompt-name>` generates documentation the same way a manual prompt would — it reads a prompt file from `context/research/<prompt-name>.md` (output location, what to investigate, sources) and writes the findings to `/docs/`, without touching source code.

## 2. Initial Context Files

After setting up `CLAUDE.md` and the `/context` files, copy the reusable skills from this repo's `.claude/skills/` folder into the new project's `.claude/skills/` so `/feature`, `/cleanup`, `/list-components` and `/research` are available there.

## 3. Creating Features

The full manual cycle (document → branch → implement → test → iterate → commit → merge → delete branch → deploy → review → mark complete) condenses to:

```text
/feature load [spec-file.md]   # document: fill current-feature.md from the spec (adds current feature's status goals notes and history)
/feature start                         # branch + implement the goals
/feature test                          # unit tests (Vitest) for actions/utilities
/feature review                        # verify goals met, quality, no scope creep
/feature complete                      # commit, merge to main, delete branch, update history, push
```

Iterate and Deploy stay manual — no skill replaces them.

### Step by step

- **Document** — `/feature load <spec-name or description>` fills `@context/current-feature.md` from a spec file or inline description, and sets the status to in progress.
- **Branch + Implement** — `/feature start` creates the branch from the feature name and implements the goals one by one.
- **Test** — `/feature test` checks which server actions/utilities the feature added, writes Vitest unit tests where they add value, and runs `npm test`.
- **Review** — `/feature review` checks goals met, code quality, and scope creep. `/feature explain` documents what changed and why. `/cleanup check` does periodic housekeeping (reports only; `/cleanup run` asks which items to fix).
- **Complete** — `/feature complete` commits, merges to main, deletes the feature branch, resets `current-feature.md` (appending the feature to History), and pushes to origin in one step.

## 4. Skills Reference

| Skill              | Arguments                                                          | What it does (replaces)                                                                                                                                                                                          |
| ------------------ | ------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `/feature`         | `load` \| `start` \| `test` \| `review` \| `explain` \| `complete` | Full feature lifecycle: fill `current-feature.md` from a spec, branch + implement, unit-test, review against goals, explain the diff, then commit/merge/cleanup                                                  |
| `/cleanup`         | `check` \| `run`                                                   | Housekeeping audit: history order, stray `console.log`s, unused imports, stale TODOs/`@ts-ignore`, orphaned files, context files vs. reality, `.env` parity. `check` reports only; `run` asks which items to fix |
| `/list-components` | `[subdirectory]`                                                   | Quick numbered inventory of component files with one-line descriptions                                                                                                                                           |
| `/research`        | `<prompt-name>`                                                    | Runs a research prompt from `context/research/` and writes documentation (no code changes)                                                                                                                       |
