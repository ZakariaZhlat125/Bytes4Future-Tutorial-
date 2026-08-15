# Git Complete Course for Beginners
### From Zero to Professional Version Control

---

# Table of Contents

1. [What is Git?](#1-what-is-git)
2. [Why Learn Git?](#2-why-learn-git)
3. [Version Control Systems](#3-version-control-systems)
4. [Git vs GitHub vs GitLab](#4-git-vs-github-vs-gitlab)
5. [Installing Git](#5-installing-git)
6. [Configure Git](#6-configure-git)
7. [Creating Your First Repository](#7-creating-your-first-repository)
8. [Git Workflow](#8-git-workflow)
9. [Git Commands](#9-git-commands)
10. [Branching](#10-branching)
11. [Merging](#11-merging)
12. [Merge Conflicts](#12-merge-conflicts)
13. [Remote Repositories](#13-remote-repositories)
14. [GitHub](#14-github)
15. [Pull Requests](#15-pull-requests)
16. [Undo Changes](#16-undo-changes)
17. [Stashing](#17-stashing)
18. [Tags](#18-tags)
19. [Git Ignore](#19-git-ignore)
20. [Git Aliases](#20-git-aliases)
21. [Git Best Practices](#21-git-best-practices)
22. [Git Project Workflow](#22-git-project-workflow)
23. [Git Interview Questions](#23-git-interview-questions)
24. [Common Mistakes](#24-common-mistakes)

---

# 1. What is Git?

Git is a **Version Control System (VCS)**.

It helps developers:

- Track changes
- Collaborate with teams
- Restore old versions
- Create branches
- Merge code safely

Git was created by **Linus Torvalds** in 2005.

---

# 2. Why Learn Git?

Every software developer should know Git.

It is used by:

- Front-End Developers
- Back-End Developers
- Full Stack Developers
- Mobile Developers
- DevOps Engineers
- QA Engineers
- Data Scientists

Git is one of the most requested skills in technical interviews.

---

# 3. Version Control Systems

A Version Control System keeps a history of your project.

Without Git

```
Project
Project-New
Project-New2
Project-Final
Project-Final-Last
Project-Final-Real
```

With Git

```
One Project
Unlimited History
```

Benefits

- Restore old code
- Compare versions
- Work with a team
- Avoid losing work

---

# 4. Git vs GitHub vs GitLab

## Git

A Version Control System.

Works locally on your computer.

---

## GitHub

A cloud platform for hosting Git repositories.

Features

- Remote repositories
- Pull Requests
- Issues
- Actions (CI/CD)

---

## GitLab

Alternative to GitHub.

Offers

- Repository Hosting
- CI/CD
- Project Management

---

# 5. Installing Git

Download Git from

https://git-scm.com

Verify installation

```bash
git --version
```

Example

```bash
git version 2.48.1
```

---

# 6. Configure Git

Configure your username

```bash
git config --global user.name "John Doe"
```

Configure your email

```bash
git config --global user.email "john@example.com"
```

Check configuration

```bash
git config --list
```

---

# 7. Creating Your First Repository

Create a folder

```
Git Course
```

Open Terminal

Initialize Git

```bash
git init
```

Git creates a hidden folder

```
.git
```

This folder stores the project's history.

---

# 8. Git Workflow

The Git workflow has four stages.

```
Working Directory

↓

Staging Area

↓

Local Repository

↓

Remote Repository
```

Commands

```
git add

git commit

git push
```

---

# 9. Git Commands

## Check Status

```bash
git status
```

Shows modified, staged, and untracked files.

---

## Add One File

```bash
git add index.html
```

---

## Add All Files

```bash
git add .
```

---

## Commit Changes

```bash
git commit -m "Create homepage"
```

A commit saves a snapshot of your project.

---

## View History

```bash
git log
```

Compact history

```bash
git log --oneline
```

---

## Show Changes

```bash
git diff
```

---

## Rename File

```bash
git mv old.txt new.txt
```

---

## Delete File

```bash
git rm file.txt
```

---

# 10. Branching

A branch is an independent line of development.

Show branches

```bash
git branch
```

Create branch

```bash
git branch login-page
```

Switch branch

```bash
git checkout login-page
```

Modern command

```bash
git switch login-page
```

Create and switch

```bash
git switch -c login-page
```

---

# 11. Merging

Merge another branch into the current branch.

Example

```bash
git merge login-page
```

Git combines the changes.

---

# 12. Merge Conflicts

A merge conflict happens when two branches edit the same code.

Example

Branch A

```html
<h1>Hello</h1>
```

Branch B

```html
<h1>Welcome</h1>
```

Git asks you to choose the correct version.

Steps

1. Open the conflicted file.
2. Remove conflict markers.
3. Keep the correct code.
4. Save the file.
5. Commit the merge.

---

# 13. Remote Repositories

Connect your project to GitHub.

Add remote

```bash
git remote add origin https://github.com/username/project.git
```

View remotes

```bash
git remote -v
```

---

# 14. GitHub

Common commands

Push

```bash
git push origin main
```

Pull

```bash
git pull origin main
```

Fetch

```bash
git fetch
```

Clone

```bash
git clone https://github.com/username/project.git
```

---

# 15. Pull Requests

A Pull Request (PR) is used to request merging code into another branch.

Typical workflow

```
Create Branch

↓

Write Code

↓

Push Branch

↓

Open Pull Request

↓

Code Review

↓

Merge
```

Benefits

- Code review
- Team collaboration
- Better quality

---

# 16. Undo Changes

Restore file

```bash
git restore index.html
```

Unstage file

```bash
git restore --staged index.html
```

Undo last commit (keep changes)

```bash
git reset --soft HEAD~1
```

Undo last commit (remove staged)

```bash
git reset --mixed HEAD~1
```

Undo completely

```bash
git reset --hard HEAD~1
```

---

# 17. Git Stash

Temporarily save unfinished work.

Save

```bash
git stash
```

Show stashes

```bash
git stash list
```

Restore

```bash
git stash pop
```

Delete

```bash
git stash drop
```

---

# 18. Tags

Tags mark releases.

Create tag

```bash
git tag v1.0.0
```

List tags

```bash
git tag
```

Push tags

```bash
git push origin --tags
```

---

# 19. Git Ignore

Ignore files that should not be tracked.

Example

```
node_modules/

.env

dist/

build/

coverage/

*.log

.DS_Store

.vscode/
```

File name

```
.gitignore
```

---

# 20. Git Aliases

Example

```bash
git config --global alias.st status
```

Now instead of

```bash
git status
```

You can write

```bash
git st
```

Another example

```bash
git config --global alias.co checkout
```

---

# 21. Git Best Practices

✔ Commit often

✔ Write meaningful commit messages

✔ Use branches

✔ Pull before pushing

✔ Never commit passwords

✔ Never commit API keys

✔ Use .gitignore

✔ Review code before pushing

✔ One feature per branch

✔ Keep commits small

---

# 22. Git Project Workflow

Example

```
Create Project

↓

git init

↓

Create Files

↓

git add .

↓

git commit

↓

Create GitHub Repository

↓

git remote add origin

↓

git push

↓

Create Feature Branch

↓

Develop Feature

↓

Commit Changes

↓

Push Branch

↓

Open Pull Request

↓

Merge

↓

Delete Branch
```

---

# 23. Git Interview Questions

### What is Git?

A distributed version control system.

---

### Difference between Git and GitHub?

Git is the software.

GitHub hosts Git repositories online.

---

### What is a repository?

A project managed by Git.

---

### What is a commit?

A snapshot of your project.

---

### What is HEAD?

The current commit you are working on.

---

### Difference between fetch and pull?

Fetch downloads changes.

Pull downloads and merges changes.

---

### Difference between merge and rebase?

Merge creates a merge commit.

Rebase rewrites commit history for a cleaner timeline.

---

### What is Git Stash?

Temporary storage for uncommitted changes.

---

### What is .gitignore?

A file listing files Git should ignore.

---

### Difference between reset, revert, and restore?

- **restore**: Restore files.
- **reset**: Move HEAD and optionally remove commits.
- **revert**: Create a new commit that undoes previous changes.

---

# 24. Common Mistakes

❌ Forgetting to commit regularly

❌ Using "Final_Final_Last.zip"

❌ Committing `.env`

❌ Committing `node_modules`

❌ Working directly on `main`

❌ Using meaningless commit messages

```
Update
Fix
Test
123
```

Better messages

```
Add login page

Fix navbar alignment

Implement authentication

Update user profile validation
```

---

# Git Command Cheat Sheet

```bash
git init

git clone

git status

git add .

git commit -m "message"

git log

git log --oneline

git diff

git branch

git switch

git merge

git stash

git stash pop

git remote -v

git remote add origin URL

git fetch

git pull

git push

git tag

git restore

git reset

git revert
```

---

# Best Learning Strategy

1. Learn one command at a time.
2. Practice immediately.
3. Create a small project.
4. Make mistakes and fix them.
5. Use Git every day.
6. Push projects to GitHub.
7. Collaborate with others.
8. Read commit history.
9. Review pull requests.
10. Never stop practicing.

---

# Final Project

Build a simple website and manage it using Git.

Tasks:

- Initialize a Git repository.
- Create `index.html`.
- Create `style.css`.
- Commit the initial project.
- Create a `navbar` branch.
- Add a navigation bar.
- Commit the changes.
- Merge the branch into `main`.
- Push the project to GitHub.
- Create a Pull Request.
- Tag the first release as `v1.0.0`.

Congratulations! 🎉

You now have the foundational Git skills used in professional software development.