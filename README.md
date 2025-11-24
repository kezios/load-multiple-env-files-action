# Load Multiple ENV Files Action

[![Test Action](https://github.com/kezios/load-multiple-env-files-action/actions/workflows/test.yml/badge.svg)](https://github.com/kezios/load-multiple-env-files-action/actions/workflows/test.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

A GitHub Action to load multiple `.env` files in a specific order. Files loaded later override variables from earlier files, allowing for cascading configuration (e.g., common → environment-specific).

## 🚀 Quick Start

Check out the [Quick Start Guide](QUICK_START.md) to get up and running in 5 minutes!

## Features

- ✅ Load multiple `.env` files in order
- ✅ Skip missing files automatically (or fail if desired)
- ✅ Later files override earlier ones
- ✅ Clean summary output
- ✅ Optional variable name logging for debugging
- ✅ Supports comments and empty lines in `.env` files

## Usage

### Basic Example

```yaml
- name: Load environment variables
  uses: kezios/load-multiple-env-files-action@v1
  with:
    files: |
      .env.common
      .env.production
```

### Advanced Example with Conditional Files

```yaml
- name: Load environment variables
  uses: kezios/load-multiple-env-files-action@v1
  with:
    files: |
      .env.common
      ${{ github.ref_name == 'main' && '.env.staging' || '.env.production' }}
      apps/api/.env.common
      ${{ github.ref_name == 'main' && 'apps/api/.env.staging' || 'apps/api/.env.production' }}
    log-variables: true
```

### Full Workflow Example

```yaml
name: Deploy Application

on:
  push:
    branches:
      - main
      - prod

env:
  IS_STAGING: ${{ github.ref_name == 'main' }}

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Load environment variables
        uses: kezios/load-multiple-env-files-action@v1
        with:
          files: |
            .env.common
            ${{ env.IS_STAGING == 'true' && '.env.staging' || '.env.production' }}
            apps/front/.env.common
            ${{ env.IS_STAGING == 'true' && 'apps/front/.env.staging' || 'apps/front/.env.production' }}

      - name: Build application
        run: npm run build
        # All variables from .env files are now available in $GITHUB_ENV
```

## Inputs

| Input             | Description                                                             | Required | Default |
| ----------------- | ----------------------------------------------------------------------- | -------- | ------- |
| `files`           | List of `.env` files to load (one per line). Files are loaded in order. | Yes      | -       |
| `log-variables`   | Whether to log loaded variable names (not values) for debugging         | No       | `false` |
| `fail-on-missing` | Whether to fail if any file is missing                                  | No       | `false` |

## Outputs

This action doesn't have explicit outputs, but it loads all variables into `$GITHUB_ENV`, making them available to all subsequent steps in the job.

## How It Works

1. **File Order**: Files are processed in the order they appear in the `files` input
2. **Variable Override**: If the same variable exists in multiple files, the last one wins
3. **Comments**: Lines starting with `#` are ignored
4. **Empty Lines**: Empty lines are skipped
5. **Missing Files**: By default, missing files are skipped with a warning. Set `fail-on-missing: true` to fail instead

## Example Output

```
🔧 Loading environment files...
✓ Loading: .env.common
  Loaded 5 variable(s)
✓ Loading: .env.production
  Loaded 3 variable(s)
⚠ Skipping (not found): .env.local
✓ Loading: apps/front/.env.common
  Loaded 7 variable(s)

📊 Summary:
  • Files loaded: 3
  • Files skipped: 1
  • Total variables: 15
✅ Done!
```

## Use Cases

### Multi-Environment Configuration

Load common variables first, then override with environment-specific ones:

```yaml
with:
  files: |
    .env.common
    .env.${{ env.ENVIRONMENT }}
```

### Monorepo with Multiple Apps

Load workspace-level config, then app-specific config:

```yaml
with:
  files: |
    .env.common
    .env.production
    apps/api/.env.common
    apps/api/.env.production
    apps/frontend/.env.common
    apps/frontend/.env.production
```

### Debug Mode

Enable variable name logging to see what's being loaded:

```yaml
with:
  files: |
    .env.common
    .env.production
  log-variables: true
```

Output:

```
✓ Loading: .env.common
  → DATABASE_URL
  → API_KEY
  → LOG_LEVEL
  Loaded 3 variable(s)
```

## File Format

Your `.env` files should follow the standard format:

```env
# This is a comment
VARIABLE_NAME=value
ANOTHER_VAR=another value

# Empty lines are fine
DATABASE_URL=postgresql://localhost:5432/db
```

## Tips

- **Order Matters**: List files from most general to most specific
- **Optional Files**: Missing files are skipped by default, so you can safely list optional files
- **Variable Expansion**: This action doesn't expand variables (e.g., `${VAR}`). Variables are loaded as-is
- **Multiline Values**: This action doesn't support multiline values (use secrets for complex values)

## License

MIT

## Contributing

Contributions welcome! Please open an issue or PR.

## Related Actions

- [xom9ikk/dotenv](https://github.com/xom9ikk/dotenv) - Load a single .env file
- [cardinalby/export-env-action](https://github.com/cardinalby/export-env-action) - Export variables from files
- [falti/dotenv-action](https://github.com/falti/dotenv-action) - Another dotenv loader
