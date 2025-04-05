# Git-commands
# Git Cheat Sheet

## Core Git Commands
- `git switch branch-name` – Switch to an existing branch (modern and clear)
- `git switch -c new-branch` – Create and switch to a new branch
- `git checkout branch-name` – Switch to a branch (older syntax)
- `git checkout -b new-branch` – Create and switch to a new branch (older style)
- `git status` – See current file state (staged, modified, untracked)
- `git add myfile.txt` – Stage a file for commit
- `git commit -m "message"` – Commit staged changes with a message
- `git log` – View commit history
- `git log --oneline` – Compact commit history (short hash and message)
- `git log --graph --oneline --all` – Visual tree of branches and commits
- `git diff` – Show changes (unstaged)
- `git diff --staged` – Show changes staged for commit
- `git push` – Push local commits to remote
- `git pull` – Fetch and merge changes from remote
- `git clone repo-url` – Clone a remote repository
- `git init` – Start a new Git repository

## File Restoration & Inspection
- `git restore myfile.txt` – Discard unstaged changes
- `git restore --staged myfile.txt` – Unstage a file
- `git restore --source=HEAD~1 myfile.txt` – Restore file from previous commit
- `git show commit_hash` – Show details of a specific commit
- `git blame file.txt` – See who last changed each line of a file

## Troubleshooting & Debugging
- `git stash` – Temporarily save your work
- `git stash pop` – Reapply last stash
- `git stash list` – See saved stashes
- `git stash show -p` – See changes in last stash
- `git reflog` – View full history of HEAD and branch changes
- `git bisect` – Find commit that introduced a bug (binary search)
- `git reset HEAD myfile.txt` – Unstage a staged file
- `git reset --hard HEAD` – Discard all changes (careful!)

## Cleanup & Branch Management
- `git branch -d branch-name` – Delete merged local branch
- `git branch -D branch-name` – Force delete local branch
- `git push origin --delete branch-name` – Delete a remote branch
- `git remote prune origin` – Remove deleted remote branches from local tracking
- `git clean -n` – Preview untracked files to be deleted
- `git clean -f` – Delete untracked files
- `git clean -fd` – Delete untracked files and directories
- `git branch --merged` – Show branches already merged
- `git branch --no-merged` – Show branches not yet merged
- `git gc --prune=now` – Clean up unnecessary files and optimize repo

## Create Pull Request (PR) from Terminal
- `gh auth login` – Login to GitHub CLI
- `gh pr create` – Start an interactive PR creation
- `gh pr create --base main --head feature-branch --title "Title" --body "Description"` – Create a PR with all fields filled
- `git push origin feature-branch` – Push feature branch to remote before creating PR

## Renaming Branches & Amending Commits

### Rename a Local Branch

| **Command** | **Why Use It** | **Instead of** | **When Not to Use It** |
|-------------|----------------|----------------|-------------------------|
| `git branch -m new-name` | Rename the current branch locally | `git branch -m old-name new-name` | Don't rename if branch has already been pushed and others are using it |
| `git branch -m old-name new-name` | Rename a **different** local branch | N/A | Avoid renaming active shared branches without informing the team |

**Push renamed branch to remote:**
```bash
git push origin -u new-name
```

**Delete old branch from remote (if needed):**
```bash
git push origin --delete old-name
```

---

### Amend a Commit Message

| **Command** | **Why Use It** | **Instead of** | **When Not to Use It** |
|-------------|----------------|----------------|-------------------------|
| `git commit --amend -m "New message"` | Fix typo or clarify message of **last commit** | Making a new commit just to fix the message | Don't amend if the commit is already pushed to shared branch — it rewrites history |

> **Why?** Amending rewrites history. If the commit is already shared with others, they will get conflicts unless they force-pull or reset.

---

### Change Older Commit Messages (More Than One Back)

| **Command** | **Why Use It** | **Instead of** | **When Not to Use It** |
|-------------|----------------|----------------|-------------------------|
| `git rebase -i HEAD~N` | Edit commit messages from last `N` commits | Trying to cherry-pick and squash manually | Don’t use this on commits already pushed to remote unless you know how to handle force-push conflicts |

**Steps:**
1. Run:
   ```bash
   git rebase -i HEAD~3  # for the last 3 commits
   ```
2. Change `pick` to `reword` for the commit you want to edit.
3. Save and edit the message when prompted.

---

### Summary: When to Use or Avoid

| **Action** | **Use It When…** | **Avoid When…** |
|------------|------------------|------------------|
| Renaming a branch | The name is wrong, temporary, or unclear | Others are already using the branch remotely |
| Amending a commit | You just committed and noticed a typo | You've already pushed it to shared remote |
| Interactive rebase | You need to clean up commit history before pushing | You've already pushed and rebasing would cause conflicts |
