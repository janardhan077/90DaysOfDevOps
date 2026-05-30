TASK1
  ## Task 1: The Problem

### 1. What can go wrong?

* Code conflicts between developers
* Bugs or broken features in production
* Different environments causing failures
* Wrong manual deployment steps
* No proper testing before deployment

### 2. What does "it works on my machine" mean?

"It works on my machine" means the code runs on a developer’s local system but fails on another system or production because of different software versions, configurations, or dependencies. It is a real problem because it causes inconsistent behavior and deployment failures.

### 3. How many times a day can a team safely deploy manually?

Manual deployments are slow and risky. A team can safely deploy only a limited number of times per day because each deployment needs careful checking and human effort. Automation makes frequent deployments safer and faster.

TASK 2
## Continuous Integration (CI)

Continuous Integration is the practice of developers frequently merging code changes into a shared repository. Automated builds and tests run on every change to catch bugs, merge conflicts, and integration issues early.

**Example:** A developer pushes code to GitHub, and GitHub Actions automatically runs tests.

## Continuous Delivery (CD)

Continuous Delivery extends CI by keeping the application always ready for release. Code is automatically built, tested, and prepared for deployment, but production release usually needs manual approval.

**Example:** An e-commerce app automatically reaches the staging environment, and the team clicks approve for production deployment.

## Continuous Deployment

Continuous Deployment goes beyond Continuous Delivery by automatically deploying every successful change to production without manual approval. Teams use it when they have strong automated testing and monitoring.

**Example:** A streaming platform automatically deploys tested updates directly to users.

TASK 3 
## CI/CD Pipeline Components

**Trigger** – Starts the pipeline execution.
Example: A push, pull request, or manual workflow run.

**Stage** – A logical phase in the pipeline such as build, test, or deploy.

**Job** – A unit of work inside a stage that performs a specific task.

**Step** – A single command or action inside a job.
Example: installing dependencies or running tests.

**Runner** – The machine that executes the pipeline jobs.
Example: GitHub-hosted Ubuntu runner.

**Artifact** – The output produced by a job and stored for later use.
Example: build files, logs, or deployment packages.

TASK 4


