# project-setup-skill

**ProjectSetup** — an OpenCode agent skill that bootstraps any new or freshly imported project folder (or GitHub clone):

1. **Setup the project** from its documented source of truth (`README*` + manifests) — discovers and verifies install / build / dev / test commands. Stack-agnostic: never assumes `npm`.
2. **Create/maintain `AGENTS.md`** — project agent guardrails (non-doings). Merges with existing rules and never weakens them.
3. **Create/maintain `context.md`** — living context for both user and agent: purpose, what we're supposed to do, current focus, recent updates, and verified setup commands.

## Safety rules

- `AGENTS.md` and `context.md` are **local by default** — never pushed, committed, or shared unless the user explicitly asks and approves that specific action.
- The agent **never** pushes, creates/merges PRs, commits, deletes, or touches secrets without explicit permission for that specific action.
- `context.md` is updated only when the user explicitly asks, or after the user **confirms** a push/PR succeeded — and every update records a **date, reason, and reference** (completed tasks also reference a canonical task file).
- Setup commands come from README/manifests only; missing or contradictory docs mean **ask**, never fabricate.

## Install

The skill works with any agent that reads the [Agent Skills](https://agentskills.io) format. Canonical install goes to `~/.agents/skills/`, which **Codex**, **Copilot CLI**, and **Gemini CLI** read natively; other runtimes get a symlink (single source of truth — update once, applies everywhere).

### Global — all runtimes (recommended)

```bash
# 1. Canonical store
mkdir -p ~/.agents/skills/project-setup
cp skills/project-setup/SKILL.md ~/.agents/skills/project-setup/SKILL.md

# 2. Symlink for runtimes that use their own skills dir (skip any you don't use)
ln -s ../../.agents/skills/project-setup ~/.claude/skills/project-setup   # Claude Code
ln -s ../../.agents/skills/project-setup ~/.codex/skills/project-setup    # Codex
ln -s ../../.agents/skills/project-setup ~/.gemini/skills/project-setup   # Gemini CLI
mkdir -p ~/.config/opencode/skills/project-setup
ln -s ../../../../.agents/skills/project-setup/SKILL.md ~/.config/opencode/skills/project-setup/SKILL.md  # OpenCode
```

If a runtime's skills directory doesn't exist yet, create it first (`mkdir -p`), or just copy the file instead of symlinking.

### Project-local (one repo only)

```bash
mkdir -p .opencode/skills/project-setup
cp skills/project-setup/SKILL.md .opencode/skills/project-setup/SKILL.md
```

Then start a new session and say things like *"set up this project"*, *"onboard this repo"*, or *"create project context and agent rules"*.

## What's inside

```text
skills/
  project-setup/
    SKILL.md    # the skill: phases 0–4, AGENTS.md/context.md contracts, guardrails, checklist
LICENSE         # MIT
README.md       # this file
```

## License

MIT — see [LICENSE](LICENSE).
