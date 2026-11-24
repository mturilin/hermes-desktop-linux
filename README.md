# hermes-desktop-arm64

Automated Linux `arm64` (`aarch64`) builds of **Hermes Light** (the remote-only desktop client for [NousResearch/hermes-agent](https://github.com/NousResearch/hermes-agent)), built on GitHub Actions (`ubuntu-24.04-arm`) and packaged for [`mise`](https://mise.jdx.dev/).

## Install with `mise`

```bash
mise use -g github:mturilin/hermes-desktop-arm64@latest
```

Then run:

```bash
hermes-desktop
```

Whenever `mise upgrade` runs, it automatically pulls new releases published here.

## How it works

- A daily GitHub Action checks `NousResearch/hermes-agent` for new stable release tags.
- When a new tag is found (or triggered manually via **Actions → Build Hermes Desktop ARM64**), it builds `apps/desktop` with `HERMES_DESKTOP_VARIANT=light` on `ubuntu-24.04-arm`.
- It packages the unpacked Electron app, a Wayland-aware `bin/hermes-desktop` wrapper, and the app icon into `hermes-desktop-linux-arm64.tar.gz` and publishes it as a GitHub Release under the matching upstream tag.
