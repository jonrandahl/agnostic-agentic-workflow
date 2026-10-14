# Pattern Migration Verification — All Agents

This file defines how to confirm a pattern change is actually complete
before calling it done. It applies whenever a change replaces, removes, or
renames something that could exist in more than one place.

---

## Purpose

A change gets made in the file that prompted it, and the task is called
done. Other files using the same pattern, out of sight during that change,
keep the old approach. Nobody notices until a reviewer greps for it, or
until documentation asserts the migration is complete when it is not.

---

## Why This Matters

- **Fixing the visible instance feels like fixing the problem.** It is not,
  if the pattern exists anywhere else.
- **Claims outlive the check that would have caught them.** A PR
  description, commit message, or ADR that says "removed" or "no more X"
  is only true if every instance was actually searched for.
- **Copy-paste spreads the old pattern faster than the new one replaces
  it.** A markup structure or class name used as a template for new pages
  will keep reproducing the pre-migration version until the migration is
  verified complete, not just applied once.
- **A search scoped to output type misses generated instances.** A font-CDN
  migration once searched only `*.html` and missed a `.mjs` script whose
  template literal generated pages with the same stale `<link>` tag baked
  in. The rendered output looked migrated; the generator that would keep
  reproducing the old pattern was never checked.
- **Reviewers should not be the verification step.** If the first time a
  stray instance is caught is in review, the check that should have run
  before the PR was opened did not run.

---

## When this applies

- Renaming, removing, or restructuring a CSS class, selector, or markup
  pattern that other pages, scripts, or tests may depend on
- Migrating an included third-party resource (CDN link, font, script) to a
  self-hosted or different alternative
- Removing or renaming a data field, config key, or naming convention that
  could have been copy-pasted elsewhere
- Any change described in a commit message, PR description, or ADR using
  language like "site-wide", "all instances", "no more X", or "fully
  migrated"

Routine, single-location fixes with no reusable pattern (a typo, a
one-off bug fix) do not need this check.

---

## Required behaviour

- Before starting the change, search the whole codebase for existing
  instances of the pattern being replaced. Not just the file already open,
  not just the directory the task started in.
- Do not scope that search to the pattern's rendered output type. A pattern
  living in HTML can also live in a template literal inside a `.mjs`, `.js`,
  or any other source file that generates HTML as a string. Search by the
  pattern itself, not by file extension, or run the search once per
  plausible source type and say which types were covered.
- Apply the change to every instance found, or explicitly record which
  instances are intentionally excluded and why.
- Before declaring the task complete, re-run the same unscoped search to
  confirm zero remaining instances outside documented exceptions.
- Never write a completion claim ("removed", "no longer used", "fully
  migrated") without that search backing it. If the search was not run,
  the claim is a guess, not a fact.

---

## Recording verification in an ADR

When the change is significant enough to warrant an ADR (see
`~/.agents/guidelines/adr-guideline.md`), record the search directly in it
using a `## Verification` section:

```markdown
## Verification

Searched the repository for `<old pattern>` on YYYY-MM-DD:
`rg "old-pattern"` — N matches, all migrated. (Or: N matches, listed as
exceptions below, with reasons.)
```

This turns a claim like "no third-party font CDN requests remaining" into
a checked fact instead of a statement true only for the file that prompted
the ADR.

---

## Periodic re-verification of Accepted ADRs

An ADR's Verification search is only true at the moment it was run.
Codebases drift afterwards: new pages get added, and an old pattern can be
reintroduced by someone copy-pasting a pre-migration page as a template.

Before merging any PR that touches files an existing Accepted ADR's
Verification search would match, re-run that ADR's search. If it still
holds, no action needed. If it does not, either fix the drift in the PR or
update the ADR to reflect current reality. Never leave an Accepted ADR
asserting something the codebase no longer does.

---

## What this file owns

- The obligation to search before and after a pattern-level change
- The convention for recording that search in an ADR
- The re-verification trigger for Accepted ADRs

## What belongs elsewhere

- The ADR template, numbering, and criteria for when to write one:
  `~/.agents/guidelines/adr-guideline.md`
- Runtime and behavioural verification (does the feature actually work):
  the `verify` skill
- Logic and correctness review: the `code-review` skill

---

## Maintenance

**Universal change to when/how this check runs?** Update this file.
**Project-specific exception list?** Belongs in that project's CLAUDE.md,
not here.

Avoid duplication. If a project's CLAUDE.md starts repeating this file's
rules verbatim, remove the duplication and reference this file instead.

---

*Last updated: 15 July 2026*
*Shared guidelines location: ~/.agents/guidelines/*
*Related files: adr-guideline.md, commit-checkpoint-guideline.md*
