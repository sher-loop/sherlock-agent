# sherlock-agent

A debugging-focused agent for [Kilo Code](https://kilo.ai), registered as a native built-in agent and wrapped in a standalone `sherlock` CLI command.

## What it does

`sherlock` is an AI debugging agent that uses a detective-style investigation methodology:

1. **Scope** — Clarify the problem and gather initial context
2. **Investigate** — Examine logs, reproduce the issue, collect evidence
3. **Hypothesize** — Form testable theories about the root cause
4. **Experiment** — Design minimal tests or add temporary logging to validate/refute each hypothesis
5. **Deduce** — Narrow to the single most likely root cause
6. **Act** — Propose (and get user approval before) applying the targeted fix
7. **Verify** — Confirm the fix resolves the issue without regressions

## Deployment

### Local development

```bash
# 1. Clone the repo
git clone https://github.com/sher-loop/sherlock-agent.git
cd sherlock-agent

# 2. Install Bun (if not already installed)
npm install -g bun

# 3. Install dependencies
bun install

# 4. Link the CLI globally so `kilo` and `sherlock` resolve
cd packages/opencode
bun link
bun link @kilocode/cli

# 5. Create the sherlock wrapper (one-time)
echo '@echo off && kilo --agent sherlock %*' > "$env:APPDATA\npm\sherlock.cmd"
```

### Usage

```bash
# Run interactively
sherlock "debug why my API returns 404"

# From within any project directory
sherlock "investigate slow query in production logs"
```

### Verify the agent is registered

```bash
kilo agent list
# Should show: sherlock (primary)
```

## Project structure

| Path | Purpose |
|---|---|
| `packages/opencode/src/agent/prompt/sherlock.txt` | System prompt for the sherlock agent |
| `packages/opencode/src/kilocode/agent/index.ts` | Native agent registration (`agents.sherlock`) |
| `.changeset/sherlock-agent.md` | Changeset for release notes |
| `~/.kilocode/agent/sherlock.md` | Global markdown agent definition (frontmatter-based) |
| `sherlock.cmd` | Standalone CLI wrapper (`kilo --agent sherlock %*`) |

## Upstream sync

This is a fork of [Kilo-Org/kilocode](https://github.com/Kilo-Org/kilocode). Upstream changes are merged into `main`; sherlock additions are kept isolated in `packages/opencode/src/kilocode/agent/`.

```bash
git fetch upstream
git merge upstream/main
```
