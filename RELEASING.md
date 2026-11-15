# Releasing libmpv-android

This repository publishes `io.github.cd16d:libmpv` to Maven Central and creates GitHub releases with compiled AAR artifacts.

## Prerequisites

The following repository secrets must be configured in GitHub Settings -> Secrets and variables -> Actions:

- `OSSRH_USERNAME`: Sonatype Central Portal user token username
- `OSSRH_PASSWORD`: Sonatype Central Portal user token password
- `SIGNING_KEY_ID`: Last 8/16 hex digits of the GPG signing key (e.g. `A99EA6F30D7B20BA`)
- `SIGNING_KEY`: Armored ASCII GPG private key
- `SIGNING_PASSWORD`: Passphrase for the GPG private key

The public GPG key must be published to public keyservers (`keys.openpgp.org`, `keyserver.ubuntu.com`).

## Release Process

1. Ensure all changes are committed and pushed to `main`.
2. Create and push a git tag matching the version pattern:
   ```bash
   git tag -a v1.1.0-filters -m "libmpv 1.1.0-filters"
   git push origin v1.1.0-filters
   ```
3. The GitHub Actions workflow `.github/workflows/publish.yaml` will automatically:
   - Build all native ABIs (`armeabi-v7a`, `arm64-v8a`, `x86`, `x86_64`)
   - Validate the AAR integrity and 16 KiB page alignment (`check-aar.sh`)
   - Sign and publish the AAR to Maven Central (`io.github.cd16d:libmpv:<version>`)
   - Create a GitHub Release with the compiled `libmpv-release.aar` and full corresponding GPL source tarball.
