# angular-renovate-demo

Demo Angular **22** app using the esbuild-based **`@angular/build`** application builder, with **self-hosted Renovate** via GitHub Actions (not the Mend/Renovate GitHub App).

## Stack

- Angular CLI / framework **22.x**
- Builder: `@angular/build:application` (see `angular.json`)
- Package manager: npm
- Renovate: `renovatebot/github-action` on a schedule (`workflow_dispatch` supported)

## Development

```bash
npm install
npm start
```

Build:

```bash
npm run build
```

## Self-hosted Renovate

| Piece | Path |
| --- | --- |
| Workflow | [`.github/workflows/renovate.yml`](.github/workflows/renovate.yml) |
| Repo config | [`renovate.json`](renovate.json) |

The workflow runs Renovate in GitHub Actions with `renovatebot/github-action`. Repository rules (npm manager, Monday schedule, Angular package grouping) live in `renovate.json`. PR branches use the `renovate-selfhosted/` prefix so they do not collide with the hosted Renovate app.

### One-time secret (required)

Add a repository (or org) Actions secret named **`RENOVATE_TOKEN`**.

Create a GitHub Personal Access Token (classic or fine-grained) for a bot/user that can open PRs on this repo, with at least:

| Permission | Why |
| --- | --- |
| `contents:read` | Read dependency manifests and clone |
| `metadata:read` | Repository metadata |
| `issues:write` | Dependency Dashboard / issue updates |
| `pull-requests:write` | Open and update Renovate PRs |
| `workflow` (classic) / Actions read if needed | Only if Renovate must update workflow files |

**Do not use `GITHUB_TOKEN` alone** for this action — Renovate needs a PAT (or GitHub App token) that can create PRs with workflows and trigger checks as intended.

After the secret exists, run **Actions → Renovate → Run workflow**, or wait for the Monday schedule.

## Notes

This repository is intended for Renovate / Angular upgrade demos. It is not production application code.
