# Android Builds and Release Assets

The `Android NDK build` GitHub Actions workflow checks every pull request and
branch push by building all supported ABIs with the Android NDK. It does not
publish a release. The `Android release assets` workflow builds and uploads
ABI-specific archives for `android-*` tags:

- `mosh-android-arm64-v8a.zip`
- `mosh-android-armeabi-v7a.zip`
- `mosh-android-x86.zip`
- `mosh-android-x86_64.zip`

The release also includes `SHA256SUMS` and `mosh-android-attestation.json`.
The checksum manifest lists each archive and its two embedded files. The
GitHub Actions build provenance attestation covers all of those files and the
manifest itself. After downloading the release assets into one directory,
extract the embedded files under their ABI-specific names, verify the
manifest's provenance, and check every hash:

```sh
mkdir -p contents
for abi in arm64-v8a armeabi-v7a x86 x86_64; do
  unzip -p "mosh-android-$abi.zip" mosh-client > "contents/mosh-client-$abi"
  unzip -p "mosh-android-$abi.zip" terminfo.zip > "contents/terminfo-$abi.zip"
done
gh attestation verify SHA256SUMS -R connectbot/mosh4android \
  --signer-workflow connectbot/mosh4android/.github/workflows/android-release.yml \
  --bundle mosh-android-attestation.json
sha256sum --check SHA256SUMS
```

You can verify any archive or extracted file directly with the same `gh
attestation verify` command and its filename in place of `SHA256SUMS`.

Each archive contains:

- `mosh-client`, a native executable mosh client binary
- `terminfo.zip`, the terminfo database needed by the client

These assets are GPL-licensed as part of mosh. Apps that consume them should
present that license fact before downloading or enabling the binary.

Pushing an `android-*` tag builds the archives and attaches them to a GitHub
Release for that tag. To build and upload assets for an existing tag, run the
workflow manually from the repository's default branch and enter the tag in
the `release_tag` field. GitHub requires the workflow file on the default
branch before manual runs are available.
