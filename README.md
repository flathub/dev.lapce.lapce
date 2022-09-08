# Lapce (flatpak)

In this repository you can find the manifests and scripts used to build [Lapce](https://github.com/lapce/lapce) for Flathub.

It makes use of `cargo/flatpak-cargo-generator.py` from [flatpak-builder-tools](https://github.com/flatpak/flatpak-builder-tools) and a modified `update.py` from the [net.veloren.veloren flatpak](https://github.com/flathub/net.veloren.veloren).

## Upstream nightly builds

The Lapce developers provide nightly builds for testing. These can be downloaded via their [releases page](https://github.com/lapce/lapce/releases).

## Building a snapshot yourself
Flathub is only meant for distributing stable releases, so nightly flatpaks won't be provided. If you want to build a flatpak from the latest upstream commit, follow these simple steps:
```sh
# create a new branch
git checkout -b bleeding-edge

# update the manifests to match the latest upstream commit
python update.py lapce-git.json

# build and install the flatpak
flatpak-builder --user --install --force-clean build dev.lapce.lapce.yaml
```
