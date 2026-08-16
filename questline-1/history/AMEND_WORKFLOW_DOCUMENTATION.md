# Git Commit Amendment: Complete Workflow Documentation

## Overview

The `git commit --amend` command is a powerful Git feature that allows you to modify the most recent commit on your current branch. This documentation covers the complete workflow, use cases, and best practices.

## What is Commit Amendment?

Commit amendment is the process of modifying the last commit on your current branch without creating a new commit. This includes:
- Adding forgotten files
- Changing the commit message
- Removing unwanted changes
- Combining related changes into a single commit

## When to Use Commit Amendment

### ✅ Ideal Scenarios

```
Scenario 1: Forgot to Include a File
├─ Made first commit with file A
├─ Realized file B should be in the same commit
├─ Solution: Stage file B and amend
└─ Result: Single commit with both A and B

Scenario 2: Typo in Commit Message
├─ Committed with message: "Add feture to system"
├─ Need to fix: "feature" is misspelled
├─ Solution: Use --amend -m with corrected message
└─ Result: Same commit, corrected message

Scenario 3: Code Review Feedback
├─ Commit made and pushed to PR
├─ Reviewer suggests small improvements
├─ Solution: Make changes, stage, and amend (if not yet merged)
└─ Result: PR shows updated commit without extra commits
```

### ❌ When NOT to Use Amendment

```
Issue 1: Already Pushed to Shared Repository
├─ You amended a commit locally
├─ Others have already pulled the old version
├─ Problem: History rewrite causes conflicts
└─ Solution: Use git revert for already-pushed commits

Issue 2: Multiple Commits to Fix
├─ You have 5 commits you want to clean up
├─ Amendment only works on HEAD
├─ Solution: Use interactive rebase (git rebase -i)

Issue 3: Public/Shared Branches
├─ Working on origin/main or origin/develop
├─ Multiple developers depend on commit history
├─ Problem: Rewriting history breaks for others
└─ Solution: Only amend on local branches
```

## Amendment Workflow: Step-by-Step

### Scenario: Adding a Forgotten File

```
BEFORE:
$ git log --oneline -3
abc1234 Add feature X          ← Want to amend this
def5678 Fix bug Y
ghi9012 Initial commit

STEP 1: Make Changes
$ vim forgotten_file.js
$ git add forgotten_file.js

STEP 2: Amend the Commit
$ git commit --amend --no-edit
[main xyz7890] Add feature X
 Date: Fri Aug 16 10:00:00 2026 +0000
 2 files changed, 50 insertions(+)

AFTER:
$ git log --oneline -3
xyz7890 Add feature X          ← Now includes forgotten file
def5678 Fix bug Y
ghi9012 Initial commit
```

### Scenario: Fixing Commit Message

```
BEFORE:
$ git log --oneline
abc1234 Fix bugg in login sistem

STEP 1: Amend with Correct Message
$ git commit --amend -m "Fix bug in login system"

AFTER:
$ git log --oneline
xyz7890 Fix bug in login system
```

## Amendment Command Reference

### Basic Amendment (Keep Message)
```bash
git add forgotten_file.txt
git commit --amend --no-edit
```

### Amendment with New Message
```bash
git add forgotten_file.txt
git commit --amend -m "New commit message"
```

### Amendment Interactive Editor
```bash
git add forgotten_file.txt
git commit --amend
# Opens editor to modify message
```

### Amendment with All Changes
```bash
# Stage specific files
git add file1.txt file2.txt
# Amend to include them
git commit --amend --no-edit
```

### Amendment Empty Commit
```bash
git commit --amend --allow-empty
```

## Real-World Example: Task Execution

This is how the task was completed:

### Initial Setup
```bash
cd /workspaces/ACMRecruitment-Chjathin
git status
# On branch main, working tree clean
```

### Step 1: Create Initial Commit
```bash
# Modified: skills.txt (added Advanced Topics section)
git add skills.txt
git commit -m "Add Advanced Topics section to skills documentation"
# Result: Commit 5d2323d created
```

### Step 2: Realize Additional File Needed
```bash
# Need to also update README.md with same content
vim README.md
# Added Advanced Topics section to README.md
```

### Step 3: Amendment Process
```bash
git add README.md
git commit --amend --no-edit
# Result: Commit hash changes to 3e1f147
#         Both files now in single commit
```

### Step 4: Verification
```bash
git log --oneline -2
# 3e1f147 Add Advanced Topics section to skills documentation ← Amended
# 67f363f Add Git rebase workflow: ...

git show --stat HEAD
# Shows both README.md and skills.txt in the commit
```

## Amendment vs. Other Approaches

