# Example Project Workflows

These are example workflows that your projects can use to call the shared workflows.

## deploy-to-dev.yml
```yaml
name: Deploy to Pantheon DEV

concurrency:
  group: ${{ github.workflow }}-${{ github.event.pull_request.number || github.ref }}
  cancel-in-progress: true

on:
  pull_request:
    branches:
      - master
    types: [ closed ]
  workflow_dispatch:

jobs:
  deploy:
    if: ${{github.event.pull_request.merged == true}}
    uses: Square360/shared-pantheon-workflows/.github/workflows/reusable-deploy-pantheon.yml@main
    with:
      pantheon_site: ${{ vars.PANTHEON_SITE }}
      target_env: "dev"
      backup_hours_threshold: 6
      run_semantic_release: true
    secrets:
      PANTHEON_SSH_KEY: ${{ secrets.PANTHEON_SSH_KEY }}
      PANTHEON_MACHINE_TOKEN: ${{ secrets.PANTHEON_MACHINE_TOKEN }}
      CI_GH_TOKEN: ${{ secrets.CI_GH_TOKEN }}
```

## deploy-multidev.yml
```yaml
name: Deploy to Pantheon Multi-Dev

concurrency:
  group: ${{ github.workflow }}-${{ github.event.pull_request.number || github.ref }}
  cancel-in-progress: true

on:
  pull_request:
    types: [opened,synchronize,reopened,closed]
    branches-ignore:
      - master
  workflow_dispatch:

jobs:
  deploy_pr:
    name: Deploy Pull Request
    permissions:
      deployments: write
      contents: write
      pull-requests: read
    runs-on: ubuntu-latest
    steps:
    - uses: Square360/shared-pantheon-workflows/.github/workflows/reusable-deploy-multidev.yml@main
      with:
        pantheon_site: ${{ vars.PANTHEON_SITE }}
        pr_number: ${{ github.event.pull_request.number }}
        base_ref: ${{ github.base_ref }}
        action: ${{ github.event.action }}
      secrets:
        PANTHEON_SSH_KEY: ${{ secrets.PANTHEON_SSH_KEY }}
        PANTHEON_MACHINE_TOKEN: ${{ secrets.PANTHEON_MACHINE_TOKEN }}
```