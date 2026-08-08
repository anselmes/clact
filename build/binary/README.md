# Binary Build Workflow

Compile a Go, Rust, or Swift binary and package it as a macOS installer
(`.pkg`). Reads the version to stamp from `<context>/.claude-plugin/plugin.json`.
Signs with a Developer ID certificate and notarizes when
`developer-name`/`developer-id`/`apple-api-key-*` inputs are provided;
otherwise falls back to an ad-hoc signed, non-notarized build. Pair with
[`anselmes/clact/macos/signing/setup`](../../macos/signing/setup) (and
[`.../cleanup`](../../macos/signing/cleanup)) to provide the certificate/key
material — this action does not import or manage keychains itself. Requires
a macOS runner and `jq`.

## Usage

```yaml
- uses: anselmes/clact/build/binary@main
  with:
    target: go
    name: my-tool
    identifier: com.example.my-tool
```

## Inputs

| Name                  | Description                                                                                                                                                                                      | Required | Default                     |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------- | --------------------------- |
| `target`              | Language toolchain to build with (`go` \| `rust` \| `swift`)                                                                                                                                     | yes      |                             |
| `name`                | Binary/package name; must match `^[A-Za-z0-9._-]+$`                                                                                                                                              | yes      |                             |
| `identifier`          | Reverse-DNS package identifier for `pkgbuild` (e.g. `com.example.mytool`)                                                                                                                        | yes      |                             |
| `context`             | Directory containing the project                                                                                                                                                                 | no       | `.`                         |
| `arch`                | Architecture to build for. Valid values depend on target: go: `arm64` \| `amd64`; rust: `aarch64` \| `x86_64`; swift: `arm64` \| `x86_64` \| `universal` (unset builds the runner's native arch) | no       | `""`                        |
| `swift-version`       | Swift version to install (swift target only, e.g. `6.3.3`, `main-snapshot`); passed straight through to `swift-actions/setup-swift`'s `swift-version` input, resolved via Swiftly                | no       | `""` (Swiftly's `latest`)   |
| `go-package`          | Package path to build (go target only)                                                                                                                                                           | no       | `./pkg/cmd`                 |
| `version-symbol`      | Go symbol path to stamp with the plugin version via `-ldflags -X` (go target only, e.g. `mymodule/pkg.version`; skipped when empty)                                                              | no       | `""`                        |
| `installer-output`    | Path to the output `.pkg`, relative to context (an `-unsigned` suffix is inserted automatically when no Developer ID is configured)                                                              | no       | `hack/<name>-installer.pkg` |
| `developer-name`      | Developer ID identity name for codesign/pkgbuild/productbuild (unset builds ad-hoc signed only)                                                                                                  | no       | `""`                        |
| `developer-id`        | Developer ID team/account identifier                                                                                                                                                             | no       | `""`                        |
| `apple-api-key-id`    | App Store Connect API key ID, for notarization                                                                                                                                                   | no       | `""`                        |
| `apple-api-issuer-id` | App Store Connect API issuer ID, for notarization                                                                                                                                                | no       | `""`                        |
| `apple-api-key-path`  | Path to the decoded App Store Connect API key (`.p8`), for notarization                                                                                                                          | no       | `""`                        |

## Outputs

| Name             | Description                                                      |
| ---------------- | ---------------------------------------------------------------- |
| `installer-path` | Path to the produced `.pkg`, relative to context                 |
| `signed`         | Whether the installer was signed with a Developer ID certificate |

## Notes

- Notarization only runs when the installer is signed and all three
  `apple-api-*` inputs are set; otherwise the `.pkg` is left signed (or
  ad-hoc signed) but not notarized.
- Combine with `macos/signing/setup`/`.../cleanup` in the same job to supply
  `developer-name`, `developer-id`, and `apple-api-key-path`.
