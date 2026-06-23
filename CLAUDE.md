# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

`agent-deps` has no skill content of its own. It exists solely so that other
projects can pull in a curated bundle of public APM (Agent Package Manager)
skills via a single `git:` dependency reference, instead of listing each
skill individually.

The functional content of the repo is two files:
- `apm.yml` — declares the dependency set
- `README.md` — one-paragraph explanation of the above

`apm.lock.yaml` (generated, see below) and `apm_modules/` (gitignored) are derived from `apm.yml`, not source.

There is no source code, no build, no lint, and no test suite.

## Commands

- `apm install` — resolves `apm.yml`, writes/updates `apm.lock.yaml` (pinned commits + content hashes per dependency), and populates `apm_modules/`.
- `apm lock` — resolve dependencies and (re)write `apm.lock.yaml` without installing.
- `apm outdated` — check locked dependencies against upstream for newer versions.

## Working with apm.yml

Dependencies are listed under `dependencies.apm`, either as:
- a short form `owner/repo/path/to/skill` (resolved against the default APM registry layout), or
- a long form object with `git:`, `path:`, and `alias:` keys, for skills that live in a git repo under a non-standard path and need a friendlier local name.

`includes: auto` and `scripts: {}` are present but currently unused (no scripts defined).

When asked to add, remove, or update a dependency, edit `apm.yml` directly, then run `apm install` (or `apm lock`) to refresh `apm.lock.yaml`. Don't hand-edit the lockfile.

`apm_modules/` is the resolved/installed output of `apm.yml` (one directory per `owner`, containing the fetched skill repos). It is gitignored local working state, not source — don't hand-edit it; regenerate it via `apm install` instead.
