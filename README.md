# Common Coding Agent Plugins

Plugins distributed as marketplaces for Claude Code, Cursor, and Codex.

## What's Inside

This repository ships three marketplace manifests backed by the same `plugins/` directory, so a single plugin source can be installed from any supported coding agent:

- `.claude-plugin/marketplace.json` — Claude Code marketplace (`claude-plugin-template`)
- `.cursor-plugin/marketplace.json` — Cursor marketplace (`cursor-plugin-template`)
- `.codex-plugin/marketplace.json` — Codex marketplace (`codex-plugin-template`)

## Available Plugins

### `essentials`

A comprehensive sample plugin that demonstrates every supported extension point. Useful as a starting point and as a working reference.

It ships:

- **Skills**:
  - `problem-solving` — Broadly and deeply analyze user intent (XY-aware) and evaluate multiple solution approaches with 0–100 scores. See [plugins/essentials/skills/problem-solving/README.md](plugins/essentials/skills/problem-solving/README.md).
  - `deep-problem-solving` — Interactive deep-research and decision support: frame the real problem, ask 10 multiple-choice questions one at a time, then produce a rigorous comparative recommendation.
  - `advanced-greet` — Sample greeting skill.
  - `essentials` — Plugin overview skill.
- **Agents**, **commands**, **hooks**, and **rules** scaffolding.
- **MCP** and **LSP** server configuration (Claude Code only): [plugins/essentials/.mcp.json](plugins/essentials/.mcp.json), [plugins/essentials/.lsp.json](plugins/essentials/.lsp.json).

For full per-plugin docs, browse [plugins/essentials/](plugins/essentials/).

## Installation

Pick the section for your coding agent.

### Claude Code

Requires the Claude CLI (`npm install -g @anthropic-ai/claude-code`).

```bash
claude plugin marketplace add yu-iskw/common-coding-agent-plugins
claude plugin install essentials@claude-plugin-template
```

Verify:

```bash
claude plugin list
```

### Cursor

Add this repository as a Cursor plugin marketplace; Cursor consumes [.cursor-plugin/marketplace.json](.cursor-plugin/marketplace.json) at the repo root and exposes the `essentials` plugin under the `cursor-plugin-template` marketplace.

### Codex

Add this repository as a Codex plugin marketplace; Codex consumes [.codex-plugin/marketplace.json](.codex-plugin/marketplace.json) at the repo root and exposes the `essentials` plugin under the `codex-plugin-template` marketplace.

## Contributing

If you want to add a plugin, modify components, or run the integration tests, see [CONTRIBUTING.md](CONTRIBUTING.md).

## License

Apache License 2.0. See [LICENSE](LICENSE).
