# Android Builds and Release Assets

The `Android NDK build` GitHub Actions workflow checks every pull request and
branch push by building all supported ABIs with the Android NDK. It does not
publish a release. The `Android release assets` workflow builds and uploads
ABI-specific archives for `android-*` tags:

- `mosh-android-arm64-v8a.zip`
- `mosh-android-armeabi-v7a.zip`
- `mosh-android-x86.zip`
- `mosh-android-x86_64.zip`

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
