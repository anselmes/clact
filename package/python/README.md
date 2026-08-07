# Python Package Build Workflow

Build a standalone Python wheel with `uv`, so `pipx install`/`uv tool install`
puts an entry point on `PATH` without requiring the caller to activate a venv.

## Usage

```yaml
- uses: anselmes/clact/package/python@main
  with:
    name: my-package
```

## Inputs

| Name        | Description                                                                                              | Required | Default |
| ----------- | -------------------------------------------------------------------------------------------------------- | -------- | ------- |
| `name`      | Package name (used to clear stale wheels before building; PEP 503 hyphens are normalized to underscores) | yes      |         |
| `context`   | Directory containing the project                                                                         | no       | `.`     |
| `wheel-dir` | Directory to write the wheel into, relative to context                                                   | no       | `hack`  |

## Outputs

| Name        | Description                                               |
| ----------- | --------------------------------------------------------- |
| `wheel-dir` | Directory the wheel was written into, relative to context |
