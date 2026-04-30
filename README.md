# Codex Pro Skill

This repository packages the `pro` Codex skill as a one-skill Codex plugin. The skill helps Codex send scoped, redacted context packets to Extended Pro through the Codex in-app browser, wait for the answer, and reconcile that answer against local files, PR state, diffs, tests, and repo conventions before acting.

## Requirements

- Codex with plugin and skill support.
- Browser Use / Codex in-app browser availability.
- Access to the Pro or Extended Pro surface used by the skill.

Unsupported environments cannot run the core workflow because the skill explicitly blocks fallback browser mechanisms.

## Install

To make this plugin available in Codex, add this repository as a marketplace source:

```bash
codex plugin marketplace add christianaranda/codex-pro-skill --ref v0.1.1
```

This registers the `Codex Pro Skill` marketplace for your local Codex user on this machine. It does not publish anything globally and does not install the plugin by itself.

Then open Codex Desktop's Plugins page, select the `Codex Pro Skill` marketplace, and install `Pro Review Orchestration`.

If you previously added `v0.1.0`, remove and re-add the marketplace so Codex uses the corrected package layout:

```bash
codex plugin marketplace remove codex-pro-skill
codex plugin marketplace add christianaranda/codex-pro-skill --ref v0.1.1
```

## Usage

```text
Use $pro to review this implementation plan before coding.
```

The skill supports plan hardening, implementation review, PR/code-review comment resolution, and eval/reporting methodology review.

## Privacy

The skill instructs Codex to build scoped context packets and redact secrets, tokens, credentials, private customer data, and unrelated large files. It does not technically prevent disclosure. Anything included in the prompt sent to Pro/OpenAI may be processed by that service, so review context packets before submission and do not include secrets or unrelated private data.

## Advanced

For local testing from a clone:

```bash
codex plugin marketplace add /path/to/codex-pro-skill
```

For manual skill-only installation, copy only the skill folder:

```bash
mkdir -p "$HOME/.agents/skills"
cp -R plugins/codex-pro-skill/skills/pro "$HOME/.agents/skills/pro"
```

## Maintainers

Release and scrub-check instructions are in `CONTRIBUTING.md`.
