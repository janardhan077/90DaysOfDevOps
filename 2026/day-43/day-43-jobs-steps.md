# Day 43 – Jobs, Steps, Variables, and Outputs

## Objective

Learn how GitHub Actions workflows use multiple jobs, environment variables, outputs, dependencies, and conditional execution.

---

# Task 1: Multi-Job Workflow

## Workflow YAML

```yaml
name: Multi Job Workflow

on:
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Building the app"

  test:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - run: echo "Running tests"

  deploy:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - run: echo "Deploying"
```

## Observation

The workflow graph showed:

```text
build → test → deploy
```

The jobs executed in sequence because each job depended on the previous one.

---

# Task 2: Environment Variables

## Workflow YAML

```yaml
name: Environment Variables Demo

on:
  workflow_dispatch:

env:
  APP_NAME: myapp

jobs:
  demo:
    runs-on: ubuntu-latest

    env:
      ENVIRONMENT: staging

    steps:
      - name: Print Variables
        env:
          VERSION: 1.0.0
        run: |
          echo "App: $APP_NAME"
          echo "Environment: $ENVIRONMENT"
          echo "Version: $VERSION"

      - name: Print GitHub Context Variables
        run: |
          echo "Commit SHA: ${{ github.sha }}"
          echo "Triggered By: ${{ github.actor }}"
```

## Notes

### Workflow Level Variable

Available to every job and step.

### Job Level Variable

Available only within the specific job.

### Step Level Variable

Available only within the specific step.

---

# Task 3: Job Outputs

## Workflow YAML

```yaml
name: Job Outputs Demo

on:
  workflow_dispatch:

jobs:
  generate-date:
    runs-on: ubuntu-latest

    outputs:
      today: ${{ steps.date.outputs.today }}

    steps:
      - id: date
        run: echo "today=$(date)" >> $GITHUB_OUTPUT

  print-date:
    needs: generate-date
    runs-on: ubuntu-latest

    steps:
      - run: echo "Date received: ${{ needs.generate-date.outputs.today }}"
```

## Why Pass Outputs Between Jobs?

Outputs allow one job to generate information that another job can use later.

Examples:

* Build version numbers
* Docker image tags
* Deployment URLs
* Test results
* Generated artifacts metadata

This helps workflows share information without repeating work.

---

# Task 4: Conditionals

## Main Branch Only Step

```yaml
- name: Run on Main Branch
  if: github.ref == 'refs/heads/main'
  run: echo "Running on main branch"
```

## Run Only When Previous Step Fails

```yaml
- name: Runs After Failure
  if: failure()
  run: echo "Previous step failed"
```

## Job Runs Only on Push Events

```yaml
job-on-push:
  if: github.event_name == 'push'
  runs-on: ubuntu-latest

  steps:
    - run: echo "Push event detected"
```

## Continue On Error

```yaml
- name: Ignore Failure
  continue-on-error: true
  run: exit 1
```

### What Does continue-on-error Do?

It allows a step to fail without stopping the workflow.

The workflow continues running the remaining steps.

Useful for:

* Experimental checks
* Optional validations
* Collecting information even when one step fails

---

# Task 5: Smart Pipeline

## Workflow YAML

```yaml
name: Smart Pipeline

on:
  push:

jobs:
  lint:
    runs-on: ubuntu-latest

    steps:
      - run: echo "Running lint checks"

  test:
    runs-on: ubuntu-latest

    steps:
      - run: echo "Running tests"

  summary:
    needs: [lint, test]
    runs-on: ubuntu-latest

    steps:
      - name: Branch Information
        run: |
          if [[ "${{ github.ref }}" == "refs/heads/main" ]]; then
            echo "Main branch push detected"
          else
            echo "Feature branch push detected"
          fi

      - name: Print Commit Message
        run: |
          echo "Commit Message:"
          echo "${{ github.event.commits[0].message }}"
```

## Workflow Flow

```text
lint ──┐
        ├── summary
test ──┘
```

The lint and test jobs run in parallel.

The summary job waits for both to finish successfully.

---

# Understanding needs:

The needs keyword creates dependencies between jobs.

Without needs:

Jobs run in parallel.

With needs:

Jobs wait until the required jobs complete successfully.

Example:

```yaml
needs: build
```

This means:

"Do not start this job until the build job succeeds."

---

# Understanding outputs:

Outputs allow one job to send data to another job.

Example:

```yaml
outputs:
  today: ${{ steps.date.outputs.today }}
```

Another job can access it using:

```yaml
${{ needs.generate-date.outputs.today }}
```

Think of outputs as a way for jobs to share information across the workflow.

---

# Key Learnings

* Jobs run in parallel by default.
* needs creates job dependencies.
* Environment variables can be defined at workflow, job, or step level.
* Outputs allow jobs to exchange information.
* Conditional execution controls when jobs and steps run.
* continue-on-error prevents a workflow from stopping when a step fails.
* Parallel jobs improve workflow speed.