### Amendment vs. New Commit
```
Using Amendment:
✓ Single logical commit
✓ Clean history
✓ One message describing all changes
- Only works on most recent commit

Using New Commit:
✗ Multiple commits for related work
✗ Cluttered history
✗ Harder to review
+ Can fix any previous commit (with rebase)
```

### Amendment vs. Revert
```
Amendment (For Unpushed Commits):
- Modifies history locally
- Clean approach for local changes
- Cannot use on public branches
- Requires: commit not yet shared

Revert (For Pushed Commits):
+ Creates new commit undoing changes
+ Safe for shared/public branches
+ Others can pull the revert
+ No history rewriting
```

### Amendment vs. Interactive Rebase
```
Amendment:
- Only affects most recent commit
- Simple and quick
+ Good for recent commits

Interactive Rebase (git rebase -i):
- Can modify multiple commits
- More powerful but complex
+ Better for restructuring history
+ Allows reordering, squashing, editing
```

## Best Practices

### ✅ DO

1. **Amendment Before Pushing**
   ```bash
   # Safe to amend
   git commit -m "Add feature"
   git add forgotten_file.txt
   git commit --amend
   # Then push once
   git push origin main
   ```

2. **Clear Commit Messages**
   ```bash
   # Use meaningful messages
   git commit --amend -m "Add API endpoint for user authentication"
   ```

3. **Small, Related Changes**
   ```bash
   # Amendment works best for closely related files
   git add module.py tests/test_module.py
   git commit --amend --no-edit
   ```

4. **Review Before Amendment**
   ```bash
   # Check what you're amending
   git log --stat HEAD~1..HEAD
   git add new_file.txt
   git commit --amend
   ```

### ❌ DON'T

1. **Don't Amend After Pushing**
   ```bash
   # ❌ BAD
   git commit -m "Fix bug"
   git push origin main
   git add more_fixes.txt
   git commit --amend
   # Others will have problems!
   ```

2. **Don't Amend Shared Commits**
   ```bash
   # ❌ BAD - On a shared branch
   git checkout main
   git commit -m "Add feature"
   git push origin main
   git commit --amend  # Don't do this!
   ```

3. **Don't Amend for Unrelated Changes**
   ```bash
   # ❌ BAD - Different logical changes
   git add feature_A.js
   git commit -m "Add feature A"
   git add bug_fix_B.js
   git commit --amend  # Should be separate commit
   ```

4. **Don't Force Push Without Reason**
   ```bash
   # After amendment, DO NOT force push without coordination
   git commit --amend
   git push -f origin main  # ❌ Dangerous on shared branches
   ```

## Common Pitfalls and Solutions

### Pitfall 1: Amendment on Wrong Branch
```
Problem:
$ git branch
* dev
  main
$ git commit --amend  # Working on dev instead of main

Solution:
$ git checkout main
$ git commit --amend
```

### Pitfall 2: Lost Changes from Amendment
```
Problem:
$ git commit --amend
# Accidentally removed staged changes

Solution:
$ git reflog  # See all commits including amended ones
$ git show abc1234  # Recover old commit
```

### Pitfall 3: Conflict with Remote
```
Problem:
$ git commit --amend
$ git push origin main
# Rejects push: "non-fast-forward"

Solution:
# Never amend after pushing to shared branches
# Use: git revert instead
```

## Advanced Techniques

### Combine Amendment with Rebase
```bash
# Fix multiple commits
git rebase -i HEAD~5
# Then amend individual commits in interactive rebase
```

### Amendment with Signoff
```bash
git commit --amend -s
# Adds sign-off line to commit message
```

### Preserve Original Date on Amendment
```bash
git commit --amend --no-edit --date="original"
# Keeps original commit date instead of current time
```

## Undoing an Amendment

### If Amendment Not Yet Pushed
```bash
# Use reflog to find original commit
git reflog
# abc1234@{1} (HEAD@{1}): commit: Original message

# Reset to original
git reset --soft abc1234
```

### If Amendment Already Pushed
```bash
# Use git revert instead of amendment in future
git revert HEAD
# Creates new commit undoing the changes
```

## Summary Checklist

Before using `git commit --amend`:

- [ ] Commit not yet pushed to shared repository
- [ ] Working on local branch
- [ ] All desired changes staged with `git add`
- [ ] Reviewed changes with `git diff --staged`
- [ ] Confirmed correct branch with `git branch`
- [ ] Understood implications for collaborators

---

**Complete Documentation:** August 16, 2026  
**Purpose:** Comprehensive Guide to Git Commit Amendment  
**Difficulty:** Intermediate
