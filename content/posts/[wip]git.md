+++
title = "Learning Git"
date = "2099-02-10T18:00:00+01:00"
tags  = ['git', 'learning', 'tech']
+++

Pretty everyone who has ever worked in IT has gotten into touch with the versioning system Git at one point or another. But most only know some basic commands like for cloning repositorys and only a few know how to work with Git advanced.

But Git is so essential when working with software that IMHO everyone should learn it in-depth and thus get a lot more efficient workflow.
Therefore this article will take you on a journey in OpenSource, DevOps and the Git-Workflows. After this article you will know how to handle git with confidence and know how to effectively collaborate together in a team.
It will also be possible for you to contribute to opensource projects or start your own.

_[Git Cheat Sheet](https://education.github.com/git-cheat-sheet-education.pdf)_
# Setup
Configuring user information used across all local repositories.

- set a name that is identifiable for credit when review version history
  ```
  git config --global user.name "<username>"
  ```

- set an email address that will be associated with each history marker
  ```
  git config --global user.email "<email>"
  ```

# Retrieve
- an entire repository from a hosted location via URL
  ```
  git clone <url>.git
  ```

# Init
Initialize repositoriy.

```
cd existing_repo
git remote add origin <url>.git
git branch -M main
git add .
git commit -m "<description>"
git push -uf origin main
```

# RSA key
- copy to .ssh

If the user has generated a ssh public/private key pair set before

- check which key have been authorized on your github or gitlab account settings
- determine which corresponding private key must be associated from your local computer

then

```
eval $(ssh-agent -s)
ssh-add ~/.ssh/github_rsa
```