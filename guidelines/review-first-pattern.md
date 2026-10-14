# Review-First Pattern for Code Changes

## Overview

Use a review-first preview when seeing the proposed approach before implementation
would materially help the user assess scope, trade-offs, or impact. A preview is
a communication aid, not a general approval gate.

Keep action authorisation separate from review-first. A preview never grants
permission to act. Follow the applicable user instructions and tool or platform
permission rules for every action. Destructive or hard-to-reverse actions,
external side effects, and actions that cross a security or data boundary need
explicit authorisation for that action and scope unless the user has already
clearly authorised it. Do not infer authorisation from silence or from approval
of a different proposal.

## When a Preview Helps

- Architectural decisions or breaking changes with meaningful trade-offs
- Broad changes across multiple files or interconnected systems
- Changes whose impact or rollback cost would be difficult to assess from a
  concise completion report
- Situations where the user asks to compare or review approaches before choosing

Offer the preview with the scope, affected areas, expected impact, and material
trade-offs. Ask for a decision only when the user needs to choose an approach or
the action requires authorisation. If the user has already requested a clear,
reversible change, implement it without adding an approval step solely because it
is complex.

## When to Skip a Preview

- Trivial, local, reversible changes
- A clear implementation request where no material choice remains
- Code samples or examples in chat that do not change workspace state
- File reads or exploration that gather context without changing state

Skipping a preview does not bypass action authorisation or other safety rules.

## The Workflow

### 1. Diagnosis

Read relevant files and understand the root cause. Be thorough—don't guess.

### 2. Show, Don't Tell

Present the fix as a clear code diff or structured explanation:

```
CHANGE: All 4 pen article index.html files
- aria-labelledby="pen-heading"
+ aria-labelledby="article-pen-heading"
```

Include:

- **What** is changing and **why**
- **Where** the changes apply
- **How** it fixes the core issue (not just symptoms)

### 3. Explain the Trade-Off

Be honest about trade-offs and why this approach is better:

- "This solves [X] by avoiding [Y], which means we don't need the [Z] cascade
  hack anymore"
- "This renames a convention, which affects [these files], but makes [this]
  clearer"

### 4. Establish the Next Step

If the user needs to choose between materially different approaches, ask which
one they want. If the proposed action requires explicit authorisation, request
that authorisation for the specific action and scope. Otherwise, continue with
the requested work. A request to review a proposal is not, by itself,
authorisation to carry it out.

Treat follow-up questions or changed requirements as new information: answer or
revise the proposal, then proceed only when the request and any required
authorisation are clear.

### 5. Implement All Changes Together

Use multi_replace_string_in_file for multiple independent edits (cost/time
efficient).

Apply changes in this order:

1. Renames/breaking changes first
2. Then dependent removals
3. Finally new rules that depend on the above

### 6. Verify & Report

Run checks appropriate to the change and the project. Report what was checked,
what passed or failed, and any unavailable checks or unresolved risks. Do not
claim a check passed unless it was run.

## Benefits

| Benefit | Why It Works |
|---------|--------------|
| **Catches misunderstandings early** | User sees the approach before code is changed |
| **Saves thrashing** | Prevents "I didn't want this" → revert → try again cycles |
| **Makes reasoning transparent** | User understands *why* this fix works (not just that it does) |
| **Builds trust** | "You showed me the code before changing it" = predictable collaboration |
| **Enables better feedback** | User can suggest tweaks before implementation |

## Example Scenarios

### Scenario 1: CSS Cascade Issue

```sh
User: "This hover state isn't suppressing in embed mode"

[AGENT DIAGNOSIS]
→ Root cause: wrong selector matching wrong element

[REVIEW-FIRST]
"The card selector matches both home-page cards and pen article pages. I propose
scoping the article selector to avoid the collision. This affects these files:
[show scope and diff]. The change is reversible. I’ll proceed with the requested
fix unless you want a different selector strategy."

[IMPLEMENT]
→ Rename aria-labelledby, remove cascade hack, add new rule
→ Verify no errors
→ ✅ Done
```

### Scenario 2: Breaking Change to Naming

```sh
User: "How do I standardise our component prop names?"

[REVIEW-FIRST]
"I found 12 components using inconsistent prop naming. Here's the mapping I
recommend:
[show before/after table]
This touches these files and introduces a breaking change:
[list files and impact]. Which naming convention would you choose? I can
implement it once you ask me to make the change."

User chooses a convention

[AGENT]
→ Explain the selected convention and confirm the implementation scope
→ Wait for an implementation request before changing files
```

### Scenario 3: Trivial Fix (Skip Review-First)

```sh
User: "There's a typo in button.css line 42"

[AGENT]
→ Read file, spot typo
→ Fix directly (no diagram needed)
→ Report: "✅ Fixed typo: 'paading' → 'padding'"
```

## Guidelines for Presenting Changes

### Clarity First

- Use diff format for code changes
- Use bullet lists for structural changes
- Use tables for before/after comparisons
- Include file paths (relative to workspace)

### Be Concise

- 3–5 lines of context around each change (not the whole file)
- Focus on the *why*, not the mechanism
- Avoid "here's my reasoning process" — give conclusions

### Invite Feedback When a Decision Is Needed

- Ask a focused question when the user needs to choose an approach or provide
  required authorisation
- Do not ask for approval merely to create a pause before a clear, authorised,
  reversible change
- State whether you are proceeding or waiting, and why

## Recovery if the Change Is Wrong

If you implement first and the user objects:

- Stop further changes and inspect the objection and current state
- Preserve user changes and any work that should not be discarded
- Explain the impact and offer the smallest safe correction
- Revert only when that is the right correction and it will not discard unrelated
  or user-owned work
- Follow the same authorisation boundary before any action that requires it

Example:

```sh
User: "Wait, this broke X"

[AGENT]
→ Stop and inspect the current diff and the dependency
→ "I missed that X depends on the old naming. Reverting the rename would also
  undo [impact]. I propose [safe correction]."
→ Proceed with the correction if already requested and authorised; otherwise
  ask for the needed choice or authorisation
```

## Adoption Notes

- Use judgment: preview when it improves the user's decision, not as ceremony
- Keep proposal review distinct from permission to perform an action
- Adjust for team speed: some teams prefer inline chat snippets, others want
  formal diffs
- Document exceptions for your team (e.g., "automated tests always OK to fix")
