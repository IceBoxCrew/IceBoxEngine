# 🚀 IceBox Engine — Profiling & Building Games

## Full documentation in English

> This document covers two production-critical workflows of **IceBox Engine**:
>
> * **Profiling** — measuring and visualizing performance, both in the **editor**
>   (Statistics panel, Advanced Profiler, viewport debug draw) and at **runtime**
>   (on-screen debug, developer console, the in-game profiler and network-profiler
>   overlays, Tracy-instrumented builds).
> * **Building games** — packaging your project into a standalone, shippable
>   application for all **six target platforms** (Windows, Linux, Android, Web, macOS,
>   iOS), including asset **cooking**, content **packing** (IcePak), build
>   **manifests**, **installers**, **crash reporting** and **DLC** packaging.
>
> Scripting is out of scope; see [LuaAPI-EN-DOC.md](LuaAPI-EN-DOC.md) and
> [PythonAPI-EN-DOC.md](PythonAPI-EN-DOC.md). For the assets you build with, see
> [Assets & Content Browser](Assets-EN-DOC.md).

---

## 📑 Table of Contents

1. [Introduction](#1-introduction)

**Part I — Profiling**

2. [Profiling architecture](#2-profiling-architecture)
   - 2.1 [What the profiler measures](#21-what-the-profiler-measures)
   - 2.2 [Scopes & render-pass timing](#22-scopes--render-pass-timing)
   - 2.3 [Tracy integration](#23-tracy-integration)
   - 2.4 [Temperature sensors](#24-temperature-sensors)
3. [The Statistics panel](#3-the-statistics-panel)
   - 3.1 [General](#31-general)
   - 3.2 [Texture Atlas](#32-texture-atlas)
   - 3.3 [Renderer](#33-renderer)
   - 3.4 [Debug visualization](#34-debug-visualization)
4. [The Advanced Profiler panel](#4-the-advanced-profiler-panel)
   - 4.1 [Recording traces](#41-recording-traces)
   - 4.2 [Overview tab](#42-overview-tab)
   - 4.3 [CPU tab](#43-cpu-tab)
   - 4.4 [Memory tab](#44-memory-tab)
   - 4.5 [GPU tab](#45-gpu-tab)
   - 4.6 [Engine tab](#46-engine-tab)
   - 4.7 [Render Passes tab](#47-render-passes-tab)
   - 4.8 [Trace History tab — timeline, frame diff, trace compare](#48-trace-history-tab--timeline-frame-diff-trace-compare)
   - 4.9 [Where traces live & Chrome-trace export](#49-where-traces-live--chrome-trace-export)
5. [Runtime profiling & debugging](#5-runtime-profiling--debugging)
   - 5.1 [On-screen debug (DebugScreen)](#51-on-screen-debug-debugscreen)
   - 5.2 [Developer Console](#52-developer-console)
   - 5.3 [The runtime profiler overlay](#53-the-runtime-profiler-overlay)
   - 5.4 [The network profiler](#54-the-network-profiler)
   - 5.5 [Driving the profiler from Lua](#55-driving-the-profiler-from-lua)
   - 5.6 [Profiling a shipped build](#56-profiling-a-shipped-build)

**Part II — Building games**

6. [The build pipeline at a glance](#6-the-build-pipeline-at-a-glance)
7. [The Build Game dialog](#7-the-build-game-dialog)
   - 7.1 [Common settings](#71-common-settings)
   - 7.2 [Configuration: Debug vs Release](#72-configuration-debug-vs-release)
   - 7.3 [Crash Reporter](#73-crash-reporter)
8. [Per-platform settings](#8-per-platform-settings)
   - 8.1 [Windows](#81-windows)
   - 8.2 [Linux](#82-linux)
   - 8.3 [Android](#83-android)
   - 8.4 [Web](#84-web)
   - 8.5 [macOS](#85-macos)
   - 8.6 [iOS](#86-ios)
9. [Asset cooking](#9-asset-cooking)
10. [Distribution: manifest, packing & installers](#10-distribution-manifest-packing--installers)
    - 10.1 [Build manifest](#101-build-manifest)
    - 10.2 [Packing content (IcePak)](#102-packing-content-icepak)
    - 10.3 [Installers](#103-installers)
11. [What happens when you click Build](#11-what-happens-when-you-click-build)
12. [Build scripts reference](#12-build-scripts-reference)
13. [Build output locations](#13-build-output-locations)
14. [Toolchain prerequisites](#14-toolchain-prerequisites)

**Part III — DLC**

15. [DLC packaging](#15-dlc-packaging)

**Reference**

16. [Quick reference tables](#16-quick-reference-tables)
17. [FAQ & troubleshooting](#17-faq--troubleshooting)

**Appendix**

- [Headless (dedicated server)](#headless-dedicated-server)

---

## 1. Introduction

Two things stand between a working project and a polished, shippable game: knowing
*where the frame time goes* and being able to *package the project for every platform
you target*. IceBox provides first-class tooling for both, built into the editor and
the engine itself.

* **Profiling** runs in the same engine you ship — the profiler lives in the engine
  core, not the editor — so editor measurements reflect real engine behavior, and the
  same instrumentation feeds the external **Tracy** profiler in instrumented builds.
* **Building** is driven by a single **Build Game** dialog that writes a per-project
  configuration and invokes platform build scripts under
  `Tools/BuildSystem/BuildGame/`. The same flow handles cooking, packing, manifests and
  installers.

---

# Part I — Profiling

## 2. Profiling architecture

At the heart of profiling is the **engine profiler**. It runs in
both the editor and runtime, collects per-frame metrics, manages **scopes** and
**traces**, and is the data source for the editor's profiling panels.

### 2.1 What the profiler measures

Every frame the profiler can collect:

| Category | Metrics |
| -------- | ------- |
| **Frame** | Frame time (ms), FPS, GPU frame time (ms). |
| **CPU** | Process CPU usage (%, exponentially smoothed), physical core count, logical processor count, frequency (GHz). |
| **Memory** | RAM current / peak / total (MB), VRAM tracked / peak / total (MB, via the VRAM tracker), GPU allocation count. |
| **Thermals** | CPU & GPU temperature (°C), when a source is available, with the source name (see [2.4](#24-temperature-sensors)). |
| **Scene counts** | Entities, sprites, draw calls, quads, physics bodies, active scripts, flipbooks, audio, FX, lights, spot lights, cameras, widgets, tilemaps, animators, skeletons, colliders, AI agents, destructibles, joints. |

These feed both the [Statistics panel](#3-the-statistics-panel) and the
[Advanced Profiler](#4-the-advanced-profiler-panel), and every one of them is stored per
frame inside a recorded [trace](#41-recording-traces).

### 2.2 Scopes & render-pass timing

Fine-grained CPU timing comes from **scopes**. Engine (and your) code is instrumented
with scope macros that time a block and nest by call depth:

* `ICE_PROFILE_SCOPE("Name")` — time a named block.
* `ICE_PROFILE_FUNCTION()` — time the current function.

Scopes accumulate per-frame call counts and durations, and also produce a hierarchical
**event list** used to draw the CPU **flame timeline**. The engine is instrumented
throughout (update, render, physics, post-processing, shadows, etc.). Lua scripts can add
their own scopes with `ProfileBegin` / `ProfileEnd` / `ProfileScope` (see
[5.5](#55-driving-the-profiler-from-lua)).

GPU work is timed per **render pass** by the **`RenderPassProfiler`**, which brackets
passes with `ICE_RENDER_PASS("Name")` and uses triple-buffered GPU timer queries to
report **CPU and GPU** time per pass (e.g. shadows, lighting, post-process, UI). Passes
**nest** by call depth (indented in the panel): a parent's time is *inclusive* of its
children, and the **Total** row sums only top-level passes so nothing is double-counted.
Up to **64 passes per frame** are recorded, and results are read back **two frames** later
so the GPU is never stalled — which is why pass GPU times lag the CPU numbers slightly.
Pass capture can be switched off entirely from the [Render Passes tab](#47-render-passes-tab).

### 2.3 Tracy integration

The engine integrates the **Tracy** frame profiler. When built with Tracy enabled, the
same scope and render-pass macros emit Tracy zones, so you can attach the external
**Tracy** application for nanosecond-level, multi-frame, multi-thread analysis. Mutexes
inside the profiler are Tracy-aware too, so lock contention shows up in the same capture.

Tracy is compiled in for:

* **Editor** builds (always), and
* **Debug** game builds.

Release game builds ship with Tracy **off** for zero overhead. Tracy is also **unavailable
on Web, Android and iOS** regardless of configuration; use it on Windows, Linux or macOS.
An instrumented build collects nothing until the Tracy application actually connects, so
leaving it compiled in costs you nothing while nothing is attached.

### 2.4 Temperature sensors

CPU/GPU temperatures come from a background sampler that polls roughly every **1.5
seconds** on its own thread, so reading them costs nothing on the frame thread. The panel
always shows *which* source answered:

| Platform | CPU sources (first that answers wins) | GPU sources |
| -------- | ------------------------------------- | ----------- |
| **Windows** | `LHM/HTTP` → `LibreHWMon` → `OpenHWMon` → `PDH` → `WMI/Perf` → `TempProbe` → `ACPI` | `NVML` → `NVAPI` → `D3DKMT` → `LibreHWMon` → `LHM/HTTP` → `OpenHWMon` |
| **Linux** | `hwmon` (coretemp / k10temp / zenpower…) → `thermal_zone` | `NVML` → `hwmon` → `thermal_zone` |
| **macOS** | Unavailable — Apple exposes no public thermal API | Unavailable (same reason) |

Most Windows machines report **no** CPU temperature out of the box. The engine logs a
hint the first time it fails: install **LibreHardwareMonitor**, run it as Administrator,
enable *Options → Remote Web Server* (default port 8085), and restart the engine — it is
then auto-detected over `http://127.0.0.1:8085/data.json` without admin rights on the
engine side. GPU temperature usually works without any of that on NVIDIA (NVML/NVAPI).

---

## 3. The Statistics panel

**`Window → Stats`** opens the panel titled **Statistics** — a lightweight,
always-available readout of the current scene and renderer, organized into collapsible
sections.

### 3.1 General

* **FPS** (color-coded: green ≥ 60, yellow ≥ 30, red below) and **Frame Time** (ms),
  with a rolling **frame-time graph**.
* **Total Entities**, plus a live per-type breakdown: sprites, flipbooks, audio, FX,
  lights, spot lights, cameras, widgets, tilemaps, animators, skeletons, rigidbodies,
  colliders, sprite collisions, scripts, AI, destructibles, joints.
* For the **selected entity**: its tag and **UUID**.

> The breakdown counts *instances*, not entities: one entity with four sprite instances
> contributes four sprites. That is why the per-type numbers can exceed **Total Entities**.

### 3.2 Texture Atlas

* Atlas **pages**, page **size**, **max sprite** size, and **packed** texture count.
* **Standalone textures** (not atlased).
* Per-page **occupancy** bars (color-coded: green < 50 %, yellow < 85 %, orange above)
  with the **region count** packed into each page, so you can spot atlas pressure.

### 3.3 Renderer

* **Draw calls**, **quad count**, derived **vertex/index** counts.
* **Active particles** and **active emitters**.
* **Active audio** voices currently playing.
* **Physics step (ms)** and, when a physics world exists, full **Box2D** counters
  (bodies, shapes, contacts, joints, islands, tree height), the number of physics
  **worker threads**, and the physics profile (step / collide / solve ms).
* **Lua script GC** size (KB) with a graph and an allocation-rate readout (KB/s,
  color-coded: green under 10 KB/s, yellow under 50, orange above) — invaluable for
  spotting script memory churn.
* **Shadow** stats: an **ON/OFF** state line (shadows count as on only when the shadow
  system is initialized, lighting is in *Lit* mode, shadows are enabled and at least one
  light casts them), shadow casters, edges, shadow-casting lights, directional shadow
  state, shadow map resolution, and — when **Show Shadow Maps** is ticked — a per-light
  **shadow map preview** strip (the directional slot is highlighted).

### 3.4 Debug visualization

The **Debug** section toggles in-viewport debug drawing (these are visualization aids,
not part of the game). The same flags are readable and writable from Lua by name — see
[5.5](#55-driving-the-profiler-from-lua):

* **Basic:** colliders, nav grid, entity markers, light radius, audio range, camera
  frustum, joints, physics contacts, sleeping bodies, velocity vectors, tilemap grid,
  FX bounds, widget bounds, Z-depth color, wireframe mode, freeze culling.
* **Advanced:** shadow maps, shadow edges, light heatmap, nav-grid heatmap, AI state
  overlay, AI perception, AI paths.

---

## 4. The Advanced Profiler panel

**`Tools → Profiler (Tracy)`** opens the panel titled **Profiler** (the "Advanced
Profiler"). This is the deep performance tool: tabbed views, recording, history, and
exports. Its **View** menu offers **Show Graphs**, **Show Engine Stats** and
**Pause Updates** (which freezes the rolling graphs without stopping the engine).

Tabs: **Overview · CPU · Memory · GPU · Engine · Trace History · Render Passes.**
The **Engine** tab is only present while *Show Engine Stats* is enabled.

The trace controls sit above the tabs and stay visible on every tab.

### 4.1 Recording traces

A **trace** is a captured run of many frames you can analyze offline.

* Enter a **trace name** and click **Start Trace**; a red **[REC]** indicator and a live
  frame count appear. Click **Stop Trace** to finish. Leave the name empty and the trace
  is called `Trace_YYYYMMDD_HHMMSS`; characters illegal in filenames are replaced with
  `_`, and a numeric suffix is appended if that name is already taken.
* On stop the trace is summarized (average/min/max frame time, **P95/P99** percentiles,
  average FPS/CPU/GPU, average & peak RAM, average VRAM, per-scope statistics, spike
  frames), **written to disk automatically**, and kept in the **Trace History** (the 20
  most recent).
* Every frame of a trace stores the full scope event list, the render-pass list and all
  scene counters — that is what makes the offline timeline and frame diff possible.
* Traces can also be started and stopped from gameplay code; see
  [5.5](#55-driving-the-profiler-from-lua).

### 4.2 Overview tab

A live dashboard: an overall **Performance** rating (Excellent ≥ 60 FPS / Good 30–60 /
Poor < 30) plus **FPS**, **Frame Time**, **CPU Usage**, **GPU Frame Time**,
**RAM Usage** (current / total, with peak), **VRAM Usage** (tracked / total, with peak),
**CPU/GPU Temperature** with their sensor sources, and headline render counts
(draw calls, quads, entities). Every value is color-coded against a budget.

With **Show Graphs** on it also draws the rolling frame-time graph (overlaid with the
current FPS/ms) and, when GPU timing is available, a GPU-time graph.

Below the graphs the Overview repeats the two things you usually want at a glance:

* **CPU Scope Breakdown** — the **8 hottest scopes** of the last frame as colored bars
  with time and call count.
* **Render Passes** — the last frame's passes as stacked CPU (blue) / GPU (orange) bars,
  with the CPU and GPU totals in the header.

### 4.3 CPU tab

* **Cores** — physical / logical processor count and, when readable, clock frequency in
  GHz.
* **CPU Usage** (%) and **CPU Temperature** with its source name.
* With **Show Graphs**: a CPU-usage graph (0–100 %) and, when a sensor is available, a
  CPU-temperature graph.
* **CPU Scope Breakdown** — a table of the current frame's scopes with **time** and
  **call count**, color-coded (green < 2 ms, yellow < 8 ms, red above).

The deep scope analysis — flame timeline, scope statistics, spikes and frame diff — lives
in the [Trace History tab](#48-trace-history-tab--timeline-frame-diff-trace-compare),
because it needs a recorded trace.

### 4.4 Memory tab

System **RAM** (current / total, peak, with a progress bar and a graph) and
**Video RAM (VRAM)** (tracked / total, peak, with a progress bar and a graph), plus the
running **GPU allocation count** — so you can watch for leaks and growth over a session
or trace.

VRAM numbers come from the engine's own **VRAM tracker** (every texture, buffer and
render target the RHI allocates), not from the driver, so they measure *your* usage
rather than the whole system's.

### 4.5 GPU tab

* A status line naming the **active backend's GPU timer** (e.g. OpenGL timer queries,
  Vulkan timestamps) and whether it is available.
* **GPU frame time** with the equivalent GPU-bound FPS, plus **draw calls** and **quads**.
* **GPU temperature** with its source and, with graphs on, a temperature graph.
* A **VRAM** block (tracked / total, peak, allocation count) with a progress bar.
* With **Show Graphs**: GPU-time, draw-call and VRAM graphs.

GPU timing relies on driver timer queries; when they are unavailable the panel says so
and the value shows as `N/A` — CPU scopes, memory and counters keep working.

### 4.6 Engine tab

Engine-level counts surfaced from the profiler's scene metrics (entities, sprites,
draw calls, quads, physics bodies, active scripts, and the per-component breakdown), plus
a **Resource Manager** block: loaded **texture** and **shader** counts, tracked VRAM,
peak VRAM and GPU allocation count. It complements the Statistics panel with a single
profiling-focused view.

### 4.7 Render Passes tab

Per-render-pass timing from the `RenderPassProfiler`:

* **Enabled** — turns pass capture (and its GPU queries) on or off.
* **Bar Chart** — toggles the graphical CPU/GPU bars above the table.
* Totals for **CPU** and **GPU** across all top-level passes.
* A bar chart of every pass (CPU in blue, GPU in orange) with a legend, indented by
  nesting depth.
* **Render Graph Metrics** — one row per render graph built this frame (the engine builds
  a *scene* graph and a *post-process* graph; see
  [Graphics → The render graph](Graphics-EN-DOC.md#3-the-render-graph)) with **Compile**
  and **Execute** milliseconds, **Passes**, **Skipped** passes, **Reordered** passes and a
  **Validation** column (green `OK`, or the error count — flagged as *strict* when the
  graph runs in strict validation mode). This is where you notice that a pass is being
  skipped or reordered when you did not expect it.
* A precise **table** of every pass with CPU and GPU milliseconds (three decimals) and a
  **Total** row.

This is the fastest way to find which rendering stage is expensive. Passes that never
issue GPU work show `---` in the GPU column.

### 4.8 Trace History tab — timeline, frame diff, trace compare

Lists recorded traces with their summaries, and hosts all offline analysis.

**Compare Traces.** Pick trace **A** and trace **B** from the two dropdowns at the top and
get a side-by-side table of **Avg Frame Time**, **Avg FPS**, **Avg CPU**, **Avg GPU Time**,
**Avg RAM** and **Avg VRAM**, with a percentage **Diff** column colored by whether the
change is an improvement or a regression. This is the fastest before/after check for an
optimization.

**Per-trace details.** Expand a trace to see average/min/max frame time, P95/P99, average
FPS/CPU/GPU, average and peak RAM, average VRAM, the number of detected spike frames, and
the path of the file it was saved to. Two buttons follow: **View in Timeline** (expands
the analysis below) and **Delete Trace** (removes it from the history *and* deletes its
file on disk).

Opening a trace in the timeline gives you:

* **Scope filter** — type a substring; matching blocks in the flame graph are outlined in
  yellow and everything else is dimmed. A small **X** clears it.
* **Frame bar** — every frame of the trace as a colored bar (green < 16.67 ms, yellow
  < 33.33 ms, red above), with 60 FPS and 30 FPS reference lines. Hover for that frame's
  time and FPS, click to select it, or step with the **←/→** arrow keys. The selected
  frame is highlighted.
* **Frame header** — `Frame n / total | ms | FPS`, plus a red **[SPIKE]** tag when the
  frame took more than twice the trace average.
* **Timeline (flame graph)** — the selected frame's nested scope events on a millisecond
  ruler, with a green marker at 16.67 ms. Hover a block for its **time**, **% of frame**
  and **depth**. Zoom with the mouse wheel (0.5×–20×) or the **Zoom** slider (0.5×–10×),
  and **Reset** to go back to 1×. When the frame has a GPU time it is drawn as a green bar
  under the CPU rows.
* **Render Passes** — the selected frame's pass table (CPU / GPU ms, indented by depth)
  with a **Total** row that sums only top-level passes.
* **Frame Comparison (Frame Diff)** — click **A** and **B** to pin the currently selected
  frame into either slot, then compare scope times side by side with a **Diff** column
  (red = slower, green = faster), with the frame time itself as the first row. **Clear**
  resets both slots.
* **Scope Statistics** — **Avg / Min / Max / Total** time, call count and **spike** count
  per scope, sortable by any column. A **spike** for a scope means a frame in which *that
  scope* exceeded twice its own average — which is deliberately different from a *frame*
  spike (a frame over twice the average **frame** time). The header shows how many frame
  spikes the trace contains and offers **Go to spike**, which jumps the timeline to the
  next one.

### 4.9 Where traces live & Chrome-trace export

Stopped traces are written as JSON to **`Tools/Helpers/Profiler/`** under the engine's
writable path — that is your project folder when it is writable (the normal editor case),
otherwise the per-user data directory (`%APPDATA%\IceBoxEngine\game` on Windows,
`~/.local/share/IceBoxEngine/game` on Linux). One file per trace, named after the trace.

On startup the profiler **loads the traces back from that folder** — the 20 most recent by
modification time — so the Trace History survives editor restarts. Deleting a trace in the
panel deletes its file too.

A saved trace file contains the summary, the per-scope statistics, the spike frame
indices, and every captured frame (frame/GPU time, FPS, CPU, temperatures, RAM/VRAM, the
scope event tree, the render-pass list and all scene counters).

**Chrome-trace export** produces a Chrome Trace Event JSON (`chrome://tracing`,
Perfetto-compatible) with named `CPU Main`, `GPU` and counter tracks and engine metadata.
It is exposed as a **scripting call, not a panel button** — call `SaveChromeTrace()` from
Lua (see [5.5](#55-driving-the-profiler-from-lua)); with no argument it writes into the
same profiler folder, and on Web it additionally triggers a browser download.

---

## 5. Runtime profiling & debugging

Profiling isn't limited to the editor — the engine ships the same facilities into the
game so you can diagnose performance on real devices and in shipped builds.

### 5.1 On-screen debug (DebugScreen)

**`DebugScreen`** is the engine's runtime on-screen text system (think "print to
screen"). From gameplay code you can:

* **Print** transient or persistent messages to the screen (with color, duration, scale,
  and an optional integer key so a message updates in place rather than stacking) —
  `PrintScreen` / `PrintScreenEx`, removed with `RemoveScreenMessage` /
  `ClearScreenMessages`.
* **Print world-space** text anchored at a world position (great for labeling entities,
  showing AI state, hit numbers, etc.) — `DrawWorldText` / `DrawWorldTextEx`, removed with
  `RemoveWorldText` / `ClearWorldText`. World text outside the visible viewport is culled.

Lifetime is driven by the duration argument: a **positive** duration expires after that
many seconds, fading out over the last half-second; **`0`** means *this frame only* (call
it every frame for a live readout — which is why `DrawWorldText` defaults to `0`); and a
**negative** duration makes the message persistent until you remove it by key or clear it.
The whole system can be switched off globally with `SetScreenEnabled(false)`
(`IsScreenEnabled()` reads it back). Text is drawn **both in the editor viewport and in
the standalone runtime**, so what you see while testing is what players see.

This is the primary way to surface live values (FPS, counters, state) inside a running
build. (The scripting entry points are in the [Lua API](LuaAPI-EN-DOC.md).)

### 5.2 Developer Console

The engine includes a drop-down **Developer Console** available at
runtime and in the editor:

* Toggle with a configurable key — **`` ` ``** (backquote/tilde) by default; it slides in
  over the game.
* Command **auto-completion** (Tab), input **history** (↑/↓), scrollback, text selection
  and color-coded output (info / warning / error / success / command).
* Executes registered **commands** and **CVars** for live inspection and tweaking while the
  game runs; commands and variables can be registered from both C++ and Lua. See
  [Lua API → Console](LuaAPI-EN-DOC.md#60-console--developer-console--command-system).

### 5.3 The runtime profiler overlay

Standalone **Debug** game builds carry a built-in text overlay that renders the profiler
straight onto the screen — no editor, no external tool. It is drawn top-left and shows:

* The state of the collider / entity-marker / nav-grid debug draws and whether a trace is
  recording.
* **FPS** and frame time (color-coded), **CPU %**, **RAM** (current + peak), **VRAM**, and
  **GPU** time when a GPU timer exists.
* **Engine Stats** — entities, sprites, draw calls, quads, physics bodies, scripts,
  tilemaps, lights, spot lights, FX, audio, widgets, cameras, flipbooks, animators,
  colliders, AI, destructibles, joints.
* **Hot Scopes** — the 8 most expensive scopes of the last frame with time and call count.
* **Render Passes** — the first 6 passes with CPU (and GPU) milliseconds.
* A `[TRACING: n frames]` line while a trace is being recorded.

The overlay is **off by default** and has **no built-in hotkey** — bind one yourself and
call `ToggleDebugProfiler()`. It exists only in builds compiled as **Debug** *without* the
editor; in Release the toggle is a harmless no-op.

### 5.4 The network profiler

A second overlay, drawn on the right-hand side of the same screen, covers multiplayer
traffic. The engine instruments every send/receive path (ENet on desktop, WebSocket on
Web) and aggregates per message type. The overlay shows:

* **State** (Offline / Server / Client-Connected / Connecting / Reconnecting) and the
  player count.
* **Ping** (color-coded: green < 80 ms, yellow < 200 ms, red above).
* **Bandwidth per second** — TX/RX in KB/s and packets/s.
* **Totals** — bytes and packets sent/received, human-readable.
* A per-**message-type** breakdown sorted by total traffic, so an unexpectedly chatty
  message type is immediately visible.

Like the profiler overlay it is Debug-runtime only and has no default key — toggle it with
`NetworkProfiler.Toggle()`. The same counters are readable from Lua at any time (with a
120-second rolling history and a `SaveReport` call), and the editor exposes them as live
graphs in the **Network Manager** panel — see
[Editor → Network Manager](Editor-EN-DOC.md#11-network-manager-enet) and
[Lua API → Network](LuaAPI-EN-DOC.md#32-network--multiplayer-network).

### 5.5 Driving the profiler from Lua

Everything the editor's trace controls do is available to gameplay code, which is how you
profile on a phone or in a shipped build:

| Call | What it does |
| ---- | ------------ |
| `StartProfilerTrace([name])` → `bool` | Start recording a trace. Returns `false` if one is already running. |
| `StopProfilerTrace()` | Stop and summarize the current trace (and write it to disk). |
| `IsProfilerTracing()` → `bool` | Whether a trace is recording. |
| `SaveChromeTrace([filename])` → `bool` | Export the latest finished trace — or the live one, or a single-frame snapshot if there is none — as Chrome Trace Event JSON. |
| `ProfileBegin(name)` / `ProfileEnd(name)` | Open and close a custom CPU scope by name. |
| `ProfileScope(name, fn)` → `any` | Run `fn` inside a scope and return its result, even if it errors. |
| `ToggleDebugProfiler()` / `GetDebugProfilerVisible()` | Flip / read the [runtime profiler overlay](#53-the-runtime-profiler-overlay). |
| `NetworkProfiler.Toggle()` / `NetworkProfiler.IsVisible()` | Flip / read the [network overlay](#54-the-network-profiler). |
| `GetDebugFlag(name)` / `SetDebugFlag(name, v)` / `ToggleDebugFlag(name)` | Read/write any viewport debug flag by name (`"ShowColliders"`, `"WireframeMode"`, …). |
| `GetDebugFlagNames()` / `ClearDebugFlags()` | List every valid flag name / reset them all. |
| `IsDebugBuild()` → `bool` | Whether this binary was compiled as Debug. |

Custom scopes appear in the CPU breakdown, the flame timeline and Tracy exactly like
engine scopes. Full signatures and examples are in
[Lua API → Debug](LuaAPI-EN-DOC.md#29-debug--debugging).

### 5.6 Profiling a shipped build

* **Debug** desktop game builds include **Tracy** instrumentation — connect the external
  Tracy app to profile the actual shipped binary on the target machine. (Tracy is not
  available on Web/Android/iOS; use the overlays and traces there.)
* Debug builds also carry the [profiler](#53-the-runtime-profiler-overlay) and
  [network](#54-the-network-profiler) overlays, and can record traces and export Chrome
  traces from Lua — that is the practical way to profile on a phone.
* The **Statistics**/**Profiler** systems are part of the engine core, so the same
  metrics are computable at runtime and can be surfaced via `DebugScreen` or custom UI.
* **Release** builds disable Tracy and both overlays for zero overhead; use
  `DebugScreen`/console and your own in-game readouts for lightweight runtime checks.

---

# Part II — Building games

## 6. The build pipeline at a glance

Building turns your project (engine runtime + your `Content/`) into a standalone app for
a chosen platform. The high-level flow:

```
Build Game dialog (settings)
        │
        ▼
[optional] Cook assets ──► Saved/Cooked/<tag>/Content   (mobile/web)
        │
        ▼
Consolidate redirectors
        │
        ▼
Invoke platform build script  (Tools/BuildSystem/BuildGame/build_<platform>.bat|.sh)
        │   → CMake + toolchain compile IceBoxRuntime with your Content embedded/referenced
        ▼
Collect output → your chosen output folder
        │
        ├─ copy runtime/bundle, game.json, LICENSE, THIRD_PARTY_NOTICES, ThirdPartyLicenses/
        ├─ copy Content/ (cooked on desktop), enabled Plugins/ and Mods/ (per Packages)
        ├─ copy + patch Config/Engine.json, Plugins.json, Mods.json, CollisionGroups.json
        ├─ [optional] Pack Content → Content.icepak (zstd, optionally split)
        ├─ [macOS] re-codesign, then .dmg / .pkg / notarize
        ├─ [optional] Generate game_manifest.json (SHA-256 of every file)
        └─ [optional] Create installer (NSIS / .deb)
```

The dialog itself does not compile code — it gathers settings, runs the appropriate
**build script** asynchronously (so the editor stays responsive, with an animated progress
bar and a **Stop** button), then performs the post-build packaging steps. Script output is
streamed line by line into the editor log with a `[build]` prefix.

## 7. The Build Game dialog

Open it from **`Tools → Build Game…`**; it shows the **Build Game Settings** modal. The
dialog adapts to the selected **Target Platform**, showing only the relevant sections, and
every setting is remembered in the editor config between sessions (passwords excepted).

### 7.1 Common settings

| Setting | Meaning |
| ------- | ------- |
| **Target Platform** | Windows, Linux, Android, Web, macOS, iOS. The rest of the dialog changes to match. |
| **Configuration** | **Debug** or **Release** (see [7.2](#72-configuration-debug-vs-release)). |
| **Game Name** | The product name; used for the executable/bundle name and installer metadata. |
| **Icon Path** | App icon (`.png`/`.ico`/`.jpg`/`.jpeg`/`.bmp`); PNG is converted to platform icon formats automatically. |
| **Output Path** | Folder to place the finished build — a `<Output>/<GameName>` subfolder is created and **wiped** at the start of each build. A **…** button opens a native folder picker. |
| **Version Name / Version Code** | Human version string (e.g. `1.2.0`) and integer build number (minimum 1). |
| **Publisher** | Studio/publisher name, used in installers and metadata. |
| **Include Plugins** | *(Packages)* Ship the plugins ticked in `Tools → Plugins & Mods`. On by default; applies to all six platforms. Plugins marked `"EditorOnly": true` are never shipped, whatever this is set to. |
| **Include Mods** | *(Packages)* Ship the mods ticked in `Tools → Plugins & Mods`. On by default; applies to all six platforms. |

The dialog refuses to start with an empty **Game Name** or **Output Path**. While a build
runs every setting is locked, the **Build** button becomes **Stop**, and cancelling wipes
the half-written output folder. **Esc** closes the dialog when nothing is running.

> A cross-platform note: on a non-Windows host, selecting **Windows** triggers a MinGW
> cross-compile; **macOS/iOS** can only be built on a macOS host (the Windows `.bat`
> stubs for those platforms tell you to run the `.sh` on a Mac).

### 7.2 Configuration: Debug vs Release

* **Debug** — symbols, unoptimized, **Tracy** profiling compiled in (desktop), plus the
  [runtime profiler](#53-the-runtime-profiler-overlay) and
  [network profiler](#54-the-network-profiler) overlays. Best for diagnosing crashes and
  performance on the target.
* **Release** — optimized, Tracy and the debug overlays off. Ship this.

> **Debug from an installed engine.** The two bullets above describe a **Debug** build made
> from a full engine source tree. An engine installed from a distribution installer has no
> engine sources — it links a pre-built engine core instead, and Build Game picks the right
> one for you:
>
> | You choose | What the game gets |
> |---|---|
> | **Release** | Optimised, no debug tooling. Ship this. |
> | **Debug** | Optimised **and** fully instrumented: **Tracy**, the [runtime profiler overlay](#53-the-runtime-profiler-overlay), the [network profiler overlay](#54-the-network-profiler), `IsDebugBuild()` returning `true`, plus full debug symbols and an unstripped binary. |
>
> So a **Debug** build from an installed engine keeps the engine's own code optimised, which
> is invisible to you: a distribution ships no engine sources to step through, and your game
> logic lives in Lua/Python either way. You still get every debug facility and full symbols
> for your own build.

### 7.3 Crash Reporter

Every platform section exposes a **Crash Report URL** field. When a shipped game crashes,
the engine writes a full report (stack, system info, graphics info and the tail of the log)
to `Saved/CrashReports/` on the player's machine and shows a crash dialog. If you fill in
an `http://` or `https://` endpoint you control, that dialog can upload the report in one
click.

* The value is validated: anything that is not an `http(s)` URL is ignored with a warning
  in the build log.
* It is written into the build's **`game.json`** as `CrashReportUrl`. On **iOS** the field
  is passed to the build script instead (`--crash-report-url`) and CMake bakes it into the
  `game.json` inside the signed `.app`.
* Leave it empty and the packaged game uploads nothing — reports stay on the player's disk.

> **You do not have to build the endpoint yourself.** The engine ships a free, serverless
> one: [`Tools/CrashReport/`](../../Tools/CrashReport/README.md) contains a Google Apps
> Script web app that accepts the upload and forwards every report to your inbox as an
> email with the report attached. Deploy it, paste the resulting URL into **Crash Report
> URL**, and one-click sending works on every platform.

---

## 8. Per-platform settings

Each platform exposes its own section in addition to the common settings. The graphics
backend chosen here is written into the build's `Config/Engine.json` and selected at
runtime (with fallbacks).

### 8.1 Windows

* **Output:** `.exe` executable with DLLs.
* **Graphics API:** **OpenGL 4.6** (default), **OpenGL 3.3** (compatible with GPUs from
  ~2010+), or **Vulkan** (falls back to OpenGL 4.6/3.3 if unavailable).
* **Architecture:** **x64** (default) or **x86** (32-bit, for maximum compatibility with
  older systems). A 64-bit editor builds both; a 32-bit editor is locked to **x86** and
  the selector is disabled, because a 32-bit toolchain cannot emit 64-bit binaries.
* Supports the **Distribution** options (manifest, pack content, installer) — see
  [Section 10](#10-distribution-manifest-packing--installers).

### 8.2 Linux

* **Output:** Linux ELF executable.
* **Graphics API:** OpenGL 4.6 / OpenGL 3.3 / Vulkan (same as Windows).
* **Architecture:** x64 / x86 — same host rule as Windows: 64-bit editor builds both,
  32-bit editor builds x86 only. The x86 build needs the 32-bit multilib toolchain
  (`gcc-multilib` / `g++-multilib`) and the `x86-linux` vcpkg triplet.
* Supports **Distribution** options, including a **.deb** installer.

### 8.3 Android

* **Output:** `.apk` package, or `.aab` **App Bundle** for Google Play.
* **ABI:** `arm64-v8a` (default, 64-bit ARM), `armeabi-v7a` (32-bit ARM, older devices),
  `x86_64` (64-bit emulators), or `x86` (32-bit legacy emulators).
* **Graphics API:** **OpenGL ES 3.2** or **Vulkan** (falls back to GLES 3.2/3.0).
* **Orientation:** Landscape / Portrait / Auto.
* **Package Name** (e.g. `com.studio.game`), **Min SDK** / **Target SDK** (24–36; Min is
  clamped so it never exceeds Target), **Version Code** / **Version Name**, **Publisher**.
* **Services** (toggles, wired into the Android template): **Ads** (AdMob App ID),
  **In-App Purchases**, **Play Games** (App ID), **Consent**, **In-App Review**,
  **Notifications**, **Bluetooth**, **Firebase**, **Saved Games**.
* **Extra Permissions** (custom Android permissions).
* **Signing:** keystore path, keystore password, key alias, key password (for release
  signing; unsigned/debug-signed otherwise). Passwords are **never stored in the editor
  config** — re-enter them each session — and they are handed to the build script through
  short-lived temp files (`--keystore-pass-file` / `--key-pass-file`, deleted when the
  build finishes) rather than on the command line, so they do not leak into process
  listings. The **Generate keystore…** button creates a new release keystore for you with
  `keytool` (JDK) — fill in a keystore password (6+ chars) first; the key alias defaults to
  the game name and the file defaults to `<project>/Keystore/<GameName>.jks`, the
  certificate name comes from **Publisher** (or the game name), and generation is refused
  if the file already exists so an existing key can never be overwritten. A release key is
  required to publish on Google Play and to ship consistent updates; it does **not** remove
  the "install from unknown sources" prompt shown when sideloading any APK. Keep the
  generated keystore and its password backed up — the same key must sign every future
  update.

### 8.4 Web

* **Output:** `.html` + `.wasm` + data files (Emscripten).
* **Renderer:** **Auto (WebGPU → WebGL 2.0)**, **WebGPU** (Chrome/Edge 113+, Safari 18+),
  or **WebGL 2.0** (maximum compatibility). Shaders are translated to **WGSL** in the
  browser at load time.
* **Web options:** **Web3** support, **main-loop** mode, **pthreads** (multithreading,
  needs cross-origin isolation).
* Version Name / Version Code / Publisher.
* The current level is passed as the **start scene**; content is embedded into the
  Emscripten `.data` (so `Engine.json`/`Plugins.json`/`Mods.json` are preloaded, not
  copied loose). Mods are not supported on Web.
* The packaging step also copies the generated companion files next to the `.html`
  (`.js`, `.wasm`, `.data`) plus `favicon.png` and `apple-touch-icon.png` when the build
  produced them.

### 8.5 macOS

* **Output:** `.app` bundle (built on a Mac with Xcode).
* **Graphics API:** **Metal via ANGLE** (OpenGL ES 3.0 translated to Metal) or
  **Metal via MoltenVK** (Vulkan on Metal).
* **Architecture:** **x86_64** (Intel) or **arm64** (Apple Silicon).
* **Deployment Target** (clamped to the engine minimum), **Bundle ID**, app **Category**.
* **Signing:** codesign identity, **Hardened Runtime**, **Notarize** (+ keychain notary
  profile).
* **Distribution:** an optional drag-install **`.dmg`** and an optional real **`.pkg`
  installer** (with its own **Installer Signing Identity**, plus **Homepage** and
  **Contact Email**) — see [10.3](#103-installers).
* Everything the game needs (`game.json`, `Content/`, `Config/`, `Plugins/`, `Mods/`,
  `LICENSE.txt`, `THIRD_PARTY_NOTICES.txt`, `ThirdPartyLicenses/`) is staged into
  `Contents/Resources/` inside the bundle. Because staging invalidates the build-time
  signature, the editor **re-signs the finished bundle** afterwards — with your identity
  (adding a secure timestamp and, with Hardened Runtime, the default entitlements) or
  ad-hoc when no identity is set.

### 8.6 iOS

* **Output:** signed **`.ipa`** (via Xcode) or unsigned `.app` bundle.
* **Graphics API:** Metal via MoltenVK (Vulkan on Metal).
* **Bundle ID**, **Development Team**, **Code-Sign Identity**, **Provisioning Profile**,
  **Deployment Target** (clamped to the engine minimum).
* **Destination:** Device (`iphoneos`) or Simulator (`iphonesimulator`).
* **Orientation:** Landscape / Portrait / All; **Requires Fullscreen**.
* **Export method** (when creating an IPA): development / ad-hoc / app-store / enterprise.
  Selecting the Simulator destination forces `--no-ipa` — simulator builds cannot be
  packaged as an IPA.
* Optional **Entitlements**, **App Icon** (`.png`/`.appiconset`), **Launch Screen**
  (`.storyboard`) assets.
* **Capabilities:** Game Center, In-App Purchases, CloudKit, Push Notifications and
  **Multicast Networking**. Each enabled capability is merged into the `.entitlements`
  file used for signing. Multicast Networking is what lets the engine's UDP-broadcast
  LAN game discovery work on iOS 14+, and Apple must approve that entitlement for your
  Team ID before a matching provisioning profile can be issued.
* **Info.plist / Privacy:** **Deep Link URL Scheme** (registered in `CFBundleURLTypes`,
  delivered to the `DeepLinks` Lua API), **AdMob App ID** (`GADApplicationIdentifier`),
  **Local Network Purpose String** (always written — LAN multiplayer needs it on
  iOS 14+), free-form **Extra Usage Descriptions** (`NSCameraUsageDescription=Reason`,
  one per line) and **Uses Non-Exempt Encryption** (`ITSAppUsesNonExemptEncryption`).
  `NSUserTrackingUsageDescription` and the Local Network string are declared
  automatically; naming either one in **Extra Usage Descriptions** replaces the default
  text instead of duplicating the key. The ATT string is not optional — iOS terminates an
  app that calls `requestTrackingAuthorization` without it, and both
  `Consent.ShowForm()` and `Permissions.Request("TRACKING")` do exactly that.
* **Privacy manifest:** every `.app` gets a `PrivacyInfo.xcprivacy` declaring the
  required-reason APIs the runtime touches (file timestamps, disk space, system boot
  time, `NSUserDefaults`). Without it App Store Connect rejects the upload with
  ITMS-91053. Drop your own `PrivacyInfo.xcprivacy` next to the project's `Content/` to
  override the engine's — the project copy always wins, so extend it rather than fight it
  when you add an SDK that collects data.
* **Ads on iOS:** the Google Mobile Ads SDK is not shipped with the engine. Run
  `Tools/BuildSystem/BuildEngine/fetch_googlemobileads.sh` once to vendor
  `GoogleMobileAds.xcframework`, then enable **Ads & Attribution** and set the AdMob App
  ID. With the SDK vendored the build links it and `Ads.*` goes live; without it those
  calls stay no-ops. An empty AdMob App ID combined with a linked SDK fails configure on
  purpose, because the SDK aborts at launch when `GADApplicationIdentifier` is absent.
* The `.app` bundle is fully self-contained: `game.json` (start scene, name, version,
  orientation, crash-report URL), `Content/`, `Config/` (with the render backend pinned to
  MoltenVK), enabled `Plugins/` and `Mods/` are staged **before** code signing, so the
  packaged `.ipa` needs no post-processing — and the editor deliberately skips its own
  sidecar copies on iOS for the same reason.

---

## 9. Asset cooking

Enable **Cook Assets** to convert raw media into compact, runtime-friendly forms before
packaging — dramatically reducing build size while runtime loaders stay unchanged. The
cook settings appear in the dialog when enabled.

| Media | Options |
| ----- | ------- |
| **Texture Format** | **PassThrough** (copy raw PNG/JPG), **WebP** (lossy, ~80% smaller), **KTX2 UASTC** (GPU-compressed, high quality), **KTX2 ETC1S** (GPU-compressed, small), **WebP Lossless**. The lossy formats share a **Quality** 1–100 (default 80). |
| **Audio Format** | **PassThrough**, **Ogg Vorbis**, or **Opus**, with a **Bitrate** 32–320 kbps for the lossy options (96 good for SFX, 128–192 for music). |
| **Video Format** | **PassThrough** or **VP9 (WebM)** at a chosen **CRF** (18 = high quality … 32 = smaller), with optional **max resolution** (Source/1080p/720p/480p), **max FPS** (Source/30/60), **strip audio**, and audio bitrate 64–320 kbps. |
| **Font Mode** | **PassThrough**, **Subset** (only glyphs declared in font sidecars), or **Auto-subset** (only glyphs actually used in the project). |
| **JSON Mode** | **PassThrough** or **Minify** (strip whitespace from `.json` and `.ice_*` sidecars). |

**Lossless guard:** with a lossy texture format selected, the cooker encodes each texture,
decodes the result back and compares it against the source. When the difference would be
visible — which is what lossy WebP and GPU block compression do to hard-edged pixel art —
that texture is stored losslessly instead, so cooked builds match uncooked ones pixel for
pixel. The summary line reports how many textures were **kept lossless**, and each one is
logged with its measured error. Override per texture with **Compression** in the texture
sidecar (`Lossless` to never compress, `Always Compressed` to skip the check).

**Platform restrictions:** WebP textures are not available on the **iOS** and **Web**
runtimes, and VP9 video is not available on **iOS** (AVFoundation decodes H.264/HEVC only);
those are forced to PassThrough. **KTX2** requires a desktop x86/x64 runtime
(Windows/Linux, or macOS Intel) because it needs the Basis Universal transcoder — on other
targets it falls back to WebP, or to PassThrough where WebP is unavailable too. The dialog
enforces all of this automatically: unsupported entries are hidden or disabled, an orange
note explains the substitution, and **your stored preference is kept** so switching back to
a capable platform restores it. A Web build with PassThrough video also gets an
informational note: H.264/MP4 sources will not play in Chromium builds without proprietary
codecs (most Linux browsers), so pick VP9 for portable Web video.

The cooker is **cache-aware** — the per-build cache lives in
`Saved/CookCache/<GameName>_<Platform>.json`, so only changed inputs are re-cooked — runs
across **parallel workers** (as many as the machine has hardware threads by default), and
mirrors deletions: files that disappeared from the source are removed from the cooked
directory. The final log line reports cooked / pass-through / cached / kept-lossless /
failed counts and the total byte savings; **any failure aborts the build**. The inputs to
cooking are the **sidecars** you author in the Content Browser (texture sRGB/max-size,
font glyph ranges, etc.) — see [Assets & Content Browser](Assets-EN-DOC.md).

> For **mobile/web** builds, cooking runs *before* the build script and the cooked
> `Saved/Cooked/<tag>/Content` directory is fed into the build (so cooked assets are
> embedded). Because those targets embed the whole project root, the cooker also stages
> `Config/`, `Plugins/`, `Mods/`, `LICENSE.txt` and the `.iceproject` file next to the
> cooked content. For **desktop**, cooking runs during the packaging step instead, and the
> cooked tree is what gets copied (and packed) into the output.

---

## 10. Distribution: manifest, packing & installers

For **Windows / Linux / macOS**, a **Distribution** section provides the final packaging
options.

### 10.1 Build manifest

**Generate Manifest** writes `game_manifest.json` next to the build, containing the game
name, version, build date, config, platform, whether the content is packed, and an entry
for **every file** with its **SHA-256 hash** and size (plus totals). Useful for store
deployment, patching, and integrity checks. It is generated *after* packing, so it
describes what actually ships.

### 10.2 Packing content (IcePak)

**Pack Content** packs the entire `Content/` tree into a single **`Content.icepak`**
archive, reducing file clutter and protecting assets from casual modification. The
runtime mounts the pak through the [IceVFS](Assets-EN-DOC.md#24-the-virtual-file-system-icevfs)
so loaders are unchanged.

| Option | Meaning |
| ------ | ------ |
| **Max Pak Size (MB)** | Split into `Content_0.icepak`, `Content_1.icepak`, … when the total exceeds this. `0` = single file. |
| **IcePak Compression Level** | **zstd** level **1–22**: 1 = fastest, 3 = default (balanced), 19 = high (~10–15% smaller, slow), 22 = maximum (very slow). |

Before packing, the editor **resolves asset redirectors**: every reference in the staged
content is rewritten to the final path (following redirect chains) and the `.ice_redirect`
stub files are deleted, so nothing in the shipped pak depends on them. After the pak is
written successfully the **loose `Content/` folder is removed** — if packing fails the
loose files are kept, so a failed pak never produces a contentless build. On macOS the pak
lands inside `Contents/Resources/`.

**IcePak format:** a simple indexed archive (magic `ICEPAK02`, version 2) with per-entry
path, offset, original & compressed sizes, and a compressed flag (zstd); a file that does
not compress is stored raw. Paks can be mounted in layers, so patch/DLC paks override base
content.

### 10.3 Installers

**Create Installer** (Windows/Linux) produces a proper installer with uninstaller and
system registration, using **Homepage** and **Contact Email** metadata. The best icon
found in the output folder (`.ico` > `.png` > `.bmp` > `.jpg`) and the project's
`LICENSE.txt` are passed to the script automatically:

* **Windows** — an **NSIS** `.exe` installer (requires NSIS installed). Invokes
  `create_game_installer_windows.bat` (`.sh` on non-Windows hosts).
* **Linux** — a **`.deb`** package (requires `dpkg-deb`; on Windows hosts this runs via
  WSL). Invokes `create_game_installer_linux.*`. The game lands in `/opt/<Game Name>`,
  with a launcher on `PATH` as `/usr/bin/<package-name>`, a menu entry in
  `/usr/share/applications`, a 256×256 icon in the hicolor theme and the license as
  `/usr/share/doc/<package-name>/copyright`. The package name is the game name reduced
  to a Debian-legal form (lowercase, `a-z0-9+.-`), and the control version is the build
  version with any leading `v` stripped. Removing the package also clears `/opt/<Game Name>`,
  so save files written next to the game do not survive an uninstall.

The installer is written **next to** the output folder — `<Output>/<Name>-<version>-<Config>-Windows-<arch>-Setup.exe`
or `…-Linux-<arch>-Setup.deb` — and, once it exists, the staged `<Output>/<GameName>/`
folder is **deleted**, because the installer now contains everything. If the installer step
fails, the loose build is left in place and the log says so.

**Size limits are checked before packing.** A **Windows** installer cannot exceed **2 GB**,
and no single file inside it may reach 2 GB either. The script measures your game folder
first, picks the compression mode that fits, and — if no mode could work — refuses up front
and names the file that is too large, instead of failing deep inside the packing step.

**`.deb` packages** have no such ceiling, but they are staged through a temporary copy, so
the script checks that the staging filesystem has room: `/tmp` is a RAM-backed `tmpfs` on
many distributions and is easy to fill with a large game. Set **`ICE_STAGING_ROOT`** to point
staging somewhere else. When you build a `.deb` **from a Windows host** (through WSL),
staging deliberately lands on the WSL filesystem rather than on `C:`, because a mounted
Windows drive cannot carry the POSIX permission bits a package needs — so `ICE_STAGING_ROOT`
must name a Linux-native directory in that case, not a path under `/mnt/`:

```bash
export ICE_STAGING_ROOT=$HOME/.cache/icebox-stage
```

Either way the staged tree is given package-correct ownership and permissions, so a `.deb`
built from Windows is as correct as one built on Linux.

**macOS** has its own two options in the macOS section, applied after the bundle is
re-signed:

* **Create .dmg** — a drag-install disk image built with `hdiutil` containing the `.app`
  and an `/Applications` symlink, signed with your identity when one is set.
* **Create .pkg Installer** — a real installer produced by `create_game_installer_macos.sh`
  (needs the Xcode Command Line Tools: `pkgbuild`/`productbuild`) that installs into
  `/Applications`, shows your license and adds a Desktop shortcut. It is written as
  `<Name>-<version>-<Config>-macOS-<arch>-Installer.pkg` and can be signed with the
  **Installer Signing Identity**.
* **Notarize** submits **each** artifact that was produced (the `.dmg`, the `.pkg`, or the
  `.app` when neither was requested) to `notarytool --wait` using your keychain notary
  profile, and staples the ticket on success.

---

## 11. What happens when you click Build

1. **Validation** — game name and output path must be set; the output `<path>/<name>`
   folder is cleaned and recreated.
2. **Script & artifact names** are chosen for the platform (e.g. `build_windows.bat`,
   runtime `IceBoxRuntime.exe`, game file `<Name>.exe`).
3. **Argument assembly** — all dialog settings become command-line flags (see
   [Section 12](#12-build-scripts-reference)); keystore passwords go through temp files.
4. **Pre-cook** (mobile/web, if Cook Assets is on) → cooked content directory; on failure
   the build aborts.
5. **Consolidate redirectors** so references resolve cleanly in the build.
6. **Run the build script** asynchronously — CMake configures and builds `IceBoxRuntime`
   with your content; the dialog shows a live progress bar and a **Stop** button, and
   streams script output to the log.
7. **Collect output** — locate the built runtime (in `out/gamebuild/…`, or the per-user
   **GameBuilds** cache fallback) and copy it to your output folder as `<GameName>`
   (file or bundle, preserving symlinks for Apple bundles).
8. **Copy legal files** — the project's `LICENSE.txt`, plus the engine's
   `THIRD_PARTY_NOTICES.txt` and `ThirdPartyLicenses/` (also into `Contents/Resources/`
   for macOS).
9. **Stage content** — copy `Content/` (cooking it here on desktop), skipping the engine's
   `Examples` folder.
10. **Write `game.json`** — game name, version, start scene, build config, build platform,
    icon file name and the crash-report URL. Skipped on iOS, where CMake writes it inside
    the signed bundle.
11. **Copy sidecar config** — `Config/Engine.json` plus `Plugins.json` / `Mods.json` /
    `CollisionGroups.json` (skipped where they are embedded instead: Web, Android, iOS;
    `Plugins.json` / `Mods.json` are also skipped when the matching **Include** option is
    off).
12. **Stage plugins & mods** — only the entries marked enabled in `Plugins.json` /
    `Mods.json` are copied, build junk (`Source/`, `build/`, `out/`, `.git/`, `*.obj`,
    `*.pdb`, `CMakeLists.txt`, …) is filtered out, and the freshly built plugin binaries
    (`.dll`/`.so`/`.dylib` + `plugin.json`) are overlaid on top. Skipped entirely when
    **Include Plugins** / **Include Mods** is off, and for any plugin whose `plugin.json`
    declares `"EditorOnly": true`. Also skipped where plugins are linked in statically
    (Web, iOS) or bundled by Gradle (Android); mods are not supported on Web.
13. **Patch `Config/Engine.json`** — set `Rendering.RenderBackend` for the chosen API and
    strip the `Editor` and `Network` sections, which are editor-only.
14. **Copy runtime dependencies** — Windows `.dll`s / Linux `.so`s from the build output
    (Python and pybind libraries excluded), the app icon, and `game_icon.ico`.
15. **Pack / codesign / manifest / installer** — per the Distribution options and, on
    macOS, the bundle re-sign followed by `.dmg` / `.pkg` / notarization.
16. **Finish** — report success (or clean up on cancel/failure).

## 12. Build scripts reference

The dialog invokes scripts in `Tools/BuildSystem/BuildGame/`. Each platform has a
`.bat` (Windows host) and `.sh` (Unix host) variant; `build_macos.bat` and
`build_ios.bat` are stubs that direct you to build on a Mac.

| Script | Platform |
| ------ | -------- |
| `build_windows.bat` / `.sh` | Windows (`.sh` cross-compiles via MinGW) |
| `build_linux.bat` / `.sh` | Linux |
| `build_android.bat` / `.sh` | Android |
| `build_web.bat` / `.sh` | Web (Emscripten) |
| `build_macos.bat`* / `.sh` | macOS (*`.bat` is a stub) |
| `build_ios.bat`* / `.sh` | iOS (*`.bat` is a stub) |
| `create_game_installer_windows.*` | NSIS installer |
| `create_game_installer_linux.*` | `.deb` installer |
| `create_game_installer_macos.sh` | macOS `.pkg` installer |
| `generate_keystore.bat` / `.sh` | Android release keystore (`keytool`) |

The scripts accept a consistent flag set assembled by the dialog. Common flags:

| Flag | Meaning |
| ---- | ------- |
| `--debug` / `--release` | Build configuration. |
| `--game-name "<name>"` | Product name. |
| `--icon "<path>"` | App icon (converted as needed). |
| `--backend <api>` | `OpenGL46`, `OpenGL33`, `Vulkan`, `OpenGLES32`, `WebGL2`, `MetalANGLE`, `MetalMoltenVK`. |
| `--arch <arch>` | `x64`/`x86` (desktop), or `x86_64`/`arm64` (macOS). |
| `--version "<v>"` / `--version-code <n>` | Version string / integer. |
| `--publisher "<name>"` | Publisher. |
| `--content-dir "<path>"` | Content to embed (raw or cooked). |
| `--start-scene "<path>"` | Initial scene (Web/Android/iOS). |
| `--clean` | Wipe the intermediate build directory first. |
| `--jobs <n>` | Parallel compile jobs. Defaults to an automatic value — see below. |
| `--target <name>` / `--with-editor` | Build a different CMake target / include the editor (desktop and Apple scripts). |
| `--no-plugins` / `--include-plugins` | Exclude / include `Plugins/` in the build. Forwarded to CMake as `-DICE_INCLUDE_PLUGINS=OFF\|ON`. Accepted by all six platform scripts. |
| `--no-mods` / `--include-mods` | Exclude / include `Mods/` in the build. Forwarded to CMake as `-DICE_INCLUDE_MODS=OFF\|ON`. Accepted by all six platform scripts. |

Plus platform-specific flags:

* **Android** — `--abi`, `--aab`, `--package-name`, `--orientation`, `--min-sdk`,
  `--target-sdk`, `--version-name`, `--permissions`, `--deep-link-scheme`,
  `--enable-ads`/`--admob-app-id`, `--enable-iap`, `--enable-play-games`/`--play-games-app-id`,
  `--enable-consent`, `--enable-review`, `--enable-notifications`, `--enable-bluetooth`,
  `--enable-firebase`, `--enable-saved-games`, `--keystore`, `--key-alias`,
  `--keystore-password`/`--keystore-pass-file`, `--key-password`/`--key-pass-file`.
* **Web** — `--renderer` (`auto`/`webgpu`/`webgl2`), `--web3`/`--no-web3`,
  `--main-loop`/`--no-main-loop`, `--pthreads`/`--no-pthreads`.
* **macOS** — `--deployment-target`, `--bundle-id`, `--category`, `--codesign-identity`,
  `--hardened-runtime`, `--entitlements`, `--notarize`, `--notary-profile`, `--dmg`.
* **iOS** — `--bundle-id`, `--team`, `--code-sign-identity`, `--provisioning-profile`,
  `--deployment-target`, `--destination`, `--export-method`, `--no-ipa`, `--orientation`,
  `--requires-fullscreen`, `--entitlements`, `--app-icon`, `--launch-screen`,
  `--enable-game-center`, `--enable-iap`, `--enable-cloudkit`, `--enable-push`,
  `--enable-multicast`, `--enable-ads`/`--admob-app-id`, `--uses-non-exempt-encryption`,
  `--deep-link-scheme`, `--local-network-usage`, `--usage-descriptions`,
  `--crash-report-url`.

### Parallel compile jobs

Every build script picks its own job count at startup, so a 24-core workstation is not
throttled and a small laptop is not driven into swap. The value is resolved in this order:

1. An explicit `--jobs <n>` on the command line.
2. The standard `CMAKE_BUILD_PARALLEL_LEVEL` environment variable, if it holds a positive
   integer. Useful for CI, where the runner already knows its own budget.
3. Automatic: `min(logical cores, total RAM in MB / 1536)`, never below `1`.

The RAM term is what protects small machines: compiling the engine is memory-hungry, so a
machine with many cores but little memory would otherwise thrash. Detection works on
Windows, Linux and macOS and honours container CPU/memory limits; if nothing can be
detected, the scripts fall back to `4` jobs.

Set `ICE_BUILD_MB_PER_JOB` to change the memory budget per job (default `1536`, minimum
`256`) — lower it to use more cores, raise it if a build ever runs out of memory.

Building through **CMake presets** rather than the scripts (`cmake --build --preset ...`,
Visual Studio, VS Code CMake Tools) uses the generator's own default instead; export
`CMAKE_BUILD_PARALLEL_LEVEL` to pin it.

**What a build script does** (Windows example): initializes the MSVC environment
(`vcvarsall`), locates **vcpkg**, requires **CMake** + **Ninja**, configures the engine
with `ICE_RUNTIME_BUILD=ON` (editor and Python off), Tracy on for Debug / off for Release,
and your content/version/backend options, builds the `IceBoxRuntime` target, and writes
the result to `out/gamebuild/Windows-<arch>-<config>/bin/Windows/IceBoxRuntime.exe`.

The render backend is patched into the build's `Config/Engine.json` as
`Rendering.RenderBackend`:

| Value | Backend | Written for |
| ----- | ------- | ----------- |
| `0` | OpenGL 4.6 | Windows / Linux (default) |
| `1` | WebGL 2.0 | Web (embedded, not patched) |
| `2` | OpenGL ES 3.2 | Android (embedded, not patched) |
| `3` | OpenGL 3.3 | Windows / Linux |
| `4` | Metal via ANGLE | macOS |
| `5` | Vulkan | Windows / Linux / Android |
| `6` | Metal via MoltenVK | macOS / iOS |

> The build scripts can also be run **manually** from a terminal with these flags, which
> is handy for CI. If the engine root is read-only, builds fall back to a per-user cache
> (`%LOCALAPPDATA%\IceBoxEngine\GameBuilds` / `~/.cache/IceBoxEngine/GameBuilds`), and the
> editor looks there too when collecting the output.

## 13. Build output locations

| Platform | Built runtime location (intermediate) |
| -------- | ------------------------------------- |
| Windows | `out/gamebuild/Windows-<arch>-<config>/bin/Windows/IceBoxRuntime.exe` |
| Linux | `out/gamebuild/Linux-<arch>-<config>/bin/Linux/IceBoxRuntime` |
| macOS | `out/gamebuild/macOS-<arch>-<config>/bin/macOS/IceBoxRuntime.app` |
| iOS | `out/gamebuild/iOS-arm64-<sdk>-<config>/bin/iOS/…` (`<sdk>` = `iphoneos` or `iphonesimulator`) |
| Web | `out/build/Web/bin/Web/<Name>-<version>-<config>-Web.html` |
| Android | `out/gamebuild/Android/project/app/build/outputs/apk(\|bundle)/<config>/app-<config>.apk\|.aab` |

The finished product is copied into **your chosen Output Path** under
`<Output>/<GameName>/`, alongside `game.json`, `LICENSE.txt`, `THIRD_PARTY_NOTICES.txt`,
`ThirdPartyLicenses/`, `Config/`, `Content/` (or `Content.icepak`), the enabled
`Plugins/`, `Mods/`, and `game_manifest.json` as applicable. It is renamed on the way:

| Platform | Final file name |
| -------- | --------------- |
| Windows / Linux | `<GameName>.exe` / `<GameName>` |
| macOS / iOS | `<GameName>.app`, or `<GameName>.ipa` when creating an IPA |
| Android | `<GameName>-<version>-<Config>-Android-<abi>.apk` (or `.aab`) |
| Web | `<GameName>-<version>-<Config>-Web.html` + companion files |

If the intermediate isn't found in `out/`, the per-user **GameBuilds** cache is checked.
Installers are written one level up, next to the `<GameName>` folder — see
[10.3](#103-installers).

## 14. Toolchain prerequisites

You build with the native toolchain for each target. Install these on the build host:

| Platform | Required tools |
| -------- | -------------- |
| **Windows** | Visual Studio (C++ workload / MSVC), **vcpkg**, **CMake 4.3+**, **Ninja**. ImageMagick optional (better PNG→ICO). NSIS for installers. |
| **Linux** | GCC/Clang, vcpkg, CMake 4.3+, Ninja. `dpkg-deb` for `.deb` installers. (From Windows: MinGW cross-compile, optionally via WSL for installers.) The CMake in the Debian/Ubuntu repositories — WSL2 images included — is normally older than 4.3; see the Linux / WSL2 section of `README.md`. |
| **Android** | **Android SDK** (`ANDROID_HOME`), **NDK**, **Gradle** (via the bundled wrapper), a **JDK** (Android Studio's JBR is auto-detected; `keytool` comes from it), vcpkg Android triplets. |
| **Web** | **Emscripten SDK** (`EMSDK`, `emcc` on PATH). |
| **macOS / iOS** | A **macOS** host with **Xcode** (and command-line tools — `pkgbuild`/`productbuild` for `.pkg`, `notarytool` for notarization) and vcpkg. iOS additionally needs a development team / signing assets for device builds & IPAs. |

The first build of a configuration also builds third-party dependencies through vcpkg,
so expect it to take longer than subsequent incremental builds.

---

# Part III — DLC

## 15. DLC packaging

The **DLC Packager** (**`Tools → DLC Packager`**, its own dialog) creates downloadable
content paks that mount on top of a base game at runtime.

| Field | Meaning |
| ----- | ------ |
| **DLC ID** | Unique identifier (e.g. `expansion01`). Required — it names every produced file. |
| **DLC Name** | Display name. Required. |
| **DLC Version** | Version of this DLC. |
| **Min Game Version** | Minimum base-game version required. |
| **Content Folder** | The folder of DLC assets to package; its path becomes the content **prefix** inside the pak (shown live under the picker). |
| **Output Path** | Where to write the DLC (defaults to the build output folder, `<Output Path>/<Game Name>`). |
| **Pack IcePak** | Package as a compressed `.icepak` (with **Max Pak Size** splitting and the same **zstd** compression level 1–22) — or ship **loose** files. |

The **Package DLC** button stays disabled until the ID, the name and an existing content
folder are all filled in.

Packaging writes into a **`DLC/`** subfolder of the output path: a descriptor
`DLC/<DLC ID>.json` (id, name, version, min game version, content prefix, file count,
total size, packed flag, file list) plus either `DLC/<DLC ID>.icepak` — split into
`<DLC ID>_0.icepak`, `<DLC ID>_1.icepak`, … when a size limit is set — or the loose files
under the content prefix.

At runtime the game scans `DLC/` at startup: each descriptor is read, the DLC is marked
**installed** when its pak (or, for loose DLC, its content folder) is actually present, and
every `.icepak` in `DLC/` is mounted **after** the base paks, in filename order. Because
[IceVFS](Assets-EN-DOC.md#24-the-virtual-file-system-icevfs) resolves later mounts first,
DLC content is added to — or can override — the base game's. **Min Game Version** is
recorded in the descriptor and exposed to scripts, so your game can refuse to enable DLC on
an incompatible build. The Lua side of this is
[`DLC.*`](LuaAPI-EN-DOC.md#42-dlc--downloadable-content).

---

## 16. Quick reference tables

### Platforms, outputs & renderers

| Platform | Output | Renderer options | Architectures |
| -------- | ------ | ---------------- | ------------- |
| **Windows** | `.exe` + DLLs | OpenGL 4.6 / 3.3 / Vulkan | x64, x86 |
| **Linux** | ELF binary | OpenGL 4.6 / 3.3 / Vulkan | x64, x86 |
| **Android** | `.apk` / `.aab` | OpenGL ES 3.2 / Vulkan | arm64-v8a, armeabi-v7a, x86_64, x86 |
| **Web** | `.html`+`.wasm`+data | WebGPU / WebGL 2.0 (Auto) | wasm |
| **macOS** | `.app` (+`.dmg`, `.pkg`) | Metal (ANGLE) / Metal (MoltenVK) | x86_64, arm64 |
| **iOS** | `.ipa` / `.app` | Metal (MoltenVK) | arm64 |

### Cook formats

| Media | Formats | Not available on |
| ----- | ------- | ---------------- |
| Texture | PassThrough · WebP · WebP Lossless · KTX2 UASTC · KTX2 ETC1S | WebP: iOS, Web · KTX2: everything except desktop x86/x64 |
| Audio | PassThrough · Ogg Vorbis · Opus | — |
| Video | PassThrough · VP9 (WebM) | VP9: iOS |
| Font | PassThrough · Subset · Auto-subset | — |
| JSON | PassThrough · Minify | — |

### Distribution options (desktop)

| Option | Output |
| ------ | ------ |
| Generate Manifest | `game_manifest.json` (SHA-256 per file) |
| Pack Content | `Content.icepak` (zstd 1–22, optional split) |
| Create Installer | NSIS `.exe` (Win) / `.deb` (Linux) |
| Include Plugins / Include Mods | *(all platforms)* Whether enabled `Plugins/` and `Mods/` are packaged at all |
| macOS distribution | `.dmg` (drag-install) and/or `.pkg` installer, optionally notarized |

### Profiler surfaces

| Surface | Where | Purpose |
| ------- | ----- | ------- |
| **Statistics** | Editor — `Window → Stats` | Live scene/renderer counts, atlas, physics, shadows, debug-draw toggles. |
| **Profiler** (Advanced) | Editor — `Tools → Profiler (Tracy)` | Traces, CPU scopes & flame timeline, frame diff, trace compare, memory/GPU graphs, render passes & render-graph metrics. |
| **Profiler overlay** | Debug runtime | On-screen FPS/CPU/RAM/VRAM/GPU, engine stats, hot scopes, render passes. |
| **Network profiler** | Debug runtime + editor Network Manager | Ping, bandwidth, packet counts, per-message-type breakdown. |
| **DebugScreen** | Editor + runtime | On-screen & world-space debug text in the running game. |
| **Developer Console** | Editor + runtime | Live command/CVar console with completion & history. |
| **Tracy** | Desktop Editor + Debug runtime | External nanosecond-level, multi-thread frame profiler. |

---

## 17. FAQ & troubleshooting

**The Build dialog says the build script wasn't found.**
Build scripts live in `Tools/BuildSystem/BuildGame/`. Make sure you're building from a
complete engine checkout (the dialog locates the engine root automatically).

**macOS/iOS build does nothing on Windows.**
By design — the `.bat` scripts are stubs. Apple platforms require a macOS host with Xcode;
run the `.sh` scripts there.

**My first build is very slow.**
The first configuration builds all third-party libraries via vcpkg. Subsequent builds are
incremental and much faster.

**Where did my build go?**
Into `<Output Path>/<Game Name>/`. The intermediate build is under `out/gamebuild/…` (or
the per-user `GameBuilds` cache if the engine folder is read-only). If you enabled
**Create Installer**, that folder is gone on purpose — the installer sits one level up.

**WebP/KTX2/VP9 options are greyed out or forced to PassThrough.**
Those formats aren't supported by every runtime: WebP is unavailable on iOS/Web, VP9 on
iOS, and KTX2 needs a desktop x86/x64 runtime (it uses the Basis Universal transcoder). The
dialog enforces valid combinations per platform but keeps your preference for targets that
do support it.

**How do I make a smaller build?**
Enable **Cook Assets** (WebP/KTX2 textures, Vorbis/Opus audio, VP9 video, font
subsetting, JSON minify) and **Pack Content** with a higher zstd level. Author good
sidecar inputs (texture max size, font glyph ranges).

**GPU times show as unavailable.**
GPU timing needs driver timer-query support. The GPU tab names the backend timer it tried
to use. CPU scopes, memory and counts still work.

**CPU temperature shows "unavailable" on Windows.**
Windows exposes no unprivileged CPU thermal API. Install LibreHardwareMonitor, run it as
Administrator, enable *Options → Remote Web Server* (port 8085) and restart the engine —
see [2.4](#24-temperature-sensors).

**My traces disappeared / I want to archive them.**
They are plain JSON files in `Tools/Helpers/Profiler/` under the engine's writable path.
The panel keeps the 20 most recent; copy older ones out of that folder before they age out,
and remember that **Delete Trace** deletes the file too.

**There is no Chrome-trace export button in the Profiler panel.**
Correct — it is a scripting call. Use `SaveChromeTrace()` from Lua; see
[4.9](#49-where-traces-live--chrome-trace-export).

**How do I profile the actual shipped game?**
Build **Debug**. On desktop attach the external **Tracy** app. On any platform you can
toggle the built-in [profiler overlay](#53-the-runtime-profiler-overlay), record traces
from Lua and export a Chrome trace, and/or surface metrics via **DebugScreen**.

**Can I run builds from CI / the command line?**
Yes — call the `build_<platform>` scripts directly with the flags in
[Section 12](#12-build-scripts-reference); `--jobs` and `--clean` are useful there.

---

## Headless (dedicated server)

Any normal game build doubles as a dedicated server. Start the runtime with `--headless`
(`-headless` also works):

```bash
IceBoxRuntime --headless
```

No window is created, no graphics context is made, and the audio device is never opened.
The engine selects the **Null render backend**: every RHI call becomes a no-op and resources
get placeholder handles, so nothing in the engine needs a special code path. `Render()` is
skipped entirely; everything else — scripts, physics, animation, skeletons and ragdoll bone
bodies, AI, networking — runs exactly as it does with a window. Server-authoritative hit
detection and lag compensation therefore behave identically to a listen server.

What this means in practice:

- The process never suspends on focus loss, so a server can run unattended indefinitely.
- Particle systems still advance, so FX lifetimes expire instead of accumulating.
- The frame limiter still applies, so the server paces itself instead of burning a core.
- Deterministic UUIDs and the shared RNG make rollback sessions reproducible on the server.
- CPU scopes, RAM and the scene counters keep being collected, so traces and the
  `NetworkProfiler` still work; only GPU timing has nothing to measure.

Requirements: none beyond the normal build — no GPU, no display server, no X11/Wayland
session on Linux. It runs in a container.

---

<sub>© IceBoxCrew Studio. All rights reserved. See [`LICENSE.txt`](../../LICENSE.txt) for full terms.</sub>
