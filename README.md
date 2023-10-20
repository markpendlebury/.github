# Reusable Workflows and Actions Repository

Welcome to the repository containing a collection of reusable GitHub Workflows and Actions designed to simplify and enhance your CI/CD pipelines. This README provides an overview of the workflows and actions available and demonstrates how to use them in your repositories.

## Table of Contents

- [Reusable Workflows](#reusable-workflows)
  - [Main Branch Workflow](#workflow-1)
  - [Plan Only Workflow](#workflow-2)
  - [Pull Request Workflow](#workflow-2)
- [Reusable Actions](#reusable-actions)
  - [Lambda Tests and Package](#action-1)
  - [Prerequisites](#action-2)
  - [Terraform Run](#action-2)
- [Usage](#usage)

### Main Branch Workflow

**Description:** This workflow collects 4 simple actions into one workflow to allow for fast deployments of lambdas and terraform code. Note that for this workflow to be affective, your repository should follow a certain file structure. See example repo. 

You should also have your environment configured and have ACCOUNT_ID and AWS_OIDC_ROLE in variables

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
