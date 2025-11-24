# Usage Examples

This directory contains various examples showing how to use the Load Multiple ENV Files action in different scenarios.

## Examples Overview

### [`usage-example.yml`](./usage-example.yml)

A comprehensive workflow file showing:

- Basic usage
- Multi-environment setup
- Monorepo configuration
- Debug mode with variable logging
- Strict mode (fail on missing files)
- Matrix strategy deployment
- Full CI/CD pipeline

## Quick Start

### Simple Two-File Setup

```yaml
- uses: kezios/load-multiple-env-files-action@v1
  with:
    files: |
      .env.common
      .env.production
```

### Environment-Based Loading

```yaml
env:
  ENVIRONMENT: ${{ github.ref_name == 'main' && 'staging' || 'production' }}

steps:
  - uses: kezios/load-multiple-env-files-action@v1
    with:
      files: |
        .env.common
        .env.${{ env.ENVIRONMENT }}
```

### Monorepo Pattern

```yaml
- uses: kezios/load-multiple-env-files-action@v1
  with:
    files: |
      .env.common
      .env.production
      apps/api/.env.common
      apps/api/.env.production
      apps/frontend/.env.common
      apps/frontend/.env.production
```

## Testing Examples Locally

1. Copy the example workflow to `.github/workflows/` in your repository
2. Create the necessary `.env` files
3. Push to trigger the workflow
4. Check the Actions tab to see the results

## File Structure

For the examples to work, your repository should have:

```
your-repo/
├── .env.common
├── .env.staging
├── .env.production
├── apps/
│   ├── api/
│   │   ├── .env.common
│   │   ├── .env.staging
│   │   └── .env.production
│   └── frontend/
│       ├── .env.common
│       ├── .env.staging
│       └── .env.production
└── .github/
    └── workflows/
        └── deploy.yml
```

## Common Patterns

### Pattern 1: Cascade Configuration

Load from general to specific:

```
.env.common          → Base config for all environments
.env.production      → Override/add production-specific vars
apps/api/.env.common → API-specific base config
apps/api/.env.production → API production overrides
```

### Pattern 2: Feature Branch Deployment

```yaml
- uses: kezios/load-multiple-env-files-action@v1
  with:
    files: |
      .env.common
      .env.preview
      .env.${{ github.ref_name }}  # Branch-specific if exists
```

### Pattern 3: Multi-Cloud Deployment

```yaml
strategy:
  matrix:
    cloud: [aws, azure, gcp]
steps:
  - uses: kezios/load-multiple-env-files-action@v1
    with:
      files: |
        .env.common
        .env.${{ matrix.cloud }}
```

## Tips

1. **Order matters**: Files are loaded sequentially, later files override earlier ones
2. **Missing files are OK**: By default, missing files are skipped (use `fail-on-missing: true` to change this)
3. **Use logging for debugging**: Enable `log-variables: true` to see which variables are loaded
4. **Keep secrets in GitHub Secrets**: Don't commit sensitive values to `.env` files

## Need Help?

- Check the [main README](../README.md) for full documentation
- See [CONTRIBUTING.md](../CONTRIBUTING.md) for development guidelines
- Open an [issue](../../issues) if you find bugs or have questions
