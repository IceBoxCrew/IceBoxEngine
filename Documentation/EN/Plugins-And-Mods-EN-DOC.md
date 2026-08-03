# 🧩 IceBox Engine — Plugins & Mods

## Full documentation in English

> **IceBox Engine** is extensible in two complementary ways:
>
> * **Plugins** — native **C++** modules that extend the **editor** and/or the
>   **runtime** through a stable C ABI. Use them to add panels, tools, menu items,
>   custom asset types, toolbar buttons, viewport overlays, settings pages, Visual
>   Script nodes, and integrations with external services.
> * **Mods** — **Lua + content** bundles loaded at runtime. Use them to add or change
>   gameplay and ship new assets (sprites, classes, widgets, …) without
>   touching the base game or recompiling anything.
>
> This document explains how both systems work, how to enable and manage them, and how
> to **author your own** plugins and mods from scratch.
>
> The Lua and Python languages/APIs themselves are documented separately in
> [LuaAPI-EN-DOC.md](LuaAPI-EN-DOC.md) and [PythonAPI-EN-DOC.md](PythonAPI-EN-DOC.md).
> For the asset types a mod ships, see [Assets & Content Browser](Assets-EN-DOC.md);
> for how plugins/mods are bundled into a shipping build, see
> [Profiling & Building Games](Profiling-And-Building-EN-DOC.md).

---

## 📑 Table of Contents

