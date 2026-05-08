# Contributing

Thanks for contributing to this repository.

## Development Prerequisites

- `git`
- `docker`
- `trunk` CLI
- Optional: `claude` CLI (`npm install -g @anthropic-ai/claude-code`) for full plugin loading and install checks

## Setup

1. Fork or branch from this repository.
2. Install Trunk:
   - `curl https://get.trunk.io -fsSL | bash`
3. Verify tools:
   - `trunk --version`
   - `docker --version`

## Repository Layout

```text
.
├── .claude-plugin/marketplace.json   # Claude Code marketplace manifest
├── .cursor-plugin/marketplace.json   # Cursor marketplace manifest
├── .codex-plugin/marketplace.json    # Codex marketplace manifest
├── plugins/                          # Container for all plugins
│   └── essentials/                   # Comprehensive sample plugin
│       ├── .claude-plugin/plugin.json
│       ├── .cursor-plugin/plugin.json
│       ├── .codex-plugin/plugin.json
│       ├── agents/                   # Custom agent definitions
│       ├── commands/                 # Slash command definitions
│       ├── skills/                   # Model-invoked skills (SKILL.md)
│       ├── hooks/                    # Event hook configurations
│       ├── rules/                    # Editor/agent rules
│       ├── assets/                   # Icons and other static assets
│       ├── .mcp.json                 # MCP server configuration
│       └── .lsp.json                 # LSP server configuration
├── integration_tests/                # Shared testing harness
│   ├── run.sh                        # Test orchestrator (scans plugins/)
│   ├── validate-manifest.sh          # Claude manifest schema validator
│   ├── validate-cursor-manifest.sh   # Cursor manifest schema validator
│   ├── validate-codex-manifest.sh    # Codex manifest schema validator
│   ├── test-component-discovery.sh
│   ├── test-plugin-loading.sh
│   ├── test-plugin-install.sh
│   ├── docker-entrypoint.sh
│   └── Dockerfile
├── .github/workflows/                # GitHub Actions (Lint, Integration Tests)
├── Makefile                          # Task runner
├── README.md                         # End-user docs
└── CONTRIBUTING.md
```

## Adding a New Plugin

Create a new directory under `plugins/` following the [Standard Plugin Layout](https://code.claude.com/docs/en/plugins-reference#standard-plugin-layout). Use [plugins/essentials/](plugins/essentials/) as the canonical reference.

Required manifests (one per coding agent you want to support):

- `plugins/<name>/.claude-plugin/plugin.json` — see [plugins/essentials/.claude-plugin/plugin.json](plugins/essentials/.claude-plugin/plugin.json).
- `plugins/<name>/.cursor-plugin/plugin.json` — see [plugins/essentials/.cursor-plugin/plugin.json](plugins/essentials/.cursor-plugin/plugin.json).
- `plugins/<name>/.codex-plugin/plugin.json` — see [plugins/essentials/.codex-plugin/plugin.json](plugins/essentials/.codex-plugin/plugin.json).

Component directories (add only what your plugin needs):

- `plugins/<name>/skills/<skill-name>/SKILL.md` — keep instructions specific, testable, and deterministic.
- `plugins/<name>/agents/` — Markdown files with front matter (`name`, `description`) and clear behavior.
- `plugins/<name>/hooks/hooks.json` — keep JSON valid and minimal.
- `plugins/<name>/commands/` — slash command definitions.
- `plugins/<name>/rules/` — agent/editor rules.
- `plugins/<name>/.mcp.json` — MCP server configuration.
- `plugins/<name>/.lsp.json` — LSP server configuration.

After adding the plugin, register it in each marketplace manifest you support:

- [.claude-plugin/marketplace.json](.claude-plugin/marketplace.json)
- [.cursor-plugin/marketplace.json](.cursor-plugin/marketplace.json)
- [.codex-plugin/marketplace.json](.codex-plugin/marketplace.json)

## Local Checks

Run these before opening a pull request:

1. `make format`
2. `make lint`
3. `make test-integration-docker` (builds the image and runs all integration tests, including the plugin **install** test: marketplace add + install + list/validate)

## Testing

The integration test runner [integration_tests/run.sh](integration_tests/run.sh) automatically discovers all directories in `plugins/` that contain a `.claude-plugin/plugin.json` file.

- Run all tests: `./integration_tests/run.sh`
- Verbose output: `./integration_tests/run.sh --verbose`
- Skip loading tests (if Claude CLI is not installed): `./integration_tests/run.sh --skip-loading`

You can also run individual scripts directly:

- [integration_tests/validate-manifest.sh](integration_tests/validate-manifest.sh) — Claude plugin manifest schema validation.
- [integration_tests/validate-cursor-manifest.sh](integration_tests/validate-cursor-manifest.sh) — Cursor plugin manifest schema validation.
- [integration_tests/validate-codex-manifest.sh](integration_tests/validate-codex-manifest.sh) — Codex plugin manifest schema validation.
- [integration_tests/test-component-discovery.sh](integration_tests/test-component-discovery.sh) — verifies declared components exist on disk.
- [integration_tests/test-plugin-loading.sh](integration_tests/test-plugin-loading.sh) — minimal plugin load smoke test (requires Claude CLI).
- [integration_tests/test-plugin-install.sh](integration_tests/test-plugin-install.sh) — adds the workspace as a marketplace, installs each plugin, and verifies with `claude plugin list` (requires Claude CLI; run from repo root).

`make test-integration-docker` runs the same suite inside a container (and additionally runs the plugin install test). The same Docker flow runs in CI.

## CI/CD

- **Trunk Check**: runs linters and static analysis on every PR.
- **Integration Tests**: automatically validates every plugin in the `plugins/` directory.
- **Plugin Install (Docker)**: the `plugin-install-docker` job runs the marketplace add + install + list flow inside the container image to mirror local `make test-integration-docker`.

## Pull Request Guidelines

1. Keep changes scoped and focused.
2. Update docs when behavior or structure changes.
3. Include test evidence in your PR description (commands run and outcomes).
4. Ensure CI passes (`trunk_check` and `integration_tests` workflows).

## Commit Guidelines

- Use clear, imperative commit messages.
- Prefer small commits that are easy to review.

## Reporting Issues

Open an issue with:

- expected behavior
- actual behavior
- reproduction steps
- logs or screenshots when relevant
