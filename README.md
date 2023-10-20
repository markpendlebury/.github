# Reusable Workflows and Actions Repository

Welcome to the repository containing a collection of reusable GitHub Workflows and Actions designed to simplify and enhance your CI/CD pipelines. This README provides an overview of the workflows and actions available and demonstrates how to use them in your repositories.

## Prerequisites

Note that for the reusable workflows to be affective, your repository should follow a certain file structure. See example repo. 

You should also have your environment configured and have `ACCOUNT_ID` and `AWS_OIDC_ROLE` in variables. 

## Table of Contents

- [Reusable Workflows](#reusable-workflows)
  - [Main Branch Workflow](#main-branch-workflow)
  - [Plan Only Workflow](#plan-only-workflow)
  - [Pull Request Workflow](#pr-workflow)
- [Reusable Actions](#reusable-actions)
  - [Lambda Tests and Package](#action-1)
  - [Prerequisites](#action-2)
  - [Terraform Run](#action-2)
- [Usage](#usage)

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
    uses: IAG-Ent/.github/.github/workflows/main.yml@main
    with:
      env_name: dev
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
  dev-ecptest:
    uses: IAG-Ent/.github/.github/workflows/plan.yml@main
    with:
      env_name: dev
```

### PR Workflow

**Description:**  Prerequisites -> Pytest -> Lambda Package -> Terraform Plan -> Terraform Apply -> Wait for Manual Testing -> Apply from main branch

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
  dev-ecptest:
    uses: IAG-Ent/.github/.github/workflows/pr.yml@main
    with:
      env_name: dev
      protected_env_name: dev-protected
```