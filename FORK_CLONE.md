[Go back to Home](./README.md)

# Contributing to a repository you do not own — Clone or Fork

## The problem

You can `git clone` any public repository and commit locally as much as you like. But when you try to send those commits back:

```bash
git push
remote: Permission to korban910/some-project.git denied to your-username.
fatal: unable to access 'https://github.com/korban910/some-project.git/': The requested URL returned error: 403
```

Cloning gives you **read** access, not **write** access. Only the repository owner and the people listed as **collaborators** are allowed to push.

There are two ways around this. Which one you use depends on whether you can get write access.

---

## Option 1 — Clone, and ask for access

Use this when you know the maintainer, or you are on the same team and expect to be added to the project.

### 1. Open an issue first

An **issue** is a GitHub discussion thread — a bug report, a question, or a proposal. It changes no code; it just starts the conversation.

On the repository page: **Issues** → **New issue**, then

- give it a short descriptive title (for example, "Broken link to GIT.md in README"),
- describe the problem or the change you want to make, with code in triple backticks,
- add a label (`bug`, `enhancement`, `documentation`) if the repository uses them,
- click **Create**.

### 2. Wait for the maintainer

The maintainer can then either

- make the change themselves, or
- add you under **Settings → Collaborators**, which gives you push access.

### 3. Once you have access, work on a branch — never straight on `main`

```bash
git checkout -b fix-readme-link
# make your changes
git add README.md
git commit -m "Fix link to GIT.md"
git push -u origin fix-readme-link
```

Then open a **pull request** from `fix-readme-link` into `main` and mention the issue number in the description, e.g. `Fixes #12`. GitHub links the two, and closes the issue automatically when the pull request is merged.

> **If you are the maintainer:** protect `main` under **Settings → Branches** so that nobody — including you — can push to it directly, and every change has to arrive as a reviewed pull request.

---

## Option 2 — Fork (the usual way to contribute)

There is no `git fork` command. **Forking is a GitHub action**, done with the **Fork** button at the top-right of the repository page.

A fork is your own complete copy of the repository, under your own account, which you fully control — so you need nobody's permission to push to it.

### 1. Fork and clone your copy

Click **Fork**. You now have `https://github.com/your-username/some-project`. Clone **that** one:

```bash
git clone https://github.com/your-username/some-project.git
cd some-project
```

### 2. Keep a link back to the original

Your clone's `origin` points at your fork. Add a second remote — conventionally named `upstream` — pointing at the original repository, so you can pull in other people's changes later:

```bash
git remote add upstream https://github.com/korban910/some-project.git
git remote -v
origin    https://github.com/your-username/some-project.git (fetch)
origin    https://github.com/your-username/some-project.git (push)
upstream  https://github.com/korban910/some-project.git (fetch)
upstream  https://github.com/korban910/some-project.git (push)
```

### 3. Work on a branch and push to your fork

```bash
git checkout -b add-dark-mode
# make your changes
git add .
git commit -m "Add dark mode toggle"
git push -u origin add-dark-mode
```

`origin` is your fork, so this push succeeds — no permissions needed.

### 4. Open the pull request

Go to your fork on GitHub. It shows a **Compare & pull request** button for the branch you just pushed. The pull request goes **from** `your-username:add-dark-mode` **into** `korban910:main`.

The maintainer reviews it, may ask for changes (push more commits to the same branch and they appear in the same pull request), and merges when satisfied.

### 5. Stay up to date with the original

While your pull request waits, the original repository keeps moving. Catch up before pushing more work:

```bash
git checkout main
git pull upstream main      # get the original's latest commits
git push origin main        # update your fork on GitHub
```

---

## Which one to use?

|                            | Clone + collaborator access                   | Fork                                        |
| -------------------------- | --------------------------------------------- | ------------------------------------------- |
| Where your commits go      | Branch in the original repository              | Branch in your own copy                     |
| Needs the maintainer's OK  | Yes, before you can push anything              | No — push to your fork immediately          |
| Pull request goes          | Branch → `main`, same repository               | Your fork → the original repository         |
| Typical use                | You are on the team                            | Open-source contribution from outside       |

Forking is the more common and the safer route: you cannot damage a repository you have no write access to, and the maintainer keeps full control over what gets merged.
