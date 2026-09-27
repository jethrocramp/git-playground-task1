# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A tiny command-line notes tool used as the practice repo for Unit 4 (Git) of a Claude Code course. It's a sandbox for doing real Git work with Claude, not a project meant to grow features — see README.md for the lesson task.

## Commands

- Run the app: `node notes.js add <text>` | `node notes.js list` | `node notes.js delete <id>`
- Run the submission check (also runs in CI on every PR): `node scripts/check.js` — verifies `notes.md` exists and is non-empty; it does not judge content.

There is no build step, linter, or test suite in this repo.

## Architecture

- `notes.js` — CLI entry point; parses `process.argv` and dispatches to `add`/`list`/`delete`.
- `lib/store.js` — persistence layer; loads/saves notes as JSON to `notes.json` (gitignored, created on first write). All reads/writes go through `load()`/`save()`, which re-read the whole file each call — there's no in-memory state between commands.
- `lib/config.js` — static app settings (e.g. `SESSION_TIMEOUT_MINUTES`), read by `notes.js` for display only; not enforced anywhere yet.
- `scripts/check.js` — CI gate for the Lesson 1 exercise, invoked by `.github/workflows/check.yml` on pull requests.

## Lesson workflow

The README describes a specific exercise: make small edits (including one easy-to-miss "stray" change), predict the diff in `notes.md`, have Claude summarize the actual changes and flag anything unintended, commit on a branch named `read-repo`, push, and open a PR against the main (upstream) repo rather than the fork.
