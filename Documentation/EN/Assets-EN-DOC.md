# 📦 IceBox Engine — Assets & Content Browser

## Full documentation in English

### Actual for PR-0.9.0 Version

> **IceBox Engine** organizes every piece of game data — textures, sounds, sprites,
> materials, tilemaps, particle effects, UI, cutscenes, AI and more — as **assets**
> that live inside your project's `Content/` folder and are managed through the
> built-in **Content Browser**.
>
> This document is a complete reference for the **asset system** and the
> **Content Browser**: how assets are stored on disk, what a *sidecar* is, how
> references and redirectors keep your project stable, every asset type and the editor
> panel that opens it, how importing works, and how the browser handles creation,
> renaming, moving, deleting, bulk-editing and migrating content between projects.
>
> Scripting is **not** covered here. The Lua and Python APIs have their own
> dedicated references:
> [LuaAPI-EN-DOC.md](LuaAPI-EN-DOC.md) and [PythonAPI-EN-DOC.md](PythonAPI-EN-DOC.md).
> Asset **cooking** (compression for shipping) and **building** are covered in the
> companion document
> [Profiling & Building Games](Profiling-And-Building-EN-DOC.md).

---

## 📑 Table of Contents

1. [Introduction & the asset model](#1-introduction--the-asset-model)
2. [How assets are stored on disk](#2-how-assets-are-stored-on-disk)
   - 2.1 [JSON-based assets](#21-json-based-assets)
   - 2.2 [Sidecars (companion files)](#22-sidecars-companion-files)
   - 2.3 [Asset references & redirectors](#23-asset-references--redirectors)
   - 2.4 [The virtual file system (IceVFS)](#24-the-virtual-file-system-icevfs)
   - 2.5 [External changes & auto-refresh](#25-external-changes--auto-refresh)
3. [The Content Browser](#3-the-content-browser)
   - 3.1 [Layout](#31-layout)
   - 3.2 [Navigation & the folder tree](#32-navigation--the-folder-tree)
   - 3.3 [Top bar & search](#33-top-bar--search)
   - 3.4 [Type filters](#34-type-filters)
   - 3.5 [Thumbnails & live previews](#35-thumbnails--live-previews)
   - 3.6 [Selecting items](#36-selecting-items)
   - 3.7 [Creating assets](#37-creating-assets)
   - 3.8 [The item context menu](#38-the-item-context-menu)
   - 3.9 [Importing external files](#39-importing-external-files)
   - 3.10 [Moving, copying & renaming (sidecar-aware)](#310-moving-copying--renaming-sidecar-aware)
   - 3.11 [Deleting assets & reference safety](#311-deleting-assets--reference-safety)
   - 3.12 [The Asset References viewer](#312-the-asset-references-viewer)
   - 3.13 [Undo / Redo of file operations](#313-undo--redo-of-file-operations)
   - 3.14 [Bulk editing — Property Matrix](#314-bulk-editing--property-matrix)
   - 3.15 [Dragging assets into the scene](#315-dragging-assets-into-the-scene)
   - 3.16 [Migrating assets to another project](#316-migrating-assets-to-another-project)
   - 3.17 [Keyboard & mouse reference](#317-keyboard--mouse-reference)
4. [Asset type reference](#4-asset-type-reference)
   - [Common editor behavior](#common-editor-behavior)
   - 4.1 [Source files & their sidecars](#41-source-files--their-sidecars)
     - 4.1.1 [Textures and the `.ice_texture` sidecar](#411-textures-and-the-ice_texture-sidecar)
     - 4.1.2 [Audio and the `.ice_sound` sidecar](#412-audio-and-the-ice_sound-sidecar)
     - 4.1.3 [Fonts and the `.ice_font` sidecar](#413-fonts-and-the-ice_font-sidecar)
     - 4.1.4 [Video and the `.ice_video` sidecar](#414-video-and-the-ice_video-sidecar)
   - 4.2 [Sprite (`.ice_sprite`)](#42-sprite-ice_sprite)
     - 4.2.1 [The Spritesheet Slicer](#421-the-spritesheet-slicer)
   - 4.3 [Flipbook (`.ice_flipbook`)](#43-flipbook-ice_flipbook)
   - 4.4 [Animation / State Machine (`.ice_animation`)](#44-animation--state-machine-ice_animation)
   - 4.5 [Skeleton (`.ice_skeleton`)](#45-skeleton-ice_skeleton)
   - 4.6 [Tileset (`.ice_ts`)](#46-tileset-ice_ts)
   - 4.7 [Tilemap (`.ice_tm`)](#47-tilemap-ice_tm)
   - 4.8 [Material (`.ice_material`)](#48-material-ice_material)
     - 4.8.1 [Material node reference](#481-material-node-reference)
   - 4.9 [Material Instance (`.ice_matinst`)](#49-material-instance-ice_matinst)
   - 4.10 [Material Function (`.ice_matfunc`)](#410-material-function-ice_matfunc)
   - 4.11 [Material Parameter Collection (`.ice_mpc`)](#411-material-parameter-collection-ice_mpc)
   - 4.12 [FX / Particle System (`.ice_fx`)](#412-fx--particle-system-ice_fx)
     - 4.12.1 [Emitter settings](#4121-emitter-settings)
     - 4.12.2 [FX module reference](#4122-fx-module-reference)
   - 4.13 [Widget / UI (`.ice_widget`)](#413-widget--ui-ice_widget)
     - 4.13.1 [Shared element properties](#4131-shared-element-properties)
     - 4.13.2 [Element type reference](#4132-element-type-reference)
   - 4.14 [View / Post-Process Volume (`.ice_view`)](#414-view--post-process-volume-ice_view)
   - 4.15 [Cinema (`.ice_cinema`)](#415-cinema-ice_cinema)
   - 4.16 [AI / Behavior Tree (`.ice_ai`)](#416-ai--behavior-tree-ice_ai)
   - 4.17 [Class (`.ice_class`)](#417-class-ice_class)
     - 4.17.1 [Component reference](#4171-component-reference)
   - 4.18 [Localization (`.ice_localization`)](#418-localization-ice_localization)
   - 4.19 [Level (`.icemap`)](#419-level-icemap)
   - 4.20 [Script (`.lua`) & Text (`.txt`)](#420-script-lua--text-txt)
   - 4.21 [Decal (`.ice_decal`)](#421-decal-ice_decal)
5. [Importers](#5-importers)
   - 5.1 [Aseprite importer](#51-aseprite-importer)
   - 5.2 [GIF importer](#52-gif-importer)
6. [Asset cooking — overview](#6-asset-cooking--overview)
7. [Quick reference tables](#7-quick-reference-tables)
   - 7.1 [Extensions, types & editors](#71-extensions-types--editors)
   - 7.2 [Source file → sidecar map](#72-source-file--sidecar-map)
   - 7.3 [Auto-prefixes on import](#73-auto-prefixes-on-import)
   - 7.4 [Default names for new assets](#74-default-names-for-new-assets)
8. [FAQ & troubleshooting](#8-faq--troubleshooting)

---

## 1. Introduction & the asset model

In IceBox Engine an **asset** is any reusable piece of game content stored as a file
inside your project's `Content/` directory. Levels reference assets, assets reference
other assets, and the engine loads them on demand at edit time and at runtime.

The asset system is built around a few deliberate design choices:

| Principle | What it means in practice |
| --------- | ------------------------- |
| **Human-readable** | Engine-native assets are plain **JSON** text. They diff cleanly in version control and can be inspected or hand-edited. |
| **Path-based references** | Assets reference each other by **relative path** (e.g. `Content/Sprites/Hero.ice_sprite`), normalized to forward slashes. |
| **Non-destructive import** | Raw source files (`.png`, `.wav`, `.ttf`, `.mp4`) are *never* modified. Import settings live in a separate **sidecar** file next to the source. |
| **Move-safe** | When you rename or move an asset, the browser rewrites references in dependent assets and, when needed, drops a **redirector** so nothing breaks. |
| **Pack-on-ship** | Loose files during development; an optional single compressed **IcePak** archive (`.icepak`) for shipping builds — with identical runtime loaders. |

The two halves of the system are:

* **The asset files & sidecars** — the on-disk representation (Section 2 & 4).
* **The Content Browser** — the editor panel that creates, organizes, imports and
  edits them (Section 3).

Each asset type has a dedicated **editor panel** that opens when you double-click the
asset. The full type-by-type catalogue, including which editor opens for each, is in
[Section 4](#4-asset-type-reference) and the [quick-reference table](#71-extensions-types--editors).

---

## 2. How assets are stored on disk

### 2.1 JSON-based assets

All engine-native assets (everything beginning with `.ice_`, plus `.icemap` levels)
are serialized as **pretty-printed JSON** (4-space indentation). Saving and loading
go through a shared serializer that:

* writes JSON to the asset path;
* on load, first looks for the file on disk, then falls back to the mounted
  **IceVFS** archive, then resolves through the **redirector** registry, and finally
  through the platform I/O layer (so the same code path works for loose files, packed
  builds, and Android/Web embedded assets).

A minimal asset therefore looks like ordinary JSON. For example, a **Flipbook**
(`.ice_flipbook`) on disk:

```json
{
    "DefaultFPS": 12.0,
    "Loop": true,
    "MaterialMode": 0,
    "OverrideShadingMode": 0,
    "OverrideBlendMode": 0,
    "OverrideAlphaClipThreshold": 0.5,
    "OverrideMaterialPath": "",
    "Frames": [
        { "SpritePath": "Content/Sprites/Run_0.ice_sprite", "Duration": 0.0833 },
        { "SpritePath": "Content/Sprites/Run_1.ice_sprite", "Duration": 0.0833 }
    ]
}
```

> **You normally never hand-edit these.** Each asset has a purpose-built editor. The
> JSON format is documented here so you understand version-control diffs and the
> relationship between assets.

### 2.2 Sidecars (companion files)

Source media — images, audio, fonts and video — are **not** engine-native assets.
They keep their original format untouched. Instead, the engine stores their *import
settings* in a **sidecar**: a hidden JSON file that sits next to the source file and
shares its base name with a different extension.

| Source file | Sidecar | Holds |
| ----------- | ------- | ----- |
| `T_Hero.png` | `T_Hero.ice_texture` | Filtering, wrapping, mipmaps, sRGB, max size, pixel format, atlas flag … |
| `S_Jump.wav` | `S_Jump.ice_sound` | Volume, pitch, group, 3D attenuation, filters, reverb … |
| `F_Title.ttf` | `F_Title.ice_font` | Default size, antialias, glyph ranges, atlas size … |
| `V_Intro.mp4` | `V_Intro.ice_video` | Width, height, FPS, duration (probed metadata) |

Sidecars are **hidden** in the Content Browser — you never see `.ice_texture` /
`.ice_sound` / `.ice_font` / `.ice_video` entries. Instead you see the source file
(e.g. the `.png`) and edit its settings through the appropriate panel
(Texture Settings, Sound Settings, Font Editor, Video Player). The sidecar is created
automatically on import (Section [3.9](#39-importing-external-files)) and is always
moved, copied, renamed and deleted **together** with its source file
(Section [3.10](#310-moving-copying--renaming-sidecar-aware)).

> Sidecars are also the input to **cooking**: e.g. a texture's `sRGB` and
> `MaxTextureSize` flags, or a font's declared glyph ranges, drive what the cooker
> produces for a shipping build. See
> [Profiling & Building Games](Profiling-And-Building-EN-DOC.md).

### 2.3 Asset references & redirectors

Because assets reference each other by **path**, moving or renaming an asset would
normally break every asset that points at it. IceBox prevents this with the
**Asset Redirector** system.

When you move or rename an asset through the Content Browser, the engine:

1. **Rewrites references** inside every dependent asset under `Content/` so they point
   at the new path.
2. If anything still referenced the old path, it writes a **redirector** — a small
   `.ice_redirect` file recorded in **`Saved/Redirectors/`** that maps
   `OldPath → NewPath`. Any later load of the old path is transparently resolved to
   the new one.
3. Clears caches and asks the asset manager to refresh.

Redirector files store the original path, the new path, and a timestamp; the file name is
the original path encoded so one flat folder can hold the whole registry. Redirectors live
**outside** `Content/` on purpose — they are project metadata, not shipped content, so they
never appear in the browser, never end up in a build, and never confuse a reference scan.

The registry is loaded once and kept in memory. On load the engine:

* **Prunes stale redirectors** — if the target of a redirector no longer exists, the
  redirector file is deleted.
* **Migrates legacy redirectors** — any `.ice_redirect` files left inside `Content/` by
  older engine versions are moved into `Saved/Redirectors/` automatically.

Redirector chains are followed up to a safety limit (20 hops). You can **consolidate**
redirectors (collapse chains, rewrite the files that still point at old paths, and remove
what is no longer needed) from the empty-space context menu — see
[Section 3.7](#37-creating-assets); the menu entry shows the current redirector count and
is disabled when there are none. The engine also consolidates redirectors automatically
right before a build.

The redirector subsystem also powers the **Asset References viewer**
(Section [3.12](#312-the-asset-references-viewer)), which scans the project to list
what references an asset (incoming) and what an asset references (outgoing).

### 2.4 The virtual file system (IceVFS)

At runtime a game can read assets either as **loose files** under `Content/` or from
one or more mounted **IcePak** archives (`.icepak`). The **IceVFS** layer makes both
transparent:

* Paks are mounted as **layers**; later mounts override earlier ones (so DLC or patch
  paks can shadow base content).
* Every read tries mounted paks first, then resolves through the redirector registry,
  then falls back to a loose file on the platform file system.

This is why the *same* asset-loading code works whether you run loose in the editor,
ship loose files, or ship a packed `Content.icepak`. The IcePak format and packing
options are covered in
[Profiling & Building Games](Profiling-And-Building-EN-DOC.md).

### 2.5 External changes & auto-refresh

The editor **watches the whole `Content/` folder** while it is open, so you do not have to
work exclusively through the Content Browser. Every asset extension is watched — all
`.ice_*` types, `.icemap`, images, audio, fonts, video, `.lua` and `.txt` — including
sub-folders.

When a file is **created** outside the editor (copied in with Explorer/Finder, produced by
an external tool, pulled in by version control), the engine does the same sidecar work the
importer would: a missing `.ice_texture`, `.ice_sound`, `.ice_font` or `.ice_video` is
generated with default settings. Dropping a `.png` straight into `Content/Textures/` is
therefore a perfectly valid way to add art.

When a file is **created, modified or deleted**, the editor:

* refreshes the asset manager, so open editors, thumbnails and the viewport pick up the new
  contents;
* reloads every class instance in the open level, so an edited class updates in place;
* reloads the game localization tables when a `.ice_localization` changed;
* lists what changed in a small **file-changes** notification window (each entry stays for a
  few seconds; **Clear** empties the list).

A short cooldown after the editor's own writes stops its own saves from triggering a reload
storm. Note that the watcher **does not** rewrite references — moving or renaming assets
outside the editor still breaks links, because nothing rewrites the paths or drops a
redirector. Reorganize through the Content Browser
([3.10](#310-moving-copying--renaming-sidecar-aware)); use the file system for *adding* and
*editing* files.

---

## 3. The Content Browser

The **Content Browser** (`Window → Content Browser`) is the central hub for all
project content. It is rooted at your project's `Content/` folder and only ever
operates inside it.

### 3.1 Layout

The panel is split into a **top bar** and two resizable columns:

```
┌─────────────────────────────────────────────────────────────────┐
│ [Back]  Current: Content/Sprites     [Search…] [X]  [+ Import]  │   ← Top bar
├──────────────┬──────────────────────────────────────┬───────────┤
│ Folder tree  │            Content area              │  Filters  │
│ ▸ Content    │   ▢ T_Hero   ▢ Hero.ice_sprite  …    │  ☐ Levels │
│   ▸ Sprites  │   ▢ …        ▢ …                     │  ☐ Sprites│
│   ▸ Audio    │                                      │  ☐ Audio  │
│   …          │                                      │  …        │
└──────────────┴──────────────────────────────────────┴───────────┘
```

* **Left column** — the recursive **folder tree** rooted at `Content`.
* **Right column** — the **content area** (a grid of thumbnails for the current
  folder) plus a thin **filter panel** along its right edge.

### 3.2 Navigation & the folder tree

* The **folder tree** mirrors the directory structure under `Content/`. Click a folder
  to make it the current directory; the tree auto-expands to reveal the current folder.
* **Double-click a folder** in the content area to enter it; **double-click an asset**
  to open its editor (Section [3.8](#38-the-item-context-menu) lists the per-type
  editors). Entering a folder also **clears the search box**, so you always land on the
  folder's real contents.
* The top-bar **Back** button (shown when you're below the root) goes to the parent
  folder.
* You can **drag items onto folders** — in the tree or in the content area — to move
  them (Section [3.10](#310-moving-copying--renaming-sidecar-aware)). Dropping onto the
  **Content** root moves items to the top level.
* The tree remembers which folders you expanded, and it auto-expands the whole path down
  to the current folder whenever the view jumps somewhere (for example from the
  [reference viewer](#312-the-asset-references-viewer) or from a script).

### 3.3 Top bar & search

| Control | Function |
| ------- | -------- |
| **Back** | Navigate to the parent folder (hidden at the root). |
| **Current: …** | Shows the current directory path. |
| **Search…** | Live, case-insensitive filter by file name — see below. |
| **X** | Clears the search box (only shown while the box has text). |
| **+ Import** (green) | Opens a native multi-file picker to import external files into the current folder — see [Section 3.9](#39-importing-external-files). |

**Search is recursive.** As soon as you type something, the browser stops listing only
the current folder:

| Where you are | What the search covers |
| ------------- | ---------------------- |
| The **Content** root | The **whole project** — every folder, recursively. |
| Any **sub-folder** | That folder **and everything under it**. |

Matches from deeper folders are labeled with their **path relative to the search root**
(e.g. `Enemies/Boss/SP_Boss`) instead of just the file name, so you always know where a
result actually lives. Clear the search (or double-click a folder) to return to the plain
one-folder listing.

### 3.4 Type filters

The narrow **filter panel** on the right of the content area lets you show only
certain asset categories. Each filter is a color-coded toggle; enabling one or more
filters restricts the view to those types (combined with the search box). Available
filters:

`Levels`, `Classes`, `Views`, `Cinemas`, `Sprites`, `Flipbooks`, `Animations`,
`Skeletons`, `Tilesets`, `Tilemaps`, `Materials` (material / instance / function /
collection), `FX`, `Widgets`, `Textures`, `Audio`, `Fonts`, `Localization`, `AI`,
`Video`, `Scripts`, `Text`.

The **All** button at the top of the panel clears every filter at once; it is highlighted
green while no filter is active.

Like search, **an active type filter makes the view recursive** — it gathers matching
assets from the current folder and everything below it (from the whole project when you
are at the root). Folders themselves are hidden while a filter is on, because the result
is a flat list of assets, not a directory listing.

With **no** filter active, everything is shown (sidecars and redirector files remain
hidden).

### 3.5 Thumbnails & live previews

**Folders are always listed first**, then files, and the content area renders rich,
type-aware thumbnails:

* **Textures** show the image itself (with a checkerboard behind transparency), honoring
  the texture's filtering settings and its aspect ratio.
* **Sprites** show only their `SourceRect` region of the texture, over a checkerboard —
  so a sprite cut out of a sheet looks exactly like the sprite, not like the sheet.
* **Flipbooks** show their first frame and **animate while you hover them**, at the
  flipbook's own per-frame timing. Move the pointer away and the preview resets.
* **Audio** files get a **play button** in the middle of the tile: click it to preview the
  clip (using its `.ice_sound` settings, forced to non-spatial and non-looping); the icon
  turns into a stop button while it plays, and clicking again stops it. Only one preview
  plays at a time, and it stops on its own when the clip ends.
* **Video** files show the decoded **first frame** framed by a film-strip border.
* **Fonts** are rendered live as an `Aa` sample using the font's own settings.
* **Tilesets** show the source texture with the **tile grid** drawn on top (using the
  tileset's tile size), inside an orange frame.
* **Tilemaps** are rendered live — the painted layers, plus a green cell grid overlay
  (subsampled for very large maps).
* **Widgets** are rendered live as a miniature of the canvas: element rectangles, colors,
  sprites/flipbooks, text, nine-slice, toggles, checkboxes and progress/slider fills,
  including elements **inherited from a parent widget** and the results of
  [Desired Size](#4131-shared-element-properties) layout.
* **FX** assets show a small static particle preview built from the emitter's modules.
* **Materials and classes** are rendered **live** into off-screen thumbnails by dedicated
  mini-renderers, so a material thumbnail shows the actual compiled shader (with a preview
  light) and a class thumbnail shows the assembled entity.
* Anything without a preview (levels, scripts, notes, AI, Cinema, Views, material
  instances/functions/collections, plugin types) shows a colored tile with its **type
  badge**.

Thumbnails and parsed asset data are cached. The cache re-checks file timestamps about
twice a second and rebuilds only what changed, and the texture cache is trimmed once it
grows past its budget, so a folder with thousands of assets stays responsive. Off-screen
rows are skipped entirely.

**Hovering** an asset for half a second shows a tooltip with its type
(e.g. `.ice_sprite (Sprite)`).

Asset **type badges** (two-letter abbreviations such as `SP`, `MA`, `TM`) and
type colors help identify items at a glance — see the
[quick-reference table](#71-extensions-types--editors).

### 3.6 Selecting items

| Action | Result |
| ------ | ------ |
| **Click** | Select a single item. |
| **Ctrl + Click** | Toggle an item in/out of the selection. |
| **Shift + Click** | Range-select from the last clicked item. |
| **Ctrl + A** | Select everything currently visible (respects search and filters). |
| **Drag on empty space** | **Marquee (rubber-band) select** — every tile the box touches is selected. Hold **Ctrl** to add the marquee result to the existing selection instead of replacing it. |
| **Click empty space** | Clear the selection (a `Ctrl+click` keeps it). |
| **Drag an item** | Begin a move (or drag into the scene — see [3.15](#315-dragging-assets-into-the-scene)). |

### 3.7 Creating assets

**Right-click empty space** in the content area to open the *create* menu. New assets
are created in the current folder:

* **New Folder**
* **(Plugin types)** — any asset type registered by an installed plugin appears here, at
  the top of the menu. A plugin supplies the extension, display name, badge text and tile
  color, so its assets look and behave like built-in ones everywhere in the browser; see
  [Plugins & Mods](Plugins-And-Mods-EN-DOC.md).
* **Logic ▸**
  * **Create Ice Class** (`.ice_class`) — a reusable game object with components and scripting.
  * **Create Cinema** (`.ice_cinema`) — a camera/event timeline (cutscene).
  * **Create AI Brain** (`.ice_ai`) — a behavior tree.
* **Graphics ▸**
  * **Create Sprite** (`.ice_sprite`)
  * **Create Tileset** (`.ice_ts`)
  * **Create Tilemap** (`.ice_tm`)
  * **Create Flipbook** (`.ice_flipbook`)
  * **Create Animation** (`.ice_animation`)
  * **Create Skeleton** (`.ice_skeleton`)
  * **Create FX Effect** (`.ice_fx`)
  * **Create View** (`.ice_view`)
* **Materials ▸**
  * **Create Material** (`.ice_material`)
  * **Create Material Parameter Collection** (`.ice_mpc`)
  * **Create Material Function** (`.ice_matfunc`)
  * **Create Material Instance** (`.ice_matinst`)
  * **Create Decal** (`.ice_decal`)
* **UI ▸**
  * **Create Interface Widget** (`.ice_widget`)
  * **Create Localization** (`.ice_localization`)
* **Other ▸**
  * **Create Lua Script** (`.lua`)
  * **Create Text Note** (`.txt`)
* **Fix Up Redirectors (n)** — collapse and clean up the redirector registry. The
  number in the label is how many redirectors exist right now; the entry is disabled when
  that number is `0`.

New assets get a **type prefix and a unique default name** (`SP_NewSprite`,
`M_NewMaterial`, `CL_NewActor`, …) and a numeric suffix if that name is taken — see
[Section 7.4](#74-default-names-for-new-assets). Creation is recorded on the undo stack
([3.13](#313-undo--redo-of-file-operations)).

> Some asset types are also created **contextually** from a source file — for example,
> right-clicking a texture offers *Create Sprite (.ice_sprite)* / *Create Tileset (.ice_ts)*. See
> [Section 3.8](#38-the-item-context-menu).

### 3.8 The item context menu

**Right-click an item** for actions that depend on its type. The full set includes:

| Action | Available on | Effect |
| ------ | ------------ | ------ |
| **Create Sprite (.ice_sprite)** | textures | Make a `.ice_sprite` (`SP_<name>`) from the whole image. |
| **Create Tileset (.ice_ts)** | textures | Make a `.ice_ts` (`TS_<name>`) from the image, tile size 64. |
| **Slice Spritesheet…** | textures | Open the [Spritesheet Slicer](#421-the-spritesheet-slicer) to cut the sheet into sprites. |
| **Import Aseprite Spritesheet** | an Aseprite `.json` in the project | Build sprites/flipbook from Aseprite data — see [5.1](#51-aseprite-importer). |
| **Create Tilemap (.ice_tm)** | tilesets | Make a `.ice_tm` (`TM_<name>`) that already uses this tileset in its first layer. |
| **Create Child Class** | classes | Make a subclass that inherits the parent class (and a stub script that calls back into it). |
| **Create Child Widget** | widgets | Make a widget that inherits the parent's canvas settings and elements. |
| **Create Material Instance (.ice_matinst)** | materials | Make a `.ice_matinst` parented to the material. |
| **Create Decal (.ice_decal)** | textures | Make a `.ice_decal` (`DC_<name>`) that already uses the image as its first texture variant. |
| **Create Flipbook from Sprite(s)** | one or more sprites selected | Build a `.ice_flipbook` from the selected sprites, ordered by file name. |
| **Bulk Edit via Property Matrix** | 2+ assets of the same kind | Open the [Property Matrix](#314-bulk-editing--property-matrix). |
| **Rename** | a single item | Rename, moving the sidecar and fixing references. |
| **Copy / Cut / Paste** | any | Clipboard operations (`Ctrl+C` / `Ctrl+X` / `Ctrl+V`). |
| **Asset References** | a single file | Open the [reference viewer](#312-the-asset-references-viewer). |
| **Migrate To...** | any | Copy the selection **and its dependencies** into another project — see [3.16](#316-migrating-assets-to-another-project). |
| **Delete** | any | Delete with reference-safety checks ([3.11](#311-deleting-assets--reference-safety)). |

Right-clicking an item that is **not** part of the current selection selects it first, so
the menu always acts on what you see highlighted. The type-specific entries at the top
appear only for a single selected item; the clipboard, migrate and delete entries work on
the whole selection.

**Double-clicking** an asset opens the matching editor:

| Asset | Opens |
| ----- | ----- |
| `.png` / `.jpg` | **Texture Settings** |
| `.wav` / `.mp3` / `.ogg` / `.flac` | **Sound Settings** |
| `.ttf` / `.otf` | **Font Editor** |
| `.mp4` / `.webm` … | **Video Player** |
| `.ice_sprite` | **Sprite Editor** |
| `.ice_flipbook` | **Flipbook Editor** |
| `.ice_animation` | **Animation Editor** |
| `.ice_skeleton` | **Skeleton Editor** |
| `.ice_ts` | **Tileset Editor** |
| `.ice_tm` | **Tilemap Editor** |
| `.ice_material` | **Material Editor** |
| `.ice_matinst` | **Material Instance Editor** |
| `.ice_matfunc` | **Material Function Editor** |
| `.ice_mpc` | **MPC Editor** |
| `.ice_decal` | **Decal Editor** |
| `.ice_fx` | **FX Editor** |
| `.ice_widget` | **Widget Editor** |
| `.ice_view` | **View Editor** |
| `.ice_cinema` | **Cinema Editor** |
| `.ice_ai` | **AI Editor** |
| `.ice_class` | **Class Editor** |
| `.ice_localization` | **Localization Creator** |
| `.lua` | **Lua Script Editor** |
| `.txt` | **Text Note editor** |
| `.icemap` | Opens the **level** in the main Viewport |

Opening an asset that is already open **focuses the existing panel** instead of opening a
second one; different assets of the same type each get their own panel — see
[Common editor behavior](#common-editor-behavior).

### 3.9 Importing external files

There are two ways to bring outside files in:

1. **Drag & drop** files from the OS onto the Content Browser.
2. The **+ Import** button in the top bar (native multi-file picker).

**Where a dropped file lands.** While you drag files over the editor, the folder under the
cursor is highlighted — in the folder tree *or* in the content grid — and that is where the
files go. Drop anywhere else in the panel and they go into the **current** folder. The
**+ Import** button always imports into the current folder.

On import, the engine:

* **Auto-prefixes** the file name by media type so content stays organized:
  `T_` for textures, `S_` for audio, `F_` for fonts, `V_` for video (skipped if the
  name already has the prefix).
* **Auto-creates the sidecar** with default settings: `.ice_texture` for images,
  `.ice_sound` for audio, `.ice_font` for fonts, `.ice_video` for video (the latter is
  probed for width/height/FPS/duration via FFmpeg when available).
* For **`.gif`** files, runs the [GIF importer](#52-gif-importer) (produces a flipbook
  of sprites) instead of a plain copy.
* For **`.json`** files that are **Aseprite** exports, runs the
  [Aseprite importer](#51-aseprite-importer) (locating the matching PNG automatically).
  A `.json` that is *not* an Aseprite export is reported in the Console and skipped.
* Records the import on the undo stack, so a mistaken drop is one `Ctrl+Z` away.

**If the name is already taken**, a **file-conflict dialog** appears with three choices:

| Choice | Effect |
| ------ | ------ |
| **Replace** | Overwrite the existing file (the overwritten version is pushed onto the undo stack first, so it can be restored). |
| **Skip** | Leave the existing file alone and drop this one. |
| **Keep Both** | Import under a unique name (`name_1`, `name_2`, …), sidecars included. |

Tick **Apply to all conflicts** to answer once for a whole batch; otherwise the dialog reappears for
each conflicting file in turn. The same dialog handles name collisions when you **move** or
**paste** assets inside the project.

### 3.10 Moving, copying & renaming (sidecar-aware)

All structural operations understand sidecars and references:

* **Move** (drag onto a folder, or Cut + Paste) — moves the asset **and its sidecar**
  together, updates references in dependent assets, and creates a redirector if the old
  path was still referenced. Moving a **folder** rewrites the paths of everything inside
  it in one batch.
* **Copy** (Copy + Paste) — duplicates the asset **and its sidecar**; name collisions
  are auto-resolved with a unique suffix. Pasting into the folder the item already lives in
  is a no-op — paste into a *different* folder to duplicate. A cut clipboard is cleared once
  it has been pasted; a copied one stays, so you can paste it into several folders.
* **Rename** (`F2`, or **Rename** in the context menu) — a small dialog opens with the
  name pre-filled and the text field already focused; press `Enter` to confirm or `Esc` to
  cancel. You type the **base name only** — the extension is preserved automatically (for
  folders the whole name is editable). Renaming also renames the sidecar to match, updates
  references, and (if needed) drops a redirector. Type caches/thumbnails for the renamed
  asset are invalidated so previews refresh.

A redirector is created **only if something still points at the old path** after the
rewrite pass, so a tidy project accumulates almost none.

Because these operations are reference-aware, you can reorganize your `Content/` folder
freely without breaking levels or cross-asset links. Every one of them is undoable
([3.13](#313-undo--redo-of-file-operations)) while the browser is focused, and a batch
(multi-selection move, paste of several files) undoes as a **single step**.

### 3.11 Deleting assets & reference safety

Deleting opens a confirmation dialog that first **scans the whole project for references**
to everything you are about to delete.

**If nothing references them**, you get a green "safe to delete" note and a simple
**Yes / Cancel** confirmation (`Enter` confirms, `Esc` cancels).

**If something does**, the dialog lists every referencing asset (labeled with its type;
hover a row for the full path), warns that those references will break, and offers a
**Replace references with** [asset picker](#common-editor-behavior) limited to the same extension(s)
as the assets being deleted. Your choices are then:

| Choice | Effect |
| ------ | ------ |
| **Replace & Delete** | Repoint every reference at the replacement asset, then delete. Shown once a valid replacement is picked. |
| **Force Delete** | Delete anyway. Dependents that pointed at it will fail to resolve (or fall back to a redirector if one exists). |
| **Cancel** | Abort. |

Notes:

* Deleting a **folder** collects and deletes all assets inside it (with the same checks).
* **Sidecars** are removed alongside their source files.
* Deleting a **class file** also deletes the classes that inherit from it, recursively — a
  child class cannot exist without its parent. They are collected when the deletion runs,
  **after** you confirm, so the dialog itself lists only what you selected. Use
  [Asset References](#312-the-asset-references-viewer) on a base class first if you are
  unsure how many classes derive from it.
* Deleting is recorded on the undo stack **with the file contents**, so `Ctrl+Z` restores
  the files (whole folder trees included, and the removed child classes) and their sidecars.

### 3.12 The Asset References viewer

The **Asset References** action (item context menu, single file) opens a dialog with two
lists:

* **Referenced by** (incoming) — every asset that points *at* this asset, labeled with
  the referencing asset's type.
* **References to** (outgoing) — every asset that this asset points *to*.

Hover a row to see the full path. **Double-click** any entry to jump to that asset in the
browser (the view navigates to its folder, expands the tree and selects it). This is the
fastest way to understand an asset's dependencies before moving or deleting it.

### 3.13 Undo / Redo of file operations

The Content Browser records structural operations (create, rename, move, copy, delete,
paste, import, overwrite) on a **file-operation undo stack**, so you can reverse an
accidental reorganization.

`Ctrl+Z` and `Ctrl+Y` (or `Ctrl+Shift+Z`) act on **that** stack while the Content Browser is
focused, and on the **scene** undo stack otherwise — so undoing a file move never undoes an
entity move by accident, and vice versa.

Undo/redo restores files (and their sidecars, and whole folder trees with their contents)
and re-applies or rolls back the associated reference updates. Operations that touched many
files at once are grouped into a **single batch** and undo together. The stack keeps the
last 50 entries (never splitting a batch).

### 3.14 Bulk editing — Property Matrix

Select several assets **of the same kind**, right-click, and choose
**Bulk Edit via Property Matrix** to open the **Property Matrix** panel.

It is not a spreadsheet — it is a **merged property sheet**. The panel shows one set of
controls for the whole selection; changing any control writes that value into **every**
selected asset. Where the selected assets currently disagree, the field is highlighted and
shows `---` (checkboxes get a `(*)` marker), so you can see at a glance which properties
are uniform and which are not.

| Control | Meaning |
| ------- | ------- |
| **Selected assets** (collapsible) | The list of files you are editing. |
| **Apply to All** | Write the current values back to every selected asset file. |
| **Revert All** | Reload from disk and discard the pending edits. |
| unsaved marker | Shown while there are unwritten changes. |

The panel has its own undo/redo (`Ctrl+Z` / `Ctrl+Y` while it is focused).

Supported kinds — the panel picks the right property set automatically and refuses mixed
selections:

| Kind | What you can bulk-edit |
| ---- | ---------------------- |
| **Textures** | The full `.ice_texture` sidecar: filtering, wrapping, mipmaps, sRGB, premultiply, max size, anisotropy, LOD bias, pixel format, compression, atlas. |
| **Sprites** | Pivot, material mode + shading/blend/alpha clip, collision flags, density/friction/restitution, events, one-way, collision group and mode. |
| **Sounds** | The whole `.ice_sound` definition: playback, group, priority, trim & fades, randomization, 3D/attenuation/cone, and the DSP chain. |
| **Flipbooks** | Default FPS, loop, material mode and the shading/blend/alpha-clip overrides. |
| **Tilesets** | Tile size, material, shared render attributes, and the shared tile physics/shadow defaults. |
| **Tilemaps** | Map settings, chunking, show-coordinates and the animated-tile playback settings. |
| **Classes** | Component instances by type — colliders, lights, and other repeated instances across the selected classes. |
| **Widgets** | Canvas settings (size, desired size, scale-with-screen, stretch mode, safe area). |
| **FX** | Emitter settings across every emitter of every selected effect. |
| **Decals** | Appearance, rotation, lifetime and placement of the selected decal assets, plus the weight and source rect of every texture variant inside them. |

This is ideal for large, repetitive edits that would be tedious one asset at a time — for
example switching a hundred pixel-art textures to `Nearest` filtering, or giving every
footstep sound the same group and attenuation.

### 3.15 Dragging assets into the scene

Drag an asset from the content area into the **Viewport** or **Level Outliner** to use
it in the level. The browser tags the drag with a payload the rest of the editor
understands:

* **Classes** (`.ice_class`) use a dedicated payload so dropping one **instantiates the
  class** as a new entity.
* Other assets use a generic payload that the drop target interprets by type (e.g.
  dropping a sprite creates a sprite entity; dropping a material assigns it).

The same payload is what every **asset picker** in the editor accepts, so you can drag a
sprite straight from the browser onto a *Sprite* field in the Properties panel, the Class
Editor, the Widget Editor and so on — the picker only accepts assets of a type it allows.

Multi-selections are dragged as a group (the drag preview shows how many items).

### 3.16 Migrating assets to another project

**Migrate To...** (item context menu) copies the selected assets — **together with
everything they depend on** — into another IceBox project, keeping their folder layout.

1. Select any mix of assets and folders and choose **Migrate To...**. Folders are expanded
   into the assets they contain.
2. The dialog lists the result in two groups:
   * **Selected assets** — what you picked.
   * **Dependencies** — everything those assets reference, found by scanning the project.
     Use **Select All** / **Deselect All** to include or exclude the dependency group
     (the selected assets themselves stay checked).
3. Pick the **target project** from the drop-down — it is populated from the projects
   known to the Launcher, minus the one you have open — or press **Browse…** to point at
   any project folder. (Picking a project's `Content` folder is understood as picking the
   project.) Choosing the project you already have open is refused.
4. Rows that already exist in the target turn **orange**, and a warning tells you how many
   files would be overwritten. Uncheck them if you'd rather keep the target's versions.
5. Press **Migrate**. Each asset is copied to the *same path relative to `Content/`* in
   the target project (missing folders are created), **sidecars included**. A status line
   reports how many were copied and how many failed.

The button stays disabled until at least one item is checked and a target is chosen. The
operation only ever **writes into the target project** — nothing in the current project is
modified — and it is not part of the undo stack, so review the list before confirming.

### 3.17 Keyboard & mouse reference

Shortcuts apply while the Content Browser is focused and no dialog or text field is
active.

| Input | Action |
| ----- | ------ |
| `Ctrl + C` | Copy the selection. |
| `Ctrl + X` | Cut the selection. |
| `Ctrl + V` | Paste into the current folder (moves on cut, duplicates on copy). |
| `Ctrl + A` | Select everything currently visible. |
| `Delete` | Delete the selection (opens the confirmation dialog). |
| `F2` | Rename the selected item. |
| `Ctrl + Z` / `Ctrl + Y` (or `Ctrl + Shift + Z`) | Undo / redo the last file operation — the browser must be focused, otherwise these undo scene edits. |
| `Esc` | Close the open dialog. |
| `Enter` | Confirm the rename / delete dialog. |
| Double-click | Enter a folder / open an asset's editor. |
| Click + drag on empty space | Marquee-select. |
| `Ctrl` + marquee | Add to the current selection. |

---

## 4. Asset type reference

This section documents every asset type: what it is, its on-disk extension, its sidecar
(if any), the editor that opens it, and the data it stores. Field names match the JSON
keys so you can read version-control diffs.

> **Shared material attributes.** Many visual assets (sprites, flipbooks, skeletons,
> tilesets) share three rendering attributes:
> * **Shading Mode** — `Lit` (affected by 2D lights & shadows) or `Unlit`.
> * **Blend Mode** — `Masked` (alpha-clipped), `Additive`, `Translucent`, or `Opaque`.
> * **Alpha Clip Threshold** — cutoff for `Masked` blending (default `0.5`).
> They also support assigning a **custom material** instead of the default.

### Common editor behavior

Every asset editor is a normal dockable panel, and they all follow the same rules:

* **One panel per asset.** Double-clicking an asset opens *its own* panel instance, so you
  can have three sprites, two materials and a tilemap open side by side and drag them into
  separate docks or tabs. Opening an asset that is already open just **focuses** the
  existing panel. Closing a panel closes only that asset.
* **Explicit save.** Editors work on an in-memory copy. A **Save** button (and `Ctrl+S`
  where the panel supports it) writes the file; an **unsaved changes** marker appears in
  the header while there are pending edits. Saving refreshes the asset manager so open
  levels, thumbnails and running previews pick the change up immediately.
* **Per-panel undo/redo.** Each editor keeps its own `Ctrl+Z` / `Ctrl+Y` history of the
  asset it is editing, independent of the scene and of the other editors.
* **Asset fields use the asset picker.** Anywhere an asset references another asset you get
  the same control: a button showing the current asset (with a thumbnail where one exists),
  an **X** to clear it, and a drop-down with a **search box** that lists every matching
  asset in the project by name and relative path. The picker is filtered to the types that
  field accepts, and it is also a **drop target** — drag an asset onto it straight from the
  Content Browser.
* **Live feedback.** Editors that can preview do (flipbook playback, FX simulation, widget
  animation, material compilation, skeleton posing), and renaming or moving an asset while
  its editor is open re-targets the panel instead of breaking it.

### 4.1 Source files & their sidecars

These are raw media files. You import them, edit their **sidecar** settings, and
reference them (directly or via sprites/fonts/etc.). The source file stays byte-for-byte
unchanged.

#### 4.1.1 Textures and the `.ice_texture` sidecar

**Source:** `.png`, `.jpg`, `.jpeg`  **Sidecar:** `.ice_texture`  **Editor:** Texture Settings

The texture sidecar stores GPU import settings:

| Setting | Values / notes |
| ------- | -------------- |
| **Min/Mag Filter** | `Nearest`, `Linear`, and the four mipmap combinations. |
| **Wrap S / Wrap T** | `Repeat`, `ClampToEdge`, `MirroredRepeat`, `ClampToBorder`. |
| **Generate Mipmaps** | Build a mip chain. |
| **sRGB** | Treat as sRGB color (vs linear). |
| **Premultiply Alpha** | Premultiply RGB by alpha on load. |
| **Max Texture Size** | Clamp the largest dimension (`0` = source size). Used by cooking. |
| **Anisotropic Level** | Anisotropic filtering level. |
| **LOD Bias** | Mip selection bias. |
| **Border Color** | Color for `ClampToBorder`. |
| **Pixel Format** | `Auto`, `RGBA`, `RGB`, `RG`, `R`, `Grayscale`. |
| **Compression** | `Auto (verified)`, `Lossless (pixel-perfect)`, `Always Compressed`. Used by cooking. |
| **Use Atlas** | Allow this texture to be packed into a shared atlas page. |

For pixel-art, set **Min/Mag = Nearest** and disable mipmaps; for smooth UI/photographic
textures use **Linear** with mipmaps.

**Compression** controls what cooking is allowed to do to the pixels. On `Auto` the cooker
encodes with the build's texture format, decodes the result back and compares it against the
source; if the difference would be visible (which is what lossy WebP and GPU block formats
do to hard-edged pixel art) it discards that result and stores the texture losslessly
instead, so the cooked build matches an uncooked one pixel for pixel. `Lossless` skips the
lossy attempt entirely; `Always Compressed` keeps the build's format without verification.

**The Texture Settings panel** is split into a live preview and the settings list:

* The **preview** re-uploads the texture whenever you change a setting, so filtering,
  wrapping, mipmaps, max size and pixel format are shown as they will actually look.
  **Scroll** to zoom (towards the cursor), **middle-drag** to pan; the footer shows the
  source resolution and the current zoom.
* **R / G / B / A** buttons above the preview isolate individual channels — the fastest way
  to inspect an alpha mask or a packed data texture.
* Settings are grouped into **Filtering**, **Wrapping**, **Mipmaps**, **Advanced** and
  **Atlas**; every field has a tooltip, and **Max Texture Size** is a preset list
  (`Full`, 32 … 4096).
* **Save** writes the sidecar (and reloads dependent assets), **Reset** restores engine
  defaults, and a **Create Sprite** button in the corner makes a `.ice_sprite` for the whole
  image without going back to the browser.

#### 4.1.2 Audio and the `.ice_sound` sidecar

**Source:** `.wav`, `.mp3`, `.ogg`, `.flac`  **Sidecar:** `.ice_sound`  **Editor:** Sound Settings

The sound sidecar is a full per-clip mixing and DSP definition:

* **Playback** — Default Volume, Default Pitch, Pan (−1 left … +1 right), Loop,
  **Group** (`Master`, `Music`, `SFX`, `Voice`, `Ambient`, `UI`), Priority (0–255).
* **Randomization** — Volume / Pitch / Pan Variation (±) for natural-sounding repeats.
* **Trim & fades** — Start Time, End Time, Fade In, Fade Out (seconds).
* **3D / spatial** — Spatial toggle, Force Mono, **Attenuation Model**
  (`None`, `Inverse`, `Linear`, `Exponential`), Min/Max Distance, Rolloff,
  Doppler Factor, and a directional **cone** (Inner/Outer angle, Outer volume).
* **Effects (DSP)** — Low-Pass filter, High-Pass filter, Lo-Shelf EQ, Hi-Shelf EQ,
  Delay/Echo (time, decay/feedback, wet, dry), and Reverb (decay, wet, room size,
  damping).

**The Sound Settings panel** starts with a **Preview** block: a Play/Stop button, a
progress bar following the playhead, and a line with the clip's **duration, sample rate and
channel count**. The preview uses your current (unsaved) settings, so trim points, fades,
pitch and the DSP chain can be auditioned before saving. The rest of the panel is the
grouped settings list — Playback, Group, Randomization, Trim & fades, Spatial and Effects —
followed by **Save** and **Reset to Defaults**.

> The Content Browser has its own one-click preview on the audio tile itself
> ([3.5](#35-thumbnails--live-previews)); it always plays non-spatial and non-looping so a
> looping ambience does not run away from you.

#### 4.1.3 Fonts and the `.ice_font` sidecar

**Source:** `.ttf`, `.otf`  **Sidecar:** `.ice_font`  **Editor:** Font Editor

The font sidecar (`FontSettings`) controls glyph atlas generation:

| Setting | Notes |
| ------- | ----- |
| **Default Size** | Pixel size the atlas is baked at (default 24). |
| **Antialiased** | Smooth glyph edges. |
| **Nearest Filter** | Crisp/pixel font rendering. |
| **Bold / Italic** | Synthetic style flags. |
| **Char Range Start / End** | Codepoint range to bake (default `32`–`1103`, covering Latin + Cyrillic). |
| **Additional Ranges** | Extra codepoint ranges (e.g. CJK blocks, symbols). |
| **Atlas Width / Height** | Glyph atlas texture size (default 1024×1024). |

The renderer supports **right-to-left** scripts (Arabic, Hebrew) and bidirectional
text. The declared glyph ranges also drive font **subsetting** when cooking.

**The Font Editor** shows a live preview of the baked font next to the settings:

* **Preview** — type any sample text, scale it, change its color, or tick **Show Atlas** to
  look at the generated glyph atlas texture itself (the fastest way to tell whether your
  ranges overflow the atlas size).
* **Presets** — one-click character ranges: `ASCII`, `Extended`, `Cyrillic`, `Chinese`,
  `Japanese`, `Arabic`, `Hebrew`, `Hindi` and `All languages`. They fill in the main range
  and the additional ranges for you.
* **+ Add Range** appends a custom start/end codepoint pair for anything the presets miss
  (symbols, emoji blocks, private-use areas).
* **Loaded font info** reports the size actually baked, line height and ascender/descender.
* **Save Settings** writes the sidecar; **Apply & Reload** re-bakes the atlas so every
  widget and text draw in the editor picks up the change immediately.

> Adding large ranges (CJK in particular) can exceed a 1024×1024 atlas — raise
> **Atlas Width/Height** if glyphs start going missing.

#### 4.1.4 Video and the `.ice_video` sidecar

**Source:** `.mp4`, `.avi`, `.mkv`, `.mov`, `.webm`  **Sidecar:** `.ice_video`  **Editor:** Video Player

Video decoding uses **FFmpeg**. On import the engine probes the file and writes the
sidecar with **Width, Height, FPS** and **Duration**. Videos can be played back in-game as
full-screen movies or as textures on surfaces (driven from script).

**The Video Player panel** decodes and plays the clip in the editor with a **Play/Pause**
button, and shows an **Info** block with the resolution, frame rate and duration read from
the sidecar. The Content Browser uses the same decoder to pull the **first frame** for the
tile thumbnail.

> Video cooking (re-encode to VP9) is build-time and platform-restricted — see
> [Section 6](#6-asset-cooking--overview).

### 4.2 Sprite (`.ice_sprite`)

**Editor:** Sprite Editor

A **sprite** wraps a texture (or a sub-rectangle of one) with a pivot, an optional
custom material, attach points, and a collision polygon. It is the fundamental 2D
visual unit.

| Field | Meaning |
| ----- | ------- |
| `TexturePath` | Source texture. |
| `PivotOffset` | Normalized pivot (default center `0.5, 0.5`). |
| `SourceRect` | Sub-rectangle within the texture (for sheets). |
| `MaterialMode` | `Default` (engine material) or `Custom` (`MaterialPath`). |
| `DefaultShadingMode` / `DefaultBlendMode` / `DefaultAlphaClipThreshold` | Default render attributes (see the shared note above). |
| `AttachPoints[]` | Named local anchors (name, position, rotation) — e.g. *muzzle*, *hand* — for attaching effects or other entities. |
| `CollisionPolygon[]` | Convex/closed polygon for physics (up to 32 points). |
| Collision physics | `CollisionDensity`, `CollisionFriction`, `CollisionRestitution`, `CollisionIsSensor`, contact/sensor/hit/pre-solve event toggles, one-way platform flag + direction, collision group index, and the **collision mode** (`No Collision`, `Query Only`, `Physics Only`, `Query and Physics`). |

The **Sprite Editor** is a preview on the left and the property list on the right:

* **Preview** — shows the sprite (the `SourceRect` region, not the whole sheet) with the
  pivot, attach points and collision polygon drawn on top. **Click** in the preview to set
  the pivot, **scroll** to zoom, **middle-drag** to pan; the footer reports the current
  zoom and pivot. With no texture assigned, the preview invites you to **drag a texture**
  onto it.
* **Pivot** — the exact normalized value plus a 3×3 grid of **preset buttons**
  (top-left … bottom-right).
* **Source Rectangle** — `X / Y / Width / Height` in pixels, with **Reset to Full Texture**
  to clear the crop.
* **Material** — `Default` or `Custom` (asset picker), plus the shared shading / blend /
  alpha-clip attributes.
* **Attach points** — add, rename, position and rotate named sockets; each one is shown and
  draggable in the preview.
* **Collision** — toggle the overlay, drag points directly in the preview, add/remove
  points by hand, **- Remove Last** / **Clear Collision**, or press **Generate Collision** to trace an outline from the
  texture's alpha using the **Alpha Threshold** slider. Below it sit the physics values
  (density, friction, restitution), the sensor and one-way flags with a direction, the
  contact/sensor/hit/pre-solve event toggles, the collision group and the collision mode.
* **Save** / **Revert** at the bottom, with an unsaved-changes marker.

Attach points (sockets) are what the socket-attach system binds to. On a Sprite
or Flipbook instance — in the **Class Editor** and in the **Properties** panel —
set *Attach To Socket* to a socket name and the engine keeps that instance glued
to it every frame, correct under entity rotation, entity scale and FlipX.
*Socket Source* picks which component provides the socket (`Auto` searches the
skeleton first, then sprites, then flipbooks), *Socket Source Index* narrows it
to one instance (`-1` = any), and *Inherit Flip X* mirrors the attached art
together with its owner. *Socket Offset* / *Socket Offset Rotation* are layered
on top of the socket in socket space, so you can nudge the placement (or animate
recoil from a script) without moving the socket itself. A flipbook exposes the
sockets of its **current frame**, so an attachment follows the animation frame by
frame. From Lua the same thing is `AttachSpriteToSocket` /
`AttachFlipbookToSocket`.

#### 4.2.1 The Spritesheet Slicer

**Slice Spritesheet…** on a texture (item context menu) opens the **Spritesheet Slicer** — a
dedicated panel that cuts a sheet into many `.ice_sprite` assets in one pass. Each sheet you
open gets its own slicer panel.

**Slice modes**

| Mode | How it works |
| ---- | ------------ |
| **Grid** | Regular cells: **Cell Width/Height**, **Offset X/Y** (skip a margin) and **Padding X/Y** (gap between cells). On open, the slicer guesses a cell size that divides the image evenly (64, 32, 16, 48 or 24). |
| **Auto** | Detects sprites by **flood-filling non-transparent pixels**. **Alpha Threshold** decides what counts as empty; **Min Width / Min Height** discard specks. Results are sorted into reading order (top-to-bottom, left-to-right) and renumbered. |
| **Manual** | Draw the rectangles yourself: **right-click-drag** in the preview to add one. |

**Preview** — **scroll** to zoom, **middle-drag** to pan, plus **Fit to Window**,
**Reset Zoom** and **Square** helpers. Every slice is outlined and numbered; selecting one
in the list highlights it.

Grid and Auto mode are applied with the **Slice Grid** / **Auto Detect** button — change a
setting, press it again, and the list is rebuilt.

**Slice list** — every rectangle with a checkbox. **All / None / Invert** control which
slices are exported; **Delete** removes a rectangle; **Clear All** empties the list.

**Naming & pivot** — a **Base Name** and **Start Index** produce `Name_0`, `Name_1`, …
(the `SP_` prefix is added automatically if you don't type it), **Re-number** re-applies the
sequence after edits, and the **Pivot Preset** (9 positions plus **Custom**) is applied to
every generated sprite.

**Output** — **Generate Sprites** writes one `.ice_sprite` per checked slice next to the
texture. Tick **Also create Flipbook** and set an **FPS** to also emit an `FB_<BaseName>`
flipbook whose frames are those sprites in order.

> **Re-slicing is non-destructive.** When a generated sprite file already exists, the
> slicer loads it first and only rewrites the **texture path, source rect and pivot** — the
> attach points, collision polygon, collision settings and custom material you authored are
> preserved. Re-cutting a sheet after the artist tweaks it therefore does not throw your
> setup away.

### 4.3 Flipbook (`.ice_flipbook`)

**Editor:** Flipbook Editor

A **flipbook** is a frame-by-frame animation built from a sequence of sprites — the
simplest way to animate.

| Field | Meaning |
| ----- | ------- |
| `DefaultFPS` | Default frame rate; per-frame `Duration` defaults to `1/FPS`. |
| `Loop` | Loop at the end. |
| `Frames[]` | Ordered list of `{ SpritePath, Duration }`. |
| `MaterialMode` + `Override…` | `From Sprites` (each frame keeps its own sprite's material and render attributes) or `Override` (one shading/blend/alpha-clip/material for the whole flipbook). |

Create one quickly by selecting several sprites and choosing
**Create Flipbook from Sprite(s)**, or from a GIF / Aseprite / spritesheet import.

**The Flipbook Editor** has three parts:

* **Preview** — plays the animation with **Play / Pause / Stop**, showing the current frame
  number. Optional overlays draw the current frame's **attach points** and **collision
  polygon**, so you can check that a socket stays put across the whole animation.
* **Timeline** — every frame as a strip of thumbnails. Click to select, drag to reorder,
  **Add Frame** / **Delete Frame** to edit the sequence, or **drag sprites in from the
  Content Browser** and drop them on the `+` slot (multi-selections are appended in order).
* **Settings** — `Default FPS` and `Loop`, the selected frame's **sprite** (asset picker)
  and its **duration in seconds**, plus **Set from FPS** to derive the duration from the
  default frame rate and **Apply to All** to push it onto every frame at once. The material
  mode block below switches between *From Sprites* and *Override*.

### 4.4 Animation / State Machine (`.ice_animation`)

**Editor:** Animation Editor

An **animation asset** is a **state machine** that drives which flipbook plays based on
parameters.

* **Parameters** — typed variables that gameplay sets: `Bool`, `Int`, `Float`,
  `Trigger`.
* **States** — each plays a `FlipbookPath` at a `SpeedMultiplier`, optionally looping,
  positioned on the editor graph.
* **Transitions** — directed edges between states with **conditions** (parameter
  compared via `==`, `!=`, `>`, `<`, `>=`, `<=`), an optional **blend duration**,
  **Has Exit Time / Exit Time** (transition only after a fraction of the clip), and a
  **Priority**.
* **Any-State transitions** — transitions that can fire from any state (e.g. *hit* or
  *die*). Any-State transitions never use exit time — they are meant to interrupt.
* A **Default State** the machine starts in.

**The Animation Editor** is a node graph with an inspector:

* **Parameters panel** — add typed parameters (`Bool`, `Int`, `Float`, `Trigger`), rename
  them, and set the value used for the preview.
* **Graph** — **Add State** drops a new node; drag nodes to arrange them (positions are
  saved in the asset), drag from one node to another to create a **transition**, scroll to
  zoom. The default state is marked; **Set as Default** promotes the selected one, and the
  context menu deletes states and transitions.
* **Inspector** — for a **state**: name, `Flipbook` (asset picker), speed multiplier and
  loop. For a **transition**: the condition list (**+ Add Condition** → parameter, operator,
  value), **Duration** (blend), **Has Exit Time** + **Exit Time**, and **Priority** —
  when several transitions out of a state are satisfied at once, the highest priority wins.
* **Preview** — plays the currently selected state's flipbook so you can check speed and
  looping without leaving the panel.

### 4.5 Skeleton (`.ice_skeleton`)

**Editor:** Skeleton Editor

A full **2D skeletal animation** system (bones, slots, skins, meshes, constraints,
events and physics) — comparable to Spine-style rigs.

| Group | Contents |
| ----- | -------- |
| **Bones** | Hierarchy with position, rotation, scale, shear, length, and inherit-rotation/scale flags. |
| **Slots** | Draw slots bound to a bone, with color, blend mode (`Normal`/`Additive`/`Multiply`/`Screen`) and the current attachment. |
| **Attachments** | `Region` (textured quad), `Mesh` (weighted deformable mesh with vertices/UVs/triangles/weights), `Point`, or `BoundingBox`. |
| **Skins** | Named sets of slot→attachment overrides (e.g. costume variants). |
| **Animations** | Timelines per target: `Rotate`, `Translate`, `Scale`, `Shear`, `SlotColor`, `SlotAttachment`, `Deform`, `DrawOrder`, `IKMix`. Keyframes support `Linear`/`Stepped`/`Bezier` curves. Plus **events** at points in time. |
| **Constraints** | **IK** (target, mix, softness, bend, compress/stretch), **Transform** (rotate/translate/scale/shear mixes + offsets), and **Path** (follow a spline). |
| **Physics bones** | Per-bone physics (shape with local offset, density, friction, joints with limits/motors). Drives the animated bone colliders and the ragdoll. |

Global settings include the default skin, default animation, pixels-per-unit, and the
shared render attributes.

**The Skeleton Editor** is a viewport with a tabbed inspector:

* **Viewport** — the live pose. Select and drag bones, slots and point attachments; the
  pose reflects the current animation time, so what you see is what the runtime produces.
* **Bone** tab — the bone hierarchy (**Add Bone**, reparent, remove) with position,
  rotation, scale, shear, length and the inherit-rotation / inherit-scale flags.
* **Slot** tab — draw slots bound to a bone, with color, blend mode, draw order and the
  default attachment. **Add Region / Add Mesh / Add Point / + BBox** create attachments;
  **Convert to Mesh** turns a region into an editable mesh. For meshes you edit vertex
  positions, add bone **weights** and **Normalize** them; bounding boxes take an arbitrary
  point list. **Skins** group slot→attachment overrides — **Add Skin** and **Set Active**
  switch costumes.
* **Animation** tab — the timeline. **Add Animation**, set its duration, then key
  `Rotate`, `Translate`, `Scale`, `Shear`, `Slot Color`, `Slot Attachment`, `Deform` and
  `Draw Order` tracks; keyframes support `Linear`, `Stepped` and `Bezier` interpolation.
  **Add Event** places a named event at a point in time for gameplay to react to.
  **Play / Pause / Stop** scrubs the animation in the viewport.
* **Constraints** tab — **IK** (target bone, mix, softness, bend direction, compress /
  stretch), **Transform** (rotate/translate/scale/shear mixes plus offsets) and **Path**
  (a spline of points with spacing, closed flag and percent position).
* **Physics** tab — per-bone physics: enable, shape with local offset, width and length
  scale, density, friction, restitution, the joint type with its lower/upper **limits**,
  **motor torque**, **break force** and a self-collision flag. These drive the animated
  bone colliders and the ragdoll.

`Point` attachments are the skeleton's **sockets**. They are drawn in the Skeleton
Editor viewport as a diamond marker with a forward arrow and the attachment name,
positioned by the live pose of their slot's bone. Click one to select it, drag it to
move it — the drag writes back into the attachment's bone-local `Offset`, so what you
see is exactly what `GetSkeletonAttachPointWorld` will report at runtime. Because
socket positions come from the live pose, they also follow bone overrides
(`SetSkeletonBoneOverride`), IK constraints and ragdoll.

### 4.6 Tileset (`.ice_ts`)

**Editor:** Tileset Editor

A **tileset** slices a texture into a grid of tiles and stores per-tile data used when
painting tilemaps.

* `TexturePath`, `TileSize`, optional custom material + shared render attributes.
* Per-tile **`TileData`** (keyed by tile index):
  * **Collider** — on/off, **collider points** in **Polygon** or **Chain** mode.
  * **Flags** — sensor, destructible, one-way (with direction).
  * **Events** — contact / sensor / hit / pre-solve toggles.
  * **Physics material** — density, friction, restitution.
  * **Filtering** — collision group index and collision mode (`No Collision`,
    `Query Only`, `Physics Only`, `Query and Physics`).
  * **Shadows** — cast shadow on/off, cast mode (`Contour` or `Colliders`), shadow origin
    (`Top` / `Center` / `Bottom`), Z-order, edge fade, and a *don't block shadows* flag.
  * **Fragments** (when the tile is destructible) — fragment count and break **pattern**
    (`Grid`, `Radial`, `Random`), lifetime and fade time, gravity scale, density, friction,
    restitution, sensor flag, event toggles, collision group, and the fragments' own shadow
    settings.
  * An optional **data name/tag** you can read from script to identify the tile type.

**The Tileset Editor** shows the texture sliced into its grid on the left and the selected
tile's properties on the right:

* Set the **Tile Size**; the grid overlay updates live. Drag a texture in from the Content
  Browser to (re)assign it. **Zoom** the sheet and the collider preview independently.
* **Click a tile** to select it, then author its collider directly on the enlarged tile
  preview: drag points, **+ Add Point**, **- Remove Last**, or use the **presets** —
  *Full Tile*, *Half Top*, *Half Bottom*, and the four *Triangle* corners. **Pixel Snap**
  keeps points on the pixel grid.
* The remaining property groups match the `TileData` list above; hints explain when a
  setting has no effect (a shadow needs a collider, a sensor ignores physics response, and
  chain colliders are open lines rather than solid shapes).

### 4.7 Tilemap (`.ice_tm`)

**Editor:** Tilemap Editor

A **tilemap** is a multi-layer grid painted from one or more tilesets, with support for
several projections.

| Feature | Details |
| ------- | ------- |
| **Dimensions** | `Width`, `Height`, `TileSize`. |
| **Projection** | `Orthogonal`, `Isometric`, or `Hexagonal` (with cell width/height, hex side length, stagger axis & index). |
| **Layers** | Multiple `MapLayer`s, each with name, visibility, lock, and a tile grid. |
| **Tilesets** | A primary tileset plus up to 127 additional tilesets; tile IDs are encoded with the tileset index so one map can mix tilesets. |
| **Tile rotation** | Every painted cell stores a rotation step alongside the tile ID — 4 steps of 90° on orthogonal/isometric maps, 6 steps of 60° on hexagonal ones. The sprite, the collider, the shadow caster, the nav-grid footprint and the destruction fragments all rotate together, so one corner tile covers every orientation. |
| **Tile span** | A placed tile can cover more than one cell. The block grows **right (`+X`)** and **up (`+Y`)** from the cell that owns it, up to `64 x 64` cells, and the tile art (static **or** flipbook) is stretched over the whole block. The collider, the 2D shadow caster, the nav-grid footprint and the destruction fragments scale with it. Orthogonal maps only. |
| **Animated tiles** | Placeholder tiles that play a flipbook, each with a speed multiplier, its own **per-frame colliders**, and the same full set of physics, event, shadow and destructible-fragment settings a static tile has. |
| **Chunking** | Optional chunked storage with a configurable chunk size and per-chunk "empty" flags for fast culling of large maps. |
| **Show Coordinates** | Editor overlay for grid coordinates. |

**The Tilemap Editor** is a canvas with a tool sidebar:

* **Painting** — all on the active layer, using the tile currently selected in the palette:

  | Input | Tool |
  | ----- | ---- |
  | **Left-drag** | Paint. |
  | **Right-drag** | Erase. |
  | **Alt + Left-click** | **Flood fill** the contiguous region under the cursor. |
  | **Alt + Right-click** | **Eyedropper** — pick the tile (or animated tile) under the cursor as the active one. |
  | **Shift + Right-drag** | **Select a rectangle** and capture it as a **stamp**. |
  | **Shift + Left-drag** | **Stamp** the captured block, with a preview under the cursor. |
  | **`Q` / `E`** | **Rotate the brush** one step counter-clockwise / clockwise. |
  | **`Ctrl+Q` / `Ctrl+E`** | **Rotate the tile already under the cursor** in place. |
  | **`Shift+D` / `Shift+A`** | **Grow / shrink the tile under the cursor** by one cell to the **right**. |
  | **`Shift+W` / `Shift+S`** | **Grow / shrink the tile under the cursor** by one cell **upwards**. |
  | **`Shift+Wheel`** | Grow (up) / shrink (down) the tile under the cursor on **both** axes at once. |
  | **Middle-drag / scroll** | Pan / zoom the canvas. |

  The cell under the cursor is outlined and shows a translucent **ghost of the tile about
  to be placed**, already rotated, so you can dial in the orientation before you click.
  There is a separate editor zoom for the palette.
* **Tile Rotation** — the sidebar shows the current brush rotation in degrees plus the step
  index, with **CCW / CW / Reset** buttons next to the `Q` / `E` shortcuts. Rotation is a
  property of the *placed cell*, not of the tileset, so the same tile can sit at several
  orientations across the map. Everything the tile owns rotates with it: the sprite (or
  flipbook frame), its collider polygon, its shadow caster, its nav-grid footprint and the
  fragments it shatters into. A
  tile using the plain full-tile collider is unchanged by rotation, so box merging and
  chunking keep working exactly as before. The eyedropper also picks up the rotation of
  the sampled cell.
* **Tile Span** — a tile does not have to be one cell. The sidebar shows the current brush
  span (`W x H` and the total cell count) with **Narrower / Wider**, **Shorter / Taller**,
  **Shrink Both / Grow Both** and **Reset** buttons next to the `Shift+A/D/S/W` and
  `Shift+Wheel` shortcuts. A block always grows **right** and **up** from its anchor cell —
  the one you painted — so the cell under your brush never moves. Paint with a `2 x 1` brush
  and you get a two-cell-wide tile in one click; or place a normal tile, hover it and press
  `Shift+D` to widen what is already on the map. The tile's texture (or flipbook frame) is
  **stretched over the whole block**, so a bush drawn two tiles wide stays one flipbook
  instead of being cut in half. Shrinking only works on an enlarged tile — a plain `1 x 1`
  tile is already the minimum. Growing over occupied cells overwrites them (`Ctrl+Z` undoes
  it); growing into the map edge simply stops. Everything the tile owns scales with the
  block: its collider (default box colliders still merge with their neighbours, custom
  polygons and chains are stretched across the block), its 2D shadow caster, its nav-grid
  footprint, particle collision and its destruction fragments — hitting any cell of a
  destructible block destroys the whole block. Erasing, flood-filling, stamping or
  eyedroppering any cell of a block treats the block as one tile. Tile span is stored per
  cell in the `.ice_tm` file and is **orthogonal-only**; on isometric and hexagonal maps the
  sidebar says so and the shortcuts do nothing.
* **Layers** — **+ Add Layer**, **Rename**, **Remove**, **Move Up / Down**, plus per-layer
  visibility and lock toggles. Painting always targets the selected layer, and locked
  layers are skipped.
* **Tilesets** — the **Active Tileset** drop-down switches the palette; drag a `.ice_ts`
  from the Content Browser onto the palette to add it to the map. Because tile IDs carry
  the tileset index, one map can freely mix tilesets.
* **Map settings** — `Width`, `Height`, `Tile Size`, **Projection** (`Orthogonal`,
  `Isometric`, `Hexagonal` with cell width/height, hex side length, stagger axis `X`/`Y`
  and stagger index `Even`/`Odd`), and **Show Coordinates**.
* **Optimization** — **Enable Chunking** with a **Chunk Size**, and a live **chunk stats**
  readout. A warning appears for very large maps suggesting you turn it on.
* **Animated tiles** — **+ Add Animated Tile** creates a placeholder you paint like any other
  tile; assign a flipbook (drag one in), set the speed multiplier, and author colliders
  **per frame** (**+ Add Collider For Frame** / remove, or **Copy To All Frames** to reuse one
  shape for the whole animation). The same collider presets, physics, event, shadow and
  fragment settings as a static tile are available.

### 4.8 Material (`.ice_material`)

**Editor:** Material Editor

A **material** is a **node-graph shader**. You build the look by wiring nodes into a
**Material Output**; the graph is compiled to a runtime shader.

* **Shading Mode** (`Lit`/`Unlit`), **Blend Mode** (`Masked`/`Additive`/`Translucent`/
  `Opaque`), **Domain** (`Surface`, `PostProcess` or `Decal`), and **Alpha Clip Threshold**.
* **Nodes & links** are stored with stable IDs and editor positions.

The node palette is extensive. The **Add Node** menu groups it into nine sub-menus:

| Menu | Nodes |
| ---- | ----- |
| **Constants** | Float, Vector2, Color (RGB), Color (RGBA). |
| **Textures** | Texture Sample. |
| **Coordinates** | Texture Coordinates, Vertex Color, Camera Position, Panner, Rotator, Tiler. |
| **Math** | Add, Subtract, Multiply, Divide, Lerp, Clamp, One Minus, Power, Abs, Floor, Ceil, Frac, Sign, Step, SmoothStep, Min, Max, Sine, Cosine, Tangent, ATan2, Saturate, Fmod, Sqrt, Round, Remap, If. |
| **Vector Operations** | Dot Product, Distance, Length, Normalize, Append, Cross Product. |
| **Make / Break** | Make Float2/3/4, Break Float2/3/4, Component Mask. |
| **Utility** | Time, Desaturate, Fresnel, Simple Noise, Sphere Mask, Linear Gradient, Radial Gradient, World Position, Screen UV, Pixel Size, Scene Color, Custom Expression. |
| **Parameters** | Scalar Param, Vector Param, Texture Param, Collection Param. |
| **Functions** | Material Function Call. |

Inside a **Material Function** the graph gains two more node kinds, **Function Input** and
**Function Output** — see [4.10](#410-material-function-ice_matfunc).

Pin types are `Float`, `Vector2`, `Vector3`, `Vector4`, and `Texture`. Values are converted
automatically when you connect pins of different widths (a `Float` fed into a `Vector3` is
splatted, a `Vector4` fed into a `Float` takes its first component).

#### 4.8.1 Material node reference

Every node in the palette, its pins and what it computes. **In** lists input pins (each with
the default used when the pin is left unconnected), **Out** lists output pins.

##### Material Output

The single root node of every material. It cannot be deleted, and only what reaches it is
compiled — unreachable nodes are ignored.

| Input | Type | Default | Meaning |
| ----- | ---- | ------- | ------- |
| **Base Color** | Vector3 | `1,1,1` | Albedo. Unconnected, the entity's own texture is used, multiplied by this default. |
| **Alpha** | Float | `1` | Opacity. Unconnected, the entity texture's alpha is used, multiplied by this default. This is also the value `Masked` compares against **Alpha Clip Threshold**. Ignored when Blend Mode is `Opaque`. |
| **Emissive** | Vector3 | `0,0,0` | Self-illumination, added after lighting — unaffected by lights, feeds bloom. |
| **Normal** | Vector3 | `0.5,0.5,1` | Tangent-space normal map (`0..1` encoded, unpacked to `-1..1`). `Lit` only, and ignored for `Additive`. |
| **Metallic** | Float | `0` | `Lit` only. |
| **Roughness** | Float | `0.5` | `Lit` only. |
| **Ambient Occlusion** | Float | `1` | `Lit` only. |
| **Subsurface Color** | Vector3 | `0,0,0` | `Lit` only — light that wraps through the surface, added on top of the normal lit response. |
| **Pixel World Offset** | Vector2 | `0,0` | Added to the UV **before** every other pin samples anything, so it distorts the whole material at once (heat shimmer, water wobble, flag ripple). |
| **Refraction** | Float | `0` | Blends the scene behind the surface into the result, sampled at a screen-UV offset derived from this value. Only read when Blend Mode is `Translucent`, and connecting it makes the material sample **Scene Color**. |

##### Constants

| Node | In | Out | Does |
| ---- | -- | --- | ---- |
| **Float** | — | `Value` (Float) | A literal number, edited on the node. |
| **Vector2** | — | `Value` (Vector2), `X`, `Y` | A literal 2-component value, plus its components. |
| **Color (RGB)** | — | `RGB` (Vector3), `R`, `G`, `B` | A literal color, edited with a color swatch. |
| **Color (RGBA)** | — | `RGBA` (Vector4), `RGB` (Vector3), `A` | A literal color with alpha. |

> Any constant node can be turned into an exposed parameter (and back) with
> **Convert to Parameter** / **Convert to Constant** in its context menu.

##### Textures

| Node | In | Out | Does |
| ---- | -- | --- | ---- |
| **Texture Sample** | `UV` (Vector2) | `RGB`, `R`, `G`, `B`, `A` | Samples a texture picked on the node itself. Leave `UV` unconnected to use the mesh's own texture coordinates. |

##### Coordinates

| Node | In | Out | Does |
| ---- | -- | --- | ---- |
| **Texture Coordinates** | — | `UV` (Vector2) | The interpolated UV of the surface being shaded. |
| **Vertex Color** | — | `RGB`, `A` | The per-vertex color — for a sprite this is the tint set on the instance. |
| **Camera Position** | — | `XY` (Vector2), `X`, `Y` | The active camera's world position. |
| **Panner** | `UV`, `Speed X` (`1`), `Speed Y` (`0`) | `UV` (Vector2) | Scrolls UVs with time: `UV + vec2(SpeedX, SpeedY) * Time`. Conveyors, water, scrolling skies. |
| **Rotator** | `UV`, `Speed` (`1`), `Center` (`0.5,0.5`) | `UV` (Vector2) | Rotates UVs around `Center` at `Speed` radians per second. |
| **Tiler** | `UV`, `Tiling` (`1,1`), `Offset` (`0,0`) | `UV` (Vector2) | `UV * Tiling + Offset` — repeats or shifts a texture. |

##### Math

Every math node accepts any pin width; the result follows the widest of its value inputs, and
secondary pins (`Min`/`Max`, `Alpha`, `Exp`) are adapted to it.

| Node | In | Out | Does |
| ---- | -- | --- | ---- |
| **Add** | `A` (`0`), `B` (`0`) | `Result` | `A + B` |
| **Subtract** | `A` (`0`), `B` (`0`) | `Result` | `A - B` |
| **Multiply** | `A` (`1`), `B` (`1`) | `Result` | `A * B` |
| **Divide** | `A` (`1`), `B` (`1`) | `Result` | `A / B` |
| **Lerp** | `A` (`0`), `B` (`1`), `Alpha` (`0.5`) | `Result` | Linear blend from `A` to `B` by `Alpha`. |
| **Clamp** | `Value` (`0`), `Min` (`0`), `Max` (`1`) | `Result` | Constrains `Value` to the range. |
| **One Minus** | `Value` (`0`) | `Result` | `1 - Value` — inverts a mask. |
| **Power** | `Base` (`2`), `Exp` (`2`) | `Result` | `Base ^ Exp` — sharpens or softens gradients. |
| **Abs** | `Value` (`0`) | `Result` | Absolute value. |
| **Floor** | `Value` (`0`) | `Result` | Rounds down. |
| **Ceil** | `Value` (`0`) | `Result` | Rounds up. |
| **Frac** | `Value` (`0`) | `Result` | Fractional part — the basis of most tiling tricks. |
| **Sign** | `Value` (`0`) | `Result` | `-1`, `0` or `+1`. |
| **Step** | `Edge` (`0.5`), `Value` (`0`) | `Result` | `0` below the edge, `1` at or above it — a hard cutoff. |
| **SmoothStep** | `Edge0` (`0`), `Edge1` (`1`), `Value` (`0.5`) | `Result` | Smooth (Hermite) ramp between the two edges. |
| **Min** | `A` (`0`), `B` (`1`) | `Result` | The smaller value. |
| **Max** | `A` (`0`), `B` (`1`) | `Result` | The larger value. |
| **Sine** | `Value` (`0`) | `Result` | `sin(Value)`, radians. |
| **Cosine** | `Value` (`0`) | `Result` | `cos(Value)`, radians. |
| **Tangent** | `Value` (`0`) | `Result` | `tan(Value)`, radians. |
| **ATan2** | `Y` (`0`), `X` (`1`) | `Result` | Angle of the vector `(X, Y)` in radians — polar coordinates, radial sweeps. |
| **Saturate** | `Value` (`0`) | `Result` | Clamps to `0..1`. |
| **Fmod** | `A` (`0`), `B` (`1`) | `Result` | Remainder of `A / B`. |
| **Sqrt** | `Value` (`0`) | `Result` | Square root. |
| **Round** | `Value` (`0`) | `Result` | Rounds to the nearest whole number. |
| **Remap** | `Value` (`0`), `In Low` (`0`), `In High` (`1`), `Out Low` (`0`), `Out High` (`1`) | `Result` | Rescales `Value` from the input range to the output range. |
| **If** | `A` (`0`), `B` (`0`), `A >= B` (`1`), `A < B` (`0`) | `Result` | Branch: outputs the third pin when `A >= B`, the fourth otherwise. |

##### Vector Operations

| Node | In | Out | Does |
| ---- | -- | --- | ---- |
| **Dot Product** | `A`, `B` (Vector3) | `Result` (Float) | Dot product — angle/facing tests. |
| **Distance** | `A`, `B` (Vector2) | `Result` (Float) | Distance between two points. |
| **Length** | `Value` (Vector2) | `Result` (Float) | Vector magnitude. |
| **Normalize** | `Value` (Vector3, default `0,1,0`) | `Result` (Vector3) | Unit-length version of the vector. |
| **Append** | `A` (Float), `B` (Float) | `Result` (Vector2) | Packs two floats into a Vector2. |
| **Cross Product** | `A` (`1,0,0`), `B` (`0,1,0`) | `Result` (Vector3) | Cross product — a vector perpendicular to both. |

##### Make / Break

| Node | In | Out | Does |
| ---- | -- | --- | ---- |
| **Make Float2** | `X`, `Y` | `Result` (Vector2) | Builds a Vector2 from components. |
| **Make Float3** | `X`, `Y`, `Z` | `Result` (Vector3) | Builds a Vector3. |
| **Make Float4** | `X`, `Y`, `Z`, `W` (`1`) | `Result` (Vector4) | Builds a Vector4. |
| **Break Float2** | `Input` (Vector2) | `X`, `Y` | Splits a Vector2 into floats. |
| **Break Float3** | `Input` (Vector3) | `X`, `Y`, `Z` | Splits a Vector3. |
| **Break Float4** | `Input` (Vector4) | `X`, `Y`, `Z`, `W` | Splits a Vector4. |
| **Component Mask** | `Input` (Vector4) | `R`, `G`, `B`, `A` | Picks individual channels out of a value. |

##### Utility

| Node | In | Out | Does |
| ---- | -- | --- | ---- |
| **Time** | — | `Time`, `Sin Time`, `Cos Time` | Seconds since start, plus its sine and cosine — the standard driver for animated materials. |
| **Desaturate** | `Color` (Vector3, `1,1,1`), `Amount` (`1`) | `Result` (Vector3) | Blends the color toward luminance grey by `Amount`. |
| **Fresnel** | `Power` (`5`), `Normal` (`0,0,1`) | `Result` (Float) | Rim factor — bright where the surface faces away from the viewer. Higher `Power` = tighter rim. |
| **Simple Noise** | `UV`, `Scale` (`10`) | `Result` (Float) | Smoothed value noise in `0..1` at the given scale. |
| **Sphere Mask** | `A`, `B` (`0.5,0.5`), `Radius` (`0.5`), `Hardness` (`1`) | `Result` (Float) | `1` inside a circle of `Radius` around `B`, falling to `0` outside; `Hardness` sets how soft the edge is. |
| **Linear Gradient** | `UV` | `Horizontal`, `Vertical` | The U and V coordinates as two 0→1 ramps. |
| **Radial Gradient** | `UV`, `Center` (`0.5,0.5`), `Radius` (`0.5`) | `Result` (Float) | `1` at the center falling linearly to `0` at `Radius`. |
| **World Position** | — | `XY` (Vector2), `X`, `Y` | The world-space position of the shaded pixel — for world-anchored patterns that do not swim when the object moves. |
| **Screen UV** | — | `UV` (Vector2) | The pixel's normalized screen coordinate. |
| **Pixel Size** | — | `Size` (Vector2), `Width`, `Height` | The render target's size in pixels — for one-pixel offsets, outlines and screen-space kernels. |
| **Scene Color** | `UV` (Vector2) | `RGB`, `R`, `G`, `B`, `A` | Samples what has already been rendered behind this surface. The basis of glass, distortion and post-process materials. |
| **Decal Data** | — | `Fade`, `Normalized Age`, `Age`, `Lifetime`, `Random` | Live state of the decal being drawn: its fade factor, age in seconds, age divided by lifetime, its lifetime, and a stable per-decal random value. Meaningful in a `Decal` domain material; elsewhere it reads as neutral. |
| **Custom Expression** | `Input0`..`Input7` (configurable) | `Result` (float/vec2/vec3/vec4) | A full GLSL statement block written on the node. The escape hatch for anything the palette does not cover — see **Custom Expression** below. |


##### Custom Expression

The **Custom Expression** node drops raw GLSL into the generated shader. It is the escape hatch for effects the node palette
cannot express — loops, iterative sampling, per-pixel raymarching, custom filtering.

| Setting | Meaning |
| --- | --- |
| **Output type** | GLSL type of the node's `Result` pin and of the `output` variable: `float`, `vec2`, `vec3` or `vec4`. |
| **Input count** | How many input pins the node exposes, `0` to `8`. They are named `input0`, `input1`, ... in the code. |
| **input0..inputN** | The GLSL type of each input pin. Values arriving from links are converted to that type automatically. |
| **GLSL Code** | The statement block itself, up to 8191 characters. |

Inside the code block:

* each input is a local variable named `input0`, `input1`, ... of the type you selected for that pin;
* a variable named `output` is pre-declared with the chosen output type and zero-initialised;
* **assign your result to `output`**;
* the block has its own scope, so local variables, `for`/`while` loops and `if` branches are all allowed and cannot
  collide with the rest of the generated shader;
* uniforms of the material are in scope — `uTime`, `uCameraPosition`, `uScreenSize`, any `uParam_<Name>` from a Scalar or
  Vector Parameter node, and any `uTexture<N>` created by a Texture Sample node.

```glsl
// output type = float, 2 inputs (vec2, float)
float acc = 0.0;
for (int i = 0; i < 16; i++) {
    vec2 p = input0 + vec2(float(i)) * 0.01;
    acc += texture(uTexture0, p).r;
}
output = acc / 16.0 * input1;
```

> **Legacy nodes.** Code that has no `output` identifier is still treated as a single expression, with an optional leading
> `return`, exactly as before. Existing materials written as `return input0;` keep working unchanged.

> Because the code is inlined verbatim, a GLSL syntax error surfaces as a material compile error with the shader log — check
> the Material Editor's error banner.

##### Parameters

| Node | In | Out | Does |
| ---- | -- | --- | ---- |
| **Scalar Param** | — | `Value` (Float) | A named float exposed to **Material Instances** and to `SetMaterialScalar` from script. |
| **Vector Param** | — | `RGBA`, `RGB`, `R`, `G`, `B`, `A` | A named color/vector, exposed the same way. |
| **Texture Param** | `UV` (Vector2) | `RGB`, `R`, `G`, `B`, `A` | A named texture slot an instance can swap. |
| **Collection Param** | — | `Value` (Float), `Vector` (Vector4) | Reads a named parameter out of a [Material Parameter Collection](#411-material-parameter-collection-ice_mpc) — one global value that every material reading it picks up at once. Pick the collection and the parameter name on the node. |

The **Name** you give a parameter node is the key instances and scripts use, so renaming one
breaks the overrides that referenced the old name.

##### Functions

| Node | In | Out | Does |
| ---- | -- | --- | ---- |
| **Material Function Call** | (from the function) | (from the function) | Inlines a `.ice_matfunc` sub-graph. Pins come from the function's Function Input / Function Output declarations; press **Reload Pins** after changing the function's signature. |
| **Function Input** *(inside functions)* | — | `Value` | Declares one input pin of the function: name, type, **index** (the order pins appear in on the caller) and a default used when a caller leaves it unconnected. |
| **Function Output** *(inside functions)* | `Value` | — | Declares one output pin of the function. |

**The Material Editor** is the graph plus a live preview:

* **Graph** — right-click empty space for the **Add Node** menu (grouped by the categories
  above). Drag between pins to link, drag nodes to arrange, and box-select several at once.
  The node context menu offers **Copy**, **Duplicate** and **Delete** (the *Material Output*
  node cannot be deleted), and acts on the whole selection when you right-click inside it.
* **Convert to Parameter / Convert to Constant** — turn a constant node into an exposed
  `Scalar`/`Vector` parameter (so material instances can override it) or back again, keeping
  the value, the position and all existing links.
* **Unconnected inputs** are editable in place: a float field, a color swatch or a texture
  picker right on the node, so you rarely need a constant node just to feed a value.
* **Material Function Call** nodes take a `.ice_matfunc` asset; **Reload Pins** re-reads the
  function's inputs and outputs after you change its signature.
* **Compile** builds the graph and reports **OK** or the shader error; the preview
  recompiles automatically as you edit, and **Refresh** forces a rebuild.
* **Preview** — a live quad rendered with the compiled material. When the material is `Lit`
  you also get a preview **light**: color, intensity, radius, falloff, and a
  **spot light** toggle with an angle, so you can check how the material reacts to lighting
  without dropping it into a level.
* **Settings** — Shading Mode, Blend Mode, Alpha Clip Threshold, and **Domain**. Switching
  the domain to `PostProcess` turns the material into one you can plug into a
  [View](#414-view--post-process-volume-ice_view)'s custom post-process stack, and the panel
  says so. Switching it to `Decal` makes the material assignable to a
  [Decal](#421-decal-ice_decal) asset; a decal material uses the same Material Output pins as
  a surface one, and additionally gets the **Decal Data** node.

### 4.9 Material Instance (`.ice_matinst`)

**Editor:** Material Instance Editor

A lightweight **override** of a parent material — change exposed parameters without
duplicating the graph.

```json
{
    "Name": "MI_Hero_Red",
    "ParentMaterial": "Content/Materials/M_Character.ice_material",
    "AlphaClipOverride": 0.4,
    "ScalarOverrides": { "Roughness": 0.2 },
    "VectorOverrides": { "Tint": [1.0, 0.2, 0.2, 1.0] },
    "TextureOverrides": { "BaseColor": "Content/Textures/T_Hero_Red.png" }
}
```

It stores the parent path plus **scalar / vector / texture** override maps and an
optional alpha-clip override.

**The Material Instance Editor** starts with a **Parent Material** picker. Once a parent is
set, the panel lists that material's exposed parameters in three groups — **Scalar**,
**Vector** and **Texture** — each showing the parent's value until you override it. It also
shows the shading and blend mode inherited from the parent (an instance never changes the
graph, only its inputs) and exposes the one structural override an instance can make, the
**Alpha Clip Threshold**. If the parent cannot be loaded the panel says so instead of
silently showing an empty list.

### 4.10 Material Function (`.ice_matfunc`)

**Editor:** Material Function Editor

A reusable **sub-graph** with typed **Function Inputs** and **Function Outputs**. Call
it from any material via the **Material Function Call** node to share common shader
logic (e.g. a custom blend or a stylization). Stored as nodes/links plus the input and
output declarations.

**The Material Function Editor** is the same graph editor as the Material Editor, minus the
material settings and with two extra node kinds: **Function Input** (name, type, index and a
default value used when a caller leaves the pin unconnected) and **Function Output**. The
input **index** decides the order the pins appear in on the calling node — after changing it,
press **Reload Pins** on the callers.

### 4.11 Material Parameter Collection (`.ice_mpc`)

**Editor:** MPC Editor

A **global** bag of named **scalar** and **vector** parameters that any material can
read through a **Collection Parameter** node. Setting a value once (from script or the
editor) updates every material that reads it — perfect for global effects like time of
day, team color, or a global wetness factor. Each parameter stores a default and a
current value; the collection can be reset to defaults.

**The MPC Editor** is deliberately small: a **collection name**, an **Add Scalar** and an
**Add Vector** button, and the two parameter lists. Each row is a name plus its value, and
names are what materials look the parameter up by — renaming one breaks the
**Collection Parameter** nodes that referenced the old name, so use
[Asset References](#312-the-asset-references-viewer) to check first.

### 4.12 FX / Particle System (`.ice_fx`)

**Editor:** FX Editor

A powerful **particle system**. An FX asset contains one or more **emitters**, each
with **Spawn**, **Initialize** and **Update** module stacks plus a **Render** module —
a stack-based design comparable to modern node/stack VFX systems.

**Emitter settings:** duration, looping, max particles, start delay, warm-up time,
simulation stages, **CPU or GPU** simulation, and a per-emitter **transform** (position,
rotation, scale) so one effect can compose several offset emitters.

**Modules** (each toggleable) by category:

| Category | Modules |
| -------- | ------- |
| **Spawn** | Spawn Rate, Spawn Burst, Spawn Shape (Point/Circle/Box/Edge/Ring), Spawn Per Unit (distance-based). |
| **Initialize** | Initial Lifetime, Velocity, Size, Color, Rotation. |
| **Update — motion** | Velocity Over Life, Gravity, Drag, Acceleration, Orbit, Noise, Attractor, Curl Noise, Vortex Force, Wind Force, Spring Force. |
| **Update — appearance** | Size Over Life, Color Over Life, Rotation Over Life, Opacity Over Life, Sub-UV Over Life, Stretch Over Life, Size/Color By Speed, Scale By Density. |
| **Update — interaction** | Collision (None/Destroy/Bounce/Slide), Particle Collision, Kill Condition (box/sphere/speed), Event Handler (spawn FX on collision/death/spawn), Conditional Module, Sub-FX (spawn another FX), Custom Script. |
| **Special** | **Light** (particles emit 2D light), **Fluid** (SPH fluid simulation with metaball or particle rendering, optional two-way coupling with rigid bodies). |
| **Render** | Sprite Renderer (sprite/material, blend mode, sub-image animation) or Ribbon Renderer (trails). |

Curves (`FXCurve`) and gradients (`FXGradient`) drive over-life properties with editable
keyframes and presets (Linear, EaseIn/Out/InOut, Bell, FadeIn/Out…).

**The FX Editor** has four regions:

* **Emitters** — the list of emitters with **+ Add Emitter** / **Remove**; selecting one
  drives everything else. Each row has a name and an **enable checkbox**, so an emitter can
  be muted without deleting it. Each emitter has its own settings block (duration, looping,
  max particles, start delay, warm-up, simulation stages, CPU/GPU) and its own transform.
* **Module stack** — four stacked lists, **Spawn**, **Initialize**, **Update** and
  **Render**, each with a `+` button whose menu offers only the modules valid for that
  stage. Each row has an enable checkbox, an `X` to remove it, and can be **dragged to
  reorder** within its stack (order matters — Update modules run top to bottom). Right-click
  a module for **Copy settings / Paste settings**, which transfers a tuned module between
  emitters or between effects.
* **Module properties** — the selected module's parameters, each with a tooltip and, where
  it applies, an editable **curve** or **gradient** with preset shapes. A short description
  at the top explains what the module does.
* **Preview** — the effect simulated live with **Play / Pause / Restart** and
  **Refresh Preview**; every edit takes effect immediately, and *Restart* clears existing
  particles so you can judge the spawn burst honestly.

Notes worth knowing:

* **Immortal particles** — *Initial Lifetime* can be set to immortal, which is what fluid
  and persistent-trail effects use; the emitter's max-particle count then becomes the real
  limit.
* **GPU simulation** is per emitter. The **Fluid** module always solves on the CPU; GPU mode
  still applies to everything else in that emitter.
* **Sub-FX** spawns another `.ice_fx` with its own offset, rotation, scale, start delay and
  timing — the way to build layered explosions without one giant emitter.

#### 4.12.1 Emitter settings

Each emitter in the list has its own settings block, independent of its modules.

| Setting | Meaning |
| ------- | ------- |
| **Name** | Label in the emitter list. |
| **Enabled** | Mute the emitter without deleting it. |
| **Duration** | Length of one emitter cycle in seconds (default `5`). |
| **Looping** | Restart the cycle when the duration elapses (default on). |
| **Max Particles** | Hard cap on live particles for this emitter (default `1000`). Immortal particles make this the real limit. |
| **Start Delay** | Seconds before the emitter begins spawning. |
| **Warmup Time** | Pre-simulates the effect for this duration when it is created, so it looks already-running on its first frame. |
| **Simulation Stages** | Update sub-steps per frame; higher is more precise and more expensive. |
| **Simulation Mode** | `CPU` (multithreaded update) or `GPU` (compute-shader update). |
| **Emitter Transform** | Position offset, rotation offset and scale multiplier — lets one effect compose several offset emitters. |

#### 4.12.2 FX module reference

Every module, grouped by the stack it belongs to. Modules can be toggled individually, and
**Update** modules run top to bottom in the order they appear in the stack.

##### Spawn

| Module | Parameters |
| ------ | ---------- |
| **Spawn Rate** | **Spawn Rate** — particles emitted per second (continuous spawn). |
| **Spawn Burst** | **Burst Time** — emitter time at which the burst fires; **Burst Count** — particles released in one burst; **Cycle Time** — period over which the burst repeats (`0` = fire once). |
| **Spawn Shape** | **Shape** — `Point`, `Circle`, `Box`, `Edge`, `Ring`; **Radius** (circle/ring); **Inner Radius** (ring — particles spawn between inner and outer radius); **Size** (box/edge); **Spawn On Edge** — spawn only on the outline instead of filling the area; **Random Direction (Radial Velocity)** — launch particles outward from the shape center instead of using the velocity setting. |
| **Spawn Per Unit** | Spawns based on how far the emitter travels. **Distance Per Spawn** — pixels the emitter must move to emit the next batch; **Particles Per Spawn** — particles emitted per batch. |

##### Initialize

| Module | Parameters |
| ------ | ---------- |
| **Initial Lifetime** | **Immortal** — particles never die (use a Kill Condition to bound them; the basis of fluids and ambient effects); otherwise **Min Lifetime** / **Max Lifetime** in seconds. |
| **Initial Velocity** | **Velocity Min** / **Velocity Max** (per-axis range); **Local Space** — interpret the velocity in the emitter's local space so it rotates with the emitter; **Inherit Emitter Velocity** + **Inherit Scale** — pass a fraction of the emitter entity's own movement to new particles (exhausts, trails, projectile sparks). |
| **Initial Size** | **Uniform Size** — one value for both axes; then **Size Min** / **Size Max**, or **Size Min XY** / **Size Max XY** when non-uniform. |
| **Initial Color** | **Color Min**; **Random Color** — pick each particle's start color randomly between **Color Min** and **Color Max**. |
| **Initial Rotation** | **Rotation Min** / **Rotation Max** in degrees; **Align To Velocity** — rotate each particle to face its direction of travel. |

##### Update — motion & forces

| Module | Parameters |
| ------ | ---------- |
| **Velocity Over Life** | **Velocity Multiplier** — a curve scaling speed across the particle's life. |
| **Gravity** | **Gravity** — a constant acceleration vector (default pulls down). |
| **Drag** | **Drag Coefficient** — air resistance; slows particles proportionally to speed. |
| **Acceleration** | **Acceleration** — a constant vector added every frame. |
| **Orbit** | **Orbit Speed** (deg/s) and **Orbit Radius** — particles circle the emitter. |
| **Noise** | **Noise Strength** — turbulence pushing particles around; **Noise Frequency** — spatial scale (lower = larger swirls). |
| **Curl Noise** | Divergence-free turbulence — smoke, magic, fire swirls. **Strength**; **Frequency** (smaller = larger swirls); **Animation Speed**; **Octaves** (layers of detail); **Lacunarity** (frequency step between octaves); **Persistence** (amplitude falloff between octaves). |
| **Attractor** | **Attractor Position**; **Strength** (positive attracts, negative repels); **Attractor Radius** — range of influence; **Attractor Falloff** — how the force weakens with distance; **Relative To Emitter** — position it relative to the emitter instead of in world space. |
| **Vortex Force** | Rotational force around a point — tornados, portals, whirlpools. **Center**; **Tangential Strength** (positive = counter-clockwise); **Radius**; **Falloff**; **Inward Pull** (positive pulls in, negative pushes out); **Relative To Emitter**. |
| **Wind Force** | **Direction**; **Strength**; **Turbulence Amount** and **Turbulence Frequency** (random perpendicular displacement); **Gust Strength** and **Gust Frequency** (occasional bursts); **Wind Drag** — how strongly particles are dragged toward wind speed. |
| **Spring Force** | Pulls particles back to their spawn position — jelly, soft bodies, elastic effects. **Stiffness** (how strongly they return); **Damping** (velocity lost to oscillation). |

##### Update — appearance

| Module | Parameters |
| ------ | ---------- |
| **Size Over Life** | **Size Multiplier** — a curve scaling size across the particle's life. |
| **Color Over Life** | **Color Gradient** — a gradient sampled by particle age. |
| **Rotation Over Life** | **Speed Min** / **Speed Max** in degrees per second. |
| **Opacity Over Life** | **Opacity** — an alpha-multiplier curve over the particle's life. |
| **Stretch Over Life** | **Stretch X** and **Stretch Y** curves (per-axis size multiplier), plus **Velocity Stretch** — stretches the particle along X proportionally to its speed. |
| **SubUV Over Life** | Maps the sprite-sheet frame to particle age (`0..1`), so the sheet plays exactly once per lifetime. Set **Sub-Images H/V** in the Sprite Renderer. |
| **Size By Speed** | **Size At Rest**; **Size At Max Speed**; **Speed Range** — the speed at which the particle reaches the max multiplier. |
| **Color By Speed** | **Speed Gradient** — color mapped from speed; **Speed Range** — the speed that maps to the right end of the gradient. |
| **Scale By Density** | Scales particle size with local SPH density — requires the **Fluid** module. **Size At Low Density**; **Size At High Density**. |

##### Update — interaction & logic

| Module | Parameters |
| ------ | ---------- |
| **Collision** | **Collision Response** — `None (Pass Through)`, `Destroy`, `Bounce`, `Slide`; **Bounce Factor** (bounce only); **Friction** (slide only); **Radius Scale** — collision radius relative to the visual size; **Destroy On Second Hit**; **Ignore Sensors** — skip trigger colliders; **Sensor Overlap Events** — report overlaps with sensors to Lua via `OnFXSensorOverlap` (damage zones, "in water" checks) without any physical response. |
| **Particle Collision** | Particles collide with each other — balls, beads, marbles. **Collision Radius**; **Bounce**; **Repulsion Force** — extra push-apart that prevents stacking. |
| **Kill Condition** | **Condition Type** — `Box Zone`, `Sphere Zone`, `Speed Min`, `Speed Max`; **Mode** — `Kill Inside`, `Kill Outside`, `Contain (Bounce Back)`; **Zone Center**; **Zone Size** (box) or **Zone Radius** (sphere); **Speed Threshold** (speed conditions); **Relative To Emitter**. |
| **Event Handler** | **Trigger** — `On Spawn`, `On Collision`, `On Death`; the **FX asset** to spawn; **Spawn Count**; **Inherit Velocity** + **Velocity Scale**. |
| **Conditional** | If/Else logic per particle. **Check Value** — `Speed`, `Age`, `Size`, `Alpha`, `Density (Fluid)`; **Comparison** — `>`, `<`, `=`; **Threshold**; then an **Action** and **Value** for the TRUE branch and for the FALSE branch. Actions are `Do Nothing`, `Multiply Alpha`, `Multiply Size`, `Multiply Velocity`, `Kill Particle`. |
| **Sub FX** | Attaches another `.ice_fx` as a child effect that follows this emitter. **FX Asset**; **Position Offset**; **Scale**; **Rotation**; **Start Delay**. |
| **Custom Script** | A Lua snippet run per particle each frame; it can read and modify the particle's fields. |

##### Update — special

| Module | Parameters |
| ------ | ---------- |
| **Light** | Each particle emits a point light. **Light Color**; **Light Intensity**; **Light Radius**; **Light Falloff** (`1` = linear, `2` = quadratic); **Inherit Particle Color** — use the particle's own color; **Cast Shadows** — allow the particle lights to cast 2D shadows (expensive with many particles). |
| **Fluid** | SPH fluid simulation between the emitter's particles. Grouped into six blocks — see the table below. |

**Fluid module parameters**

| Block | Parameters |
| ----- | ---------- |
| **SPH Core** | **Rest Density** — target density (lower = more spread out); **Stiffness (Pressure)** — how strongly particles push apart above rest density; **Near Stiffness** — near-pressure that stops particles collapsing into each other, key to the liquid look; **Viscosity** — velocity smoothing between neighbours (low = water, high = honey); **Surface Tension** — cohesion that minimizes surface area; **Interaction Radius** — neighbour search radius, best at ~2–4× particle size; **Particle Mass**. |
| **Stability & Control** | **Velocity Damping** per frame (`1.0` = none); **Max Speed** — prevents numerical blow-ups; **Collision Damping** — extra velocity lost when the fluid hits a collider; **Sub-Steps** per frame (2–3 is usually enough); **Allow Sleep** — stop simulating a settled pool and resume when disturbed (a large CPU saving; automatically disabled when a force module is present) and, with it on, **Sleep Speed** — the speed below which every particle must fall for the fluid to sleep (`0` disables sleeping). |
| **Water / Pool Settings** | **Pre-Fill Area** — instantly fill the spawn shape on the first frame (use with a Box/Circle Spawn Shape); **Pre-Fill Count**. |
| **Render Mode** | **Fluid Render** — `Particles` (individual sprites) or `Metaball (Surface)` (a smooth merged surface); **Surface Threshold** — density cutoff at the edge (lower = thicker liquid); **Blob Scale** — each particle's density contribution (higher = smoother but blurrier). |
| **Physics Interaction** | **Affect Rigid Bodies** — two-way coupling, so objects float, sink and get carried by the flow; **Push Scale** — momentum transferred to bodies (`0` = the fluid reacts to bodies but never moves them); **Body Drag** — viscous drag on bodies moving through the fluid. Requires the Collision module and a dynamic Rigidbody. |
| **Fluid-To-Fluid** | **Interact With Other Fluids** — push against particles of other fluid emitters that also have this on, so water and lava do not pass through each other; the lighter fluid (lower Particle Mass) floats on top. **Separation** — how hard they push apart; **Mixing Drag** — how much they drag each other along. |

> Tips shown in the panel: add a **Collision** module for the fluid to interact with
> colliders, and a **Gravity** module to make it fall and pool.

##### Render

An emitter has exactly one Render module, and it is either a sprite renderer or a ribbon
renderer.

| Module | Parameters |
| ------ | ---------- |
| **Sprite Renderer** | **Sprite Path** — the `.ice_sprite` drawn per particle; **Use Custom Material** + **Material Path** (`.ice_material` / `.ice_matinst`, `Surface` domain); **Blend Mode** — `Alpha`, `Additive` (glow), `Multiply`; **Sub-Images H** / **Sub-Images V** — sprite-sheet grid; **Animate Sprite** + **Animation Speed** (FPS) — play the sheet as an animation; **Shading Mode** — `Unlit` or `Lit` (affected by scene lights). Assigning a custom material takes over blend and shading. |
| **Ribbon Renderer** | Draws trails behind particles. **Sprite Path**; **Texture Mode** — `Stretch`, `Tile`, `Distance Based`; **Use Custom Material** + **Material Path**; **Blend Mode**; **Ribbon Width**; **Max Segments** — points kept in the trail; **Min Vertex Distance** — distance before a new point is added (smooths trails); **Width Over Life** curve; **Color Over Life** gradient. |

##### Curves and gradients

Any *Over Life* parameter is an editable **curve** (`FXCurve`) or **gradient**
(`FXGradient`). Both editors let you drag, add and remove keys, and both offer one-click
presets: curves have `Linear`, `Constant`, `Ease In`, `Ease Out`, `Ease In/Out` and `Bell`;
gradients have `Solid White`, `Fade Out`, `Fade In` and `Fade In/Out`.

### 4.13 Widget / UI (`.ice_widget`)

**Editor:** Widget Editor

A **widget** is a UI document — a canvas of UI **elements** with layout, styling,
animation and event hooks. It is the building block for menus, HUDs and dialogs.

There are **19 element types**, in five categories: `Panel`, `Text`, `Button` and
`Image` (*Common*); `InputField`, `Slider`, `Checkbox`, `Toggle` and `Dropdown`
(*Input*); `ProgressBar` and `Throbber` (*Display*); `HorizontalBox`, `VerticalBox`,
`SizeBox`, `Overlay` and `ScrollView` (*Layout*); `Spacer`, `WidgetSwitcher` and
`SubWidget` (*Misc*). Each one is described in
[4.13.2](#4132-element-type-reference).

Every element carries the same core set of properties — transform and anchoring,
optional content-driven sizing, depth and post-processing, appearance, tooltip,
clipping, nine-slice, gamepad navigation and interaction callbacks — plus the
document-level canvas settings. They are documented field by field in
[4.13.1](#4131-shared-element-properties).

**The Widget Editor** is a hierarchy, a canvas and an inspector:

* **Hierarchy** — the element tree with a **search box**, **+ Add Element** (grouped into
  *Common*, *Input*, *Display*, *Layout* and *Misc*), a per-element *add child* menu,
  **move up/down** to reorder siblings, **Move to Root**, copy/paste, and **Reparent** with
  cycle detection. Elements inherited from a parent widget are marked as such.
* **Canvas** — the widget rendered at its real proportions. Click to select, drag to move,
  and drag any of the eight **resize handles** (four corners, four edges). A **Grid** toggle
  snaps movement and resizing; **middle-drag** pans and **scroll** zooms. The anchor widget
  in the inspector shows the 9 presets plus the stretch variants.
* **Animation panel** — toggled from the toolbar and docked under the canvas.
  **+ Add Track** targets an element (or the widget root) and a property — position X/Y,
  size width/height, rotation, scale X/Y, opacity, color R/G/B/A — then **+ Add Keyframe** at
  the playhead with an **easing** (`Linear`, `EaseIn`, `EaseOut`, `EaseInOut`, `Bounce`,
  `Elastic`). Animations have a duration, **loop** and **autoplay** flags and can carry
  timed **events** that call a script function with a parameter. **Play / Stop** and a
  reset button preview the animation right on the canvas.
* **Script tab** — either a **Lua** text editor or a **Visual Script** node graph, chosen
  per widget, with a **Compile** button that validates the graph and reports the error
  inline.

> Event callbacks reference script function names; the Lua API itself is documented in
> [LuaAPI-EN-DOC.md](LuaAPI-EN-DOC.md).

#### 4.13.1 Shared element properties

The inspector shows these groups for **every** element, in this order. Type-specific groups
([4.13.2](#4132-element-type-reference)) are inserted where they apply.

**Transform**

| Property | Meaning |
| -------- | ------- |
| **Name** | Element name — also how scripts look the element up. Unique within the widget. |
| **Position** | Offset from the anchor, in canvas pixels. |
| **Size** | Width and height. Read-only (and driven by the content) while **Desired Size** is on. |
| **Desired Size** | Off by default. While on, the element sizes itself to its content instead of a fixed value — measured text, sprite/flipbook pixel size, sub-widget canvas, throbber diameter, children bounds, or the horizontal/vertical box flow (spacing + padding) — and the **Size** field shows the live computed value. |
| **Scale** | X/Y multiplier applied around the pivot. |
| **Pivot** | Normalized origin for rotation and scaling (default `0.5, 0.5`). |
| **Rotation** | Degrees. |
| **Opacity** | `0..1`, multiplies down through children. |
| **Anchor** | One of 9 corner/edge presets plus 7 stretch variants (`StretchLeft/Center/Right`, `StretchTop/Middle/Bottom`, `StretchAll`). Picking one keeps the element where it is and only rewrites `Position`; **Shift + click** snaps it to `Position = 0,0` of that anchor instead. Stretch anchors make the element fill the parent rectangle on that axis, and `Scale` then grows or shrinks that rectangle around the pivot. |
| **Custom Anchors** | Use free **Anchor Min** / **Anchor Max** ranges (`0..1`) instead of a preset, so each edge tracks the parent independently. |
| **Z Order** | Draw order within this widget. |
| **Global Z** | Sort this element against the *scene* by Z instead of inside the UI overlay. A negative Z Order with Global Z on turns the element into a scene background. |
| **Post Processed** | Draw the element inside the scene so it receives bloom, exposure, depth of field and so on. Off (default) draws it as a crisp overlay after post-processing. Forced on, and disabled in the UI, for a Global-Z background. |
| **Visible** | Hide the element (and its children) without deleting it. |

**Appearance**

| Property | Meaning |
| -------- | ------- |
| **Color** | Tint (RGBA). For a Panel/Image it is the fill; for a sprite it multiplies the sprite. |
| **Lighting** | `Unlit` or `Lit` — whether the element takes part in the dynamic 2D lighting and shadow system. Available on Image, Button, Throbber, Panel, ProgressBar, Slider, Checkbox, Dropdown, ScrollView, Text, InputField and Toggle. |
| **Shadow Receiver** | (`Lit` only) let dynamic shadows fall onto this element, in both world-space and screen-space widgets. |
| **Sprite** | A `.ice_sprite` **or** `.ice_flipbook` drawn as the element's visual. Available on Image, Button, Throbber, InputField, Checkbox, Toggle and Dropdown. Assigning a flipbook clears the sprite and vice versa. |

**Tooltip** *(all elements)*

| Property | Meaning |
| -------- | ------- |
| **Tooltip Text** | Text shown when the player hovers this element at runtime. |
| **Tooltip Delay** | Seconds the pointer must hover before it appears. |
| **Font** / **Font Size** | Shown once a tooltip text exists, for element types that have no text block of their own. |

**Clipping** *(all elements)* — **Clip Children** masks descendants to this element's rectangle.

**9-Slice** — on Panel, Button, Image, ScrollView, Slider, ProgressBar, InputField, Checkbox,
Toggle and Dropdown. **Use 9-Slice** stretches the sprite with fixed corners; **Border
(L,T,R,B)** sets the border widths in source-texture pixels.

**Gamepad Navigation** *(interactable elements)* — **Nav Up / Down / Left / Right** each
point at another element, or `Auto` to let the engine pick the nearest neighbour in that
direction.

**Interaction**

| Property | Meaning |
| -------- | ------- |
| **Interactable** | Allow the element to receive hover, click and focus input. Everything below appears only while it is on. |
| **OnClick / OnHover / OnUnhover / OnPressed / OnReleased** | Names of Lua functions called on those events. |
| **OnValueChanged** | Only for Slider, InputField, Checkbox, Dropdown and Toggle. |
| **OnFocusGained / OnFocusLost** | Called when the element gains or loses input focus. |
| **State Colors** | **Use Custom State Colors** replaces the built-in hover/press tints with **Hovered Color** and **Pressed Color**. |
| **State Sounds** | **Use Custom State Sounds** plays **Hovered Sound** and **Pressed Sound** on interaction. |

**Document-level settings** (the widget itself, not an element)

| Setting | Meaning |
| ------- | ------- |
| **Canvas Size** | The widget's design resolution. Read-only while **Desired Size** is on, which makes the canvas hug its root elements. |
| **Scale With Screen** | Scale the whole widget with the viewport instead of using raw pixels. |
| **Stretch Mode** | `Stretch`, `Letterbox`, `MatchWidth`, `MatchHeight`. |
| **Safe Area** + **Insets (L,T,R,B)** | Keep content clear of notches and rounded corners. |
| **Parent Widget** | Inherit another widget's canvas settings and elements. **Reparent** changes it with cycle detection; inherited elements are marked in the hierarchy and can be overridden per field. |
| **Is Element** | Marks the document as a reusable building block meant to be embedded through `SubWidget` rather than shown on its own. Only widgets with this flag can be picked by a SubWidget element. |
| **Script** | A Lua text script or a Visual Script graph, chosen per widget. |
| **Animations** | See the **Animation panel** above. |

#### 4.13.2 Element type reference

| Element | Category | What it is | Own properties |
| ------- | -------- | ---------- | -------------- |
| **Panel** | Common | A rectangle used as a background or container. | **Backdrop Blur** — frosted-glass blur of whatever is behind the panel (lower the panel color's alpha to reveal it; the real blur appears in Play mode, the editor canvas shows an approximation) and **Blur Strength**. |
| **Text** | Common | A text label. | **Text** (multi-line) or **Localization Key**; **Font**, **Font Size**, **Text Color**; **Horizontal / Vertical Alignment**; **Text Wrap**. |
| **Button** | Common | A clickable element with hover/press states. | Uses the shared sprite, state colors and state sounds; its label is normally a child Text element. |
| **Image** | Common | A sprite or flipbook. | Uses the shared sprite and 9-slice groups. |
| **Input Field** | Input | An editable text box. | The Text group (text, font, size, color, alignment, wrap) plus **Max Length** (`0` = unlimited). |
| **Slider** | Input | A draggable value control. | **Min**, **Max**, **Value**; **Fill Color**; **Bar Sprite/Flipbook**; **Thumb Sprite/Flipbook**. |
| **Checkbox** | Input | An on/off box. | **Is Checked**; **Check Color**; **Check Sprite**. |
| **Toggle** | Input | A sliding on/off switch. | **Toggled**; **Toggled Color**; **Untoggled Color**; **Handle Ratio** (`0.2`–`0.8`) sizes the handle relative to the track; **Handle Sprite**. |
| **Dropdown** | Input | A selection list. | **Options** list (add / edit / remove); **Selected Index**; **Max Height** before the list scrolls; **Font**, **Font Size**, **Text Color**. |
| **Progress Bar** | Display | A fill bar. | **Min**, **Max**, **Value**; **Fill Color**; **Bar Sprite/Flipbook**. |
| **Throbber** | Display | A loading spinner. | **Speed**; **Radius**; **Dots**; **Clockwise**; **Paused**. |
| **HorizontalBox** | Layout | Lays children out left to right. | **Spacing** between children; **Padding (L,T,R,B)**. |
| **VerticalBox** | Layout | Lays children out top to bottom. | **Spacing**; **Padding (L,T,R,B)**. |
| **SizeBox** | Layout | Forces a fixed size on its child. | **Override Width**; **Override Height**; **Override Size** (the width/height applied when the overrides are on). |
| **Overlay** | Layout | Stacks children on top of each other in the same rectangle. | None beyond the shared groups. |
| **ScrollView** | Layout | A scrollable viewport. | **Content Size** (`0,0` = auto-calculate from children bounds); **Scroll Offset**; **Scrollbar V**; **Scrollbar H**; **Drag Scroll** (touch/mouse drag scrolling). |
| **Spacer** | Misc | An invisible gap, used inside layout boxes. | None beyond the shared groups. |
| **WidgetSwitcher** | Misc | Shows exactly one of its children. | **Active Child** — the index of the visible child. |
| **SubWidget** | Misc | Embeds another widget document. | **Widget (.ice_widget)** — only widgets marked **Is Element** can be picked. |

> The categories match the **+ Add Element** menu, which is grouped into *Common*, *Input*,
> *Display*, *Layout* and *Misc*.

### 4.14 View / Post-Process Volume (`.ice_view`)

**Editor:** View Editor

A **view** defines a **post-processing volume** and/or a **navigation-grid volume**
with bounds, blend radius and priority — like a camera/post-process volume that the
active camera blends into.

The post-process stack is extensive:

* **Bloom** (intensity, threshold, radius) and **Bloom Dirt**.
* **Color Grading** (saturation, contrast, gamma, tint) and a **LUT**.
* **Tonemap** (`None`/`Reinhard`/`ACES`) and **Auto Exposure** (compensation, min/max,
  speeds).
* **Vignette**, **Film Grain**, **Chromatic Aberration**.
* **Ambient Occlusion (SSAO)**, **Screen-Space Reflections (SSR)**.
* **Godrays**, **Lens Flare**, **Lens Sharpen**, **CAS** (contrast-adaptive sharpen).
* **Depth of Field**, **Motion Blur**.
* **Global Illumination** (radiance cascades), a procedural **Sky**.
* **Fog** (Linear/Exp/Exp²) and **Volumetric Fog**.
* **Heat Haze** and **Underwater** (tint, distortion, caustics).
* **Custom post-process materials** (materials with `Domain = PostProcess`), each with
  strength, an enable toggle and **before/after** placement relative to the built-in stack.
  Add and remove them from the list at the bottom of the panel.

The **navigation grid** settings define how AI pathfinding is built in that volume:
**Mode** (`Top View` for top-down grids or `Side View` for platformers), **Cell Size**,
**Allow Diagonal**, **Auto Build From Colliders**, **Agent Radius** and **Agent Height**,
the side-view **Max Jump Height / Max Jump Distance / Max Fall Height** limits, and its own
bounds (or **Infinite / Unbound**).

**The View Editor** mirrors the asset: a **Volume settings** block (name, bounds or
infinite/unbound, blend radius, priority), then two independently-toggled sections —
**Post-Process Volume** and **Nav Grid Volume**. Every effect is a collapsible group that
only shows its parameters when enabled, and each field has a tooltip. Saving rebuilds the
post-process volumes in the open level, so changes are visible in the viewport at once.

> A view is used by placing it as a **world asset** in a level; see the
> [Editor](Editor-EN-DOC.md) document for placing and blending volumes, and
> [Graphics](Graphics-EN-DOC.md) for what each effect actually does.

### 4.15 Cinema (`.ice_cinema`)

**Editor:** Cinema Editor

A **cinema** is a **timeline-based editor** for cutscenes and scripted camera moves.

* **Duration**, **Frame Rate**, **Loop**, **Playback Rate**, and a starting camera
  position/zoom.
* **Tracks**, each holding **keyframes**, with a name and **mute** / **lock** flags.
  A keyframe is either a **Camera** key (position + zoom) or a **Lua Callback** key (fire a
  named function at that time), with an **easing** curve (`Linear`, `EaseIn`, `EaseOut`,
  `EaseInOut`, `Bounce`, `Elastic`) and a duration.

**The Cinema Editor** is a track/timeline UI:

* **Toolbar** — duration, frame rate, loop, playback rate, **Snap** (snap the playhead and
  keyframes to whole frames), and the timeline zoom.
* **Start camera** — the position/zoom the shot begins at, with **Capture Start** to take the
  current editor camera and **Go To Start** to send the editor camera back there.
* **Tracks** — **+ Add Track**, **Duplicate Track**, delete, mute and lock. Add keyframes of
  either type, drag them along the timeline, and multi-select to move several at once.
* **Inspector** — the selected keyframe's time, duration, easing and payload. For a camera
  key, **Capture Current Camera** writes the *current editor camera* position and zoom into the
  keyframe — frame the shot in the viewport, then press the button. **Go To** does
  the reverse and moves the editor camera to the stored value.
* **Preview** — scrub or play the timeline and watch the editor camera follow it.

> Camera positions are stored **relative to the cinema's placement in the level**, so the
> same cutscene can be reused at different spots in a map.

### 4.16 AI / Behavior Tree (`.ice_ai`)

**Editor:** AI Editor

An **AI asset** is a **behavior tree** plus its **blackboard** key definitions. Trees
are ticked per-entity at runtime.

**Node types:**

| Kind | Nodes |
| ---- | ----- |
| **Composites** | `Selector`, `Sequence`, `Parallel` (require-one/require-all policy), `RandomSelector`. |
| **Decorators** | `Inverter`, `Repeater` (repeat count), `RepeatUntilFail`, `Cooldown` (seconds), `BlackboardCondition` (key, operation, value, **abort mode**), `ForceSuccess`, `ForceFailure`, `TimeLimit`. |
| **Tasks** | `Wait` (duration), `MoveTo` (target blackboard key + acceptable radius), `SetBlackboardValue` (typed value), `ClearBlackboardValue`, `PlayAnimation` (state name), `RunEQS` (query + result key), `CustomLua` (function name). |
| **Services** | Periodic ticks (interval + Lua function) attached to a node. |

Supporting systems: the **Blackboard** (typed key/value memory), **EQS**
(Environment Query System) for spatial scoring queries, and **AI Perception**
(sight/sense). Conditions and custom tasks can call into Lua.

**The AI Editor** is a node graph with a blackboard panel:

* **Blackboard keys** — **+ Add Key**, name it and give it a type; every node that consumes a
  key picks from this list instead of accepting free text, so typos cannot silently break a
  tree.
* **Graph** — **Create Root (Selector)** starts the tree; each node's context menu offers
  **Add Node** (filtered to what that node type accepts), **Add Service** and
  **Delete Node**. Children are ordered left to right, which is the order composites
  evaluate them in.
* **Node properties** — the selected node's parameters (the table above), each with a
  tooltip. `BlackboardCondition` also has an **Abort (Observer)** mode that decides whether a change in
  the key can interrupt a branch that is already running — the standard way to make an enemy
  drop what it is doing when it spots the player.
* Custom Lua tasks return their status to the tree; the panel reminds you of the expected
  return values.

### 4.17 Class (`.ice_class`)

**Editor:** Class Editor

A **class** is IceBox's **reusable game object**: an entity definition
with components, scripting and **inheritance**. Drop a class into a level (drag from the
browser) to instantiate it as an entity; create a **child class** to specialize a parent.

The Class Editor is a mini-scene with:

* **A viewport** showing the assembled entity, with a grid, a lit/unlit preview and the
  debug overlays. **Reset Camera** re-frames the entity; you can edit either the
  **local** or the **global** transform.
* **A components panel** to add and configure components — Transform, Sprite Renderer,
  Collider (box/sphere/capsule), Tilemap Renderer, Flipbook, Audio, FX, Widget,
  Rigidbody (with ragdoll settings), Animator, Skeleton, Camera, Decal, Light (point/spot),
  Point Marker, AI, Joint, Stencil, Destructible, **Replication** (multiplayer sync), and
  nested **Class Components** (compose other classes).
* **Multiple instances per component.** Most visual and physical components are lists —
  a class can hold several sprites, colliders, lights, audio sources or joints at once.
  Each instance is named, can be reordered (**move up/down**), and has its own **socket
  attachment** settings. Right-click a component for **Copy settings / Paste settings**.
* **Three tabs** — **Components** (the list above), **Inheritance** (shown when the class
  has a parent) and **Debugging** (per-class viewport debug flags saved with the asset).
* **A script editor** offering either a **Lua text** script or a **Visual Script**
  node graph — your choice per class — with a **Compile** button. A child class shows its
  parent's script read-only alongside its own.
* **Inheritance** — a class can have a parent; it inherits the parent's components and
  script. Inherited components are labeled, can be **overridden** per field and
  **reset to parent** individually, and the *Inheritance* tab shows the full parent chain.
  **Reparent** changes the parent with cycle detection; **Show/Hide Parent** toggles the
  parent's visuals in the preview.
* **Is Component** marks the class as a building block intended to be nested inside other
  classes rather than placed in a level directly.

> Deleting a class from the Content Browser also deletes the classes that inherit from it
> — see [3.11](#311-deleting-assets--reference-safety).

> The class stores component data and script references. The Lua/Visual scripting APIs
> are out of scope here — see [LuaAPI-EN-DOC.md](LuaAPI-EN-DOC.md).

#### 4.17.1 Component reference

**Add Component** groups the catalogue into six sub-menus:

| Menu | Components |
| ---- | ---------- |
| **Rendering** | Sprite Renderer, Tilemap Renderer, Animator, Skeleton, Flipbook, Decal, Camera. |
| **Lighting** | Point Light, Spot Light. |
| **Collisions** | Rigidbody, Box Collider, Sphere Collider, Capsule Collider, Joint, Destructible. |
| **Audio & Effects** | Audio, FX. |
| **UI** | Widget, Point Marker. |
| **Logic** | AI Brain, Class Component. |

**Stencil Mask** and **Replication** are always available on the entity rather than added
from this menu, and **Transform** is intrinsic. Components marked *(single)* below exist
once per class; the rest are lists you can add many instances to. A class marked
**Is Component** cannot take the single-instance components (Animator, Skeleton, Camera,
Rigidbody, Destructible, AI), because those belong to the class that hosts it.

**Properties every instance shares**

| Property | Meaning |
| -------- | ------- |
| **Name** | Instance name — how scripts and *Attach To* fields refer to it. |
| **Local Transform** | Position, Rotation and Scale relative to the entity. |
| **Visible** | Show the instance in the editor viewport. |
| **Render In Game** | Show the instance during Play mode. |

Instances can be reordered (**move up/down**), and any component header offers
**Copy settings / Paste settings**.

**Attachment** — offered on **Sprite Renderer** and **Flipbook** instances only:

| Property | Meaning |
| -------- | ------- |
| **Attach To Socket** | Follow a named socket (attach point) on this entity's sprite, flipbook or skeleton; the engine recomputes position and rotation every frame, correct under entity rotation and scale. |
| **Socket Source** | Which component provides the socket. `Auto` searches the skeleton first, then sprites, then flipbooks. |
| **Socket Source Index** | Instance index of the source component (`-1` searches every instance). |
| **Inherit Flip X** | Copy Flip X from the socket owner so the attached art mirrors with the character. |
| **Socket Offset** / **Socket Offset Rotation** | Extra offset and rotation applied on top of the socket, in socket space — scales with the entity and mirrors with Flip X. Use it to fine-tune placement or to drive recoil from a script. |
| **Attach To Collider** | Follow a **Separate Body** collider part of this entity (position and rotation). |

**Shadow properties** — shared by Sprite, Flipbook, Skeleton and the colliders:

| Property | Meaning |
| -------- | ------- |
| **Cast Shadow** | Whether this instance casts 2D shadows. |
| **Cast Shadow Mode** | `Colliders` — cast from the asset's collision shapes; `Contour` — cast from the texture's alpha silhouette, even without a collider. (Sprite / Flipbook / Skeleton.) |
| **Shadow Origin** | Where the shadow is cast from along the caster's vertical axis: `Top`, `Center` or `Bottom`. |
| **Shadow Edge Fade** | Softens shadow edges by shrinking the silhouette inward (`0` = sharp, `1` = fully collapsed). |
| **Shadow Z Order** | Z offset of the shadow plane relative to the caster. The shadow only darkens objects whose render Z is below that plane — negative pushes it toward the background, positive brings it forward. |
| **Don't Block Shadows** | With *Colliders Block Shadows* enabled globally, this object still lets shadows pass through it. Turn it off on terrain/tiles so they block shadows. |

##### Rendering

**Sprite Renderer** — one or more sprite instances.

| Property | Meaning |
| -------- | ------- |
| **Sprite** | The `.ice_sprite` to draw. |
| **Color** | Tint (RGBA). |
| **Flip X / Flip Y** | Mirror the sprite on each axis. |
| **Material** | `Shading Mode`, `Blend Mode` and `Alpha Clip Threshold` for this instance, or a custom material assigned on the sprite asset. |
| shadows, visibility, socket/collider attach | See the shared tables above. |

**Tilemap Renderer** — one or more tilemap instances.

| Property | Meaning |
| -------- | ------- |
| **Tilemap** | The `.ice_tm` to draw (drag one in from the Content Browser). |
| **Layers** | Read-out of the map's layers. |
| **Flip X / Flip Y** | Mirror the map. |
| — | Shadows are authored **per tile** in the Tileset and Tilemap editors, not here. |

**Flipbook** — one or more flipbook instances.

| Property | Meaning |
| -------- | ------- |
| **Flipbook** | The `.ice_flipbook` to play. |
| **Playing** | Start playing immediately. |
| **Speed Multiplier** | Playback rate. |
| **Color**, **Flip X / Flip Y** | As for sprites. |
| **Material** | Shading / blend / alpha-clip, or the flipbook's own material. |
| shadows, visibility, socket/collider attach | See the shared tables above. |

**Animator** *(single)* — drives a flipbook from an `.ice_animation` state machine.

| Property | Meaning |
| -------- | ------- |
| **Animation** | The `.ice_animation` state machine. |
| **Target Sprite** | Which sprite instance the animation drives (`Auto` = the first one). |
| **State** | Read-out of the current state, frame and time while previewing. |

**Skeleton** *(single)* — a `.ice_skeleton` rig.

| Property | Meaning |
| -------- | ------- |
| **Skeleton** | The `.ice_skeleton` asset. |
| **Animation** | The animation to play, and **Loop**. |
| **Color**, **Flip X / Flip Y** | As for sprites. |
| **Shading Mode**, **Blend Mode**, **Material** | Render attributes for the whole rig. |
| **Bone Colliders** | Per-bone physics colliders that follow the animation and become dynamic when the ragdoll activates. |
| **Ragdoll Enabled** | Allow the physics ragdoll to be switched on at runtime. |
| **Ragdoll on Start** | Activate the ragdoll as soon as Play begins. |
| **Ragdoll Angular Damping** | Angular damping while the ragdoll is active (higher = less flailing). |
| **Ragdoll Gravity Scale** | Gravity multiplier while the ragdoll is active. |
| shadows, visibility | See the shared tables above. |

**Camera** *(single)*

| Property | Meaning |
| -------- | ------- |
| **Primary** | Use this camera as the active view. Only one primary camera renders at a time. |
| **Ortho Width** | Visible world width in pixels — larger means more of the scene is seen. |
| **Offset** | Offset of the camera center from the entity position. |
| **Background** | Color the camera clears to where nothing is drawn. |
| **Near Plane / Far Plane** | Z clip planes. |
| **Player Index** | Split-screen slot (`-1` = not assigned, `0`–`3` = player). |
| **Viewport Rect** | Viewport rectangle (X, Y, Width, Height) in normalized `0..1` coordinates. |

**Decal** — decals placed by hand as part of the level: graffiti, cracks, stains. Runtime
marks (bullet holes, blood) are spawned from script instead — see
[LuaAPI-EN-DOC.md](LuaAPI-EN-DOC.md) section 62.

| Property | Meaning |
| -------- | ------- |
| **Decal Asset** | The `.ice_decal` this instance renders. |
| **Color** | Tint multiplied into the decal asset color; alpha scales opacity. |
| **Size Override** | Size in pixels; zero keeps the size from the decal asset. |
| **Variant** | Texture variant index; `-1` picks one automatically from the asset's weights. |
| **Override Sort Order** / **Sort Order** | Use an explicit draw order instead of the asset's. |
| **Flip X / Flip Y** | Mirror the decal. |
| **Visible** / **Render In Game** | Editor visibility, and whether it also draws in Play mode. |

##### Lighting

**Point Light** and **Spot Light** instances live in the same component.

| Property | Point | Spot | Meaning |
| -------- | :---: | :--: | ------- |
| **Enabled** | ● | ● | Turn the light on or off. |
| **Light Color** | ● | ● | Color of the light. |
| **Intensity** | ● | ● | Brightness. |
| **Radius** | ● | ● | Attenuation radius in pixels; the light fades to zero at this distance. |
| **Falloff Exponent** | ● | ● | How light fades with distance (`1` = linear, `2` = quadratic). |
| **Cast Shadows** | ● | ● | Requires `Lit` mode and shadows enabled in Settings. |
| **Direction** | | ● | Where the spot points, in degrees. |
| **Inner Angle** | | ● | Cone angle of full brightness in the center. |
| **Outer Angle** | | ● | Cone angle where the light fades to zero; must be ≥ the inner angle. |
| **Cookie Texture** | ● | ● | A texture that masks/patterns the light, with its own **Cookie Intensity** and **Cookie Rotation**. |

##### Collisions

**Rigidbody** *(single)*

| Property | Meaning |
| -------- | ------- |
| **Body Type** | `Static` (never moves), `Dynamic` (full physics) or `Kinematic` (script-driven, ignores forces). |
| **Fixed Rotation** | Lock rotation so the body never spins from collisions or torque. |
| **Gravity Scale** | Multiplier on world gravity (`0` floats, `1` normal, negative floats up). |
| **Linear Damping** | Slows linear velocity over time (air drag). |
| **Angular Damping** | Slows spin over time. |
| **Is Bullet (CCD)** | Continuous collision detection for fast-moving objects. |
| **Allow Sleep** | Let the body sleep at rest to save CPU. |
| **Ragdoll Enabled** | Allow ragdoll mode to be switched on from Lua. |
| **Ragdoll Gravity Scale** | Gravity multiplier while the ragdoll is active. |
| **Ragdoll Angular Damping** | Angular damping while the ragdoll is active. |

**Colliders** — Box, Sphere and Capsule instances share one component and almost all their
properties.

| Property | Box | Sphere | Capsule | Meaning |
| -------- | :-: | :----: | :-----: | ------- |
| **Size** | ● | | | Width and height in pixels. |
| **Radius** | | ● | ● | Circle / capsule radius in pixels. |
| **Length** | | | ● | Length of the capsule's straight section. |
| **Density** | ● | ● | ● | Mass per area (kg/m²); with the shape this gives the body its mass. `0` = massless. |
| **Friction** | ● | ● | ● | Surface friction (`0` = ice, `1` = grippy), combined with the other contact's friction. |
| **Restitution** | ● | ● | ● | Bounciness (`0` = none, `1` = full). |
| **Is Sensor** | ● | ● | ● | Detect overlaps and fire events without blocking movement (trigger volume). |
| **One-Way Platform** + **Direction** | ● | ● | ● | Solid only from one side (`Up`, `Down`, `Left`, `Right`). |
| **Collision Group** | ● | ● | ● | The Collision Matrix in Settings decides which groups interact. |
| **Collision Enabled** | ● | ● | ● | `NoCollision`, `QueryOnly`, `PhysicsOnly` or `QueryAndPhysics`. |
| **Contact Events** | ● | ● | ● | Fire begin/end contact callbacks. |
| **Sensor Events** | ● | ● | ● | Fire overlap callbacks. |
| **Hit Events** | ● | ● | ● | Fire hit callbacks with impact speed and normal on hard collisions. |
| **Pre-Solve Events** | ● | ● | ● | Fire a callback before the solver resolves each contact (advanced custom response). |
| **Separate Body** | ● | ● | ● | Simulate this collider as its own dynamic body (a *part*) instead of a shape on the entity body. Connect it with a joint (*Target Part*) and attach sprites to it. |
| shadow properties | ● | ● | ● | See the shared shadow table. |

**Joint** — connects this entity's body to another body.

| Property | Applies to | Meaning |
| -------- | ---------- | ------- |
| **Joint Type** | all | `Revolute` (hinge), `Distance` (rope/spring), `Weld`, `Prismatic` (slider), `Wheel`, `Motor`. |
| **Target Entity Tag** | all | Tag of the entity to connect to (it needs a Rigidbody). |
| **Target Part** | all | Connect to a *Separate Body* collider part of **this** entity instead; overrides the target tag. |
| **Anchor A / Anchor B** | all | Local anchors on each body, in pixels from the body origin. |
| **Collide Connected** | all | Let the two connected bodies still collide with each other. |
| **Enable Spring**, **Hertz**, **Damping Ratio** | revolute, distance, prismatic, wheel | Make the joint springy: stiffness in oscillations per second, and damping (`0` = bouncy, `1` = critically damped). |
| **Enable Limit** | revolute, distance, prismatic, wheel | Restrict motion to the range below. |
| **Lower / Upper Angle** | revolute | Angle limits in degrees. |
| **Min / Max Length** | distance | Distance limits in pixels. |
| **Lower / Upper Translation** | prismatic, wheel | Slide limits in pixels along the axis. |
| **Enable Motor** | revolute, distance, prismatic, wheel | Drive the joint toward a target speed with a capped force/torque. |
| **Motor Speed** | revolute, wheel *(deg/s)*; distance, prismatic *(px/s)* | Target speed the motor drives toward. |
| **Max Motor Torque** | revolute, wheel | Cap on the torque the motor may apply. |
| **Max Motor Force** | distance, prismatic | Cap on the force the motor may apply. |
| **Reference Angle** | revolute, weld, prismatic | Resting angle between the two bodies when the joint is created. |
| **Local Axis A** | prismatic, wheel | Axis of allowed translation, in body A's local space. |
| **Linear / Angular Hertz** and **Linear / Angular Damping** | weld | Spring stiffness and damping of the weld — it has no Enable Spring toggle, these are always shown. |
| **Linear Offset**, **Angular Offset** | motor | Target position (pixels) and angle (degrees) of body B relative to body A. |
| **Max Force**, **Max Torque**, **Correction Factor** | motor | Corrective limits, and how aggressively the joint corrects position error (`0..1`). |
| **Break Force**, **Break Torque** | all | Thresholds at which the joint breaks. `0` = unbreakable. |

**Destructible** *(single)* — lets the entity fracture into debris.

| Property | Meaning |
| -------- | ------- |
| **Enabled** | Allow this entity to fracture at runtime. |
| **Destruct On Start** | Fracture automatically when Play mode starts. |
| **Health** | Hit points before it fractures, reduced by damage and qualifying impacts. |
| **Pattern** | `Grid`, `Radial` (from the impact) or `Random` shards. |
| **Fragment Count** | How many fragments it breaks into. |
| **Explosion Force** | Outward impulse applied to the fragments. |
| **Impact Threshold** | Minimum hit speed that auto-fractures it (`0` = manual only). |
| **Fragment Lifetime** / **Fragment Fade Time** | Seconds a fragment lives, then how long it takes to fade out. |
| **Fragment Gravity / Density / Friction / Restitution** | Physics of the debris. |
| **Fragment Is Sensor** | Make fragments non-blocking sensors instead of solid debris. |
| **Fragment Contact / Sensor / Hit / Pre-Solve Events** | Which callbacks fragments fire. |
| **Fragment Collision Group** | Collision group for the debris (`Default (Debris)` uses the built-in group). |
| **Max Debris Count** | Cap on live debris entities; the oldest are removed first. |
| **Fragment shadow settings** | Cast Shadow, Don't Block Shadows, Shadow Origin, Shadow Edge Fade, Shadow Z Order — as in the shared shadow table, applied to fragments. |
| **Destroy Original** | Remove the original entity once it fractures. |

##### Audio & Effects

**Audio** — one or more sound sources.

| Property | Meaning |
| -------- | ------- |
| **Sound** | The audio file to play (its `.ice_sound` sidecar supplies the defaults). |
| **Group** | Mixing group: `Master`, `Music`, `SFX`, `Voice`, `Ambient`, `UI`. |
| **Volume** / **Pitch** | `-1` uses the asset default; otherwise the custom value. |
| **Override Loop** + **Loop** | Override the asset's loop flag. |
| **Play On Wake** | Start playing when the entity spawns / Play begins. |
| **Override Spatial** + **Spatial (3D)** | Override the asset's 3D flag, then **Min Distance**, **Max Distance** and **Rolloff**. |

**FX** — one or more particle effects.

| Property | Meaning |
| -------- | ------- |
| **FX** | The `.ice_fx` asset. |
| **Playing** | Simulate immediately. |
| **Loop** | Restart the effect when its emitters finish. |
| **Speed Multiplier** | Simulation rate. |
| **Play On Wake** | Start when the entity spawns / Play begins. |
| **Flip X / Flip Y** | Mirror the effect. |
| shadow properties | Cast Shadow here means the effect's **particle lights** may cast shadows — the FX asset's Light module must also allow it. |

##### UI

**Widget** — one or more UI documents attached to the entity.

| Property | Meaning |
| -------- | ------- |
| **Widget** | The `.ice_widget` document. |
| **Screen Space** | Draw as a screen overlay (HUD) instead of in the world. |
| **Scale** | Size multiplier. |
| **Render Order** | Sorting among widgets. |
| **Interactable** | Whether the widget receives input. |
| **Player Index** | `-1` = shared across all players (HUD); `0`–`3` = visible only in that local player's split-screen viewport. |
| **Flip X / Flip Y** | Mirror the widget (world-space use). |

**Point Marker** — editor-visible debug markers.

| Property | Meaning |
| -------- | ------- |
| **Shape** | `Arrow`, `Line`, `Circle`, `Square`. |
| **Color** | Marker color. |
| **Size** | Overall size in pixels. |
| **Thickness** | Line thickness in pixels. |
| **Arrow Head Size** / **Arrow Direction** | Arrow only — head size and the direction it points, in degrees. |
| **Line End Offset** | Line only — the end point relative to the entity. |
| **Visible** / **Render In Game** | Editor visibility, and whether it also draws in Play mode (off by default). |

##### Logic

**AI Brain** *(single)*

| Property | Meaning |
| -------- | ------- |
| **AI Asset** | The `.ice_ai` behavior tree. |
| **Move Speed** | Agent movement speed in pixels/second. |
| **Detection Radius** | Range within which the AI can detect targets. |
| **Attack Radius** | Range within which it attempts to attack. |
| **Movement Mode** | `Auto` (physics when it has a dynamic Rigidbody, otherwise transform), `Transform` (move directly) or `Physics` (drive the Rigidbody velocity). |
| **Arrival Radius** | Distance at which a physics-driven agent counts as having arrived. |
| **Perception Enabled** | Turn on sight/hearing perception, then: **Sight Radius**, **Sight Angle** (FOV cone), **Hearing Radius**, **Require Line of Sight**, **Forget Time** (seconds a lost target is remembered), **Use Facing X** (aim the cone along the sprite's FlipX facing instead of its rotation) and **Awareness Radius** (detect in any direction within this radius, ignoring the cone; `0` = off). |

**Class Component** — nests another class inside this one.

| Property | Meaning |
| -------- | ------- |
| **Class (.ice_class)** | The class to embed. Only classes marked **Is Component** can be picked. |
| **Local Transform** | Where the embedded class sits relative to this entity. |
| — | The embedded class's components appear in this class's list, marked as provided by the component: you can override their values here, but add, delete and rename only in the source class. |

##### Always available

**Stencil Mask**

| Property | Meaning |
| -------- | ------- |
| **Enabled** | Enable stencil masking for this entity. |
| **Mode** | `Off`, `Write` (mark the mask) or `Read` (clip rendering to the mask). |
| **Stencil ID** | Reference value (`1`–`255`). Writers and readers with the same ID interact. |
| **Compare Function** | `Equal` or `NotEqual` — used in `Read` mode to decide which pixels pass. |

**Replication** — multiplayer synchronization.

| Property | Meaning |
| -------- | ------- |
| **Replicate** | Replicate this entity over the network. Off = purely local, exactly like singleplayer. |
| **Owner** | `Server` (the host simulates it) or `Player` (the given player owns it — client-predicted characters), with **Owner Player ID**. |
| **Transform / Velocity / Visuals** | Which parts to sync every tick: position-rotation-scale; rigidbody linear velocity; sprite, flipbook, skeleton and animator parameters. |
| **Full State** + **Full State Rate** | Periodically sync the remaining component settings (lights, tilemap, AI …) whenever they change, at this rate in Hz (`0` = the global replication rate). |
| **Scripts On Replicas** | Whether Lua callbacks run on clients for this entity when someone else owns it (`Auto` follows the global gating setting). |
| **Relevancy** | `Area Of Interest` (sent only to nearby players) or `Always Relevant` (sent to everyone — bosses, objectives). |
| **Kinematic On Clients** | Make the rigidbody kinematic on clients so local physics never fights the replicated position. |

### 4.18 Localization (`.ice_localization`)

**Editor:** Localization Creator

A **game localization** table — the translations your *game* ships (distinct from the
editor's own UI language). Structure:

```json
{
    "Languages": ["en", "ru", "de"],
    "Keys": {
        "menu_play": { "en": "Play", "ru": "Играть", "de": "Spielen" },
        "menu_quit": { "en": "Quit", "ru": "Выход",  "de": "Beenden" }
    }
}
```

At runtime the game loads this asset, picks a language, and looks up keys (with
`{}`-style format arguments and automatic RTL handling for Arabic/Hebrew). Widgets can
bind text directly to a localization key.

**The Localization Creator** manages the language list and the key/translation grid:

* Opened with no asset, it offers **Scan Content Folder** — which finds every `.ice_localization`
  in the project and lets you open one from the list — and **Create New Localization Asset**, which makes a
  fresh table in `Content/`.
* **+ Add** appends a language code (`en`, `ru`, `de`, …) as a new column;
  **+ Add Key** adds a row. Deleting works on the selected key or language.
* A **filter** box narrows the key list, which matters once a real game reaches hundreds of
  strings.
* Selecting a key shows its translation for every language side by side, so gaps are
  obvious.

> This is the **game's** localization. The editor's own interface language is a
> Preferences setting — see [Editor](Editor-EN-DOC.md).

### 4.19 Level (`.icemap`)

**Editor:** main Viewport / Level Outliner

A **level** (`.icemap`) is a scene: the entities, their components, and world settings.
It is the one asset you edit in the **main Viewport** rather than a dedicated panel —
double-clicking it loads the level. Levels reference the assets above (sprites,
tilemaps, materials, classes, widgets, etc.).

> Editing levels, entities and components is the domain of the editor and scripting
> docs; this document focuses on the assets a level consumes.

### 4.20 Script (`.lua`) & Text (`.txt`)

* **`.lua`** — a standalone Lua script (badge `LU`), creatable via **Other ▸ Create Lua
  Script**. New scripts start as a module skeleton (`local M = {} … return M`) so they can be
  pulled into class, level and widget scripts with `require`. Double-clicking one opens the
  **Lua Script Editor** — the same Lua code editor the Class Editor uses, on its own and
  nothing else: syntax highlighting, **Save** (`Ctrl+S`), **Compile** with an inline
  `OK`/`Error` badge (the failing line is marked and the message shown as a tooltip), a
  cursor read-out, and the shared Lua helpers — undo/redo, search & replace, go-to-line,
  the function navigator, snippet templates, autocomplete and optional auto-compile.
  Scripts move, rename and delete like any other asset (references inside them are fixed up
  and a redirector is left behind), they are syntax-checked by **Compile Scripts**
  (`Ctrl+R`), and they can be stepped through in the **Lua Script Debugger**. The **Lua
  language and engine API are documented separately** in
  [LuaAPI-EN-DOC.md](LuaAPI-EN-DOC.md).
* **`.txt`** — a plain text note (badge `NT`), creatable via **Other ▸ Text Note**.
  Double-clicking one opens the **Text Note editor**: a syntax-neutral code editor with a
  *File ▸ Save* (`Ctrl+S`) and an *Edit* menu (undo/redo, copy/cut/paste, select all).
  Useful for design notes, TODO lists and small data files.

---

### 4.21 Decal (`.ice_decal`)

**Editor:** Decal Editor

A **decal** is a mark the game leaves on a surface at runtime: a bullet hole, a blood
splatter, a scorch mark, a footprint, a crack. One `.ice_decal` asset describes how that
mark looks and behaves, and the engine's decal system takes care of everything else —
random variation, lifetime, fade in / fade out, sorting, culling, pooling and budget.

Create one from **Materials ▸ Create Decal**, or right-click a texture and pick
**Create Decal** to get an asset already pointing at it. Double-clicking opens the
**Decal Editor**: a live preview on the left, the settings on the right.

**Texture Variants** — the list of images the decal can use. Each row has a texture, a
**weight** (relative chance of being picked) and an optional **source rect** for pulling a
sub-image out of an atlas. A random variant is chosen on every spawn, which is what keeps
ten bullet holes on the same wall from looking identical. Leave a single variant for a
mark that always looks the same.

**Material** — an optional material whose **Domain** is **Decal** (or a material instance
of one). When set, it replaces the plain textured draw and the decal texture becomes the
material's entity texture, so the whole node graph works: texture samples, scalar / vector
/ texture parameters, material functions and parameter collections. The **Decal Data** node
additionally exposes the live state of the decal — fade, age, normalized age, lifetime and a
stable per-decal random value.

**Appearance** — size (zero takes it from the texture), size variance and whether that
variance keeps the aspect ratio, pivot, tint, tint variance, shading mode (Lit / Unlit),
blend mode and the alpha clip threshold used by Masked blending.

**Rotation** — Fixed, Random (between a min and a max), Align To Normal, or Align To Normal
with a random 180° flip. Random horizontal and vertical mirroring can be enabled on top.

**Lifetime** — how long a decal lives (zero = forever), how long it takes to fade in and
fade out, and **Max Instances**: a per-asset cap that starts fading the oldest decal of this
kind once it is exceeded.

**Placement** — the Z offset that lifts the decal in front of its surface, the sort order
among decals, an offset along the hit normal, **Follow Receiver** (attach to the entity that
was hit so the decal moves with it) and **Clip To Receiver** (cut the decal to the bounds of
the surface it was placed on, so blood never hangs off the edge of a character).

Decals are spawned from script with the `Decal.*` API, and can also be **placed by hand**
with the **Decal** component in the Class Editor. See
[LuaAPI-EN-DOC.md](LuaAPI-EN-DOC.md) section 62 for the scripting side.

---

## 5. Importers

Beyond plain copy-with-sidecar, two specialized importers turn external formats into
native assets automatically.

### 5.1 Aseprite importer

Drop an **Aseprite JSON export** (the `.json` produced by Aseprite's *Export Sprite
Sheet* with JSON data) onto the browser, or use **Import Aseprite Spritesheet** on an Aseprite `.json`
already in the project. The importer:

* Detects whether the JSON is an Aseprite file and **finds the matching PNG**
  automatically (from the JSON's image reference); if the PNG is missing it says so in the
  Console instead of producing half an import.
* **Copies the sheet in** as `T_<name>.png` with a default `.ice_texture` sidecar.
* Slices the sheet into **sprites** named `SP_<name>_0`, `SP_<name>_1`, … using the frame
  rectangles, with a centered pivot.
* Reconstructs **flipbooks** from Aseprite **frame tags**: one `FB_<name>_<tag>` per tag
  (or a single `FB_<name>` when the file has no tags), with per-frame durations taken from
  the JSON and the default FPS derived from the real total duration. A tag's
  **direction** is honored — `reverse` flips the frame order and `pingpong` appends the
  return leg.

This is the fastest path from Aseprite to in-engine animated sprites: draw, export with
JSON data, drop the JSON into the browser, done.

### 5.2 GIF importer

Importing a **`.gif`** decodes every frame and produces a complete, ready-to-use set:

* All frames are packed into a **single spritesheet texture** (a near-square grid) with a
  default `.ice_texture` sidecar.
* One **sprite** per frame, pointing at its cell in that sheet.
* One **flipbook** whose per-frame durations come from the GIF's own frame delays.

Great for quickly bringing in animated pixel art or reference loops.

---

## 6. Asset cooking — overview

During development you work with **loose, uncompressed** assets for fast iteration. For
a shipping build, the optional **cooker** converts raw media into compressed,
runtime-friendly forms — typically shrinking build size dramatically while the runtime
loaders stay unchanged.

What cooking can do (summary):

| Media | Cooked forms |
| ----- | ------------ |
| **Textures** | WebP (lossy/lossless) or KTX2 (GPU-compressed: UASTC high-quality / ETC1S small). Driven by each texture's sidecar (sRGB, max size, compression…). Lossy results are verified and downgraded to lossless when they would visibly alter the art. |
| **Audio** | Ogg Vorbis or Opus at a chosen bitrate. |
| **Video** | VP9 at a chosen quality (CRF), with optional resolution/FPS caps and audio handling. |
| **Fonts** | Subset to declared glyph ranges (or auto-subset to used glyphs). |
| **JSON / sidecars** | Minify. |

The sidecars you author in this document are the **inputs** to cooking. The full cook
settings, per-platform restrictions, the IcePak archive format, packing/splitting, build
manifests and installers are documented in
[Profiling & Building Games](Profiling-And-Building-EN-DOC.md).

---

## 7. Quick reference tables

### 7.1 Extensions, types & editors

| Extension | Type | Badge | Editor |
| --------- | ---- | ----- | ------ |
| `.icemap` | Level | `LV` | Viewport |
| `.ice_class` | Class | `CL` | Class Editor |
| `.ice_view` | View / Post-Process Volume | `VW` | View Editor |
| `.ice_cinema` | Cinema | `CN` | Cinema Editor |
| `.ice_ai` | AI / Behavior Tree | `AI` | AI Editor |
| `.ice_sprite` | Sprite | `SP` | Sprite Editor |
| `.ice_flipbook` | Flipbook | `FB` | Flipbook Editor |
| `.ice_animation` | Animation / State Machine | `AN` | Animation Editor |
| `.ice_skeleton` | Skeleton | `SK` | Skeleton Editor |
| `.ice_ts` | Tileset | `TS` | Tileset Editor |
| `.ice_tm` | Tilemap | `TM` | Tilemap Editor |
| `.ice_material` | Material | `MA` | Material Editor |
| `.ice_matinst` | Material Instance | `MI` | Material Instance Editor |
| `.ice_matfunc` | Material Function | `MF` | Material Function Editor |
| `.ice_mpc` | Material Parameter Collection | `MC` | MPC Editor |
| `.ice_decal` | Decal | `DC` | Decal Editor |
| `.ice_fx` | FX / Particle System | `FX` | FX Editor |
| `.ice_widget` | Widget / UI | `WI` | Widget Editor |
| `.ice_localization` | Localization | `LC` | Localization Creator |
| `.png` `.jpg` `.jpeg` | Texture | `TX` | Texture Settings |
| `.wav` `.mp3` `.ogg` `.flac` | Audio | `AU` | Sound Settings |
| `.mp4` `.avi` `.mkv` `.mov` `.webm` | Video | `VD` | Video Player |
| `.ttf` `.otf` | Font | `FT` | Font Editor |
| `.lua` | Script | `LU` | Lua Script Editor |
| `.txt` | Text note | `NT` | Text Note editor |

Assets registered by **plugins** appear here too, with the badge, color and name the
plugin declares; double-clicking one calls the plugin's own open handler.

### 7.2 Source file → sidecar map

| Source | Sidecar (hidden) | Settings type |
| ------ | ---------------- | ------------- |
| `.png` / `.jpg` / `.jpeg` | `.ice_texture` | Texture import settings |
| `.wav` / `.mp3` / `.ogg` / `.flac` | `.ice_sound` | Sound definition |
| `.ttf` / `.otf` | `.ice_font` | Font settings |
| `.mp4` / `.avi` / `.mkv` / `.mov` / `.webm` | `.ice_video` | Video metadata |
| (moved/renamed asset) | `.ice_redirect` in `Saved/Redirectors/` | Path redirector |

### 7.3 Auto-prefixes on import

| Media | Prefix | Example |
| ----- | ------ | ------- |
| Texture | `T_` | `hero.png` → `T_hero.png` |
| Audio | `S_` | `jump.wav` → `S_jump.wav` |
| Font | `F_` | `title.ttf` → `F_title.ttf` |
| Video | `V_` | `intro.mp4` → `V_intro.mp4` |

A file that already starts with the right prefix is left alone.

### 7.4 Default names for new assets

Assets created from the **empty-space menu** ([3.7](#37-creating-assets)) get these names,
with a number appended if the name is taken (`SP_NewSprite1`, `SP_NewSprite2`, …):

| Asset | Default name | Asset | Default name |
| ----- | ------------ | ----- | ------------ |
| Folder | `NewFolder` | Material | `M_NewMaterial` |
| Class | `CL_NewActor` | Material Instance | `MI_NewInstance` |
| Cinema | `CN_NewCinema` | Material Function | `MF_NewFunction` |
| AI | `AI_NewBrain` | Material Collection | `MPC_NewCollection` |
| Sprite | `SP_NewSprite` | Widget | `WI_NewWidget` |
| Tileset | `TS_NewTileset` | Localization | `LC_GameLocalization` |
| Tilemap | `TM_NewTilemap` | Text Note | `NT_NewNote` |
| Flipbook | `FB_NewFlipbook` | View | `VW_NewView` |
| Animation | `AN_NewAnimation` | Skeleton | `SK_NewSkeleton` |
| FX | `FX_NewFX` | Lua Script | `LU_NewScript` |
| Decal | `DC_NewDecal` | | |

Assets created **from another asset** ([3.8](#38-the-item-context-menu)) derive their name
from the source, dropping the source's prefix first:

| Action | Result |
| ------ | ------ |
| **Create Sprite (.ice_sprite)** on `T_Hero.png` | `SP_T_Hero.ice_sprite` |
| **Create Tileset (.ice_ts)** on `T_Cave.png` | `TS_T_Cave.ice_ts` |
| **Create Tilemap (.ice_tm)** on `TS_Cave.ice_ts` | `TM_Cave.ice_tm` |
| **Create Material Instance (.ice_matinst)** on `M_Char.ice_material` | `MI_Char.ice_matinst` |
| **Create Decal (.ice_decal)** on `T_Hole.png` | `DC_T_Hole.ice_decal` |
| **Create Child Class** on `CL_Enemy.ice_class` | `CL_Enemy_Child.ice_class` |
| **Create Child Widget** on `WG_Menu.ice_widget` | `WG_Menu_Child.ice_widget` (child widgets use the `WG_` prefix, and only an existing `WG_` is stripped) |
| **Create Flipbook from Sprite(s)** (first is `SP_Run_0`) | `FB_Run_0.ice_flipbook` |
| **Spritesheet Slicer** with base name `Hero` | `SP_Hero_0…`, plus `FB_Hero.ice_flipbook` |

---

## 8. FAQ & troubleshooting

**Why don't I see `.ice_texture` / `.ice_sound` files in the browser?**
They're **sidecars** and are hidden by design. Select the source file (e.g. the `.png`)
and open its settings panel. The sidecar moves/renames/deletes with the source
automatically.

**I moved an asset and a reference still works — how?**
Either the reference was rewritten to the new path, or a **redirector** now resolves the
old path. Use **Asset References** to inspect dependencies, and **Fix Up
Redirectors** periodically to tidy up.

**Where are redirectors stored?**
In **`Saved/Redirectors/`**, outside `Content/` — they are project metadata, not content.
That folder is safe to delete if you are certain no asset still points at an old path;
otherwise use **Fix Up Redirectors**, which rewrites the stragglers first. Redirectors
whose target no longer exists are pruned automatically.

**Why does the search show files from other folders?**
Because search is **recursive** — from the root it covers the whole project, from a
sub-folder it covers that subtree. Results outside the current folder are shown with their
relative path. The same applies to the type filters. Clear the search box (the **X**) or
the filters (the **All** button) to get the plain folder listing back.

**I deleted a class and other classes disappeared too.**
Classes that **inherit** from the deleted class are deleted with it — a child class cannot
exist without its parent. This happens after you confirm, so the dialog does not list them;
`Ctrl+Z` (with the browser focused) restores the whole batch.

**How do I copy assets into another project?**
Select them, right-click, **Migrate To...** ([3.16](#316-migrating-assets-to-another-project)).
It brings the dependencies along and preserves the folder layout, which plain file copying
does not.

**Can I have two assets open at once?**
Yes — each asset gets its own editor panel, and you can dock or tab them freely. Opening an
asset that is already open focuses the existing panel instead of duplicating it.

**Can I edit asset JSON by hand?**
Yes — engine assets are plain JSON — but the editors are safer (they keep IDs,
references and caches consistent). Hand-editing is mainly useful for understanding diffs.

**What's the difference between a Material, a Material Instance, and an MPC?**
A **Material** is the shader node graph. A **Material Instance** overrides a material's
exposed parameters cheaply. An **MPC** is a *global* parameter set that many materials
read at once.

**What's the difference between a Flipbook and an Animation asset?**
A **Flipbook** is a fixed frame sequence. An **Animation** asset is a *state machine*
that chooses which flipbook to play based on parameters and transitions.

**Class vs Level?**
A **Class** is a reusable game object (instantiate many in many levels). A **Level**
(`.icemap`) is a concrete scene that contains instances.

**How do I make my game smaller?**
Enable **Cook Assets** (and optionally **Pack Content**) at build time — see
[Profiling & Building Games](Profiling-And-Building-EN-DOC.md). Author texture max-size
and font glyph ranges in the sidecars to give the cooker good inputs.

---

<sub>© IceBoxCrew Studio. All rights reserved. See [`LICENSE.txt`](../../LICENSE.txt) for full terms.</sub>
