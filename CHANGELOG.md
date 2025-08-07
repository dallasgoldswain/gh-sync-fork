# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.1.0] - 2025-07-06

### Added

- Initial release of gh-sync-fork extension
- SSH and HTTPS protocol support for upstream remotes (SSH by default)
- Multiple merge strategies:
  - Fast-forward only (default, safest)
  - Merge commits with `--merge`
  - Rebase with `--rebase`
- Advanced workflow support with `--rebase-current`
- Dry-run mode with `--dry-run` for testing changes
- Verbose mode with `--verbose` for debugging
- Comprehensive error handling and validation
- Auto-detection of upstream repository from GitHub fork relationship
- Force-with-lease pushing for safety
- Support for custom branch selection
- Comprehensive help documentation

### Security

- Uses `--force-with-lease` instead of `--force` for safer force pushes
- Validates all user inputs
- No hardcoded credentials or sensitive data
