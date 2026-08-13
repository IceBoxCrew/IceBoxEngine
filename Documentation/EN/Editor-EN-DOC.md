# 🛠️ IceBox Engine — Editor & Interface

## Full documentation in English

> **IceBox Engine** ships as a single editor application: a dockable, multi-panel
> workspace built on Dear ImGui where you build levels, place and edit entities,
> tune the world, run your game in-place, and ship it.
>
> This document is the complete reference for the **editor interface** itself —
> every menu and sub-menu, the toolbar, the **Viewport**, the **Level Outliner**,
> the **Properties** panel, **World Settings**, the **Console**, the **Preferences**
> window, the **Network Manager**, **Remote Preview**, and the **Help** panels
> (Hot-Keys, Documentation, About). It also covers level files, the editor dialogs,
> the editor's own config files, and the full keyboard/mouse reference.
>
> A few areas have their own dedicated documents and are **only linked** from here,
> not re-explained:
> * The **Content Browser** and every asset type/editor →
>   [Assets & Content Browser](Assets-EN-DOC.md).
> * The **Statistics** panel, **Profiler (Tracy)**, **Build Game…** and **DLC
>   Packager** → [Profiling & Building Games](Profiling-And-Building-EN-DOC.md).
> * **Run Python Script** and the editor Python API →
>   [Python API](PythonAPI-EN-DOC.md).
> * The **Lua Script Debugger** and gameplay scripting →
>   [Lua API](LuaAPI-EN-DOC.md).
> * **Plugins & Mods** → [Plugins & Mods](Plugins-And-Mods-EN-DOC.md).
>
> Rendering, lighting, shadows and physics are covered in the companion
> [Graphics, Rendering & Physics](Graphics-EN-DOC.md) document.

---

## 📑 Table of Contents

