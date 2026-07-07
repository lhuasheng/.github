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
| `.github/ISSUE_TEMPLATE/*.yml` | Default issue templates for every repo (Bug, Feature, Incident) |
| `pull_request_template.md` | Default PR checklist for every repo |

Per-project overrides live in each project repo's own `.github/` directory.
A project's local file always wins over the org default.

Note: `.github/workflows/*.md` is a different thing entirely — those are
agentic workflow definitions, not propagated defaults. Nothing inherits them
automatically; they only run when explicitly dispatched. See below.

## What does **not** live here

Deterministic gate logic (CI checks, PR-size, label sync, on-demand AI review)
lives in [`lhuasheng/shared-sdlc`](../shared-sdlc) as reusable composite
actions. Project repos call those actions from thin caller workflows copied
out of `shared-sdlc/templates/`.

## Agentic workflows also live here

Reasoning-heavy automation (issue triage, weekly digest, release notes,
architecture/vuln review) runs as **GitHub Agentic Workflows** — Markdown
files under [`.github/workflows/`](.github/workflows/) in *this same repo*.
The PRD originally proposed a separate `shared-agentic` repository for these;
that plan was dropped in favor of hosting them here, since every project repo
already depends on `.github` for org defaults and a fourth shared repo wasn't
worth the overhead.

`shared-sdlc`'s `dispatch-agentic` action routes commands and triggers (like
`/ai-review` or a tag push) to these workflow files via the GitHub Actions
dispatch API, pointing `workflow-repo` at `lhuasheng/.github`. This requires
GitHub Copilot's agentic workflow feature to be enabled for the org (Settings
→ Copilot → Policies) — until then, `/ai-review` falls back to the legacy
Anthropic-API script path built into `shared-sdlc`'s `ai-pr-review` action.

We keep gate logic in one place so a change (say, the PR-size threshold)
rolls out everywhere without touching every repo.

## Multi-repo map

```
lhuasheng/
├── .github/         ← you are here (org defaults + agentic workflows in .github/workflows/)
├── shared-sdlc/     ← composite actions + scripts (deterministic gates)
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
