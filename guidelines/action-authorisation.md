# Action Authorisation and Least-Destructive Defaults

## Purpose

Define when an agent may act on a user request, when it must show a preview,
and when it must obtain explicit authorisation before execution. This policy is
agent-agnostic and supplements the review-first pattern.

## Core rule

Choose the least destructive action that satisfies the request. Treat a clear
user request as sufficient authorisation only for low-risk, local, reversible
work. Do not infer permission for a broader, external, destructive, or
hard-to-reverse action.

Review and authorisation are separate:

- A preview explains a proposed action but does not grant permission to perform
  it
- Authorisation applies to the specific action and scope
- Approval of one proposal does not authorise a different or expanded action
- Silence is not authorisation

## Action levels

| Level | Typical actions | Default |
|---|---|---|
| A: Read-only | Read files, search, inspect status, review history, run non-mutating analysis | Proceed |
| B: Local and reversible | Edit a clearly scoped file, add a test, update documentation, create a local branch | Proceed when clearly requested |
| C: Broad but reversible | Multi-file refactor, dependency update, reversible migration, shared configuration change | Show a preview; ask when scope or approach is ambiguous |
| D: External or consequential | Commit, push, create a pull request, update an issue, install packages, change shared configuration | Obtain explicit authorisation for the action and scope |
| E: Destructive or hard to reverse | Delete files, rewrite history, force-push, merge, deploy, drop data, revoke access | Obtain explicit confirmation immediately before execution |

Reversibility is practical, not theoretical. A change is not low risk merely
because Git could restore it if it affects production, shared branches, external
services, credentials, permissions, or user data.

## Least-destructive choices

When several actions would satisfy the request:

- Inspect before editing
- Prefer a targeted patch over replacing a whole file
- Prefer a new commit over rewriting history
- Prefer a draft pull request over merging
- Prefer a reversible migration over an in-place destructive change
- Prefer a targeted command over a broad recursive command
- Validate locally before publishing externally

## Request, proposal, and authorisation

Treat these as separate states:

1. **Requested**: the user describes an outcome
2. **Proposed**: the agent explains a specific action and scope
3. **Authorised**: the user permits that action and scope

A clear request can cover a low-risk, reversible action. For consequential
actions, the agent must ask for the missing authorisation.

For example, “fix the issue and open a pull request” authorises both the fix and
pull request creation. “Fix the issue” does not authorise pushing, opening a
pull request, or merging it.

## Preview guidance

Use a preview when it helps the user assess:

- An architectural or breaking change
- A broad multi-file change
- Material trade-offs
- Impact or rollback cost that is difficult to assess concisely

Skip a preview for trivial, local, reversible changes or a clear request where
no material choice remains. Skipping a preview does not bypass authorisation
requirements.

## Common action matrix

| Action | Approval boundary |
|---|---|
| Read, search, inspect, or run non-mutating checks | No approval |
| Edit a clearly requested local file | User request is sufficient |
| Create a local branch or checkpoint commit | User request is sufficient; offer optional checkpoint prompts |
| Run a formatter or tests that only change disposable output | No extra approval |
| Change a dependency manifest or lockfile | Explicit approval unless directly requested |
| Install packages or tools | Explicit approval unless directly requested |
| Create a commit when not requested | Offer an optional checkpoint; do not commit automatically |
| Push to a remote | Explicit approval |
| Create or update a pull request or issue | Explicit approval unless directly requested |
| Merge, deploy, publish, or release | Explicit approval immediately before execution |
| Delete files, data, branches, or resources | Explicit approval immediately before execution |
| Rewrite history or force-push | Explicit approval, with impact and rollback warning |
| Change credentials, permissions, or security controls | Explicit approval for the exact scope |

## Host and tool enforcement

Instructions guide behaviour but cannot guarantee it. Where the host supports
deterministic controls, encode the policy at the tool boundary:

- Classify tools as read-only, reversible, external, or destructive
- Require a confirmation field or interactive confirmation for levels D and E
- Use pre-tool hooks to return `allow`, `ask`, or `deny`
- Block force-push, history rewriting, destructive deletion, and production
  deployment by default
- Require a preview or dry-run result before an irreversible operation
- Record the approved scope, tool call, actor, and result in an audit log
- Use separate credentials or capabilities for inspection, modification, and
  deployment

If the host has no enforcement layer, apply this policy procedurally and report
the limitation rather than implying that the repository can enforce it.

## Verification scenarios

Implementations of this policy should verify at least these cases:

- A clear one-file fix proceeds without an unnecessary approval prompt
- An ambiguous multi-file change asks the user to choose an approach
- A preview does not count as permission to act
- A request to fix an issue does not authorise pushing or merging
- Push, merge, deploy, delete, and force-push require explicit authorisation
- Approval for one scope does not authorise an expanded scope
- Silence after a preview does not authorise execution
- The least destructive viable operation is selected

