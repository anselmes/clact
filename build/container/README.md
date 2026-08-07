# Container Build Workflow

Build and push Docker images. Supports multi-platform builds, generates an
SBOM (Software Bill of Materials), and keyless-signs published images with
cosign.

## Usage

```yaml
- uses: anselmes/clact/build/container@main
  with:
    tag: my-app
    publish: "true"
```

## Inputs

| Name                | Description                                | Required | Default                                 |
| ------------------- | ------------------------------------------ | -------- | --------------------------------------- |
| `tag`               | The tag name (e.g. `v1.2.3`, `docker-foo`) | yes      |                                         |
| `build-args`        | Build arguments for the Dockerfile         | no       | `""`                                    |
| `dockerfile`        | Path to the Dockerfile                     | no       | `Dockerfile`                            |
| `context`           | Context directory for the build            | no       | `.`                                     |
| `platforms`         | Platforms to build for                     | no       | `linux/amd64,linux/arm64,linux/riscv64` |
| `registry_url`      | Container registry to push to              | no       | `ghcr.io`                               |
| `registry_username` | Username for the container registry        | no       | `${{ github.actor }}`                   |
| `registry_password` | Password for the container registry        | no       | `${{ secrets.GITHUB_TOKEN }}`           |
| `publish`           | Whether to publish the artifact            | no       | `false`                                 |

## Notes

- Callers that set `publish: true` must grant the job `packages: write`
  (registry push) and `id-token: write` (keyless cosign signing) permissions.
- Images are tagged with a branch/tag ref prefix and the commit SHA, and
  built with GitHub Actions cache (`type=gha`).
- On pushes to `main`, published images have build provenance attested.
