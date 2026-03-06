# Git Fetch vs Git Pull – Notes (What I Learned)

## 1. Remote and Local Repository

A remote repository is stored on platforms like GitHub, while the local repository is on my system.
Sometimes commits are made directly on the remote repository. My local repository will not know about those changes until I update it.

---

## 2. Git Fetch

`git fetch` is used to download the latest changes from the remote repository without modifying my local files.

Example:
git fetch origin feature-1

What it does:

* Downloads commits from the remote repository
* Updates the remote tracking branch (origin/feature-1)
* Creates a temporary pointer called FETCH_HEAD
* Does NOT change the current branch
* HEAD does not move

So after running fetch, I can see that new commits exist but my working files remain the same.

---

## 3. Git Pull

`git pull` is used to download and immediately apply the changes to the current branch.

Example:
git pull origin master

What it does:

* Downloads commits from the remote repository
* Merges them into the current branch
* Updates my local files

---

## 4. Difference Between Fetch and Pull

git fetch

* Only downloads updates
* Does not change files
* Safe way to check changes first

git pull

* Downloads updates
* Merges them automatically
* Local files get updated immediately

---

## 5. Important Git Terms I Learned

HEAD
The pointer that shows the current branch I am working on.

origin
The default name of the remote repository.

origin/branch-name
A remote tracking branch that shows the latest state of the branch on the remote repository.

FETCH_HEAD
A temporary reference to the latest commit fetched from the remote repository.

---

## 6. Simple Workflow

git fetch
git log origin/master
git merge origin/master

This allows me to download changes first, review them, and then merge them safely.

What I Learned

1. Git Branch Basics

* `git branch` shows all branches in the repository.
* `*` indicates the current branch I am working on.
* Git does not allow deleting the branch that is currently active.

Example:
git branch
git switch master
git branch -d branch_name

2. Deleting Branches
   To delete a branch, I must first switch to another branch.

Commands:
git switch master
git branch -d jana

If Git refuses because the branch is not merged:
git branch -D branch_name

3. Adding a Remote Repository
   A remote repository connects my local project to GitHub.

Command:
git remote add origin https://github.com/username/repository.git

To verify the remote:
git remote -v

4. Fixing Remote URL Errors
   If the remote URL is wrong, it can be corrected using:

git remote set-url origin https://github.com/username/repository.git

Common mistakes I learned:

* Typing `htpps` instead of `https`
* Using the wrong remote name
* Adding branch name instead of repository URL

5. Pushing Code to GitHub
   To upload my local code to GitHub:

git push -u origin master

The `-u` option sets the upstream branch so future pushes can use:
git push

Key Things I Learned

1. Git will not delete the branch I am currently using.
2. Remote repositories must use the correct URL format.
3. Small typing mistakes in Git commands can cause errors.

This practice helped me understand how Git branches and remote repositories work in real DevOps workflows.

---
