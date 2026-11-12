---
name: codex
description: Use when the user asks to run or resume OpenAI Codex CLI for code analysis, refactoring, implementation, testing, or automated editing. Choose safe execution settings, explain the planned command, and report verified results.
---

# Codex CLI Workflow

Use the Codex CLI to delegate software engineering tasks while keeping the user in control of scope, permissions, and consequential changes.

## 1. Confirm the environment

- Check that Codex CLI is installed with `codex --version`.
- Inspect the current directory and repository state before running tasks.
- Check the installed CLI's help output for current options. Do not assume a flag or model identifier exists because it appears in an old example.
- Confirm the target repository and working directory.

## 2. Select an execution mode

Choose the least permissive sandbox that can complete the task:

| Task | Suggested mode |
| --- | --- |
| Analysis, explanation, code review | `--sandbox read-only` |
| Changes confined to the checked-out workspace | `--sandbox workspace-write` |
| Network access or broad machine access | Only when required and explicitly approved |

Do not default to unrestricted execution. Do not silently add full-auto, bypass, or broad-access flags. Explain why elevated permissions are needed and obtain approval before using them.

## 3. Build the command

- Use the installed CLI's documented syntax.
- Set a working directory explicitly when needed.
- Select model and reasoning settings only when supported by the installed version or requested by the user.
- Prefer passing task instructions through standard input for multiline prompts.
- Capture standard output and standard error for diagnosis. Do not suppress errors by default.
- Avoid exposing secrets in command arguments, logs, or transcripts.

Example read-only task:

```bash
codex exec --sandbox read-only "Review the authentication flow and identify likely defects."
```

Workspace editing example, only after confirming the intended scope:

```bash
codex exec --sandbox workspace-write "Implement the requested fix and run the relevant tests."
```

These examples are templates. Check `codex exec --help` and the installed version before execution.

## 4. Resume a session

- Inspect `codex exec resume --help` before choosing resume options.
- Identify the intended session before resuming if more than one session may be relevant.
- Preserve the original session settings unless the user explicitly requests a change.
- Send the new task as a clear prompt and report which session was resumed.

## 5. Verify changes

After Codex finishes:

1. Inspect `git status` and the complete diff.
2. Check that only intended files changed.
3. Review generated code for correctness, security, and maintainability.
4. Run relevant tests, lint, and build commands.
5. Report failed or skipped checks honestly.
6. Do not commit, push, deploy, or publish unless explicitly requested.

## Error handling

If the CLI is unavailable, authentication fails, a command exits non-zero, or results are incomplete, stop and report the exact issue. Retry only after diagnosing the cause or receiving user direction.

## Final report

Summarize the task, files changed, checks run, unresolved issues, and whether any action remains. Distinguish Codex's claims from changes independently verified in the repository.
