---
title: Getting Started with MkDocs on GitHub
description: How to set up an MkDocs project, connect it to GitHub, and manage changes with feature branches.
date: 2026-04-23
authors:
  - max
tags:
  - mkdocs
  - git
  - github
---

Setting up an MkDocs site and publishing it to GitHub Pages only takes a few commands — but the Git workflow around it trips people up at first. Here's the sequence I use.

<!-- more -->

## Create the project

```bash
mkdocs new <project>
```

## Create a ssh key for communication with GitHub (if you're new to github)

== Mac Terminal ==

1. Create a .ssh directory 
```bash
$ cd $HOME
$ mkdir .ssh
$ cd .ssh
```

2.  now generate the ssh key
```bash
$ ssh-keygen -t ed25519 -C "your_email@example.com"
```

3. Edit or create the config file. Use an editor to open `config` and enter the following information:

```plaintext
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519  # The file ssh-keygen made
  IdentitiesOnly yes
```

4. Copy the contents of the file `id_ed25519.pub` to the clipboard.

```bash
$ pbcopy < ~/.ssh/id_ed25519.pub
```
## Create a new GitHub repository
Then create an **empty** repository on GitHub (no README, no license — keep it totally empty), and link your local project to it:

```bash
cd <project>
git init
git branch -M main
git remote add origin git@github.com:<username>/<repo>.git
```

!!! note "SSH key"
    If you're connecting via SSH, make sure your key is in `~/.ssh/` and added to your GitHub account.

Stage and push your initial files:

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
