# hayamiz-agentkit

A personal collection of skills and plugins for coding agents (primarily
[Claude Code](https://claude.com/claude-code)) by [hayamiz](https://github.com/hayamiz).

Two distribution channels:

- **Plugins** (`plugins/`) — shipped through the Claude Code plugin marketplace. Available as commands with a plugin-name prefix (e.g. `/ticket-fix`, `/gardener`).
- **Skills** (`skills/`) — shipped via [APM](https://github.com/apm-pkg/apm). Deployed directly into `.claude/skills/` and invoked by their bare name.

## Layout

```
skills/     APM-distributed skills (one directory per skill, each with SKILL.md)
  commit-all/      Commit all dirty files in the worktree, grouped semantically
  commit-session/  Commit only the files changed in the current session
plugins/    Claude Code plugin marketplace plugins (one directory per plugin)
  gardener/  Repository health audit — docs sync, best-practices checks, etc.
  ticket/    File-based ticket workflow (init / create / check / triage / fix)
.claude-plugin/
  marketplace.json   Marketplace manifest exposing plugins/ as a Claude Code plugin source
```

For details on each plugin or skill, see its `SKILL.md` / `plugin.json`.

## Installation

### Plugins (Claude Code)

Register the marketplace and install plugins with Claude Code's plugin commands.

```text
# Register the marketplace (first time only)
/plugin marketplace add hayamiz/hayamiz-agentkit

# Install plugins
/plugin install ticket@hayamiz-agentkit
/plugin install gardener@hayamiz-agentkit
```

After installation the following commands become available:
`/ticket-init`, `/ticket-create`, `/ticket-check`, `/ticket-triage`, `/ticket-fix`, `/gardener`.

### Skills (APM)

Requires the [`apm`](https://github.com/apm-pkg/apm) CLI.

```sh
apm install hayamiz/hayamiz-agentkit/skills/commit-all
apm install hayamiz/hayamiz-agentkit/skills/commit-session
```

Add `-g` to install globally into `~/.apm/`:

```sh
apm install -g hayamiz/hayamiz-agentkit/skills/commit-session
```

To reinstall all dependencies declared in this repo's own `apm.yml` (`skill-creator` + the two skills above):

```sh
apm install
```

## Development Notes

- `.claude/skills/` is gitignored — it is populated by `apm install`. Edit sources in `skills/<name>/` or `plugins/<name>/skills/<name>/` instead.
- When adding a plugin, add an entry to `.claude-plugin/marketplace.json`. When adding a skill, add it to `apm.yml` under `dependencies.apm` and run `apm install` to regenerate the lockfile.
- See [`CLAUDE.md`](CLAUDE.md), [`skills/CLAUDE.md`](skills/CLAUDE.md), and [`plugins/CLAUDE.md`](plugins/CLAUDE.md) for detailed conventions.
