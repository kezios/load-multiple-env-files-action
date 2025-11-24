# Contributing to Load Multiple ENV Files Action

Thank you for your interest in contributing! 🎉

## Development Setup

1. Fork and clone the repository
2. Create a new branch for your feature/fix
3. Make your changes
4. Test your changes using the test workflow

## Testing Locally

You can test the action locally by creating test `.env` files and running the action in a workflow:

```yaml
# .github/workflows/test-local.yml
name: Test Local

on: [push]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: ./
        with:
          files: |
            .env.common
            .env.production
```

## Running Tests

Tests run automatically on push and pull requests. You can also trigger them manually:

1. Go to the Actions tab
2. Select "Test Action" workflow
3. Click "Run workflow"

## Test Coverage

Our tests cover:
- ✅ Basic file loading
- ✅ Variable overriding
- ✅ Missing file handling
- ✅ Comments and empty lines
- ✅ Whitespace handling
- ✅ Conditional file loading
- ✅ Fail on missing option
- ✅ Variable logging

## Code Style

- Use clear, descriptive variable names
- Add comments for complex logic
- Keep the shell script POSIX-compliant where possible
- Test on Ubuntu (GitHub Actions runner)

## Submitting Changes

1. Ensure all tests pass
2. Update README.md if adding features
3. Update CHANGELOG.md
4. Submit a pull request with a clear description

## Reporting Issues

When reporting issues, please include:
- GitHub Actions runner OS
- Action version
- Example `.env` files (sanitized)
- Full error messages
- Expected vs actual behavior

## Feature Requests

We welcome feature requests! Please:
- Check existing issues first
- Describe the use case
- Provide example usage

## Questions?

Feel free to open an issue for questions or discussions.

Thank you for contributing! 🙌

