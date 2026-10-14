# Pull Request Template Guidelines

## Purpose

Ensure consistent, clear pull request descriptions that facilitate review,
approval, and deployment tracking across the team.

**Usage reminder:** Review this guide before drafting any PR description.

**Tone and Language:**

- Use British English (en-gb) spelling and grammar throughout.
- Write for clarity and professionalism.

> [!TIP]
> Use the changelog as your source of truth for "What's Changed". If you want
> a head start, you can also generate draft notes with GitHub's "Generate
> release notes" feature:
>
> 1. **Open PR creation page** targeting `production` branch
> 2. **Click "Generate release notes"** button (top-right of description field)
> 3. **Specify version range:** Previous release tag (e.g., `v2.3.1`) to current
>    branch (`dev`)
> 4. GitHub automatically lists merged PRs, categorized by labels
> 5. **Review and adjust** if needed, then paste into PR description
>
> **Prerequisites:**
>
> - Repository has `.github/release.yml` configured (copy from `templates/github/.github/release.yml`)
> - PRs are properly labeled during merge (see PR Labeling Guide below)
> - Release tags follow semantic versioning (e.g., `v3.0.0`)

---

## PR Labeling Guide

Proper labels ensure accurate release note generation and clear categorization.

### Label Each PR With ONE Primary Category

| Label | Use When | Example |
|-------|----------|---------|
| `feature` / `enhancement` | Adding new functionality | "Add user profile page" |
| `bug` / `fix` | Fixing defects or errors | "Resolve login timeout" |
| `chore` | Maintenance, refactoring, tooling | "Update linter configuration" |
| `docs` / `documentation` | Documentation only | "Update API reference" |
| `performance` / `perf` | Performance improvements | "Optimize database queries" |
| `security` | Security fixes or improvements | "Patch XSS vulnerability" |
| `style` | UI/UX, formatting (no logic) | "Adjust button spacing" |
| `test` / `testing` | Test additions or changes | "Add integration tests" |
| `ci` / `build` | CI/CD or build changes | "Update GitHub Actions" |
| `dependencies` | Dependency updates | "Bump Rails to 8.1" |

### Additional Labels (Optional)

- `skip-changelog`: Exclude from release notes (internal changes)
- `breaking`: Indicates breaking changes (use with primary label)

### Applying Labels

1. **During PR creation:** Add label in GitHub UI before submitting
2. **During review:** Reviewer can add/adjust labels before merge
3. **Before release:** Audit unlabeled merged PRs and apply labels

### Best Practices

- Use lowercase labels consistently
- One primary category label per PR
- Add `breaking` label if introducing breaking changes
- Use `skip-changelog` sparingly (team visibility matters)
- Review labels before generating release notes

---

## Pointers

- **Release PR titles:** Use format "Release v3.0.0" (concise, version clear)
- **Gem release PRs:** Use `templates/github/pr-gem-release.md` for versioned package releases
- **Standard PR titles:** Use conventional commit format when possible
- **Deploy target:** Always state explicitly in release PRs for clarity
- **Fast-forward merges:** Preferred for clean history; if conflicts arise,
  resolve manually and document
- **Changelog sync:** For major releases, ensure changelog matches generated
  release notes

---

## "Release" Pull Request Template

**When to use:** Merging code to `production` branch for deployment.

**Key characteristics:**

- No code review needed (changes already reviewed and deployed)
- Approval required only for release version and included changes
- Optionally use GitHub's "Generate release notes" as a starting point

### Template Structure

