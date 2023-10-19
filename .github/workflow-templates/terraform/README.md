# Terraform Run Reusable Workflow

## Using Terraform Run in your repository

This currently is in progress.

In the calling workflow file, use the `uses` property to specify the location and version of a
reusable workflow file to run as a job.

```yml
name: {Job name}
on:
  pull_request:
jobs:
  terraform_plan:
    uses: IAG-Ent/.github/.github/workflow-templates/terraform/terraform_run.yml@main
    with:
        aws_role: ${{ vars.AWS_DNS_OIDC_ROLE }}
        terraform_action: 'plan'
        terraform_version: 1.5.6
        tfstate_bucket: ccoe-ecp-${{ vars.DNS_ACCOUNT_ID }}-eu-west-1-tfstate
        tfstate_dynamodb_table: ccoe-ecp-${{ vars.DNS_ACCOUNT_ID }}-eu-west-1-tfstate
        tf_working_dir: terraform
```
