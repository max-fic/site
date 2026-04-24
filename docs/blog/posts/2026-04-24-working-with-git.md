---
title: iWorking with git
description: How to use git for managing your source file
date: 2026-04-24
authors:
  - max
tags:
  - git
---

`Git` is a version control system for your files. You can look at as a `undo` button for your files. E.g. if you screwed up your file it will allow you to go back to newer version. Let us explore how to use git, what are typical workflows and best practices are.


<!-- more -->


## Setting up `git`

The following commands bring all your files in the current directory and subdirectories under `git` control. The last command

```bash
git add .
git commit -m "Initial commit"
git push -u origin main
```

---

## Merging a feature branch into main

Merging locally gives you full control before anything goes public. Here's the exact sequence using a branch called `blog-update` as an example.

### 1. Save your work

Make sure the feature branch is clean before switching away from it.

```bash
git add .
git commit -m "Finish blog updates"
```

### 2. Switch to main

You merge **into** the branch you're standing on.

```bash
git checkout main
```

### 3. Sync with GitHub (optional but recommended)

If you've made any changes directly on GitHub, pull them first so your local `main` is up to date.

```bash
git pull origin main
```

### 4. Merge

```bash
git merge blog-update
```

Two outcomes are possible:

- **Fast-forward** — `main` hasn't changed since you branched off, so Git just moves the pointer forward. Clean and instant.
- **Recursive merge / conflict** — both branches have new changes. Git combines them or asks you to resolve conflicts.

### 5. Push

```bash
git push origin main
```

This is usually what triggers the GitHub Actions workflow to rebuild and deploy the site.

---

## Resolving merge conflicts

If the merge fails with "Automatic merge failed", don't panic. It just means the same line was edited in both branches.

1. Run `git status` to see which files are conflicted.
2. Open each file — look for `<<<<<<< HEAD` and `>>>>>>> blog-update` markers.
3. Delete the markers and keep the version you want.
4. Stage and commit:

```bash
git add <filename>
git commit -m "fix: resolve merge conflict"
git push origin main
```

---

## Clean up

Once a branch is merged, you can safely delete it:

```bash
git branch -d blog-update
```

### Staging and unstaging a File
 Staging a specific file
```bash
git add <filename>
```

Staging everything under `git` control:
```bash
git add .
```

Unstaging a file:
```bash
git restore --stage <filename>
```


### Committing changes and undoing commits

Commit everything that has been staged
```bash
git commit -m "Commit Message"
```

Committing a specific file (does not have to be staged yet):
```bash
git commit <filename> -m "Commit Message"
```

Undoing a commit and unstaging the files last committed.
```bash
git reset HEAD~1
```

Undoing a commit but leaving the files staged (e.g. to add another file to the same commit).
```bash
git reset --soft HEAD~1
```

Resetting the directory to the state of the last of the previous commits. This resets the HEAD, unstages any staged files, removes any uncommited changes. It will delete files that were added after the lst previous commit.
```bash
git reset --hard HEAD~1
```



