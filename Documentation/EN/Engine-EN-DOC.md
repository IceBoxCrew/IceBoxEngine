# ⚙️ IceBox Engine — Engine, Multiplayer & Physics

## Full documentation in English

### Actual for PR-0.9.1 Version

> This document covers the parts of **IceBox Engine** that live *below* the editor and
> *beside* the scripting API: how the runtime is put together, how **multiplayer** works
> (both **split-screen / local** and **online**), and the parts of **physics** that the
> graphics document does not reach.
>
> It is deliberately **complementary**, never a repeat:
>
> * Renderers, lighting, shadows, post-processing, and the **fundamentals of the physics
>   world** (bodies, colliders, joints, one-way platforms, the fixed timestep, world
>   settings) are in [Graphics, Rendering & Physics](Graphics-EN-DOC.md).
> * Every panel, menu, preference and the **Network Manager** test harness are in
>   [Editor & Interface](Editor-EN-DOC.md).
> * Asset types and their editors are in [Assets & Content Browser](Assets-EN-DOC.md).
> * The **scripting surface** — every `Network.*`, `Rollback.*`, `LocalPlayer.*`,
>   physics, audio and input function you call from gameplay code — is in the
>   [Lua API](LuaAPI-EN-DOC.md) and [Python API](PythonAPI-EN-DOC.md). This document
>   explains *what the engine does*, not *how to call it*.
> * Profilers, the developer console, headless servers and shipping are in
>   [Profiling & Building Games](Profiling-And-Building-EN-DOC.md).
>
> Where a topic belongs to another document, this one links to it instead of restating it.
> [Section 9](#9-where-everything-is-documented) is a map of the whole set.

---

## 📑 Table of Contents

1. [Overview & scope](#1-overview--scope)
2. [Runtime architecture](#2-runtime-architecture)
   - 2.1 [Modes: editor, standalone, headless](#21-modes-editor-standalone-headless)
   - 2.2 [The frame, step by step](#22-the-frame-step-by-step)
   - 2.3 [Inside the scene update](#23-inside-the-scene-update)
   - 2.4 [Scenes, entities & components](#24-scenes-entities--components)
   - 2.5 [The job system](#25-the-job-system)
   - 2.6 [Time, pause & suspend](#26-time-pause--suspend)
   - 2.7 [Determinism & random streams](#27-determinism--random-streams)
3. [Physics — beyond the basics](#3-physics--beyond-the-basics)
   - 3.1 [What this section adds](#31-what-this-section-adds)
   - 3.2 [Collision modes, per shape](#32-collision-modes-per-shape)
   - 3.3 [Separate bodies — multi-body entities](#33-separate-bodies--multi-body-entities)
   - 3.4 [Generated collision: sprites, flipbooks & tilemaps](#34-generated-collision-sprites-flipbooks--tilemaps)
   - 3.5 [How collision events reach your code](#35-how-collision-events-reach-your-code)
   - 3.6 [Joints at runtime, targets & breaking](#36-joints-at-runtime-targets--breaking)
   - 3.7 [Ragdolls & bone physics](#37-ragdolls--bone-physics)
   - 3.8 [Interpolation, teleports & threading](#38-interpolation-teleports--threading)
   - 3.9 [Live edits: collision groups & flipping](#39-live-edits-collision-groups--flipping)
   - 3.10 [Physics quick reference](#310-physics-quick-reference)
4. [Local multiplayer & split-screen](#4-local-multiplayer--split-screen)
   - 4.1 [Local players & input devices](#41-local-players--input-devices)
   - 4.2 [Layouts & viewport rectangles](#42-layouts--viewport-rectangles)
   - 4.3 [Cameras, UI & audio per player](#43-cameras-ui--audio-per-player)
   - 4.4 [What runs where](#44-what-runs-where)
   - 4.5 [Split-screen quick reference](#45-split-screen-quick-reference)
5. [Online multiplayer](#5-online-multiplayer)
   - 5.1 [Architecture & transports](#51-architecture--transports)
   - 5.2 [Session lifecycle](#52-session-lifecycle)
   - 5.3 [Channels & message types](#53-channels--message-types)
   - 5.4 [Entity state & world snapshots](#54-entity-state--world-snapshots)
   - 5.5 [Delta compression & area of interest](#55-delta-compression--area-of-interest)
   - 5.6 [Time sync, interpolation, prediction & lag compensation](#56-time-sync-interpolation-prediction--lag-compensation)
   - 5.7 [Automatic replication](#57-automatic-replication)
   - 5.8 [Rollback netcode](#58-rollback-netcode)
   - 5.9 [Chat & voice](#59-chat--voice)
   - 5.10 [Server discovery & NAT traversal](#510-server-discovery--nat-traversal)
   - 5.11 [Security, validation & moderation](#511-security-validation--moderation)
   - 5.12 [Dedicated servers & web clients](#512-dedicated-servers--web-clients)
   - 5.13 [Network quick reference](#513-network-quick-reference)
6. [The audio engine](#6-the-audio-engine)
7. [The input stack](#7-the-input-stack)
8. [Other runtime services](#8-other-runtime-services)
9. [Where everything is documented](#9-where-everything-is-documented)
10. [FAQ & troubleshooting](#10-faq--troubleshooting)

---

## 1. Overview & scope

IceBox is one engine compiled into several front-ends. The **editor** embeds the whole
runtime so **PLAY** is the real game; a **standalone build** is the same runtime without
the editor UI; a **headless** run is the same standalone binary with the renderer replaced
by a no-op backend. Everything in this document is shared by all three unless it says
otherwise.

The engine is built on a small set of third-party foundations, and knowing which is which
explains a lot of behaviour:

| Area | Foundation | Consequence |
| ---- | ---------- | ----------- |
| **Windowing, input, audio devices** | **SDL3** | One event pump feeds keyboard, mouse, gamepads, joysticks, touch, pen, sensors and haptics on all six platforms. |
| **Entities** | **EnTT** registry | Entities are integer handles; components are stored in packed arrays and iterated as views. |
| **Physics** | **Box2D v3** | Fixed-step solving, shape-level events, multithreaded solve. |
| **Audio** | **miniaudio** | One engine object with multiple listeners, node-graph effects and streaming decoders. |
| **Online transport** | **ENet** (native) / **WebSocket** (Web) | Reliable and unreliable delivery over UDP, with a WebSocket path for browsers. |
| **Crypto** | **libsodium** | Packet AEAD, password hashing, nonces and secure random. |
| **Voice** | **Opus** (when compiled in) | Encoded voice frames instead of raw PCM. |

Everything else — replication, rollback, split-screen, ragdolls, the job system — is
engine code, and that is what the rest of this document describes.

---

## 2. Runtime architecture

### 2.1 Modes: editor, standalone, headless

| Mode | How it starts | What is different |
| ---- | ------------- | ----------------- |
| **Editor** | The `IceBoxEngine` application; the scene renders into a framebuffer that the Viewport panel displays. | The full ImGui workspace, the Python interpreter, the asset watchers and the editor undo stacks exist. Play mode runs on a scene snapshot that is restored on STOP (see [Editor → Play, Pause & Eject](Editor-EN-DOC.md#45-play-pause--eject)). |
| **Standalone** | A packaged game; the scene renders straight to the window. | No editor UI, no Python. Split-screen, the developer console and the runtime overlays are active here. |
| **Headless** | The standalone binary with `--headless` (or `-headless`). | The **Null** RHI backend is selected, no window/context/audio device is created, and `Render()` is replaced by an FX-only update. Everything else ticks normally. Details in [Profiling & Building → Headless](Profiling-And-Building-EN-DOC.md#headless-dedicated-server). |

Two build-time distinctions matter for behaviour described later:

* **Editor vs. standalone** — split-screen rendering and split-screen UI input routing are
  compiled into the standalone runtime only ([4.4](#44-what-runs-where)).
* **Debug vs. Release** — Debug standalone builds carry the profiler and network overlays
  and Tracy instrumentation; Release strips them
  ([Profiling & Building → 5.6](Profiling-And-Building-EN-DOC.md#56-profiling-a-shipped-build)).

### 2.2 The frame, step by step

One iteration of the main loop, in order:

1. **Frame mark & profiler start** — the Tracy frame boundary is emitted and the scope
   profiler starts a new frame.
2. **`ProcessInput()`** — the SDL event queue is drained into the [input state](#7-the-input-stack)
   and window events (resize, focus, close) are applied.
3. **Suspend check** — if the app is suspended ([2.6](#26-time-pause--suspend)) the loop
   waits on an event with a 100 ms timeout and starts over, so a backgrounded game costs
   nothing.
4. **`Update()`** — gameplay: scene simulation, replication, UI, cameras
   ([2.3](#23-inside-the-scene-update)).
5. **Prewarm tick** — the [project prewarm](Graphics-EN-DOC.md#24-the-shader-pipeline--caches)
   consumes its per-frame time budget.
6. **Pending texture flush** — textures decoded on worker threads are uploaded to the GPU
   on the main thread, where a GPU context exists.
7. **`Render()`** — the render graphs execute ([Graphics → 3.2](Graphics-EN-DOC.md#32-the-frame-step-by-step)).
   In headless mode this step is replaced by an FX-only update so particle lifetimes still
   expire.
8. **Input roll-over** — per-frame edge state (*just pressed* / *just released*), scroll
   and touch deltas are rolled over for the next frame.
9. **Counters** — entity/component statistics and the profiler are updated.
10. **Networking** — `Network.*`, `Rollback.*` and LAN discovery are ticked, in that
    order, followed by the network profiler in Debug builds.
11. **Applied settings** — the editor pushes its live Preferences (FPS target, clipping
    planes, backend, lighting) into the engine; a level's rendering override is re-applied
    if it is enabled.
12. **Frame pacing** — if a target FPS is set, the loop sleeps the remaining time. On
    desktop it sleeps to within ~1.5 ms and then spins to hit the deadline precisely; on
    Android and iOS it sleeps the whole remainder; on Web the browser owns the cadence and
    the limiter is skipped.

> **Networking ticks after rendering.** Networking runs at the end of the
> frame, so a snapshot always reflects the state the frame just finished producing. It
> also has its own accumulator: it only does work once per **Tick Rate** interval
> ([5.4](#54-entity-state--world-snapshots)), regardless of your framerate.

### 2.3 Inside the scene update

`Update()` in Play mode does, in order:

1. **Resolve the game delta** — the real frame delta is multiplied by the accessibility
   **Game Speed** ([2.6](#26-time-pause--suspend)). The unscaled delta is kept separately
   for UI, debug text and timers that must ignore slow-motion.
2. **Pause requests** — a pause requested from script or by `F5` in the editor is
   consumed, and the resulting state is published so every subsystem sees the same flag.
3. **Scene simulation** — run on the game delta; detailed below.
4. **Replication** — replication runs *after* the simulation, on the
   **unscaled** delta, so slow-motion never changes network rates
   ([5.7](#57-automatic-replication)).
5. **Widgets** — the UI runtime updates and then processes pointer/text input, with
   [split-screen routing](#43-cameras-ui--audio-per-player) in standalone builds.
6. **Camera** — a playing cinema takes the camera; otherwise a free (ejected) camera or
   the primary Camera component drives it, with optional camera lag; then camera shake is
   decayed for the primary and every split-screen camera.

The scene simulation itself runs these stages, each of which appears by name in the
[Profiler](Profiling-And-Building-EN-DOC.md#43-cpu-tab):

| Stage | What happens |
| ----- | ------------ |
| *(collision groups)* | If the collision matrix changed since last frame, every live shape's filter is rewritten ([3.9](#39-live-edits-collision-groups--flipping)). |
| **`Update.Physics`** | The fixed-timestep loop: `b2World_Step`, contact/sensor/hit event dispatch, transform read-back, part-body sync, interpolation bookkeeping, joint-break checks, and `OnFixedUpdate` — repeated until the accumulator is drained, then render interpolation is applied. |
| **`Update.TilemapAnimColliders`** | Animated tiles advance and their collider shapes are rebuilt when the visible frame changes. |
| **`Update.Scripts`** | `OnUpdate` for every scripted entity and the level script. |
| **`Update.BehaviorTree`** | AI behavior trees tick. |
| **`Update.Perception`** | AI perception (sight/hearing) updates — parallelized across worker threads at 8 or more perceiving agents. |
| **`Update.Hierarchy`** | Parent→child transforms are propagated. |
| **`Update.Audio`** | Listener position(s) and every spatial audio source position are pushed to the audio engine ([4.3](#43-cameras-ui--audio-per-player), [6](#6-the-audio-engine)). |
| **`Update.Debris`** | Destruction fragments age, fade and expire. |
| *(animation)* | Flipbooks, animators and skeletons advance and resolve their frames. |
| **`Update.SocketAttachments`** | Entities attached to bones/sockets are placed. |
| *(late)* | `OnLateUpdate` for scripts, then socket attachments are resolved **a second time** so anything a late-update moved is still correctly attached. |
| *(sync)* | Sprite/flipbook collision shapes bound to physics bodies are re-synchronized. |

### 2.4 Scenes, entities & components

A **scene** is an ECS world plus a Box2D physics world. An **entity** is a handle into that
world; everything about it lives in components:

* Every entity carries an **`IDComponent`** (a 64-bit **UUID**, stable across save/load and
  used by joints, replication and replays), a **`TagComponent`** (its name) and a
  **`TransformComponent`**.
* **Hierarchy** is a component, not a container: a child stores its parent and its local
  transform, and the engine walks the tree each frame. Destroying a parent destroys its
  children with it.
* **Multi-instance components.** Sprite Renderer, Flipbook, Collider, Audio, FX, Widget,
  Joint, Point Marker and Class Component each hold a **list of named instances** with
  their own local transforms — one entity can carry several sprites, several colliders and
  several joints without extra entities.
* **Destroy signals** clean up non-registry resources automatically: destroying an entity
  releases its FX emitters, stops its sounds and detaches its children.
* The set of components an entity has is defined by its **class** (`.ice_class`), which is
  why the Properties panel edits components but never adds them — see
  [Editor → Components](Editor-EN-DOC.md#72-components) and
  [Assets → Class](Assets-EN-DOC.md#417-class-ice_class).

Component *contents* are documented per component in the
[Editor](Editor-EN-DOC.md#72-components) and [Assets](Assets-EN-DOC.md) documents; the
scripting view of entities is in the [Lua API](LuaAPI-EN-DOC.md).

### 2.5 The job system

A single global **thread pool** is created at startup with `hardware_concurrency − 1`
workers (minimum 2), and is shut down last. It offers two primitives: `Submit` for a
one-off task that returns a future, and `ParallelFor`, which splits a range into chunks
across the workers and blocks until they finish.

The pool is used opportunistically — every caller checks a **threshold** first and falls
back to a plain serial loop below it, so small scenes never pay for synchronization:

| Work | Runs in parallel when |
| ---- | --------------------- |
| Physics transform read-back | ≥ 64 rigidbodies |
| AI perception | ≥ 8 perceiving agents |
| Particle simulation | ≥ 256 particles (per emitter) |
| Emitter updates | ≥ 4 emitters |
| Fluid (SPH) neighbour/force passes | ≥ 128 particles |
| Sprite culling & sort | chunked across all workers |
| CPU shadow ray casts, edge collection | chunked across all workers |
| Texture decode / async asset loads | always (via `Submit`) |

On Web the pool exists but has **zero workers** unless the build enables pthreads, in
which case it is capped at 4; every call site therefore has to work single-threaded, and
does.

> Box2D's own solver threading is **separate** — it uses the physics task scheduler
> configured by **Physics Worker Threads**, described in
> [Graphics → 13.1](Graphics-EN-DOC.md#131-the-simulation-loop).

### 2.6 Time, pause & suspend

Three independent multipliers sit between the wall clock and gameplay:

| Layer | Set by | Affects |
| ----- | ------ | ------- |
| **Real frame delta** | The frame loop | The editor, UI, debug overlays, network timing. Never scaled. |
| **Game Speed** | [Preferences → Accessibility](Editor-EN-DOC.md#108-accessibility) (0.25×–2×) | The delta handed to the scene simulation in Play mode. A player-facing accessibility setting. |
| **Time Scale** | Gameplay script | Scaled *again* on top, for slow-motion and pause effects; per-entity scales exist as well. |

When the effective scale reaches zero the engine broadcasts **`OnPause`** to scripts, and
**`OnResume`** when it leaves zero — so systems can stop and restart cleanly instead of
polling.

**Suspend** is a different thing: with **Is Suspended** on (the default, in
[Preferences → Optimization](Editor-EN-DOC.md#106-optimization)) the app stops updating,
rendering and playing audio whenever the window loses focus, is minimized or is
backgrounded, on all six platforms. The loop then blocks on an event wait instead of
spinning. A dedicated server never suspends, which is what lets it run unattended.

### 2.7 Determinism & random streams

Determinism matters for [rollback netcode](#58-rollback-netcode), replays and reproducible
procedural generation, so randomness is a service rather than a global:

* A **master seed** derives every named **stream** by hashing the seed together with the
  stream name, so `Stream("loot")` and `Stream("terrain")` are independent and reproducible
  regardless of the order in which they are used.
* Each stream is an **xoshiro256-family** generator with an explicit **state** that can be
  read and restored — which is how a rollback frame can save and reload randomness
  together with the world state.
* The whole service can be **serialized to a string** and reloaded, so a save file can
  restore not only the world but the exact position of every random sequence.

Physics contributes the other half of determinism: it advances in **fixed steps** with a
sanitized timestep and sub-step count ([Graphics → 13.1](Graphics-EN-DOC.md#131-the-simulation-loop)),
so the same inputs produce the same result at any framerate.

---

## 3. Physics — beyond the basics

### 3.1 What this section adds

[Graphics → 13. Physics](Graphics-EN-DOC.md#13-physics) is the primary physics reference:
the fixed timestep and interpolation, body types, collider shapes and materials, collision
groups and the matrix, contacts and sensors, one-way platforms, joint types, queries,
destruction, and the world-settings table.

This section covers what that document does not: the **per-shape collision mode**, entities
made of **several bodies**, how collision geometry is **generated** from sprites and
tilemaps, exactly **how events are delivered** to scripts, **runtime joint** resolution and
breaking, **ragdolls and bone physics**, and the interpolation/threading details that
explain most "why does it look like that" questions.

### 3.2 Collision modes, per shape

Every collider instance — box, sphere (circle) or capsule, and every generated sprite,
flipbook and tile collider — has a **Collision Enabled** mode in addition to its
**Is Sensor** flag and its collision group. The mode is the master switch:

| Mode | Physical blocking | Overlap (sensor) events | Notes |
| ---- | ----------------- | ----------------------- | ----- |
| **NoCollision** | No | No | The shape is never created — nothing in the physics world knows about it, and the [2D shadow system](Graphics-EN-DOC.md#72-shadow-casters) skips it as a caster too. It still exists as authored data. |
| **QueryOnly** | No | Yes | Forced to a sensor with sensor events on — the classic trigger volume. |
| **PhysicsOnly** | Yes | No | Forced solid with sensor events off — cheapest solid collision. |
| **QueryAndPhysics** | Follows **Is Sensor** | Follows **Is Sensor** | The default. The shape behaves exactly as its own Is Sensor flag says. |

Two consequences worth knowing:

* **NoCollision is a build-time skip, not a runtime disable.** Changing it while the game
  runs does not create the missing shape; the shapes are built when the runtime body is
  created. Changing the *group* of an existing shape, on the other hand, is live
  ([3.9](#39-live-edits-collision-groups--flipping)).
* **Chain colliders** (tilemap outlines) support only *PhysicsOnly* and
  *QueryAndPhysics*; a chain set to NoCollision or QueryOnly is skipped, because a Box2D
  chain cannot be a sensor.

> **Collider shadows follow the same rule.** When the shadow system collects casters from
> collider shapes, it skips shapes that are **NoCollision** *and* shapes that are
> **sensors** — a trigger volume never casts a collider-shaped shadow. Contour shadows are
> unaffected, because those are traced from the artwork rather than from the collider; see
> [Graphics → Shadow casters](Graphics-EN-DOC.md#72-shadow-casters).

### 3.3 Separate bodies — multi-body entities

Normally every collider on an entity becomes a **shape on the entity's single rigidbody**.
Ticking **Separate Body** on a collider instead gives that collider **its own dynamic
body**, which is how one entity becomes a jointed assembly — a vehicle with wheels, a
crane with an arm, a character built from limbs — without splitting it into several
entities:

* Each part body is created as **dynamic**, inheriting the entity Rigidbody's gravity
  scale, damping, bullet and sleep settings, positioned from the collider's local
  transform (mirrored when the entity is flipped).
* The entity must have an **active Rigidbody**; without one, the engine logs a warning and
  creates no part bodies at all.
* Part bodies carry the **same user data as the owner entity**, so a contact on a wheel is
  reported as a contact on the entity that owns it.
* Every frame after the solve, `SyncPhysicsPartsToInstances` writes each part body's world
  transform **back into the collider instance's local transform**, so the shapes you see in
  the editor overlays and in the shadow system follow the simulation.
* A **sprite or flipbook instance** can name a part in its **Attach To Collider** field. On
  the first runtime tick the engine records that instance's offset from the part and then
  keeps it glued to it — so the wheel sprite rides the wheel body, and if that instance has
  a collision polygon it is created **on the part body** rather than on the entity body.
* **Joints** target a part by name (see below), which is what actually holds the assembly
  together.

Two helpers exist because parts do not follow the entity transform automatically:
teleporting an entity **shifts** its part bodies by the same delta and wakes them, and
flipping an entity **rebuilds** the parts on the mirrored side.

### 3.4 Generated collision: sprites, flipbooks & tilemaps

Not all collision is authored as primitives. Three sources generate shapes:

**Sprite and flipbook collision polygons.** A sprite's collision polygon (authored in the
[Sprite Editor](Assets-EN-DOC.md#42-sprite-ice_sprite)) is normalized to the sprite's UV
box, so it survives resizing, flipping and pivot changes. At runtime it is converted to
body space and then made convex for Box2D:

* Up to Box2D's maximum vertex count, the polygon becomes **one convex hull**.
* Beyond that, it is **fanned around its centroid** into a group of overlapping hulls that
  together cover the outline — concave silhouettes work, at the cost of several shapes.
* The result is cached against a hash of the polygon, the flip flags and the pivot, plus a
  second hash of the local transform and size. When only the transform changed the shapes
  are **moved** rather than rebuilt; when the polygon itself changed they are recreated.

**Tilemap collision.** Each tilemap instance gets one **static body**, and the tiles on it
are merged so a large map does not become thousands of shapes:

* **Box-shaped tiles** with identical settings are **greedily merged into rectangles** —
  each run is extended as far right as it can go, then as far down as whole rows allow —
  and every merged rectangle becomes a single polygon shape.
* **Polygon-shaped tiles** are merged topologically: every tile edge is inserted into an
  edge table, edges shared by two adjacent tiles **cancel out**, and the surviving edges are
  walked into closed **chain loops**. A solid block of polygon tiles therefore becomes one
  outline instead of one shape per tile.
* Merging only groups tiles that agree on the properties that matter — material, sensor
  flag, one-way direction, event flags and collision group — so a single special tile in a
  run is preserved.
* **Animated tiles** are re-evaluated every frame; when the visible frame changes and its
  collider differs, that tile's shapes are rebuilt in place.

**Skeletons** generate bone bodies, covered in [3.7](#37-ragdolls--bone-physics).

### 3.5 How collision events reach your code

Box2D reports events per **shape**; the engine turns them into per-**entity** callbacks:

1. After each step, a **body→entity map** is rebuilt from every rigidbody, every separate
   part body, every tilemap body and every skeleton bone body. This is what lets a contact
   on a wheel or on a tilemap resolve to the owning entity.
2. For each event, both sides are resolved to entities, their **tags** are read, and the
   **name of the specific collider** that touched is looked up — which is why collision
   callbacks can tell you *"the `Feet` collider touched something tagged `Ground`"* rather
   than just *"something touched me"*.
3. Both directions are dispatched: A is told about B, and B about A. An entity with an
   empty tag is not reported to the other side.

| Event | Delivered as |
| ----- | ------------ |
| **Begin touch** | Collision-enter on both entities, with the other's tag, id and the local collider name. |
| **End touch** | Collision-exit. The pair is remembered while touching, so the exit still carries the right tags and collider names even if a shape was destroyed in between. |
| **Hit** | A collision whose approach speed exceeds the world's **Hit-Event Threshold**, with the speed converted back to world units. This is also what feeds impact-triggered [destruction](Graphics-EN-DOC.md#137-joints-queries--destruction). |
| **Sensor begin/end** | Sensor-enter/exit, dispatched to the sensor and to the visitor. |
| **Stay** | Recomputed each step by walking the set of currently-touching pairs — **only for entities whose script actually implements a stay callback**. If nobody wants them, the whole pass is skipped. |
| **Pre-solve** | Used internally to cancel contacts for [one-way platforms](Graphics-EN-DOC.md#136-one-way-platforms). |

Each shape independently enables the event kinds it wants (contact / sensor / hit /
pre-solve), so you only pay for what you use — and a shape flagged **one-way** gets
pre-solve enabled automatically even if you left the flag off.

The script-side signatures are in the [Lua API](LuaAPI-EN-DOC.md#14-collision--collisions-aabb).

### 3.6 Joints at runtime, targets & breaking

A **Joint** instance names its second body in one of three ways, resolved in this order:

1. **Target Part Name** — a named **Separate Body** collider on the *same* entity
   ([3.3](#33-separate-bodies--multi-body-entities)). If the part does not exist or has no
   body, the joint is skipped with an explicit warning naming the part.
2. **Target Entity UUID** — an exact entity, stable across renames.
3. **Target Entity Tag** — the first entity with that tag. If several match, the engine
   warns with the count and uses the first, so ambiguity is visible rather than silent.

When a joint resolves by tag, the engine **writes the resolved UUID back** into the
instance, so subsequent rebuilds are exact. A joint whose target does not exist yet is left
pending and **retried when a matching entity appears** — which is what makes joints work
when the two halves of an assembly spawn in an unpredictable order.

**Breaking.** Every step, each live joint's constraint force and torque are read back and
compared against its **Break Force** and **Break Torque** (either can be `0` to disable
that test). When one is exceeded the joint's bodies are woken, the joint is destroyed and
flagged broken, and a **joint-break event** is raised to script with the joint's index,
name and target tag — after the whole pass, so destroying things inside the callback is
safe.

Anchors and axes are mirrored when the owning or the target entity is flipped, so a jointed
assembly behaves identically facing left or right.

### 3.7 Ragdolls & bone physics

Skeletal characters can drive physics bodies from their bones, and hand control back and
forth. A skeleton asset defines **physics bones** (shape, size, offset, material, collision
group, self-collide flag, joint limits and motor torque, break force); the runtime turns
them into bodies in one of two regimes:

| Regime | Bodies | Purpose |
| ------ | ------ | ------- |
| **Bone colliders** (`Bone Colliders Enabled`) | **Kinematic** | The animation drives the bodies. Each frame the engine computes the bone's target world transform and sets the body's **velocity** to reach it in one step (snapping instead if it is far away or the frame delta is degenerate). Result: hitboxes that follow the animation exactly and can be hit by traces and projectiles, without disturbing the animation. |
| **Ragdoll** (`Ragdoll Enabled`) | **Dynamic** | Physics drives the bones. Bodies fall under gravity with the component's ragdoll gravity scale and angular damping. |

Between the two sits the **ragdoll blend** (0–1), which is what makes an *active* ragdoll:

* Bones are chained to their nearest physics-bone ancestor with **revolute joints** whose
  limits come from the asset and whose **motor torque is scaled by `(1 − blend)`** — at
  blend 0 the motors are at full strength and hold the animated pose, at blend 1 they are
  free and the character goes fully limp.
* While blended, each joint's motor is continuously driven toward the *animated* deviation
  of that bone, so the character keeps trying to reach the animation while still reacting
  to impacts.
* A physics bone marked **not self-colliding** is placed in a filter group shared by the
  whole skeleton and derived from its entity, using Box2D's "never collide with each other"
  convention — so limbs pass through each other while still colliding with the world. The
  owning entity's own controller collider is temporarily excluded the same way, and its
  original filter is restored when the ragdoll ends.
* Bones can be **severed** (dismemberment) — the joint is destroyed and that bone's body
  detaches from the chain.

Rigidbodies have their own, simpler ragdoll toggle: it remembers the body's original
**fixed rotation**, **gravity scale** and **angular damping**, replaces them with the
ragdoll values, and restores them exactly when the ragdoll is switched off.

Skeleton bone bodies are included in the body→entity map, so a bullet that hits a forearm
reports a hit on the character.

### 3.8 Interpolation, teleports & threading

The renderer draws bodies interpolated between the previous and current fixed steps
([Graphics → 13.1](Graphics-EN-DOC.md#131-the-simulation-loop)). Two details in that
mechanism matter in practice:

* **Teleport detection.** After each step, the distance a body actually moved is compared
  against what its velocity could plausibly have produced in one step (plus a one-metre
  slack). A body that moved further is treated as **teleported**: its previous transform is
  set equal to its new one, so it snaps instead of smearing across the screen. This is why
  `SetPosition`-style moves look instant instead of sliding.
* **Body generation tracking.** Interpolation state is keyed by body index *and*
  generation. When a body is destroyed and its slot reused, the stale state is discarded
  instead of interpolating the new body from the old one's position.

**Threading.** Reading transforms back out of Box2D is parallelized across the
[job system](#25-the-job-system) at **64 or more rigidbodies** and runs serially below
that. The read-back is safe to parallelize because each entity writes only its own
transform. Everything else in the physics stage — event dispatch, part sync, joint checks —
runs on the main thread.

### 3.9 Live edits: collision groups & flipping

Two kinds of change are applied to a *running* world instead of requiring a restart:

**Collision groups.** The collision-group manager carries a **generation counter**. When
you edit groups or the matrix in [Preferences → Collision](Editor-EN-DOC.md#103-collision),
the counter bumps, and on the next scene update every live shape — collider shapes, sprite
and flipbook polygon shapes, and skeleton bone shapes — has its category and mask bits
rewritten from the new configuration. A shape whose mode is **NoCollision** gets an empty
mask, so it stays inert.

**Flipping.** Flipping an entity horizontally mirrors its collision: the collider set is
rebuilt on the mirrored side, part bodies are recreated, and joint anchors and axes are
mirrored to match. The engine tracks the last flip sign it built for, so this happens
exactly once per flip rather than every frame.

### 3.10 Physics quick reference

| Concept | Where it is configured | Notes |
| ------- | ---------------------- | ----- |
| Collision mode | Per collider / per sprite polygon / per tile | NoCollision, QueryOnly, PhysicsOnly, QueryAndPhysics ([3.2](#32-collision-modes-per-shape)) |
| Separate Body | Per collider instance | Own dynamic body; needs an active Rigidbody ([3.3](#33-separate-bodies--multi-body-entities)) |
| Attach To Collider | Per sprite/flipbook instance | Glues the visual to a named part body |
| Joint target | Per joint instance | Part name → UUID → tag, in that order ([3.6](#36-joints-at-runtime-targets--breaking)) |
| Break Force / Break Torque | Per joint instance | `0` disables that test; raises a break event |
| Bone Colliders Enabled | Skeleton component | Kinematic hit bodies driven by animation |
| Ragdoll Enabled + blend | Skeleton component | Dynamic bodies; blend scales joint motor torque |
| Parallel transform read-back | Automatic | ≥ 64 rigidbodies |
| Teleport snap threshold | Automatic | Velocity × step × 4 + 1 metre |

---

## 4. Local multiplayer & split-screen

Local multiplayer is **couch co-op**: up to **four** players on one machine, each with
their own input device, camera, UI and audio listener. It is entirely independent of
[online multiplayer](#5-online-multiplayer) — you can use either, both, or neither.

### 4.1 Local players & input devices

The **local player manager** owns four slots. Each slot is either inactive or bound to an
**input device**:

| Device type | Meaning |
| ----------- | ------- |
| **KeyboardMouse** | The keyboard and mouse. |
| **Gamepad** | A connected gamepad, by index. |

Registering a player activates a slot and records its device type and index;
unregistering clears it. A convenience call **auto-assigns** everything connected — it
clears all slots, optionally gives slot 0 to the keyboard, then hands each connected
gamepad the next free slot, always registers at least one player, and finally picks the
layout that matches the resulting player count.

Because a slot stores *which device* it owns, gameplay code asks "what is player 2's stick
doing" rather than "what is gamepad 1 doing", and a lookup in the other direction
(`device → player`) exists for join screens: poll for any button press on an unassigned
device and register it as the next player. The scripting API for all of this is
[Lua API → LocalPlayer](LuaAPI-EN-DOC.md#55-localplayer--local-multiplayer-and-split-screen).

> **Slots are sparse, layout positions are not.** If players occupy slots 0 and 2, they
> take the *first two* layout positions — the manager maps a player index to its ordinal
> among the active slots, so an empty slot never leaves a blank quadrant on screen.

### 4.2 Layouts & viewport rectangles

The split-screen **layout** decides how the window is divided:

| Layout | Players | Division |
| ------ | ------- | -------- |
| **None** | 1 | Full window. |
| **Vertical2** | 2 | Side by side (left / right). |
| **Horizontal2** | 2 | Stacked (top / bottom). |
| **ThreeTop1Bottom2** | 3 | Full-width top, two halves below. |
| **ThreeLeft1Right2** | 3 | Full-height left, two halves on the right. |
| **ThreeHorizontal** | 3 | Three full-width bands. |
| **ThreeVertical** | 3 | Three full-height columns. |
| **Quad4** | 4 | Four quadrants (top-left, top-right, bottom-left, bottom-right). |
| **Auto** | any | Resolves by player count: 1 → None, 2 → the **two-player orientation** setting (Vertical by default, Horizontal optional), 3 → ThreeTop1Bottom2, 4+ → Quad4. |

Each layout maps a player position to a **normalized rectangle** `(x, y, width, height)`
in the window, with the origin at the bottom-left. Converting to pixels rounds to whole
pixels, clamps to the window, and then applies the **divider thickness**: each internal
edge is inset by half the divider, so the gap between two views is exactly the configured
number of pixels and outer edges stay flush with the window. The window is cleared to the
**divider colour** before any view is drawn, so the gaps show that colour.

Split-screen is considered active only when the effective layout is not *None* **and** more
than one player is registered — registering a single player is perfectly valid and simply
means "local multiplayer with one player".

### 4.3 Cameras, UI & audio per player

**Cameras.** Every Camera component has a **Player Index**. Each frame the engine collects
the views to render:

* The **primary** camera goes first. It takes its own player index if that slot is
  registered; otherwise it falls back to the first active slot.
* Then every non-primary camera whose player index is registered and not already taken
  claims its slot. At most four views exist.
* Views after the first are sorted by player slot, so the on-screen order is stable no
  matter what order the cameras appear in the scene.

Each view then renders the scene with its own camera position, zoom, background colour,
post-processor and widget-overlay framebuffer, scissored to its rectangle — described from
the renderer's side in [Graphics → 3.2](Graphics-EN-DOC.md#32-the-frame-step-by-step) and
[Graphics → 11](Graphics-EN-DOC.md#11-cameras--viewports). Camera **shake** is decayed
independently for the primary camera and for every player camera.

**UI.** A widget instance has its own **Player Index**: `-1` means "show in every view",
anything else restricts it to that player's view. Pointer input is routed by rectangle —
the engine finds the view under the cursor, converts the cursor into that view's local
space, filters widgets to that player, and passes the view's camera and zoom so world-space
UI hit-tests correctly. Two refinements make dragging work:

* Once a press starts inside a view, that view **captures** the pointer until the button is
  released, so dragging a slider across a seam does not hand the drag to the neighbour.
* A cursor in a divider gap (in no view at all) is pushed far off-screen and all buttons
  are treated as up, so nothing is hovered or clicked by accident.

**Audio.** In split-screen the audio engine is switched to **multiple listeners**: one
listener is placed at the primary camera, and one at each additional registered player
camera, up to however many listeners the audio engine supports. Spatial sounds are then
mixed for all of them, so a sound near player 2 is audible even when player 1 is far away.
Outside split-screen the engine falls back to a single listener at the primary camera.

### 4.4 What runs where

Split-screen is a **standalone-runtime feature**. In the editor, Play mode shows the
primary camera's view filling the Viewport, exactly as a single-player game would:

| Behaviour | Editor Play mode | Standalone build |
| --------- | ---------------- | ---------------- |
| Viewport split into per-player rectangles | — | ✔ |
| Per-view post-processing and widget overlays | — | ✔ |
| Widget input routed to the view under the cursor | — | ✔ |
| Widget **Player Index** filtering | — | ✔ |
| Multiple audio listeners | ✔ | ✔ |
| Per-player camera shake decay | ✔ | ✔ |
| Player registration, layouts, device queries | ✔ | ✔ |

This is not a limitation of the layout code — the manager works identically in both — but
of where the split-screen render and input paths are compiled in. Test the layout itself in
the editor; test what it actually looks like in a build.

### 4.5 Split-screen quick reference

| Item | Value |
| ---- | ----- |
| Maximum local players | 4 |
| Layouts | None, Vertical2, Horizontal2, ThreeTop1Bottom2, ThreeLeft1Right2, ThreeHorizontal, ThreeVertical, Quad4, Auto |
| Two-player orientation | Vertical (default) or Horizontal |
| Divider | Thickness in pixels + colour; window is cleared to the colour |
| Camera binding | `CameraComponent.PlayerIndex` |
| Widget binding | Widget instance `PlayerIndex` (`-1` = all views) |
| Audio | One listener per active player camera |
| Active when | Effective layout ≠ None **and** more than one player registered |

---

## 5. Online multiplayer

### 5.1 Architecture & transports

Online play is **client–server** with a **host-authoritative** default. One peer is the
server (a listen server that also plays, or a [dedicated](#512-dedicated-servers--web-clients)
one that does not); everyone else is a client. There is no separate server binary — the
same build serves both roles.

| Platform | Transport | Can host? |
| -------- | --------- | --------- |
| Windows, Linux, macOS, Android, iOS | **ENet** over UDP | Yes |
| Web (browser) | **WebSocket** (via Emscripten) | **No** — Web builds are clients only |

Native hosts can additionally open a **WebSocket bridge** so browser clients join the same
session ([5.12](#512-dedicated-servers--web-clients)).

Both transports carry the same wire format: a **one-byte message type** followed by the
payload, optionally encrypted. Native sessions also enable ENet's **range-coder
compression** on the whole host when packet compression is on (and log a warning and carry
on uncompressed if the range coder is unavailable). The network profiler instruments both
paths identically, which is why its per-message-type breakdown works on Web too.

`Initialize()` sets up libsodium and ENet, and is called **lazily** — starting a server or
connecting initializes the subsystem if you did not do it yourself.

### 5.2 Session lifecycle

**Hosting.** Starting a server clamps **Max Players** into 2…256, creates the ENet host on
the configured port with four channels, enables compression, registers the host itself as
a local player, generates a **server secret** used for reconnect tokens, and — if a
**WebSocket Port** is configured — starts the bridge.

**Joining.** Connecting creates a client host and initiates the ENet connection. The
connect attempt is polled with a **timeout** taken from the config; failure raises a
connection-failed event rather than blocking.

**The join handshake** is challenge–response, so a password never travels in a replayable
form:

1. The server accepts the raw peer connection and immediately sends an **auth challenge**
   containing a fresh random **nonce**, and records the peer as *pending authentication*.
2. The client answers with a **PlayerJoin** carrying its name and, if the server has a
   password, the hash of *password + nonce*.
3. The server recomputes the expected hash and compares it in **constant time**. A mismatch
   disconnects the peer.
4. On success the player is assigned an id, a **Welcome** is sent, the roster is
   broadcast, and the new player is announced to everyone.

Peers that never complete step 2 are **reaped after 10 seconds**, and the number of
simultaneously pending peers is capped, so an unauthenticated flood cannot exhaust the
server. Until a peer is authenticated, the server ignores every message from it except
*PlayerJoin* and *Reconnect*.

**Reconnect.** With reconnection enabled, losing the connection puts the client into a
*Reconnecting* state and it retries up to **Max Attempts** with an interval that grows by a
back-off multiplier up to a ceiling. The server hands out a **reconnect token** derived from
its secret so a returning player can be recognized as the same player rather than a new
one.

**Shutdown.** Stopping a server sends an explicit **shutdown notice** to every peer,
flushes, disconnects them, and clears all session state — so clients report *"server is
shutting down"* instead of timing out. A client disconnect is marked intentional so the
reconnect logic does not fight it.

| State | Meaning |
| ----- | ------- |
| **Disconnected** | No session. |
| **Connecting** | Connection initiated, waiting for the handshake or the timeout. |
| **Connected** | In a session (as client or server). |
| **Reconnecting** | Connection lost, retrying. |
| **Failed** | The connection attempt failed. |

### 5.3 Channels & message types

Traffic is split across **four ENet channels** so a burst of one kind cannot delay another:

| Channel | Carries |
| ------- | ------- |
| **Control** | Join/leave, welcome, ping/pong, chat, RPCs, replication control, auth, shutdown. |
| **State** | Entity syncs, snapshots, delta snapshots, spawn/destroy, ownership. |
| **Input** | Client input frames and their acknowledgements, rollback input. |
| **Voice** | Voice frames. |

Message types are grouped by purpose — session (`PlayerJoin`, `PlayerLeave`, `Ping`,
`Pong`, `Welcome`), chat (`TextChat`, `PrivateChat`, `ChannelChat`, `ChannelJoin`,
`ChannelLeave`, `VoiceData`), world state (`EntitySync`, `EntitySpawn`, `EntityDestroy`,
`TransformUpdate`, `Snapshot`, `DeltaSnapshot`, `EntityOwnership`), input (`InputState`,
`InputAck`), `RemoteCall` for RPCs, `Reconnect`, and a control block (`RateExceeded`,
`ReplicationControl`, `AuthChallenge`, `ShutdownNotice`, `RollbackInput`, `RollbackSync`).
Two type ids are reserved for you — **`Custom`** and **`UserData`** — and are delivered to
your event callback untouched.

Each message can be sent **reliably** or **unreliably**. The engine picks sensibly on your
behalf: full keyframe snapshots and control messages are reliable; delta snapshots and
voice are not, because a lost one is superseded by the next.

### 5.4 Entity state & world snapshots

The server keeps an **entity state table**: one `EntitySnapshot` per registered network
entity. A snapshot is far more than a transform — besides position, rotation, velocity,
scale, flips, pivot, owner and arbitrary string properties, it carries the replicable
state of the sprite, flipbook, animator (including its parameter table), skeleton (with
per-bone transforms and velocities), audio, FX, rigidbody, point and spot lights, tilemap,
widget, destructible, AI and class-component settings.

Alongside it, a **dirty-flag mask** records which of those groups changed since the last
send, so a snapshot only re-serializes what actually moved.

**The tick.** Networking accumulates real time and only runs once per
**Tick Rate** interval (1–128 Hz, default 60). Inside a tick the server drains ENet events,
updates voice, broadcasts snapshots on their own **Snapshot Rate** accumulator (1–128,
default 20 Hz), records entity history for lag compensation, and reaps unauthenticated
peers. A client instead reads its round-trip time and runs time synchronization.

Clients keep the **last 128 snapshots** so they can interpolate; the server keeps a
per-entity **history** bounded both by a duration (**Max History Duration**, default 1 s)
and by an entry count (**Max Entity History Entries**, default 256).

Ownership is explicit: each entity has an **owner player id**, `0` meaning the server.
Ownership can be transferred at runtime and the change is propagated to clients.

### 5.5 Delta compression & area of interest

Two independent mechanisms keep bandwidth down.

**Delta compression** (on by default). Instead of the full world, the server sends only
what changed since the previous snapshot, using the dirty flags. Because a client that
joins mid-stream or drops a packet cannot rebuild state from deltas alone, the server
forces a **full keyframe** once per second (specifically, after as many delta snapshots as
the snapshot rate). Keyframes go out reliably, deltas unreliably.

**Area of interest** (off by default). When enabled, each player publishes an *interest
position* (normally their character), and the server builds a **per-player view** of the
snapshot containing only entities that are within the **AoI radius** of that player, owned
by that player, or explicitly marked **always relevant**. Each player then receives their
own filtered snapshot.

> AoI and delta compression are **mutually exclusive per send**: when AoI is on, each
> player gets a personalized *full* snapshot, because a delta against a per-player view is
> not meaningful. Turn AoI on for large worlds with many distant entities; leave it off for
> small arenas, where delta compression is the better trade.

### 5.6 Time sync, interpolation, prediction & lag compensation

**Time synchronization.** Once a second the client sends a ping stamped with its clock; the
server answers with its own. From the round trip the client derives its **ping** and a
**time-sync offset** (`serverTime − clientTime + RTT/2`), which is what lets client and
server talk about the same instant.

**Interpolation.** Clients do not render the newest snapshot; they render the world as it
was a short **interpolation delay** ago and interpolate between the two snapshots that
bracket that time, which hides jitter. The delay is **adaptive by default**: the engine
measures the actual spacing of arriving snapshots and uses twice that spacing, clamped to
between 20 ms and your configured delay — so a smooth connection gets a small delay
automatically. Setting the delay explicitly turns adaptation off and pins it.

**Client-side prediction** (off by default). With prediction on, the client applies input
locally at once and keeps every unacknowledged input frame in a queue (bounded at 128). The
server acknowledges input by sequence number; acknowledged frames are dropped. When a
server state arrives for a predicted entity, the client **reconciles**: it takes the
authoritative state and replays the still-pending inputs on top of it. Reconciliation is
smoothed rather than snapped — a configurable smoothing factor blends toward the corrected
position — up to a **snap distance**, above which the error is simply too large to hide and
the entity is teleported to the corrected position instead.

**Lag compensation** (off by default). The server records a short history of each
replicated entity's position and rotation, bounded by a configurable **window**. When a
client reports a hit, the server can **rewind** the world to the time that client actually
saw — derived from that player's round-trip time and the interpolation delay — run the hit
test, and then **restore** every rewound body. Rewinding backs up the current transforms
first, so restoration is exact. As a safety net, if a script leaves the world rewound, the
next server tick notices, warns, and restores it before simulating.

### 5.7 Automatic replication

Sending entity state by hand is optional. Adding a **Replication** component and ticking
**Replicate** hands the entity to the replication system, which does the rest. The
component is off by default, so a singleplayer project is completely unaffected.

| Setting | Effect |
| ------- | ------ |
| **Replicate** | Master switch. |
| **Owner** | **Server** (the host simulates it) or **Player** + an **Owner Player ID** (that client owns it — the setup for a predicted character). |
| **Transform / Velocity / Visuals / Full State** | Which groups are synchronized. Transform, velocity and visuals go every tick; full state is checked periodically. |
| **Full State Rate** | How often, in Hz, full state is re-checked (`0` = use the global rate, 8 Hz by default). |
| **Scripts On Replicas** | **Auto** (follow the global gating setting), **Always Run** or **Never Run** — whether Lua callbacks execute on clients for an entity somebody else owns. |
| **Relevancy** | **Area Of Interest** (sent only to nearby players) or **Always Relevant** (sent to everyone — bosses, objectives). Feeds directly into [5.5](#55-delta-compression--area-of-interest). |
| **Kinematic On Clients** | Make the replicated body kinematic on clients so local physics never fights the incoming position. |

**On the server**, each tick: newly-flagged entities are adopted and assigned a network id;
unflagged ones are dropped; owner changes are pushed through; each entity is announced with
a **spawn** message (including its prefab path and its UUID so clients can bind to an
entity that already exists in the level) and then pushed every tick; entities that vanished
are **despawned**; and, if it changed, the level's **world-settings override** is broadcast
so everyone simulates with the same gravity and physics settings. When a new player joins,
every entity is **re-announced** so the newcomer receives the whole world.

**On clients**, each tick: pending spawns, despawns, full states and level changes are
drained (a spawn whose prefab is not ready yet is retried for up to 30 seconds instead of
being dropped), then every replicated entity is either **reconciled** (if prediction is on
and this client owns it) or made kinematic and **driven** from the replicated state.

Level changes are part of the protocol: the server can broadcast a **level change** and
clients load the named level through a callback, so the session stays together across
level transitions.

### 5.8 Rollback netcode

For fighting games and anything else that needs frame-perfect, latency-free input, the
engine ships a **deterministic rollback** implementation that is entirely separate from the
snapshot path above. It runs over the same transport (its packets use dedicated message
types on the input channel) but replaces state replication with **input** replication.

How it works: every peer simulates every player. Local input is sent to the others and
remote input is **predicted** until it arrives. When real input turns out to differ from
the prediction, the session **rolls back** to the first incorrect frame, restores the saved
world state and **re-simulates forward** to the present — all within one frame.

| Setting | Meaning |
| ------- | ------- |
| **Num Players** | Participants in the session. |
| **Input Size** | Bytes per player per frame (a bitmask is usually enough). |
| **Frame Delay** | Frames of deliberate local input delay; trades responsiveness for fewer rollbacks. |
| **Max Rollback Frames** | How far back the session will ever re-simulate. |
| **Include Random State** | Save and restore the [random service](#27-determinism--random-streams) with each frame, so randomness rolls back too. |

The session requires four callbacks from the game: **save state** (produce a byte buffer
plus a checksum), **load state**, **advance frame** (simulate exactly one fixed step from
the given inputs) and **event**. It maintains a ring of saved frames and a ring of inputs,
computes a **Fletcher-32** checksum of each saved state, and reports lifecycle events —
synchronizing, synchronized, running, disconnected, connection interrupted/resumed, time
sync, and **desync detected** when two peers' checksums disagree for the same frame.

Two session types exist: **P2P** for real play, and **SyncTest**, which runs locally and
deliberately rolls back every frame by a chosen distance and compares checksums — the
practical way to find non-determinism in your simulation before shipping.

Live statistics — current and confirmed frame, predicted frames, rollbacks per second,
average and maximum rollback depth, and frame advantage — are exposed to script and shown
in the editor's [Network Manager](Editor-EN-DOC.md#11-network-manager-enet) under
*Rollback Diagnostics*.

> **Rollback demands determinism.** The simulation must produce bit-identical results from
> identical inputs on every peer: fixed timestep only, no wall-clock reads, no unseeded
> randomness, no iteration over unordered containers. The engine gives you the fixed step
> and seeded random streams; the rest is your code's responsibility. The scripting contract
> is in [Lua API → Rollback](LuaAPI-EN-DOC.md#325-rollback--deterministic-rollback-netcode-rollback).

### 5.9 Chat & voice

**Text chat** has three scopes — **public**, **channel** (join/leave named channels; a
message goes to everyone in that channel) and **private** (direct to one player). Messages
are stored in a bounded history (default 200 entries) and delivered to your event callback.
Server-side moderation is described in [5.11](#511-security-validation--moderation).

**Voice chat** is off by default. Enabling it builds a full capture→encode→send→jitter→
decode→play chain:

1. **Capture** — an SDL audio capture device records mono frames.
2. **Noise gate** — frames below a configurable threshold (with attack and release times)
   are dropped, so silence costs nothing.
3. **Encode** — **Opus** at 48 kHz mono when the engine was compiled with Opus support;
   otherwise raw PCM is sent and a warning is logged at startup.
4. **Send** — each frame is stamped with a sequence number and sent unreliably on the voice
   channel.
5. **Relay** — the server forwards it to the other players, prefixing the original sender's
   id so clients know who is talking.
6. **Jitter buffer** — each speaker gets their own reordering buffer that drops duplicates
   and late packets and keeps a bounded backlog.
7. **Decode & play** — frames are decoded and mixed per player, with a per-player volume.

Three controls shape the relay:

* **Proximity voice** — when enabled, the server only relays a speaker to listeners whose
  replicated entity is within the **proximity range**, giving positional voice chat for
  free.
* **Max relayed voice players** — a cap on how many people can be relayed at once; beyond
  it, additional simultaneous speakers are dropped rather than multiplying bandwidth.
* **Per-player mute and volume**, plus a **transmitting** indicator that clears after the
  **voice activity timeout** so speaker indicators fall silent on their own.

### 5.10 Server discovery & NAT traversal

**Discovery** is a small UDP protocol (magic bytes `ICED`) with four message kinds —
*beacon*, *register*, *query*, *query response*:

* A server **advertises** by broadcasting a JSON beacon (name, port, players, max players,
  password flag, game mode) to the LAN broadcast address once per interval, and/or by
  sending the same payload as a **register** to a master server.
* A client **discovers** by listening for LAN beacons and/or periodically **querying** the
  master server. Discovered servers are keyed by address and **expire** after a timeout
  (default 6 s) if their beacon stops, so the browser list self-cleans. A server discovered
  on the machine's own address is rewritten to `127.0.0.1` so "host and join" works.
* The engine can also **be** the master server: it binds a UDP port, keeps a bounded
  registry of registered servers and answers queries from it.

**NAT traversal** is a **STUN** client: it sends a binding request to a configurable STUN
server (Google's public one by default), parses the XOR-mapped address out of the response
and reports your **external IP and port**. It can run **asynchronously** so a menu does not
block, with a pending flag and a poll for the result. This tells a host what address to
share; it does not perform hole punching or relaying on your behalf.

> **Matchmaking is a separate, script-side layer.** Discovery answers *"which servers
> exist"*; deciding *"which players belong in the same match"* is done by the engine's
> skill-based matchmaking module. It pools the players you register, filters them by skill
> window, region and custom attributes, widens that window as the search runs, and picks
> the closest-skilled candidates to fill the match. It has no transport of its own — you
> feed it the players you know about and connect the resulting match with the API above.
> See [Lua API → Matchmaking](LuaAPI-EN-DOC.md#59-matchmaking--player-matchmaking).

### 5.11 Security, validation & moderation

The server assumes clients may be hostile. What it enforces:

| Mechanism | Behaviour |
| --------- | --------- |
| **Trust model** | **Competitive (Authoritative)** runs every check below. **Co-op (Trusted Peers)** skips ownership and movement validation, for friendly games where the cost is not worth it. |
| **Pre-auth gate** | Messages other than *PlayerJoin* / *Reconnect* from an unauthenticated peer are dropped outright. |
| **Password** | Nonce-salted hash, compared in constant time ([5.2](#52-session-lifecycle)). |
| **Whitelist** | When enabled, only listed player names may join. Loadable and savable to a file. |
| **Bans** | By player id, by name or by address; ban lists persist to a file. Banned addresses are rejected at join. |
| **Ownership validation** | A client may only update entities it owns; violations are logged and dropped. |
| **Movement validation** | A position update is rejected if it is non-finite, or if the implied speed exceeds **Max Entity Speed** by more than the per-tick allowance — logged explicitly as a speed-hack detection. |
| **Rate limiting** | Per-peer packet and byte budgets per second. Exceeding one sends a *rate exceeded* notice and counts a violation; after **Max Rate-Limit Violations** the peer is disconnected. |
| **Message size checks** | Voice and RPC payloads are bounds-checked before they are parsed. |
| **Encryption** | Optional **XChaCha20-Poly1305** AEAD on the message payload, with a per-packet random nonce and a key derived from your passphrase by a keyed hash. A packet that fails authentication is discarded. |
| **Chat moderation** | An optional profanity filter with a custom word list, a per-player chat **cooldown**, a **burst limit** within a rate window, and per-player text mute. |

Non-finite coordinates are rejected **regardless** of trust model — that check is not
optional, because a NaN would poison the physics world.

### 5.12 Dedicated servers & web clients

**Dedicated servers.** A **Dedicated Server** flag in the config marks a session as
host-only. Combined with `--headless`, the same game build becomes a server with no window,
no GPU and no audio device — see
[Profiling & Building → Headless](Profiling-And-Building-EN-DOC.md#headless-dedicated-server).
Because only rendering is skipped, server-authoritative hit detection and lag compensation
behave exactly as they do on a listen server.

**Web clients.** Browsers cannot open UDP sockets, so a Web build connects over
**WebSocket** (`ws://`, or `wss://` when SSL is enabled) and can only be a **client** —
hosting from the browser is refused with an explicit warning. To let browser players join a
normal session, set a non-zero **WebSocket Port** on the host: the engine starts an
in-process **bridge** that accepts WebSocket connections and relays each one to a local
ENet peer connected to the server's own port. To the game logic, a web player is just
another peer.

### 5.13 Network quick reference

| Setting | Default | Range / notes |
| ------- | ------- | ------------- |
| Port | 7777 | 1024–65535 |
| WebSocket Port | 0 (off) | Enables the browser bridge on a host |
| Max Players | 16 | Clamped to 2…256 |
| Tick Rate | 60 | 1–128 Hz |
| Snapshot Rate | 20 | 1–128 Hz (the Network Manager's live slider offers 1–60) |
| Interpolation Delay | 0.1 s | Adaptive by default; 20 ms floor |
| Timeout | 5000 ms | Connection attempt |
| Reconnect | off | Max attempts 5, interval 2 s, back-off to a ceiling |
| Delta Compression | on | Forced keyframe once per second |
| Area Of Interest | off | Radius 2000; mutually exclusive with delta per send |
| Prediction | off | Pending inputs bounded at 128 |
| Lag Compensation | off | Window default 1 s |
| Max Entity Speed | 1000 | Movement validation |
| Entity Validation | on | Ownership + speed checks |
| Trust Model | Competitive | Or Co-op (skips ownership/speed checks) |
| Max History Duration / Entries | 1 s / 256 | Lag-compensation history |
| Chat cooldown / burst / window | 0.5 s / 5 / 5 s | Per player |
| Max chat history | 200 | Messages retained |
| Voice | off | Opus 48 kHz mono when compiled in; proximity range 500; max 8 relayed speakers |
| Rate limit violations | 5 | Then disconnect |
| Channels | 4 | Control, State, Input, Voice |

Defaults live in `Config/Engine.json` and are edited in
[Preferences → Network](Editor-EN-DOC.md#109-network); the live test harness is
[Network Manager](Editor-EN-DOC.md#11-network-manager-enet).

---

## 6. The audio engine

Audio is a **miniaudio** engine created once at startup. It plays the source formats the
editor imports — `.wav`, `.ogg`, `.mp3`, `.flac` — and the engine registers an additional
**Opus** decoding backend so [cooked Opus audio](Profiling-And-Building-EN-DOC.md#9-asset-cooking)
plays back with no special case. Sounds are registered by **name**, which is the handle
every later call uses.

**Groups and volume.** Every sound belongs to a group — **Master**, **Music**, **SFX**,
**Voice**, **Ambient** or **UI**. A sound's audible level is its own volume × its group's
volume × the master volume × the global gain, and any group (or the master) can be muted
independently. The mixer is configured in [Preferences → Audio](Editor-EN-DOC.md#107-audio).

**Streaming vs. resident.** The distinction is *how you load it*, not a per-asset setting:
anything loaded as a **sound** is decoded into memory, while anything loaded as **music** is
**streamed** from disk (and defaults to the Music group and to looping). Assets that live
inside a packed build are read through the virtual file system and decoded from a memory
buffer, so packing changes nothing about playback.

**Spatial audio.** A sound can be 2D or **3D**, with position, velocity, direction, a
min/max distance pair, a rolloff, a Doppler factor and an optional **cone** (inner angle,
outer angle, outer volume). The engine supports **multiple listeners**, which is what
[split-screen](#43-cameras-ui--audio-per-player) uses; outside split-screen exactly one
listener is active and it follows the primary camera. Global defaults for distance, rolloff,
speed of sound and Doppler are engine-wide settings.

**Per-sound effects.** Each sound can carry its own DSP chain, rebuilt on demand: a
**low-pass** and **high-pass** filter, **low-shelf** and **high-shelf** EQ, a **delay**
(time, decay, wet, dry) and a **reverb** (decay, wet, room size, damping). Fades — fade to
a target volume over a duration, fade in, fade out — are handled by the engine rather than
by your update loop.

**Quality.** The **Sound Quality** preset selects the engine sample rate (from 8 kHz to
96 kHz). Changing it re-creates the audio engine and **restores every loaded sound** —
including whether it was playing, its cursor position, volume, pitch, pan, loop flag and 3D
placement — so a settings menu can change audio quality without interrupting the game.

**Suspend & the editor monitor.** Audio is suspended and resumed with the app
([2.6](#26-time-pause--suspend)). In the editor, a separate **monitor volume and mute**
scale what *you* hear while editing without touching project settings — the toolbar control
described in [Editor → 4.2](Editor-EN-DOC.md#42-editor-audio-monitor). Editor builds can
also tap the output for [Remote Preview](Editor-EN-DOC.md#12-remote-preview) audio
streaming.

The scripting API is [Lua API → Audio](LuaAPI-EN-DOC.md#12-audio--sound-and-music); sound
assets and their per-asset settings are in
[Assets → Source files & their sidecars](Assets-EN-DOC.md#41-source-files--their-sidecars).

---

## 7. The input stack

All input arrives through **SDL3** events, is accumulated into a single static state, and
is rolled over once per frame. That rollover is why the API can offer three tenses for
every button — *is pressed*, *just pressed*, *just released* — without you tracking previous
frames.

| Device | What the engine tracks |
| ------ | ---------------------- |
| **Keyboard** | Full scancode state, plus text input (with IME/on-screen keyboard support where the platform has one). |
| **Mouse** | Buttons, position, per-frame delta, both scroll axes, and a **relative mode** for FPS-style look. |
| **Gamepads** | Up to **4** pads: every standard button (including paddles, misc and touchpad), six axes with a configurable **deadzone**, rumble and trigger rumble, LED colour, power state, touchpad fingers, and motion **sensors** (accelerometer/gyroscope) where the pad has them. |
| **Joysticks** | Up to **8** raw HID devices — wheels, HOTAS, flight sticks, arcade sticks — with buttons, axes, hats, balls, rumble and LED, identified by name and GUID. |
| **Touch** | Up to **10** simultaneous fingers with pressure and deltas, **pinch** (scale and rotation), and **swipe** recognition with configurable minimum distance and maximum duration. |
| **Pen / stylus** | Position, pressure, tilt, distance, rotation, eraser flag and barrel buttons. |
| **Device sensors** | Accelerometer and gyroscope, plus compass heading and barometric altitude on mobile. |
| **Haptics** | Simple rumble, plus full force-feedback effects (constant, sine, triangle, sawtooth, ramp, left/right) with duration, delay, magnitude envelope, period, direction, attack and fade — created, run, updated and destroyed as objects. |
| **Desktop integration** | File and text **drag-and-drop** onto the window, and clipboard (and X11 primary-selection) access. |

Two paths inject input that did not come from a local device:

* **Remote Preview** feeds touch, accelerometer and gyroscope events from a connected
  Android device straight into this state, so the game reacts exactly as it would on the
  phone — see [Editor → Remote Preview](Editor-EN-DOC.md#12-remote-preview).
* **Rollback and replay** drive gameplay from recorded inputs rather than the live device
  state.

Higher-level input concepts — action bindings, axis and 2D-axis actions, input contexts,
triggers, rebinding, virtual on-screen controls and the gamepad cursor — are built on top of
this layer and are documented in
[Lua API → Input](LuaAPI-EN-DOC.md#6-input--input-keyboard-mouse-gamepad-touch). Local
multiplayer's device→player binding is [4.1](#41-local-players--input-devices).

---

## 8. Other runtime services

Short notes on engine services that have no home in the other documents. Most of them also
have a scripting API in the [Lua API](LuaAPI-EN-DOC.md); this is what the engine does
underneath.

| Service | What it does |
| ------- | ------------ |
| **Async level loading** | Loads a level without stalling the frame: the level JSON is parsed on a **worker thread**, then its textures are preloaded on the main thread in phases (*Parsing → Preloading Assets → Ready → Complete*), each with a progress fraction and a status string you can drive a loading screen from. The finished level is applied either automatically or when you ask for it, so you control the exact frame the world swaps. |
| **Scene state snapshots** | Capture and restore the whole scene as data — the mechanism behind checkpoints, save-states and rollback's save/load callbacks. |
| **Replay system** | Samples tracked entities (transform, velocity, visibility) at a fixed rate into frames, plus arbitrary named numbers and strings you record per frame. It runs in two modes: **recording** to a growing buffer you save to disk, and a **circular buffer** of the last N seconds that can be captured on demand — which is how killcams work. Playback applies samples back onto entities with optional interpolation, looping and speed control. |
| **Destruction** | Fractures sprites, flipbooks and tiles into real physics debris on impact; the fragment settings are in [Graphics → 13.7](Graphics-EN-DOC.md#137-joints-queries--destruction). The engine ages, fades and reaps debris in the `Update.Debris` stage and enforces a per-entity debris cap. |
| **Navigation** | Nav grids are built from **view volumes** ([Assets → View](Assets-EN-DOC.md#414-view--post-process-volume-ice_view)): each volume owns its own grid with its own cell size, agent radius, diagonal flag and mode (top-view or side-view, the latter with jump/fall limits). Several grids can coexist; a path query picks the grid that contains the endpoints, runs A\* and can smooth the result. |
| **Behavior trees** | AI assets are ticked per entity in the `Update.BehaviorTree` stage, with blackboards, services and EQS queries; the node reference is in [Assets → AI](Assets-EN-DOC.md#416-ai--behavior-tree-ice_ai). |
| **Command system & CVars** | Named commands and console variables registered from C++ or Lua, executed by the [developer console](Profiling-And-Building-EN-DOC.md#52-developer-console) or from script. |
| **Crash reporter** | Installed by every IceBox application; it records the active renderer, GPU and driver at startup so a crash report always names them. Reporting options are set per build in [Profiling & Building → Crash Reporter](Profiling-And-Building-EN-DOC.md#73-crash-reporter). |
| **Video playback** | An FFmpeg-based player that decodes video into a texture the game can draw and streams the audio track alongside it, with play/pause/resume/stop, a **skippable** flag, volume, looping, progress and duration, and a completion event. |
| **Localization** | The editor UI and game text are separate: the editor reads `Config/Languages/*.json` ([Editor → 2.5](Editor-EN-DOC.md#25-language-fonts--rtl-layout)), while your game reads a `.ice_localization` asset ([Assets → 4.18](Assets-EN-DOC.md#418-localization-ice_localization)), which hot-reloads when it changes on disk. |
| **Platform services** | Ads, in-app purchases, Play Games, saved games, analytics, notifications, consent, reviews, deep links, permissions, Bluetooth and Web3 are bridged per platform and exposed to script; see the [Lua API](LuaAPI-EN-DOC.md). |

---

## 9. Where everything is documented

| Topic | Document |
| ----- | -------- |
| Installing the engine, the launcher, the updater, creating projects | [Getting Started](Getting-Started-EN-DOC.md) |
| Every editor panel, menu, dialog, preference and shortcut; the Network Manager; Remote Preview | [Editor & Interface](Editor-EN-DOC.md) |
| Renderers, RHI, render graph, batching, lighting, 2D shadows, GI, post-processing, cameras, debug overlays, **physics fundamentals** | [Graphics, Rendering & Physics](Graphics-EN-DOC.md) |
| Asset types, sidecars, the Content Browser, every asset editor, importers, cooking | [Assets & Content Browser](Assets-EN-DOC.md) |
| Profilers, statistics, the developer console, building for six platforms, installers, DLC, headless servers | [Profiling & Building Games](Profiling-And-Building-EN-DOC.md) |
| Native C++ plugins and Lua/content mods | [Plugins & Mods](Plugins-And-Mods-EN-DOC.md) |
| The complete gameplay scripting API | [Lua API](LuaAPI-EN-DOC.md) |
| Editor automation and tooling | [Python API](PythonAPI-EN-DOC.md) |
| Runtime architecture, the frame loop, the job system, determinism, **multiplayer** (local and online), **advanced physics**, audio and input internals | **this document** |

---

## 10. FAQ & troubleshooting

**I registered two players but the screen isn't split.**
Check three things: more than one player is registered, the layout is not **None** (use
**Auto** if you want it chosen for you), and you are running a **standalone build** —
split-screen rendering is not compiled into the editor ([4.4](#44-what-runs-where)).

**Player 2's camera renders in the wrong place, or not at all.**
Each additional camera needs a **Player Index** matching a *registered* slot, and it must
not be the primary camera. Two cameras claiming the same slot are ignored after the first —
only one view per slot exists.

**A HUD element shows up in both players' views.**
A widget's **Player Index** of `-1` means "every view". Set it to the owning player's index.

**Clicking in player 2's half does nothing.**
Pointer routing is a standalone-build feature. Also remember the divider gap belongs to no
view: a click that lands exactly in the gap is discarded on purpose.

**Sounds near player 2 are inaudible.**
Multiple listeners only engage when split-screen is active. With a single player registered
the engine keeps one listener on the primary camera.

**Clients see other players stuttering.**
Raise the **Snapshot Rate** or leave the **Interpolation Delay** on its adaptive default,
which sizes itself from the measured snapshot spacing. A fixed delay that is smaller than
the real inter-snapshot gap always stutters.

**A client's own character feels laggy.**
Enable **Prediction** and make sure the entity's **Owner** is set to that **Player** in the
Replication component. Without ownership the client has nothing to predict
([5.7](#57-automatic-replication)).

**Clients get disconnected under load.**
That is the rate limiter. Reduce how often you send custom messages, or raise the packet and
byte budgets — repeated violations disconnect the peer by design
([5.11](#511-security-validation--moderation)).

**A client's movement is being rejected.**
Server validation refuses updates for entities the client does not own and movement faster
than **Max Entity Speed** allows per tick. Legitimate fast movement (dashes, teleports) needs
either a higher limit or a server-side move; alternatively switch the **Trust Model** to
**Co-op** for a friendly game.

**Players join but see an empty world.**
Entities only replicate when their **Replication** component has **Replicate** ticked. The
server re-announces everything when a player joins, so a genuinely empty world means nothing
is flagged.

**Bandwidth is fine locally but the server melts with many players.**
Turn on **Area Of Interest** and mark only genuinely global entities as **Always Relevant**.
Note that AoI replaces delta compression for that send ([5.5](#55-delta-compression--area-of-interest)).

**Web players cannot connect.**
Web builds are WebSocket clients only. The host must set a non-zero **WebSocket Port** to
start the bridge, and browsers on an HTTPS page require `wss://`
([5.12](#512-dedicated-servers--web-clients)).

**Rollback reports a desync.**
Something in the simulation is not deterministic. Run a **SyncTest** session, which rolls
back every frame and compares checksums locally, and audit for wall-clock reads, unseeded
randomness (use a [named random stream](#27-determinism--random-streams)), and iteration
over containers with unspecified order.

**A joint didn't get created.**
Read the Console — every skip is logged with the reason: the owner has no active body, the
named **part** does not exist or is not marked **Separate Body**, the target entity was not
found, or the target has no Rigidbody. A joint whose target simply does not exist *yet* is
retried when it appears ([3.6](#36-joints-at-runtime-targets--breaking)).

**My "Separate Body" colliders do nothing.**
The entity needs an **active Rigidbody**; without one the parts are not created and a
warning names the entity. Also check the collider's collision mode is not **NoCollision**.

**A trigger never fires.**
Check the collider's **Collision Enabled** mode: **PhysicsOnly** disables sensor events
entirely, and **NoCollision** never creates the shape. **QueryOnly** is the guaranteed
trigger setting ([3.2](#32-collision-modes-per-shape)).

**Collision-stay callbacks aren't firing.**
Stay events are only computed for entities whose script actually implements the callback —
if nothing wants them, the pass is skipped for performance. Implement the callback and they
appear.

**A moved object smears across the screen for a frame.**
It should not: teleports are detected and snapped ([3.8](#38-interpolation-teleports--threading)).
If you see smearing, the object is probably being moved a little every frame by something
other than the solver, which is indistinguishable from real motion.

**My ragdoll is stiff / floppy at the wrong time.**
The **ragdoll blend** scales joint motor torque by `(1 − blend)`. Blend 0 holds the animated
pose, blend 1 goes fully limp; anything between is an active ragdoll
([3.7](#37-ragdolls--bone-physics)).

**Hitboxes don't follow the animation.**
That is what **Bone Colliders** are for — kinematic bodies driven to the animated bone
transforms. They are separate from the ragdoll toggle and can be used without it.

**Changing collision groups at runtime did nothing.**
Group and matrix changes *are* applied live, on the next scene update. What is not live is
the **NoCollision** mode, because that shape was never created.

---

<sub>© IceBoxCrew Studio. All rights reserved. See [`LICENSE.txt`](../../LICENSE.txt) for full terms.</sub>
