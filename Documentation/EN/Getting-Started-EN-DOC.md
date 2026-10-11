# 🧊 IceBoxEngine — Getting Started: Launcher & Updater

## Full documentation in English

### Actual for R-1.0.1 Version

> This is the **first stop** for anyone who has just installed **IceBoxEngine**.
> Before you ever open the editor you meet two small companion applications, and
> this document is the complete reference for both:
>
> * **The Launcher** (`IceBoxEngineLauncher`) — the front door to the engine. It lists
>   your projects, creates new ones, attaches plugins and mods, carries your
>   language/font/theme preferences, and opens a project in the editor.
> * **The Updater** (`IceBoxEngineUpdater`) — keeps your installation current. It checks
>   the project's published releases, compares them to the version you have, downloads
>   the correct installer for your platform, verifies it, installs it, and reopens
>   itself afterwards to tell you how it went.
>
> The **editor itself** — menus, viewport, panels, play mode — is documented
> separately in [The Editor & Interface](Editor-EN-DOC.md). **Building and shipping
> games**, including the engine installers the updater consumes, lives in
> [Profiling & Building Games](Profiling-And-Building-EN-DOC.md). Attaching
> packages to a project is expanded in [Plugins & Mods](Plugins-And-Mods-EN-DOC.md),
> and the assets a project is built from in
> [Assets & Content Browser](Assets-EN-DOC.md). The two scripting modes the
> launcher asks you to pick between are both covered by the
> [Lua API](LuaAPI-EN-DOC.md): it opens with
> [Scripting modes: Code and Visual](LuaAPI-EN-DOC.md#scripting-modes-code-and-visual)
> and the [Visual Scripting reference](LuaAPI-EN-DOC.md#visual-scripting-reference) for
> node graphs, and every function it documents is also a node.

---

## 📑 Table of Contents

1. [Introduction](#1-introduction)
2. [Installation & the three apps](#2-installation--the-three-apps)
   - 2.1 [System requirements](#21-system-requirements)
   - 2.2 [Running the installer](#22-running-the-installer)
   - 2.3 [What gets installed, and where](#23-what-gets-installed-and-where)
   - 2.4 [Starting the Launcher and Updater](#24-starting-the-launcher-and-updater)
   - 2.5 [The engine version scheme](#25-the-engine-version-scheme)
   - 2.6 [Activating the engine](#26-activating-the-engine)
3. [The Launcher](#3-the-launcher)
   - 3.1 [The window at a glance](#31-the-window-at-a-glance)
   - 3.2 [My Projects](#32-my-projects)
   - 3.3 [New Project](#33-new-project)
   - 3.4 [Plugins & Mods](#34-plugins--mods)
   - 3.5 [Settings](#35-settings)
   - 3.6 [About](#36-about)
   - 3.7 [Opening a project: the hand-off to the editor](#37-opening-a-project-the-hand-off-to-the-editor)
4. [The Updater](#4-the-updater)
   - 4.1 [The window at a glance](#41-the-window-at-a-glance)
   - 4.2 [How a check works](#42-how-a-check-works)
   - 4.3 [Downloading & installing an update](#43-downloading--installing-an-update)
   - 4.4 [Settings](#44-settings)
   - 4.5 [Where the version is recorded](#45-where-the-version-is-recorded)
   - 4.6 [Per-platform install behavior](#46-per-platform-install-behavior)
5. [Typical workflows](#5-typical-workflows)
6. [Files & locations](#6-files--locations)
7. [FAQ & troubleshooting](#7-faq--troubleshooting)

---

## 1. Introduction

IceBoxEngine is delivered as **three separate programs** that live side by side in
one installation folder:

| Program | Executable | What it is for |
| ------- | ---------- | -------------- |
| **Launcher** | `IceBoxEngineLauncher` | Pick, create and configure projects; the program you start day-to-day. |
| **Editor** | `IceBoxEngine` | The actual game editor; normally opened *through* the launcher. |
| **Updater** | `IceBoxEngineUpdater` | Check for, download and install new engine versions. |

You almost never run the editor directly. The intended flow is **Launcher → pick a
project → editor opens**, and **Updater whenever you want to move to a newer engine
build**. Both the launcher and the updater are small, single-window ImGui
applications that share the engine's look, its 15-language localization, its
font/theme preferences and its crash reporter, so they feel like one product.

> Everything in this document uses the **default English** labels. Both apps are
> fully localized; switch language at any time in their **Settings** screens (see
> [3.5](#35-settings) and [4.4](#44-settings)).

---

## 2. Installation & the three apps

### 2.1 System requirements

**The editor, the Launcher and the Updater (desktop)**

| | |
| --- | --- |
| **OS** | Windows 10 or newer (x64, x86, arm64) · Linux — Ubuntu 22.04+ / Debian 12+ (x64, x86, arm64) · macOS 11.0 or newer (Apple Silicon or Intel) |
| **CPU** | Dual-core processor |
| **RAM** | 4 GB |
| **GPU** | Windows / Linux: OpenGL 3.3 / 4.6, Vulkan 1.1–1.4 or Direct3D 12 (feature level 11_0). macOS: any Metal-capable GPU (native Metal, ANGLE or MoltenVK). 512 MB of video memory |
| **Disk** | 10–20 GB free space |

**The editor on Android**

| | |
| --- | --- |
| **OS** | Android 8.0 (API 26) or newer — the APK for each CPU architecture is listed in [2.2](#22-running-the-installer) |
| **RAM** | 4 GB (6 GB recommended — the editor keeps the whole project in memory) |
| **GPU** | Vulkan 1.1–1.4, or OpenGL ES 3.2 with an automatic fallback to OpenGL ES 3.0 |
| **Storage** | 2 GB free — the app unpacks its engine data on first run |
| **Input** | Touch; a keyboard and a mouse over USB or Bluetooth work as ordinary input |

**The games you ship**

| Platform | What the player needs |
| -------- | --------------------- |
| **Windows, Linux** | The same OS versions and graphics APIs as the editor above |
| **macOS** | macOS 11.0 or newer (a game can raise it with its **Deployment Target**); Metal |
| **iOS** | iOS 14.0 or newer (iPhone and iPad, arm64); Metal — native or through MoltenVK |
| **Android** | Android 7.0 (API 24) or newer (a game can raise its minimum SDK); OpenGL ES 3.2 / 3.0 or Vulkan 1.1–1.4 |
| **Web** | A modern browser with WebGL 2.0 or WebGPU ([wasm64](Profiling-And-Building-EN-DOC.md#84-web) builds need Chrome/Edge 133+ or Firefox 134+) |
| **Xbox** | Xbox Series X\|S and Xbox One, or Windows 10+ x64 through the Microsoft Store / Xbox app; Direct3D 12 |

Building games additionally needs the toolchain of every target platform — Visual Studio,
Xcode, the Android SDK, Emscripten and so on — on the machine that builds. What to install, and
which host builds which platform, is in
[Profiling & Building → 14. Toolchain prerequisites](Profiling-And-Building-EN-DOC.md#14-toolchain-prerequisites).

### 2.2 Running the installer

Engine installers are named after the version, configuration, OS and CPU
architecture they were built for:

```
IceBoxEngine-1.0.1-Release-Windows-x64-Setup.exe
IceBoxEngine-1.0.1-Release-Windows-x86-Setup.exe
IceBoxEngine-1.0.1-Release-Windows-arm64-Setup.exe
IceBoxEngine-1.0.1-Release-Linux-x64-Setup.deb
IceBoxEngine-1.0.1-Release-Linux-x86-Setup.deb
IceBoxEngine-1.0.1-Release-Linux-arm64-Setup.deb
IceBoxEngine-1.0.1-Release-macOS-arm64-Setup.pkg
IceBoxEngine-1.0.1-Release-macOS-x64-Setup.pkg
```

**Windows.** The `…-Setup.exe` is an **NSIS** installer and requires
administrator rights. It walks you through: *Welcome* → *License agreement*
(`LICENSE.txt`) → *Install folder* → *Components* → *Installing* → *Finish*. The
installer itself speaks **English or Russian**. The finish page offers a
**Launch IceBoxEngineLauncher** checkbox.

The **Components** page lets you turn parts of the install on and off:

| Component | What it does |
| --------- | ------------ |
| **IceBoxEngine (required)** | The editor, launcher, updater, content, config, documentation, tools and SDK. Cannot be unticked. |
| **Desktop Shortcut** | Creates *IceBoxEngineLauncher* on the desktop. |
| **Start Menu Shortcuts** | Creates the *IceBoxEngine* group with *IceBoxEngineLauncher*, *IceBoxEngineUpdater*, *Documentation* and *Uninstall*. |
| **Register .iceproject Extension** | Associates `.iceproject` files so a double-click opens the **editor** directly, with the engine icon. |

The 64-bit installer refuses to run on a 32-bit system and points you at the x86
build instead; the **arm64** installer likewise refuses to run on anything that is
not Windows on ARM and points you at the x64 build. Installing **on top of** an existing installation is handled for
you: the installer offers to uninstall the previous version first, and in silent
mode (`/S`) it closes any running `IceBoxEngine`, `IceBoxEngineLauncher` and
`IceBoxEngineUpdater`, removes the old version and installs into the same folder — which
is exactly what the updater triggers under the hood
([4.6](#46-per-platform-install-behavior)).

Windows installs also write a small amount of registry state:
`HKLM\Software\IceBoxEngine` (`InstallDir`, `Version`, `Path`) and the standard
*Add or Remove Programs* entry under
`HKLM\…\CurrentVersion\Uninstall\IceBoxEngine`. Uninstalling removes the install
folder, both shortcut sets, the file association and all of that state.

**Linux.** Install the `.deb` with your package manager or `sudo apt install
./IceBoxEngine-…-Setup.deb`. The package first asks you to accept the
**IceBoxEngine License Agreement** (a short summary followed by the question
*Do you accept the IceBoxEngine License Agreement?*); declining cancels the
installation (with a bare `dpkg -i`, which has already unpacked the files by then, the
package is left unconfigured and tells you to remove it with
`sudo dpkg --purge iceboxengine`). The full agreement is installed as
`/opt/iceboxengine/LICENSE.txt` and `/usr/share/doc/iceboxengine/copyright`.

The package installs into `/opt/iceboxengine`, symlinks
`IceBoxEngineLauncher`, `IceBoxEngine` and `IceBoxEngineUpdater` into `/usr/bin`, installs
`IceBoxEngineLauncher.desktop`, `iceboxengine.desktop` and `IceBoxEngineUpdater.desktop` into
`/usr/share/applications`, puts the engine icon into the `hicolor` icon theme and
registers the `application/x-iceproject` MIME type for `*.iceproject`.
It depends on `libgl1`, `libx11-6` and `zenity`, and recommends
`libasound2`/`libasound2t64`, `libpulse0`, `libwayland-client0`, `libxkbcommon0`,
`libdecor-0-0` and `libvulkan1` (the Vulkan backend loads `libvulkan.so.1` at
runtime). Removing the package deletes `/opt/iceboxengine` and refreshes the
desktop and MIME databases.

**macOS.** The `.pkg` is a `productbuild` package that shows the license agreement,
then installs system-wide into
`/Applications/IceBoxEngine`, where the three programs live as `.app` bundles
(`IceBoxEngineLauncher.app`, `IceBoxEngine.app`, `IceBoxEngineUpdater.app`). Its post-install
script places an **IceBoxEngineLauncher** alias on the desktop of the logged-in user and
re-registers the bundles with LaunchServices.

**Android.** The engine also ships as an `.apk` that carries the launcher, the editor and the
game runtime in one app. There are four of them, one per CPU architecture, and you download the
one that matches your device:

| File | For |
| ---- | --- |
| `IceBoxEngine-1.0.1-Release-Android-arm64-v8a.apk` | Every phone and tablet made in roughly the last eight years. **Take this one if you are not sure.** |
| `IceBoxEngine-1.0.1-Release-Android-armeabi-v7a.apk` | Older 32-bit ARM devices |
| `IceBoxEngine-1.0.1-Release-Android-x86_64.apk` | Android emulators on a PC, Chromebooks, x86 tablets |
| `IceBoxEngine-1.0.1-Release-Android-x86.apk` | 32-bit x86 emulators and very old x86 tablets |

Each file holds one architecture only, which is what keeps it small; the wrong one simply
refuses to install with `INSTALL_FAILED_NO_MATCHING_ABIS`, so nothing breaks if you guess wrong
— download another one. Install it the way you install any sideloaded APK: copy it to the
device, tap it, and allow your file manager or browser to install apps once when Android asks.
There is no separate updater on Android; a new version is a new `.apk`, installed over the old
one (the same signing key keeps your data in place).

On first run the app unpacks its engine data into
`Android/data/com.iceboxengine.editor/files/Engine/` and asks for **All Files Access**; the
launcher opens as soon as you have answered. With that permission it creates `IceBoxProjects/`,
`IceBoxBuilds/`, `IceBoxKeystores/`, `IceBoxExports/` and `UserData/` in
`/storage/emulated/0/IceBoxEngine/` — right next to `Download/`, `DCIM/` and the rest, where a
file manager and a PC over USB reach them. Without it they are created beside `Engine/` instead;
granting the permission later from the launcher switches over on the spot — the launcher reloads
itself and brings your project list and preferences along. Unpacking takes a few seconds and
happens again after every engine update; your projects, builds and keystores are outside
`Engine/` and are never touched. `UserData/` holds the launcher's project list and your editor
preferences - language, font, theme and the renderer that was picked - so they survive every
update, and because the folder sits in shared storage they also survive uninstalling and
reinstalling the app.

The Android app needs **Android 8.0 (API 26) or newer**. Take the APK that matches the
device: **arm64-v8a** for practically every current phone and tablet, **x86_64** for an
emulator or a Chromebook. The two 32-bit builds (**armeabi-v7a**, **x86**) run the full
editor too, but a 32-bit process has far less address space, so a large project can run
out of memory there.

A project started on the phone does not have to stay there. **Build Game… → Project Export →
Export Project (.zip)** writes the whole project to `IceBoxExports/` and to the phone's
**Downloads** folder; unpack that `.zip` on a PC and add the folder from the desktop launcher.
The other way round, zip the project folder on the PC, copy it to the phone and press **Import
Project (.zip)…** on the launcher's *My Projects* tab. Details in
[Profiling & Building, 8.8](Profiling-And-Building-EN-DOC.md#88-building-on-android-itself).

### 2.3 What gets installed, and where

| Platform | Install root |
| -------- | ------------ |
| **Windows** | `C:\Program Files\IceBoxEngine` (x86 build: `C:\Program Files (x86)\IceBoxEngine`) |
| **Linux** | `/opt/iceboxengine` |
| **macOS** | `/Applications/IceBoxEngine` |
| **Android** | `Android/data/com.iceboxengine.editor/files/Engine` (unpacked from the APK on first run) |

Whatever the platform, the layout inside that folder is the same:

```
<install root>/
├── IceBoxEngine(.exe)           ← editor
├── IceBoxEngineLauncher(.exe)   ← launcher
├── IceBoxEngineUpdater(.exe)    ← updater
├── *.dll / *.so                 ← shared libraries
├── Config/                      ← engine + updater configuration, fonts, languages
│   ├── Updater.json             ← current engine version + updater settings
│   ├── Engine.json              ← engine/editor defaults (rendering, audio, accessibility…)
│   ├── Editor.json              ← editor + build settings
│   ├── Plugins.json  Mods.json  ← which packages the engine enables
│   ├── CollisionGroups.json  VisualScriptAPI.json  DebugBreakpoints.json
│   ├── Fonts/                   ← NotoSans + its Arabic/Hebrew/Devanagari/CJK faces, Noto Emoji
│   └── Languages/               ← en, ru, ua, zh, ar, hi, es, pt, ja, fr, de, it, pl, he, kr
├── Content/
│   └── Examples/                ← bundled examples + the Examples.json catalog
├── Documentation/               ← this documentation set (EN/ and RU/)
├── Plugins/  Mods/              ← engine-level packages you can add to projects
├── Tools/                       ← Build Game scripts & templates, Plugin Builder, Remote Preview
│                                  companion app, Python scripts, helpers (logo/icons)
├── Source/  lib/                ← the SDK headers and prebuilt core libraries used to build games
├── Lib/  DLLs/  python(.exe)    ← the Python runtime of the editor's Python scripting
│                                  (Windows layout; Linux uses lib/python3.x/ and bin/python3)
├── CMakeLists.txt  vcpkg.json   ← read by Build Game
├── LICENSE.txt  THIRD_PARTY_NOTICES.txt  PRIVACY_NOTICE.txt  ThirdPartyLicenses/
└── Uninstall.exe                ← Windows only
```

Nothing you create ever lands here: projects live wherever you put them, and your
personal settings live in the user-data folder
([6. Files & locations](#6-files--locations)).

On **Android** the same layout is unpacked from the APK, minus the pieces a phone has no use
for: there is no `IceBoxEngineUpdater`, no `Tools/IceBoxPreview` and no `Tools/PythonScripts`,
and `Tools/BuildSystem` carries only the Android runtime template the editor repacks into your
games. The editor and the launcher are the one installed app rather than separate
executables.

### 2.4 Starting the Launcher and Updater

There is no wrong way to start them:

* **Launcher** — the desktop shortcut/alias, the Start-Menu or application-menu
  entry, or `IceBoxEngineLauncher` in the install folder. The finish page of the Windows
  installer can also launch it for you. On **Android** the launcher is simply the app's first
  screen: tap the IceBoxEngine icon and it opens, and picking a project hands it straight to
  the editor in the same app.
* **Updater** — the *IceBoxEngineUpdater* Start-Menu entry, `IceBoxEngineUpdater` in the
  install folder, or the **Updater** button in the launcher's sidebar
  ([3.1](#31-the-window-at-a-glance)).

Both apps **set their working directory to the install root** on launch, so they
always find the same `Config/`, `Content/`, fonts and languages regardless of where
they were started from. Both open a **1280 × 720** window that is resizable and
high-DPI aware, both mirror their layout automatically for the right-to-left
languages (Arabic, Hebrew), and both install the engine's **crash reporter** — if a
previous run crashed, its pending report is picked up at startup.

They also pick their graphics backend the same way the editor does. On the very first
run — when `Config/Engine.json` has no `Rendering.RenderBackend` recorded yet — the
platform chain is probed from the top and the first renderer that answers is written
back, so every later start goes straight to it:

| Platform | Chain, highest first |
| -------- | -------------------- |
| **Windows** | Direct3D 12 → Vulkan 1.1-1.4 → OpenGL 4.6 → OpenGL 3.3 |
| **Linux** | Vulkan 1.1-1.4 → OpenGL 4.6 → OpenGL 3.3 |
| **macOS** | Metal → Metal (MoltenVK) → Metal (ANGLE) |
| **Android** (the launcher inside the app) | Vulkan → OpenGL ES 3.2 → OpenGL ES 3.0 |

From then on the recorded `Rendering.RenderBackend` decides, and every failure steps
exactly one rung further down the same chain: if Direct3D 12 fails to initialise the
window is recreated on Vulkan and then on OpenGL; if Metal fails the window is recreated
on MoltenVK and then on ANGLE; an OpenGL 4.6 context falls back to 3.3; on Android a
failed Vulkan start moves on to OpenGL ES 3.2 and then 3.0. Whatever the
tools end up on is written back and shown in their **Settings**. You never have to
configure this by hand — it is only worth knowing if a machine has unusual drivers.

> The launcher and the updater stay **independent programs**. The launcher's
> **Updater** button is only a shortcut that starts the updater — it never checks,
> downloads or installs anything itself. Updating remains a deliberate, separate
> action you take inside the updater window, which keeps project work and engine
> maintenance cleanly apart.

### 2.5 The engine version scheme

Both apps speak the same version format:

```
<prefix>-<major>.<minor>.<patch>      e.g.  R-1.0.0
```

The short prefix marks the **maturity stage**; the launcher and editor expand it
into a friendly name for display:

| Prefix | Stage (displayed) | Example | Precedence |
| :----: | ----------------- | ------- | :--------: |
| `P`  | Prototype   | `P-0.1.0`  | lowest |
| `A`  | Alpha       | `A-0.3.0`  | ↓ |
| `B`  | Beta        | `B-0.8.4`  | ↓ |
| `PR` | Pre-Release | `PR-0.9.1` | ↓ |
| `R`  | Release     | `R-1.0.0`  | highest |

> **IceBoxEngine ships at the `R` (Release) stage.** The stages below it are the
> ladder the engine climbed to get here. They stay in the scheme so that older tags
> still compare correctly, and so that a preview build — if we ever hand one out —
> is told apart from a shipping one at a glance.

When comparing two versions, the **stage is weighed first** (a `R-` build always
outranks any `B-` build, regardless of numbers), and only then the numeric
`major.minor.patch`. So `B-0.8.4` is *newer* than `B-0.6.9`, and `R-1.0.0` is newer
than `PR-9.9.9`. A tag carrying an unrecognized prefix sorts **below** `P`, and a
tag with no prefix at all is read as `0.0.0` at the Prototype level — so a malformed
release tag can never look newer than a real one.

`Config/Updater.json` → `currentVersion` is the single source of truth for the
version, and far more than the updater reads it:

* the launcher shows it under the logo (e.g. **Release 1.0.0**) and on its **About**
  page, and the editor displays it too;
* every IceBoxEngine program also carries it as a compiled-in fallback, so the version
  still displays correctly if the file is missing or unreadable;
* the updater uses it as the baseline for every comparison
  ([4.5](#45-where-the-version-is-recorded)).

> Projects record only the **numeric part** (`1.0.0`) in their `.iceproject`
> manifest, and the launcher's *engine version mismatch* marker compares just those
> numbers — it does not care whether a project was last saved by a Beta or a
> Release build of the same `major.minor.patch`.

### 2.6 Activating the engine

A distributed IceBoxEngine build asks for a **license key** the first time you
start it. Activation is a one-off step per computer: after it succeeds the
launcher, the editor and the updater all open straight away, and reinstalling the
engine does **not** ask again.

**The activation screen.** Start **IceBoxEngineLauncher**. If the machine is not
activated yet, the launcher opens on a single centred card instead of the project
hub:

| Element | What it does |
| ------- | ------------ |
| **License key** | A multi-line box. Paste the key exactly as you received it — spaces, line breaks and the `ICEB-` prefix are all tolerated, and lookalike characters (`I`/`1`, `O`/`0`) are corrected automatically. |
| **Paste** | Pastes the clipboard into the box. `Ctrl+V` inside the box works too. |
| **Clear** | Empties the box. |
| Drag & drop | Dropping a key file (for example `mykey.icekey`) anywhere on the window loads its contents into the box. |
| **Device ID** | The identifier of *this* computer, shown as `XXXX-XXXX-XXXX-XXXX`. |
| **Copy** | Copies a short support block — Device ID, key id, status, key-set id, and the activation request when one is on screen — to the clipboard. |
| **Activate** | Validates and stores the key. `Ctrl+Enter` does the same. |
| **Quit** | Closes the launcher without activating. |

**Three kinds of key.**

* A **machine-locked key** is issued for one specific **Device ID** and activates
  only on that computer. If you bought the engine before installing it, install
  first, read the Device ID off the activation screen, send it to support and you
  will receive a key that fits your machine.
* A **single-machine key** is handed out immediately at purchase and is spent on
  the first computer it activates. When the build carries an activation endpoint,
  pasting it is all you have to do — the key is checked once over the internet,
  bound to this computer and activated on the spot; a second computer using the
  same key is refused. When there is no endpoint, or it cannot be reached, the
  screen answers instead with an **activation request** — a short `ICEQ-…` code
  that names the key and this computer. Send that code to support (the **Copy
  activation request** button puts it on the clipboard together with the rest of
  the details) and you get back a machine-locked key for this computer.
* A **shared key** activates straight away on any computer. Some stores hand
  these out so that a purchase needs no message to support at all.

**The one-time online check.** Distributed builds are configured with an
activation endpoint. Pressing **Activate** then sends the key once over HTTPS so
the server can confirm it is genuine, has not been revoked, and is not already in
use on another computer. The screen says so above the key box, and shows
*Checking the license key with the activation server…* while it waits.

  * It happens **once**, when you activate. The engine never contacts the server
    again — not at startup, not on a timer, not when it updates.
  * A machine-locked key still activates with no internet at all, because it is
    already tied to your computer.
  * If the server cannot be reached, the message says so; connect and press
    **Activate** again, or ask support for a key that activates offline.
  * What is sent, and what is kept, is listed in section 9 of `PRIVACY_NOTICE.txt`
    next to the engine.

The launcher's **About** tab shows **Online activation: Verified** once the check
has been passed.

**Where the activation is stored.** Once a key is accepted, a signed activation
record is written to several independent places at once, so removing any one of
them does not cost you the activation:

| Platform | Locations |
| -------- | --------- |
| Windows | `%ProgramData%\IceBoxCrew\IceBoxEngine\`, `%LOCALAPPDATA%\IceBoxCrew\IceBoxEngine\`, `%APPDATA%\IceBoxEngine\`, `%USERPROFILE%\.iceboxengine\`, and the registry under `HKCU\Software\IceBoxCrew\IceBoxEngine` (plus `HKLM` when the launcher runs elevated) |
| Linux | `/var/lib/IceBoxCrew/IceBoxEngine/` (when writable), `~/.config/IceBoxEngine/`, `~/.local/share/IceBoxCrew/IceBoxEngine/`, `~/.iceboxengine/` |
| macOS | `/Users/Shared/IceBoxCrew/IceBoxEngine/`, `~/Library/Application Support/IceBoxEngine/`, `~/Library/Preferences/IceBoxCrew/`, `~/.config/IceBoxEngine/`, `~/.iceboxengine/` |
| Android | `/storage/emulated/0/IceBoxEngine/UserData/` and `/storage/emulated/0/.iceboxengine/` (both need All Files Access), plus the app's own `Android/data/com.iceboxengine.editor/files/UserData/`, `.iceboxengine/` and `.local/share/IceBoxCrew/IceBoxEngine/` |

None of these live inside the install folder, so **uninstalling and reinstalling
the engine — or updating it — keeps the activation**. Wiping the machine (a fresh
OS install, a reformat or a phone's factory reset) removes them all; ask support for
a replacement key in that case.

On Android the copies inside `Android/data/` go away together with the app, so it is
the two in shared storage that carry the activation across a reinstall: answer the
**All Files Access** request after installing again and the launcher finds the
activation by itself — no key to enter. A copy that goes missing from one of the
locations is written back the next time the engine starts.

The record is bound to the hardware it was created on, so copying it to another
computer does not carry the activation with it. Ordinary upgrades are fine: the
check tolerates a renamed machine, a new GPU, more RAM or a swapped secondary
drive.

**The editor and the updater.** They perform the same check at startup. On a
machine that is not activated they show a short notice, open the launcher for you
and exit — so activation always happens in one place. Games you build and ship
never carry any of this: the licence check exists only in the launcher, the editor
and the updater, never in the runtime or the engine core your game links against.

**Checking your licence later.** The launcher's **About** tab
([3.6](#36-about)) shows the status, edition, key id, activation date, expiry and
Device ID, with a **Copy activation details** button for support requests.

**If activation fails.** The screen names the exact reason:

| Message | Meaning |
| ------- | ------- |
| *Enter a license key first* | **Activate** was pressed with an empty box. |
| *That does not look like an IceBoxEngine license key* | Part of the key is missing — copy the whole block again. |
| *This key is not genuine* | The signature does not verify. The key was altered or did not come from IceBoxCrew. |
| *This key belongs to a different product* | A genuine IceBoxCrew key, but not one issued for IceBoxEngine. Check that you pasted the right key. |
| *This key was issued for a different computer* | A machine-locked key on the wrong machine. Send your Device ID to support. |
| *This key unlocks one computer…* | Not an error. A single-machine key was recognised and the activation server was not reachable; send the `ICEQ-` activation request shown underneath to support and paste the key that comes back. |
| *This key is already activated on another computer* | A single-machine key that the activation server has already bound to a different machine. If you changed computers, contact support with the key id. |
| *The activation server could not be reached* | No internet, or the endpoint is temporarily down. Connect and press **Activate** again, or ask support for a key that activates offline. |
| *The activation server refused this key* | The server rejected the key for a reason other than the seat being taken. Contact support with the key id. |
| *This key requires a newer version of IceBoxEngine* | A single-machine key on a build that predates them. Update the engine, then activate. |
| *This key has expired* / *has been revoked* | Contact support with the key id. |
| *The activation could not be saved* | No storage location was writable. Start the launcher once as administrator (Windows) or check the home-directory permissions. |
| *This activation belongs to a different computer* | A record copied from another machine was found. Enter your own key to activate this one. |
| *The activation record is damaged* | The stored activation no longer verifies (it was edited, or a single-machine activation is missing its server confirmation). Enter your key again to activate. |
| *This computer could not be identified, so activation is not possible* | The engine could not read any hardware identifier on this machine. Contact support with the details from **Copy**. |

---

## 3. The Launcher

The launcher is a project hub: a fixed **sidebar** of tabs on the left and a wide
**content area** on the right.

### 3.1 The window at a glance

```
┌───────────────────┬──────────────────────────────────────────────┐
│   🧊 logo          │                                              │
│ IceBoxEngine™     │            Active tab content                │
│ Release 1.0.1     │   (My Projects / New Project / Plugins &     │
│ ───────────────── │    Mods / Settings / About)                  │
│ My Projects       │                                              │
│ New Project       │                                              │
│ Plugins&Mods      │                                              │
│ ───────────────── │                                              │
│ Settings          │                                              │
│ About             │                                              │
│                   │                                              │
│ [ Updater ]       │                                              │
│ [ Exit ]          │                                              │
└───────────────────┴──────────────────────────────────────────────┘
```

The sidebar is 240 px wide and always shows the **engine logo**, the
**IceBoxEngine™** name, and the **installed version** read from
`Config/Updater.json`. Below them are the five tabs (the **My Projects** tab also
shows a live project count, e.g. *My Projects (3)*), and two buttons pinned to the
bottom corner: an amber **Updater** button and a red **Exit** button. Switching
tabs cross-fades the content area over ~0.18 s.

**Updater** starts the separate `IceBoxEngineUpdater` app (resolved as a sibling of the
launcher, the same way the editor is) so you do not have to open the install folder
to reach it. The launcher stays open and nothing is updated by pressing it — every
check, download and install still happens inside the updater window, exactly as
described in [4](#4-the-updater). If the updater executable is missing next to the
launcher, or fails to start, the launcher says so instead of failing silently.

You can **drag and drop** a project folder onto the window from anywhere in the
launcher to add it to your list; dropping a file uses the folder that contains it,
and a successful drop switches you to **My Projects**.

### 3.2 My Projects

This is the default tab — your library of known projects. The area is split into a
**project list** on the left and a **details panel** on the right; the details panel
narrows below ~700 px of available width and disappears entirely below ~560 px, so
the list always stays usable.

**Populating the list.** Two buttons sit at the top:

| Button | Action |
| ------ | ------ |
| **Add Existing Project…** | Opens a folder picker. Choose a folder that contains a `.iceproject` file to register it. If the folder has no `.iceproject`, or the project is already listed, the launcher tells you which of the two it was. |
| **Scan All Drives…** | Spawns a background search for project folders — any folder that holds a `.iceproject` file — in your home folder and on every local disk, and adds the ones that are not listed yet, including projects you removed from the list. A *Scanning all drives for projects…* indicator runs while it works and the button is disabled; you can keep using the launcher meanwhile, and closing it cancels the scan. When it finishes you get the number of projects added, or *No new projects found on any drive.* What exactly is searched is described below. |

You can also **drag-and-drop** a project folder onto the window to add it. And on a
completely fresh install — when there is no saved list yet — the launcher seeds
itself from the default projects folder (`~/IceBoxProjects`).

**What Scan All Drives covers.** The search starts in your home folder and then walks
the local disks: every fixed and removable drive on Windows, `/Users` and `/Volumes` on
macOS, the whole tree from `/` (plus `/run/media`) on Linux. To stay fast and to keep
junk out of the list it skips:

* **system locations** — `Windows`, `Program Files`, `Program Files (x86)`,
  `ProgramData` and a File History backup (`FileHistory` in the root of a drive) on
  Windows; `~/Library` on macOS; `/proc`, `/sys`, `/dev`, `/run`, `/tmp`, `/boot`,
  `/bin`, `/sbin`, `/lib*`, `/usr`, `/etc`, `/snap`, `/nix`, `/gnu`, `/sysroot`,
  `/ostree`, `/lost+found` and the service folders of `/var` (`lib`, `cache`, `log`,
  `tmp`, `spool`, `crash`, `backups`, `snap`) on Linux;
* **hidden folders** — a name that starts with a dot on any system, plus the *Hidden*
  attribute on Windows, which covers the Recycle Bin and `AppData`;
* **network and optical drives**, and on macOS the volumes Finder does not show;
* **symbolic links and junctions**, so nothing is visited twice.

A folder the launcher may not read, or whose path is too long, is passed over on its
own — it never stops the rest of the search. The home folder and the root of a drive
are never taken for a project themselves. The search does not look inside a project it
has found (unless that folder has neither `Content` nor `Config`, in which case it is
added and searched further), and it ignores packaged games — a folder where a
`game.json` with a `GameName` sits next to the `.iceproject` — so cooked copies under
`Saved/Cooked` and staged build folders never show up as projects. On macOS the system
may ask you to allow access to Desktop, Documents, Downloads and removable volumes;
whatever you decline is simply skipped. A project that lives on a network or optical
drive, or inside a hidden folder, is added with **Add Existing Project…** or
drag-and-drop.

On **Android** the two buttons are **Import Project (.zip)…**, which unpacks a project
archive picked in the system file picker into `IceBoxProjects/`, and **Scan Device
Storage…**, which searches the phone's shared storage and any memory card (both need
All Files Access) plus the app's own folder for `.iceproject` files. The scan follows
the same rules as on desktop: it skips `Android/data`, `Android/obb` and hidden folders,
does not look inside a project it has already found and ignores packaged games, so it
also brings back projects you removed from the list. The default projects folder there
is `/storage/emulated/0/IceBoxEngine/IceBoxProjects`.

Every startup the list is **validated**: entries whose folder or `.iceproject` file
has disappeared are dropped, the *Modified* timestamp of the survivors is refreshed,
and each manifest is re-read so description, license, scripting mode and package
lists stay current.

**Finding and ordering.** A **search** box (`Search projects…`) filters by **name
and path** (with a **Clear** button), and a **Sort by** dropdown offers **Name**,
**Date Modified**, **Last Launched** and **Custom**, plus an
**ascending/descending** toggle (the `^` / `v` button, tooltip *Ascending* /
*Descending*). Ties are broken by name, name sorting is case-insensitive, and
**pinned projects always come first** whatever the mode.

In **Custom** order you can **drag project rows** up and down to arrange them by
hand. Reordering only works when the search box is empty and the row is not pinned,
and the direction toggle has no effect in this mode — the order is exactly the one
you set.

**Each project row** is 64 px tall and shows:

* the project **thumbnail** (48 px) when the project has one — see below;
* a `*` **pin marker** and the project **name**;
* a small **green dot** after the name if the project was launched in the last
  **7 days**;
* the full **path** underneath;
* **Modified: `<date>`** right-aligned;
* an amber **`!`** just before it when the project's recorded engine version differs
  from the one you have installed, so you know the project may upgrade on open.

> **Project thumbnails.** The editor writes `project_thumbnail.png` (256 × 256) and
> `project_thumbnail_high.png` (512 × 512) into the project folder when it shuts
> down, capturing the last frame of the viewport. They are what you see in the row
> and in the details panel — so a project only gets a picture after you have opened
> and closed it once. Deleting the files simply removes the preview.

**Selecting.**

* **Double-click** a row (or right-click → **Open Project**) to launch it in the
  editor.
* **Single-click** selects; **Ctrl-click** toggles individual rows and
  **Shift-click** selects a range.
* Selecting **two or more** projects reveals a **bulk toolbar** above the list:

| Bulk button | What it does |
| ----------- | ------------ |
| **Pin All** / **Unpin All** | Pins or unpins every selected project. |
| **Remove from List** | Forgets all selected projects. **No files are deleted.** |
| **Export List…** | Writes the selected projects (name, path, engine version) to `exported_projects.json` in your user-data folder and shows you the full path. |
| **Clear** | Clears the selection. |

**Right-clicking** a row opens the context menu:

| Menu item | What it does |
| --------- | ------------ |
| **Open Project** | Launches the project in the editor. |
| **Show in File Browser** | Reveals the project folder in Explorer / Finder / your file manager. |
| **Pin to Top** / **Unpin** | Keeps the project at the top of the list. |
| **Rename…** | Renames the project folder **and** its `.iceproject`, and updates `Name` inside the manifest. |
| **Move…** | Moves the project folder to a new parent location (falls back to copy-then-delete across filesystems). |
| **Duplicate…** | Copies the project under a new name/location, renames the manifest and adds the copy to your list. |
| **Manage Plugins & Mods…** | Jumps to the Plugins & Mods tab with this project pre-selected. |
| **Remove from List** | Forgets the project here **without deleting** any files. |
| **Delete Project** | Permanently deletes the project folder from disk (after a confirmation). |

> **Remove from List** is safe — it only forgets the entry. **Delete Project** is
> destructive and cannot be undone; the launcher always asks first and names the
> project in the confirmation dialog.

Rename, Move and Duplicate all refuse to overwrite an existing folder. If a
project's `.iceproject` turns out not to be valid JSON, the launcher **keeps the
file exactly as it is** and skips the `Name` update rather than risk losing your
data — the failure is reported in the log.

**Keyboard shortcuts** (on the **My Projects** tab, while you are not typing in a
text field):

| Shortcut | Action |
| -------- | ------ |
| `Ctrl` + `F` | Focus the search box |
| `Enter` | Open the selected project |
| `F2` | Rename the selected project |
| `Ctrl` + `D` | Duplicate the selected project |
| `Ctrl` + `Shift` + `P` | Pin / unpin — the whole multi-selection if there is one |
| `Delete` | Delete the selected project (asks for confirmation) |
| `Esc` | Close the open dialog |

**The details panel** on the right shows everything the launcher knows about the
selected project (and *Select a project to see details* when nothing is selected):

* the **preview image** — the high-resolution thumbnail when available, otherwise
  the small one, otherwise a `[ No Preview ]` placeholder;
* the project **name**, plus a **Pinned** marker;
* **Path**, **Last Modified**, **Last Launched** (or *Never*);
* **Created with Engine** — the version recorded in the manifest, or
  *Unknown (legacy project)*, followed by an amber
  **Engine version mismatch (`0.6.9` -> `1.0.0`)** line when they differ;
* **License** — the display name of the license chosen at creation;
* **Scripting Mode** — *Code Scripting* or *Visual Scripting*;
* **Plugins** and **Mods** — resolved to the packages' display names and versions
  where the engine knows them, or *None*;
* an editable **Description** box. Type into it and click away: the text is written
  straight back into the project's `.iceproject` manifest (clearing it removes the
  field). A manifest that cannot be parsed is left untouched.
* a green **Open Project** button.

Your list, pins and custom order persist between sessions (see
[6. Files & locations](#6-files--locations)).

### 3.3 New Project

The **New Project** tab creates a fresh project folder. You provide:

| Field | Meaning |
| ----- | ------- |
| **Project Name** | Project name; also the folder name and the `.iceproject` file name. |
| **Description** | Optional free-text blurb stored in the manifest (and editable later from the details panel). |
| **License** | The project's license/EULA — see the table below. *Proprietary* additionally requires a **Studio / Copyright Holder** name, and the launcher refuses to create the project without it. |
| **Location** | Parent folder the project is created in; **Browse…** opens a native folder picker. Defaults to `~/IceBoxProjects`. |
| **Include Starter Content** + **Select Example** | Optionally seed the project with one of the bundled examples. Every example declares which scripting mode it is written for. **Refresh Catalog** re-reads `Examples.json` without restarting. |
| **Include Plugins / Include Mods** | Tick engine-level packages to copy into the new project. Each row shows the package icon, name and version, with its description as a tooltip. |
| **Scripting Mode** | **Code Scripting** (gameplay logic in Lua, text editor) or **Visual Scripting** (node-graph, no coding required). |

**The example decides the scripting mode.** Each entry in the list carries a
**Code** / **Visual** / **Code + Visual** badge. Picking an example that is written
for one mode switches **Scripting Mode** to that mode and greys the other option
out — so a Lua example can never be dropped into a Visual Scripting project, or the
other way round. The greyed-out radio button explains why in its tooltip, and a
warning line repeats it under the choice. Examples that work with both modes leave
the choice to you, and unticking **Include Starter Content** frees the choice again.
Picking an example also **auto-ticks** the plugins and mods it declares.

**The details panel.** With an example selected, the panel on the right of the tab
shows its **preview image** (letterboxed, never stretched), name, mode badge,
description, **Platforms** (as badges, with the platform you are running
highlighted, and a warning when yours is not listed), **Author**, **Version**,
**Tags**, **Enabled with this example**, and the **Folder** it comes from.
**Open Example Folder** reveals it in your file browser. If the window is too narrow
for two columns, the same card is folded into the form under the scripting mode.

#### Project licenses

| License id | Shown as | Writes `LICENSE.txt`? |
| ---------- | -------- | :-------------------: |
| `None` | None (default) | no |
| `Custom` | Custom | yes — a placeholder for you to replace |
| `Proprietary` | Proprietary | yes — full all-rights-reserved text, requires the studio name |
| `MIT` | MIT | yes |
| `Apache-2.0` | Apache License 2.0 | yes |
| `BSD-2-Clause` | BSD 2-Clause | yes |
| `BSD-3-Clause` | BSD 3-Clause | yes |
| `ISC` | ISC | yes |
| `MPL-2.0` | Mozilla Public License 2.0 | yes — SPDX stub |
| `LGPL-3.0-only` | GNU LGPL v3.0 | yes — SPDX stub |
| `GPL-3.0-only` | GNU GPL v3.0 | yes — SPDX stub |
| `AGPL-3.0-only` | GNU AGPL v3.0 | yes — SPDX stub |
| `BSL-1.0` | Boost Software License 1.0 | yes — SPDX stub |
| `Zlib` | zlib License | yes — SPDX stub |
| `Unlicense` | The Unlicense | yes |
| `CC0-1.0` | Creative Commons CC0 1.0 | yes |
| `WTFPL` | WTFPL | yes — SPDX stub |

The copyright holder is the **Studio / Copyright Holder** field when you filled it
in, otherwise the project name, and the year is the current one. An *SPDX stub* is a
short header with the `SPDX-License-Identifier`, the copyright line and a link to
the full text on spdx.org — replace it with the full license if you ship the
project.

##### Copyleft licenses need a linking exception

`LGPL-3.0-only`, `GPL-3.0-only` and `AGPL-3.0-only` are copyleft licenses, and the
launcher shows a warning when you pick one. The file they write covers **your own
source code and assets**, which is yours to license however you like. It does not,
on its own, cover the **binary** that Build Game produces.

The reason is that a built game links statically against `lib/IceBoxCore`, the
prebuilt engine core, which is proprietary and licensed to you under `LICENSE.txt`.
The GPL family requires the whole combined work to be distributable under GPL terms;
the engine core cannot be, and the GPL's system-library exception does not reach it.
So the binary you ship would satisfy neither license at once.

There is a standard, well-understood fix, and it is one paragraph. Add a linking
exception to your own license, alongside the SPDX header:

```
As a special exception, the copyright holders of this program give you permission
to link it with the IceBoxEngine runtime and the prebuilt IceBoxEngine core
libraries (lib/IceBoxCore), and to distribute the resulting executable, without
this permission extending the requirements of the GNU General Public License to
those components. You must obey the GNU General Public License in all respects for
all of the code used other than those components.
```

With that paragraph in place your source stays copyleft, the shipped binary is
distributable, and nobody has to guess. `MPL-2.0` needs none of this — it is
file-level copyleft and explicitly permits combining with proprietary code in a
Larger Work. Every other license in the table above is permissive and combines with
the engine core without any extra wording.

#### What Create Project does

Pressing **Create Project** makes the launcher:

1. Refuse to continue if the target folder already exists, or if *Proprietary* was
   chosen without a studio name.
2. Create `<Location>/<Name>/` with `Content/` and `Config/` inside it.
3. Copy the chosen example into `Content/Examples/<Example>/` (the folder name is
   validated first — a catalog entry can never escape `Content/Examples`).
4. Write `Config/Engine.json` seeding the project's editor language, font name,
   font size and font color from your current preferences, plus
   `Rendering.RenderBackend` — probed right there by walking this machine's renderer
   chain from the top, so the new project opens on the best renderer it has instead of
   a generic default.
5. Copy the ticked plugins and mods into `Plugins/` and `Mods/`.
6. Write `LICENSE.txt` for every license except *None*.
7. Write the `<Name>.iceproject` manifest.
8. Add the project to **My Projects** and **open it in the editor straight away** —
   the launcher exits as it hands over, exactly as it does for an existing project
   ([3.7](#37-opening-a-project-the-hand-off-to-the-editor)). When you next start the
   launcher, the new project is waiting in the list.

The manifest the launcher writes looks like this:

```json
{
    "Name": "MyNewGame",
    "EngineVersion": "1.0.1",
    "StartScene": "",
    "ScriptingMode": "Code",
    "Description": "…",
    "LicenseType": "MIT",
    "LicenseDisplayName": "MIT",
    "LicenseFile": "LICENSE.txt",
    "LicenseStudio": "…",
    "Plugins": [],
    "Mods": ["PlatformerCrates"]
}
```

`Description`, `LicenseFile` and `LicenseStudio` are only written when they have a
value. The editor adds its own keys to the same file as you work.

#### The examples catalog — `Content/Examples/Examples.json`

The **Select Example** list is built from
`<install>/Content/Examples/Examples.json`. Each entry describes one folder that
sits directly inside `Content/Examples/`:

```json
{
    "Examples": [
        {
            "Folder": "Platformer",
            "Name": "2D Platformer",
            "NameKey": "example_platformer_name",
            "Description": "A ready-to-play 2D platformer: tilemap level, animated player…",
            "DescriptionKey": "example_platformer_description",
            "Scripting": "Code",
            "Platforms": ["Windows", "Linux", "macOS", "Web"],
            "Image": "platformer_example_thumbnail.png",
            "Author": "IceBoxCrew",
            "Version": "1.0.0",
            "Tags": ["2D", "Platformer", "Tilemap", "Lua"],
            "Plugins": [],
            "Mods": ["PlatformerCrates"],
            "SortOrder": 0,
            "Hidden": false
        }
    ]
}
```

| Field | Meaning |
| ----- | ------- |
| `Folder` | **Required.** The example's folder name inside `Content/Examples/`. A plain name only — no slashes, no backslashes, no colons, no `.`/`..`, and it may not start with a dot. `Path` is accepted as an alias. |
| `Name` | Display name. Either a plain string, or an object of `"lang": "text"` pairs (the current language wins, then `"en"`, then the first entry). Defaults to the folder name. `DisplayName` is accepted as an alias. |
| `NameKey` | Optional key from `Config/Languages/*.json`. When the key exists it wins over `Name` — use it for names you translate through the engine's own language files. |
| `Description` | Same two forms as `Name`. Shown in the details panel and as the tooltip in the list. `Summary` is accepted as an alias. |
| `DescriptionKey` | Optional localization key for the description, same rule as `NameKey`. |
| `Scripting` | `"Code"`, `"Visual"`, or `"Both"` (an array such as `["Code", "Visual"]` works too). This is what locks **Scripting Mode**. `ScriptingMode` / `ScriptingModes` are accepted as aliases. Missing or unrecognized ⇒ both modes allowed, with a note in the log. |
| `Platforms` | Target platforms the example is verified for: any of `Windows`, `Linux`, `macOS`, `Android`, `iOS`, `Web`, `Xbox`, plus the shorthands `"All"`, `"Desktop"`, `"Mobile"`, `"Console"`. `Platform` / `SupportedPlatforms` are accepted as aliases. Missing ⇒ all. Shown as badges with the platform you are running highlighted; if it is not listed, the panel says so. |
| `Image` | Preview image, relative to the example folder — or, failing that, to `Content/Examples/` itself (`png`, `jpg`, `jpeg`, `bmp`, `tga`). It must stay inside the examples tree: absolute paths and `..` are rejected. `Preview` is accepted as an alias. |
| `Author`, `Version`, `Tags` | Free-text metadata for the details panel. |
| `Plugins`, `Mods` | Engine-level packages (folder names under `Plugins/` and `Mods/`) that get ticked automatically when the example is picked. A single string works as well as an array. |
| `SortOrder` | Position in the list, lowest first. Defaults to the entry's position in the file. |
| `Hidden` | `true` keeps the example out of the launcher without deleting it — and without it reappearing as an undescribed folder. |

Both `Scripting` and `Platforms` accept generous spellings, so a catalog written by
hand rarely needs fixing: `lua`, `script`, `text`, `cpp` all mean *Code*;
`visual`, `graph`, `nodes`, `nodegraph`, `blueprint` all mean *Visual*; `any`,
`both`, `all`, `either`, `mixed`, `*` mean both. Platforms understand `win`,
`win64`, `pc`, `osx`, `darwin`, `mac`, `html5`, `wasm`, `webgl`, `emscripten` and
friends.

How the launcher reads the file:

* `//` and `/* … */` comments are allowed, and the file may also be written as a
  bare `[ … ]` array instead of the `{ "Examples": [ … ] }` object.
* An entry whose folder does not exist on disk is skipped, with the reason in the
  log. A malformed entry is skipped on its own — the rest of the catalog still loads.
* If the same folder is declared twice, only the **first** entry is used.
* If an image is declared but missing, unreadable or not an image, the launcher logs
  it and still falls back to auto-detection: `preview.png`, `preview.jpg`,
  `preview.jpeg`, `thumbnail.png` or `<Folder>.png` / `<Folder>.jpg` inside the
  example folder (capitalised variants included). With no image at all the preview
  box shows *No preview image*.
* A folder that exists but is **not** described in the catalog still shows up,
  tagged **Not in Examples.json**, with both scripting modes allowed, and sorted
  after every described example. Add an entry for it to control its mode,
  description and preview.
* If `Examples.json` is missing or cannot be parsed, the launcher falls back to
  listing the folders as they are — you never lose access to your examples.
* Folder names are matched case-insensitively, so a catalog authored on Windows
  keeps working on Linux and macOS; the log then asks you to fix the letter case.
* **Refresh Catalog** next to the list re-reads the file without restarting the
  launcher — and keeps your current selection if it survived the reload. That is
  what you want while authoring an example.

#### The bundled examples

A stock installation ships two examples, one per scripting mode. Both are verified
on Windows, Linux, macOS and Web, and both take their names and descriptions from
`Config/Languages/*.json` through `NameKey` / `DescriptionKey`, so the launcher
shows them in your own language.

**2D Platformer** (`Content/Examples/Platformer`, Code Scripting) is a tilemap level
with an animated player driven by a Lua character controller, a follow camera and a
bitmap-font HUD. Its catalog entry declares the **PlatformerCrates** mod, so ticking
the example also ticks that mod, which adds collectible crates and a score counter
on top. The mod doubles as a reference for asset bundling, runtime entity spawning
and level lifecycle hooks — see [Plugins & Mods](Plugins-And-Mods-EN-DOC.md).

**2D Top-Down** (`Content/Examples/TopDown`, Visual Scripting) is a tilemap maze with
solid walls, zero gravity and a character you walk around with WASD. Nothing in it is
hand-written Lua: `CL_TopDownPlayerActor`, `CL_TopDownMapActor` and the level script
are all node graphs, so it is the place to start reading if you picked Visual
Scripting. The player graph is the whole game loop in one **On Update** chain — read
the two axes, scale them by the `MoveSpeed` graph variable, push them into **Set
Velocity**, feed the animator's `Speed` parameter and flip the sprite through a
**Branch** — and the three comment boxes over it label each step. It reuses the
platformer's placeholder art (same player sheet and tileset), so the two examples can
sit side by side in a project without clashing.

### 3.4 Plugins & Mods

This tab manages which **plugins** and **mods** are attached to a project. Opening
it from a project's context menu (**Manage Plugins & Mods…**) pre-selects that
project; opening it from the sidebar uses whatever project is currently selected in
**My Projects**. With no project selected it just says so and offers a button back
to the list.

The tab reads the available packages from the engine's own `Plugins/` and `Mods/`
folders — every subfolder containing a `plugin.json` / `mod.json` — and shows them
as two columns of cards, each headed with a count (*Plugins (1)*, *Mods (1)*). A
card carries the package **icon** (`Icon` from the manifest, or `icon.png` /
`icon.jpg` / `icon.jpeg` picked up automatically, or a `?` placeholder), a
**checkbox**, the **name** (green when ticked) and **version**, the
**description**, the **Author**, and a **Show in File Browser** button.

Three buttons drive it:

| Button | What it does |
| ------ | ------------ |
| **Refresh Catalog** | Rescans the engine's `Plugins/` and `Mods/` folders and reloads the icons. |
| **Reload Selection** | Re-reads the ticks from the project's manifest on disk, discarding unapplied changes. |
| **Apply to Project** | Writes the ticks to the project and syncs the files. |

**Apply to Project** rewrites the `Plugins` and `Mods` arrays in the project's
`.iceproject`, then copies every newly ticked package into `<project>/Plugins/…` or
`<project>/Mods/…` and **removes the folder of every package you unticked**. Copying
skips build leftovers — the `CMakeFiles`, `build`, `out`, `.vs` and `.git` folders,
`CMakeLists.txt`, and `.obj` / `.o` / `.ilk` / `.exp` / `.lib` / `.a` / `.pdb`
files — and, for plugins, the `Source` folder, since a project only needs the
compiled binary. A plugin's `Binaries/` folder is copied whole, `.a` and `.pdb` files
included: it holds the libraries the Plugin Builder made for the game platforms
(Android, iOS, Web, Xbox), and a project that ships the plugin there needs them. If the
manifest cannot be parsed even after repair, nothing is touched and you are told the
apply failed.

When a project's manifest has no `Plugins` / `Mods` key at all (for example, one
written by hand), the ticks are seeded from whatever folders already exist inside the
project, so applying does not silently wipe packages the manifest does not list.

> Attaching a package here is about the **editor**. What actually ships with a
> built game is decided later, in the Build Game dialog's **Include Plugins** /
> **Include Mods** options — and a plugin whose manifest says
> `"EditorOnly": true` never ships at all.

> Authoring your own plugins and mods — their structure, manifests, lifecycle and
> shipping — is a topic of its own:
> [Plugins & Mods](Plugins-And-Mods-EN-DOC.md).

### 3.5 Settings

Launcher **Settings** carry three preferences and one read-only readout:

* **Language** — 15 built-in languages (English, Русский, Українська, 中文,
  العربية, हिन्दी, Español, Português, 日本語, Français, Deutsch, Italiano, Polski,
  עברית, 한국어), laid out three per row. The active language is highlighted; click
  another to switch instantly — no restart, and the whole window flips to a
  right-to-left layout for Arabic and Hebrew.
* **Font Settings** — pick any `.ttf`/`.otf` from `Config/Fonts/`, set the **Size**
  (8–72 px), then **Apply** (the button only appears once you change something).
  **Refresh Fonts** rescans the folder, and a warning replaces the list when
  `Config/Fonts/` is empty. The chosen face is merged with the Noto fonts in
  `Config/Fonts/` — Latin (extended letters included), Greek, Cyrillic, Hebrew,
  Arabic, Devanagari, Simplified Chinese, Japanese and Korean — and every glyph is
  rendered the first time it is drawn, so characters the chosen face lacks come from
  those fonts and text stays sharp at any size.
* **Theme** — **Dark** or **Light**, applied immediately across the whole launcher.
* **Active renderer** — read-only: the graphics API the launcher itself is drawing
  with, and the **GPU** behind it. On its very first run the launcher probes the
  platform chain from the top — Windows: Direct3D 12 → Vulkan → OpenGL 4.6 →
  OpenGL 3.3; Linux: Vulkan → OpenGL 4.6 → OpenGL 3.3; macOS: Metal →
  Metal (MoltenVK) → Metal (ANGLE); Android: Vulkan → OpenGL ES 3.2 → OpenGL ES 3.0 — and records the
  first one that answers in
  `Config/Engine.json` (the per-user copy when the install folder is read-only), so
  every later start goes straight to it. The Updater shows
  the same two lines in its own **Settings** panel and shares the same recorded
  choice. Full details are in
  [Graphics → Backends & platforms](Graphics-EN-DOC.md#22-backends--platforms).

**Language** and **Font** are written to the shared editor preferences
(`Editor.Language`, `Editor.FontName`, `Editor.FontSize`, `Editor.FontColor` in
`Config/Engine.json` under your user-data folder), so the editor and the updater
open with the same language, font and feel. **Theme**, together with your sort mode
and direction, is launcher-only and lives in `launcher_settings.json` next to it.

### 3.6 About

The **About IceBoxEngine** page shows the studio name, the **installed engine
version**, a short tagline, copyright, contact, and clickable links — **Website**
(`https://www.ice-box-crew.com/`), **Email** (`iceboxcrew057@gmail.com`), and
**Issues** (`https://github.com/IceBoxCrew/IceBoxEngine/issues`) — plus a summary of
the license. Links underline on hover, show their target as a tooltip, and open in
your browser or mail client. It is the quickest place to confirm exactly which
version you are running.

### 3.7 Opening a project: the hand-off to the editor

When you open a project — by double-click, by **Open Project**, by `Enter`, or right
after **Create Project** — the launcher:

1. Confirms the project folder still exists and still contains a `.iceproject`
   file. If either has gone missing, the launcher shows *Project no longer exists on
   disk and was removed from the list.* and drops the entry.
2. Records the project's **last-launched** time and saves the list.
3. Locates the editor (`IceBoxEngine`) **next to itself** in the install folder.
4. Launches the editor with the project path and turns its own window into the
   start-up splash — *Launching editor…* under the project name — until the editor's
   window has taken over (15 seconds at most), then **exits**. If the editor cannot be
   started, the launcher stays open and logs the failure.

Because the editor is resolved as a *sibling* of the launcher, the launcher and
editor always stay a matched pair from the same installation.

**The start-up splash.** The editor opens on the same splash and keeps it up while it
loads: the engine logo, the project name, a progress bar with a percentage, *Step N of
M*, and the current stage — *Loading editor…*, *Initializing renderer…*, *Compiling
shaders…* (with a running *Shaders ready* count), *Initializing engine systems…*,
*Initializing scripting…* (only in editors with Python scripting), *Loading level
<name>…* and *Preparing workspace…*. Keyboard, mouse, touch and pen input is ignored
until the splash fades into the editor, so nothing you press while it loads lands in a
half-built workspace. On Android, where the launcher and the editor are one app, the
hand-off happens inside that app and the splash simply continues.

---

## 4. The Updater

The updater is a single window whose whole job is to move you from the version you
have to the latest one published for your platform — safely, on the right
architecture, and with a clear report once it is done.

### 4.1 The window at a glance

Under the **IceBoxEngineUpdater** title and logo, the main view shows:

* A colored **result banner** at the very top, but only right after an update was
  installed: green **Update installed successfully** with the version you are now on,
  or red **Update was not installed** with the installer's exit code and the version
  you are still on. **OK** dismisses it. This is the window that opens by itself when
  an update finishes ([4.6](#46-per-platform-install-behavior)).
* A colored **Status** line — grey (idle), amber (working: checking / downloading /
  verifying / installing), green (update available), blue (up to date / complete),
  red (error).
* **Current version** (the version actually installed, or *Not set*) and, once a check
  finishes, **Latest version** (the newest published release).
* The detected **Platform** and **Architecture**, so you can see at a glance which
  installer the updater will pick for this machine.
* A **progress bar** during download and install.
* **Release Notes** for the available version, in a small scrollable box — shown
  only when an update is actually available and the release has notes.
* Two buttons: **Check for Updates** (disabled while a check is running) and
  **Install Update** (enabled only when an update is actually available).
* The error message in red underneath, when something went wrong.

A **Settings** button (bottom-left) toggles the settings screen, and **Exit**
(bottom-right) closes the updater.

### 4.2 How a check works

A check runs automatically on startup if **Check for updates on startup** is
enabled, and any time you press **Check for Updates**. Under the hood the updater:

1. Asks the IceBoxEngine update service for the list of published releases. The address is
   part of the installed engine, so there is nothing to configure.
2. Ignores **draft** releases and releases without a version tag, then sorts the rest
   by the version rules from [2.5](#25-the-engine-version-scheme) and takes the newest.
3. Compares that newest release against your **current version**:
   * newer → **New version available: `<tag>`** (the **Install Update** button
     lights up, and release notes appear);
   * same or older → **Engine is up to date**;
   * nothing published → **No releases found**.

The request verifies TLS, follows up to 10 redirects (HTTPS only), gives up on a
connection after 15 s and on the whole request after 30 s, and aborts a stalled
transfer. Transient failures — timeouts, dropped connections, HTTP
408/429/500/502/503/504 — are **tried up to three times in all**, with a growing
pause between attempts (0.5 s, then 1 s). Everything runs on a background thread, so the window stays
responsive.

Failures map to a specific message rather than a generic error:

| Situation | Message |
| --------- | ------- |
| The service refused the request | *Access to the update service was denied. Check your connection and try again; if it keeps happening, please report it to support.* |
| The service had nothing to offer | *The update service returned no releases.* |
| No response at all | *Failed to connect to the update service. Check your connection and try again.* |
| Any other HTTP status | *HTTP error `<code>`* |
| The reply was not valid JSON | *Failed to parse response: `<detail>`* |
| The installed engine carries no service address | *The update service address is missing from the engine configuration. Reinstall the engine to restore it.* |

The first five mean the same thing from your side: the update service could not be
reached or answered with something unusable. Check your internet connection and try
again; if it persists, report it to support with the message shown. The last one is
different — it means the installation itself is incomplete, and only reinstalling the
engine puts the address back.

### 4.3 Downloading & installing an update

Pressing **Install Update** runs a careful download-verify-install pipeline:

1. **Pick the right asset.** From the release's attached files, the updater scores
   each one for your **operating system and CPU architecture** and picks the best
   match. Only real installers are candidates — `.exe`/`.msi` on Windows,
   `.dmg`/`.pkg`/`.zip`/`.tar.gz` on macOS, `.deb`/`.AppImage`/`.tar.gz` on Linux —
   and anything named for another platform, or looking like debug symbols
   (`dbg`, `symbols`, `.pdb`), source (`-src`, `source`) or an uninstaller is
   rejected outright. Among the survivors, a name matching your architecture
   (`x64`/`amd64`/`x86_64`, `arm64`/`aarch64`/`apple-silicon`, `x86`/`i386`/`i686`)
   scores highest, a name advertising a *different* architecture is penalised, and
   the platform's preferred format wins the tie-break — `.exe` over `.msi`,
   `.dmg` over `.pkg`, `.deb` over `.AppImage` over a tarball. If nothing fits, you
   get *No installer for `<platform>` found in the release assets*.
2. **Download.** The staging folder `<temp>/IceBoxUpdate/` is wiped and recreated,
   and the asset is downloaded into it with a live progress bar. The asset's name is
   sanitised before it becomes a local file name, so a strange name published in a
   release can never escape the staging folder. A download that fails for a transient
   reason is tried up to three times in all (with a 1 s, then a 2 s pause), and a
   partial file is deleted rather than left behind.
3. **Verify.** The status changes to *Verifying download…*. A download that turns out
   to be a **web page** rather than an installer is rejected immediately with its own
   message. The file must then be at least 1 KB and must match the size the service
   reported for the asset, after which its **SHA-256** is computed and compared against
   the published hash. A mismatch aborts the install with a checksum error. (If no hash
   was published, verification is skipped gracefully and noted in the log.)
4. **Install and report back.** The verified installer runs for your platform
   ([4.6](#46-per-platform-install-behavior)). Because the updater has to get out of
   the way while its own files are replaced, it shows *Installer started. The updater
   will close and reopen when the installation finishes.*, closes, and the **newly
   installed updater opens again by itself** with the green banner **Update installed
   successfully**. If the installer failed instead — a declined UAC prompt, for
   instance — the updater reopens with the red banner and the installer's exit code,
   and your recorded version is left untouched so the update stays on offer.

> The updater never installs silently behind your back. Auto-check only *checks*;
> the actual download and install happen only when **you** press **Install
> Update**. Closing the window during a download cancels it and cleans up the
> staging folder.

### 4.4 Settings

The updater's **Settings** screen shows your **Current version**, the detected
**Platform** and **Architecture**, and the **Active renderer** with its **GPU** (the
same two lines as in the launcher, [3.5](#35-settings)). Below them you can set:

* **Check for updates on startup** — toggles the automatic check described in
  [4.2](#42-how-a-check-works).
* **Language** and **Font** — the same 15 languages and font controls as the
  launcher ([3.5](#35-settings)), shared with the rest of the engine.

**Save** writes the auto-check flag and closes Settings; **Cancel** closes Settings
without saving it. The language and font buttons apply immediately and are not
affected by Cancel.

### 4.5 Where the version is recorded

The version of the engine you have is kept in `Config/Updater.json` inside the
**install folder**, as `currentVersion` — that is the single value the launcher and
the editor display, and the baseline every update check compares against. A new
installation replaces the file, which is how the version bumps after an update.

Your own preferences — the auto-check flag and the version last installed — live in a
second copy of the same file inside your **user-data folder**
([6. Files & locations](#6-files--locations)). It is the only one the updater writes,
and it is written atomically, so an interrupted write cannot corrupt it.

The **installed** version always wins: if an install fails halfway, the recorded
version stays where it was and the update is offered again instead of being treated as
applied. Neither file is meant to be edited by hand — use the **Settings** screen.

### 4.6 Per-platform install behavior

The final install step is tailored to each OS:

| Platform | How the update is installed |
| -------- | --------------------------- |
| **Windows** | A small `run_installer.bat` shim is dropped next to the download and started detached and hidden; if Windows demands elevation it is relaunched through UAC. The updater shows *Installer started…* for a couple of seconds and **exits**, so its own files can be replaced. The shim waits about four seconds, then launches the **NSIS** installer silently into the same folder (`"<asset>" /S /D=<install dir>`) — or, for an `.msi`, runs `msiexec /i … /qn /norestart INSTALLDIR=…`. When the installer returns, the shim **starts the freshly installed updater again**: with `--post-update=<tag>` on success (exit code 0, or 3010 "reboot pending" for an `.msi`), or with `--update-failed=<tag> --update-code=<n>` on failure. Finally it deletes the temporary download folder. |
| **macOS** | A `.dmg` is mounted read-only on a temporary mount point, its `.app` is copied into place with `ditto` and unmounted again; a `.pkg` is installed with `installer -pkg … -target /` after an administrator prompt; a `.zip` is expanded with `ditto -x -k`; a `.tar.gz` is untarred. The quarantine flag is cleared so the app runs without a Gatekeeper warning. The install runs to completion inside the updater, which then **reopens the newly installed `IceBoxEngineUpdater.app`** with `--post-update=<tag>` and closes itself. |
| **Linux** | A `.deb` is installed with `dpkg -i` (falling back to `apt-get install -f` to pull dependencies); an `.AppImage` is copied to `<install>/IceBoxEngine.AppImage` and made executable; a `.tar.gz` is extracted into the install folder. Elevation is obtained through `pkexec`, then `sudo`, then a graphical `sudo -A` askpass helper. As on macOS, the updater then **reopens the newly installed `IceBoxEngineUpdater`** with `--post-update=<tag>` and closes itself. |

So the visible behaviour is the same on all three systems: the updater goes away for
a moment, and the **new** updater comes back on its own with a green banner naming
the version you are now on. Should reopening fail (the binary is missing, the desktop
session refuses to start it), the updater that ran the install stays open and shows
the same banner itself instead — the result is never left unreported. On macOS and
Linux the config records the new version as soon as the install returns; on Windows
it is the reopened updater that records it, after reading the version the installer
actually put on disk. In all cases the engine ends up upgraded in the **same install
folder**, and your projects, settings and language are untouched.

---

## 5. Typical workflows

**First run after installing.**
1. Open the **Launcher** from the desktop shortcut.
2. Go to **New Project**, name it, pick a location, choose a license and a scripting
   mode, optionally tick **Include Starter Content** and pick an example, then press
   **Create Project** — the editor opens on the new project right away.
3. Or use **Add Existing Project…** / **Scan All Drives…** / drag-and-drop to import
   projects you already have, and double-click one to open it.

**Day-to-day.**
Open the launcher, double-click the project you want, work in the editor. Pin your
active projects to the top, set a **Sort by** that suits you, and use `Ctrl` + `F`
when the list grows.

**Adding a package to an existing project.**
Right-click the project → **Manage Plugins & Mods…**, tick or untick packages, press
**Apply to Project**. Remember that unticking a package **deletes its folder** from
the project.

**Authoring your own example.**
1. Create `Content/Examples/<YourExample>/` in the install folder and put the
   project content in it.
2. Add an entry to `Content/Examples/Examples.json` with at least `Folder`, and
   ideally `Name`, `Description`, `Scripting`, `Platforms` and `Image`.
3. Drop a `preview.png` in the folder, or point `Image` at one.
4. In the launcher's **New Project** tab, tick **Include Starter Content** and press
   **Refresh Catalog** — your example appears without a restart. Anything the
   launcher did not like about the entry is explained in the log.

> The install folder belongs to the system (`Program Files`, `/opt/iceboxengine`,
> `/Applications/IceBoxEngine`), so changing it needs administrator rights — and an
> engine update replaces it. On Windows the previous version is uninstalled first, which
> removes everything in the folder, your own additions included; on every platform the
> update brings its own `Examples.json`. Keep the master copy of your example somewhere
> else and copy it back after updating.

**Moving to a newer engine build.**
1. Open the **Updater** — the launcher's amber **Updater** button, or Start Menu →
   *IceBoxEngineUpdater*.
2. It checks automatically (or press **Check for Updates**).
3. If it says **New version available**, read the **Release Notes** and press
   **Install Update**.
4. Let it download, verify and install. The updater closes while the installer runs
   and reopens by itself with the green **Update installed successfully** banner; open
   the **Launcher** again — the sidebar now shows the new version.

> If you opened the updater from the launcher's **Updater** button, close the
> launcher (and the editor) before pressing **Install Update**. The installer
> replaces the files of the whole installation, and it cannot overwrite programs
> that are still running.

---

## 6. Files & locations

**In the install folder**

| Path | What lives there |
| ---- | ---------------- |
| `<install>/IceBoxEngineLauncher`, `IceBoxEngineUpdater`, `IceBoxEngine` | The three programs, side by side. |
| `<install>/Config/Updater.json` | The canonical engine **version** the launcher and editor display. |
| `<install>/Config/Fonts/`, `Config/Languages/` | Fonts and the 15 language files shared by all three apps. |
| `<install>/Plugins/`, `Mods/` | Engine-level packages the launcher can attach to projects. |
| `<install>/Content/Examples/` | Starter content offered by **New Project**, described by `Examples.json` ([3.3](#33-new-project)). |
| `<install>/Documentation/` | This documentation set, also readable from the editor's Help panel. |

**In your user-data folder** — Windows: `%APPDATA%\IceBoxEngine` · Linux/macOS:
`~/.config/IceBoxEngine` (or `$XDG_CONFIG_HOME/IceBoxEngine`)

| File | What it holds |
| ---- | ------------- |
| `Launcher.json` | The remembered **project list**: name, path, modified/launched timestamps, pin flag and custom order. |
| `launcher_settings.json` | Launcher-only UI preferences: theme, sort mode, sort direction. |
| `Config/Engine.json` | The shared editor preferences — language, font name, size and color — used by launcher, updater and editor. |
| `Config/Updater.json` | The updater's working copy: auto-check preference and the last installed version. |
| `exported_projects.json` | Written by the bulk **Export List…** button. |

Crash reports of the launcher and the updater land in `Saved/CrashReports/` **inside the
install folder** when that folder is writable. A system-wide install usually is not, and
then they go to the per-user folder instead — `%APPDATA%\IceBoxEngine\game\Saved\CrashReports`
on Windows, `~/Library/Application Support/IceBoxEngine/game/Saved/CrashReports` on macOS
and `~/.local/share/IceBoxEngine/game/Saved/CrashReports` on Linux. The editor writes its
own reports into the open project's `Saved/CrashReports/`. Each folder keeps the newest 15
reports.

**In a project folder**

| Path | What it holds |
| ---- | ------------- |
| `<Name>.iceproject` | The project manifest — name, engine version, scripting mode, description, license, plugin and mod lists. |
| `Config/Engine.json` | Per-project editor and rendering settings, seeded from your preferences and from the renderer probe at creation. |
| `Config/Editor.json`, `Config/CollisionGroups.json`, `imgui.ini` | The editor's other per-project state — open panels and asset editors, Build Game settings, collision groups, the dock layout ([Editor 2.3](Editor-EN-DOC.md#23-panel-persistence)). |
| `Content/`, `Content/Examples/<Example>/` | Project content, including the copied starter example. |
| `Plugins/`, `Mods/` | Packages copied in by the launcher. |
| `LICENSE.txt` | Written at creation for every license except *None*. |
| `project_thumbnail.png`, `project_thumbnail_high.png` | 256 × 256 and 512 × 512 viewport snapshots written by the editor on exit; used as the launcher preview. |
| `Saved/` | Working data the editor keeps for the project: the asset redirectors in `Saved/Redirectors/` ([Assets 2.3](Assets-EN-DOC.md#23-asset-references--redirectors) — keep them), the shader and cook caches, cooked content and the editor's own crash reports. The caches and cooked content are rebuilt whenever they are missing. |
| `Tools/Helpers/Profiler/` | Traces recorded with the Advanced Profiler ([Profiling 4.9](Profiling-And-Building-EN-DOC.md#49-where-traces-live--chrome-trace-export)). |
| `Tools/PythonScripts/` | Your Python tool scripts, with `Startup/`, `Lib/` and `Modules/` — created by **Open Scripts Folder** in the Python Console's **Run Python Script** menu. Editor-only, never shipped with the game ([Python API → Tool script folders](PythonAPI-EN-DOC.md#tool-script-folders)). |

**Temporary**

| Path | What it holds |
| ---- | ------------- |
| `<temp>/IceBoxUpdate/` | Staging folder for an in-progress download and the Windows installer shim; wiped before each download and cleaned up afterwards. |

---

## 7. FAQ & troubleshooting

**The launcher shows the wrong / no version.**
The version comes from `<install>/Config/Updater.json` → `currentVersion`. If it is
empty or the file is missing, the sidebar/About fall back to the version compiled
into the binary. Running a successful update, or reinstalling, restores it.

**"Project no longer exists on disk and was removed from the list."**
The folder was moved or deleted, or its `.iceproject` file is gone. The launcher
removes the stale entry automatically — re-add it with **Add Existing Project…**.

**I removed a project by accident.**
If you used **Remove from List** (or the bulk version of it), nothing was deleted —
just **Add Existing Project…** and point at the folder again. **Delete Project**
*does* erase files and cannot be undone.

**I can't drag projects into a custom order.**
Dragging only works with **Sort by → Custom**, an empty search box, and on rows that
are not pinned. Pinned projects are always held at the top.

**My project has no picture in the list.**
Thumbnails are written by the **editor** when it closes. Open the project once and
close it, and `project_thumbnail.png` appears in the project folder.

**My example doesn't show up in New Project.**
The folder has to sit directly inside `<install>/Content/Examples/` and the entry's
`Folder` must be a plain name that matches it. Press **Refresh Catalog** and check
the log — every skipped entry says why. A folder with no catalog entry still appears,
tagged *Not in Examples.json*.

**Applying plugins & mods deleted a folder from my project.**
That is by design: **Apply to Project** makes the project match the ticks exactly,
so an unticked package is removed. Use **Reload Selection** first if you are unsure
what the project currently has.

**The updater closed by itself in the middle of an update.**
That is the normal hand-off: the updater cannot replace its own running files, so it
steps aside while the installer works and the **new** updater opens again a few
seconds later with a green banner naming the version you are now on
([4.6](#46-per-platform-install-behavior)). Nothing is lost if you close that window;
the engine is already updated.

**The updater reopened with a red "Update was not installed" banner.**
The installer ran but did not finish — most often because an elevation prompt was
declined (Windows reports code 740 for that). Your recorded version was deliberately
left alone, so simply press **Install Update** again and allow the prompt.

**The updater cannot reach the update service.**
The messages in [4.2](#42-how-a-check-works) all come down to the same thing: the
service did not answer, or answered with something the updater could not read. Check
your internet connection (and any proxy or firewall) and press **Check for Updates**
again. If it keeps failing, send the exact message to support.

**"No installer for … found in the release assets."**
The published release carries no file matching your OS and CPU architecture. Wait for
a complete release, or ask support for the installer for your platform.

**"Checksum verification failed."**
The download was corrupted in transit — its SHA-256 did not match the published hash.
Simply press **Install Update** again; nothing was installed.

**"The download server returned a web page instead of the installer."**
The download host answered with an HTML page instead of a file — usually a captive
portal (hotel/office Wi-Fi sign-in) or a temporary outage on the download host. Try
again on a normal connection, and report it if it persists.

**Does updating wipe my projects or settings?**
No. Updates replace engine files in the install folder only. Your projects live in
their own folders, and your language/font/theme and project list live in the user
data folder. The one thing an update does not keep is what you added to the **install
folder** yourself — an example in `Content/Examples/`, a package dropped into the
engine's own `Plugins/` or `Mods/`: on Windows the old installation is removed as a whole
before the new one goes in. Keep such files outside the install folder and copy them back
afterwards.

**Can I just run the editor directly?**
Yes — double-clicking a `.iceproject` opens it in the editor. But going through the
launcher is recommended: it tracks recent projects, manages packages, and keeps the
editor/launcher pair matched.

---

<sub>© IceBoxCrew Studio. All rights reserved. See [`LICENSE.txt`](../../LICENSE.txt) for full terms.</sub>
