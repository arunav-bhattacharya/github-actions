# Github Actions

## Overview

- Events trigger workflows, e.g. a push to a branch.
- Workflows contain one or more jobs, which contains one or more steps.
- These steps can reference actions or execute commands.
- The term "Github Actions" include all components, not just the Actions themselves.

## Basic Syntax of a Workflow

Workflow file is located under the `./github/workflows` directory of the repository -

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

<br>

## Steps 

- Steps are basic unit of execution in GitHub Actions. They consist of either invocations of a predefined action or a shell command to be run on the runner.
- Shell commands are executed using the `run` clause.
- Predefined actions are pulled in via the `uses` clause.
- `with` clause specifies any arguments/parameters to pass to the action.

<br>

## Jobs

- Aggregate steps and define which runner to execute them on.
- Success or failure is dislayed at job level and not on individual step.
- By default, jobs are executed in parallel unless we specify the `needs` as a clause under another job, in which the job will wait for the completion of the job that is specified under `needs`.

<br>

## Workflows

- Workflow is like a pipeline. We define events on which a particular workflow will be triggered. The jobs that will be triggered as a part of that workflow are specified under the jobs section.

<br>

## Actions

- Action can be as simple as a shell command or it can be a whole project maintained under a separate repo.
- To be used as an action, a GitHub repository must contain an `action.yaml` file. This file contains metadata about the action.
- The format of the file is broken down in four key areas:
  - basic info (name, author, description)
  - `inputs`
  - `outputs`
  - `runs`
- There is also a less used branding section that allows adding an icon to the action if desired.
- The `inputs` and `outputs` sections have typical properties, that can be set as -
  - `description`
  - `default`
  - ...
- The inputs section is important because it defines how workflows interface with this action. If a parameter is required, your workflow needs to use a with statement, when calling the action, to provide a value to pass in for that parameter.
- The `runs` section specifies what kind of programming the action uses for its implementation.

## References

### GitHub Docs

#### Workflows
- [GitHub Actions workflow syntax](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions)
- [Workflow syntax for events](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions#on)
- [Workflow syntax for scheduled events](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions#onschedule) 
- [Scheduled events](https://docs.github.com/en/actions/reference/events-that-trigger-workflows#schedule)
- [Manually running a workflow](https://docs.github.com/en/actions/managing-workflow-runs/manually-running-a-workflow)
- [Workflow Dispatch](https://docs.github.com/en/actions/reference/events-that-trigger-workflows#workflow_dispatch)
- [Create a repository dispatch event](https://docs.github.com/en/rest/reference/repos#create-a-repository-dispatch-event)
- [Manual events](https://docs.github.com/en/actions/reference/events-that-trigger-workflows#manual-events) 
- [workflow_dispatch API](https://docs.github.com/en/rest/reference/actions#create-a-workflow-dispatch-event) 
- [GitHub CLI - trigger a workflow_dispatch event](https://cli.github.com/manual/gh_workflow_run)
- [Workflow syntax for steps](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions#jobsjob_idsteps)
- 
#### Runners
- [Workflow syntax for runners](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions#jobsjob_idruns-on) 
- [Using GitHub-hosted runners](https://docs.github.com/en/actions/using-github-hosted-runners) 
- [Using self-hosted runners](https://docs.github.com/en/actions/hosting-your-own-runners)
- [About GitHub-hosted runners](https://docs.github.com/en/actions/using-github-hosted-runners/about-github-hosted-runners) 
- [Customizing GitHub-hosted runners](https://docs.github.com/en/actions/using-github-hosted-runners/customizing-github-hosted-runners)
- [Billing for GitHub Actions](https://docs.github.com/en/billing/managing-billing-for-github-actions/about-billing-for-github-actions) 
- [Usage limits for GitHub-hosted runners](https://docs.github.com/en/actions/reference/usage-limits-billing-and-administration#usage-limits)
- [About self-hosted runners](https://docs.github.com/en/actions/hosting-your-own-runners/about-self-hosted-runners) 
- [Self-hosted runners usage limits](https://docs.github.com/en/actions/reference/usage-limits-billing-and-administration#usage-limits) 
- [Adding self-hosted runners](https://docs.github.com/en/actions/hosting-your-own-runners/adding-self-hosted-runners)
- [Using a proxy with self-hosted runners](https://docs.github.com/en/actions/hosting-your-own-runners/using-a-proxy-server-with-self-hosted-runners) 
- [Using self-hosted runners in a workflow](https://docs.github.com/en/actions/hosting-your-own-runners/using-self-hosted-runners-in-a-workflow) 
- [Manage access to runners using groups](https://docs.github.com/en/actions/hosting-your-own-runners/managing-access-to-self-hosted-runners-using-groups)

#### Additional Features
- [Essential features of GitHub Actions](https://docs.github.com/en/actions/learn-github-actions/essential-features-of-github-actions)
- [Publishing Actions to the GitHub Marketplace](https://docs.github.com/en/actions/creating-actions/publishing-actions-in-github-marketplace)
- [Service Containers](https://docs.github.com/en/actions/using-containerized-services/about-service-containers)
- [Expressions](https://docs.github.com/en/enterprise-server@3.6/actions/learn-github-actions/expressions)
- [Environment docs](https://docs.github.com/en/actions/reference/environments) 

#### Secrets
- [Secrets](https://docs.github.com/en/actions/reference/encrypted-secrets) 
- [Environment secrets](https://docs.github.com/en/actions/reference/encrypted-secrets#creating-encrypted-secrets-for-an-environment) 
- [Repository secrets](https://docs.github.com/en/actions/reference/encrypted-secrets#creating-encrypted-secrets-for-a-repository) 
- [Organization secrets](https://docs.github.com/en/actions/reference/encrypted-secrets#creating-encrypted-secrets-for-an-organization) 
- [GITHUB_TOKEN](https://docs.github.com/en/actions/reference/authentication-in-a-workflow) 
- [GITHUB_TOKEN permissions](https://docs.github.com/en/actions/reference/authentication-in-a-workflow#permissions-for-the-github_token)

#### Enforcing Actions policies
- [Enforcing Actions policies in your enterprise](https://docs.github.com/en/github/setting-up-and-managing-your-enterprise/setting-policies-for-organizations-in-your-enterprise-account/enforcing-github-actions-policies-in-your-enterprise-account) 
- [Enforcing Actions policies in your organizations](https://docs.github.com/en/organizations/managing-organization-settings/disabling-or-limiting-github-actions-for-your-organization) 
- [Enforcing Actions policies in your repositories](https://docs.github.com/en/github/administering-a-repository/managing-repository-settings/disabling-or-limiting-github-actions-for-a-repository)

<br>


### Additional References

- [Crontab](https://crontab.guru/)
- [List of pre-installed software on GitHub-hosted runners](https://github.com/actions/virtual-environments)
- [Amex](https://github.aexp.com/pages/amex-eng/cicd-amex-action/docs/intro/)
- [GitHub Marketplace](https://github.com/marketplace?type=actions) 
- https://libsodium.gitbook.io/doc/public-key_cryptography/sealed_boxes
- [VS code Extension - Enabling Intellisense for Github Actions](https://www.meziantou.net/enabling-intellisense-for-github-actions-workflows-in-vs-code.htm)
- [VS code Extension - Github Actions](https://marketplace.visualstudio.com/items?itemName=cschleiden.vscode-github-actions)
- https://github.com/github/safe-settings
- [Deployment API](https://developer.github.com/v3/repos/deployments/)

