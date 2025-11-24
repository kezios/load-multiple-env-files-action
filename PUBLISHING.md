# Publishing This Action

## Prerequisites

- GitHub account
- Repository must be public (for GitHub Marketplace)
- Action must be in the root of the repository

## Steps to Publish

### 1. Create a Release

```bash
# Tag the release
git tag -a v1.0.0 -m "Release version 1.0.0"
git push origin v1.0.0

# Also create/update the major version tag
git tag -fa v1 -m "Update v1 tag"
git push origin v1 --force
```

### 2. Create GitHub Release

1. Go to your repository on GitHub
2. Click on "Releases" → "Create a new release"
3. Select the tag you just created (v1.0.0)
4. Title: "v1.0.0 - Initial Release"
5. Description: Copy from CHANGELOG.md
6. Click "Publish release"

### 3. Publish to GitHub Marketplace

1. Go to your repository settings
2. Check "Publish this Action to the GitHub Marketplace"
3. Fill in the required information:
   - Primary Category: "Utilities"
   - Additional Categories: "Deployment", "Testing"
4. Accept the terms
5. Click "Publish"

## Version Management

This action follows semantic versioning:

- **Major version (v1, v2)**: Breaking changes
- **Minor version (v1.1, v1.2)**: New features, backward compatible
- **Patch version (v1.0.1, v1.0.2)**: Bug fixes

### Best Practices

1. **Always maintain major version tags** (v1, v2, etc.)
   - Users reference `@v1` to get the latest v1.x.x automatically
   - Update the v1 tag with each release

2. **Tag strategy**:
   ```bash
   # For version 1.2.3
   git tag v1.2.3      # Exact version
   git tag -f v1       # Update major version pointer
   git push origin v1.2.3 v1
   ```

3. **Breaking changes**:
   - Increment major version (v2.0.0)
   - Document migration path in CHANGELOG.md
   - Keep v1 branch for critical bug fixes

## After Publishing

Once published, users can use your action:

```yaml
# Using specific version (recommended for production)
- uses: your-username/load-multiple-env-action@v1.0.0

# Using major version (gets latest v1.x.x)
- uses: your-username/load-multiple-env-action@v1

# Using main branch (not recommended)
- uses: your-username/load-multiple-env-action@main
```

## Updating the Action

### For Bug Fixes (Patch)

```bash
# Make your changes
git add .
git commit -m "fix: resolve issue with whitespace handling"

# Create patch version
git tag v1.0.1
git tag -fa v1 -m "Update v1 to v1.0.1"
git push origin v1.0.1 v1 --force
```

### For New Features (Minor)

```bash
# Make your changes
git add .
git commit -m "feat: add support for variable expansion"

# Create minor version
git tag v1.1.0
git tag -fa v1 -m "Update v1 to v1.1.0"
git push origin v1.1.0 v1 --force
```

### For Breaking Changes (Major)

```bash
# Make your changes
git add .
git commit -m "BREAKING CHANGE: change input parameter name"

# Create major version
git tag v2.0.0
git tag v2 -m "Create v2 tag"
git push origin v2.0.0 v2

# Keep v1 branch for maintenance
git checkout -b v1-maintenance v1
git push origin v1-maintenance
```

## Marketplace Badge

Add this to your README.md:

```markdown
[![GitHub Marketplace](https://img.shields.io/badge/Marketplace-Load%20Multiple%20ENV%20Files-blue.svg?colorA=24292e&colorB=0366d6&style=flat&longCache=true&logo=github)](https://github.com/marketplace/actions/load-multiple-env-files)
```

## Testing Before Publishing

Always test your action before publishing:

1. Create a test repository
2. Reference your action using the repository path:
   ```yaml
   - uses: your-username/load-multiple-env-action@your-branch
   ```
3. Verify all features work as expected
4. Check test workflow passes

## Maintenance

- Monitor issues and pull requests
- Update dependencies periodically
- Keep documentation up to date
- Respond to user feedback

## Useful Links

- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Publishing Actions](https://docs.github.com/en/actions/creating-actions/publishing-actions-in-github-marketplace)
- [Action Metadata Syntax](https://docs.github.com/en/actions/creating-actions/metadata-syntax-for-github-actions)

