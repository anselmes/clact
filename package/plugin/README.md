# Claude Plugin Bundle

Validate a Claude Code plugin manifest and stage its distributable content
(config, hooks, tools, skills, agents, docs — no compiled binary) into a
`.plugin` ZIP archive, plus an unzipped copy for marketplace/Pages hosting.
Requires `npm` (for the Claude Code CLI) and `jq` on the runner.

## Usage

```yaml
- uses: anselmes/clact/package/plugin@main
  with:
    name: my-plugin
```

## Inputs

| Name       | Description                                                                                | Required | Default                                                    |
| ---------- | ------------------------------------------------------------------------------------------ | -------- | ---------------------------------------------------------- |
| `name`     | Plugin name (also used as the archive/dist directory name); must match `^[A-Za-z0-9._-]+$` | yes      |                                                            |
| `context`  | Directory containing the plugin (`.claude-plugin/plugin.json` etc.)                        | no       | `.`                                                        |
| `version`  | Version to write into `plugin.json` before staging (skipped when empty)                    | no       | `""`                                                       |
| `output`   | Path to the output `.plugin` ZIP, relative to context                                      | no       | `hack/<name>.plugin`                                       |
| `dist-dir` | Directory to copy the unzipped, staged plugin content into, relative to context            | no       | `../dist`                                                  |
| `paths`    | Space-separated list of files/directories (relative to context) to include in the bundle   | no       | `.mcp.json README.md LICENSE hooks ../tools skills agents` |

## Outputs

| Name          | Description                                                      |
| ------------- | ---------------------------------------------------------------- |
| `plugin-path` | Path to the produced `.plugin` ZIP, relative to context          |
| `dist-path`   | Path to the unzipped, staged plugin content, relative to context |

## Notes

- Installs the Claude Code CLI globally (`npm install -g @anthropic-ai/claude-code`)
  and validates the plugin manifest with `claude plugin validate` before staging.
