# PlaceMux Phase 1 — Task 2: Pre-Project Repository Architecture

**Repo:** https://github.com/shashankcodes-10/altrodev-task

---

## Part 1: The Task

**Objective:** Architect the repository — branching, structure, and conventions —
before the codebase grows, to prevent merge conflicts and inconsistent
contributions later.

**Deliverables required:**
- A documented repo architecture with branching strategy, protections, and
  contribution rules
- Demonstrable live on real data — not just described

**Key concepts covered:**
- Branching strategy (trunk-based or GitFlow, chosen deliberately)
- Repo structure (monorepo vs multi-repo, folders by concern)
- Conventions (commit messages, PR rules, code owners)
- Protected branches (gates that stop unreviewed code reaching main)
- `.gitignore` and hygiene (keeping junk and secrets out of history)

**Scoring (out of 100):**
| Parameter | Marks |
|---|---|
| Core deliverable — documented repo architecture | 50 |
| Real-data quality & correctness (real inputs, not a toy) | 20 |
| Live verification & evidence (demonstrated live, not claimed) | 15 |
| Dependency, failure & edge-case handling | 15 |

**Pitfalls to avoid:** no branch protection, inconsistent conventions, secrets
committed to history.

---

## Part 2: The Answer — What Was Built

### 1. Branching strategy: Trunk-based

`main` is always deployable. Short-lived branches (`feat/...`, `docs/...`,
`chore/...`) are cut from `main`, merged back via PR, then deleted.

**Why not GitFlow?** GitFlow's `develop`/`release`/`hotfix` branches exist to
coordinate multiple teams shipping scheduled releases with hotfix support. This
is a solo project with no such coordination problem, so trunk-based keeps the
same safety (protected `main`, PR gate) without the extra branch overhead.

### 2. Repo structure

```text
altrodev-task/
├── src/            # application code
├── config/         # environment/app configuration (non-secret)
├── scripts/        # setup, build, deploy helper scripts
├── docs/           # architecture docs and evidence
├── .github/
│   ├── PULL_REQUEST_TEMPLATE.md
│   └── CODEOWNERS
├── .gitignore
├── CONTRIBUTING.md
└── README.md
```

Single repo, structured by concern rather than split into multiple repos —
there's no infra-as-code yet to justify a separate repo. Revisit if/when
Terraform or similar is added with its own release lifecycle.

### 3. Conventions

- **Commits:** [Conventional Commits](https://www.conventionalcommits.org/) —
  `type(scope): summary`, e.g. `docs(day-2): add repo architecture notes`
- **PRs:** every merge to `main` uses `.github/PULL_REQUEST_TEMPLATE.md`,
  which requires a "how to verify" section — evidence, not claims
- **Ownership:** `.github/CODEOWNERS` — `@shashankcodes-10` owns everything for
  now, scoped per-folder if collaborators join

### 4. Protected branches

Set up via GitHub → Settings → Rules → Rulesets, targeting `main`:
- Enforcement status: **Active**
- Require a pull request before merging
- Restrict deletions
- Block force pushes

### 5. `.gitignore` & hygiene

Covers `.env`/`.env.*`, key/cert files (`*.pem`, `*.key`, `*.crt`), Terraform
state, and standard OS/editor/build junk.

### 6. Proof it actually works

Instead of relying on a settings screenshot alone, the protection was tested
directly — a commit was pushed straight to `main` and GitHub rejected it:

```
remote: error: GH013: Repository rule violations found for refs/heads/main.
remote: - Changes must be made through a pull request.
! [remote rejected] main -> main (push declined due to repository rule violations)
```

That rejection is the live evidence the rubric asks for.

### 7. How it was delivered

1. Created a new branch (`setup/repo-architecture`) off `main`
2. Added all the files above on that branch
3. Committed and pushed the branch
4. Opened a pull request into `main`, reviewed the diff
5. Merged the PR, deleted the branch
6. Set up and activated the branch ruleset
7. Verified the protection live via a rejected direct push
8. Documented everything here and in `docs/REPO-ARCHITECTURE.md`

---

## Final status — Definition of Done

| Requirement | Status |
|---|---|
| Branching strategy documented and justified | ✅ |
| Repo structure and ownership defined | ✅ |
| Commit/PR conventions and templates | ✅ |
| `main` branch protected | ✅ |
| Protection verified live (not just claimed) | ✅ |
| `.gitignore` in place | ✅ |
| Contribution workflow documented | ✅ |
