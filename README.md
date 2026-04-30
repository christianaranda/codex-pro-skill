# Codex Pro Skill

This repository packages the `pro` Codex skill for public installation. The skill helps Codex send scoped, redacted context packets to Extended Pro through the Codex in-app browser, wait for the answer, and reconcile that answer against local files, PR state, diffs, tests, and repo conventions before acting.

## Requirements

- Codex with skills support.
- Browser Use / Codex in-app browser availability.
- Access to the Pro or Extended Pro surface used by the skill.

Unsupported environments cannot run the core workflow because the skill explicitly blocks fallback browser mechanisms.

## Install

Recommended: ask Codex to install the skill with `$skill-installer`:

```text
Use $skill-installer to install https://github.com/christianaranda/codex-pro-skill/tree/v0.1.2/skills/pro
```

Restart Codex after installation so the new skill is discovered.

The installer places the skill under your Codex skills directory, typically `${CODEX_HOME}/skills/pro` when `CODEX_HOME` is set, or `~/.codex/skills/pro` otherwise.

To verify the install:

```bash
test -f "${CODEX_HOME:-$HOME/.codex}/skills/pro/SKILL.md" && echo "pro skill installed"
```

## Usage

```text
Use $pro to review this implementation plan before coding.
```

The skill supports plan hardening, implementation review, PR/code-review comment resolution, and eval/reporting methodology review.

## Privacy

The skill instructs Codex to build scoped context packets and redact secrets, tokens, credentials, private customer data, and unrelated large files. It does not technically prevent disclosure. Anything included in the prompt sent to Pro/OpenAI may be processed by that service, so review context packets before submission and do not include secrets or unrelated private data.

## Manual Install

If you do not want to use `$skill-installer`, clone the repo and copy only the skill folder:

```bash
git clone --depth 1 --branch v0.1.2 https://github.com/christianaranda/codex-pro-skill.git
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills"
cp -R codex-pro-skill/skills/pro "${CODEX_HOME:-$HOME/.codex}/skills/pro"
```

Restart Codex after copying the skill.

## Maintainers

Release and scrub-check instructions are in `CONTRIBUTING.md`.