1. [Introduction](#1-introduction)
2. [The editor at a glance](#2-the-editor-at-a-glance)
   - 2.1 [Window regions](#21-window-regions)
   - 2.2 [Docking & layout](#22-docking--layout)
   - 2.3 [Panel persistence](#23-panel-persistence)
   - 2.4 [Live content watching](#24-live-content-watching)
   - 2.5 [Language, fonts & RTL layout](#25-language-fonts--rtl-layout)
3. [The main menu bar](#3-the-main-menu-bar)
   - 3.1 [File](#31-file)
   - 3.2 [Edit](#32-edit)
   - 3.3 [Window](#33-window)
   - 3.4 [Tools](#34-tools)
   - 3.5 [Help](#35-help)
4. [The toolbar](#4-the-toolbar)
   - 4.1 [Gizmo mode (Q / E / R)](#41-gizmo-mode-q--e--r)
   - 4.2 [Editor audio monitor](#42-editor-audio-monitor)
   - 4.3 [Grid toggle](#43-grid-toggle)
   - 4.4 [Screenshot](#44-screenshot)
   - 4.5 [Play, Pause & Eject](#45-play-pause--eject)
   - 4.6 [Remote Preview button](#46-remote-preview-button)
   - 4.7 [Project name & version](#47-project-name--version)
5. [The Viewport](#5-the-viewport)
   - 5.1 [Camera navigation](#51-camera-navigation)
   - 5.2 [Selecting entities](#52-selecting-entities)
   - 5.3 [Transform gizmos & snapping](#53-transform-gizmos--snapping)
   - 5.4 [The viewport context menu](#54-the-viewport-context-menu)
   - 5.5 [Dragging assets into the scene](#55-dragging-assets-into-the-scene)
   - 5.6 [Play mode in the viewport](#56-play-mode-in-the-viewport)
   - 5.7 [On-screen debug text](#57-on-screen-debug-text)
   - 5.8 [Debug overlays in the viewport](#58-debug-overlays-in-the-viewport)
6. [The Level Outliner](#6-the-level-outliner)
7. [The Properties panel](#7-the-properties-panel)
   - 7.1 [Entity header & class overrides](#71-entity-header--class-overrides)
   - 7.2 [Components](#72-components)
   - 7.3 [Multi-selection editing](#73-multi-selection-editing)
   - 7.4 [World-asset properties (Views & Cinemas)](#74-world-asset-properties-views--cinemas)
   - 7.5 [Per-instance script overrides](#75-per-instance-script-overrides)
8. [World Settings](#8-world-settings)
9. [The Console](#9-the-console)
   - 9.1 [Menu bar](#91-menu-bar)
   - 9.2 [Toolbar & filters](#92-toolbar--filters)
   - 9.3 [The log list](#93-the-log-list)
   - 9.4 [The input bar & commands](#94-the-input-bar--commands)
   - 9.5 [Where console output comes from](#95-where-console-output-comes-from)
10. [Preferences](#10-preferences)
    - 10.1 [Engine](#101-engine)
    - 10.2 [Physics](#102-physics)
    - 10.3 [Collision](#103-collision)
    - 10.4 [Editor](#104-editor)
    - 10.5 [Rendering](#105-rendering)
    - 10.6 [Optimization](#106-optimization)
    - 10.7 [Audio](#107-audio)
    - 10.8 [Accessibility](#108-accessibility)
    - 10.9 [Network](#109-network)
11. [Network Manager (ENet)](#11-network-manager-enet)
12. [Remote Preview](#12-remote-preview)
13. [The Help panels](#13-the-help-panels)
    - 13.1 [Hot-Keys](#131-hot-keys)
    - 13.2 [Documentation](#132-documentation)
    - 13.3 [About](#133-about)
14. [Levels, dialogs & scripts](#14-levels-dialogs--scripts)
15. [Keyboard & mouse reference](#15-keyboard--mouse-reference)
16. [FAQ & troubleshooting](#16-faq--troubleshooting)

---

## 1. Introduction

The IceBox editor is the program you launch to make a game. It is a **single
window** divided into dockable panels, with a menu bar and a toolbar across the
top. Everything you do — building a level, editing an entity, tuning physics,
testing gameplay — happens inside this window.

The workflow centres on one concept: a **level** (a `.icemap` file) is the scene
you are editing. You populate it with **entities** (objects made of components),
organize them in the **Level Outliner**, edit them in the **Properties** panel,
preview the result in the **Viewport**, and press **PLAY** to run the level
exactly as the shipped game will. Assets the level consumes — sprites, tilemaps,
materials, classes, widgets — come from the **Content Browser**.

> The editor's own language and theme are configurable (see
> [Preferences → Editor](#104-editor)); this document uses the default English
> labels. Every label shown here is localized into all supported editor
> languages.

---

## 2. The editor at a glance

### 2.1 Window regions

A fresh editor window has five regions:

```
┌──────────────────────────────────────────────────────────────────────────────┐
│ File Edit Window Tools Help │ Q E R │ 🔊 100% ▦ │ Screenshot Pause PLAY Eject │  ← Menu bar + toolbar
│                             │ Remote Preview │ MyNewGame          Beta 0.7.1 │
├───────────────────────────────────────────────────┬──────────────────────────┤
│                                                   │  Level Outliner          │
│                                                   │  World Settings          │  ← Right dock
│                  Viewport                         │  Properties              │     (tabbed)
│             (the level you edit)                  │                          │
│                                                   │                          │
├───────────────────────────────────────┬───────────┴──────────────────────────┤
│  Content Browser │ Console            │              Statistics              │  ← Bottom dock
└───────────────────────────────────────┴──────────────────────────────────────┘
```

| Region | What it is |
| ------ | ---------- |
| **Menu bar** | `File`, `Edit`, `Window`, `Tools`, `Help` — see [Section 3](#3-the-main-menu-bar). |
| **Toolbar** | Gizmo buttons, editor audio monitor, grid toggle, the **Screenshot / Pause / PLAY / Eject** cluster, the **Remote Preview** button, any plugin buttons, the project name, and the version — see [Section 4](#4-the-toolbar). |
| **Viewport** | The central scene view where you edit the level — see [Section 5](#5-the-viewport). |
| **Right dock** | The **Level Outliner**, **World Settings** and **Properties** panels (tabbed by default). |
| **Bottom dock** | The **Content Browser** and **Console** (tabbed), plus the **Statistics** panel. |

The menu bar and the toolbar are one single ImGui main menu bar: the menus sit on
the left, then the gizmo/audio/grid controls, the Play cluster is centred, and the
project name and version are pushed to the right edge.

The **Content Browser** and **Statistics** panels are documented separately
([Assets](Assets-EN-DOC.md) and
[Profiling & Building](Profiling-And-Building-EN-DOC.md) respectively).

### 2.2 Docking & layout

The whole interface is a **dock space**. Every panel is a window you can:

* **Drag by its tab** to re-dock it anywhere (split a region, stack it as a tab,
  or float it free of the main window).
* **Resize** by dragging the splitter between docked regions.
* **Close** with its `X`, and re-open from the [Window](#33-window) menu.

The **default layout** is created automatically the first time you run the editor
(when no `imgui.ini` exists):

* **Viewport** fills the centre.
* **Level Outliner**, **World Settings** and **Properties** are docked on the
  **right** as tabs (25 % of the width).
* **Content Browser** and **Console** are docked at the **bottom** as tabs
  (30 % of the height).
* **Statistics** is docked at the **bottom-right**.

Every asset editor (Sprite Editor, Class Editor, Material Editor, …) opens as a
free-floating window that you can dock anywhere. Opening the same asset twice does
not create a second window — the existing one is focused instead.

### 2.3 Panel persistence

Several files remember your workspace and your settings between sessions:

| File | Remembers |
| ---- | --------- |
| `imgui.ini` | The dock layout — panel positions, sizes, which are tabbed, and which float. Delete it to reset to the default layout. |
| `Config/Editor.json` | Which panels are open, the last opened level, the list of open asset editors, the console preferences (auto-scroll, timestamps, word wrap, regex, collapse repeats, level mask, buffer limit), the gizmo mode, grid visibility, the per-level debug overlay flags, the editor audio monitor volume/mute, **Update All Assets on Play**, **Auto-Compile on Play**, and every Build Game / DLC Packager field. |
| `Config/Engine.json` | Everything on the [Preferences](#10-preferences) tabs except collision groups — window, rendering, optimization, audio, accessibility and network defaults. Shipped games read the same file. |
| `Config/CollisionGroups.json` | The collision group names and the collision matrix ([Preferences → Collision](#103-collision)). |
| `<Project>.iceproject` | The project manifest. The editor keeps `StartScene` pointing at the level you have open, and refreshes `EngineVersion` when you open the project. |

`Config/Editor.json` is written **automatically every 5 seconds** and again on
exit, so panel state and build settings survive even an unclean shutdown. Toggling
the gizmo mode, the grid, or the editor audio monitor saves it immediately.

Open **asset editors** (Sprite Editor, Material Editor, etc.) are also restored:
when you reopen the editor, the asset panels you had open are reopened to the same
assets.

### 2.4 Live content watching

The editor watches the whole `Content/` folder recursively while it runs. Every
engine asset type is watched (`.ice_*` and `.icemap`) plus the raw source formats
— `.png`, `.jpg`, `.jpeg`, `.wav`, `.ogg`, `.mp3`, `.flac`, `.ttf`, `.otf`,
`.mp4`, `.avi`, `.mkv`, `.mov`, `.webm`, `.lua` and `.txt`.

When something changes on disk:

* A small floating **File Changes** window lists the recent `Created` / `Modified`
  / `Deleted` entries. Each line lives for 5 seconds; **Clear** empties the list
  and closing the window dismisses it. It only appears outside Play mode.
* A **newly created** raw file gets its sidecar asset created automatically —
  images get `.ice_texture` import settings, audio gets an `.ice_sound`, fonts get
  an `.ice_font`, videos get an `.ice_video`. See
  [Assets](Assets-EN-DOC.md) for what sidecars are.
* A changed `.ice_localization` reloads the project's game localization.
* All assets are refreshed and every class instance in the open level is reloaded,
  so external edits show up in the viewport without a restart.

Saving from inside the editor suppresses the watcher for a few seconds so your own
writes don't trigger a redundant reload.

Separately, **class scripts** (`.ice_class`) are hot-reloaded on change when
[Auto-Compile on Play](#32-edit) is on — see that entry for the exact behaviour.

### 2.5 Language, fonts & RTL layout

The editor UI language is chosen in [Preferences → Editor](#104-editor) and covers
**14 languages**: English, Russian, Ukrainian, Chinese, Arabic, Hindi, Spanish,
Portuguese, Japanese, French, German, Italian, Polish and Hebrew.

* The editor font is loaded from `Config/Fonts` with a merged glyph range that
  covers Latin, **Cyrillic**, **Chinese (simplified common)**, **Japanese**,
  **Arabic**, **Hebrew** and **Devanagari**, so no language falls back to boxes.
* For a right-to-left language (Arabic, Hebrew) the editor mirrors its own layout:
  window titles, button and selectable text align right, and the window
  collapse/menu button moves to the right side.
* With the accessibility **Dyslexia-Friendly Font** active, the editor rebuilds its
  UI font with your dyslexia font as the primary and the normal editor font merged
  in as a glyph fallback (see [Preferences → Accessibility](#108-accessibility)).
* Panels fade in and out over a fraction of a second when they open and close;
  this is purely cosmetic and needs no configuration.

Font file, size and colour, plus the UI scale (1×–4×), are also on the
[Editor](#104-editor) tab. Changing the font or the language rebuilds the font
atlas.

---

## 3. The main menu bar

The menu bar runs across the very top of the window.

### 3.1 File

| Item | Shortcut | Action |
| ---- | -------- | ------ |
| **New Level** | — | Opens the **New Level** dialog: pick a destination folder, type a name, and the editor shows a live preview of the resulting `…/Name.icemap` path (with a warning if it already exists). **Create** makes and opens an empty level. If Play mode is running it is stopped first. |
| **Open Level** | — | Opens the **Open Level** dialog with an `.icemap` asset picker. **Open** loads the chosen level. If Play mode is running it is stopped first. |
| **Save Level** | `Ctrl+S` | Saves the current level (entities, Outliner folders, placed view/cinema volumes, World Settings, and the level script) to its `.icemap`. |
| **Level Script Editor** | — | Toggles the **Level Script** window — a Lua (or Visual Script) editor for the level's own `OnLevelStart` / `OnLevelUpdate(dt)` / `OnLevelEnd` callbacks. See [Section 14](#14-levels-dialogs--scripts). |
| **Update All Assets** | — | Saves the level and every dirty asset editor, refreshes all assets, **compiles all class scripts**, rebuilds the post-process volumes and reloads every class instance in the level. This is the manual "make everything current" button. |
| **Update All Assets on Play** | (toggle) | When **on** (default), pressing **PLAY** runs the same save-and-refresh pass first, so Play mode always reflects your latest edits. Turn it off for faster Play starts when you know nothing changed. |
| **Exit** | `Alt+F4` | Quits the editor. If the level has unsaved changes, the **Unsaved Changes** dialog appears first (Save / Don't Save / Cancel). |

A separator sits between **Save Level** and **Level Script Editor**.

### 3.2 Edit

| Item | Shortcut | Action |
| ---- | -------- | ------ |
| **Undo** | `Ctrl+Z` | Reverts the last change. Which stack is used depends on focus — see the note below. |
| **Redo** | `Ctrl+Y` (or `Ctrl+Shift+Z`) | Re-applies the last undone change. |
| **Compile All Scripts** | `Ctrl+R` | Compiles all class scripts and reports the error count. Disabled while Play mode is running. (`Ctrl+R` in the editor also refreshes assets afterwards — it runs the same pass as **Update All Assets**.) |
| **Auto-Compile on Play** | (toggle) | Two effects, both on by default: (1) class scripts **hot-reload** on live entities when their `.ice_class` changes on disk — including entities whose class inherits from the changed one; (2) if **Update All Assets on Play** is off, entering Play mode compiles all class scripts first and warns in the Console if any failed. |
| **Preferences** | — | Opens the [Preferences](#10-preferences) window. |

> **Three independent undo stacks.** The editor does not share one history:
> * The **scene** stack (move/scale/rotate/create/delete entities, folder and
>   world-volume edits) — used when the viewport or the Outliner has focus.
> * The **Content Browser file** stack (create/rename/move/copy/delete) — used
>   while the Content Browser is focused. File operations that were performed as
>   one batch are undone as one batch.
> * A **per-panel** stack inside each asset editor (Sprite, Tilemap, Material,
>   Widget, …). While that panel is focused, `Ctrl+Z`/`Ctrl+Y` act on it and the
>   global stacks are left alone.
>
> Each stack keeps up to **50 states**. Drags coalesce into a single step, so
> dragging a gizmo across the viewport is one undo. Scene and file undo are
> disabled while Play mode is running, and both stacks are cleared when you create
> or open a level.

### 3.3 Window

Toggles the visibility of the core panels. Each item is a checkable entry that
shows or hides its panel:

| Item | Panel |
| ---- | ----- |
| **Level Outliner** | The scene hierarchy — [Section 6](#6-the-level-outliner). |
| **Properties** | The selected entity's components — [Section 7](#7-the-properties-panel). |
| **Content Browser** | Project content — see [Assets](Assets-EN-DOC.md). |
| **Stats** | The **Statistics** panel — performance counters and the debug overlay toggles, see [Profiling & Building](Profiling-And-Building-EN-DOC.md). |
| **World Settings** | Per-level overrides — [Section 8](#8-world-settings). |
| **Show Console** | Logs & command input — [Section 9](#9-the-console). |
| *(Plugin panels)* | Any panels registered by installed plugins appear below a separator at the bottom — see [Plugins & Mods](Plugins-And-Mods-EN-DOC.md). |

Separators group the list as: the four main panels, then **World Settings**, then
**Show Console**, then plugin panels.

### 3.4 Tools

| Item | Documented in | Notes |
| ---- | ------------- | ----- |
| **Run Python Script** | [Python API](PythonAPI-EN-DOC.md) | Opens the **Python Console** panel — a script editor plus a command line for editor automation. |
| **Network Manager (ENet)** | *This document, [Section 11](#11-network-manager-enet)* | Live multiplayer test client/host, chat, voice, rollback diagnostics and the network profiler. |
| **Build Game…** | [Profiling & Building](Profiling-And-Building-EN-DOC.md) | The packaging/cooking/installer pipeline for all six platforms. |
| **DLC Packager** | [Profiling & Building](Profiling-And-Building-EN-DOC.md) | Builds add-on content packages. |
| **Remote Preview** | *This document, [Section 12](#12-remote-preview)* | Streams the running game to an Android device over ADB. **Not shown on macOS.** |
| **Profiler (Tracy)** | [Profiling & Building](Profiling-And-Building-EN-DOC.md) | The advanced frame profiler. |
| **Lua Script Debugger** | [Lua API](LuaAPI-EN-DOC.md) | Breakpoints and stepping for gameplay Lua. Visual Script graphs have their own debugger, driven from the graph editor. |
| **Plugins & Mods** | [Plugins & Mods](Plugins-And-Mods-EN-DOC.md) | Manage installed plugins and mods. |
| *(Plugin tools)* | [Plugins & Mods](Plugins-And-Mods-EN-DOC.md) | Plugins can add their own items (optionally grouped into sub-menus) below the built-ins. |

Only **Network Manager** and **Remote Preview** are explained in this document;
the rest link to their dedicated references.

> Besides Tools items and panels, a plugin can also add **toolbar buttons**,
> **viewport overlays**, **right-click context items**, **Preferences pages** and
> even **new asset types** with their own editors. All of those surfaces are
> described in [Plugins & Mods](Plugins-And-Mods-EN-DOC.md).

### 3.5 Help

| Item | Action |
| ---- | ------ |
| **Hot-Keys** | Opens the **Hot-Keys** panel — a tabbed reference of every editor shortcut. See [Section 13.1](#131-hot-keys). |
| **Documentation** | Opens the in-editor **Documentation** reader (this very document set). See [Section 13.2](#132-documentation). |
| **About** | Opens the **About IceBox Engine** window — version, studio, links and license. See [Section 13.3](#133-about). |

---

## 4. The toolbar

To the right of the menus, the toolbar packs the most-used controls. From left to
right:

### 4.1 Gizmo mode (Q / E / R)

Three buttons choose how the [transform gizmo](#53-transform-gizmos--snapping) in
the viewport behaves. The active mode is highlighted blue.

| Button | Mode | Hotkey |
| ------ | ---- | ------ |
| **Q** | **Translate** (move) | `Q` |
| **E** | **Scale** | `E` |
| **R** | **Rotate** | `R` |

The choice is saved to `Config/Editor.json` immediately and restored between
sessions. The `Q`/`E`/`R` keys switch modes only while the **viewport is hovered**
and no text field has keyboard focus, so typing a name in the Outliner never
changes the gizmo. The Class Editor keeps its own independent gizmo mode.

### 4.2 Editor audio monitor

A small **speaker icon** followed by a **volume slider** (shown as a percentage,
e.g. `100%`). This controls the **editor's audio monitoring** — the sound you hear
while editing (previewing sounds, animations, FX). It does **not** change project
settings and has no effect on game builds.

* Click the **speaker** to mute/unmute the editor monitor (the icon turns red when
  muted).
* Drag the **slider** to set the monitor volume from 0–100 %.

Both settings persist in `Config/Editor.json`.

### 4.3 Grid toggle

A **grid icon** that shows or hides the viewport's editor grid. This is
editor-only and never affects the game. The grid's size, colour, line thickness and
the snap-to-grid option live in [Preferences → Editor](#104-editor); the grid is
drawn in Edit mode only.

### 4.4 Screenshot

The **Screenshot** button captures the current viewport image and opens a native
Save dialog, defaulting to a timestamped `screenshot_YYYYMMDD_HHMMSS.png`. Notes:

* The capture is the **scene framebuffer** at its physical resolution, so
  **Render Scale** (see [Preferences → Engine](#101-engine)) changes the output
  size.
* Only the rendered scene is captured — no editor UI, no gizmos.
* The image is saved as **RGBA PNG**; if you type a filename without `.png` the
  extension is appended. The saved path is echoed in the Console.

### 4.5 Play, Pause & Eject

The central cluster runs and controls Play mode. **Play mode** runs the level
exactly as the shipped runtime would: scripts execute, physics simulates, input is
captured.

| Button | State | Action |
| ------ | ----- | ------ |
| **PLAY** / **STOP** | always enabled | Enters or leaves Play mode (`F3`). |
| **Pause** / **Continue** | only in Play | Pauses or resumes the running simulation (`F5`). |
| **Eject** / **Inject** | only in Play | Detaches the camera so you can fly around the running game with `RMB + WASD` without affecting it; **Inject** returns control to the game (`F2`). |

**Entering Play** performs, in order:

1. If **Update All Assets on Play** is on — saves and refreshes all assets.
2. Records an undo step and takes a **full text snapshot of the scene** plus the
   current selection.
3. If **Auto-Compile on Play** is on and step 1 was skipped — compiles all class
   scripts and warns about failures.
4. Applies the effective physics settings (the level's
   [World Settings](#8-world-settings) override if enabled, otherwise the global
   Preferences values) and the rendering override.
5. Pushes the level script into the script engine and hands the placed **cinema**
   volumes (auto-play / play-once / trigger flags) to the cutscene player.
6. Applies the **custom cursor** from Preferences, or restores the default one.
7. Hides the OS cursor, switches to relative-mouse mode, and starts the runtime.
8. Notifies the Lua and Visual Script debuggers that Play has begun.

**Leaving Play** stops the runtime, clears pause and eject, destroys live FX
emitters, wipes the registry and **restores the pre-play snapshot** — any change
made during Play is discarded — then restores the cursor.

> **Cursor in Play mode.** Entering Play hides the OS cursor and switches to
> relative-mouse mode (for FPS-style look). Press `Shift+F1` to toggle the cursor
> back on without leaving Play.

> **Editing while playing.** With the camera **ejected**, you can select and move
> entities during Play mode; the gizmo writes straight into the live simulation
> (and syncs the physics body for translate and rotate). Remember that these edits
> are discarded when Play stops.

> **Live World Settings.** Editing a World Settings override during Play applies it
> to the running world immediately, so you can tune gravity or lighting while the
> game runs.

### 4.6 Remote Preview button

A toggle that starts/stops the [Remote Preview](#12-remote-preview) streaming
server. It turns **green** while active and its label reflects the connection
state: `Remote Preview` → `Remote (Waiting...)` → `Remote (Connected)`. Hover it
for the `adb reverse` command and, when a device is attached, live stream FPS and
bandwidth. The detailed settings panel is opened from **Tools → Remote Preview**.

This button is **not present on macOS builds**.

### 4.7 Project name & version

* The **project name** (e.g. `MyNewGame`) is shown after a separator — it is the
  `Name` field of the project's `.iceproject` manifest.
* The **engine version** (e.g. `Beta 0.7.1`) is right-aligned at the far end of the
  bar. It is read from `Config/Updater.json` (`B-0.7.1` → `Beta 0.7.1`), falling
  back to the version the editor was compiled with.

Any **toolbar buttons registered by plugins** appear between the Remote Preview
button and the project name.

---

## 5. The Viewport

The **Viewport** is the central panel titled `Viewport - <LevelName>` (an asterisk
`*` marks unsaved changes). It shows the rendered level and is where you place,
select and transform entities.

### 5.1 Camera navigation

The editor camera is a 2D orthographic camera. Navigation works whenever the
viewport is hovered:

| Input | Action |
| ----- | ------ |
| **Right-Mouse + W/A/S/D** | Pan the camera (hold RMB, then use WASD). Pan speed = **Editor Camera Speed** × frame time × current zoom, so panning stays consistent as you zoom out. |
| **Mouse Wheel** | Zoom in/out, anchored on the cursor — the world point under the pointer stays put. |
| **Left Shift + Mouse Wheel** | Adjust the editor camera **pan speed** on the fly (clamped between the engine minimum and twice the configured speed). |
| **F** | Focus the camera on the selected entity (centres it in the viewport). |

Zoom limits and the zoom step are configured in
[Preferences → Editor](#104-editor). In Play mode zooming is disabled unless the
camera is **ejected**.

### 5.2 Selecting entities

| Input | Result |
| ----- | ------ |
| **Left-Click** | Select the topmost entity under the cursor. Hit-testing uses the entity's **visual bounds** (sprite, flipbook or tilemap instances, else a default box), and the winner is the candidate with the **highest Transform Z**. |
| **Ctrl + Click** | **Toggle** the clicked entity in the multi-selection — clicking a selected entity removes it. |
| **Left-Click + Drag** | Draw a **marquee** rectangle; every entity whose bounds overlap it is selected (hold `Ctrl` to add to the current selection). Skeleton meshes are included in the marquee test. |
| **Click on empty space** | Clears the selection — unless the click lands inside a placed **view** volume (post-process or nav-grid bounds) or within the marker box of a **cinema** volume, in which case that world asset is selected instead. |

A drag shorter than 5 px counts as a click, so small hand jitter still selects.
The selection is shared with the [Level Outliner](#6-the-level-outliner) and drives
the [Properties](#7-the-properties-panel) panel. Clicking in the viewport also
clears any Content Browser selection.

### 5.3 Transform gizmos & snapping

When an entity is selected, a **transform gizmo** appears at its origin, drawn in
**world space**. The active [gizmo mode](#41-gizmo-mode-q--e--r) decides what it
does:

* **Translate** — drag the axis handles to move.
* **Scale** — drag to scale; each axis is clamped to **0.01 – 10**.
* **Rotate** — drag the ring to rotate; the result is clamped to **±360°**.

With a **multi-selection**, dragging the gizmo applies the same delta (move, scale
or rotate) to every selected entity. Finishing a drag records one undo step. Axis
flipping is disabled so the handles never jump direction mid-drag.

**Snapping.** When **Snap to Grid** is enabled in
[Preferences → Editor](#104-editor), **translation** snaps to the grid size, and
assets dropped into the viewport snap to the grid as well. Scale and rotation are
never snapped.

**World assets.** A selected **view** or **cinema** volume also gets a gizmo, but
only a **translate** one — and only if the volume is bounded (an infinite/unbound
view volume has no position to drag). Moving a view volume rebuilds the
post-process volume list immediately.

### 5.4 The viewport context menu

A **short right-click** (a quick tap that doesn't pan — under 0.2 s and under 5 px
of movement) opens the viewport context menu:

* A greyed hint that you can drag a `.ice_class` into the viewport to create an
  entity.
* **Edit Class** — if exactly one entity is selected and it came from a class,
  opens its Class asset in the Class Editor (following the asset redirector if the
  class was moved).
* **Delete Selected** (`Del`) — deletes the selected entities.
* For a selected **view/cinema** volume: its name, then **Open in Editor** and
  **Remove**.
* Any `viewport` or `entity` context items registered by plugins.

Nothing but plugin items is shown while Play mode is running without an ejected
camera. A **long right-click or right-drag** pans the camera instead of opening the
menu.

### 5.5 Dragging assets into the scene

Drag from the **Content Browser** onto the viewport:

| Dragged asset | Result |
| ------------- | ------ |
| **Class** (`.ice_class`) | Instantiates the class as a new entity at the drop point (snapped to grid if enabled), then selects it. |
| **View** (`.ice_view`) | Places a **post-process / nav-grid volume** at the drop point (appears under *World Assets* in the Outliner). |
| **Cinema** (`.ice_cinema`) | Places a **cutscene trigger** at the drop point (also a *World Asset*). |

Any other asset type dropped on the viewport is ignored. Dropping is blocked while
Play mode is running unless the camera is ejected. Other asset types are placed via
the Class Editor; see [Assets](Assets-EN-DOC.md).

### 5.6 Play mode in the viewport

While in Play mode the viewport shows the **running game**. The Outliner and
Properties panels are greyed out (read-only) unless you **Eject** the camera, at
which point selection, the gizmo, `F`, `Delete` and `Ctrl+C/X/V` all work again for
debugging. See [Play, Pause & Eject](#45-play-pause--eject).

### 5.7 On-screen debug text

Gameplay scripts can print text onto the viewport — fixed HUD-style messages
anchored at the top-left and world-space labels that follow world coordinates (via
`PrintScreen` / `PrintScreenEx` / `DrawWorldText`). Both kinds are drawn directly
over the viewport in edit and Play mode, with a drop shadow, per-message colour and
scale, and a fade-out as their timer runs down. World-space labels outside the
viewport are skipped. The functions are part of the runtime debug API listed in the
[Hot-Keys](#131-hot-keys) panel and documented in the
[Lua API](LuaAPI-EN-DOC.md).

### 5.8 Debug overlays in the viewport

The **Statistics** panel's *Debug* section holds ~23 overlay toggles that draw into
the viewport — colliders, nav grid, entity markers, light radii, audio ranges,
camera frustum, joints, physics contacts, sleeping bodies, velocity vectors,
tilemap grid, FX/widget bounds, Z-depth colouring, wireframe, freeze culling, plus
an advanced group (shadow maps, shadow edges, light heatmap, nav-grid heatmap, AI
state/perception/paths).

Two things are worth knowing from the editor's side:

* The toggles are **saved per project** in `Config/Editor.json` and restored on the
  next launch.
* They are also readable and writable from script by name
  (`GetDebugFlag` / `SetDebugFlag` / `ToggleDebugFlag`), so a level script can
  switch an overlay on while you test.

The overlays themselves are described in
[Graphics → Debug visualization](Graphics-EN-DOC.md#12-debug-visualization); the
panel that hosts them is in
[Profiling & Building](Profiling-And-Building-EN-DOC.md#34-debug-visualization).

---

## 6. The Level Outliner

The **Level Outliner** (`Window → Level Outliner`) is the scene hierarchy: every
entity in the level, organized into optional folders, plus any placed world
volumes.

**Top controls**

* **Search…** — live, case-insensitive filter of entities by name. Folders stay
  visible; only their non-matching children are hidden.
* **Create Folder** — adds an organizational folder named `New Folder`.
* A greyed hint: *Drag .ice_class to Viewport*.
* **Delete** — appears only when entities are selected; deletes them.

**The tree**

* **Entities** are listed by their tag (name) with a one-letter icon: `[C]` for a
  camera, `[S]` for a sprite renderer, `[L]` for a class/script entity, `[E]`
  otherwise. If an entity was instantiated from a class and later renamed, the row
  shows `Tag (ClassName)` so you can still tell what it is.
* **Folders** (`[F]`) are purely organizational groupings. Drag entities onto a
  folder to file them; folders can be nested (the tree stops recursing past 10
  levels). Expansion state and membership are saved with the level.
* **World Assets** — placed **view** (`[V]`) and **cinema** (`[C]`) volumes appear
  in their own section at the bottom, under a *World Assets* heading. Hovering one
  shows its asset path and position (and scale, for a bounded view volume, or
  *Infinite* for an unbounded one).

**Operations**

| Input | Action |
| ----- | ------ |
| **Click** | Select an entity, folder or world asset — the three selection kinds are mutually exclusive. |
| **Ctrl + Click** | Toggle an entity in the multi-selection. |
| **Shift + Click** | Select the whole visible range between the last clicked entity and this one. |
| **Double-click** | On a class entity: opens its Class asset. On a world asset: opens the `.ice_view` / `.ice_cinema` asset. |
| **F2** | Rename the selected entity, or the selected folder. |
| **Delete** | Delete the selected entity, folder or world asset. |
| **Ctrl + C / X / V** | Copy / cut / paste entities (pasted copies are offset slightly and renamed). Also works while the viewport is focused. |
| **Ctrl + A** | Select all entities. |
| **Drag** | Re-file an entity (or the whole multi-selection) into a folder, or drag a folder into another folder. Dropping onto empty space in the panel moves items out of all folders. |
| **Right-click an entity** | Copy / Cut / Paste, and **Move to Folder ▸** with `(None)` plus every folder. |
| **Right-click a folder** | **Rename Folder**, **Create Subfolder**, **Delete Folder**, and **Move to Root** for a nested folder. |
| **Right-click a world asset** | **Remove**, **Open in Editor**. |
| **Right-click empty space** | **Create Folder**, plus the drag hints. |

All shortcuts require the Outliner to be focused, no popup open, and no text field
active.

> Dragging a **class** asset onto the Outliner instantiates it at the origin
> (drop it on a folder to file it there at the same time); dragging a **view**
> asset adds a world volume. **Cinema** volumes must be dropped on the
> [viewport](#55-dragging-assets-into-the-scene), where they get a position.

---

## 7. The Properties panel

The **Properties** panel (`Window → Properties`) edits whatever is currently
selected — an entity, several entities, or a placed world volume.

### 7.1 Entity header & class overrides

For a single entity, the top of the panel shows:

* **Tag** — the entity's name, editable inline.
* If the entity was **instantiated from a class**, a **class-sync** section:
  * **Override Class Defaults** — when **on**, edits you make here are stored
    per-entity in the level and are *not* overwritten by the class template on
    save/reload. When **off**, an amber warning notes that the class defaults are
    authoritative and local edits will reset on reload.
  * A live status line: green **"In sync with class defaults"**, or an amber
    collapsible **"Modified from class (N)"** listing exactly which components
    differ. A component the class defines but the entity no longer has is listed as
    `… (removed)`.
  * The [per-instance script override](#75-per-instance-script-overrides) section.

The diff deliberately **ignores the Transform's Position**, so simply placing an
instance somewhere in the level never counts as "modified from class". Identity
components (ID, tag, hierarchy) are ignored too, and the comparison is made against
the class's **fully merged** template, so inherited classes diff correctly.

### 7.2 Components

Below the header, each **component** on the entity is a collapsing section with its
own fields, open by default. The available component types are:

| Component | Purpose |
| --------- | ------- |
| **Transform** | Position, scale, rotation (the spatial root of every entity). |
| **Sprite Renderer** | Draws one or more sprites, with material/shading/blend/tint and per-instance transforms. |
| **Flipbook** | Plays frame-by-frame sprite animations. |
| **Animator** | Drives flipbooks from an Animation state machine (`.ice_animation`). |
| **Skeleton** | 2D skeletal animation (bones, slots, skins, IK, physics bones). |
| **Tilemap Renderer** | Renders a tilemap (`.ice_tm`), optionally generating collision. |
| **Camera** | A game camera (primary flag, zoom, viewport rect, player index for split-screen). |
| **Rigidbody** | Box2D body (Static / Kinematic / Dynamic) with mass, damping, gravity scale, bullet (CCD), sleep. |
| **Collider** | Box / Circle (sphere) / Capsule shapes with physics material, sensors, one-way, and collision groups. |
| **Destructible** | Breakable geometry that fractures on impact. |
| **Audio** | An attached sound source (2D or spatial 3D). |
| **FX** | A particle system instance (`.ice_fx`). |
| **Widget** | An attached UI widget (`.ice_widget`). |
| **Light** | Point and spot lights (color, intensity, radius, shadows, light cookie). |
| **Point Marker** | Named local anchors for scripting/attachment. |
| **AI** | A behavior tree instance (`.ice_ai`) with its blackboard. |
| **Joint** | A physics joint connecting two bodies. |
| **Stencil** | Stencil mask/clip control for masked rendering. |
| **Replication** | Network replication settings for the whole entity: whether it replicates, who owns it, which aspects sync (transform, velocity, visuals, full state), whether Lua runs on replicas, and Area-Of-Interest relevancy. Off by default, so singleplayer projects are unaffected. |
| **Class Component** | Embeds other classes as sub-objects (composition). Each instance has a name, a transform and its class path; below the list, a *Provides (resolved at runtime)* summary groups everything the embedded classes contribute (sprites, colliders, lights, …). |

> **Where components come from.** The Properties panel edits the components an
> entity **already has** — it has no *Add Component* button. Components are added to
> and removed from a **Class** in the [Class Editor](Assets-EN-DOC.md), and every
> instance of that class picks the change up. That is what keeps instances in sync
> with their template and what the "Modified from class" indicator measures.

> The data each component renders/simulates is detailed in the
> [Graphics, Rendering & Physics](Graphics-EN-DOC.md) document (rendering, lights,
> physics) and the [Assets](Assets-EN-DOC.md) document (the assets they reference).
> Scripting components are covered in the [Lua API](LuaAPI-EN-DOC.md).

### 7.3 Multi-selection editing

With several entities selected, the panel shows a *N entities selected* header and
a single **Transform** section holding the first selection's position, scale and
rotation. Editing a field applies the **delta** to every selected entity that has a
Transform, and position/rotation changes are pushed straight into the live physics
body during Play. Component sections are not shown for multi-selections — use the
gizmo or edit entities one at a time.

For bulk editing of *assets* (not entities), use the **Property Matrix** from the
Content Browser (see [Assets](Assets-EN-DOC.md)).

### 7.4 World-asset properties (Views & Cinemas)

When a placed **view** or **cinema** volume is selected (instead of an entity), the
Properties panel shows that volume's placement: its **name**, its **asset path**,
and an editable **Position**. Everything else lives elsewhere:

* The post-process stack and nav-grid parameters belong to the `.ice_view` asset —
  double-click the volume (or use **Open in Editor**) to edit them in the View
  Editor. The stack itself is described in
  [Graphics → Post-processing](Graphics-EN-DOC.md#9-post-processing).
* A cinema's **Auto Play / Play Once / Trigger On Overlap / Trigger Tag** flags are
  per-placement and live in [World Settings → Cutscene](#8-world-settings); the
  cutscene timeline itself is in the Cinema Editor.

### 7.5 Per-instance script overrides

Under the class-sync header, an entity created from a class gets a **Lua Script
(Class Override)** section. It exists so one instance can behave differently from
its class without cloning the class — for example wiring one specific button to one
specific door.

**Lua projects**

* With **Override Lua Script** off, the class's script is shown **read-only**, and
  a **Clear Saved Override** button appears if a stale override is still stored in
  the level.
* Turning it **on** copies the class script into the level as the starting point
  and unlocks the editor. The toolbar then offers **Compile** with an inline
  `OK`/`Error` badge (errors are marked on the offending line and shown as a
  tooltip), **Reset to Class Script**, and **Show Parent** / **Hide Parent**, which
  opens a read-only pane of the class script for comparison.
* The override text is stored in the `.icemap`, not in the class file.

**Visual Script projects**

* The same checkbox switches between the class graph and a per-instance copy of it.
* **Edit Visual Graph** opens the graph in its own window with a **Compile** button
  and a status line; the node count is shown next to it. Edits recompile to Lua on
  the fly and are stored with the entity.
* **Reset to Class Script** re-copies the class graph.

---

## 8. World Settings

The **World Settings** panel (`Window → World Settings`) holds **per-level
overrides** — settings that apply only to the level you are editing, layered on top
of the global [Preferences](#10-preferences). It shows the current level's name and
path at the top; with no level open it says *No level loaded. Open or create a
level first.*

**Physics override** — tick **Override Level Physics** to give this level its own
physics. The fields mirror [Preferences → Physics](#102-physics): Pixels-Per-Meter,
Gravity X/Y, solver Sub-steps and Fixed Timestep, and the world flags (Enable
Sleep, Enable Continuous/CCD, Restitution Threshold, Hit-Event Threshold, Contact
Hertz, Contact Damping Ratio, Max Contact Push Speed, Maximum Linear Speed).
**Reset to Global Values** copies the current global values in. When the override is
off the panel says so and the level uses the global physics.

> **Physics Worker Threads** is deliberately *not* overridable per level — it is
> engine-global and fixed when the physics world is created. See
> [Preferences → Physics](#102-physics).

**Rendering override** — tick **Override Level Rendering** to give this level its
own look. This mirrors [Preferences → Rendering](#105-rendering):

* **Lighting Mode** (Unlit / Lit) and, when Lit: **Ambient** color & intensity.
* **2D Shadows** — enable, ray quality (Low/Medium/High/Ultra), softness,
  intensity, bias (X/Y), PCF samples, **directional shadow length**, **directional
  depth fade**, and **colliders block shadows**.
* **Ray Tracing** (only when the hardware supports it, otherwise the section says
  so) — enable, quality, intensity, bounce, max bounces, reflection, max distance,
  ray-traced AO and its radius, albedo response, sky light, detail sharpness,
  screen colour bleeding, denoise, shadow rays.
* **Directional Light** — enable, cast shadows, color, intensity, direction X/Y.
* **Clipping Planes** (Near / Far) and the **Clear Color** (background).

**Reset Rendering to Global Values** copies the current global values in.

**Cutscene section** — if the level contains placed **cinema** volumes, a
*Cutscene* header lists each of them with its path and position and lets you set
**Auto Play**, **Play Once**, **Trigger On Overlap** and, when overlap is on, the
**Trigger Tag**.

All World Settings are saved inside the `.icemap`. They are applied automatically
when the level loads and when you press PLAY — and while Play is running, editing
any of them re-applies to the live world immediately. Any edit here also marks the
level dirty and records an undo step.

---

## 9. The Console

The **Console** (`Window → Show Console`, or the bottom-dock tab) shows engine and
game logs and lets you run commands. It tabs with the Content Browser by default.

### 9.1 Menu bar

* **Edit** — Copy (`Ctrl+C`), Copy Visible, Select All (`Ctrl+A`), Save Log…,
  Save Visible Log…, Clear (`Ctrl+L`). Items grey out when they have nothing to
  act on.
* **View** — toggles for **Auto-scroll**, **Show Timestamps**, **Word Wrap**,
  **Collapse Repeats** (fold identical consecutive lines into an `(xN)` counter),
  and the **Buffer Limit (lines)** — the maximum number of retained lines,
  100–50000, default 2000.

All of these preferences persist in `Config/Editor.json`.

### 9.2 Toolbar & filters

| Control | Function |
| ------- | -------- |
| **Pause / Resume** | Freeze incoming logs. They are buffered while paused (up to the same buffer limit) and flushed on resume. |
| **Clear** | Empty the log (`Ctrl+L`). |
| **Auto-scroll** | Stick to the newest line. Scrolling up with the wheel releases the stick; scrolling back to the bottom re-arms it. |
| **Trace / Info / Warn / Error** | Per-level toggle buttons with live counts, tinted by level. Left-click toggles a level; **right-click solos** just that level. |
| **Filter…** | Live, case-insensitive substring filter over the message text. |
| **.\*** | Toggle **regex** mode for the filter (ECMAScript, case-insensitive). An invalid pattern simply matches nothing until you fix it. |
| **[category]** | When you filter by a row's leading bracketed tag from its context menu, a chip appears here with an `x` to remove it. |

### 9.3 The log list

Each row is prefixed with a level letter (`T`, `I`, `W`, `E`), optionally a
timestamp (`HH:MM:SS.mmm`), and gets a `(xN)` suffix when repeats are collapsed.
Text is coloured by level — Trace grey, Info white, Warn amber, Error red — and
Warn/Error rows also get a tinted row background so they stand out while scrolling.

* **Click** to select a line; `Ctrl+Click` adds individual lines, `Shift+Click`
  extends a range, and dragging selects a range (auto-scrolling past the edges).
* **Double-click** copies that line.
* **F3 / Shift+F3** jump to the next/previous line matching the current filter,
  scrolling it into view and selecting it.
* **Right-click** a row for: Copy, Copy Visible, Select All, **Filter by
  category** (only when the message starts with a bracketed tag), **Hide this
  message** (permanently hides every line with that exact text for this session),
  **Open source** (`file:line` — parsed out of the message and opened in the
  matching editor), Save Log…, Save Visible Log…, and Clear.

The **status bar** at the bottom reports Total / Shown counts, per-level counts
(T/I/W/E, with warn and error highlighted when non-zero), the number of selected
lines, and a `[PAUSED]` badge when paused.

Long logs stay responsive: rows are clipped to what is visible, and the filtered
view is cached and only rebuilt when the log or a filter actually changes.

### 9.4 The input bar & commands

The bottom input line runs code or commands. A **language button** cycles the input
mode between **Lua**, **Py** (Python) and **Sh** (system shell) — the default is
**Sh** — and plain input runs in the selected language. **Up/Down** browse the
input history (the last 200 entries), **Esc** clears the field, and **Enter** or
the **Execute** button runs it. Focus returns to the field after each run.

Lines beginning with `/` (or `:`) are **console commands**:

| Command | Effect |
| ------- | ------ |
| `/clear`, `/cls` | Clear all logs. |
| `/help`, `/?` | List the commands. |
| `/pause`, `/resume` | Pause/resume incoming logs. |
| `/filter <text>` | Set the quick filter. |
| `/level <T\|I\|W\|E\|*>` | Solo a log level (`*` shows all). |
| `/save` | Save the full log to a file. |
| `/echo <text>` | Print text. |
| `/lua <code>` | Run a line of Lua. |
| `/py <code>` (`/python`) | Run a line of Python. |
| `/sh <command>` (`/cmd`, `/shell`, `/bash`) | Run a system shell command (supports pipes, `&&`, `.bat`, …). Output and the exit code are echoed into the log. |
| `/cd [dir]`, `/pwd` (`/cwd`) | Change / print the shell working directory. `~` and relative paths are resolved; the directory persists between commands. |
| `/mode <lua\|py\|sh>` | Set the default language for plain input. |

An unknown command prints a warning pointing at `/help`.

> The Lua and Python languages themselves are documented in the
> [Lua API](LuaAPI-EN-DOC.md) and [Python API](PythonAPI-EN-DOC.md). The console is
> just one place to invoke them. Python is only available in editor builds.

> The **Console** panel here is the editor's log view. The separate in-game
> **developer console** (an overlay your shipped game can open at runtime) is part
> of the runtime, not this editor panel.

### 9.5 Where console output comes from

The Console is wired in as a **log sink** on both engine loggers at trace level, so
it receives everything the engine and your game emit — including messages logged
from Lua and Python and from plugins. Each line arrives already formatted as
`[HH:MM:SS] [logger] message`, which is what the *Filter by category* action reads
its tag from.

Log entries are captured on whatever thread produced them and merged under a lock,
so background work (asset loading, builds, the network threads) shows up safely and
in order.

---

## 10. Preferences

**Edit → Preferences** opens the editor's settings window. It is organized into
**nine tabs**, and each tab ends with three buttons:

* **Save** — write that tab's settings to disk (`Config/Engine.json`, or
  `Config/CollisionGroups.json` for the Collision tab) so they persist.
* **Apply** — apply that tab's settings to the running editor immediately.
* **Reset** — restore that tab's defaults.

Below the tabs, three window-wide buttons act on **every** tab at once:
**Save All**, **Apply All** and **Reset All to Defaults** (which resets, saves and
applies in one step).

Some settings note that they need a **restart** to take effect — anti-aliasing when
MSAA is involved, the shader-limit values on the Optimization tab, and the physics
worker-thread count.

### 10.1 Engine

Window and top-level performance:

* **Window Width / Height**, **Target FPS** (1–240).
* **Render Scale** (1–200 %) — internal resolution multiplier (upscale/downscale).
* **VSync**, **Low Latency Mode**, **Project Pre-Warm on Startup** (warm caches at
  load).
* **HDR10** output where supported, with **paper-white** (80–400 nits) and
  **max luminance** (400–10000 nits).
* **Window Mode** — Windowed / Fullscreen / Borderless.
* **Anti-Aliasing** — Off, FXAA (Post), MSAA 2×/4×/8×, SSAA 2×/4×. Switching to or
  from MSAA shows a restart warning; MSAA also exposes **Alpha-to-Coverage**.
* **Upscaling** — **FSR** (AMD FidelityFX Super Resolution 1.0) or **NIS**
  (NVIDIA Image Scaling), with a quality preset (Ultra Performance 33 % / Performance
  50 % / Balanced 59 % / Quality 67 % / Ultra Quality 77 % / Native 100 %), a
  **Sharpening** toggle and its 0–1 strength. Off by default. Like ray tracing this is
  **Vulkan-only and gated on the GPU** — NIS additionally requires an NVIDIA card — and
  the section shows why it is unavailable when it is. The preset multiplies with Render
  Scale and the SSAA modes, and the panel prints the resulting internal resolution. See
  [Graphics → Upscaling](Graphics-EN-DOC.md#102-upscaling-fsr--nis).
* **Adaptive Quality** with a target FPS (30–240) — auto-tunes quality to hold the
  framerate.
* **Auto Detect** — probes the machine and fills in sensible FPS, render scale, AA,
  upscaling, VSync, HDR10, low-latency, pre-warm and audio-quality values.
* **Sound Quality** preset — Very Low (8000 Hz) / Low (16000 Hz) /
  Medium-Low (22050 Hz) / Medium (44100 Hz) / High (48000 Hz) /
  Very High (96000 Hz).
* **Custom Cursor** — a `.ice_sprite` or `.ice_flipbook`, its hotspot X/Y, and a
  scale (0.5–10). It is applied when Play mode starts and in the shipped game.

### 10.2 Physics

Global Box2D defaults (a level can override these in [World Settings](#8-world-settings)):
**Pixels-Per-Meter**, **Gravity X/Y**, solver **Sub-steps** (1–16) and
**Fixed Timestep**, and the world flags: **Enable Sleep**, **Enable Continuous**
(CCD), **Restitution Threshold**, **Hit-Event Threshold**, **Contact Hertz**,
**Contact Damping Ratio**, **Max Contact Push Speed**, **Maximum Linear Speed**.
These feed directly into the physics world; see
[Graphics → Physics](Graphics-EN-DOC.md#13-physics).

**Physics Worker Threads** controls how many threads Box2D uses to solve a physics
step, counting the main thread. **Auto** (the default) derives a safe value from the
CPU core count — at most 8 on desktop, at most 4 on mobile — and `1` turns physics
multithreading off. Unlike the other fields, this one is engine-global, cannot be
overridden per level, and only takes effect after an engine restart, because the
worker count is fixed when the physics world is created. It pays off in scenes with
many awake bodies and contacts; small scenes see no difference. The Statistics
panel's **Physics World** section reports the count actually in use. Web builds
always run physics single-threaded.

### 10.3 Collision

Manages the **collision groups** and the **collision matrix**:

* Add, rename and remove named collision groups (group `0` is the default and
  cannot be removed).
* The **matrix** is a grid of checkboxes deciding which groups collide with which.
* **Save / Apply / Reset** write `Config/CollisionGroups.json`.

Colliders reference these groups by index; see
[Graphics → Physics](Graphics-EN-DOC.md#13-physics).

### 10.4 Editor

Look-and-feel of the editor itself:

* **Theme** — Dark / Light / **Custom**. Choosing *Custom* seeds the palette from
  the current theme and reveals a full ImGui colour editor grouped into
  **General** (text, disabled text, window/child/popup backgrounds, border, menu
  bar), **Frames** (frame background and its hovered/active states, scrollbar,
  checkmark, slider grab), **Buttons**, **Headers** (header states and separator),
  **Tabs** and **Title** (title bar, active, collapsed).
* **Language** — the editor UI language, from the 14 supported languages.
* **Font** — file (scanned from `Config/Fonts`), size (8–72 px) and colour, with a
  **Refresh** button that rescans the folder.
* **UI Scale** (1×–4×).
* **Grid** — size (1–1000), line thickness (0.01–1), colour, **Snap to Grid**,
  **Show Grid**.
* **Camera** — editor camera speed, min zoom, max zoom (the two clamp each other)
  and zoom step.

### 10.5 Rendering

Global rendering defaults (a level can override these in
[World Settings](#8-world-settings)):

* **Lighting Mode** (Unlit / Lit).
* **Render Backend** — the list depends on the platform and the build: OpenGL 4.6 /
  OpenGL 3.3 (/ Vulkan when compiled in) on Windows and Linux, Metal (ANGLE) /
  Metal (MoltenVK) on Apple.
* When **Lit**: **Ambient** color & intensity; **2D Shadows** (enable, ray quality,
  softness, intensity, bias, PCF samples, directional length and depth fade,
  colliders-block-shadows); **Ray Tracing** (when supported); **Directional Light**
  (enable, cast shadows, color, intensity, direction).
* **Clipping Planes** (Near / Far) and the **Clear Color** (background).

The full meaning of each is in the
[Graphics, Rendering & Physics](Graphics-EN-DOC.md) document.

### 10.6 Optimization

Tuning knobs for the renderer and memory:

* **Culling** — cull padding in pixels, and culling mode (**AABB (Default)** /
  **PerPixel (Precise)**).
* **Batching** — max quads / particles / instances per batch.
* **Texture Slots** — max texture units for the GL and GLES backends.
* **Lighting Limits** — max active point lights.
* **Debug** — the debug vertex buffer size.
* **Texture Atlas** — atlas page size, max sprite size, max texture size.
* **Suspend** — **Is Suspended**: when on (the default behaviour), the app pauses
  update, render and audio while the window is unfocused, minimized or
  backgrounded, on all six platforms; when off, the game keeps running and playing
  audio without focus. Also scriptable via `Settings.SetIsSuspended`.
* A read-out of the compile-time **Hard Caps** the engine enforces (max point
  lights, max texture slots).

Changing texture slots, point-light limits, the debug buffer, atlas sizes or any
batch size shows an amber "requires engine restart" note, because those values are
baked into the shaders at startup.

### 10.7 Audio

The global mixer:

* **Master Controls** — Mute All, Master Volume, Global Gain (0–2).
* **Group Volumes** — Music, SFX, Voice, Ambient, UI.
* **3D Audio** — Spatial Audio toggle and, when on, Doppler Factor, Speed of Sound
  and the default attenuation min/max distance and rolloff.
* An **Audio Info** read-out of the active device name, sample rate and channel
  count (shown once the audio system is initialized).

### 10.8 Accessibility

A broad accessibility suite (master **Accessibility Enabled** toggle, then):
Gamma, Contrast, Brightness, Saturation; **Colorblind** mode (Protanopia /
Deuteranopia / Tritanopia / Achromatopsia) with strength; a
**Dyslexia-Friendly Font**; **Text-to-Speech** (enable, rate, volume, pitch);
**Game Speed** (0.25×–2×, applied to gameplay delta time); and **Force Mono Audio**.
These settings are exposed to games so they can honor the player's accessibility
choices.

The **Dyslexia Font Path** points at a `.ttf`/`.otf` inside the project's
`Content/` folder (picked via the asset picker; stored Content-relative, but
`Content/`-prefixed and absolute paths work too). When active, the editor's own
UI font is rebuilt with the dyslexia font as primary (the regular editor font
stays merged in as a glyph fallback), and every font used by game text
rendering — widget elements with or without custom fonts, tooltips, dropdown
lists — is overridden in both the editor viewport and the running game.

With **Text-to-Speech** enabled, the editor reads hovered UI text and tooltips
aloud after a short dwell; in play mode and in the packaged game the widget runtime
automatically speaks the widget element under the cursor (including
non-interactable text labels), tooltips, hovered dropdown options, and the element
focused with gamepad/keyboard navigation. Both features follow `Config/Engine.json`,
so a shipped game picks them up automatically — press **Save** (or **Save All**) so
the values persist beyond the current editor session.

### 10.9 Network

Default values for the multiplayer subsystem, grouped into:

* **Server Settings** — max players, port.
* **Connection Settings** — server IP, timeout.
* **Reconnection** — enable, max attempts, interval.
* **Performance** — tick rate, snapshot rate, interpolation delay.
* **Security** — password, whitelist, entity validation, max entity speed.
* **Voice Chat** — enable, proximity mode, proximity range, activity timeout, max
  relayed voice players.
* **Advanced** — dedicated-server mode, **WebSocket Port (0 = Off)** for web
  clients, **Trust Model** (*Competitive (Authoritative)* / *Co-op (Trusted
  Peers)*), lag compensation, delta compression, prediction, max history duration.
* **Moderation** — profanity filter, chat cooldown, chat burst limit, chat rate
  window.
* **Limits** — max chat history, max entity history, max rate-limit violations.
* **Live Status** — role, players online, capacity and ping while a session is
  running; otherwise a note that the network is not active.

The **live** test client/host is the [Network Manager](#11-network-manager-enet).

---

## 11. Network Manager (ENet)

**Tools → Network Manager (ENet)** opens a live multiplayer test harness built on
ENet. Its own menu bar has **Network → Clear Log**, and the window has two top-level
tabs.

**Test Network** — host or join a session and exercise it:

* A coloured **Status** line: *Offline*, *Connecting…*, *Reconnecting…*,
  *Connection Failed*, *Connected* or *Server Running*.
* **Mode** radio buttons — **Offline / Host / Client**. Hosting exposes **Max
  Players**; joining exposes **Server IP**. Both expose **Port** (1024–65535) and a
  masked **Password**. All of these lock while a session is up.
* **Start Server** / **Connect**, and **Stop Server** / **Disconnect** (red) once
  connected. Connect/host events are echoed into the chat log as system messages.
* Four inner tabs:
  * **Chat** — a message log with a **Mode** selector: **Public / Channel / DM**.
    Channel mode adds a channel name field with **Join** and **Leave** and lists the
    channels you are in; DM mode adds a target player name. Messages are
    colour-coded by kind (system, public, `#channel`, private) and the view
    auto-scrolls.
  * **Peers** — as a server, the connected players with per-peer **Kick**, **Ban**
    and **Mute Voice**; as a client, the server address and your round-trip time.
  * **Stats** — connection type, ping, tick rate, snapshot rate, bytes sent and
    received, peer count, entity count, server time and interpolation delay, plus
    the feature flags. As a server it also gives live sliders and checkboxes for
    **Tick Rate** (1–128), **Snapshot Rate** (1–60), **Delta Compression**,
    **Prediction** and **Entity Validation**.
  * **Voice** — only present when voice chat is enabled in the config: enable/mute
    yourself, and per-player mute, volume (0–2) and a *transmitting* indicator.
* **Rollback Diagnostics** — a collapsible section that reports the rollback
  netcode state when it is running: synchronizing/running, local player handle,
  player count, current and confirmed frame, predicted frames, rollbacks per second,
  average and maximum rollback depth, and frame advantage.

**Network Profiler** — a second tab with live graphs sampled a few times a second
and a **Pause** checkbox that freezes sampling:

* **Latency** — ping (green/amber/red by threshold), interpolation delay and
  time-sync offset for clients, plus a ping graph.
* **Bandwidth** — total bytes sent/received, current send/receive rates, and two
  rate graphs.
* **Packets** — packet sequence, packet rate, tick rate, snapshot rate and a packet
  rate graph.
* **Entity Sync** — replicated entity count (with a graph) and, as a server,
  connected peers against capacity.
* **Features** — lag compensation, delta compression, prediction, entity validation
  and encryption, plus a dedicated-server badge.
* **Per-Peer Stats** — as a server, an expandable node per peer with uptime, RTT,
  packets lost and bytes sent/received.

> The network configuration defaults live in
> [Preferences → Network](#109-network); the gameplay-facing networking API is in
> the [Lua API](LuaAPI-EN-DOC.md). The scriptable `NetworkProfiler.*` surface that
> mirrors this panel is listed in the [Hot-Keys](#131-hot-keys) panel.

---

## 12. Remote Preview

**Tools → Remote Preview** (and the toolbar button) streams the running game to an
**Android device over USB**, with touch and motion input sent back — ideal for
testing mobile controls without a full build. The feature is **not available on
macOS**.

**How it works.** The editor runs a small TCP server that binds to **localhost
only**, and a companion Android app connects to it through `adb reverse`. While Play
mode runs, the editor sends JPEG frames plus game audio to the device, and the
device sends touch, accelerometer and gyroscope events back into the engine's input
system. Nothing is streamed in edit mode — enter Play mode to see a picture.

The panel shows:

* **Status** — *Stopped*, *Waiting for device…*, *Handshaking…* or
  *Device Connected*, colour-coded.
* **Port** (1024–65535, default 7780). If the port is busy the server tries the next
  15 ports automatically and reports the one it actually bound.
* **Stream FPS** (10–60), **JPEG Quality** (10–100) and **Resolution Scale**
  (0.1–1.0, default 0.5) to trade bandwidth for fidelity.
* **Start Server** / **Stop Server**.
* When running: the `adb reverse tcp:<port> tcp:<port>` command, and a one-click
  **Deploy to Device** button.
* When connected: the device's live **Stream FPS** and **Bandwidth**.

**Deploy to Device** does the whole set-up in one step, reporting progress inline:

1. Verifies that `adb` is on `PATH` (and tells you to install the Android SDK
   Platform-Tools if not).
2. Detects an authorized device (and tells you if the USB-debugging prompt is still
   pending).
3. Runs `adb reverse` for the bound port.
4. Makes sure the **IceBox Preview** companion APK is present — the engine normally
   ships it pre-built in `Tools/IceBoxPreview`, and only builds it there itself
   (which needs an Android SDK and can take a few minutes) when no pre-built one
   is available — then installs or updates it on the device, reinstalling from
   scratch if the signatures don't match.
5. Force-stops any previous instance, launches the companion with the port, and
   waits up to 25 seconds for the handshake.

The connection is kept alive with a heartbeat and drops cleanly if the device goes
away. The toolbar [Remote Preview button](#46-remote-preview-button) starts/stops the
same server and reflects the connection state.

---

## 13. The Help panels

### 13.1 Hot-Keys

**Help → Hot-Keys** opens a tabbed shortcut reference. Five tabs:

* **Editor** — file (`Ctrl+S`, `Alt+F4`), edit (`Ctrl+Z/Y/R`, `Del`), selection
  (`F`, `Ctrl+Click`, LMB-drag marquee).
* **Play Mode** — `F5` pause/resume, `F3` toggle Play, `F2` toggle free camera
  (Eject), `Shift+F1` toggle cursor.
* **Viewport** — gizmos (`Q/E/R`), camera (`RMB+WASD`, scroll, `Shift+Scroll`),
  mouse (LMB select, RMB context menu).
* **Tilemap Editor** — paint/erase/fill/pick, region/stamp and `Ctrl+S` (see the
  Tilemap Editor in [Assets](Assets-EN-DOC.md)).
* **Runtime (Debug)** — not shortcuts but the **runtime debug API surface**: the
  `PrintScreen` / `PrintScreenEx` / `RemoveScreenMessage` / `ClearScreenMessages` /
  `DrawWorldText` overlay calls, the `GetDebug…` / `ToggleDebug…` flag helpers,
  `IsDebugBuild`, the profiler-trace calls (`StartProfilerTrace`,
  `StopProfilerTrace`, `SaveChromeTrace`), the whole `NetworkProfiler.*` table, and
  a list of viewers that can open an exported Chrome trace. These are reference for
  scripting — see [Lua API](LuaAPI-EN-DOC.md).

The full shortcut list is reproduced in [Section 15](#15-keyboard--mouse-reference).

### 13.2 Documentation

**Help → Documentation** opens an in-editor Markdown reader for this very
documentation set. It provides:

* A top bar with a **Language:** selector, a **Refresh** button (rescans the folder
  tree) and a **Search:** box. The reader treats **every sub-folder of
  `Documentation/`** as a language, so adding a folder adds a language, and it
  pre-selects the one matching the current editor language.
* Search highlights every hit, shows a `current / total` counter, and gives up/down
  arrows to jump between matches plus an `x` to clear. If nothing matches it says
  so.
* A **sidebar** with two tabs: **Documents** (the file list, with its own filter
  box) and **Contents** (a clickable, indented table of contents built from the
  open document's headings — clicking an entry scrolls to it).
* Rendered Markdown — headings, paragraphs, **bold/italic**, `inline code`, code
  blocks, quotes, ordered and unordered lists, tables, horizontal rules and
  clickable links — using the same font stack as the rest of the editor.

> Any `.md` file you place in `Documentation/EN` (such as this one) appears here
> automatically. If the `Documentation` folder cannot be found at all, the panel
> says so and offers a **Refresh**.

### 13.3 About

**Help → About** opens the **About IceBox Engine** window: the studio name, the
engine name and version, a tagline, the copyright, clickable links (website,
contact email, issue tracker — each opens in your browser or mail client and shows
the target as a tooltip), and a short license summary. Close it with the **Close**
button.

---

## 14. Levels, dialogs & scripts

**Levels.** A level is a `.icemap` file (see [Assets](Assets-EN-DOC.md#419-level-icemap)).
The Viewport title shows the open level and a `*` when it has unsaved changes.
Saving writes the entities, the Outliner's folders and world volumes, the World
Settings and the level script into that one file. The editor separately keeps the
project's `.iceproject` **StartScene** pointing at the level you have open, so
launching the game runs it.

Creating or opening a level clears the scene and file undo stacks, resets the
Outliner folders and world assets and the World Settings, and fires the
corresponding editor Python events (`scene_new`, `scene_loaded`, `scene_saved`) —
see [Python API](PythonAPI-EN-DOC.md).

**Dialogs.**

| Dialog | When | Buttons |
| ------ | ---- | ------- |
| **New Level** | File → New Level | Folder picker + name + path preview (+ "already exists" warning) → **Create** / **Cancel**. `Enter` creates, `Esc` cancels; **Create** is disabled for an empty name or an existing path. |
| **Open Level** | File → Open Level | `.icemap` picker → **Open** / **Cancel**. **Open** is disabled until an existing file is picked; `Esc` cancels. |
| **Unsaved Changes** | Exit with a dirty level | **Save** (save & quit) / **Don't Save** (quit) / **Cancel**. |

Double-clicking a `.icemap` in the Content Browser opens it directly, without the
dialog.

**Level Script.** **File → Level Script Editor** opens an editor for the level's own
script — the `OnLevelStart()`, `OnLevelUpdate(dt)` and `OnLevelEnd()` callbacks. It
is saved inside the `.icemap`.

* In a **code project** it is a Lua editor with syntax highlighting, a **Save**
  button, a **Compile** button with an inline `OK`/`Error` badge (the failing line
  is marked and the message shown as a tooltip), a cursor position read-out, and the
  shared Lua editing helpers (autocomplete and overlays). Its menu bar offers
  File → Save Level and Edit → Undo/Redo/Copy/Paste.
* In a **visual-scripting project** — or when the level already contains a saved
  graph — the same window hosts a **Visual Script graph** instead, with **Save**,
  **Compile** and a status line; the graph compiles to Lua on save.
* Pressing `Ctrl+S` while the Level Script window is focused **during Play mode**
  hot-reloads the script into the running game instead of saving the level.

**Text notes.** Plain `.txt` notes open in a small built-in text editor titled
`Text Note: <name>`, with File → Save (`Ctrl+S`) and Edit → Undo / Redo / Copy /
Cut / Paste / Select All (see [Assets](Assets-EN-DOC.md)).

---

## 15. Keyboard & mouse reference

**File & edit**

| Shortcut | Action |
| -------- | ------ |
| `Ctrl+S` | Save level (or hot-reload the level script during Play, when its window is focused) |
| `Alt+F4` | Exit |
| `Ctrl+Z` | Undo (scene, Content Browser files, or the focused asset editor) |
| `Ctrl+Y` / `Ctrl+Shift+Z` | Redo |
| `Ctrl+R` | Compile scripts + refresh assets |
| `Delete` | Delete selected |

**Selection**

| Shortcut | Action |
| -------- | ------ |
| `LMB` | Select entity |
| `Ctrl+Click` | Toggle entity in the multi-selection |
| `Shift+Click` | Range-select (in the Outliner) |
| `LMB` drag | Marquee (rectangle) select |
| `F` | Focus selected |
| `F2` | Rename entity or folder (in the Outliner) |
| `Ctrl+C / X / V` | Copy / cut / paste entities (Outliner or viewport) |
| `Ctrl+A` | Select all (in the Outliner) |

**Viewport — gizmos & camera**

| Shortcut | Action |
| -------- | ------ |
| `Q` / `E` / `R` | Translate / Scale / Rotate gizmo (viewport hovered) |
| `RMB + W/A/S/D` | Pan camera |
| `Scroll` | Zoom at the cursor |
| `Left Shift+Scroll` | Camera pan speed |
| `RMB` (short) | Context menu |
| `RMB` (held / dragged) | Pan instead of opening the menu |

**Play mode**

| Shortcut | Action |
| -------- | ------ |
| `F3` | Toggle Play mode |
| `F5` | Pause / resume |
| `F2` | Toggle free camera (Eject / Inject) |
| `Shift+F1` | Toggle cursor |

**Console**

| Shortcut | Action |
| -------- | ------ |
| `Ctrl+C` | Copy selected lines |
| `Ctrl+A` | Select all lines |
| `Ctrl+L` | Clear |
| `F3` / `Shift+F3` | Next / previous filter match |
| `Up` / `Down` | Browse input history (input bar) |
| `Enter` / `Esc` | Execute / clear the input bar |

**Dialogs**

| Shortcut | Action |
| -------- | ------ |
| `Enter` | Confirm (New Level) |
| `Esc` | Cancel / close |

**Tilemap Editor** (see [Assets](Assets-EN-DOC.md))

| Shortcut | Action |
| -------- | ------ |
| `LMB` / `RMB` | Paint / erase |
| `Alt+LMB` / `Alt+RMB` | Fill / pick |
| `Shift+RMB` / `Shift+LMB` | Select region / stamp |
| `Ctrl+S` | Save tilemap |

---

## 16. FAQ & troubleshooting

**My panel layout is broken — how do I reset it?**
Delete `imgui.ini` next to the editor and restart; the default layout is rebuilt.
Individual panels can be re-opened from the [Window](#33-window) menu.

**I closed the Console / Outliner / Properties — where did it go?**
Re-enable it from the **Window** menu (the Console is `Window → Show Console`).

**Why did my changes disappear after I stopped Play mode?**
Play mode runs on a **snapshot**; the scene is restored to its pre-Play state when
you press STOP. Make edits in edit mode, or **Eject** during Play knowing the edits
are temporary.

**`Ctrl+Z` undid the wrong thing.**
Undo follows focus. With the Content Browser focused it undoes the last **file**
operation; with an asset editor focused it undoes inside that editor; otherwise it
undoes the last **scene** change. Click the panel you mean first. Undo is disabled
entirely while Play mode is running.

**Changes to a class instance keep resetting on reload.**
Turn on **Override Class Defaults** for that entity in the
[Properties](#71-entity-header--class-overrides) panel, or edit the class itself so
all instances inherit the change.

**I can't find the Add Component button.**
There isn't one on an entity — components belong to the **class**. Open the entity's
class (double-click it in the Outliner, or **Edit Class** in the viewport context
menu), add the component there, and every instance picks it up. See
[7.2](#72-components).

**I edited a file outside the editor and nothing happened.**
The editor watches the whole `Content/` folder and reloads automatically — check
the **File Changes** window and the Console. If the file lives outside `Content/`,
or you want a forced pass, use **File → Update All Assets** (`Ctrl+R`). See
[2.4](#24-live-content-watching).

**A setting in Preferences had no effect.**
Press **Apply** (or **Apply All**) — most settings only change the running editor
when applied — and note that a few require a **restart**: MSAA changes, the
Optimization tab's shader-limit values, and the physics worker-thread count. Use
**Save** / **Save All** to make settings persist.

**This level needs different gravity/lighting than the rest of the game.**
Use [World Settings](#8-world-settings) to override physics and/or rendering for
that level only. You can even tune them while Play mode runs.

**Remote Preview never connects.**
It binds to localhost only, so the device must reach it through `adb reverse` —
use **Deploy to Device**, which sets that up, installs the companion app and
launches it. Also remember that frames are only sent **in Play mode**, and that the
feature is unavailable on macOS. See [Section 12](#12-remote-preview).

**Where are the Build, Profiler, Content Browser and scripting docs?**
Build/Profiler/DLC/Statistics → [Profiling & Building](Profiling-And-Building-EN-DOC.md);
Content Browser & assets → [Assets](Assets-EN-DOC.md);
Lua → [Lua API](LuaAPI-EN-DOC.md); Python → [Python API](PythonAPI-EN-DOC.md);
Plugins/Mods → [Plugins & Mods](Plugins-And-Mods-EN-DOC.md);
rendering/physics → [Graphics, Rendering & Physics](Graphics-EN-DOC.md).

---

<sub>© IceBoxCrew Studio. All rights reserved. See [`LICENSE.txt`](../../LICENSE.txt) for full terms.</sub>
