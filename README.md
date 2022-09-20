# gerp

GitHub Enforced Repository Policy

## Usage

First, create a template repository that will house your templates. After, add a `.github/workflows/template_dispatch.yaml` file with the following contents.

```yaml
name: Template dispatch

on:
  workflow_dispatch:

jobs:
  release-pr:
    uses: OliverMKing/gerp/.github/workflows/template_dispatch.yaml@main
```

Replace the `repos` input with a list of your repos.
