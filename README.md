# PPSSPP Fresh (snap)

[![Get it from the Snap Store](https://snapcraft.io/static/images/badges/en/snap-store-black.svg)](https://snapcraft.io/ppsspp-fresh)

Unofficial snap of [PPSSPP](https://www.ppsspp.org), the open-source PlayStation
Portable emulator. It is built from source at the release tag of
[hrydgard/ppsspp](https://github.com/hrydgard/ppsspp) pinned in
`snapcraft.yaml` (SDL frontend, OpenGL and Vulkan, Wayland support enabled),
for amd64. Not affiliated with or endorsed by the PPSSPP project.

    sudo snap install ppsspp-fresh
    sudo snap connect ppsspp-fresh:joystick          # gamepads
    sudo snap connect ppsspp-fresh:removable-media   # games on /media, /mnt
    sudo snap connect ppsspp-fresh:bluez             # optional, Bluetooth controllers
    sudo snap connect ppsspp-fresh:mount-observe     # optional

## Automatic updates

`.github/workflows/build-and-publish.yml` runs every day at 09:00 UTC, on
manual dispatch from the Actions tab, and on pushes to `main` that change
`snapcraft.yaml` or the workflow itself:

1. `check-upstream` reads the newest `vX.Y.Z` tag of `hrydgard/ppsspp`
   (`git ls-remote`) and compares it with the `version:` pinned in
   `snapcraft.yaml`. When upstream is ahead it posts a notice in the run
   summary; it does not bump the version by itself.
2. `build-and-publish` builds the pinned tag for amd64 and uploads it to
   `edge`.
3. `promote-to-stable` promotes `edge` to `stable`.

A new PPSSPP release reaches the snap once `version:` in `snapcraft.yaml` is
updated and pushed to `main`.

Required repository secret: `SNAPCRAFT_STORE_CREDENTIALS`
(`snapcraft export-login --snaps=ppsspp-fresh --channels=edge,stable creds.txt`).

## Notes

- Settings and save data live in the snap's data directory
  (`~/snap/ppsspp-fresh/current/.config/ppsspp`), not in `~/.config/ppsspp`,
  so they are separate from a non-snap install.
- Games are reachable in `$HOME` (non-hidden paths) and, after connecting
  `removable-media`, in `/media` and `/mnt`.
- Ad hoc multiplayer and the remote disc streaming server use the `network`
  and `network-bind` interfaces, which connect automatically.
- No firmware or games are included. Dump your own UMDs or use homebrew.
- Bugs specific to this packaging belong in this repository's issue tracker,
  not upstream.

## License

Packaging files in this repository: GPL-3.0 (see `LICENSE`).
PPSSPP itself: GPL-2.0-or-later, Copyright Henrik Rydgård and contributors.
