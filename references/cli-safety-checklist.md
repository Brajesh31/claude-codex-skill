# Codex CLI Safety Checklist

## Before invocation
- [ ] Confirm the repository and working directory.
- [ ] Check `codex --version` and relevant command help.
- [ ] Inspect `git status --short` and existing user changes.
- [ ] Define task scope, expected output, and verification criteria.
- [ ] Select the least-permissive supported sandbox.

## Permission boundaries
- [ ] Use read-only mode for analysis and review where supported.
- [ ] Use workspace-write only for changes confined to the intended workspace.
- [ ] Obtain explicit approval for broader filesystem access, network access, destructive actions, commits, pushes, or deployment.
- [ ] Never pass secrets in prompts, command-line arguments, or logs.

## After invocation
- [ ] Inspect the actual diff and changed-file list.
- [ ] Independently verify important claims and generated changes.
- [ ] Run relevant tests and checks.
- [ ] Report exit status, checks, failures, and unresolved work.
