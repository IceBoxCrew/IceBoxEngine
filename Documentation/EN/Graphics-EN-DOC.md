# 🎨 IceBox Engine — Graphics, Rendering & Physics

## Full documentation in English

### Actual for PR-0.9.1 Version

> **IceBox Engine** renders 2D worlds through a modern, backend-agnostic graphics
> pipeline: a thin **RHI** (Render Hardware Interface) sits over **eleven** renderers —
> OpenGL 4.6, OpenGL 3.3, OpenGL ES 3.2, WebGL 2.0, Vulkan, Direct3D 12, Metal,
> Metal (ANGLE), Metal (MoltenVK), WebGPU, plus a **Null** renderer for headless servers —
> across **six**
> platforms; a **render graph** schedules the frame in validated passes; a
> heavily-optimized **2D batch renderer** draws the scene; a **screen-space
> post-processing** stack finishes the image; a **2D lighting** system with
> **ray-cast shadows** and optional **ray-traced global illumination** lights it; and
> a **Box2D v3** physics simulation moves it.
>
> This document explains **how the graphics and physics work** — the renderers, the
> backends, lighting, shadows, global illumination, post-processing, and the physics
> engine. It is the companion to the
> [Editor & Interface](Editor-EN-DOC.md) document (which covers the UI that drives
> these systems) and the [Assets](Assets-EN-DOC.md) document (which covers materials,
> sprites, tilemaps, FX, views and the other renderable assets).
>
> Scripting hooks for these systems are out of scope here — see the
> [Lua API](LuaAPI-EN-DOC.md) and [Python API](PythonAPI-EN-DOC.md). The advanced
> frame profiler and shipping pipeline are in
> [Profiling & Building Games](Profiling-And-Building-EN-DOC.md).

---

## 📑 Table of Contents

