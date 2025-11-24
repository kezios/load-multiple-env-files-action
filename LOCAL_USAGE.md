# Using the Action Locally

This guide shows how to use the action before publishing it to GitHub Marketplace.

## Method 1: Same Repository

If your action is in the same repository as your workflows:

```yaml
- name: Load environment variables
  uses: ./ # Points to root of current repository
  with:
    files: |
      .env.common
      .env.production
```

**Example:** `blink-snap-action` can use this method if the action is in its root:

```
blink-snap-action/
├── action.yml              # Your action
├── .github/
│   └── workflows/
│       └── deploy.yml      # Your workflow using ./
```

## Method 2: Same Organization/User

Reference by repository path:

```yaml
- name: Load environment variables
  uses: kezios/load-multiple-env-files-action@main
  with:
    files: |
      .env.common
      .env.production
```

You can also use specific branches or commits:

```yaml
# Use specific branch
uses: kezios/load-multiple-env-files-action@develop

# Use specific commit SHA (most secure)
uses: kezios/load-multiple-env-files-action@abc123def456

# Use tag
uses: kezios/load-multiple-env-files-action@v1.0.0
```

## Method 3: Private Repository

For private repositories, the action will work automatically if:

- The workflow repository and action repository are in the same organization
- The `GITHUB_TOKEN` has access to both repositories

```yaml
- name: Load environment variables
  uses: your-org/private-action@main
  with:
    files: |
      .env.common
      .env.production
```

If you need custom access, use a Personal Access Token:

```yaml
- name: Checkout action repository
  uses: actions/checkout@v4
  with:
    repository: your-org/private-action
    token: ${{ secrets.PAT_TOKEN }}
    path: .github/actions/load-env

- name: Load environment variables
  uses: ./.github/actions/load-env
  with:
    files: |
      .env.common
      .env.production
```

## Example: Using in blink-snap-action

### Option A: Copy action into repository

1. Copy action to blink-snap-action:

   ```bash
   cd blink-snap-action
   mkdir -p .github/actions/load-multiple-env
   cp ../load-multiple-env-action/action.yml .github/actions/load-multiple-env/
   ```

2. Use in workflow:
   ```yaml
   - name: Load environment variables
     uses: ./.github/actions/load-multiple-env
     with:
       files: |
         .env.common
         ${{ env.IS_STAGING == 'true' && '.env.staging' || '.env.production' }}
         apps/front/.env.common
         ${{ env.IS_STAGING == 'true' && 'apps/front/.env.staging' || 'apps/front/.env.production' }}
   ```

### Option B: Reference separate repository

1. Make sure `load-multiple-env-action` is pushed to GitHub

2. Use in blink-snap-action workflow:
   ```yaml
   - name: Load environment variables
     uses: kezios/load-multiple-env-files-action@main
     with:
       files: |
         .env.common
         ${{ env.IS_STAGING == 'true' && '.env.staging' || '.env.production' }}
         apps/front/.env.common
         ${{ env.IS_STAGING == 'true' && 'apps/front/.env.staging' || 'apps/front/.env.production' }}
   ```

## Complete Example for blink-snap-action

Replace the current bash script in `front-build-and-deploy.yml`:

**Before:**

```yaml
- name: Load environment variables
  run: |
    ENV_SUFFIX="${{ env.IS_STAGING == 'true' && 'staging' || 'production' }}"

    for file in \
      ".env.common" \
      ".env.${ENV_SUFFIX}" \
      "apps/front/.env.common" \
      "apps/front/.env.${ENV_SUFFIX}"; do
      if [ -f "$file" ]; then
        echo "Loading $file"
        cat "$file" >> $GITHUB_ENV
      else
        echo "File $file not found, skipping"
      fi
    done
```

**After:**

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

## Testing Locally

### Test in load-multiple-env-action repo

The test workflow (`.github/workflows/test.yml`) runs automatically and tests all scenarios.

### Test in blink-snap-action repo

1. Push the updated workflow
2. Trigger the workflow (push to main or prod)
3. Check the Actions tab
4. Verify the "Load environment variables" step shows:
   ```
   🔧 Loading environment files...
   ✓ Loading: .env.common
     Loaded X variable(s)
   ✓ Loading: .env.production
     Loaded Y variable(s)
   ...
   ```

## Advantages of Using the Action

### Before (Bash Script)

- ❌ Harder to maintain
- ❌ No reusability
- ❌ Duplicated across repositories
- ❌ No summary output
- ❌ Limited error handling

### After (GitHub Action)

- ✅ Clean, declarative syntax
- ✅ Reusable across all repositories
- ✅ Centralized maintenance
- ✅ Beautiful summary output
- ✅ Better error handling
- ✅ Optional features (logging, fail-on-missing)

## Migration Checklist

- [ ] Action repository created and pushed
- [ ] Tests passing in action repository
- [ ] Decided on usage method (local copy vs reference)
- [ ] Updated workflow to use action
- [ ] Tested in staging environment
- [ ] Verified variables are loaded correctly
- [ ] Removed old bash script
- [ ] Documented usage in project README

## Next Steps

1. Test the action locally
2. Verify it works in your use case
3. Optionally publish to GitHub Marketplace
4. Update documentation
5. Share with your team!

## Troubleshooting

### "Action not found"

- Ensure repository exists and is accessible
- Check repository name and branch
- Verify `GITHUB_TOKEN` permissions

### "Variables not loaded"

- Check action output for errors
- Verify `.env` files exist
- Enable `log-variables: true` for debugging

### "Permission denied"

- For private repos, ensure proper access
- Use PAT if needed
- Check organization settings

## Support

- Check [README.md](README.md) for full documentation
- See [examples](examples/) for more use cases
- Open an [issue](../../issues) for help
