# 001. Action authorisation policy

## Status

Accepted

## Date

2026-11-01

## Context

The shared agentic workflow distinguishes between ordinary reversible work and
actions that can cause external, destructive, or hard-to-reverse effects. The
existing review-first guidance explains that a preview is not permission, but it
does not provide a common risk taxonomy or action matrix for all agents.

Without a shared policy, agents may ask for approval too often for harmless
work, or fail to ask before pushing, deleting, merging, deploying, or changing
security boundaries.

## Decision

Adopt a graduated, agent-agnostic authorisation policy:

- Read-only work proceeds without approval
- Clear, local, reversible work proceeds when requested
- Broad or materially consequential work receives a preview or explicit choice
  where appropriate
- External, destructive, or hard-to-reverse actions require explicit,
  scope-specific authorisation immediately before execution
- Agents choose the least destructive action that satisfies the request
- Silence, review of a proposal, or approval of a different action does not
  authorise execution
- Commits remain optional checkpoints rather than approval gates

The detailed taxonomy, action matrix, enforcement guidance, and verification
scenarios live in
[guidelines/action-authorisation.md](../guidelines/action-authorisation.md).

## Rationale

This preserves speed for low-risk work whilst placing a deliberate boundary
around actions with meaningful blast radius. Separating review from
authorisation prevents a plan or diff from being mistaken for permission.

The policy is agent-agnostic so that Copilot, Claude, Gemini, ChatGPT, Codex,
and future integrations share the same operating model.

## Alternatives considered

### Approve every action

Rejected because it creates unnecessary friction for read-only and reversible
work, and makes approval prompts less meaningful.

### Let each adapter define its own rules

Rejected because the same action could receive different treatment depending on
which agent performs it.

### Rely only on prose instructions

Accepted only as the baseline, not as the complete solution. Host-level hooks,
tool permissions, dry runs, capability separation, and audit logs should enforce
the boundary where those controls are available.

## Consequences

- Agents need to judge reversibility and external impact consistently
- Adapters point to one shared policy instead of duplicating the matrix
- Hosts that support pre-tool hooks can enforce `allow`, `ask`, and `deny`
  decisions deterministically
- Hosts without enforcement must report that limitation
- Projects still need their own ADRs for project-specific irreversible
  decisions

## Verification

- The policy contains levels A through E and a common action matrix
- The policy distinguishes requested, proposed, and authorised states
- The policy includes host/tool enforcement examples
- Adapter files reference the shared policy
- Verification scenarios cover harmless edits, expanded scope, push, merge,
  deploy, delete, and force-push boundaries
