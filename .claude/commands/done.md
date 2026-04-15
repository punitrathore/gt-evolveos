---
description: Signal work complete using the active delivery workflow
allowed-tools: Bash(gt done:*), Bash(git status:*), Bash(git log:*), Bash(git add:*), Bash(git commit:*), Bash(git push:*), Bash(bd close:*), Bash(gh pr view:*), Bash(gh pr create:*)
argument-hint: [--status COMPLETED|ESCALATED|DEFERRED] [--pre-verified]
---

# Done — Complete Delivery Workflow

Signal that your work is complete and ready for delivery.

Arguments: $ARGUMENTS

## Pre-flight Checks

Before running `gt done`, verify your work is ready:

```bash
git status                          # Must be clean (no uncommitted changes)
git log --oneline origin/main..HEAD # Must have at least 1 commit
```

If there are uncommitted changes, commit them first:
```bash
git add <files>
git commit -m "<type>: <description>"
```

## Execute

For PR-first repos, make sure the branch is pushed and a PR exists before `gt done --no-merge`:

```bash
git push origin HEAD -u
gh pr view --json number,url 2>/dev/null || gh pr create --fill
```

Then run `gt done` with any provided arguments:

```bash
gt done $ARGUMENTS
```

**Common usage:**
- `gt done` — Submit completed work using the default workflow
- `gt done --pre-verified` — Submit with pre-verification to the merge queue
- `gt done --no-merge` — Finish a PR-first workflow without merge-queue submission
- `gt done --status ESCALATED` — Signal blocker, skip MR
- `gt done --status DEFERRED` — Pause work, skip MR

**If the bead has nothing to implement** (already fixed, can't reproduce):
```bash
bd close <issue-id> --reason="no-changes: <brief explanation>"
gt done
```

This command completes the active delivery workflow. In merge-queue mode it
submits an MR to Refinery. In PR-first mode it records completion after the
PR is opened. You are done after this.
