---
name: pro
description: Orchestrate high-context reviews by sending a scoped repo context packet to Extended Pro in the Codex in-app browser, waiting for its answer, and reconciling the answer against current local repo, PR, diff, and test state. Use when the user asks to use /pro, Extended Pro, pro review, plan hardening, implementation review, PR/code-review comment resolution, eval/reporting methodology review, or asks for an external second-pass review before coding or shipping.
---

# Pro Review Orchestration

Use this skill to ask Extended Pro for a scoped review, then verify and reconcile its answer before taking action.

## Non-Negotiables

- Use the Codex in-app browser / Browser Use tools for the Pro interaction.
- Do not use Computer Use, Playwright, or macOS `open` for the Pro interaction.
- Prefer Extended Pro. Verify the visible model picker when possible.
- Do not blindly accept Pro output. Compare it to current local files, live PR state, tests, and repo patterns.
- If Pro raises architecture or product tradeoffs, ask the user before implementing those changes.
- Preserve traceability by summarizing the Pro prompt and response in the thread. Save a temporary artifact only when it is useful for a long or high-stakes review.

If the Browser Use tools are unavailable, say that the skill is blocked instead of substituting another browser mechanism.

## Workflow

1. Define the review target.
   - Identify whether this is plan hardening, implementation review, PR/code-review comment resolution, or eval/reporting methodology review.
   - Write the exact decision Pro is being asked to review.

2. Build a scoped context packet from local and live state.
   - Gather branch name, base branch, PR number or URL if any, and relevant issue links.
   - Gather the diff, changed-file list, key changed files, plan docs, test results, known failures, and open questions.
   - Include repo-specific constraints, existing patterns, and commands already run.
   - Redact secrets, tokens, credentials, private customer data, and unrelated large files.
   - Keep the packet scoped: include enough evidence for Pro to reason, but do not paste the whole repo.

3. Select the prompt template.
   - Use "Plan Hardening" for strategy, phases, sequencing, and missing decisions.
   - Use "Implementation Review" for code diffs, tests, edge cases, and ship readiness.
   - Use "PR / Code-Review Comment Resolution" for resolved review threads or CodeRabbit cleanup.
   - Use "Eval / Reporting Methodology" for metrics, comparability, reporting validity, and recommendation criteria.

4. Submit through the Codex browser.
   - Use a Pro thread specified by the user when provided.
   - Otherwise prefer reusing the existing Pro thread for the same repo or workstream when it preserves useful context.
   - Start a new Pro thread when the review target is unrelated, the prior thread is stale/noisy, or the user asks for a clean review.
   - When reusing a thread, briefly restate the current decision, what changed since the last review, and which prior assumptions should still apply.
   - Open or reuse the Codex in-app browser.
   - Confirm Extended Pro in the visible model picker when possible.
   - Send the context packet and selected prompt.

5. Wait patiently.
   - Poll the browser response until completion.
   - Do not give up just because it takes 10-15 minutes.
   - If the browser is interrupted or authentication is required, report the specific blocker and wait for user direction.

6. Reconcile the response.
   - Verify each Pro claim against current files, diff, PR state, tests, and repo conventions.
   - Classify each actionable item as `FIX`, `DEFER`, `DISMISS`, or `QUESTION`.
   - For `FIX`, implement only when the change is clearly within scope and does not require product or architecture approval.
   - For `DEFER`, explain why it is outside the current scope or better handled later.
   - For `DISMISS`, state the concrete evidence that makes the feedback incorrect or already handled.
   - For `QUESTION`, ask the user a direct question before proceeding.

7. Report back.
   - Include Pro's overall result: `SIGNED OFF` or `BLOCKED`.
   - Summarize required changes, risks, and tests.
   - Note what was implemented, deferred, dismissed, or left as a question.
   - Include the verification commands run locally.

## Output Contract For Pro

Ask Pro to respond in this shape:

```text
Verdict: SIGNED OFF | BLOCKED

Required changes:
- ...

Risks:
- ...

Tests or verification:
- ...

Reasoning notes:
- ...
```

`SIGNED OFF` means Pro sees no blocking issue for the stated decision. `BLOCKED` means Pro found at least one required change or unresolved question that should stop the decision.

## Prompt Templates

### Plan Hardening

```text
You are Extended Pro reviewing this plan before implementation.

Decision to review:
[exact decision]

Context packet:
[branch, PR/issue links, plan docs, relevant repo constraints, open questions]

Please answer using the required output contract:
- Verdict: SIGNED OFF or BLOCKED
- Required changes
- Risks
- Tests or verification
- Reasoning notes

Focus on whether the plan actually reaches the stated workflow goal, which decisions are missing, and which assumptions are weak.
```

### Implementation Review

```text
You are Extended Pro reviewing this implementation for ship readiness.

Decision to review:
[exact decision]

Context packet:
[branch, PR/issue links, diff summary, changed files, key code snippets, tests run, known failures, repo patterns]

Please answer using the required output contract:
- Verdict: SIGNED OFF or BLOCKED
- Required changes
- Risks
- Tests or verification
- Reasoning notes

Focus on correctness, edge cases, regressions, missing tests, and whether the diff is shippable for the stated scope.
```

### PR / Code-Review Comment Resolution

```text
You are Extended Pro reviewing whether PR/code-review comments were resolved correctly.

Decision to review:
[exact decision]

Context packet:
[PR URL, branch, review comments, responses/fixes, relevant diffs, tests run, unresolved questions]

Please answer using the required output contract:
- Verdict: SIGNED OFF or BLOCKED
- Required changes
- Risks
- Tests or verification
- Reasoning notes

Focus on whether any response or fix is weak, incomplete, inconsistent with the repo, or likely to fail review.
```

### Eval / Reporting Methodology

```text
You are Extended Pro reviewing this evaluation or reporting methodology.

Decision to review:
[exact decision]

Context packet:
[metrics, datasets or reports, methodology, assumptions, comparison criteria, known limitations, relevant code or queries]

Please answer using the required output contract:
- Verdict: SIGNED OFF or BLOCKED
- Required changes
- Risks
- Tests or verification
- Reasoning notes

Focus on whether the metrics are comparable enough to support the recommendation, which confounders remain, and what evidence is missing.
```

## Reconciliation Report Shape

After reconciling Pro's answer, report in this format:

```text
Pro verdict: SIGNED OFF | BLOCKED

Reconciled actions:
- FIX: ...
- DEFER: ...
- DISMISS: ...
- QUESTION: ...

Local verification:
- ...

Traceability:
- Prompt summary: ...
- Response summary: ...
```
