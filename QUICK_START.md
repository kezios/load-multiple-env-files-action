# Quick Start Guide

Get started with Load Multiple ENV Files action in 5 minutes! ⚡

## Step 1: Add Action to Your Workflow

Create or edit `.github/workflows/your-workflow.yml`:

```yaml
name: Deploy

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Load environment variables
        uses: kezios/load-multiple-env-files-action@v1
        with:
          files: |
            .env.common
            .env.production

      - name: Build
        run: npm run build
```

## Step 2: Create Your `.env` Files

Create `.env.common`:

```env
APP_NAME=my-app
LOG_LEVEL=info
```

Create `.env.production`:

```env
ENVIRONMENT=production
API_URL=https://api.example.com
```

## Step 3: Commit and Push

```bash
git add .github/workflows/ .env.common .env.production
git commit -m "Add environment configuration"
git push
```

## Step 4: Verify

1. Go to your repository's "Actions" tab
2. Click on your workflow run
3. Expand the "Load environment variables" step
4. You should see:
   ```
   ✓ Loading: .env.common
     Loaded 2 variable(s)
   ✓ Loading: .env.production
     Loaded 2 variable(s)
   ```

## That's it! 🎉

Your environment variables are now loaded and available in all subsequent workflow steps.

## Next Steps

- [Read the full documentation](README.md)
- [See more examples](examples/)
- [Learn about publishing](PUBLISHING.md)
- [Contribute](CONTRIBUTING.md)

## Common Use Cases

### Multi-Environment Setup

```yaml
env:
  IS_STAGING: ${{ github.ref_name == 'main' }}

steps:
  - uses: kezios/load-multiple-env-files-action@v1
    with:
      files: |
        .env.common
        ${{ env.IS_STAGING == 'true' && '.env.staging' || '.env.production' }}
```

### Monorepo

```yaml
- uses: your-username/load-multiple-env-action@v1
  with:
    files: |
      .env.common
      apps/api/.env.common
      apps/frontend/.env.common
```

## Troubleshooting

### Variables not available?

Make sure you're using `${{ env.VAR_NAME }}` in YAML or `$VAR_NAME` in bash scripts.

### File not found?

Check:

- File exists in your repository
- Path is relative to repository root
- File is not in `.gitignore`

## Get Help

- [Full Documentation](README.md)
- [Open an Issue](../../issues)
- [View Examples](examples/)
