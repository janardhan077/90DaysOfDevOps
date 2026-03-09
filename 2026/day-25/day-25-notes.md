Task 1: Git Reset — Hands-On
What happens when using git reset
git reset --soft HEAD~1

Moves HEAD to previous commit.

Changes remain staged (in staging area).

Files are not deleted.

git reset --mixed HEAD~1

Moves HEAD to previous commit.

Changes remain in working directory.

Changes are unstaged.

git reset --hard HEAD~1

Moves HEAD to previous commit.

Deletes changes from staging area and working directory.

Files go back exactly to the previous commit state.

Difference between --soft, --mixed, --hard
Option	Commit History	Staging Area	Working Directory
--soft	Moves HEAD	Keeps staged changes	Keeps files
--mixed	Moves HEAD	Unstages changes	Keeps files
--hard	Moves HEAD	Deletes staged changes	Deletes files
Which one is destructive and why?

git reset --hard is destructive because it permanently deletes changes from staging and working directory.

When to use each one

--soft

When you want to undo a commit but keep changes staged.

--mixed

When you want to undo commit and unstage the changes.

--hard

When you want to completely remove commits and changes.

Should you use reset on pushed commits?

❌ No. Avoid using git reset on commits that are already pushed.

Reason:

It rewrites history.

Other collaborators will face merge conflicts or broken history.

Task 2: Git Revert — Hands-On
Reverting commit Y (middle commit)

Command example:

git revert <commit-hash-of-Y>
What happens?

Git creates a new commit that undoes the changes of commit Y.

Check git log

✔ Commit Y is still in history.

But there will be a new revert commit that cancels its changes.

Example history:

X → Y → Z → Revert Y
How is git revert different from git reset?
Feature	git reset	git revert
History	Rewrites history	Keeps history
Commit removal	Removes commits	Adds a new undo commit
Safety	Risky	Safe
Why revert is safer for shared branches

It does not change commit history.

Other developers won't face conflicts due to rewritten history.

When to use revert vs reset

Use git revert

For public/shared branches

When commits are already pushed

Use git reset

For local commits

When commits are not pushed yet

Task 3: Reset vs Revert — Summary
Feature	git reset	git revert
What it does	Moves HEAD to previous commit	Creates new commit that undoes changes
Removes commit from history	Yes	No
Safe for shared branches	No	Yes
When to use	Local commits not pushed	Public/shared branches
Task 4: Branching Strategies
1. GitFlow
How it works

Uses multiple long-term branches:

main

develop

feature

release

hotfix

Flow Diagram
main
  │
develop
  ├── feature-login
  ├── feature-payment
  │
release
  │
hotfix
When used

Large teams

Projects with scheduled releases

Pros

Structured workflow

Good for release management

Cons

Complex

Too heavy for small teams

2. GitHub Flow
How it works

Only main branch

Create feature branch

Open Pull Request

Merge to main

Flow Diagram
main
 │
 ├── feature-login
 ├── feature-ui
 │
 merge → main
When used

Continuous deployment

Web applications

Pros

Simple

Easy collaboration

Cons

Not ideal for scheduled releases

3. Trunk-Based Development
How it works

Developers commit frequently to main (trunk)

Branches are very short-lived

Flow Diagram
main (trunk)
 ├─ small feature
 ├─ small bug fix
 ├─ small update
When used

DevOps teams

Continuous integration environments

Pros

Fast development

Less merge conflicts

Cons

Requires strong testing and CI

Answers

Which strategy for a startup shipping fast?

✔ Trunk-Based Development
Because it allows fast development and continuous deployment.

Which strategy for a large team with scheduled releases?

✔ GitFlow
Because it provides structured release management.

Which one does most open-source projects use?

✔ GitHub Flow

Most GitHub repositories use:

main

feature branches

pull requests
