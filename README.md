# gh-sync-fork — GitHub CLI extension (SSH by default)

Sync your local fork from **upstream** and push to **origin**.  
New options supported: `--protocol ssh|https`, `--rebase-current`, `--dry-run`, **`--verbose`**.

## Install (local)
```bash
gh extension install .
```

## Usage
```bash
gh sync-fork [options]
```

**Examples**
```bash
gh sync-fork                           # SSH upstream, FF-only, push
gh sync-fork -b develop                # sync a specific branch
gh sync-fork --rebase -f               # rebase base branch then safe force push
gh sync-fork --rebase-current -f       # also rebase your starting branch onto base
gh sync-fork --protocol https          # prefer HTTPS for upstream
gh sync-fork --dry-run                 # print intended mutating commands
gh sync-fork --verbose                 # enable Git trace + shell xtrace (no secrets masked)
```

## `--verbose` details
- Exports `GIT_TRACE=1`, `GIT_TRACE_SETUP=1`, `GIT_TRACE_PERFORMANCE=1`, `GIT_CURL_VERBOSE=1`
- For SSH, sets `GIT_SSH_COMMAND='ssh -vvv'` for extra transport logs
- Adds `-v` to `git fetch` and `git push`
- Enables `set -x` (shell xtrace) unless `--dry-run` is used

**Note:** Verbose output may include URLs and ref names. Avoid sharing raw logs publicly if they may contain private repo names or internal refs.
