# Contributing

## Branching strategy

- `main` is protected. No direct pushes. No force-push. No deletion.
- Every change lands via a pull request targeting `main`.
- One logical change per branch. Keep PRs small and reviewable.

### Branch naming

Use a type prefix + short kebab-case slug:

| Prefix    | Use for                              | Example                        |
|-----------|--------------------------------------|--------------------------------|
| `feat/`   | New user-facing functionality        | `feat/techdocs-search`         |
| `fix/`    | Bug fix                              | `fix/auth-redirect-loop`       |
| `chore/`  | Build, deps, tooling, non-user       | `chore/bump-backstage-1.51`    |
| `docs/`   | Docs-only                            | `docs/onboarding-guide`        |
| `refactor/` | Internal restructuring, no behaviour | `refactor/catalog-processor` |
| `test/`   | Tests only                           | `test/scaffolder-actions`      |
| `ci/`     | CI / workflow changes                | `ci/add-trivy-severity-gate`   |

### Commit messages

[Conventional Commits](https://www.conventionalcommits.org/):

```
<type>(<scope>): <subject>

<body — optional, explains *why*>
```

Types match the branch prefix. Keep the subject ≤ 72 chars, imperative mood.

### Pull request flow

1. Branch off `main`: `git switch -c feat/<slug>`.
2. Commit and push the branch.
3. Open a PR targeting `main`. Fill the PR body with **what** changed and **why**.
4. CI must pass (Build & Audit, Scan & Push).
5. Resolve all review conversations before merge.
6. **Squash merge** only. The branch is deleted automatically on merge.
7. Rebase on `main` before merging if the branch is behind (linear history required).

### Release tags

Production releases are cut as annotated tags `vMAJOR.MINOR.PATCH` on `main`.
CI builds and pushes the image on tag push.

## Running locally

```bash
yarn install --immutable
yarn start       # frontend + backend
yarn tsc         # type check
yarn lint:all    # lint
yarn build:backend
```
