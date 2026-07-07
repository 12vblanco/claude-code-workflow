# CC Workflow

Repo: https://github.com/12vblanco/claude-code-workflow.git

My personal workflow hub for developing with Claude Code — reusable prompts, templates,
coding standards, and checklists I copy from when working on projects.

## What's here

| Folder          | What it holds                                                                    |
|-----------------|----------------------------------------------------------------------------------|
| `prompts/`      | Reusable prompts (e.g. prototype/mock-up prompt for Stitch, V0, Lovable)          |
| `templates/`    | Starter context files: project-overview, coding-standards, ai-interaction, current-feature |
| `.claude/skills/` | Custom skill / slash-command definitions (feature, research, cleanup, list-components) — auto-loaded by Claude Code when working in this repo; copy into other projects' `.claude/skills/` to use them there |
| `standards/`    | Per-language coding standards + high-level architecture diagrams (frontend, TypeScript, React, Vue) |
| `workflow/`     | Step-by-step SOPs / checklists for my dev flow (see `workflow/main-workflow.md`)  |
| `_screenshots/` | Design reference images used as UI mock-up examples                               |

## How I use it

Browse to the relevant folder and copy what I need into the active project.
The full process — from project spec to feature loop — is documented in
[`workflow/main-workflow.md`](workflow/main-workflow.md).

## Adding a new language

Create `standards/<language>/` with `coding-standards.md` and `architecture.md`
(architecture uses a Mermaid diagram so it renders on GitHub).
