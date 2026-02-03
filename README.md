# Square360 Shared Pantheon Workflows

This repository contains reusable GitHub Actions workflows for Square360 Pantheon projects.

## Available Workflows

### Pantheon Deployment Workflows

#### `reusable-deploy-pantheon.yml`
Deploys code to Pantheon environments with optional semantic release.

**Usage:**
```yaml
jobs:
  deploy:
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

**Inputs:**
- `pantheon_site` (required): Pantheon site name
- `target_env` (optional): Target environment (default: "dev")
- `backup_hours_threshold` (optional): Hours threshold for backup check (default: 6)
- `run_semantic_release` (optional): Whether to run semantic release first (default: true)

#### `reusable-deploy-multidev.yml`
Deploys pull requests to Pantheon multidev environments.

**Usage:**
```yaml
jobs:
  deploy:
    uses: Square360/shared-pantheon-workflows/.github/workflows/reusable-deploy-multidev.yml@main
    with:
      pantheon_site: ${{ vars.PANTHEON_SITE }}
      pr_number: ${{ github.event.pull_request.number }}
      base_ref: ${{ github.base_ref }}
      action: ${{ github.event.action }}
    secrets:
      PANTHEON_SSH_KEY: ${{ secrets.PANTHEON_SSH_KEY }}
      PANTHEON_MACHINE_TOKEN: ${{ secrets.PANTHEON_MACHINE_TOKEN }}
```

## Setup Instructions

### 1. Repository Setup

1. Create a new repository named `shared-pantheon-workflows` in your organization
2. Copy the workflow files from this repository to `.github/workflows/`
3. Update your project repositories to use these shared workflows

### 2. Required Secrets
Each project using these workflows needs these secrets configured:
- `PANTHEON_SSH_KEY`: SSH key for Pantheon access
- `PANTHEON_MACHINE_TOKEN`: Pantheon machine token
- `CI_GH_TOKEN`: GitHub token for semantic release (optional)

### 3. Required Variables
- `PANTHEON_SITE`: Your Pantheon site name

## Semantic Release Configuration

These workflows use [semantic-release](https://semantic-release.gitbook.io/) to automatically determine version numbers and generate release notes based on commit messages.

### Supported Commit Types

The workflows are configured to include **all** commit types in release notes, organized into sections:

| Commit Type | Section in Release Notes | Triggers Version Bump |
|-------------|-------------------------|----------------------|
| `feat` | Features | Minor (1.x.0) |
| `fix` | Bug Fixes | Patch (1.0.x) |
| `perf` | Performance Improvements | Patch (1.0.x) |
| `revert` | Reverts | Patch (1.0.x) |
| `refactor` | Code Refactoring | Minor (1.x.0) |
| `chore` | Chores | Patch (1.0.x) |
| `docs` | Documentation | No version bump |
| `style` | Styles | No version bump |
| `test` | Tests | No version bump |
| `build` | Build System | No version bump |
| `ci` | Continuous Integration | No version bump |

### Commit Message Format

Follow the [Conventional Commits](https://www.conventionalcommits.org/) specification:

```
<type>(<scope>): <subject>

<body>

<footer>
```

**Examples:**
- `feat(auth): add JWT authentication`
- `fix(api): resolve null pointer exception in user endpoint`
- `chore(deps): update drupal core to 10.2.0`
- `docs(readme): update deployment instructions`

### Breaking Changes

To trigger a major version bump (x.0.0), add `BREAKING CHANGE:` in the commit footer:

```
feat(api): change user endpoint response format

BREAKING CHANGE: The user endpoint now returns an array instead of an object
```

Or use the `!` notation:

```
feat!: change API response format
```

## Versioning

We recommend pinning to specific versions for production use:
```yaml
uses: Square360/shared-pantheon-workflows/.github/workflows/reusable-deploy-pantheon.yml@v1.0.0
```

For development, you can use the latest version:
```yaml
uses: Square360/shared-pantheon-workflows/.github/workflows/reusable-deploy-pantheon.yml@main
```

## Contributing

When updating workflows:
1. Test changes in a development branch
2. Create a pull request for review
3. Tag releases for version management
4. Update documentation as needed

## Support

For questions or issues with these workflows, please create an issue in this repository.