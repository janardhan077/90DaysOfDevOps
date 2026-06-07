# Day 42 – GitHub Actions Runners

## Objective

Learn the difference between GitHub-hosted and self-hosted runners, explore pre-installed software, and execute workflows on different operating systems.

---

# Task 1: GitHub-Hosted Runners

## Workflow YAML

```yaml
name: Multi-OS Runner Demo

on:
  workflow_dispatch:

jobs:
  ubuntu:
    runs-on: ubuntu-latest
    steps:
      - name: Print Runner Information
        run: |
          echo "OS: $RUNNER_OS"
          hostname
          whoami

  windows:
    runs-on: windows-latest
    steps:
      - name: Print Runner Information
        run: |
          echo "OS: $env:RUNNER_OS"
          hostname
          whoami

  macos:
    runs-on: macos-latest
    steps:
      - name: Print Runner Information
        run: |
          echo "OS: $RUNNER_OS"
          hostname
          whoami
```

## Notes

### What is a GitHub-hosted runner?

A GitHub-hosted runner is a virtual machine provided by GitHub to execute workflow jobs.

### Who manages it?

GitHub manages the infrastructure, operating system updates, maintenance, and cleanup after each workflow run.

---

# Task 2: Explore Pre-installed Software

## Workflow Step

```yaml
- name: Check Installed Tools
  run: |
    docker --version
    python --version
    node --version
    git --version
```

## Sample Output

```text
Docker version 28.x.x
Python 3.x.x
Node v22.x.x
git version 2.x.x
```

## Notes

### Why do pre-installed tools matter?

Pre-installed tools reduce setup time, make workflows faster, and eliminate the need to install common software during every workflow run.

---

# Task 3: Self-Hosted Runner Setup

Configured a Linux self-hosted runner using the setup commands provided by GitHub.

Verified that the runner appeared in:

Settings → Actions → Runners

Status showed:

```text
Idle
```

## Screenshot

![Self-Hosted Runner Idle](runner-idle.png)

*(Replace with your actual screenshot.)*

---

# Task 4: Run Workflow on Self-Hosted Runner

## Workflow YAML

```yaml
name: Self Hosted Runner Test

on:
  workflow_dispatch:

jobs:
  test-runner:
    runs-on: self-hosted

    steps:
      - name: Print Hostname
        run: hostname

      - name: Print Working Directory
        run: pwd

      - name: Create File
        run: |
          touch runner-test.txt
          ls -l runner-test.txt
```

## Verification

After the workflow completed, the file:

```text
runner-test.txt
```

was successfully created on the self-hosted machine.

---

# Task 5: Runner Labels

Added custom label:

```text
my-linux-runner
```

Updated workflow:

```yaml
runs-on: [self-hosted, my-linux-runner]
```

The workflow successfully executed on the labeled runner.

## Why are labels useful?

Labels help target specific runners when multiple self-hosted runners exist. They allow jobs to run only on machines with required capabilities, software, or hardware configurations.

---

# Screenshot of Job Running on Self-Hosted Runner

![Job Running on Self-Hosted Runner](runner-job.png)

*(Replace with your actual screenshot.)*

---

# Task 6: GitHub-Hosted vs Self-Hosted Comparison

| Feature             | GitHub-Hosted Runner                                 | Self-Hosted Runner                                      |
| ------------------- | ---------------------------------------------------- | ------------------------------------------------------- |
| Who manages it?     | GitHub                                               | User / Organization                                     |
| Cost                | Free minutes available, then usage charges may apply | Infrastructure cost paid by user                        |
| Pre-installed tools | Large collection of tools already installed          | User installs and maintains tools                       |
| Good for            | General CI/CD workloads and quick setup              | Custom environments, special hardware, internal systems |
| Security concern    | Code runs on GitHub-managed infrastructure           | User responsible for securing runner and machine        |

---

# Key Learnings

* GitHub-hosted runners are easy to use and require no maintenance.
* Self-hosted runners provide full control over the execution environment.
* Labels help route jobs to specific runners.
* Many development tools come pre-installed on GitHub-hosted runners.
* Self-hosted runners can access internal resources and custom software unavailable on GitHub-hosted machines.
