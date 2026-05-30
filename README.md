# epic-games-launcher snap

An unofficial snap package that runs the official
[Epic Games Launcher](https://store.epicgames.com/en-US/download) on Linux
through [Wine](https://www.winehq.org/).

> **Note:** This project is not affiliated with or endorsed by Epic Games, Inc.

---

## How it works

| Step | What happens |
|------|-------------|
| Build | Snapcraft downloads the official Windows MSI installer and stages it alongside WineHQ Staging. |
| First launch | The snap creates a 64-bit Wine prefix, runs the MSI, and the installer downloads the full launcher (internet required). |
| Subsequent launches | The launcher starts directly and self-updates as normal. |

---

## Requirements

- Ubuntu 22.04+ or any `snapd`-capable distro
- Internet connection on first launch (~500 MB download)
- `snapd` ≥ 2.58.3

---

## Build

```bash
# Install Snapcraft if needed
sudo snap install snapcraft --classic

# Build the snap (requires an internet connection)
snapcraft

# Install the resulting .snap file
sudo snap install --dangerous epic-games-launcher_latest_amd64.snap
```

### Build with Multipass / LXD (recommended for clean builds)

```bash
snapcraft --use-lxd    # or --use-multipass
```

---

## Install from the Snap Store (once published)

```bash
sudo snap install epic-games-launcher
```

---

## Permissions

The snap requests the following interfaces:

| Interface | Reason |
|-----------|--------|
| `network` / `network-bind` | Launcher updates, store browsing, game downloads |
| `audio-playback` / `pulseaudio` | Game and launcher audio |
| `opengl` | GPU-accelerated rendering via Wine |
| `joystick` | Controller support |
| `home` | Access game files saved in `$HOME` |
| `removable-media` | Games installed on external drives |
| `hardware-observe` | Hardware detection for system requirements |
| `process-control` | Wine process management |
| `screen-inhibit-control` | Prevent sleep during downloads |

---

## Data locations

| Data | Path |
|------|------|
| Wine prefix (Windows install) | `$HOME/snap/epic-games-launcher/common/wine/` |
| Snap user data | `$HOME/snap/epic-games-launcher/` |

To reset the installation (removes all Wine data and installed EGL):

```bash
rm -rf "$HOME/snap/epic-games-launcher/common/wine"
```

---

## Troubleshooting

**Launcher fails to start after installation**
- The MSI bootstrapper may still be downloading components in the background.
  Wait a minute and re-launch.

**Audio issues**
- Ensure `pulseaudio` or PipeWire with PulseAudio compatibility is running.
- Try: `snap connect epic-games-launcher:audio-playback`

**Graphics issues / black screen**
- Confirm your GPU drivers are installed (`vulkaninfo` to verify Vulkan support).
- Try: `snap connect epic-games-launcher:opengl`

**Reset Wine prefix**
```bash
rm -rf "$HOME/snap/epic-games-launcher/common/wine"
epic-games-launcher   # re-runs first-time setup
```

---

## Icon

The placeholder icon (`snap/gui/icon.svg`) should be replaced with a proper
256×256 PNG before publishing to the Snap Store. Do not use Epic's trademarked
assets without permission.
