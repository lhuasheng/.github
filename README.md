# lhuasheng/.github

This is the **org-wide defaults repo**. GitHub treats it specially: anything in
this repo is inherited automatically by every other repo in `lhuasheng` that
doesn't override it.

## What propagates from here

| File | What it does |
|---|---|
| `profile/README.md` | The public landing page at github.com/lhuasheng |
| `copilot-instructions.md` | Default Copilot rules for every repo |
| `CONTRIBUTING.md` | Default contributor guide (5 gates, workflow) |
| `ISSUE_TEMPLATE/*.md` | Default issue templates for every repo |
| `pull_request_template.md` | Default PR checklist for every repo |

Per-project overrides live in each project repo's own `.github/` directory.
A project's local file always wins over the org default.

## What does **not** live here

CI/CD logic and the AI-review automation live in
[`lhuasheng/shared-sdlc`](../shared-sdlc) as reusable composite actions. Project
repos call those actions from thin workflow files. We keep the gate logic in
one place so a change to (say) the PR-size threshold rolls out everywhere
without touching every repo.

## Multi-repo map

```
lhuasheng/
├── .github/         ← you are here (org defaults)
├── shared-sdlc/     ← composite actions + scripts (the gate logic)
├── project-1/       ← example product repo (thin caller workflows)
├── project-2/       ← future product
└── ...
```

## Other org docs in this repo

- `docs/adr/TEMPLATE.md` — Architecture Decision Record template
- `UNIFIED-ISSUE-TRACKING.md` — how cross-repo features are tracked on one board

## Updating an org-wide rule

1. Open a PR against this repo.
2. Tag the engineering channel — org-wide rule changes deserve a heads-up.
3. After merge, the rule is live for every repo on the next clone / next
   Copilot session.
