# Contributing to Setup Krew

Thank you for your interest in contributing to setup-krew! This document provides guidelines and instructions for contributing.

## How to Contribute

### Reporting Issues

If you find a bug or have a suggestion for improvement:

1. Check if the issue already exists in the [issue tracker](https://github.com/fenio/setup-krew/issues)
2. If not, create a new issue with a clear title and description
3. Include:
   - Steps to reproduce (for bugs)
   - Expected behavior
   - Actual behavior
   - Your environment (OS, GitHub Actions runner, etc.)
   - Relevant logs or error messages

### Submitting Pull Requests

1. Fork the repository
2. Create a new branch for your feature or fix:
   ```bash
   git checkout -b feature/your-feature-name
   ```
3. Make your changes
4. Test your changes thoroughly
5. Commit your changes with a clear commit message:
   ```bash
   git commit -m "Add feature: description of your changes"
   ```
6. Push to your fork:
   ```bash
   git push origin feature/your-feature-name
   ```
7. Open a Pull Request with a clear title and description

### Development Guidelines

- Keep the action simple and focused on its core purpose
- Ensure compatibility with both Linux and macOS runners
- Support both amd64 and arm64 architectures
- Add appropriate error handling
- Update documentation for any new features
- Follow the existing code style

### Testing

Before submitting a PR, test your changes by:

1. Creating a test workflow in your fork
2. Testing on different runner types (ubuntu-latest, macos-latest)
3. Testing with different input combinations
4. Verifying all outputs are correct

Example test workflow:

```yaml
name: Test

on: [push, pull_request]

jobs:
  test:
    runs-on: ${{ matrix.os }}
    strategy:
      matrix:
        os: [ubuntu-latest, macos-latest]
    steps:
      - uses: actions/checkout@v4
      
      - name: Test action
        uses: ./
        with:
          plugins: 'ctx ns'
      
      - name: Verify installation
        run: |
          kubectl krew version
          kubectl ctx --help
          kubectl ns --help
```

### Documentation

- Update README.md for any new features or changes
- Add examples for new functionality
- Update CHANGELOG.md following [Keep a Changelog](https://keepachangelog.com/en/1.0.0/) format
- Use clear, concise language

### Code of Conduct

- Be respectful and inclusive
- Welcome newcomers and help them get started
- Focus on constructive feedback
- Assume good intentions

## Questions?

If you have questions about contributing, feel free to:
- Open an issue with the `question` label
- Start a discussion in the Discussions tab (if enabled)

Thank you for contributing to setup-krew!
