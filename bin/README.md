# Development Support Files

This directory contains development tools and scripts that are used for maintaining code quality and running tests. These files are **not required** for installing or running the main `yt` application.

## Files

- **`ci`** - Continuous Integration script that runs all quality checks
  - Run `bin/ci` to validate code formatting, style, and tests
  - Run `bin/ci --fix` to automatically fix formatting and style issues
- **`coverage`** - Test coverage report generator and formatter
- **`yt-test`** - Test suite runner with coverage collection

## Usage

These scripts are primarily used during development and by the pre-commit hooks to ensure code quality. End users who just want to use the YouTube video manager should ignore this directory and run the main `yt` script directly.
