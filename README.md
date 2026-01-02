
# AI Automation Template

This repository is a GitHub Template used to create AI-assisted automation projects with consistent standards.

## How to Use This Template

1. Click **Use this template**
2. Create a new repository
3. Populate `docs/requirements.md`
4. Use AI tools (Cursor, ChatGPT) anchored to `docs/ai/CONTEXT.md`

## Key Files

- `docs/ai/CONTEXT.md` – AI behavior standards
- `docs/requirements.md` – Behavioral contract
- `.github/*` – Workflow and governance enforcement

## AI Usage Pattern

Always instruct AI tools to:

- Follow `docs/ai/CONTEXT.md`
- Implement `docs/requirements.md`
- Update documentation
- Ensure CI passes

## Configuration, Secrets, and `.gitignore`

This repository uses a strict `.gitignore` policy to prevent secrets, runtime
state, and generated artifacts from being committed.

All contributors and AI tools must follow the standards defined in
`docs/ai/CONTEXT.md`.

### Environment Variables and Secrets

- Never commit secrets or environment-specific configuration
- Local configuration must be stored in `.env` or equivalent files
- Example files **must** be committed to document required variables

| File | Purpose | Committed |
|----|--------|----------|
| `.env` | Local runtime secrets | ❌ No |
| `.env.example` | Example/template for variables | ✅ Yes |
| `vars.env` | Alternative env file (discouraged) | ❌ No |
| `vars.env.example` | Example alternative | ✅ Yes |

If additional environment files are required (e.g. `.env.prod`),
they must remain uncommitted and be documented.

### Generated Files and Artifacts

The following are intentionally excluded from version control:

- Pre-commit and lint artifacts
- AI-generated logs
- Build and runtime output
- Virtual environments and caches

These files may be generated locally or in CI, but must never be committed.

### Why This Matters

- Prevents accidental secret disclosure
- Keeps the repository portable and clean
- Ensures consistent behavior across environments
- Allows AI tools to safely operate without leaking sensitive data

If you believe a file should be committed but is currently ignored,
update `.gitignore` deliberately and document the rationale.

## Pre-commit Checks and Log Review

This repository uses **pre-commit** to enforce formatting, linting, and safety
standards before changes are committed and merged.

### How Pre-commit Is Run

Pre-commit checks may run in two ways:

1. **Automatically during `git commit`**
   If pre-commit is installed locally, checks run automatically and may block
   the commit until issues are resolved.

2. **Manually via the helper script (recommended)**
   For full visibility and AI-assisted workflows, use:

   ```bash
   ./scripts/run-precommit.sh
