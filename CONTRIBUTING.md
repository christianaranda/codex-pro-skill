# Contributing

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

Do not add release or maintainer-only instructions to `skills/pro`; the installable skill folder should stay limited to the skill instructions and its UI metadata.