1. [Overview & philosophy](#1-overview--philosophy)
2. [Rendering architecture](#2-rendering-architecture)
   - 2.1 [The RHI (Render Hardware Interface)](#21-the-rhi-render-hardware-interface)
   - 2.2 [Backends & platforms](#22-backends--platforms)
   - 2.3 [RHI capabilities](#23-rhi-capabilities)
   - 2.4 [The shader pipeline & caches](#24-the-shader-pipeline--caches)
3. [The render graph](#3-the-render-graph)
   - 3.1 [Passes & resources](#31-passes--resources)
   - 3.2 [The frame, step by step](#32-the-frame-step-by-step)
   - 3.3 [The scene-drawing chain](#33-the-scene-drawing-chain)
4. [The 2D batch renderer](#4-the-2d-batch-renderer)
   - 4.1 [Batching & draw submission](#41-batching--draw-submission)
   - 4.2 [Instancing, GPU culling & sorting](#42-instancing-gpu-culling--sorting)
   - 4.3 [Blend & shading modes](#43-blend--shading-modes)
   - 4.4 [Buffer streaming](#44-buffer-streaming)
   - 4.5 [Meshes, blur & special draws](#45-meshes-blur--special-draws)
   - 4.6 [Vertex effects](#46-vertex-effects)
   - 4.7 [Stencil masking](#47-stencil-masking)
5. [Materials & shaders](#5-materials--shaders)
6. [Lighting](#6-lighting)
   - 6.1 [Lighting modes](#61-lighting-modes)
   - 6.2 [Light types](#62-light-types)
   - 6.3 [Ambient & light cookies](#63-ambient--light-cookies)
   - 6.4 [The light budget & the light UBO](#64-the-light-budget--the-light-ubo)
   - 6.5 [Where lighting and shadow settings come from](#65-where-lighting-and-shadow-settings-come-from)
7. [2D shadows](#7-2d-shadows)
   - 7.1 [How ray-cast shadows work](#71-how-ray-cast-shadows-work)
   - 7.2 [Shadow casters](#72-shadow-casters)
   - 7.3 [Quality, softness & bias](#73-quality-softness--bias)
   - 7.4 [Directional shadows](#74-directional-shadows)
   - 7.5 [GPU compute & caching](#75-gpu-compute--caching)
8. [Ray-traced global illumination](#8-ray-traced-global-illumination)
9. [Post-processing](#9-post-processing)
   - 9.1 [The G-buffer & flow](#91-the-g-buffer--flow)
   - 9.2 [The effect stack](#92-the-effect-stack)
   - 9.3 [Effect order](#93-effect-order)
   - 9.4 [Post-process volumes](#94-post-process-volumes)
10. [Image quality: AA, HDR & scaling](#10-image-quality-aa-hdr--scaling)
    - 10.1 [Anti-aliasing & render scale](#101-anti-aliasing--render-scale)
    - 10.2 [Upscaling: FSR & NIS](#102-upscaling-fsr--nis)
    - 10.3 [HDR10 output](#103-hdr10-output)
    - 10.4 [Pacing: VSync, low latency & adaptive quality](#104-pacing-vsync-low-latency--adaptive-quality)
11. [Cameras & viewports](#11-cameras--viewports)
12. [Debug visualization](#12-debug-visualization)
13. [Physics](#13-physics)
    - 13.1 [The simulation loop](#131-the-simulation-loop)
    - 13.2 [Bodies](#132-bodies)
    - 13.3 [Colliders & materials](#133-colliders--materials)
    - 13.4 [Collision filtering (groups)](#134-collision-filtering-groups)
    - 13.5 [Contacts, sensors & events](#135-contacts-sensors--events)
    - 13.6 [One-way platforms](#136-one-way-platforms)
    - 13.7 [Joints, queries & destruction](#137-joints-queries--destruction)
    - 13.8 [World settings reference](#138-world-settings-reference)
14. [Quick reference tables](#14-quick-reference-tables)
    - 14.1 [Render backends](#141-render-backends)
    - 14.2 [Platforms & selectable renderers](#142-platforms--selectable-renderers)
    - 14.3 [Render passes](#143-render-passes)
    - 14.4 [Lighting & shadow quality](#144-lighting--shadow-quality)
    - 14.5 [Light limits](#145-light-limits)
    - 14.6 [Blend × shading modes](#146-blend--shading-modes)
    - 14.7 [Physics body types](#147-physics-body-types)
    - 14.8 [Joint types](#148-joint-types)
15. [FAQ & troubleshooting](#15-faq--troubleshooting)

---

## 1. Overview & philosophy

IceBox is a **2D** engine, but its renderer is built like a modern 3D one:

| Principle | What it means |
| --------- | ------------- |
| **Backend-agnostic** | All rendering goes through an **RHI** so the same engine runs on OpenGL, OpenGL ES, WebGL, Vulkan, Direct3D 12, WebGPU and native Metal without per-feature branching in game code. |
| **Pass-based** | A **render graph** declares passes and the resources they read/write; it validates, (re)orders and profiles them every frame. |
| **Batched & GPU-driven** | The 2D renderer batches thousands of sprites per draw and can push culling, sorting and draw generation onto the GPU. |
| **Forward-lit, G-buffer-assisted** | Lights and 2D shadows are evaluated **in the sprite shaders** (forward), while a thin **G-buffer** (normal/roughness/metallic + AO/emissive + depth) is written alongside so screen-space effects — SSR, GI, godrays, volumetric fog — have the data they need. The G-buffer attachments are only bound when an active effect actually needs them. |
| **Data-driven look** | The visual result is authored as **assets** — materials (node-graph shaders), view/post-process volumes, lights and FX — not hard-coded. |
| **Deterministic physics** | Box2D v3 runs on a **fixed timestep** with render interpolation, so simulation is stable regardless of framerate. |
| **Headless-capable** | A **Null** RHI backend satisfies every GPU call without a device, so a dedicated server can run the same build with no window, no context and no audio. |

The pipeline at the highest level:

```
        ┌─────────────── Scene render graph ───────────────┐      ┌──── Post-process graph ────┐
Scene → │ Shadow pass → FX pass → Geometry pass (G-buffer) │  →   │ Post-process → Composite   │ → Screen
        └──────────────────────────────────────────────────┘      └────────────────────────────┘
                       ↑ lights, 2D shadows, GI                          ↑ bloom, tonemap, … 
```

Filling `SceneColor` is itself an ordered chain — background UI, editor grid, tilemaps,
flipbooks, skeletons and sprites inside the **Geometry** pass, then ray-traced GI,
particles, fog of war, foreground UI and debug overlays after it — described in
[3.3](#33-the-scene-drawing-chain).

---

## 2. Rendering architecture

### 2.1 The RHI (Render Hardware Interface)

Every GPU operation in the engine goes through the **RHI** — a small abstraction
with two halves:

* **Device** — creates and destroys GPU resources (textures, buffers, vertex
  arrays, framebuffers, shader programs, queries).
* **Context** — issues commands (bind, draw, clear, blit, dispatch compute, set
  state, read pixels, …).

Game and engine code never call OpenGL/Vulkan/Direct3D/Metal/etc. directly; they call the RHI,
which dispatches to the active backend. The backend is chosen at startup, and on
initialization the device's driver string is recorded into the **crash reporter**,
so a crash report always names the renderer, GPU and driver that produced it.

The RHI also reports what the current device can do — compute shaders, persistent
mapping, bindless textures, multi-draw indirect, GPU timers, SPIR-V, program-binary
caching, compressed texture formats — plus limits such as maximum texture size and
total VRAM. Every optional renderer path is gated on one of these capabilities.

### 2.2 Backends & platforms

The RHI has **six** implementation families and **eleven** concrete backends
(`RenderBackend`), covering every shipping platform:

| Family | Backend | Reported as | Shader dialect |
| ------ | ------- | ----------- | -------------- |
| **GL** | `OpenGL46` | OpenGL 4.6 | `#version 460 core` |
| **GL** | `OpenGL33` | OpenGL 3.3 | `#version 330 core` |
| **GL** | `OpenGLES32` | OpenGL ES 3.2 | `#version 320 es` |
| **GL** | `WebGL2` | WebGL 2.0 | `#version 300 es` |
| **GL** | `MetalANGLE` | Metal (ANGLE / OpenGL ES 3.0) | `#version 300 es` |
| **VK** | `Vulkan` | Vulkan 1.4 (1.1 minimum) | GLSL → SPIR-V |
| **VK** | `MetalMoltenVK` | Metal (MoltenVK / Vulkan 1.x) | GLSL → SPIR-V |
| **D3D12** | `D3D12` | Direct3D 12 (feature level 11_0 minimum) | GLSL → SPIR-V → HLSL → DXIL (SM 6.x) or DXBC (SM 5.1) |
| **Metal** | `Metal` | Metal (native) | GLSL → SPIR-V → MSL (2.3–3.0) |
| **WGPU** | `WebGPU` | WebGPU | GLSL → WGSL |
| **Null** | `Null` | Null (headless) | — |

The **six** target platforms and the renderers each one offers:

| Platform | Selectable renderers | Fallback chain (highest → lowest) |
| -------- | -------------------- | -------------------------------- |
| **Windows** | OpenGL 4.6, OpenGL 3.3, Vulkan, Direct3D 12 | Direct3D 12 → Vulkan 1.1-1.4 → OpenGL 4.6 → OpenGL 3.3 |
| **Linux** | OpenGL 4.6, OpenGL 3.3, Vulkan | Vulkan 1.1-1.4 → OpenGL 4.6 → OpenGL 3.3 |
| **Android** | OpenGL ES 3.2, Vulkan | Vulkan 1.1-1.4 → OpenGL ES 3.2 → OpenGL ES 3.0 |
| **Web** | WebGPU or WebGL 2.0 | WebGPU → WebGL 2.0 |
| **macOS** | Metal (native), Metal (ANGLE over GLES) or Metal (MoltenVK over Vulkan) | Metal (native) → Metal (MoltenVK) → Metal (ANGLE) |
| **iOS** | Metal (native) or Metal (MoltenVK over Vulkan) | Metal (native) → Metal (MoltenVK) |

Every chain runs top-down: the entry that is asked for is tried first, and each failure
steps exactly one rung down, never sideways and never back up.

**First-run auto-probe.** The Launcher, the Updater and the Editor do not start on a
hard-coded default. The first time one of them runs against a configuration that has no
resolved renderer yet, it walks the chain above from the top and probes each candidate
without creating a window — DXGI adapter enumeration plus `D3D12CreateDevice` for
Direct3D 12, a throwaway instance plus the device baseline check for Vulkan, and device
plus command-queue creation for Metal. The first candidate that answers is the one the
tool runs on, and it is written to `Config/Engine.json` as `Rendering.RenderBackend`
together with `Rendering.RenderBackendAutoProbed: true`, so every later start goes
straight to it. The Launcher and the Updater write into the install folder's
`Config/Engine.json` when it is writable and into the per-user one
(`%APPDATA%\IceBoxEngine\Config\Engine.json` / `~/.config/IceBoxEngine/Config/Engine.json`)
when it is not; both are read back in that order, so a value placed in the install
folder by hand always wins. The same probe runs when a **new project is created**, so the project's
`Config/Engine.json` already names the best renderer that machine has before the editor
opens it for the first time. Picking a renderer by hand in
[Preferences → Rendering](Editor-EN-DOC.md#105-rendering), in `Settings.SetRenderer()`
or in the build dialog replaces that recorded value and is never probed over again.
The Launcher and the Updater show the renderer they ended up on — and the GPU behind
it — in their **Settings** tab; the editor shows the same two lines under
**Preferences → Rendering**, next to the renderer it will use after the next restart.

Games never auto-probe: a build starts on the renderer the build dialog wrote into its
`Config/Engine.json` and falls back from there, and `Settings.SetRenderer()` can still
move it anywhere along the chain at runtime (applied on the next launch).

The editor's active backend is chosen in
[Preferences → Rendering](Editor-EN-DOC.md#105-rendering); the build target's
renderer is chosen per platform in **Build Game…** (see
[Profiling & Building](Profiling-And-Building-EN-DOC.md)).

**Selection & fallback.** The requested backend is never taken on faith:

* **Vulkan / MoltenVK** — if Vulkan was not compiled in, or a pre-flight device probe
  finds no usable device, the engine falls back to OpenGL 4.6 (to Metal/ANGLE on
  macOS) *before* creating the window. iOS is the exception: it renders only through
  MoltenVK, so the probe is treated as advisory there — Metal is guaranteed — and no
  fallback backend exists. MoltenVK itself is searched for in the app
  bundle, the `Frameworks` directory, `@rpath`, and finally the SDL default loader.
* **Direct3D 12** (Windows only) — if Direct3D 12 was not compiled in, or the pre-flight
  adapter probe finds no hardware device with feature level 11_0, the engine switches to
  **Vulkan** (when Vulkan itself probes clean) and otherwise to **OpenGL 4.6**, *before*
  the window is created. If device or swapchain creation still fails after the window
  exists, the window is destroyed and recreated for the fallback backend, and the new
  choice is written back to `Config/Engine.json` so the next launch starts on it
  directly.
* **Metal** (macOS / iOS) — if the native Metal backend was not compiled in, or the
  pre-flight device probe finds no Metal device that can create a command queue, the
  engine switches to **Metal (MoltenVK)** (when Vulkan itself probes clean) and, on
  macOS, otherwise to **Metal (ANGLE)**, *before* the window is created. If device or
  layer creation still fails after the window exists, the window is destroyed and
  recreated for the fallback backend and the new choice is written back to
  `Config/Engine.json`, so the next launch starts on it directly. The ANGLE and
  MoltenVK translation layers are untouched by the native backend and remain fully
  selectable.
* **WebGPU** — if the device is not ready, the engine logs a warning and drops to
  WebGL 2.0.
* **Editor viewport** — selecting a GLES/WebGL backend inside the editor on a non-Apple
  desktop substitutes **OpenGL 3.3**, which is the closest GLES-compatible preview a
  desktop driver can give you. The build itself still uses the backend you picked.

**The Null backend (headless).** Launching the runtime with `--headless` (or
`-headless`) switches the active backend to `Null` and skips window, graphics context
and audio-device creation entirely; SDL is initialized for events only. Both `RHIDeviceNull`
and `RHIContextNull` accept and account for every call — handles are still handed out
and destroyed so resource bookkeeping stays valid — but nothing reaches a GPU. The
frame loop replaces `Render()` with an FX-only update, so particle simulation, physics,
scripts and networking keep ticking at full fidelity. This is what a **dedicated
network server** runs: the same executable and the same game code, without a renderer.

> **Capability gating.** Higher-end features (compute-shader shadows/GI/particles,
> hardware ray tracing, bindless textures, multi-draw-indirect, GPU sorting, persistent
> mapping, float render targets, linear filtering of float textures) are detected at
> runtime. Where a backend or device lacks them, the engine falls back to a portable
> path automatically — e.g. CPU ray-cast shadows when compute is unavailable, `RGBA8`
> render targets when `EXT_color_buffer_float` is missing on GLES.

Backend differences the renderer compensates for automatically:

| Difference | Backends affected | How it is handled |
| ---------- | ----------------- | ----------------- |
| **Framebuffer origin** | Vulkan, MoltenVK, Direct3D 12, Metal and WebGPU are top-left; GL family is bottom-left | The V coordinate of full-screen blits is flipped through `FramebufferTopV()` / `FramebufferBottomV()`. |
| **Clip-space depth** | Vulkan, MoltenVK, Direct3D 12 and Metal use 0…1 depth; GL uses −1…1 | The vertex shader is wrapped at compile time so `gl_Position.z` is remapped once, keeping every projection matrix in the engine GL-style. |
| **Explicit UBO binding** | Only GL 4.6, Vulkan, MoltenVK, Direct3D 12, Metal and WebGPU support `layout(binding=…)` on uniform blocks | Other backends bind the light UBO by block index at link time. |
| **Dynamic texture indexing** | WebGL 2.0, GLES 3.2, GL 3.3, Metal/ANGLE and WebGPU cannot index a sampler array with a varying | A generated branched `switch` samples the right slot instead. |
| **Direct State Access** | GL 4.6 only | `RHIContextGL` is constructed in DSA mode on 4.6 and classic bind-then-modify mode elsewhere. |
| **HDR float targets** | Native GLES/WebGL need `EXT_color_buffer_float` | Scene and ping-pong targets drop to `RGBA8` when it is missing. |
| **Sampler LOD bias** | Metal samplers have no LOD-bias parameter | A positive per-texture LOD bias is applied through the sampler's LOD minimum; a negative bias is clamped to 0 and logged once at startup. Every other backend applies it natively. |

### 2.3 RHI capabilities

The RHI exposes a modern feature set so the renderer can be GPU-driven:

| Area | What the RHI provides |
| ---- | --------------------- |
| **Texture formats** | `R8`/`RG8`/`RGB8`/`RGBA8`, sRGB variants (`SRGB8`, `SRGB8_Alpha8`), float formats (`R16F`, `RGBA16F`, `R32F`, `RG32F`, `RGBA32F`), `RGB10_A2` (HDR10 output), `Depth24` / `Depth32F` / `Depth24Stencil8`, and **GPU-compressed** formats — ETC2 (RGB8/RGBA8 + sRGB), ASTC 4×4/5×5/6×6/8×8 (+ sRGB), and BC1/BC3/BC4/BC5/BC7 (+ sRGB). |
| **Texture types** | 2D and 2D-array, with per-channel swizzles, all filter modes (including every mipmap combination), wrap modes (repeat / clamp-to-edge / mirrored / clamp-to-border), border color, **anisotropy**, **LOD bias**, mipmap generation, `ClearTexImage`, and **texture views** onto a mip/layer range of an existing texture. |
| **Buffers** | Vertex, index, uniform (UBO), shader-storage (SSBO), draw-indirect and dispatch-indirect buffers, with static/dynamic/stream usage, immutable storage, ranged mapping and **persistent + coherent** mapping. |
| **Compute** | Compute dispatch (direct and indirect), memory barriers (SSBO / texture-fetch / image / command) and image load/store — used by shadows, GI, particle simulation, GPU culling and sorting. |
| **Draw** | Indexed, instanced, indirect and **multi-draw-indirect** draws (`RHIDrawElementsIndirectCommand`), across triangle/line/line-loop/line-strip primitives. |
| **State** | Blend (including separate RGB/alpha factors), depth test + mask + range, full stencil func/op/mask, scissor, multisample and alpha-to-coverage, color mask. |
| **Queries & sync** | Timestamp and time-elapsed queries (GPU timing) and fence sync objects (buffer streaming and the low-latency mode). |
| **Framebuffers** | Up to four color attachments plus depth/stencil, renderbuffers for **MSAA** storage, draw-buffer and read-buffer selection, blits (color/depth, nearest/linear) and attachment **invalidation**. |
| **Shaders** | Vertex+fragment and compute program creation, uniform and uniform-block introspection, plus optional **program-binary** get/create for the on-disk shader cache. |
| **Ray tracing** | `BuildRaytracingScene()` builds a bottom/top-level acceleration structure from a triangle soup; implemented by the Vulkan, Direct3D 12 and Metal backends, a no-op elsewhere. |

The Vulkan backend enables what the device offers on top of core 1.1: dynamic
rendering, synchronization2, timeline semaphores, buffer device address, descriptor
indexing (bindless), host query reset, `VK_EXT_hdr_metadata`, and — when present —
`VK_KHR_ray_query` + `VK_KHR_acceleration_structure` + `VK_KHR_deferred_host_operations`
for [ray-traced GI](#8-ray-traced-global-illumination). It also carries a
`VK_KHR_portability_subset` path for MoltenVK. Internally it keeps a pipeline cache, a
descriptor allocator, a deferred deletion queue and a render-pass cache so state changes
do not stall the frame.

The **Direct3D 12** backend is the Windows-native peer of the Vulkan one and exposes the
same feature set. It picks the highest-performance hardware adapter through
`IDXGIFactory6`, runs a flip-model swapchain with tearing support, and reports its
capabilities from `D3D12_FEATURE_D3D12_OPTIONS*`: resource-binding tier, highest shader
model, **DirectX Raytracing 1.1** (`D3D12_RAYTRACING_TIER_1_1` + shader model 6.5) for
[ray-traced GI](#8-ray-traced-global-illumination), HDR10 output detection through
`IDXGIOutput6`, and FSR/NIS upscaling support. Internally it keeps a pipeline-state
cache, CPU descriptor heaps for RTV/DSV/SRV plus a per-frame shader-visible descriptor
ring and a content-hashed sampler heap, a deferred deletion queue that retires every
resource only after the frame that used it has finished on the GPU, and a recycling
upload-buffer pool. Storage buffers that a shader writes are promoted to a device-local
resource on first UAV use, so compute output, GPU culling, GPU sorting and
`ExecuteIndirect` draws all behave exactly as they do on Vulkan.

The **Metal** backend is the Apple-native peer of the Vulkan and Direct3D 12 ones and
exposes the same feature set on **macOS and iOS**. It renders straight into a
`CAMetalLayer` attached to the SDL window — no ANGLE, no MoltenVK, no translation layer
in between — picking the highest-performance `MTLDevice` on multi-GPU Macs and falling
back to the system default device elsewhere. Capabilities are read from the GPU family
(`MTLGPUFamilyApple1…9` / `MTLGPUFamilyMac2`): maximum texture size, argument limits,
border-colour clamping, BC/ASTC/ETC compressed-texture support, unified memory, and
**hardware ray tracing** (`MTLDevice.supportsRaytracing` + MSL 2.4) for
[ray-traced GI](#8-ray-traced-global-illumination), where the engine builds a primitive
and an instance `MTLAccelerationStructure` per frame slot and binds the top-level one to
the compute encoder. Internally it keeps a render/compute pipeline-state cache and a
separate depth-stencil-state cache, a deferred deletion queue that retires every
resource only after the frame that used it has finished on the GPU, a recycling
upload-buffer pool, a triple-buffered staging ring for texture uploads, and a per-frame
uniform ring buffer. MSAA is resolved by Metal itself through the render pass's
`resolveTexture`, so multisampled targets never round-trip through memory, and buffers
use shared storage on unified-memory devices and managed storage (with explicit
`didModifyRange` flushes) on discrete Macs. Compute output, GPU culling, GPU sorting and
indirect draws all behave exactly as they do on Vulkan and Direct3D 12.

### 2.4 The shader pipeline & caches

All engine shaders are authored **once, in GLSL**, and specialized per backend at
startup:

* **Version & precision header** is chosen from the active backend (see the table in
  [2.2](#22-backends--platforms)); GLES/WebGL dialects get explicit `highp` precision
  qualifiers for every sampler type.
* **Generated defines** inject the live configuration into the source —
  `MAX_TEXTURES` (the backend's texture-slot count), `MAX_LIGHTS_CAP` (the hard cap of
  128 lights in the UBO), `MAX_LIGHTS_ACTIVE` (the configured **Max Point Lights**) and
  `MAX_PCF_HALF_SAMPLES` (4).
* **Swappable function bodies** — texture sampling has four variants (GL 4.6 array
  indexing, bindless handles, SPIR-V backends (Vulkan / MoltenVK / Direct3D 12 / Metal),
  and generated branched GLES sampling), and the
  lighting function has UBO-binding and no-UBO-binding variants. Fragment shaders that
  participate in the G-buffer append the extra `GBufferNormal` / `GBufferMaterial`
  outputs.
* **Vulkan / MoltenVK** compile that GLSL to **SPIR-V** with `shaderc` and reflect it
  with **SPIRV-Cross** to build descriptor layouts automatically.
* **Direct3D 12** reuses exactly the same GLSL → SPIR-V step and the same SPIRV-Cross
  reflection, then cross-compiles the SPIR-V to **HLSL** and compiles that to **DXIL**
  with `dxcompiler.dll` (shader model 6.0, or 6.5 when the adapter supports inline ray
  tracing). If the DirectX Shader Compiler is not present next to the executable, it
  falls back to the always-available FXC path (`d3dcompiler`, shader model 5.1) — the
  renderer still works, only hardware ray tracing stays disabled. Reflection drives an
  automatically generated **root signature**: one descriptor table per register space for
  CBV/SRV/UAV and one for samplers.
* **Metal** reuses exactly the same GLSL → SPIR-V step and the same SPIRV-Cross
  reflection, then cross-compiles the SPIR-V to **Metal Shading Language** and compiles
  that with `newLibraryWithSource:` into an `MTLLibrary` (MSL 3.0 on macOS 13 / iOS 16,
  MSL 2.4 on macOS 12 / iOS 15, MSL 2.3 below that). Reflection assigns every uniform
  block, storage buffer, texture and sampler an explicit MSL argument index that is
  shared by the vertex, fragment and compute stages, so a single binding table drives
  all of them; vertex streams live in a reserved slot range above the resource slots.
* **WebGPU** cross-compiles the same GLSL to **WGSL**.

Three caches keep the cost off the startup path:

| Cache | What it stores | Where |
| ----- | -------------- | ----- |
| **ShaderCache** | Linked **program binaries**, keyed by a 128-bit FNV-1a hash of the shader source and tagged with a driver fingerprint so a GPU-driver update invalidates the whole cache automatically. Hit/miss counts are tracked. | `Saved/Cache/Shaders` |
| **SPIR-V cache** | Compiled SPIR-V modules keyed by source + stage, so the `shaderc` compile only happens once per shader per machine. | Alongside the shader cache |
| **Vulkan pipeline cache** | Driver-side pipeline objects, so pipeline creation for a known state combination is near-free. | In-process, per Vulkan device |
| **Direct3D 12 bytecode cache** | Compiled DXIL/DXBC blobs keyed by the generated HLSL, entry point and target profile, so the SPIR-V → HLSL → DXC/FXC chain runs once per shader per machine. Pipeline-state objects are then cached in-process by their full state key. | Alongside the shader cache |
| **Metal MSL cache** | Generated Metal Shading Language, keyed by the whole program (both stages plus the resolved argument-index assignment and the MSL version), so the SPIR-V → MSL translation runs once per shader per machine. Render, compute and depth-stencil states are then cached in-process by their full state key. | Alongside the shader cache |

**Project Prewarm** ([Preferences → Engine](Editor-EN-DOC.md#101-engine)) walks the
project up-front and warms sprites, flipbooks, skeletons, materials, material
instances, textures, FX, tilemaps, tilesets, widgets, fonts, animations, sounds and
views on a time budget per frame, so the first frame of gameplay does not pay for a
shader compile or a texture upload.

---

## 3. The render graph

The renderer is organized as a **render graph** (`RenderGraph`): a list of **passes**
that each declare which named **resources** they read and write. Every frame the
graph is *compiled* (validated and ordered) and then *executed*.

What the graph gives you:

* **Automatic ordering** — compilation is a topological sort: a pass runs as soon as
  every resource it reads is available (either produced by an already-scheduled pass or
  **imported** into the graph). The number of passes that ended up in a different slot
  than they were added in is reported as *reordered passes*.
* **Validation** — every resource a pass reads must be produced by an earlier pass or
  imported. Missing producers are reported by name, together with the pass that wanted
  them. **Strict validation** is on in debug builds and off in release: in strict mode
  a validation error is logged as an error and the graph **skips execution entirely**
  rather than drawing something undefined; in non-strict mode it is a warning and the
  offending passes run last.
* **Skipping** — a pass whose `IsEnabled()` returns false (e.g. shadows when no
  light casts them) is skipped, and the skip is counted.
* **Profiling** — per-graph metrics (compile time, execute time, pass count, skipped
  passes, reordered passes, validation-error count, strict flag) are published to a
  global snapshot every frame, and each pass is wrapped in a scope that records **CPU
  and GPU time** — see the **Render Passes** tab of the
  [Profiler](Profiling-And-Building-EN-DOC.md#47-render-passes-tab). The pass profiler
  keeps a three-frame ring of GPU timer queries (so results are read back without
  stalling), tracks nesting depth, and handles up to 64 passes per frame.
* **A frame arena** — passes and their transient per-frame data are allocated from a
  bump allocator that is reset each frame (no per-frame heap churn). Non-trivial types
  are destructor-tracked, the arena grows geometrically, and it shrinks itself back
  down after 120 consecutive frames of using less than a quarter of its capacity.
* **A transient resource pool** — `RenderResourcePool` hands out and recycles
  intermediate textures and framebuffers by descriptor match, and releases anything
  left idle for more than a few frames, so effects that come and go do not thrash
  allocations. All GPU allocations are also accounted in the **VRAM tracker**, which
  reports current, peak and allocation counts (and forwards them to Tracy).

All per-frame inputs (projection, view, camera position, zoom, viewport size and
offset, scaled render size and the rescale flag, render scale, frustum-cull bounds and
cull padding, near/far planes, delta time, play/pause flags, the post-process flag, and
pointers to the scene, batch renderer, post-processor and framebuffers) are gathered
into a single `RenderFrameData` struct that is passed to every pass.

### 3.1 Passes & resources

The engine builds **two** graphs each frame.

**Scene graph** — renders the world into the scene color target:

| Pass | Reads | Writes | Job |
| ---- | ----- | ------ | --- |
| **Shadows** | — | `ShadowMaps` | Collect shadow casters and generate 2D shadow maps for shadow-casting lights (and the directional light). Skipped when nothing casts shadows. |
| **FX.Setup** | `ShadowMaps` | — | Bind the scene to the particle (FX) manager so emitters simulate/draw with the geometry. |
| **Geometry** | `ShadowMaps` | `SceneColor` | Draw the whole scene: background UI, the editor grid, sprites/flipbooks/tilemaps/skeletons, particles, foreground UI, and debug overlays — into the G-buffer. |

**Post-process graph** — turns the scene color into the final image:

| Pass | Reads | Writes | Job |
| ---- | ----- | ------ | --- |
| **PostProcess** | `SceneColor` | `FinalColor` | Run the [post-process stack](#9-post-processing). Enabled when a post-process volume is active, HDR10 output is on, FXAA is on, or an accessibility colour filter is active. |
| **Composite** | `SceneColor` | `FinalColor` | Resolve the scene color straight to the output. **Only runs when post-processing is *not* active** (the two passes are mutually exclusive producers of `FinalColor`). |

`SceneColor` is *imported* into the post-process graph rather than produced inside it,
because the scene graph already wrote it.

### 3.2 The frame, step by step

A single rendered frame proceeds roughly as:

1. **Wait on the low-latency fence** from the previous frame (when **Low Latency Mode**
   is on), then **begin frame** on the active backend — Vulkan, Direct3D 12, Metal and
   WebGPU acquire a swapchain image (Metal takes the next `CAMetalDrawable`) and bail out
   cleanly if the window has zero area; GL binds the
   default framebuffer. The pass profiler and the transient resource pool start their
   frame here.
2. **Resolve render size.** Apply **Render Scale** (clamped to 0.25×–4× at this point)
   and resize the scene/scaled framebuffers to match the viewport.
3. **Split-screen.** In local multiplayer, compute each player's viewport rectangle
   from their camera, clear the whole window to the **divider colour**, and then render
   the following steps once per player with a scissored viewport. Secondary players get
   their own `PostProcessor` instance and their own widget-overlay framebuffer.
4. **Evaluate post-process volumes** for the camera centre (blending volumes by
   priority), and decide whether post-processing is active this frame — it is when a
   volume is active, HDR10 output is live, FXAA is selected, or an accessibility colour
   filter is on.
5. **Clear** to the configured background color — from
   [Preferences → Rendering](Editor-EN-DOC.md#105-rendering), overridden by the level's
   [World Settings](Editor-EN-DOC.md#8-world-settings), and overridden again by a
   secondary split-screen camera's own background colour.
6. **Gather lights.** Point and spot lights are collected from `LightComponent`s (each
   tested against the view rectangle expanded by its own radius, and skipped when the
   transform is hidden or excluded from play mode), then particle emitters contribute
   their **particle lights**.
7. **Execute the scene graph** → Shadows → FX.Setup → Geometry, producing `SceneColor`
   (plus the G-buffer, when it is active), then finish that target with ray-traced GI,
   particles, fog of war, foreground UI and debug overlays
   ([3.3](#33-the-scene-drawing-chain)).
8. **Execute the post-process graph** → PostProcess (or Composite) → `FinalColor`.
9. **Composite the widget overlay.** When post-processing is on, UI drawn after the
   scene is rendered into its own full-resolution framebuffer and composited over the
   post-processed image, so the UI stays crisp at reduced **Render Scale** and is not
   distorted by scene effects.
10. **Finalize split-screen** — each player's image is blitted into its window rectangle.
11. **Render the editor UI** (ImGui, gizmos) on top — in a shipped game this step is
    absent and the final image is presented directly.
12. **Enqueue the low-latency fence**, then **end frame / present** on the backend.

### 3.3 The scene-drawing chain

Everything that ends up in `SceneColor` is drawn in a fixed order, and each stage
appears by name in the profiler. The **Geometry** graph node covers the world
geometry; the remaining stages run after it, still into the same target, before the
post-process graph takes over:

| Order | Stage | Where | What it draws |
| ----- | ----- | ----- | ------------- |
| 1 | `Widgets.GlobalZBackground` | Geometry pass | UI widgets that opted into world-Z ordering behind the scene. |
| 2 | *(editor only)* Grid | Geometry pass | The editor world grid, drawn through a dedicated grid shader. |
| 3 | `Render.Tilemaps` | Geometry pass | Tilemap layers, merged and cached per tilemap; animated tiles are resolved once per frame. |
| 4 | `Render.Flipbooks` | Geometry pass | Flipbook (frame-animation) instances. |
| 5 | `Render.Skeletons` | Geometry pass | Skeletal meshes and their attachments, drawn as meshes rather than quads. |
| 6 | `Render.Sprites` | Geometry pass | Sprite instances — the bulk of a typical scene. |
| 7 | Ray-traced GI | after the graph | `Raytracer2D` traces and composites [global illumination](#8-ray-traced-global-illumination) over the scene colour. |
| 8 | `FX.Update` / `FX.Render` | after the graph | Particle simulation and drawing (see below). |
| 9 | `FogOfWar` | after the graph | The fog-of-war overlay, in Play mode only. |
| 10 | `Widgets` | after the graph | Foreground UI widgets, plus the overlay capture described in [3.2](#32-the-frame-step-by-step). |
| 11 | Debug overlays | after the graph | Everything from [Debug visualization](#12-debug-visualization). |

Particles are simulated and drawn once per frame even in split-screen (the first view
owns the update), while every view redraws the geometry it can see.

Tilemaps, flipbooks, skeletons and sprites are each sorted before submission — by
world Z and then by **stencil state**, so that stencil writers and readers stay grouped
and state changes are minimized.

**Per-pixel cull scissor.** When **Culling Mode** is *Per-Pixel*, the world-space cull
rectangle is projected to window coordinates and installed as a scissor around the
whole world-drawing chain (intersected with any scissor already in force, and skipped
when it would not clip anything). It is restored exactly as it was afterwards.

**Particles.** The particle system simulates emitters and draws them through the same batch
renderer, so particles batch alongside sprites. Emitters can be **Alpha**, **Additive**
or **Multiply** blended, Lit or Unlit, use a custom material, and emit light into the
lighting pass. Where compute shaders are available the per-particle update runs on the
GPU (with an automatic CPU fallback), and ribbon/trail emitters get their own strip
renderer. Emitters also feed the fluid (SPH) solver, sub-emitters and collision against
scene colliders — all authored in the [FX asset](Assets-EN-DOC.md#412-fx--particle-system-ice_fx).

**Fog of war.** The fog-of-war stage maintains a grid of cells in three states — *Unseen*,
*Explored*, *Visible* — computed with recursive shadow-casting field-of-view from each
revealing point against per-cell opacity. It renders as a batched quad overlay at a
configurable Z with per-state alphas, an optional smooth per-cell fade, and a
serializable state so exploration survives a save/load. It is a Play-mode-only stage
and is skipped entirely when disabled.

**UI.** `WidgetRuntime` draws widgets through the batch renderer in **screen-space
mode**: the lit shaders switch to a screen-space branch that transforms the fragment
back to world space through a *shadow receiver* matrix, so on-screen UI can still be
lit and shadowed consistently with the world when you want it, or bypass lighting
entirely when you don't. Widgets can also request a **backdrop blur** of the scene
behind them ([4.5](#45-meshes-blur--special-draws)).

---

## 4. The 2D batch renderer

The **`BatchRenderer`** draws virtually everything visible: sprites, flipbook
frames, tilemap tiles, skeleton parts and UI quads. It is built to turn tens of
thousands of objects into a handful of draw calls.

### 4.1 Batching & draw submission

The renderer collects draws between `BeginBatch()` and `EndBatch()` and flushes
them in as few GPU draws as possible:

* **Quad batching** — each sprite becomes four vertices (`vec3` position, `vec2` UV,
  **packed RGBA8** color, texture index) appended to a growing vertex buffer; one draw
  renders the whole batch. Batch size is tunable (**Max Quads/Particles/Instances per
  Batch** in [Preferences → Optimization](Editor-EN-DOC.md#106-optimization)), with
  defaults of 50 000 quads / 100 000 instances / 10 000 particles and a hard cap of
  1 000 000.
* **Multi-texture batching** — up to *N* textures are bound to slots in one batch so
  sprites with different textures still share a draw. The slot count is configured
  separately for the GL and GLES backends (14 and 12 by default, hard cap 32) and is
  further clamped to what the device actually reports.
* **Bindless textures** — where supported, textures are referenced by 64-bit handle
  through an SSBO (up to 1024 resident handles), removing the slot limit entirely and
  shrinking draw counts further. The handle map is invalidated whenever textures are
  reloaded.
* **CPU frustum culling** — every sprite is AABB-tested against the camera rectangle
  expanded by **Cull Padding** before it is ever submitted. In **Per-Pixel** culling
  mode, textures additionally carry an alpha **coverage bitmask** (a coarse grid of
  "does this cell contain any non-transparent texel", plus a strict-opaque grid and an
  opaque UV window), so fully-transparent regions of a sprite can be rejected and fully
  opaque regions can be used for occlusion.
* **Stats** — draw calls, quad count and texture binds are tracked each frame and
  surface in the [Statistics](Profiling-And-Building-EN-DOC.md#33-renderer) panel.

### 4.2 Instancing, GPU culling & sorting

For large scenes the renderer can move work onto the GPU:

* **Instanced rendering** — instead of four vertices per sprite, a single unit quad
  is drawn *N* times from a per-instance buffer. Each instance is seven `vec4`s:
  position + rotation, size + pivot, UV offset + scale, colour, texture index, and two
  **effect-parameter** vectors ([4.6](#46-vertex-effects)) — drastically cutting CPU
  work and vertex bandwidth.
* **GPU frustum culling** — a compute shader tests every instance against the camera
  frustum (using the cull bounds + **Cull Padding**) and writes a compacted draw list
  into an **indirect draw** buffer, so off-screen sprites cost nothing on the CPU.
  Culling mode (AABB vs per-pixel) is set in
  [Preferences → Optimization](Editor-EN-DOC.md#106-optimization) and is passed through
  to the cull shader.
* **GPU sorting** — a bitonic-sort compute pass orders instances by draw category
  (4 blend modes × 2 shading modes = 8 categories) so state changes are minimized.
* **Multi-draw-indirect (MDI)** — a second compute pass bins the sorted instances into
  per-category regions and fills a command buffer, so all categories are issued as a
  single multi-draw call where supported.
* **Deferred translucency** — Translucent draws are held back in a separate instance
  list, order-sorted back-to-front on the CPU, and flushed last so overlapping
  transparency composites correctly.

Each of these is optional and gated on capability; the renderer falls back to plain
batched draws when a path is unavailable.

**Freeze culling** (a [debug flag](#12-debug-visualization)) locks the frustum bounds
at their current values so you can fly the camera out and inspect exactly what the
culler was keeping.

### 4.3 Blend & shading modes

Every draw has a **blend mode** and a **shading mode**, which select one of the
renderer's shader variants:

| Blend mode | Result |
| ---------- | ------ |
| **Masked** | Alpha-tested (cutout) — pixels below the alpha-clip threshold are discarded. The default for pixel-art sprites. |
| **Additive** | Adds to the framebuffer — for fire, glows, magic. |
| **Translucent** | Standard alpha blending — sorted back-to-front (deferred) for correct overlap. |
| **Opaque** | No blending. |

| Shading mode | Result |
| ------------ | ------ |
| **Unlit** | Drawn at full color, ignoring lights. |
| **Lit** | Affected by the 2D lighting & shadow systems (Section [6](#6-lighting)/[7](#7-2d-shadows)). |

The four blend modes × two shading modes give **eight** core shader variants (plus
instanced equivalents, plus bindless and GLES-branched variants of each). A per-draw
**alpha-clip threshold** controls the Masked cutoff.

Three renderer-wide switches modify how a draw is shaded, independent of the material:

| Switch | Effect |
| ------ | ------ |
| **Force Unlit** | Draw everything with the unlit variant regardless of its shading mode — used for previews and for passes that must not be re-lit. |
| **Screen-space mode** | Treat incoming positions as screen space and transform them back through the **shadow receiver matrix** before evaluating lights, so UI drawn in screen space still receives world lighting and shadows correctly. |
| **Suppress shadows** | Evaluate lights but skip all shadow sampling for this draw. |

### 4.4 Buffer streaming

To avoid GPU stalls when uploading per-frame geometry, the renderer uses one of
several streaming strategies depending on backend support:

* **Persistent-mapped, triple-buffered** vertex/instance buffers — the CPU writes
  directly into mapped, coherent GPU memory, rotating through three buffers each
  guarded by its own fence sync.
* **Stream double-buffering with orphaning** — alternates two buffers and orphans
  the previous contents to avoid read-after-write hazards.
* A plain dynamic-buffer path as the universal fallback.

The vertex and instance buffers each pick their strategy independently, so a device
that supports persistent mapping for one and not the other still gets the best
available path for both.

### 4.5 Meshes, blur & special draws

Beyond quads, the batch renderer can:

* **Draw meshes** — arbitrary triangle meshes (positions, UVs, indices) used by
  **skeletal** mesh attachments, with the same blend/shading/alpha-clip options.
* **Panel backdrop blur** — a Gaussian backdrop blur behind UI panels (captures the
  scene, blurs it, and composites it under the panel) for frosted-glass UI. When
  post-processing is active the capture source is **redirected** to the post-processor's
  own scene target (with the widget overlay layered in), so a blurred panel sees the
  real, effect-processed image rather than a stale copy.
* **Custom materials** — draw a sprite or mesh with a compiled material-graph shader
  (Section [5](#5-materials--shaders)), including dynamic per-instance parameter
  overrides; the renderer can blit the current scene color so materials can sample
  it (refraction, distortion).
* **Effect params** — per-draw parameter vectors that built-in and custom shaders can
  read for cheap per-sprite effects; the built-in shaders use them for the vertex
  effects below.

### 4.6 Vertex effects

Sprites, flipbooks and skeletons carry a small set of **vertex effects** that the
built-in shaders apply for free — no material and no extra draw call. They are packed
into the two effect-parameter vectors and evaluated in the vertex shader:

| Effect | Parameters | Result |
| ------ | ---------- | ------ |
| **Parallax** | `Parallax X`, `Parallax Y` | Scrolls the sprite's **UVs** against camera motion, per axis, scaled by the sprite's size. The quad itself does not move, so a single fixed quad becomes an infinitely scrolling parallax layer — stack a few with different factors for instant multi-layer backgrounds, with no extra cameras and no extra draw calls. |
| **Sway** | `Amplitude`, `Speed`, `Phase Offset`, `Gradient` | A time-based horizontal displacement. **Gradient** scales the displacement by the vertex's height in the quad, so at `1` the base stays planted and only the top moves (grass, foliage, banners) and at `0` the whole sprite slides uniformly; **Phase Offset** de-synchronizes neighbouring instances so a field does not move as one block. |
| **Wind** | `Strength`, `Speed` | A second horizontal wave on top of the sway, additionally phase-shifted by the vertex's **world height**, so objects at different heights are naturally out of step. It obeys the same gradient as the sway. |

Because they run per vertex on data already in the instance stream, hundreds of swaying
props cost the same number of draw calls as static ones.

### 4.7 Stencil masking

The **Stencil** component turns any renderable into a stencil mask or a masked
receiver, which is how "show this sprite only inside that shape" effects are built —
portals, spotlights on the UI, scratch-off reveals, clipped minimaps:

| Setting | Meaning |
| ------- | ------- |
| **Mode** | **Off**, **Write** (this object stamps its ID into the stencil buffer and is not drawn to colour) or **Read** (this object is only drawn where the stencil test passes). |
| **Stencil ID** | The reference value, 1–255, so several independent masks can coexist. |
| **Compare** | **Equal** (draw inside the mask) or **Not Equal** (draw outside it). |

Draws are sorted by stencil state within each renderable category, so writers and their
readers stay grouped and the stencil state is switched as rarely as possible. The
stencil buffer is part of the depth/stencil attachment and is cleared with the frame.

---

## 5. Materials & shaders

A **material** in IceBox is a **node graph** that compiles to a runtime shader. You
build the look by wiring nodes (texture samples, math, generators, parameters,
material-function calls, custom expressions) into a **Material Output**, choosing a
**Shading Mode** (Lit/Unlit), **Blend Mode**, **Domain** (Surface, PostProcess or Decal)
and alpha-clip threshold.

* **Surface** materials are applied to sprites/meshes through the batch renderer
  (Section [4.5](#45-meshes-blur--special-draws)).
* **PostProcess** materials are inserted into the post-process stack (Section
  [9.2](#92-the-effect-stack)).
* **Decal** materials are assigned to a `.ice_decal` asset and drawn by the decal pass
  over the surfaces underneath. They compile exactly like a surface material — the decal
  texture arrives as the entity texture — and gain the **Decal Data** node, which exposes
  the drawn decal's fade, age, normalized age, lifetime and a stable per-decal random
  value. The pass runs right after sprites, inside the same batch, so decals depth-sort
  and light like any other 2D geometry.
* **Material Instances** cheaply override an exposed parameter set; **Material
  Functions** are reusable sub-graphs; **Material Parameter Collections (MPC)** are
  global parameter bags many materials can read at once.

What the renderer does with a compiled material:

* **It writes the G-buffer.** A generated surface shader always emits all three
  attachments. A **Lit** material writes its normal (encoded `×0.5+0.5`), roughness and
  metallic into `GBufferNormal`, and its ambient occlusion and emissive luminance into
  `GBufferMaterial`; an **Unlit** material writes the neutral defaults (flat normal,
  full AO, no emissive). This is what makes SSR, godrays, volumetric fog and the
  screen-space GI compose respond to authored material properties.
* **It feeds uniforms every draw.** Time, alpha-clip threshold, camera position, screen
  size, projection/view, the entity's own texture, all scalar/vector/texture
  parameters, and any Material Parameter Collections the graph reads.
* **It can sample the scene.** A graph with a refraction/scene-colour node makes the
  renderer blit the current scene colour into a sampler before the draw, so distortion
  and refraction see what is already on screen.
* **Dynamic material instances** override scalars, vectors, textures and the alpha-clip
  threshold per draw, without recompiling or duplicating the material.
* **Sampler budget.** A surface material may use at most **16** texture samplers in
  total (its own textures, the entity texture, the optional scene colour, and — when
  Lit — the shadow-map and light-cookie arrays). Exceeding that is a compile error with
  an explicit message, because WebGL 2.0 guarantees no more.

The authoring of materials — every node, pin type and the editors — is documented
in [Assets → Materials](Assets-EN-DOC.md#48-material-ice_material). This document
only covers how compiled materials are *consumed* by the renderer.

---

## 6. Lighting

### 6.1 Lighting modes

The world has one of two lighting modes (global, with a per-level override in
[World Settings](Editor-EN-DOC.md#8-world-settings)):

| Mode | Behavior |
| ---- | -------- |
| **Unlit** | Sprites render at their authored color. Lights are ignored. Fastest; ideal for flat-shaded or hand-lit art. |
| **Lit** | **Lit**-shaded sprites are affected by ambient light, point/spot/directional lights, 2D shadows and (optionally) ray-traced GI. **Unlit**-shaded sprites still bypass lighting even in Lit mode. |

Lighting is evaluated **in the forward sprite shaders**, not in a deferred pass: all
active lights are gathered once per frame into a **light uniform buffer** (`LightUBO`)
that every lit shader reads, and each lit fragment loops over the lights that reach it.

### 6.2 Light types

| Light | Parameters | Notes |
| ----- | ---------- | ----- |
| **Point** | Position (including **Z**), **Radius**, Color, **Intensity**, **Falloff Exponent**, Cast Shadows, cookie | Omnidirectional. The most common scene light. |
| **Spot** | + **Direction**, **Inner Angle**, **Outer Angle** | A cone of light with a soft edge between inner and outer angle. |
| **Directional** | Direction, Color, Intensity, Cast Shadows | A single global light (like the sun) with parallel rays; one per world. |

Point and spot lights are attached to entities via the **Light** component (see
[Editor → Properties](Editor-EN-DOC.md#72-components)); the directional light is set
globally in [Preferences → Rendering](Editor-EN-DOC.md#105-rendering) or per level in
[World Settings](Editor-EN-DOC.md#8-world-settings).

How a light is evaluated:

* **Attenuation** is `(1 − distance/radius)` raised to the **Falloff Exponent**, and a
  fragment beyond the radius is rejected outright — so radius is a hard cutoff, not
  just a falloff hint.
* **Spot cone** multiplies that by a `smoothstep` between the cosines of the outer and
  inner angles, giving a soft cone edge.
* **A cheap specular highlight** is added on top of both point/spot and directional
  light, derived from a half-vector against the light's height above the receiver — it
  gives sprites a subtle sense of relief without any normal map.
* **Particle lights** are contributed by FX emitters each frame, on the same budget as
  scene lights.

### 6.3 Ambient & light cookies

* **Ambient light** — a base color and intensity added everywhere, so shadowed areas
  are never fully black (unless you want them to be).
* **Light cookies** — point and spot lights can project a **texture mask** (a
  "cookie" / gobo) with its own **intensity** and **rotation**, packed into a shared
  cookie atlas: a 2D texture array of up to **32 layers of 256×256**, keyed by texture
  path so two lights using the same cookie share one layer. For a spot light the cookie
  is oriented along the cone direction; for a point light it is projected radially over
  the light's radius. Use them for window light, foliage dapple, projected logos,
  flickering patterns, and so on.

### 6.4 The light budget & the light UBO

Lights live in a single fixed-size uniform block, which sets a hard structural limit
and a soft configurable one:

| Limit | Value | Meaning |
| ----- | ----- | ------- |
| **Hard cap** | 128 | The size of the light arrays compiled into the UBO and the shaders (`MAX_LIGHTS_CAP`). Lights past this are dropped when they are collected. |
| **Active limit** | 32 by default | **Max Point Lights** in [Preferences → Optimization](Editor-EN-DOC.md#106-optimization) (`MAX_LIGHTS_ACTIVE`) — how many the shader loop will actually walk. Raising it costs shader time on every lit pixel. |
| **Shadow-map slots** | 4 by default, grown on demand | Layers in the shadow-map texture array; the directional light takes one of them. |

Only lights whose radius reaches the visible rectangle are gathered at all, so a level
with hundreds of lights only pays for the ones on screen. The UBO holds, per light, its
position + radius, colour + intensity, falloff + shadow flags + shadow-map index + spot
flag, spot direction + cone cosines, and cookie layer + intensity + rotation, plus the
global ambient, shadow parameters and the directional-light and directional-shadow
blocks. It is bound at uniform binding point 0 and re-uploaded only when it changes.

---

### 6.5 Where lighting and shadow settings come from

Everything in `Config/Engine.json` → `Rendering` is the **project default** for the whole
game: lighting mode, ambient, shadows (including **Colliders Block Shadows**, directional
shadow length and depth fade), ray tracing, the directional light, the clear colour and the
near/far planes. The editor authors that file through
[Preferences → Rendering](Editor-EN-DOC.md#105-rendering); the shipped runtime reads it at
startup on every platform, so the editor viewport, Play mode and the built game all start
from exactly the same values.

Three layers stack on top of each other, highest wins:

| # | Layer | Source | Scope |
| - | ----- | ------ | ----- |
| 1 | **Project defaults** | `Config/Engine.json` → `Rendering` | Every level, every platform |
| 2 | **Level override** | [World Settings](Editor-EN-DOC.md#8-world-settings) with **Override Enabled** on | The level it is saved in |
| 3 | **Runtime override** | A [Lua](LuaAPI-EN-DOC.md#133-lighting-and-shadows--global-settings) / Visual Script call such as `SetShadowsEnabled(false)`, `SetCollidersBlockShadows(true)` or `Settings.SetLightingEnabled(false)` | The rest of the session |

A runtime override is tracked **per parameter**: calling `SetShadowsEnabled(false)` pins
only "shadows enabled" and leaves every other value following the project default or the
level override. Runtime overrides **survive level transitions** — loading a new level
re-applies layers 1 and 2 but never resets a value the game has taken control of, so an
in-game "Shadows: Off" option stays off when the player leaves the main menu for the first
level. In the editor they are cleared when Play mode ends, so the viewport returns to the
project settings.

`Settings.Save()` writes the runtime overrides that are active into the player's
`GameSettings.json` under a `Rendering` block — only the parameters the game actually
changed, never the whole `Rendering` section. They are re-applied as runtime overrides at
the next startup (and when Play mode starts in the editor), which is what makes an in-game
graphics menu persist across launches. `Settings.ResetDefaults()` drops them all and returns
to the `Engine.json` values.

---

## 7. 2D shadows

When shadows are enabled and at least one light casts them, the **Shadow2DSystem**
produces real-time 2D shadows by ray-casting against scene geometry.

### 7.1 How ray-cast shadows work

For each shadow-casting light the system builds a compact **radial shadow map**: rays
are cast outward from the light in every direction, and for each angular bin the system
records the **intervals** of that ray that are occluded. During shading, a lit pixel
converts its offset from the light into an angle (the U coordinate) and a distance, and
tests that distance against the recorded intervals.

Each angular bin stores **four layers**, and each layer is an RGB triple:

| Channel | Meaning |
| ------- | ------- |
| **R** | Where the occluded interval **starts**, normalized to the light radius. |
| **G** | The occluder's **shadow Z** (its height). |
| **B** | Where the occluded interval **ends**, normalized to the light radius. |

Four layers means up to four overlapping occluders along the same ray are resolved
independently, rather than the nearest one swallowing everything behind it. Because
each interval carries its occluder's height, a receiver whose own Z is **at or above**
the occluder's shadow Z is left lit — that is what makes a tall wall shadow the floor
but not the roof, and lets sprites at different heights shadow each other correctly.
All lights' shadow maps are packed into a single **texture array**.

This is a true 2D visibility technique: shadows are cast by the actual silhouettes
of scene geometry, support any number of occluders, and update every frame.

### 7.2 Shadow casters

Shadow casters are collected automatically from the scene each frame (only those
within the view region, for performance). A caster can be a:

| Caster | Source |
| ------ | ------ |
| **Box** | Box colliders and rectangular sprite/flipbook/skeleton bounds. |
| **Circle** | Circle (sphere) colliders. |
| **Polygon** | Sprite collision polygons, capsule colliders, traced texture contours and convex shapes. |
| **Segment** | Edge/chain geometry (e.g. tilemap collision chains). |

Anything renderable can cast — sprites, flipbooks, skeletons, tilemaps and tilesets, FX
instances, collider shapes, and the fragments produced by
[destruction](#137-joints-queries--destruction) — and each carries its own shadow
settings:

| Setting | Effect |
| ------- | ------ |
| **Cast Shadow** | Whether this instance contributes to the shadow maps at all. |
| **Cast Shadow Mode** | **Colliders** — use the attached collider shapes as the silhouette (cheap, predictable). **Contour** — trace the texture's alpha contour so the shadow matches the artwork exactly (the traced contour is cached per texture). Available where there is artwork to trace: sprites, flipbooks, skeletons, tilemaps and tilesets. Collider shapes and FX always cast from their own shape. |
| **Shadow Origin** | Where the caster's height is anchored: **Bottom**, **Center** or **Top**. |
| **Shadow Z Order** | The caster's height, which drives the height-aware occlusion described above. |
| **Shadow Edge Fade** | Shrinks/feathers the caster's edges so hard silhouettes soften at their extremities. |
| **Don't Block Shadows** | On by default. When cleared, this caster also **blocks** shadows cast by others — see below. |

**Casting vs blocking.** Every edge carries two independent flags. *Casts* means "this
edge creates shadow"; *Blocks* means "this edge stops shadow that other geometry cast".
Blocking is what lets a floor tile or a ceiling piece cut off a shadow that would
otherwise fall through it. The world-level **Colliders Block Shadows** switch (in
[Preferences → Rendering](Editor-EN-DOC.md#105-rendering)) turns collider geometry into
blockers globally; the per-instance **Don't Block Shadows** flag opts an individual
object out. Changing either invalidates the shadow cache.

Each caster contributes **edges** to a spatial **edge grid** that accelerates the
ray casts.

### 7.3 Quality, softness & bias

Shadow appearance is controlled in
[Preferences → Rendering](Editor-EN-DOC.md#105-rendering) (or per level in
[World Settings](Editor-EN-DOC.md#8-world-settings)):

| Setting | Effect |
| ------- | ------ |
| **Ray Quality** | Shadow-map resolution: **Low** (180), **Medium** (360), **High** (720), **Ultra** (1080) angular samples per light. Higher = crisper shadows, more cost. Turning shadows off is a fifth state (`Off`) that stops the system entirely. |
| **Softness** | 0–1. Above ~0.01 it switches shadow sampling from a single hard tap to a **Gaussian-weighted PCF kernel** with a per-pixel dither, blending a penumbra so shadow edges are soft rather than hard. |
| **Intensity** | How dark the shadow is (0 = invisible, 1 = full); the final result is a lerp between "fully lit" and the sampled shadow. |
| **Bias** | A **2D** offset (±1000): **X** is a *distance* bias along the ray, **Y** is a *perpendicular* bias that shifts the sample sideways. Together they prevent self-shadowing artifacts ("shadow acne") on both flat and glancing contacts. |
| **PCF Samples** | Percentage-closer-filtering taps (1–7). The shader uses half that count on each side of centre, capped at 4. |
| **Directional Length** | Maximum length of directional shadows in world units; beyond it the shadow fades out instead of stretching to the horizon. `0` = unlimited. |
| **Directional Depth Fade** | Fades a directional shadow out as the height gap between occluder and receiver grows, so tall objects do not paint hard shadows onto distant ground. |
| **Colliders Block Shadows** | Makes collider geometry block other objects' shadows globally (see [7.2](#72-shadow-casters)). |

### 7.4 Directional shadows

The **directional light** casts a separate shadow map computed along its direction
across the visible region (an orthographic-style projection), giving long, parallel
"sun" shadows. Instead of an angle, the map is indexed by the receiver's coordinate
**perpendicular** to the light direction, and the stored interval is compared against
its coordinate **along** the light direction — the same four-layer, height-aware
structure as point lights.

It has its own enable/cast-shadows flags, shares the softness/intensity/bias settings,
adds **Directional Length** and **Directional Depth Fade**, and is rendered into its
own slot of the shadow-map array at a **boosted resolution** (up to 2048) because a
single map has to cover the whole visible region. Its extent is fitted to the view each
frame and clamped by the configured shadow length so resolution is not wasted on
off-screen space.

### 7.5 GPU compute & caching

The shadow system has two execution paths and two big optimizations:

* **GPU compute path** — when compute shaders are available, occluder edges and
  lights are uploaded to SSBOs and all shadow maps are generated on the GPU in one
  dispatch.
* **CPU fallback** — otherwise, a brute-force ray caster runs on the CPU using the
  edge grid for acceleration.
* **Edge-grid acceleration** — ray casts query a uniform grid of edges instead of
  testing every edge, keeping cost roughly proportional to local complexity. The grid
  is rebuilt only when the edge set changes.
* **Temporal caching** — if a light's position/radius and the surrounding occluder
  edges (compared by hash) are unchanged from last frame, its shadow map is **reused**
  instead of regenerated. The directional light has its own separate cache keyed on
  light direction, view bounds, edge hash, bias, shadow length, resolution and slot.
  Static scenes pay almost nothing.
* **Once-per-frame collection** — casters are gathered a single time per rendered
  frame and shared by the shadow pass, the ray tracer and the debug overlays, even in
  split-screen.

Shadow counters — casters, edges, shadow-casting lights, directional state, map
resolution and a per-light **shadow-map preview** strip — are in the
[Statistics panel](Profiling-And-Building-EN-DOC.md#33-renderer).

---

## 8. Ray-traced global illumination

For richer lighting, IceBox includes an optional **2D ray-traced global
illumination** system (`Raytracer2D`) — soft, bounced light and color bleed driven
by a compute shader. It is enabled in
[Preferences → Rendering](Editor-EN-DOC.md#105-rendering) (or per level) and only
where the device reports the required capability.

How it works:

* Occluder **edges** are extruded into a triangle mesh and built into a hardware
  **acceleration structure** (BLAS + TLAS) once per geometry change; static scenes
  reuse the structure for free.
* Every shadow-casting **point light**, **spot light** (with cone, falloff, cookie
  and per-light shadow flag), the **directional light** and the **ambient colour**
  feed the trace, exactly like the rasterised lighting path — so the ray tracer
  sees the same lighting rig you author.
* A compute shader casts stratified rays per GI texel through `VK_KHR_ray_query`,
  gathering incoming light with **multiple bounces** so light spreads around
  corners and picks up nearby colour.
* At every hit the bounced radiance is read from the **rendered scene** when the
  hit is on-screen, giving full-detail colour bleeding and emissive glow;
  off-screen hits fall back to analytic light evaluation with ray-traced shadow
  rays and area-sampled penumbra.
* Ray hit distances also produce **ray-traced ambient occlusion** — contact
  darkening wherever geometry meets — at no extra ray cost.
* Results accumulate into a **history** texture with variance-based clamping,
  pass through an **à-trous spatial denoiser**, and are composited with a
  bicubic (Catmull-Rom) upsample so the result stays sharp at reduced GI
  resolution.

Quality settings trade cost for fidelity:

| Setting | Effect |
| ------- | ------ |
| **Quality** | Rays per texel, GI resolution scale and denoiser passes: Low (4 rays / 0.40× / 1 pass), Medium (8 / 0.55× / 2), High (14 / 0.75× / 2), Ultra (24 / 1.0× / 3). |
| **Intensity** | Overall strength of the bounced light. |
| **Bounce** | Strength of light carried between bounces. |
| **Max Bounces** | Number of light bounces (1–8). |
| **Reflection** | Blends the bounce direction from diffuse toward the mirror direction (0 = diffuse GI, 1 = mirror-like light streaks). |
| **Max Distance** | How far rays travel before giving up; also sets the distance falloff of bounced light. |
| **Ray-Traced AO** | Strength of the contact-darkening term (0 = off). |
| **AO Radius** | World-space reach of the ambient occlusion. |
| **Albedo Response** | 0 adds GI on top of the image, 1 multiplies it by the surface colour so bounced light behaves like real light. AO is always applied multiplicatively. |
| **Sky Light** | Brightness of the sky light picked up by rays that escape the scene; multiplies the ambient colour and adds to the ambient intensity. |
| **Detail Sharpness** | Denoiser aggressiveness — higher keeps crisper contact shadows, lower is smoother and more stable. |
| **Screen Colour Bleeding** | Take bounce colour from the rendered scene (needs post-processing on and MSAA off); otherwise analytic lighting is used everywhere. |
| **Denoise** | Toggle the temporal + spatial denoiser. |
| **Shadow rays** | Toggle ray-traced occlusion for the analytic lighting path. |

> GI is a higher-end feature. It requires a Vulkan device with hardware ray
> tracing (`VK_KHR_ray_query` + acceleration structures), a Direct3D 12 adapter with
> **DirectX Raytracing 1.1** (inline ray tracing, shader model 6.5), or a Metal device
> that reports `supportsRaytracing` with MSL 2.4 ray queries (Apple silicon, macOS 12+ /
> iOS 15+); everywhere else the setting is shown as unsupported and the scene falls back
> to direct lighting + shadows.

> **Two different things are called "GI".** The system above (`Raytracer2D`) is the
> *world* GI: it is a rendering setting, needs hardware ray tracing, and lights the
> scene. There is also a **Global Illumination** effect inside the post-process stack
> — a **radiance-cascade** screen-space approximation configured per
> [View volume](#94-post-process-volumes) (cascade count, base ray count, max distance,
> intensity) that runs on any backend with compute and composes over the rendered
> image. They are independent and can be used separately or together.

---

## 9. Post-processing

After the scene is rendered, the **`PostProcessor`** applies screen-space effects to
produce the final image. The stack is configured by **View** assets
(`.ice_view`) placed as **post-process volumes** (see
[Assets → View](Assets-EN-DOC.md#414-view--post-process-volume-ice_view) and
[Editor → World-asset properties](Editor-EN-DOC.md#74-world-asset-properties-views--cinemas)).

### 9.1 The G-buffer & flow

The scene is rendered into a small **G-buffer**:

| Attachment | Format | Holds |
| ---------- | ------ | ----- |
| **Color 0 — Scene color** | `RGBA16F` (falls back to `RGBA8` where float targets are unsupported) | The lit, composited scene. |
| **Color 1 — Normal** | `RGBA8` | `xy` = encoded surface normal, `z` = roughness, `w` = metallic. Cleared to a flat normal. |
| **Color 2 — Material** | `RGBA8` | `r` = ambient occlusion, `g` = emissive luminance. Cleared to full AO / no emissive. |
| **Depth/Stencil** | `Depth24Stencil8` | Scene depth for DoF, fog, SSAO, SSR, godrays and volumetric fog; the stencil half drives [stencil masking](#47-stencil-masking). |

Three optimizations keep this cheap when you are not using it:

* **The extra attachments are conditional.** The two G-buffer targets are only bound as
  draw buffers when an effect that reads them is active — SSR, screen-space GI, godrays
  or volumetric fog — or when MSAA is on. Otherwise the scene renders into a
  single-attachment "lite" framebuffer that shares the same colour and depth textures,
  so the geometry pass writes one target instead of three.
* **The depth attachment is invalidated** at the end of the scene when no active effect
  reads depth, which saves the write-back on tiled (mobile) GPUs.
* **MSAA is resolved once.** With MSAA on, the scene renders into multisampled
  renderbuffers and is resolved into the sampleable textures exactly once, before the
  first effect that needs to read the scene.

Effects then run as a chain of full-screen passes ping-ponging between two buffers,
with dedicated mip chains for **bloom** (up to 6 mips, downsample/upsample) and
**luminance** (up to 12 mips, for auto exposure), a two-texture ping-pong for
**exposure adaptation**, and up to 8 **radiance cascades** for the screen-space GI. The
result is tonemapped and written to the final target.

### 9.2 The effect stack

The post-process stack is extensive. Each effect is independently toggled and
configured per volume:

| Category | Effects |
| -------- | ------- |
| **Exposure & tone** | **Auto Exposure** (eye adaptation with exposure compensation, min/max exposure and separate up/down adaptation speeds), **Tonemap** (None / Reinhard / ACES), **HDR10** output tonemap. |
| **Bloom** | **Bloom** (intensity, threshold, radius — mip-chain based, with a Gaussian ping-pong fallback) and **Bloom Dirt** (lens-dirt texture overlay with its own intensity). |
| **Color** | **Color Grading** (saturation, contrast, gamma, tint) and **LUT** (color lookup table with blend intensity). |
| **Lens** | **Vignette** (intensity, radius, softness), **Film Grain**, **Chromatic Aberration**, **Lens Flare** (threshold, ghost count, dispersal, halo width, chroma distortion), **Lens Sharpen** (intensity, radius), **CAS** (contrast-adaptive sharpening). |
| **Depth-based** | **Depth of Field** (focus distance, focus range, blur amount), **Motion Blur** (intensity, samples, max blur — driven by camera pan and zoom velocity), **Fog** (Linear / Exp / Exp², with height falloff and offset, applied inside the composite pass) and **Volumetric Fog** (steps, scattering, density, colour, light position). |
| **Screen-space GI/AO/SSR** | **Ambient Occlusion (SSAO)** (intensity, radius), **Screen-Space Reflections (SSR)** (intensity, max distance, max steps, thickness, roughness cutoff, edge fade), **Godrays** (samples, decay, weight, exposure, screen-space light position), **Global Illumination** (radiance cascades: cascade count, base ray count, max distance, intensity — with a compute path where available), procedural **Sky** (texture, time of day, intensity, horizon offset — drawn *before* the scene). |
| **Stylize** | **Heat Haze** (intensity, speed, scale, top/bottom mask), **Underwater** (tint + intensity, distortion + speed + scale, caustics + scale + speed). |
| **Custom** | **Custom post-process materials** (materials with `Domain = PostProcess`), each with a strength, an enable flag and a **Before**/**After** placement in the chain. A material that fails to compile is remembered so it is not retried every frame. |
| **Accessibility** | A final pair of accessibility passes driven by [Preferences → Accessibility](Editor-EN-DOC.md#108-accessibility): **Field of View** (a screen-space lens warp — barrel above 90°, pincushion below, aspect-corrected, with automatic scale compensation so the frame stays filled) followed by the colour pass (gamma/contrast/brightness/saturation, colorblind correction — Protanopia / Deuteranopia / Tritanopia / Achromatopsia with strength). Either one activates the post-process path on its own, so accessibility settings apply even in a level with no volume. |
| **Anti-aliasing** | **FXAA** as a post pass (MSAA/SSAA are handled at raster time — Section [10](#10-image-quality-aa-hdr--scaling)). |

### 9.3 Effect order

The chain is fixed, and knowing it explains most "why does X look like that" questions
— for example, why fog is unaffected by colour grading (it is applied earlier), or why
FXAA never softens the HDR10 tonemap (it runs after it):

1. **Sky** — drawn *before* the scene, so geometry composites over it.
2. **Auto Exposure** — luminance mip chain → adaptation ping-pong.
3. **Bloom** → **Bloom Dirt** texture → **Lens Flare** generation.
4. **Composite** — scene + bloom + exposure + **tonemap** + lens dirt + lens flare + **fog**.
5. **Custom materials** marked *Before*.
6. **Depth of Field** → **SSAO** → **SSR** → **Global Illumination** → **Godrays** → **Volumetric Fog**.
7. **Heat Haze** → **Underwater** → **Motion Blur**.
8. **Lens Sharpen** → **CAS**.
9. **Color Grading** → **LUT** → **Chromatic Aberration** → **Vignette** → **Film Grain**.
10. **Field of View** lens warp → **Accessibility** colour pass.
11. **Custom materials** marked *After*.
12. **Final composite** — passthrough, or the **HDR10** PQ tonemap when HDR10 output is live.
13. **FXAA**.

### 9.4 Post-process volumes

Rather than one global setting, post-processing is driven by **volumes**:

* A **View** asset dropped into the level becomes a post-process volume with
  **bounds**, a **blend radius** and a **priority**.
* As the camera moves, the active settings are **blended** from the volumes it is
  inside (highest priority wins, blended over the radius). An **infinite/unbounded**
  view acts as the global default.
* Volumes are evaluated against the **camera centre**, not the camera origin.
* Volumes can fire **enter/exit callbacks** to script (e.g. play a sound on entering
  water); the set of volumes the camera is currently inside is tracked between frames
  so each transition fires exactly once.
* In **split-screen**, each player's camera evaluates volumes independently and drives
  its own post-processor, so two players in different rooms get different grading.
* Views can be **prewarmed** so the first frame inside a volume does not stall on
  compiling its effect shaders.

A View asset can also carry **navigation-grid** settings used by AI pathfinding; see
[Assets → View](Assets-EN-DOC.md#414-view--post-process-volume-ice_view).

---

## 10. Image quality: AA, HDR & scaling

Set in [Preferences → Engine](Editor-EN-DOC.md#101-engine).

### 10.1 Anti-aliasing & render scale

| Mode | How it works |
| ---- | ------------ |
| **Off** | No anti-aliasing. |
| **FXAA** | A cheap screen-space post pass, applied last in the [effect chain](#93-effect-order). It activates the post-process path on its own, so it works in a level with no volume. |
| **MSAA 2× / 4× / 8×** | Hardware multisampling: the scene renders into multisampled renderbuffers and is resolved once. Changing it needs a **restart**, and it exposes **Alpha-to-Coverage** for crisp masked (cutout) edges. MSAA forces the full G-buffer path and disables the ray tracer's screen-colour bleeding. |
| **SSAA 2× / 4×** | Supersampling, implemented as an internal render-scale multiplier of **1.414×** and **2×** respectively — render bigger, downscale on output. Highest quality, highest cost. |

**Render Scale** is an internal-resolution multiplier exposed as 1–200 % and clamped to
0.25×–4× at render time. Below 100 % the scene renders into a smaller framebuffer and
is upscaled (performance); above 100 % it renders larger and is downsampled (quality).
UI drawn after the scene is composited at full resolution regardless, so a low render
scale does not blur the interface.

### 10.2 Upscaling: FSR & NIS

Render the scene at a lower internal resolution and reconstruct it to the output
resolution at the very end of the frame — after post-processing, before the UI. Like
[ray tracing](#8-ray-traced-global-illumination) this is **Vulkan / Direct3D 12 / Metal
only and gated on the GPU**: the capability is probed once at device creation, and on any other backend or an
unsupported device the setting is stored but does nothing, leaving the normal linear
blit in place. Defaults to **Off**.

| Upscaler | How it works | Requires |
| -------- | ------------ | -------- |
| **FSR** | AMD FidelityFX Super Resolution 1.0 — **EASU**, a 12-tap edge-adaptive spatial upsample that fits an anisotropic elliptical filter to the local gradient, then **RCAS**, robust contrast-adaptive sharpening with a noise-aware limiter that cannot overshoot the local ring. Two passes. | Vulkan or Direct3D 12 + any GPU that can sample and render `RGBA8` with linear filtering |
| **NIS** | NVIDIA Image Scaling — directional scaling driven by the local structure tensor (eigen-decomposed to get edge direction and coherence), with the reconstruction kernel stretched along the edge, plus an edge-adaptive unsharp mask clamped against the local 2×2 range to suppress ringing. One pass. | Vulkan or Direct3D 12 + an **NVIDIA** GPU |

**Quality presets** set the internal render resolution as a fraction of the output:
Ultra Performance 33 %, Performance 50 %, Balanced 59 %, Quality 67 %, Ultra Quality
77 %, Native 100 % (sharpening pass only, no resolution reduction). The preset
**multiplies** with **Render Scale** and the SSAA multiplier, so keep Render Scale at
100 % to let the preset drive the resolution on its own.

**Sharpening** is a separate toggle with a 0–1 strength: FSR routes it through RCAS as a
second pass, NIS folds it into its single pass. Turning it off makes FSR a pure EASU
upsample and zeroes the NIS unsharp mask.

At **Native** no reconstruction is performed: FSR runs RCAS alone as a single pass and
NIS applies only its unsharp mask, so the image is sharpened but never resampled. With
Native **and** sharpening disabled the upscaler is a true no-op — the frame skips the
extra render target entirely and costs exactly what it would with upscaling off. In
every other case the engine routes the frame through the scaled framebuffer so the
upscaler has a distinct source image to read. The passes appear in the render-pass
profiler as `Upscale.FSR.EASU` / `Upscale.FSR.RCAS` / `Upscale.NIS`.

### 10.3 HDR10 output

HDR10 (ST.2084 PQ / Rec.2020) output with configurable **paper-white** and
**max-luminance** nits. Real HDR signalling is delivered by the **Vulkan**,
**Direct3D 12** and **Metal** backends in a standalone build: the swapchain is created
with the HDR10 colour space when the surface offers it
(`VK_COLOR_SPACE_HDR10_ST2084_EXT`, `DXGI_COLOR_SPACE_RGB_FULL_G2084_NONE_P2020` via
`IDXGISwapChain3::SetColorSpace1`, or a `CAMetalLayer` with
`wantsExtendedDynamicRangeContent`, the `ITUR_2100_PQ` colour space and `CAEDRMetadata`
on a display that reports EDR headroom), the
final target switches to `RGB10_A2`, HDR metadata is published to the
display, and the last post-process step becomes the PQ tonemap instead of a
passthrough. In the editor viewport and on other backends (OpenGL / WebGPU / GLES) the
request is ignored and the image stays correct SDR rather than emitting a washed-out PQ
signal. When HDR10 is live, the normal SDR tonemap is bypassed so the image is not
tonemapped twice.

### 10.4 Pacing: VSync, low latency & adaptive quality

* **VSync** — present synchronized to the display; the Vulkan swapchain picks its
  present mode accordingly, the Direct3D 12 swapchain switches between a sync
  interval of 1 and tear-allowed immediate presentation, and the Metal layer toggles
  `displaySyncEnabled` and its maximum drawable count.
* **Low Latency Mode** — a fence is enqueued at the end of each frame and waited on at
  the start of the next, keeping the CPU from running ahead of the GPU. It trades a
  little throughput for noticeably lower input latency.
* **Adaptive Quality** — holds a target FPS by degrading quality in three steps and
  recovering when there is headroom:

  | Level | Anti-aliasing | Render scale |
  | ----- | ------------- | ------------ |
  | 1 | Step down one notch (SSAA 4×→2×, SSAA 2×→FXAA, any MSAA→FXAA) | unchanged |
  | 2 | Same step-down | −0.15 |
  | 3 | Off | −0.30 (never below 0.25×) |

  Framerate is tracked as an exponential moving average, quality drops below 85 % of
  the effective target and is restored above 97 %. The effective target is the lower of
  your target FPS, the FPS limit, and — when the display caps presentation — the actual
  refresh rate, so a 60 Hz monitor never makes the engine chase 144 FPS. Hitches longer
  than half a second are ignored, and if quality starts oscillating the recovery
  cooldown doubles each time (up to 32 s) so the picture settles instead of pulsing.
  The values you configured are remembered as the baseline and fully restored when the
  load passes.

---

## 11. Cameras & viewports

* The world is viewed through an **orthographic 2D camera**. In the editor it is the
  free editor camera; at runtime it is the entity with a **primary Camera component**.
  The component carries **Ortho Width** (zoom), **Near/Far planes**, a **Background
  Color**, a positional **Offset**, a **shake** state (intensity, duration, current
  offset), a **Player Index** and a normalized **Viewport Rect**.
* **Near/Far clipping planes** bound the orthographic depth range and are configurable
  globally and per level; the defaults are −10 000 and +10 000, giving sprites a very
  wide Z range to sort in.
* **Split-screen** local multiplayer is supported: each local player's camera renders
  into its own viewport rectangle within the window, computed from the player count and
  index. The window is first cleared to a configurable **divider colour** so the seams
  between views are visible, each view is scissored to its rectangle, secondary views
  use their camera's own background colour and their own post-processor and
  widget-overlay framebuffer, and the finished views are blitted into place at the end
  of the frame. Split-screen resources are released as soon as it is switched off.
* **Widget player filtering** lets UI be shown only in the view of the player it
  belongs to.

Camera control APIs (follow, shake, zoom, cinema playback) are scripting features —
see the [Lua API](LuaAPI-EN-DOC.md) and the **Cinema** asset in
[Assets](Assets-EN-DOC.md#415-cinema-ice_cinema).

---

## 12. Debug visualization

The engine draws debug geometry with three dedicated renderers:

* **DebugRenderer** — a lightweight immediate-mode batcher for rectangles, filled
  rectangles, lines, circles, filled circles and arcs. It writes neutral G-buffer
  values so overlays never pollute screen-space effects.
* **DebugSilhouetteRenderer** — draws a textured or solid **silhouette** of a sprite,
  either filled or as an alpha-thresholded **outline**, which is how selection
  highlights and shadow-caster contours are visualized.
* **Physics debug draw** — Box2D's own debug output, wired through the DebugRenderer.

Toggles live in the [Statistics](Profiling-And-Building-EN-DOC.md#34-debug-visualization)
panel and are readable/writable from script by name:

| Overlay | Shows |
| ------- | ----- |
| **Colliders** | Every collider shape, sensor and one-way platform. |
| **Nav grid** / **Nav-grid heatmap** | The AI nav-grid built from view volumes, and its cost heatmap. |
| **Entity markers** | Origins/markers for entities. |
| **Light radius** | Each light's reach circle (and spot cone). |
| **Audio range** | Audio-source attenuation distances. |
| **Camera frustum** | The active camera's visible rectangle. |
| **Joints** | Physics joints, their anchors and limits. |
| **Physics contacts** | Live contact points. |
| **Sleeping bodies** | Which bodies the solver has put to sleep. |
| **Velocity vectors** | Per-body linear velocity arrows. |
| **Tilemap grid** | Tile boundaries of tilemap layers. |
| **FX bounds** | Emitter bounds. |
| **Widget bounds** | UI element rectangles. |
| **Z-depth color** | Colour-codes every draw by its Z, to debug sort order. |
| **Wireframe mode** | Draws geometry as wireframe. |
| **Freeze culling** | Locks the cull frustum in place so you can fly out and inspect it. |
| **Shadow maps** | A preview strip of every generated shadow map (the directional slot is highlighted). |
| **Shadow edges** | The occluder edges the shadow system actually collected. |
| **Light heatmap** | How many lights affect each pixel. |
| **AI state / AI perception / AI paths** | Behaviour-tree state, perception ranges and computed paths. |

The **editor grid** is separate from these — its size, colour and snapping live in
[Preferences → Editor](Editor-EN-DOC.md#104-editor), and it is drawn inside the
geometry pass in Edit mode only.

The runtime debug API (e.g. `ToggleDebugColliders`, `DrawWorldText`, `PrintScreen`,
and the `NetworkProfiler.*` functions) is listed in the editor's
[Hot-Keys → Runtime Debug](Editor-EN-DOC.md#131-hot-keys) panel and documented in the
[Lua API](LuaAPI-EN-DOC.md).

---

## 13. Physics

IceBox simulates 2D physics with **Box2D (v3)**. Each running scene owns a physics
**world**; bodies, shapes and joints are created from the entity's physics
components when Play mode (or the runtime) starts.

### 13.1 The simulation loop

Physics runs on a **fixed timestep** decoupled from the render framerate:

1. Real frame time is added to an **accumulator** (clamped to at most 8 steps so a
   hitch can't trigger a "spiral of death").
2. While the accumulator holds a full step, the world is advanced by exactly
   **Fixed Timestep** seconds with **Sub-step Count** solver iterations
   (`b2World_Step`), contact/sensor/hit events are processed, the `OnFixedUpdate`
   script callback fires, and a step is subtracted.
3. The leftover fraction becomes an **interpolation alpha**. Each body's previous and
   current transforms are blended by that alpha so rendering is **smooth** even though
   the simulation ticks at a fixed rate.

This makes the simulation **deterministic and framerate-independent**: the same fixed
timestep produces the same result whether the game runs at 30 or 240 FPS.

Settings are sanitized before the world is built — the timestep is clamped to
0.001–0.1 s, sub-steps to 1–64, and non-finite values are replaced with defaults — so a
bad value in a level file cannot destabilize the solver.

**Multithreaded solving.** Box2D's work is distributed across a task scheduler
(`PhysicsTaskScheduler`). The worker count includes the main thread and is resolved once
at startup: **Auto** takes CPU cores − 1, capped at 8 on desktop and at half the cores
(max 4) on mobile, with a hard ceiling of 16; an explicit **Physics Worker Threads**
value in [Preferences → Physics](Editor-EN-DOC.md#102-physics) is clamped to the
hardware cap; `1` disables physics multithreading. Because the count is fixed when the
world is created, changing it requires an engine restart, it cannot be overridden per
level, and Web builds always run single-threaded. It pays off in scenes with many awake
bodies and contacts. The count actually in use, plus the step/collide/solve breakdown,
is reported in the [Statistics panel](Profiling-And-Building-EN-DOC.md#33-renderer).

**Pixels-Per-Meter (PPM)** converts between world (pixel) units and Box2D meters; it,
the timestep, sub-steps and gravity are configured in
[Preferences → Physics](Editor-EN-DOC.md#102-physics) and overridable per level in
[World Settings](Editor-EN-DOC.md#8-world-settings).

### 13.2 Bodies

The **Rigidbody** component creates a Box2D body:

| Body type | Behavior |
| --------- | -------- |
| **Static** | Never moves (walls, ground). Infinite mass. |
| **Kinematic** | Moved by script/velocity, unaffected by forces/collisions (moving platforms). |
| **Dynamic** | Fully simulated — gravity, forces, collisions. |

Body properties include **Fixed Rotation**, **Gravity Scale**, **Linear/Angular
Damping**, **Bullet** (continuous collision detection for fast objects), and **Allow
Sleep** (let idle bodies stop simulating to save CPU).

### 13.3 Colliders & materials

The **Collider** component attaches shapes to a body. Shape types:

* **Box** (rectangle), **Circle** (sphere), **Capsule** — primitive shapes authored
  directly on the component, each with its own local transform, so one entity can carry
  any mix of them.
* **Polygon / Chain** — convex polygons from a **sprite's collision polygon**, and
  chains/polygons from **tileset tiles** (merged efficiently across the tilemap).

Each shape carries a physics **material**:

| Property | Meaning |
| -------- | ------- |
| **Density** | Drives the body's mass (with the shape area). |
| **Friction** | Resistance to sliding along surfaces. |
| **Restitution** | Bounciness (0 = no bounce, 1 = perfectly elastic). |
| **Is Sensor** | Detects overlaps but produces no physical response (triggers, pickups, zones). |

Collider shapes double as **shadow casters** — every shape carries the Cast Shadow /
Shadow Origin / Shadow Z Order / Shadow Edge Fade / Don't Block Shadows settings
described in [7.2](#72-shadow-casters). Shapes also follow entity **flipping**: the
collider set is rebuilt when an entity's horizontal flip changes, so mirrored
characters keep matching collision.

### 13.4 Collision filtering (groups)

Which shapes can collide is controlled by **collision groups** and a **collision
matrix**, edited in [Preferences → Collision](Editor-EN-DOC.md#103-collision):

* Define named groups (e.g. *Player*, *Enemy*, *Projectile*, *World*).
* The matrix decides which groups collide with which.
* Each collider is assigned a group index (and a per-shape enabled mask), so you can
  make, say, enemy projectiles pass through other enemies but hit the player and the
  world. The configuration is saved to `Config/CollisionGroups.json`.

### 13.5 Contacts, sensors & events

Each physics step produces events the gameplay layer can react to:

| Event | Fires when |
| ----- | ---------- |
| **Contact begin / end** | Two solid shapes start / stop touching. |
| **Sensor begin / end** | A shape enters / leaves a sensor. |
| **Hit** | A contact exceeds the **Hit-Event Threshold** impact speed (for impact sounds, damage). |
| **Pre-solve** | Just before a contact is resolved — used to *cancel* contacts (the mechanism behind one-way platforms). |

Each shape individually enables the event kinds it cares about (contact / sensor /
hit / pre-solve), so the engine only does the work you ask for. The script-side event
callbacks are documented in the [Lua API](LuaAPI-EN-DOC.md).

### 13.6 One-way platforms

Colliders and tiles can be flagged as **one-way platforms** with a pass-through
**direction** — **Up**, **Down**, **Left** or **Right**, naming the side that is solid.
Using the **pre-solve** callback, the engine compares the contact normal against that
solid direction (rotated with the platform's body, so tilted and rotating one-ways work
too) and cancels contacts arriving from the "open" side, so a character can jump up
through a platform and land on top — the classic platformer behavior.

### 13.7 Joints, queries & destruction

**Joints.** The **Joint** component connects two bodies. Six types are available:

| Type | Constrains |
| ---- | ---------- |
| **Revolute** | A hinge about a shared anchor — doors, wheels, ragdoll limbs. |
| **Distance** | Keeps two anchors a fixed distance apart — ropes, chains, springs. |
| **Weld** | Rigidly fuses two bodies, optionally with spring softness. |
| **Prismatic** | Sliding along a single axis — lifts, pistons, sliding doors. |
| **Wheel** | A suspension axis plus free rotation — vehicles. |
| **Motor** | Drives one body towards a target offset/angle relative to another. |

Every joint supports **Collide Connected**, a **spring** (hertz + damping ratio, and
separate linear/angular hertz and damping for motor joints), a **limit** (lower/upper),
and a **motor** (speed, max torque, max force), plus a reference angle and local axis
where the type uses one. **Break Force** and **Break Torque** let a joint snap under
load and mark itself broken. Joints can target another entity by tag or UUID and even a
named body **part**; a joint whose target does not exist yet is resolved when it
appears.

**Queries.** The world can be probed without stepping it: **line traces** (single, multi,
by tag, by direction, at the cursor) and **shape casts** (box, rotated box, circle,
capsule — single and multi), plus **overlap** tests (box, rotated box, circle, capsule,
at a screen point or cursor) and screen-rectangle entity selection. All of them respect
collision groups and accept a layer-mask override.

**Destruction.** The **Destructible** component fractures geometry into physical debris
on impact. Sprites, flipbooks and tilemap tiles can all be
fractured, with a **Grid**, **Radial** or **Random** fragment pattern and a configurable
fragment count. Every fragment is a real physics body and inherits a full set of
overridable properties: lifetime and fade time, gravity scale, density, friction,
restitution, sensor flag, which contact/sensor/hit/pre-solve events it raises, its
collision group, and its own shadow settings (cast, origin, edge fade, Z order, blocking).
Tiles can carry per-tile overrides for any of these.

These are configured via components ([Editor → Properties](Editor-EN-DOC.md#72-components))
and driven from script ([Lua API](LuaAPI-EN-DOC.md)).

### 13.8 World settings reference

The physics world is created from these settings (global in
[Preferences → Physics](Editor-EN-DOC.md#102-physics), overridable per level in
[World Settings](Editor-EN-DOC.md#8-world-settings)):

| Setting | Role |
| ------- | ---- |
| **Pixels-Per-Meter** | World-unit ↔ meter conversion (default 64). |
| **Gravity X / Y** | Global acceleration. |
| **Sub-step Count** | Solver iterations per step, 1–64 (accuracy vs cost). |
| **Fixed Timestep** | Seconds per physics step, 0.001–0.1. |
| **Enable Sleep** | Let idle bodies stop simulating. |
| **Enable Continuous** | Continuous collision detection (anti-tunneling). |
| **Restitution Threshold** | Minimum speed for a bounce to apply. |
| **Hit-Event Threshold** | Minimum impact speed to raise a hit event. |
| **Contact Hertz / Damping Ratio** | Contact solver stiffness/damping. |
| **Max Contact Push Speed** | Cap on separation push-out speed. |
| **Maximum Linear Speed** | Global speed clamp for bodies. |

**Physics Worker Threads** is the exception: it is engine-global, lives only in
[Preferences → Physics](Editor-EN-DOC.md#102-physics), cannot be overridden per level,
and takes effect on restart ([13.1](#131-the-simulation-loop)).

The values in Preferences are stored in `Config/Engine.json` → `Physics` and are the
**project defaults** in the same sense as the rendering ones ([6.5](#65-where-lighting-and-shadow-settings-come-from)):
the shipped runtime reads them at startup on every platform, and they are re-seeded every
time a level starts. A level that enables **Override Enabled** replaces them for itself
only — leaving that level no longer leaks its physics into the next one, in the editor or
in the build. `Config/Engine.json` → `Audio` works the same way for the audio globals
(gain, Doppler, speed of sound, spatial audio, default attenuation distances) and for the
starting volumes, which the player's `GameSettings.json` then overrides.

---

## 14. Quick reference tables

### 14.1 Render backends

| Backend | Family | Typical platforms |
| ------- | ------ | ----------------- |
| OpenGL 4.6 | GL | Windows, Linux |
| OpenGL 3.3 | GL | Windows, Linux (older GPUs); also the editor's GLES-preview substitute |
| OpenGL ES 3.2 | GL | Android |
| WebGL 2.0 | GL | Web |
| Metal (ANGLE) | GL→Metal | macOS |
| Vulkan | VK | Windows, Linux, Android |
| Direct3D 12 | D3D12 | Windows |
| Metal | Metal (native) | macOS, iOS |
| Metal (MoltenVK) | VK→Metal | macOS, iOS |
| WebGPU | WGPU | Web |
| Null (headless) | Null | Dedicated servers on every platform |

### 14.2 Platforms & selectable renderers

| Platform | Renderers | Fallback chain (highest → lowest) |
| -------- | --------- | -------------------------------- |
| Windows | OpenGL 4.6, OpenGL 3.3, Vulkan, Direct3D 12 | Direct3D 12 → Vulkan → OpenGL 4.6 → OpenGL 3.3 |
| Linux | OpenGL 4.6, OpenGL 3.3, Vulkan | Vulkan → OpenGL 4.6 → OpenGL 3.3 |
| Android | OpenGL ES 3.2, Vulkan | Vulkan → OpenGL ES 3.2 → OpenGL ES 3.0 |
| Web | WebGPU, WebGL 2.0 | WebGPU → WebGL 2.0 |
| macOS | Metal, Metal (ANGLE), Metal (MoltenVK) | Metal → Metal (MoltenVK) → Metal (ANGLE) |
| iOS | Metal, Metal (MoltenVK) | Metal → Metal (MoltenVK) |

Any of the six can additionally run **headless** on the Null renderer via `--headless`.

### 14.3 Render passes

| Graph | Pass | Reads | Produces |
| ----- | ---- | ----- | -------- |
| Scene | Shadows | — | `ShadowMaps` |
| Scene | FX.Setup | `ShadowMaps` | (binds FX) |
| Scene | Geometry | `ShadowMaps` | `SceneColor` (+ G-buffer) |
| Post | PostProcess | `SceneColor` | `FinalColor` (when active) |
| Post | Composite | `SceneColor` | `FinalColor` (when no post-process) |

### 14.4 Lighting & shadow quality

| Shadow Ray Quality | Shadow-map resolution |
| ------------------ | --------------------- |
| Off | shadows disabled |
| Low | 180 |
| Medium | 360 |
| High | 720 |
| Ultra | 1080 |

The directional light gets its own slot at a boosted resolution (up to 2048).

| GI Quality | Rays / texel | Resolution scale | Denoiser passes |
| ---------- | ------------ | ---------------- | --------------- |
| Low | 4 | 0.40× | 1 |
| Medium | 8 | 0.55× | 2 |
| High | 14 | 0.75× | 2 |
| Ultra | 24 | 1.00× | 3 |

### 14.5 Light limits

| Limit | Value |
| ----- | ----- |
| Hard cap (UBO array size) | 128 |
| Active lights (default) | 32 |
| Shadow-map array slots | 4, grown on demand |
| Cookie atlas | 32 layers × 256×256 |

### 14.6 Blend × shading modes

| | Unlit | Lit |
| --- | --- | --- |
| **Masked** | ✓ | ✓ |
| **Additive** | ✓ | ✓ |
| **Translucent** | ✓ | ✓ |
| **Opaque** | ✓ | ✓ |

### 14.7 Physics body types

| Type | Moves? | Affected by forces? |
| ---- | ------ | ------------------- |
| Static | No | No |
| Kinematic | By script | No |
| Dynamic | Yes | Yes |

### 14.8 Joint types

| Type | Constrains |
| ---- | ---------- |
| Revolute | Rotation about a shared anchor |
| Distance | Fixed separation between anchors |
| Weld | Rigid fusion |
| Prismatic | Sliding along one axis |
| Wheel | Suspension axis + free spin |
| Motor | Driven relative offset/angle |

---

## 15. FAQ & troubleshooting

**My lights don't do anything.**
Lighting only applies in **Lit** mode, and only to sprites whose shading mode is
**Lit**. Set the world to Lit in [Preferences → Rendering](Editor-EN-DOC.md#105-rendering)
(or [World Settings](Editor-EN-DOC.md#8-world-settings)), and make sure your sprites/
materials are Lit.

**My lights work but there are no shadows.**
Enable **2D Shadows** in Rendering settings, set the light's **Cast Shadows** flag, and
tick **Cast Shadow** on the occluders. In the default **Colliders** cast mode the
silhouette comes from collider geometry, so an occluder with no collider casts nothing —
either give it a collider or switch its **Cast Shadow Mode** to **Contour**.

**My shadows stop at the wrong height, or an object doesn't shadow another.**
Shadows are height-aware: a receiver whose Z is at or above the occluder's **Shadow Z
Order** stays lit. Check the caster's **Shadow Origin** (Bottom/Center/Top) and its
Shadow Z Order against the receiver's Z. If a shadow falls through a floor it should
not, clear **Don't Block Shadows** on the floor (or turn on **Colliders Block
Shadows**) so it blocks.

**The shadow doesn't match the artwork.**
Set the caster's **Cast Shadow Mode** to **Contour**, which traces the texture's alpha
outline instead of using the collider shape. **Colliders** mode is cheaper; **Contour**
is exact.

**Ray tracing / GI is greyed out or missing.**
The world ray-traced GI requires a **Vulkan** device with hardware ray tracing
(`VK_KHR_ray_query` + acceleration structures), a **Direct3D 12** adapter with DirectX
Raytracing 1.1 and a shader-model-6.5 compiler, or a **Metal** device that reports
`supportsRaytracing` with MSL 2.4 ray queries. On any other backend or on hardware
without it, the setting reads as unsupported and the scene uses direct lighting + 2D
shadows. The separate **Global Illumination** effect in a post-process volume
(radiance cascades) has no such requirement — see [Section 8](#8-ray-traced-global-illumination).

**Performance drops with many sprites.**
Raise the batch sizes and enable GPU culling in
[Preferences → Optimization](Editor-EN-DOC.md#106-optimization), reduce **Render
Scale**, and prefer **Masked/Opaque** over **Translucent** where overlap sorting
isn't needed. Use the [Statistics](Profiling-And-Building-EN-DOC.md) panel and the
[Profiler](Profiling-And-Building-EN-DOC.md) to find the cost.

**Things look soft/blurry or aliased.**
Adjust **Render Scale** and **Anti-aliasing** in
[Preferences → Engine](Editor-EN-DOC.md#101-engine). For crisp pixel art, use
**Nearest** texture filtering (per-texture; see [Assets](Assets-EN-DOC.md)) and
avoid mipmaps.

**Fast objects pass through walls.**
Enable **Bullet** (CCD) on the moving body and **Enable Continuous** on the world,
and consider a higher **Sub-step Count**.

**Physics feels jittery.**
The renderer interpolates bodies between fixed steps, so jitter usually means a body
is being teleported every frame (e.g. set directly each frame bypassing the solver),
or the **Fixed Timestep** is very large. Move bodies with velocities/forces and keep
a sensible timestep (e.g. 1/60).

**A character can't jump through a platform.**
Flag that collider/tile as a **one-way platform** with the correct pass-through
direction (Section [13.6](#136-one-way-platforms)).

**How do I run a dedicated server with no GPU?**
Start the runtime with `--headless`. The engine selects the **Null** renderer, skips
window/context/audio creation, and runs physics, scripts, FX and networking normally.
See [2.2](#22-backends--platforms).

**My post-process volume settings do nothing.**
The volume must have both **Post-Process Volume Enabled** and the post-process
**Enabled** flag set, and the camera *centre* must be inside its bounds (or the view
must be marked infinite/unbounded). If two volumes overlap, the higher **Priority**
wins and the **Blend Radius** controls the crossfade.

**HDR10 is on but the picture looks the same (or washed out).**
Real HDR10 signalling needs the **Vulkan**, **Direct3D 12** or **Metal** backend in a
standalone build, on an HDR/EDR display with HDR enabled in the OS. Everywhere else — including the editor viewport —
the request is deliberately ignored and a correct SDR image is shown instead of a
washed-out PQ signal. See [10.3](#103-hdr10-output).

**My UI is blurry when Render Scale is low.**
It should not be: UI drawn after the scene is composited at full resolution over the
post-processed image. If a specific widget is blurry, check that it is not part of the
world-Z background stage, which renders with the scene.

**How do I show a sprite only inside a shape?**
Use the **Stencil** component — one object in **Write** mode stamps the mask, the other
in **Read** mode with the same **Stencil ID** is clipped to it. See
[4.7](#47-stencil-masking).

**Where do I author materials, views (post-process) and FX?**
In the asset editors — see [Assets](Assets-EN-DOC.md). This document explains how the
renderer *uses* them; the assets themselves are authored there.

---

<sub>© IceBoxCrew Studio. All rights reserved. See [`LICENSE.txt`](../../LICENSE.txt) for full terms.</sub>
