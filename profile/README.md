## Welcome

This is the **IAG** Organization enabling collaboration across many projects at once. Owners and administrators can manage member access to the organization's data and projects.

# Getting started with Git and Github

Some useful labs that are worth following if you are new to Git and GitHub, from [Learning Labs](https://lab.github.com/):

- [First Day on GitHub](https://skills.github.com/#first-day-on-github)
- [First Week on GitHub](https://skills.github.com/#first-week-on-github)

## About reusable workflows

To avoid code duplication of GitHub Actions workflow files across thousands of repositories, we
utilize [reusable workflows](https://docs.github.com/en/actions/using-workflows/reusing-workflows).

In the calling workflow file, use the `uses` property to specify the location and version of a
reusable workflow file to run as a job.

```yml
name: {Job name}
on:
  pull_request:
jobs:
  {topic}-{workflow}:
    uses: IAG-Ent/.github/.github/workflow-templates/{topic}/{topic}-{workflow}.yml@main
```

Please note that the individual workflows have different (optional) parameters that you can pass
either as `input`s or `secret`s. 