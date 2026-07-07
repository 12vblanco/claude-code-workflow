# Main Workflow

Each section below shows the **manual way** (prompts you write yourself) and, where one exists, the **faster way** using the reusable skills in `.claude/skills/` (marked with ⚡). See the [Skills Reference](#5-skills-reference) at the end.

## 1. Project Specs

The project starts by hand-writing a rough `project-spec.md` with our planning notes
→ run it through ChatGPT/Claude to format and enrich it (Prisma draft, diagrams, etc.)
→ the output becomes `context/project-overview.md`.

This defines the main guidelines in terms of:

- The problem to solve
- The users
- The features needed
- How the data will look
- The tech stack used
- Monetization (if any)
- UX main components

> ⚡ **Skill:** later, once the codebase exists, `/research <prompt-name>` generates documentation the same way — it reads a prompt file from `context/research/<prompt-name>.md` (output location, what to investigate, sources) and writes the findings to `/docs/`, without touching source code.

## 2. Creating the App

Set the environment manually. If React/TypeScript, for example, use:

```bash
npm create vite@latest my-app -- --template react-ts
```

Then remove the boilerplate that isn't needed.

Initialize git and push to GitHub before starting the first feature:

```bash
git init
git add .
git commit -m "chore: initial setup"
# create the GitHub repo and push
```

## 3. Initial Context Files

Run `/init` first to let Claude generate a baseline `CLAUDE.md`, then replace/extend it so it contains:

- Pointers to the four context files in `/context` (below)
- A short tech-stack summary
- Quick commands (dev, build, lint)
- Hard rules (e.g. "do not add Claude to commit messages")

All the files are in the `/template` folder.

> ⚡ **Skill:** also copy the reusable skills from this repo's `.claude/skills/` folder into the new project's `.claude/skills/` so `/feature`, `/cleanup`, `/list-components` and `/research` are available there (restart Claude Code to pick them up).

In `CLAUDE.md` inside the project a message will direct the agent to read these files that will be included in the `/context` folder:

| File                  | Purpose                                                                      |
| --------------------- | ---------------------------------------------------------------------------- |
| `project-overview.md` | The project specs that can be created and refined with AI                    |
| `coding-standards.md` | Coding style and rules — <https://cursor.directory/> contains best practices |
| `ai-interaction.md`   | How the AI must interact and communicate                                     |
| `current-feature.md`  | The feature we are working right now                                         |

## 4. Creating Features

This is the common workflow for every single feature/fix:

1. **Document** — Document the feature in `@context/current-feature.md` by using a spec file and asking AI to update `@current-feature` — ⚡ `/feature load <spec-name or description>`
2. **Branch** — Create new branch for feature, fix, etc — ⚡ `/feature start` (creates the branch from the feature name)
3. **Implement** — Implement the feature/fix that I create in `@context/current-feature.md` — ⚡ `/feature start` (implements the goals one by one)
4. **Test** — Verify it works in the browser. Implement unit testing later. Run `npm run build` and fix any errors — ⚡ `/feature test` (writes/updates Vitest unit tests and runs them)
5. **Iterate** — Iterate and change things if needed
6. **Commit** — Only after build passes and everything works (PR in a team env) — ⚡ `/feature complete`
7. **Merge** — Merge to main — ⚡ `/feature complete`
8. **Delete Branch** — Delete branch after merge — ⚡ `/feature complete`
9. **Deploy** — Push to production and test the production version
10. **Review** — Review AI-generated code periodically and on demand — ⚡ `/feature review` (goals met, quality, scope creep) · `/feature explain` (what changed and why) · `/cleanup check` (periodic housekeeping)
11. **Mark Complete** — Mark as completed in `@context/current-feature.md` and add to history — ⚡ `/feature complete` (resets the file and appends to History)

### ⚡ The Faster Way — Skill Loop

The whole cycle above condenses to:

```text
/feature load dashboard-phase-1-spec   # document: fill current-feature.md from the spec
/feature start                         # branch + implement the goals
/feature test                          # unit tests (Vitest) for actions/utilities
/feature review                        # verify goals met, quality, no scope creep
/feature complete                      # commit, merge to main, delete branch, update history, push
```

Steps 5 (Iterate) and 9 (Deploy) stay manual.

### Mock-ups & Dummy Data

When creating a feature we can run the `@prototype-example-prompt.md` plus the UI/UX section of the `project-overview.md` through Stitch, or V0, or Lovable to obtain a mock-up that then we can use in Claude Code for reference. The mock-up can go in a `context/screenshots` folder. We tell CC it does not have to be exact but more as a reference.

In project overview then:

> Refer to the screenshots below as a base for the dashboard. It does not have to be exact but more as a reference.
>
> `@context/screenshots/dashboard-ui-drawer.png`
> `@context/screenshots/dashboard-ui-main.png`

The same way we create a screenshot as reference, we can create dummy data for our feature if needed. Use a prompt and save + reference.

### Feature Spec Files

Then in `context/feature` we create the spec file for each feature or each part of a feature. The prompt should be:

> Update the `@context/current-feature.md` to add the feature from `@dashboard-phase-1-spec.md` [or the relevant spec file]. Set the status to in progress.

⚡ Skill equivalent: `/feature load dashboard-phase-1-spec` (also accepts an inline description instead of a spec file).

> Create a new branch and implement the `@context/current-feature.md`.

⚡ Skill equivalent: `/feature start`.

> Update the current feature file as completed and proceed to commit and push, and delete the old branch.

⚡ Skill equivalent: `/feature complete`.

We can create a test file in `@scripts/test.ts` and update for each feature (once the feature is implemented ask to update the test file) and check if the changes worked each time.

⚡ Skill equivalent: `/feature test` — checks which server actions/utilities the feature added, writes Vitest unit tests where they add value, and runs `npm test`.

### Closing the Loop

Once the feature works and the build passes, close the loop:

> Set the current feature in `@context/current-feature.md` to completed, remove the info and add it to the history.

> Commit to the feature branch, merge to main, delete the feature branch and push to remote.

⚡ Skill equivalent: `/feature complete` does both of these in one step — commit, merge to main, delete the branch, reset `current-feature.md` (appending the feature to History), and a single push to origin.

## 5. Skills Reference

Reusable skills live in `.claude/skills/` (copy them into each project's `.claude/skills/` folder). Manual prompts always remain an option — the skills are just the faster, repeatable version.

| Skill | Arguments | What it does (replaces) |
| --- | --- | --- |
| `/feature` | `load` \| `start` \| `test` \| `review` \| `explain` \| `complete` | Full feature lifecycle: fill `current-feature.md` from a spec, branch + implement, unit-test, review against goals, explain the diff, then commit/merge/cleanup |
| `/cleanup` | `check` \| `run` | Housekeeping audit: history order, stray `console.log`s, unused imports, stale TODOs/`@ts-ignore`, orphaned files, context files vs. reality, `.env` parity. `check` reports only; `run` asks which items to fix |
| `/list-components` | `[subdirectory]` | Quick numbered inventory of component files with one-line descriptions |
| `/research` | `<prompt-name>` | Runs a research prompt from `context/research/` and writes documentation (no code changes) |
