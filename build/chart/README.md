# Helm Chart Build Workflow

Lint, install-test, package, and publish Helm charts. Uses
[chart-testing](https://github.com/helm/chart-testing-action) to detect
changed charts and only lint/install/package/publish charts that changed.

## Usage

```yaml
- uses: anselmes/clact/build/chart@main
  with:
    publish: "true"
```

## Inputs

| Name                | Description                         | Required | Default                       |
| ------------------- | ----------------------------------- | -------- | ----------------------------- |
| `context`           | Context directory for the build     | no       | `.`                           |
| `registry_url`      | Container registry to push to       | no       | `ghcr.io`                     |
| `registry_username` | Username for the container registry | no       | `${{ github.actor }}`         |
| `registry_password` | Password for the container registry | no       | `${{ secrets.GITHUB_TOKEN }}` |
| `publish`           | Whether to publish the artifact     | no       | `false`                       |

## Notes

- Callers that set `publish: true` must grant the job `packages: write`
  permission.
- Only charts changed relative to the repository's default branch are
  linted, installed (in a local `kind` cluster), packaged, and published.
- Published charts are pushed as OCI artifacts and, on pushes to `main`,
  have build provenance attested.
