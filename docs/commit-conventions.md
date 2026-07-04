# Commit Conventions

## Message Format

- Use `<type>: <short English summary>`.  Prefer including a brief body
  describing what changed and why; title-only commits are acceptable for
  trivial or self-explanatory changes.
- Do not use PowerShell here-string syntax (`@'...'@`) in Bash — it
  embeds a literal `@` in the commit message.
- After committing, verify with `git log -1 --format='%s'` that the
  subject line starts with the expected `<type>:` prefix.

## Git Operations

- Prefer small, verifiable PowerShell and Git commands instead of long chained
  commands.
- Run Git write operations serially. Do not overlap `git add`, `git commit`,
  `git merge`, `git rebase`, or branch-changing commands.
