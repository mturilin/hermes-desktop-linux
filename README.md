# hermes-desktop-linux

Automated Linux (`x64` & `arm64`) builds of **Hermes Desktop** (full client) and **Hermes Light** (remote-only client) from [NousResearch/hermes-agent](https://github.com/NousResearch/hermes-agent), built daily on GitHub Actions.

Upstream currently publishes signed macOS Apple Silicon and Windows MSIX installers, but no stable Linux desktop releases. This repo fills that gap with both **AppImage** and **`.tar.gz`** bundles.

## Artifacts per release

| Variant | Architecture | Formats | Best for |
|---|---|---|---|
| **Hermes Desktop** (Standard) | `x64`, `arm64` | `.AppImage`, `.tar.gz` | Local `hermes` CLI runtime **and** remote gateways |
| **Hermes Light** (Remote-only) | `x64`, `arm64` | `.AppImage`, `.tar.gz` | Connecting to a remote Hermes gateway (no local runtime) |

## Quick install

### Option 1: AppImage (single executable)

Download the `.AppImage` for your architecture from the [latest release](https://github.com/mturilin/hermes-desktop-linux/releases/latest):

```bash
chmod +x hermes-desktop-linux-x64.AppImage   # or arm64 / hermes-light-*
./hermes-desktop-linux-x64.AppImage
```

*(If FUSE 2 isn't installed on your distro, run with `--appimage-extract-and-run` or use the `.tar.gz` / `mise` option below.)*

### Option 2: `mise` (auto-updates via `mise upgrade`)

Add to `~/.config/mise/config.toml` (auto-detects `x64` vs `arm64`):

```toml
# Full Hermes Desktop (local + remote):
"github:mturilin/hermes-desktop-linux" = { version = "latest", matching = "desktop", minimum_release_age = "0s" }

# Or Hermes Light (remote-only):
"github:mturilin/hermes-desktop-linux" = { version = "latest", matching = "light", minimum_release_age = "0s" }
```

Then install and launch:

```bash
mise install
hermes-desktop   # (Hermes Light also exposes `hermes-light`)
```

### Option 3: `.tar.gz` (no FUSE required)

```bash
tar -xzf hermes-desktop-linux-x64.tar.gz   # or arm64 / hermes-light-*
./hermes-desktop/bin/hermes-desktop
```

The included `bin/hermes-desktop` launcher automatically enables native Wayland flags (`--ozone-platform-hint=auto`) when running under a Wayland session.

## How it works

- A daily GitHub Action checks `NousResearch/hermes-agent` for new stable release tags.
- When a new tag is found (or triggered manually via **Actions → Build Hermes Desktop Linux**), a 4-way matrix builds `standard` and `light` on `ubuntu-24.04` (`x64`) and `ubuntu-24.04-arm` (`arm64`).
- Each build verifies the architecture ELF header and `node-pty` native addon before publishing all 8 artifacts + `SHA256SUMS` to a GitHub Release under the matching upstream tag.
