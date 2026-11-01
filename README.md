 # Git & GitHub Notes

Beginner-friendly notes covering Git commands, branches, commits, remote repositories, forks, and Pull Requests.

## 1. What is Git?

Git is a **version control system** used to track changes in files and source code.

It helps developers:
- Track code changes over time.
- Restore previous versions.
- Work on different features using branches.
- Collaborate with other developers.

## 2. Git vs GitHub

| Git | GitHub |
|---|---|
| Version control system | Online platform for hosting Git repositories |
| Works locally on your computer | Stores repositories online |
| Tracks changes and commits | Supports collaboration, Pull Requests, and code reviews |

## 3. Important Git terms

- **Repository:** A project managed by Git, including its files and history.
- **Commit:** A saved snapshot of changes.
- **Branch:** A separate development path within a repository.
- **HEAD:** Points to the currently checked-out commit.
- **Working Directory:** The files you are currently editing.
- **Staging Area:** The area where you prepare changes for the next commit.
- **Remote:** A reference to a repository hosted elsewhere, such as GitHub.

### Git workflow

```text
Working Directory
       |
     git add
       v
  Staging Area
       |
    git commit
       v
 Local Repository
       |
     git push
       v
 Remote Repository (GitHub)
```

## 4. Basic Git commands

### Check Git version

```bash
git --version
```

Displays the installed Git version.

### Initialize a repository

```bash
git init
```

Creates a new Git repository in the current directory.

### Check repository status

```bash
git status
```

Shows modified, staged, and untracked files.

### Stage changes

```bash
git add .
```

Stages all changes in the current directory and its subdirectories.

To stage a specific file:

```bash
git add filename
```

### Commit changes

```bash
git commit -m "Add meaningful commit message"
```

Saves the staged changes as a commit.

### View commit history

```bash
git log --oneline
```

Displays a compact list of commits.

```bash
git log --oneline --graph --all
```

Displays commit history as a graph, including branches.

## 5. Understanding `git diff`

### `git diff`

```bash
git diff
```

**Meaning:** Compares the Working Directory with the Staging Area.

**What it does:** Shows changes in tracked files that have not yet been staged using `git add`.

**Use:** Review modified code before staging it.

### `git diff --staged`

```bash
git diff --staged
```

**Meaning:** Compares the Staging Area with the last commit (`HEAD`).

**What it does:** Shows changes staged using `git add` and ready to be committed.

**Use:** Review exactly what will be included in the next commit.

Alternative command:

```bash
git diff --cached
```

Both `git diff --staged` and `git diff --cached` perform the same operation.

### `git diff HEAD`

```bash
git diff HEAD
```

**Meaning:** Compares the Working Directory and Staging Area together against the last commit (`HEAD`).

**What it does:** Shows all changes in tracked files since the last commit, including staged and unstaged changes.

**Use:** Review all tracked modifications since the last commit.

**Note:** These commands do not show untracked files unless they are staged or otherwise included in the comparison.

## 6. Git branches

A branch is a separate development path within a repository. It allows you to work on a feature without directly changing another branch.

### List branches

```bash
git branch
```

Lists local branches. The `*` identifies the current branch.

### Create a new branch

```bash
git branch feature/chat-interface
```

Creates a branch at the current commit but does not switch to it.

### Switch to a branch

```bash
git switch feature/chat-interface
```

Switches to the specified branch.

### Create and switch in one command

```bash
git switch -c feature/chat-interface
```

Creates a new branch and switches to it.

Alternative older command:

```bash
git checkout -b feature/chat-interface
```

### Understanding HEAD

`HEAD` points to the currently checked-out commit.

When you switch branches, `HEAD` moves to the commit where that branch currently points.

Each branch maintains its own latest commit independently.

Example:

```bash
git branch feature1
```

Creates `feature1` at the current commit.

```bash
git switch feature1
```

Switches to `feature1`.

```bash
git merge feature1
```

Merges `feature1` into the branch you are currently on. If you are already on `feature1`, this does not merge it into another branch.

## 7. Merge one branch into another

To merge a feature branch into `main`:

```bash
# 1. Switch to the destination branch
git switch main

# 2. Merge the source branch into main
git merge feature/chat-interface
```

