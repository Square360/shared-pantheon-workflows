# Contributing to Square360 Shared Workflows

Thank you for your interest in contributing to our shared workflows!

## Development Process

1. **Fork the repository** and create a feature branch
2. **Make your changes** following our coding standards
3. **Test thoroughly** in a development environment
4. **Create a pull request** with a clear description
5. **Wait for review** from the maintainers

## Testing Workflows

Before submitting changes:

1. Test workflows in a separate repository
2. Verify all input parameters work correctly
3. Check that secrets are properly handled
4. Ensure documentation is updated

### Local Testing of Semantic Release Configuration

To test semantic-release configuration changes locally before deployment:

1. **Install dependencies:**
   ```bash
   npm install semantic-release @semantic-release/changelog @semantic-release/commit-analyzer @semantic-release/release-notes-generator @semantic-release/github @semantic-release/git conventional-changelog-conventionalcommits
   ```

2. **Create a test configuration:**
   - Copy the semantic-release config from the workflow to `package.json`
   - Ensure the `release.branches` array includes your current branch

3. **Set GitHub token:**
   ```bash
   export GITHUB_TOKEN=your_github_token_here
   ```
   Or use the secure prompt method:
   ```bash
   read -s GITHUB_TOKEN && export GITHUB_TOKEN
   ```

4. **Run dry-run test:**
   ```bash
   npx semantic-release --dry-run
   ```

5. **Review the output:**
   - Check that commits are being analyzed correctly
   - Verify merge commits are recognized (look for "Merge pull request #" pattern)
   - Confirm all commit types appear in the release notes
   - Validate the determined release version is correct

6. **Clean up:**
   - All testing files are gitignored (node_modules/, package.json, etc.)
   - No need to manually clean up after testing

**Note:** Dry-run mode will not create actual releases or modify the repository. It only shows what would happen.

## Versioning

We use semantic versioning:
- **Major**: Breaking changes to workflow interfaces
- **Minor**: New features, backward compatible
- **Patch**: Bug fixes, documentation updates

## Code Standards

- Use clear, descriptive step names
- Include helpful comments for complex logic
- Follow YAML best practices
- Update documentation for any interface changes

## Questions?

Create an issue or reach out to the DevOps team.
