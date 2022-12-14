# gerp

GitHub Enforced Repository Patterning (GERP) uses a single template repo to synchronize repository templates via batch Pull Requests. This is fantastic for the things that live in your `.github` folder (codeowners, issue template, pull request templates, actions) and so much more! Essentially any file that you need to be consistient across multiple repos fits perfectly with GERP.

## Usage

The easiest way to get started is with our [template](https://github.com/gerp-project/gerp-template).

### Manual Setup

First, create a template repository that will house your templates. After, add a `.github/workflows/gerp_sync_templates.yaml` file with the following contents.

```yaml
name: GERP Sync

on:
  push:
    branches:
      - main

jobs:
  sync-templates:
    uses: gerp-project/gerp/.github/workflows/sync_templates.yaml@v0
    secrets:
      PAT: ${{ secrets.PAT }}
```

Create a `.gerp/config.json` file.

```json
{
  "repos": {
    "org/repo": {
      "inputs": {}
    },
    "org2/repo2": {
      "inputs": {}
    }
  }
}
```

The key of each entry in repos is the full repository name (org + name). Inputs are the input object expected by [mustache](https://www.npmjs.com/package/mustache) for templating.

Then create a `template` directory. The inside the template directory represent the root directory of your supported repos. For example, `template/.github/workflows/test.yaml` maps to `.github/workflows/test.yaml` in your supported repos. Mustache expressions can be used inside files in the `template` directory.

## Advanced Usage

### GPG Commit Signing

```yaml
name: GERP Sync

on:
  push:
    branches:
      - main

jobs:
  sync-templates:
    uses: OliverMKing/gerp/.github/workflows/template_dispatch.yaml@main
    with:
      committer: gpgUserName <gpgUser@mail.com>
    secrets:
      PAT: ${{ secrets.PAT }}
      GPG_PRIVATE_KEY: ${{ secrets.GPG_PRIVATE_KEY }}
      GPG_PASSPHRASE: ${{ secrets.GPG_PASSPHRASE }}
```

Note: Your committer must exactly match the email used for your GPG key

See [crazy-max/ghaction-import-gpg](https://github.com/crazy-max/ghaction-import-gpg) for details on `GPG_PRIVATE_KEY` and `GPG-PASSPHRASE`