```markdown
**Deploy Target:** Production

> [!TIP]
> This is a release PR. Code has been reviewed and deployed already.
> Approval required only for the version and changes listed below.
>
> <details>
>   <summary><em>Use <code>git merge --ff-only</code> if possible</em></summary>
>   <dl>
>     <dt>Step 1: Clone or update your local repository</dt>
>     <dd><code>git pull origin production</code></dd>
>     <dt>Step 2: Switch to the production branch</dt>
>     <dd><code>git checkout production</code></dd>
>     <dt>Step 3: Merge the dev branch into production</dt>
>     <dd><code>git merge --ff-only dev</code></dd>
>     <dt>Step 4: Push the changes</dt>
>     <dd><code>git push -u origin production</code></dd>
>   </dl>
>   <p>
>    If <code>git merge --ff-only</code> fails because the target branch has intentional
>    changes, first bring those commits into the base branch (merge or cherry-pick) and
>    retry. Only if you must merge both histories immediately, run
>    <code>git merge --no-ff</code>, then back-merge the target branch into base branch
>    and document the reconciliation.
>   </p>
> </details>

## What's Changed

[Highlight the current changes from the CHANGELOG]

**Full Changelog**: https://github.com/epimorphics/[REPOSITORY]/compare/vO.L.D...vN.E.W
```

<details>
<summary><strong>Example</strong></summary>
<blockquote>
<strong>Title:</strong> Release v3.0.0

<strong>Description:</strong>

<pre>
**Deploy Target:** Production

This is a release PR. Code has been reviewed and deployed to development.
Approval required only for the version and changes listed below.

[Merge instructions collapsed]

## What's Changed

