# 🐙 Git for Dummies (Complete Guide)

This guide explains how to set up Git, work with commits, branches, merges, rebases, resets, and handle conflicts. If you can't find a solution in here go to [Oh Shit! Git.com](https://ohshitgit.com/)

---

## 🔑 SSH Setup

If you don’t have an SSH key, generate one:

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

This creates two files:
- `~/.ssh/id_ed25519` → private key (**never share**)
- `~/.ssh/id_ed25519.pub` → public key (add this to GitHub/GitLab/etc.)

To copy your public key:

```bash
cat ~/.ssh/id_ed25519.pub
```

Paste the key into your Git provider’s SSH key settings.

---

## 📦 Repositories

- **Clone a repo**:
  ```bash
  git clone <repo-ssh-url>
  ```

- **Create a repo**:
  ```bash
  git init
  ```

---

## 📑 Staging & Committing

- Stage all files (tracks new files too unless ignored in `.gitignore`):
  ```bash
  git add .
  ```

- Commit staged files:
  ```bash
  git commit -m "message"
  ```

- Stage & commit tracked files:
  ```bash
  git commit -a -m "message"
  ```

- Amend the last commit:
  ```bash
  git commit -a --amend --no-edit
  ```
  - Adds staged changes to the previous commit.  
  - `--no-edit` keeps the old commit message.  
  - Without `--no-edit` you’ll be prompted to change the message.  
  - ⚠️ Avoid amending commits that are already pushed unless you use:
    ```bash
    git push --force
    ```

---

## ⚙️ Recommended Setup

Add a `.gitattributes` file:

```
* text=auto
* text eol=lf
```

This ensures consistent line endings across systems, avoiding merge conflicts on newlines.

---

## 🔄 Syncing with Remote

- **Rebase pull** (preferred, cleaner history):
  ```bash
  git pull --rebase
  ```

- If you have uncommitted changes:
  ```bash
  git stash          # saves staged & unstaged changes
  git pull --rebase  # syncs with remote
  git stash pop      # reapplies stashed changes
  ```

---

## 🌿 Branching

- Create a branch:
  ```bash
  git branch my-branch
  ```

- Create and switch:
  ```bash
  git checkout -b my-branch
  ```

- Switch branches:
  ```bash
  git checkout branch-name
  ```

- Merge into main:
  ```bash
  git checkout main
  git merge my-branch
  ```

- Update feature branch after merging:
  ```bash
  git checkout feature-branch
  git merge main
  ```

⚠️ You can use `git rebase main` instead, but rebasing after pushing requires `git push --force`, which can break shared history.

- Delete branches:
  ```bash
  git branch -d branch-name            # local
  git push origin --delete branch-name # remote
  ```

---

## ⚔️ Handling Conflicts

- **Merge conflict**:
  - Edit conflicted files, remove `<<<<<<<`, `=======`, `>>>>>>>`
  - Then:
    ```bash
    git add <file>
    git commit
    ```

- **Rebase conflict**:
  ```bash
  git add <file>
  git rebase --continue
  ```
  - Skip commit: `git rebase --skip`
  - Abort rebase: `git rebase --abort`

👉 If `git pull --rebase` causes too many conflicts, do:
```bash
git rebase --abort
git pull   # fallback to normal merge
```

- Recommended tools for resolving conflicts: **VS Code Merge Editor** or **IntelliJ Merge Tool**.  

---

## 🔍 Status & Branch Info

- Show branch and status:
  ```bash
  git status
  ```

- List branches (current has `*`):
  ```bash
  git branch
  ```

- Print current branch only:
  ```bash
  git rev-parse --abbrev-ref HEAD
  ```

---

## ⏪ Rolling Back Commits

- Show history:
  ```bash
  git log --oneline
  ```

- Show reflog (HEAD time machine, includes lost commits):
  ```bash
  git reflog
  ```

- Checkout an old commit (detached HEAD, for testing only):
  ```bash
  git checkout <commit-hash>
  ```

- Reset to a commit:
  ```bash
  git reset --hard <commit>   # reset & discard changes
  git reset --soft <commit>   # reset & keep staged
  git reset --mixed <commit>  # reset & keep unstaged
  ```

👉 Rules of thumb:
- **soft** → undo commit but keep everything staged (good for fixing a message)  
- **mixed** → undo commit but keep changes unstaged (default, flexible cleanup)  
- **hard** → nuke changes and reset completely (dangerous)  

⚠️ Good practice:
- Run `git checkout <commit>` first to preview state.  
- If stable, switch back to main and reset:
  ```bash
  git checkout main
  git reset --hard <commit>
  ```

---

## 🎒 Stashing

- Save changes (staged + unstaged):
  ```bash
  git stash
  ```

- View stash list:
  ```bash
  git stash list
  ```

- Apply stash (and remove from stack):
  ```bash
  git stash pop
  ```

- Apply stash but keep in stack:
  ```bash
  git stash apply
  ```

👉 Workflow example:
1. `git stash`
2. `git reset ...`
3. `git stash pop`

---

# ✅ Best Practices

- Keep a stable `main` branch.  
- Do work in feature branches, merge them back into `main`.  
- Prefer `git pull --rebase` for clean history.  
- Use **stash** to hide unfinished work temporarily.  
- Use **reset** carefully:
  - `--soft` = staged
  - `--mixed` = unstaged
  - `--hard` = erased  
- Use **reflog** to recover lost commits.  
- Merge for teamwork safety; rebase for clean solo history.  
