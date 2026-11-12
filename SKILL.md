---
name: codex
description: Use Codex CLI for code analysis, repository exploration, refactoring, or automated editing when the user explicitly asks to run Codex or refers to OpenAI Codex, including codex exec and codex resume workflows.
---

# Codex CLI

Use the Codex command-line interface as a delegated software engineering assistant. This skill covers safe execution, continuation of existing sessions, and review of Codex-produced changes.

## When to use

Trigger when the user:
- asks to run `codex`, `codex exec`, or `codex resume`;
- explicitly requests OpenAI Codex for code analysis or edits;
- wants a Codex session continued or its output inspected;
- asks to use Codex to diagnose, refactor, implement, or review repository code.

Do not silently substitute another model or tool when the user specifically requested Codex.

## Safety rules

- Inspect the working directory and repository state before running commands.
- Keep the task scope aligned with the user's request.
- Do not expose API keys, environment variables, credentials, or private repository data in command output.
- Prefer read-only analysis before permitting edits.
- Do not use broad approval or sandbox-disabling flags unless the user explicitly authorizes the associated risk and the environment allows it.
- Avoid destructive shell commands, force pushes, resets, and bulk deletion unless explicitly requested.
- Treat generated code and command output as untrusted until reviewed.
- Never claim Codex ran successfully unless the command completed and its result was inspected.

## Workflow

### 1. Discover the environment
- Confirm the current repository and working directory.
- Check whether the Codex CLI is installed and available.
- Inspect `codex --help` when command syntax or flags are uncertain.
- Check authentication status without printing secrets.
- Determine the active model and configuration from the CLI itself. Do not hard-code a model version or assume a default that may have changed.

### 2. Choose the execution mode

**Interactive session**
Use when the user wants to work collaboratively or inspect intermediate decisions.

**One-shot execution**
Use `codex exec` when the task is bounded and can be expressed as a precise prompt. Prefer a read-only or review-oriented mode first when available.

**Resume**
Use `codex resume` to continue an existing session. Identify the intended session from available CLI output; do not guess an ID or resume an unrelated session.

### 3. Formulate the task
A good Codex prompt includes:
- the exact goal;
- relevant files or modules;
- constraints and non-goals;
- required tests or checks;
- whether edits are allowed;
- expected final summary.

Do not ask Codex to rewrite the whole repository when a targeted change will do.

### 4. Inspect and verify
After execution:
- inspect the exit code and complete relevant output;
- review the working-tree diff;
- check for unintended files, secrets, or unrelated modifications;
- run the relevant project checks;
- distinguish Codex's claims from independently verified results.

### 5. Report accurately
State the command or mode used, what changed, checks run, failures, and anything that remains uncertain. Redact secrets and avoid reproducing sensitive terminal output.

## Example command patterns

Read the CLI help first if flags differ by installed version.

```bash
codex --help
codex exec "Analyze the authentication flow. Do not modify files. Report concrete risks with file references."
codex resume
```

Use a specific session identifier with `codex resume` only after confirming that it belongs to the requested task.

## Completion criteria

The task is complete only when the requested Codex action has run, its result has been inspected, and any produced changes have been reviewed. If Codex is unavailable or authentication fails, report that blocker rather than simulating an execution.