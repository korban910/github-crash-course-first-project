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

|                     | `git fetch`                 | `git pull --rebase`               |
| ------------------- | --------------------------- | --------------------------------- |
| Downloads commits   | Yes                         | Yes                               |
| Changes your branch | No                          | Yes — replays your commits on top |
| Typical use         | Check what changed remotely | Update your branch before pushing |

## 7. Copying an existing repository — `git clone`

`git clone` downloads a repository that already exists (for example on GitHub) and creates a ready-to-use local copy — you do **not** run `git init` afterwards.

```
git clone https://github.com/korban/first-project.git
git clone https://github.com/korban/first-project.git my-folder-name
```

- The first form creates a folder named after the repository (`first-project`).
- The second form puts the same content in a folder you name yourself (`my-folder-name`).
- Cloning brings the full history, not just the latest files, so `git log`, `checkout`, `revert` all work right away.
- The repository you cloned from is automatically saved as a remote called **`origin`**, so `git fetch`, `git pull`, and `git push` already know where to go.
- Private repositories work the same way, but Git asks you to sign in (a GitHub token or an SSH key) because the URL is not publicly readable.

## 8. Looking at the remotes — `git remote` and `git branch -r`

A **remote** is a nickname for a repository URL stored somewhere else (usually GitHub). "`origin`" is just the default nickname Git gives to the repository you cloned from — the name has no special power.

### Which remotes does this repository know about?

```
git remote
origin

git remote -v
origin  https://github.com/korban/first-project.git (fetch)
origin  https://github.com/korban/first-project.git (push)
```

- `git remote` lists the nicknames only.
- `git remote -v` ("verbose") also shows the URL behind each nickname, once for downloading (`fetch`) and once for uploading (`push`).
- `git remote get-url origin` prints just the URL of a single remote when that is all you need.

### Which branches exist on the remote?

```
git branch          # branches on your computer
git branch -r       # branches on the remote
  origin/HEAD -> origin/main
  origin/main

git branch -a       # both lists together
```

- Names like `origin/main` are **read-only copies** of the remote branches. You do not work on them directly.
- These copies only refresh when you run `git fetch` or `git pull` — if a teammate pushed a new branch a minute ago, you will not see it until then.
- To start working on a remote branch, switch to it by name and Git creates a matching local branch for you:

```
git fetch
git checkout feature-login
```
