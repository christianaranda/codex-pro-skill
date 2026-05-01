# Codex Pro Skill

`pro` is a Codex skill for getting a second pass from Extended Pro. It helps Codex prepare a scoped, redacted context packet, send it through the Codex in-app browser, wait for the response, and reconcile that response against the local repo before acting.

## Install

In Codex, ask:

```text
Use $skill-installer to install https://github.com/christianaranda/codex-pro-skill/tree/v0.1.3/skills/pro
```

Restart Codex after installation.

## Use

```text
Use $pro to review this implementation plan before coding.
```

The skill supports plan hardening, implementation review, PR/code-review comment resolution, and eval/reporting methodology review.

## Requirements

- Codex with skills support.
- Browser Use / Codex in-app browser availability.
- Access to the Pro or Extended Pro surface used by the skill.

## Privacy

The skill helps Codex redact context before sending it to Pro/OpenAI, but it cannot guarantee that sensitive information is removed. Review context packets before submission and do not include secrets or unrelated private data.

## Maintainers

Release and scrub-check instructions are in `CONTRIBUTING.md`.
