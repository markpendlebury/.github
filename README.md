# Reusable Workflows and Actions Repository

Welcome to the repository containing a collection of reusable GitHub Workflows and Actions designed to simplify and enhance your CI/CD pipelines. This README provides an overview of the workflows and actions available and demonstrates how to use them in your repositories.

## Prerequisites

Note that for the reusable workflows to be affective, your repository should follow a certain file structure.

You should also have your environment configured and have `ACCOUNT_ID` and `AWS_OIDC_ROLE` in variables. 

## Table of Contents

- [Reusable Workflows and Actions Repository](#reusable-workflows-and-actions-repository)
  - [Prerequisites](#prerequisites)
  - [Table of Contents](#table-of-contents)
    - [Main Branch Workflow](#main-branch-workflow)
    - [Plan Only Workflow](#plan-only-workflow)
    - [PR Workflow](#pr-workflow)

### Main Branch Workflow

**Description:** This workflow collects 5 simple actions into one workflow to allow for fast deployments of lambdas and terraform code. Prerequisites -> Pytest -> Lambda Package -> Terraform Plan -> Terraform Apply.  

**Usage:** To use this workflow create a main.yml in your own .github folder and add the following yaml file.

```yaml
name: Deploy to environments
on:
  push:
    branches:
      - "main"

permissions:
  id-token: write
  contents: read

jobs:
  dev:
    uses: markpendlebury/.github/.github/workflows/main.yml@main
    with:
      env_name: dev
      tf_working_dir: terraform
```

### Plan Only Workflow

**Description:**  Prerequisites -> Pytest -> Lambda Package -> Terraform Plan

**Usage:** To use this workflow create a main.yml in your own .github folder and add the following yaml file.

```yaml
name: Plan Only Workflow

on:
  push:
    branches-ignore:
      - "main"

permissions:
  id-token: write
  contents: read

jobs:
  dev:
    uses: markpendlebury/.github/.github/workflows/plan.yml@main
    with:
      env_name: dev
      tf_working_dir: terraform
```

### PR Workflow

**Description:**  Prerequisites -> Pytest -> Lambda Package -> Terraform Plan -> Terraform Apply -> Integration Tests -> Wait for Manual Testing -> Apply from main branch

**Usage:** To use this workflow create a main.yml in your own .github folder and add the following yaml file. For this workflow to be effective you will need to set up a protected environment and protection rules

```yaml
name: PR Workflow

on:
  pull_request:
    branches-ignore:
      - "automation/**"

permissions:
  id-token: write
  contents: read
  pull-requests: write

jobs:
  dev:
    uses: markpendlebury/.github/.github/workflows/pr.yml@main
    with:
      env_name: dev
      protected_env_name: dev-protected
      tf_working_dir: terraform
```