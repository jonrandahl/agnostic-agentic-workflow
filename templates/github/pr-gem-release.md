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
