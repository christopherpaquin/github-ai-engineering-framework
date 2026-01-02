# pre-commit and CI: How This Repository Enforces Quality

This document explains how this repository uses:

- `.pre-commit-config.yaml`
- `.github/workflows/ci.yml`

to enforce consistent quality checks locally and in GitHub.

The goal is to catch issues early, prevent regressions, and make
AI-generated code safe and auditable.

---

## 1. High-level mental model (start here)

Think of this as **two layers of the same checks**:

| Layer        | Where it runs | When it runs                    | Purpose |
|-------------|---------------|----------------------------------|---------|
| pre-commit  | Your machine  | Before `git commit`              | Fast feedback, auto-fix issues |
| CI (GitHub) | GitHub        | On pull requests and pushes      | Enforced gate before merge |

**Key idea:**
The *same checks* run in both places. CI exists so checks **cannot be skipped**.

---

## 2. What `.pre-commit-config.yaml` does

### Purpose

`.pre-commit-config.yaml` defines a set of automated checks that run
*before a commit is created*.

These checks include:
- Formatting code
- Linting Python
- Linting Bash
- Validating Markdown
- Catching common mistakes (merge conflicts, private keys, etc.)

### How it works (step by step)

#### One-time setup (per developer machine)

```bash
pip install pre-commit
