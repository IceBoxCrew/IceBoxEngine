<p align="center">
  <img src="logoIceBox.png" alt="IceBox Engine Logo" width="200">
</p>

<h1 align="center">🧊 IceBox Engine™</h1>

<p align="center">
  <strong>🇬🇧 English</strong> &nbsp;•&nbsp; <a href="README.ru.md">🇷🇺 Русский</a>
</p>

<p align="center">
  <strong>A powerful, modular 2D game engine built with modern C++ and open-source libraries</strong>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/C%2B%2B-26-blue?style=for-the-badge&logo=cplusplus" alt="C++26">
  <img src="https://img.shields.io/badge/CMake-4.3%2B-064F8C?style=for-the-badge&logo=cmake" alt="CMake 4.3+">
  <img src="https://img.shields.io/badge/vcpkg-Managed-purple?style=for-the-badge" alt="vcpkg">
  <img src="https://img.shields.io/badge/License-Proprietary-red?style=for-the-badge" alt="Proprietary">
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Platform-Windows%20%7C%20Linux%20%7C%20macOS-lightgrey?style=flat-square" alt="Platform">
  <img src="https://img.shields.io/badge/Runtime-Windows%20%7C%20Linux%20%7C%20macOS%20%7C%20iOS%20%7C%20Android%20%7C%20Web-green?style=flat-square" alt="Runtime">
</p>

---

## 🧊 About

IceBox Engine is a cross-platform 2D game engine designed for creating games of any visual style — from simple pixel-art projects to high-resolution 4K HD 2D games with rich visual effects. The engine includes a full-featured visual editor, a project launcher, an automatic updater, and a lightweight runtime for shipping finished games to players.

**Scripting:**
- **Lua** — game scripting language for gameplay classes, UI, levels and more
- **Python** — engine-side scripting for editor tools and automation

---

## ✨ Features

- **Rendering** — Data-driven 2D renderer with a node-based **material editor** (instances & functions), **decals** (bullet holes, blood splatter, scorch marks), post-processing / **FX**, and a multi-backend RHI: OpenGL 3.3/4.6, OpenGL ES 3.0/3.2, Vulkan 1.1-1.4, Direct3D 12, native Metal (plus Metal via ANGLE & MoltenVK), WebGL 2.0 and WebGPU.
- **Scenes & ECS** — Entity-Component-System core (EnTT), level outliner, reusable entity classes, and a property/world editor.
- **2D Physics** — Rigid bodies, colliders, and joints powered by Box2D, with a multithreaded solver (enkiTS) on desktop and mobile.
- **Sprites & Tilemaps** — Sprite editor, spritesheet slicer, flipbook animation, and dedicated tilemap / tileset editors.
- **Animation** — Skeletal animation, flipbooks, and timeline-driven clips.
- **Text & UI** — In-engine UI widgets and high-quality text via FreeType, HarfBuzz, and SheenBidi (full Unicode shaping with right-to-left support).
- **Audio** — Mixing and playback with Opus / Vorbis codec support.
- **Scripting** — Lua gameplay scripting with an integrated debugger, a **visual node-graph** editor, and Python for editor tooling.
- **AI** — Pathfinding, behaviour trees, and navigation.
- **Networking** — Reliable UDP (ENet) plus WebSocket transport (IXWebSocket) for browser/server play, with cryptography via libsodium.
- **Video** — Video playback and a cinematic / cutscene editor (FFmpeg).
- **Localization** — 14 built-in editor languages with right-to-left support and game localization editable from the localization panel.
- **Extensibility** — Drop-in **plugin** system and **mod** support.
- **Tooling** — Built-in Tracy profiler, stats overlays, a hot-key reference, and a one-click **Build Game** pipeline targeting every supported platform.

---

## 🎮 Platform Support

| Platform | Development | Runtime |
|----------|:-----------:|:-------:|
| **Windows** | ✅ | ✅ |
| **Linux** | ✅ | ✅ |
| **macOS** | ✅ | ✅ |
| **iOS** | ❌ | ✅ |
| **Android** | ❌ | ✅ |
| **Web** | ❌ | ✅ |