**Meaning:** `git merge` combines the changes and history of the specified branch into the branch you are currently on.

Remember:

- `git branch` creates or lists branches.
- `git switch` changes the current branch.
- `git merge` combines another branch's changes into the current branch.

## 8. Git reset vs Git revert

Both can undo changes, but they work differently.

### `git reset --hard`

```bash
git reset --hard <commit-hash>
```

**What happens:**
- Moves the current branch's `HEAD` to the specified commit.
- Resets the tracked working files and staging area to match that commit.
- Commits after that point are no longer part of the current branch's history.
- Uncommitted changes can be permanently discarded.

Example:

```bash
git reset --hard d4c3456
```

This resets the current branch to commit `d4c3456`.

**Warning:** This is destructive. Use it carefully, especially if the commits have already been pushed or if you have uncommitted work.

### `git revert`

```bash
git revert <commit-hash>
```

**Meaning:** Undoes the changes introduced by a specific commit.

**What it does:**
- Creates a new commit that reverses the selected commit's changes.
- Keeps the original commit in history.
- Is generally safer for commits already shared with others.

Example:

```bash
git revert e5b2345
```

This creates a new commit that reverses the changes made by commit `e5b2345`.

**Remember:** `reset` moves the branch pointer; `revert` adds a new commit to undo changes.

## 9. Remote repositories

A remote is a connection to another repository, usually hosted on GitHub.

### Add a remote

```bash
git remote add origin <repository-url>
```

Connects the local repository to a remote repository.

- `origin` is the conventional remote name.
- It is a nickname, not a special Git keyword.
- This command does not upload code.

### View remote repositories

```bash
git remote -v
```

Displays configured remote URLs.

### Push changes to GitHub

```bash
git push origin main
```

Uploads local commits from `main` to the remote `origin`.

### Push and set upstream

```bash
git push -u origin main
```

Pushes the branch and sets its upstream tracking branch. Later, you can often use:

```bash
git push
```

## 10. Git fetch vs Git pull

### Git fetch

```bash
git fetch origin
```

**Meaning:** Downloads new commits and branch information from the remote repository without integrating them into the current branch.

**What it does:**
- Downloads new commits from GitHub.
- Updates remote-tracking branches such as `origin/main`.
- Does not automatically merge changes into your current branch.
- Allows you to inspect remote changes before integrating them.

To view the fetched history:

```bash
git log --oneline --graph --all
```

To compare your current branch with the fetched remote branch:

```bash
git diff HEAD origin/main
```

To merge the fetched changes into your current branch:

```bash
git merge origin/main
```

### Git pull

```bash
git pull
```

**Meaning:** Fetches remote changes and integrates them into the current branch, typically using merge or rebase depending on configuration.

**Difference:** `git fetch` downloads remote changes; `git pull` downloads and integrates them.

## 11. Git fork

### What is a fork?

A **fork** is a separate copy of another GitHub repository under your own GitHub account.

**Purpose:** It allows you to modify a project without directly changing the original repository.

### Fork workflow

1. Open the original repository on GitHub.
2. Click **Fork**.
3. Select your GitHub account.
4. Click **Create fork**.
5. Clone your fork to your computer.
6. Create a feature branch.
7. Make changes, commit, and push.
8. Open a Pull Request to the original repository.

### Important remote names

- `origin` → Your fork.
- `upstream` → The original repository.

### Fork commands

```bash
# Clone your fork
git clone <your-fork-url>

# Enter the repository directory
cd <repository-folder>

# Add the original repository as upstream
git remote add upstream <original-repository-url>

# Download updates from the original repository
git fetch upstream

# Create and switch to a feature branch
git switch -c feature/my-feature

# Stage and commit your changes
git add .
git commit -m "Add my feature"

# Push the feature branch to your fork
git push -u origin feature/my-feature
```

### Fork vs branch vs clone

| Term | Meaning |
|---|---|
| Fork | Creates a separate repository on GitHub under your account |
| Branch | Creates a development path inside a repository |
| Clone | Downloads a repository to your computer |

## 12. Six steps to contribute using a fork

### Step 1: Fork on GitHub

Create your own copy of the original repository on GitHub.

### Step 2: Clone

