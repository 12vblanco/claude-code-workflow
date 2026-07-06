# Main Workflow

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

In `CLAUDE.md` inside the project a message will direct the agent to read these files that will be included in the `/context` folder:

| File | Purpose |
| --- | --- |
| `project-overview.md` | The project specs that can be created and refined with AI |
| `coding-standards.md` | Coding style and rules — <https://cursor.directory/> contains best practices |
| `ai-interaction.md` | How the AI must interact and communicate |
| `current-feature.md` | The feature we are working right now |

## 4. Creating Features

This is the common workflow for every single feature/fix:

1. **Document** — Document the feature in `@context/current-feature.md` by using a spec file and asking AI to update `@current-feature`
2. **Branch** — Create new branch for feature, fix, etc
3. **Implement** — Implement the feature/fix that I create in `@context/current-feature.md`
4. **Test** — Verify it works in the browser. Implement unit testing later. Run `npm run build` and fix any errors
5. **Iterate** — Iterate and change things if needed
6. **Commit** — Only after build passes and everything works (PR in a team env)
7. **Merge** — Merge to main
8. **Delete Branch** — Delete branch after merge
9. **Deploy** — Push to production and test the production version
10. **Review** — Review AI-generated code periodically and on demand
11. **Mark Complete** — Mark as completed in `@context/current-feature.md` and add to history

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

### Closing the Loop

Once the feature works and the build passes, close the loop:

> Set the current feature in `@context/current-feature.md` to completed, remove the info and add it to the history.

> Commit to the feature branch, merge to main, delete the feature branch and push to remote.
