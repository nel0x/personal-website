+++
title = "Learning Git"
date = "2022-02-10T18:00:00+01:00"
tags  = ['git', 'learning', 'tech']
+++

Pretty everyone who has ever worked in IT has gotten into touch with the versioning system Git at one point or another. But most only know some basic commands like for cloning repositorys and only a few know how to work with Git advanced.

But Git is so essential when working with software that IMHO everyone should learn it in-depth and thus get a lot more efficient workflow.
Therefore this article should serve as an introducing walkthrough but also as a cheat sheet for looking things quickly up.

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


  
  
See also: https://education.github.com/git-cheat-sheet-education.pdf
