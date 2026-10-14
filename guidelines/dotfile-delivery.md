# Dot file delivery

## Rule

When producing a dot file for the user (any file whose name begins with a dot: `.zshrc`, `.gitconfig`, `.env.example`, and so on), deliver it with the leading dot dropped and a `.txt` extension appended: `zshrc.txt`, `gitconfig.txt`, `env.example.txt`.

## Why

macOS hides dot files in Finder and frequently blocks them from opening in GUI editors. Delivering the raw dot file makes review impossible without terminal gymnastics. The `.txt` form opens anywhere, can be compared against the existing file, and is renamed in one step on adoption.

## On adoption

Always include the command to restore the real name alongside the file, for example:

```sh
mv ~/Downloads/zshrc.txt ~/.zshrc
```

Where the file replaces an existing one, also include a backup step first:

```sh
cp ~/.zshrc ~/.zshrc.bak
```

## Scope

Applies to any agent or tool producing file output for the user, regardless of environment. Files written directly into place inside a project or home directory during an agreed task are exempt; this rule concerns files handed over for review.
