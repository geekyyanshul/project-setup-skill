---
name: project-setup
description: Use when starting work in a new or freshly imported project folder, or when asked to set up, onboard, initialize, or establish project context and agent guardrails for a repository.
---

# ProjectSetup

## Overview
Make a project runnable from its documented source of truth (README/manifests), then establish explicit agent guardrails (`AGENTS.md`) and living project context (`context.md`). Read first, confirm sensitive actions, never fabricate commands, and never weaken existing rules.

**Core principle:** README/manifest-driven setup + safe, merge-friendly project memory for both user and agent.

## When to Use
- New local project folder or freshly cloned/imported GitHub repository.
- User asks to “set up”, “bootstrap”, “onboard”, “initialize”, “get this running”, or “create project context/agent rules”.
- Existing repo lacks clear `AGENTS.md`/`context.md` or they need a safe refresh.

## When NOT to Use
- Routine feature/bugfix work after the project is already bootstrapped (unless user explicitly asks to re-bootstrap or refresh context/rules).
- Cases where the user only wants a code change and not setup/context/rule work.

## Defaults (confirm/adjust if needed)
- **Artifact names:** `AGENTS.md` and `context.md` at repo root (or user-specified paths).
- **Artifact handling:** `AGENTS.md` and `context.md` are **local project files by default and must NOT be pushed/committed/shared** unless the user explicitly asks/approves that specific action.
- **Context updates:** update `context.md` only when the user explicitly asks, or after the user **confirms** a push/PR succeeded and wants the context updated. The agent never pushes/creates/merges PRs.
- **Canonical “other file” for completed/accepted tasks:** ask the user; if none specified, suggest `docs/tasks.md` or `CHANGELOG.md` and record the choice in `context.md`.

## Guardrails (non-negotiable)
- **Never do without explicit user permission for that specific action:** `git push` (any remote), create/update/merge pull requests, `git commit`, delete/remove/destroy files or data (`rm`, `rmdir`, `unlink`, `shred`, `git clean`, `git reset --hard`, `git branch -D`, etc.), destructive DB/docker/k8s/system commands.
- **Never push `AGENTS.md` or `context.md` by default.** Only stage/commit/push/share them if the user explicitly asks/approves that specific action; before doing so, remind them these files may contain project/process context and confirm scope.
- **Secrets never leave this device:** never read into logs/context, store, stage, commit, push, paste, upload, or share values from `.env*`, `*.pem`, `*.key`, or files matching `*secret*` / `*credentials*`. Record env *names* only if needed, never values.
- **Ask before:** commands that fetch/execute remotely (`curl`, `wget`, install scripts, `| sh/bash/zsh`), global installs, privilege/system changes, or overwriting ambiguous existing content.
- **If a request conflicts with guardrails:** refuse that action, explain why, and offer a safe alternative (e.g., draft a `context.md` entry; provide commands for the user to run).

## Phase 0 — Inspect (read-only first)
1. Read available sources: `README*`, `CONTRIBUTING*`, manifests/lockfiles (`package.json`, `pnpm-lock.yaml`, `yarn.lock`, `pyproject.toml`, `requirements.txt`, `Pipfile`, `go.mod`, `Cargo.toml`, `pom.xml`, `build.gradle*`, `Makefile`, `Taskfile*`), `docker-compose*`, CI configs, existing `AGENTS.md`/`context.md`.
2. Detect stack, runtime/prerequisites, and canonical commands. **Do not assume Node/npm.**
3. Report: stack, prerequisites, install/build/dev/test commands found, how they were determined, ambiguities, and `[TBD]` items.
4. If README/manifests are missing or contradictory, **ask before inventing commands**.

## Phase 1 — Setup (README-driven, verified)
1. Present a setup plan: exact commands, purpose, and any sensitive steps.
2. Run only documented/safe steps appropriate to the request. Confirm before sensitive actions.
3. Verify outcomes (install/build/test) and capture brief, non-secret results.
4. **Long-running dev servers:** document the verified `dev` command; run it only if explicitly requested or clearly appropriate, and explain how it will run.
5. Record verified commands and prerequisites in `context.md`.

## Phase 2 — Create or Refresh `AGENTS.md`
1. If `AGENTS.md` exists, **merge carefully**: preserve user/project-specific rules, append missing guardrails, and **never weaken** existing rules. Ask on conflicts.
2. If missing, create it using the contract below.
3. Include project-specific non-doings discovered during inspection (e.g., protected directories, generated files, migration rules) only when clearly justified; mark uncertain items `[TBD]` and ask.

