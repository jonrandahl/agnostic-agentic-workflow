# ChatGPT and Codex Adapter — Shared Guidelines Integration

This adapter points ChatGPT and Codex integrations to the shared agentic
workflow guidance in `~/.agents/guidelines/`. It supplements, rather than
duplicates, the shared policy.

## Read shared guidance first

Apply the relevant files from:

- `~/.agents/guidelines/core.md`
- `~/.agents/guidelines/technical-patterns.md`
- `~/.agents/guidelines/commit-checkpoint-guideline.md`
- `~/.agents/guidelines/action-authorisation.md`
- `~/.agents/guidelines/adr-guideline.md`
- `~/.agents/guidelines/review-first-pattern.md`

## ChatGPT and Codex operating posture

- Make material assumptions visible
- Prefer the least destructive action that satisfies the request
- Proceed directly only with clear, local, reversible work
- Show a preview when scope, impact, or trade-offs are material
- Obtain explicit, scope-specific authorisation before external, destructive, or
  hard-to-reverse actions
- Never infer authorisation from silence or approval of a different proposal
- Verify outcomes before reporting completion

## Integration notes

Use this file as the adapter entry point for ChatGPT and Codex environments.
Keep universal policy in `~/.agents/guidelines/`; keep provider-specific
bootstrap, tool, or runtime instructions here.
