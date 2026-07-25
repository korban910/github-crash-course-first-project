# First Project — Git & GitHub Crash Course

Hands-on practice with the core Git commands. Concept notes (What is Git/GitHub, GitHub Actions, CI/CD) live in [NOTES.md](NOTES.md).

## Key Commands

![Key Commands](images/key_commands.png)

| Command               | What it does                                                       |
| --------------------- | ------------------------------------------------------------------ |
| `git init`            | Initialize a Git repository (only required once per project)       |
| `git add <file(s)>`   | Stage code changes (for the next commit)                           |
| `git commit -m "..."` | Create a commit for the staged changes (with a message)            |
| `git status`          | Get the current repository status (e.g., which changes are staged) |
| `git log`             | Output a chronologically ordered list of commits                   |
| `git checkout <id>`   | Temporarily move back to commit `<id>`                             |
| `git revert <id>`     | Revert the changes of commit `<id>` (by creating a new commit)     |
| `git reset <id>`      | Undo commit(s) up to commit `<id>` by deleting commits             |

[Git Documentation](./GIT.md)

[Git Branch](./BRANCH.md)

[Git Clone or Fork](./CLONE_OR_FORK.md)
