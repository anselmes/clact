# macOS Signing Setup

Import the Developer ID certificate into an ephemeral keychain and decode
the App Store Connect API key for notarization. Exports `KEYCHAIN_PATH`,
`APPLE_API_KEY_PATH`, `PREVIOUS_DEFAULT_KEYCHAIN`, and `PREVIOUS_KEYCHAINS`
via `GITHUB_ENV`, and the same four values as step outputs (`keychain-path`,
`api-key-path`, `previous-default-keychain`, `previous-keychains`); pair with
[`macos/signing/cleanup`](../cleanup), which accepts these as inputs or
reads them from the environment. Requires a macOS runner.

## Usage

```yaml
- uses: anselmes/clact/macos/signing/setup@main
  with:
    cert-p12-base64: ${{ secrets.APPLE_CERT_P12_BASE64 }}
    cert-password: ${{ secrets.APPLE_CERT_PASSWORD }}
    api-key-p8-base64: ${{ secrets.APPLE_API_KEY_P8_BASE64 }}
```

## Inputs

| Name                       | Description                                                                                                                                                             | Required | Default |
| -------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- | ------- |
| `cert-p12-base64`          | Base64-encoded Developer ID certificate (`.p12`)                                                                                                                        | yes      |         |
| `cert-password`            | Password for the `.p12` certificate                                                                                                                                     | yes      |         |
| `api-key-p8-base64`        | Base64-encoded App Store Connect API key (`.p8`)                                                                                                                        | yes      |         |
| `keychain-timeout-seconds` | Lock timeout for the ephemeral keychain, in seconds. Defaults to 21600 (6 hours) to comfortably cover typical CI job runtimes; raise this for longer-running workflows. | no       | `21600` |

## Outputs

| Name                        | Description                                                              |
| --------------------------- | ------------------------------------------------------------------------ |
| `keychain-path`             | Path to the ephemeral keychain created for this job.                     |
| `api-key-path`              | Path to the decoded App Store Connect API key (`.p8`).                   |
| `previous-default-keychain` | Default keychain that was active before setup ran.                       |
| `previous-keychains`        | Newline-separated keychain search list that was active before setup ran. |

## Notes

- On failure, the action rolls back any partial changes (restores the
  previous keychain list/default and removes the certificate/API key files
  it created).
- Always pair with `macos/signing/cleanup` (ideally with `if: always()`) in
  the same job to remove the ephemeral keychain and API key afterward.