1. [Introduction](#1-introduction)
2. [Plugins vs Mods — which should I use?](#2-plugins-vs-mods--which-should-i-use)

**Part I — Plugins (native C++)**

3. [The plugin system](#3-the-plugin-system)
   - 3.1 [What a plugin can do](#31-what-a-plugin-can-do)
   - 3.2 [Anatomy of a plugin](#32-anatomy-of-a-plugin)
   - 3.3 [The `plugin.json` manifest](#33-the-pluginjson-manifest)
   - 3.4 [The `IPlugin` interface & lifecycle](#34-the-iplugin-interface--lifecycle)
   - 3.5 [Host APIs — talking back to the engine](#35-host-apis--talking-back-to-the-engine)
   - 3.6 [Editor extension points](#36-editor-extension-points)
   - 3.7 [Plugin contexts & ImGui](#37-plugin-contexts--imgui)
   - 3.8 [Entry points: dynamic vs static](#38-entry-points-dynamic-vs-static)
   - 3.9 [API / ABI versioning](#39-api--abi-versioning)
   - 3.10 [Building a plugin (CMake)](#310-building-a-plugin-cmake)
   - 3.11 [Worked example: a minimal editor plugin](#311-worked-example-a-minimal-editor-plugin)
   - 3.12 [Shipping data & Visual Script nodes with a plugin](#312-shipping-data--visual-script-nodes-with-a-plugin)

**Part II — Mods (Lua + content)**

4. [The mod system](#4-the-mod-system)
   - 4.1 [What a mod can do](#41-what-a-mod-can-do)
   - 4.2 [Anatomy of a mod](#42-anatomy-of-a-mod)
   - 4.3 [The `mod.json` manifest](#43-the-modjson-manifest)
   - 4.4 [The mod environment & injected globals](#44-the-mod-environment--injected-globals)
   - 4.5 [Lifecycle hooks](#45-lifecycle-hooks)
   - 4.6 [Shipping content with a mod](#46-shipping-content-with-a-mod)
   - 4.7 [Load order & dependencies](#47-load-order--dependencies)
   - 4.8 [Worked example: a minimal mod](#48-worked-example-a-minimal-mod)

**Part III — Managing & shipping**

5. [Managing plugins & mods](#5-managing-plugins--mods)
   - 5.1 [The Plugins & Mods panel](#51-the-plugins--mods-panel)
   - 5.2 [Configuration files](#52-configuration-files)
   - 5.3 [Managing mods from Lua](#53-managing-mods-from-lua)
   - 5.4 [The launcher's Plugins & Mods tab](#54-the-launchers-plugins--mods-tab)
   - 5.5 [Player-installed plugins & mods](#55-player-installed-plugins--mods)
6. [How plugins & mods are loaded](#6-how-plugins--mods-are-loaded)
7. [Shipping plugins & mods in a build](#7-shipping-plugins--mods-in-a-build)

**Reference**

8. [Quick reference](#8-quick-reference)
   - 8.1 [Plugins vs Mods](#81-plugins-vs-mods)
   - 8.2 [IPlugin hooks](#82-iplugin-hooks)
   - 8.3 [Mod hooks & globals](#83-mod-hooks--globals)
   - 8.4 [Files & folders](#84-files--folders)
9. [FAQ & troubleshooting](#9-faq--troubleshooting)

---

## 1. Introduction

Both extension systems are managed by a single engine subsystem (the **Plugin
Manager**) and surfaced through a single editor panel (**Plugins & Mods**), but they
are very different tools:

* A **plugin** is compiled native code. It links against the engine's **Plugin SDK**,
  implements the `IPlugin` interface, and is loaded as a shared library
  (`.dll` / `.so` / `.dylib`) — or linked statically on platforms without dynamic
  loading. Plugins can reach deep into the editor and runtime through host APIs.
* A **mod** is a folder of Lua and content. Its entry script runs inside a sandboxed
  Lua environment with engine globals available; it can spawn the classes, widgets and
  assets it ships and hook into the level lifecycle. No compiler required.

Both live in their own top-level folders — `Plugins/` and `Mods/` — and both are
enabled per-project through small JSON config files.

The engine ships one reference example of each:

* **`Plugins/AIHelper`** — a native editor plugin that adds *Tools → AI Helper*, a chat
  panel wired to a local Ollama LLM. It indexes every language folder under
  `Documentation/`, the scripting API catalog from `Config/VisualScriptAPI.json`, the
  project README files and optionally your own Lua/Python sources, then retrieves the
  most relevant sections for each question. Role, answer style, sampling, retrieval and
  interface are all configurable in the panel and stored in `Config/AIHelper.json`;
  saved conversations live under `Saved/AIHelper/`. See
  [`Plugins/AIHelper/README.md`](../../Plugins/AIHelper/README.md).
* **`Mods/PlatformerCrates`** — a Lua mod that adds collectible crates and a HUD counter
  to the Platformer example, shipping its own classes, widget and sprite. See
  [`Mods/PlatformerCrates/README.md`](../../Mods/PlatformerCrates/README.md).

---

## 2. Plugins vs Mods — which should I use?

| | **Plugins** | **Mods** |
| --- | ----------- | -------- |
| **Language** | C++ (compiled) | Lua + content (interpreted) |
| **Primarily extends** | Editor **and** runtime (deeply, natively) | Runtime gameplay + ships assets |
| **Manifest** | `plugin.json` | `mod.json` |
| **Entry point** | `CreatePlugin()` → an `IPlugin` | `main.lua` (entry script) |
| **Needs building?** | Yes — CMake, ships as a library (or static) | No — ships as-is |
| **Folder** | `Plugins/<Name>/` | `Mods/<Name>/` |
| **Config** | `Config/Plugins.json` | `Config/Mods.json` |
| **Runs in the editor?** | Yes — that is the point of an editor plugin | No — mods run only in Play mode / the game |
| **Toggle without restarting?** | Yes, once the library is built (see [5.1](#51-the-plugins--mods-panel)) | Yes — enabled/disabled and refreshed live |
| **Sandboxed?** | No (native code, full power) | Yes (own Lua environment) |
| **Versioning** | C ABI version (`ICE_PLUGIN_API_VERSION`) | mod `APIVersion` |
| **Typical use** | New panels/tools, custom asset types, menu/toolbar items, viewport overlays, Visual Script nodes, external integrations | New levels/mechanics/content, total conversions, gameplay tweaks |

**Rule of thumb:** if you need new **editor tooling** or native performance/integration,
write a **plugin**. If you want to add or change **game content and behavior** with
scripting, write a **mod**.

---

# Part I — Plugins (native C++)

## 3. The plugin system

A plugin is a native module that implements `IceBox::IPlugin` (declared in
`PluginInterface.h`) and is built against the **`IceBoxPluginSDK`** CMake target. The
engine discovers it in `Plugins/<Name>/`, loads it when enabled, and calls into it
through a set of lifecycle hooks. In return, the plugin talks back to the engine through
a versioned **host API**.

### 3.1 What a plugin can do

Through the editor host API a plugin can, among other things:

* **Register new editor UI** — dockable panels, **Tools** menu items, toolbar buttons,
  viewport overlays, viewport/entity context-menu items, and settings pages.
* **Register custom asset types** — a new `.ext` with its own icon, color, display name
  and *open/create* handlers that integrate into the Content Browser.
* **Drive the scene** — create/duplicate/delete entities, query and set transforms, add
  or remove components, instantiate classes, import/export entity JSON.
* **Control the editor** — selection, camera/viewport, undo/redo, play/stop, save/load
  scenes, open assets, show/hide panels, log and notify.
* **Bind scripting** — register functions into every Lua state and/or the editor's
  Python module.
* **React to events** — receive editor/engine events, and run logic every runtime frame
  and on play start/stop.
* **Ship data** — bundle its own content folders and a `VisualScriptAPI.json` that adds
  nodes to the Visual Script graph editor (see [3.12](#312-shipping-data--visual-script-nodes-with-a-plugin)).

### 3.2 Anatomy of a plugin

A plugin is a folder under `Plugins/`:

```
Plugins/MyPlugin/
    plugin.json           # manifest (name, version, icon, dependencies …)
    CMakeLists.txt        # builds the plugin against IceBoxPluginSDK
    Source/
        MyPlugin.cpp      # your IPlugin subclass + ICE_PLUGIN_ENTRY(...)
        ...               # any other source/headers
    Icon.png              # optional icon shown in the Plugins panel
    README.md             # optional; indexed by the AI Helper plugin
    VisualScriptAPI.json  # optional; extra Visual Script nodes
    Content/  Assets/  Scripts/  Lua/    # optional data folders
```

When built, the compiled library and a copy of `plugin.json` end up in the engine's
plugin output directory (`bin/<platform>/Plugins/MyPlugin/`), which is where the engine
loads it from. On Android the library instead goes to `lib/Android/<ABI>/`; for static
builds (Web, iOS) it is linked directly into the runtime executable.

### 3.3 The `plugin.json` manifest

A small JSON file describing the plugin:

```json
{
    "Name": "MyPlugin",
    "Description": "Adds a Hello panel to the editor.",
    "Author": "You",
    "Version": "1.0.0",
    "Icon": "Icon.png",
    "Dependencies": []
}
```

| Field | Meaning | Default |
| ----- | ------- | ------- |
| `Name` | Unique plugin name (must match what you enable in `Plugins.json`). | folder name |
| `Description` | Shown in the Plugins panel. | `""` |
| `Author` | Shown in the Plugins panel. | `""` |
| `Version` | Display version string. | `"1.0.0"` |
| `Icon` | Path (relative to the plugin folder) to a `.png` / `.jpg` / `.jpeg` icon. | auto-detected |
| `Dependencies` | Names of other plugins that must load first. | `[]` |

> **Icon resolution.** If `Icon` is missing, empty, points at a non-image or a
> non-existent file, the engine falls back to auto-detection and looks for
> `icon.png`, `icon.jpg`, `icon.jpeg`, `Icon.png`, `Icon.jpg`, `Icon.jpeg` in the plugin
> folder, in that order. With no icon at all the panel draws a gray square with a `?`.

> The authoritative `Name`, `Version` and **API version** also come from the plugin's
> `GetPluginInfo()` at load time; the manifest is what the editor shows before loading.

### 3.4 The `IPlugin` interface & lifecycle

Your plugin subclasses `IceBox::IPlugin` and overrides the hooks it needs. Only the
first three are required. The **Min API** column is the `PluginInfo::APIVersion` your
plugin must report for the engine to dispatch that hook at all (see
[3.9](#39-api--abi-versioning)).

| Hook | Min API | When it's called |
| ---- | :-----: | ---------------- |
| `PluginInfo GetPluginInfo() const` | — | Identify the plugin (name, description, author, version, **APIVersion**). **Required.** |
| `bool OnLoad()` | — | The plugin was loaded. Return `false` to abort loading. **Required.** |
| `void OnUnload()` | — | The plugin is being unloaded. **Required.** |
| `void OnUpdate(float dt)` | — | Every **runtime** frame — i.e. while the game or Play mode is running. It does **not** tick while the editor sits idle; use `OnEditorUI` for per-frame editor work. |
| `void OnRuntimeStart()` / `OnRuntimeStop()` | — | Play mode / the game started or stopped. |
| `void OnEditorInit(const EditorPluginContext&)` | — | The **editor** host is ready (gives you the ImGui context). |
| `void OnEditorShutdown()` | — | The editor is shutting the plugin's editor side down (also called just before unload). |
| `void OnEditorToolsMenu()` | — | Add items to the editor's **Tools** menu (draw `ImGui::MenuItem`s). |
| `void OnEditorUI(float dt)` | — | Draw your own ImGui every editor frame (panels, windows). |
| `void OnRegisterLua(void* L)` / `OnUnregisterLua(void* L)` | 3 | A Lua state was created/destroyed — bind/unbind your Lua functions. |
| `void OnEngineInit(const PluginRuntimeContext&)` | 4 | Right after `OnLoad`, handing you the `RuntimeHostAPI`. Dispatched in **both** the editor and the standalone runtime — it is not a game-process-only hook. |
| `void OnRegisterPython(void* module)` | 4 | Register bindings into the editor's Python module. |
| `void OnEditorEvent(const char* name, const char* payload)` | 4 | An editor event was dispatched. |
| `void OnEngineEvent(const char* name, const char* payload)` | 4 | A runtime/engine event was dispatched. |

A plugin can be **editor-only** (implement only the `OnEditor*` hooks), **runtime-only**
(implement `OnEngineInit`/`OnUpdate`/`OnRuntime*`), or both.

**Which Lua states you get.** `OnRegisterLua` is called for every Lua state the engine
creates, and for every state that already exists when your plugin loads: the main
gameplay/script state and the separate **widget runtime** state used by `.ice_widget`
UIs. Register your functions into each `L` you are handed, and undo that in
`OnUnregisterLua`.

**Which Python module you get.** `OnRegisterPython` receives a `pybind11::module_*`
pointing at the editor's `__main__` module — the same module that carries the built-in
`editor`, `scene`, `engine` and `browser` bindings.

**Which events you get.** `OnEditorEvent` fires for every event raised through the
editor's Python `FireEvent(...)`; the payload is currently always `nullptr`.
`OnEngineEvent` fires with `"runtime_started"` and `"runtime_stopped"` around play
mode. Note that `RuntimeHostAPI::FireEvent` emits a **Lua** event to script listeners —
it does not re-enter `OnEngineEvent`.

> **Exceptions are contained.** Every hook the manager dispatches is wrapped in a
> `try/catch`; a throwing plugin is logged (`Plugin '<name>' <hook> threw: …`) and the
> other plugins keep running. It is still your job not to throw across the ABI boundary.

### 3.5 Host APIs — talking back to the engine

The contexts handed to your plugin (Section [3.7](#37-plugin-contexts--imgui)) carry a
pointer to a host API — a plain C struct of function pointers — plus an opaque
`HostContext` handle you pass as the first argument to every call. Both structs start
with `StructSize` and `AbiVersion` so you can sanity-check what you were given.

**`EditorHostAPI`** (editor) exposes, among many others:

* **Logging / UX:** `Log`, `Notify`. Both take a level: `0` = trace, `1` = info,
  `2` = warning, `3` = error, and land in the editor **Console**.
* **Selection:** `EnumSelectedEntities`, `GetPrimarySelected`, `SetSelection`,
  `SelectEntity`, `ClearSelection`, `SelectAll`.
* **Entities:** `EnumAllEntities`, `GetEntityCount`, `CreateEntity`, `InstantiateClass`,
  `DuplicateEntity`, `DeleteEntity`, `EntityExists`, `FindEntityByName`,
  `Get/SetEntityName`, `Export/ImportEntityJson`.
* **Transforms:** `Get/SetPosition`, `Get/SetScale`, `Get/SetRotation`, `Translate`.
* **Components:** `HasComponent`, `AddComponent`, `RemoveComponent`, `EnumComponentTypes`.
* **Scene:** `GetScenePath`, `SaveScene`, `LoadScene`, `NewScene`, `MarkSceneDirty`,
  `IsSceneDirty`.
* **Assets / browser:** `OpenAsset`, `RefreshContentBrowser`, `GetContentBrowserPath`.
* **Viewport / camera:** `GetViewportSize`, `ScreenToWorld`, `IsViewportHovered`,
  `IsViewportFocused`, `Get/SetCamera`, `FocusEntity`.
* **Editor control:** `RecordUndo`, `Undo`, `Redo`, `IsPlayMode`, `Play`, `Stop`,
  `Set/IsPanelVisible`.
* **Registration:** see [Section 3.6](#36-editor-extension-points).

Functions that return text (`GetEntityName`, `GetScenePath`, `ExportEntityJson`,
`GetContentBrowserPath`, …) follow the usual C convention: they write at most
`bufSize - 1` bytes plus a terminator and return the **required** length, so you can
call them once with a small buffer to size the allocation.

`Set/IsPanelVisible` take one of the editor's panel names:
`Hierarchy`, `Properties`, `Stats`, `ContentBrowser`, `Settings`, `NetworkPanel`,
`Profiler`, `WorldSettings`, `HotKeys`, `Documentation`, `LuaDebugger`, `Plugins`,
`Console`, `PythonConsole`, `About`, `PropertyMatrix`, `LevelScriptEditor`,
`TextNoteEditor`, `RemotePreview`, `BuildGame`, `DLCPackager`. Asset editor panels
(`ClassEditor`, `SpriteEditor`, `MaterialEditor`, …) can only be **closed** this way —
open them with `OpenAsset`.

**`RuntimeHostAPI`** (runtime) exposes: `Log`, `GetEngineRoot`, `IsPlaying`, `GetTime`,
`HasScene`, `EnumEntities`, `GetEntityName`, `GetConfigString` / `SetConfigString`,
`FireEvent`, and `GetLuaState`. `GetConfigString`/`SetConfigString` are a simple
in-memory key/value store shared by all runtime plugins; `FireEvent` emits a Lua event
that `On("name", handler)` listeners receive.

### 3.6 Editor extension points

These `EditorHostAPI` functions let a plugin add first-class editor features. Call them
from `OnEditorInit` (or later) — they are only available in editor builds.

| Function | Adds | Where it shows up |
| -------- | ---- | ----------------- |
| `RegisterPanel(id, title, render, user)` | A dockable panel; returns a panel id you can use with `ShowPanel`/`IsPanelOpen` (`0` means registration failed). | A checkable entry at the bottom of the **Window** menu; the window itself is `title`, 480×360 on first use, and your `render(bool* pOpen, user)` draws inside it. |
| `RegisterAssetType(const IceAssetTypeDesc*)` | A custom asset type — extension, display name, icon text + color, and `OnOpen`/`OnCreate` callbacks. | The Content Browser: badge text, badge color and type name for that extension, an entry at the top of the empty-space **create** context menu (only if `OnCreate` is set), and double-click handling through `OnOpen`. |
| `RegisterMenuItem(menuPath, shortcut, cb, user)` | A menu item. | The **Tools** menu, below the built-ins. A `menuPath` without `/` becomes a flat item; `Group/Item` puts `Item` inside a `Group` sub-menu. |
| `RegisterToolbarButton(id, tooltip, cb, user)` | A toolbar button. | The main toolbar, after the built-in buttons. `id` is the visible label; `tooltip` shows on hover. |
| `RegisterViewportOverlay(cb, user)` | A draw callback. | Rendered last inside the viewport window each frame, so `ImGui::GetWindowDrawList()` draws over the scene. |
| `RegisterContextMenuItem(context, label, cb, user)` | A context-menu item. | The viewport right-click menu. `context` must be `"viewport"` (always shown) or `"entity"` (shown only when at least one entity is selected). |
| `RegisterSettingsPage(category, cb, user)` | A settings page. | The **Settings** tab of the Plugins & Mods panel, as a collapsible header named `category`. The tab itself only appears once at least one page is registered. |

Notes:

* **`IceAssetTypeDesc`** carries `StructSize`, `Extension` (lower-cased for you),
  `DisplayName`, `IconText`, `IconColor[4]`, `OnOpen`, `OnCreate` and `User`.
  `OnCreate` receives the content browser's **current folder**; `OnOpen` receives the
  **asset path**. Built-in engine extensions always win, so you cannot re-skin
  `.ice_class` & co.; if two plugins claim the same extension the **last** registration
  wins.
* **Unloading cleans up.** When a plugin is unloaded (or disabled), every panel, asset
  type, menu item, toolbar button, overlay, context item and settings page it registered
  is removed automatically — you do not have to unregister them yourself.
* `RegisterAssetType` is how plugins introduce brand-new asset kinds; the Content
  Browser queries registered types for their icon, color and create-menu entry — see
  [Assets & Content Browser](Assets-EN-DOC.md).

### 3.7 Plugin contexts & ImGui

On initialization the engine hands your plugin a context struct:

* **`EditorPluginContext`** (to `OnEditorInit`) — contains the editor's `ImGuiContext*`
  and ImGui allocator functions, the engine root path, this plugin's folder path, a
  `Log` function, and the `EditorHostAPI* Host` + `HostContext`.
* **`PluginRuntimeContext`** (to `OnEngineInit`) — engine root path, plugin folder path,
  `Log`, and the `RuntimeHostAPI* Host` + `HostContext`.

`PluginFolderPath` always uses forward slashes and is the right base for loading your
own data files; `EngineRootPath` is the engine's working directory.

**Drawing ImGui across the DLL boundary.** Because a plugin is a separate binary with its
own ImGui, you must adopt the host's ImGui context and allocator before drawing — do this
once in `OnEditorInit`:

```cpp
void OnEditorInit(const IceBox::EditorPluginContext& ctx) override {
    m_Ctx = ctx;
    if (ctx.ImGuiCtx) ImGui::SetCurrentContext(ctx.ImGuiCtx);
    if (ctx.ImGuiAllocFn && ctx.ImGuiFreeFn)
        ImGui::SetAllocatorFunctions(ctx.ImGuiAllocFn, ctx.ImGuiFreeFn, ctx.ImGuiUserData);
}
```

After that, your `OnEditorToolsMenu` and `OnEditorUI` can call ImGui normally and your
widgets render inside the editor.

### 3.8 Entry points: dynamic vs static

A plugin exposes a factory pair, `CreatePlugin()` / `DestroyPlugin()`. The
`ICE_PLUGIN_ENTRY(MyPluginClass)` macro generates them for you:

```cpp
ICE_PLUGIN_ENTRY(MyPlugin)   // at file scope, after the class definition
```

* **Dynamic (default)** — on desktop, this exports `CreatePlugin`/`DestroyPlugin` from the
  shared library; the engine `dlopen`/`LoadLibrary`s it and resolves the symbols. The
  loader takes the **first** `.dll` (Windows) or `.so`/`.dylib` (Unix) it finds in the
  plugin folder, so keep one library per folder.
* **Static** — on platforms without dynamic loading (**Web**, **iOS**), or when you opt
  in, the engine builds plugins with `ICE_PLUGIN_STATIC_BUILD`. The same macro then
  **registers the plugin in a static registry** at startup, and the engine links it
  directly into the runtime with whole-archive linkage. (You can also register
  explicitly with `ICE_PLUGIN_REGISTER_STATIC(Name, Class)`.)
* **Android** is a hybrid: Gradle packs plugin `.so` files into the APK's
  `nativeLibraryDir`, so when no library is found next to `plugin.json` the loader
  retries `dlopen("lib<Name>.so")` using the sanitized plugin name, then falls back to
  the static registry.

Statically registered plugins that have no folder on disk are still discovered — the
manager synthesizes an entry named after the registration so you can enable it in
`Plugins.json` like any other plugin.

Your plugin code is identical in all modes — the macro handles the difference, driven by
the CMake variable `ICE_PLUGIN_BUILD_STATIC`.

### 3.9 API / ABI versioning

The plugin ABI is versioned by `IceBox::ICE_PLUGIN_API_VERSION` (currently **4**). Your
plugin reports the version it was built against in `PluginInfo::APIVersion` (set it to
`ICE_PLUGIN_API_VERSION`).

* If a plugin's `APIVersion` is **greater** than the engine's, the engine **refuses to
  load it** (it was built for a newer ABI) and logs
  `Plugin '<name>' requires API version N (engine has 4)`.
* Newer hooks/features are gated by version internally, so older plugins keep working as
  the ABI grows:

| Version | What it unlocked |
| :-----: | ---------------- |
| ≤ 2 | The base lifecycle: `GetPluginInfo`, `OnLoad`, `OnUnload`, `OnUpdate`, `OnRuntimeStart/Stop`, and all `OnEditor*` hooks. |
| 3 | `OnRegisterLua` / `OnUnregisterLua`. |
| 4 | `OnEngineInit` (and therefore the `RuntimeHostAPI`), `OnRegisterPython`, `OnEditorEvent`, `OnEngineEvent`. |

Always rebuild plugins against the engine version you ship with.

### 3.10 Building a plugin (CMake)

Each plugin folder has a `CMakeLists.txt`. The engine **auto-discovers** plugin folders
at configure time and adds them to the build, so you don't edit the engine's build files
— you just drop your plugin folder in. Discovery rules:

* A folder is only added if it contains a **`CMakeLists.txt`**.
* Folders are searched in the **project root** first (the parent of `ICE_CONTENT_DIR`,
  when building a game project) and then in the **engine root**; the first folder with a
  given name wins, so a project can shadow an engine plugin.
* If `Config/Plugins.json` exists and has an `Enabled` object, only plugins listed there
  with `true` are configured — everything else is skipped with
  `Skipping disabled plugin '<name>' (not enabled in Plugins.json)`. **Enable a new
  plugin in `Plugins.json` before you build it.**

A minimal `CMakeLists.txt`:

```cmake
cmake_minimum_required(VERSION 3.20)
project(MyPlugin LANGUAGES CXX)

if(ICE_PLUGIN_BUILD_STATIC)
    add_library(MyPlugin STATIC Source/MyPlugin.cpp)
    target_compile_definitions(MyPlugin PRIVATE ICE_PLUGIN_STATIC_BUILD)
else()
    add_library(MyPlugin SHARED Source/MyPlugin.cpp)
endif()

# The Plugin SDK provides PluginInterface.h and (for editor UI) imgui::imgui.
target_link_libraries(MyPlugin PRIVATE IceBoxPluginSDK)

# No "lib" prefix; the engine looks for "<Name>.dll/.so/.dylib".
set_target_properties(MyPlugin PROPERTIES PREFIX "" OUTPUT_NAME "MyPlugin")

# Ship the manifest next to the dynamic library so the engine can read it.
if(NOT ICE_PLUGIN_BUILD_STATIC)
    add_custom_command(TARGET MyPlugin POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy_if_different
            "${CMAKE_CURRENT_SOURCE_DIR}/plugin.json"
            "$<TARGET_FILE_DIR:MyPlugin>/plugin.json")
endif()
```

Notes:

* **`IceBoxPluginSDK`** is an INTERFACE target. It puts `Source/Engine/Core` on the
  include path (that's where `PluginInterface.h` lives), links `nlohmann_json` and
  `fmt`, adds `imgui::imgui` when ImGui is available, and requires **C++23**. Link it
  `PRIVATE`.
* Guard editor-only plugins (those that include `imgui.h`) so they **skip** on platforms
  where they don't apply — e.g. `if(ANDROID OR EMSCRIPTEN OR IOS) return() endif()` and
  `if(NOT TARGET imgui::imgui) return() endif()`, as the bundled AIHelper does.
* The engine sets `ICE_PLUGIN_OUTPUT_DIR` per platform — `bin/<platform>/Plugins/<Name>/`
  on desktop, `lib/Android/<ABI>/` on Android, `static_plugins/<Name>/` for static
  builds. Respect it if you customize output paths (the AIHelper `CMakeLists.txt` shows
  the full set of `RUNTIME_/LIBRARY_/PDB_OUTPUT_DIRECTORY*` properties for multi-config
  generators).
* Add `add_dependencies(IceBoxEngine <plugin>)` / `add_dependencies(IceBoxRuntime <plugin>)`
  (guarded by `if(TARGET …)`) so your plugin rebuilds whenever the editor or runtime is
  built. The user builds the engine; you don't need to build the plugin manually.

### 3.11 Worked example: a minimal editor plugin

`Plugins/MyPlugin/Source/MyPlugin.cpp`:

```cpp
#include "PluginInterface.h"
#include <imgui.h>

namespace {

class MyPlugin final : public IceBox::IPlugin {
public:
    IceBox::PluginInfo GetPluginInfo() const override {
        IceBox::PluginInfo info;
        info.Name        = "MyPlugin";
        info.Description  = "Adds a Hello panel to the editor.";
        info.Author      = "You";
        info.Version     = "1.0.0";
        info.APIVersion  = IceBox::ICE_PLUGIN_API_VERSION;
        return info;
    }

    bool OnLoad() override  { return true; }
    void OnUnload() override {}

    void OnEditorInit(const IceBox::EditorPluginContext& ctx) override {
        m_Ctx = ctx;
        if (ctx.ImGuiCtx) ImGui::SetCurrentContext(ctx.ImGuiCtx);
        if (ctx.ImGuiAllocFn && ctx.ImGuiFreeFn)
            ImGui::SetAllocatorFunctions(ctx.ImGuiAllocFn, ctx.ImGuiFreeFn, ctx.ImGuiUserData);
    }

    void OnEditorToolsMenu() override {
        if (ImGui::MenuItem("My Plugin", nullptr, m_Show)) m_Show = !m_Show;
    }

    void OnEditorUI(float /*dt*/) override {
        if (!m_Show) return;
        if (ImGui::Begin("My Plugin", &m_Show)) {
            ImGui::TextUnformatted("Hello from a native plugin!");
            if (ImGui::Button("Spawn an entity") && m_Ctx.Host)
                m_Ctx.Host->CreateEntity(m_Ctx.HostContext, "FromPlugin");
        }
        ImGui::End();
    }

private:
    IceBox::EditorPluginContext m_Ctx{};
    bool m_Show = false;
};

}

ICE_PLUGIN_ENTRY(MyPlugin)
```

Drop this (plus `plugin.json` and `CMakeLists.txt` from above) into `Plugins/MyPlugin/`,
add `"MyPlugin": true` to `Config/Plugins.json`, build the engine, and you'll find
*Tools → My Plugin*.

### 3.12 Shipping data & Visual Script nodes with a plugin

A plugin folder is more than a library — the engine also treats it as a small package.

**Data folders.** `Content/`, `Assets/`, `Scripts/` and `Lua/` inside a plugin folder are
packaged with the plugin on Web (embedded into the Emscripten `.data`) and iOS (copied
into the signed `.app` bundle); on Web, loose `*.lua` files at the plugin root are
embedded too. Load them at runtime relative to `PluginFolderPath`.

**Visual Script nodes.** If a plugin folder contains a `VisualScriptAPI.json`, the editor
loads it alongside the engine's own `Config/VisualScriptAPI.json` and turns its entries
into nodes in the Visual Script graph editor. This is how a plugin that registers Lua
functions (via `OnRegisterLua`) makes them usable without writing code. The file has the
same shape as the engine catalog:

```json
{
    "version": 1,
    "functions": [
        {
            "name": "MyPluginPing",
            "call": "MyPluginPing",
            "surface": "class",
            "category": "MyPlugin",
            "pure": false,
            "args": [
                { "name": "message", "type": "String", "opt": false }
            ],
            "ret": "Bool"
        }
    ]
}
```

| Key | Meaning |
| --- | ------- |
| `name` | The function name, and the node title unless `title`/`call` say otherwise. |
| `call` | The Lua expression actually emitted. |
| `title` | Explicit node title (defaults to `call` when it contains a dot, else `name`). |
| `surface` | Where the node is offered: `class`, `level`, `widget`, `any`. |
| `category` | Palette category. |
| `pure` | `true` for a value node with no execution pins. |
| `ret` / `rets` | Single return type, or a list of named outputs. |
| `args` | Input pins: `name`, `type`, optional `picker`, `values` (enum), `default`, `opt`. |
| `method`, `object`, `colon` | Emit `object.name(...)` / `object:name(...)` instead of a free function. |

Duplicate names are ignored, and entries that collide with a curated engine node are
skipped, so you cannot accidentally overwrite the built-in palette.

---

# Part II — Mods (Lua + content)

## 4. The mod system

A mod is a folder under `Mods/` containing a `mod.json` manifest, an entry Lua script,
and any content it ships. The engine discovers it, and when enabled runs its entry script
inside a **sandboxed Lua environment**, then dispatches lifecycle hooks to it. Mods never
compile and can be toggled live.

> **Mods are a runtime feature.** They are loaded when Play mode (or the shipped game)
> starts and unloaded when it stops — they never run while you are just editing. The
> Mods tab says as much at the bottom of the list.

### 4.1 What a mod can do

* **Ship content** — bundle `.ice_class` game objects, `.ice_widget` UIs, sprites, textures and
  any other assets inside the mod folder, referenced relative to the mod directory.
* **Spawn and drive gameplay** — instantiate its classes at runtime, schedule timers,
  read/write shared game state, and generally use the engine's **Lua API**.
* **Hook the level lifecycle** — run logic when the mod loads/unloads, when levels
  start/end, and every frame or fixed tick.
* **Split itself into modules** — load extra Lua files from its own folder with
  `ModRequire`.
* **Extend the example/base game non-destructively** — add mechanics and UI without
  editing base-game files.

> What a mod *can call* is the engine's Lua API (e.g. `SpawnEntity`, `SetInterval`,
> `GetGameInt`/`SetGameInt`, …). That API is documented in
> [LuaAPI-EN-DOC.md](LuaAPI-EN-DOC.md), whose **§41 Mods** section is the scripting-side
> companion to this chapter. This document covers the mod *structure* and *lifecycle*.

### 4.2 Anatomy of a mod

```
Mods/MyMod/
    mod.json                      # manifest (name, version, entry script, load order …)
    main.lua                      # entry script (runs in a sandboxed env)
    Icon.png                      # optional icon shown in the Mods tab
    README.md                     # optional; indexed by the AI Helper plugin
    modules/                      # optional extra Lua loaded with ModRequire
        helpers.lua
    Content/                      # assets the mod ships
        Classes/   *.ice_class
        Widgets/   *.ice_widget
        Assets/    *.png, *.ice_sprite, ...
```

This mirrors the bundled `Mods/PlatformerCrates`, which ships a crate class, a HUD class,
a HUD widget and a sprite/texture.

### 4.3 The `mod.json` manifest

```json
{
    "Name": "MyMod",
    "Description": "Spawns a pickup when a level starts.",
    "Author": "You",
    "Version": "1.0.0",
    "Icon": "Icon.png",
    "EntryScript": "main.lua",
    "LoadOrder": 100,
    "APIVersion": 1,
    "Dependencies": []
}
```

| Field | Meaning | Default |
| ----- | ------- | ------- |
| `Name` | Unique mod name (matches `Mods.json`). | folder name |
| `Description` | Shown in the Mods tab. | `""` |
| `Author` | Shown in the Mods tab. | `""` |
| `Version` | Display version; also exposed to the script as `MOD_VERSION`. | `"1.0.0"` |
| `Icon` | Path (relative to the mod folder) to a `.png` / `.jpg` / `.jpeg` icon. | auto-detected |
| `EntryScript` | Lua file run on load. | `main.lua` |
| `LoadOrder` | Lower values load first. | `100` |
| `APIVersion` | Mod API version this mod targets. | `1` |
| `Dependencies` | Names of other mods that must load first. | `[]` |

Icon resolution is identical to plugins — see [3.3](#33-the-pluginjson-manifest).

> **`APIVersion` is checked against the engine's plugin ABI version** (currently **4**).
> A mod that declares a higher number is refused with
> `Mod '<name>' requires API version N (engine has 4)`. Leave it at `1` unless you have
> a reason to raise it.

### 4.4 The mod environment & injected globals

Each mod's entry script runs in its **own sandboxed Lua environment**, so a mod's
top-level locals/globals don't collide with other mods or the level script. The
environment inherits the engine's global Lua API and, when a scene is active, the level
API is bound into it as well. The engine injects these globals:

| Global | Value |
| ------ | ----- |
| `MOD_NAME` | The mod's name. |
| `MOD_VERSION` | The mod's version string. |
| `MOD_DIR` | Absolute path to the mod folder (forward slashes). Use it to reference shipped content. |
| `ModRequire(name)` | Loads another Lua file from this mod. See below. |

Reference your bundled content relative to `MOD_DIR`:

```lua
local PICKUP = MOD_DIR .. "/Content/Classes/CL_Pickup.ice_class"
```

**`ModRequire`** is the mod-local equivalent of `require`. It replaces `.` with `/`,
appends `.lua`, resolves the path against `MOD_DIR`, runs the file **in the same sandbox**
as the mod (so `MOD_NAME`/`MOD_DIR` are visible there too) and returns whatever the file
returns:

```lua
local helpers = ModRequire("modules.helpers")   -- <MOD_DIR>/modules/helpers.lua
if helpers then helpers.SayHello() end
```

It soft-fails: a missing file, a syntax error or a runtime error is logged with the mod
name and `ModRequire` returns `nil`, leaving the rest of the mod loading. There is no
result cache — each call re-runs the file.

### 4.5 Lifecycle hooks

Define these as **global functions** in your entry script (or in a file pulled in with
`ModRequire`). The engine calls them automatically:

| Hook | When |
| ---- | ---- |
| `OnModLoad()` | Once, right after the mod's script loads — before the level script runs at play start. (If the mod is enabled mid-session while a level is already active, `OnLevelStart()` is then called too.) |
| `OnLevelStart()` | Each time a level becomes active, **after** the level script and every entity script has been initialized. Dispatched to every loaded mod. |
| `OnLevelUpdate(dt)` | Every frame while the level runs, after the level script's `OnLevelUpdate` and before entity scripts. |
| `OnLevelFixedUpdate(dt)` | Every fixed-timestep tick — use this for physics-adjacent work. |
| `OnLevelLateUpdate(dt)` | Every frame, after the regular update pass. |
| `OnLevelEnd()` | Each time the active level ends/unloads. Dispatched to every loaded mod. |
| `OnModUnload()` | When the mod is unloaded (disabled or on shutdown). Clean up timers, spawned entities, etc. here. |

A typical pattern: set up state and timers in `OnLevelStart`, tear them down in
`OnLevelEnd`/`OnModUnload`.

Errors are per-mod and non-fatal: a hook that raises is logged as
`[Mod:<name>] <hook> error: …` and the other mods still receive the same event. The
per-frame hooks are suppressed while the Lua debugger has execution paused.

### 4.6 Shipping content with a mod

Assets placed inside the mod folder travel with the mod. Reference them by path built from
`MOD_DIR`. Shipped `.ice_class` game objects can carry their **own** embedded scripts and
components — for example, the bundled crate class uses an `OnSensorEnter` script to
detect the player, bump a shared counter, and destroy itself, while the HUD widget reads
that counter every frame. This means a mod can deliver complete, self-contained mechanics
purely as content + a thin entry script.

The Content Browser indexes the project's `Content/` tree, not `Mods/`, so mod assets are
not browsable in the editor — that is by design: a mod resolves its own files through
`MOD_DIR` at runtime. For the asset types you can ship (sprites, classes, widgets,
materials, …) and how they work, see [Assets & Content Browser](Assets-EN-DOC.md).

### 4.7 Load order & dependencies

* Mods load in ascending **`LoadOrder`** (default `100`). Use a lower number to load
  earlier, a higher number to load later.
* **`Dependencies`** lists other mod names that must load first. The engine resolves the
  order with a dependency-aware sort over the `LoadOrder` sequence and **detects cycles**
  (a mod in a dependency cycle, or one whose dependency is missing, broken or not
  enabled, is skipped with a warning).
* **Plugins work the same way.** `plugin.json` `Dependencies` are resolved with the same
  cycle-detecting sort before anything is loaded, and a plugin whose dependency is
  missing or broken is skipped with a warning.

### 4.8 Worked example: a minimal mod

`Mods/MyMod/mod.json` — see [4.3](#43-the-modjson-manifest).

`Mods/MyMod/main.lua`:

```lua
local PICKUP = MOD_DIR .. "/Content/Classes/CL_Pickup.ice_class"
local timerId = 0

function OnModLoad()
    print(string.format("[%s] v%s loaded from %s", MOD_NAME, MOD_VERSION, MOD_DIR))
end

function OnLevelStart()
    -- Spawn one pickup immediately, then one every 5 seconds.
    SpawnEntity(PICKUP, 0.0, 0.0, 0.0)
    timerId = SetInterval(5.0, function()
        SpawnEntity(PICKUP, 0.0, 0.0, 0.0)
    end)
end

function OnLevelEnd()
    if timerId ~= 0 then CancelTimer(timerId); timerId = 0 end
end

function OnModUnload()
    if timerId ~= 0 then CancelTimer(timerId); timerId = 0 end
end
```

Drop the folder into `Mods/`, ship a `CL_Pickup.ice_class` under
`Mods/MyMod/Content/Classes/`, enable **MyMod** in the Mods tab, and it runs the next time
you press Play — no rebuild needed. (`SpawnEntity`, `SetInterval`, `CancelTimer` are
engine Lua functions; see the Lua API doc.)

---

# Part III — Managing & shipping

## 5. Managing plugins & mods

### 5.1 The Plugins & Mods panel

Open it from `Tools → Plugins & Mods`. It has up to three tabs:

* **Plugins (C++)** — every discovered plugin with its icon, an **enable checkbox**, name
  (green when **loaded**, gray otherwise), version, a `[Loaded]` badge, description,
  author, and a **Show in File Browser** button.
* **Mods (Lua)** — the same layout for mods, plus each mod's **Load Order**. A mod that is
  enabled but not yet running shows a yellow `[Pending]` badge; a running one turns green
  with `[Loaded]`.
* **Settings** — appears only when a loaded plugin registered a settings page (via
  `RegisterSettingsPage`); shows those pages grouped by category.

Both list tabs have a **Refresh** button and a **search box** that filters the list by
name (case-insensitive substring). Toggling a checkbox immediately writes the
corresponding config file. The panel's own visibility is remembered in
`Config/Engine.json` under `PanelVisibility.PluginsPanel`.

**What the buttons actually do:**

| Action | Effect |
| ------ | ------ |
| Plugin checkbox on | Loads the plugin **right away** if its library is present, dispatches `OnEngineInit`, then `OnEditorInit`, and saves `Config/Plugins.json`. Its Tools items, panels and asset types appear immediately. |
| Plugin checkbox off | Unloads the plugin, removes everything it registered, and saves the config. |
| Plugin **Refresh** | Unloads every plugin, re-scans `Plugins/`, re-reads the config, re-loads the enabled ones and re-runs `OnEditorInit`. Use it after rebuilding a plugin. |
| Mod checkbox | Saves `Config/Mods.json`. If a level is currently running the mod is loaded/unloaded immediately; otherwise it takes effect at the next Play. |
| Mod **Refresh** | Unloads all mods and re-scans `Mods/` + config. |

> **A brand-new plugin still needs a build.** CMake only configures plugin folders that
> are enabled in `Config/Plugins.json`, so the order is: add the folder → enable it in
> `Plugins.json` (by hand or via the checkbox) → build the engine → **Refresh** or
> restart. Once the library exists, enabling and disabling is live.

### 5.2 Configuration files

Two small files under `Config/` record which extensions are enabled:

`Config/Plugins.json`:

```json
{
    "SearchDirectory": "Plugins",
    "Enabled": {
        "AIHelper": true
    }
}
```

`Config/Mods.json`:

```json
{
    "SearchDirectory": "Mods",
    "Enabled": {
        "PlatformerCrates": false
    }
}
```

| Field | Meaning |
| ----- | ------- |
| `SearchDirectory` | Folder scanned for plugins/mods (`Plugins` / `Mods`). Changing it re-runs discovery against that folder. |
| `Enabled` | Map of `name → bool`. Only `true` entries are loaded, configured by CMake and bundled into builds. |

You can edit these by hand; the editor rewrites them when you toggle checkboxes, merging
into whatever else the file contains.

> **Read-only installs.** If the config lives inside a macOS `.app` bundle, the engine
> keeps the shipped file as the baseline and writes/reads a per-user override in the
> writable config directory, so a player's choices survive without touching the signed
> bundle.

### 5.3 Managing mods from Lua

Mods can also be inspected and managed at runtime from script through the engine's
`Mods` module — the foundation for an in-game mod menu:

| Function | Returns |
| -------- | ------- |
| `Mods.GetAll()` | Array of tables: `name`, `description`, `author`, `version`, `enabled`, `loaded`, `loadOrder`. |
| `Mods.GetInfo(name)` | The same table plus `entryScript` and `folderPath`, or `nil`. |
| `Mods.GetCount()` / `Mods.GetEnabledCount()` | Totals. |
| `Mods.IsEnabled(name)` / `Mods.IsLoaded(name)` | Booleans. |
| `Mods.SetEnabled(name, enabled)` | Enables/disables and **saves** `Config/Mods.json`. |
| `Mods.Refresh()` | Unloads all mods, re-scans `Mods/`, re-reads the config and re-loads the enabled ones. |

```lua
Mods.SetEnabled("PlatformerCrates", false)
Mods.Refresh()
```

The whole module is also available as **Visual Script** nodes under the `Mods` category.
See [LuaAPI-EN-DOC.md](LuaAPI-EN-DOC.md) for the full reference.

### 5.4 The launcher's Plugins & Mods tab

The launcher manages packages **per project**, one level above the editor's per-engine
view. Its **Plugins & Mods** tab (also reachable from a project's context menu via
*Manage Plugins & Mods…*, which pre-selects that project) lists everything found in the
**engine's** `Plugins/` and `Mods/` folders — name, version, author, description and
icon read straight from the manifests — and lets you tick the ones a project should use.

Pressing **Apply** records the selection in the project's `.iceproject` manifest as the
string arrays `"Plugins"` and `"Mods"` (folder names), then makes the project folder match
it — copying in what you ticked and deleting the folders of what you unticked. The full
walkthrough of that tab, including which build leftovers the copy filters out, is in
[Getting Started → Plugins & Mods](Getting-Started-EN-DOC.md#34-plugins--mods).

Two consequences matter here:

* A package attached this way lives **inside the project**, so the project carries its own
  copy independent of the engine installation it was created from.
* Project-level `Plugins/` folders are also picked up by the build: CMake searches the
  project root before the engine root, so a project can ship — or shadow — a plugin
  ([3.10](#310-building-a-plugin-cmake)).

### 5.5 Player-installed plugins & mods

Shipped games on **macOS and iOS** additionally look for user-installed packages outside
the app bundle. On first run the runtime creates a user content root — the app's
writable data directory, or `~/Documents` on iOS — containing `Saves/`, `Config/`,
`Mods/`, `Plugins/` and a short `README.txt` explaining the layout.

Any folder dropped into that `Mods/` or `Plugins/` with a valid `mod.json` / `plugin.json`
is discovered after the game's own packages, is **enabled automatically**, and is skipped
with a log line if a package of the same name already ships with the game. This is what
lets players install mods into a signed, read-only build.

## 6. How plugins & mods are loaded

The **Plugin Manager** is a single engine subsystem used by both the editor and the
runtime. Its flow:

1. **Discover** — scan `Plugins/` and `Mods/` for subfolders with a `plugin.json` /
   `mod.json` (plugins also include any statically-registered ones; mods are sorted by
   `LoadOrder`). In the editor, missing folders are created.
2. **Read config** — load `Config/Plugins.json` and `Config/Mods.json` to know what's
   enabled. If a config names a different `SearchDirectory`, discovery re-runs against it.
3. **Load enabled plugins** — resolve dependency order (skipping cycles and broken
   dependencies), then for each plugin: load the library (or use the static
   registration), resolve `CreatePlugin`, instantiate, check the **API version**, call
   `OnLoad`, replay `OnRegisterLua` for the Lua states that already exist, then
   `OnEngineInit`, and — in the editor — `OnEditorInit`.
4. **Load enabled mods** — this happens at **play/runtime start**, not at editor startup.
   Resolve order from `LoadOrder` + dependencies (skipping cycles), run each mod's entry
   script in its sandboxed environment, and call `OnModLoad`.
5. **Dispatch** — forward `OnUpdate` (runtime frames), runtime start/stop, level
   start/end/update, Lua/Python registration and events to the loaded plugins/mods, gated
   by API version where relevant. Every dispatch is exception-guarded.

Steps 1–3 run once when the editor or the game starts; step 4 runs on every Play.

**Unloading.** Mods are unloaded on play stop, in **reverse** load order, receiving
`OnLevelEnd` then `OnModUnload`. Plugins are unloaded on shutdown (or when you untick
them) in discovery order; each one first loses every editor registration it made, then
gets `OnUnregisterLua`, `OnEditorShutdown` (if its editor side is still live) and
`OnUnload`, and only then is the library freed.

## 7. Shipping plugins & mods in a build

When you build a game (see
[Profiling & Building Games](Profiling-And-Building-EN-DOC.md)), the build:

* Copies **only the enabled** plugins and mods (per `Plugins.json` / `Mods.json`, read
  from the project first and the engine second) into the output, logging each skipped
  package, and copies the two config files alongside.
* Filters out build noise while copying — the `Source/`, `src/`, `build/`, `out/`,
  `CMakeFiles/`, `.cache/`, `.vs/`, `.vscode/`, `.idea/`, `.git/`, `node_modules/` and
  `__pycache__/` folders, the `.obj/.o/.lib/.a/.exp/.ilk/.pdb/.pyc/.pyo/.cmake` files and
  `CMakeLists.txt` / `CMakeCache.txt` / `.gitignore` / `.gitattributes`.
* Then **overlays the built plugin artifacts** on top: only the compiled library
  (`.dll`/`.so`/`.dylib`), its `.pdb` if present, and `plugin.json`.
* For **mods**, ships the whole enabled mod folder (its `Content/`, `main.lua`,
  `mod.json`, icon, README), since mods are loaded as-is at runtime.

Per-platform placement:

| Platform | Plugins | Mods |
| -------- | ------- | ---- |
| Windows / Linux | `<Output>/Plugins/<Name>/` | `<Output>/Mods/<Name>/` |
| macOS | `<Game>.app/Contents/Resources/Plugins/` | `<Game>.app/Contents/Resources/Mods/` |
| Android | `.so` → the APK's `jniLibs/<ABI>/`, the rest of the folder → `assets/Plugins/<Name>/` | staged into the APK's `assets/Mods` |
| iOS | statically linked; `plugin.json` + data folders staged into the signed `.app` | staged into the `.app` bundle |
| Web | statically linked into the runtime | embedded into the Emscripten `.data` via `--preload-file` |

Cooked builds additionally stage `Config/`, `Plugins/` and `Mods/` next to the cooked
content so the packaged game sees the same layout.

The result is that a shipped game loads exactly the plugins and mods you enabled, through
the same Plugin Manager flow as the editor.

---

## 8. Quick reference

### 8.1 Plugins vs Mods

| | Plugins | Mods |
| --- | ------- | ---- |
| Form | Native C++ library / static | Lua + content folder |
| Manifest | `plugin.json` | `mod.json` |
| Entry | `ICE_PLUGIN_ENTRY(Class)` → `IPlugin` | `main.lua` |
| SDK / API | `IceBoxPluginSDK`, C ABI v`4` | engine Lua API, mod `APIVersion` (≤ `4`) |
| Folder / config | `Plugins/` · `Config/Plugins.json` | `Mods/` · `Config/Mods.json` |
| Alive in | Editor + runtime | Play mode / game only |
| Live toggle | Yes, once built | Yes |

### 8.2 IPlugin hooks

`GetPluginInfo` · `OnLoad` · `OnUnload` · `OnUpdate` · `OnRuntimeStart` ·
`OnRuntimeStop` · `OnEngineInit` · `OnEditorInit` · `OnEditorShutdown` ·
`OnEditorToolsMenu` · `OnEditorUI` · `OnRegisterLua` · `OnUnregisterLua` ·
`OnRegisterPython` · `OnEditorEvent` · `OnEngineEvent`

Editor registration: `RegisterPanel` · `RegisterAssetType` · `RegisterMenuItem` ·
`RegisterToolbarButton` · `RegisterViewportOverlay` · `RegisterContextMenuItem` ·
`RegisterSettingsPage` (+ `ShowPanel` · `IsPanelOpen`)

### 8.3 Mod hooks & globals

Hooks: `OnModLoad` · `OnLevelStart` · `OnLevelUpdate` · `OnLevelFixedUpdate` ·
`OnLevelLateUpdate` · `OnLevelEnd` · `OnModUnload`
Globals: `MOD_NAME` · `MOD_VERSION` · `MOD_DIR` · `ModRequire`
Lua module: `Mods.GetAll` · `GetInfo` · `GetCount` · `GetEnabledCount` · `IsEnabled` ·
`IsLoaded` · `SetEnabled` · `Refresh`

### 8.4 Files & folders

| Path | What it is |
| ---- | ---------- |
| `Plugins/<Name>/plugin.json` | Plugin manifest (source folder). |
| `Plugins/<Name>/VisualScriptAPI.json` | Optional extra Visual Script nodes. |
| `Mods/<Name>/mod.json` | Mod manifest. |
| `Config/Plugins.json` · `Config/Mods.json` | Which packages are enabled. |
| `Config/Engine.json` → `PanelVisibility.PluginsPanel` | Whether the panel is open. |
| `bin/<platform>/Plugins/<Name>/` | Built plugin library + `plugin.json`. |
| `<Project>.iceproject` → `"Plugins"`, `"Mods"` | Per-project package selection (launcher). |
| `<user data>/Mods/`, `<user data>/Plugins/` | Player drop-in packages (macOS/iOS builds). |

---

## 9. FAQ & troubleshooting

**Should I write a plugin or a mod?**
Plugin for **editor tooling / native integration**; mod for **content & gameplay** via
Lua. See [Section 2](#2-plugins-vs-mods--which-should-i-use).

**I enabled a plugin but nothing happened.**
If its library was already built, enabling loads it immediately. If it is a new plugin,
CMake never configured it — plugin folders are only added to the build when they are
enabled in `Config/Plugins.json`. Enable it there, rebuild the engine, then press
**Refresh**. Also check the log: a plugin whose `APIVersion` is newer than the engine's is
refused, as is one whose folder has no `.dll`/`.so`/`.dylib`.

**My plugin's ImGui windows don't show.**
You must adopt the host's ImGui context and allocator in `OnEditorInit` (Section
[3.7](#37-plugin-contexts--imgui)). Without it, ImGui calls from the plugin go nowhere.

**My plugin's `OnUpdate` never runs.**
`OnUpdate` is a **runtime** tick — it only fires while the game or Play mode is running.
For per-frame editor work use `OnEditorUI`.

**Where does the engine look for plugins/mods?**
`Plugins/` and `Mods/` relative to the working directory (configurable via
`SearchDirectory` in the config files). Compiled plugins are loaded from the runtime's
`Plugins/<Name>/` output directory. At build-configure time CMake also searches the
project root, and shipped macOS/iOS games additionally scan the player's writable
`Mods/`/`Plugins/` folders.

**Can mods run native code?**
No — mods are Lua + content, sandboxed. For native code, write a plugin.

**My mod's content can't be found.**
Build paths from `MOD_DIR` (e.g. `MOD_DIR .. "/Content/Classes/Foo.ice_class"`); don't
hard-code absolute paths. Extra Lua files are loaded with `ModRequire("folder.file")`,
also relative to `MOD_DIR`.

**My mod's assets don't appear in the Content Browser.**
They aren't supposed to — the browser indexes the project's `Content/` tree only. Mods
resolve their own files at runtime through `MOD_DIR`.

**A mod isn't loading.**
Check the log for: a missing/broken/not-enabled **dependency**, a dependency **cycle**, an
`APIVersion` higher than the engine supports, or a missing `EntryScript`. Disabled mods
(in `Mods.json`) won't load — and no mod loads until you press **Play**.

**Do plugins/mods ship with my game?**
Only the ones **enabled** in `Plugins.json` / `Mods.json`. Plugins ship as compiled
artifacts (source is excluded); mods ship their whole folder. See
[Section 7](#7-shipping-plugins--mods-in-a-build).

**How do I distribute a plugin to others?**
Share the plugin **folder** (with `plugin.json` and `CMakeLists.txt`) so it builds against
their engine, or share a prebuilt library matching their platform and engine ABI version.

**How do I attach a plugin or mod to a specific project?**
Use the launcher's **Plugins & Mods** tab — it copies the package into the project and
records it in the `.iceproject` manifest. See
[Section 5.4](#54-the-launchers-plugins--mods-tab).

---

<sub>© IceBoxCrew Studio. All rights reserved. See [`LICENSE.txt`](../../LICENSE.txt) for full terms.</sub>