- New Feature: Add user authentication
  [#123](https://github.com/epimorphics/repo/pull/123)

- Bug Fix: Resolve dropdown menu alignment
  [#124](https://github.com/epimorphics/repo/pull/124)
- Maintenance: Update dependencies to latest versions
  [#125](https://github.com/epimorphics/repo/pull/125)

**Full Changelog**: <https://github.com/epimorphics/repo/compare/v2.3.1...v3.0.0>
</pre>
</blockquote>
</details>

---

## Standard Pull Request Template

**When to use:** Feature branches, bug fixes, or any changes merging into the
primary branch.

> [!NOTE]
> Choose the issue reference style that applies to your work.

### Section Headers

Use **"What's Changed"** (not "Changes") as the section header for the list of modifications.
This ensures consistency across all PR descriptions and aligns with release note generation.

### Checklist Curation

**Remove inapplicable checklist items** — do not leave them unchecked or use
strikethrough. This keeps the PR focused and signals that the author has
actively considered each item.

For example, if no unit tests were added and no documentation changes were
needed, remove those items entirely rather than leaving empty checkboxes.

### Template Structure

```markdown
## Summary

[Brief description of what this PR does and why - 1-2 sentences]

Specific to ticket #[issue-number]
OR
Relates to #[issue-number]

## What's Changed

- [List significant changes as bullet points]
- [Use sentence case, past tense verbs]
- [Focus on what and why, not how]

## Breaking Changes

[If applicable, list any breaking changes and migration steps]
[Remove this section if no breaking changes]

## Checklist

### Testing
- [ ] Unit tests added/updated
- [ ] Manual testing completed
- [ ] No regressions introduced

### Build & Assets
- [ ] Assets compile successfully
- [ ] Linting passes (rubocop, eslint)
- [ ] Docker image builds successfully

### Process & Quality
- [ ] Changelog updated
- [ ] Documentation updated
- [ ] Database migrations tested
- [ ] API/integration tests passing
- [ ] Accessibility standards met
```

<details>
<summary><strong>Example</strong></summary>
<blockquote>
<strong>Title:</strong> Modernised asset pipeline and CSS tooling

<strong>Description:</strong>

<pre>
## Summary

Modernised asset pipeline and CSS tooling by migrating from deprecated
`sass-rails` gem to Ruby Sass, whilst integrating Bootstrap framework
dependencies for enhanced styling capabilities and improved maintainability.

Specific to ticket #158

## What's Changed

- Migrated from deprecated `sass-rails` gem to Ruby Sass for long-term CSS
  compilation support
- Added Bootstrap framework dependencies for enhanced styling capabilities and
  maintainability
- Updated asset pipeline configuration and loading order for improved
  performance
- Enabled Autoprefixer source maps for better CSS debugging in development
- Streamlined Rails framework loading configuration
- Updated Sass configuration for compatibility with new build toolchain
- Introduced interactive dependency update approach for more flexible gem
  management

## Breaking Changes

None

## Checklist

### Testing
- [x] Unit tests added/updated
- [x] Manual testing completed
- [x] No regressions introduced

### Build & Assets
- [x] Assets compile successfully
- [x] Linting passes (rubocop, eslint)
- [ ] Docker image builds successfully

### Process & Quality
- [x] Changelog updated

</pre>
</blockquote>
</details>

---

## Gem Release Pull Request Template

**When to use:** Releasing a gem version (patch/minor/major). Tailored for
projects where the gem is consumed by specific applications.

**Guidance:**

- Use the concise headings above; keep bullets focused on user-facing impact
  and operational steps.
- Call out consumers explicitly when limited (e.g., LR Landing only).
- Include concrete smoke checks relevant to the change (UI behaviour, logging,
  build/gem packaging).

### Template Structure (copy/paste)

```markdown
# Release: <Gem Name> v<version>

## Release Summary
Brief summary of the release intent and scope (1-2 sentences). Mention primary
consumers if limited (e.g., "Consumer: LR Landing only").

Version: <version>
Issue: <link to issue if applicable>
Breaking Changes: Yes/No

## Changes in This Release
- <Bullet change 1 (what + why, past tense)>
- <Bullet change 2>
- <Bullet change 3>

## Testing Checklist
- [ ] make test
- [ ] make gem (local build verification)
- [ ] Local testing in consuming app(s) with local path to Gemfile
- [ ] Consumer smoke test after bump:
  - <UI/behaviour check>
  - <Logging/observability check>
- [ ] Remove path Gemfile reference before publishing and update to version tag.

## Consumers

- List consuming apps. If single consumer, call it out (e.g., "LR Landing
  only"). Provide the action: update Gemfile to <version> and run smoke checks.

## Post-Release

- Publish gem and bump consuming app(s) to <version>.
- Monitor for regressions relevant to this release (name the areas).

```

<details>
<summary><strong>Example</strong></summary>
<blockquote>
<strong>Title:</strong> Release: Qonsole-Rails v2.3.1

<strong>Description:</strong>

<pre>
# Release: Qonsole-Rails v2.3.1

> [!TIP]
> This is a release PR. Code has been reviewed and deployed already.
> Approval required only for the version and changes listed below.

## Release Summary
Patch release focused on accessibility improvements, logging noise reduction,
and housekeeping. Qonsole is only consumed by LR Landing.

Version: 2.3.1
Issue: [#105](https://github.com/epimorphics/qonsole-rails/issues/105)
Breaking Changes: No

## Changes in This Release
- Refactored vertical dropdowns to use Bootstrap 5-compatible button toggles
  with improved accessibility attributes.
- Updated ignore rules to exclude macOS-generated files.
- Improved gemspec formatting (whitespace-only cleanup).
- Limited response logging of headers/error bodies to debug mode to keep
  production logs quiet.

## Testing Checklist
- [x] make test
- [x] make gem (local build verification successful)
- [x] Local testing in LR Landing with path Gemfile:
  Added to Gemfile: `gem 'qonsole_rails', path: '../qonsole-rails'`
  Ran `bundle install` and verified dropdown behaviour and logging.
- [x] LR Landing smoke test after bump:
  - Dropdown behaviour in the vertical view (no page jumps, correct toggles)
  - Log output quiet in non-debug environments

## Consumers

- LR Landing (only). Update Gemfile to 2.3.1 and run the smoke checks above.

## Post-Release

- Publish gem and bump LR Landing to 2.3.1.
- Monitor LR Landing UI and logs for regressions in dropdown behaviour and
  logging volume.
</pre>
</blockquote>
</details>
