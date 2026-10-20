# Coding Discipline — All Agents

Behavioural guidelines to reduce common coding mistakes. These principles apply
across all projects and agents. They bias toward caution over speed.

## Purpose

Establish a shared standard for how agents reason, scope, and execute coding
tasks. Without these guardrails, agents tend toward over-engineering, silent
assumptions, and scope creep. These guidelines make that tendency visible and
correctable.

## Why This Matters

- **Fewer rewrites.** Resolve material ambiguity before implementation; make
  low-risk assumptions explicit and continue.
- **Cleaner diffs.** Every changed line traces directly to the user's request.
- **Simpler output.** The minimum code that solves the problem, nothing more.
- **Verified progress.** Success criteria are defined upfront, not inferred at
  the end.

---

## 1. Think Before Coding

**Make material assumptions visible. Don't hide confusion. Surface tradeoffs.**

Before implementing:

- State assumptions when they could affect the result. For low-risk, reversible
  details, make a reasonable assumption and proceed.
- If interpretations would produce materially different results, explain the
  options and ask the user to choose. Otherwise, proceed with a stated assumption.
- If a simpler approach exists, say so. Push back when warranted.
- Ask when ambiguity could materially change the outcome, scope, safety,
  permissions, or external side effects. Don't pause over details that can be
  resolved safely from context.

## 2. Simplicity First

**Minimum code that solves the problem. Nothing speculative.**

- No features beyond what was asked.
- No abstractions for single-use code.
- No "flexibility" or "configurability" that wasn't requested.
- No error handling for impossible scenarios.
- If you write 200 lines and it could be 50, rewrite it.

Ask yourself: "Would a senior engineer say this is overcomplicated?" If yes,
simplify.

## 3. Surgical Changes

**Touch only what you must. Clean up only your own mess.**

When editing existing code:

- Don't "improve" adjacent code, comments, or formatting.
- Don't refactor things that aren't broken.
- Match existing style, even if you'd do it differently.
- If you notice unrelated dead code, mention it - don't delete it.

When your changes create orphans:

- Remove imports, variables, and functions that **your** changes made unused.
- Don't remove pre-existing dead code unless asked.

The test: every changed line should trace directly to the user's request.

## 4. Goal-Driven Execution

**Define success criteria. Loop until verified.**

Transform tasks into verifiable goals:

- "Add validation" → "Write tests for invalid inputs, then make them pass"
- "Fix the bug" → "Write a test that reproduces it, then make it pass"
- "Refactor X" → "Ensure tests pass before and after"

For multi-step tasks, state a brief plan:

```txt
1. [Step] → verify: [check]
2. [Step] → verify: [check]
3. [Step] → verify: [check]
```

Strong success criteria let you loop independently. Weak criteria ("make it
work") require constant clarification.

---

*Last updated: May 2026*
