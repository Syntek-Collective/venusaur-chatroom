# Contributing to Venusaur Chatroom

Thanks for your interest in contributing! This document covers how to propose changes, plus our Git workflow, branch naming, commit messages, and pull-request process. AI coding agents should also read `AGENTS.md`.

By participating, you agree to follow our [Code of Conduct](CODE_OF_CONDUCT.md). To report a security problem, follow [SECURITY.md](SECURITY.md) instead of opening a public issue.

## Ways to Contribute

- **Report a bug:** open an issue with the bug report template.
- **Suggest a feature:** open an issue with the feature request template so we can talk it through before you build it.
- **Fix something:** comment on an issue to say you're working on it, then open a pull request.

Small fixes like typos don't need an issue first.

## Git Workflow

`main` is protected. Nobody pushes to it directly, and every change goes through a pull request with one maintainer's approval.

```text
pick an issue
→ fork the repo (or create a branch, if you're a maintainer)
→ make focused changes and commit
→ push the branch
→ open a pull request into main
→ get one maintainer's approval
→ merge
```

### Starting New Work

If you're not a maintainer, first [fork the repository](https://github.com/Syntek-Collective/venusaur-chatroom/fork), clone your fork, and add this repository as `upstream`:

```bash
git clone https://github.com/<your-username>/venusaur-chatroom.git
cd venusaur-chatroom
git remote add upstream https://github.com/Syntek-Collective/venusaur-chatroom.git
```

Then start each change from an up-to-date `main` (use `upstream` instead of `origin` if you're working from a fork):

```bash
git switch main
git pull origin main
git switch -c <type>/<short-description>
```

See `README.md` for installing dependencies and setting up `server/.env`.

## Branch Naming

```text
<type>/<short-description>
```

- `feat/` new feature
- `fix/` bug fix
- `refactor/` restructuring without changing behavior
- `docs/` documentation
- `test/` tests
- `chore/` configuration or tooling
- `style/` visual styling or formatting

Examples: `feat/room-list`, `feat/signup-form`, `fix/socket-reconnect`, `chore/setup-vite`

Keep branch names lowercase with hyphens. Use one branch per task, and delete it after it's merged.

## Commit Messages

Follow Conventional Commits:

```text
<type>(<scope>): <short description>
```

Suggested scopes: `client`, `server`, `auth`, `db`, `sockets`, `docs`

Examples:

```text
feat(sockets): broadcast messages to room members
feat(auth): add sign-up route
fix(db): save message timestamps
docs(readme): add setup steps
```

Rules:

- Keep the subject under 72 characters.
- Use imperative mood: `add`, not `added`.
- Use lowercase after the colon, with no trailing period.
- One logical change per commit.

## Pull Requests

- Open pull requests against `main`.
- Fill in the pull request template: summary, changes, type, and how to test.
- Only list commands you actually ran.
- Keep each pull request focused on one task.
- Link the issue it closes, for example `Closes #12`.
- If a change touches authentication, payments, or sessions, say so in the summary so a maintainer looks closely.

### Before Opening a Pull Request

```bash
git switch main
git pull origin main
git switch <your-branch>
git merge main
```

Then check the following. `npm run dev` does not type-check, so the build step is what catches TypeScript errors. See `README.md` for how to run the app.

- The app starts locally.
- If you changed `client/`: `npm run build` and `npm run lint` pass in `client/`.
- If you changed `server/`: `npm run build` passes in `server/`.
- `npm test` passes in each folder you changed.
- `npm run format:check` passes in the repository root. If not, run `npm run format`.
- You tested the change by hand.
- Debugging code and stray `console.log`s are removed.
- No secrets or `.env` files are included.
- Docs are updated if behavior or decisions changed.

## Reviewing

Reviewers should check that the change:

- matches the task
- works when tested by hand
- doesn't break another part of the app
- passes the server and client tests (`npm test` in `server/` and `client/`)

Keep comments clear, kind, and about the code.

## After a Pull Request Is Merged

```bash
git switch main
git pull origin main
git branch -d <branch-name>
```

## Secrets

The repository is public. Never commit API keys, database URLs, or passwords. Put real values in `.env`, which is gitignored, and add only the variable names to `.env.example`. If a secret is ever pushed, revoke it immediately, because deleting the commit is not enough. Use Stripe test-mode keys (`sk_test_...`) for local development, never live keys.

## License

By contributing, you agree that your contributions will be licensed under the project's [MIT License](LICENSE).

## Documentation Ownership

Each document owns one concept. Reference it instead of copying its details:

- `README.md` covers introduction, setup, and running the app
- `PROJECT_BRIEF.md` covers goals, MVP scope, ownership, timeline, and stretch goals
- `DECISIONS.md` covers technical decisions, their reasoning, and open questions
- `CONTRIBUTING.md` covers Git workflow and pull requests
- `AGENTS.md` covers AI-agent behavior

If code and docs disagree, point out the inconsistency rather than guessing which one is right.
