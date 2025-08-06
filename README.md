# gh-sync-fork — GitHub CLI extension

Sync your local fork from **upstream** and push to **origin**. Supports SSH and HTTPS protocols, various merge strategies, and advanced workflow options.

## Features

- **Auto-detection**: Automatically detects upstream repository from GitHub fork relationship or existing `upstream` remote
- **Protocol support**: Choose between SSH (default) or HTTPS for upstream remote
- **Merge strategies**: Fast-forward only (default), merge commits, or rebase
- **Advanced workflows**: Rebase your current feature branch after syncing the base branch
- **Safety features**: Dry-run mode, force-with-lease pushing, comprehensive error handling
- **Debugging**: Verbose mode with Git tracing and detailed logging

## Install

### From GitHub (recommended)

```bash
gh extension install dallasgoldswain/gh-sync-fork
```

### Local development

```bash
# Clone the repository first
git clone https://github.com/dallasgoldswain/gh-sync-fork.git
cd gh-sync-fork

# Install locally
gh extension install .
```

## Usage

```bash
gh sync-fork [options]
```

### Options

- `-b, --branch <name>` - Branch to sync (default: upstream default branch)
- `-s, --source <owner/repo>` - Upstream source repo owner/name override
- `--protocol <ssh|https>` - Protocol for upstream remote (default: ssh)
- `--merge` - Use a merge commit (default is fast-forward only)
- `--rebase` - Rebase local branch onto upstream/\<branch\>
- `--rebase-current` - After syncing, rebase the branch you started on onto \<branch\>
- `-f, --force` - Use --force-with-lease when pushing (useful with rebase)
- `--no-push` - Do not push to origin after updating locally
- `--dry-run` - Print intended commands (read-only queries still run); make no changes
- `--verbose` - Extra Git and network logs (sets GIT_TRACE/HTTP trace; shell xtrace)
- `-h, --help` - Show help

## Examples

```bash
gh sync-fork                           # SSH upstream, FF-only, push default branch
gh sync-fork -b develop                # sync a specific branch
gh sync-fork --rebase -f               # rebase default branch then force-with-lease push
gh sync-fork --rebase-current -f       # also rebase your current feature branch onto updated base
gh sync-fork --protocol https          # use HTTPS for upstream remote
gh sync-fork --dry-run                 # show what would happen without making changes
gh sync-fork --verbose                 # enable detailed Git tracing
gh sync-fork --merge                   # use merge commits instead of fast-forward
gh sync-fork --no-push                 # sync locally but don't push to origin
gh sync-fork -s owner/repo             # override upstream repository detection
```

## Verbose Mode Details

- Exports `GIT_TRACE=1`, `GIT_TRACE_SETUP=1`, `GIT_TRACE_PERFORMANCE=1`, `GIT_CURL_VERBOSE=1`
- For SSH, sets `GIT_SSH_COMMAND='ssh -vvv'` for extra transport logs
- Adds `-v` to `git fetch` and `git push` commands
- Enables `set -x` (shell xtrace) unless `--dry-run` is used

**Note:** Verbose output may include URLs and ref names. Avoid sharing raw logs publicly if they may contain private repository names or internal references.

## How It Works

1. **Upstream Detection**: Automatically detects the upstream repository from:
   - GitHub's fork relationship (using `gh repo view --json parent`)
   - Existing `upstream` remote (if configured)
   - Manual override with `--source owner/repo`

2. **Remote Management**: Ensures the `upstream` remote exists and points to the correct repository using your chosen protocol (SSH by default, HTTPS optional)

3. **Branch Handling**:
   - Uses upstream's default branch if no branch specified
   - Creates local branch if it doesn't exist
   - Tracks from origin if the branch exists there

4. **Sync Strategies**:
   - **Fast-forward** (default): Safe, no merge commits, fails if history diverged
   - **Merge**: Creates merge commits, preserves history
   - **Rebase**: Replays your commits on top of upstream, cleaner history

5. **Advanced Workflows**: With `--rebase-current`, after syncing the base branch, it will also rebase your original feature branch onto the updated base

## Requirements

- [GitHub CLI](https://cli.github.com/) (`gh`)
- Git
