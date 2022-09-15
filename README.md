# Lapce (flatpak)

This repository contains the manifests and scripts used for building and packaging [Lapce](https://github.com/lapce/lapce) for Flathub.

The scripts make use of `cargo/flatpak-cargo-generator.py` from [flatpak-builder-tools](https://github.com/flatpak/flatpak-builder-tools) and a modified `update.py` from [flathub/net.veloren.veloren](https://github.com/flathub/net.veloren.veloren).

## Permissions

Running a code editor sandboxed can be tricky. Linters and other plugins often need access to source files normally not accessible by the flatpak sandbox. Therefore this flatpak sets the following [permissions](https://docs.flatpak.org/en/latest/sandbox-permissions-reference.html) by default:
* `filesystem=host`: The whole host filesystem is accessible by Lapce (to work arrouns issues like [this](https://github.com/flathub/dev.lapce.lapce/issues/3)).
* `talk-name=org.freedesktop.Flatpak`: Allows the use of [flatpak-spawn --host](https://docs.flatpak.org/en/latest/flatpak-command-reference.html#flatpak-spawn).

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

## Building a flatpak from custom Lapce repository/branch

Similar to the instructions above, you can build from a different repository or branch for testing:
1. create a new branch: `git checkout -b test-feature-x`
2. edit `lapce-git.json` to point to the desired repository/branch:
```json
{
    "type": "git",
    "url": "https://github.com/user-name/lapce",
    "branch": "branch-name"
}
```
5. `git add lapce-git.json && git commit -m "Use custom repository"`
4. update the manifests to match the latest commit: `python update.py lapce-git.json`. Run this command again, every time the remote repository changes.
5. build and install the flatpak: `flatpak-builder --user --install --force-clean build dev.lapce.lapce.yaml`
