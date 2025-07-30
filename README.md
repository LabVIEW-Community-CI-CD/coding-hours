# Coding Hours

This repository contains a reusable GitHub composite action for running the
[git-hours](https://github.com/kimmobrunfeldt/git-hours) tool.

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

Run the workflow manually and the resulting `git-hours.json` file will be
available as an artifact.
