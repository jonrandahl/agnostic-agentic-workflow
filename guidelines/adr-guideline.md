# Architectural Decision Record Guideline — All Agents

This file defines when and how to write Architectural Decision Records (ADRs).
It applies to every project. The ADRs themselves are project-specific and live
in each project's repository, not here.

---

## Purpose

Significant technical decisions get made in conversation and then forgotten.
Six months later nobody, human or agent, can explain why a technology,
naming convention, or scope boundary was chosen, and the same debate happens
again. ADRs capture the decision, the alternatives considered, and the reason
they were rejected, at the point the decision is made.

---

## Why This Matters

- **Context decays fast.** The reasoning behind a decision is clearest at the
  moment it is made and hardest to reconstruct later.
- **Agents lack memory across sessions.** A future agent (or the same agent in
  a new session) needs a written record to avoid relitigating settled
  decisions or silently reversing them.
- **Alternatives are as valuable as the choice.** Recording what was rejected,
  and why, prevents re-proposing options that were already ruled out.
- **Decisions compound.** Early architectural choices constrain later ones.
  An ADR trail lets anyone trace why the current shape exists.

---

## When to write an ADR

Write one when a decision is hard to reverse, constrains future work, or
would need re-explaining if questioned later. Examples:

- Choosing a core technology, framework, or library with long-term lock-in
  (build tooling, hosting platform, database)
- Data model or schema decisions that later migrations must respect
- Authentication, authorisation, or security architecture choices
- Terminology or UI conventions that must stay consistent site-wide
- Scope boundaries (what a version explicitly does not do, and why)
- Superseding or reversing a previous ADR

Do not write one for routine bug fixes, small refactors, styling tweaks,
dependency bumps, or anything easily reversible with low blast radius. Not
every decision is architectural.

---

## Required behaviour

- When a task involves a decision matching the criteria above, propose
  writing an ADR before or immediately after implementing it. Keep the
  prompt optional and non-blocking, in the same spirit as
  `~/.agents/guidelines/commit-checkpoint-guideline.md`.
- If the user declines, continue without friction. If genuinely unsure
  whether a decision qualifies, ask one short question rather than guessing.
- Never edit an accepted ADR to reflect a changed decision. Write a new ADR
  that supersedes it and update the old one's status.
- Before proposing a new ADR, check the project's existing decision log for
  a prior entry the new decision might supersede.
- Before writing a claim of the form "all X migrated" or "no more Y" into
  an ADR, verify it repository-wide first. Follow
  `~/.agents/guidelines/pattern-migration-guideline.md` and record the
  search in the ADR's Verification section, not just the file that
  prompted the decision.
- Before merging any PR that touches files an existing Accepted ADR's
  Verification search would match, re-run that search per
  `~/.agents/guidelines/pattern-migration-guideline.md`. If it no longer
  holds, fix the drift or update the ADR; do not leave it asserting
  something the codebase no longer does.

---

## File location and naming

Store ADRs in a `decisions/` directory at the project root, one file per
decision:

```
decisions/001-short-kebab-case-title.md
```

- Three-digit, zero-padded, sequential numbering, never reused
- Kebab-case title after the number, short enough to scan in a file listing
- If a project already uses a different established location (for example
  `docs/adr/`), keep that project's convention rather than migrating it;
  apply this default only to projects with no existing convention

---

## Template

```markdown
# ADR-NNN: Title

**Status:** Proposed | Accepted | Deprecated | Superseded by ADR-NNN
**Date:** YYYY-MM-DD

## Context

What problem or question prompted this decision. Include research or
evidence where it materially informed the choice.

## Decision

The choice made, stated plainly and unambiguously.

## Rationale

Why this option, in bullet form. What it achieves, what it avoids.

## Alternatives considered

- **Option A** — what it was, why it was rejected
- **Option B** — what it was, why it was rejected

## Consequences

What this decision commits the project to, including any follow-up work or
constraints it places on future changes.

## Verification

If the decision or its Consequences claim a pattern was fully migrated,
removed, or made consistent site-wide, the repository-wide search that
confirms it, per `~/.agents/guidelines/pattern-migration-guideline.md`:

`rg "<old pattern>"` on YYYY-MM-DD — N matches, all migrated (or listed as
exceptions with reasons). Omit this section entirely if the ADR makes no
such claim.
```

Status starts as `Proposed` for decisions still open to challenge, and moves
to `Accepted` once settled. A decision that reverses an earlier one sets the
old ADR's status to `Superseded by ADR-NNN` rather than deleting it. An
Accepted ADR with a Verification section is re-checked against the current
codebase at the trigger point in Required behaviour above; a search that no
longer holds means the ADR needs updating, not blind trust.

---

## What this file owns

- The criteria for when an ADR is warranted
- The file location, naming, and template convention
- The agent workflow for proposing one

## What belongs elsewhere

- The ADRs themselves: project's own `decisions/` directory
- Project-specific decision criteria that narrow this guideline: project
  CLAUDE.md
- Non-architectural documentation patterns (README, CHANGELOG, CONTRIBUTING):
  `~/.agents/guidelines/documentation-templates.md`

---

## Maintenance

**Universal change to criteria, template, or workflow?** Update this file.
**Project-specific ADR content?** Belongs in that project's `decisions/`
directory, never here.

Avoid duplication. If a project's CLAUDE.md starts repeating this file's
rules verbatim, remove the duplication and reference this file instead.

---

*Last updated: 15 July 2026*
*Shared guidelines location: ~/.agents/guidelines/*
*Related files: pattern-migration-guideline.md, commit-checkpoint-guideline.md,
documentation-templates.md, documentation-style.md*
