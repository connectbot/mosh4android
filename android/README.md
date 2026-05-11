# Android Release Assets

The `android-release-assets` GitHub Actions workflow builds ABI-specific
archives with the Android NDK:

- `mosh-android-arm64-v8a.zip`
- `mosh-android-armeabi-v7a.zip`
- `mosh-android-x86.zip`
- `mosh-android-x86_64.zip`

Each archive contains:

- `mosh-client`, a native executable mosh client binary
- `terminfo.zip`, the terminfo database needed by the client

These assets are GPL-licensed as part of mosh. Apps that consume them should
present that license fact before downloading or enabling the binary.
