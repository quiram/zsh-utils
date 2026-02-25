# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Setup

To make all scripts available on PATH:

```zsh
./make-available
source ~/.zshrc
```

This appends a `source add-to-path` line to `~/.zshrc`, which adds every subfolder of the repo to `$PATH`.

## Repository Structure

This is a collection of standalone zsh utility scripts organized by category:

- **`common/utils`** — shared library sourced by most scripts; provides `log()`, `fail()`, and `silentExec()` helpers, plus the `trap`/`TOP_PID` pattern for killing scripts across nested function calls
- **`git/utils`** — shared git library; detects whether the default branch is `main` or `master` (exported as `$main`), and provides `rebase_if_remote()` and `push_changes()` helpers
- **`build/`** — scripts for building, pushing, and opening projects (`pushcode`, `run-full-build`, `openproject`, `openmain`)
- **`git/`** — git workflow scripts (`new-branch`, `update-from-main`, `chain-update-from-main`, `remove-merged-branches`, etc.)
- **`aws/`**, **`homebrew/`**, **`java/`** — domain-specific utilities

## Key Conventions

- All scripts use `#!/usr/bin/env zsh` and should be written in zsh (not bash).
- Scripts that need shared utilities source them via their own `$BUILD_SCRIPTS_DIR` (resolved with `readlink -f`), never via hardcoded paths.
- Use `fail "message"` (from `common/utils`) instead of `exit 1` when inside functions; this kills the top-level process via `kill -s TERM $TOP_PID`.
- The `$main` variable (set in `git/utils`) holds the name of the default branch — always use it instead of hardcoding `main` or `master`.
- Scripts support optional behaviour through named flags (`--safe`, `--force`, `--stash`, `--merge`, etc.) parsed with a `case` loop; unrecognised flags call a local `usage_and_exit`.
- Many scripts support default values via environment variables (e.g. `NEW_BRANCH_PREFIX`, `DEFAULT_NEW_BRANCH_NAME`).

## `run-full-build` Detection Logic

`run-full-build` auto-detects the build tool by checking for indicator files in the current directory, in priority order:

1. `build.sh` → runs `./build.sh`
2. `moon.yml` → runs `moon ci` (or custom goals if args passed)
3. `pom.xml` → Maven (`mvn clean install`)
4. `build.sbt` → sbt (`sbt test it:test func:test`)
5. `build.gradle` → Gradle (`./gradlew clean test`)
6. `package.json` → Node (`npm install && npm run build`)
7. `Makefile` → Make (`make build`)

## Pushing Code

Use `pushcode` instead of `git push`. It: verifies no uncommitted changes, rebases from remote, updates from main, runs the full build, then pushes.

```zsh
pushcode              # standard push
pushcode --skip-build # skip build step
pushcode --force      # skip rebase, force push
pushcode --merge      # use merge instead of rebase
```
