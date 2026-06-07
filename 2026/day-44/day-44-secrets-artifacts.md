# Day 44 – Secrets, Artifacts, Testing, and Caching

## Objective

Learn how to securely manage secrets, store artifacts, run tests in CI, and speed up workflows using caching.

---

# Task 1: GitHub Secrets

## Secret Created

Repository → Settings → Secrets and Variables → Actions

Created:

```text
MY_SECRET_MESSAGE
```

## Workflow YAML

```yaml
name: Secret Demo

on:
  workflow_dispatch:

jobs:
  secret-check:
    runs-on: ubuntu-latest

    steps:
      - name: Check Secret
        run: |
          if [ -n "${{ secrets.MY_SECRET_MESSAGE }}" ]; then
            echo "The secret is set: true"
          else
            echo "The secret is set: false"
          fi
```

## What Happens If You Print the Secret?

Example:

```yaml
run: echo "${{ secrets.MY_SECRET_MESSAGE }}"
```

GitHub automatically masks the value and displays:

```text
***
```

instead of the actual secret.

## Why Should You Never Print Secrets?

* Logs may be visible to team members.
* Secrets could be copied from logs.
* Exposed secrets can compromise systems and accounts.
* Security best practices require keeping credentials confidential.

---

# Task 2: Using Secrets as Environment Variables

## Workflow Example

```yaml
name: Secret Environment Variable

on:
  workflow_dispatch:

jobs:
  use-secret:
    runs-on: ubuntu-latest

    steps:
      - name: Use Secret Safely
        env:
          SECRET_MSG: ${{ secrets.MY_SECRET_MESSAGE }}

        run: |
          echo "Secret received successfully"
          echo "Length: ${#SECRET_MSG}"
```

## Additional Secrets Added

```text
DOCKER_USERNAME
DOCKER_TOKEN
```

These will be used later for Docker Hub authentication.

---

# Task 3: Upload Artifacts

## Workflow Example

```yaml
name: Upload Artifact

on:
  workflow_dispatch:

jobs:
  upload:
    runs-on: ubuntu-latest

    steps:
      - name: Generate Report
        run: |
          echo "Test Report Generated" > report.txt

      - name: Upload Artifact
        uses: actions/upload-artifact@v4
        with:
          name: test-report
          path: report.txt
```

## Verification

After the workflow completed:

1. Open Actions tab.
2. Open workflow run.
3. Locate Artifacts section.
4. Download artifact.

### Screenshot

![Artifact Download](artifact-download.png)

*(Replace with your actual screenshot.)*

---

# Task 4: Download Artifacts Between Jobs

## Workflow Example

```yaml
name: Artifact Sharing

on:
  workflow_dispatch:

jobs:
  create-file:
    runs-on: ubuntu-latest

    steps:
      - run: echo "Hello from Job 1" > message.txt

      - uses: actions/upload-artifact@v4
        with:
          name: shared-file
          path: message.txt

  read-file:
    needs: create-file
    runs-on: ubuntu-latest

    steps:
      - uses: actions/download-artifact@v4
        with:
          name: shared-file

      - run: cat message.txt
```

## When Are Artifacts Useful?

Artifacts are commonly used for:

* Build packages
* Test reports
* Log files
* Deployment bundles
* Generated documentation
* Sharing files between jobs

---

# Task 5: Run Real Tests in CI

## Example Python Script

```python
def add(a, b):
    return a + b

assert add(2, 3) == 5
print("Tests Passed")
```

Saved as:

```text
test_script.py
```

## Workflow YAML

```yaml
name: Run Tests

on:
  push:

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Setup Python
        uses: actions/setup-python@v5
        with:
          python-version: "3.12"

      - name: Run Tests
        run: python test_script.py
```

## Verification

### Failed Run

When the assertion was intentionally changed:

```python
assert add(2, 3) == 6
```

The workflow failed and turned red.

### Successful Run

After fixing the assertion:

```python
assert add(2, 3) == 5
```

The workflow passed and turned green.

### Screenshot

![Passing Test Run](passing-test-run.png)

*(Replace with your actual screenshot.)*

---

# Task 6: Caching

## Workflow Example

```yaml
name: Cache Demo

on:
  workflow_dispatch:

jobs:
  cache-example:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - uses: actions/cache@v4
        with:
          path: ~/.cache/pip
          key: pip-cache-${{ runner.os }}

      - run: pip install requests
```

## Observation

### First Run

```text
Cache Miss
```

Dependencies downloaded normally.

### Second Run

```text
Cache Hit
```

Dependencies restored from cache.

Workflow completed faster.

## What Is Being Cached?

In this example:

```text
Python package cache (~/.cache/pip)
```

is stored.

## Where Is It Stored?

GitHub stores cache data associated with the repository and cache key.

Future workflow runs can restore it to avoid downloading dependencies again.

---

# What I Learned About Secrets Management

* Secrets should never be hardcoded into workflows.
* GitHub Secrets provide a secure way to store credentials.
* Secret values are automatically masked in logs.
* Secrets can be passed safely through environment variables.
* Docker credentials and API keys should always be stored as secrets.
* Proper secret management is critical for CI/CD security.

---

# Key Learnings

* GitHub Secrets protect sensitive information.
* Artifacts allow files to be stored and shared between jobs.
* CI pipelines should run automated tests.
* Failed tests immediately stop deployments.
* Caching significantly reduces workflow execution time.
* Secure credential management is a core DevOps practice.
