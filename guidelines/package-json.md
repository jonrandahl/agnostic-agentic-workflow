# Package JSON Guidelines - Shared Project Metadata Standards

This file documents the shared conventions for package.json files across all
projects that use Node-based tooling.

Use this when creating or updating package.json so the file stays predictable,
easy to scan, and consistent across repos.

---

## Purpose

Package.json is the contract between a project and its tooling. A consistent
shape makes it easier to review, safer to update, and faster to copy into new
projects without forgetting important metadata.

---

## Why This Matters

- **Humans read package.json often.** Clear grouping reduces friction during
  maintenance and code review.
- **Tooling depends on it.** pnpm, npm, Vitest, and Playwright all consume this
  file.
- **Metadata helps discovery and ownership.** Accurate fields make the project
  easier to understand and maintain.
- **Consistent ordering avoids noise.** When every project follows the same
  shape, diffs stay smaller and clearer.

---

## Recommended Order

There is no npm-enforced key order. Common practice is semantic grouping rather
than sorting the entire file alphabetically.

Use this order unless a project has a stronger reason to deviate:

1. Core identity and package metadata
   - `name`
   - `version`
   - `description`
   - `private`
   - `packageManager`
   - `type`
2. Scripts and package behaviour
   - `scripts`
3. Ownership and legal metadata
   - `author`
   - `license`
   - `keywords`
4. Linkable project metadata, when available
   - `homepage`
   - `repository`
   - `bugs`
   - `funding`
5. Runtime and packaging controls
   - `engines`
   - `files`
   - `publishConfig`
   - `config`
6. Dependencies
   - `dependencies`
   - `devDependencies`
   - `peerDependencies`
   - `optionalDependencies`

Sort simple list values and dependency maps alphabetically where that does not
hide meaning.

For front-end applications that are not published as packages, omit `main`
unless there is a specific consumer that requires it.

---

## Required Fields

- `name` must be present and should stay short, lower-case, and package-safe.
- `version` should use semantic versioning.
- Unreleased projects should begin at `0.0.1`.
- If prerelease tags are used, attach them to a minor-or-higher bump such as
  `0.1.0-alpha.1` or `0.1.0-beta.1`, not to patch-only bumps.
- Use `1.0.0` only when the project is ready to be treated as a stable public
  release.
- `description` should explain what the project is in one concise sentence.
- `private: true` should be set for application repos that are not meant to be
  published.
- `packageManager` should pin the active package manager and version, such as
  `pnpm@11.1.2`.
- `license` should use a valid SPDX identifier.
- `author` should name the project owner or maintainer.

`main` is optional and is usually unnecessary for a front-end app that is not
being consumed as a library.

---

## Recommended Optional Fields

Add these only when the values exist and are real URLs or meaningful metadata:

- `homepage` for the live demo or project landing page.
- `repository` for the source control location.
- `bugs` for the issue tracker or support contact.
- `funding` if the project has a real sponsorship or donation target.
- `engines` when a project needs to document supported Node versions.

Place `keywords` after author and licence so the ownership and legal details
read as one block, with discoverability metadata last in that group.

Do not add placeholder URLs. If a project does not have a public homepage or
issue tracker yet, omit the field until it exists.

---

## Licence Guidance

Use a recognised SPDX licence identifier in package.json and keep it aligned
with the repository licence file.

- `ISC` is short and permissive.
- `MIT` is also permissive and widely recognised.
- `Apache-2.0` is permissive and adds an explicit patent grant.
- `GPL` or `AGPL` are copyleft licences and should be chosen deliberately.

For quick review:

- npm package.json docs: <https://docs.npmjs.com/cli/v11/configuring-npm/package-json>
- SPDX licence list: <https://spdx.org/licenses/>
- Choose a licence: <https://choosealicense.com/>

---

## Maintenance

When adding package metadata to a new project, follow this structure first.
When a project needs a deviation, document the reason in the project README or
decision log before changing the order.

---

*Last updated: 18 May 2026*
