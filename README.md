# Coding Hours

This repository contains a reusable GitHub composite action for running the [git-hours](https://github.com/kimmobrunfeldt/git-hours) tool.

## Composite Action

The action lives at `.github/actions/git-hours` and performs the following:

1. Checks out your repository.
2. Installs Go and the `git-hours` tool.
3. Runs `git-hours` with an optional `window_start` input.
4. Converts the output to `git-hours.json`.
5. Exposes the generated file path as the `results` output.

## Example Workflow

Create `.github/workflows/coding-hours-report.yml`:

```yaml
name: Coding Hours Report

on:
  workflow_dispatch:
    inputs:
      window_start:
        description: 'Start date for git-hours (YYYY-MM-DD)'
        required: false

jobs:
  report:
    runs-on: ubuntu-latest
    steps:
      - uses: ./.github/actions/git-hours
        id: hours
        with:
          window_start: ${{ github.event.inputs.window_start }}
      - uses: actions/upload-artifact@v4
        with:
          name: git-hours
          path: ${{ steps.hours.outputs.results }}
```

Run the workflow manually and the resulting `git-hours.json` file will be available as an artifact.

## Organization Reports and GitHub Pages

A separate workflow, `.github/workflows/org-coding-hours.yml`, reads a list of repositories from `docs/repos.json` and generates coding hours reports for each one. The reports can be published or downloaded as artifacts. The repository also serves a GitHub Pages site under the `docs` folder that lists the repositories being tracked. You can view `docs/index.html` on GitHub Pages to see the current list.

The default `docs/repos.json` file includes open source repositories such as `ni/labview-icon-editor`, `ni/actor-framework`, and `ni/open-source`. You can modify this file to track additional projects. Running the workflow will generate a `git-hours.json` report for each repository, which can be used to recognize the contributions of collaborators.
