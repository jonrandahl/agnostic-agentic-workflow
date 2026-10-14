# Review-First Pattern for Code Changes

## Overview

For complex, architectural, or cascade-sensitive changes, present the proposed
code **before** implementing it. This catches misunderstandings early, saves
thrashing, and makes the reasoning transparent.

## When to Use Review-First

- ✅ Architectural decisions (selector renames, layer reorganisation, refactoring
  patterns)
- ✅ CSS cascade fixes or state management logic
- ✅ Changes affecting multiple files or interconnected systems
- ✅ User expresses frustration or asks for "brutal honesty" (signals rethinking
  is needed)
- ✅ Breaking change to naming conventions, data attributes, or component APIs
- ✅ Anything that would require reverting if wrong

## When to Skip Review-First

- ⏭️ Trivial fixes (typos, single-line logic, obvious bug corrections)
- ⏭️ Automated fixes after user approval ("make it so", "fix that", "go ahead")
- ⏭️ Code samples or examples in chat (no workspace edits)
- ⏭️ File reads or explorations (gathering context)

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

### 4. Wait for Approval

User says:

- "approved" → implement immediately
- "make it so" → proceed
- "yes" / "go ahead" → proceed
- Questions → revise and re-present

### 5. Implement All Changes Together

Use multi_replace_string_in_file for multiple independent edits (cost/time
efficient).

Apply changes in this order:

1. Renames/breaking changes first
2. Then dependent removals
3. Finally new rules that depend on the above

### 6. Verify & Report

Run get_errors on changed files and confirm they're clean. Report results
briefly: "✅ All 5 files updated cleanly. No errors."

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
"The card selector matches both home-page cards AND pen article pages.
Let me show you how to rename the article selector so they never match:
[show diff]
Approved?"

User: "approved"

[IMPLEMENT]
→ Rename aria-labelledby, remove cascade hack, add new rule
→ Verify no errors
→ ✅ Done
```

### Scenario 2: Breaking Change to Naming

```sh
User: "How do I standardise our component prop names?"

[REVIEW-FIRST]
"I found 12 components using inconsistent prop naming.
Here's the mapping I'd apply:
[show before/after table]
This will touch these files:
[list files]
OK to proceed?"

User: "looks good"

[IMPLEMENT]
→ Apply all renames together
→ Update related tests/docs
→ Verify
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

### Invite Feedback

- "Does this approach work for you?" or "Should I proceed?"
- "Any concerns?" or "Want me to adjust anything?"
- Make it clear you're waiting before acting

## Recovery if You Misjudged

If you implement first and the user objects:

- **Immediately revert** (don't defend the change)
- **Ask what went wrong** (fix your understanding)
- **Present the corrected approach**
- **Get approval before second attempt**

Example:

```sh
User: "Wait, this broke X"

[AGENT]
→ Revert immediately
→ "I missed that X depends on the old naming. Let me revise..."
→ [present corrected approach with better context]
→ "Ready to retry?"
```

## Adoption Notes

- This pattern is lightweight—not a rigid process, just a mindset
- Use judgment: trivial changes don't need ceremony
- Adjust for team speed: some teams prefer inline chat snippets, others want
  formal diffs
- Document exceptions for your team (e.g., "automated tests always OK to fix")
