# clact

Reusable composite GitHub Actions for building, packaging, signing, and
publishing artifacts.

---

<!-- [![OpenSSF Scorecard][ossf-score-badge]][ossf-score-link] -->

[![Contiuos Integration][ci-badge]][ci-link]
[![Review][review-badge]][review-link]

<!-- [ossf-score-badge]: https://api.securityscorecards.dev/projects/github.com/anselmes/clact/badge
[ossf-score-link]: https://securityscorecards.dev/viewer/?uri=github.com/anselmes/clact -->

[ci-badge]: https://github.com/anselmes/clact/actions/workflows/ci.yml/badge.svg
[ci-link]: https://github.com/anselmes/clact/actions/workflows/ci.yml
[review-badge]: https://github.com/anselmes/clact/actions/workflows/required/anselmes/cicd/.github/workflows/review.yml/badge.svg
[review-link]: https://github.com/anselmes/clact/actions/workflows/required/anselmes/cicd/.github/workflows/review.yml

---

## Actions

| Action                                           | Description                                                                      |
| ------------------------------------------------ | -------------------------------------------------------------------------------- |
| [`build/binary`](build/binary)                   | Compile a Go, Rust, or Swift binary and package it as a macOS installer (`.pkg`) |
| [`build/chart`](build/chart)                     | Lint, install-test, package, and publish Helm charts                             |
| [`build/container`](build/container)             | Build and push multi-platform Docker images, signed with cosign                  |
| [`package/plugin`](package/plugin)               | Validate and bundle a Claude Code plugin into a `.plugin` ZIP                    |
| [`package/python`](package/python)               | Build a standalone Python wheel with `uv`                                        |
| [`macos/signing/setup`](macos/signing/setup)     | Import a Developer ID certificate into an ephemeral keychain for notarization    |
| [`macos/signing/cleanup`](macos/signing/cleanup) | Delete the ephemeral keychain/API key created by `macos/signing/setup`           |

Each action's readme documents its inputs, outputs, and usage.

## Usage

Reference an action from this repository in a workflow step:

```yaml
- uses: anselmes/clact/package/python@main
  with:
    name: my-package
```

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

This project is licensed under the GNU General Public License v3.0 - see
[LICENSE](LICENSE) for details.
