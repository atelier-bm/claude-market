# Olive Claude Code plugin

This directory is the canonical source for the Olive Claude Code plugin package.

The plugin is named `olive`. This scaffold reserves the package layout for follow-up work:

- `.claude-plugin/plugin.json` — plugin manifest and version metadata
- `mcp/` — remote Olive MCP server configuration
- `skills/olive/SKILL.md` — Olive task-management skill

Marketplace deployment publishes this source package to `plugins/olive` in `https://git.bit-monkey.io/public/claude-market.git` when the Olive source deployment workflow runs.

## Validation

Run plugin validation from the repository root before changing this package:

```bash
scripts/validate-claude-plugin.sh claude-plugin
# or
make validate-claude-plugin
```

The validation command checks only the `claude-plugin` package source path. When the Claude Code CLI is installed, it runs `claude plugin validate claude-plugin` first. CI also uses the local fallback validator in `scripts/validate-claude-plugin.go` so invalid JSON, missing `.claude-plugin/plugin.json`, missing referenced package paths, or malformed skill/MCP package structure fail with a clear message even when the official validator is unavailable.
