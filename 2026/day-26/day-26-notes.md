Day 26 – GitHub CLI Notes
Observations

Today I practiced using the GitHub CLI (gh) to interact with GitHub directly from the terminal.
Using gh, many tasks like creating repositories, issues, pull requests, and viewing workflows can be done without opening the browser. It helps automate GitHub operations and is useful for DevOps workflows.

Task Answers
1. Authentication Methods Supported by gh

gh supports the following authentication methods:

Browser-based login

Personal Access Token (PAT)

SSH authentication

2. Working with Repositories

Using GitHub CLI we can:

Create repositories from the terminal

Clone repositories using gh repo clone

View repository details

List all repositories of the account

Open repositories directly in the browser

Delete repositories from the terminal

This makes repository management faster compared to doing everything from the web UI.

3. Using gh issue in Automation

gh issue can be used in scripts or automation to:

Automatically create issues when builds fail

Log bugs generated during automated testing

Create task tracking issues from CI/CD pipelines

Manage project workflows programmatically

4. Pull Request Operations
Merge methods supported by gh pr merge

Merge commit

Squash merge

Rebase merge

Reviewing someone else's PR using gh

A developer can review pull requests from the terminal by:

Checking out the PR locally

Viewing the PR details

Adding comments or approving the PR using review commands

This helps developers review contributions without leaving the terminal.

5. GitHub Actions (Preview)
Use of gh run and gh workflow in CI/CD

These commands can help to:

Monitor workflow runs

Check build or deployment status

Debug failed pipelines

Trigger workflows directly from the terminal

This is helpful when integrating GitHub with CI/CD pipelines.

Conclusion

GitHub CLI simplifies many GitHub operations from the terminal. It improves developer productivity and is very useful for automation, scripting, and DevOps workflows.

Add these commands to git-commands.md:

gh auth login
gh auth status
gh repo create
gh repo clone
gh repo list
gh repo view
gh repo delete
gh issue create
gh issue list
gh issue view
gh issue close
gh pr create
gh pr list
gh pr view
gh pr merge
gh run list
gh run view
gh workflow list
