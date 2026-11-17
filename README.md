# .agents

## Skills

`skills/` is the personal skill discovery directory. Skills maintained in this
repository are tracked by default. Installed copies from external sources stay
in place for agent compatibility but are ignored by exact directory rules in
`.gitignore`.

When installing an external skill, add its `/skills/<name>/` directory to
`.gitignore` and group the rule under its source. Do not restore a blanket
`/skills/*` ignore rule: new skills maintained here should appear in Git by
default. Check a skill's status with `git status --short -- skills/<name>` and
whether it is ignored with `git check-ignore -v skills/<name>/SKILL.md`.
