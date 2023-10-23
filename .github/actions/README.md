# Reusable Actions

Create your own unique workflows with these re-usable actions.

## Table of Contents

- [Reusable Actions](#reusable-actions)
  - [Prerequisites](#prerequisites)
  - [Lambda Tests and Package](#lambda-tests-and-package-2)
  - [Terraform run](#terraform-run)

### Prerequisites

**Description:** Set ecp_release_version and collects lambda changes. This will give you an output of lambdas_changed which will be a list of lambda naames where changes have been detected. Note your repo should follow a specific file structure for this to work. [See example repo](https://github.com/IAG-Ent/iag-ecp-example-repo).

**Usage:** To use this action:

```yaml
jobs:
  prerequisites:
    name: "Prerequisites"
    runs-on: ubuntu-latest
    environment: dev
    outputs:
      lambdas_changed: ${{ steps.prerequisites.outputs.lambdas_changed }}
    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Run Prerequisites
        id: prerequisites
        uses: IAG-Ent/.github/.github/actions/prerequisites@main
```

### Pytest

**Description:** This action will Set up Python -> Install requirements.txt -> Lint with Black -> Test with pytest -

**Usage:** To use this action:

```yaml
jobs:
  lambda_tests_and_package:
    name: "Run Pytest"
    runs-on: ubuntu-latest
    environment: dev
    needs: prerequisites
    strategy:
      matrix:
        python-version: [ "3.10" ]
    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Run Pytest
        uses: IAG-Ent/.github/.github/actions/pytest@main
        with:
          working_dir: integration_tests
          python_version_matrix: "${{ matrix.python-version  }}"
```

### Lambda Tests and Package

**Description:** This action will Set up Python -> Install requirements.txt and/or test-requirements.txt -> Lint with Black -> Test with pytest -> Package Lambda Code - > Copy package to ccoe-ecp-${{ inputs.aws_account_id }}-eu-west-1-lambda-code

**Usage:** To use this action:

```yaml
jobs:
  lambda_tests_and_package:
    name: "Run Lambda Tests and Package"
    runs-on: ubuntu-latest
    environment: dev
    needs: prerequisites
    strategy:
      matrix:
        python-version: [ "3.10" ]
    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Run Lambda Tests and Package
        uses: IAG-Ent/.github/.github/actions/lambda_tests_and_package@main
        with:
          lambda_names: "${{ needs.prerequisites.outputs.lambdas_changed  }}"
          python_version_matrix: "${{ matrix.python-version  }}"
          aws_role: ${{ vars.AWS_OIDC_ROLE }}
          aws_account_id: ${{ vars.ACCOUNT_ID }}
```

### Terraform Run 

**Description:** This action will Configure AWS credentials -> setup-terraform -> Terraform Backend Config -> Terraform Init -> Terraform Validate -> Terraform Plan -> Terraform Apply

**Usage:** To use this action:

```yaml
jobs:
  terraform_plan:
    name: "Terraform Plan dev-ecptest"
    runs-on: ubuntu-latest
    environment: ${{ inputs.env_name }}
    concurrency: ${{ inputs.env_name }}
    needs: 
      - prerequisites
      - lambda_tests_and_package
    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Run Terraform
        uses: IAG-Ent/.github/.github/actions/terraform_run@main
        with:
          aws_role: ${{ vars.AWS_OIDC_ROLE }}
          terraform_action: 'plan'
          terraform_version: 1.5.6
          tfstate_bucket: ccoe-ecp-${{ vars.ACCOUNT_ID }}-eu-west-1-tfstate
          tfstate_dynamodb_table: ccoe-ecp-${{ vars.ACCOUNT_ID }}-eu-west-1-tfstate
          tf_working_dir: terraform
```