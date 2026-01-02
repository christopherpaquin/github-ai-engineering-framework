# AI Engineering Context and Standards

This repository uses AI-assisted development.
All AI tools (Cursor, ChatGPT, Copilot, etc.) must follow this document.

If behavior is not explicitly allowed here or in docs/requirements.md,
it must not be implemented without clarification.

---

## 1. Design Principles

- Correctness over cleverness
- Explicit behavior over implicit assumptions
- Idempotent and safe by default
- Fail loudly, clearly, and early
- Prefer boring, maintainable solutions
- All code should be easily installed and uninstalled
- Adhere to Security best practices
- Adhere to SELinux best practices (semanage over chcon)

---

## 2. Supported Environments

- Primary OS: RHEL 9 / RHEL 10
- Secondary OS: Ubuntu 22.04 (best-effort)
- Shell: bash
- Python: 3.11+

Do not assume additional tools are installed unless explicitly documented.

---

## 3. Bash Standards

- Use `#!/usr/bin/env bash`
- Scripts must start with:
  - `set -euo pipefail`
- Quote all variables
- Never use `eval`
- Validate all inputs early
- Use functions; avoid large monolithic scripts
- Use `trap` for cleanup when modifying system state

---

## 4. Python Standards

- Prefer standard library
- Use type hints for public functions
- Avoid global mutable state
- Use structured logging
- External calls must have timeouts
- Avoid `shell=True`
- CLI tools must support `--help`

---

## 5. Yaml and Json Standards

Configuration and data files must follow consistent, predictable formatting
to ensure readability, tooling compatibility, and long-term maintainability.

### 5.1 File Naming and Extensions

- YAML files **must** use the `.yaml` extension
  - `.yml` is **not allowed**
- JSON files **must** use the `.json` extension
- File extensions must accurately reflect file contents

### 5.2 YAML Standards

- YAML files must be valid YAML 1.2
- Indentation:
  - Use **2 spaces**
  - Never use tabs
- Use block-style YAML where possible
- Avoid implicit typing when ambiguity may exist:
  - Quote strings that could be misinterpreted (e.g., `yes`, `no`, `on`, `off`)
- Use explicit `null` instead of empty values when intentional
- Keys should use `snake_case` unless an external specification requires otherwise
- Lists should be written in expanded form (one item per line)
- Comments should explain *why*, not *what*

Example:

```yaml
log_level: "info"
enable_feature: false
retry_count: 3
servers:
  - host: example1.local
    port: 443
  - host: example2.local
    port: 443

```

---

### 5.3 JSON Standards

- JSON must be strictly valid
  - No comments
  - No trailing commas
- Use **2-space indentation**
- Use UTF-8 encoding
- Keys should use `snake_case` unless constrained by an external API
- Arrays and objects should be formatted for readability, not compactness
- Values must use explicit types (`true`, `false`, `null`)

Example:

```json
{
  "log_level": "info",
  "enable_feature": false,
  "retry_count": 3,
  "servers": [
    {
      "host": "example1.local",
      "port": 443
    },
    {
      "host": "example2.local",
      "port": 443
    }
  ]
}
```

---

## 6. Idempotency and Safety

- Scripts must be safe to re-run
- Existing state must be detected, not overwritten blindly
- Partial failures must be handled
- Destructive actions must be explicit and documented
- Provide `--dry-run` where reasonable

---

## 7. Security

- Never log secrets
- credentails and IP addresses should never be hardcoded into scripts, should exist in .env file
- .env file should not be checked into any repo (.gitignore)
- An example .env file (.env.example) should be created and checked in via git
- Treat all inputs as untrusted
- Document required permissions
- Use least privilege
- Sanitize file paths and user input

---

## 8. Logging and Exit Codes

- Logs must be actionable and informative
- Logs should exist in /var/log
- Errors must explain what failed and why
- Exit codes:
  - 0: success
  - 1: general failure
  - 2: invalid usage or input
  - 3: missing dependency

---

## 9. Documentation Requirements

README must include:
- Overview
- Include "tested on" shields [https://img.shields.io/]
- Requirements / dependencies
- Include high level architecture overview
- Installation
- Uninstall steps
- Usage examples
- Configuration
- Troubleshooting
- Security notes
- License (Apache 2.0)

Operational tools must include docs/runbook.md.

---

## 10. Change Discipline

- Do not invent requirements
- If requirements are unclear, update docs/requirements.md first
- Implementation must map to acceptance criteria

## 11. AI Agent Operational Responsibilities

This section defines mandatory operational behavior for AI agents working
within this repository. These rules are authoritative.

### 11.1 File Handling and `.gitignore` Compliance

The `.gitignore` file is authoritative and must be strictly respected.

AI agents must:

- Read `.gitignore` before creating or modifying files
- Never create, move, or commit ignored files
- Never recommend committing ignored files
- Never log, echo, or persist secret values
- Use example files (e.g. `.env.example`) for documentation only

Violations of `.gitignore` rules are considered security defects.

---

### 11.2 Pre-commit Execution and Log Generation

Pre-commit is a mandatory quality gate.

AI agents must execute pre-commit using the repository helper script:

```bash
./scripts/run-precommit.sh
```