```bash
git clone <fork-url>
```

Download your forked repository to your local computer.

### Step 3: Create a branch

```bash
git checkout -b <branch-name>
```

Create and switch to a new branch for your changes.

Alternative:

```bash
git switch -c <branch-name>
```

### Step 4: Commit

```bash
git add .
git commit -m "Add meaningful commit message"
```

Stage and save your changes in Git.

### Step 5: Push

```bash
git push origin <branch-name>
```

Upload your branch and commits to your fork on GitHub.

### Step 6: Open a Pull Request

Request to merge your changes from your fork into the original repository.

## 13. Git merge vs Pull Request

### `git merge`

- A Git command.
- Usually performed locally in the terminal.
- Combines changes from one branch into the branch currently checked out.

Example:

```bash
git switch main
git merge feature1
```

This merges `feature1` into `main`.

### Pull Request (PR)

- A GitHub collaboration feature.
- Proposes merging one branch into another.
- Allows teammates to review code, comment, and approve changes.
- The PR is not merged until someone completes the merge.

### Difference

| `git merge` | Pull Request |
|---|---|
| Git command | GitHub feature |
| Combines branch changes | Proposes and reviews changes |
| Usually done locally | Created and reviewed on GitHub |
| Can be used without GitHub | Commonly used for team collaboration |

**Remember:** A Pull Request is the review process; merging is the operation that combines the branches.

## 14. Three GitHub Pull Request merge options

GitHub provides three common options for merging a Pull Request. Each combines the same code changes but creates a different Git history.

### 1. Merge commit — Keeps everything

- Preserves all individual commits from the feature branch.
- Adds a merge commit showing where the branches joined.
- Useful when you want to preserve the complete development history.

### 2. Squash and merge — Combines commits into one

- Combines all feature-branch commits into one new commit on the target branch.
- Keeps the target branch history cleaner.
- Useful when the feature branch contains many small or temporary commits.

### 3. Rebase and merge — Straight-line history

- Replays each feature-branch commit onto the target branch.
- Keeps individual commits but does not add a merge commit.
- Creates a linear history.

### Quick comparison

| Option | Commits on target branch | Merge commit added? |
|---|---|---|
| Merge commit | All feature commits | Yes |
| Squash and merge | One combined commit | No |
| Rebase and merge | All feature commits, replayed | No |

**Easy way to remember:**

- Merge commit → Keep all commits and join branches.
- Squash and merge → Combine many commits into one.
- Rebase and merge → Keep commits in a straight line.

## 15. Common workflow for a feature branch

```bash
# 1. Switch to the main branch
git switch main

# 2. Download remote updates
git fetch origin

# 3. Update main with the remote branch
git pull origin main

# 4. Create and switch to a feature branch
git switch -c feature/my-feature

# 5. Make code changes, then stage them
git add .

# 6. Commit the changes
git commit -m "Add my feature"

# 7. Push the feature branch
git push -u origin feature/my-feature

# 8. Open a Pull Request on GitHub
```

**Note:** This workflow assumes your local `main` branch tracks `origin/main`. In a fork workflow, make sure you update from the correct remote.

## 16. Useful commands summary

| Command | Purpose |
|---|---|
| `git status` | Check the current repository status |
| `git add .` | Stage changes |
| `git commit -m "message"` | Create a commit |
| `git diff` | Show unstaged changes |
| `git diff --staged` | Show staged changes |
| `git diff HEAD` | Show tracked changes since the last commit |
| `git branch` | List local branches |
| `git branch <name>` | Create a branch |
| `git switch <name>` | Switch branches |
| `git switch -c <name>` | Create and switch to a branch |
| `git merge <branch>` | Merge a branch into the current branch |
| `git log --oneline` | View compact commit history |
| `git fetch origin` | Download remote changes without integrating |
| `git pull` | Fetch and integrate remote changes |
| `git push` | Upload commits to a remote |
| `git remote -v` | View remote URLs |
| `git reset --hard <hash>` | Reset branch and tracked files to a commit |
| `git revert <hash>` | Create a commit that reverses another commit |

---

**Learning tip:** Practice these commands in a separate test repository before using destructive commands such as `git reset --hard` in a shared project.
