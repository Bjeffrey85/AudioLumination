# Audio Lumination

**See where game sounds come from.** Audio Lumination turns your game's surround sound into a glow around the edge of your screen, so deaf and hard-of-hearing players can tell where footsteps, gunfire and explosions are coming from.

[![Latest release](https://img.shields.io/github/v/release/Bjeffrey85/AudioLumination?label=download&color=2ea44f)](https://github.com/Bjeffrey85/AudioLumination/releases/latest)
![Windows 10/11](https://img.shields.io/badge/Windows-10%20%7C%2011-0078D6)
![C++17](https://img.shields.io/badge/C%2B%2B-17-00599C)
![Status: beta](https://img.shields.io/badge/status-beta-orange)

<!-- Add a screenshot or GIF here, e.g. ![Audio Lumination](docs/screenshot.png) -->

### ⬇️ [Download the latest version](https://github.com/Bjeffrey85/AudioLumination/releases/latest/download/AudioLumination-Windows.zip)

[All releases and release notes](https://github.com/Bjeffrey85/AudioLumination/releases/latest)

---

## Features

- **360° direction:** reads all 8 channels of 7.1 surround and lights the matching part of the screen edge.
  - Front-left → top-left corner, left → left edge, behind you → bottom middle, and so on.
- **Straight ahead and directly behind light up dead center.** Most games build these from two speakers playing together (front-left plus front-right, or the two rears), and Audio Lumination recognizes that.
- **Works with every way games send sound:** 7.1, both kinds of 5.1, quad and stereo. It detects what the game is really sending, adapts on the fly, and shows it in the status line (see [Sound formats](#sound-formats)).
- **Sensitivity by direction:** **Front / Sides / Back** sliders let you, for example, catch quieter footsteps behind you without the front getting noisy.
- **Adjustable positions:** a click-to-place setup screen plays a sound from each direction, and you click where on the screen edge you want it to glow.
- **Brighter = louder:** close or loud sounds glow brighter.
- **Colors by sound type:**
  - 🟧 low: explosions, engines
  - 🟨 mid: voices, footsteps
  - 🟦 high: gunshots, glass
- **Ignores steady noise:** music, wind and engine hum fade out after a few seconds, so short sounds stand out.
- **You still hear the game:** audio passes through to your headphones, or choose **visual only**.
- **Streamer friendly:** send audio to a second device, and optionally hide the glow from OBS and screen capture.
- **Speaker check and live channel meters:** confirm your whole setup in seconds.
- **Hotkey toggle** (default `Ctrl+Alt+L`), tray icon, starts with Windows.
- **Fully automatic install:** sets up the virtual audio driver for you, plus desktop and Start menu icons.
- **Automatic updates:** checks this repo's latest release at launch and every 12 hours. It then downloads the update, overwrites the old files in the folder it's installed in, and restarts. No questions asked.
- **Clean uninstall:** puts your sound back on your headphones and removes everything it added.
- **Doesn't touch the game:** no injection and no memory reading, just Windows audio and a click-through window.

## How it works

```
Game ──► "Audio Lumination" (virtual 7.1 device, VB-CABLE)
              │
              ├──► direction analysis ──► edge glow overlay
              │
              └──► your headphones / speakers
```

1. Setup installs [VB-CABLE](https://vb-cable.com) (a free virtual audio driver), renames it **Audio Lumination**, sets it to **7.1 surround** and makes it your default output.
2. The app records the 8-channel stream (WASAPI loopback) and measures energy per speaker in three frequency bands.
3. A panning model works out the direction of each sound. Steady background noise and sound that's the same in every speaker are filtered out.
4. The result is drawn as a click-through, always-on-top glow around your chosen monitor.
5. The audio is mixed for your output device (7.1 → stereo for most headsets) and played as normal.

## Install

1. [Download **`AudioLumination-Windows.zip`**](https://github.com/Bjeffrey85/AudioLumination/releases/latest/download/AudioLumination-Windows.zip) from the [latest release](https://github.com/Bjeffrey85/AudioLumination/releases/latest), unzip it and run **`AudioLumination.exe`**.
2. Click **OK**, then **Yes** when Windows asks for permission.
3. If Windows asks *"Would you like to install this device software?"*, click **Install**.
4. If asked, restart. Setup finishes automatically afterwards.

> **⚠️ The app isn't code-signed yet.**
> - **SmartScreen** may say "Windows protected your PC". Click **More info → Run anyway**.
> - **Smart App Control** blocks unsigned apps completely. Turn it off under *Windows Security → App & browser control → Smart App Control* to use Audio Lumination.

**Updates:** once installed, Audio Lumination keeps itself up to date. A few seconds after it starts, and every 12 hours, it checks the latest release here. If there's a newer one, it downloads it, writes the new files over the old ones in its own folder, and restarts. The bottom of Settings shows the result, such as *up to date (checked 8:12 PM)*, and the tray menu has **Check for updates**. Turn it off with *Update automatically* in Settings.

> Installs from 1.5.0 onward live in `%LOCALAPPDATA%\Programs\Audio Lumination`, so updates need no Windows permission prompt. Copies installed by older versions in `C:\Program Files` still update in place, but Windows asks for permission each time. Uninstalling and reinstalling moves them to the prompt-free location.

**Offline install:** put `VBCABLE_Driver_Pack45.zip` from vb-cable.com next to the exe, and setup uses it instead of downloading.

## In-game setup

| Setting | Use |
|---|---|
| Display mode | **Borderless** or **Windowed fullscreen**. Exclusive fullscreen hides overlays. |
| Game audio | **7.1 / Surround / Home theater**, not "Headphones" or HRTF mode, which mixes everything to stereo. |
| Windows Sonic / Dolby Atmos / DTS | **Off** for the Audio Lumination device |

### Sound formats

| What the game sends | What you see |
|---|---|
| **7.1 surround** | Full 360°: front, front corners, sides, rear corners, behind |
| **5.1 surround** (rears on the side channels, the usual Windows 5.1) | Full 360°. Detected automatically after a few seconds and placed correctly behind you. |
| **5.1 / quad** (rears on the back channels) | Full 360°. Detected automatically. |
| **Stereo**, or a game's *Headphones / HRTF / 3D audio* mode | **Left ↔ right only**, spread across the full width. Two channels can't reliably say front from back, so pick 7.1 or surround in the game for 360°. |
| Windows Sonic / Dolby Atmos / DTS turned on for the device | Mixed down to stereo, so it acts like stereo. Leave these off. |

Detection switches back to full 7.1 immediately as soon as the game uses a channel the current mode ignores.

**Everything glows at the front?** The source is stereo. YouTube and most browser videos play stereo even when labelled 5.1 or 7.1. Use **Speaker check** or a game set to surround instead. The channel meters show it at a glance: if only FL and FR move, it's stereo.

## Anti-cheat

Audio Lumination doesn't inject code, read game memory or simulate input. It only listens to Windows audio output and draws a separate window, the same approach as ASUS Sonic Radar and hardware LED-strip "audio radars". It *should* be safe, but no guarantee is possible. Some games and leagues consider sound visualizers an advantage, so check the rules before competitive or tournament play. For your main competitive game, consider asking its support team first.

## Uninstall

Go to *Settings → Apps → Audio Lumination → Uninstall*, or use **Uninstall...** in the app. It:

- restores your original sound device
- optionally removes VB-CABLE (and offers a restart)
- removes the trust certificate it added
- removes the files, icons and startup entries for every user account

## Building from source

The project is plain C++17 and Win32 with no dependencies.

```sh
# Linux / macOS / MSYS2 with mingw-w64
./build.sh                      # → build/AudioLumination.exe

# Unit tests for the direction engine (runs on any OS)
g++ -O2 -std=c++17 tests/test_core.cpp src/core/al_core.cpp -o test && ./test
```

| Path | What's inside |
|---|---|
| `src/core/` | Portable direction analysis (speaker-pair panning model with phantom-source and 5.1/7.1/stereo detection), downmixing, glow rendering, screen layout, settings (unit-tested) |
| `src/win/audio.cpp` | WASAPI loopback capture, pass-through output, speaker test, device control |
| `src/win/overlay.cpp` | Click-through layered windows around the screen edge |
| `src/win/setup.cpp` | Installer, VB-CABLE download / silent install / setup, uninstaller |
| `src/win/main.cpp` | Tray icon, hotkey, settings window, calibration screen |

| `src/win/updater.cpp` | Checks GitHub releases, downloads and verifies the new version |
| `src/version.h` | The version number (the only place to change it) |

Logs: `C:\ProgramData\AudioLumination\setup.log` and `%APPDATA%\AudioLumination\setup.log`

## Releasing a new version

1. Change the version in `src/version.h` (for example `1.4.1`) and build.
2. On GitHub, go to **Releases → Draft a new release**, then create the tag **`v1.4.1`**. It must match the version exactly.
3. Attach **`AudioLumination-Windows.zip`**, always with exactly this name. It contains `AudioLumination/AudioLumination.exe`. Then publish.

Installed copies find it at their next launch, or within 12 hours. The updater accepts the zip or a bare `AudioLumination.exe`, and only installs a file whose built-in version matches the release tag.

> Optional: if the source and `.github/workflows/release.yml` are in the repo, pushing a `v*` tag builds and publishes the release automatically.

> 🔐 Whoever controls this GitHub account controls what installed copies download, so keep two-factor authentication on.

## Roadmap

- [ ] Code signing (or Microsoft Store release) so Smart App Control and SmartScreen don't block it
- [ ] Per-game profiles
- [ ] Optional on-screen icons for sound types

## Credits

- [VB-CABLE](https://vb-cable.com) by VB-Audio Software is donationware. If it helps you, please donate to them. Audio Lumination isn't affiliated with VB-Audio.
- Inspired by hardware LED "audio radar" strips, built as free software for everyone.

