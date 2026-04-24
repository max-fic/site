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



Setting up a `mkdocs` site and publishing it to GitHub Pages only takes a few commands — but the Git workflow around it trips people up at first. 

<!-- more -->

`git` is a version control system for file. It allows you to manage different versions of your files (keeping older versions, showing what has been done in which version ...).

No worries, one needs not to learn or use `git` when writing stuff for your website. We only need to install it as it is necessary for `mkdocs` to talk to GitHub.

The following steps show how to connect your project to GitHub.


## Connect a `mkdocs` project to GitHub

The following assumes that you have already a `mkdocs` site locally.

#### 1, Create a new GitHub repository
Create an **empty** repository on GitHub (no README, no license — keep it totally empty), and link your local project to it:

==On GitHub==

1. Click on your icon, then `Repositories`, then `New`

2. Enter the name of the repository

4. Make sure, you have no README.md, no license file

#### 2. Set up `git` and link your local repository to GitHub 

No we must activate git in our directory and tell git how to send our files to GitHub.

==Mac Terminal==
```bash
cd <project>
git init
git branch -M main
git remote add origin git@github.com:<username>/<repo>.git
```

!!! note "SSH key"
    If you're connecting via SSH (as in the example above), make sure your key is in `~/.ssh/` and added to your GitHub account (see the steps at the end).

## Deploy your site on GitHub

Once connected everything is very easy, especially if you don't use `git` for the management of the source files (mkdocs.yml, *.md files ...).

To deploy you simply build the site and instruct `mkdocs` to send the generated site to GitHub

```bash

mkdocs build --strict
mkdocs gh-deploy
```

Once run your site is visible under 

`<yourgitname>.github.io/<project>`

!!! Tip 
    If you do not want to use git for managing the versions of your 
    source files you can forget about `git`. Just work on your files, 
    add, edit or delete your files. Do not forget to build and deploy 
    your website after changes

## Appendix: Create a ssh key for communication with GitHub (if you're new to GitHub)

If you haven't used `git` with GitHub yet, you must set a `ssh` key for secure communications with GitHub. This key authenticates you with GitHub and allows you to send files to your repository.

The key creation process has essentially two steps. First we generate a key locally which creates two keys, private and public. The private key stays stored on our computer. The public key will be given to GitHub.

==Mac Terminal==

#### 1. Create a .ssh directory 
```bash
$ cd $HOME
$ mkdir .ssh
$ cd .ssh
```

#### 2. Generate the ssh key
```bash
$ ssh-keygen -t ed25519 -C "<your_email@example.com>"
```

#### 3. Edit or create the config file
Use an editor to open `config` and enter the following information:

```plaintext
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519  # The file ssh-keygen made
  IdentitiesOnly yes
```

#### 4. Copy the contents of the file `id_ed25519.pub` to the clipboard.

```bash
$ pbcopy < ~/.ssh/id_ed25519.pub
```

==On GitHub==

#### 5. Paste clipboard contents into GitHub

1. On GitHub click your profile icon, then `Settings`, then `SSH and GPG keys`

2. Click on `New SSH key`

3. In the `Title` Box, give a name for the key
4. Paste the clipboard contents into the `Text` box
5. Press the `Add SSH key` button

And "Voilá" we can now send files from our local repository to GitHub.
