# Day 40 – My First GitHub Actions Workflow

## Objective

Learn the basics of GitHub Actions by creating a workflow that runs automatically on every push.

---

## Workflow YAML

```yaml
name: Hello Workflow

on:
  push:

jobs:
  greet:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4

      - name: Print Greeting
        run: echo "Hello from GitHub Actions!"

      - name: Print Current Date and Time
        run: date

      - name: Print Branch Name
        run: echo "Branch: ${{ github.ref_name }}"

      - name: List Repository Files
        run: ls -la

      - name: Print Runner Operating System
        run: echo "Runner OS: $RUNNER_OS"
```

---

## Screenshot of Successful Workflow Run

![Green Pipeline Run](screenshot.png)

*(Replace `screenshot.png` with the actual screenshot file name.)*

---

## Understanding Workflow Keys

### on:

Defines the event that triggers the workflow.
In this project, the workflow runs whenever code is pushed to the repository.

### jobs:

Contains one or more jobs that GitHub Actions executes.

### runs-on:

Specifies the type of runner (virtual machine) where the job will execute.

### steps:

Lists the individual actions or commands that make up the job.

### uses:

Uses a pre-built GitHub Action from the GitHub Marketplace.

Example:

```yaml
uses: actions/checkout@v4
```

This action checks out the repository code onto the runner.

### run:

Executes shell commands directly on the runner.

Example:

```yaml
run: echo "Hello from GitHub Actions!"
```

### name:

Provides a readable name for a workflow, job, or step that appears in the Actions tab.

---

## Additional Tasks Completed

### Printed Current Date and Time

```yaml
- name: Print Current Date and Time
  run: date
```

### Printed Branch Name

```yaml
- name: Print Branch Name
  run: echo "Branch: ${{ github.ref_name }}"
```

### Listed Repository Files

```yaml
- name: List Repository Files
  run: ls -la
```

### Printed Runner Operating System

```yaml
- name: Print Runner Operating System
  run: echo "Runner OS: $RUNNER_OS"
```

---

## Failed Pipeline Experiment

To understand workflow failures, I intentionally added a failing step:

```yaml
- name: Intentional Failure
  run: exit 1
```

### Observation

* The workflow status turned red.
* The failed step was highlighted in the Actions tab.
* GitHub displayed the error message and exit code.
* The remaining steps were skipped.

### How to Read Errors

1. Open the failed workflow run.
2. Select the failed job.
3. Expand the failed step.
4. Read the logs to identify the command that caused the failure.
5. Fix the issue and push the changes again.

---

## Outcome

Successfully created and executed my first GitHub Actions workflow. Learned how workflows are triggered, how jobs and steps are organized, and how to diagnose workflow failures using GitHub Actions logs.
