# Display Profile Manager

[![Platform](https://img.shields.io/badge/platform-Windows-blue.svg)](https://www.microsoft.com/windows)
[![.NET Framework](https://img.shields.io/badge/.NET%20Framework-4.8-purple.svg)](https://dotnet.microsoft.com/download/dotnet-framework/net48)
[![License: MIT + Commons Clause](https://img.shields.io/badge/License-MIT%20%2B%20Commons%20Clause-green.svg)](LICENSE)
[![Built with Claude Code](https://img.shields.io/badge/Built%20with-Claude%20Code-orange.svg)](https://claude.ai/code)

A lightweight Windows desktop application for managing display profiles — save your monitor layout, resolution, refresh rate, HDR state, DPI, audio devices, and scripts into named profiles and switch between them on demand.

This is a fork based on [zac15987/DisplayProfileManager](https://github.com/zac15987/DisplayProfileManager).
and
[exytral/DisplayProfileManager](https://github.com/exytral/DisplayProfileManager)
Distributed under MIT + Commons Clause.

---

# Display Profile Manager

> This is a fork of [exytral/DisplayProfileManager](https://github.com/exytral/DisplayProfileManager).
>
> This fork contains additional fixes focused on HDR state handling on Windows 11 24H2.

---

# Fork Changes

## HDR Profile Switching Improvements

### Background

On Windows 11 24H2, HDR profile switching could behave incorrectly on some HDR-capable displays.

The original implementation used the legacy Windows Display Configuration API
(`DISPLAYCONFIG_GET_ADVANCED_COLOR_INFO`) to detect advanced color states.

However, this API does not clearly separate HDR and Wide Color Gamut (WCG) states.
On some displays using RGB output, the reported state could be ambiguous.

This could cause situations such as:

- HDR ON profile not being detected correctly
- HDR OFF profile changes being skipped
- The application incorrectly assuming the current display state already matched the profile

---

## Changes in This Fork

This fork improves HDR handling with the following changes:

### 1. Improved HDR State Detection

Added support for: on Windows 11 24H2 and later.

Benefits:

- HDR state is detected independently from WCG state
- HDR ON/OFF transitions are more reliable
- Display state detection matches the actual Windows HDR state

The legacy API remains available as a fallback for older Windows versions.

---

### 2. HDR State Verification After Applying Profiles

After changing HDR state, the application now verifies the actual display state.

The verification:

- Re-queries the display configuration
- Confirms that the requested HDR state was applied
- Handles asynchronous Windows display state updates

This prevents silent failures where Windows accepts the request but the display state has not changed.

---

### 3. Improved HDR OFF Profile Handling

HDR OFF profiles now explicitly apply the HDR disabled state.

This prevents a case where:

- Current state: HDR ON
- Profile state: HDR OFF
- Application detects an ambiguous state
- HDR OFF transition is skipped

---

# Relationship with the Original Project

This fork is based on the work of the original Display Profile Manager project and its actively maintained DPM-CS fork.

Special thanks to:

- **[@zac15987](https://github.com/zac15987)** for originally creating Display Profile Manager (DPM)
- **[@exytral](https://github.com/exytral)** for creating and maintaining the DPM-CS fork
- All contributors who improved display management, multi-monitor support,
  color profile handling, and the modern display engine

This fork aims to provide targeted compatibility improvements while preserving
the original project's design and functionality.

---

## ✨ Features

**Profiles & switching**
- 🗂️ **Unlimited display profiles** — save any combination of monitor settings as a named profile
- 🖱️ **Multiple ways to switch** — GUI, global hotkey, system tray, or CLI
- 📺 **Full per-monitor control** — resolution, refresh rate, rotation, DPI, color profile, enable/disable, primary, HDR/ACM
- 🎨 **Color profile per display** — assign an `.icc` color profile to each monitor
- 🪞 **Mirror display support** — clone monitors in pure or mixed extended/mirror configurations
- 🖼️ **Custom profile icons** — assign a `.ico` icon to any profile; shown in the profile list, details panel, and system tray when active
- 📋 **Profile duplication** — copy an existing profile as a starting point
- 📥 **Import** — import `.dpm` profile files

**Automation**
- ⌨️ **Global hotkeys** — assign a keyboard shortcut to any profile for quick switching
- 🔊 **Audio device switching** — automatically switch default playback and recording devices with each profile
- 📜 **Script execution** — run `.exe`, `.ps1`, `.bat`, `.vbs`, `.js`, `.py`, or `.ahk` scripts automatically on profile apply
- 💻 **CLI support** — apply profiles, switch themes, and trigger refreshes from scripts or external tools
- 🎮 **DPM Shortcut Builder** — included Python tool to create game/app launch shortcuts that auto-switch display profiles before launch and restore them on exit, with launcher integration for Steam, Epic, GOG Galaxy, Heroic, and Playnite
- 🚀 **Auto-start with Windows** — Registry mode (no admin) or Task Scheduler mode (faster, one-time admin setup)

**Themes**
- 🎨 **Built-in themes** — Light, Dark, Black, and System (follows Windows)
- 🖌️ **Custom themes** — import compatible `.xaml` theme files; they appear in the dropdown instantly on refresh
- 🛠️ **DPM Theme Builder** — included Python tool to generate themes from the [tinted-themes](https://github.com/tinted-theming/tinted-themes) database

---

## 🚀 Installation

1. Download the latest release from the [Releases](../../releases) page
2. Run `DisplayProfileManager.exe`
3. The application lives in your system tray
4. Your current display settings are automatically saved as the "Default" profile on first launch

**Requirements**
- Windows 10 version 1709+ (Windows 7/8 unsupported — mirror displays, HDR, DPI, and some UI elements will not work correctly)
  - Full ACM support requires Windows 11 24H2+
- [.NET Framework 4.8](https://dotnet.microsoft.com/en-us/download/dotnet-framework)
- No administrator rights required for normal use. Admin is only needed once for Task Scheduler auto-start mode setup.

---

## 📸 Screenshots

### Main Window
![Main Window](./docs/img/main-window.webp)

### Profile Editor
![Edit Window](./docs/img/profile-editor.png)

---

## 📖 Documentation

- [Creating and Managing Profiles](./docs/wiki/profiles.md) — profiles, hotkeys, audio, etc
- [Scripts](./docs/wiki/scripts.md) — supported types, execution, arguments, examples
- [Settings](./docs/wiki/settings.md) — set theme and UX behavior, see configured hotkeys and attributions
- [Themes & DPM Theme Builder](./docs/wiki/themes.md) — built-in themes, importing, generating custom themes
- [CLI & DPM Shortcut Builder](./docs/wiki/cli.md) — all flags, usage examples, generating custom shortcuts
- [Reporting Issues](./docs/wiki/bug_report.md) — what to include when filing a bug report

---

## 🛠️ Development

See [CLAUDE.md](./CLAUDE.md) for architecture, display engine details, and development guidelines.

### Prerequisites
- Visual Studio 2019 or later
- .NET Framework 4.8 SDK

### Building

```bash
git clone https://github.com/exytral/DisplayProfileManager.git
cd DisplayProfileManager
nuget restore DisplayProfileManager.sln
powershell -File dev-build.ps1
```

---

## 📝 License

MIT + Commons Clause — see [LICENSE](LICENSE) for details. Third-party licenses: [THIRD-PARTY-LICENSES.md](THIRD-PARTY-LICENSES.md).

## 🙏 Acknowledgments

- [Newtonsoft.Json](https://www.newtonsoft.com/json) (MIT) — JSON serialization
- [NLog](https://nlog-project.org/) (BSD-3-Clause) — Logging
- [windows-DPI-scaling-sample](https://github.com/lihas/windows-DPI-scaling-sample) (Unlicense) — DPI scaling foundation
- [tinted-themes](https://github.com/tinted-theming) (MIT) — Theme database for DPM Theme Builder
- [Claude Code](https://claude.ai/code) — Built in collaboration with Anthropic's Claude Code

### 🤝 Contributors

**Upstream**
- [@zac15987](https://github.com/zac15987) ([Original project](https://github.com/zac15987/DisplayProfileManager/releases)) — Display profiles, themes, system tray, auto-start, global hotkeys, initial audio device switching support
- [@jarandal](https://github.com/jarandal) ([PR #8](https://github.com/zac15987/DisplayProfileManager/pull/8)) — Initial HDR support, screen rotation
- [@jonathanasdf](https://github.com/jonathanasdf) ([PR #14](https://github.com/zac15987/DisplayProfileManager/pull/14)) — Initial clone display support
- [@rvahilario](https://github.com/rvahilario) ([PR #23](https://github.com/zac15987/DisplayProfileManager/pull/23)) — Partial clone fixes, clone UI, test infrastructure
- [@xtrilla](https://github.com/xtrilla/DisplayProfileManager) ([fork](https://github.com/xtrilla/DisplayProfileManager)) — Safe file saves, stability improvements

**Community**
- [@Catriks](https://github.com/Catriks) ([#1](https://github.com/zac15987/DisplayProfileManager/issues/1)) — Requested audio device switching
- [@Alienmario](https://github.com/Alienmario) ([#1](https://github.com/zac15987/DisplayProfileManager/issues/1), [#5](https://github.com/zac15987/DisplayProfileManager/issues/5)) — Suggested audio improvements and reported multi-monitor switching issues
- [@anodynos](https://github.com/anodynos) ([#2](https://github.com/zac15987/DisplayProfileManager/issues/2)) — Suggested global hotkeys for profile switching
- [@xtrilla](https://github.com/xtrilla) ([#4](https://github.com/zac15987/DisplayProfileManager/issues/4)) — Requested monitor enable/disable
- [@ffgtthr](https://github.com/ffgtthr) ([#2](https://github.com/zac15987/DisplayProfileManager/issues/2)) — Custom profile icons
