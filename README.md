# CC Workflow

Repo: https://github.com/12vblanco/CC-WORKFLOW.git


My personal workflow hub for developing with Claude Code — reusable prompts, templates,
commands, config, coding standards, and checklists I copy from when working on projects.

## What's here

| Folder        | What it holds                                                        |
|---------------|----------------------------------------------------------------------|
| `prompts/`    | Reusable prompts, grouped by task (planning, review, debugging, …)   |
| `templates/`  | Starter docs: CLAUDE.md, context files, PR/commit templates          |
| `commands/`   | Custom slash-command / skill definitions                             |
| `agents/`     | Subagent definitions                                                 |
| `config/`     | settings.json snippets, hooks, MCP configs                           |
| `standards/`  | Per-language coding standards + high-level architecture diagrams     |
| `workflows/`  | Step-by-step SOPs / checklists for my dev flow                       |

## How I use it

Browse to the relevant folder and copy what I need into the active project.

## Adding a new language

Create `standards/<language>/` with `coding-standards.md` and `architecture.md`
(architecture uses a Mermaid diagram so it renders on GitHub).