---

## 🏗️ Architecture

IceBox Engine consists of several components:

| Component | Binary | Description |
|-----------|--------|-------------|
| **Launcher** | `IceBoxLauncher` | Entry point for users. Manages projects (create, open, delete), checks for engine updates, and launches the editor for the selected project. |
| **Editor** | `IceBoxEngine` | The main visual editor. Scene editing, asset management, tilemap editor, animation tools, scripting workspace, and game build pipeline (Tools → Build Game). |
| **Updater** | `IceBoxUpdater` | Standalone update app. Checks GitHub releases (or an update manifest) for a newer engine version, then downloads, verifies and installs it — always on your explicit confirmation, never silently — and reopens itself afterwards to report the result. |
| **Runtime** | `IceBoxRuntime` | Lightweight, editor-free executable shipped with built games. Runs the game project directly on the target platform. |

---

## 🚧 Project Status

**IceBox Engine is under active development.** Core systems, editor tools, and architecture are evolving continuously with new features and improvements.

---

## 💻 System Requirements

### Engine & Editor (Desktop)

| | |
|-|-|
| **OS** | Windows 10+ (x64/x86/arm64), Linux — Ubuntu 22.04+ / Debian 12+ (x64/x86/arm64) or macOS 11.0+ (Apple Silicon or Intel) |
| **CPU** | Dual-core processor |
| **RAM** | 4 GB |
| **GPU** | OpenGL 3.3/4.6, Vulkan 1.1-1.4 or Direct3D 12 (feature level 11_0) compatible (Windows / Linux) or Metal-capable GPU (macOS — native Metal, ANGLE or MoltenVK), 512 MB VRAM |
| **Disk** | 10-20 GB free space |

### Runtime — iOS

| | |
|-|-|
| **OS** | iOS 14.0+ (iPhone & iPad, arm64) |
| **GPU** | Metal (native Metal renderer, or rendered via MoltenVK) |

### Runtime — Android

| | |
|-|-|
| **OS** | Android 7.0+ (API 24) |
| **GPU** | OpenGL ES 3.2/3.0 or Vulkan 1.1-1.4|

### Runtime — Web

| | |
|-|-|
| **Browser** | Any modern browser with WebGL 2.0 or WebGPU support |

---

## 📦 Build Requirements

Building a game runs the native toolchain of the target platform on your machine: the engine ships the SDK headers and the pre-built cores, you supply the compiler.

### Base tools (every target)

