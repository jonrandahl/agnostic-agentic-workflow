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

## 5. Verification

Choose evidence that matches the change and its risk rather than applying every
check to every change:

- When runtime behaviour matters, run the real app and exercise the changed
  flow where practical. Use end-to-end tests for important user journeys when
  they add stronger evidence; retain focused tests for faster, narrower checks.
- Use property-based tests when broad input ranges or invariants make them
  useful. For rewrites and migrations, compare old and new behaviour against
  the same representative cases and identify intentional differences.
- Protect the validation signal. Do not weaken, skip, or remove a check just to
  get a pass. When behaviour changes require test or check updates, preserve
  assertions for unchanged requirements and review those updates as part of
  the change.
- Run the cheapest relevant checks first, then broader or slower checks. Do not
  retry a flaky check just to obtain a pass: diagnose and fix it, or replace or
  remove it deliberately while preserving any still-needed coverage.
- Stop after three unsuccessful repair attempts for the same failure and
  report what remains unresolved.
- When a failure exposes a reusable process gap, add or revise a focused rule
  in the right place. Avoid turning one-off incidents into global guidance,
  and remove rules that are stale or superseded.

---

*Last updated: November 2026*
