# Contributing to This Project

**Read this before writing your first line of code.**
This is not optional. The gates described here are enforced automatically.

---

## The 5 Gates

Every piece of work passes through 5 gates. Skipping a gate blocks the next one.

```
Gate 1: Intent     → Is the spec approved before you branch?
Gate 2: Generation → Are you owning what Copilot generates?
Gate 3: PR Review  → Has the PR been pre-screened by AI + peer?
Gate 4: CI         → Do all automated checks pass?
Gate 5: Deploy     → Has it been staged and monitored?
```

---

## Gate 1 — Intent (Before You Write Code)

1. **Find or create a GitHub Issue** using the Feature Request or Bug Report template.
2. **Fill in every field.** Acceptance criteria must be specific enough that "done" is unambiguous.
3. **Wait for spec approval.** The tech lead will comment "approved" on the issue. No branch before approval.
4. **Create your branch** from `main`:
   ```
   git checkout -b feat/issue-42-csv-export
   ```
   Branch naming: `feat/`, `fix/`, `refactor/`, `docs/` + issue number + short description.

> Why: AI generates the wrong thing perfectly if the spec is vague.
> Catching ambiguity at Gate 1 costs 10 minutes. Catching it at Gate 3 costs 2 hours.

---

## Gate 2 — Generation (While You Code)

### Using GitHub Copilot
- **You own every line Copilot writes.** "Copilot wrote it" is not an excuse for a bug.
- **Write the function signature yourself**, then let Copilot suggest the body.
- **Read every suggestion before accepting.** No "accept all" without reading.
- **Commit in small, logical chunks.** Never one giant "implement feature" commit.
- AI-generated code must pass the same standards as hand-written code.

### Commit discipline
```
feat(export): add CSV serialisation for dashboard data

Closes #42

Used streams to avoid loading entire dataset into memory.
Handles empty data set by returning 204 rather than empty CSV.
```

Commits must explain WHY, not just WHAT (the diff already shows what).

---

## Gate 3 — PR Review

### Opening a PR
- PR description must include `Closes #NNN` (links the spec, required by CI).
- PR must be under **400 lines changed** (excludes lock files, snapshots, generated files).
  If your feature is larger, split into: schema → logic → UI.
- Fill in the PR template fully.

### Getting an AI pre-screen
Comment `/ai-review` on your own PR. Claude will post a review within ~2 minutes.
Fix any 🔴 issues before requesting human review. This is mandatory.

### Human review
- Assign the tech lead (and a peer if the team is > 6 people).
- Address all comments, or explicitly explain why you're not.
- Do not merge your own PR.

---

## Gate 4 — CI (Automated, Blocks Merge)

These checks run automatically and **block merge if they fail**:

| Check | What it catches |
|---|---|
| ESLint | Style violations, obvious errors |
| TypeScript | Type errors |
| Tests + Coverage | Broken behaviour, coverage below 70% |
| PR size | PRs over 400 lines |
| Security scan | Known vulnerabilities, hardcoded secrets |
| Issue link | PRs not linked to a spec |

If CI fails: fix it. Do not ask for exceptions. Do not disable the check.

---

## Gate 5 — Deploy

1. Merge to `main` triggers deploy to **staging** automatically.
2. Test your feature on staging.
3. Monitor for 24 hours (check Sentry if configured).
4. Tech lead promotes to production.

---

## ADR — Architecture Decision Records

Any decision that:
- affects more than one file, **or**
- will be hard to reverse

...needs an ADR in `/docs/adr/` **before the code is written**.
Use the template at `/docs/adr/TEMPLATE.md`.

---

## Day 1 Checklist for New Engineers

- [ ] Read this file completely
- [ ] Clone the repo, run `npm install`, run `npm test` — everything green?
- [ ] Read `.github/copilot-instructions.md`
- [ ] Read the most recent ADR in `/docs/adr/`
- [ ] Pair with a teammate on your first PR (even if small)
- [ ] Ask questions in the team channel, not in PRs

---

## What happens if you skip a gate?

- **Gate 1**: PR will be closed and you'll be asked to write the spec first.
- **Gate 2**: Bugs traced back to unread Copilot output are a learning moment, documented.
- **Gate 3**: PRs without `/ai-review` and without 🔴 items addressed will not be approved.
- **Gate 4**: CI blocks the merge. Period.
- **Gate 5**: Deploying to prod without staging sign-off is a team norm violation.

This isn't bureaucracy — it's how we ship reliably with a team that uses AI heavily.