### `AGENTS.md` Contract (template)
```md
# Agent Rules (Project)

## Scope
Project-specific guardrails for AI agents working in this repository. These rules complement global/user rules and must not weaken them.

## Never do (without explicit user permission for that specific action)
- Push to any remote, or create/update/merge pull requests.
- Commit to git.
- Delete/remove/destroy files or data (`rm`, `rmdir`, `unlink`, `shred`, `git clean`, `git reset --hard`, `git branch -D`, or equivalents).
- Run destructive database, Docker, Kubernetes, or system commands.
- Read values into output/logs, store, stage, commit, push, paste, upload, or share secrets (`.env*`, `*.pem`, `*.key`, `*secret*`, `*credentials*`).
- Push/commit/share `AGENTS.md` or `context.md` unless the user explicitly asks/approves that specific action.

## Ask before
- Running commands that fetch/execute remotely (`curl`, `wget`, install scripts, piped shell execution) or change global/system state.
- Overwriting ambiguous existing content in `AGENTS.md` or `context.md`.
- Running long-lived dev servers or background processes.

## Always do
- Read `README*` and manifests before running setup.
- Use documented/verified commands; if unclear, ask instead of fabricating.
- Prefer read-only exploration first.
- Preserve existing user-authored rules and context history; never weaken guardrails.
- Record verified setup commands and meaningful `context.md` updates with date, reason, and reference.

## Project-specific non-doings
- [TBD / add only project-specific items]
```

## Phase 3 — Create or Refresh `context.md`
1. If `context.md` exists, **append/update structured sections**; keep history readable. Do not overwrite history without reason.
2. If missing, create it using the contract below.
3. Seed with facts learned in Phase 0/1: purpose (from README if clear), current focus, verified setup/run/test, open questions.

### `context.md` Contract (template)
```md
# Project Context

_Last updated: YYYY-MM-DD — Reason: <why> — Ref: <task/issue/PR/file/commit or n/a>_

## Purpose
What this project is and what problem it solves.

## What we are supposed to do
The intended outcome/support this work should provide.

## Current focus
What we are doing now.

## Recent updates (append-style log)
- YYYY-MM-DD — <change/outcome>
  - Reason: <why this update was made>
  - Ref: <task/issue/PR/file/commit/user-confirmed event>
  - Task: <name/status, if applicable> — referenced in: `<canonical other file/path or link>`

## Verified setup & run
- Prerequisites: <runtime/tooling versions or notes; no secrets>
- Install: `<verified command>` (source: README/manifest, verified: YYYY-MM-DD)
- Build: `<verified command or n/a>`
- Dev: `<verified command>` (run only when explicitly requested/appropriate)
- Test: `<verified command or n/a>`
- Env/config notes: <env *names* only, never values>

## Decisions / constraints
- <key decisions, boundaries, risks>

## Open questions
- [TBD] <ambiguity needing user input>
```

## Phase 4 — Ongoing `context.md` Updates
Update `context.md` only when:
1. The user explicitly asks to update it; or
2. The user confirms a push/PR succeeded **and** asks/confirms they want `context.md` updated.

For trigger (2), use this safe prompt pattern:
> “Push/PR confirmed successful. Do you want `context.md` updated? If yes, what’s the reason and reference (task/issue/PR/file/commit)? Which canonical task file should completed work be referenced in?”

**Every update must include:** date, reason, and reference (or `n/a` only when no reference exists and user agrees). If a task was completed/accepted, include task name/status and `referenced in: <other file/path>`.

**Never:** auto-push, auto-PR, auto-commit, or auto-update context as a side effect of unconfirmed events.

## Verification Checklist (before finishing)
- [ ] Setup commands were discovered from README/manifests (or marked `[TBD]`/asked).
- [ ] No commands were fabricated; sensitive actions were confirmed.
- [ ] `AGENTS.md` created/merged without weakening existing rules.
- [ ] `AGENTS.md` includes “do not push/commit/share `AGENTS.md` or `context.md` unless explicitly asked.”
- [ ] `context.md` created/merged with structured sections and no secrets.
- [ ] Any `context.md` update has date, reason, and reference.
- [ ] No push/PR/commit/delete/secret-sharing action was taken without explicit permission.
- [ ] Outcome summary includes verified commands, file changes, open questions, and safe next steps.

## Quick Reference
| Trigger | Action | Output |
|---|---|---|
| New/imported repo or “set up/onboard/bootstrap” | Inspect README/manifests (read-only) | Stack/prereqs/commands report |
| Setup requested | Plan → confirm sensitive steps → verify | Verified install/build/dev/test notes |
| Missing guardrails | Create/merge `AGENTS.md` | Non-weakening project agent rules |
| Missing living context | Create/merge `context.md` | Structured purpose/focus/updates/setup |
| User confirms push/PR succeeded | Ask whether to update context; collect reason+ref | Append-style `context.md` update (if approved) |
| Request conflicts with guardrails | Refuse action + explain + safe alternative | No harmful side effects |

## Common Mistakes
- Guessing install/dev/test commands not found in docs/manifests.
- Treating “after push/PR” as permission to push/PR or auto-edit context without confirmation.
- Pushing/committing/sharing `AGENTS.md` or `context.md` by default.
- Overwriting `context.md` history or weakening existing `AGENTS.md` rules.
- Storing secret values or env file contents in `context.md`, commits, or chat.
- Running long-lived `dev` servers without need/confirmation.
- Writing a workflow summary into `description` (description must be trigger-only).
