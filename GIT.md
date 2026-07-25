[Go back to Home](./README.md)

## 1. Creating the repository and the first commit

In the **first-project** folder:

```
git init
git branch -M main
git add index.html
git commit -m "Add index.html"
```

- `git init` turns the folder into a Git repository — needed once per project.
- `git branch -M main` renames the default branch to `main`.
- `git add` **stages** changes; `git commit` saves a **snapshot** of all staged changes.

![Creating commits and moving between them](images/git_checkout_add.png)

## 2. Inspecting history — `git log`

```
git log
commit 751a6b1f4b658558c70f0a2248d267fa942dbcc4 (HEAD -> main)
Author: korban <korban.torsun@gmail.com>
Date:   Tue Jul 21 17:01:19 2026 -0400

    added linked to Git docs.

commit e6153040c7669c5750712965c777a316051435da
Author: korban <korban.torsun@gmail.com>
Date:   Tue Jul 21 16:56:43 2026 -0400

    initial commit
```

- Commits are listed newest first; `HEAD -> main` marks where you currently are.
- Each commit has a unique id (hash) used by `checkout`, `revert`, and `reset`.

## 3. Moving between commits — `git checkout`

```
git checkout <commit_id>
git status
git checkout main
```

- `git checkout <commit_id>` **temporarily** moves HEAD to an older snapshot — nothing is deleted.
- `git checkout main` brings you back to the latest commit on the branch.

## 4. Undoing commits — `git revert` (safe)

![Undoing commits with revert](images/git_revert.png)

```
git revert <commit_id>
git revert 751a6b1f4b658558c70f0a2248d267fa942dbcc4
```

- Revert undoes the changes of a commit **by creating a new commit** — history is preserved.

```
git log
Author: korban <korban.torsun@gmail.com>
Date:   Tue Jul 21 17:07:59 2026 -0400

    Revert "added linked to Git docs."

    This reverts commit 751a6b1f4b658558c70f0a2248d267fa942dbcc4.

commit 751a6b1f4b658558c70f0a2248d267fa942dbcc4
Author: korban <korban.torsun@gmail.com>
Date:   Tue Jul 21 17:01:19 2026 -0400

    added linked to Git docs.

commit e6153040c7669c5750712965c777a316051435da
Author: korban <korban.torsun@gmail.com>
Date:   Tue Jul 21 16:56:43 2026 -0400

    initial commit
```

Note the reverted commit is still in the log — a new "Revert" commit was added on top.

## 5. Undoing commits — `git reset --hard` (destructive)

![Undoing commits with reset](images/git_reset.png)

**NOT RECOMMENDED** — reset rewrites history:

```
git reset --hard <commit_id>
git reset --hard 751a6b1f4b658558c70f0a2248d267fa942dbcc4
```

- `git reset --hard <id>` undoes changes by **deleting all commits since `<id>`** — they disappear from the log.
- Prefer `git revert` when the history matters (and always for commits already shared with others).

## Revert vs. Reset — summary

|               | `git revert <id>`                      | `git reset --hard <id>`          |
| ------------- | -------------------------------------- | -------------------------------- |
| How it undoes | Adds a new commit that reverses `<id>` | Deletes all commits after `<id>` |
| History       | Preserved                              | Rewritten (commits lost)         |
| Safe to use   | Yes                                    | Not recommended                  |

## 6. Getting other people's commits — `git fetch` and `git pull`

When someone else pushes to the remote (e.g. GitHub), your local repository does **not** know about it until you ask.

### Look without changing anything — `git fetch`

```
git fetch
git log --oneline main..origin/main
```

- `git fetch` downloads the new commits from the remote and updates `origin/main` (the local copy of the remote branch).
- Your own branch and your files stay exactly as they are — nothing is merged yet.
- `git log --oneline main..origin/main` lists the commits the remote has that you don't, so you can see what is coming.

### Download **and** apply — `git pull --rebase`

```
git pull --rebase
```

- `git pull` = `git fetch` + apply the new commits to your current branch.
- With `--rebase`, your local commits are temporarily set aside, the remote commits are applied first, then your commits are replayed on top. The result is one straight line of history with no extra "Merge branch..." commit.
- Without `--rebase`, Git joins the two histories with a merge commit instead.
- Commit or stash your work first — pull refuses to run when you have uncommitted changes it would overwrite.

### If a rebase stops on a conflict

```
git status              # shows the conflicting files
# edit the files, keep the correct lines
git add <file>
git rebase --continue
```

- `git rebase --abort` cancels everything and puts the branch back the way it was before the pull.

| | `git fetch` | `git pull --rebase` |
| ------------------- | --------------------------- | ---------------------------------------- |
| Downloads commits | Yes | Yes |
| Changes your branch | No | Yes — replays your commits on top |
| Typical use | Check what changed remotely | Update your branch before pushing |
