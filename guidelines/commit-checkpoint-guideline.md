# Commit Checkpoint Guideline — All Agents

## Purpose

Prevent unrecorded progress by prompting for optional commit checkpoints at
sensible points in the workflow. Agentic work can span many hours and many
changes. Without checkpoints, work becomes a monolithic commit with no
intermediate history. Checkpoints allow agents to preserve progress
transparently whilst remaining non-blocking.

## Why This Matters

- **Preserves progress transparency.** Each checkpoint is a retrievable moment
  in history.
- **Prevents catastrophic loss.** If a session ends unexpectedly, progress is
  not lost to uncommitted work.
- **Enables better reviews.** Reviewers see incremental steps instead of a wall
  of changes.
- **Non-blocking workflow.** Agents never force commits; users control timing.
- **Builds trust.** Visible checkpoints show that agentic work is tracked and
  reversible.

## Trigger points

- Before starting new implementation work, check for uncommitted changes.
- After a meaningful unit of work, check again.
- Before switching tasks, check again.
- Before ending a session, check again.

## Ticket completion boundary

After a ticket has been validated and its commit or pull request is ready,
record a concise completion checkpoint before starting the next independent
ticket. Compact the session at that boundary when the next task does not
depend on the current investigation.

Do not compact during an active debugging loop, between a failing check and
its repair, or when continuity is required to finish the current ticket.

## Required behaviour

- Detect whether changes exist at file level and project level.
- If changes exist, generate a commit proposal that follows project commit
  rules.
- Keep the prompt optional and non-blocking.
- Continue work immediately if the user declines.

## Prompt format

Use this exact structure:

```
Uncommitted changes detected in [N] files.
Suggested commit message (optional):

type(scope): subject

- Did X to achieve Y
- Improved Z to reduce A

Commit now? (yes/no)
This is an optional checkpoint. You can ignore this and continue batching changes.
```

## User responses

- yes: stage relevant files and commit with the proposed message (or edited
  message).
- no: continue work with no friction.
- If unclear: ask one short follow-up, then proceed.

## Commit message rules

- Conventional Commits: type(scope): subject
- Subject in imperative mood, 50 chars max
- British English spelling
- Body bullets prefixed with `-`
- Explain what and why, not low-level implementation detail
- One logical change per commit proposal

## Mixed-change rule

If unrelated changes are present, offer split commit proposals and ask:

```
Split into separate commits? (yes/no)
```

If no, continue with one commit proposal and warn that rollback will be less
precise.

## Safety rule

Never force a commit. Checkpoint prompts are advisory only.

---

*Last updated: May 2026*
