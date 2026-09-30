# github practice
# GitHub-Practice: Git, GitHub & Networking Practical Tasks

A hands-on practice repository documenting my Git, GitHub and networking exercises. It covers everyday version-control workflows (commits, branching, merging, reverting, ignoring files, resolving conflicts, stashing) and basic network troubleshooting with Windows PowerShell.

**Environment:** Windows 10/11, PowerShell / Git Bash / VS Code terminal
**Level:** Beginner to DevOps fundamentals

---

## Table of Contents

1. [Project Overview](#project-overview)
2. [Repository Structure](#repository-structure)
3. [Task Checklist](#task-checklist)
4. [Part 1: Git and GitHub Fundamentals](#part-1-git-and-github-fundamentals)
5. [Part 2: Resolving a Merge Conflict](#part-2-resolving-a-merge-conflict)
6. [Part 3: Git Stash](#part-3-git-stash)
7. [Part 4: Networking Practicals](#part-4-networking-practicals)
8. [Key Concepts Learned](#key-concepts-learned)
9. [Review-Call Q&A](#review-call-qa)
10. [Command Cheat Sheet](#command-cheat-sheet)

---

## Project Overview

The goal of this repository was to practice the core Git workflow end to end: create a local repository, make several commits, push it to GitHub, work with branches, undo changes safely, handle conflicts, and temporarily shelve unfinished work. I also completed a set of networking tasks to understand IP addressing, connectivity testing, routing and DNS.

## Repository Structure

```
GitHub-Practice/
├── index.html        # Commit 1: basic HTML page
├── style.css         # Commit 2: basic stylesheet
├── app.js            # Commit 3: basic JavaScript file
├── README.md         # Commit 4: this documentation
├── config.json       # Commit 5: sample configuration file
├── testing.txt       # Added on the testing branch, merged into main
├── conflict.txt      # Used for the merge conflict exercise
├── stash-demo.txt    # Used for the git stash exercise
├── .gitignore        # Ignores *.log files
└── app.log           # Sample log file (ignored, not tracked)
```

## Task Checklist

- [x] Create a repository and make 5 separate commits
- [x] Create, merge and delete a testing branch
- [x] Display the last 3 commits
- [x] Revert a specific commit
- [x] Create `.gitignore` and ignore `.log` files
- [x] Rename `master` to `main`
- [x] Create two branches, modify the same line, merge, identify and resolve the conflict
- [x] Create and modify a file, stash, view the stash list, restore
- [x] Networking: IP addresses, subnet mask, default gateway, ping, traceroute, DNS lookups

---

## Part 1: Git and GitHub Fundamentals

### Task 1: Create a repository and make 5 separate commits

**Objective:** Create a Git repository, add five different files and save each in its own commit.

**Step 1: Create the project folder**

```powershell
mkdir GitHub-Practice
cd GitHub-Practice
```

- `mkdir` creates a new directory.
- `cd` moves into it. This folder is the local Git project.

**Step 2: Initialize Git**

```bash
git init
git status
```

`git init` creates a hidden `.git` directory that stores the repository's history, commits, branches and configuration. `git status` shows the current state of the working tree.

**Step 3: Configure Git identity**

```bash
git config --global user.name "Your Name"
git config --global user.email "your-email@example.com"
git config --global --list
```

Using the same email as my GitHub account links commits to my GitHub profile.

**Step 4: Create five files and commit each one separately**

| # | File | Commands | Commit message |
|---|------|----------|----------------|
| 1 | `index.html` | `Set-Content index.html "<h1>Welcome to GitHub Practice</h1>"` | Add index.html |
| 2 | `style.css` | `Set-Content style.css "body { font-family: Arial; }"` | Add style.css |
| 3 | `app.js` | `Set-Content app.js "console.log('Git practice');"` | Add app.js |
| 4 | `README.md` | `Set-Content README.md "# GitHub Practice"` | Add README |
| 5 | `config.json` | `Set-Content config.json '{"environment":"development"}'` | Add config file |

For each file the pattern was:

```bash
git add <file>
git commit -m "<message>"
```

`git add` stages the file; `git commit` records the staged change permanently in the repository history.

**Step 5: Verify the commits**

```bash
git log --oneline
```

Expected output (IDs will differ):

```
a12bc34 Add config file
b23cd45 Add README
c34de56 Add app.js
d45ef67 Add style.css
e56fa78 Add index.html
```

**Step 6: Create a GitHub repository and push**

1. Sign in to GitHub and click **New repository**.
2. Name it `GitHub-Practice` and choose Public or Private.
3. Leave "Initialize with a README" **unchecked** because a local README already exists.
4. Click **Create repository**.
5. Connect and push:

```bash
git remote add origin https://github.com/YOUR-USERNAME/GitHub-Practice.git
git branch -M main
git push -u origin main
```

**Result:** one local repository, five separate commits, one remote GitHub repository, and a `main` branch pushed to GitHub. This workflow lets developers share code for collaboration, code review and CI/CD pipelines.

---

### Task 2: Create a testing branch, merge and delete it

**Step 1: Create and switch to the branch**

```bash
git switch -c testing
git branch
```

The asterisk (`*`) marks the current branch.

**Step 2: Make a change on the branch**

```powershell
Set-Content testing.txt "This change is from the testing branch."
git add testing.txt
git commit -m "Add testing file"
```

**Step 3: Switch back to main and merge**

```bash
git switch main
git merge testing
```

Because `testing.txt` is a new file and there are no competing changes, this is a straightforward merge.

**Step 4: Delete the branch**

```bash
git branch -d testing
git branch
```

The `-d` flag only deletes a branch that has been fully merged. If the branch had been pushed to GitHub, the remote copy can be removed with:

```bash
git push origin --delete testing
```

**Why it matters:** A feature or testing branch isolates changes from `main`. Once reviewed and merged, the temporary branch is deleted to keep the branch list clean.

---

### Task 3: Display only the last 3 commits

```bash
git log -3 --oneline
git log -3 --oneline --graph --decorate
```

| Option | Meaning |
|--------|---------|
| `git log` | Shows commit history |
| `-3` | Limits output to three commits |
| `--oneline` | One line per commit |
| `--graph` | Draws branch and merge relationships |
| `--decorate` | Shows branch and tag names |

---

### Task 4: Revert a specific commit

`git revert` creates a **new commit** that reverses the changes of an earlier commit while preserving the original in history.

```bash
git log --oneline              # find the commit ID
git revert c34de56             # revert the chosen commit (opens an editor)
git revert --no-edit c34de56   # same, skipping the editor when changes apply cleanly
git log --oneline              # verify the new revert commit appears
```

**Revert vs reset**

| Command | Purpose |
|---------|---------|
| `git revert <commit>` | Creates a new commit that reverses previous changes |
| `git reset --soft <commit>` | Moves HEAD, keeps changes staged |
| `git reset --mixed <commit>` | Moves HEAD, leaves changes unstaged |
| `git reset --hard <commit>` | Moves HEAD and discards tracked changes after the target commit |

For commits already shared with others, `git revert` is generally preferable because it does not rewrite shared history.

---

### Task 5: Create `.gitignore` and ignore `.log` files

A `.gitignore` file tells Git which untracked files to ignore (logs, temp files, build output, local environment files).

```powershell
Set-Content app.log "This is a sample log"
git status                       # app.log shows as untracked
Set-Content .gitignore "*.log"
git add .gitignore
git commit -m "Add gitignore for log files"
git status                       # app.log no longer appears
git check-ignore -v app.log      # shows which rule ignores the file
```

**Important:** `.gitignore` does not stop tracking files that were already committed. To untrack a file but keep it on disk:

```bash
git rm --cached app.log
git commit -m "Stop tracking log file"
```

---

### Task 6: Rename the default branch from `master` to `main`

```bash
git branch                       # check current branch
git branch -m master main        # rename locally
git push -u origin main          # push the new branch
```

Then on GitHub: **Settings → Branches →** set the default branch to `main`.

After confirming `main` is the default and contains all needed commits, the old branch can be removed:

```bash
git push origin --delete master
```

If the old branch is protected or used by a pull request or deployment, resolve those dependencies first.

---

## Part 2: Resolving a Merge Conflict

A merge conflict happens when Git cannot automatically combine changes, for example when two branches modify the same line differently.

**Scenario**

| Branch | Line content |
|--------|--------------|
| `main` (original) | Hello from original |
| `conflict-branch` | Hello from testing |
| `main` (later change) | Hello from main |

**Step 1: Create the original file on main**

```powershell
git switch main
Set-Content conflict.txt "Hello from original"
git add conflict.txt
git commit -m "Add original conflict file"
```

**Step 2: Create the first branch and change the line**

```powershell
git switch -c conflict-branch
Set-Content conflict.txt "Hello from testing"
git add conflict.txt
git commit -m "Update greeting in conflict branch"
```

**Step 3: Switch to main and change the same line**

```powershell
git switch main
Set-Content conflict.txt "Hello from main"
git add conflict.txt
git commit -m "Update greeting in main"
```

**Step 4: Merge and trigger the conflict**

```bash
git merge conflict-branch
```

Expected message:

```
Auto-merging conflict.txt
CONFLICT (content): Merge conflict in conflict.txt
Automatic merge failed; fix conflicts and then commit the result.
```

**Step 5: Resolve manually**

Open the file (`code conflict.txt`) and find the markers:

```
<<<<<<< HEAD
Hello from main
=======
Hello from testing
>>>>>>> conflict-branch
```

`HEAD` is the current branch (`main`); the lower section is the incoming change from `conflict-branch`. I removed all markers and kept a combined version:

```
Hello from main and testing
```

**Step 6: Stage and commit the resolution**

```bash
git add conflict.txt
git commit -m "Resolve merge conflict"
```

**Step 7: Verify**

```bash
git status
git log --oneline --graph --decorate -5
```

The working tree is clean and the history shows both branches merged.

---

## Part 3: Git Stash

`git stash` temporarily saves uncommitted changes so you can switch context (for example, to fix an urgent production issue) without making an unfinished commit.

**Step 1: Create a file and commit it**

```powershell
Set-Content stash-demo.txt "Original version"
git add stash-demo.txt
git commit -m "Add stash demo file"
```

**Step 2: Modify the file**

```powershell
Set-Content stash-demo.txt "Modified version - work in progress"
git status
git diff
```

**Step 3: Stash the changes**

```bash
git stash
# or, with a descriptive message:
git stash push -m "My unfinished changes"
```

Use one or the other for a given change. By default untracked files are not stashed; add `-u` to include them.

**Step 4: View the stash list**

```bash
git stash list
```

Example: `stash@{0}: WIP on main: a12bc34 Add stash demo file` (`stash@{0}` is the most recent).

**Step 5: Restore the stash**

```bash
git stash apply              # restore and keep the stash entry
git stash pop                # restore and remove the stash entry
git stash apply 'stash@{0}'  # apply a specific stash
```

**Step 6: Clean up**

```bash
git stash drop 'stash@{0}'   # remove one stash
git stash clear              # remove all stashes
```

> Stashing is for temporarily switching context. It is not a replacement for committing important work or backing up code.

---

## Part 4: Networking Practicals

Performed in Windows PowerShell. The commands below are the standard ones for each task; replace the placeholder values with the results from your own machine.

| Task | Command | What it shows |
|------|---------|---------------|
| Private IPv4, subnet mask, default gateway | `ipconfig` (or `ipconfig /all`) | IPv4 address, subnet mask and default gateway of the active adapter |
| Public IPv4 | `(Invoke-RestMethod https://api.ipify.org)` | The address the internet sees for your network |
| Ping Google DNS | `ping 8.8.8.8` | Connectivity and round-trip time to Google Public DNS |
| Ping router | `ping <default-gateway-IP>` | Connectivity to your local router |
| Trace route and count hops | `tracert 8.8.8.8` | Each router (hop) between you and the target; the number of lines is the hop count |
| DNS lookup (default server) | `nslookup google.com` | IP addresses for the domain using your default DNS server |
| DNS lookup (Google DNS) | `nslookup google.com 8.8.8.8` | Same lookup via Google DNS |
| DNS lookup (Cloudflare DNS) | `nslookup google.com 1.1.1.1` | Same lookup via Cloudflare DNS |

### My results

| Item | Value |
|------|-------|
| Private IPv4 address | _fill in_ |
| Public IPv4 address | _fill in_ |
| Subnet mask | _fill in_ |
| Default gateway | _fill in_ |
| Ping to 8.8.8.8 (avg) | _fill in_ ms |
| Ping to router (avg) | _fill in_ ms |
| Hops to 8.8.8.8 | _fill in_ |

---

## Key Concepts Learned

- **Repository:** a project folder tracked by Git, with history stored in `.git`.
- **Commit:** a saved snapshot of staged changes.
- **Branch:** an isolated line of development.
- **Merge:** combining changes from one branch into another.
- **Merge conflict:** competing changes to the same lines that Git cannot combine automatically.
- **Revert vs reset:** revert adds a new commit that undoes a change; reset moves HEAD and can rewrite history.
- **`.gitignore`:** prevents untracked files from being added, but does not untrack files already committed.
- **Stash:** a temporary shelf for uncommitted changes.
- **Remote:** a hosted copy of the repository (GitHub), named `origin` by convention.

## Review-Call Q&A

**How do you resolve a merge conflict?**
I identify conflicted files with `git status`, open them, inspect the conflicting changes from both branches, and manually choose or combine the correct code. I remove the conflict markers, save, stage with `git add`, commit the resolution, and verify with `git status` and `git log`.

**What is the difference between `git stash apply` and `git stash pop`?**
`apply` restores the saved changes and keeps the stash entry. `pop` restores the changes and removes the entry if it applies successfully.

**Why use `git revert` instead of `git reset` on shared branches?**
Revert adds a new commit and preserves history, so it does not disrupt collaborators. Reset rewrites history.

**Why did `.gitignore` not hide a file that was already committed?**
Git keeps tracking files that are already in the index. Use `git rm --cached <file>` to stop tracking it.

## Command Cheat Sheet

```bash
git init                          # create a repository
git status                        # working tree status
git add <file>                    # stage a file
git commit -m "message"           # commit staged changes
git log --oneline                 # compact history
git switch -c <branch>            # create and switch to a branch
git switch <branch>               # switch branches
git merge <branch>                # merge a branch into the current one
git branch -d <branch>            # delete a merged branch
git revert <commit>               # undo a commit with a new commit
git branch -m master main         # rename branch
git remote add origin <url>       # link a remote
git push -u origin main           # push and set upstream
git stash / git stash pop         # shelve and restore changes
```
