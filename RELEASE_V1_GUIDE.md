# Release v1.0.0 Guide

## Changes Made Before Release

### Bug Fixes
- ✅ Fixed variable loading by improving whitespace trimming
- ✅ Added validation to ensure lines contain `=`
- ✅ Improved `$GITHUB_ENV` file checking
- ✅ Made variable counting more robust

### Documentation Updates
- ✅ Updated all URLs to `kezios/load-multiple-env-files-action`
- ✅ Fixed README formatting
- ✅ Updated author to "Kezios"
- ✅ Updated copyright to "Kezios" (MIT License)

## Steps to Create Release v1

### 1. Commit and Push Latest Changes

```bash
cd /Users/justin/load-multiple-env-action

# Check status
git status

# Add all changes
git add .

# Commit
git commit -m "fix: improve variable loading and update documentation URLs"

# Push
git push origin main
```

### 2. Wait for Tests to Pass

1. Go to https://github.com/kezios/load-multiple-env-files-action/actions
2. Wait for the "Test Action" workflow to complete
3. Verify all tests pass ✅

### 3. Create Release Tags

```bash
# Create v1.0.0 tag
git tag -a v1.0.0 -m "Release v1.0.0 - Initial stable release"

# Create v1 tag (for users who want latest v1.x.x)
git tag -a v1 -m "Release v1"

# Push both tags
git push origin v1.0.0 v1
```

### 4. GitHub Release Will Be Created Automatically

The release workflow (`.github/workflows/release.yml`) will automatically:
- Create a GitHub Release
- Extract changelog notes
- Mark it as the latest release

### 5. Verify Release

1. Go to https://github.com/kezios/load-multiple-env-files-action/releases
2. Verify v1.0.0 release appears
3. Check release notes are correct

## Using the Action After Release

### In blink-snap-action

Update `.github/workflows/front-build-and-deploy.yml`:

```yaml
- name: Load environment variables
  uses: kezios/load-multiple-env-files-action@v1
  with:
    files: |
      .env.common
      ${{ env.IS_STAGING == 'true' && '.env.staging' || '.env.production' }}
      apps/front/.env.common
      ${{ env.IS_STAGING == 'true' && 'apps/front/.env.staging' || 'apps/front/.env.production' }}
```

### In Any Other Project

```yaml
- name: Load environment variables
  uses: kezios/load-multiple-env-files-action@v1
  with:
    files: |
      .env.common
      .env.production
```

## Version References

Users can reference the action in three ways:

1. **By major version** (recommended - gets latest v1.x.x)
   ```yaml
   uses: kezios/load-multiple-env-files-action@v1
   ```

2. **By exact version** (most stable)
   ```yaml
   uses: kezios/load-multiple-env-files-action@v1.0.0
   ```

3. **By branch** (for testing)
   ```yaml
   uses: kezios/load-multiple-env-files-action@main
   ```

## What's Included in v1.0.0

### Core Features
- ✅ Load multiple `.env` files in order
- ✅ Skip missing files automatically
- ✅ Variable override support
- ✅ Comment and empty line handling
- ✅ Clean summary output

### Advanced Features
- ✅ `log-variables` option for debugging
- ✅ `fail-on-missing` option for strict mode
- ✅ Detailed statistics

### Documentation
- ✅ Comprehensive README
- ✅ Quick start guide
- ✅ Usage examples
- ✅ Local usage instructions
- ✅ Publishing guide
- ✅ Contributing guidelines

### Testing
- ✅ 7 automated test scenarios
- ✅ Full test coverage
- ✅ CI/CD pipeline

## After Release

### Announce the Release

Share with your team:
- Link to release: https://github.com/kezios/load-multiple-env-files-action/releases/tag/v1.0.0
- Link to documentation: https://github.com/kezios/load-multiple-env-files-action#readme

### Next Steps

1. Use in blink-snap-action
2. Monitor for issues
3. Collect feedback
4. Plan v1.1.0 improvements

## Troubleshooting

### If tests fail:
1. Check the Actions tab for error details
2. Fix issues
3. Commit and push
4. Don't create tags until tests pass

### If release workflow fails:
1. Check `.github/workflows/release.yml`
2. Verify CHANGELOG.md format
3. Delete and recreate tags if needed:
   ```bash
   git tag -d v1.0.0 v1
   git push origin :refs/tags/v1.0.0 :refs/tags/v1
   # Then recreate and push again
   ```

## Future Releases

For future releases (v1.1.0, v1.2.0, etc.):

1. Update CHANGELOG.md
2. Commit changes
3. Create new tag
4. Update v1 tag to point to latest:
   ```bash
   git tag -fa v1 -m "Update v1 to v1.1.0"
   git push origin v1 --force
   ```

---

**Ready to release? Follow the steps above! 🚀**