| | |
|-|-|
| **Compiler** | Visual Studio 2026+ (MSVC — the "Desktop development with C++" workload) on Windows, latest GCC / Clang on Linux, or Apple Clang (Xcode 15+) on macOS |
| **Build tools** | CMake 4.3+ and Ninja (Xcode on iOS) |
| **Package manager** | [vcpkg](https://github.com/microsoft/vcpkg) — a **full** clone (a `--depth 1` clone cannot resolve the `builtin-baseline` commit pinned in `vcpkg.json`), recent enough to contain that commit. An existing clone from before it was pinned needs `git pull` **and** a re-run of `bootstrap-vcpkg` — pulling alone leaves the old `vcpkg` binary behind, which then fails with `document schema version 2 is not supported` |
| **Optional** | NSIS (Windows game installers), `dpkg-deb` (Linux `.deb` packages), ImageMagick (higher-quality icon conversion) |

### To build games (Tools → Build Game)

| Target platform | Additional requirements |
|-----------------|----------------------|
| 🪟 **Windows** | *(nothing extra — same tools as above)* |
| 🐧 **Linux** | WSL2 (if building from Windows) or native GCC/Clang + Ninja |
| 🍎 **macOS** | macOS host with Xcode 15+ Command Line Tools, Python 3.12+ **with development headers** (Homebrew — the bundled system Python 3.9 is too old), vcpkg with `arm64-osx` / `x64-osx` triplets. Building the x86_64 (Intel) target **on an Apple Silicon host** additionally needs Rosetta 2 — `softwareupdate --install-rosetta --agree-to-license` — because CPython's `configure` and CMake's `FindPython` execute freshly built x86_64 binaries |
| 📱 **iOS** | macOS host with Xcode 15+ (full IDE, not just CLI tools), vcpkg with `arm64-ios` triplet, Apple Developer account **only** for on-device deployment (compiling needs no account) |
| 🤖 **Android** | Android SDK 37+, NDK 29+, Java JDK 25+, Gradle 9.7.0 *(auto-downloaded)* |
| 🌐 **Web** | [Emscripten SDK](https://emscripten.org/) — plus Node.js 24+ if you build the `wasm64` memory model |

---

## 🚀 Getting Started

### 1. Install the engine

Run the installer for your platform: an NSIS `…-Setup.exe` on Windows, a `…-Setup.deb` on Linux (installs into `/opt/iceboxengine`), or a `…-Setup.pkg` on macOS (installs into `/Applications/IceBoxEngine`). Every installer carries the Launcher, the Editor, the Updater and the Runtime, together with the documentation, the example projects and the SDK used to build games.

### 2. Launch and activate

Start **IceBox Launcher** — from the desktop shortcut, the Start-menu / application-menu entry, or `IceBoxLauncher` in the install folder. On a computer that is not activated yet the launcher opens the activation screen: paste your license key and press **Activate**. That is a one-off step per computer; afterwards the launcher, the editor and the updater all open straight away.

### 3. Create a project

In the Launcher, open the **New Project** tab, choose a name, location and license, then press **Create Project**. The Launcher sets up the project structure and opens the **Editor**.

### 4. Build your game

In the Editor, go to **Tools → Build Game**, select the target platform (Windows, Linux, macOS, iOS, Android, or Web), and build. The output is a ready-to-distribute package with the Runtime included — and, when you ask for one, an NSIS `.exe`, a `.deb` package or a macOS `.pkg` / `.dmg` installer next to it.

Step-by-step walkthroughs: **[Getting Started](Documentation/EN/Getting-Started-EN-DOC.md)** for the installer, the Launcher and the Updater; **[Profiling & Building Games](Documentation/EN/Profiling-And-Building-EN-DOC.md)** for every build option and platform setting.

---

## ⚙️ Quick Setup

### Windows

```bash
# Install vcpkg
git clone https://github.com/microsoft/vcpkg C:\dev\vcpkg
C:\dev\vcpkg\bootstrap-vcpkg.bat
set VCPKG_ROOT=C:\dev\vcpkg
```

> Building a game needs no Developer Command Prompt: the build script locates the Visual
> Studio installation with `vswhere` and calls `vcvarsall.bat` for the target architecture
> itself.

### Linux / WSL2

```bash
# 1. Install all system dependencies (one command)
sudo apt update && sudo apt install -y \
    build-essential cmake ninja-build git curl zip unzip tar pkg-config nasm xdg-utils squashfs-tools \
    autoconf autoconf-archive automake libtool \
    python3-dev python3-venv \
    rsync gdb nsis imagemagick \
    libx11-dev libxft-dev libxext-dev libxrandr-dev libxcursor-dev libxi-dev libxfixes-dev libxss-dev libxtst-dev \
    libxkbcommon-dev libwayland-dev wayland-protocols libdecor-0-dev \
    libibus-1.0-dev \
    libgl1-mesa-dev libegl1-mesa-dev libgles2-mesa-dev \
    libasound2-dev libpulse-dev \
    libdbus-1-dev \
    libssl-dev zenity libespeak-ng-dev \
    mingw-w64 g++-mingw-w64

# 2. CMake 4.3+ — what the game build's cmake_minimum_required asks for. Debian and
#    Ubuntu, the usual WSL2 images included, still package an older one, so check
#    `cmake --version` first and skip this step if apt already gave you 4.3+.
ICE_CMAKE_VERSION=4.4.2
curl -fsSLO https://github.com/Kitware/CMake/releases/download/v$ICE_CMAKE_VERSION/cmake-$ICE_CMAKE_VERSION-linux-x86_64.tar.gz
sudo tar -xzf cmake-$ICE_CMAKE_VERSION-linux-x86_64.tar.gz -C /opt
sudo ln -sf /opt/cmake-$ICE_CMAKE_VERSION-linux-x86_64/bin/{cmake,cpack,ctest} /usr/local/bin/
cmake --version

# 3. Install vcpkg
git clone https://github.com/microsoft/vcpkg ~/vcpkg
~/vcpkg/bootstrap-vcpkg.sh
echo 'export VCPKG_ROOT=~/vcpkg' >> ~/.bashrc && source ~/.bashrc

# Optional: cross-building arm64 from this x86_64 host needs its own toolchain and
# arm64 libraries - see "arm64 (AArch64) cross builds" below
```

#### 32-bit (x86) builds

The steps above cover the default 64-bit games. A 32-bit Linux game — **Tools → Build
Game → Linux** with the x86 architecture, or `build_linux.sh --arch x86` — is compiled
with `-m32`, which additionally needs the **i386 multiarch** enabled plus the multilib
toolchain and i386 copies of the development libraries. The tools installed above (CMake,
Ninja, git, NSIS, etc.) are architecture-neutral and are **not** reinstalled — only the
compiler and the linkable `:i386` libraries:

```bash
# 1. Enable the i386 architecture and refresh the package lists
sudo dpkg --add-architecture i386
sudo apt update

# 2. Install the 32-bit toolchain and the i386 development libraries.
sudo apt install --no-remove \
    gcc-multilib g++-multilib \
    libx11-dev:i386 libxft-dev:i386 libxext-dev:i386 libxrandr-dev:i386 libxcursor-dev:i386 libxi-dev:i386 libxfixes-dev:i386 libxss-dev:i386 libxtst-dev:i386 \
    libxkbcommon-dev:i386 libwayland-dev:i386 libdecor-0-dev:i386 \
    libibus-1.0-dev:i386 \
    libgl1-mesa-dev:i386 libegl1-mesa-dev:i386 libgles2-mesa-dev:i386 \
    libasound2-dev:i386 libpulse-dev:i386 \
    libdbus-1-dev:i386 \
    libssl-dev:i386 libespeak-ng-dev:i386

```

#### arm64 (AArch64) cross builds

On a **native AArch64 machine** nothing below is needed: the system compiler and the
system libraries are already arm64, so the arm64 target and the Build Game scripts work
out of the box.

Cross-building from an x86_64 host needs the AArch64 toolchain **and** arm64 copies of the
development libraries — the same shape as the i386 set above. A game links SDL3 against the
system audio and display stack (PulseAudio, Wayland, EGL, xkbcommon, libdecor), which vcpkg
does not ship. Without the `:arm64` copies the AArch64 link reaches into
`/usr/lib/x86_64-linux-gnu` and fails with `file in wrong format`:

```bash
# 1. Enable the arm64 architecture and refresh the package lists
sudo dpkg --add-architecture arm64
sudo apt update

# 2. Install the AArch64 toolchain and the arm64 development libraries.
sudo apt install crossbuild-essential-arm64
sudo apt install --no-remove \
    libx11-dev:arm64 libxft-dev:arm64 libxext-dev:arm64 libxrandr-dev:arm64 libxcursor-dev:arm64 libxi-dev:arm64 libxfixes-dev:arm64 libxss-dev:arm64 libxtst-dev:arm64 \
    libxkbcommon-dev:arm64 libwayland-dev:arm64 libdecor-0-dev:arm64 \
    libibus-1.0-dev:arm64 \
    libgl1-mesa-dev:arm64 libegl1-mesa-dev:arm64 libgles2-mesa-dev:arm64 \
    libasound2-dev:arm64 libpulse-dev:arm64 \
    libdbus-1-dev:arm64 \
    libssl-dev:arm64 libespeak-ng-dev:arm64
```

> `crossbuild-essential-arm64` **removes** `gcc-multilib` / `g++-multilib`, because the
> multilib `/usr/include/asm` symlink is wrong for every cross compiler and apt refuses to
> keep both. One host therefore cross-builds either x86 or arm64, not both — swap the
> packages between runs, or keep two machines.

#### Windows game builds from a Linux host (MinGW-w64)

`build_windows.sh` cross-compiles a Windows game from Linux with MinGW-w64 and links the
MinGW cores that ship with the engine, so no Windows machine is needed for that target.

`mingw-w64 g++-mingw-w64` from the dependency command above covers **x64 and x86**. There is
no `aarch64-w64-mingw32` compiler in any distribution repository, so **arm64 needs
[llvm-mingw](https://github.com/mstorsjo/llvm-mingw/releases)**, which provides the
`aarch64-w64-mingw32-*` drivers (clang behind the gcc/g++ names) plus `dlltool` and
`windres`:

```bash
# 1. Unpack a release next to the other toolchains. Pick the ucrt-ubuntu-*-x86_64 build.
curl -fsSLO https://github.com/mstorsjo/llvm-mingw/releases/download/20260616/llvm-mingw-20260616-ucrt-ubuntu-22.04-x86_64.tar.xz
tar -xJf llvm-mingw-20260616-ucrt-ubuntu-22.04-x86_64.tar.xz -C ~
mv ~/llvm-mingw-20260616-ucrt-ubuntu-22.04-x86_64 ~/llvm-mingw

# 2. Put it on PATH for this shell only, then build the arm64 game
export PATH="$HOME/llvm-mingw/bin:$PATH"
```

> Add llvm-mingw to `PATH` **only in the shell that builds the arm64 game**, not to
> `~/.bashrc`. Its `bin/` also carries `clang`, `ld.lld` and the `llvm-*` tools, plus its own
> `x86_64-` and `i686-w64-mingw32-` drivers; left on `PATH` permanently they shadow the
> system ones and quietly build the x64 and x86 targets with clang instead of GCC.

`dlltool` has to be present, not just the compiler: FFmpeg is LGPL-2.1 and ships as a
replaceable DLL, so it is built shared, and `dlltool` is what turns its export tables into
MinGW import libraries. Without it the vcpkg `ffmpeg` port fails while installing
`avdevice`.

### macOS / iOS build tools (Apple host required)

> **Note:** macOS and iOS targets must be built **on a macOS host**. Windows / Linux machines cannot cross-compile to Apple platforms because Apple's SDKs (Metal, UIKit, Cocoa) and `xcodebuild` are macOS-only.

```bash
# 1. Install Xcode (full IDE from the Mac App Store), accept the license and
#    run its first-launch setup. Verify with: xcodebuild -checkFirstLaunchStatus
sudo xcodebuild -license accept
sudo xcodebuild -runFirstLaunch

#    A fresh Xcode ships the iOS SDK but NOT the full iOS platform support.
#    Without it `ibtool` fails to compile LaunchScreen.storyboard with
#    "iOS <version> Platform Not Installed". This also installs the iOS
#    Simulator runtime, which lets you test iOS games without a device.
#    (~9 GB download, no sudo required.)
xcodebuild -downloadPlatform iOS

# 2. Install command-line dependencies
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
brew install cmake ninja pkg-config autoconf automake libtool autoconf-archive nasm python@3.13 imagemagick

# 3. Install vcpkg. Use a FULL clone - a shallow (--depth 1) clone cannot
#    resolve the builtin-baseline commit pinned in vcpkg.json.
git clone https://github.com/microsoft/vcpkg ~/vcpkg
~/vcpkg/bootstrap-vcpkg.sh
echo 'export VCPKG_ROOT="$HOME/vcpkg"' >> ~/.zprofile && source ~/.zprofile

```

> **MoltenVK needs no extra step.** Vulkan-over-Metal ships with the engine in
> `Tools/BuildSystem/Vendor/MoltenVK`, and on macOS the vcpkg `apple-moltenvk` feature
> provides `libMoltenVK.dylib` as a fallback — there is nothing to fetch by hand.

### Android build tools (Windows)

```bash
# 1. Install Android SDK + NDK 29 via Android Studio or command-line tools
# 2. Install Java JDK 25+
# 3. Set environment variables:
set ANDROID_HOME=C:\Users\%USERNAME%\AppData\Local\Android\Sdk
set ANDROID_NDK_ROOT=%ANDROID_HOME%\ndk\29.0.14206865
set JAVA_HOME=C:\Program Files\Java\jdk-25

# Gradle 9.7.0 is downloaded automatically by the build script if not installed.
```

### Android build tools (Linux / WSL2)

```bash
# 1. Install Java JDK 25+
sudo apt update && sudo apt install -y openjdk-25-jdk
export JAVA_HOME=/usr/lib/jvm/java-25-openjdk-amd64

# 2. Install Android SDK command-line tools
mkdir -p ~/Android/Sdk/cmdline-tools
cd ~/Android/Sdk/cmdline-tools
curl -fL -o tools.zip https://dl.google.com/android/repository/commandlinetools-linux-15859902_latest.zip
unzip -q tools.zip && rm -rf latest && mv cmdline-tools latest && rm tools.zip

# 3. Install SDK components & NDK 29
export ANDROID_HOME=~/Android/Sdk
export PATH=$PATH:$ANDROID_HOME/cmdline-tools/latest/bin
yes | sdkmanager --licenses
sdkmanager "platform-tools" "platforms;android-37.0" "build-tools;37.0.0" "ndk;29.0.14206865"
export ANDROID_NDK_ROOT=$ANDROID_HOME/ndk/29.0.14206865

# Gradle 9.7.0 is downloaded automatically by the build script if not installed.

# 4. (Optional) Persist environment variables
echo 'export JAVA_HOME=/usr/lib/jvm/java-25-openjdk-amd64' >> ~/.bashrc
echo 'export ANDROID_HOME=~/Android/Sdk' >> ~/.bashrc
echo 'export ANDROID_NDK_ROOT=$ANDROID_HOME/ndk/29.0.14206865' >> ~/.bashrc
source ~/.bashrc
```

### Android build tools (macOS)

Android games build from a macOS host just like from Linux — the Android SDK,
NDK and Gradle are all cross-platform. Use a JDK from Homebrew (a formula, not a
cask) so no `sudo` / admin password is needed.

```bash
# 1. Install JDK 25 (Homebrew formula installs into /opt/homebrew, no sudo).
#    Use exactly openjdk@25 - the Android build.gradle targets Java 25, and the
#    newer default `openjdk` (26) breaks the Android Gradle Plugin's jlink step.
brew install openjdk@25
export JAVA_HOME="$(brew --prefix openjdk@25)/libexec/openjdk.jdk/Contents/Home"

# 2. Install the Android command-line tools
brew install --cask android-commandlinetools    # provides `sdkmanager`

# 3. Install SDK components & NDK 29 into the macOS-default SDK location
export ANDROID_HOME="$HOME/Library/Android/sdk"
yes | sdkmanager --sdk_root="$ANDROID_HOME" --licenses
sdkmanager --sdk_root="$ANDROID_HOME" \
    "platform-tools" "platforms;android-37.0" "build-tools;37.0.0" "ndk;29.0.14206865"
export ANDROID_NDK_ROOT="$ANDROID_HOME/ndk/29.0.14206865"

# Gradle 9.7.0 is downloaded automatically by the build script if not installed.

# 4. (Optional) Persist environment variables
{
  echo "export JAVA_HOME=\"$(brew --prefix openjdk@25)/libexec/openjdk.jdk/Contents/Home\""
  echo 'export ANDROID_HOME="$HOME/Library/Android/sdk"'
  echo 'export ANDROID_NDK_ROOT="$ANDROID_HOME/ndk/29.0.14206865"'
} >> ~/.zprofile && source ~/.zprofile

```

### Web build tools (Windows)

> **`wasm64` needs Node.js 24 or newer.** emsdk still bundles Node 22, and Emscripten's
> MEMORY64 output refuses to start on it, so every configure check that runs a compiled
> program fails — vcpkg's `libsodium` is the usual first casualty, with `cannot run C
> compiled programs` and configure exit code 77. The build scripts search `PATH`, nvm, Volta,
> fnm and asdf for a newer Node and hand it to Emscripten through `EM_NODE_JS`, so installing
> one anywhere is enough. `wasm32` builds fine on the bundled Node.

```bash
# Install Emscripten SDK
git clone https://github.com/emscripten-core/emsdk.git C:\dev\emsdk
cd C:\dev\emsdk
.\emsdk install latest
.\emsdk activate latest
```

### Web build tools (Linux / WSL2)

> **`wasm64` needs Node.js 24 or newer** — see the note under *Web build tools (Windows)*.
> Debian and Ubuntu package an older one, so install it separately if `node --version` is
> below 24; nvm is enough, the scripts pick it up on their own.

```bash
# 1. Install Emscripten SDK
git clone https://github.com/emscripten-core/emsdk.git ~/emsdk
cd ~/emsdk
./emsdk install latest
./emsdk activate latest
source ./emsdk_env.sh

# 2. (Optional) Persist environment
echo 'source ~/emsdk/emsdk_env.sh' >> ~/.bashrc
```

### Web build tools (macOS)

> **`wasm64` needs Node.js 24 or newer** — see the note under *Web build tools (Windows)*.
> `brew install node` covers it.

```bash
# 1. Install Emscripten SDK. emsdk needs Python >= 3.10; the system Python is
#    3.9, so point emsdk at the Homebrew Python (see the macOS build section).
brew install python@3.13
git clone https://github.com/emscripten-core/emsdk.git ~/emsdk
cd ~/emsdk
export EMSDK_PYTHON="$(brew --prefix python@3.13)/bin/python3.13"
"$EMSDK_PYTHON" emsdk.py install latest
"$EMSDK_PYTHON" emsdk.py activate latest

# 2. (Optional) Persist environment
echo 'source ~/emsdk/emsdk_env.sh' >> ~/.zprofile
```

---

## 📖 Documentation

The full technical documentation ships with the engine in **[`Documentation/`](Documentation)**, in English (`EN/`) and Russian (`RU/`). It is also readable inside the editor through **Help → Documentation**, with full-text search and a table-of-contents sidebar.

Start here: **[Documentation/README.md](Documentation/README.md)** — an index of every document with a short description of what is inside.

| Document | What's inside |
|----------|---------------|
| [Getting Started](Documentation/EN/Getting-Started-EN-DOC.md) | Installing the engine, the Launcher & Updater, creating and managing projects. |
| [Editor & Interface](Documentation/EN/Editor-EN-DOC.md) | Every panel, menu, dialog, preference and shortcut. |
| [Graphics, Rendering & Physics](Documentation/EN/Graphics-EN-DOC.md) | RHI & backends, render graph, batching, lighting, 2D shadows, GI, post-processing, physics. |
| [Engine, Multiplayer & Physics](Documentation/EN/Engine-EN-DOC.md) | Runtime architecture, split-screen & online multiplayer, advanced physics, audio and input. |
| [Assets & Content Browser](Documentation/EN/Assets-EN-DOC.md) | Every asset type, its editor, sidecars, importers and cooking. |
| [Profiling & Building Games](Documentation/EN/Profiling-And-Building-EN-DOC.md) | Profilers, statistics, building for all six platforms, installers, DLC, headless servers. |
| [Plugins & Mods](Documentation/EN/Plugins-And-Mods-EN-DOC.md) | Native C++ plugins and Lua/content mods. |
| [Lua API](Documentation/EN/LuaAPI-EN-DOC.md) | The complete gameplay scripting reference. |
| [Python API](Documentation/EN/PythonAPI-EN-DOC.md) | Editor automation and tooling. |

Russian versions of all of the above live next to them in **[`Documentation/RU/`](Documentation/RU)**.

---

## 🔒 Privacy

The editor, launcher and updater send only two things, and only when you ask them to: a crash report, if you press **Send** in the crash dialog, and a single license activation check, when you press **Activate**. The updater additionally asks the release manifest whether a newer version exists when it starts — that carries nothing about you, and it can be switched off. No telemetry, no analytics, no background reporting. A packaged game does none of it — it never checks a license, never contacts us, and reports a crash only to an endpoint you configure yourself.

What is transmitted, why, how long it is kept and what your rights are: **[PRIVACY_NOTICE.txt](PRIVACY_NOTICE.txt)**

---

## 📚 Third-Party Libraries

IceBox Engine uses a number of open-source third-party libraries, each distributed under its own license (MIT, zlib, BSD-3-Clause, Apache-2.0, ISC, FreeType/FTL, SIL OFL for the bundled fonts, and LGPL-2.1 for FFmpeg, which is dynamically linked so it can be replaced freely and is not part of iOS or Web builds). Games you ship for iOS and Web contain no copyleft component at all.

Full list of libraries and their licenses: **[THIRD_PARTY_NOTICES.txt](THIRD_PARTY_NOTICES.txt)**

---

## 🤝 Code of Conduct

The engine's source is closed, but the repository, the issues, the discussions and the mailbox are open — and the same rules hold for everyone in them, us included.

- **Criticism is not misconduct.** A bad review, an argument that a design decision is wrong, a complaint about the price — we do not hide, lock or ban over any of it.
- **Behavior is.** Harassment, threats, doxxing, slurs, pile-ons and spam are acted on, at whatever level fits.
- **Your license is never a lever.** No sanction here revokes a Key, ends a license, stops your updates or touches a game you have shipped.
- **Report it by email** — [iceboxcrew057@gmail.com](mailto:iceboxcrew057@gmail.com) with `CONDUCT` in the subject. English or Russian.

Who it covers, where it applies, how reports are handled and what enforcement looks like: **[CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md)**

---

## 📬 Contact

| | |
|-|-|
| 🌐 **Website** | [www.ice-box-crew.com](https://www.ice-box-crew.com/) |
| 📧 **Email** | [iceboxcrew057@gmail.com](mailto:iceboxcrew057@gmail.com) |
| 🐛 **Issues** | [GitHub Issues](https://github.com/IceBoxCrew/IceBoxEngine/issues) |

---

## 📄 License

**IceBox Engine** is proprietary software.  
All rights reserved by **IceBoxCrew Studio** © 2026.

What that means for you:

- **Everything you make with it is yours.** Your games, code, art, scripts, plugins and mods belong to you. We claim no ownership of them and assert no rights in them.
- **You keep all of your revenue.** No royalties, no revenue share, no per-title fee, no reporting. One payment, and that is the whole price.
- **Every future update is free.** All of them, across major versions too — no paid upgrade, no edition to buy again later.
- **No forced credit.** No splash screen, no watermark, no logo in anything you build. Credit us if you want to — you are welcome to, and never required to.
- **Your players need nothing from us.** No key, no account. A packaged game contains no license check at all and never contacts us.
- **You may write, publish and sell plugins and mods.** Native C++ plugins, Lua and Python scripts, visual scripts, templates — yours to sell.
- **What you may not do is resell the engine itself,** modified or not, or present it as your own engine. That is the one thing the license exists to protect.

Full terms, including license keys and activation, updates, warranty and liability: **[LICENSE.txt](LICENSE.txt)**

---

<p align="center">
  <strong>Built with ❄️ by IceBoxCrew Studio</strong>
</p>
