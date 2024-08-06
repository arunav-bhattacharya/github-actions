# Github Actions

## Overview

- Events trigger workflows, e.g. a push to a branch.
- Workflows contain one or more jobs, which contains one or more steps.
- These steps can reference actions or execute commands.
- The term "Github Actions" include all components, not just the Actions themselves.

## Basic Syntax of a Workflow

`./.github/workflows/workflow-file-name.yaml`

```yaml
# Workflow Name
name: Some Linting Workflow

# Events
on: 
  push:

# Jobs
jobs:
  lint:
    name: Lint Code base

#   Runner
    runs-on: ubuntu-latest

#   Steps
    steps:
#     Actions
      - uses: actions/checkout@v2
      - uses: github/super-linter@v3
        env:
#     Secrets
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

<br>

## Events

### Types

- Webhook events
  - Pull request
  - Issues
  - Push
  - Release
  - ....
  ```yaml
  on:
    issues:
      types: [closed, reopened]
  ```
- Scheduled events
  ```yaml
  on:
    schedule:
      - cron: `30 6 * * 5` # Every Friday at 06:30 UTC
  ```
- Manual events
  - workflow_dispatch
  - repository_dispatch
  ```yaml
  on:
    workflow_dispatch:
  ```

<br>
  
## Runners

### Github hosted runners

- OS: ubuntu, windows or macOS
- Ephemeral
- 2-core CPU (macOS: 3-core)
- 7 GB RAM (macOS: 14 GB)
- 14 GB SSD disk space
- Software installed: wget, GH CLI, AWS CLI, Java, ...

### Self hosted runners

- Custom hardware config
- Runs on OS not supported on Github-hosted runner
- Reference runner using custom labels
- Can be grouped together
- Control which organizations/repositories have access to which runner/runner groups
- Do not use with public repositories