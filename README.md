# Codex Pro Skill

This repository packages the `pro` Codex skill as a one-skill Codex plugin. The skill helps Codex send scoped, redacted context packets to Extended Pro through the Codex in-app browser, wait for the answer, and reconcile that answer against local files, PR state, diffs, tests, and repo conventions before acting.

## Requirements

- Codex with plugin and skill support.
- Browser Use / Codex in-app browser availability.
- Access to the Pro or Extended Pro surface used by the skill.

Unsupported environments cannot run the core workflow because the skill explicitly blocks fallback browser mechanisms.

## Install

After this repository is public, add it as a plugin marketplace:

```bash
codex plugin marketplace add christianaranda/codex-pro-skill --ref v0.1.0
```

Then open the Codex plugin directory, select the `Codex Pro Skill` marketplace, and install `Pro Review Orchestration`.

For local testing from a clone:

```bash
codex plugin marketplace add /path/to/codex-pro-skill
```

For manual skill-only installation, copy only the skill folder:

```bash
mkdir -p "$HOME/.agents/skills"
cp -R skills/pro "$HOME/.agents/skills/pro"
```

## Usage

```text
Use $pro to review this implementation plan before coding.
```

The skill supports plan hardening, implementation review, PR/code-review comment resolution, and eval/reporting methodology review.

## Privacy

The skill instructs Codex to build scoped context packets and redact secrets, tokens, credentials, private customer data, and unrelated large files. It does not technically prevent disclosure. Anything included in the prompt sent to Pro/OpenAI may be processed by that service, so review context packets before submission and do not include secrets or unrelated private data.

## Maintainer Verification

Run these checks before committing or pushing:

```bash
find . -maxdepth 5 -type f | sort
git status --short
git diff --stat
python3 /path/to/skill-creator/scripts/quick_validate.py skills/pro
python3 - <<'PY'
import json
import yaml
from pathlib import Path

json.loads(Path(".codex-plugin/plugin.json").read_text())
yaml.safe_load(Path("skills/pro/agents/openai.yaml").read_text())
print("metadata OK")
PY
gitleaks detect --no-git --source . --redact --verbose
trufflehog filesystem . --no-update --fail
```

Also run any organization-specific regex scrub rules from a local pattern file, then inspect the file inventory manually for private repo names, branch names, issue links, internal URLs, screenshots, transcripts, logs, shell history, browser artifacts, or local Codex/agent scratch material.
