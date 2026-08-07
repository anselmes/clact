# macOS Signing Cleanup

Delete the ephemeral keychain and notarization API key created by
[`macos/signing/setup`](../setup), restoring the runner's previous default
keychain and keychain search list first. Reads `KEYCHAIN_PATH`,
`APPLE_API_KEY_PATH`, `PREVIOUS_DEFAULT_KEYCHAIN`, and `PREVIOUS_KEYCHAINS`
from the environment (as exported by `macos/signing/setup` in the same job)
unless overridden via the inputs below, which can also be wired directly
from `macos/signing/setup`'s step outputs (`keychain-path`, `api-key-path`,
`previous-default-keychain`, `previous-keychains`) — set these explicitly if
invoking this action outside that same-job flow. Requires a macOS runner.

## Usage

```yaml
- if: always()
  uses: anselmes/clact/macos/signing/cleanup@main
```

## Inputs

| Name                        | Description                                                                                                                                           | Required | Default |
| --------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- | -------- | ------- |
| `keychain-path`             | Path to the ephemeral keychain to delete. Defaults to the `KEYCHAIN_PATH` environment variable exported by `macos/signing/setup`.                     | no       |         |
| `api-key-path`              | Path to the decoded App Store Connect API key to remove. Defaults to the `APPLE_API_KEY_PATH` environment variable exported by `macos/signing/setup`. | no       |         |
| `previous-default-keychain` | Default keychain to restore. Defaults to the `PREVIOUS_DEFAULT_KEYCHAIN` environment variable exported by `macos/signing/setup`.                      | no       |         |
| `previous-keychains`        | Newline-separated keychain search list to restore. Defaults to the `PREVIOUS_KEYCHAINS` environment variable exported by `macos/signing/setup`.       | no       |         |

## Notes

- Run with `if: always()` so cleanup happens even if a prior step in the job
  fails.
- A failure to restore the previous keychain state doesn't stop the
  remaining deletions from running; all failures are reported and the step
  exits non-zero only after every cleanup action has been attempted.
