# Day 41 — GitHub Actions Triggers

## 📌 Objective

Learn and practice different **GitHub Actions triggers**:

* Pull Request trigger
* Scheduled trigger (cron)
* Manual trigger (`workflow_dispatch`)

---

# 1) Pull Request Trigger Workflow

File: `.github/workflows/pr-check.yml`

```yaml
name: PR Check Workflow

on:
  pull_request:
    branches:
      - main
    types:
      - opened
      - synchronize

jobs:
  pr-check:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Print PR branch name
        run: echo "PR check running for branch: ${{ github.head_ref }}"
```



> Add your PR workflow run screenshot here

---

# 2) Scheduled Trigger Workflow

File: `.github/workflows/schedule.yml`

```yaml
name: Nightly Check

on:
  schedule:
    - cron: '0 0 * * *'
  workflow_dispatch:

jobs:
  nightly-job:
    runs-on: ubuntu-latest

    steps:
      - name: Print current date
        run: echo "Workflow ran at $(date)"
```



> Add your scheduled/manual test run screenshot here

### 📝 Cron Expression Answer

**Every Monday at 9 AM:**

```text
0 9 * * 1
```

---

# 3) Manual Trigger Workflow

File: `.github/workflows/manual.yml`

```yaml
name: Manual Deployment Workflow

on:
  workflow_dispatch:
    inputs:
      environment:
        description: "Choose environment"
        required: true
        type: choice
        options:
          - staging
          - production

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Print selected environment
        run: echo "Deploying to ${{ inputs.environment }}"
```


> Add your manual workflow run screenshot here

---

# ✅ Key Learnings

* `pull_request` runs on PR creation and updates
* `schedule` automates recurring jobs using cron syntax
* `workflow_dispatch` enables manual runs with custom inputs
* Cron format uses: `minute hour day month weekday`

---

# 🚀 Outcome

Successfully created and tested multiple workflow triggers in GitHub Actions:

* PR validation
* Daily scheduled workflow
* Manual deployment workflow

This is the foundation for **real CI/CD automation pipelines**.
