# 🧊 IceBox Engine — Lua API (Ice Scripting)

## Full documentation in English

### Actual for PR-0.9.1 Version

> **IceBox Engine** uses **Lua** through **sol2** to script gameplay logic.
> Scripts can be embedded in `.ice_class` (entity classes), `.icemap` (level scripts),
> `.ice_widget` (UI widgets).
>
> This documentation covers **every available** Lua function with clear examples and usage notes.

---

## 📑 Contents

1. [Architecture and Basics](#1-architecture-and-basics)
   - [Scripting modes: Code and Visual](#scripting-modes-code-and-visual)
   - [Class editor: `.ice_class` components](#class-editor-ice_class-components)
   - [Lua script editor](#lua-script-editor)
2. [Lua Language Basics — Full Course for Beginners](#2-lua-language-basics--full-course-for-beginners)
   - [What is Lua?](#what-is-lua)
   - [Comments](#comments)
   - [Variables and data types](#variables-and-data-types)
   - [Operators](#operators)
   - [Conditions (if / elseif / else)](#conditions-if--elseif--else)
   - [Loops (for, while, repeat)](#loops-for-while-repeat)
   - [Functions](#functions)
   - [Tables — the heart of Lua](#tables--the-heart-of-lua)
   - [Strings and working with them](#strings-and-working-with-them)
   - [Standard `math` library](#standard-math-library)
   - [Standard `string` library](#standard-string-library)
   - [Standard `table` library](#standard-table-library)
   - [Scope and closures](#scope-and-closures)
   - [Metatables and OOP](#metatables-and-oop)
   - [Error handling (pcall, xpcall)](#error-handling-pcall-xpcall)
   - [Useful patterns and idioms](#useful-patterns-and-idioms)
   - [Modules and require](#modules-and-require)
   - [Iterators and generic for](#iterators-and-generic-for)
   - [Garbage collector](#garbage-collector)
   - [Standard libraries](#standard-libraries)
3. [Script lifecycle](#3-script-lifecycle)
4. [Transform — Position, Scale, Rotation](#4-transform--position-scale-rotation)
5. [Physics — Physics (Box2D)](#5-physics--physics-box2d)
6. [Input — Input (Keyboard, Mouse, Gamepad, Touch)](#6-input--input-keyboard-mouse-gamepad-touch)
7. [Entity — Entities](#7-entity--entities)
8. [Sprite — Sprites](#8-sprite--sprites)
9. [Flipbook — Frame Animation](#9-flipbook--frame-animation)
10. [Animation — Animator (State Machine)](#10-animation--animator-state-machine)
   - [Skeleton — Bone Animation and Ragdoll](#105-skeleton--bone-animation-and-ragdoll)
11. [Camera — Camera](#11-camera--camera)
12. [Audio — Sound and Music](#12-audio--sound-and-music)
13. [FX — Particle Visual Effects](#13-fx--particle-visual-effects)
   - [Point Light — Point light sources](#131-point-light--point-light-sources)
   - [Spot Light — Directed cone light](#132-spot-light--directed-cone-light)
   - [Lighting and Shadows — Global settings](#133-lighting-and-shadows--global-settings)
   - [World Override — Persistent Level Override (Physics + Rendering)](#134-world-override--persistent-level-override-physics--rendering)
14. [Collision — Collisions (AABB)](#14-collision--collisions-aabb)
15. [Traces — Tracing (Raycast and Shape Sweep)](#15-traces--tracing-raycast-and-shape-sweep)
16. [Time — Time and Timers](#16-time--time-and-timers)
17. [Tween — Smooth Value Animations](#17-tween--smooth-value-animations)
18. [Coroutine — Coroutines](#18-coroutine--coroutines)
19. [FSM — Finite State Machine](#19-fsm--finite-state-machine)
20. [Scene — Scenes, Saves, Files](#20-scene--scenes-saves-files)
21. [Widget — UI widgets](#21-widget--ui-widgets)
22. [PostProcess — Post-Processing](#22-postprocess--post-processing)
23. [Cinema — Cinematics](#23-cinema--cinematics)
24. [Settings — Game Settings](#24-settings--game-settings)
25. [Math — Math and Noise](#25-math--math-and-noise)
   - [RNG — Deterministic Random Streams (seeded)](#rng--deterministic-random-streams-seeded)
26. [Events — Event System](#26-events--event-system)
27. [Gameplay — Gameplay Systems](#27-gameplay--gameplay-systems)
28. [Localization — Localization](#28-localization--localization)
29. [Debug — Debugging](#29-debug--debugging)
   - [Lua Script Debugger (Text and Visual)](#lua-script-debugger-text-and-visual)
   - [Debug functions: how to use](#debug-functions-how-to-use)
30. [Tilemap — Tile Maps](#30-tilemap--tile-maps)
31. [Component — Component Checks and Management](#31-component--component-checks-and-management)
32. [Network — Multiplayer (Network)](#32-network--multiplayer-network)
   - [Rollback — Deterministic Rollback Netcode (Rollback)](#325-rollback--deterministic-rollback-netcode-rollback)
33. [Navigation — Navigation and Pathfinding](#33-navigation--navigation-and-pathfinding)
   - [Fog of War — Vision and Visibility](#335-fog-of-war--vision-and-visibility)
34. [AI — Artificial Intelligence](#34-ai--artificial-intelligence)
35. [Joint — Physics Joints (Box2D)](#35-joint--physics-joints-box2d)
36. [PointMarker — Point Markers](#36-pointmarker--point-markers)
37. [DataUtils — Data Structures and Utilities](#37-datautils--data-structures-and-utilities)
38. [Material — Materials and Shaders](#38-material--materials-and-shaders)
39. [Destruction — Destruction](#39-destruction--destruction)
40. [Practical examples](#40-practical-examples)
41. [Mods — Mod System](#41-mods--mod-system)
   - [Mods Lua API (`Mods.*`)](#4110-mods--lua-api-for-mod-management)
42. [DLC — Downloadable Content](#42-dlc--downloadable-content)
43. [Ads — Advertising (Google AdMob)](#43-ads--advertising-google-admob)
44. [IAP — In-App Purchases (Google Play Billing)](#44-iap--in-app-purchases-google-play-billing)
45. [PlayGames — Google Play Games Services](#45-playgames--google-play-games-services)
46. [SavedGames — Google Play Saved Games](#46-savedgames--google-play-saved-games)
47. [Firebase — Firebase Analytics](#47-firebase--firebase-analytics)
48. [Notifications — Push and Local Notifications](#48-notifications--push-and-local-notifications)
49. [Consent — GDPR Consent (UMP)](#49-consent--gdpr-consent-ump)
50. [Review — In-App Review](#50-review--in-app-review)
51. [Bluetooth — Bluetooth Communication](#51-bluetooth--bluetooth-communication)
52. [DeepLinks — Deep Linking](#52-deeplinks--deep-linking)
53. [Permissions — Android Runtime Permissions](#53-permissions--android-runtime-permissions)
54. [Web3 — Blockchain Integration (Ethereum / BNB Smart Chain)](#54-web3--blockchain-integration-ethereum--bnb-smart-chain)
55. [LocalPlayer — Local Multiplayer and Split-Screen](#55-localplayer--local-multiplayer-and-split-screen)
56. [Video — Runtime Video Playback](#56-video--runtime-video-playback)
57. [Voice — Microphone, Opus, Voice Analysis](#57-voice--microphone-opus-voice-analysis)
58. [Replay — Recording, Playback, Killcams](#58-replay--recording-playback-killcams)
59. [Matchmaking — Player Matchmaking](#59-matchmaking--player-matchmaking)
60. [Console — Developer Console & Command System](#60-console--developer-console--command-system)
61. [Draw — Immediate-Mode Rendering (Draw / Texture / RenderTarget)](#61-draw--immediate-mode-rendering-draw--texture--rendertarget)
62. [Decal — Bullet Holes, Blood Splatter, Scorch Marks](#62-decal--bullet-holes-blood-splatter-scorch-marks)

---

## 1. Architecture and Basics

### What is Ice Scripting?

**Ice Scripting** is IceBox Engine's visual-and-textual scripting system. Logic is written in **Lua**, a lightweight and fast language, or built visually as a node graph.

### Scripting modes: Code and Visual

IceBox offers two ways to author gameplay logic. The mode is chosen **once when you create a project** (in the launcher) and applies to all scripted assets — `.ice_class`, `.ice_widget` and `.icemap`.

| Mode | What you do | Editor |
|------|-------------|--------|
| **Code Scripting** | Write Lua directly | Built-in text/code editor |
| **Visual Scripting** | Connect nodes on a graph | Node graph editor |

**Code Scripting** is the classic workflow described throughout this document — you type Lua in the code editor.

**Visual Scripting** replaces the code editor with a **node graph**. Instead of typing, you drag nodes and wire them together:

- **Event nodes** are entry points (`On Create`, `On Update`, `On Collision Enter`, …) — the same lifecycle callbacks listed in [Script lifecycle](#3-script-lifecycle).
- **Action / value nodes** wrap the engine's Lua functions (`Set Position`, `Is Key Pressed`, `Add Force`, `Play Sound`, …). Every global function in this document is available as a node, with typed nodes for the most common categories (Transform, Physics, Input, Entity, Audio, Camera, and more).
- **Flow control nodes** add the structure a visual graph needs: `Branch` (if), `Sequence`, `For Loop`, `While`, `For Each`, `Do Once`, `Flip Flop`, `Do N`, `Delay`.
- **Variable nodes** (`Get` / `Set`) plus **math and logic nodes** (`+`, `-`, `>`, `AND`, `Make Vec2`, …) let you compute and store values.

Under the hood the graph is **compiled to the exact same Lua** and runs through the same engine. This means:

- Both modes share **one API and one lifecycle** — everything in this documentation applies to both. A node simply represents a Lua call.
- There is **no runtime difference**; a visual project behaves identically to the equivalent code project.

The chosen mode is stored as `"ScriptingMode": "Code"` or `"Visual"` in the project's `.iceproject` file. Visual graphs are saved inside the asset next to the generated script, so an asset always reopens in the editor you authored it with.

**Working in the node graph:**
- **Right-click** the canvas to open the searchable node palette and place a node.
- **Drag from a pin** to another pin to wire nodes; release on empty canvas to pick a node to create and connect.
- **White pins/wires** are execution flow (the order things run); **colored pins/wires** are data (values), colored by type.
- Add variables in the **Variables** panel; select a node to edit its details on the right.

> Node titles and categories are in English by design.

### File types

| File | Extension | Description |
|------|-----------|-------------|
| **Class** | `.ice_class` | Scene object (player, enemy, item). Contains components + Lua script |
| **Map** | `.icemap` | Level/scene. May contain a level script |
| **Widget** | `.ice_widget` | UI element (HUD, menu, inventory) |

### How it works

1. Create an **`.ice_class`** (class) in the editor
2. Add **components** (sprite, physics, collider, etc.)
3. Write a **Lua script** in the built-in code editor
4. Place the class in the scene (`.icemap`) — **`OnConstruct`** runs immediately in the editor
5. Press **Play** — the engine runs `OnConstruct` → `OnCreate` → `OnUpdate` each frame

### Two API types

| Type | Access | Example |
|------|--------|--------|
| **Global functions** | Available everywhere | `IsKeyPressed("space")`, `GetDeltaTime()` |
| **Entity functions** | Only inside the script of a specific entity | `SetPosition(x, y, z)`, `GetVelocity()` |

> **Important:** Entity-bound functions work with “self” — the object the script is attached to. For example, `SetPosition(100, 200, 0)` moves the entity whose script contains that line.

> **Under the hood:** entity functions are attached to a script's environment **on first use**, one
> API group at a time, so an entity only pays for the groups it actually touches. Normal code never
> notices — `SetPosition(...)`, `_ENV.SetPosition` and `Interfaces.Call(id, "SetPosition", …)` all
> behave exactly as before. The difference is visible only when you inspect the environment itself:
> `pairs(_ENV)` and `rawget(_ENV, "SetPosition")` see an entity function only after the script has
> used it.

### Class inheritance

IceBox supports **class inheritance**. If `.ice_class` has a `ParentClass` field, then:
- Parent components are inherited
- Parent Lua functions are available via the `Parent` table

```lua
-- Child class
function OnCreate()
    -- Call parent OnCreate
    if Parent and Parent.OnCreate then
        Parent.OnCreate()
    end
    -- Own initialization
    health = 100
end
```

### Child Entities (Children)

`.ice_class` can contain **child entities** (children). Such entities are created and live with the parent as part of one class:

- Child entities automatically inherit the parent transform and follow it.
- They can be used for weapons, effects, markers, UI, additional colliders, and more.
- You can control them at runtime through the hierarchy API (`AttachToEntity`, `GetChildEntities`, `GetParentEntity`, `SetLocalOffset`).

### ClassComponent (Class-Based Component)

`.ice_class` can also be attached as a **ClassComponent** to another class. This lets you reuse prebuilt sets of components and logic:

- The class component is added like any other component in the class editor.
- It becomes part of the entity and follows the same lifecycle.
- At runtime, information about attached `ClassComponent` instances is available through the API (`GetClassComponentCount`, `GetClassComponentPath`, `FindClassComponentIndex`).

### Health — health system

A ready-to-use HP system with invulnerability, shielding, and callbacks.

```lua
local hp = Health({
    max = 100,
    current = 100,
    invulnTime = 0.5,
    onDamage = function(amount, damageType) Print("-" .. amount) end,
    onHeal = function(amount) Print("+" .. amount) end,
    onDeath = function() Print("Dead") end,
    onRevive = function() Print("Revived") end
})

hp.TakeDamage(25, "fire")
hp.Heal(10)
hp.SetShield(20)
local shield = hp.GetShield()
local current = hp.GetHP()
local maxHp = hp.GetMaxHP()
hp.SetMaxHP(150)
hp.SetHP(80)
local pct = hp.GetPercent()
local dead = hp.IsDead()
local alive = hp.IsAlive()
local invuln = hp.IsInvulnerable()
hp.Revive()
hp.FullHeal()
hp.Update(dt)
```

### Inventory — inventory

```lua
local inv = Inventory(20)   -- max slots (0 = unlimited)

inv.AddItem("potion", 3)
inv.RemoveItem("potion", 1)
local has = inv.HasItem("potion", 2)
local count = inv.GetItemCount("potion")
local item = inv.GetItem("potion")
local all = inv.GetAllItems()
inv.SetItemData("potion", "rarity", "common")
local rarity = inv.GetItemData("potion", "rarity")
inv.SetMaxStack("potion", 10)
local slots = inv.GetSlotCount()
local full = inv.IsFull()
inv.Clear()

-- Transfer items between inventories
local other = Inventory(10)
local moved = inv.TransferTo(other, "potion", 2)
```

### Dialog — dialogs

```lua
local dlg = Dialog({
    start = { text = "Hello!", next = "q1" },
    q1 = {
        speaker = "NPC",
        text = "Where are we going?",
        choices = {
            { text = "To the city", next = "city" },
            { text = "To the forest", next = "forest" }
        }
    },
    city = { text = "Let's go to the city.", next = "end" },
    forest = { text = "Let's go to the forest.", next = "end" },
    end = { text = "Good luck!" }
})

dlg.SetCallbacks({
    onText = function(speaker, text) Print(speaker .. ": " .. text) end,
    onChoice = function(speaker, text, choices) Print(text) end,
    onEnd = function() Print("Dialogue ended") end,
    onEvent = function(eventName) Print("Event: " .. eventName) end
})

dlg.Start("start")
dlg.Next(1)  -- choose option
local active = dlg.IsActive()
local current = dlg.GetCurrent()
local history = dlg.GetHistory()
dlg.SetVar("reputation", 10)
local rep = dlg.GetVar("reputation")
dlg.Skip()
```

### QuestLog — quest log

```lua
local ql = QuestLog()
ql.SetCallbacks({
    onQuestAdded = function(id, quest) Print("Quest: " .. id) end,
    onQuestComplete = function(id) Print("Quest complete: " .. id) end,
    onQuestFailed = function(id) Print("Quest failed: " .. id) end,
    onObjectiveUpdate = function(qid, oid, cur, target)
        Print(qid .. ": " .. oid .. " " .. cur .. "/" .. target)
    end
})

ql.AddQuest({
    id = "find_artifact",
    title = "Find the artifact",
    description = "Explore the ruins",
    objectives = {
        { id = "enter_ruins", text = "Enter the ruins", target = 1 },
        { id = "collect", text = "Collect the artifact", target = 1 }
    }
})

ql.UpdateObjective("find_artifact", "enter_ruins", 1)
local quest = ql.GetQuest("find_artifact")
local status = ql.GetStatus("find_artifact")
local active = ql.IsActive("find_artifact")
local complete = ql.IsComplete("find_artifact")
local activeQuests = ql.GetActiveQuests()
local allQuests = ql.GetAllQuests()
ql.FailQuest("find_artifact")
ql.RemoveQuest("find_artifact")
```

### ScoreSystem — score and combo

```lua
local score = ScoreSystem({
    highScore = 1000,
    comboTimeout = 2.0,
    comboMultiplierStep = 0.1
})

score.SetCallbacks({
    onScoreChanged = function(total, delta) Print("+" .. delta) end,
    onComboChanged = function(combo) Print("Combo: " .. combo) end,
    onComboBroken = function(combo) Print("Broken: " .. combo) end,
    onHighScore = function(value) Print("New record: " .. value) end
})

score.AddScore(100)
score.Update(dt)
local total = score.GetScore()
local high = score.GetHighScore()
local combo = score.GetCombo()
local maxCombo = score.GetMaxCombo()
local mult = score.GetMultiplier()
score.SetMultiplier(2.0)
score.SetHighScore(2000)
score.BreakCombo()
score.Reset()
```

### Grid — 2D grid

```lua
local grid = Grid(10, 5, 0)  -- width, height, defaultValue
grid.Set(3, 2, 7)
local v = grid.Get(3, 2)
local w = grid.GetWidth()
local h = grid.GetHeight()
local valid = grid.IsValid(5, 5)
grid.Fill(1)
grid.FillRect(2, 2, 4, 4, 9)
grid.Swap(1, 1, 2, 2)
local neighbors = grid.GetNeighbors(3, 3, true)
local found = grid.Find(9)
local first = grid.FindFirst(9)
local count = grid.Count(1)
grid.ForEach(function(x, y, value) Print(x, y, value) end)
local manhattan = grid.ManhattanDistance(1, 1, 5, 6)
local chebyshev = grid.ChebyshevDistance(1, 1, 5, 6)
grid.FloodFill(1, 1, 3)
```

### LootTable — loot table

```lua
local loot = LootTable({
    { id = "gold", weight = 10, minAmount = 1, maxAmount = 5 },
    { id = "potion", weight = 2, minAmount = 1, maxAmount = 1 }
})

local drops = loot.Roll(3)       -- 3 rolls (may repeat)
local unique = loot.RollUnique(2) -- 2 unique rewards
```

### StatusEffects — status effects

```lua
local se = StatusEffects()

local id = se.Add({
    name = "burn",
    duration = 5.0,
    tickInterval = 1.0,
    stacks = 1,
    maxStacks = 3,
    onApply = function(eff) Print("Apply", eff.name) end,
    onTick = function(eff) Print("Tick", eff.name) end,
    onExpire = function(eff) Print("Expire", eff.name) end,
    onRemove = function(eff) Print("Remove", eff.name) end
})

se.Update(dt)
se.Remove(id)
se.RemoveByName("burn")
local has = se.Has("burn")
local stacks = se.GetStacks("burn")
local remaining = se.GetRemaining("burn")
local all = se.GetAll()
local count = se.GetCount()
se.ClearAll()
```

### StatBlock — stat block

```lua
local stats = StatBlock({ hp = 100, dmg = 10, speed = 200 })

local base = stats.GetBase("hp")
stats.SetBase("hp", 120)
local total = stats.Get("hp")

local modId = stats.AddModifier("hp", "flat", 20, "buff")
stats.AddModifier("hp", "percentAdd", 0.1)
stats.AddModifier("hp", "percentMult", 0.2)
stats.RemoveModifier("hp", modId)
stats.RemoveBySource("buff")
stats.ClearModifiers()
stats.ClearModifiers("hp")
```

### TurnManager — turn-based combat

```lua
local tm = TurnManager({
    onTurnStart = function(actor, idx) Print("Turn", idx) end,
    onTurnEnd = function(actor, idx) Print("End turn", idx) end,
    onRoundStart = function(round) Print("Round", round) end,
    onRoundEnd = function(round) Print("Round end", round) end
})

tm.AddParticipant(FindEntityByTag("Player"), 0)
tm.AddParticipant(FindEntityByTag("Enemy"), 1)
tm.Start()
tm.EndTurn()
local current = tm.GetCurrent()
local idx = tm.GetCurrentIndex()
local turn = tm.GetTurnNumber()
local active = tm.IsActive()
tm.SetParticipantActive(1, false)
local count = tm.GetParticipantCount()
tm.Stop()
```

> **Note:** achievements are fully documented in [Section 27 — Gameplay](#27-gameplay--gameplay-systems).

### Str — string utilities

> **Lightweight string helpers.** For extended string operations (`TrimLeft`, `TrimRight`, `IsEmpty`, `IsBlank`, `ReplaceFirst`, `Reverse`, `Find`, `Count`, `CharAt`, `ToNumber`, `Byte`, `Char`, `Join`), see [`String.*` in Section 37](#string-utilities).

```lua
local parts = Str.Split("a,b,c", ",")
local trimmed = Str.Trim("  hello  ")
local hasPrefix = Str.StartsWith("hello", "he")
local hasSuffix = Str.EndsWith("hello", "lo")
local contains = Str.Contains("hello", "ell")
local replaced = Str.Replace("aabb", "bb", "XX")
local upper = Str.ToUpper("hi")
local lower = Str.ToLower("HI")
local len = Str.Length("hello")
local sub = Str.Sub("hello", 1, 3)
local repeated = Str.Repeat("ab", 3)
local left = Str.PadLeft("42", 5, "0")
local right = Str.PadRight("42", 5, "0")
local formatted = Str.Format("HP: {0}", 100)
```

### Class editor: `.ice_class` components

The class editor has a `Components` tab where class components are configured. Key features:

- Add components via the `Add component` button.
- Remove a component with the `X` button in the component header.
- Multiple instances are supported for components (for example `SpriteRenderer`, `Flipbook`, `Audio`, `FX`, `Widget`, `Light`, `PointMarker`, `Tilemap`, `Joint`).
- Select the active instance by clicking in the list.
- Inherited components are marked as `(P)` and shown in a separate `Inheritance` tab.

The available components depend on the class type, but the `Class Editor` includes basics: `Transform`, `SpriteRenderer`, `Collider`, `TilemapRenderer`, `Rigidbody`, `Animator`, `Flipbook`, `Camera`, `Audio`, `FX`, `Widget`, `PointLight`, `SpotLight`, `PointMarker`, `AI`, `Joint`.

### Lua script editor

Inside the `Class Editor` there is a built-in Lua script editor for `.ice_class`:

- `Save`/`Ctrl+S` saves the class as a whole.
- `Compile` checks the script for errors and **executes `OnConstruct`** in the class viewport.
- Shows line/column cursor position and line count.
- View parent script in read-only mode (if inheritance exists).
- Autocomplete from the Lua API database.
- Find/replace, go-to line, list of functions.
- Quick snippets for common callbacks.
- Code folding.

---

## 2. Lua Language Basics — Full Course for Beginners

> **This section** describes the **Lua language** itself — syntax, data types, control flow, functions, tables, and standard libraries.
> If you **have never programmed** or are switching from another language — read this section **from start to finish** before moving to the engine API.
> Everything described here is **basic Lua** that works in any Lua app, not only in IceBox Engine.

---

### What is Lua?

**Lua** (Portuguese for “moon”) is a lightweight, fast, and simple scripting language. It is used in many commercial games, embedded systems, and of course in IceBox Engine.

**Why Lua?**
- Simple syntax — can be learned in a day
- Dynamic typing — no need to specify types
- Tables — one data type replaces arrays, dictionaries, and objects
- Fast — one of the fastest scripting languages
- Easy to embed into C/C++ engines

**Which Lua does IceBox run?** The engine embeds **Lua 5.5.1**, bound to C++ through sol2. Everything in the reference
manual for 5.5 applies, including integer/float subtypes, bitwise operators, `//` integer division, `goto`, the `utf8`
library and the `<const>` / `<close>` variable attributes — with the sandbox restrictions listed under
[Standard libraries](#standard-libraries). Native plugins that touch `lua_State*` must link the **same** Lua 5.5 the
engine links; the plugin build system checks this for you.

---

### Comments

Comments are text that the engine completely ignores. They are used for explanations.

```lua
-- This is a single-line comment (everything after -- is ignored)

--[[
    This is a multi-line comment.
    You can write as many lines as you want.
    Handy for temporarily disabling code.
]]

-- Tip: use comments to explain WHY, not WHAT
speed = 200  -- Player movement speed (px/sec)
```

---

### Variables and data types

A variable is a **name** you assign a **value** to. In Lua you don't need to specify a type — the language determines it automatically.

#### Creating variables

```lua
-- Global variable (visible everywhere in the current script)
health = 100
name = "Player"
isAlive = true

-- Local variable (visible only inside the current block/function)
local speed = 200
local score = 0
local playerName = "Hero"
```

> **Rule:** Always use `local` if the variable is only needed inside a function. Global variables (without `local`) are visible everywhere in the current `.ice_class` script and persist between `OnUpdate` calls.

#### Data types

Lua has **8 data types**. Here are the main ones you need for games:

| Type | Description | Example |
|------|-------------|--------|
| **nil** | Empty, no value | `local x = nil` |
| **boolean** | Logical: `true` or `false` | `local alive = true` |
| **number** | Number (integer or float) | `local hp = 100`, `local pi = 3.14` |
| **string** | Text string | `local name = "Hero"` |
| **table** | Table (array, dictionary, object) | `local pos = {x = 10, y = 20}` |
| **function** | Function (code block) | `local fn = function() end` |

```lua
-- nil means “nothing”, “does not exist”
local weapon = nil         -- The player has no weapon
if weapon == nil then
    Print("Weapon not selected!")
end

-- boolean — logical values
local isAlive = true
local isDead = false

-- number — numbers (Lua does not distinguish int/float)
local health = 100         -- Integer
local speed = 250.5        -- Float
local negative = -42       -- Negative
local big = 1e6            -- 1000000 (scientific notation)

-- string — text (single or double quotes)
local name = "Player"
local dialog = 'Hello, world!'
local multiline = [[
    This is a multi-line string.
    You can write on multiple lines
    without escaping.
]]

-- Determine a variable's type
Print(type(42))        -- "number"
Print(type("hello"))   -- "string"
Print(type(true))      -- "boolean"
Print(type(nil))       -- "nil"
Print(type({}))        -- "table"
```

#### Multiple assignment

```lua
-- Lua allows assigning multiple variables at once
local x, y, z = 100, 200, 0
local a, b = 1, 2

-- Swap values without a temp variable!
a, b = b, a  -- Now a=2, b=1

-- If there are fewer values than variables, the rest become nil
local p, q, r = 1, 2
-- p=1, q=2, r=nil
```

#### Variable naming rules

```lua
-- ✅ Valid:
local health = 100
local playerName = "Hero"      -- camelCase (recommended)
local max_speed = 300           -- snake_case (also ok)
local _privateVar = 42          -- Starts with _
local item1 = "Sword"          -- Letters + digits

-- ❌ Invalid:
-- local 1item = "Sword"       -- Cannot start with a digit
-- local my-var = 10            -- Cannot use hyphen
-- local my var = 10            -- Cannot use spaces
-- local function = 10          -- Cannot use reserved words
```

**Reserved words** (cannot be used as variable names):
`and`, `break`, `do`, `else`, `elseif`, `end`, `false`, `for`, `function`, `goto`, `if`, `in`, `local`, `nil`, `not`, `or`, `repeat`, `return`, `then`, `true`, `until`, `while`

---

### Operators

#### Arithmetic operators

```lua
local a = 10
local b = 3

Print(a + b)    -- 13     Addition
Print(a - b)    -- 7      Subtraction
Print(a * b)    -- 30     Multiplication
Print(a / b)    -- 3.333  Division (always float)
Print(a % b)    -- 1      Modulus
Print(a ^ b)    -- 1000   Power (10³)
Print(-a)       -- -10    Unary minus

-- Integer division (Lua 5.3+)
Print(a // b)   -- 3      Truncates the fractional part
```

#### Comparison operators

```lua
-- All operators return true or false

Print(10 == 10)    -- true    Equal
Print(10 ~= 5)     -- true    Not equal (note: ~=, not !=)
Print(10 > 5)      -- true    Greater
Print(10 < 5)      -- false   Less
Print(10 >= 10)    -- true    Greater or equal
Print(10 <= 5)     -- false   Less or equal

-- ⚠️ Important: in Lua, “not equal” is ~=, not !=
```

#### Logical operators

```lua
local a = true
local b = false

Print(a and b)     -- false   AND (both true)
Print(a or b)      -- true    OR (at least one true)
Print(not a)       -- false   NOT (inversion)

-- Complex conditions
local hp = 50
local hasPotion = true
if hp < 100 and hasPotion then
    Print("You can heal!")
end

-- ═══════════════════════════════════
-- IMPORTANT LUA QUIRK:
-- and/or return a value, not true/false
-- ═══════════════════════════════════

-- and: if the first is falsy → returns first, otherwise → second
Print(nil and "hello")     -- nil
Print("hi" and "hello")   -- "hello"

-- or: if the first is truthy → returns first, otherwise → second
Print(nil or "default")    -- "default"
Print("hi" or "default")  -- "hi"

-- This enables a powerful “default value” pattern:
local name = playerName or "Unnamed"
-- If playerName == nil → name becomes "Unnamed"

-- Ternary operator emulation (a ? b : c):
local label = (hp > 50) and "Healthy" or "Wounded"
-- hp=80 → "Healthy", hp=20 → "Wounded"
```

#### String concatenation operator

```lua
-- Two dots (..) concatenate strings
local firstName = "Icy"
local lastName = "Boxer"
local fullName = firstName .. " " .. lastName
Print(fullName)  -- "Icy Boxer"

-- Numbers are automatically converted to strings when concatenated
local score = 42
Print("Score: " .. score)     -- "Score: 42"
Print("HP: " .. 100 .. "/100")  -- "HP: 100/100"
```

#### Length operator

```lua
-- # returns the length of a string or array
local name = "Hello"
Print(#name)  -- 5

local items = {"Sword", "Shield", "Potion"}
Print(#items) -- 3
```

---

### Conditions (if / elseif / else)

Conditions allow executing different code depending on the situation.

#### Basic if

```lua
local hp = 75

if hp <= 0 then
    Print("Character is dead!")
end
```

#### if / else

```lua
local hp = 75

if hp <= 0 then
    Print("Dead!")
else
    Print("Alive! HP: " .. hp)
end
```

#### if / elseif / else

```lua
local hp = 75

if hp <= 0 then
    Print("Dead!")
elseif hp < 25 then
    Print("Critical condition!")
elseif hp < 50 then
    Print("Wounded")
elseif hp < 75 then
    Print("Lightly wounded")
else
    Print("Healthy!")
end
```

#### Nested conditions

```lua
local hasKey = true
local doorLocked = true

if doorLocked then
    if hasKey then
        Print("Opening the door with a key!")
    else
        Print("Door is locked, need a key!")
    end
else
    Print("Door is open, passing through!")
end
```

#### What is considered true and false?

```lua
-- In Lua only TWO values are false: nil and false
-- EVERYTHING ELSE is true (even 0, "", and {})

if 0 then Print("0 is true in Lua!") end           -- Will print!
if "" then Print("Empty string is true!") end       -- Will print!
if {} then Print("Empty table is true!") end        -- Will print!

if nil then Print("Won't print") end                -- Won't print
if false then Print("Won't print") end              -- Won't print

-- ⚠️ This is DIFFERENT from many languages! In Python/C++/JS,
-- 0 and empty strings are falsey, but in Lua they are not.
```

---

### Loops (for, while, repeat)

Loops execute code repeatedly.

#### Numeric for

```lua
-- for var = start, finish, step do ... end
-- Step defaults to 1

-- Count from 1 to 5
for i = 1, 5 do
    Print("Iteration: " .. i)
end
-- Prints: 1, 2, 3, 4, 5

-- Step by 2
for i = 0, 10, 2 do
    Print(i)
end
-- Prints: 0, 2, 4, 6, 8, 10

-- Countdown (step -1)
for i = 5, 1, -1 do
    Print(i .. "...")
end
Print("Launch!")
-- Prints: 5... 4... 3... 2... 1... Launch!
```

#### Generic for (tables)

```lua
-- ipairs — for arrays (in order, from 1)
local fruits = {"Apple", "Banana", "Cherry"}
for index, value in ipairs(fruits) do
    Print(index .. ": " .. value)
end
-- 1: Apple
-- 2: Banana
-- 3: Cherry

-- pairs — for dictionaries (order NOT guaranteed)
local player = { name = "Hero", hp = 100, level = 5 }
for key, value in pairs(player) do
    Print(key .. " = " .. tostring(value))
end
-- name = Hero
-- hp = 100
-- level = 5
```

#### while

```lua
-- while condition do ... end
-- Executes WHILE the condition is true

local countdown = 5
while countdown > 0 do
    Print(countdown)
    countdown = countdown - 1   -- Lua has no ++, --, +=
end
Print("Let's go!")

-- ⚠️ Be careful! If the condition never becomes false — infinite loop!
-- In IceBox this will freeze the game!
```

#### repeat...until

```lua
-- repeat ... until condition
-- Like while, but checks the condition AFTER executing (at least 1 iteration)

local input = ""
repeat
    Print("Enter 'yes' to continue")
    input = "yes"  -- In real usage this would be user input
until input == "yes"
```

#### break — early loop exit

```lua
-- break exits the nearest loop

local enemies = {"Goblin", "Orc", "Dragon", "Slime"}
local found = nil

for i, enemy in ipairs(enemies) do
    if enemy == "Dragon" then
        found = enemy
        Print("Found a dragon at index " .. i .. "!")
        break  -- Exit loop, don't check the rest
    end
end
```

#### Practical example: spawning enemies

```lua
function SpawnWave(count)
    for i = 1, count do
        local x = RandomRange(-400, 400)
        local y = -300
        SpawnEntity("Content/Classes/Enemy.ice_class", x, y)
    end
    Print("Wave: " .. count .. " enemies!")
end
```

---

### Functions

A function is a **named block of code** that can be called repeatedly.

#### Declaration and call

```lua
-- Function declaration
function SayHello()
    Print("Hello, world!")
end

-- Function call
SayHello()  -- Prints: Hello, world!
SayHello()  -- Can be called many times
```

#### Parameters (arguments)

```lua
-- Function with parameters
function Greet(name)
    Print("Hello, " .. name .. "!")
end

Greet("Player")   -- Hello, Player!
Greet("Enemy")    -- Hello, Enemy!

-- Multiple parameters
function DealDamage(target, amount, damageType)
    Print(target .. " took " .. amount .. " damage (" .. damageType .. ")")
end

DealDamage("Goblin", 50, "fire")
```

#### Default values

```lua
-- Lua doesn't have built-in default parameters, but there's a pattern:
function CreateEnemy(name, hp, speed)
    name = name or "Enemy"       -- If nil → "Enemy"
    hp = hp or 100               -- If nil → 100
    speed = speed or 150         -- If nil → 150
    Print(name .. " created: HP=" .. hp .. " Speed=" .. speed)
end

CreateEnemy("Dragon", 500, 80)   -- Dragon created: HP=500 Speed=80
CreateEnemy("Slime")              -- Slime created: HP=100 Speed=150
CreateEnemy()                     -- Enemy created: HP=100 Speed=150
```

#### Return values

```lua
-- Single return value
function Add(a, b)
    return a + b
end

local sum = Add(10, 20)
Print(sum)  -- 30

-- Multiple return values (Lua feature!)
function GetPlayerInfo()
    return "Hero", 100, 5  -- name, HP, level
end

local name, hp, level = GetPlayerInfo()
Print(name .. " Lvl " .. level)  -- Hero Lvl 5

-- Example: damage calculation
function CalculateDamage(baseDmg, armor)
    local reduction = armor * 0.5
    local finalDmg = math.max(baseDmg - reduction, 1)
    local isCrit = math.random() < 0.2  -- 20% crit chance
    if isCrit then
        finalDmg = finalDmg * 2
    end
    return finalDmg, isCrit
end

local dmg, crit = CalculateDamage(50, 20)
if crit then
    Print("CRIT! " .. dmg .. " damage!")
else
    Print(dmg .. " damage")
end
```

#### Local functions

```lua
-- Global function (visible everywhere in the script)
function Attack()
    Print("Attack!")
end

-- Local function (visible only in the current scope)
local function CalculateBonus(level)
    return level * 10
end

-- Alternative local function syntax
local Heal = function(amount)
    Print("Heal for " .. amount)
end
```

#### Anonymous functions (lambdas)

```lua
-- Function without a name — passed as an argument
Delay(2.0, function()
    Print("2 seconds passed!")
end)

-- Event subscription
On("PlayerDied", function()
    Print("Player died!")
end)

-- Sorting with custom comparison
local scores = {40, 10, 80, 25}
table.sort(scores, function(a, b)
    return a > b  -- Descending
end)
-- scores = {80, 40, 25, 10}
```

#### Variable number of arguments (varargs)

```lua
-- ... accepts any number of arguments
function PrintAll(...)
    local args = {...}  -- Collect into a table
    for i, v in ipairs(args) do
        Print(tostring(v))
    end
end

PrintAll("Hello", 42, true, "World")
-- Hello
-- 42
-- true
-- World

-- Useful example: formatted log
function Log(prefix, ...)
    local args = {...}
    local msg = prefix .. ": "
    for i, v in ipairs(args) do
        if i > 1 then msg = msg .. ", " end
        msg = msg .. tostring(v)
    end
    Print(msg)
end

Log("INFO", "Player created", "HP=100")
-- INFO: Player created, HP=100
```

---

### Tables — the heart of Lua

A `table` is the **only data structure** in Lua. It replaces arrays, dictionaries (hash maps), lists, sets, and even objects.

#### Array (sequential table)

```lua
-- Array — table with numeric indices (starting from 1, NOT 0!)
local inventory = {"Sword", "Shield", "Potion"}

-- Access by index (1-based!)
Print(inventory[1])   -- "Sword"
Print(inventory[2])   -- "Shield"
Print(inventory[3])   -- "Potion"
Print(inventory[4])   -- nil (no element)

-- Array length
Print(#inventory)     -- 3

-- Add to the end
table.insert(inventory, "Bow")
Print(#inventory)     -- 4

-- Insert at position
table.insert(inventory, 2, "Helmet")
-- {"Sword", "Helmet", "Shield", "Potion", "Bow"}

-- Remove by position (and get the removed element)
local removed = table.remove(inventory, 3)
Print(removed)  -- "Shield"

-- Remove last
local last = table.remove(inventory)

-- Iterate array
for i, item in ipairs(inventory) do
    Print(i .. ") " .. item)
end
```

> **⚠️ Important!** Lua arrays start at index **1**, not 0. This is one of the main differences from C, C++, JavaScript, Python, etc.

#### Dictionary (associative table)

```lua
-- Dictionary — table with string keys
local player = {
    name = "Hero",
    hp = 100,
    maxHp = 100,
    level = 1,
    isAlive = true
}

-- Access via dot
Print(player.name)      -- "Hero"
Print(player.hp)        -- 100

-- Access via brackets (for dynamic keys)
Print(player["name"])   -- "Hero"
local key = "hp"
Print(player[key])      -- 100

-- Modify values
player.hp = 75
player.level = player.level + 1

-- Add new field
player.weapon = "Sword"
player.armor = 20

-- Remove field (assign nil)
player.weapon = nil

-- Iterate dictionary
for key, value in pairs(player) do
    Print(key .. " = " .. tostring(value))
end
```

#### Mixed table

```lua
-- A table can contain both array and dictionary parts
local item = {
    -- Dictionary part
    name = "Flaming Sword",
    damage = 50,
    type = "weapon",

    -- Array part
    "enchant_fire",      -- [1]
    "enchant_glow",      -- [2]
}

Print(item.name)    -- "Flaming Sword"
Print(item[1])      -- "enchant_fire"
```

#### Nested tables

```lua
-- A table can contain other tables (like nested objects)
local party = {
    {
        name = "Warrior",
        hp = 200,
        skills = {"Strike", "Defense", "Taunt"}
    },
    {
        name = "Mage",
        hp = 80,
        skills = {"Fireball", "Heal", "Teleport"}
    },
    {
        name = "Archer",
        hp = 120,
        skills = {"Shot", "Multishot", "Dodge"}
    }
}

-- Access nested data
Print(party[1].name)           -- "Warrior"
Print(party[2].skills[1])     -- "Fireball"
Print(#party)                  -- 3 (three characters)

-- Iterate party
for i, member in ipairs(party) do
    Print(member.name .. " (HP: " .. member.hp .. ")")
    for _, skill in ipairs(member.skills) do
        Print("  - " .. skill)
    end
end
```

#### Table as configuration

```lua
-- Tables are ideal for passing settings into functions
local enemyConfig = {
    class = "Content/Classes/Orc.ice_class",
    hp = 150,
    speed = 120,
    dropChance = 0.3,
    drops = {"Gold", "Potion"},
    patrol = {
        {x = 100, y = 200},
        {x = 300, y = 200},
        {x = 300, y = 400},
    }
}

function CreateConfiguredEnemy(config)
    local id = SpawnEntity(config.class, config.patrol[1].x, config.patrol[1].y)
    SetEntityData(id, "hp", config.hp)
    SetEntityData(id, "speed", config.speed)
    return id
end
```

#### Checking for key existence

```lua
local data = { name = "Hero", hp = 100 }

-- Check: does the key exist?
if data.name ~= nil then
    Print("Name: " .. data.name)
end

-- Short form (idiomatic Lua)
if data.name then
    Print("Name: " .. data.name)
end

-- Safe access to nested data
local weapon = data.equipment and data.equipment.weapon
-- If data.equipment == nil, weapon will be nil (no error)
```

---

### Strings and working with them

#### Declaring strings

```lua
local s1 = "Double quotes"
local s2 = 'Single quotes'
local s3 = [[Multi-line
string without escaping]]

-- Escaping special characters
local s4 = "He said: \"Hi!\""
local s5 = "Line 1\nLine 2"     -- \n = newline
local s6 = "Tab:\tvalue"         -- \t = tab
local s7 = "Backslash: \\"       -- \\ = single \
```

#### Concatenation (joining)

```lua
local first = "Ice"
local last = "Engine"
local full = first .. " " .. last  -- "Ice Engine"

-- Numbers are converted automatically
local msg = "HP: " .. 100 .. "/" .. 100  -- "HP: 100/100"
```

#### String length

```lua
local s = "Hello"
Print(#s)              -- 5
Print(string.len(s))   -- 5 (same)
```

#### Basic `string` functions

```lua
local s = "Hello, World!"

-- Upper / lower case
Print(string.upper(s))    -- "HELLO, WORLD!"
Print(string.lower(s))    -- "hello, world!"

-- Substring (start, end) — 1-based indexing
Print(string.sub(s, 1, 5))    -- "Hello"
Print(string.sub(s, 8))       -- "World!"
Print(string.sub(s, -6))      -- "orld!" (from the end)

-- Find substring → start, end (or nil)
local start, finish = string.find(s, "World")
Print(start)   -- 8
Print(finish)  -- 12

-- Replace
local replaced = string.gsub(s, "World", "Lua")
Print(replaced)  -- "Hello, Lua!"

-- Replace with count
local result, count = string.gsub("aabbaabb", "bb", "XX")
Print(result)  -- "aaXXaaXX"
Print(count)   -- 2

-- Repeat
Print(string.rep("Ha", 3))   -- "HaHaHa"
Print(string.rep("-", 20))   -- "--------------------"

-- Reverse
Print(string.reverse("Hello"))  -- "olleH"

-- Character code and char from code
Print(string.byte("A"))       -- 65
Print(string.char(65))        -- "A"
```

#### String formatting

```lua
-- string.format — like printf in C
local name = "Hero"
local hp = 75
local maxHp = 100

local msg = string.format("%s: %d/%d HP", name, hp, maxHp)
Print(msg)  -- "Hero: 75/100 HP"

-- Specifiers:
-- %s = string
-- %d = integer
-- %f = float
-- %.2f = float with 2 decimals
-- %% = % symbol

Print(string.format("Coordinates: (%.1f, %.1f)", 123.456, 789.012))
-- "Coordinates: (123.5, 789.0)"

Print(string.format("Progress: %d%%", 75))
-- "Progress: 75%"

Print(string.format("Item: [%20s]", "Sword"))
-- "Item: [               Sword]" (alignment)
```

#### String ↔ number conversion

```lua
-- String → number
local n = tonumber("42")       -- 42
local f = tonumber("3.14")     -- 3.14
local bad = tonumber("abc")    -- nil (invalid)

-- Number → string
local s = tostring(42)         -- "42"
local s2 = tostring(3.14)      -- "3.14"
local s3 = tostring(true)      -- "true"
local s4 = tostring(nil)       -- "nil"

-- Numbers auto-convert during concatenation
Print("Score: " .. 100)       -- "Score: 100"
```

---

### Standard `math` library

```lua
-- ═══════════════════════════════════
-- CONSTANTS
-- ═══════════════════════════════════

math.pi          -- 3.14159265358979 (Pi)
math.huge        -- infinity
math.maxinteger  -- max integer
math.mininteger  -- min integer

-- ═══════════════════════════════════
-- BASIC FUNCTIONS
-- ═══════════════════════════════════

math.abs(-5)            -- 5        (absolute value)
math.max(10, 20, 5)     -- 20       (max)
math.min(10, 20, 5)     -- 5        (min)
math.floor(3.7)         -- 3        (round down)
math.ceil(3.2)          -- 4        (round up)
math.sqrt(16)           -- 4.0      (square root)
math.log(2.718)         -- ~1.0     (natural log)

-- ═══════════════════════════════════
-- TRIGONOMETRY (angles in RADIANS!)
-- ═══════════════════════════════════

math.sin(math.pi / 2)   -- 1.0
math.cos(0)             -- 1.0
math.tan(math.pi / 4)   -- ~1.0
math.asin(1)            -- ~1.5708 (π/2)
math.acos(0)            -- ~1.5708
math.atan(1, 1)         -- ~0.7854 (π/4) — atan2(y, x)

-- Degrees ↔ radians conversion
math.rad(180)           -- π (degrees → radians)
math.deg(math.pi)       -- 180 (radians → degrees)

-- ═══════════════════════════════════
-- RANDOM NUMBERS
-- ═══════════════════════════════════

math.random()           -- Float 0.0 — 1.0
math.random(6)          -- Integer 1 — 6 (dice)
math.random(10, 20)     -- Integer 10 — 20
math.randomseed(42)     -- Set seed (repeatable)

-- Typical usage:
math.randomseed(os.time())  -- Initialize with current time
```

#### Useful math patterns for games

```lua
-- Clamp value
function Clamp(value, min, max)
    return math.max(min, math.min(max, value))
end
local hp = Clamp(hp - damage, 0, maxHp)

-- Linear interpolation (lerp)
function Lerp(a, b, t)
    return a + (b - a) * t
end
-- Smooth movement (t from 0 to 1)
local x = Lerp(startX, endX, 0.5)  -- Middle

-- Distance between two points
function Distance(x1, y1, x2, y2)
    local dx = x2 - x1
    local dy = y2 - y1
    return math.sqrt(dx * dx + dy * dy)
end

-- Normalize vector (length = 1)
function Normalize(x, y)
    local len = math.sqrt(x * x + y * y)
    if len > 0 then
        return x / len, y / len
    end
    return 0, 0
end

-- Angle between two points (degrees)
function AngleBetween(x1, y1, x2, y2)
    return math.deg(math.atan(y2 - y1, x2 - x1))
end
```

---

### Standard `string` library

> All `string` functions can be called in two ways:

```lua
local s = "Hello World"

-- Method 1: via the library
string.upper(s)          -- "HELLO WORLD"

-- Method 2: as a string method (syntactic sugar)
s:upper()                -- "HELLO WORLD"

-- Both are identical. The second is shorter.
```

#### Full `string` function table

| Function | Description | Example |
|---------|-------------|--------|
| `string.byte(s, i?)` | Character code at position i | `string.byte("A")` → `65` |
| `string.char(n...)` | Character from code | `string.char(65)` → `"A"` |
| `string.find(s, pattern)` | Find → start, end, or nil | `string.find("hello", "ll")` → `3, 4` |
| `string.format(fmt, ...)` | Formatting | `string.format("%d HP", 100)` → `"100 HP"` |
| `string.gmatch(s, pattern)` | Match iterator | see below |
| `string.gsub(s, pattern, repl)` | Replace all matches | `string.gsub("aa", "a", "b")` → `"bb"` |
| `string.len(s)` | String length | `string.len("abc")` → `3` |
| `string.lower(s)` | Lowercase | `string.lower("ABC")` → `"abc"` |
| `string.upper(s)` | Uppercase | `string.upper("abc")` → `"ABC"` |
| `string.match(s, pattern)` | First match | `string.match("abc123", "%d+")` → `"123"` |
| `string.rep(s, n)` | Repeat n times | `string.rep("ab", 3)` → `"ababab"` |
| `string.reverse(s)` | Reverse | `string.reverse("abc")` → `"cba"` |
| `string.sub(s, i, j?)` | Substring | `string.sub("hello", 2, 4)` → `"ell"` |

#### Patterns (search templates)

```lua
-- Lua uses its own patterns (not regex, but similar)

-- Main special characters:
-- %d = digit (0-9)
-- %a = letter (a-z, A-Z)
-- %l = lowercase letter
-- %u = uppercase letter
-- %s = whitespace
-- %p = punctuation
-- %w = letter or digit
-- .  = any character
-- +  = 1 or more repetitions
-- *  = 0 or more repetitions
-- -  = 0 or more (lazy)
-- ?  = 0 or 1 repetition

-- Extract numbers from a string
local text = "Damage: 50, Defense: 30"
for num in string.gmatch(text, "%d+") do
    Print(num)  -- "50", "30"
end

-- Capture groups
local date = "2024-01-15"
local year, month, day = string.match(date, "(%d+)-(%d+)-(%d+)")
Print(year .. "/" .. month .. "/" .. day)  -- "2024/01/15"

-- Split a string by delimiter
function Split(str, sep)
    local result = {}
    for part in string.gmatch(str, "([^" .. sep .. "]+)") do
        table.insert(result, part)
    end
    return result
end

local parts = Split("Sword,Shield,Potion", ",")
-- parts = {"Sword", "Shield", "Potion"}
```

---

### Standard `table` library

The `table` library works with the **array part** of tables (numeric indices from 1).

```lua
local items = {"Sword", "Shield", "Potion"}

-- ═══════════════════════════════════
-- ADDING AND REMOVING
-- ═══════════════════════════════════

-- Add to the end
table.insert(items, "Bow")
-- {"Sword", "Shield", "Potion", "Bow"}

-- Insert at position (shifts others)
table.insert(items, 2, "Helmet")
-- {"Sword", "Helmet", "Shield", "Potion", "Bow"}

-- Remove by position (returns removed element)
local removed = table.remove(items, 3)  -- "Shield"
-- {"Sword", "Helmet", "Potion", "Bow"}

-- Remove last
local last = table.remove(items)  -- "Bow"
-- {"Sword", "Helmet", "Potion"}

-- ═══════════════════════════════════
-- SORTING
-- ═══════════════════════════════════

-- Ascending (numbers / strings)
local numbers = {30, 10, 50, 20}
table.sort(numbers)
-- {10, 20, 30, 50}

-- Descending (custom comparison)
table.sort(numbers, function(a, b)
    return a > b
end)
-- {50, 30, 20, 10}

-- Sort objects by field
local enemies = {
    { name = "Orc", hp = 150 },
    { name = "Goblin", hp = 50 },
    { name = "Dragon", hp = 500 },
}
table.sort(enemies, function(a, b)
    return a.hp < b.hp  -- By HP (weak to strong)
end)

-- ═══════════════════════════════════
-- CONCATENATION
-- ═══════════════════════════════════

local words = {"Hello", "world", "Lua"}
local sentence = table.concat(words, " ")
Print(sentence)  -- "Hello world Lua"

local csv = table.concat({1, 2, 3, 4}, ",")
Print(csv)  -- "1,2,3,4"

-- ═══════════════════════════════════
-- MOVE (Lua 5.3+)
-- ═══════════════════════════════════

-- table.move(src, from, to, dest_start, dest_table?)
-- Copies elements from one position to another
local a = {1, 2, 3, 4, 5}
local b = {}
table.move(a, 1, 3, 1, b)  -- Copy a[1..3] to b
-- b = {1, 2, 3}
```

#### Useful functions for working with tables

```lua
-- Check: is the array empty?
function IsEmpty(t)
    return next(t) == nil
end

-- Count dictionary keys (# works only for arrays)
function CountKeys(t)
    local count = 0
    for _ in pairs(t) do
        count = count + 1
    end
    return count
end

-- Does the array contain a value?
function Contains(arr, value)
    for _, v in ipairs(arr) do
        if v == value then return true end
    end
    return false
end

-- Shallow copy table
function ShallowCopy(t)
    local copy = {}
    for k, v in pairs(t) do
        copy[k] = v
    end
    return copy
end

-- Deep copy (including nested tables)
function DeepCopy(t)
    if type(t) ~= "table" then return t end
    local copy = {}
    for k, v in pairs(t) do
        copy[DeepCopy(k)] = DeepCopy(v)
    end
    return copy
end

-- Merge two tables (second overrides first)
function Merge(base, override)
    local result = ShallowCopy(base)
    for k, v in pairs(override) do
        result[k] = v
    end
    return result
end

-- Filter array
function Filter(arr, predicate)
    local result = {}
    for _, v in ipairs(arr) do
        if predicate(v) then
            table.insert(result, v)
        end
    end
    return result
end

-- Map array
function Map(arr, transform)
    local result = {}
    for i, v in ipairs(arr) do
        result[i] = transform(v)
    end
    return result
end

-- Example usage:
local numbers = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10}
local evens = Filter(numbers, function(n) return n % 2 == 0 end)
-- {2, 4, 6, 8, 10}

local doubled = Map(numbers, function(n) return n * 2 end)
-- {2, 4, 6, 8, 10, 12, 14, 16, 18, 20}
```

> **Note:** IceBox Engine provides advanced utilities `Array.*`, `Map.*`, `Set()` and more in the [DataUtils](#37-datautils--data-structures-and-utilities) section, which do all of the above and more.

---

### Scope and closures

#### Global vs local variables

```lua
-- Global variable — visible EVERYWHERE in the script
health = 100

function TakeDamage(amount)
    health = health - amount  -- Modifies global health
end

-- Local variable — visible only inside its block
function CreateBullet()
    local speed = 500         -- Visible only inside CreateBullet
    local damage = 10
    Print(speed)
end

-- Print(speed) -- ERROR! speed is undefined here
```

#### Blocks and scope

```lua
-- local inside if/for/while is visible only in that block
if true then
    local temp = 42
    Print(temp)     -- 42 (OK)
end
-- Print(temp)      -- nil! (temp doesn't exist outside)

-- Nested scopes: inner block sees outer variables
local outerVar = "outside"

function Inner()
    Print(outerVar)  -- "outside" (sees outer variable)

    local innerVar = "inside"
    Print(innerVar)  -- "inside" (OK)
end

-- Print(innerVar)   -- nil! (not visible outside)
```

#### Closures

A closure is a function that “remembers” variables from its creation scope.

```lua
-- Simple closure
function CreateCounter()
    local count = 0  -- Captured by the closure

    return function()
        count = count + 1
        return count
    end
end

local counter = CreateCounter()
Print(counter())  -- 1
Print(counter())  -- 2
Print(counter())  -- 3

-- Each CreateCounter call creates an INDEPENDENT counter
local counterA = CreateCounter()
local counterB = CreateCounter()
Print(counterA())  -- 1
Print(counterA())  -- 2
Print(counterB())  -- 1 (independent!)

-- Practical example: damage factory
function CreateDamageDealer(baseDamage, critChance)
    return function(target)
        local dmg = baseDamage
        if math.random() < critChance then
            dmg = dmg * 2
            Print("CRIT!")
        end
        Print(target .. " took " .. dmg .. " damage")
        return dmg
    end
end

local fireBall = CreateDamageDealer(50, 0.3)
local iceSpike = CreateDamageDealer(30, 0.5)

fireBall("Goblin")   -- Goblin took 50 (or 100) damage
iceSpike("Orc")      -- Orc took 30 (or 60) damage
```

---

### Metatables and OOP

Metatables allow you to override table behavior. They enable object-oriented programming in Lua.

#### Basic metamethods

```lua
-- Metatable — normal table with special keys (__add, __index, etc.)
local vec1 = { x = 1, y = 2 }
local vec2 = { x = 3, y = 4 }

-- Without metatable: vec1 + vec2 = ERROR
-- With metatable: you can define what "+" means

local VecMeta = {}
VecMeta.__add = function(a, b)
    return setmetatable({ x = a.x + b.x, y = a.y + b.y }, VecMeta)
end
VecMeta.__tostring = function(v)
    return "(" .. v.x .. ", " .. v.y .. ")"
end

setmetatable(vec1, VecMeta)
setmetatable(vec2, VecMeta)

local vec3 = vec1 + vec2
Print(tostring(vec3))  -- "(4, 6)"
```

#### OOP: Classes via metatables

```lua
-- ═══════════════════════════════════
-- CREATING A “CLASS” IN LUA
-- ═══════════════════════════════════

-- Define class Character
local Character = {}
Character.__index = Character

-- Constructor
function Character.new(name, hp, damage)
    local self = setmetatable({}, Character)
    self.name = name
    self.hp = hp
    self.maxHp = hp
    self.damage = damage
    self.isAlive = true
    return self
end

-- Methods
function Character:TakeDamage(amount)
    self.hp = math.max(self.hp - amount, 0)
    Print(self.name .. " took " .. amount .. " damage! HP: " .. self.hp)
    if self.hp <= 0 then
        self.isAlive = false
        Print(self.name .. " died!")
    end
end

function Character:Heal(amount)
    self.hp = math.min(self.hp + amount, self.maxHp)
    Print(self.name .. " healed for " .. amount .. ". HP: " .. self.hp)
end

function Character:Attack(target)
    Print(self.name .. " attacks " .. target.name .. "!")
    target:TakeDamage(self.damage)
end

-- Usage
local hero = Character.new("Hero", 100, 25)
local goblin = Character.new("Goblin", 50, 10)

hero:Attack(goblin)     -- Hero attacks Goblin! → Goblin took 25 damage!
goblin:Attack(hero)     -- Goblin attacks Hero! → Hero took 10 damage!
hero:Heal(15)           -- Hero healed for 15. HP: 105 → 100 (max)
```

> **Important:** `:` (colon) when defining and calling methods is syntactic sugar.
> `Character:Attack(target)` is the same as `Character.Attack(self, target)`.
> When calling `hero:Attack(goblin)`, Lua automatically passes `hero` as `self`.

#### OOP: Inheritance

```lua
-- Base class
local Entity = {}
Entity.__index = Entity

function Entity.new(name, x, y)
    local self = setmetatable({}, Entity)
    self.name = name
    self.x = x
    self.y = y
    return self
end

function Entity:GetPosition()
    return self.x, self.y
end

function Entity:MoveTo(x, y)
    self.x = x
    self.y = y
end

-- Child class (inherits from Entity)
local Enemy = setmetatable({}, { __index = Entity })
Enemy.__index = Enemy

function Enemy.new(name, x, y, damage)
    local self = Entity.new(name, x, y)  -- Call parent constructor
    setmetatable(self, Enemy)
    self.damage = damage
    self.isAggressive = true
    return self
end

function Enemy:Attack()
    Print(self.name .. " deals " .. self.damage .. " damage!")
end

-- Usage
local orc = Enemy.new("Orc", 100, 200, 30)
orc:MoveTo(150, 250)        -- Inherited from Entity
orc:Attack()                -- Enemy method
local x, y = orc:GetPosition()  -- Inherited from Entity
Print(orc.name .. " at " .. x .. ", " .. y)
```

> **Note:** In IceBox Engine, OOP is usually unnecessary in entity scripts because the engine already provides a component system. But OOP is useful for building utility modules (inventory, dialogue, quests, etc.).

---

### Error handling (pcall, xpcall)

Normally, a Lua error **stops the entire script**. The `pcall` function lets you “catch” errors.

```lua
-- ═══════════════════════════════════
-- pcall — Protected Call
-- ═══════════════════════════════════

-- Dangerous code (can fail)
local success, result = pcall(function()
    local x = 10 / 0       -- Not an error in Lua (= inf)
    local t = nil
    return t.field          -- ERROR! Attempt to access field of nil
end)

if success then
    Print("Result: " .. tostring(result))
else
    Print("Error: " .. result)  -- result contains error message
end

-- ═══════════════════════════════════
-- Practical example: safe data loading
-- ═══════════════════════════════════

function SafeLoadData(key)
    local success, data = pcall(function()
        local raw = ReadFile(key .. ".json")
        if not raw then
            error("File not found: " .. key)
        end
        return raw
    end)

    if success then
        return data
    else
        PrintWarning("Failed to load " .. key .. ": " .. data)
        return nil
    end
end

-- ═══════════════════════════════════
-- xpcall — with custom error handler
-- ═══════════════════════════════════

local function errorHandler(err)
    return "SCRIPT ERROR: " .. err
end

local ok, msg = xpcall(function()
    error("something went wrong")
end, errorHandler)

Print(msg)  -- "SCRIPT ERROR: something went wrong"

-- ═══════════════════════════════════
-- error() — raise your own errors
-- ═══════════════════════════════════

function SetHealth(value)
    if type(value) ~= "number" then
        error("SetHealth expects a number, got: " .. type(value))
    end
    if value < 0 then
        error("HP cannot be negative: " .. value)
    end
    health = value
end

-- assert — check condition + error if false
function DivideScore(score, divisor)
    assert(divisor ~= 0, "Division by zero!")
    return score / divisor
end
```

---

### Useful patterns and idioms

This section collects typical Lua idioms you will use in game scripts.

#### Default value

```lua
function CreateEnemy(config)
    config = config or {}
    local name = config.name or "Enemy"
    local hp = config.hp or 100
    local speed = config.speed or 150
end
```

#### Ternary operator

```lua
-- Lua has no ternary a ? b : c, but you can emulate it:
local status = (hp > 0) and "Alive" or "Dead"
local direction = (vx > 0) and 1 or -1

-- ⚠️ Careful! This fails if the “true value” can be false/nil:
-- local val = condition and false or "default"  — always returns "default"!
```

#### Safe access to nested data

```lua
-- Problem: if an intermediate key = nil → error
-- local name = data.player.inventory.weapon.name  -- ERROR if player = nil

-- Safe chain:
local weapon = data and data.player and data.player.inventory
                and data.player.inventory.weapon
local name = weapon and weapon.name or "No weapon"
```

#### Caching results

```lua
-- If a function is expensive, call it once and cache the result
function OnUpdate(dt)
    -- ❌ Bad: called every frame (60+ times per second)
    -- for _, enemy in ipairs(FindEntitiesByTag("Enemy")) do ... end

    -- ✅ Good: search + cache (refresh as needed)
    if not cachedEnemies or refreshTimer <= 0 then
        cachedEnemies = FindEntitiesByTag("Enemy")
        refreshTimer = 0.5  -- Refresh every 0.5 seconds
    end
    refreshTimer = refreshTimer - dt

    for _, enemy in ipairs(cachedEnemies) do
        -- ...
    end
end
```

#### “Lookup table” instead of long if/elseif

```lua
-- ❌ Long if/elseif chain:
function GetDamageMultiplier(element, target)
    if element == "fire" and target == "ice" then return 2.0
    elseif element == "fire" and target == "fire" then return 0.5
    elseif element == "water" and target == "fire" then return 2.0
    -- ... dozens of lines
    else return 1.0 end
end

-- ✅ Lookup table — cleaner, faster, easier to extend:
local damageTable = {
    fire  = { ice = 2.0, fire = 0.5, grass = 2.0, water = 0.5 },
    water = { fire = 2.0, water = 0.5, grass = 0.5, ice = 2.0 },
    ice   = { grass = 2.0, ice = 0.5, fire = 0.5, water = 2.0 },
    grass = { water = 2.0, grass = 0.5, ice = 0.5, fire = 0.5 },
}

function GetDamageMultiplier(element, target)
    local row = damageTable[element]
    return (row and row[target]) or 1.0
end

Print(GetDamageMultiplier("fire", "ice"))   -- 2.0
Print(GetDamageMultiplier("fire", "rock"))  -- 1.0 (default)
```

#### “Command table” instead of switch/case

```lua
-- Lua has no switch/case, but a table of functions is more powerful:
local commands = {
    attack = function(entity)
        SetAnimTrigger("attack")
        DealDamage()
    end,
    heal = function(entity)
        health = math.min(health + 25, maxHp)
        PlaySound("heal")
    end,
    dash = function(entity)
        local dir = GetForwardVector()
        AddImpulse(dir.x * 500, dir.y * 500)
    end,
}

function ExecuteCommand(name)
    local cmd = commands[name]
    if cmd then
        cmd()
    else
        PrintWarning("Unknown command: " .. name)
    end
end

-- Usage:
ExecuteCommand("attack")
ExecuteCommand("heal")
```

#### Global script variables for OnUpdate

```lua
-- Variables without local inside OnConstruct/OnCreate persist between frames
-- This is the primary way to store state in IceBox scripts
--
-- NOTE: OnConstruct runs in the editor too.
-- Variables set in OnConstruct will configure the entity at edit time.
-- At runtime, OnConstruct runs first, then OnCreate, then OnUpdate each frame.

function OnConstruct()
    -- These variables live for the entire entity lifetime
    -- AND are applied in the editor viewport immediately
    speed = 250
    health = 100
    maxHealth = 100
    isAlive = true
    attackCooldown = 0
    facingRight = true
end

function OnUpdate(dt)
    -- All variables above are available here
    attackCooldown = attackCooldown - dt
    if attackCooldown < 0 then attackCooldown = 0 end

    if IsKeyJustPressed("space") and attackCooldown <= 0 then
        attackCooldown = 0.5  -- Half-second cooldown
        -- Attack!
    end
end
```

#### Safe iteration with removal

```lua
-- ❌ DANGEROUS: removal during ipairs
-- for i, v in ipairs(list) do
--     if v.dead then table.remove(list, i) end  -- Skips elements!
-- end

-- ✅ Safe: iterate from the end
for i = #list, 1, -1 do
    if list[i].dead then
        table.remove(list, i)
    end
end

-- ✅ Or: create a new array (filter)
local alive = {}
for _, v in ipairs(list) do
    if not v.dead then
        table.insert(alive, v)
    end
end
list = alive
```

#### Useful one-liners

```lua
-- Swap values
a, b = b, a

-- Max of three
local max3 = math.max(a, math.max(b, c))

-- Sign of number (-1, 0, 1)
local sign = x > 0 and 1 or (x < 0 and -1 or 0)

-- Check if value is within range
local inRange = x >= min and x <= max

-- Random element from an array
local item = list[math.random(#list)]

-- Check if table is empty
local empty = next(t) == nil
```

---

### Modules and require

Lua supports splitting code into modules. A module is a regular `.lua` file that returns a table.

In the editor a module is a **first-class asset**: create it with the Content Browser's
**Other ▸ Create Lua Script (.lua)** and double-click it to open the **Lua Script Editor**
(see [Assets](Assets-EN-DOC.md#420-script-lua--text-txt)). New scripts already contain the
module skeleton below.

```lua
-- Content/Utils/Math.lua
local M = {}

function M.Clamp(v, min, max)
    return math.max(min, math.min(max, v))
end

return M
```

```lua
-- In another file (class script, level script or widget script)
local Math = require("Content.Utils.Math")
local hp = Math.Clamp(150, 0, 100)  -- 100
```

> The path in `require` uses dots, not slashes, and is resolved from the **project root**,
> so `Content/Utils/Math.lua` becomes `require("Content.Utils.Math")`.
>
> Modules are read through the engine's virtual file system, so the same `require` works
> in the editor, in a build that ships a loose `Content/` folder, in a build packed into
> `Content.icepak`, and from the APK assets on Android. Moving a module in the Content
> Browser leaves a redirector behind, and `require` follows it, so the old module path
> keeps working.
>
> `require` is available in **class scripts**, the **level script**, **widget scripts**
> and **mods**.
>
> A module runs **once per Lua state** and its result is cached in `package.loaded`.
> Class scripts, the level script and mods share one state; **widget scripts run in their
> own state**, so a module required from both sides gets one instance per state — the code
> is shared, the data inside the module is not. Use events or level data to pass state
> between widgets and entities.
>
> The editor drops the cached project modules of both states every time you press
> **Play**, so edits made in the Lua Script Editor take effect on the next run without
> restarting the editor.

---

### Iterators and generic for

The generic `for` can work with any iterator that returns the next value.

```lua
-- Simple custom iterator: numbers from 1 to n
function Range(n)
    local i = 0
    return function()
        i = i + 1
        if i <= n then return i end
    end
end

for i in Range(3) do
    Print(i)  -- 1, 2, 3
end
```

---

### Garbage collector

Lua uses automatic garbage collection (GC). You usually don't need manual control, but it is available:

```lua
collectgarbage("collect")   -- Force a GC pass
local kb = collectgarbage("count")  -- Memory usage (KB)
collectgarbage("stop")      -- Stop GC
collectgarbage("restart")   -- Resume GC
```

---

### Standard libraries

IceBox opens exactly these Lua standard libraries, in **both** script states (entity/level/mod
scripts and widget scripts):

| Library | Available | Notes |
| ------- | --------- | ----- |
| `base` | ✅ | `print` is redirected to the engine log; prefer `Print`. |
| `string` | ✅ | Lua patterns, `string.format`, … |
| `table` | ✅ | `table.insert`, `table.sort`, … |
| `math` | ✅ | See also the engine `Math.*` helpers. |
| `coroutine` | ✅ | See also `StartCoroutine` / `WaitSeconds` below. |
| `utf8` | ✅ | `utf8.len`, `utf8.char`, `utf8.codepoint`, `utf8.codes`, `utf8.offset`, `utf8.charpattern`. |
| `package` | ✅ | `require` — see [Modules and require](#modules-and-require). |
| `os` | ❌ | It also exposes `os.execute` / `os.remove` / `os.exit`. Use the engine date/time API instead: `GetUnixTime`, `GetDateTable`, `FormatDate`. |
| `io` | ❌ | Use the sandboxed `WriteFile` / `ReadFile` / `PersistentTable` / `SaveGameState` instead — they write only inside the save folder. |
| `debug` | ❌ | The engine installs its own error handler, so **every runtime error already carries a full stack traceback**; `debug.sethook` would collide with the built-in Lua debugger. |

```lua
-- coroutine: basic example
local co = coroutine.create(function()
    Print("Step 1")
    coroutine.yield()
    Print("Step 2")
end)

coroutine.resume(co)  -- Step 1
coroutine.resume(co)  -- Step 2
```

```lua
-- utf8: text that is not plain ASCII
local text = "Привет"
Print(#text)            -- 12 — bytes
Print(utf8.len(text))   -- 6  — characters

for position, codepoint in utf8.codes(text) do
    Print(position, codepoint, utf8.char(codepoint))
end
```

```lua
-- real date and time (instead of os.time / os.date)
local now = GetUnixTime()
local today = FormatDate("%Y-%m-%d", now)
```

---

> **You have learned all the Lua basics!** Now you are ready to use the IceBox Engine API. Go to [Section 3 (Script lifecycle)](#3-script-lifecycle) and beyond.

---

## 3. Script lifecycle

### Entity script (Entity Script)

Each `.ice_class` can contain these callback functions:

```lua
-- ═══════════════════════════════════
-- ENTITY LIFECYCLE
-- ═══════════════════════════════════

function OnConstruct()
    -- CONSTRUCTION SCRIPT
    --
    -- Called in EDITOR MODE:
    --   • When you drag-and-drop a class onto the viewport
    --   • When you click "Compile" in the Class Editor
    --   • When a level is opened (for all scripted entities)
    --   • When class instances are hot-reloaded
    --
    -- Called in RUNTIME (Play Mode):
    --   • Before OnCreate, as the very first callback
    --
    -- Use this to set up visuals, generate tilemaps, configure
    -- components — anything you want to SEE in the editor without
    -- pressing Play.
    --
    -- The global variable IsEditorMode (bool) is available:
    --   true  = running in the editor viewport (edit mode)
    --   false / nil = running in play mode (runtime)
    --
    speed = 200
    health = 100
    isAlive = true

    if IsEditorMode then
        -- Editor-only preview logic (e.g. debug visualization)
        Print("[Editor] Entity constructed")
    end
end

function OnCreate()
    -- Called ONLY at runtime (Play Mode), after OnConstruct.
    -- Use for runtime-specific initialization: input bindings,
    -- spawning helpers, starting coroutines, etc.
    -- This is NEVER called in the editor.
    Print("Player created!")
end

function OnUpdate(dt)
    -- Called EVERY FRAME. dt = time between frames (delta time)
    -- Main logic: movement, checks, AI
    local vx = 0
    if IsKeyPressed("d") then vx = speed end
    if IsKeyPressed("a") then vx = -speed end
    SetVelocityX(vx)
end

function OnFixedUpdate(dt)
    -- Called with FIXED step (for physics)
    -- Use for physics calculations
end

function OnLateUpdate(dt)
    -- Called every frame AFTER animations (Animator/flipbook frame swap)
    -- have updated, right before rendering.
    -- Use for anything that must follow the CURRENT animation frame:
    -- camera follow, look-at, manual socket work.
    --
    -- For attaching art to a socket prefer AttachSpriteToSocket("ArmSocket", 1)
    -- (set once, the engine keeps it glued every frame). Use the manual form
    -- below only when you need extra logic per frame — and always go through
    -- the World setters, never SetSpriteLocalPosition with a world delta.
    local sock = GetSpriteAttachPointWorld("ArmSocket", 0)
    if sock and sock.found then
        SetSpriteWorldPosition(sock.x, sock.y, 1)
        SetSpriteWorldRotation(sock.rotation, 1)
        SetFlipX(GetFlipX(0), 1)
    end
end

function OnDestroy()
    -- Called when the entity is destroyed
    Print("Entity removed")
end

function OnEnable()
    -- Called when the entity is enabled
end

function OnDisable()
    -- Called when the entity is disabled
end

-- ═══════════════════════════════════
-- COLLISIONS (Box2D physics)
-- ═══════════════════════════════════

function OnCollisionEnter(otherTag, otherEntityId)
    -- Collision with a physics body started
    if otherTag == "Enemy" then
        health = health - 10
    end
end

function OnCollisionExit(otherTag, otherEntityId)
    -- Collision ended
end

-- ═══════════════════════════════════
-- SENSORS (TRIGGERS)
-- ═══════════════════════════════════

function OnSensorEnter(otherTag, otherEntityId)
    -- Enter sensor zone (collider with isSensor flag)
    if otherTag == "Coin" then
        DestroyEntity(otherEntityId)
        score = score + 1
    end
end

function OnSensorExit(otherTag, otherEntityId)
    -- Exit sensor zone
end

-- ═══════════════════════════════════
-- HIT
-- ═══════════════════════════════════

function OnHit(otherTag, otherEntityId, speed)
    -- Called when hit with a certain speed
end

-- ═══════════════════════════════════
-- LANDING / FALLING
-- ═══════════════════════════════════

function OnLanded()
    -- Called when entity lands (air → ground)
    -- Requires RigidbodyComponent
end

function OnStartFalling()
    -- Called when entity starts falling (ground → air)
    -- Requires RigidbodyComponent
end

-- ═══════════════════════════════════
-- PAUSE / RESUME
-- ═══════════════════════════════════

function OnPause()
    -- Called on SetTimeScale(0) / PauseGame()
end

function OnResume()
    -- Called on ResumeGame()
end

-- ═══════════════════════════════════
-- LOCALIZATION
-- ═══════════════════════════════════

function OnLanguageChanged(newLang, oldLang)
    -- Game language changed
    Print("Language: " .. oldLang .. " -> " .. newLang)
end
```

### Level script (Level Script)

Written in `.icemap`. Has its own callbacks:

```lua
function OnLevelStart()
    -- Called when the level starts
    Print("Level started!")
end

function OnLevelUpdate(dt)
    -- Every frame (level)
end

function OnLevelLateUpdate(dt)
    -- Every frame after animations update, right before rendering
end

function OnLevelEnd()
    -- When level ends
end
```

> From the level script you can access: `FindEntityByTag`, `SetCameraPosition`, `GetCameraPosition`, `SpawnEntity`, `LineTrace` and other global + EntityLua/CameraLua/ComponentLua functions.

### OnConstruct vs OnCreate — execution order

```
┌─────────────────────────────────────────────────────────┐
│                    EDITOR MODE                          │
│                                                         │
│  Place class on scene ──► OnConstruct()                 │
│  Open level ─────────────► OnConstruct() for all        │
│  Click Compile ──────────► OnConstruct() in class view  │
│  Hot-reload class ───────► OnConstruct()                │
│                                                         │
│  OnCreate is NEVER called in editor                     │
│  IsEditorMode = true                                    │
└─────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────┐
│                   RUNTIME (Play Mode)                   │
│                                                         │
│  OnConstruct()  ← first, set up defaults/components     │
│       ↓                                                 │
│  OnCreate()     ← runtime init (input, coroutines)      │
│       ↓                                                 │
│  OnUpdate(dt)   ← every frame                           │
│  OnFixedUpdate(dt) ← fixed physics step                 │
│  OnLateUpdate(dt) ← after animations, pre-render        │
│       ↓                                                 │
│  OnDestroy()    ← cleanup                               │
│                                                         │
│  IsEditorMode = nil (not set)                           │
└─────────────────────────────────────────────────────────┘
```

**Rule of thumb:**
- **`OnConstruct`** = visual setup, component configuration, procedural generation (tilemap, spawn decorations). Works in editor AND runtime.
- **`OnCreate`** = runtime-only logic: start coroutines, bind input, play sounds, set AI state. Only runs in Play Mode.

| Variable | In Editor | In Runtime |
|----------|-----------|------------|
| `IsEditorMode` | `true` | `nil` / not set |

```lua
-- Example: tilemap generation visible in editor
function OnConstruct()
    SetTileAt(0, 0, 0, 1)  -- visible immediately in viewport
    SetTileAt(1, 0, 0, 2)

    if not IsEditorMode then
        -- Runtime-only initialization in OnConstruct
        Print("Spawned at runtime")
    end
end

function OnCreate()
    -- This only runs in Play Mode
    StartCoroutine(function()
        coroutine.yield(WaitSeconds(2))
        Print("Ready!")
    end)
end
```

---

## 4. Transform — Position, Scale, Rotation

> **Type:** Entity-bound (works with the current entity)

### Position

```lua
-- Set position (x, y, z)
SetPosition(100, 200, 0)

-- Get position → {x, y, z}
local pos = GetPosition()
Print("X=" .. pos.x .. " Y=" .. pos.y)

-- Get Transform component (userdata, may be nil)
local t = GetTransform()

-- Get/set individual axes
local x = GetPositionX()
local y = GetPositionY()
local z = GetPositionZ()
SetPositionX(100)
SetPositionY(200)
SetPositionZ(0)  -- Z = draw order (higher = farther)

-- Move by delta (dx, dy)
Translate(5, 0)  -- Move right by 5 pixels
```

### Scale

```lua
-- Set scale
SetScale(2, 2)  -- Double size

-- Get scale → {x, y}
local scale = GetScale()
```

### Rotation

```lua
-- Set rotation (degrees)
SetRotation(45)

-- Get rotation
local rot = GetRotation()

-- Add rotation
Rotate(10)  -- Rotate +10 degrees
```

### Utilities

```lua
-- Move toward a point with max speed. Returns true if reached.
local reached = MoveTowards(targetX, targetY, speed * dt)

-- Distance to a point
local dist = DistanceTo(x, y)

-- Rotate toward a direction vector
LookAtDirection(dx, dy)

```

---

### GLSLua — GPU Vertex Effects

GLSLua is a per-instance GPU vertex effect system. Effects (parallax, sway, wind) are applied
directly in the vertex shader on the GPU — no CPU transform manipulation.
Each sprite/flipbook instance has 8 float parameters packed into 2×vec4
that control the effect.

This system is **independent** of the Material system. You can use GLSLua for vertex effects
on entities with the standard rendering pipeline, or implement your own vertex effects
through custom Material shaders — they don't conflict.

#### Parameter layout

| Index | Name           | Default | Description                                    |
|-------|----------------|---------|------------------------------------------------|
| 0     | ParallaxX      | 0.0     | Horizontal parallax factor (camera-relative)   |
| 1     | ParallaxY      | 0.0     | Vertical parallax factor (camera-relative)     |
| 2     | SwayAmplitude  | 0.0     | Sway oscillation amplitude (world units)       |
| 3     | SwaySpeed      | 0.0     | Sway oscillation speed (radians/sec)           |
| 4     | SwayPhaseOffset| 0.0     | Sway phase offset (for desynchronization)      |
| 5     | SwayGradient   | 0.0     | Gradient factor: 0=uniform, 1=bottom fixed     |
| 6     | WindStrength   | 0.0     | Wind effect strength                           |
| 7     | WindSpeed      | 1.5     | Wind effect speed                              |

#### High-level functions

```lua
-- Parallax (auto-detects sprite or flipbook)
GLSL_SetParallax(factorX, factorY [, instanceIndex])
local px, py = GLSL_GetParallax([instanceIndex])

-- Sway (phaseOffset, gradient are optional — keep previous if omitted)
GLSL_SetSway(amplitude, speed [, phaseOffset] [, gradient] [, instanceIndex])
local amp, spd, phase, grad = GLSL_GetSway([instanceIndex])

-- Wind
GLSL_SetWind(strength [, speed] [, instanceIndex])
local str, spd = GLSL_GetWind([instanceIndex])

-- Reset all effects to defaults
GLSL_ClearEffects([instanceIndex])
```

#### Low-level raw parameter access

```lua
-- Set/get any parameter by index (0-7), see table above
GLSL_SetParam(paramIndex, value [, instanceIndex])
local val = GLSL_GetParam(paramIndex [, instanceIndex])
```

#### Component-specific functions

Apply to all instances (index = nil) or a specific instance by index:

```lua
-- Sprite-only
GLSL_SpriteSetParallax(factorX, factorY [, index])
GLSL_SpriteSetSway(amplitude, speed [, phaseOffset] [, gradient] [, index])
GLSL_SpriteSetWind(strength [, speed] [, index])

-- Flipbook-only
GLSL_FlipbookSetParallax(factorX, factorY [, index])
GLSL_FlipbookSetSway(amplitude, speed [, phaseOffset] [, gradient] [, index])
GLSL_FlipbookSetWind(strength [, speed] [, index])
```

#### Examples

```lua
-- Background parallax: moves at half camera speed
function OnBegin()
    GLSL_SetParallax(0.5, 0.5)
end

-- Tree sway: bottom fixed, top sways, with wind
function OnBegin()
    SetSpritePivot(0.5, 1.0)  -- pivot at bottom center
    GLSL_SetSway(3.0, 2.0, math.random() * 6.28, 1.0)  -- gradient=1 → bottom fixed
    GLSL_SetWind(1.5, 1.2)
end

-- Grass field: each instance has unique phase
function OnBegin()
    for i = 0, GetSpriteCount() - 1 do
        GLSL_SpriteSetSway(2.0, 3.0, math.random() * 6.28, 0.8, i)
        GLSL_SpriteSetWind(0.5, 1.0, i)
    end
end

-- Raw param access for custom effects
GLSL_SetParam(2, 5.0)   -- SwayAmplitude = 5.0
GLSL_SetParam(6, 2.0)   -- WindStrength = 2.0
local wind = GLSL_GetParam(6)
```

#### UV Scroll (Auto-Parallax)

`SetSpriteUVScroll` / `GetSpriteUVScroll` allow you to continuously offset a sprite's
texture coordinates at runtime. Combined with `GLSL_SpriteSetParallax`, this enables
**auto-scrolling parallax** — the texture tiles infinitely via GPU (GL_REPEAT) while also
drifting automatically over time. This is ideal for clouds, water, fog, or any background
layer that should scroll on its own in addition to camera-based parallax.

```lua
-- Set UV scroll offset for sprite instance (in normalized UV space, 0..1 = one full texture)
SetSpriteUVScroll(offsetX, offsetY [, instanceIndex])

-- Get current UV scroll offset → {x, y}
local scroll = GetSpriteUVScroll([instanceIndex])
```

**Example — auto-scrolling clouds with infinite parallax:**

```lua
local IDX_CLOUDS = 2
cloudScrollSpeed = 6.0   -- world units per second
cloudScrollU = 0.0

function OnCreate()
    -- GPU parallax for infinite tiling when camera moves
    GLSL_SpriteSetParallax(-0.04, -0.03, IDX_CLOUDS)

    -- Convert world speed to UV speed
    local size = GetSpriteSize(IDX_CLOUDS)
    local scale = GetSpriteLocalScale(IDX_CLOUDS)
    cloudUVSpeed = cloudScrollSpeed / (size.width * scale.x)
end

function OnUpdate(dt)
    -- Accumulate UV offset — texture wraps seamlessly via GL_REPEAT
    cloudScrollU = cloudScrollU + cloudUVSpeed * dt
    SetSpriteUVScroll(cloudScrollU, 0.0, IDX_CLOUDS)
end
```

#### UV Scale (Tiling)

`SetSpriteUVScale` / `GetSpriteUVScale` control how many times a sprite's texture
repeats (tiles) across the quad. The default is `(1.0, 1.0)` — the texture maps
1:1 to the sprite. Setting `(2.0, 1.0)` makes the texture tile twice horizontally.

Combined with `SetSpriteUVScroll`, this is essential for **auto-scrolling layers
that are wider than the viewport**: scale the sprite up so its edges are off-screen,
then set UV scale to match so the texture tiles correctly instead of stretching.

```lua
-- Set UV tiling factor for sprite instance (1.0 = default, 2.0 = tile twice, etc.)
SetSpriteUVScale(scaleX, scaleY [, instanceIndex])

-- Get current UV scale → {x, y}
local uvs = GetSpriteUVScale([instanceIndex])
```

**Example — scrolling cloud wider than the viewport (no visible seam):**

```lua
local IDX_CLOUDS = 2

function OnCreate()
    -- Make cloud sprite 2× wider than default so edges are off-screen
    SetSpriteLocalScale(0.5, 0.25, IDX_CLOUDS)  -- 2× wider than (0.25, 0.25)

    -- Tile the texture 2× horizontally to compensate for the wider sprite
    SetSpriteUVScale(2.0, 1.0, IDX_CLOUDS)

    -- ...
end
```

### Aiming and direction

```lua
-- Rotate entity toward a point
LookAt(mouseX, mouseY)

-- Rotate entity toward another entity
LookAtEntity(enemyId)

-- Angle from self to a point (degrees)
local angle = GetAngleTo(targetX, targetY)

-- Angle from self to an entity (degrees)
local angle = GetAngleToEntity(entityId)

-- Forward vector from current rotation → {x, y}
local fwd = GetForwardVector()

-- Right vector (perpendicular to forward) → {x, y}
local right = GetRightVector()
```

### Movement

```lua
-- Move in a direction angle
MoveInDirection(angleDeg, speed, dt)

-- Move forward (by current rotation)
MoveForward(speed, dt)

-- Rotate position around a point
RotateAround(cx, cy, angleDeg)

-- Orbit around a point
OrbitAround(cx, cy, radius, angleDeg)

-- Flip sprite by movement direction (platformers)
FaceMovementDirection()

-- Flip sprite by sign of dirX
FaceDirection(dirX)
```

### Example: top-down shooter aiming and movement

```lua
function OnUpdate(dt)
    -- Aim at mouse
    local mpos = GetMouseWorldPosition()
    LookAt(mpos.x, mpos.y)

    -- Move forward in facing direction
    if IsKeyPressed("w") then
        MoveForward(200, dt)
    end

    -- Strafe by angle
    local angle = GetAngleTo(mpos.x, mpos.y)
    if IsKeyPressed("a") then
        MoveInDirection(angle - 90, 150, dt)
    end
    if IsKeyPressed("d") then
        MoveInDirection(angle + 90, 150, dt)
    end
end
```

### Example: orbital satellite

```lua
function OnConstruct()
    orbitAngle = 0
end

function OnUpdate(dt)
    local boss = FindEntityByTag("Boss")
    if boss then
        local bpos = GetEntityPosition(boss)
        orbitAngle = orbitAngle + 90 * dt
        OrbitAround(bpos.x, bpos.y, 100, orbitAngle)
    end
end
```

---

## 5. Physics — Physics (Box2D)

> **Type:** Entity-bound. Requires **RigidbodyComponent**.

### Velocity

```lua
-- Set velocity (px/sec)
SetVelocity(200, 0)
SetVelocityX(200)
SetVelocityY(-300)

-- Get velocity → {x, y}
local vel = GetVelocity()
local vx = GetVelocityX()
local vy = GetVelocityY()

-- Speed (magnitude)
local speed = GetSpeed()

-- Clamp velocity
ClampVelocity(500)     -- Max overall speed
ClampVelocityX(300)    -- Max X speed
ClampVelocityY(300)    -- Max Y speed

-- Stop movement (zero velocity + angular velocity)
StopMovement()
```

> On an entity that has an **AI component**, `StopMovement()` stops AI locomotion
> instead (see the Navigation section) — in a side-scroller that zeroes only the
> horizontal velocity so gravity keeps working. Without an AI component it is the
> plain physics stop shown above.

### Forces and impulses

```lua
-- Impulse — instant velocity change (jump, hit)
AddImpulse(0, 500)  -- Jump up

-- Force — constant acceleration (engine, wind)
AddForce(100, 0)     -- Push right

-- Force at point (creates torque)
AddForceAtPoint(100, 0, pointX, pointY)

-- Impulse at point
AddImpulseAtPoint(100, 200, pointX, pointY)

-- Impulse/force in angle direction
AddImpulseDirection(45, 300)
AddForceDirection(90, 50)

-- Torque (rotational moment)
AddTorque(50)

-- Angular impulse
AddAngularImpulse(10)
```

### Gravity and state

```lua
-- Gravity scale (0 = no gravity, 2 = double)
SetGravityScale(0)
local gs = GetGravityScale()

-- World gravity (global)
SetGravity(0, -9.81)
local gravity = GetGravity()  -- → {x, y}

-- State checks
local falling = IsFalling()    -- Falling down?
local rising  = IsRising()     -- Moving up?
local ground  = IsGrounded()   -- On ground? (raycast down)
local onWall = IsOnWall()       -- Wall nearby?
local onWallRight = IsOnWall(1) -- Check right
local onWallLeft = IsOnWall(-1) -- Check left
local ceiling = IsCeiling()     -- Ceiling overhead
```

> `IsGrounded`, `IsInAir`, `IsOnWall` and `IsCeiling` probe with short rays and only count **solid** geometry. Sensor shapes and the entity's own body are skipped and do **not** block the probe — a trigger volume, coin or checkpoint lying on the floor will not stop `IsGrounded()` from finding the floor underneath it. `IsInAir()` is exactly `not IsGrounded()` — both run the same check.

### Body properties

```lua
-- Body type: "Dynamic", "Static", "Kinematic"
SetBodyType("Dynamic")
local type = GetBodyType()

-- Mass and inertia
local mass = GetMass()
local inertia = GetInertia()

-- Damping (movement resistance, like viscosity)
SetLinearDamping(2.0)
SetAngularDamping(1.0)
local ld = GetLinearDamping()
local ad = GetAngularDamping()

-- Fixed rotation (no physics rotation)
SetFixedRotation(true)
local fixed = IsFixedRotation()

-- Bullet mode (continuous collision for fast objects)
SetBullet(true)
local isBullet = IsBullet()

-- Sleep (optimization for static bodies)
SetAwake(true)
EnableSleep(true)
local awake = IsAwake()
local sleepEnabled = IsSleepEnabled()

-- Enable/disable physical body
SetBodyEnabled(true)
local bodyEnabled = IsBodyEnabled()

-- Angular velocity (degrees/sec)
SetAngularVelocity(90)
local av = GetAngularVelocity()
```

### Freeze and ragdoll

```lua
-- Freeze (make static)
Freeze()

-- Unfreeze (make dynamic)
Unfreeze()

-- Ragdoll physics
EnableRagdoll()
DisableRagdoll()
local ragdoll = IsRagdoll()

-- Ragdoll with initial impulse (impact effect)
EnableRagdollWithImpulse(300, -200, 5.0)
-- ix, iy = linear impulse, angularImpulse = rotational
```

### Jump and crouch

```lua
-- Jump (only if grounded, sets upward speed)
Jump(500)                        -- Jump with force 500 (only if IsGrounded)

-- Forced jump (ignores grounded check — double jump, wall jump, etc.)
JumpForce(400)                   -- Works even in air

-- Stop jump (cut vertical speed — control jump height)
-- Call when jump key released. Multiplies upward velocity by 0.25.
StopJump()

-- Jump state
local jumping = IsJumping()      -- true if airborne after Jump/JumpForce (reset on land)

-- Jump count (auto-reset on land)
local count = GetJumpCount()     -- 0 on ground, 1 after first, 2 after double...
ResetJumpCount()                 -- Force reset (e.g. wall jump grants extra jump)

-- Crouch (shrinks colliders, feet stay in place)
Crouch()                         -- Default: 50% height
Crouch(0.6)                      -- 60% of original collider height

-- Stand up (restore original collider size)
UnCrouch()

-- Crouch state
local crouching = IsCrouching()
```

### Typical jump + crouch example

```lua
local MAX_JUMPS = 2  -- Double jump

function OnUpdate(dt)
    -- Jump with height control
    if IsKeyJustPressed("space") and GetJumpCount() < MAX_JUMPS then
        if GetJumpCount() == 0 then
            Jump(600)            -- First jump (from ground)
        else
            JumpForce(500)       -- Air jump (double)
        end
    end

    -- Release = cut jump (variable height)
    if IsKeyJustReleased("space") then
        StopJump()
    end

    -- Crouch
    if IsKeyPressed("s") or IsKeyPressed("down") then
        Crouch(0.5)
    else
        UnCrouch()
    end
end
```

### Movement state

```lua
-- Is the entity moving? (speed > threshold, default 1.0)
local moving = IsMoving()
local moving = IsMoving(5.0)  -- Custom threshold

-- Normalized direction of velocity → {x, y}
local dir = GetMovementDirection()

-- Not grounded? (alias for not IsGrounded())
local airborne = IsInAir()
```

### Shapes, teleport, and physics position

```lua
-- Number of collider shapes on the body
local shapeCount = GetShapeCount()

-- Shape settings (by index)
SetShapeDensity(1.0, 0)
SetShapeFriction(0.3, 0)
SetShapeRestitution(0.2, 0)
SetShapeSensor(true, 0)

-- Get shape settings (by index)
local density = GetShapeDensity(0)
local friction = GetShapeFriction(0)
local restitution = GetShapeRestitution(0)
local isSensor = IsShapeSensor(0)

-- One-way platform (shape passes through from one side)
SetShapeOneWay(true, 0)
local oneWay = IsShapeOneWay(0)

-- One-way direction: 1 = Up (default, solid from above), 2 = Down, 3 = Left, 4 = Right
-- Direction is the side from which the platform IS solid (the side bodies cannot pass through)
SetShapeOneWayDirection(1, 0)
local dir = GetShapeOneWayDirection(0)  -- returns 0 if shape is not one-way

-- Contact events (begin/end contact callbacks)
SetShapeContactEvents(true, 0)
local contacts = AreShapeContactEventsEnabled(0)

-- Sensor events (sensor enter/exit callbacks)
SetShapeSensorEvents(true, 0)
local sensors = AreShapeSensorEventsEnabled(0)

-- Hit events (collision hit callbacks)
SetShapeHitEvents(true, 0)
local hits = AreShapeHitEventsEnabled(0)

-- Pre-solve events (called before collision response is calculated)
SetShapePreSolveEvents(true, 0)
local preSolve = AreShapePreSolveEventsEnabled(0)

-- Teleport (updates Transform too)
TeleportTo(100, 200)

-- Position/rotation from physics
local physPos = GetPhysicsPosition()  -- → {x, y}
local physRot = GetPhysicsRotation()
```

### Collision filters (Box2D)

```lua
SetCollisionCategory(0x0002)
SetCollisionMask(0xFFFF)
SetCollisionGroup(-1)
SetCollisionFilter(0x0002, 0xFFFF, 0)

local category = GetCollisionCategory()
local mask = GetCollisionMask()
local group = GetCollisionGroup()

DisableCollision()
EnableCollision()

-- For another entity
SetEntityCollisionFilter(entityId, 0x0002, 0xFFFF, 0)
```

### Named collision groups (entity-bound)

> Apply a **named** collision group (defined in **Settings → Collision**) to every shape of this entity's `RigidbodyComponent` at runtime. Group `categoryBits` / `maskBits` are derived from the collision matrix automatically.

```lua
-- Apply a named collision group to all shapes of this body.
-- Returns true on success, false if no rigidbody / no runtime body / group not found.
local ok = SetCollisionGroupByName("Player")

-- Name of the group currently assigned to the first shape of this body
-- (looked up by its categoryBits). Returns "" if not found / no runtime body.
local groupName = GetCollisionGroupName()

-- Change the collision mode for every shape on this body.
-- 0 = NoCollision     (maskBits = 0, no contact/sensor/hit events)
-- 1 = QueryOnly       (force sensor; sensor events on, contact/hit events off)
-- 2 = PhysicsOnly     (solid; contact + hit events on, sensor events off)
-- 3 = QueryAndPhysics (solid; all events enabled — default)
SetCollisionEnabled(2)
```

> These functions only affect the **runtime** Box2D filter/flags — they do not change the serialized `CollisionGroupIndex` / `CollisionEnabled` fields of the collider components. Use them inside `OnUpdate` / event handlers to flip behaviour at runtime.

### Collision Groups (global `CollisionGroups.*`)

> **Type:** Global. Read / edit the project-wide collision matrix from Lua. Groups and their pairwise `collide?` flags live in **Settings → Collision** (saved in `Config/CollisionGroups.json`).

```lua
-- Lookup
local idx     = CollisionGroups.GetIndex("Player")     -- → int (−1 if not found)
local name    = CollisionGroups.GetName(idx)           -- → string ("" if empty slot)
local count   = CollisionGroups.GetCount()             -- → int (number of slots, max 64)
local names   = CollisionGroups.GetAllNames()          -- → array of non-empty group names

-- Raw Box2D filter bits for a group index
local catBits  = CollisionGroups.GetCategoryBits(idx)  -- → uint64
local maskBits = CollisionGroups.GetMaskBits(idx)      -- → uint64

-- Pairwise collision matrix
local a = CollisionGroups.GetIndex("Player")
local b = CollisionGroups.GetIndex("Enemy")
local doesCollide = CollisionGroups.DoCollide(a, b)    -- → bool
CollisionGroups.SetCollide(a, b, true)                 -- symmetric; also updates mask bits

-- Build a 64-bit layer mask from group names (or indices). Useful as input
-- for LineTrace / Overlap* layer filtering.
local mask = CollisionGroups.LayerMaskFromNames({"Enemy", "Environment"})
local all  = CollisionGroups.LayerMaskAll()           -- → uint64 (~0)
local none = CollisionGroups.LayerMaskNone()          -- → uint64 (0)

-- Persistent trace-layer override (thread-local). While set to a non-zero
-- mask, every LineTrace / CircleTrace / BoxTrace / CapsuleTrace / Overlap*
-- call uses this mask as its query filter. Pass 0 to clear.
CollisionGroups.SetTraceLayerMask(mask)
local cur = CollisionGroups.GetTraceLayerMask()        -- → uint64

-- Scoped helper: runs `fn` with the trace-layer mask temporarily set to
-- `mask`, then restores the previous override automatically (even on
-- Lua error). Returns the first value returned by `fn`.
local hit = CollisionGroups.WithLayerMask(mask, function()
    return LineTrace(x1, y1, x2, y2)
end)
```

### Control physics of other entities

```lua
-- Set velocity of another entity
SetEntityVelocity(entityId, 150, 0)

-- Add impulse to another entity
AddEntityImpulse(entityId, 0, -400)

-- Gravity scale for another entity
SetEntityGravityScale(entityId, 0.5)
```

### Mouse Joint (dragging a physics body)

```lua
CreateMouseJoint(targetX, targetY)
CreateMouseJoint(targetX, targetY, 1000, 5.0, 0.7)  -- maxForce, hertz, dampingRatio

SetMouseJointTarget(targetX, targetY)
DestroyMouseJoint()
local has = HasMouseJoint()
```

### World physics settings

> These functions control scene-level physics parameters. Accessible from any entity with **RigidbodyComponent**.

```lua
-- Pixels Per Meter (read-only)
local ppm = GetPPM()

-- Sub-step count (solver iterations per step, 1..64)
SetSubStepCount(8)
local sub = GetSubStepCount()

-- Fixed timestep (physics step size)
SetFixedTimestep(1/60)
local step = GetFixedTimestep()

-- Restitution threshold (minimum speed for bounce)
SetRestitutionThreshold(1.0)
local rest = GetRestitutionThreshold()

-- Hit event speed threshold
SetHitEventThreshold(5.0)
local hit = GetHitEventThreshold()

-- Contact tuning (hertz, dampingRatio, maxPushSpeed)
SetContactTuning(60, 0.5, 3.0)
local ct = GetContactTuning()  -- → {hertz, dampingRatio, pushSpeed}

-- Maximum linear speed
SetMaxLinearSpeed(200)
local maxSpeed = GetMaxLinearSpeed()

-- Enable/disable body sleeping
SetWorldSleeping(true)
local sleep = IsWorldSleepingEnabled()

-- Enable/disable continuous collision detection
SetWorldContinuous(true)
local continuous = IsWorldContinuousEnabled()
```

---

## 6. Input — Input (Keyboard, Mouse, Gamepad, Touch)

> **Type:** Global functions

### Keyboard

```lua
-- Key held?
if IsKeyPressed("space") then Jump() end
if IsKeyPressed("w") then MoveUp() end

-- Key just pressed? (one frame)
if IsKeyJustPressed("e") then Interact() end

-- Key just released?
if IsKeyJustReleased("shift") then StopSprint() end

-- Any key pressed?
if IsAnyKeyPressed("w", "up") then MoveUp() end

-- All keys pressed?
if IsAllKeysPressed("ctrl", "s") then Save() end
```

**Available key names:**

| Category | Keys |
|---------|------|
| Letters | `a`-`z` |
| Digits | `0`-`9` |
| Arrows | `up`, `down`, `left`, `right` |
| Control | `space`, `enter`/`return`, `escape`/`esc`, `tab`, `backspace`, `delete` |
| Modifiers | `shift`/`lshift`/`rshift`, `ctrl`/`lctrl`/`rctrl`, `alt`/`lalt`/`ralt` |
| Function | `f1`-`f12` |
| Numpad | `numpad0`-`numpad9`, `numpad_enter`, `numpad_plus`, `numpad_minus`, etc. |
| Other | `capslock`, `numlock`, `scrolllock`, `printscreen`, `pause`, `insert`, `home`, `end`, `pageup`, `pagedown`, `grave`, `semicolon`, `comma`, `period`, `slash`, `backslash`, `minus`, `equals`, `leftbracket`, `rightbracket`, `apostrophe` |

### Axes (for movement)

```lua
-- One axis: -1 (negative), 0, or +1 (positive)
local moveX = GetAxis("a", "d")     -- A = -1, D = +1

-- 2D axis (normalized!) → {x, y}
local move = GetAxis2D("a", "d", "s", "w")
SetVelocity(move.x * speed, move.y * speed)

-- Universal movement (keyboard + gamepad automatically)
local input = GetUniversalMovement("a", "d", "s", "w")
```

### Mouse

```lua
-- Mouse buttons (1 = left, 2 = middle, 3 = right)
if IsMousePressed(1) then Shoot() end
if IsMouseJustPressed(3) then Aim() end
if IsMouseJustReleased(1) then StopShoot() end

-- Mouse position (screen coordinates)
local mx = GetMouseX()
local my = GetMouseY()
local mpos = GetMousePosition()  -- → {x, y}

-- Mouse position in world coordinates
local wpos = GetMouseWorldPosition()  -- → {x, y}

-- Screen ↔ world coordinate conversion
local world = ScreenToWorld(400, 300)  -- → {x, y}
local screen = WorldToScreen(10, 20)   -- → {x, y}

-- Mouse wheel scroll
local scroll = GetMouseScroll()          -- vertical
local scrollX = GetMouseScrollX()        -- horizontal (trackpad, tilt wheel)

-- Mouse delta per frame (for camera, drag, etc.)
local delta = GetMouseDelta()  -- → {x, y} pixels

-- Relative mouse mode (FPS — captures and hides mouse, reports deltas only)
SetRelativeMouseMode(true)               -- enable
SetRelativeMouseMode(false)              -- disable
local relative = GetRelativeMouseMode()  -- is enabled?

-- Cursor
-- Cursor (.ice_sprite and .ice_flipbook assets only)
ShowCursor()
HideCursor()
local visible = IsCursorVisible()
SetCursor("Content/Sprites/cursor.ice_sprite", 0, 0, 1.0)              -- Auto-detect by extension
SetCursorSprite("Content/Sprites/cursor.ice_sprite", 0, 0, 1.0)        -- From sprite asset
SetCursorFlipbook("Content/Flipbooks/cursor.ice_flipbook", 0, 0, 1.0)  -- Animated
SetCursorHotspot(8, 8)              -- Change hotspot (pivot)
SetCursorScale(2.0)                 -- Change scale
local hx, hy = GetCursorHotspot()
local s = GetCursorScale()
ResetCursor()
local hasCursor = HasCustomCursor()
local cursorPath = GetCustomCursorPath()
local animated = IsCursorAnimated()
local cursorType = GetCursorType()  -- "None" | "Sprite" | "Flipbook"
SetCursorPosition(400, 300)
```

### Gamepad

```lua
-- Connected?
if IsGamepadConnected() then ... end
if IsGamepadConnected(1) then ... end  -- Second gamepad
local count = GetGamepadCount()
local name = GetGamepadName()

-- Buttons (strings or PlayStation names)
if IsGamepadButtonPressed("a") then Jump() end       -- Xbox: A
if IsGamepadButtonPressed("cross") then Jump() end    -- PS: ×
if IsGamepadButtonJustPressed("x") then Attack() end  -- Xbox: X / PS: □
if IsGamepadButtonJustReleased("b") then ... end

-- Sticks → {x, y} (-1 to 1)
local left = GetGamepadLeftStick()
local right = GetGamepadRightStick()

-- Triggers (0..1)
local lt = GetGamepadTriggerLeft()
local rt = GetGamepadTriggerRight()

-- Universal axis
local axis = GetGamepadAxis("leftx")  -- "leftx", "lefty", "rightx", "righty", "lt"/"l2", "rt"/"r2"

-- Rumble
SetGamepadRumble(0.5, 0.8, 200)  -- lowFreq, highFreq, milliseconds
StopGamepadRumble()

-- Trigger rumble (Xbox/PS5 DualSense)
SetGamepadTriggerRumble(0.3, 0.7, 200)  -- left, right, milliseconds
StopGamepadTriggerRumble()

-- LED (PS4/PS5 lightbar)
SetGamepadLED(255, 0, 0)           -- red
SetGamepadLED(0, 255, 0, 1)       -- green, second gamepad

-- Controller type detection
local gtype = GetGamepadType()  -- "xbox360"/"xboxone"/"ps3"/"ps4"/"ps5"/"switch_pro"/"joycon_left"/"joycon_right"/"joycon_pair"/"gamecube"/"standard"/"unknown"

-- Button labels (actual face label per controller type)
local label = GetGamepadButtonLabel("a")  -- "a" on Xbox, "cross" on PlayStation

-- Battery / power info
local percent = GetGamepadPowerPercent()  -- 0-100 or -1 if unknown
local pstate = GetGamepadPowerState()     -- "battery" / "charging" / "charged" / "no_battery" / "unknown"

-- Touchpad (DualSense / DualShock 4)
if GamepadHasTouchpad() then
    local maxFingers = GetGamepadTouchpadFingerCount()  -- max supported fingers
    local finger = GetGamepadTouchpadFinger(0)          -- → {down, x, y, pressure}
end

-- Gamepad sensors (DualShock 4 / DualSense / Joy-Con)
if GamepadHasSensor("gyro") then
    SetGamepadSensorEnabled("gyro", true)
    local gyro = GetGamepadSensorData("gyro")  -- → {x, y, z} rad/s
    local rate = GetGamepadSensorDataRate("gyro")
end
if GamepadHasSensor("accel") then
    SetGamepadSensorEnabled("accel", true)
    local accel = GetGamepadSensorData("accel")  -- → {x, y, z} m/s²
end
local enabled = IsGamepadSensorEnabled("gyro")

-- Deadzone
SetGamepadDeadzone(0.15)
local dz = GetGamepadDeadzone()

-- Capability detection (especially important for Web — browsers usually disable rumble)
if IsGamepadRumbleSupported() then SetGamepadRumble(0.5, 0.5, 200) end
if IsGamepadTriggerRumbleSupported() then SetGamepadTriggerRumble(0.3, 0.7, 200) end
if IsGamepadLEDSupported() then SetGamepadLED(0, 255, 0) end

-- Per-pad index (0-3)
if IsGamepadRumbleSupported(1) then SetGamepadRumble(0.5, 0.5, 200, 1) end
```

| Function | Description |
|---|---|
| `IsGamepadRumbleSupported(idx?)` | Does the pad support low/high frequency rumble (returns false in most browsers) |
| `IsGamepadTriggerRumbleSupported(idx?)` | Does the pad support adaptive trigger rumble (DualSense / Xbox Elite) |
| `IsGamepadLEDSupported(idx?)` | Does the pad have an RGB LED that `SetGamepadLED` can drive |

**Gamepad sensor types:**

| String | Description |
|---|---|
| `"accel"` / `"accelerometer"` | Accelerometer |
| `"gyro"` / `"gyroscope"` | Gyroscope |
| `"accel_l"` | Left Joy-Con accelerometer |
| `"gyro_l"` | Left Joy-Con gyroscope |
| `"accel_r"` | Right Joy-Con accelerometer |
| `"gyro_r"` | Right Joy-Con gyroscope |

**Gamepad buttons:**

| Xbox | PlayStation | String |
|------|-------------|--------|
| A | × (Cross) | `"a"` / `"cross"` |
| B | ○ (Circle) | `"b"` / `"circle"` |
| X | □ (Square) | `"x"` / `"square"` |
| Y | △ (Triangle) | `"y"` / `"triangle"` |
| LB | L1 | `"lb"` / `"l1"` / `"leftshoulder"` |
| RB | R1 | `"rb"` / `"r1"` / `"rightshoulder"` |
| Back | Share | `"back"` / `"select"` / `"share"` |
| Start | Options | `"start"` / `"menu"` / `"options"` |
| LS | L3 | `"leftstick"` / `"l3"` |
| RS | R3 | `"rightstick"` / `"r3"` |
| D-Pad | D-Pad | `"dpadup"`, `"dpaddown"`, `"dpadleft"`, `"dpadright"` |
| Touchpad | Touchpad | `"touchpad"` |

### Touch screen (mobile)

```lua
-- Support
local supported = IsTouchSupported()
local count = GetTouchCount()

-- Touches
if IsTouchPressed() then ... end
if IsTouchJustPressed(0) then ... end  -- Finger 0
if IsTouchJustReleased(0) then ... end

-- Finger position (normalized 0..1)
local pos = GetTouchPosition(0)  -- → {x, y}

-- Finger position (pixels)
local pix = GetTouchPositionPixels(0)  -- → {x, y}

-- Delta (movement)
local delta = GetTouchDelta(0)  -- → {x, y}

-- Pressure
local pressure = GetTouchPressure(0)

-- Multi-touch gestures
local pinching = IsPinching()
local scale = GetPinchScale()
local rotation = GetPinchRotation()
```

### Swipe gestures

```lua
-- Was there a swipe this frame?
if IsSwipe() then
    local dir = GetSwipeDirection()  -- "left", "right", "up", "down"
    Log("Swipe: " .. dir)
end

-- Check specific direction
if IsSwipeLeft() then ... end
if IsSwipeRight() then ... end
if IsSwipeUp() then ... end
if IsSwipeDown() then ... end

-- Swipe count (multi-touch — multiple fingers at once)
local count = GetSwipeCount()
for i = 0, count - 1 do
    local dir = GetSwipeDirection(i)
    local delta = GetSwipeDelta(i)         -- → {x, y} (normalized delta)
    local velocity = GetSwipeVelocity(i)   -- speed (distance/second)
    local distance = GetSwipeDistance(i)    -- traveled distance (normalized)
end

-- Sensitivity settings
SetSwipeMinDistance(0.05)   -- min distance to trigger (default 0.05 = 5% of screen)
SetSwipeMaxDuration(0.5)    -- max swipe time in seconds (default 0.5)
local minDist = GetSwipeMinDistance()
local maxDur = GetSwipeMaxDuration()
```

| Function | Description |
|---|---|
| `IsSwipe()` | Was there a swipe this frame (any direction) |
| `IsSwipeLeft()` / `Right()` / `Up()` / `Down()` | Swipe in specific direction |
| `GetSwipeCount()` | Number of swipes this frame |
| `GetSwipeDirection(index?)` | Direction: `"left"`, `"right"`, `"up"`, `"down"` |
| `GetSwipeDelta(index?)` | `{x, y}` — normalized finger displacement |
| `GetSwipeVelocity(index?)` | Swipe speed (distance/second) |
| `GetSwipeDistance(index?)` | Swipe length (normalized, 0..1) |
| `SetSwipeMinDistance(float)` | Min distance to trigger (default `0.05`) |
| `GetSwipeMinDistance()` | Current min distance |
| `SetSwipeMaxDuration(float)` | Max swipe duration in seconds (default `0.5`) |
| `GetSwipeMaxDuration()` | Current max duration |

### Device vibration (Haptic)

```lua
-- Support check
if IsHapticSupported() then
    PlayHaptic(0.7, 300)    -- strength (0..1), duration in ms
    StopHaptic()             -- stop vibration
end

-- Crisp UI feedback: one system-tuned tap instead of a hand-timed buzz
PlayHapticPreset("tick")          -- "tick" | "click" | "doubleclick" | "heavyclick"

-- Custom waveform: segment durations + per-segment strength
PlayHapticPattern({ 0, 40, 80, 120 }, { 0.0, 0.4, 0.0, 1.0 })

-- Looping pattern: loop forever from segment 1 -> 60 ms on, 60 ms off, ...
PlayHapticPattern({ 60, 60 }, { 1.0, 0.0 }, 1)
StopHaptic()                      -- always needed to end a looping pattern
```

| Function | Description |
|---|---|
| `IsHapticSupported()` | Is a haptic device available (mobile vibration, rumble motor, etc.) |
| `IsHapticAmplitudeControlSupported()` | Does the device honour `strength`. When `false`, every vibration plays at the system default level and only `durationMs` shapes the feel |
| `PlayHaptic(strength, durationMs)` | Start vibration (`strength` 0..1, `durationMs` in milliseconds) |
| `PlayHapticPreset(name?)` | Play a short system-tuned effect: `"tick"` (default), `"click"`, `"doubleclick"`, `"heavyclick"` |
| `PlayHapticPattern(timingsMs, amplitudes?, repeatIndex?)` | Play a waveform. `timingsMs[i]` is the length of segment `i` in ms, `amplitudes[i]` its strength `0..1` (`0` = pause). Without `amplitudes` the segments alternate on/off starting with **on**. `repeatIndex` is the 1-based table index the pattern loops back to and runs until `StopHaptic()`; omit it (or pass `-1`) to play once |
| `StopHaptic()` | Stop current vibration |

> **Platform backends:**
> • **Windows / Linux / macOS** — the first SDL force-feedback device (FFB wheel or joystick). A desktop without such hardware reports `false`. Presets fall back to a short `PlayHaptic`, patterns to one rumble of the total "on" time at peak strength.
> • **Android** — the engine drives `android.os.Vibrator` natively through `IceBoxHaptics` (`VibratorManager` on Android 12+), so `strength` maps to a real `VibrationEffect` amplitude where the motor supports it, `PlayHapticPreset` uses the OEM-tuned `EFFECT_TICK / CLICK / DOUBLE_CLICK / HEAVY_CLICK`, and `PlayHapticPattern` becomes a `VibrationEffect` waveform with looping. Every vibration is tagged with game/media usage attributes so it is governed by the game's own volume of feedback rather than by the system "touch feedback" toggle. Requires the `VIBRATE` permission, which the Android build template already declares. If the native class is missing (an old generated project) the engine silently falls back to SDL's haptic device.
> • **iOS** — Core Haptics (`CHHapticEngine`) with a `UIImpactFeedbackGenerator` fallback; SDL itself ships only a dummy haptic driver on iOS, so the engine drives the Taptic Engine natively. Durations of 60 ms or less play as a single transient tap, longer ones as a continuous event; patterns become a multi-event `CHHapticPattern`. `repeatIndex` is ignored — Core Haptics has no pattern loop, so re-trigger from Lua instead. iPad / iPod touch have no vibration motor and report `false`.
> • **Web** — `navigator.vibrate` on mobile browsers (Chrome, Firefox, Edge, Samsung Internet on Android). Desktop browsers report `false`. **Safari never implemented the Vibration API, so a browser on iOS/iPadOS can never vibrate the device** — that is a platform limit, not an engine one. The Web API has no amplitude control: `strength` is ignored there and only `durationMs` applies (`IsHapticAmplitudeControlSupported()` returns `false`); patterns are converted to a `navigator.vibrate` on/off array and `repeatIndex` is ignored. Chrome additionally drops vibration calls made before the user has interacted with the page.
>
> `PlayHaptic` targets the *device*, not the controller. For gamepad motors always use `SetGamepadRumble(...)` — that one **does** work in the browser (Chromium exposes `gamepad.vibrationActuator`), so a web build can rumble a pad even where `IsHapticSupported()` is `false`.
>
> On mobile the device backend always wins over SDL's haptic device, and vibration is stopped automatically when the app goes to the background — a looping pattern can never keep buzzing behind the home screen.
>
> **In the editor's Remote Preview**, every call in this section is routed to the connected Android device instead of the desktop, and `IsHapticSupported()` reports `true` — so mobile vibration can be felt and tuned without a full build. See *Editor → Remote Preview*.

### Force-Feedback Effects (advanced haptic)

Beyond simple rumble the engine exposes the full SDL3 `SDL_HapticEffect` pipeline: **constant force**, **periodic waves** (sine, triangle, sawtooth), **ramps** (rising / falling intensity), and **left/right** (independent low- and high-frequency motors on console gamepads). Effects are played on the **first opened haptic device** (force-feedback wheel, joystick or gamepad with FFB). Mobile rumble motors usually only support `"leftright"`.

Each effect is described by a Lua table; common fields:

| Field | Meaning | Default |
|---|---|---|
| `type` | `"constant"` │ `"sine"` │ `"triangle"` │ `"sawtoothup"` │ `"sawtoothdown"` │ `"ramp"` │ `"leftright"` | `"sine"` |
| `length` (or `duration`) | Effect length in ms | `1000` |
| `delay` | Delay before start, ms | `0` |
| `magnitude` (or `level`, or `start`) | Strength 0..1 (constant level / periodic peak / ramp start) | `1.0` |
| `finish` (or `end`) | Ramp end strength 0..1 | `0.0` |
| `period` | Period of one wave cycle, ms (periodic only) | `100` |
| `direction` | Polar direction in degrees (0 = front, 90 = right) | `0` |
| `attack_length` / `attack_level` | Envelope ramp-in length (ms) and peak strength (0..1) | `0` / `0` |
| `fade_length` / `fade_level` | Envelope ramp-out length (ms) and end strength (0..1) | `0` / `0` |
| `large` / `small` | Left/right magnitudes 0..1 (`leftright` only) | `0.5` / `0.5` |

```lua
-- Capability detection (always do this before using effects on Web/mobile)
if IsHapticEffectTypeSupported("sine") then
    local id = PlayHapticEffect{
        type = "sine",
        magnitude = 0.7,
        period = 80,
        length = 1500,
        direction = 0,
        attack_length = 100, attack_level = 0.0,
        fade_length = 200,   fade_level = 0.0,
    }
    -- id can later be reused / stopped / destroyed
end

-- One-shot constant force kick
PlayHapticEffect{ type = "constant", level = 0.9, length = 120, direction = 90 }

-- Rising ramp (engine spool-up)
PlayHapticEffect{ type = "ramp", start = 0.0, finish = 1.0, length = 800 }

-- Console-style dual-motor (Xbox / DualSense)
PlayHapticEffect{ type = "leftright", large = 0.8, small = 0.3, length = 250 }

-- Manual lifecycle: create once, run many times, update on the fly
local sineId = CreateHapticEffect{ type = "sine", magnitude = 0.4, period = 60, length = 9999999 }
RunHapticEffect(sineId, 1)              -- 1 iteration; pass 0 for infinite
-- ...later, change parameters without restarting from scratch:
UpdateHapticEffect(sineId, { type = "sine", magnitude = 0.9, period = 30, length = 9999999 })
if IsHapticEffectPlaying(sineId) then ... end
StopHapticEffect(sineId)
DestroyHapticEffect(sineId)             -- free the slot on the device

-- Stop everything (useful on pause / scene change)
StopAllHapticEffects()

-- Query
local feats = GetHapticFeatures()  -- { constant=true, sine=true, ramp=false, leftright=true, ... }
local slots = GetHapticMaxEffects()         -- e.g. 32  (how many effects can be uploaded)
local slotsPlaying = GetHapticMaxEffectsPlaying()  -- e.g. 16  (how many can play at once)
```

| Function | Description |
|---|---|
| `PlayHapticEffect(cfg, iterations?)` | Create + run an effect. Returns `effectId` (>=0) or `-1`. `iterations` default `1`, `0` = infinite |
| `CreateHapticEffect(cfg)` | Upload effect to device. Returns `effectId` (>=0) or `-1`. Does **not** start it |
| `RunHapticEffect(id, iterations?)` | Trigger an existing effect (`0` = infinite) |
| `UpdateHapticEffect(id, cfg)` | Change parameters of a running/uploaded effect in place |
| `StopHapticEffect(id)` | Stop one effect immediately |
| `DestroyHapticEffect(id)` | Free the device slot occupied by `id` |
| `StopAllHapticEffects()` | Stop every running effect on the device |
| `IsHapticEffectPlaying(id)` | Is the effect currently running |
| `IsHapticEffectTypeSupported(typeName)` | Does the device advertise support for this effect type |
| `GetHapticFeatures()` | Table of booleans: `constant, sine, triangle, sawtoothup, sawtoothdown, ramp, leftright, spring, damper, inertia, friction, custom` |
| `GetHapticMaxEffects()` | Total upload slots available on the device |
| `GetHapticMaxEffectsPlaying()` | Max effects that can play simultaneously |

> **Platform notes:**
> • Force-feedback wheels (Logitech G29/G923, Thrustmaster T-series, Fanatec) usually support all effect types incl. `constant`, `sine`, `ramp`, `friction`, `damper`.
> • Xbox / DualSense gamepads typically expose only `leftright` (and trigger rumble — see `SetGamepadTriggerRumble`).
> • Mobile devices and Web have **no** SDL haptic effect support — always check `IsHapticEffectTypeSupported(...)` first; for phone vibration use `PlayHaptic(...)`.

### Device sensors (phone accelerometer / gyroscope)

```lua
-- Check if the device has a built-in sensor (phone/tablet)
if IsDeviceSensorAvailable("accel") then
    SetDeviceSensorEnabled("accel", true)          -- must enable before reading
    local accel = GetDeviceSensorData("accel")      -- → {x, y, z} m/s²
    local enabled = IsDeviceSensorEnabled("accel")
end

if IsDeviceSensorAvailable("gyro") then
    SetDeviceSensorEnabled("gyro", true)
    local gyro = GetDeviceSensorData("gyro")        -- → {x, y, z} rad/s
end

-- Disable when no longer needed
SetDeviceSensorEnabled("accel", false)
SetDeviceSensorEnabled("gyro", false)
```

| Function | Description |
|---|---|
| `IsDeviceSensorAvailable(type)` | Is the sensor physically present on the device |
| `SetDeviceSensorEnabled(type, bool)` | Open/close the sensor (must enable before reading) |
| `IsDeviceSensorEnabled(type)` | Is the sensor currently open |
| `GetDeviceSensorData(type)` | `{x, y, z}` — sensor data (accel: m/s², gyro: rad/s) |

**Sensor types:**

| String | Meaning | Units | Platforms |
|---|---|---|---|
| `"accel"` / `"accelerometer"` | Accelerometer | m/s² (`{x,y,z}`) | All (SDL3) |
| `"gyro"` / `"gyroscope"` | Gyroscope | rad/s (`{x,y,z}`) | All (SDL3) |
| `"mag"` / `"magnetometer"` | Magnetic field | µT (`{x,y,z}`) | Android (JNI), iOS (CoreMotion) |
| `"compass"` / `"heading"` | Synthetic — enables the fused heading source for `GetCompassHeading()` | — | Android (JNI), iOS (CoreMotion) |
| `"baro"` / `"barometer"` / `"pressure"` | Atmospheric pressure | hPa (`x` field) | Android (JNI), iOS (CMAltimeter) |
| `"gravity"` | Gravity vector | m/s² (`{x,y,z}`) | Android (JNI), iOS (CoreMotion) |
| `"linearaccel"` | Linear acceleration (gravity removed) | m/s² (`{x,y,z}`) | Android (JNI), iOS (CoreMotion) |
| `"rotationvector"` | Fused rotation vector | unit quaternion components | Android (JNI), iOS (CoreMotion) |
| `"proximity"` | Proximity sensor | cm (`x`) — iOS reports `0` (near) or `5` (far) | Android (JNI), iOS (UIDevice) |
| `"light"` | Ambient light | lux (`x`) | **Android only** |
| `"stepcounter"` / `"step"` | Step count | count (`x`) | Android (JNI), iOS (CMPedometer) — see the counter-origin note below |
| `"ambienttemperature"` / `"temperature"` | Ambient temperature | °C (`x`) | **Android only** |
| `"humidity"` | Relative humidity | % (`x`) | **Android only** |

> **Note:** These are the **device's built-in** sensors (phone/tablet hardware), not gamepad sensors.
> For gamepad sensors (DualSense, Joy-Con), use `GamepadHasSensor()` / `GetGamepadSensorData()` instead.
>
> Sensor names are matched case-insensitively on every platform.
>
> **Step counter — origin and permissions.** The two platforms count from different origins, so treat the value as a **monotonic counter and use deltas**, never as an absolute number that means the same thing everywhere:
> • **Android** — `TYPE_STEP_COUNTER`, counting since the device booted. Needs the `ACTIVITY_RECOGNITION` runtime permission on Android 10+. `IsDeviceSensorAvailable("stepcounter")` reports the *hardware*, so it can return `true` while the counter stays at `0` until the permission is granted — request it with `Permissions.Request(Permissions.ACTIVITY_RECOGNITION)` and add it to the build's extra permissions.
> • **iOS** — `CMPedometer`, counting from midnight of the current day. `SetDeviceSensorEnabled("stepcounter", true)` seeds the value with a one-shot query for the steps already taken today and then keeps it live, so the first read is correct rather than `0`. The first enable triggers the system "Motion & Fitness" prompt; the engine auto-declares `NSMotionUsageDescription` in Info.plist (override the wording with `NSMotionUsageDescription=<reason>` in Extra Usage Descriptions). Unlike Android, `IsDeviceSensorAvailable("stepcounter")` returns `false` once the user has denied or restricted Motion & Fitness, so a denied permission is visible to the game.
>
> **Web caveats:** SDL registers accelerometer and gyroscope on *every* browser, so `IsDeviceSensorAvailable("accel")` returns `true` even on a desktop browser with no motion hardware — the readings simply stay `0`. On iOS/iPadOS Safari the browser also requires an explicit motion permission: `SetDeviceSensorEnabled("accel", true)` asks for it right away and, if that call was not inside a user gesture, re-asks once on the next tap or click, so the values start arriving after the player touches the page.

### Compass and Barometer (Android / iOS)

Accelerometer and gyroscope work everywhere, but **magnetometer** (compass) and **barometer** (atmospheric pressure / altitude) exist only on Android and iOS, where the engine reaches them through a native bridge. On desktop and Web these calls return `0` / `false`.

```lua
-- Compass (heading in degrees, 0 = magnetic north, 90 = east)
if IsDeviceSensorAvailable("compass") then
    SetDeviceSensorEnabled("compass", true)   -- enables accel + magnetometer internally
    local heading = GetCompassHeading()        -- 0..360
end

-- Raw magnetic field if you need the vector
if IsDeviceSensorAvailable("magnetometer") then
    SetDeviceSensorEnabled("magnetometer", true)
    local mag = GetDeviceSensorData("magnetometer")  -- {x, y, z} µT
end

-- Barometer
if IsDeviceSensorAvailable("barometer") then
    SetDeviceSensorEnabled("barometer", true)
    local p   = GetDeviceSensorData("pressure").x  -- hPa
    local alt = GetBarometricAltitude()             -- meters above sea level (std atm)
end

-- Ambient light + proximity (mobile UX adjustments)
SetDeviceSensorEnabled("light", true)
local lux = GetDeviceSensorData("light").x
SetDeviceSensorEnabled("proximity", true)
local nearCm = GetDeviceSensorData("proximity").x
```

| Function | Description |
|---|---|
| `GetCompassHeading()` | Magnetic heading in degrees (0..360, 0 = north). Requires `"compass"` enabled. **Android / iOS**. Returns `0` elsewhere |
| `GetBarometricAltitude()` | Altitude in meters derived from current pressure against the standard atmosphere (1013.25 hPa). Requires `"barometer"` enabled. **Android / iOS**. Returns `0` elsewhere |

> Always disable sensors you no longer need — they drain battery.
> **Desktop / Web:** SDL3 does not expose magnetometer or barometer there; these calls return `0` and `false` so it is safe to call them everywhere without `#ifdef`.

### Text input

```lua
StartTextInput()               -- Enable text input (opens keyboard on mobile)
local text = GetTextInput()    -- Get entered text
ClearTextInput()               -- Clear buffer
StopTextInput()                -- Disable input
local active = IsTextInputActive()
```

### Screen keyboard (mobile IME)

On Android / iOS, `StartTextInput()` brings up the soft keyboard. Use these helpers to know whether a soft keyboard exists at all and whether it is currently visible — useful for resizing UI to avoid being covered.

```lua
if HasScreenKeyboardSupport() then
    StartTextInput()
end

if IsScreenKeyboardShown() then
    -- Push UI up so the on-screen keyboard does not cover the input field
end
```

| Function | Description |
|---|---|
| `HasScreenKeyboardSupport()` | The platform has a software keyboard (Android/iOS true; desktop usually false) |
| `IsScreenKeyboardShown()` | The soft keyboard is currently visible on the focused window |

### Joystick (raw HID — wheels, HOTAS, arcade sticks, flight controllers)

Unlike `Gamepad.*`, the `Joystick` table exposes raw HID devices that SDL3 does **not** map to the standard gamepad layout — racing wheels, throttle quadrants, arcade stick boards, dance pads, generic USB controllers. Up to 8 such devices can be connected at once. Devices already classified as gamepads are filtered out (you only get them via the Gamepad API, no double-counting).

```lua
if Joystick.IsConnected() then
    Print("Wheel/HOTAS: " .. Joystick.GetName() .. " (GUID " .. Joystick.GetGUID() .. ")")
    Print("Buttons: " .. Joystick.GetButtonCount() .. ", axes: " .. Joystick.GetAxisCount())
end

local n = Joystick.GetCount()                       -- how many raw joysticks

-- Buttons (0-based)
if Joystick.IsButtonPressed(0) then ... end
if Joystick.IsButtonJustPressed(3) then Fire() end
if Joystick.IsButtonJustReleased(3) then Reload() end

-- Axes — normalized -1..1 with engine deadzone
local steer    = Joystick.GetAxis(0)
local throttle = Joystick.GetAxis(1)
local brake    = Joystick.GetAxis(2)

-- Hat (POV) — 8-way string
local hat = Joystick.GetHat(0)   -- "centered" | "up" | "down" | "left" | "right" | "leftup" | "rightup" | "leftdown" | "rightdown"

-- Trackball delta (rare — old joysticks)
local ball = Joystick.GetBall(0)  -- → {x, y}

-- Force feedback (only if hardware reports support)
Joystick.Rumble(0.7, 0.7, 250)    -- low, high, durationMs
Joystick.SetLED(255, 0, 0)         -- some sticks have RGB

-- Second device
if Joystick.IsConnected(1) then
    local pedalsBrake = Joystick.GetAxis(0, 1)
end
```

| Function | Description |
|---|---|
| `Joystick.IsConnected(idx?)` | Slot occupied (`idx` 0..7, default 0) |
| `Joystick.GetCount()` | Number of connected raw joysticks |
| `Joystick.GetName(idx?)` | Human-readable device name |
| `Joystick.GetGUID(idx?)` | Stable hex GUID — use for per-device config |
| `Joystick.GetButtonCount(idx?)` | Number of buttons exposed |
| `Joystick.GetAxisCount(idx?)` | Number of analog axes |
| `Joystick.GetHatCount(idx?)` | Number of hats (POV) |
| `Joystick.GetBallCount(idx?)` | Number of trackballs |
| `Joystick.IsButtonPressed(b, idx?)` | Button currently held |
| `Joystick.IsButtonJustPressed(b, idx?)` | Button pressed this frame |
| `Joystick.IsButtonJustReleased(b, idx?)` | Button released this frame |
| `Joystick.GetAxis(a, idx?)` | Axis value -1..+1 (deadzone applied) |
| `Joystick.GetHat(h, idx?)` | Hat direction string |
| `Joystick.GetBall(b, idx?)` | `{x, y}` trackball delta |
| `Joystick.Rumble(low, high, durationMs, idx?)` | Force feedback 0..1 |
| `Joystick.SetLED(r, g, b, idx?)` | RGB LED 0..255 |

### Pen / Stylus (Wacom, Apple Pencil, S-Pen, Surface Pen)

Full support for pressure-sensitive pens: tip pressure, X/Y tilt, distance from surface, barrel rotation, eraser end and side buttons. Works on Windows (Wintab/Pointer Events), macOS, Linux, Android (S-Pen), iPadOS (Apple Pencil) and Web (PointerEvent `pointerType="pen"`).

```lua
if Pen.IsActive() then
    local pos = Pen.GetPosition()           -- → {x, y} in pixels
    local p   = Pen.GetPressure()           -- 0..1
    local tilt = Pen.GetTilt()              -- → {x, y} degrees, -90..+90
    local rot  = Pen.GetRotation()          -- degrees (barrel rotation)
    local dist = Pen.GetDistance()          -- proximity 0..1 (1 = touching)

    if Pen.IsEraser() then
        EraseAt(pos.x, pos.y, p)
    elseif Pen.IsDown() then
        DrawAt(pos.x, pos.y, p)
    end
end

if Pen.IsJustPressed()  then BeginStroke() end
if Pen.IsJustReleased() then EndStroke() end

-- Side buttons (1-based, usually 1 = primary, 2 = eraser-toggle)
if Pen.IsButtonJustPressed(1) then OpenColorPicker() end
if Pen.IsButtonPressed(2)     then PanCanvas() end
```

| Function | Description |
|---|---|
| `Pen.IsSupported()` | Engine knows about pen events on this build |
| `Pen.IsActive()` | Pen is currently in proximity (hovering or touching) |
| `Pen.IsDown()` | Tip is touching the surface |
| `Pen.IsJustPressed()` / `IsJustReleased()` | Edge events for this frame |
| `Pen.GetPosition()` | `{x, y}` in pixels |
| `Pen.GetPressure()` | 0..1 |
| `Pen.GetTilt()` | `{x, y}` degrees |
| `Pen.GetDistance()` | 0..1 (1 = touching) |
| `Pen.GetRotation()` | barrel rotation in degrees |
| `Pen.IsEraser()` | The eraser end is being used |
| `Pen.IsButtonPressed(n)` | Side button `n` (1..32) currently held |
| `Pen.IsButtonJustPressed(n)` / `IsButtonJustReleased(n)` | Edge events for side button `n` |

### Drag & Drop (files / text dropped onto the window)

The runtime captures files and text dropped onto the engine window from the OS. Useful for in-game level editors, asset import flows, importing custom skins/maps, etc. Each frame `Update()` clears the lists, so consume them the same frame the drop happens.

```lua
function OnUpdate(dt)
    if DragDrop.HasItems() then
        local pos = DragDrop.GetPosition()
        for _, path in ipairs(DragDrop.GetFiles()) do
            ImportFile(path, pos.x, pos.y)
        end
        for _, txt in ipairs(DragDrop.GetTexts()) do
            PasteIntoNote(txt, pos.x, pos.y)
        end
        DragDrop.Clear()  -- optional, lists are cleared next frame anyway
    end
end
```

| Function | Description |
|---|---|
| `DragDrop.HasItems()` | At least one file or text was dropped this frame |
| `DragDrop.GetFiles()` | Array of absolute file paths |
| `DragDrop.GetTexts()` | Array of text strings dropped from another app |
| `DragDrop.GetPosition()` | `{x, y}` window-relative drop position |
| `DragDrop.Clear()` | Manually empty the buffers |

### Clipboard

Text clipboard works on Windows / macOS / Linux / Android / Web (Web requires a user-gesture for `SetText`). The primary selection (`GetPrimarySelection` / `SetPrimarySelection`) is the X11 middle-click clipboard — only meaningful on Linux.

```lua
if Clipboard.HasText() then
    local s = Clipboard.GetText()
end

Clipboard.SetText("Saved string from the game")

-- Linux/X11 only — fallback gracefully on other platforms
local sel = Clipboard.GetPrimarySelection()
Clipboard.SetPrimarySelection("primary")
```

| Function | Description |
|---|---|
| `Clipboard.GetText()` | Current clipboard string (`""` if empty) |
| `Clipboard.SetText(text)` | Replace clipboard contents (Web requires user gesture) |
| `Clipboard.HasText()` | Clipboard contains text |
| `Clipboard.GetPrimarySelection()` | X11 primary selection (Linux); empty elsewhere |
| `Clipboard.SetPrimarySelection(text)` | Set X11 primary selection |

### Webcam / Camera

Low-overhead access to system cameras via SDL3. Works on Windows, macOS, Linux, Android, iOS and Web (Web requires HTTPS and user permission prompt — first frames may be unavailable for 100–300 ms). Camera subsystem is initialised lazily on first call.

```lua
-- Enumerate
for _, dev in ipairs(Webcam.GetDevices()) do
    Print(dev.id .. ": " .. dev.name)
end

-- Open default camera (or specific device id)
if Webcam.Open() then
    local fmt = Webcam.GetFormat()        -- → {width, height}
    Print("Camera ready: " .. fmt.width .. "x" .. fmt.height)
end

-- Per-frame check
function OnUpdate(dt)
    if Webcam.IsOpen() and Webcam.HasNewFrame() then
        Webcam.SaveFrameToPNG("saves/photo.png")
    end
end

Webcam.Close()
```

| Function | Description |
|---|---|
| `Webcam.GetDevices()` | Array of `{id, name}` |
| `Webcam.Open(deviceId?)` | Opens device by id; without argument opens the first available. Returns `true` on success |
| `Webcam.Close()` | Closes the active camera |
| `Webcam.IsOpen()` | A camera is currently open |
| `Webcam.GetFormat()` | `{width, height}` of the negotiated stream |
| `Webcam.HasNewFrame()` | A fresh frame is available this poll |
| `Webcam.SaveFrameToPNG(path)` | Acquires the latest frame and writes it as PNG. Returns `true` on success |

> **Web note:** browsers gate camera access behind a permission prompt; until the user accepts it, `IsOpen()` may be true but `HasNewFrame()` returns false for several frames.

### Input buffer (frame history, motion inputs)

A ring buffer of past input frames, kept in C++. You decide what a "frame of input" means by packing your own bitmask,
which keeps the whole thing deterministic and rollback-safe: the buffer never reads live devices, it only stores what
you hand it.

This is what makes fighting-game inputs practical — quarter-circles, charge moves, double taps, leniency windows and
input buffering during recovery frames — and it is just as useful for a platformer's jump buffer and coyote time.

```lua
InputBuffer.SetEnabled(true)         -- off by default; enabling clears the history
InputBuffer.IsEnabled()
InputBuffer.SetCapacity(120)         -- frames kept (2..4096, default 120 = 2 s at 60 Hz)
InputBuffer.GetCapacity()

-- Record exactly one frame. Call it from OnFixedUpdate for a fixed-step game.
InputBuffer.Push(bits)

InputBuffer.Get(framesAgo)           -- bitmask N frames back (0 = the newest, default 0)
InputBuffer.GetCount()               -- frames currently stored
InputBuffer.GetFrame()               -- how many frames have ever been pushed
InputBuffer.Clear()
```

Queries, all in frames:

| Call | Meaning |
| ---- | ------- |
| `InputBuffer.IsHeld(mask, frames)` | every one of the last `frames` frames had all of `mask` set |
| `InputBuffer.WasPressed(mask, withinFrames)` | `mask` went from not-set to set inside the window — a **jump buffer** |
| `InputBuffer.WasReleased(mask, withinFrames)` | `mask` went from set to not-set inside the window |
| `InputBuffer.MatchSequence(steps, windowFrames)` | the listed masks occurred **in order** inside the window |

`MatchSequence` scans oldest to newest and matches greedily, so intermediate frames and held inputs do not break a
motion. A step matches when every bit it asks for is present (`bits & step == step`), which means diagonals fall out
naturally: `DOWN | RIGHT` matches a frame where the player is holding down-right.

```lua
local LEFT, RIGHT, DOWN, UP = 1, 2, 4, 8
local PUNCH, KICK = 16, 32

function OnFixedUpdate(dt)
    local bits = 0
    if IsKeyPressed("a")     then bits = bits | LEFT  end
    if IsKeyPressed("d")     then bits = bits | RIGHT end
    if IsKeyPressed("s")     then bits = bits | DOWN  end
    if IsKeyPressed("w")     then bits = bits | UP    end
    if IsKeyPressed("j")     then bits = bits | PUNCH end
    InputBuffer.Push(bits)

    -- Quarter-circle forward + punch, within the last 16 frames
    if InputBuffer.MatchSequence({ DOWN, DOWN | RIGHT, RIGHT, PUNCH }, 16) then
        DoFireball()
    end

    -- Charge move: hold back for 40 frames, then forward + kick
    if InputBuffer.IsHeld(LEFT, 40) and InputBuffer.WasPressed(RIGHT | KICK, 6) then
        DoSonicBoom()
    end

    -- Jump buffer: the press may have landed up to 6 frames before touching the ground
    if IsGrounded() and InputBuffer.WasPressed(UP, 6) then
        Jump()
    end
end
```

> **Rollback.** The buffer holds only what you pushed, so it replays identically. If you roll back, push the same inputs
> again for the re-simulated frames and every query answers the same way. `InputBuffer.Clear()` on match start.

### Action System (action bindings)

```lua
-- Bind actions to buttons
BindAction("Jump", "space")
BindAction("Jump", "gamepad_a")
BindAction("Jump", "touch_0")
BindAction("Shoot", "mouse_1")

-- Multiplayer: bind to specific gamepad (0-3)
BindAction("P2_Jump", "gamepad_1_a")     -- second gamepad, button A
BindAction("P2_Attack", "gamepad_1_x")   -- second gamepad, button X
BindAction("P3_Jump", "gamepad_2_a")     -- third gamepad

-- Check actions (any bound input)
if IsActionPressed("Jump") then Jump() end
if IsActionJustPressed("Shoot") then Fire() end
if IsActionJustReleased("Shoot") then StopFire() end

-- Remove bindings
UnbindAction("Jump")
ClearAllActions()

-- Convenient universal checks
if IsConfirmPressed() then ... end   -- Space/Enter/Gamepad A/Touch
if IsCancelPressed() then ... end    -- Escape/Gamepad B
```

**Action binding format:**

| Format | Example | Description |
|---|---|---|
| `"key"` | `"space"`, `"w"`, `"f1"` | Keyboard key |
| `"mouse_N"` | `"mouse_1"`, `"mouse_3"` | Mouse button (1-5) |
| `"gamepad_button"` | `"gamepad_a"`, `"gamepad_lb"` | Gamepad 0, button |
| `"gamepad_N_button"` | `"gamepad_1_a"`, `"gamepad_2_x"` | Gamepad N (0-3), button |
| `"touch"` / `"touch_N"` | `"touch_0"`, `"touch_1"` | Finger N |

### Axis Actions (analog 1D)

Map analog inputs to a single float value (e.g., throttle, steering).

```lua
-- Gamepad axis sources
BindAxisAction("Throttle", "gamepad_triggerright")                 -- right trigger
BindAxisAction("Steer", "gamepad_leftx", { deadzone = 0.15 })     -- left stick X
BindAxisAction("Look", "gamepad_1_rightx")                        -- gamepad 1, right X
BindAxisAction("Zoom", "scroll_y")                                -- mouse scroll

-- Keyboard pair (negative + positive keys → -1 / 0 / +1)
BindAxisActionKeys("MoveX", "a", "d")
BindAxisActionKeys("MoveX", "left", "right", { scale = 0.5 })

-- Mouse delta sources
BindAxisAction("LookX", "mouse_deltax")
BindAxisAction("LookY", "mouse_deltay", { scale = -1.0 })

-- Read value
local val = GetActionValue("Throttle")   -- float (with deadzone + scale)

-- Remove
UnbindAxisAction("Throttle")
```

**Axis source formats:**

| Format | Example | Description |
|---|---|---|
| `"gamepad_AXIS"` | `"gamepad_leftx"`, `"gamepad_triggerleft"` | Gamepad 0 axis |
| `"gamepad_N_AXIS"` | `"gamepad_1_righty"` | Gamepad N axis |
| `"mouse_deltax"` | — | Mouse delta X |
| `"mouse_deltay"` | — | Mouse delta Y |
| `"scroll_x"` | — | Scroll wheel X |
| `"scroll_y"` | — | Scroll wheel Y |

**Axis names:** `leftx`, `lefty`, `rightx`, `righty`, `triggerleft` (`lt`, `l2`), `triggerright` (`rt`, `r2`)

**Options table (optional):** `{ scale = 1.0, deadzone = 0.0 }`

### 2D Axis Actions (analog 2D)

Map 2D inputs to an `{x, y}` table (e.g., movement, camera look).

```lua
-- Gamepad stick
BindAxis2DAction("Move", "gamepad_leftstick", { deadzone = 0.15 })
BindAxis2DAction("Look", "gamepad_rightstick")
BindAxis2DAction("P2Move", "gamepad_1_leftstick")   -- second gamepad

-- Mouse delta (for FPS camera)
BindAxis2DAction("MouseLook", "mouse_delta", { scale = 0.1 })

-- Keyboard quad (left, right, down, up)
BindAxis2DActionKeys("Move", "a", "d", "s", "w")

-- Read value
local move = GetActionAxis2D("Move")   -- → { x = ..., y = ... }
entity.x = entity.x + move.x * speed * dt
entity.y = entity.y + move.y * speed * dt

-- Remove
UnbindAxis2DAction("Move")
```

**2D source formats:**

| Format | Example | Description |
|---|---|---|
| `"gamepad_leftstick"` | — | Gamepad 0 left stick |
| `"gamepad_rightstick"` | — | Gamepad 0 right stick |
| `"gamepad_N_leftstick"` | `"gamepad_1_leftstick"` | Gamepad N left stick |
| `"mouse_delta"` | — | Mouse movement delta |

### Input Contexts

Group actions into contexts that can be enabled/disabled (e.g., gameplay vs menu).

```lua
-- Create contexts
CreateInputContext("Gameplay")        -- enabled by default
CreateInputContext("Menu", false)     -- created disabled

-- Assign actions to contexts
SetActionContext("Jump", "Gameplay")
SetActionContext("Shoot", "Gameplay")
SetActionContext("MenuSelect", "Menu")

-- Switch contexts
DisableInputContext("Gameplay")
EnableInputContext("Menu")

-- Check
if IsInputContextEnabled("Gameplay") then ... end

-- Remove context (also unlinks all its actions)
RemoveInputContext("Menu")
```

> Actions without a context are always active.

### Triggers (Hold / Tap / DoubleTap / Pulse)

Add trigger conditions to existing boolean actions.

```lua
-- Hold: triggers after holding for 0.5s
BindAction("Interact", "e")
SetActionTrigger("Interact", "Hold", 0.5)

-- Tap: triggers on quick press-and-release (< 0.3s)
BindAction("Dash", "shift")
SetActionTrigger("Dash", "Tap", 0.3)

-- Double-tap: two quick taps within 0.4s
BindAction("Roll", "space")
SetActionTrigger("Roll", "DoubleTap", 0.4)

-- Pulse: fires immediately on press, then repeats every 0.2s while held
BindAction("Fire", "mouse_left")
SetActionTrigger("Fire", "Pulse", 0.2)

-- IMPORTANT: call every frame with delta time!
UpdateActionTriggers(dt)

-- Check trigger result
if IsActionTriggered("Interact") then OpenDoor() end
if IsActionTriggered("Dash") then DoDash() end
if IsActionTriggered("Fire") then ShootBullet() end

-- Hold progress (0.0 → 1.0)
local progress = GetActionHoldProgress("Interact")
DrawProgressBar(progress)

-- Remove trigger (action still works normally)
ClearActionTrigger("Interact")
```

### Rebinding (for settings menu)

Detect any input for keybinding UI.

```lua
-- Call every frame during rebind mode
local binding = DetectInputBinding()
if binding then
    -- binding = "w", "space", "mouse_1", "gamepad_a", "touch_0", etc.
    UnbindAction("Jump")
    BindAction("Jump", binding)
end
```

### Query & Export/Import (persistence)

The binding system covers **all** action types — digital actions (`BindAction`), 1D axes
(`BindAxisAction` / `BindAxisActionKeys`) and 2D axes (`BindAxis2DAction` /
`BindAxis2DActionKeys`). Save/Load preserves all three categories together with their
`scale` and `deadzone` options.

```lua
-- Get all bindings for an action (digital actions only)
local bindings = GetActionBindings("Jump")    -- → {"space", "gamepad_a"}

-- Get all registered action names (digital + 1D + 2D)
local names = GetAllActionNames()             -- → {"Jump", "Shoot", "Move", ...}

-- Find which action already uses a given binding (for conflict detection in UI)
local owner = GetActionByBinding("space")     -- → "Jump" or nil
if owner and owner ~= "NewAction" then
    UnbindAction(owner)                       -- resolve the conflict
end

-- Built-in JSON persistence (recommended)
SaveInputBindings()                           -- → InputBindings.json in user config
SaveInputBindings("profiles/wasd.json")       -- custom path / multi-profile
LoadInputBindings()                           -- returns true on success, false otherwise
LoadInputBindings("profiles/wasd.json")
local path = GetInputBindingsPath()           -- absolute path to default file

-- Manual export/import (for embedding bindings in your own save files)
local data = ExportInputBindings()
-- data = {
--   actions = { Jump = { "space", "gamepad_a" }, ... },
--   axes    = { Throttle = { { type="source", source="gamepad_triggerright",
--                              scale=1.0, deadzone=0.0 } }, ... },
--   axes2d  = { Move = { { type="keys", left="a", right="d",
--                          down="s", up="w", scale=1.0, deadzone=0.0 } }, ... }
-- }
ImportInputBindings(data)                     -- replaces current bindings
```

**Typical settings menu flow**

```lua
function OnCreate()
    BindAction("Jump", "space")
    BindAction("Jump", "gamepad_a")
    BindAxis2DActionKeys("Move", "a", "d", "s", "w")
    BindAxis2DAction("Move", "gamepad_leftstick")
    BindAxisAction("Throttle", "gamepad_triggerright")
    LoadInputBindings()
end

function RebindAction(actionName)
    local newBinding = DetectInputBinding()
    if not newBinding then return end
    local conflict = GetActionByBinding(newBinding)
    if conflict and conflict ~= actionName then
        UnbindAction(conflict)
    end
    UnbindAction(actionName)
    BindAction(actionName, newBinding)
    SaveInputBindings()
end
```

### Virtual controls (mobile)

```lua
-- Virtual stick
CreateVirtualStick("move", 0.15, 0.7, 0.12)  -- name, centerX, centerY, radius
UpdateVirtualSticks()  -- Call every frame!

local axis = GetVirtualStickAxis("move")  -- → {x, y}
local knob = GetVirtualStickKnob("move")  -- → {x, y} knob position
local active = IsVirtualStickActive("move")
SetVirtualStickCenter("move", 0.15, 0.7)  -- Move center
RemoveVirtualStick("move")

-- Virtual button
CreateVirtualButton("jump", 0.8, 0.7, 0.1, 0.1)  -- name, x, y, w, h
UpdateVirtualButtons()  -- Call every frame!

if IsVirtualButtonPressed("jump") then ... end
if IsVirtualButtonJustPressed("jump") then ... end
if IsVirtualButtonJustReleased("jump") then ... end
SetVirtualButtonRect("jump", 0.8, 0.7, 0.12, 0.12)
RemoveVirtualButton("jump")

-- Remove all virtual controls
ClearVirtualControls()
```

### Gamepad Cursor (virtual cursor via gamepad stick)

```lua
-- Enable a virtual cursor controlled by a gamepad stick
EnableGamepadCursor()                              -- defaults: speed=400, rightStick, gamepad 0
EnableGamepadCursor(600)                           -- custom speed
EnableGamepadCursor(400, true, 0)                  -- speed, useRightStick, gamepadIndex

-- Disable the virtual cursor
DisableGamepadCursor()

-- Query state
local enabled = IsGamepadCursorEnabled()
local pos = GetGamepadCursorPosition()  -- → {x, y}

-- Set position / speed manually
SetGamepadCursorPosition(960, 540)
SetGamepadCursorSpeed(800)

-- Update every frame (moves cursor based on stick input)
UpdateGamepadCursor(dt)
```

### Universal Pointer (mouse / touch / gamepad cursor)

These functions automatically choose the active input source:
gamepad cursor → touch → mouse (in that priority order).

```lua
-- Screen-space position (pixels)
local ptr = GetPointerScreenPosition()
-- ptr.x, ptr.y   = screen coordinates
-- ptr.source      = "mouse" | "touch" | "gamepad"

-- World-space position (uses primary camera)
local wptr = GetPointerWorldPosition()
-- wptr.x, wptr.y  = world coordinates
-- wptr.source      = "mouse" | "touch" | "gamepad"
```

---

## 7. Entity — Entities

> **Type:** Entity-bound

### Finding entities

```lua
-- Find entity by tag (first one)
local enemy = FindEntityByTag("Enemy")

-- Find ALL entities with a tag → table of IDs
local enemies = FindEntitiesByTag("Enemy")
for _, id in ipairs(enemies) do
    -- handle each enemy
end

-- Get entity tag
local tag = GetEntityTag(entityId)

-- Closest entity by tag
local closest = GetClosestEntityByTag("Coin")

-- Entities in radius
local nearby = GetEntitiesInRadius(100, 200, 500)           -- x, y, radius
local coins  = GetEntitiesInRadius(100, 200, 500, "Coin")   -- with tag filter
local nearMe = GetEntitiesInMyRadius(300, "Enemy")           -- from current entity

-- Entities in a rectangle
local enemies = GetEntitiesInRect(0, 0, 500, 300, "Enemy")  -- x, y, w, h, filterTag?
for _, id in ipairs(enemies) do
    DestroyEntity(id)
end

-- All entities in the scene
local all = GetAllEntities()
local count = GetEntityCount()
local enemyCount = GetEntityCountByTag("Enemy")
```

### Iteration

```lua
-- Iterate all entities
ForEachEntity(function(id)
    local tag = GetEntityTag(id)
    -- return false to break
end)

-- Iterate entities with a tag
ForEachEntityByTag("Enemy", function(id)
    DestroyEntity(id)
end)
```

### Creation and deletion

```lua
-- Spawn entity from class
local id = SpawnEntity("Content/Classes/Bullet.ice_class", x, y)
local id = SpawnEntity("Content/Classes/Bullet.ice_class", x, y, z)  -- with Z

-- Spawn with initial velocity
local id = SpawnEntityWithVelocity("Content/Classes/Bullet.ice_class", x, y, vx, vy)

-- Create an empty entity
local id = CreateEmptyEntity()  -- tag="Entity", x=y=z=0
local id = CreateEmptyEntity("Player", 100, 200, 0)

-- Clone existing entity (with components and script)
local cloneId = CloneEntity(entityId, x, y)  -- New position
local cloneId = CloneEntity(entityId)         -- Original position

-- Destroy entity
DestroyEntity(entityId)

-- Destroy self
DestroySelf()

-- Check existence
if EntityExists(entityId) then ... end
```

### Properties

```lua
-- Get self ID
local myId = GetEntityId()

-- Tag
local tag = GetTag()
SetTag("Player")
SetEntityTag(entityId, "Dead")
local hasTag = EntityHasTag(entityId, "Enemy")

-- Enable/disable entity
SetEnabled(false)
SetEntityEnabled(entityId, false)
local enabled = IsEnabled()
local enabled = IsEntityEnabled(entityId)

-- Entity visibility (global flag — affects all components)
SetVisible(false)
SetEntityVisible(entityId, false)
local vis = IsVisible()
local vis = IsEntityVisible(entityId)

-- Render in game (global flag — if false, entity is fully disabled at runtime)
SetRenderInGame(false)
SetEntityRenderInGame(entityId, false)
local rig = GetRenderInGame()
local rig = GetEntityRenderInGame(entityId)
```

### Working with other entities

```lua
-- Other entity position
local pos = GetEntityPosition(entityId)  -- → {x, y, z}
SetEntityPosition(entityId, 100, 200)
SetEntityPosition(entityId, 100, 200, 0)  -- with Z

-- Other entity velocity
local vel = GetEntityVelocity(entityId)  -- → {x, y}
SetEntityVelocity(entityId, 200, 0)
AddEntityImpulse(entityId, 0, -500)

-- Flip other entity sprite
SetEntityFlipX(entityId, true)
SetEntityFlipY(entityId, true)

-- Distance to other entity
local dist = DistanceToEntity(targetId)

-- Direction to entity (normalized vector)
local dir = GetDirectionToEntity(targetId)  -- → {x, y}

-- Direction to point
local dir = GetDirectionTo(targetX, targetY)  -- → {x, y}

-- Quick range check (no exact distance)
local inRange = IsEntityInRange(targetId, 200)  -- Within 200?

-- Entities in view cone (from current entity)
local seen = GetEntitiesInCone(dirAngle, halfAngle, radius)
local seen = GetEntitiesInCone(dirAngle, halfAngle, radius, "Enemy")  -- Tag filter
```

### Entity Data (custom data)

```lua
-- Attach data to self
SetData("health", 100)
local hp = GetData("health")
local has = HasData("health")
ClearData()

-- Attach data to any entity
SetEntityData(entityId, "health", 100)
local hp = GetEntityData(entityId, "health")
local has = HasEntityData(entityId, "health")
ClearEntityData(entityId)
```

### Hierarchy (parent attachment)

Hierarchy is stored as an EnTT component (`HierarchyComponent`). It is created automatically
when you call `AttachToEntity` and destroyed automatically when the entity is destroyed
(children are detached, parent is notified). No manual cleanup required.

```lua
-- Attach to parent entity (follow it)
-- Stores full local transform (position, rotation, scale) on attach
AttachToEntity(parentId)

-- Detach
DetachFromParent()

-- Explicit (any entity, usable from a level script or to manage other entities):
AttachEntityToParent(childId, parentId)   -- attach an arbitrary child to a parent
AttachChildEntity(childId)                -- attach a child to the current (self) entity
DetachEntityFromParent(childId)           -- detach an arbitrary entity from its parent

-- Spawn a class instance already attached to a parent, positioned at a parent-local offset.
-- SpawnEntityAsChild(classPath, parentId [, localX, localY, z]) → new entity id, or nil
local turretId = SpawnEntityAsChild("Content/Classes/Turret.ice_class", GetEntityId(), 0, 24)

-- Get parent
local parentId = GetParentEntity()  -- nil if none

-- Get child entities
local children = GetChildEntities()  -- table of IDs

-- Checks
local hasParent = HasParent()
local childCount = GetChildCount()

-- Local offset (sets LocalPosition.x, LocalPosition.y on HierarchyComponent)
SetLocalOffset(10, 5)
local offset = GetLocalOffset()  -- → {x, y}

-- Force position update from parent (accounts for rotation and scale)
FollowParent()

-- Recursively update entire entity hierarchy including all descendants
FollowParentHierarchy()

-- Clear entire hierarchy (removes HierarchyComponent from all entities)
ClearHierarchy()

-- Check component presence
local has = HasHierarchy(entityId)
local has = HasComponent("Hierarchy")
local has = EntityHasComponent(entityId, "Hierarchy")
```

### Local and world transform (Entity)

When an entity is attached to a parent via `AttachToEntity`, separate functions
are available for **local** (relative to parent) and **world** (absolute) transform.
If there is no parent — local and world transform are identical.

```lua
-- Local position (relative to parent, or absolute if no parent)
local lp = GetLocalPosition()      -- → {x, y, z}
SetLocalPosition(10, 20)           -- Set (x, y)
SetLocalPosition(10, 20, 0.5)     -- Set (x, y, z)

-- Local rotation
local lr = GetLocalRotation()      -- → float (degrees)
SetLocalRotation(45)

-- Local scale
local ls = GetLocalScale()         -- → {x, y}
SetLocalScale(2, 2)

-- World position (absolute, from TransformComponent)
local wp = GetWorldPosition()      -- → {x, y, z}
SetWorldPosition(100, 200)         -- Set (x, y), recalculates local transform
SetWorldPosition(100, 200, 1.0)   -- Set (x, y, z)

-- World rotation
local wr = GetWorldRotation()      -- → float (degrees)
SetWorldRotation(90)               -- Recalculates local rotation

-- World scale
local ws = GetWorldScale()         -- → {x, y}
SetWorldScale(3, 3)                -- Recalculates local scale

-- Incremental offset (add to current value)
AddWorldOffset(10, 5)              -- Shift world position by (dx, dy), recalculates local transform
AddWorldOffset(10, 5, 1.0)         -- Shift by (dx, dy, dz)
AddLocalOffset(3, -2)              -- Shift local position by (dx, dy)
AddLocalOffset(3, -2, 0.5)         -- Shift by (dx, dy, dz)

-- Incremental rotation
AddWorldRotation(15)               -- Add to world rotation (degrees), recalculates local
AddLocalRotation(15)               -- Add to local rotation (degrees)

-- Incremental scale
AddWorldScale(0.5, 0.5)            -- Add to world scale (dx, dy), recalculates local
AddLocalScale(0.5, 0.5)            -- Add to local scale (dx, dy)
```

**Example: Attaching a weapon to a character:**

```lua
function OnCreate()
    local weaponId = SpawnEntity("Content/Classes/Sword.ice_class", 0, 0)
    AttachToEntity(GetEntityId())  -- Attach to current entity
    SetLocalPosition(20, -10)      -- Weapon to the right and slightly below
    SetLocalRotation(0)
end

function OnUpdate(dt)
    FollowParent()  -- Automatically follows accounting for rotation and scale
end
```

### Class inheritance

```lua
-- Entity class name
local className = GetEntityClassName(entityId)  -- "Player"

-- Class name/path for current entity
local myClass = GetClassName()
local myPath = GetClassPath()
local parentName = GetParentClassName()
local hasParent = HasParentClass()

-- Check if entity is an instance of a class (with inheritance)
if EntityIsA(entityId, "Character") then
    -- Entity is Character or inherits from Character
end

-- Inheritance checks for current entity
if IsA("Character") then ... end
if IsChildOf("Character") then ... end

-- Get inheritance chain
local chain = GetInheritanceChain()  -- {"Player", "Character", "Entity"}
```

### Interfaces

Interfaces declare “contracts” between entities.
An entity declares that it implements an interface, and other entities can call its functions.
Interfaces are stored as an EnTT component (`InterfaceComponent`). The component is created
automatically when `ImplementInterface` is called and cleaned up when the entity is destroyed.

```lua
-- Declare that this entity implements an interface
ImplementInterface("Damageable")
ImplementInterfaces({"Damageable", "Interactable"})

-- Check own interfaces
local has = HasInterface("Damageable")       -- true
local list = GetInterfaces()                  -- {"Damageable", "Interactable"}

-- Check interface on another entity
local has = EntityHasInterface(entityId, "Damageable")

-- Call function via interface
CallInterface(entityId, "TakeDamage", 50)
-- Calls TakeDamage(50) in the entity's script

-- Call function on ALL entities with a tag that have InterfaceComponent
CallInterfaceOnTag("Enemy", "OnAlert")

-- Find all entities with an interface
local healables = FindEntitiesWithInterface("Healable")

-- Management
RemoveInterface("Damageable")
ClearInterfaces()                -- Remove all interfaces from current entity
ClearAllInterfaces()             -- Remove all interfaces from all entities

-- Check component presence
local has = HasInterfaceComponent(entityId)
local has = HasComponent("Interface")
local has = EntityHasComponent(entityId, "Interface")
```

### Gameplay Tags

Gameplay Tags is a hierarchical tagging system stored as an EnTT component (`GameplayTagComponent`).
Tags use dot-separated hierarchy (e.g. `"Status.Buff.Speed"`) — a query for `"Status.Buff"` will match `"Status.Buff.Speed"`.

```lua
-- Add / remove tags
AddGameplayTag("Status.Buff.Speed")
AddGameplayTags({"Status.Buff.Speed", "Team.Ally"})  -- batch add from table
RemoveGameplayTag("Status.Buff.Speed")
ClearGameplayTags()

-- Exact match (no hierarchy)
local has = HasExactGameplayTag("Status.Buff.Speed")

-- Hierarchical match ("Status.Buff" matches "Status.Buff.Speed")
local has = HasGameplayTag("Status.Buff")

-- Check multiple tags (from a table)
local any = HasAnyGameplayTag({"Status.Buff", "Status.Debuff"})
local all = HasAllGameplayTags({"Status.Buff.Speed", "Team.Ally"})

-- Get all tags
local tags = GetGameplayTags()  -- table of strings

-- Query other entities
local has = EntityHasGameplayTag(entityId, "Status.Buff")
local has = EntityHasExactGameplayTag(entityId, "Status.Buff.Speed")

-- Find entities by tag
local entities = FindEntitiesWithGameplayTag("Status.Buff")
local entities = FindEntitiesWithExactGameplayTag("Status.Buff.Speed")

-- Check component presence
local has = HasGameplayTagComponent(entityId)
local has = HasComponent("GameplayTag")
local has = EntityHasComponent(entityId, "GameplayTag")
```

### `GameplayTags` global module

The global `GameplayTags` table is a tag system built on top of `GameplayTagComponent`.
It adds a dynamic tag registry, hierarchy utilities, container helpers, a query builder
(`any` / `all` / `none`, exact or hierarchical), scene-wide search, and event/messaging hooks.

All per-entity mutations (both the per-entity `AddGameplayTag`/`RemoveGameplayTag`/`ClearGameplayTags`
calls and the `GameplayTags.*` ones) fire the listener and messaging callbacks.

```lua
-- ---------- Registry (known / declared tags) ----------
GameplayTags.Register("Status.Buff.Speed")            -- also registers "Status" and "Status.Buff"
GameplayTags.RegisterMany({"Team.Ally", "Team.Enemy"})-- returns count of newly-registered tags
local known = GameplayTags.IsRegistered("Status.Buff")
local all   = GameplayTags.GetAllRegistered()         -- { tag, tag, ... }
GameplayTags.Unregister("Team.Enemy")
GameplayTags.ClearRegistry()

-- ---------- Format / hierarchy utilities ----------
GameplayTags.IsValid("Status.Buff.Speed")   -- true  (allowed: [A-Za-z0-9_-] and dots)
GameplayTags.IsValid("..Bad")               -- false
GameplayTags.Split("A.B.C")                 -- {"A","B","C"}
GameplayTags.Join({"A","B","C"})            -- "A.B.C"
GameplayTags.GetParent("A.B.C")             -- "A.B"
GameplayTags.GetParents("A.B.C")            -- {"A.B","A"}       (bottom-up)
GameplayTags.GetDepth("A.B.C")              -- 3
GameplayTags.IsChildOf("A.B.C", "A.B")      -- true   (strict: "A.B" is NOT a child of itself)
GameplayTags.MatchesTag("A.B.C", "A.B")     -- true   (held satisfies query)
GameplayTags.GetChildren("Status")          -- direct registered children
GameplayTags.GetDescendants("Status")       -- every registered descendant

-- ---------- Container helpers (tag table of strings) ----------
local c = {"Status.Buff.Speed", "Team.Ally"}
GameplayTags.ContainerHasTag(c, "Status")          -- true  (hierarchical)
GameplayTags.ContainerHasExact(c, "Status")        -- false (exact)
GameplayTags.ContainerHasAny(c, {"Foo","Status"})  -- true
GameplayTags.ContainerHasAll(c, {"Team","Status"}) -- true
GameplayTags.ContainerHasAnyExact(c, {"Team.Ally"})-- true
GameplayTags.ContainerHasAllExact(c, {"Team.Ally","Status.Buff.Speed"}) -- true
GameplayTags.ContainerFilter(c, "Status")          -- {"Status.Buff.Speed"}
GameplayTags.ContainerMerge(c, {"Team.Ally","NewTag"}) -- deduped union

-- ---------- Per-entity ----------
GameplayTags.AddTag(entityId, "Status.Buff.Speed")            -- fires OnTagAdded
GameplayTags.AddTags(entityId, {"Status.Buff.Speed","Team.Ally"})
GameplayTags.RemoveTag(entityId, "Status.Buff.Speed")         -- fires OnTagRemoved
GameplayTags.RemoveTags(entityId, {"Status.Buff.Speed"})
GameplayTags.Clear(entityId)                                  -- fires OnTagRemoved for each

GameplayTags.HasTag(entityId, "Status")            -- hierarchical
GameplayTags.HasExactTag(entityId, "Status.Buff")  -- exact
GameplayTags.HasAny(entityId, {"A","B"})
GameplayTags.HasAll(entityId, {"A","B"})
GameplayTags.HasAnyExact(entityId, {"A","B"})
GameplayTags.HasAllExact(entityId, {"A","B"})
local tags  = GameplayTags.GetTags(entityId)       -- { tag, tag, ... }
local count = GameplayTags.CountTags(entityId)
local has   = GameplayTags.HasTagComponent(entityId)

-- ---------- Scene-wide search ----------
local e1 = GameplayTags.FindByTag("Status.Buff")              -- hierarchical
local e2 = GameplayTags.FindByExactTag("Status.Buff.Speed")
local e3 = GameplayTags.FindAnyOf({"Team.Ally","Team.Enemy"})
local e4 = GameplayTags.FindAllOf({"Status.Buff","Team.Ally"})
local n  = GameplayTags.CountEntitiesWith("Status.Buff")
local all= GameplayTags.GetAllTagsInScene()                   -- deduped

GameplayTags.AddTagToAll({id1, id2, id3}, "Team.Enemy")       -- returns count applied
GameplayTags.RemoveTagFromAll({id1, id2}, "Team.Enemy")

-- ---------- Tag queries (GameplayTagQuery-lite) ----------
local q = GameplayTags.MakeQuery({
    all   = { "Status.Buff" },                -- every tag must match at least one held
    any   = { "Team.Ally", "Team.Neutral" },  -- at least one must match
    none  = { "Status.Debuff" },              -- none may match
    exact = false,                            -- false = hierarchical (default)
})

GameplayTags.EvaluateQuery(q, {"Status.Buff.Speed","Team.Ally"})  -- true
q:Matches({"Status.Buff.Speed","Team.Ally"})                      -- true (sugar)
GameplayTags.EntityMatchesQuery(entityId, q)
local matches = GameplayTags.FindEntitiesMatchingQuery(q)         -- { entityId, ... }

-- ---------- Events (add / remove) ----------
-- Hierarchical listeners: subscribing to "Status" also fires for "Status.Buff.Speed".
local l1 = GameplayTags.OnTagAdded("Status", function(entityId, tag) end)
local l2 = GameplayTags.OnTagAddedExact("Status.Buff.Speed", function(eid, tag) end)
local l3 = GameplayTags.OnAnyTagAdded(function(eid, tag) end)
local l4 = GameplayTags.OnTagAddedForEntity(entityId, "Status", function(eid,tag) end)

local r1 = GameplayTags.OnTagRemoved("Status", function(eid, tag) end)
local r2 = GameplayTags.OnTagRemovedExact("Status", function(eid, tag) end)
local r3 = GameplayTags.OnAnyTagRemoved(function(eid, tag) end)
local r4 = GameplayTags.OnTagRemovedForEntity(entityId, "Status", function(eid,tag) end)

GameplayTags.RemoveListener(l1)
GameplayTags.RemoveAllListeners()                 -- also cleared automatically on runtime stop

-- ---------- Messaging (GameplayMessageSubsystem-lite) ----------
-- Broadcast() fires every Listen()/ListenExact() subscriber whose subscribed tag is
-- a parent of (or equal to, or exactly equal to) the broadcast tag.
local m1 = GameplayTags.Listen("Combat", function(tag, payload) end)
local m2 = GameplayTags.ListenExact("Combat.Damage.Fire", function(tag, payload) end)
GameplayTags.Broadcast("Combat.Damage.Fire", { amount = 25, source = myId })
GameplayTags.Unlisten(m1)                         -- alias of RemoveListener
```

### `Interfaces` global module

The global `Interfaces` table is an interface dispatch system built on top of
`InterfaceComponent`. It adds a central interface declaration registry, bulk operations,
safe/typed calls, multi-return results, broadcast dispatch, and implementation lifecycle hooks.

All per-entity mutations (both the per-entity `ImplementInterface`/`RemoveInterface`/`ClearInterfaces`
calls and the `Interfaces.*` ones) fire the listener callbacks.

```lua
-- ---------- Declaration registry ----------
Interfaces.Declare("Damageable", {"TakeDamage", "Heal"})
Interfaces.Declare("Interactable")                     -- function list optional
Interfaces.AddFunction("Damageable", "Die")
Interfaces.HasDeclaredFunction("Damageable", "Heal")   -- true
local list  = Interfaces.GetFunctions("Damageable")    -- {"TakeDamage","Heal","Die"}
local all   = Interfaces.GetDeclared()
local known = Interfaces.IsDeclared("Damageable")
Interfaces.Undeclare("Damageable")

-- ---------- Per-entity ----------
Interfaces.Implement(entityId, "Damageable")           -- fires OnImplemented
Interfaces.ImplementMany(entityId, {"Damageable","Interactable"})
Interfaces.Remove(entityId, "Damageable")              -- fires OnRemoved
Interfaces.Clear(entityId)                             -- fires OnRemoved per interface
Interfaces.ClearAll()                                  -- all entities in the scene

Interfaces.Implements(entityId, "Damageable")          -- bool
Interfaces.ImplementsAny(entityId, {"A","B"})
Interfaces.ImplementsAll(entityId, {"A","B"})
local ifs   = Interfaces.Get(entityId)                 -- {"Damageable","Interactable"}
local count = Interfaces.Count(entityId)
local has   = Interfaces.HasInterfaceComponent(entityId)

-- ---------- Scene-wide search ----------
local impls = Interfaces.FindImplementers("Damageable")
local any   = Interfaces.FindImplementersOfAny({"Damageable","Healable"})
local all   = Interfaces.FindImplementersOfAll({"Damageable","Interactable"})
local n     = Interfaces.CountImplementers("Damageable")

-- ---------- Dispatch ----------
-- Call(entityId, functionName, ...) returns (ok:bool, returnValues...) — multi-return.
local ok, result, extra = Interfaces.Call(entityId, "TakeDamage", 50, "fire")

-- TryCall: silent on missing function, returns first value or nil.
local healed = Interfaces.TryCall(entityId, "Heal", 10)

-- CallIfImplements: only dispatches when the entity has the given interface declared.
local x = Interfaces.CallIfImplements(entityId, "Damageable", "TakeDamage", 50)

-- Broadcast to every implementer (fire and forget). Returns count of successful calls.
local n = Interfaces.Broadcast("Damageable", "TakeDamage", 10)

-- Broadcast and collect per-entity return values: { { entity=id, value=result }, ... }
local rows = Interfaces.BroadcastWithResults("Damageable", "GetHealth")
for _, row in ipairs(rows) do print(row.entity, row.value) end

-- Dispatch by TagComponent (name tag)
Interfaces.CallOnTag("Enemy", "OnAlert")

-- Dispatch by GameplayTagComponent (hierarchical!)
Interfaces.CallOnGameplayTag("Team.Enemy", "OnAlert")

-- Dispatch to EVERY entity whose env exposes the function (no interface required)
Interfaces.CallOnAll("OnGamePaused", true)

-- Reflection helpers
Interfaces.HasFunction(entityId, "TakeDamage")     -- true if entity env has that function
local fns = Interfaces.ListFunctions(entityId)     -- the functions the script itself defines

-- ---------- Cross-entity variables (read/write another entity's script globals) ----------
-- The variable counterpart to Interfaces.Call: reach a variable that lives in another
-- live entity's script environment (e.g. a `health` global defined in its OnConstruct).
Interfaces.SetVar(entityId, "health", 80)          -- write a global on the target's env → bool
local hp   = Interfaces.GetVar(entityId, "health") -- read it back (nil if absent)
local known = Interfaces.HasVar(entityId, "health")-- true if the var is defined (non-nil)

-- ---------- Lifecycle listeners ----------
local l1 = Interfaces.OnImplemented("Damageable", function(entityId, name) end)
local l2 = Interfaces.OnAnyImplemented(function(eid, name) end)
local l3 = Interfaces.OnRemoved("Damageable", function(eid, name) end)
local l4 = Interfaces.OnAnyRemoved(function(eid, name) end)

Interfaces.RemoveListener(l1)
Interfaces.RemoveAllListeners()                    -- also cleared automatically on runtime stop
```

### Level scripts

Level Script is a global script attached to the scene (level).
Entities can call its functions and share data.

```lua
-- Call function from Level Script
CallLevelFunction("OnBossDeath")
CallLevelFunction("SpawnWave", 3)  -- With args
local result = CallLevelFunction("GetDifficulty")

-- Check if function exists
if HasLevelFunction("OnBossDeath") then
    CallLevelFunction("OnBossDeath")
end

-- Shared level data (accessible from any script)
SetLevelData("score", 1000)
local score = GetLevelData("score")
```

---

## 8. Sprite — Sprites

> **Type:** Entity-bound. Requires **SpriteRendererComponent**.
> An entity can have multiple sprites (layers). `index` = 0 is the first sprite.

### Basic properties

```lua
-- Sprite count
local count = GetSpriteCount()

-- Flip
SetFlipX(true)     -- Horizontal
SetFlipY(true)     -- Vertical
local fx = GetFlipX()
local fy = GetFlipY()

-- Color (RGBA, each 0..1)
SetColor(1, 0, 0, 1)         -- Red
SetColor(1, 1, 1, 0.5)       -- Semi-transparent
local r, g, b, a = GetColor()

-- Opacity
SetSpriteAlpha(0.5)           -- Semi-transparent
local alpha = GetSpriteAlpha()

-- Visibility
SetSpriteVisible(false)
local visible = IsSpriteVisible()
```

### Texture and region

```lua
-- Change texture at runtime
SetSpriteTexture("Content/Textures/hero_damaged.png")
local path = GetSpriteTexturePath()

-- Region (for sprite sheets / atlases)
SetSpriteRegion(0, 0, 32, 32)     -- x, y, w, h in texture
local reg = GetSpriteRegion()       -- → {x, y, w, h}

-- Size of current texture/region
local size = GetSpriteSize()        -- → {width, height}
```

A region set via `SetSpriteRegion` is safe to apply as early as `OnCreate`: it persists across deferred texture loading and is not reset back to the sprite asset's default rect. `SetSpriteTexture` clears the custom region and returns the sprite to asset-driven region resolution.

### Local transform (sprite offset relative to entity)

```lua
SetSpriteLocalPosition(10, 5)           -- Offset
local lp = GetSpriteLocalPosition()     -- → {x, y}

SetSpriteLocalScale(2, 2)              -- Layer scale
local ls = GetSpriteLocalScale()        -- → {x, y}

SetSpriteLocalRotation(45)             -- Local rotation
local lr = GetSpriteLocalRotation()

-- UV scroll offset (for auto-scrolling textures, see GLSLua section)
SetSpriteUVScroll(0.5, 0.0)            -- Shift texture UVs
local uv = GetSpriteUVScroll()          -- → {x, y}

-- UV scale / tiling (how many times texture repeats across the sprite)
SetSpriteUVScale(2.0, 1.0)             -- Tile 2× horizontally
local uvs = GetSpriteUVScale()          -- → {x, y}
```

### World transform (sprite in world space)

Local values are relative to the entity: the engine rotates and scales them by
the entity transform before rendering. When you already have a **world**
coordinate (a socket, a raycast hit, a mouse position), never assign it as a
local one — it would be rotated and scaled a second time. Use the World
accessors, which do the inverse conversion for you:

```lua
SetSpriteWorldPosition(120, 64, 1)      -- Place sprite 1 at a world point
local wp = GetSpriteWorldPosition(1)    -- → {x, y, z}

SetSpriteWorldRotation(30, 1)           -- World angle in degrees (clockwise-positive)
local wr = GetSpriteWorldRotation(1)    -- → number

local ws = GetSpriteWorldScale(1)       -- → {x, y} (entity scale × local scale)
```

World scale is read-only — set the entity scale or the local scale instead.
`SetSpriteWorldPosition` only touches x/y; the z (draw order) stays where
`SetSpriteOrder` put it.

### Sprite asset

```lua
-- Swap the whole .ice_sprite: texture, pivot, source rect, attach points,
-- collision polygon, shading/blend and material are all reloaded.
local ok = SetSpriteAsset("Content/Weapons/SP_Pistol.ice_sprite", 1)
local path = GetSpritePath(1)           -- → current .ice_sprite path

-- SetSpriteTexture only swaps the raw image. It clears attach points
-- (they belong to the old sprite) and keeps pivot and collision polygon.
SetSpriteTexture("Content/Weapons/T_Pistol.png", 1)
```

### Draw order and more

```lua
-- Z-order (render)
SetSpriteOrder(5)
local order = GetSpriteOrder()

-- Pivot (anchor point, 0..1)
SetSpritePivot(0.5, 0)         -- Center-top
local piv = GetSpritePivot()    -- → {x, y}

-- Automatic Y sorting: the engine derives the draw order from the sprite's world Y
-- every frame, so what is lower on screen draws in front. This is the top-down /
-- isometric depth rule, and it runs in C++ - no per-entity Lua call per frame.
SetSpriteYSort(true)              -- enable on sprite 0
SetSpriteYSort(true, -8)          -- enable and set the bias in one call
SetSpriteYSort(true, 0, 1)        -- ... on sprite index 1
local sorted = GetSpriteYSort()

-- Bias is added to the computed order: negative pushes back, positive pulls forward.
-- Use it for a shadow that must stay behind its owner, or a hat that must stay in front.
SetSpriteYSortBias(-4)
local bias = GetSpriteYSortBias()

-- Name / path
local name = GetSpriteName()
local path = GetSpritePath()

-- Render in game
SetSpriteRenderInGame(true)
local render = GetSpriteRenderInGame()

-- Cast shadows (without collider)
SetSpriteCastShadow(true)
local shadow = GetSpriteCastShadow()

-- Don't block shadows (default true): while the global "Colliders Block Shadows" option is on, this sprite still lets shadows pass through it. Turn off so it blocks shadows like terrain.
SetSpriteDontBlockShadows(true)
local dontBlock = GetSpriteDontBlockShadows()  -- → bool

-- Cast shadow mode: 0 = Colliders (asset collision shapes), 1 = Contour (texture alpha silhouette)
SetSpriteCastShadowMode(1)
local mode = GetSpriteCastShadowMode()      -- → int

-- Shadow origin: 0 = Center, 1 = Top, 2 = Bottom
SetSpriteShadowOrigin(1)
local origin = GetSpriteShadowOrigin()      -- → int

-- Shadow edge fade [0..1]
SetSpriteShadowEdgeFade(0.25)
local fade = GetSpriteShadowEdgeFade()      -- → float

-- Shadow Z-order: negative = toward background, positive = toward foreground, 0 = caster plane (default)
SetSpriteShadowZOrder(1)
local zo = GetSpriteShadowZOrder()          -- → float

-- Optional second argument selects sprite index (default 0).
SetSpriteShadowZOrder(1, 0)
```

### Attach points

```lua
-- Local point
local ap = GetSpriteAttachPoint("Weapon")
-- → {found=true/false, x, y, rotation}

-- World point (with entity transform)
local wp = GetSpriteAttachPointWorld("Weapon")
-- → {found=true/false, x, y, rotation}
-- rotation is in degrees (clockwise-positive) and matches the socket arrow
-- authored in the Sprite Editor; FlipX/FlipY mirror both position and angle

-- List of attach point names
local names = GetSpriteAttachPointNames()   -- → {"Weapon", "Shield", ...}
local count = GetSpriteAttachPointCount()   -- Count
```

### Socket attach (engine-driven)

Instead of repositioning an attached sprite yourself every frame, tell the
engine which socket it should follow. The engine recomputes position and
rotation after animations and before `OnLateUpdate`, correctly under entity
rotation, entity scale and FlipX.

```lua
-- Sprite 1 follows the socket named "ArmSocket" found anywhere on this entity
AttachSpriteToSocket("ArmSocket", 1)

-- Explicit source. source = "auto" | "sprite" | "flipbook" | "skeleton"
-- sourceIndex = instance index of that component, -1 = search all instances
-- inheritFlipX = copy FlipX from the socket owner (default true)
AttachSpriteToSocketFrom("hand_point", "skeleton", -1, true, 1)

DetachSpriteFromSocket(1)               -- Back to a normal local transform

local a = GetSpriteSocketAttach(1)
-- → {socket="ArmSocket", source="auto", sourceIndex=-1, inheritFlipX=true,
--    offsetX=0, offsetY=0, offsetRotation=0, attached=true/false}

-- attached is false when the current animation frame has no such socket —
-- typically what you want to drive visibility with:
SetSpriteVisible(IsSpriteSocketAttached(1), 1)

-- Offset layered on top of the socket: socket space, pixels, scales with the
-- entity and mirrors with Flip X. Fine-tune placement, or animate recoil:
SetSpriteSocketOffset(-3, 0, -8, 1)     -- 3 px back, 8° muzzle-up
local off = GetSpriteSocketOffset(1)    -- → {x, y, rotation}
```

An attached instance's local transform is engine-owned — write to
`AttachSocketOffset` (via `SetSpriteSocketOffset`) instead of
`SetSpriteLocalPosition`, otherwise your value is overwritten on the next
socket pass. The pass runs twice per frame — before and after `OnLateUpdate` —
so an aim rotation applied in `OnLateUpdate` still lands on the same frame.
Local scale and draw order (`SetSpriteOrder`) stay yours.

Sources are searched in this order when `source` is `"auto"`: skeleton →
sprite instances → flipbook instances. The attached instance is skipped, so a
sprite can never attach to its own socket. The same fields exist on the
Sprite and Flipbook instances in the Class Editor and the Properties panel
(*Attach To Socket*), so you can set this up without any script at all.

*Attach To Collider* wins over *Attach To Socket*: an instance already bound to
a separate-body collider keeps following that body and its socket binding is
ignored (`IsSpriteSocketAttached` reports `false`). Clear the collider binding
first if you want the socket to drive it.

Because the attachment is derived from replicated animation state, it resolves
identically on every peer in multiplayer — no extra replication needed.

### Collision polygon

```lua
-- Check if sprite has a collision polygon (>= 3 points)
local has = HasSpriteCollisionPolygon()          -- true/false
local has = HasSpriteCollisionPolygon(1)          -- Second sprite

-- Get number of collision polygon points
local count = GetSpriteCollisionPointCount()      -- 0 if no polygon

-- Get all collision polygon points (normalized 0-1)
local poly = GetSpriteCollisionPolygon()
-- → { {x=0.1, y=0.2}, {x=0.9, y=0.2}, {x=0.9, y=0.9}, ... }
for i, pt in ipairs(poly) do
    print(pt.x, pt.y)
end
```

The generated Box2D shapes follow the instance's **full local transform** —
position, rotation and scale — as well as the entity transform, so the physics
shape always lands exactly where the sprite is drawn and matches the collider
debug view. A negative scale mirrors the polygon about the pivot just like it
mirrors the art. That includes socket-attached instances: a pistol glued to a
rotating hand keeps a correctly placed and rotated collider.
While only the transform changes the shapes are updated in place, so contacts and
sensor state are preserved; a full rebuild happens only when the polygon itself,
the flips, the pivot or a shape property changes (for example on a flipbook frame
swap).

### Collision shape properties

```lua
-- Number of runtime collision shapes
local count = GetSpriteCollisionShapeCount()

-- Density
SetSpriteCollisionDensity(1.0)
local density = GetSpriteCollisionDensity()

-- Friction
SetSpriteCollisionFriction(0.5)
local friction = GetSpriteCollisionFriction()

-- Restitution (bounciness)
SetSpriteCollisionRestitution(0.3)
local rest = GetSpriteCollisionRestitution()

-- Sensor (triggers overlap events instead of physical collision)
SetSpriteCollisionSensor(true)
local sensor = IsSpriteCollisionSensor()

-- One Way Platform (objects pass through from below, collide from above)
SetSpriteCollisionOneWay(true)
local oneWay = IsSpriteCollisionOneWay()

-- One-way direction: 1 = Up (default), 2 = Down, 3 = Left, 4 = Right
-- Direction is the side from which the platform is solid (cannot pass through)
SetSpriteCollisionOneWayDirection(1)
local dir = GetSpriteCollisionOneWayDirection()

-- Contact events (OnCollisionEnter / OnCollisionExit callbacks)
SetSpriteContactEventsEnabled(true)
local contact = AreSpriteContactEventsEnabled()

-- Sensor events (OnSensorEnter / OnSensorExit callbacks)
SetSpriteSensorEventsEnabled(true)
local sensorEvt = AreSpriteSensorEventsEnabled()

-- Hit events (OnCollisionHit callback with impact force)
SetSpriteHitEventsEnabled(true)
local hit = AreSpriteHitEventsEnabled()

-- Pre-solve events (called before collision response is calculated)
SetSpritePreSolveEventsEnabled(true)
local preSolve = AreSpritePreSolveEventsEnabled()


-- All functions accept an optional sprite index:
SetSpriteCollisionDensity(2.0, 1)  -- Second sprite
SetSpriteCollisionOneWay(true, 1)  -- Second sprite
```

### Working with multiple sprites

```lua
-- All functions accept an optional index:
SetFlipX(true, 1)              -- Second sprite
SetColor(1, 0, 0, 1, 2)         -- Third sprite
SetSpriteVisible(false, 1)
```

---

## 9. Flipbook — Frame Animation

> **Type:** Entity-bound. Requires **FlipbookComponent**.
> Flipbook is sprite-sheet frame animation. It is not the same as Animator.

```lua
-- Playback control
SetFlipbookPlaying(true)
SetFlipbookPlaying(false)
local playing = IsFlipbookPlaying()

-- Speed
SetFlipbookSpeed(2.0)           -- Double speed
local speed = GetFlipbookSpeed()

-- Frames
SetFlipbookFrame(5)             -- Go to frame 5
local frame = GetFlipbookFrame()
local total = GetFlipbookFrameCount()
ResetFlipbook()                 -- Frame 0

-- Timer
local timer = GetFlipbookTimer()

-- Visual properties
SetFlipbookColor(1, 0, 0, 1)
local r, g, b, a = GetFlipbookColor()
SetFlipbookFlipX(true)
local flipX = GetFlipbookFlipX()     -- optional: GetFlipbookFlipX(index)
SetFlipbookFlipY(false)
local flipY = GetFlipbookFlipY()     -- optional: GetFlipbookFlipY(index)
SetFlipbookAlpha(0.5)
local alpha = GetFlipbookAlpha()
SetFlipbookVisible(true)
local vis = IsFlipbookVisible()
SetFlipbookRenderInGame(true)
local render = GetFlipbookRenderInGame()

-- Cast shadows (without collider)
SetFlipbookCastShadow(true)
local shadow = GetFlipbookCastShadow()

-- Don't block shadows (default true): while the global "Colliders Block Shadows" option is on, this flipbook still lets shadows pass through it. Turn off so it blocks shadows like terrain.
SetFlipbookDontBlockShadows(true)
local dontBlock = GetFlipbookDontBlockShadows()  -- → bool

-- Cast shadow mode: 0 = Colliders (asset collision shapes), 1 = Contour (texture alpha silhouette)
SetFlipbookCastShadowMode(1)
local mode = GetFlipbookCastShadowMode()    -- → int

-- Shadow origin: 0 = Center, 1 = Top, 2 = Bottom
SetFlipbookShadowOrigin(1)
local origin = GetFlipbookShadowOrigin()    -- → int

-- Shadow edge fade [0..1]
SetFlipbookShadowEdgeFade(0.25)
local fade = GetFlipbookShadowEdgeFade()    -- → float

-- Shadow Z-order: negative = toward background, positive = toward foreground, 0 = caster plane (default)
SetFlipbookShadowZOrder(1)
local zo = GetFlipbookShadowZOrder()        -- → float

-- Optional second argument selects flipbook index (default 0).
SetFlipbookShadowZOrder(1, 0)

-- Count and names
local count = GetFlipbookCount()
local name = GetFlipbookName(0)
local path = GetFlipbookPath(0)

-- Change flipbook at runtime
SetFlipbookPath("Content/Flipbooks/Run.ice_flipbook")

-- Render order (Z-depth)
SetFlipbookOrder(5.0)                -- without index — sets entity Z
SetFlipbookOrder(5.0, 1)             -- with index — sets flipbook instance Z
local order = GetFlipbookOrder()     -- optional: GetFlipbookOrder(index)

-- Local transform
SetFlipbookLocalPosition(5, 0)
local lp = GetFlipbookLocalPosition()
SetFlipbookLocalScale(1, 1)
local ls = GetFlipbookLocalScale()
SetFlipbookLocalRotation(0)
local lr = GetFlipbookLocalRotation()

-- UV scroll offset (for auto-scrolling flipbook textures)
SetFlipbookUVScroll(0.5, 0.0)
local uv = GetFlipbookUVScroll()          -- → {x, y}

-- UV scale / tiling (how many times texture repeats across the flipbook)
SetFlipbookUVScale(2.0, 1.0)
local uvs = GetFlipbookUVScale()          -- → {x, y}

-- World transform (see the Sprite section — same rules and same conversion)
SetFlipbookWorldPosition(120, 64, 0)
local fwp = GetFlipbookWorldPosition(0)   -- → {x, y, z}
SetFlipbookWorldRotation(30, 0)
local fwr = GetFlipbookWorldRotation(0)   -- → number
local fws = GetFlipbookWorldScale(0)      -- → {x, y}, read-only

-- Attach points. A flipbook has no sockets of its own — it exposes the
-- sockets of the sprite of the CURRENT frame, so they animate frame by frame.
local ap = GetFlipbookAttachPoint("Hand")
local wp = GetFlipbookAttachPointWorld("Hand")
-- → {found, x, y, rotation}
-- rotation is in degrees (clockwise-positive) and matches the socket arrow
-- authored in the Sprite Editor; FlipX/FlipY mirror both position and angle
local names = GetFlipbookAttachPointNames()   -- → {"Hand", "Foot", ...}
local apCount = GetFlipbookAttachPointCount()

-- Socket attach (identical semantics to the Sprite version)
AttachFlipbookToSocket("ArmSocket", 1)
AttachFlipbookToSocketFrom("hand_point", "skeleton", -1, true, 1)
DetachFlipbookFromSocket(1)
SetFlipbookSocketOffset(-3, 0, -8, 1)
local fOff = GetFlipbookSocketOffset(1)   -- → {x, y, rotation}
local fa = GetFlipbookSocketAttach(1)
local isAttached = IsFlipbookSocketAttached(1)
```

### Collision polygon

```lua
-- Check if current flipbook frame has a collision polygon
local has = HasFlipbookCollisionPolygon()          -- true/false
local has = HasFlipbookCollisionPolygon(1)          -- Second flipbook

-- Get number of collision polygon points
local count = GetFlipbookCollisionPointCount()

-- Get all collision polygon points (normalized 0-1)
local poly = GetFlipbookCollisionPolygon()
-- → { {x=0.1, y=0.2}, {x=0.9, y=0.2}, ... }
-- Note: polygon changes per frame since each frame is a separate sprite
```

### Collision shape properties

```lua
-- Number of runtime collision shapes
local count = GetFlipbookCollisionShapeCount()

-- Density
SetFlipbookCollisionDensity(1.0)
local density = GetFlipbookCollisionDensity()

-- Friction
SetFlipbookCollisionFriction(0.5)
local friction = GetFlipbookCollisionFriction()

-- Restitution (bounciness)
SetFlipbookCollisionRestitution(0.3)
local rest = GetFlipbookCollisionRestitution()

-- Sensor (triggers overlap events instead of physical collision)
SetFlipbookCollisionSensor(true)
local sensor = IsFlipbookCollisionSensor()

-- One Way Platform (objects pass through from below, collide from above)
SetFlipbookCollisionOneWay(true)
local oneWay = IsFlipbookCollisionOneWay()

-- One-way direction: 1 = Up (default), 2 = Down, 3 = Left, 4 = Right
-- Direction is the side from which the platform is solid (cannot pass through)
SetFlipbookCollisionOneWayDirection(1)
local dir = GetFlipbookCollisionOneWayDirection()

-- Contact events (OnCollisionEnter / OnCollisionExit callbacks)
SetFlipbookContactEventsEnabled(true)
local contact = AreFlipbookContactEventsEnabled()

-- Sensor events (OnSensorEnter / OnSensorExit callbacks)
SetFlipbookSensorEventsEnabled(true)
local sensorEvt = AreFlipbookSensorEventsEnabled()

-- Hit events (OnCollisionHit callback with impact force)
SetFlipbookHitEventsEnabled(true)
local hit = AreFlipbookHitEventsEnabled()

-- Pre-solve events (called before collision response is calculated)
SetFlipbookPreSolveEventsEnabled(true)
local preSolve = AreFlipbookPreSolveEventsEnabled()


-- All functions accept an optional flipbook index:
SetFlipbookCollisionDensity(2.0, 1)  -- Second flipbook
SetFlipbookCollisionOneWay(true, 1)  -- Second flipbook
```

### Animation state

```lua
-- Looping
SetFlipbookLoop(false)            -- Disable looping
local loop = GetFlipbookLoop()    -- true/false

-- Progress (0.0 — 1.0)
local progress = GetFlipbookProgress()

-- Is animation finished? (for non-looping)
local finished = IsFlipbookFinished()
local justFinished = DidFlipbookJustFinish()  -- true only on finish frame

-- Frame tracking
local changed = HasFrameChanged()             -- Frame changed this frame?
local prevFrame = GetPreviousFrame()          -- Previous frame

-- Check if specific frame reached (this frame)
if WasFrameReached(3) then
    PlaySound("footstep")
end

-- Check if frame range active
local inRange = WasFrameRangeActive(2, 5)     -- Frame between 2 and 5?
local rangeStarted = DidFrameRangeStart(2, 5) -- Just entered range?
```

### Example: attack with frame events

```lua
function OnUpdate(dt)
    if IsKeyJustPressed("space") then
        SetFlipbookPath("Content/Flipbooks/Attack.ice_flipbook")
        SetFlipbookLoop(false)
        SetFlipbookPlaying(true)
    end

    -- Frame 3 — deal damage
    if WasFrameReached(3) then
        DealDamage()
    end

    -- Animation finished — return to Idle
    if DidFlipbookJustFinish() then
        SetFlipbookPath("Content/Flipbooks/Idle.ice_flipbook")
        SetFlipbookLoop(true)
        SetFlipbookPlaying(true)
    end
end
```

---

## 10. Animation — Animator (State Machine)

> **Type:** Entity-bound. Requires **AnimatorComponent**.
> Animator is a state machine with transitions, driven by parameters.

### Parameters

```lua
-- Set parameters (used in animation transitions)
SetAnimBool("isRunning", true)
SetAnimInt("direction", 2)
SetAnimFloat("speed", 150.5)
SetAnimTrigger("attack")        -- Trigger — one-shot flag

-- Get
local running = GetAnimBool("isRunning")
local dir = GetAnimInt("direction")
local speed = GetAnimFloat("speed")
```

### Trigger journal (replication)

A trigger is a one-shot event: the animator consumes it on the very frame the transition fires,
so polling `GetAnimBool`-style is not enough to detect it. The animator therefore keeps a journal
of triggers that have been raised but not yet read by a script.

```lua
local epoch = GetAnimTriggerEpoch()   -- Monotonic counter, +1 per new trigger name in the journal

local log = ConsumeAnimTriggers()     -- Drains the journal
-- log.epoch          -- Counter value at the moment of the read (number)
-- log.names          -- Array of trigger names raised since the previous read
for i = 1, #log.names do
    print(log.names[i])
end
```

`ConsumeAnimTriggers` clears the journal but never resets `epoch` — the counter only grows while
the component lives. This is what makes triggers replicable over the network: send `epoch` plus
the last batch of names in **every** state packet, and the receiver replays a batch exactly once,
when the incoming `epoch` exceeds the one it applied last. Because the data is resent every packet,
a lost packet or a missed sampling window no longer swallows an animation.

```lua
local log = ConsumeAnimTriggers()
if #log.names > 0 then
    lastEpoch, lastNames = log.epoch, log.names
end

-- the triggers argument is a { name = true } map, not an array
local trigSet = {}
for _, name in ipairs(lastNames or {}) do trigSet[name] = true end

Network.SyncEntityAnimatorParams(key, bools, { trigEpoch = lastEpoch }, nil, trigSet, false)
```

### State

```lua
-- Current state
local state = GetAnimState()        -- State name (string)
local stateTime = GetAnimStateTime() -- Time in current state

-- Current frame
local frame = GetAnimFrame()

-- Force transition to a state
ForceAnimState("Jump")
ForceAnimState("Jump", 0.2)         -- With transition duration

-- Transition
local transitioning = IsAnimTransitioning()
local progress = GetAnimTransitionProgress()  -- 0..1

-- Reset all triggers
ResetAnimTriggers()

-- Has animator? entityId is optional — omit it for the current entity
local has = HasAnimator()
local other = HasAnimator(entityId)

-- Is the animation asset actually loaded (not just the component present)?
local loaded = IsAnimationLoaded()
```

### Target Sprite

```lua
-- Set which sprite instance the animator targets (by name)
-- When an entity has multiple sprites, this selects which one receives animation textures
SetAnimTargetSprite("Body")

-- Get the current target sprite name (empty string = auto / first sprite)
local target = GetAnimTargetSprite()

-- Reset to default (first sprite)
SetAnimTargetSprite("")
```

### Typical example

```lua
function OnUpdate(dt)
    local vx = GetVelocityX()
    local vy = GetVelocityY()

    SetAnimBool("isRunning", math.abs(vx) > 10)
    SetAnimBool("isFalling", IsFalling())
    SetAnimBool("isGrounded", IsGrounded())

    -- Facing direction
    if vx > 0 then SetFlipX(false) end
    if vx < 0 then SetFlipX(true) end

    -- Attack on key press
    if IsKeyJustPressed("space") then
        SetAnimTrigger("attack")
    end
end
```

### Animation asset

```lua
local path = GetAnimationPath()                        -- → current .ice_animation
local ok = SetAnimationAsset("Content/Anim/Boss.ice_animation")
-- Resets state, frame, transition and blending, then loads the new graph.
-- Re-apply your parameters (SetAnimBool/Int/Float) afterwards.
```

---

## 10.5. Skeleton — Bone Animation and Ragdoll

> **Type:** Entity-bound. Requires **SkeletonComponent** (a `.ice_skeleton` asset).
> A skeleton is a bone hierarchy with image/mesh attachments, skins, keyframed animations (incl. mesh deformation and IK), animation events, and an integrated hybrid active-ragdoll. World-space results follow the engine convention: **X+ right, Y+ up, rotation clockwise-positive (degrees)**.

### Playback

```lua
-- Play an animation by name (loop defaults to true, blendDuration defaults to 0)
PlaySkeletonAnimation("run")
PlaySkeletonAnimation("attack", false)         -- one-shot
PlaySkeletonAnimation("walk", true, 0.15)      -- crossfade over 0.15s

StopSkeletonAnimation()

-- Time / speed
SetSkeletonAnimationTime(0.0)
local t = GetSkeletonAnimationTime()
local name = GetSkeletonAnimation()
local playing = IsSkeletonPlaying()
SetSkeletonSpeed(1.5)
local s = GetSkeletonSpeed()

local has = HasSkeleton()

-- Loop control & introspection
SetSkeletonLoop(true)
local looping = IsSkeletonLooping()
local dur  = GetSkeletonDuration()                  -- duration (s) of the current animation
local d2   = GetSkeletonAnimationDuration("attack") -- duration (s) of a named animation
local ex   = HasSkeletonAnimation("attack")
local list = GetSkeletonAnimationList()             -- array of all animation names
local nt   = GetSkeletonNormalizedTime()            -- 0..1 phase of the current animation
SetSkeletonNormalizedTime(0.5)                      -- jump to 50% of the current animation
local done = IsSkeletonAnimationFinished()          -- true once a non-looping animation ends
```

> During a crossfade the previous animation keeps playing (its time keeps advancing), so transitions stay fluid instead of fading from a frozen pose.

### Animation layers (blending / mixing)

Layers let several animations play **simultaneously** on one skeleton — run with the legs while attacking with the arms, crouch and shoot, breathe additively on top of everything. Each layer is an independent track applied **on top** of the base animation (the one driven by `PlaySkeletonAnimation`). Higher track numbers are applied later (they win where they overlap).

```lua
-- PlaySkeletonLayerAnimation(track, name [, loop=false, fade=0, weight=1, rootBone="", additive=false])
--   track    — any integer >= 1; each track holds one animation at a time
--   loop     — one-shots auto fade out when they finish (see SetSkeletonLayerHold)
--   fade     — seconds used for fade-in, fade-out and crossfades inside this track
--   weight   — 0..1 blend strength of the layer
--   rootBone — limit the layer to a bone and ALL its descendants ("" = whole skeleton)
--   additive — add the animation's deltas on top instead of overriding the pose

-- Attack with the upper body while the base layer keeps running:
PlaySkeletonAnimation("run", true, 0.12)
PlaySkeletonLayerAnimation(1, "attack", false, 0.08, 1.0, "chest")

-- Aim-hold on the arm only, at 70% strength:
PlaySkeletonLayerAnimation(2, "aim", true, 0.15, 0.7, "arm_up_near")

-- Additive breathing over everything:
PlaySkeletonLayerAnimation(3, "breath", true, 0.3, 1.0, "", true)

StopSkeletonLayerAnimation(1)            -- instant
StopSkeletonLayerAnimation(1, 0.1)       -- fade out over 0.1s
StopAllSkeletonLayerAnimations(0.1)

-- Introspection / control
local name = GetSkeletonLayerAnimation(1)       -- "" when the track is empty
local on   = IsSkeletonLayerActive(1)           -- true while playing or fading
local pl   = IsSkeletonLayerPlaying(1)
local fin  = IsSkeletonLayerFinished(1)         -- non-loop reached its end
local nt   = GetSkeletonLayerNormalizedTime(1)  -- 0..1
local t    = GetSkeletonLayerTime(1)
SetSkeletonLayerTime(1, 0.0)
SetSkeletonLayerWeight(1, 0.5)
local w    = GetSkeletonLayerWeight(1)
SetSkeletonLayerSpeed(1, 1.5)
local sp   = GetSkeletonLayerSpeed(1)
SetSkeletonLayerHold(1, true)                   -- keep the last frame instead of auto fade-out
SetSkeletonLayerAdditive(1, true)
SetSkeletonLayerRootBone(1, "chest")
local tracks = GetSkeletonActiveLayerTracks()   -- array of active track numbers
```

Notes:

- A layer only touches the channels its animation actually keys (Spine-style), so an attack that keys just the arms leaves the torso bob of the base run intact even when `rootBone` covers the whole upper body.
- Playing a new animation on an occupied track crossfades inside the track using `fade`; re-playing the same name restarts it with the same smooth blend — safe to spam attack.
- Slot timelines (colors, attachment swaps, deforms) of a layer apply only to slots whose bone is inside the layer mask; attachment/draw-order switches engage while the layer is dominant (weight ≥ 0.5).
- Animation **events fire from layers too** — put your `hit` event in the attack animation and read it via `GetSkeletonEvents()` as usual.
- Layer time scales with both `SetSkeletonLayerSpeed` and the skeleton's global `SetSkeletonSpeed`.
- Bone overrides (`SetSkeletonBoneOverride`) apply after all layers; ragdoll motors track the final layered pose.

### Skins and attachments

```lua
-- Swap the whole skin (armor sets, character variants)
SetSkeletonSkin("gold_armor")
local skin = GetSkeletonSkin()

-- Override the active attachment of a single slot (per-entity, not shared)
SetSkeletonAttachment("weapon", "axe")
SetSkeletonAttachment("weapon", "")            -- empty hides the slot

-- Tint and facing (FlipX = horizontal facing, FlipY = texture-V convention, default on)
SetSkeletonColor(1, 0.5, 0.5)                  -- r, g, b [, a]
SetSkeletonFlip(true, true)                    -- flipX, flipY

-- Slot / skin enumeration
local slots = GetSkeletonSlotNames()           -- array of slot names
local skins = GetSkeletonSkinNames()           -- array of skin names

-- Per-slot tint override (e.g. flash one limb white on hit). Replaces the
-- slot's animated colour until cleared.
SetSkeletonSlotColor("left_arm", 1, 1, 1)      -- r, g, b [, a]
ClearSkeletonSlotColor("left_arm")

-- Explicitly hide / show a single slot (independent of its active attachment)
HideSkeletonSlot("left_arm")
ShowSkeletonSlot("left_arm")

-- Whole-skeleton visibility
SetSkeletonVisible(true)
local vis = IsSkeletonVisible()
```

### Shadows

```lua
-- Cast shadows (per-slot)
SetSkeletonCastShadow(true)
local shadow = GetSkeletonCastShadow()

-- Don't block shadows (default true): while the global "Colliders Block Shadows" option is on, this skeleton still lets shadows pass through it. Turn off so it blocks shadows like terrain.
SetSkeletonDontBlockShadows(true)
local dontBlock = GetSkeletonDontBlockShadows()  -- → bool

-- Cast shadow mode: 0 = Colliders (per-slot quads), 1 = Contour (per-slot texture alpha silhouette)
SetSkeletonCastShadowMode(1)
local mode = GetSkeletonCastShadowMode()       -- → int

-- Shadow origin: 0 = Center, 1 = Top, 2 = Bottom
SetSkeletonShadowOrigin(1)
local origin = GetSkeletonShadowOrigin()       -- → int

-- Shadow edge fade [0..1]
SetSkeletonShadowEdgeFade(0.25)
local fade = GetSkeletonShadowEdgeFade()       -- → float

-- Shadow Z-order: negative = toward background, positive = toward foreground, 0 = caster plane (default)
SetSkeletonShadowZOrder(1)
local zo = GetSkeletonShadowZOrder()           -- → float
```

### Bones

```lua
-- World transform of a bone (already composes the entity transform)
local b = GetSkeletonBoneWorld("head")
-- b = { found, x, y, rotation, scaleX, scaleY }
if b.found then DrawDebugCircle(b.x, b.y, 4) end

-- Procedural override (e.g. look-at, recoil). weight 0..1 blends over the animation.
SetSkeletonBoneOverride("head", x, y, rot)            -- defaults sx=sy=1, weight=1
SetSkeletonBoneOverride("head", x, y, rot, 1, 1, 0.5) -- 50% blend
ClearSkeletonBoneOverride("head")

-- Enumeration & local (parent-relative) pose
local bones   = GetSkeletonBoneNames()                -- array of bone names
local count   = GetSkeletonBoneCount()
local hasBone = HasSkeletonBone("head")
local l = GetSkeletonBoneLocal("head")
-- l = { found, x, y, rotation, scaleX, scaleY, shearX, shearY }
```

### Attach points (weapons / VFX)

```lua
-- Point attachments resolved to world space (composes the entity transform)
local p = GetSkeletonAttachPointWorld("muzzle")
-- p = { found, x, y, rotation }
if p.found then SpawnBullet(p.x, p.y, p.rotation) end

-- Same point in skeleton-local space (no entity transform, no FlipX)
local lp = GetSkeletonAttachPoint("muzzle")    -- → {found, x, y, rotation}

local names = GetSkeletonAttachPointNames()    -- array of strings
local count = GetSkeletonAttachPointCount()    -- → int
```

Socket positions follow the live pose, so they also track `SetSkeletonBoneOverride`
(mouse-aim pitch, look-at) and IK constraints. During ragdoll they come straight
from the physics bodies. To glue a weapon sprite to one, use the engine attach
instead of moving it by hand:

```lua
AttachSpriteToSocketFrom("hand_point", "skeleton", -1, true, 1)
```

### Skeleton asset

```lua
local path = GetSkeletonPath()                       -- → current .ice_skeleton
local ok = SetSkeletonAsset("Content/SK_Boss.ice_skeleton")
-- Destroys ragdoll/bone bodies, clears pose, skins, layers and overrides,
-- then loads the new skeleton. Call PlaySkeletonAnimation afterwards.
```

### Bone colliders (animated hit bodies)

While the skeleton is animated (ragdoll off), every physics-bone gets a **kinematic collider that follows the pose** each frame — entity transform, FlipX and scale included. They push dynamic objects (crates, other ragdolls), receive hits, show up in the collider debug view, and never collide with the entity's own Rigidbody controller. When the ragdoll activates the same bodies switch to dynamic and inherit their tracked velocities, so momentum carries over for free.

```lua
SetSkeletonBoneColliders(true)                  -- on by default (editor checkbox "Bone Colliders")
local on = AreSkeletonBoneCollidersEnabled()
```

### Ragdoll (hybrid active-ragdoll)

```lua
-- Build bodies + joints from the physics-bones and switch to physics.
-- blend: 0 = stiff motors (tracks animation but reacts to collisions), 1 = fully limp.
EnableSkeletonRagdoll()        -- defaults to 1 (limp)
EnableSkeletonRagdoll(0.85)    -- mostly limp, a little driven

DisableSkeletonRagdoll()       -- destroy bodies, back to animation
local r = IsSkeletonRagdoll()

SetSkeletonRagdollBlend(0.4)   -- live blend; lerp toward 0 for a get-up
local b = GetSkeletonRagdollBlend()

-- Directional hit reaction on a specific bone body
ApplySkeletonBoneImpulse("torso", dirX * 400, dirY * 400)

-- Manual enable/disable also works via the component flag (editor checkbox / Lua):
-- the runtime keeps RagdollEnabled and the live state in sync.

-- Extended physics control on bone bodies (ragdoll must be active)
ApplySkeletonBoneImpulseAtPoint("torso", ix, iy, px, py) -- impulse at a world point (adds spin)
ApplySkeletonBoneTorque("torso", 5000)                   -- continuous torque (clockwise-positive)
ApplySkeletonBoneAngularImpulse("torso", 200)            -- instant angular kick
local v = GetSkeletonBoneVelocity("torso")               -- { found, x, y } in px/s

ApplySkeletonRagdollImpulse(dirX * 500, dirY * 500)      -- push every bone body (whole-body blast)
SetSkeletonRagdollGravityScale(1.0)                      -- live gravity scale for all bodies
SetSkeletonRagdollAngularDamping(0.5)                    -- live angular damping for all bodies
```

### Dismemberment (limb severing)

```lua
-- Snap a bone's joint so the limb (and everything below it in the hierarchy)
-- detaches and flies off as free debris. Ragdoll must be active — the severed
-- bones keep simulating as loose bodies.
local cut  = SeverSkeletonBone("left_forearm")    -- → true if a joint was broken
local gone = IsSkeletonBoneSevered("left_forearm")
local all  = GetSeveredSkeletonBones()            -- array of severed bone names

-- Per-bone break threshold: the joint auto-snaps when the constraint force
-- exceeds it. Author it in the Skeleton Editor (physics tab) or override per
-- instance at runtime. 0 = never auto-breaks.
SetSkeletonBoneBreakForce("neck", 800)

-- Bones torn off by force this frame (auto-break) — trigger gore VFX / SFX here
for _, boneName in ipairs(GetSkeletonSeverEvents()) do
    SpawnBloodFountain(boneName)
    HideSkeletonSlot(boneName .. "_skin")         -- or swap to a bloody-stump attachment
end
```

### Hitboxes (bounding boxes)

```lua
-- Bounding-box attachments authored per slot are resolved to world space every
-- frame, following the live pose / ragdoll — ideal for per-limb hurtboxes.
local boxes = GetSkeletonBoundingBoxNames()                  -- array of box names
local hb = GetSkeletonBoundingBoxWorld("head_hurtbox")
-- hb = { found, points = { {x = .., y = ..}, ... } }
if SkeletonBoundingBoxContainsPoint("head_hurtbox", hitX, hitY) then
    SeverSkeletonBone("head")
end
```

### Events

```lua
-- Events crossed this frame (set up in the Skeleton Editor's animation timeline)
for _, e in ipairs(GetSkeletonEvents()) do
    -- e = { name, int, float, string, time }
    if e.name == "footstep" then PlaySound("step") end
    if e.name == "hit" then DealDamage(e.int) end
end
```

### Typical example

```lua
function OnUpdate(dt)
    if IsSkeletonRagdoll() then
        -- ease back to animation control after a knockdown
        local b = GetSkeletonRagdollBlend() - dt * 0.8
        if b <= 0 then
            DisableSkeletonRagdoll()
            PlaySkeletonAnimation("getup", false, 0.1)
        else
            SetSkeletonRagdollBlend(b)
        end
        return
    end

    local vx = GetVelocityX()
    if math.abs(vx) > 10 then
        PlaySkeletonAnimation("run", true, 0.12)
        SetSkeletonFlip(vx < 0, true)
    else
        PlaySkeletonAnimation("idle", true, 0.12)
    end

    for _, e in ipairs(GetSkeletonEvents()) do
        if e.name == "footstep" then PlaySound("step") end
    end
end

function OnHit(dirX, dirY)
    EnableSkeletonRagdoll(0.9)
    ApplySkeletonBoneImpulse("torso", dirX * 500, dirY * 500)
end
```

### Material, shading & blend

```lua
-- Per-entity shading and blend (independent of the skeleton asset)
SetSkeletonShadingMode("Unlit")            -- "Lit" | "Unlit"
local sm = GetSkeletonShadingMode()
SetSkeletonBlendMode("Additive")           -- "Masked" | "Additive" | "Translucent" | "Opaque"
local bm = GetSkeletonBlendMode()
SetSkeletonAlphaClip(0.5)                  -- masked cutoff 0..1
local ac = GetSkeletonAlphaClip()

-- Assign a custom material / material instance (.ice_material or .ice_matinst)
SetSkeletonMaterial("Materials/Hero.ice_matinst")
local mat = GetSkeletonMaterial()          -- "" when none / not a custom shader
ClearSkeletonMaterial()
```

### Vertex effects (GLSL)

```lua
-- Skeleton-specific helpers
GLSL_SkeletonSetParallax(0.1, 0.05)
GLSL_SkeletonSetSway(2.0, 1.5)             -- amplitude, speed [, phaseOffset, gradient]
GLSL_SkeletonSetWind(0.3)                  -- strength [, speed]

-- The generic GLSL_* helpers also resolve to the skeleton when the entity
-- has no sprite / flipbook component:
GLSL_SetParallax(0.1, 0.05)
GLSL_ClearEffects()
```

---

## 11. Camera — Camera

> **Type:** Entity-bound + global. Works with the **primary camera** (Primary = true).

### Position

```lua
SetCameraPosition(100, 200)
local pos = GetCameraPosition()  -- → {x, y}
MoveCamera(5, 0)                 -- Move
```

### Orthographic width (camera scale)

```lua
SetCameraOrthoWidth(2.0)               -- Scale up (zoom in)
local ortho = GetCameraOrthoWidth()    -- Get current ortho width
```

### Follow the character

```lua
-- Instant
CameraFollowMe()

-- Smooth (lerp)
CameraFollowSmooth(0.1)         -- lerpFactor (0..1). Lower = smoother.
```

### Camera lag

```lua
-- Camera lag is a persistent setting. Call ONCE (e.g. in OnCreate)
-- to configure how the engine smooths the primary camera toward its
-- follow target. Speed is in pixels/second.
SetCameraLag(600, 120)          -- speed (px/s), max distance (px)

-- Back-compat alias of SetCameraLag (same stateful semantics).
CameraFollowWithLag(600, 120)

-- Query / disable.
local lag = GetCameraLag()      -- → {speed, maxDistance, enabled}
DisableCameraLag()              -- Turn lag off
```

### Typical camera lag example

```lua
function OnCreate()
    -- Configure once. The engine applies lag automatically every frame.
    SetCameraLag(600, 120)
end
```

### Camera shake

```lua
ShakeCamera(5.0, 0.3)           -- intensity, duration (sec)
StopCameraShake()
local shaking = IsCameraShaking()
```

### Camera roll (rotation)

The camera can roll around the centre of its view. Degrees, **clockwise positive** — the same convention as
`SetSpriteLocalRotation` and every other rotation in the engine. `0` is the default and costs nothing: with no roll set,
every view, cull and coordinate conversion takes exactly the same path it always did.

```lua
SetCameraRotation(15)          -- roll the view 15° clockwise
local roll = GetCameraRotation()
RotateCamera(dt * 30)          -- add to the current roll

-- Split-screen / secondary cameras, addressed by entity id:
SetCameraRotationByEntity(camId, -20)
local camRoll = GetCameraRotationByEntity(camId)
```

What rolls and what does not:

| Layer | Rolls with the camera |
| ----- | --------------------- |
| Sprites, tilemaps, skeletons, particles, decals, fog of war | ✅ |
| `Draw` world-space geometry | ✅ |
| Lighting, 2D shadows, ray tracing | ✅ |
| `ScreenToWorld`, `WorldToScreen`, `GetMouseWorldPosition`, cursor traces, `IsOnScreen` | ✅ (corrected for the roll) |
| Widgets and UI | ❌ — the interface always stays upright, which is what you want |
| `Draw` screen-space geometry | ❌ — screen space is by definition unrotated |

Culling widens automatically to the bounding box of the rolled view, so nothing pops in at the corners.

> **Screen-space post effects.** Camera motion blur and the underwater world-height reference are screen-space
> approximations that only track camera *translation*; under a rolling camera they stay stable but are not
> rotation-exact. Everything that affects gameplay — picking, traces, visibility — is exact.

```lua
-- A ship game where the world turns around the player
function OnUpdate(dt)
    local heading = GetPhysicsRotation()
    SetCameraRotation(heading)          -- the hull stays upright, the sea turns
end
```

> `GetCameraWorldBounds()` returns the axis-aligned world box that the rolled view covers, plus `rotation`,
> `viewWidth` and `viewHeight` for the unrotated view rectangle. `IsOnScreen(x, y, margin)` tests the real rolled
> rectangle, not its bounding box.

### Additional

```lua
-- Camera offset (for cutscenes, etc.)
SetCameraOffset(50, 0)
local offset = GetCameraOffset()  -- → {x, y}

-- Background color
SetCameraBackgroundColor(0.1, 0.1, 0.2)
local bg = GetCameraBackgroundColor()  -- → {r, g, b}

-- World visibility bounds
local bounds = GetCameraWorldBounds()
-- → {left, right, top, bottom, width, height}

-- On-screen visibility check
local visible = IsOnScreen(worldX, worldY)
local visible = IsOnScreen(worldX, worldY, 50)  -- With padding

-- Viewport size
local vp = GetViewportSize()  -- → {width, height}

-- Near / far clipping planes
SetCameraNearPlane(0.1)
local near = GetCameraNearPlane()  -- → float (default 0.1)
SetCameraFarPlane(1000.0)
local far = GetCameraFarPlane()    -- → float (default 1000.0)
```

### Camera bounds

Sets world limits the camera cannot leave. Applied automatically for `SetCameraPosition`, `CameraFollowMe`, `CameraFollowSmooth`.

```lua
-- Set level bounds
SetCameraBounds(-500, -300, 2000, 600)  -- minX, minY, maxX, maxY

-- Camera will be constrained automatically
CameraFollowSmooth(0.1)

-- Check
if HasCameraBounds() then
    local bounds = GetCameraBounds()  -- → {enabled, minX, minY, maxX, maxY}
    Print("MinX: " .. bounds.minX)
end

-- Clear bounds
ClearCameraBounds()
```

### Split-screen camera

Per-entity functions for split-screen multiplayer. `PlayerIndex` assigns a camera to a local player slot, `ViewportRect` defines its screen region.

```lua
-- Assign player index to this entity's camera (-1 = not assigned)
SetCameraPlayerIndex(1)
local idx = GetCameraPlayerIndex()  -- → int

-- Set viewport rect (x, y, width, height in normalized 0-1 coordinates)
SetCameraViewportRect(0.5, 0.0, 0.5, 1.0)  -- Right half of screen
local vr = GetCameraViewportRect()  -- → {x, y, width, height}

-- Same but targeting another entity
SetCameraPlayerIndexByEntity(2)  -- Assign player 2 to this entity's camera
SetCameraViewportRectByEntity(0.0, 0.5, 1.0, 0.5)  -- Bottom half

-- Primary camera (the one the engine renders from). Setting one primary clears the others.
SetCameraPrimary(true)                 -- make this entity's camera the primary one
SetCameraPrimary(false)                -- demote it
local isPrimary = IsCameraPrimary()    -- → bool
-- Switch the active camera to a specific entity (great for cutscene / multi-view swaps):
SetCameraPrimaryByEntity(otherCameraId, true)
local p = GetCameraPrimaryByEntity(otherCameraId)  -- → bool

-- Get all cameras in the scene
local cameras = GetAllCameras()
-- → array of { entityId, primary, orthoWidth, playerIndex, viewportX, viewportY, viewportW, viewportH }
for _, cam in ipairs(cameras) do
    Print("Entity " .. cam.entityId .. " player=" .. cam.playerIndex)
end
```

---

## 12. Audio — Sound and Music

> **Type:** Global functions. `Audio` table.

### Initialization

```lua
local ok = Audio.Initialize()
Audio.Shutdown()
local ready = Audio.IsInitialized()
```

### Sounds (SFX)

```lua
-- Quick functions (without Audio table)
LoadSound("Content/Audio/jump.wav", "jump")
PlaySound("jump")

-- Via Audio table
Audio.LoadSound("Content/Audio/shoot.wav", "shoot")
Audio.PlaySound("shoot")
Audio.StopSound("shoot")
Audio.PauseSound("shoot")
Audio.ResumeSound("shoot")

-- Checks
local playing = Audio.IsSoundPlaying("shoot")
local finished = Audio.IsSoundFinished("shoot")
local has = Audio.HasSound("shoot")

-- Sound properties
Audio.SetSoundVolume("shoot", 0.8)
Audio.SetSoundPitch("shoot", 1.2)     -- Pitch (1.0 = normal)
Audio.SetSoundLoop("shoot", false)
Audio.SetSoundPan("shoot", -0.5)      -- Pan (-1..1: left..right)

-- Get
local vol = Audio.GetSoundVolume("shoot")
local pitch = Audio.GetSoundPitch("shoot")
local time = Audio.GetSoundCurrentTime("shoot")
local dur = Audio.GetSoundDuration("shoot")

-- Seek
Audio.SeekSound("shoot", 1.5)         -- Jump to 1.5 seconds

-- Unload
Audio.UnloadSound("shoot")
```

### Music

```lua
Audio.LoadMusic("Content/Audio/bg_music.ogg", "bgm")
Audio.PlayMusic("Content/Audio/bg_music.ogg")
Audio.StopMusic()
Audio.PauseMusic()
Audio.ResumeMusic()
Audio.SetMusicVolume(0.5)
Audio.SetMusicLoop(true)
local playing = Audio.IsMusicPlaying()
local time = Audio.GetMusicCurrentTime()
local dur = Audio.GetMusicDuration()
Audio.SeekMusic(30.0)
```

Quick functions (without the `Audio` table):

```lua
PlayMusic("Content/Audio/bg_music.ogg")
StopMusic()
```

### Spatial sound (3D Audio)

```lua
-- Load spatial sound
Audio.LoadSoundSpatial("Content/Audio/explosion.wav", "explosion", 1.0, 100.0, 1.0)
-- filePath, name, minDistance, maxDistance, rolloff

-- Source position/velocity/direction
Audio.SetSoundPosition("explosion", x, y, z)
Audio.SetSoundVelocity("explosion", vx, vy, vz)
Audio.SetSoundDirection("explosion", dx, dy, dz)
Audio.SetSoundMinDistance("explosion", 2.0)
Audio.SetSoundMaxDistance("explosion", 200.0)
Audio.SetSoundRolloff("explosion", 1.5)
Audio.SetSoundDopplerFactor("explosion", 1.0)
Audio.SetSoundCone("explosion", 90, 180, 0.3)  -- innerAngle, outerAngle, outerVolume

-- Listener (usually camera or player)
Audio.SetListenerPosition(x, y, z)
Audio.SetListenerVelocity(vx, vy, vz)
Audio.SetListenerDirection(dx, dy, dz)
Audio.SetListenerWorldUp(0, 1, 0)
```

### Sound groups

```lua
-- Group constants
Audio.GROUP_MASTER   -- 0
Audio.GROUP_MUSIC    -- 1
Audio.GROUP_SFX      -- 2
Audio.GROUP_VOICE    -- 3
Audio.GROUP_AMBIENT  -- 4
Audio.GROUP_UI       -- 5

-- Group volume
Audio.SetGroupVolume(Audio.GROUP_SFX, 0.8)
local vol = Audio.GetGroupVolume(Audio.GROUP_SFX)

-- Mute group
Audio.SetGroupMuted(Audio.GROUP_MUSIC, true)
local muted = Audio.IsGroupMuted(Audio.GROUP_MUSIC)

-- Master volume
Audio.SetMasterVolume(1.0)
local masterVol = Audio.GetMasterVolume()
Audio.SetMasterMuted(false)
local masterMuted = Audio.IsMasterMuted()
```

### Audio processing effects

```lua
-- Filters
Audio.SetSoundLowPassFilter("music", true, 5000)      -- cutoffHz
Audio.SetSoundHighPassFilter("music", true, 200)
Audio.SetSoundLoShelf("music", true, 200, -6.0)       -- freqHz, gainDB
Audio.SetSoundHiShelf("music", true, 4000, 3.0)

-- Delay (echo)
Audio.SetSoundDelay("music", true, 0.25, 0.5, 0.5, 1.0)
-- enabled, delaySec, decay, wet, dry

-- Reverb
Audio.SetSoundReverb("music", true, 0.7, 0.3, 0.5, 0.5)
-- enabled, decay, wet, roomSize, damping
```

### Fade

```lua
Audio.FadeSound("bgm", 0.0, 2.0)      -- Fade out over 2 seconds
Audio.FadeIn("bgm", 1.0)              -- Fade in over 1 second
Audio.FadeOut("bgm", 2.0)             -- Fade out over 2 seconds
```

### Control all sounds

```lua
Audio.StopAllSounds()
Audio.PauseAllSounds()
Audio.ResumeAllSounds()
```

### Entity sounds (AudioComponent)

```lua
PlayEntitySound()           -- Play first instance
PlayEntitySound(1)          -- Play second instance
StopEntitySound(0)
PauseEntitySound(0)
ResumeEntitySound(0)
local playing = IsEntitySoundPlaying(0)

SetEntitySoundVolume(0.8, 0)
local vol = GetEntitySoundVolume(0)        -- read the live channel volume
SetEntitySoundPitch(1.1, 0)
local pitch = GetEntitySoundPitch(0)       -- read the live channel pitch
SetEntitySoundLoop(true, 0)                -- loop the playing instance
SetEntitySoundPosition(0, 0, 0, 0)
FadeEntitySound(0.0, 1.0, 0)

local count = GetAudioCount()
local name = GetAudioName(0)
local path = GetAudioPath(0)

SetAudioLocalPosition(10, 5, 0)
local pos = GetAudioLocalPosition(0)
SetAudioLocalScale(1, 1, 0)
local scale = GetAudioLocalScale(0)
SetAudioLocalRotation(45, 0)
local rot = GetAudioLocalRotation(0)

-- World transform (entity transform already applied — see the Sprite section)
SetAudioWorldPosition(120, 64, 0)
local awp = GetAudioWorldPosition(0)       -- → {x, y, z}
SetAudioWorldRotation(30, 0)
local awr = GetAudioWorldRotation(0)       -- → number
local aws = GetAudioWorldScale(0)          -- → {x, y}, read-only

-- Swap the sound asset (stops and unloads the previous one for this instance)
local ok = SetAudioAsset("Content/Audio/Explosion.wav", 0)
local sndPath = GetAudioPath(0)            -- → current sound path

-- Per-instance config (stored on the AudioInstance, applied on next play)
SetAudioGroup(2, 0)                        -- bus: 0=Master,1=Music,2=SFX,3=Voice,4=Ambient,5=UI
local group = GetAudioGroup(0)             -- → int (default 2 = SFX)
SetAudioSpatial(true, 0)                   -- enable 3D positional audio for this instance
local spatial = IsAudioSpatial(0)          -- → bool
SetAudioMinDistance(1.0, 0)                -- also applied live if the instance is playing
local minD = GetAudioMinDistance(0)
SetAudioMaxDistance(100.0, 0)              -- also applied live if the instance is playing
local maxD = GetAudioMaxDistance(0)
SetAudioRolloff(1.0, 0)                    -- also applied live if the instance is playing
local rolloff = GetAudioRolloff(0)
SetAudioPlayOnWake(false, 0)               -- auto-play when the entity wakes
local playOnWake = GetAudioPlayOnWake(0)
SetAudioOverrideLoop(true, 0)              -- if true, instance Loop overrides the sound asset
local ovLoop = GetAudioOverrideLoop(0)
SetAudioOverrideSpatial(true, 0)           -- if true, instance spatial fields override the asset
local ovSpatial = GetAudioOverrideSpatial(0)
```

### Global settings

```lua
Audio.SetGlobalGain(1.0)
local gain = Audio.GetGlobalGain()
Audio.SetGlobalDopplerFactor(1.0)
local doppler = Audio.GetGlobalDopplerFactor()
Audio.SetSpeedOfSound(343.0)
local speed = Audio.GetSpeedOfSound()
Audio.SetSpatialAudioEnabled(true)
local spatial = Audio.IsSpatialAudioEnabled()
Audio.SetGlobalVolume(1.0)  -- alias for master volume
local name = Audio.GetDeviceName()
local rate = Audio.GetSampleRate()
```

---

## 13. FX — Particle Visual Effects

> **Type:** Entity-bound. Requires **FXComponent**.

### Playback

```lua
PlayFX()              -- Play first effect (resets time, clears pause)
PlayFX(1)             -- Play second effect
StopFX()              -- Stop emitting and clear existing particles
PauseFX()             -- Freeze the effect in place, keeping its particles
ResumeFX()            -- Unfreeze without time reset
ResetFX()             -- Reset time to 0

-- All effects at once
PlayAllFX()           -- Play all (resets time)
StopAllFX()           -- Stop all
PauseAllFX()          -- Freeze all in place
ResumeAllFX()         -- Unfreeze all
```

**Stop vs Pause.** `StopFX` stops emission *and* clears the particles that are already
alive. `PauseFX` freezes everything exactly as it is — the particles stay on screen and
resume from the same state on `ResumeFX`.

**End of a non-looping effect.** When an emitter reaches its Duration it stops emitting,
but the particles it already spawned keep simulating until their lifetime runs out. Only
then does `IsFXFinished` become true, and only then are non-component effects (those
started by `SpawnFXAtPosition` and friends) released. Set an emitter's Duration to 0 for
an effect that should emit forever, such as a standing pool of water.

### Checks and information

```lua
local playing = IsFXPlaying()           -- Is playing?
local finished = IsFXFinished()         -- Finished (not playing + no alive particles)?
local count = GetFXCount()              -- Number of FX instances on entity
local emitters = GetFXEmitterCount()    -- Number of runtime emitters in instance
local time = GetFXElapsedTime()         -- Elapsed time
local duration = GetFXDuration()        -- Duration from asset (emitter Duration)
local particles = GetFXParticleCount()  -- Total alive particles
```

### Name and path

```lua
local name = GetFXName()
local path = GetFXPath()
```

### Speed, loop and auto-play

```lua
SetFXSpeed(2.0)
local speed = GetFXSpeed()

SetFXLoop(true)
local loop = GetFXLoop()

SetFXPlayOnWake(true)
local pow = GetFXPlayOnWake()
```

### Visibility and render

```lua
SetFXVisible(true)
local vis = IsFXVisible()

SetFXRenderInGame(true)
local rig = GetFXRenderInGame()

SetFXCastShadow(false)
local shadow = GetFXCastShadow()

-- Don't block shadows (default true): while the global "Colliders Block Shadows" option is on, this FX still lets shadows pass through it. Turn off so it blocks shadows like terrain.
SetFXDontBlockShadows(true)
local dontBlock = GetFXDontBlockShadows()  -- → bool

SetFXShadowOrigin(1)                -- 0 = Center, 1 = Top, 2 = Bottom
local origin = GetFXShadowOrigin()  -- → int

SetFXShadowEdgeFade(0.25)
local fade = GetFXShadowEdgeFade()  -- → float [0..1]

SetFXShadowZOrder(1)                -- negative = toward background, positive = toward foreground, 0 = caster plane (default)
local zo = GetFXShadowZOrder()      -- → float

-- Optional second argument selects FX instance index (default 0).
SetFXShadowZOrder(1, 0)
```

### Render order

```lua
SetFXOrder(5)                -- Set render order (Z-depth) for entity
SetFXOrder(3, 1)             -- Set render order for specific FX instance
local order = GetFXOrder()   -- Get render order
local order = GetFXOrder(1)  -- Get render order for specific instance
```

### Flip

```lua
SetFXFlipX(true)
local flipX = GetFXFlipX()
SetFXFlipY(false)
local flipY = GetFXFlipY()
```

### Local transform

```lua
SetFXLocalPosition(10, 5)
local lp = GetFXLocalPosition()   -- → {x, y}

SetFXLocalScale(2, 2)
local ls = GetFXLocalScale()      -- → {x, y}

SetFXLocalRotation(45)
local lr = GetFXLocalRotation()

-- World transform (entity transform already applied — see the Sprite section)
SetFXWorldPosition(120, 64, 0)
local fwp = GetFXWorldPosition(0)          -- → {x, y, z}
SetFXWorldRotation(30, 0)
local fwr = GetFXWorldRotation(0)          -- → number
local fws = GetFXWorldScale(0)             -- → {x, y}, read-only

-- Swap the FX asset (destroys the current emitters, they respawn on next draw)
local ok = SetFXAsset("Content/FXs/MuzzleFlash.ice_fx", 0)
local fxPath = GetFXPath(0)                -- → current .ice_fx path
```

Muzzle flashes and impact VFX read best when placed with the world setters:

```lua
function OnLateUpdate(dt)
    local muzzle = GetSpriteAttachPointWorld("Muzzle", 1)
    if muzzle.found then
        SetFXWorldPosition(muzzle.x, muzzle.y, 0)
        SetFXWorldRotation(muzzle.rotation, 0)
    end
end
```

### Particle management

```lua
ClearFXParticles()      -- Kill all particles (spawning continues)
ClearFXParticles(1)     -- For second instance
```

### Global FX system settings

```lua
-- Global time scale (affects all FX in scene)
SetFXGlobalTimeScale(0.5)              -- Slow down 2x
local ts = GetFXGlobalTimeScale()

-- Global quality scale (particle count multiplier)
SetFXQualityScale(0.5)                 -- Halve particle count
local qs = GetFXQualityScale()
```

### FX Event Callbacks

Register callbacks that fire when particles experience specific events.  
`OnFXDeath` and `OnFXSpawn` receive particle position and velocity `(px, py, vx, vy)`.
`OnFXCollision` receives the same four values plus the id of the entity that was hit.

```lua
-- Called when a particle collides with a collider
OnFXCollision(function(px, py, vx, vy, entityId)
    -- px, py     — particle position at collision
    -- vx, vy     — particle velocity at collision
    -- entityId   — entity that owns the collider (0xFFFFFFFF when unknown)
    SpawnFXAtPosition("Content/FX/Sparks.ice_fx", px, py)
end)

-- Called when a particle dies (age >= lifetime)
OnFXDeath(function(px, py, vx, vy)
    SpawnFXAtPosition("Content/FX/Smoke.ice_fx", px, py)
end)

-- Called when a particle is spawned
OnFXSpawn(function(px, py, vx, vy)
    Print("Spawned at " .. px .. ", " .. py)
end)
```

**Optional parameters** — FX instance index and emitter index:

```lua
-- Second FX instance (index 1), all emitters
OnFXCollision(callback, 1)

-- First FX instance (index 0), second emitter (index 1)
OnFXDeath(callback, 0, 1)
```

| Function | Arguments | Description |
|---|---|---|
| `OnFXCollision(fn [, index [, emitterIdx]])` | `fn(px, py, vx, vy, entityId)` | Fires when a particle collides |
| `OnFXDeath(fn [, index [, emitterIdx]])` | `fn(px, py, vx, vy)` | Fires when a particle dies |
| `OnFXSpawn(fn [, index [, emitterIdx]])` | `fn(px, py, vx, vy)` | Fires when a particle is spawned |
| `OnFXSensorOverlap(fn [, index [, emitterIdx]])` | `fn(entityId, count, px, py)` | Fires once per overlapped entity per frame |

### Sensor overlap events — `OnFXSensorOverlap`

Enable **Sensor Overlap Events** on the emitter's Collision module, then register a callback.
Every frame, particles are tested against all sensor colliders in the scene and the result is
aggregated per entity: `count` is how many particles touch that entity this frame, `px, py` is
their average position. The event itself never applies a physical response; whether particles
also *bounce off* sensors is governed separately by **Ignore Sensors** (on by default, so
particles pass straight through). Each particle counts at most once per entity per frame.

```lua
-- Lava that damages everything it touches, per particle
OnFXSensorOverlap(function(entityId, count, px, py)
    local dmg = count * 4.0 * GetDeltaTime()
    CallInterface(entityId, "TakeDamage", dmg)
end)
```

Requires the **Collision** module on the emitter (it drives the collider cache).
The reported entity is the owner of the sensor collider, not the FX itself.

### Fluid queries — `GetFluidDensityAt`

```lua
local d = GetFluidDensityAt(x, y)   -- 0 = no fluid, ~1 = inside settled fluid
```

Samples every non-preview fluid emitter in the scene at a world point and returns the highest
normalized density (local SPH density divided by the emitter's Rest Density). Use it for
"is this point submerged" checks — drowning timers, swim state, splash triggers.

```lua
local underwater = 0.0

function OnUpdate(dt)
    local p = GetPosition()
    if GetFluidDensityAt(p.x, p.y + 24) > 0.4 then   -- head is submerged
        underwater = underwater + dt
        if underwater > 8.0 then
            Health = Health - 10 * dt                -- your own drowning logic
        end
    else
        underwater = 0.0
    end
end
```

A sleeping fluid still answers this query, still reports sensor overlaps, and still renders —
sleep only skips the simulation, never the results.

### Determinism and rollback

`FluidAffectBodies`, `OnFXSensorOverlap`, `OnFXCollision`/`OnFXDeath`/`OnFXSpawn` and
`GetFluidDensityAt` are the only parts of FX that can influence gameplay state. Everything
else is purely visual.

Particle randomness is deterministic: each emitter owns its own random stream, seeded from its
creation index within the scene, and every draw is portable across platforms. Two runs of the
same scene, and two machines running the same build, produce identical particles.

FX is nevertheless **not part of rollback state** — `Rollback.*` does not save, restore or
re-simulate particles, by design (rolling back thousands of particles would cost more than it is
worth). So if you use deterministic rollback multiplayer, keep the four gameplay-affecting
features above out of anything that feeds the rollback checksum, or drive that logic from a
regular Box2D sensor instead. Server-authoritative `NetworkReplication` is unaffected.

### Simulation Mode and fluids

An emitter's **Simulation Mode** (CPU / GPU) applies to the general per-particle update. The
**Fluid** module always solves on the CPU regardless of this setting — its spatial-grid solver
is faster than a GPU pass would be for the particle counts fluids run at.

### Fluid sleep

With **Allow Sleep** on (the default), a fluid stops simulating once every particle has been
slower than **Sleep Speed** for about half a second, and resumes by itself. It wakes when any
of these happens: a particle is spawned or dies, the emitter moves, its parameters are rebuilt
or written from Lua, the set of colliders in the scene changes, a moving body's collider comes
near the fluid, or an awake interacting fluid comes near it. Sleep is disabled automatically
while a force module is present (Wind, Vortex, Attractor, Curl Noise, Noise, Spring Force,
Particle Collision, Conditional Module, Custom Script, Size/Color By Speed), because those
would keep changing the state.

Scenes that constantly create and destroy colliders keep waking every fluid; set
`SetFXEmitterFlag("FluidAllowSleep", false)` if you would rather pay the simulation cost than
have sleep toggle every frame.

### Fluid-to-fluid interaction

Emitters with **Interact With Other Fluids** enabled push against each other's particles, so
water and lava keep a boundary instead of passing through. The push is weighted by the mass
ratio, so the fluid with the lower **Particle Mass** is pushed harder and ends up floating on
top of the heavier one. Emitters without the flag are invisible to each other, exactly as
before.

```lua
SetFXEmitterFlag("FluidInteract", true)
SetFXEmitterParam("FluidInteractStrength", 60.0)
```

`WakeFXFluid([index])` forces a sleeping fluid awake. You need it only in one case: a body
teleported into settled fluid with no velocity, which none of the automatic conditions can
see. Omit the index to wake every instance on the entity.

```lua
SetPosition(poolX, poolY)
WakeFXFluid()          -- on the entity that owns the water FX
```

### Emitter parameters — `SetFXEmitterParam` / `GetFXEmitterParam`

Read and write `CachedEmitterParams` at runtime by string name.

```lua
SetFXEmitterParam("paramName", value)          -- First instance
SetFXEmitterParam("paramName", value, 1)       -- Second instance
local val = GetFXEmitterParam("paramName")     -- Read (first emitter)
local val = GetFXEmitterParam("paramName", 1)  -- Second instance
```

#### Full list of supported parameters:

**Spawn:**
| Parameter | Type | Description |
|---|---|---|
| `"SpawnRate"` | float | Spawn rate (particles/sec) |
| `"ShapeRadius"` | float | Spawn shape radius |
| `"ShapeInnerRadius"` | float | Inner radius (Ring) |
| `"ShapeSize.X"` | float | Spawn shape size on X |
| `"ShapeSize.Y"` | float | Spawn shape size on Y |
| `"SpawnPerUnitDistance"` | float | Distance for Spawn Per Unit |
| `"SpawnPerUnitCount"` | int→float | Count for Spawn Per Unit |
| `"SpawnTextureThreshold"` | float | Brightness threshold for Spawn From Texture |

**Init:**
| Parameter | Type | Description |
|---|---|---|
| `"LifetimeMin"` | float | Minimum lifetime |
| `"LifetimeMax"` | float | Maximum lifetime |
| `"VelocityMin.X"` | float | Minimum velocity X |
| `"VelocityMin.Y"` | float | Minimum velocity Y |
| `"VelocityMin.Z"` | float | Minimum velocity Z |
| `"VelocityMax.X"` | float | Maximum velocity X |
| `"VelocityMax.Y"` | float | Maximum velocity Y |
| `"VelocityMax.Z"` | float | Maximum velocity Z |
| `"InheritEmitterVelocityScale"` | float | Emitter velocity inheritance multiplier |
| `"SizeMin"` | float | Minimum size (uniform) |
| `"SizeMax"` | float | Maximum size (uniform) |
| `"SizeMinXY.X"` | float | Minimum size X (non-uniform) |
| `"SizeMinXY.Y"` | float | Minimum size Y (non-uniform) |
| `"SizeMaxXY.X"` | float | Maximum size X (non-uniform) |
| `"SizeMaxXY.Y"` | float | Maximum size Y (non-uniform) |
| `"ColorMin.R"` | float | Minimum color — red (0..1) |
| `"ColorMin.G"` | float | Minimum color — green |
| `"ColorMin.B"` | float | Minimum color — blue |
| `"ColorMin.A"` | float | Minimum color — alpha |
| `"ColorMax.R"` | float | Maximum color — red |
| `"ColorMax.G"` | float | Maximum color — green |
| `"ColorMax.B"` | float | Maximum color — blue |
| `"ColorMax.A"` | float | Maximum color — alpha |
| `"RotationMin"` | float | Minimum initial angle (degrees) |
| `"RotationMax"` | float | Maximum initial angle |
| `"RotationSpeedMin"` | float | Minimum rotation speed |
| `"RotationSpeedMax"` | float | Maximum rotation speed |

**Forces:**
| Parameter | Type | Description |
|---|---|---|
| `"Gravity.X"` | float | Gravity X |
| `"Gravity.Y"` | float | Gravity Y |
| `"Gravity.Z"` | float | Gravity Z |
| `"Drag"` | float | Drag coefficient |
| `"Acceleration.X"` | float | Constant acceleration X |
| `"Acceleration.Y"` | float | Constant acceleration Y |
| `"Acceleration.Z"` | float | Constant acceleration Z |
| `"OrbitSpeed"` | float | Orbital movement speed |
| `"OrbitRadius"` | float | Orbit radius |
| `"NoiseStrength"` | float | Noise strength |
| `"NoiseFrequency"` | float | Noise frequency |

**Curl Noise:**
| Parameter | Type | Description |
|---|---|---|
| `"CurlNoiseStrength"` | float | Curl noise strength |
| `"CurlNoiseFrequency"` | float | Curl noise frequency |
| `"CurlNoiseSpeed"` | float | Curl noise animation speed |
| `"CurlNoiseOctaves"` | int→float | Octaves |
| `"CurlNoiseLacunarity"` | float | Lacunarity |
| `"CurlNoisePersistence"` | float | Persistence |

**Vortex:**
| Parameter | Type | Description |
|---|---|---|
| `"VortexCenter.X"` | float | Vortex center X |
| `"VortexCenter.Y"` | float | Vortex center Y |
| `"VortexCenter.Z"` | float | Vortex center Z |
| `"VortexStrength"` | float | Vortex strength |
| `"VortexRadius"` | float | Vortex radius |
| `"VortexFalloff"` | float | Vortex falloff |
| `"VortexInwardPull"` | float | Pull strength toward the center |

**Wind:**
| Parameter | Type | Description |
|---|---|---|
| `"WindDirection.X"` | float | Wind direction X |
| `"WindDirection.Y"` | float | Wind direction Y |
| `"WindStrength"` | float | Wind strength |
| `"WindTurbulence"` | float | Turbulence |
| `"WindTurbulenceFrequency"` | float | Turbulence frequency |
| `"WindGustStrength"` | float | Gust strength |
| `"WindGustFrequency"` | float | Gust frequency |
| `"WindDrag"` | float | Wind drag |

**Attractor:**
| Parameter | Type | Description |
|---|---|---|
| `"AttractorPosition.X"` | float | Attractor position X |
| `"AttractorPosition.Y"` | float | Attractor position Y |
| `"AttractorPosition.Z"` | float | Attractor position Z |
| `"AttractorStrength"` | float | Attraction strength |
| `"AttractorRadius"` | float | Effect radius |
| `"AttractorFalloff"` | float | Falloff |

**Collision:**
| Parameter | Type | Description |
|---|---|---|
| `"CollisionBounceFactor"` | float | Bounce coefficient (0..1) |
| `"CollisionFriction"` | float | Friction on collision |
| `"CollisionRadiusScale"` | float | Collision radius scale |

**Light:**
| Parameter | Type | Description |
|---|---|---|
| `"LightColor.R"` | float | Light color — red |
| `"LightColor.G"` | float | Light color — green |
| `"LightColor.B"` | float | Light color — blue |
| `"LightIntensity"` | float | Intensity |
| `"LightRadius"` | float | Radius |
| `"LightFalloff"` | float | Falloff |

**Size/Color By Speed:**
| Parameter | Type | Description |
|---|---|---|
| `"SizeBySpeedMin"` | float | Minimum size multiplier by speed |
| `"SizeBySpeedMax"` | float | Maximum size multiplier by speed |
| `"SizeBySpeedRange"` | float | Speed range |
| `"ColorBySpeedRange"` | float | Speed range for color |

**Scale By Density:**
| Parameter | Type | Description |
|---|---|---|
| `"ScaleByDensityMin"` | float | Minimum scale by density |
| `"ScaleByDensityMax"` | float | Maximum scale by density |

**Spring:**
| Parameter | Type | Description |
|---|---|---|
| `"SpringStiffness"` | float | Spring stiffness |
| `"SpringDamping"` | float | Spring damping |

**Particle Collision:**
| Parameter | Type | Description |
|---|---|---|
| `"ParticleCollisionRadius"` | float | Collision radius |
| `"ParticleCollisionBounce"` | float | Bounce |
| `"ParticleCollisionRepulsion"` | float | Repulsion strength |

**Stretch / Animation / Ribbon:**
| Parameter | Type | Description |
|---|---|---|
| `"StretchVelocityScale"` | float | Stretch scale by speed |
| `"SpriteAnimationSpeed"` | float | Sprite animation speed |
| `"RibbonWidth"` | float | Ribbon width |
| `"RibbonMinVertexDistance"` | float | Minimum distance between ribbon segments |

**Kill Condition:**
| Parameter | Type | Description |
|---|---|---|
| `"KillZoneCenter.X"` | float | Zone center X |
| `"KillZoneCenter.Y"` | float | Zone center Y |
| `"KillZoneCenter.Z"` | float | Zone center Z |
| `"KillZoneSize.X"` | float | Zone size X |
| `"KillZoneSize.Y"` | float | Zone size Y |
| `"KillZoneRadius"` | float | Zone radius (for Sphere) |
| `"KillSpeedThreshold"` | float | Speed threshold |

**Fluid:**
| Parameter | Type | Description |
|---|---|---|
| `"FluidRestDensity"` | float | Rest density |
| `"FluidStiffness"` | float | Stiffness |
| `"FluidViscosity"` | float | Viscosity |
| `"FluidSurfaceTension"` | float | Surface tension |
| `"FluidInteractionRadius"` | float | Interaction radius |
| `"FluidParticleMass"` | float | Particle mass |
| `"FluidNearStiffness"` | float | Near-field stiffness |
| `"FluidDamping"` | float | Damping |
| `"FluidMaxSpeed"` | float | Maximum speed |
| `"FluidCollisionDamping"` | float | Collision damping |
| `"FluidSubSteps"` | int→float | Number of substeps |
| `"FluidMetaballThreshold"` | float | Metaball threshold |
| `"FluidMetaballScale"` | float | Metaball scale |
| `"FluidBodyPushScale"` | float | Momentum transferred to rigid bodies |
| `"FluidBodyDrag"` | float | Viscous drag on submerged bodies (1/sec) |
| `"FluidSleepSpeed"` | float | Speed below which the fluid may sleep |
| `"FluidInteractStrength"` | float | Separation force against other fluids |
| `"FluidInteractViscosity"` | float | Drag between different fluids |

**Conditional / Event:**
| Parameter | Type | Description |
|---|---|---|
| `"CondModuleThreshold"` | float | Condition threshold |
| `"CondModuleValueTrue"` | float | Value when true |
| `"CondModuleValueFalse"` | float | Value when false |
| `"EventSpawnCount"` | int→float | Spawn count on event |
| `"EventInheritVelocityScale"` | float | Velocity inheritance multiplier |

### Modules, flags & modes — runtime structural control

`SetFXEmitterParam` only reaches scalar (`float`/`int`) fields. The functions below cover
everything else on `CachedEmitterParams` at runtime — enabling/disabling whole modules,
boolean flags, and enum/int "mode" fields — applied live to every emitter of the instance
(no re-authoring needed). All accept an optional trailing instance index (defaults to `0`).

```lua
-- Enable / disable a whole module (toggles its runtime gate)
SetFXModuleEnabled("Collision", true)
SetFXModuleEnabled("Light", false, 1)          -- second instance
local on = IsFXModuleEnabled("Fluid")

-- Boolean parameters
SetFXEmitterFlag("RandomColor", true)
SetFXEmitterFlag("AlignToVelocity", true)
local immortal = GetFXEmitterFlag("Immortal")

-- Enum / int "mode" parameters (set by integer value)
SetFXEmitterMode("Shape", 1)                   -- 0=Point,1=Circle,2=Box,3=Edge,4=Ring
SetFXEmitterMode("BlendMode", 1)               -- 0=Alpha,1=Additive,2=Multiply
local blend = GetFXEmitterMode("BlendMode")
```

**Toggleable modules (`SetFXModuleEnabled` / `IsFXModuleEnabled`):**
`"SpawnRate"`, `"SpawnPerUnit"`, `"VelocityOverLife"`, `"SizeOverLife"`, `"ColorOverLife"`,
`"OpacityOverLife"`, `"StretchOverLife"`, `"SubUVOverLife"`, `"CurlNoise"`, `"VortexForce"`,
`"WindForce"`, `"SizeBySpeed"`, `"ColorBySpeed"`, `"ScaleByDensity"`, `"SpringForce"`,
`"ParticleCollision"`, `"Attractor"`, `"Collision"`, `"Light"`, `"Fluid"`, `"KillCondition"`,
`"EventHandler"`, `"RibbonRenderer"`, `"ConditionalModule"`, `"CustomScript"`.

**Boolean flags (`SetFXEmitterFlag` / `GetFXEmitterFlag`):**
`"Immortal"`, `"VelocityInLocalSpace"`, `"UniformSize"`, `"RandomColor"`, `"AlignToVelocity"`,
`"SpawnOnEdge"`, `"RandomDirection"`, `"LightInheritParticleColor"`, `"LightCastShadows"`,
`"CollisionKillOnSecondHit"`, `"CollisionIgnoreSensors"`, `"CollisionSensorEvents"`,
`"FluidPreFill"`, `"FluidAffectBodies"`, `"FluidAllowSleep"`, `"FluidInteract"`, `"AnimateSprite"`,
`"UseCustomMaterial"`, `"InheritEmitterVelocity"`, `"VortexRelativeToEmitter"`,
`"AttractorRelativeToEmitter"`, `"KillZoneRelativeToEmitter"`, `"EventInheritVelocity"`.

**Enum / int modes (`SetFXEmitterMode` / `GetFXEmitterMode`):**
| Name | Values |
|---|---|
| `"Shape"` | 0=Point, 1=Circle, 2=Box, 3=Edge, 4=Ring |
| `"CollisionResponse"` | 0=None, 1=Destroy, 2=Bounce, 3=Slide |
| `"BlendMode"` | 0=Alpha, 1=Additive, 2=Multiply |
| `"FluidRenderMode"` | 0=Particles, 1=Metaball |
| `"KillCondType"` | 0=Box, 1=Sphere, 2=SpeedMin, 3=SpeedMax |
| `"KillBoxMode"` | 0=Inside, 1=Outside, 2=Contain |
| `"EventTrigger"` | 0=OnCollision, 1=OnDeath, 2=OnSpawn |
| `"ShadingMode"` | shading mode index |
| `"SpriteSubImageH"` / `"SpriteSubImageV"` | Sub-UV grid columns / rows |
| `"RibbonMaxSegments"` | Max ribbon segments |
| `"RibbonTextureMode"` | Ribbon texture mode |
| `"FluidPreFillCount"` | Fluid pre-fill particle count |
| `"CondModuleSourceType"` / `"CondModuleComparison"` | Conditional source / comparison |
| `"CondModuleActionTrue"` / `"CondModuleActionFalse"` | Conditional actions |

> **Note:** these edit the live runtime cache of the effect instance. They do not modify the
> `.ice_fx` asset on disk and are reset if the emitter's parameters are re-cached (e.g. after
> an asset reload). Enabling a module that was not authored turns it on with default values,
> which you can then tune via `SetFXEmitterParam` / `SetFXEmitterFlag` / `SetFXEmitterMode`.

### Example: complex usage

```lua
function OnStart()
    PlayFX()
    SetFXLoop(true)
    SetFXSpeed(1.5)
    SetFXEmitterParam("SpawnRate", 50)
    SetFXEmitterParam("Gravity.Y", -200)
    SetFXEmitterParam("WindStrength", 80)
    SetFXEmitterParam("WindDirection.X", 1)
end

function OnUpdate(dt)
    -- Dynamic parameter changes
    local rate = GetFXEmitterParam("SpawnRate")
    if GetFXParticleCount() > 500 then
        SetFXEmitterParam("SpawnRate", rate * 0.5)
    end

    -- Check completion of a one-shot effect
    if IsFXFinished(1) then
        PlayFX(1)
    end
end
```

---

## 13.1. Point Light — Point light sources

> **Type:** Entity-bound. Requires **PointLightComponent**.

### Basic properties

```lua
-- Light count
local count = GetLightCount()

-- Enable/disable light
SetLightEnabled(true)
SetLightEnabled(false, 1)       -- Second light
local on = IsLightEnabled()
ToggleLight()                    -- Toggle

-- Enable/disable all at once
SetAllLightsEnabled(true)
```

### Color, intensity, radius

```lua
SetLightColor(1.0, 0.8, 0.3)           -- Warm light
local c = GetLightColor()               -- → {r, g, b}

SetLightIntensity(2.0)
local i = GetLightIntensity()

SetLightRadius(200.0)
local r = GetLightRadius()

SetLightFalloff(2.0)                    -- Falloff exponent
local f = GetLightFalloff()

-- Batch set (color + intensity + radius)
SetLight(1, 0.9, 0.7, 2.0, 150)
```

### Position and transform

```lua
SetLightPosition(100, 200)
local pos = GetLightPosition()          -- → {x, y}

SetLightLocalScale(2, 2)
local ls = GetLightLocalScale()         -- → {x, y}

SetLightLocalRotation(45)
local lr = GetLightLocalRotation()

-- World transform (entity transform already applied — see the Sprite section).
-- Note: the local position accessors are named SetLightPosition/GetLightPosition
-- and SetSpotLightPosition/GetSpotLightPosition for historical reasons.
SetLightWorldPosition(120, 64, 0)
local lwp = GetLightWorldPosition(0)         -- → {x, y, z}
SetLightWorldRotation(30, 0)
local lwr = GetLightWorldRotation(0)         -- → number
local lws = GetLightWorldScale(0)            -- → {x, y}, read-only

SetSpotLightWorldPosition(120, 64, 0)
local swp = GetSpotLightWorldPosition(0)     -- → {x, y, z}
SetSpotLightWorldRotation(30, 0)
local swr = GetSpotLightWorldRotation(0)     -- → number
local sws = GetSpotLightWorldScale(0)        -- → {x, y}, read-only
```

### Shadows and visibility

```lua
SetLightCastShadows(true)
local shadows = GetLightCastShadows()

-- SetLightVisible / GetLightVisible control Enabled (backward compatibility)
SetLightVisible(true)                   -- same as SetLightEnabled(true)
local vis = GetLightVisible()           -- same as IsLightEnabled()
```

---

## 13.2. Spot Light — Directed cone light

> **Type:** Entity-bound. Requires **SpotLightComponent**.

```lua
-- Count
local count = GetSpotLightCount()

-- Enable
SetSpotLightEnabled(true)
local on = IsSpotLightEnabled()
ToggleSpotLight()                    -- Toggle
SetAllSpotLightsEnabled(true)        -- Enable/disable all at once

-- Color and intensity
SetSpotLightColor(1, 1, 0.9)
local c = GetSpotLightColor()           -- → {r, g, b}
SetSpotLightIntensity(3.0)
local i = GetSpotLightIntensity()

-- Radius
SetSpotLightRadius(300)
local r = GetSpotLightRadius()

-- Direction
SetSpotLightDirection(0, -1)
local dir = GetSpotLightDirection()     -- → {x, y}

-- Cone angles (inner and outer, 1°–90°)
SetSpotLightAngles(15, 30)
local a = GetSpotLightAngles()          -- → {inner, outer}

-- Falloff
SetSpotLightFalloff(2.0)
local f = GetSpotLightFalloff()

-- Position and transform
SetSpotLightPosition(100, 200)
local pos = GetSpotLightPosition()      -- → {x, y}

SetSpotLightLocalScale(2, 2)
local ls = GetSpotLightLocalScale()     -- → {x, y}

SetSpotLightLocalRotation(45)
local lr = GetSpotLightLocalRotation()

-- Shadows and visibility
SetSpotLightCastShadows(true)
local sh = GetSpotLightCastShadows()

-- SetSpotLightVisible / GetSpotLightVisible control Enabled (backward compatibility)
SetSpotLightVisible(true)               -- same as SetSpotLightEnabled(true)
local vis = GetSpotLightVisible()       -- same as IsSpotLightEnabled()

-- Batch set (color r,g,b + intensity + radius + dirX,dirY + innerAngle + outerAngle)
SetSpotLight(1, 0.9, 0.7, 2.0, 300, 0, -1, 15, 30)
```

### Light Cookies (texture projectors)

**PointLight cookies** (operate on the currently selected PointLight):

```lua
SetLightCookie("Content/cookies/iris.png")
local p = GetLightCookie()              -- texture path

SetLightCookieIntensity(0.75)           -- 0..4, default 1.0
local i = GetLightCookieIntensity()

SetLightCookieRotation(45.0)            -- degrees
local r = GetLightCookieRotation()
```

**SpotLight cookies**:

```lua
SetSpotLightCookie("Content/cookies/gobo.png", 0)
local p = GetSpotLightCookie(0)

SetSpotLightCookieIntensity(1.5, 0)
local sci = GetSpotLightCookieIntensity(0)
SetSpotLightCookieRotation(90.0, 0)
local scr = GetSpotLightCookieRotation(0)
```

All Set/Get accept an optional trailing `index` parameter (defaults to 0) to address
specific lights when the entity has multiple PointLights or SpotLights.

---

## 13.3. Lighting and Shadows — Global settings

> **Type:** Global functions (not bound to entity)

> **Where the starting values come from.** Every setter in this section overrides a
> **project default** authored in `Config/Engine.json` → `Rendering` (editor:
> [Preferences → Rendering](Editor-EN-DOC.md#105-rendering)), optionally replaced for one
> level by [World Settings](Editor-EN-DOC.md#8-world-settings). The runtime reads
> `Engine.json` at startup on all six platforms, so an untouched game looks the same in the
> editor viewport, in Play mode and in the shipped build.
>
> Calling a setter marks **that one parameter** as game-controlled: it keeps the value you
> gave it across level loads, while every parameter you did not touch keeps following the
> project default and the level override. `Settings.Save()` persists the game-controlled
> parameters to `GameSettings.json` (a `Rendering` block holding only those keys) and they
> are re-applied at the next startup; `Settings.ResetDefaults()` drops them and returns
> everything to `Engine.json`. This is what an in-game "Lighting / Shadows" options screen
> should rely on — no extra preference file of your own is needed.

### Lighting mode

```lua
SetLightingMode("Lit")                  -- Enable lighting
SetLightingMode("Unlit")                -- Disable
local mode = GetLightingMode()          -- → "Lit" or "Unlit"
```

### Ambient (ambient light)

```lua
SetAmbientLight(0.1, 0.1, 0.15)        -- Color (RGB)
SetAmbientLight(0.1, 0.1, 0.15, 0.2)   -- Color + intensity
SetAmbientIntensity(0.1)
local ai = GetAmbientIntensity()
local ac = GetAmbientColor()            -- → {r, g, b}
```

### Directional Light (sun)

```lua
-- dirX, dirY, r, g, b, intensity [, enabled [, castShadows]]
SetDirectionalLight(0.5, -1, 1, 1, 0.9, 0.5)
SetDirectionalLight(0.5, -1, 1, 1, 0.9, 0.5, true)
SetDirectionalLight(0.5, -1, 1, 1, 0.9, 0.5, true, true) -- with shadows

SetDirectionalLightEnabled(true)
local on = IsDirectionalLightEnabled()

SetDirectionalLightCastShadows(true)
local cs = IsDirectionalLightCastShadows()

local dl = GetDirectionalLight()
-- → {dirX, dirY, r, g, b, intensity, enabled, castShadows}
```

### Shadows

```lua
SetShadowsEnabled(true)
local on = AreShadowsEnabled()

-- Quality: 0=Off, 1=Low, 2=Medium, 3=High, 4=Ultra
SetShadowQuality(3)
local q = GetShadowQuality()

SetShadowSoftness(0.5)                 -- 0.0–1.0
local s = GetShadowSoftness()

SetShadowIntensity(1.0)                -- 0.0–1.0
local i = GetShadowIntensity()

SetShadowBias(2.0, 0.0)                -- (x, y) world-space offset, [-1000, 1000]
local b = GetShadowBias()              -- → { x, y }

SetShadowPCFSamples(5)                 -- 1–7
local p = GetShadowPCFSamples()

-- Directional (sun) shadow throw distance in world units; 0 = unlimited.
-- Fades the shadow out after this distance so tall/high casters don't streak
-- their shadow across the level through platforms.
SetShadowDirectionalLength(256.0)
local dl = GetShadowDirectionalLength()

-- Directional shadow Z-depth fade in layer units; 0 = off. Fades the shadow as
-- the receiver gets further behind the caster in Z (keeps shadows off far layers).
SetShadowDirectionalDepthFade(3.0)
local df = GetShadowDirectionalDepthFade()

-- When true, every solid collider blocks shadows: a caster's shadow stops at the
-- next collider instead of passing through it (hard, realistic occlusion; costs more).
SetCollidersBlockShadows(true)
local cb = GetCollidersBlockShadows()
```

### Ray Tracing (Vulkan / Direct3D 12 / Metal only)

```lua
-- Real-time 2D ray-traced global illumination (soft indirect light, color
-- bleeding, ray-traced ambient occlusion). Works ONLY on Vulkan devices with
-- hardware ray tracing support, on Direct3D 12 adapters with DirectX
-- Raytracing 1.1, and on Metal devices that report supportsRaytracing
-- (MSL 2.4 ray queries); on any other backend/device the calls are
-- remembered but produce no visual effect (the feature behaves as if absent).
local supported = IsRaytracingSupported()   -- true only on RT-capable Vulkan / D3D12 / Metal

SetRaytracingEnabled(true)
local on = IsRaytracingEnabled()

-- Quality: 0=Low, 1=Medium, 2=High, 3=Ultra (rays per pixel + internal resolution)
SetRaytracingQuality(2)
local q = GetRaytracingQuality()

SetRaytracingIntensity(1.0)            -- overall GI strength (0.0–8.0)
local i = GetRaytracingIntensity()

SetRaytracingBounce(1.0)               -- bounced-light / color-bleed strength (0.0–4.0)
local b = GetRaytracingBounce()

SetRaytracingMaxBounces(2)             -- light bounces / path depth, multi-bounce GI (1–8)
local mb = GetRaytracingMaxBounces()

SetRaytracingReflection(0.0)           -- specular reflection strength (0 = diffuse GI, 1 = mirror)
local rf = GetRaytracingReflection()

SetRaytracingMaxDistance(512.0)        -- max ray travel distance in world units
local d = GetRaytracingMaxDistance()

SetRaytracingDenoise(true)             -- temporal + spatial denoising for stable lighting
local dn = GetRaytracingDenoise()

SetRaytracingShadows(true)             -- shadow rays: lighting blocked by occluders
local sh = GetRaytracingShadows()

-- Ray-traced ambient occlusion: contact darkening where geometry meets (0.0–1.0, 0 = off)
SetRaytracingAOIntensity(0.65)
local ao = GetRaytracingAOIntensity()

SetRaytracingAORadius(96.0)            -- AO reach in world units (1.0–8192.0)
local aor = GetRaytracingAORadius()

-- How indirect light is applied: 0 = added on top of the image,
-- 1 = multiplied by the surface colour so bounced light behaves like real light
SetRaytracingAlbedoResponse(0.75)
local ar = GetRaytracingAlbedoResponse()

-- Sky light picked up by rays that escape the scene; multiplies the ambient
-- colour and adds to the ambient intensity (0.0–8.0)
SetRaytracingSkyIntensity(1.0)
local sk = GetRaytracingSkyIntensity()

-- Denoiser sharpness: higher = crisper contact shadows but more noise (0.0–1.0)
SetRaytracingSharpness(0.5)
local sp = GetRaytracingSharpness()

-- Bounced light takes its colour from the rendered scene (full-detail colour
-- bleeding and emissive glow); falls back to analytic lighting off-screen
SetRaytracingScreenRadiance(true)
local sr = GetRaytracingScreenRadiance()
```

### Clear color (background)

```lua
SetClearColor(0.1, 0.1, 0.15)          -- RGB (0.0–1.0)
local c = GetClearColor()              -- → {r, g, b}
```

> `SetClearColor` enables the scene's World Override (see section 13.4), so the value persists per-level and survives Play/Stop. Use `ResetWorldOverride()` to revert to project defaults.

### Back to the project defaults

Every setter above pins one parameter as game-controlled for the rest of the session.
These three hand a parameter back, so it follows `Config/Engine.json` → `Rendering` and the
level's World Override again:

```lua
-- Hand one parameter back to the project default. Returns false (and logs a warning)
-- for an unknown name. Takes effect immediately, including the level override if the
-- current level has one.
ResetRenderSetting("ShadowsEnabled")
ResetRenderSetting("CollidersBlockShadows")

-- Hand back everything this session changed - lighting mode, ambient, shadows,
-- ray tracing and the directional light - without touching resolution, audio or
-- the other Settings.* values (that is what Settings.ResetDefaults() does).
ResetAllRenderSettings()

-- True while the game controls this parameter, false while it follows the project
-- default / level override. Useful for a "using project default" hint in a menu.
local pinned = IsRenderSettingOverridden("ShadowsEnabled")
```

The name is the `Config/Engine.json` → `Rendering` key, so both files read the same:

| | | | |
| --- | --- | --- | --- |
| `LightingMode` | `AmbientColor` | `AmbientIntensity` | `ShadowsEnabled` |
| `ShadowRayQuality` | `ShadowSoftness` | `ShadowIntensity` | `ShadowBias` |
| `ShadowPCFSamples` | `ShadowDirectionalLength` | `ShadowDirectionalDepthFade` | `CollidersBlockShadows` |
| `RaytracingEnabled` | `RaytracingQuality` | `RaytracingIntensity` | `RaytracingBounce` |
| `RaytracingMaxBounces` | `RaytracingReflection` | `RaytracingMaxDistance` | `RaytracingDenoise` |
| `RaytracingShadows` | `RaytracingAOIntensity` | `RaytracingAORadius` | `RaytracingAlbedoResponse` |
| `RaytracingSkyIntensity` | `RaytracingSharpness` | `RaytracingScreenRadiance` | `DirLightDirX` |
| `DirLightColor` | `DirLightIntensity` | `DirLightEnabled` | `DirLightCastShadows` |

`ShadowQuality` is accepted as an alias of `ShadowRayQuality` (it is the name of the Lua
setter), and `DirLightDirY` as an alias of `DirLightDirX` — the light direction is one
parameter. A reset is not itself persisted: call `Settings.Save()` to drop the parameter
from `GameSettings.json` too.

---

## 13.4. World Override — Persistent Level Override (Physics + Rendering)

> **Type:** Global functions. Operate on the active scene's **World Override** — a per-level snapshot stored inside the `.icemap` and applied automatically on level load (both editor and runtime builds).
>
> Use this when you want a Lua script to permanently change physics or rendering parameters for the current level. Changes apply live (the Box2D world is reconfigured, lighting is updated, near/far/clear-color take effect on the next frame).

### Reset and read

```lua
ResetWorldOverride()                   -- Disable override and restore defaults
local data = GetWorldOverride()
-- data.physics  = { enabled, ppm, gravity_x, gravity_y, sub_step_count,
--                   fixed_timestep, restitution_threshold, hit_event_threshold,
--                   contact_hertz, contact_damping_ratio, max_contact_push_speed,
--                   maximum_linear_speed, enable_sleep, enable_continuous }
-- data.rendering = { enabled, near_plane, far_plane, lighting_mode,
--                    ambient_color={r,g,b}, ambient_intensity,
--                    clear_color={r,g,b},
--                    shadows_enabled, shadow_quality, shadow_softness,
--                    shadow_intensity, shadow_bias={x,y}, shadow_pcf_samples,
--                    raytracing_enabled, raytracing_quality, raytracing_intensity,
--                    raytracing_bounce, raytracing_max_bounces, raytracing_reflection,
--                    raytracing_max_distance, raytracing_denoise, raytracing_shadows,
--                    raytracing_ao_intensity, raytracing_ao_radius,
--                    raytracing_albedo_response, raytracing_sky_intensity,
--                    raytracing_sharpness, raytracing_screen_radiance,
--                    dir_light_enabled, dir_light_cast_shadows,
--                    dir_light_dir_x, dir_light_dir_y,
--                    dir_light_color={r,g,b}, dir_light_intensity }
```

### Apply override (any subset of fields)

```lua
ApplyWorldOverride({
    physics = {
        gravity_x = 0,
        gravity_y = -20,
        sub_step_count = 8,
        enable_sleep = true,
        enable_continuous = true,
    },
    rendering = {
        clear_color  = { r = 0.05, g = 0.02, b = 0.10 },
        ambient_color = { r = 0.2, g = 0.1, b = 0.3 },
        ambient_intensity = 0.4,
        lighting_mode = 1,            -- 0=Unlit, 1=Lit
        near_plane = 0.1,
        far_plane = 2000,
        shadows_enabled = true,
        shadow_quality = 3,
        dir_light_enabled = true,
        dir_light_dir_x = 0.3,
        dir_light_dir_y = -1,
        dir_light_color = { r = 1, g = 0.95, b = 0.85 },
        dir_light_intensity = 0.8,
        dir_light_cast_shadows = true,
    },
})
```

> Both `physics` and `rendering` tables are optional and any field inside is optional too. Only listed fields are written; the rest keep their current values. Calling `ApplyWorldOverride` automatically enables override mode for whichever section is provided. Physics changes are pushed into the live Box2D world; rendering changes are pushed into the lighting system and used for the next frame's projection and clear color.

---

## 14. Collision — Collisions (AABB)

> **Type:** Entity-bound. Requires **ColliderComponent**.
> This is *not* Box2D physics collisions, but AABB intersection checks (rectangles).

```lua
-- Overlapping tag?
if IsOverlappingTag("Coin") then CollectCoin() end
-- Tags support prefixes: "Enemy" matches "EnemyBoss"

-- Get overlapping entity
local coin = GetOverlappingEntity("Coin")  -- First found (or nil)

-- All overlapping
local coins = GetAllOverlappingEntities("Coin")

-- Overlap count
local count = GetOverlappingCount("Enemy")

-- Overlap with specific entity
if IsOverlappingEntity(targetId) then ... end

-- Collision side
local side = GetCollisionSide("Ground")
-- → {left=false, right=false, top=false, bottom=true}
if side.bottom then onGround = true end

-- Near a tag? (distance-based, no collider)
if IsNearTag("Enemy", 200) then Alert() end

-- Closest by tag
local nearest = GetNearestByTag("Enemy")

-- Distance to nearest by tag
local dist = DistanceToTag("Checkpoint")
```

### Collider properties

#### Shadow casting

```lua
-- Box collider
SetBoxColliderCastShadow(true)
local shadow = GetBoxColliderCastShadow()  -- → bool

-- Sphere collider
SetSphereColliderCastShadow(true)
local shadow = GetSphereColliderCastShadow()

-- Capsule collider
SetCapsuleColliderCastShadow(true)
local shadow = GetCapsuleColliderCastShadow()

-- Don't block shadows (default true): while the global "Colliders Block Shadows" option is on,
-- these colliders still let shadows pass through them. Turn off so the collider blocks shadows like terrain.
SetBoxColliderDontBlockShadows(true)
local boxDontBlock = GetBoxColliderDontBlockShadows()      -- → bool
SetSphereColliderDontBlockShadows(true)
local sphDontBlock = GetSphereColliderDontBlockShadows()   -- → bool
SetCapsuleColliderDontBlockShadows(true)
local capDontBlock = GetCapsuleColliderDontBlockShadows()  -- → bool
```

#### Shadow origin and edge fade

```lua
-- Origin: 0 = Center (default), 1 = Top, 2 = Bottom
SetBoxColliderShadowOrigin(1)
local origin = GetBoxColliderShadowOrigin()       -- → int

SetSphereColliderShadowOrigin(2)
local origin = GetSphereColliderShadowOrigin()

SetCapsuleColliderShadowOrigin(0)
local origin = GetCapsuleColliderShadowOrigin()

-- Edge fade: 0.0 = sharp full silhouette, 1.0 = fully collapsed
SetBoxColliderShadowEdgeFade(0.25)
local fade = GetBoxColliderShadowEdgeFade()       -- → float [0..1]

SetSphereColliderShadowEdgeFade(0.5)
local fade = GetSphereColliderShadowEdgeFade()

SetCapsuleColliderShadowEdgeFade(0.0)
local fade = GetCapsuleColliderShadowEdgeFade()

-- Z-order: negative = toward background, positive = toward foreground, 0 = caster plane (default)
SetBoxColliderShadowZOrder(1)
local zo = GetBoxColliderShadowZOrder()           -- → float

SetSphereColliderShadowZOrder(0)
local zo = GetSphereColliderShadowZOrder()

SetCapsuleColliderShadowZOrder(1)
local zo = GetCapsuleColliderShadowZOrder()

-- Optional second argument selects collider index (default 0).
SetBoxColliderShadowOrigin(1, 0)
SetBoxColliderShadowEdgeFade(0.3, 0)
SetBoxColliderShadowZOrder(1, 0)
```

#### Local transform

```lua
-- Box collider
SetBoxColliderLocalPosition(5, 10)
local pos = GetBoxColliderLocalPosition()    -- → {x, y}
SetBoxColliderLocalScale(2, 2)
local scale = GetBoxColliderLocalScale()     -- → {x, y}
SetBoxColliderLocalRotation(45)
local rot = GetBoxColliderLocalRotation()    -- → float (degrees)

-- Sphere collider
SetSphereColliderLocalPosition(5, 10)
local pos = GetSphereColliderLocalPosition() -- → {x, y}
SetSphereColliderLocalScale(2, 2)
local scale = GetSphereColliderLocalScale()  -- → {x, y}
SetSphereColliderLocalRotation(45)
local rot = GetSphereColliderLocalRotation() -- → float (degrees)

-- Capsule collider
SetCapsuleColliderLocalPosition(5, 10)
local pos = GetCapsuleColliderLocalPosition() -- → {x, y}
SetCapsuleColliderLocalScale(2, 2)
local scale = GetCapsuleColliderLocalScale()  -- → {x, y}
SetCapsuleColliderLocalRotation(45)
local rot = GetCapsuleColliderLocalRotation() -- → float (degrees)

-- All functions accept an optional index for multiple colliders:
SetBoxColliderLocalPosition(5, 10, 1)  -- Second box collider
```

#### World transform

Same conversion rules as sprites — the entity transform is already applied.
Available for all three collider kinds (`Box`, `Sphere`, `Capsule`):

```lua
-- Box
SetBoxColliderWorldPosition(120, 64, 0)
local cwp = GetBoxColliderWorldPosition(0)          -- → {x, y, z}
SetBoxColliderWorldRotation(30, 0)
local cwr = GetBoxColliderWorldRotation(0)          -- → number
local cws = GetBoxColliderWorldScale(0)             -- → {x, y}, read-only

-- Sphere
SetSphereColliderWorldPosition(120, 64, 0)
local swp = GetSphereColliderWorldPosition(0)       -- → {x, y, z}
SetSphereColliderWorldRotation(30, 0)
local swr = GetSphereColliderWorldRotation(0)       -- → number
local sws = GetSphereColliderWorldScale(0)          -- → {x, y}, read-only

-- Capsule
SetCapsuleColliderWorldPosition(120, 64, 0)
local pwp = GetCapsuleColliderWorldPosition(0)      -- → {x, y, z}
SetCapsuleColliderWorldRotation(30, 0)
local pwr = GetCapsuleColliderWorldRotation(0)      -- → number
local pws = GetCapsuleColliderWorldScale(0)         -- → {x, y}, read-only
```

> These move the collider's transform offset. Like the local setters they do
> not rebuild the Box2D shape by themselves — use the runtime shape geometry
> functions below when you need the physics shape to follow.

#### Runtime shape geometry (resize / rebuild)

> These functions mutate the underlying Box2D shape **at runtime**: they update the component's serialized geometry fields and then destroy & recreate the `b2Shape` on the existing body with the same material, filter, sensor and one-way settings. Use this to shrink a player capsule on crouch, grow a hitbox on an attack frame, or pulse a sphere trigger radius.
>
> All setters and `Rebuild*` functions return `true` on success and accept an optional index argument (default `0`) to pick the collider inside the component when there is more than one. After a rebuild any cached `b2ShapeId` value is invalidated — query it again with the matching `Get*` API or your own state.
> Sizes are in **sprite units** (1.0 = one full sprite tile, `DEFAULT_SPRITE_SIZE` pixels). Final world size also accounts for entity scale and the collider's local scale.

```lua
-- Box collider — Size is a 2D vector in sprite units
SetBoxColliderSize(2.0, 1.0)            -- width = 2 tiles, height = 1 tile
SetBoxColliderSize(2.0, 1.0, 1)         -- second Box collider
local size = GetBoxColliderSize()       -- → {x = width, y = height}
RebuildBoxColliderShape()               -- recreate b2Shape without changing Size
RebuildBoxColliderShape(0)

-- Sphere collider — Radius is a scalar in sprite units
SetSphereColliderRadius(0.75)
SetSphereColliderRadius(0.75, 1)
local r = GetSphereColliderRadius()     -- → float
RebuildSphereColliderShape()
RebuildSphereColliderShape(0)

-- Capsule collider — Length is the segment between the two hemisphere centers, Radius is the cap radius (both in sprite units)
SetCapsuleColliderLength(0.5)
SetCapsuleColliderLength(0.5, 1)
local len = GetCapsuleColliderLength()  -- → float

SetCapsuleColliderRadius(0.25)
SetCapsuleColliderRadius(0.25, 1)
local cr  = GetCapsuleColliderRadius()  -- → float

RebuildCapsuleColliderShape()
RebuildCapsuleColliderShape(0)
```

> **Tilemap and flipbook collision shapes** are baked from the underlying tileset / sprite asset. For tilemaps, use `RebuildTilemapPhysics()` after toggling collision flags or swapping tiles to regenerate the static body. Flipbook collision polygons follow the active frame automatically — material properties (density / friction / restitution / sensor / one-way) can still be mutated at runtime via `SetFlipbookCollision*` functions.

#### Collision group and mode — per-collider

> Unlike `SetCollisionGroupByName` / `SetCollisionEnabled` (which affect the **entire** body at once), these functions configure **each** collider of a component independently. The optional `index` (0 by default) picks the shape inside the component when there is more than one.
> All setters return `true` on success, `false` if the component / shape / group was not found. Changes apply to the Box2D runtime filter **and** are written back into the component's serialized fields (`CollisionGroupIndex` / `CollisionEnabled`).

```lua
-- Box collider
SetBoxColliderGroup(2)                          -- by group index
SetBoxColliderGroup(2, 1)                       -- second Box collider
SetBoxColliderGroupByName("Player")             -- by group name
SetBoxColliderGroupByName("Enemy", 0)
local gi   = GetBoxColliderGroup()              -- → int (−1 if none)
local name = GetBoxColliderGroupName()          -- → string
SetBoxColliderCollisionEnabled(2)               -- 0..3 (see values below)
local mode = GetBoxColliderCollisionEnabled()   -- → int

-- Sphere collider
SetSphereColliderGroup(2)
SetSphereColliderGroupByName("Player")
local gi   = GetSphereColliderGroup()
local name = GetSphereColliderGroupName()
SetSphereColliderCollisionEnabled(2)
local mode = GetSphereColliderCollisionEnabled()

-- Capsule collider
SetCapsuleColliderGroup(2)
SetCapsuleColliderGroupByName("Player")
local gi   = GetCapsuleColliderGroup()
local name = GetCapsuleColliderGroupName()
SetCapsuleColliderCollisionEnabled(2)
local mode = GetCapsuleColliderCollisionEnabled()
```

> `CollisionEnabled` modes:
> - `0` — **NoCollision** (maskBits = 0; no contact / sensor / hit events)
> - `1` — **QueryOnly** (forced sensor; only sensor events)
> - `2` — **PhysicsOnly** (solid body; contact + hit events, no sensor events)
> - `3` — **QueryAndPhysics** (solid body; all events enabled — default)

---

## 15. Traces — Tracing (Raycast and Shape Sweep)

> **Type:** Entity-bound / Level. Requires a physics world (Box2D).

### LineTrace — raycast

> **Filtering:** the caster's own body is always skipped, and with `ignoreSensors = true` (the default) sensor shapes are skipped too. Skipped shapes do **not** block the ray — it keeps going and returns the closest shape that survives the filter. A sensor in front of a wall therefore still reports the wall. The same rule applies to `LineTraceDirection`, `LineTraceAtCursor`, `LineTraceMulti` and every shape sweep (`CircleTrace`, `BoxTrace`, `CapsuleTrace`, …).

```lua
-- Cast a ray
local hit = LineTrace(startX, startY, endX, endY)
local hit = LineTrace(startX, startY, endX, endY, true)        -- ignoreSensors
local hit = LineTrace(startX, startY, endX, endY, true, true)  -- debugDraw
local hit = LineTrace(startX, startY, endX, endY, true, true, 2.0)  -- debugDuration

-- Ray by direction
local hit = LineTraceDirection(startX, startY, dirX, dirY, distance, true)

-- Ray with tag filter (tag supports prefix matching)
-- Line-of-sight semantics: the closest shape must carry the tag, otherwise no hit.
-- The caster's own body is skipped; anything else (walls, sensors) blocks the ray.
local hit = LineTraceTag(startX, startY, endX, endY, "Enemy", true, 0.5)

-- Result:
-- hit.hit       = true/false
-- hit.x, hit.y  = hit point (world coordinates)
-- hit.normalX, hit.normalY = surface normal
-- hit.fraction   = fraction of the path (0..1)
-- hit.distance   = distance to hit
-- hit.entityId   = entity ID (or nil)
-- hit.tag         = entity tag
-- hit.isSensor    = is collider a sensor

-- Example: line of sight check
local target = FindEntityByTag("Player")
if target then
    local tpos = GetEntityPosition(target)
    local myPos = GetPosition()
    local hit = LineTrace(myPos.x, myPos.y, tpos.x, tpos.y, true)
    if hit.hit and hit.tag == "Player" then
        -- We can see the player!
        canSeePlayer = true
    end
end
```

### CircleTrace — circle sweep

> Unlike `LineTrace` (ray), `CircleTrace` sweeps a circle along the path and finds the first hit.

```lua
-- Sweep a circle radius 16 from point A to point B
local hit = CircleTrace(startX, startY, endX, endY, 16)
local hit = CircleTrace(startX, startY, endX, endY, 16, true)         -- ignoreSensors
local hit = CircleTrace(startX, startY, endX, endY, 16, true, true)   -- debugDraw
local hit = CircleTrace(startX, startY, endX, endY, 16, true, true, 2.0) -- debugDuration

-- Result is the same as LineTrace:
-- hit.hit, hit.x, hit.y, hit.normalX, hit.normalY, hit.fraction,
-- hit.distance, hit.entityId, hit.tag, hit.isSensor
```

### BoxTrace — box sweep

```lua
-- Sweep a box halfW×halfH from point A to point B
local hit = BoxTrace(startX, startY, endX, endY, 20, 10)
local hit = BoxTrace(startX, startY, endX, endY, 20, 10, true, true, 2.0)
-- startX, startY, endX, endY, halfWidth, halfHeight, ignoreSensors?, debugDraw?, debugDuration?

-- Result is the same as LineTrace
```

### LineTraceMulti — multiple hits

```lua
local hits = LineTraceMulti(startX, startY, endX, endY, 10, true, true, 0.5)
for _, hit in ipairs(hits) do
    Print(hit.entityId)
end
```

### OverlapBox / OverlapCircle — overlaps by area

Both take the same optional `debugDraw` / `debugDuration` tail as every other trace and overlap function. Outline color: green on hit, red when empty.

```lua
local entities = OverlapBox(cx, cy, halfW, halfH, true)
local entities = OverlapCircle(cx, cy, radius, true)

local entities = OverlapBox(cx, cy, halfW, halfH, true, true, 0)
-- cx, cy, halfW, halfH, ignoreSensors?, debugDraw?, debugDuration?

local entities = OverlapCircle(cx, cy, radius, true, true, 0)
-- cx, cy, radius, ignoreSensors?, debugDraw?, debugDuration?
```

### OverlapBoxDebug / OverlapCircleDebug — overlap with debug draw

> Kept for backwards compatibility. `OverlapBoxDebug` is equivalent to `OverlapBox` with the same arguments; prefer `OverlapBox` / `OverlapCircle` in new code. `OverlapCircleDebug` still differs in one way: it performs an **exact** test via `b2World_OverlapShape`, whereas `OverlapCircle` uses an AABB broad-phase test plus a center-distance check. Outline color: green on hit, red when empty.

```lua
local entities = OverlapBoxDebug(cx, cy, halfW, halfH, true, true, 2.0)
-- cx, cy, halfW, halfH, ignoreSensors?, debugDraw?, debugDuration?

local entities = OverlapCircleDebug(cx, cy, radius, true, true, 2.0)
-- cx, cy, radius, ignoreSensors?, debugDraw?, debugDuration?
```

### OverlapCapsule — capsule overlap

> Finds all entities inside a capsule defined by two hemisphere-center points (`A`, `B`) and a radius. Exact test via `b2World_OverlapShape`.

```lua
local entities = OverlapCapsule(ax, ay, bx, by, radius, true, true, 2.0)
-- ax, ay, bx, by, radius, ignoreSensors?, debugDraw?, debugDuration?
```

### OverlapBoxRotated — oriented box (OBB) overlap

> Unlike `OverlapBox` (AABB), this takes an **oriented** rectangle (OBB) with a rotation angle `angleRad` (radians, counter-clockwise). Exact test via `b2World_OverlapShape`.

```lua
local entities = OverlapBoxRotated(cx, cy, halfW, halfH, angleRad, true, true, 2.0)
-- cx, cy, halfW, halfH, angleRad, ignoreSensors?, debugDraw?, debugDuration?
```

### CircleTraceMulti / BoxTraceMulti — multi-hit shape sweeps

> Collect **all** intersections along the path (like `LineTraceMulti`, but for circle / box). Results are already sorted by `fraction`, and duplicates on the same body are deduplicated (nearest hit kept).

```lua
local hits = CircleTraceMulti(startX, startY, endX, endY, radius, 10, true, true, 0.5)
-- startX, startY, endX, endY, radius, maxHits?, ignoreSensors?, debugDraw?, debugDuration?

local hits = BoxTraceMulti(startX, startY, endX, endY, halfW, halfH, 10, true, true, 0.5)
-- startX, startY, endX, endY, halfW, halfH, maxHits?, ignoreSensors?, debugDraw?, debugDuration?

for _, h in ipairs(hits) do
    Print(h.entityId, h.fraction)
end
```

### CapsuleTrace / CapsuleTraceMulti — capsule sweep

> Sweeps a capsule from a start pose (`sAx,sAy` → `sBx,sBy`) to an end pose (`eAx,eAy` → `eBx,eBy`). `CapsuleTrace` returns the first hit, `CapsuleTraceMulti` returns all of them.

```lua
local hit  = CapsuleTrace(sAx, sAy, sBx, sBy, radius,
                          eAx, eAy, eBx, eBy,
                          true, true, 2.0)
-- ignoreSensors?, debugDraw?, debugDuration?

local hits = CapsuleTraceMulti(sAx, sAy, sBx, sBy, radius,
                               eAx, eAy, eBx, eBy,
                               10, true, true, 2.0)
-- maxHits?, ignoreSensors?, debugDraw?, debugDuration?
```

### BoxTraceRotated / BoxTraceRotatedMulti — OBB sweep

> Sweeps an oriented box (OBB) with rotation angle `angleRad` from `(startX,startY)` to `(endX,endY)`. The rotation is fixed along the entire path.

```lua
local hit  = BoxTraceRotated(startX, startY, endX, endY,
                             halfW, halfH, angleRad,
                             true, true, 2.0)
-- ignoreSensors?, debugDraw?, debugDuration?

local hits = BoxTraceRotatedMulti(startX, startY, endX, endY,
                                  halfW, halfH, angleRad,
                                  10, true, true, 2.0)
-- maxHits?, ignoreSensors?, debugDraw?, debugDuration?
```

> **Important:** all trace and overlap functions honor the current `CollisionGroups.WithLayerMask(...)`, `selfBody` (colliders of the owning entity are automatically excluded), and `ignoreSensors`. The fields of the returned hit tables are identical to `LineTrace`.

### Cursor & Screen Traces

Convenience functions that automatically convert screen/cursor position to world space.
They use mouse, touch, or gamepad cursor — whichever is active.

```lua
-- Overlap at cursor position (mouse or touch)
local ids = OverlapAtCursor()                    -- radius=4, ignoreSensors=true
local ids = OverlapAtCursor(8.0, false)          -- custom radius, include sensors
for _, entityId in ipairs(ids) do
    Print(entityId)
end

-- Overlap at arbitrary screen point
local ids = OverlapAtScreenPoint(400, 300)       -- screenX, screenY
local ids = OverlapAtScreenPoint(400, 300, 8.0, false)

-- All entities inside a screen-space rectangle
local ids = GetEntitiesInScreenRect(100, 100, 500, 400)  -- sx1, sy1, sx2, sy2
local ids = GetEntitiesInScreenRect(100, 100, 500, 400, false)  -- include sensors

-- Raycast at cursor (vertical ray centered on cursor world position)
local hit = LineTraceAtCursor()                  -- radius=1, ignoreSensors=true
local hit = LineTraceAtCursor(2.0, true, true, 1.0)  -- radius, ignoreSensors, debugDraw, debugDuration
-- Result: same as LineTrace (hit.hit, hit.x, hit.y, hit.entityId, hit.tag, etc.)
```

### DrawFilledRect / DrawSelectionRect

```lua
-- Filled rectangle (world coordinates)
DrawFilledRect(x, y, w, h)                           -- default: green, alpha 0.3
DrawFilledRect(x, y, w, h, 1, 0, 0, 0.5, 2.0)       -- r, g, b, a, duration

-- Selection rectangle with border and transparent fill
DrawSelectionRect(x1, y1, x2, y2)                    -- default: green, fillAlpha 0.15
DrawSelectionRect(x1, y1, x2, y2, 0, 0, 1, 0.2, 1.0)  -- r, g, b, fillAlpha, duration
```

### Example: melee attack area

```lua
function MeleeAttack()
    local pos = GetPosition()
    local fwd = GetForwardVector()
    local endX = pos.x + fwd.x * 80
    local endY = pos.y + fwd.y * 80

    -- Wide box attack
    local hit = BoxTrace(pos.x, pos.y, endX, endY, 30, 15, true, true, 0.5)
    if hit.hit and hit.tag == "Enemy" then
        CallInterface(hit.entityId, "TakeDamage", 25)
    end
end
```

---

## 16. Time — Time and Timers

> **Type:** Global functions

### Delta time

```lua
-- Time since last frame (accounts for TimeScale)
local dt = GetDeltaTime()

-- Total game time
local t = GetTime()

-- Real time (not affected by pause)
local rt = GetRealTime()

-- Unscaled delta
local udt = GetUnscaledDeltaTime()
```

### Time scale (Slow-Mo, Pause)

```lua
-- Slow down
SetTimeScale(0.5)   -- Half speed
SetTimeScale(2.0)   -- Double speed
ResetTimeScale()    -- Back to 1.0
local ts = GetTimeScale()

-- Pause
PauseGame()                 -- SetTimeScale(0)
ResumeGame()                -- SetTimeScale(1)
ResumeGame(0.5)             -- SetTimeScale(0.5)
local paused = IsPaused()
```

### Per-entity time scale

```lua
SetEntityTimeScale(entityId, 0.5)
local ets = GetEntityTimeScale(entityId)
local edt = GetEntityDeltaTime(entityId)  -- dt * entityTimeScale
```

### FPS

```lua
local fps = GetFPS()
local frameTime = GetFrameTime()  -- In milliseconds
```

### Real date and time

`GetTime` and friends measure **game** time. These functions read the **system clock** — use
them for daily rewards, "time since the last session", save timestamps and similar features.
They replace `os.time` / `os.date`, which are not available (see
[Standard libraries](#standard-libraries)).

```lua
local now = GetUnixTime()      -- seconds since 1970-01-01 UTC
local nowMs = GetUnixTimeMs()  -- the same in milliseconds

-- Broken down into fields (local time by default, pass true for UTC)
local d = GetDateTable()
Print(d.year, d.month, d.day, d.hour, d.min, d.sec)
Print(d.wday, d.yday, d.isdst)   -- wday: 1 = Sunday, yday: 1..366

local utc = GetDateTable(now, true)

-- strftime formatting
FormatDate("%Y-%m-%d %H:%M:%S")        -- → "2026-08-21 22:10:35"
FormatDate("%d.%m.%Y", savedTimestamp) -- format a stored timestamp
FormatDate("%H:%M", now, true)         -- UTC

-- Daily reward
local lastClaim = tonumber(ReadFile("lastClaim.txt") or "0")
if GetSecondsSince(lastClaim) >= 24 * 60 * 60 then
    GiveDailyReward()
    WriteFile("lastClaim.txt", tostring(GetUnixTime()))
end
```

| Function | Returns |
| -------- | ------- |
| `GetUnixTime()` | Seconds since the Unix epoch (integer). |
| `GetUnixTimeMs()` | Milliseconds since the Unix epoch (integer). |
| `GetDateTable([unixTime] [, utc])` | Table `{ year, month, day, hour, min, sec, wday, yday, isdst }`. `month` is 1..12, `wday` is 1..7 starting at Sunday, `yday` is 1..366. Defaults to now, local time. |
| `FormatDate(format [, unixTime] [, utc])` | `strftime`-formatted string (max 255 characters, `""` on overflow). |
| `GetSecondsSince(unixTime)` | How many seconds have passed since the given timestamp. |

> The system clock can be changed by the player, so never use it as the only protection for
> timed rewards in a game that has a server.

### Timers (simple utilities)

```lua
-- Check: has time elapsed?
local startTime = GetTime()
-- ...later:
if TimerElapsed(startTime, 3.0) then
    -- 3 seconds passed
end

-- Remaining time
local remaining = TimerRemaining(startTime, 3.0)

-- Progress (0..1)
local progress = TimerProgress(startTime, 3.0)
```

### Delay and SetInterval (advanced timers)

```lua
-- Call a function after N seconds
local timerId = Delay(2.0, function()
    Print("2 seconds passed!")
end)

-- Repeat every N seconds
local intervalId = SetInterval(1.0, function()
    Print("Every second")
end)

-- Timer control
CancelTimer(timerId)
CancelAllTimers()
local active = IsTimerActive(timerId)
local count = GetActiveTimerCount()
```

### RetriggerableDelay — retriggerable delay

```lua
-- Regular Delay creates a new timer each call
-- RetriggerableDelay resets the timer if called again with the same key

-- Example: show UI hint for 3 seconds, refreshing the timer
RetriggerableDelay("hide_hint", 3.0, function()
    SetWidgetVisible(false)
end)

-- If called again before 3s — timer resets
RetriggerableDelay("hide_hint", 3.0, function()
    SetWidgetVisible(false)
end)

-- Example: input debounce
function OnUpdate(dt)
    if IsKeyPressed("e") then
        RetriggerableDelay("interact_cooldown", 0.5, function()
            Interact()
        end)
    end
end
```

---

## 17. Tween — Smooth Value Animations

> **Type:** Global functions
>
> Tween smoothly changes a number from A to B over time. Great for UI animations, movement, effects.

### Simple Tween

```lua
-- Simple: from 0 to 100 in 1 second
local id = Tween(0, 100, 1.0, function(value, t)
    -- value = current value (0..100)
    -- t = eased progress (0..1)
    SetSpriteAlpha(value / 100)
end, "outQuad")  -- Easing type (optional)
```

### Extended TweenEx

```lua
local id = TweenEx({
    from = 0,
    to = 255,
    duration = 2.0,
    delay = 0.5,               -- Delay before start
    easing = "outElastic",     -- Easing type
    loops = 3,                 -- Loop count (0 = infinite)
    yoyo = true,               -- Ping-pong
    onUpdate = function(value, t)
        SetSpriteAlpha(value / 255)
    end,
    onComplete = function()
        Print("Tween finished!")
    end
})
```

### Control

```lua
StopTween(id)
PauseTween(id)
ResumeTween(id)
local running = IsTweenRunning(id)
StopAllTweens()
local count = GetActiveTweenCount()
```

### Apply easing manually

```lua
local easedT = Ease(0.5, "outBounce")
```

### Tween sequence

```lua
local id = TweenSequence({
    { from = 0, to = 1, duration = 0.5, easing = "inQuad",
      onUpdate = function(v) SetSpriteAlpha(v) end },
    { from = 1, to = 0, duration = 0.5, easing = "outQuad",
      onUpdate = function(v) SetSpriteAlpha(v) end }
})
```

### Available easing types

| Category | Types |
|-----------|------|
| Linear | `linear` |
| Sine | `inSine`, `outSine`, `inOutSine` |
| Quad | `inQuad`, `outQuad`, `inOutQuad` |
| Cubic | `inCubic`, `outCubic`, `inOutCubic` |
| Quart | `inQuart`, `outQuart`, `inOutQuart` |
| Quint | `inQuint`, `outQuint`, `inOutQuint` |
| Expo | `inExpo`, `outExpo`, `inOutExpo` |
| Circ | `inCirc`, `outCirc`, `inOutCirc` |
| Back | `inBack`, `outBack`, `inOutBack` |
| Elastic | `inElastic`, `outElastic`, `inOutElastic` |
| Bounce | `inBounce`, `outBounce`, `inOutBounce` |

> `in` = slow start, `out` = slow finish, `inOut` = both

### Timeline — multi-track timeline with keyframes

> Timeline lets you interpolate multiple values at once with different easing and keyframes.

```lua
local tl = Timeline({
    duration = 3.0,          -- Total duration (seconds)
    loops = 0,               -- 0 = infinite, 1 = once
    yoyo = true,             -- Play back and forth
    playRate = 1.0,          -- Playback speed
    autoPlay = true,         -- Start immediately

    tracks = {
        {
            name = "posX",
            easing = "inOutQuad",
            keyframes = {
                { time = 0.0, value = 100 },
                { time = 1.5, value = 400 },
                { time = 3.0, value = 100 }
            },
            onUpdate = function(value, progress)
                SetPositionX(value)
            end
        },
        {
            name = "alpha",
            easing = "outSine",
            keyframes = {
                { time = 0.0, value = 0 },
                { time = 0.5, value = 1 },
                { time = 2.5, value = 1 },
                { time = 3.0, value = 0 }
            },
            onUpdate = function(value, progress)
                SetSpriteAlpha(value)
            end
        }
    },

    onComplete = function()
        Print("Timeline finished!")
    end,
    onLoop = function(loopNum)
        Print("Loop #" .. loopNum)
    end
})

-- Call Update in OnUpdate:
function OnUpdate(dt)
    tl.Update(dt)
end

-- Control:
tl.Play()
tl.Pause()
tl.Stop()
tl.Reverse()               -- Reverse direction
tl.SetPlayRate(2.0)        -- 2x speed
local rate = tl.GetPlayRate()  -- current play rate
tl.SetTime(1.5)            -- Seek to 1.5s
local p = tl.GetProgress()  -- 0..1
local e = tl.GetElapsed()
local d = tl.GetDuration()
local playing = tl.IsPlaying()
local done = tl.IsFinished()
```

---

## 18. Coroutine — Coroutines

> **Type:** Global functions
>
> Coroutines let you write **sequential** logic that spans multiple frames.

```lua
-- Start a coroutine
local id = StartCoroutine(function()
    Print("Start!")

    -- Wait 2 seconds
    coroutine.yield(WaitSeconds(2.0))
    Print("2 seconds passed!")

    -- Wait 3 frames
    coroutine.yield(WaitFrames(3))
    Print("3 frames passed!")

    -- Wait for condition
    coroutine.yield(WaitUntil(function()
        return IsKeyJustPressed("space")
    end))
    Print("Pressed Space!")
end, "myCoroutine")  -- Name (optional)

-- Control
StopCoroutine(id)
StopAllCoroutines()
local running = IsCoroutineRunning(id)
local count = GetCoroutineCount()
```

### Example: dialogue

```lua
StartCoroutine(function()
    SetWidgetText("dialog", "Hello, traveler!")
    SetWidgetVisible(true)
    coroutine.yield(WaitSeconds(2.0))

    SetWidgetText("dialog", "A dangerous adventure awaits you...")
    coroutine.yield(WaitSeconds(2.0))

    SetWidgetText("dialog", "Good luck!")
    coroutine.yield(WaitSeconds(1.5))

    SetWidgetVisible(false)
end)
```

---

## 19. FSM — Finite State Machine

> **Type:** Global functions
>
> FSM (Finite State Machine) is a pattern to manage states for AI, player, etc.

```lua
local sm = StateMachine({
    initial = "Idle",  -- Initial state
    states = {
        Idle = {
            onEnter = function(args)
                Print("Entered Idle")
                SetAnimBool("isRunning", false)
            end,
            onUpdate = function(dt)
                if IsKeyPressed("d") or IsKeyPressed("a") then
                    sm:SetState("Run")
                end
                if IsKeyJustPressed("space") then
                    sm:SetState("Jump")
                end
            end,
            onExit = function(args)
                Print("Exited Idle")
            end
        },
        Run = {
            onEnter = function()
                SetAnimBool("isRunning", true)
            end,
            onUpdate = function(dt)
                local vx = 0
                if IsKeyPressed("d") then vx = 200 end
                if IsKeyPressed("a") then vx = -200 end
                SetVelocityX(vx)

                if vx == 0 then
                    sm:SetState("Idle")
                end
            end
        },
        Jump = {
            onEnter = function()
                AddImpulse(0, 400)
            end,
            onUpdate = function(dt)
                if IsGrounded() then
                    sm:SetState("Idle")
                end
            end
        }
    }
})

function OnUpdate(dt)
    sm:Update(dt)
end
```

### FSM methods

```lua
sm:SetState("Run")                  -- Switch state
sm:SetState("Run", { speed = 200 }) -- With args (passed to onEnter/onExit)
local state = sm:GetState()         -- Current state (string)
local prev = sm:GetPreviousState()  -- Previous
local is = sm:IsState("Idle")       -- Check
local has = sm:HasState("Fly")      -- State exists?

-- Dynamic add/remove
sm:AddState("Fly", { onEnter = ..., onUpdate = ..., onExit = ... })
sm:RemoveState("Fly")

-- FSM data
sm:SetData("jumpCount", 0)
local jc = sm:GetData("jumpCount")
```

---

## 20. Scene — Scenes, Saves, Files

> **Type:** Global functions

### Scene management

```lua
-- Load level
LoadLevel("Content/Maps/Level2.icemap")

-- Reload current
ReloadLevel()

-- Current level path
local path = GetCurrentLevel()

-- Quit game
QuitGame()
local quit = IsQuitRequested()
```

### Application suspend (pause everything)

Freezes update, render and audio across all 6 platforms (Windows, Linux, macOS, iOS, Android, Web). Useful for in-game pause menu when window is minimized, sent to background or when you need to pause the whole app from script.

```lua
-- Manually suspend the application (audio off, update/render skipped)
SuspendApp()

-- Resume from a manual suspend
ResumeApp()

-- Check whether a manual suspend is currently active
local suspended = IsAppSuspended()
```

> **Note:** The engine also auto-suspends on window minimize / hide and on mobile background events; the Lua API stacks on top of that — it represents an explicit `RequestManualSuspend` flag separate from the OS-driven window state. That automatic window-driven suspend can be disabled with `Settings.SetIsSuspended(false)` (see the "Suspend" section in Settings) — the manual API here keeps working either way.

### Global game state (Game State)

Data that **persists between levels** (until the game closes):

```lua
-- Simple types
SetGameInt("score", 100)
local score = GetGameInt("score", 0)       -- 0 = default value
AddToGameInt("score", 10)                  -- score += 10

SetGameFloat("time", 99.5)
local t = GetGameFloat("time", 0.0)
AddToGameFloat("time", -1.0)

SetGameString("playerName", "Hero")
local name = GetGameString("playerName", "Unknown")

SetGameBool("hasKey", true)
local key = GetGameBool("hasKey", false)

-- Vectors
SetGameVec2("spawn", 100, 200)
local sp = GetGameVec2("spawn")              -- → {x, y}

SetGameVec3("direction", 1, 0, 0)
local dir = GetGameVec3("direction")          -- → {x, y, z}

SetGameVec4("border", 0, 0, 1920, 1080)
local b = GetGameVec4("border")               -- → {x, y, z, w}

SetGameColor("highlight", 1, 0, 0, 1)
local c = GetGameColor("highlight")            -- → {r, g, b, a}

SetGameRect("area", 100, 200, 300, 400)
local r = GetGameRect("area")                  -- → {x, y, w, h}

-- Clear everything
ClearGameState()
```

### Scene state snapshot (save-states, checkpoints, rollback)

Game state above stores values you write by hand. A **scene state snapshot** is different: it
captures the live simulation of every entity in one binary blob, natively in C++ — transform,
the Box2D body (position, rotation, linear and angular velocity, awake and enabled flags), the
animator (state, frame, transition and every parameter including the trigger journal), the
skeleton (animation, time, blending, ragdoll flag **and the physics body of every ragdoll bone**),
and flipbook playback.

```lua
local state = SaveSceneState()      -- Binary blob as a Lua string; empty string on failure
-- ... play on ...
LoadSceneState(state)               -- Restores everything; returns true on success
```

Entities are keyed by UUID, so a snapshot restores correctly regardless of creation order, and
entities missing on load are skipped instead of corrupting the rest.

This is what makes deterministic rollback netcode practical: `Rollback` takes this snapshot on
**every** saved frame, so you never serialize physics from Lua by hand.

`Rollback.OnSaveState` / `Rollback.OnLoadState` are **additive**, not a replacement — use them only
for state the engine cannot see, such as your own Lua tables. Whatever string you return is appended
to the native snapshot and handed straight back to `OnLoadState`; physics, animator and skeleton stay
the engine's job either way.

```lua
Rollback.OnSaveState(function(frame) return SaveTableToString(myCombatState) end)
Rollback.OnLoadState(function(extra, frame) myCombatState = LoadTableFromString(extra) end)
```

### Tables (complex data)

```lua
-- Save a table to state
SaveTable("inventory", { "Sword", "Shield", gold = 500 })

-- Load
local inv = LoadTable("inventory")
if inv then
    Print("Gold: " .. inv.gold)
end

-- Check / remove
local has = HasSaveTable("inventory")
RemoveSaveTable("inventory")
```

### Save to disk

```lua
-- Save all state to file (in Saves folder)
SaveGameState()                    -- savegame.json
SaveGameState("slot1.json")

-- Load
LoadGameState()
LoadGameState("slot1.json")
```

### Working with files

> `WriteFile` and `ReadFile` are **byte-exact**: the file is opened in binary mode on every platform, so a Lua string
> round-trips unchanged, embedded zero bytes and all. That makes them a valid transport for compact binary save formats —
> a `PixelBuffer:ToString()` blob, a packed chunk of world tiles built with `string.pack`, a serialized RNG state — which
> is what large worlds (sandbox survival, colony sims, grand strategy) want instead of JSON.

```lua
-- Write (to save folder)
WriteFile("notes.txt", "Hello World!")
WriteFile("world.chunk", chunkBuffer:ToString())   -- binary is safe

-- Read
local content = ReadFile("notes.txt")  -- nil if it doesn't exist

-- Check existence
local exists = SaveFileExists("notes.txt")

-- Delete
DeleteSaveFile("notes.txt")

-- File list
local files = GetSaveFiles()           -- → table of names
local files = GetSaveFiles("saves/")   -- In subfolder

-- File metadata (size + modification time, useful for save-slot UI)
local info = GetSaveFileInfo("slot1.json")
--   → { name = "slot1.json", size = 1024, mtime = 1714816800 }
--   mtime is a unix timestamp in seconds; empty table if file is missing

local infos = GetSaveFilesInfo()           -- whole save folder
local infos = GetSaveFilesInfo("slots/")   -- subfolder
-- → { { name=..., size=..., mtime=... }, ... }
for _, e in ipairs(infos) do
    Print(e.name .. "  " .. e.size .. " bytes  ts=" .. e.mtime)
end
```

### World and FX

```lua
-- FX
SpawnFXAtPosition("Content/FX/Explosion.ice_fx", 100, 200)
SpawnFXAtPosition("Content/FX/Explosion.ice_fx", 100, 200, 10)
SetFXTimeScale(0.5)
local fxScale = GetFXTimeScale()

-- Physics world parameters
SetWorldGravity(0, -9.81)
local gravity = GetWorldGravity()  -- → {x, y}

SetWorldRestitutionThreshold(1.0)
local rest = GetWorldRestitutionThreshold()

SetWorldHitEventThreshold(5.0)
local hitThreshold = GetWorldHitEventThreshold()

SetWorldContactTuning(60, 0.5, 3.0)  -- hertz, dampingRatio, maxContactPushSpeed
local hertz = GetWorldContactHertz()
local damping = GetWorldContactDampingRatio()
local pushSpeed = GetWorldMaxContactPushSpeed()

SetWorldMaximumLinearSpeed(200)
local maxSpeed = GetWorldMaximumLinearSpeed()

EnableWorldSleep(true)
local sleep = IsWorldSleepEnabled()

EnableWorldContinuous(true)
local continuous = IsWorldContinuousEnabled()

-- Fixed timestep (physics simulation step size)
SetFixedTimestep(1/60)               -- default 1/60 ≈ 0.0167 sec
local step = GetFixedTimestep()       -- → 0.01667

-- Pixels Per Meter (read-only, set in Settings/World Override)
local ppm = GetPPM()                  -- → 100.0 (default)

-- Sub-step count (solver iterations per physics step)
local sub = GetSubStepCount()         -- → 4 (default)
SetSubStepCount(8)                    -- range 1..16
```

### Global Physics Queries

These are **scene-level** (global) query functions that work without being bound to a specific entity.
For entity-bound trace functions, see [Section 15 — Traces](#15-traces--tracing-raycast-and-shape-sweep).

```lua
-- Raycast — cast a ray and find the closest hit
-- All coordinates are in pixels; PPM conversion is automatic
local hit = Raycast(originX, originY, dirX, dirY)          -- maxDist = 1000
local hit = Raycast(originX, originY, dirX, dirY, 500)     -- custom maxDist

-- Result:
-- hit.hit        = true / false
-- hit.x, hit.y   = hit point (world pixels)
-- hit.normalX, hit.normalY = surface normal
-- hit.fraction    = fraction of path (0..1)
-- hit.entityId    = entity ID (or nil)
-- hit.tag         = entity tag (or nil)
-- hit.isSensor    = is the hit shape a sensor

-- OverlapCircle — find all entities inside a circle
local entities = OverlapCircle(cx, cy, radius)
for _, e in ipairs(entities) do
    Print(e.entityId .. " tag=" .. (e.tag or "") .. " sensor=" .. tostring(e.isSensor))
end

-- OverlapBox — find all entities inside an axis-aligned box
local entities = OverlapBox(cx, cy, halfW, halfH)
for _, e in ipairs(entities) do
    Print(e.entityId .. " tag=" .. (e.tag or ""))
end
```

> **Note:** Entity-bound `OverlapBox(cx, cy, halfW, halfH, ignoreSensors)` and
> `OverlapCircle(cx, cy, radius, ignoreSensors)` in entity scripts (Section 15)
> have an extra `ignoreSensors` parameter and return a richer result set.
> The global versions above are lighter and available in level scripts.

### Async Level Loading

Load a level in the background while the current scene keeps rendering.
Ideal for loading screens with real progress.

```lua
-- Start async loading (scene keeps running)
LoadLevelAsync("Content/Maps/Level2.icemap")

-- Start with auto-apply (swaps immediately when ready)
LoadLevelAsync("Content/Maps/Level2.icemap", true)

-- In OnUpdate / OnLevelUpdate — poll progress:
function OnLevelUpdate(dt)
    if IsAsyncLoading() then
        local progress = GetAsyncLoadProgress()   -- 0.0 to 1.0
        local status   = GetAsyncLoadStatus()     -- "Loading assets (3/7)..."
        local phase    = GetAsyncLoadPhase()       -- 0=Idle,1=Parsing,2=Preloading,3=Ready,4=Complete,5=Failed

        -- Update your loading screen UI
        SetWidgetProgress("LoadBar", progress)
        SetWidgetText("StatusText", status)
    end

    if IsAsyncLoadReady() then
        -- All assets preloaded, apply the level
        ApplyAsyncLevel()
    end

    if IsAsyncLoadFailed() then
        Print("Load error: " .. GetAsyncLoadError())
    end
end

-- Cancel an in-progress load
CancelAsyncLoad()
```

| Function | Returns | Description |
|----------|---------|-------------|
| `LoadLevelAsync(path)` | — | Start background level load |
| `LoadLevelAsync(path, true)` | — | Start with auto-apply when ready |
| `IsAsyncLoading()` | bool | True while loading |
| `IsAsyncLoadReady()` | bool | True when fully preloaded |
| `IsAsyncLoadFailed()` | bool | True on error |
| `GetAsyncLoadProgress()` | float | 0.0–1.0 progress |
| `GetAsyncLoadStatus()` | string | Human-readable status |
| `GetAsyncLoadPhase()` | int | 0=Idle, 1=Parsing, 2=Preloading, 3=Ready, 4=Complete, 5=Failed |
| `GetAsyncLoadError()` | string | Error message (if failed) |
| `ApplyAsyncLevel()` | — | Apply the preloaded level |
| `CancelAsyncLoad()` | — | Cancel in-progress load |

---

## 21. Widget — UI widgets

> **Type:** Entity-bound. Requires **WidgetComponent**.
> Widgets are created in the `.ice_widget` editor and attached to an entity.

`.ice_widget` is a hierarchical UI asset: a widget can contain **child elements** and can itself be used as a **nested element** inside another widget. This makes it possible to build composite interfaces from reusable parts.

### Visibility management

```lua
SetWidgetVisible(true)
SetWidgetVisible(false, 1)     -- Second widget
local vis = IsWidgetVisible()
ToggleWidget()                 -- Toggle
ShowAllWidgets()
HideAllWidgets()
```

### Properties

```lua
-- Scale
SetWidgetScale(2.0)
local scale = GetWidgetScale()

-- Position
SetWidgetPosition(100, 50)
local pos = GetWidgetPosition()  -- → {x, y}

-- Render order: widgets are sorted globally by RenderOrder (higher = on top).
-- When RenderOrder is equal, world-space widgets always render beneath
-- screen-space widgets — in-world UI stays with the world, screen-space UI
-- overlays it. Give a widget a higher RenderOrder to override this explicitly.
SetWidgetRenderOrder(10)

-- Interactivity
SetWidgetInteractable(true)
local interactable = IsWidgetInteractable()

-- Player Index (split-screen visibility, per WidgetComponent instance)
-- -1 = shared across all players (HUD), 0..3 = visible only in that local player's viewport
SetWidgetPlayerIndex(-1)         -- first instance (index 0)
SetWidgetPlayerIndex(1, 2)       -- third instance (index 2), only for player 1
local pi = GetWidgetPlayerIndex()      -- first instance
local pi2 = GetWidgetPlayerIndex(1)    -- second instance

-- Screen Space (attach to screen, not world)
SetWidgetScreenSpace(true)

-- Render in game (visibility in play mode)
SetWidgetRenderInGame(true)
SetWidgetRenderInGame(false, 1)    -- Second widget
local render = GetWidgetRenderInGame()  -- → bool (default true)

-- Widget count
local count = GetWidgetCount()
```

### Working with widget elements

```lua
-- Text
SetWidgetText("ScoreLabel", "Score: 100")
local text = GetWidgetText("ScoreLabel")

-- Progress bar (0..1)
SetWidgetProgress("HealthBar", 0.75)
local progress = GetWidgetProgress("HealthBar")

-- Element visibility
SetWidgetElementVisible("Hint", true)
local vis = IsWidgetElementVisible("Hint")

-- Localization
SetWidgetLocalizationKey("Title", "menu_title")
local key = GetWidgetLocalizationKey("Title")

-- Element image
SetWidgetElementImage("Icon", "Content/Textures/sword.png")
local img = GetWidgetElementImage("Icon")

-- Lit/Unlit (per-element lighting & shadow participation; default Unlit)
SetWidgetElementLit("Background", true)
local lit = GetWidgetElementLit("Background")

-- Shadow receiver (per-element; default OFF). When ON, dynamic shadows fall
-- onto this Lit element. Works for both world-space and screen-space widgets.
SetWidgetElementShadowReceiver("Background", true)
local recv = GetWidgetElementShadowReceiver("Background")

-- Panel backdrop blur (frosted glass; only meaningful on Panel elements,
-- default OFF). Lower the panel's color alpha to reveal the blurred backdrop.
SetWidgetElementPanelBlur("GlassPanel", true)
local blurOn = GetWidgetElementPanelBlur("GlassPanel")
SetWidgetElementPanelBlurStrength("GlassPanel", 16.0)   -- blur radius in canvas px
local blurStrength = GetWidgetElementPanelBlurStrength("GlassPanel")

-- Flipbook (animated image)
SetWidgetElementFlipbook("Anim", "Content/Flipbooks/fire.ice_flipbook")
local fb = GetWidgetElementFlipbook("Anim")
SetWidgetFlipbookFrame("Anim", 3)
local frame = GetWidgetFlipbookFrame("Anim")

-- Element color
SetWidgetElementColor("Background", 0.2, 0.2, 0.2, 0.8)

-- Slider
local val = GetSliderValue("Volume")
SetSliderValue("Volume", 0.5)

-- Checkbox (toggle)
local on = IsCheckboxChecked("Fullscreen")
SetCheckboxChecked("Fullscreen", true)

-- Dropdown
local sel = GetDropdownSelected("Language")
local text = GetDropdownSelectedText("Language")
SetDropdownSelected("Language", 2)

-- InputField
local input = GetInputText("PlayerName")
SetInputText("PlayerName", "Hero")
```

### Additional element properties

```lua
-- Element type
local type = GetElementType("Title")

-- Position and size (set and get)
SetWidgetElementPosition("Title", 10, 20)
local pos = GetWidgetElementPosition("Title")   -- → {x, y}
SetWidgetElementSize("Title", 200, 40)
local size = GetWidgetElementSize("Title")       -- → {x, y}

-- Desired Size (size-to-content). While enabled the element sizes itself to its
-- content (text, sprite/flipbook, children, box flow) and Size becomes read-only.
SetWidgetElementUseDesiredSize("Title", true)
local uses = GetWidgetElementUseDesiredSize("Title")
local desired = GetWidgetElementDesiredSize("Title")   -- → {x, y} (computed, even when disabled)

-- Element color (set and get)
SetWidgetElementColor("Background", 0.2, 0.2, 0.2, 0.8)
local color = GetWidgetElementColor("Background")  -- → {r, g, b, a}

-- Opacity
SetWidgetElementOpacity("Title", 0.75)
local opacity = GetWidgetElementOpacity("Title")

-- Scale and rotation (set and get)
SetWidgetElementScale("Icon", 1.2, 1.2)
local scale = GetWidgetElementScale("Icon")      -- → {x, y}
SetWidgetElementRotation("Icon", 15)
local rot = GetWidgetElementRotation("Icon")     -- → float

-- Pivot
SetWidgetElementPivot("Icon", 0.5, 0.5)
local pivot = GetWidgetElementPivot("Icon")  -- → {x, y}

-- Anchor
SetWidgetElementAnchor("Title", "MiddleCenter")
local anchor = GetWidgetElementAnchor("Title")  -- → string
-- Available values: "TopLeft", "TopCenter", "TopRight",
-- "MiddleLeft", "MiddleCenter", "MiddleRight",
-- "BottomLeft", "BottomCenter", "BottomRight",
-- "StretchLeft", "StretchCenter", "StretchRight",
-- "StretchTop", "StretchMiddle", "StretchBottom", "StretchAll"

-- Interactivity
SetWidgetElementInteractable("Button", true)
local interactable = IsWidgetElementInteractable("Button")

-- Z-order
SetWidgetElementZOrder("Panel", 5)
local z = GetWidgetElementZOrder("Panel")

-- Global Z: when true, this element's ZOrder participates in global widget
-- Z sorting (across widgets and even the entity scene). With ZOrder < 0 the
-- element renders BEFORE scene entities (tilemaps/sprites/flipbooks/FX) — useful
-- for HUD backgrounds that should appear behind the gameplay. With ZOrder >= 0
-- the element renders together with other widgets, sorted globally by ZOrder
-- against widget RenderOrder. Default is false (element sorts only inside its widget).
-- Ties follow the space rule: in the overlay pass (ZOrder >= 0) world-space
-- entries render beneath screen-space ones; in the background pass (ZOrder < 0)
-- screen-space backdrops render first (farthest back), so world-space backdrops
-- sit closer to the gameplay layer.
SetWidgetElementGlobalZ("Background", true)
local isGlobal = GetWidgetElementGlobalZ("Background")  -- → bool

-- Post processing participation: when true, this element is drawn inside the
-- scene and receives post processing (bloom, exposure, depth of field, color
-- grading, etc.). When false (the default) the element is drawn as an overlay
-- AFTER the post-process chain, so it stays crisp and unaffected — the right
-- choice for HUD icons, bars and text. Background elements (GlobalZ with
-- ZOrder < 0) are always part of the scene and ignore this flag.
SetWidgetElementIsPostProcessed("BackgroundSky", true)
local isPP = GetWidgetElementIsPostProcessed("BackgroundSky")  -- → bool

-- Font and text color
SetWidgetElementFontSize("Title", 32)
local size = GetWidgetElementFontSize("Title")
SetWidgetElementTextColor("Title", 1, 1, 1, 1)
local color = GetWidgetElementTextColor("Title")  -- → {r, g, b, a}

-- Tooltip
SetWidgetElementTooltip("Button", "Click to continue")
local tooltip = GetWidgetElementTooltip("Button")

-- Fill color (ProgressBar/Slider)
SetFillColor("HealthBar", 0.8, 0.1, 0.1, 1)

-- Button state colors (Hovered/Pressed)
-- First arg after element name is the enable flag — when false (the default for a fresh element),
-- elements do NOT auto-change color on hover/press. Set it to true to opt-in to the built-in colors,
-- or drive color transitions yourself from OnHover/OnUnhover/OnPressed/OnReleased callbacks.
SetWidgetStateColors("Button", true, 1, 0.8, 0.8, 1, 0.8, 0.2, 0.2, 1)

-- Button state sounds (Hovered/Pressed)
-- First arg after element name is the enable flag — when false (the default for a fresh element),
-- elements do NOT auto-play sounds on hover/press. Set it to true to opt-in to the built-in sounds.
SetWidgetStateSounds("Button", true, "Content/Audio/hover.wav", "Content/Audio/click.wav")

-- Text wrap (Text, Button)
SetWidgetTextWrap("Description", true)
local wrap = IsWidgetTextWrap("Description")

-- Custom anchors (stretch/layout)
SetWidgetCustomAnchors("Panel", 0, 0, 1, 1)    -- minX, minY, maxX, maxY
ClearWidgetCustomAnchors("Panel")

-- Checkbox check sprite
SetWidgetElementCheckedSprite("Toggle", "Content/Textures/checked.png")
```

### Widget animations

```lua
PlayWidgetAnimation("Intro")
PauseWidgetAnimation("Intro")
ResumeWidgetAnimation("Intro")
StopWidgetAnimation("Intro", true)

local playing = IsWidgetAnimationPlaying("Intro")
SetWidgetAnimationTime("Intro", 0.5)
local t = GetWidgetAnimationTime("Intro")

SetAnimationCompleteCallback("Intro", "OnIntroFinished")

-- Animation events — fire a callback at a specific time in the animation
AddAnimationEvent("Intro", 0.5, "OnHalfway")               -- animName, time, callback
AddAnimationEvent("Intro", 1.0, "OnFinish", "myParam")     -- with optional parameter
```

### Local transforms and instances

```lua
SetWidgetLocalPosition(10, 20)
local lp = GetWidgetLocalPosition()

SetWidgetLocalScale(1.5, 1.5)
local ls = GetWidgetLocalScale()

SetWidgetLocalRotation(15)
local lr = GetWidgetLocalRotation()

-- World transform (entity transform already applied — see the Sprite section)
SetWidgetWorldPosition(120, 64, 0)
local wwp = GetWidgetWorldPosition(0)      -- → {x, y, z}
SetWidgetWorldRotation(30, 0)
local wwr = GetWidgetWorldRotation(0)      -- → number
local wws = GetWidgetWorldScale(0)         -- → {x, y}, read-only

SetWidgetInstanceVisible(true)
local vis = IsWidgetInstanceVisible()

SetWidgetInstanceScale(1.2)
local scale = GetWidgetInstanceScale()

SetWidgetScreenSpace(true)
local screen = IsWidgetScreenSpace()

SetWidgetStretchMode("Letterbox")
local mode = GetWidgetStretchMode()

SetWidgetFlipX(true)
local fx = GetWidgetFlipX()

SetWidgetFlipY(false)
local fy = GetWidgetFlipY()
```

### Additional UI settings

```lua
-- Max text length (InputField)
SetWidgetMaxLength("NameInput", 16)
local maxLen = GetWidgetMaxLength("NameInput")

-- Spacing and padding (HorizontalBox/VerticalBox)
SetWidgetSpacing("Row", 8)
local spacing = GetWidgetSpacing("Row")

SetWidgetPadding("Row", 4, 4, 4, 4)

-- Safe area (mobile notches / rounded corners)
SetWidgetSafeArea(true)                              -- enable
SetWidgetSafeArea(true, 10, 20, 10, 20)              -- enable, left, top, right, bottom
SetWidgetSafeArea(false)                             -- disable

-- Drag scroll (ScrollView)
SetWidgetDragScroll("Inventory", true)

-- Scroll offset (ScrollView)
SetWidgetScrollOffset("Inventory", 0, 100)           -- x, y
local offset = GetWidgetScrollOffset("Inventory")    -- → {x, y}
```

### Element callbacks

```lua
-- Basic interaction
SetWidgetCallback("PlayButton", "OnClick", "OnPlayClicked")
local cb = GetWidgetCallback("PlayButton", "OnClick")

-- Hover (when cursor enters the element)
SetWidgetCallback("PlayButton", "OnHover", "OnPlayHover")
-- Unhover (when cursor leaves the element) — fires once when the hover state ends
SetWidgetCallback("PlayButton", "OnUnhover", "OnPlayUnhover")

-- Mouse press / release on the element
-- OnPressed fires the moment the mouse button is pressed down on this element
-- OnReleased fires when the mouse button is released — even if released outside the element
SetWidgetCallback("PlayButton", "OnPressed", "OnPlayPressed")
SetWidgetCallback("PlayButton", "OnReleased", "OnPlayReleased")

-- Value / focus events
SetWidgetCallback("NameInput", "OnValueChanged", "OnNameChanged")
SetWidgetCallback("NameInput", "OnFocusGained", "OnNameFocus")
SetWidgetCallback("NameInput", "OnFocusLost", "OnNameBlur")
```

> **Note.** By default elements do NOT auto-change color on hover/press. To use built-in hover/pressed colors, enable `UseCustomStateColors` via `SetWidgetStateColors(...)`. Otherwise drive color transitions yourself from `OnHover` / `OnUnhover` / `OnPressed` / `OnReleased` callbacks using `SetWidgetElementColor`.

### Throbber (spinner)

```lua
SetThrobberClockwise("Loading", true)
local cw = IsThrobberClockwise("Loading")

SetThrobberSpeed("Loading", 2.0)
local speed = GetThrobberSpeed("Loading")

-- Pause / resume animation
SetThrobberPaused("Loading", true)
local paused = IsThrobberPaused("Loading")
PauseThrobber("Loading")
ResumeThrobber("Loading")

-- Reset time to zero (jump back to first dot)
ResetThrobber("Loading")
```

### Toggle and WidgetSwitcher

```lua
SetToggleState("MusicToggle", true)
local on = GetToggleState("MusicToggle")

SetSwitcherActiveChild("Tabs", 1)
local idx = GetSwitcherActiveChild("Tabs")
```

### Dynamic widget instances

```lua
local idx = AddWidgetInstance("Content/UI/HUD.ice_widget", true)
RemoveWidgetInstance(idx)
```

### Dynamic elements

```lua
-- typeName: "Text", "Button", "Image", "ProgressBar", "Slider", "InputField",
-- "Checkbox", "Dropdown", "ScrollView", "HorizontalBox", "VerticalBox",
-- "SizeBox", "Overlay", "Throbber", "WidgetSwitcher", "Spacer", "Toggle"
local elemId = AddWidgetElement("Text", "HintText", "Root")
RemoveWidgetElement("HintText")
```

### Widget element hierarchy

```lua
-- Check if element exists
local exists = HasWidgetElement("Title")

-- Get child elements
local children = GetWidgetElementChildren("Panel")   -- → table of names
-- Example: {"Label", "Button", "Icon"}

-- Get root elements of widget
local roots = GetWidgetRootElements()                 -- → table of names

-- Get parent element
local parentName = GetWidgetElementParent("Button")   -- → string or ""

-- Move element to a different parent
MoveWidgetElement("Button", "NewPanel")               -- Move into "NewPanel"
MoveWidgetElement("Button", "")                       -- Move to root
```

### Dropdown (extended controls)

```lua
-- Set/get options list
SetDropdownOptions("Language", {"English", "Русский", "日本語"})
local options = GetDropdownOptions("Language")   -- → table of strings
local count = GetDropdownOptionCount("Language") -- → number
```

### Widget instance properties

```lua
-- Instance name
local name = GetWidgetInstanceName(0)

-- Path to .ice_widget (can be changed at runtime)
local path = GetWidgetInstancePath(0)
SetWidgetInstancePath("Content/UI/NewHUD.ice_widget", 0)

-- Render order
local order = GetWidgetRenderOrder(0)
```

### Extended element properties (entity-bound)

Full set of per-element getters/setters added for complete script control. All accept an optional trailing `instanceIndex` (defaults to `0`).

```lua
-- Text alignment (Text / Button / InputField)
SetWidgetElementTextAlign("Title", "Center")    -- "Left" | "Center" | "Right"
local ha = GetWidgetElementTextAlign("Title")
SetWidgetElementTextVAlign("Title", "Middle")    -- "Top" | "Middle" | "Bottom"
local va = GetWidgetElementTextVAlign("Title")

-- Font face (path to .ice_font / font asset; "" = default font)
SetWidgetElementFont("Title", "Content/Fonts/Title.ice_font")
local font = GetWidgetElementFont("Title")

-- Generic value (raw CurrentValue, no clamping)
SetWidgetElementValue("Bar", 42)
local v = GetWidgetElementValue("Bar")

-- Value range (Slider / ProgressBar) — CurrentValue is re-clamped into the range
SetWidgetValueRange("Volume", 0, 100)
local minV = GetWidgetMinValue("Volume")
local maxV = GetWidgetMaxValue("Volume")

-- Fill color getter (ProgressBar / Slider) — setter is SetFillColor
local fill = GetFillColor("HealthBar")           -- → {r, g, b, a}

-- Nine-slice (stretchable sprite borders)
SetWidgetNineSlice("Panel", true, 12, 12, 12, 12)   -- enable, left, top, right, bottom
local ns = GetWidgetNineSlice("Panel")           -- → {enabled, left, top, right, bottom}

-- Clip children to the element rect
SetWidgetClipChildren("Panel", true)
local clip = GetWidgetClipChildren("Panel")

-- Per-element gamepad navigation neighbors (element names; "" / nil clears)
SetWidgetNavigation("PlayBtn", "TitleLabel", "OptionsBtn", nil, nil)   -- up, down, left, right
local nav = GetWidgetNavigation("PlayBtn")       -- → {up, down, left, right} (names)

-- Throbber geometry
SetThrobberRadius("Loading", 30)
local rad = GetThrobberRadius("Loading")
SetThrobberDots("Loading", 12)
local dots = GetThrobberDots("Loading")

-- Hover / Pressed state colors (individual access + getters)
SetWidgetUseStateColors("Button", true)
local useState = GetWidgetUseStateColors("Button")
SetWidgetHoveredColor("Button", 1, 0.8, 0.8, 1)
local hov = GetWidgetHoveredColor("Button")      -- → {r, g, b, a}
SetWidgetPressedColor("Button", 0.8, 0.2, 0.2, 1)
local prs = GetWidgetPressedColor("Button")      -- → {r, g, b, a}

-- Hover / Pressed state sounds (individual access + getters)
SetWidgetUseStateSounds("Button", true)
local useSnd = GetWidgetUseStateSounds("Button")
SetWidgetHoveredSound("Button", "Content/Audio/hover.wav")
local hovSnd = GetWidgetHoveredSound("Button")   -- → string path
SetWidgetPressedSound("Button", "Content/Audio/click.wav")
local prsSnd = GetWidgetPressedSound("Button")   -- → string path

-- Getters for previously set-only properties
local pad = GetWidgetPadding("Row")              -- → {left, top, right, bottom}
local drag = GetWidgetDragScroll("Inventory")    -- → bool
local checkSprite = GetWidgetElementCheckedSprite("Toggle")
local anchors = GetWidgetCustomAnchors("Panel")  -- → {enabled, minX, minY, maxX, maxY}

-- Tooltip delay (seconds before the tooltip appears)
SetWidgetElementTooltipDelay("Button", 0.5)
local td = GetWidgetElementTooltipDelay("Button")

-- Dropdown list max height (px)
SetWidgetDropdownMaxHeight("Language", 240)
local dh = GetWidgetDropdownMaxHeight("Language")

-- ScrollView scrollbars + content extent
SetWidgetScrollbars("Inventory", true, false)    -- showVertical, showHorizontal
local sb = GetWidgetScrollbars("Inventory")      -- → {vertical, horizontal}
SetWidgetContentSize("Inventory", 300, 1200)
local cs = GetWidgetContentSize("Inventory")     -- → {x, y}

-- SizeBox forced dimensions
SetWidgetSizeOverride("Box", true, true, 200, 120)  -- overrideW, overrideH, width, height
local so = GetWidgetSizeOverride("Box")          -- → {overrideWidth, overrideHeight, width, height}

-- Slider thumb art
SetWidgetSliderThumbImage("Volume", "Content/UI/knob.png")
local thumb = GetWidgetSliderThumbImage("Volume")
SetWidgetSliderThumbFlipbook("Volume", "Content/UI/knob.ice_flipbook")
local thumbFb = GetWidgetSliderThumbFlipbook("Volume")

-- Toggle colors / handle
SetWidgetToggleColors("MusicToggle", 0.3, 0.7, 0.4, 1, 0.5, 0.5, 0.55, 1)  -- on rgba, off rgba
local onCol = GetWidgetToggleOnColor("MusicToggle")    -- → {r, g, b, a}
local offCol = GetWidgetToggleOffColor("MusicToggle")  -- → {r, g, b, a}
SetWidgetToggleHandleRatio("MusicToggle", 0.45)
local hr = GetWidgetToggleHandleRatio("MusicToggle")
SetWidgetToggleHandleImage("MusicToggle", "Content/UI/handle.png")
local hImg = GetWidgetToggleHandleImage("MusicToggle")
```

### Widget canvas, scaling & safe area (entity-bound)

```lua
-- Design canvas size of the widget instance
SetWidgetCanvasSize(1920, 1080)
local canvas = GetWidgetCanvasSize()             -- → {x, y}

-- Desired Size for the canvas itself: the canvas hugs its root elements and
-- Canvas Size becomes read-only (recomputed every frame).
SetWidgetCanvasUseDesiredSize(true)
local canvasAuto = GetWidgetCanvasUseDesiredSize()
local canvasDesired = GetWidgetCanvasDesiredSize()  -- → {x, y} (computed, even when disabled)

-- Scale widget contents with the screen (true) or keep pixel size (false)
SetWidgetScaleWithScreen(true)
local sws = GetWidgetScaleWithScreen()

-- Read back the safe-area config (setter is SetWidgetSafeArea)
local sa = GetWidgetSafeArea()                   -- → {enabled, left, top, right, bottom}
```

### Element reordering & rename (entity-bound)

```lua
-- Move an element up/down among its siblings (affects draw order within parent)
ReorderWidgetElementUp("Icon")
ReorderWidgetElementDown("Icon")

-- Reorder relative to a reference sibling
MoveWidgetElementBefore("Icon", "Label")
MoveWidgetElementAfter("Icon", "Label")

-- Rename an element. Returns the actual (uniquified) name that was applied.
local applied = SetWidgetElementName("OldName", "NewName")
```

### SubWidget — nested widgets (ClassComponent analog for UI)

SubWidget is an element of type `SubWidget` that references another `.ice_widget`.
This allows building composite interfaces from reusable parts,
similar to how ClassComponent works for `.ice_class`.

```lua
-- Path to nested widget
local path = GetSubWidgetPath("HealthBar")
SetSubWidgetPath("HealthBar", "Content/UI/NewHealthBar.ice_widget")

-- Working with elements inside SubWidget
-- First param — SubWidget element name, second — element name inside nested widget

-- Text
SetSubWidgetText("HealthBar", "Label", "100 HP")
local text = GetSubWidgetText("HealthBar", "Label")

-- Element visibility
SetSubWidgetElementVisible("HealthBar", "Warning", true)
local vis = IsSubWidgetElementVisible("HealthBar", "Warning")

-- Color
SetSubWidgetElementColor("HealthBar", "Fill", 1, 0, 0, 1)
local color = GetSubWidgetElementColor("HealthBar", "Fill")  -- → {r, g, b, a}

-- Position and size
SetSubWidgetElementPosition("HealthBar", "Icon", 10, 5)
local pos = GetSubWidgetElementPosition("HealthBar", "Icon")  -- → {x, y}
SetSubWidgetElementSize("HealthBar", "Icon", 32, 32)
local sz = GetSubWidgetElementSize("HealthBar", "Icon")       -- → {x, y}

-- Desired Size on a nested widget element
SetSubWidgetElementUseDesiredSize("HealthBar", "Icon", true)
local subUses = GetSubWidgetElementUseDesiredSize("HealthBar", "Icon")
local subDesired = GetSubWidgetElementDesiredSize("HealthBar", "Icon")  -- → {x, y}

-- Progress bar
SetSubWidgetProgress("HealthBar", "Bar", 0.75)
local progress = GetSubWidgetProgress("HealthBar", "Bar")

-- Image
SetSubWidgetElementImage("HealthBar", "Icon", "Content/Textures/heart.png")

-- Opacity
SetSubWidgetElementOpacity("HealthBar", "Label", 0.5)
local opacity = GetSubWidgetElementOpacity("HealthBar", "Label")

-- Rotation (degrees)
SetSubWidgetElementRotation("HealthBar", "Icon", 45)
local rot = GetSubWidgetElementRotation("HealthBar", "Icon")

-- Scale
SetSubWidgetElementScale("HealthBar", "Icon", 1.5, 1.5)
local scale = GetSubWidgetElementScale("HealthBar", "Icon")  -- → {x, y}

-- Pivot
SetSubWidgetElementPivot("HealthBar", "Icon", 0.5, 0.5)
local pivot = GetSubWidgetElementPivot("HealthBar", "Icon")  -- → {x, y}

-- Interactability
SetSubWidgetElementInteractable("Menu", "Button", false)

-- Callback
SetSubWidgetCallback("Menu", "PlayButton", "OnClick", "OnPlayClicked")
-- callbackType: "OnClick", "OnHover", "OnValueChanged", "OnFocusGained", "OnFocusLost"
```

#### SubWidget — extended API

All functions below accept an **optional last `instanceIndex`** argument (defaults to `0`) — the index of the `WidgetInstance` on the entity, same semantics as other `GetWidget*`/`SetWidget*` calls. The first argument is always the name of the **SubWidget element** in the outer widget; the second is the name of the **inner element** inside the nested `.ice_widget`.

```lua
-- ── Structure / introspection ─────────────────────────────
HasSubWidgetElement("HealthBar", "Label")                    -- → bool
GetSubWidgetElementType("HealthBar", "Label")                -- → "Text" | "Button" | "Image" | ...
GetSubWidgetRootElements("HealthBar")                        -- → { "Root1", "Root2", ... }
GetSubWidgetElementChildren("HealthBar", "Panel")            -- → { "Child1", "Child2", ... }
GetSubWidgetElementParent("HealthBar", "Icon")               -- → parent name or ""

-- ── Text properties ───────────────────────────────────────
SetSubWidgetElementText("HealthBar", "Label", "100 HP")
local t   = GetSubWidgetElementText("HealthBar", "Label")    -- → string
SetSubWidgetElementFontSize("HealthBar", "Label", 18)
local fs  = GetSubWidgetElementFontSize("HealthBar", "Label")-- → number
SetSubWidgetElementTextColor("HealthBar", "Label", 1, 1, 1, 1)
local tc  = GetSubWidgetElementTextColor("HealthBar", "Label") -- → {r, g, b, a}

-- ── Image / Flipbook ──────────────────────────────────────
SetSubWidgetElementFlipbook("HealthBar", "Fx", "Content/FX/spin.ice_flipbook")
local fb  = GetSubWidgetElementFlipbook("HealthBar", "Fx")   -- → string
local img = GetSubWidgetElementImage("HealthBar", "Icon")    -- → string (sprite path)

-- ── Layout ────────────────────────────────────────────────
SetSubWidgetElementZOrder("HealthBar", "Icon", 5)
local z   = GetSubWidgetElementZOrder("HealthBar", "Icon")   -- → int
-- GlobalZ on a sub-widget inner element: same semantics as
-- SetWidgetElementGlobalZ — when true, the element's ZOrder is used as
-- a global Z sort key (negative ZOrder renders behind the scene).
SetSubWidgetElementGlobalZ("HealthBar", "Background", true)
local sg  = GetSubWidgetElementGlobalZ("HealthBar", "Background") -- → bool
-- Note: a nested widget renders as one unit — which pass it lands in is decided
-- by the SubWidget element's own IsPostProcessed flag on the host widget.
-- Inner-element flags apply when that widget asset is used standalone.
SetSubWidgetElementIsPostProcessed("HealthBar", "Background", true)
local spp = GetSubWidgetElementIsPostProcessed("HealthBar", "Background") -- → bool
SetSubWidgetElementTooltip("HealthBar", "Icon", "Health")
local tip = GetSubWidgetElementTooltip("HealthBar", "Icon")  -- → string
SetSubWidgetElementAnchor("HealthBar", "Icon", "TopLeft")
local a   = GetSubWidgetElementAnchor("HealthBar", "Icon")   -- → anchor name

-- ── Interactive controls ──────────────────────────────────
SetSubWidgetCheckboxChecked("Menu", "SoundOn", true)
IsSubWidgetCheckboxChecked("Menu", "SoundOn")                -- → bool

SetSubWidgetToggleState("Menu", "MusicBtn", true)
GetSubWidgetToggleState("Menu", "MusicBtn")                  -- → bool

SetSubWidgetSliderValue("Settings", "Volume", 0.5)
GetSubWidgetSliderValue("Settings", "Volume")                -- → number

SetSubWidgetInputText("LoginForm", "Name", "Player1")
GetSubWidgetInputText("LoginForm", "Name")                   -- → string

SetSubWidgetDropdownOptions("Settings", "Quality", { "Low", "Medium", "High" })
GetSubWidgetDropdownOptions("Settings", "Quality")           -- → table
SetSubWidgetDropdownSelected("Settings", "Quality", 2)
GetSubWidgetDropdownSelected("Settings", "Quality")          -- → int (0-based)

SetSubWidgetSwitcherActiveChild("Tabs", "Pages", 1)
GetSubWidgetSwitcherActiveChild("Tabs", "Pages")             -- → int

-- ── Animations (on the nested widget) ─────────────────────
PlaySubWidgetAnimation("HealthBar", "Pulse")                 -- start by name
StopSubWidgetAnimation("HealthBar", "Pulse")
PauseSubWidgetAnimation("HealthBar", "Pulse")
ResumeSubWidgetAnimation("HealthBar", "Pulse")
IsSubWidgetAnimationPlaying("HealthBar", "Pulse")            -- → bool

-- Optional instance index (for entities with multiple WidgetInstances):
SetSubWidgetElementText("HealthBar", "Label", "0/100", 1)
PlaySubWidgetAnimation("HealthBar", "Pulse", 1)
```

> **Note.** The extended API works recursively — it resolves the inner element through any depth of nested SubWidgets. Calls that target a widget/instance that is not yet loaded trigger an on-demand `LoadWidget`, after which animations and flipbooks of the nested widget tick automatically each frame along with the rest of the UI.

#### SubWidget — full inner-element properties

The complete property set for inner elements, mirroring the top-level element API. First arg = SubWidget element name, second = inner element name; optional trailing `instanceIndex`.

```lua
-- Lighting & panel blur
SetSubWidgetElementLit("HealthBar", "Bg", true);             GetSubWidgetElementLit("HealthBar", "Bg")
SetSubWidgetElementShadowReceiver("HealthBar", "Bg", true);  GetSubWidgetElementShadowReceiver("HealthBar", "Bg")
SetSubWidgetElementPanelBlur("HealthBar", "Glass", true);    GetSubWidgetElementPanelBlur("HealthBar", "Glass")
SetSubWidgetElementPanelBlurStrength("HealthBar", "Glass", 16.0); GetSubWidgetElementPanelBlurStrength("HealthBar", "Glass")

-- Text alignment / font / wrap / localization
SetSubWidgetElementTextAlign("HealthBar", "Label", "Center"); GetSubWidgetElementTextAlign("HealthBar", "Label")
SetSubWidgetElementTextVAlign("HealthBar", "Label", "Middle");GetSubWidgetElementTextVAlign("HealthBar", "Label")
SetSubWidgetElementFont("HealthBar", "Label", "Content/Fonts/F.ice_font"); GetSubWidgetElementFont("HealthBar", "Label")
SetSubWidgetElementTextWrap("HealthBar", "Label", true);     IsSubWidgetElementTextWrap("HealthBar", "Label")
SetSubWidgetElementLocalizationKey("HealthBar", "Label", "hp_text"); GetSubWidgetElementLocalizationKey("HealthBar", "Label")
SetSubWidgetElementMaxLength("Form", "Name", 16);            GetSubWidgetElementMaxLength("Form", "Name")

-- Values, range, fill, content
SetSubWidgetElementValue("HealthBar", "Bar", 42);            GetSubWidgetElementValue("HealthBar", "Bar")
SetSubWidgetElementValueRange("HealthBar", "Bar", 0, 100)
GetSubWidgetElementMinValue("HealthBar", "Bar");             GetSubWidgetElementMaxValue("HealthBar", "Bar")
SetSubWidgetElementFillColor("HealthBar", "Bar", 0.8, 0.1, 0.1, 1); GetSubWidgetElementFillColor("HealthBar", "Bar")
SetSubWidgetElementContentSize("Menu", "Scroll", 300, 1200); GetSubWidgetElementContentSize("Menu", "Scroll")

-- State colors / interactivity / callback read-back
SetSubWidgetElementUseStateColors("Menu", "Btn", true);      GetSubWidgetElementUseStateColors("Menu", "Btn")
SetSubWidgetElementHoveredColor("Menu", "Btn", 1, 0.9, 0.6, 1)
SetSubWidgetElementPressedColor("Menu", "Btn", 0.7, 0.5, 0.3, 1)
SetSubWidgetElementUseStateSounds("Menu", "Btn", true);      GetSubWidgetElementUseStateSounds("Menu", "Btn")
SetSubWidgetElementHoveredSound("Menu", "Btn", "Content/Audio/hover.wav")
SetSubWidgetElementPressedSound("Menu", "Btn", "Content/Audio/click.wav")
IsSubWidgetElementInteractable("Menu", "Btn")
GetSubWidgetCallback("Menu", "Btn", "OnClick")

-- Layout: spacing / padding / nine-slice / clip / anchors
SetSubWidgetElementSpacing("Menu", "Row", 8);                GetSubWidgetElementSpacing("Menu", "Row")
SetSubWidgetElementPadding("Menu", "Row", 4, 4, 4, 4);       GetSubWidgetElementPadding("Menu", "Row")
SetSubWidgetElementNineSlice("Menu", "Panel", true, 12, 12, 12, 12)
SetSubWidgetElementClipChildren("Menu", "Panel", true);      GetSubWidgetElementClipChildren("Menu", "Panel")
SetSubWidgetElementCustomAnchors("Menu", "Panel", 0, 0, 1, 1); ClearSubWidgetElementCustomAnchors("Menu", "Panel")

-- ScrollView / SizeBox / Slider thumb
SetSubWidgetElementScrollOffset("Menu", "Scroll", 0, 100);   GetSubWidgetElementScrollOffset("Menu", "Scroll")
SetSubWidgetElementDragScroll("Menu", "Scroll", true)
SetSubWidgetElementSizeOverride("Menu", "Box", true, true, 200, 120)
SetSubWidgetElementSliderThumbImage("Settings", "Vol", "Content/UI/knob.png")
SetSubWidgetElementSliderThumbFlipbook("Settings", "Vol", "Content/UI/knob.ice_flipbook")

-- Toggle colors / handle, checkbox sprite, tooltip delay, dropdown height
SetSubWidgetElementToggleColors("Menu", "Music", 0.3,0.7,0.4,1, 0.5,0.5,0.55,1)
SetSubWidgetElementToggleHandleRatio("Menu", "Music", 0.45)
SetSubWidgetElementToggleHandleImage("Menu", "Music", "Content/UI/handle.png")
SetSubWidgetElementCheckedSprite("Menu", "Sound", "Content/UI/checked.png")
SetSubWidgetElementTooltipDelay("Menu", "Btn", 0.5);        GetSubWidgetElementTooltipDelay("Menu", "Btn")
SetSubWidgetElementDropdownMaxHeight("Settings", "Quality", 240)
GetSubWidgetDropdownSelectedText("Settings", "Quality");    GetSubWidgetDropdownOptionCount("Settings", "Quality")

-- Throbber controls
SetSubWidgetThrobberSpeed("HUD", "Spinner", 2.0);           GetSubWidgetThrobberSpeed("HUD", "Spinner")
SetSubWidgetThrobberClockwise("HUD", "Spinner", true)
SetSubWidgetThrobberRadius("HUD", "Spinner", 30)
SetSubWidgetThrobberDots("HUD", "Spinner", 12)
SetSubWidgetThrobberPaused("HUD", "Spinner", true);         IsSubWidgetThrobberPaused("HUD", "Spinner")

-- Per-inner-element navigation + flipbook frame
SetSubWidgetElementNavigation("Menu", "PlayBtn", "Title", "OptionsBtn", nil, nil)
SetSubWidgetFlipbookFrame("HUD", "Anim", 3);                GetSubWidgetFlipbookFrame("HUD", "Anim")

-- ── Structure editing (runtime) ───────────────────────────
-- Build/modify the inner element tree of a nested widget at runtime — the
-- SubWidget analog of AddWidgetElement / RemoveWidgetElement / MoveWidgetElement.
-- typeName is any widget type ("Panel","Text","Button","Image","ProgressBar",
-- "Slider","InputField","Checkbox","Dropdown","ScrollView","HorizontalBox",
-- "VerticalBox","SizeBox","Overlay","Throbber","WidgetSwitcher","Spacer","Toggle","SubWidget").
local id = AddSubWidgetElement("HealthBar", "Text", "NewLabel")            -- → new element ID
AddSubWidgetElement("HealthBar", "Image", "Icon", "Root")                  -- with inner parent
RemoveSubWidgetElement("HealthBar", "NewLabel")
MoveSubWidgetElement("HealthBar", "Icon", "OtherPanel")                    -- reparent ("" = root)
ReorderSubWidgetElementUp("HealthBar", "Icon")                            -- → bool
ReorderSubWidgetElementDown("HealthBar", "Icon")                         -- → bool
MoveSubWidgetElementBefore("HealthBar", "Icon", "Label")                 -- → bool
MoveSubWidgetElementAfter("HealthBar", "Icon", "Label")                  -- → bool
local unique = SetSubWidgetElementName("HealthBar", "Icon", "HeartIcon") -- → applied unique name
```

### Focus & Interaction State

```lua
-- Set focus to a specific element
FocusWidgetElement("NameInput")
ClearWidgetFocus()

-- Query current interaction state (global)
local focused = GetWidgetFocusedElement()   -- → element name or ""
local hovered = GetWidgetHoveredElement()   -- → element name or ""
local pressed = GetWidgetPressedElement()   -- → element name or ""

-- Check per-element state (entity-bound)
local isFocused = IsWidgetElementFocused("NameInput")
local isHovered = IsWidgetElementHovered("PlayButton")
local isPressed = IsWidgetElementPressed("PlayButton")
```

### Drag & drop between UI elements

A drag session is engine state: you start it from a pressed element, the engine tracks the pointer and draws the ghost
above every widget, and you ask it what is under the cursor when the player lets go. Inventory grids, hotbars, card
hands, equipment slots and skill bars are all the same four calls.

```lua
-- Start dragging. The payload is any Lua value - it comes back untouched on drop.
BeginWidgetDrag("Slot3", { item = "potion", count = 5 }, {
    icon = "Content/UI/potion.png",   -- ghost sprite drawn under the cursor
    width = 48, height = 48,
    offsetX = 0, offsetY = 0,         -- ghost offset from the pointer
    r = 1, g = 1, b = 1, a = 0.85,    -- ghost tint
})

IsWidgetDragActive()          -- bool
GetWidgetDragSource()         -- element the drag started from
GetWidgetDragPayload()        -- the value you passed in
GetWidgetDragPosition()       -- { x, y } pointer position in widget space
GetWidgetDragTarget()         -- element under the cursor right now ("" over the source or nothing)
SetWidgetDragIcon(path, w, h) -- swap the ghost mid-drag (valid / invalid target feedback)

local target = DropWidgetDrag()   -- ends the drag, returns the element it landed on ("" = nowhere)
CancelWidgetDrag()                -- ends it with no drop
```

The engine does not decide what a drop *means* — that is your game's rule. It gives you source, target and payload; you
move the item.

```lua
function OnUpdate(dt)
    if not IsWidgetDragActive() then
        if IsWidgetElementPressed("Slot3") and IsMousePressed(1) then
            BeginWidgetDrag("Slot3", inventory[3], { icon = inventory[3].icon })
        end
    elseif not IsMousePressed(1) then
        local target = DropWidgetDrag()
        if target ~= "" then
            MoveItem(GetWidgetDragSource(), target, GetWidgetDragPayload())
        end
    end
end
```

> The ghost is drawn in the same pass as tooltips — above every widget, below nothing. A drag started in one widget can
> be dropped on an element of another; targets are resolved by the same hit test that drives hover.

### Large lists — off-screen culling

Elements inside a `ScrollView` that fall entirely outside its visible rectangle are skipped before any layout or draw
work happens, so a list pays for the rows on screen rather than the rows it contains. A ten-thousand-row table costs
about the same as a twenty-row one.

```lua
SetWidgetScrollCulling(true)     -- on by default
IsWidgetScrollCulling()
GetWidgetCulledCount()           -- elements skipped last frame - useful while tuning
```

Culling is exact — an element is dropped only when its rectangle lies fully outside the scroll viewport — and is
disabled automatically for a rotated `ScrollView`, where the axis-aligned test would not be safe. Turn it off only if
you deliberately rely on off-screen elements running their own per-frame side effects.

### Gamepad navigation (customizable)

The widget system supports full gamepad navigation: DPad + analog stick to move
focus between interactable elements, an Activate button to click / toggle /
open dropdown / focus input field, a Cancel button to drop focus. Sliders also
react to left/right on DPad and stick. All settings below are **global** —
they apply to navigation across every widget on the scene.

```lua
-- Enable / disable gamepad navigation entirely
SetGamepadEnabled(true)
local on = IsGamepadEnabled()

-- Remap action / direction buttons.
-- role:   "Activate" | "Cancel" | "Up" | "Down" | "Left" | "Right"
-- button: "A" | "B" | "X" | "Y" | "Back" | "Guide" | "Start"
--         | "LeftStick" | "RightStick" | "LeftShoulder" | "RightShoulder"
--         | "DPadUp" | "DPadDown" | "DPadLeft" | "DPadRight"
SetGamepadButton("Activate", "A")
SetGamepadButton("Cancel",   "B")
SetGamepadButton("Up",       "DPadUp")
SetGamepadButton("Down",     "DPadDown")
SetGamepadButton("Left",     "DPadLeft")
SetGamepadButton("Right",    "DPadRight")

-- Sensitivity
SetGamepadNavCooldown(0.2)            -- seconds between auto-repeat steps on stick
SetGamepadStickThreshold(0.5)         -- analog stick deadzone for navigation (0..1)
SetGamepadSliderStickThreshold(0.3)   -- analog stick deadzone for sliders
SetGamepadSliderStepSize(0.05)        -- step per DPad press on a focused slider (normalized)
SetGamepadSliderStickSpeed(0.02)      -- speed per frame from stick on focused slider

-- Which stick drives navigation
SetGamepadUseLeftStick(true)          -- false = right stick

-- Focus border (the highlight rectangle around the focused element)
SetGamepadFocusBorder(true)                                 -- show / hide
SetGamepadFocusBorder(true, 1.0, 0.9, 0.2, 1.0)             -- show + color (r,g,b,a)
SetGamepadFocusBorder(true, 1.0, 0.9, 0.2, 1.0, 3.0)        -- show + color + thickness

-- Programmatically set / read the focused element
SetGamepadFocusedElement("PlayButton")      -- focus by element name (current entity's widget)
ClearGamepadFocus()
local name = GetGamepadFocusedElement()     -- → string or ""

-- Navigation mode (keyboard/gamepad focus-highlight visibility)
local navOn = IsUINavigationActive()        -- → bool: true while the focus highlight is shown
SetUINavigationActive(false)                -- force nav mode on (true) / off (false)
```

> **Per-element navigation routing.** Each element also has explicit
> `NavUpID` / `NavDownID` / `NavLeftID` / `NavRightID`, set in the editor or
> from a widget script via `SetNavigation(...)`. If they are not set, the runtime
> falls back to spatial search (closest interactable element in the requested
> direction).

> **SubWidget navigation.** Keyboard/gamepad navigation walks into `SubWidget`
> elements: interactable elements inside a sub-widget (and inside nested
> sub-widgets) are part of the same focus set as the host widget's own elements,
> and the spatial search compares their real on-screen positions, so focus moves
> in and out of a sub-widget naturally. Activation, hover/unhover callbacks,
> state sounds and TTS run in the owning widget's own script environment.
> Explicit `NavUpID` / `NavDownID` / `NavLeftID` / `NavRightID` links stay
> widget-local — use `SetSubElementNavigation(...)` (or
> `SetSubWidgetElementNavigation(...)` from an entity script) to route elements
> inside a sub-widget, and leave the links unset where focus should cross the
> sub-widget boundary via spatial search.
>
> Note that all `SubWidget` elements pointing at the same widget file share one
> runtime state, so repeating the same sub-widget several times inside one host
> yields a single focusable copy of each of its elements.

**Example: Composite HUD with a reusable health bar:**

```lua
-- In HUD.ice_widget there's a SubWidget element "PlayerHealth",
-- which references HealthBar.ice_widget.
-- HealthBar.ice_widget contains: ProgressBar "Bar", Text "Label", Image "Icon"

function OnCreate()
    -- Set initial health
    SetSubWidgetProgress("PlayerHealth", "Bar", 1.0)
    SetSubWidgetText("PlayerHealth", "Label", "100/100")
end

function OnUpdate(dt)
    local hp = GetData("health") or 100
    local maxHp = GetData("maxHealth") or 100
    SetSubWidgetProgress("PlayerHealth", "Bar", hp / maxHp)
    SetSubWidgetText("PlayerHealth", "Label", hp .. "/" .. maxHp)

    -- Highlight red at low health
    if hp < 30 then
        SetSubWidgetElementColor("PlayerHealth", "Bar", 1, 0.2, 0.2, 1)
    else
        SetSubWidgetElementColor("PlayerHealth", "Bar", 0.2, 0.8, 0.2, 1)
    end
end
```

### Widget-Internal API (`.ice_widget` scripts)

Inside `.ice_widget` Lua scripts, shorter function names are available. They operate on elements **of the current widget** by name, without requiring a `Widget` prefix or instance index.

> These are **convenience aliases** — each maps to a longer entity-bound equivalent documented above. Use them **only inside `.ice_widget` scripts**.

#### Element properties (short names)

```lua
-- Transform
SetElementPosition("Title", 100, 50)
local pos = GetElementPosition("Title")         -- → {x, y}
SetElementSize("Title", 200, 40)
local sz = GetElementSize("Title")              -- → {width, height}
SetElementUseDesiredSize("Title", true)         -- size-to-content; Size becomes read-only
local usesDesired = GetElementUseDesiredSize("Title")
local desired = GetElementDesiredSize("Title")  -- → {width, height} (computed, even when disabled)
SetElementRotation("Title", 45)
local rot = GetElementRotation("Title")
SetElementScale("Title", 1.5, 1.5)
local sc = GetElementScale("Title")             -- → {x, y}
SetElementPivot("Title", 0.5, 0.5)
local pv = GetElementPivot("Title")             -- → {x, y}

-- Appearance
SetElementColor("Title", 1, 0.5, 0, 1)
local col = GetElementColor("Title")            -- → {r, g, b, a}
SetElementOpacity("Title", 0.8)
local op = GetElementOpacity("Title")
SetElementFontSize("Title", 24)
SetElementVisible("Title", true)
local vis = IsElementVisible("Title")
SetElementLit("Background", true)
local lit = IsElementLit("Background")
SetElementShadowReceiver("Background", true)   -- shadows fall on this Lit element (default OFF)
local recv = IsElementShadowReceiver("Background")

-- Text and values
SetElementText("Title", "Hello")
local txt = GetElementText("Title")
SetElementValue("Slider", 0.5)
local val = GetElementValue("Slider")

-- Interactivity
SetElementInteractable("Button", true)
local ia = IsElementInteractable("Button")
```

#### Focus and interaction state

```lua
FocusElement("NameInput")
ClearFocus()
local focused = GetFocusedElement()              -- → element name or ""
local hovered = GetHoveredElement()
local pressed = GetPressedElement()
local isFocused = IsElementFocused("NameInput")
local isHovered = IsElementHovered("Button")
local isPressed = IsElementPressed("Button")
```

#### Specific widget types

```lua
-- Toggle
SetToggled("SoundToggle", true)
local on = IsToggled("SoundToggle")

-- Switcher (tab container)
SetActiveChild("TabPanel", 2)
local idx = GetActiveChild("TabPanel")

-- Dropdown
SetDropdownSelected("Language", 1)
local sel = GetDropdownSelected("Language")
local text = GetDropdownSelectedText("Language")

-- Progress bar
SetProgressValue("HealthBar", 0.75)
local pv = GetProgressValue("HealthBar")

-- Throbber (loading spinner)
SetThrobberSpeed("Loader", 2.0)
local spd = GetThrobberSpeed("Loader")
SetThrobberRadius("Loader", 30)
local rad = GetThrobberRadius("Loader")
SetThrobberDots("Loader", 8)
local dots = GetThrobberDots("Loader")
SetThrobberClockwise("Loader", false)
local cw = IsThrobberClockwise("Loader")
-- Pause / resume / reset
SetThrobberPaused("Loader", true)
local paused = IsThrobberPaused("Loader")
PauseThrobber("Loader")
ResumeThrobber("Loader")
ResetThrobber("Loader")                          -- jump time back to 0

-- Slider value range (also valid for ProgressBar)
SetValueRange("Volume", 0, 100)                  -- min, max
local minV = GetMinValue("Volume")
local maxV = GetMaxValue("Volume")

-- Checkbox state
SetChecked("Fullscreen", true)
local on2 = IsChecked("Fullscreen")

-- InputField text
SetInputText("PlayerName", "Hero")
local typed = GetInputText("PlayerName")

-- Fill color (ProgressBar / Slider fill region)
SetFillColor("HealthBar", 0.8, 0.1, 0.1, 1)
local fillCol = GetFillColor("HealthBar")        -- → {r, g, b, a}

-- Checkbox check sprite
SetCheckedSprite("Toggle", "Content/UI/checked.png")

-- Dropdown options (string array)
SetDropdownOptions("Language", {"English", "Русский", "日本語"})
local opts = GetDropdownOptions("Language")       -- → table of strings
local optCount = GetDropdownOptionCount("Language")
```

#### Element styling and visuals (short names)

```lua
-- Sprite / Flipbook
SetElementImage("Icon", "Content/Textures/sword.png")
local img = GetElementImage("Icon")
SetElementFlipbook("Anim", "Content/Flipbooks/fire.ice_flipbook")
local fb = GetElementFlipbook("Anim")

-- Text color
SetElementTextColor("Title", 1, 1, 1, 1)
local tc = GetElementTextColor("Title")          -- → {r, g, b, a}

-- Z-order and global Z
SetElementZOrder("Panel", 5)
local z = GetElementZOrder("Panel")
SetElementGlobalZ("Background", true)            -- participate in global Z sort
local gz = GetElementGlobalZ("Background")

-- Post processing participation (default false = crisp overlay after post processing)
SetElementIsPostProcessed("BackgroundSky", true)
local pp = GetElementIsPostProcessed("BackgroundSky")

-- Anchor preset
SetElementAnchor("Title", "MiddleCenter")
local a = GetElementAnchor("Title")
-- Values: "TopLeft", "TopCenter", "TopRight",
--         "MiddleLeft", "MiddleCenter", "MiddleRight",
--         "BottomLeft", "BottomCenter", "BottomRight",
--         "StretchLeft", "StretchCenter", "StretchRight",
--         "StretchTop", "StretchMiddle", "StretchBottom", "StretchAll"

-- Tooltip
SetElementTooltip("Button", "Click to continue")
SetElementTooltip("Button", "Click to continue", 0.5)   -- + delay (seconds)
local tip = GetElementTooltip("Button")

-- Hover / Pressed colors. Without this, elements do NOT change color on
-- hover/press automatically — drive transitions yourself from callbacks.
SetStateColors("PlayButton", true,
    1.0, 0.9, 0.6, 1.0,    -- HoveredColor (r,g,b,a)
    0.7, 0.5, 0.3, 1.0)    -- PressedColor (r,g,b,a)
SetUseStateColors("PlayButton", true)
SetHoveredColor("PlayButton", 1.0, 0.9, 0.6)
SetPressedColor("PlayButton", 0.7, 0.5, 0.3)

-- Hover / Pressed sounds (same pattern as the colors above)
SetStateSounds("PlayButton", true,
    "Content/Audio/hover.wav",     -- HoveredSound
    "Content/Audio/click.wav")     -- PressedSound
SetUseStateSounds("PlayButton", true)
local useSnd = GetUseStateSounds("PlayButton")   -- → bool
SetHoveredSound("PlayButton", "Content/Audio/hover.wav")
local hovSnd = GetHoveredSound("PlayButton")     -- → string path
SetPressedSound("PlayButton", "Content/Audio/click.wav")
local prsSnd = GetPressedSound("PlayButton")     -- → string path

-- Nine-Slice for stretchable sprite borders
SetNineSlice("Panel", true)                                 -- enable
SetNineSlice("Panel", true, 12, 12, 12, 12)                 -- + border (left, top, right, bottom)

-- Clip child elements to the parent rect
SetClipChildren("ScrollPanel", true)

-- SubWidget path (path to the nested .ice_widget)
SetSubWidgetPath("HealthBar", "Content/UI/NewHealthBar.ice_widget")
local p = GetSubWidgetPath("HealthBar")
```

#### Hierarchy and introspection (short names)

```lua
-- Existence check
local exists = HasElement("Title")

-- Element type as string ("Panel", "Text", "Button", ..., "SubWidget")
local typeName = GetElementType("Title")

-- Tree navigation
local children = GetElementChildren("Panel")     -- → { "Child1", "Child2", ... }
local parentName = GetElementParent("Button")    -- → string or ""
local roots = GetRootElements()                  -- → { "Root1", "Root2", ... }

-- Reparent an element (empty parent name = move to root)
MoveElement("Button", "OtherPanel")
MoveElement("Button", "")                        -- to root
```

#### Callbacks (short names)

```lua
-- Set a callback by type. callbackType is one of:
-- "OnClick", "OnHover", "OnUnhover", "OnPressed", "OnReleased",
-- "OnValueChanged", "OnFocusGained", "OnFocusLost"
SetCallback("PlayButton", "OnClick",    "OnPlayClicked")
SetCallback("PlayButton", "OnHover",    "OnPlayHover")
SetCallback("PlayButton", "OnUnhover",  "OnPlayUnhover")
SetCallback("PlayButton", "OnPressed",  "OnPlayPressed")
SetCallback("PlayButton", "OnReleased", "OnPlayReleased")

-- Read back the currently-bound callback name
local cb = GetCallback("PlayButton", "OnClick")
```

#### Gamepad navigation IDs and configuration (short names)

```lua
-- Set per-element navigation neighbors (pass element names; "" or nil clears)
SetNavigation("PlayBtn",
    "TitleLabel",       -- up
    "OptionsBtn",       -- down
    nil,                -- left (unchanged when nil)
    nil)                -- right (unchanged when nil)

-- Same global gamepad config as on the entity side — available inside widget scripts
SetGamepadEnabled(true)
local on = IsGamepadEnabled()

SetGamepadButton("Activate", "A")
SetGamepadButton("Cancel",   "B")
SetGamepadButton("Up",       "DPadUp")
SetGamepadButton("Down",     "DPadDown")
SetGamepadButton("Left",     "DPadLeft")
SetGamepadButton("Right",    "DPadRight")

SetGamepadNavCooldown(0.2)
SetGamepadStickThreshold(0.5)
SetGamepadSliderStickThreshold(0.3)
SetGamepadSliderStepSize(0.05)
SetGamepadSliderStickSpeed(0.02)
SetGamepadUseLeftStick(true)

SetGamepadFocusBorder(true, 1.0, 0.9, 0.2, 1.0, 2.0)   -- show, r, g, b, a, width

SetGamepadFocusedElement("PlayBtn")
ClearGamepadFocus()
local focused = GetGamepadFocusedElement()             -- → string or ""

local navOn = IsUINavigationActive()                   -- → bool: keyboard/gamepad nav mode active
SetUINavigationActive(false)                           -- force nav mode on (true) / off (false)
```

#### Sub-widget element access (short names)

These are the convenience equivalents of `Get/SetSubWidgetElement*` available inside a widget script. The first argument is the **SubWidget element** name in the current widget; the second is the name of the **inner element** inside the nested `.ice_widget`.

```lua
HasSubElement("HealthBar", "Label")                        -- → bool

SetSubElementText("HealthBar", "Label", "100 HP")
local t = GetSubElementText("HealthBar", "Label")

SetSubElementVisible("HealthBar", "Warning", true)
local vis = IsSubElementVisible("HealthBar", "Warning")

SetSubElementColor("HealthBar", "Fill", 1, 0, 0, 1)
SetSubElementValue("HealthBar", "Bar", 0.75)
local v = GetSubElementValue("HealthBar", "Bar")

SetSubElementImage("HealthBar", "Icon", "Content/UI/heart.png")
SetSubElementSize("HealthBar", "Icon", 32, 32)
SetSubElementPosition("HealthBar", "Icon", 10, 5)
SetSubElementUseDesiredSize("HealthBar", "Icon", true)
local subUses = GetSubElementUseDesiredSize("HealthBar", "Icon")
local subDesired = GetSubElementDesiredSize("HealthBar", "Icon")   -- → {width, height}

-- Bind a callback on an inner element of the SubWidget
-- callbackType: same set as SetCallback above
SetSubElementCallback("Menu", "PlayButton", "OnClick", "OnPlayClicked")
```

#### Layout and scroll

```lua
SetPadding("Panel", 10, 10, 10, 10)             -- left, top, right, bottom
SetSpacing("List", 5)
SetTextWrap("Description", true)
local wrap = IsTextWrap("Description")
SetCustomAnchors("Element", 0, 0, 1, 1)         -- minX, minY, maxX, maxY
ClearCustomAnchors("Element")
SetDragScroll("ScrollPanel", true)
SetScrollOffset("ScrollPanel", 0, 100)
local off = GetScrollOffset("ScrollPanel")       -- → {x, y}
SetMaxLength("NameInput", 20)
local ml = GetMaxLength("NameInput")
SetLocalizationKey("Title", "ui.main.title")
local key = GetLocalizationKey("Title")
```

#### Dynamic elements and animations

```lua
-- Create/remove elements at runtime
local id = CreateElement("Text", "DynamicLabel", "Root")  -- type, name, parent
RemoveElement("DynamicLabel")

-- Element info
local info = GetElement("Title")                 -- → {name, visible, text, ...}

-- Animations
PlayAnimation("FadeIn")
PlayAnimation("FadeIn", 2.0)                     -- with speed
StopAnimation("FadeIn")
StopAnimation("FadeIn", true)                    -- reset to start
PauseAnimation("FadeIn")
ResumeAnimation("FadeIn")
local playing = IsAnimationPlaying("FadeIn")
AddAnimationEvent("FadeIn", 0.5, "OnHalfway")   -- callback at time 0.5
AddAnimationEvent("FadeIn", 1.0, "OnDone", "param")
```

#### Extended element properties (short names)

```lua
-- Font size getter (setter is SetElementFontSize)
local fs = GetElementFontSize("Title")

-- Text alignment / font face
SetElementTextAlign("Title", "Center");   local ha = GetElementTextAlign("Title")
SetElementTextVAlign("Title", "Middle");  local va = GetElementTextVAlign("Title")
SetElementFont("Title", "Content/Fonts/Title.ice_font"); local f = GetElementFont("Title")

-- Panel backdrop blur
SetElementPanelBlur("Glass", true);       local pb = GetElementPanelBlur("Glass")
SetElementPanelBlurStrength("Glass", 16); local pbs = GetElementPanelBlurStrength("Glass")

-- Getters for previously set-only properties
local ns   = GetNineSlice("Panel")        -- → {enabled, left, top, right, bottom}
local clip = GetClipChildren("Panel")
local nav  = GetNavigation("PlayBtn")     -- → {up, down, left, right}
local pad  = GetPadding("Row")            -- → {left, top, right, bottom}
local sp   = GetSpacing("Row")
local drag = GetDragScroll("Scroll")
local chk  = GetCheckedSprite("Toggle")
local ca   = GetCustomAnchors("Panel")    -- → {enabled, minX, minY, maxX, maxY}
local hov  = GetHoveredColor("Btn")       -- → {r, g, b, a}
local prs  = GetPressedColor("Btn")       -- → {r, g, b, a}
local us   = GetUseStateColors("Btn")

-- Tooltip delay
SetElementTooltipDelay("Btn", 0.5);       local td = GetElementTooltipDelay("Btn")

-- Dropdown list height, scrollbars, content extent
SetDropdownMaxHeight("Lang", 240);        local dh = GetDropdownMaxHeight("Lang")
SetScrollbars("Scroll", true, false);     local sb = GetScrollbars("Scroll")   -- → {vertical, horizontal}
SetContentSize("Scroll", 300, 1200);      local cs = GetContentSize("Scroll")  -- → {x, y}

-- SizeBox / slider thumb
SetSizeOverride("Box", true, true, 200, 120); local so = GetSizeOverride("Box")
SetSliderThumbImage("Vol", "Content/UI/knob.png");    local th = GetSliderThumbImage("Vol")
SetSliderThumbFlipbook("Vol", "Content/UI/knob.ice_flipbook"); local tf = GetSliderThumbFlipbook("Vol")

-- Toggle colors / handle
SetToggleColors("Music", 0.3,0.7,0.4,1, 0.5,0.5,0.55,1)
local onc = GetToggleOnColor("Music");    local offc = GetToggleOffColor("Music")
SetToggleHandleRatio("Music", 0.45);      local thr = GetToggleHandleRatio("Music")
SetToggleHandleImage("Music", "Content/UI/handle.png"); local thi = GetToggleHandleImage("Music")

-- Flipbook current frame
SetFlipbookFrame("Anim", 3);              local fr = GetFlipbookFrame("Anim")
```

#### Widget canvas, scaling & safe area (short names)

```lua
SetCanvasSize(1920, 1080);     local canvas = GetCanvasSize()       -- → {x, y}
SetCanvasUseDesiredSize(true);     local canvasAuto = GetCanvasUseDesiredSize()
local canvasDesired = GetCanvasDesiredSize()                        -- → {x, y}
SetScaleWithScreen(true);      local sws = GetScaleWithScreen()
SetStretchMode("Letterbox");   local sm = GetStretchMode()          -- "Stretch"|"Letterbox"|"MatchWidth"|"MatchHeight"
SetSafeArea(true, 10, 20, 10, 20); local sa = GetSafeArea()         -- → {enabled, left, top, right, bottom}
```

#### Element reordering & rename (short names)

```lua
ReorderElementUp("Icon")
ReorderElementDown("Icon")
MoveElementBefore("Icon", "Label")
MoveElementAfter("Icon", "Label")
local applied = SetElementName("OldName", "NewName")   -- returns the uniquified name
```

#### Full sub-element access (short names)

Complete inner-element control for nested `.ice_widget` SubWidgets. First arg = SubWidget element name, second = inner element name.

```lua
-- Introspection
GetSubElementType("HealthBar", "Label")
GetSubRootElements("HealthBar")
GetSubElementChildren("HealthBar", "Panel")
GetSubElementParent("HealthBar", "Icon")

-- Transform getters/setters
SetSubElementRotation("HealthBar", "Icon", 45);  GetSubElementRotation("HealthBar", "Icon")
SetSubElementScale("HealthBar", "Icon", 1.5, 1.5); GetSubElementScale("HealthBar", "Icon")
SetSubElementPivot("HealthBar", "Icon", 0.5, 0.5); GetSubElementPivot("HealthBar", "Icon")
SetSubElementOpacity("HealthBar", "Label", 0.5); GetSubElementOpacity("HealthBar", "Label")
GetSubElementPosition("HealthBar", "Icon")       -- → {x, y}
GetSubElementSize("HealthBar", "Icon")           -- → {width, height}
GetSubElementColor("HealthBar", "Icon")          -- → {r, g, b, a}

-- Appearance
SetSubElementFontSize("HealthBar", "Label", 18); GetSubElementFontSize("HealthBar", "Label")
SetSubElementTextColor("HealthBar", "Label", 1,1,1,1); GetSubElementTextColor("HealthBar", "Label")
SetSubElementFlipbook("HealthBar", "Fx", "Content/FX/spin.ice_flipbook"); GetSubElementFlipbook("HealthBar", "Fx")
GetSubElementImage("HealthBar", "Icon")
SetSubElementZOrder("HealthBar", "Icon", 5);     GetSubElementZOrder("HealthBar", "Icon")
SetSubElementGlobalZ("HealthBar", "Bg", true);   GetSubElementGlobalZ("HealthBar", "Bg")
SetSubElementIsPostProcessed("HealthBar", "Bg", true); GetSubElementIsPostProcessed("HealthBar", "Bg")
SetSubElementTooltip("HealthBar", "Icon", "Health", 0.5); GetSubElementTooltip("HealthBar", "Icon")
SetSubElementAnchor("HealthBar", "Icon", "TopLeft"); GetSubElementAnchor("HealthBar", "Icon")
SetSubElementLit("HealthBar", "Bg", true);       IsSubElementLit("HealthBar", "Bg")
SetSubElementShadowReceiver("HealthBar", "Bg", true); IsSubElementShadowReceiver("HealthBar", "Bg")
SetSubElementPanelBlur("HealthBar", "Glass", true); SetSubElementPanelBlurStrength("HealthBar", "Glass", 16)
SetSubElementTextAlign("HealthBar", "Label", "Center"); SetSubElementTextVAlign("HealthBar", "Label", "Middle")
SetSubElementFont("HealthBar", "Label", "Content/Fonts/F.ice_font"); GetSubElementFont("HealthBar", "Label")
SetSubElementTextWrap("HealthBar", "Label", true); IsSubElementTextWrap("HealthBar", "Label")
SetSubElementLocalizationKey("HealthBar", "Label", "hp"); GetSubElementLocalizationKey("HealthBar", "Label")

-- Interactivity & state colors
SetSubElementInteractable("Menu", "Btn", false); IsSubElementInteractable("Menu", "Btn")
GetSubElementCallback("Menu", "Btn", "OnClick")
SetSubElementUseStateColors("Menu", "Btn", true)
SetSubElementHoveredColor("Menu", "Btn", 1,0.9,0.6,1)
SetSubElementPressedColor("Menu", "Btn", 0.7,0.5,0.3,1)
SetSubElementUseStateSounds("Menu", "Btn", true)
SetSubElementHoveredSound("Menu", "Btn", "Content/Audio/hover.wav")
SetSubElementPressedSound("Menu", "Btn", "Content/Audio/click.wav")

-- Values / fill / range / progress
SetSubElementFillColor("HealthBar", "Bar", 0.8,0.1,0.1,1); GetSubElementFillColor("HealthBar", "Bar")
SetSubElementValueRange("HealthBar", "Bar", 0, 100)
GetSubElementMinValue("HealthBar", "Bar");       GetSubElementMaxValue("HealthBar", "Bar")
SetSubProgressValue("HealthBar", "Bar", 0.75);   GetSubProgressValue("HealthBar", "Bar")

-- Interactive controls
SetSubChecked("Menu", "Sound", true);            IsSubChecked("Menu", "Sound")
SetSubToggled("Menu", "Music", true);            IsSubToggled("Menu", "Music")
SetSubSliderValue("Settings", "Vol", 0.5);       GetSubSliderValue("Settings", "Vol")
SetSubInputText("Form", "Name", "Player1");      GetSubInputText("Form", "Name")
SetSubDropdownOptions("Settings", "Quality", {"Low","Medium","High"})
GetSubDropdownOptions("Settings", "Quality");    SetSubDropdownSelected("Settings", "Quality", 2)
GetSubDropdownSelected("Settings", "Quality");   GetSubDropdownSelectedText("Settings", "Quality")
SetSubSwitcherActiveChild("Tabs", "Pages", 1);   GetSubSwitcherActiveChild("Tabs", "Pages")

-- Layout / scroll / sizebox / slider thumb / toggle
SetSubElementMaxLength("Form", "Name", 16)
SetSubElementSpacing("Menu", "Row", 8);          SetSubElementPadding("Menu", "Row", 4,4,4,4)
SetSubElementCheckedSprite("Menu", "Sound", "Content/UI/checked.png")
SetSubElementNineSlice("Menu", "Panel", true, 12,12,12,12); SetSubElementClipChildren("Menu", "Panel", true)
SetSubElementScrollOffset("Menu", "Scroll", 0, 100); GetSubElementScrollOffset("Menu", "Scroll")
SetSubElementDragScroll("Menu", "Scroll", true); SetSubElementScrollbars("Menu", "Scroll", true, false)
SetSubElementContentSize("Menu", "Scroll", 300, 1200); GetSubElementContentSize("Menu", "Scroll")
SetSubElementSizeOverride("Menu", "Box", true, true, 200, 120)
SetSubElementSliderThumbImage("Settings", "Vol", "Content/UI/knob.png")
SetSubElementSliderThumbFlipbook("Settings", "Vol", "Content/UI/knob.ice_flipbook")
SetSubElementToggleColors("Menu", "Music", 0.3,0.7,0.4,1, 0.5,0.5,0.55,1)
SetSubElementToggleHandleRatio("Menu", "Music", 0.45); SetSubElementToggleHandleImage("Menu", "Music", "Content/UI/h.png")
SetSubElementCustomAnchors("Menu", "Panel", 0,0,1,1); ClearSubElementCustomAnchors("Menu", "Panel")
SetSubElementDropdownMaxHeight("Settings", "Quality", 240); SetSubElementTooltipDelay("Menu", "Btn", 0.5)

-- Throbber / navigation / flipbook frame
SetSubThrobberSpeed("HUD", "Spinner", 2.0);      SetSubThrobberClockwise("HUD", "Spinner", true)
SetSubThrobberRadius("HUD", "Spinner", 30);      SetSubThrobberDots("HUD", "Spinner", 12)
SetSubThrobberPaused("HUD", "Spinner", true)
SetSubElementNavigation("Menu", "PlayBtn", "Title", "OptionsBtn", nil, nil)
SetSubFlipbookFrame("HUD", "Anim", 3);           GetSubFlipbookFrame("HUD", "Anim")

-- Animations on the nested widget
PlaySubAnimation("HealthBar", "Pulse");          PlaySubAnimation("HealthBar", "Pulse", 2.0)
StopSubAnimation("HealthBar", "Pulse");          PauseSubAnimation("HealthBar", "Pulse")
ResumeSubAnimation("HealthBar", "Pulse");        IsSubAnimationPlaying("HealthBar", "Pulse")
```

**Short name → Entity-bound equivalent mapping:**

| Short (`.ice_widget`) | Entity-bound (`.ice_class`) |
|---|---|
| `SetElementPosition` | `SetWidgetElementPosition` |
| `SetElementSize` | `SetWidgetElementSize` |
| `SetElementUseDesiredSize` | `SetWidgetElementUseDesiredSize` |
| `SetCanvasUseDesiredSize` | `SetWidgetCanvasUseDesiredSize` |
| `SetSubElementUseDesiredSize` | `SetSubWidgetElementUseDesiredSize` |
| `SetElementColor` | `SetWidgetElementColor` |
| `SetElementText` | `SetWidgetText` |
| `SetElementVisible` | `SetWidgetElementVisible` |
| `SetElementLit` | `SetWidgetElementLit` |
| `SetElementShadowReceiver` | `SetWidgetElementShadowReceiver` |
| `SetElementImage` | `SetWidgetElementImage` |
| `SetElementFlipbook` | `SetWidgetElementFlipbook` |
| `SetElementTextColor` | `SetWidgetElementTextColor` |
| `SetElementZOrder` | `SetWidgetElementZOrder` |
| `SetElementGlobalZ` | `SetWidgetElementGlobalZ` |
| `SetElementIsPostProcessed` | `SetWidgetElementIsPostProcessed` |
| `SetElementAnchor` | `SetWidgetElementAnchor` |
| `SetElementTooltip` | `SetWidgetElementTooltip` |
| `SetStateColors` | `SetWidgetStateColors` |
| `SetStateSounds` | `SetWidgetStateSounds` |
| `SetFillColor` | `SetFillColor` |
| `SetValueRange` | `SetValueRange` |
| `SetProgressValue` | `SetWidgetProgress` |
| `SetToggled` | `SetToggleState` |
| `SetChecked` | `SetCheckboxChecked` |
| `SetInputText` | `SetInputText` |
| `SetDropdownOptions` | `SetDropdownOptions` |
| `SetActiveChild` | `SetSwitcherActiveChild` |
| `SetCallback` | `SetWidgetCallback` |
| `SetNavigation` | *(per-element NavUp/Down/Left/Right IDs)* |
| `SetSubWidgetPath` | `SetSubWidgetPath` |
| `HasElement` | `HasWidgetElement` |
| `GetElementType` | `GetElementType` |
| `GetElementChildren` | `GetWidgetElementChildren` |
| `GetElementParent` | `GetWidgetElementParent` |
| `GetRootElements` | `GetWidgetRootElements` |
| `MoveElement` | `MoveWidgetElement` |
| `PlayAnimation` | `PlayWidgetAnimation` |
| `CreateElement` | `AddWidgetElement` |
| `FocusElement` | `FocusWidgetElement` |
| `SetGamepadEnabled` *(and all `SetGamepad*` helpers)* | same name on entity side |
| `SetSubElementText` | `SetSubWidgetText` |
| `SetSubElementColor` | `SetSubWidgetElementColor` |

---

## 22. PostProcess — Post-Processing

> **Type:** Global functions. `PP` table + `PP_*` functions.
> Every function is available in two forms: `PP.FunctionName(...)` (table method) and `PP_FunctionName(...)` (global function). They are identical.

### General management

```lua
PP.SetEnabled(true)
PP_SetEnabled(true)
local enabled = PP.IsEnabled()
local enabled2 = PP_IsEnabled()
PP_Reset()  -- Reset all settings
```

### Bloom

```lua
PP.SetBloomEnabled(true)               -- or PP_SetBloomEnabled(true)
local bloomOn = PP.IsBloomEnabled()
local bloomOn2 = PP_IsBloomEnabled()
PP.SetBloomIntensity(1.5)              -- or PP_SetBloomIntensity(1.5)
local bloomIntensity = PP.GetBloomIntensity()
local bloomIntensity2 = PP_GetBloomIntensity()
PP.SetBloomThreshold(0.8)              -- or PP_SetBloomThreshold(0.8)
local bloomThreshold = PP.GetBloomThreshold()
local bloomThreshold2 = PP_GetBloomThreshold()
PP.SetBloomRadius(5.0)                 -- or PP_SetBloomRadius(5.0)
local bloomRadius = PP.GetBloomRadius()
local bloomRadius2 = PP_GetBloomRadius()
```

### Color grading

```lua
PP.SetColorGradingEnabled(true)        -- or PP_SetColorGradingEnabled(true)
local gradingOn = PP.IsColorGradingEnabled()
local gradingOn2 = PP_IsColorGradingEnabled()
PP.SetSaturation(1.2)         -- >1 = more saturated, <1 = less; or PP_SetSaturation(1.2)
local saturation = PP.GetSaturation()
local saturation2 = PP_GetSaturation()
PP.SetContrast(1.1)                    -- or PP_SetContrast(1.1)
local contrast = PP.GetContrast()
local contrast2 = PP_GetContrast()
PP.SetGamma(1.0)                       -- or PP_SetGamma(1.0)
local gamma = PP.GetGamma()
local gamma2 = PP_GetGamma()
PP.SetColorTint(1.0, 0.9, 0.8)        -- Warm tint; or PP_SetColorTint(...)
local tint = PP.GetColorTint()
local tint2 = PP_GetColorTint()
```

### Vignette

```lua
PP.SetVignetteEnabled(true)            -- or PP_SetVignetteEnabled(true)
local vignetteOn = PP.IsVignetteEnabled()
local vignetteOn2 = PP_IsVignetteEnabled()
PP.SetVignetteIntensity(0.5)           -- or PP_SetVignetteIntensity(0.5)
local vignetteIntensity = PP.GetVignetteIntensity()
local vignetteIntensity2 = PP_GetVignetteIntensity()
PP.SetVignetteRadius(0.8)              -- or PP_SetVignetteRadius(0.8)
local vignetteRadius = PP.GetVignetteRadius()
local vignetteRadius2 = PP_GetVignetteRadius()
PP.SetVignetteSoftness(0.5)            -- or PP_SetVignetteSoftness(0.5)
local vignetteSoftness = PP.GetVignetteSoftness()
local vignetteSoftness2 = PP_GetVignetteSoftness()
```

### Film grain

```lua
PP_SetFilmGrainEnabled(true)
local filmGrainOn = PP_IsFilmGrainEnabled()
PP_SetFilmGrainIntensity(0.1)
local filmGrainIntensity = PP_GetFilmGrainIntensity()
```

### Chromatic aberration

```lua
PP_SetChromaticAberrationEnabled(true)
local chromaOn = PP_IsChromaticAberrationEnabled()
PP_SetChromaticAberrationIntensity(0.02)
local chromaIntensity = PP_GetChromaticAberrationIntensity()
```

### Ambient Occlusion

```lua
PP_SetAmbientOcclusionEnabled(true)
local aoOn = PP_IsAmbientOcclusionEnabled()
PP_SetAmbientOcclusionIntensity(0.5)   -- darkening strength (0 = none, 1 = max)
local aoIntensity = PP_GetAmbientOcclusionIntensity()
PP_SetAmbientOcclusionRadius(32.0)     -- sample radius in pixels
local aoRadius = PP_GetAmbientOcclusionRadius()
```

### Screen-Space Reflections (SSR)

Uses the G-buffer (normals, roughness, metallic) and the scene depth buffer to ray-march reflections of the rendered frame on metallic / glossy surfaces. Works automatically on any material whose `Metallic > 0.05` and `Roughness < RoughnessCutoff` (e.g. mirrors, polished metal, wet floors, ice, glass).

```lua
PP.SetSSREnabled(true)                 -- or PP_SetSSREnabled(true)
local ssrOn = PP.IsSSREnabled()        -- or PP_IsSSREnabled()
PP.SetSSRIntensity(0.8)                -- overall reflection strength (0..2)
local ssrI = PP.GetSSRIntensity()
PP.SetSSRMaxDistance(256.0)            -- ray length in pixels (1..2048)
local ssrDist = PP.GetSSRMaxDistance()
PP.SetSSRMaxSteps(48)                  -- ray-march iterations (1..96 hard cap)
local ssrSteps = PP.GetSSRMaxSteps()
PP.SetSSRThickness(2.0)                -- depth-test thickness (0.1..32)
local ssrThk = PP.GetSSRThickness()
PP.SetSSRRoughnessCutoff(0.7)          -- surfaces rougher than this get no SSR (0..1)
local ssrCut = PP.GetSSRRoughnessCutoff()
PP.SetSSRFadeEdge(0.1)                 -- screen-edge fade distance in UV (0..0.5)
local ssrFade = PP.GetSSRFadeEdge()
```

All setters are also blendable per-volume via `ViewAsset` post-process volumes (each field has a matching `SSR*` entry alongside the existing AO/Bloom blends).

### Volumetric Godrays (light shafts)

Radial blur from a configurable screen-space light position, modulated by emissive luminance from the G-buffer (`GBufferMaterial.g`) and occluded by scene depth. Produces volumetric light shafts behind silhouettes — works great for sun rays through trees, shafts through windows, magic portal glow.

```lua
PP.SetGodraysEnabled(true)                  -- or PP_SetGodraysEnabled(true)
local grOn = PP.IsGodraysEnabled()
PP.SetGodraysIntensity(1.5)                 -- overall multiplier (0..4)
PP.SetGodraysSamples(96)                    -- ray-march steps from light to fragment (4..192)
PP.SetGodraysDecay(0.96)                    -- per-step falloff (0.5..1)
PP.SetGodraysWeight(0.4)                    -- per-sample contribution (0..2)
PP.SetGodraysExposure(0.6)                  -- final output multiplier (0..4)
PP.SetGodraysLightScreenPos(0.5, 0.2)       -- light source UV position (sun in upper screen)

local gIntensity = PP.GetGodraysIntensity()
local gSamples  = PP.GetGodraysSamples()
local gDecay    = PP.GetGodraysDecay()
local gWeight   = PP.GetGodraysWeight()
local gExposure = PP.GetGodraysExposure()
local gLx       = PP.GetGodraysLightScreenX()   -- light UV.x
local gLy       = PP.GetGodraysLightScreenY()   -- light UV.y
```

### Procedural Sky (1D gradient + time-of-day)

Renders a sky background BEFORE the scene by sampling a 2D gradient texture treated as a 1D LUT (X = time of day, Y = vertical position from horizon to zenith). Drawn into the scene FBO color attachment 0 with G-buffer normal/material attachments left untouched (preserves G-buffer correctness for SSR/AO/GI).

```lua
PP.SetSkyEnabled(true)
PP.SetSkyTexturePath("Content/Textures/sky_timecycle.png")
PP.SetSkyTimeOfDay(0.5)                     -- 0=midnight, 0.25=dawn, 0.5=noon, 0.75=dusk
PP.SetSkyIntensity(1.0)                     -- color multiplier (0..4)
PP.SetSkyHorizonOffset(0.0)                 -- shifts gradient sampling on Y axis (-1..1)

local skyOn     = PP.IsSkyEnabled()
local skyPath   = PP.GetSkyTexturePath()
local timeOfDay = PP.GetSkyTimeOfDay()
local skyI      = PP.GetSkyIntensity()
local horizon   = PP.GetSkyHorizonOffset()
```

### Global Illumination — Radiance Cascades 2D

Multi-cascade probe-based global illumination that ray-marches the lit emissive G-buffer (`GBufferMaterial.g` + scene color) outward from each pixel along N evenly-spaced rays per cascade. Higher cascades have larger reach but lower angular weight, producing soft indirect bounce lighting for free in 2D scenes (e.g. emissive crystals lighting nearby walls, fire glow on the ground).

```lua
PP.SetGIEnabled(true)
PP.SetGIIntensity(1.0)                      -- bounce strength multiplier (0..4)
PP.SetGICascadeCount(4)                     -- pyramid depth (1..8); each adds 2x reach, halved weight
PP.SetGIBaseRayCount(16)                    -- rays per cascade (4..64)
PP.SetGIMaxDistance(512.0)                  -- cascade-0 reach in pixels (16..2048)

local giOn       = PP.IsGIEnabled()
local giIntensity = PP.GetGIIntensity()
local giCascades = PP.GetGICascadeCount()
local giRays     = PP.GetGIBaseRayCount()
local giDistance = PP.GetGIMaxDistance()
```

All three effects support per-volume blending in `ViewAsset`. `Sky` requires a sky gradient texture (e.g. 256x256 PNG with horizon→sky gradient on Y, time-of-day sweep on X).

### Auto exposure

```lua
PP_SetAutoExposureEnabled(true)
local autoExposure = PP_IsAutoExposureEnabled()
PP_SetExposureCompensation(0.5)
local exposure = PP_GetExposureCompensation()
PP_SetMinExposure(-4.0)
local minExp = PP_GetMinExposure()
PP_SetMaxExposure(4.0)
local maxExp = PP_GetMaxExposure()
PP_SetExposureSpeedUp(3.0)
local speedUp = PP_GetExposureSpeedUp()
PP_SetExposureSpeedDown(1.0)
local speedDown = PP_GetExposureSpeedDown()
```

### Depth of Field

```lua
PP.SetDepthOfFieldEnabled(true)       -- or PP_SetDepthOfFieldEnabled(true)
local dofOn = PP.IsDepthOfFieldEnabled()  -- or PP_IsDepthOfFieldEnabled()
PP.SetFocusDistance(500.0)       -- distance to focus point; or PP_SetFocusDistance(500.0)
local focusDist = PP.GetFocusDistance()   -- or PP_GetFocusDistance()
PP.SetFocusRange(200.0)          -- range around focus; or PP_SetFocusRange(200.0)
local focusRange = PP.GetFocusRange()     -- or PP_GetFocusRange()
PP.SetBlurAmount(1.0)            -- blur strength; or PP_SetBlurAmount(1.0)
local blur = PP.GetBlurAmount()           -- or PP_GetBlurAmount()
```

### LUT (Color Lookup Table)

```lua
PP.SetLUTTexturePath("Content/LUT/cinematic.png")  -- or PP_SetLUTTexturePath(...)
local lutPath = PP.GetLUTTexturePath()              -- or PP_GetLUTTexturePath()
PP.SetLUTIntensity(1.0)          -- 0 = original, 1 = full LUT; or PP_SetLUTIntensity(1.0)
local lutIntensity = PP.GetLUTIntensity()            -- or PP_GetLUTIntensity()
```

### Tonemap (LDR path)

> Tone mapping mode used on the LDR composite path. On the HDR10 path tonemapping is bypassed (PQ encoding handles dynamic range).

```lua
-- Mode constants
PP.TONEMAP_NONE      -- 0: linear clamp to [0,1]
PP.TONEMAP_REINHARD  -- 1: x / (1 + x)
PP.TONEMAP_ACES      -- 2: ACES filmic (default)

PP.SetTonemap(PP.TONEMAP_ACES)     -- or PP_SetTonemap(2)
local mode = PP.GetTonemap()        -- → 0 / 1 / 2; or PP_GetTonemap()
```

### Motion Blur

> Camera/screen-space motion blur driven by reprojected velocities.

```lua
PP.SetMotionBlurEnabled(true)              -- or PP_SetMotionBlurEnabled(true)
local on = PP.IsMotionBlurEnabled()         -- or PP_IsMotionBlurEnabled()

PP.SetMotionBlurIntensity(0.5)              -- 0..1; or PP_SetMotionBlurIntensity(0.5)
local intensity = PP.GetMotionBlurIntensity()

PP.SetMotionBlurSamples(8)                  -- integer sample count; or PP_SetMotionBlurSamples(8)
local samples = PP.GetMotionBlurSamples()

PP.SetMotionBlurMaxBlur(0.05)               -- max blur radius (UV units); or PP_SetMotionBlurMaxBlur(0.05)
local maxBlur = PP.GetMotionBlurMaxBlur()
```

### CAS (Contrast Adaptive Sharpening)

> AMD-style contrast adaptive sharpening pass over the final image.

```lua
PP.SetCASEnabled(true)                      -- or PP_SetCASEnabled(true)
local on = PP.IsCASEnabled()

PP.SetCASSharpness(0.5)                     -- 0..1; or PP_SetCASSharpness(0.5)
local sharpness = PP.GetCASSharpness()
```

### Lens Sharpen

> Radius-based unsharp-mask sharpening centered around the focal area.

```lua
PP.SetLensSharpenEnabled(true)              -- or PP_SetLensSharpenEnabled(true)
local on = PP.IsLensSharpenEnabled()

PP.SetLensSharpenIntensity(0.5)             -- 0..1
local intensity = PP.GetLensSharpenIntensity()

PP.SetLensSharpenRadius(1.0)                -- in pixels
local radius = PP.GetLensSharpenRadius()
```

### Bloom Dirt

> Lens dirt overlay multiplied by the bloom signal. Requires a dirt texture.

```lua
PP.SetBloomDirtEnabled(true)                -- or PP_SetBloomDirtEnabled(true)
local on = PP.IsBloomDirtEnabled()

PP.SetBloomDirtTexturePath("Content/Textures/lens_dirt.png")
local path = PP.GetBloomDirtTexturePath()

PP.SetBloomDirtIntensity(1.0)               -- multiplier
local intensity = PP.GetBloomDirtIntensity()
```

### Lens Flare

> Procedural ghosts/halo lens flare derived from bright pixels.

```lua
PP.SetLensFlareEnabled(true)                -- or PP_SetLensFlareEnabled(true)
local on = PP.IsLensFlareEnabled()

PP.SetLensFlareIntensity(0.5)
local intensity = PP.GetLensFlareIntensity()

PP.SetLensFlareThreshold(1.0)               -- HDR threshold for flare source
local threshold = PP.GetLensFlareThreshold()

PP.SetLensFlareGhostCount(4)                -- integer
local count = PP.GetLensFlareGhostCount()

PP.SetLensFlareGhostDispersal(0.3)          -- spacing of ghosts
local dispersal = PP.GetLensFlareGhostDispersal()

PP.SetLensFlareHaloWidth(0.45)              -- halo radius (UV)
local halo = PP.GetLensFlareHaloWidth()

PP.SetLensFlareChromaDistortion(2.0)        -- chromatic spread on ghosts
local chroma = PP.GetLensFlareChromaDistortion()
```

### Fog

> Distance-based fog with linear / exponential / squared exponential modes plus optional height falloff.

```lua
-- Mode constants
PP.FOG_LINEAR  -- 0
PP.FOG_EXP     -- 1
PP.FOG_EXP2    -- 2

PP.SetFogEnabled(true)                      -- or PP_SetFogEnabled(true)
local on = PP.IsFogEnabled()

PP.SetFogColor(0.6, 0.7, 0.8)               -- RGB 0..1
local r, g, b = PP.GetFogColor()

PP.SetFogDensity(0.02)
local density = PP.GetFogDensity()

PP.SetFogStart(10.0)                        -- linear mode start distance
local s = PP.GetFogStart()

PP.SetFogEnd(200.0)                         -- linear mode end distance
local e = PP.GetFogEnd()

PP.SetFogHeightFalloff(0.1)                 -- 0 disables height attenuation
local falloff = PP.GetFogHeightFalloff()

PP.SetFogHeightOffset(0.0)                  -- world Y reference
local offset = PP.GetFogHeightOffset()

PP.SetFogMode(PP.FOG_EXP2)                  -- 0 / 1 / 2
local mode = PP.GetFogMode()
```

### Volumetric Fog

> Ray-marched single-scattering fog around the directional light.

```lua
PP.SetVolumetricFogEnabled(true)            -- or PP_SetVolumetricFogEnabled(true)
local on = PP.IsVolumetricFogEnabled()

PP.SetVolumetricFogIntensity(1.0)
local intensity = PP.GetVolumetricFogIntensity()

PP.SetVolumetricFogSteps(32)                -- integer raymarch steps
local steps = PP.GetVolumetricFogSteps()

PP.SetVolumetricFogScattering(0.5)          -- Henyey-Greenstein g, -1..1
local scattering = PP.GetVolumetricFogScattering()

PP.SetVolumetricFogDensity(0.1)
local density = PP.GetVolumetricFogDensity()

PP.SetVolumetricFogColor(1.0, 0.95, 0.85)
local r, g, b = PP.GetVolumetricFogColor()

PP.SetVolumetricFogLightPos(0.5, 0.5)       -- screen-space god-ray origin (UV 0..1)
local x, y = PP.GetVolumetricFogLightPos()
```

### Heat Haze

> Animated UV displacement that mimics heat shimmer, masked by a vertical gradient.

```lua
PP.SetHeatHazeEnabled(true)                 -- or PP_SetHeatHazeEnabled(true)
local on = PP.IsHeatHazeEnabled()

PP.SetHeatHazeIntensity(0.02)               -- displacement strength (UV)
local intensity = PP.GetHeatHazeIntensity()

PP.SetHeatHazeSpeed(1.0)                    -- animation speed
local speed = PP.GetHeatHazeSpeed()

PP.SetHeatHazeScale(20.0)                   -- noise frequency
local scale = PP.GetHeatHazeScale()

PP.SetHeatHazeMaskTop(0.6)                  -- top of haze band (UV.y)
local top = PP.GetHeatHazeMaskTop()

PP.SetHeatHazeMaskBottom(0.0)               -- bottom of haze band (UV.y)
local bottom = PP.GetHeatHazeMaskBottom()
```

### Underwater

> Underwater look: tint, distorted UVs and animated caustics.

```lua
PP.SetUnderwaterEnabled(true)               -- or PP_SetUnderwaterEnabled(true)
local on = PP.IsUnderwaterEnabled()

PP.SetUnderwaterTint(0.1, 0.4, 0.6)         -- RGB 0..1
local r, g, b = PP.GetUnderwaterTint()

PP.SetUnderwaterTintIntensity(0.6)          -- 0..1 mix toward tint
local tintIntensity = PP.GetUnderwaterTintIntensity()

PP.SetUnderwaterDistortion(0.01)            -- UV displacement
local distortion = PP.GetUnderwaterDistortion()

PP.SetUnderwaterDistortionSpeed(1.0)
local distortionSpeed = PP.GetUnderwaterDistortionSpeed()

PP.SetUnderwaterDistortionScale(15.0)
local distortionScale = PP.GetUnderwaterDistortionScale()

PP.SetUnderwaterCausticsIntensity(0.5)
local causticsIntensity = PP.GetUnderwaterCausticsIntensity()

PP.SetUnderwaterCausticsScale(8.0)
local causticsScale = PP.GetUnderwaterCausticsScale()

PP.SetUnderwaterCausticsSpeed(1.0)
local causticsSpeed = PP.GetUnderwaterCausticsSpeed()
```

### Custom Post-Process Materials

> Attach node-based **Post Process** domain materials (Material Editor → Domain = Post Process) to the active view. Each material runs as a full-screen pass, samples the scene and blends by a per-material strength (0..1). A per-material **order** decides whether it runs **before** all built-in effects (so bloom, color grading, vignette, etc. apply on top of it) or **after** them (the default). Only materials whose domain is **Post Process** are applied; surface materials are ignored. Paths point to `.ice_material` assets and are matched after normalization, so either path- or index-based addressing works.

```lua
-- Add a material (strength defaults to 1.0, order defaults to 0 = after). Returns the new material count.
local count = PP.AddCustomMaterial("Content/PP_Invert.ice_material", 0.75)
-- Optional 3rd arg = order: 0 = after all built-in effects (default), 1 = before all built-in effects.
local count2 = PP.AddCustomMaterial("Content/PP_Grade.ice_material", 1.0, 1)

-- Query
local n        = PP.GetCustomMaterialCount()
local has      = PP.HasCustomMaterial("Content/PP_Invert.ice_material")
local path     = PP.GetCustomMaterialPath(1)        -- 1-based index, "" if out of range
local strength = PP.GetCustomMaterialStrength("Content/PP_Invert.ice_material")
local enabled  = PP.IsCustomMaterialEnabled("Content/PP_Invert.ice_material")
local location = PP.GetCustomMaterialLocation("Content/PP_Invert.ice_material")  -- 0 = after all effects, 1 = before all effects

-- Tune by path...
PP.SetCustomMaterialStrength("Content/PP_Invert.ice_material", 0.5)  -- clamped 0..1
PP.SetCustomMaterialEnabled("Content/PP_Invert.ice_material", false)
PP.SetCustomMaterialLocation("Content/PP_Invert.ice_material", 1)    -- 0 = after all effects, 1 = before all effects

-- ...or by 1-based index (handles duplicates and enumeration)
PP.SetCustomMaterialStrengthAt(1, 0.5)
PP.SetCustomMaterialEnabledAt(1, true)
PP.SetCustomMaterialLocationAt(1, 0)

-- Remove
PP.RemoveCustomMaterial("Content/PP_Invert.ice_material")  -- returns true if removed
PP.RemoveCustomMaterialAt(1)                               -- 1-based, returns true if removed
PP.ClearCustomMaterials()
```

> Note: changes target the **active** post-process settings (like the other `PP.*` functions) and are not persisted to the `.ice_view` asset. To make a material permanent, add it in the View Editor → Post Process → Custom Post Process.

### Custom Post-Process Material Parameters

> Post-process materials can expose **Scalar Parameter**, **Vector Parameter** and **Texture Parameter** nodes just like surface
> materials. These functions feed those parameters per material, every frame, straight from Lua — no Material Parameter
> Collection needed. Overrides are keyed by material path and survive strength/enable changes and volume blending.

```lua
local MAT = "Content/PP_Raycast.ice_material"

PP.SetCustomMaterialScalar(MAT, "PlayerAngle", angle)          -- float parameter
PP.SetCustomMaterialVector(MAT, "PlayerPos", x, y, 0, 1)       -- vec4 parameter (a defaults to 1)
PP.SetCustomMaterialTexture(MAT, "MapData", "levelmap")        -- texture parameter; accepts a
                                                               -- file path or a Texture.Create name

local angleNow = PP.GetCustomMaterialScalar(MAT, "PlayerAngle")
local posNow   = PP.GetCustomMaterialVector(MAT, "PlayerPos")  -- returns { r, g, b, a }

PP.ClearCustomMaterialParams(MAT)      -- drop overrides for one material
PP.ClearAllCustomMaterialParams()      -- drop every override
```

> The setters load the material if it is not loaded yet and return `false` if it cannot be found.
> Parameter names must match the **Parameter Name** field of the node in the Material Editor.
> Material Parameter Collections (`MPC.*`) still work and are still the right tool for values shared by many materials.

### Post-Process Volume callbacks

> Subscribe Lua callbacks to per-volume enter/exit events. Volumes are identified by their name (as authored in the scene). The callback receives the volume name as a single argument.

```lua
PP.OnVolumeEnter("CaveVolume", function(name)
    print("Entered:", name)
end)

PP.OnVolumeExit("CaveVolume", function(name)
    print("Exited:", name)
end)

-- Pass nil to clear a single direction:
PP.OnVolumeEnter("CaveVolume", nil)

-- Remove both enter and exit callbacks for a single volume:
PP.RemoveVolumeCallback("CaveVolume")

-- Drop every registered callback:
PP.ClearVolumeCallbacks()
```

---

## 23. Cinema — Cinematics

> **Type:** Global functions. `Cinema` table.
>
> Cinematics are created in `.ice_cinema` files with a timeline and can call Lua functions.

```lua
-- Playback
Cinema.Play("Content/Cinema/intro.ice_cinema")
Cinema.Stop("Content/Cinema/intro.ice_cinema")
Cinema.Pause("Content/Cinema/intro.ice_cinema")
Cinema.Resume("Content/Cinema/intro.ice_cinema")

-- With blend-in
Cinema.PlayWithBlend("Content/Cinema/intro.ice_cinema", 0.5)

-- Checks
local playing = Cinema.IsPlaying("Content/Cinema/intro.ice_cinema")
local paused = Cinema.IsPaused("Content/Cinema/intro.ice_cinema")
local controlling = Cinema.IsControllingCamera()
local anyPlaying = Cinema.IsAnyPlaying()

-- Time
local t = Cinema.GetTime("Content/Cinema/intro.ice_cinema")
Cinema.SetTime("Content/Cinema/intro.ice_cinema", 5.0)
local dur = Cinema.GetDuration("Content/Cinema/intro.ice_cinema")

-- Progress (0.0 — 1.0)
local progress = Cinema.GetProgress("Content/Cinema/intro.ice_cinema")

-- Playback rate
Cinema.SetPlaybackRate("Content/Cinema/intro.ice_cinema", 2.0) -- 2x speed
local rate = Cinema.GetPlaybackRate("Content/Cinema/intro.ice_cinema")

-- Skip
Cinema.Skip("Content/Cinema/intro.ice_cinema")

-- Stop all playing cinematics
Cinema.StopAll()

-- Reload
Cinema.Reload("Content/Cinema/intro.ice_cinema")

-- Blend weight
Cinema.SetBlendWeight("Content/Cinema/intro.ice_cinema", 0.5)

-- Loop control
Cinema.SetLoop("Content/Cinema/intro.ice_cinema", true)
local looping = Cinema.GetLoop("Content/Cinema/intro.ice_cinema")

-- Current cinematic name
local name = Cinema.GetName()

-- Fade effects (independent of keyframes)
Cinema.FadeOut(1.0)              -- fade to black in 1 second
Cinema.FadeOut(1.0, 1, 0, 0)     -- fade to red in 1 second
Cinema.FadeIn(0.5)               -- fade in from black in 0.5 seconds
Cinema.FadeIn(0.5, 1, 1, 1)      -- fade in from white in 0.5 seconds

-- Fade state queries
local fading = Cinema.IsFading()
local alpha = Cinema.GetFadeAlpha()       -- 0.0 (transparent) to 1.0 (opaque)
local color = Cinema.GetFadeColor()       -- → {r, g, b, a}

-- Camera info (while playing)
local camPos = Cinema.GetCameraPosition()  -- → {x, y, z}
local camZoom = Cinema.GetCameraZoom()

-- Camera shake (independent of keyframes)
Cinema.ShakeCamera(5.0, 10.0, 0.5)  -- intensity, frequency, duration

-- List of currently playing cinematics
local list = Cinema.GetPlayingList()  -- → {"path1", "path2", ...}

-- Completion callback
Cinema.OnFinished("Content/Cinema/intro.ice_cinema", function(path)
    print("Cinema finished: " .. path)
end)
Cinema.ClearOnFinished("Content/Cinema/intro.ice_cinema")
```

> In `.ice_cinema`, you can add **Lua Callback** keyframes — they call global Lua functions at a specific time. These functions are defined in the level script or globally (class/widget/AI scripts).
>
> The engine also calls two global functions automatically:
> - `OnCinemaStart()` — fired right after `Cinema.Play` (or world-asset autoplay/trigger). Use `Cinema.GetName()` to disambiguate if multiple cinemas can play.
> - `OnCinemaEnd()` — fired when a cinema ends naturally, is skipped, or reverse-finishes.
>
> For per-path completion logic, prefer `Cinema.OnFinished(path, callback)` — it's cleaner than checking names inside `OnCinemaEnd`.

| Function | Description |
|---|---|
| `Cinema.Play(path)` | Start cinematic playback |
| `Cinema.Stop(path)` | Stop and reset the cinematic |
| `Cinema.Pause(path)` | Pause playback |
| `Cinema.Resume(path)` | Resume playback |
| `Cinema.PlayWithBlend(path, blendIn)` | Play with blend-in |
| `Cinema.Skip(path)` | Skip to the end |
| `Cinema.StopAll()` | Stop all playing cinematics at once |
| `Cinema.Reload(path)` | Reload the cinematic from disk |
| `Cinema.IsPlaying(path)` | Check if playing |
| `Cinema.IsPaused(path)` | Check if paused |
| `Cinema.IsAnyPlaying()` | Check if any cinematic is playing |
| `Cinema.IsControllingCamera()` | Check if cinematic controls the camera |
| `Cinema.GetTime(path)` | Get current playback time |
| `Cinema.SetTime(path, time)` | Set playback time |
| `Cinema.GetDuration(path)` | Get total duration |
| `Cinema.GetProgress(path)` | Get progress 0.0–1.0 |
| `Cinema.SetPlaybackRate(path, rate)` | Set playback rate (-10.0–10.0, negative = reverse) |
| `Cinema.GetPlaybackRate(path)` | Get playback rate |
| `Cinema.SetBlendWeight(path, weight)` | Set blend weight (0.0–1.0) |
| `Cinema.SetLoop(path, loop)` | Enable/disable looping at runtime |
| `Cinema.GetLoop(path)` | Check if looping is enabled |
| `Cinema.GetName()` | Get current cinematic name |
| `Cinema.FadeIn(duration, r?, g?, b?)` | Fade in from color (default black) |
| `Cinema.FadeOut(duration, r?, g?, b?)` | Fade out to color (default black) |
| `Cinema.IsFading()` | Check if a fade effect is active |
| `Cinema.GetFadeAlpha()` | Get current fade opacity (0.0–1.0) |
| `Cinema.GetFadeColor()` | Get fade color as `{r, g, b, a}` table |
| `Cinema.GetCameraPosition()` | Get cinematic camera position `{x, y, z}` |
| `Cinema.GetCameraZoom()` | Get cinematic camera zoom |
| `Cinema.ShakeCamera(intensity, frequency, duration)` | Trigger camera shake effect (independent of keyframes) |
| `Cinema.GetPlayingList()` | Get list of currently playing cinematic paths |
| `Cinema.OnFinished(path, callback)` | Register a callback for when the cinematic finishes |
| `Cinema.ClearOnFinished(path)` | Remove the completion callback for a cinematic |

---

## 24. Settings — Game Settings

> **Type:** Global functions. `Settings` table.

### Graphics

```lua
Settings.SetFPSLimit(60)
local fps = Settings.GetFPSLimit()

Settings.SetResolution(1920, 1080)
local res = Settings.GetResolution()  -- → {width, height}

Settings.SetFullscreen(true)
local fs = Settings.IsFullscreen()

Settings.SetWindowMode("windowed")    -- "windowed", "fullscreen", "borderless"
local mode = Settings.GetWindowMode()

Settings.SetVSync(true)
local vsync = Settings.IsVSync()

Settings.SetLowLatencyMode(false)            -- see "Low Latency Mode" section
local ll = Settings.IsLowLatencyMode()

Settings.SetProjectPrewarm(false)            -- see "Project Pre-Warm" section
local pp = Settings.IsProjectPrewarm()

Settings.SetIsSuspended(true)                -- see "Suspend" section
local sus = Settings.IsSuspended()

Settings.SetHDR10(true)
local hdr = Settings.IsHDR10()

Settings.SetHDR10PaperWhiteNits(200.0)
local pw = Settings.GetHDR10PaperWhiteNits()

Settings.SetHDR10MaxLuminanceNits(1000.0)
local maxNits = Settings.GetHDR10MaxLuminanceNits()

-- Screen size (physical monitor)
local screen = Settings.GetScreenSize()  -- → {width, height}
```

### Quality

```lua
-- Universal render scale (range: 0.01 .. 2.0, i.e. 1% .. 200%)
-- Drives both offscreen render resolution and particle quality scale.
-- Combines multiplicatively with SSAA modes (ssaa_2x adds ~1.41x, ssaa_4x adds 2.0x).
Settings.SetRenderScale(1.0)
local rs = Settings.GetRenderScale()  -- float

-- AA mode (recommended): canonical string-based API for all AA techniques
-- Constants:
--   Settings.AA_MODE_OFF      -- "off"      (no anti-aliasing)
--   Settings.AA_MODE_FXAA     -- "fxaa"     (post-process, fast)
--   Settings.AA_MODE_MSAA_2X  -- "msaa_2x"  (multisample, requires restart)
--   Settings.AA_MODE_MSAA_4X  -- "msaa_4x"  (multisample, requires restart)
--   Settings.AA_MODE_MSAA_8X  -- "msaa_8x"  (multisample, requires restart)
--   Settings.AA_MODE_SSAA_2X  -- "ssaa_2x"  (super-sampling, ~1.41x render scale)
--   Settings.AA_MODE_SSAA_4X  -- "ssaa_4x"  (super-sampling, 2.0x render scale)
Settings.SetAAMode(Settings.AA_MODE_FXAA)
local mode = Settings.GetAAMode()  -- → "off" | "fxaa" | "msaa_*" | "ssaa_*"

-- MSAA Alpha-To-Coverage (only effective when AA mode is one of msaa_*)
Settings.SetMSAAAlphaToCoverage(true)
local a2c = Settings.IsMSAAAlphaToCoverage()

-- Audio quality (sample rate):
--   AUDIO_VERY_LOW(0)    = 8000 Hz
--   AUDIO_LOW(1)         = 16000 Hz
--   AUDIO_MEDIUM_LOW(2)  = 22050 Hz
--   AUDIO_MEDIUM(3)      = 44100 Hz
--   AUDIO_HIGH(4)        = 48000 Hz   (default)
--   AUDIO_VERY_HIGH(5)   = 96000 Hz
Settings.SetAudioQuality(Settings.AUDIO_HIGH)
local aq = Settings.GetAudioQuality()

-- Lighting
Settings.SetLightingEnabled(true)    -- true = Lit, false = Unlit
local lit = Settings.IsLightingEnabled()

-- Lighting constants
Settings.LIGHTING_LIT    -- true
Settings.LIGHTING_UNLIT  -- false
```

### Upscaling (FSR / NIS)

> Renders the scene at a lower internal resolution and reconstructs it to the output resolution at the very end of the frame, **after** post-processing. **Vulkan, Direct3D 12 and Metal only, and only on a compatible GPU** — exactly like ray tracing. On any other backend or an unsupported device the setting is still stored, but it has no effect and the frame falls back to the plain linear filter.
>
> - **`"fsr"`** — AMD FidelityFX Super Resolution 1.0: EASU (edge-adaptive spatial upsampling) followed by RCAS (robust contrast-adaptive sharpening). Runs on **any** Vulkan, Direct3D 12 or Metal GPU.
> - **`"nis"`** — NVIDIA Image Scaling: structure-tensor directional scaling with an edge-adaptive unsharp mask, in a single pass. Requires an **NVIDIA** GPU.
>
> The quality preset sets the internal render resolution as a fraction of the output resolution and **multiplies** with `SetRenderScale()` and the SSAA modes, so leave `RenderScale` at `1.0` to let the preset drive the resolution on its own. Defaults to `"off"`.

```lua
-- Upscaler constants
--   Settings.UPSCALING_OFF   -- "off"   (plain linear filter, default)
--   Settings.UPSCALING_FSR   -- "fsr"   (AMD FidelityFX Super Resolution 1.0)
--   Settings.UPSCALING_NIS   -- "nis"   (NVIDIA Image Scaling, NVIDIA GPUs only)
Settings.SetUpscalingMode(Settings.UPSCALING_FSR)
local upscaler = Settings.GetUpscalingMode()   -- → "off" | "fsr" | "nis"

-- Quality preset constants (internal render resolution vs. output resolution)
--   Settings.UPSCALING_ULTRA_PERFORMANCE  -- "ultra_performance"   33%
--   Settings.UPSCALING_PERFORMANCE        -- "performance"         50%
--   Settings.UPSCALING_BALANCED           -- "balanced"            59%
--   Settings.UPSCALING_QUALITY            -- "quality"             67%  (default)
--   Settings.UPSCALING_ULTRA_QUALITY      -- "ultra_quality"       77%
--   Settings.UPSCALING_NATIVE             -- "native"             100%  (sharpening pass only)
Settings.SetUpscalingQuality(Settings.UPSCALING_QUALITY)
local preset = Settings.GetUpscalingQuality()  -- → "quality"

-- Sharpening pass (FSR → RCAS, NIS → adaptive unsharp mask)
Settings.SetUpscalingSharpening(true)
local sharpening = Settings.IsUpscalingSharpening()

Settings.SetUpscalingSharpness(0.5)            -- 0.0 .. 1.0 (default 0.5)
local sharpness = Settings.GetUpscalingSharpness()

-- Capability queries
local anySupported = Settings.IsUpscalingSupported()                        -- any upscaler on this device
local fsrSupported = Settings.IsUpscalingSupported(Settings.UPSCALING_FSR)  -- a specific one
local nisSupported = Settings.IsUpscalingSupported(Settings.UPSCALING_NIS)

local active = Settings.IsUpscalingActive()       -- selected AND supported right now
local factor = Settings.GetUpscalingRenderScale() -- 1.0 when inactive, otherwise the preset ratio
```

```lua
-- Typical in-game graphics menu: pick the best upscaler the device supports
if Settings.IsUpscalingSupported(Settings.UPSCALING_NIS) then
    Settings.SetUpscalingMode(Settings.UPSCALING_NIS)
elseif Settings.IsUpscalingSupported(Settings.UPSCALING_FSR) then
    Settings.SetUpscalingMode(Settings.UPSCALING_FSR)
end

if Settings.IsUpscalingActive() then
    Settings.SetRenderScale(1.0)                              -- let the preset drive the resolution
    Settings.SetUpscalingQuality(Settings.UPSCALING_BALANCED)
    Settings.SetUpscalingSharpness(0.6)
    Settings.Save()
end
```

### Renderer / Graphics API

> Read the active graphics backend or request a different one. `GetRenderer()` works on **every platform** and returns the renderer currently in use. `SetRenderer(name)` writes the requested backend to `Config/Engine.json` → `Rendering.RenderBackend` and **takes effect on the next launch** — the backend is selected once at startup when the window and graphics context are created, so a live in-session switch is not performed. On platforms where the backend is fixed by the build (Web → WebGPU or WebGL 2.0, chosen at build time with automatic WebGL 2.0 fallback when the browser lacks WebGPU; macOS → native Metal, Metal via ANGLE or Metal via MoltenVK, whichever the build was made with; iOS → native Metal or Metal via MoltenVK; Android → the backend the APK was built with), the value is still persisted but the active renderer stays what the platform provides; any backend not compiled into the build gracefully falls back at startup.
>
> Accepted names are case- and spacing-insensitive: `Direct3D 12` (also `D3D12`, `DirectX 12`, `DX12`), `Vulkan`, `OpenGL 4.6`, `OpenGL 3.3`, `OpenGL ES 3.2`, `OpenGL ES 3.0` (also `WebGL 2.0` — the same GLSL ES 3.0 feature set, reported as *OpenGL ES 3.0* on Android and as *WebGL 2.0* on Web), `WebGPU`, `Metal` (native, also `MetalNative`), `Metal (ANGLE)` (also `MetalANGLE`, `ANGLE`) and `Metal (MoltenVK)` (also `MetalMoltenVK`, `MoltenVK`). The Vulkan backend negotiates **1.4 → 1.3 → 1.2 → 1.1** at runtime and falls back to OpenGL 4.6 → OpenGL 3.3 (desktop) / OpenGL ES 3.2 → OpenGL ES 3.0 (Android) when no compatible device is available. `Direct3D 12` is Windows-only and needs feature level 11_0; when no usable adapter is present it falls back to **Vulkan → OpenGL 4.6 → OpenGL 3.3**, and on non-Windows platforms the value is persisted but the platform's own backend stays active. `Metal` is macOS/iOS-only and needs a Metal device that can create a command queue; when none is present it falls back to **Metal (MoltenVK) → Metal (ANGLE)** on macOS and to **Metal (MoltenVK)** on iOS. On Web, `WebGPU` falls back to `WebGL 2.0` at startup when the browser does not support WebGPU.

```lua
-- Read the active renderer (all platforms)
local renderer = Settings.GetRenderer()    -- e.g. "Metal", "Direct3D 12", "Vulkan", "OpenGL 4.6", "OpenGL 3.3", "OpenGL ES 3.2", "OpenGL ES 3.0", "WebGL 2.0", "WebGPU", "Metal (ANGLE)"

-- Request a renderer; persisted to config, applied on next launch. Returns true on success.
local ok = Settings.SetRenderer("Vulkan")
Settings.SetRenderer("Direct3D 12")         -- Windows-only, falls back to Vulkan/OpenGL
Settings.SetRenderer("Metal")               -- macOS/iOS-only native Metal, falls back to MoltenVK/ANGLE
Settings.SetRenderer("OpenGL 4.6")          -- explicit OpenGL on desktop
```

### Optimization / Performance

> Global render-tuning knobs that mirror the editor's **Optimization** tab. Every setter clamps its value to a safe range and is persisted to `GameSettings.json`; values authored in `Config/Engine.json` → `Optimization` are applied at startup **before** the renderer initializes.
>
> - **Applied immediately:** `CullPadding`, `CullingMode`, `IsSuspended`.
> - **Requires restart** (GPU buffers/shaders, texture atlases, and the cached device texture limit are built once at startup from these values): `MaxPointLights`, `MaxTextureSlotsGL`, `MaxTextureSlotsGLES`, `MaxQuadsPerBatch`, `MaxParticlesPerBatch`, `MaxInstancesPerBatch`, `DebugVertexBufferSize`, `AtlasSize`, `MaxSpriteSize`, `MaxTextureSize`.

```lua
-- Culling
Settings.SetCullPadding(256.0)            -- off-screen cull margin in px (range: 0 .. 16384)
local pad = Settings.GetCullPadding()

Settings.SetCullingMode(0)                -- 0 = AABB (fast), 1 = PerPixel (strict)
local cm = Settings.GetCullingMode()

-- Auto-suspend when the window is unfocused / minimized (see "Suspend" section)
Settings.SetIsSuspended(true)             -- default: true
local sus = Settings.IsSuspended()

-- Batching: GPU buffers are allocated to exactly the configured value (no waste),
-- so raising these costs more VRAM and lowering them saves it. Require restart.
Settings.SetMaxQuadsPerBatch(50000)       -- range: 1 .. 1000000
local quads = Settings.GetMaxQuadsPerBatch()
Settings.SetMaxParticlesPerBatch(10000)   -- range: 1 .. 1000000
local parts = Settings.GetMaxParticlesPerBatch()
Settings.SetMaxInstancesPerBatch(100000)  -- range: 1 .. 1000000
local inst = Settings.GetMaxInstancesPerBatch()

-- Texture slots per batch (require restart; effective count is also clamped at startup to the GPU/renderer limit)
Settings.SetMaxTextureSlotsGL(14)         -- desktop OpenGL, range: 1 .. 32
local slGL = Settings.GetMaxTextureSlotsGL()
Settings.SetMaxTextureSlotsGLES(12)       -- GLES / WebGL2 / Metal-ANGLE, range: 1 .. 32
local slES = Settings.GetMaxTextureSlotsGLES()

-- Active point-light limit (requires restart; hard cap 128 = UBO array size)
Settings.SetMaxPointLights(32)            -- range: 1 .. 128
local mpl = Settings.GetMaxPointLights()

-- Texture atlas / textures (require restart)
Settings.SetAtlasSize(4096)               -- atlas page size in px (range: 256 .. 16384)
local atl = Settings.GetAtlasSize()
Settings.SetMaxSpriteSize(2048)           -- largest sprite packed into the shared atlas (range: 64 .. 16384)
local mss = Settings.GetMaxSpriteSize()
Settings.SetMaxTextureSize(0)             -- upper cap for GPU max texture size; 0 = use the real device maximum (range: 0 .. 65536)
local mts = Settings.GetMaxTextureSize()

-- Debug renderer vertex buffer (requires restart)
Settings.SetDebugVertexBufferSize(128)    -- range: 16 .. 4096
local dvb = Settings.GetDebugVertexBufferSize()
```

### Sound (via Settings)

```lua
Settings.SetMasterVolume(1.0)
local master = Settings.GetMasterVolume()
Settings.SetMusicVolume(0.5)
local music = Settings.GetMusicVolume()
Settings.SetSFXVolume(0.8)
local sfx = Settings.GetSFXVolume()
Settings.SetVoiceVolume(1.0)
local voice = Settings.GetVoiceVolume()
Settings.SetAmbientVolume(0.6)
local ambient = Settings.GetAmbientVolume()
Settings.SetUIVolume(0.7)
local ui = Settings.GetUIVolume()
Settings.SetMuted(false)
local muted = Settings.IsMuted()
```

### Platform

The engine supports 6 platforms: **Windows**, **Linux**, **macOS**, **iOS**, **Android**, **Web**. From Lua you can detect the current platform and write platform-specific code (or code shared across all platforms). For the CPU architecture that same build was compiled for, see the **Architecture** section right below.

```lua
local platform = Settings.GetPlatform()
-- Returns one of: "Windows", "Linux", "macOS", "iOS", "Android", "Web"
-- ("Unknown" only if compiled for an unrecognized target)

-- Per-platform boolean checks (one returns true on the current build):
local isWindows = Settings.IsWindows()
local isLinux   = Settings.IsLinux()
local isMacOS   = Settings.IsMacOS()
local isIOS     = Settings.IsIOS()
local isAndroid = Settings.IsAndroid()
local isWeb     = Settings.IsWeb()

-- Family checks:
local isApple     = Settings.IsApple()      -- macOS or iOS
local isMobile    = Settings.IsMobile()     -- iOS or Android (native build)
local isMobileWeb = Settings.IsMobileWeb()  -- true ONLY on the Web build when the browser runs on a mobile device (UA + touch + pointer:coarse). Always false on every other platform.
local isDesktop   = Settings.IsDesktop()    -- Windows, Linux or macOS

-- Platform string constants (handy for switch-style logic and comparisons):
Settings.PLATFORM_WINDOWS  -- "Windows"
Settings.PLATFORM_LINUX    -- "Linux"
Settings.PLATFORM_MACOS    -- "macOS"
Settings.PLATFORM_IOS      -- "iOS"
Settings.PLATFORM_ANDROID  -- "Android"
Settings.PLATFORM_WEB      -- "Web"

-- Patterns:

-- 1) Run code only on a single platform
if Settings.IsAndroid() then
    -- Android-only logic (e.g. ads, IAP, permissions)
end

-- 2) Different code per platform
if Settings.IsMobile() or Settings.IsMobileWeb() then
    -- touch input, larger UI, battery-friendly settings
    -- IMPORTANT: use this exact pair so the virtual sticks/buttons appear
    -- both in the native mobile build and when a phone visits the same
    -- Web build through the browser.
elseif Settings.IsWeb() then
    -- desktop browser (mouse + keyboard)
else
    -- desktop branch (Windows / Linux / macOS)
end

-- 3) Switch-style on the platform name
local p = Settings.GetPlatform()
if p == Settings.PLATFORM_WINDOWS then
    -- ...
elseif p == Settings.PLATFORM_MACOS or p == Settings.PLATFORM_LINUX then
    -- shared desktop-Unix branch
elseif p == Settings.PLATFORM_IOS then
    -- ...
end

-- 4) Code that runs on ALL 6 platforms — just don't gate it: the
--    same script works everywhere because all platforms expose the
--    same Lua API surface.
```

### Architecture

`Settings.GetPlatform()` answers *which OS*, `Settings.GetArch()` answers *which CPU the build was compiled for*. That is the level at which "this device is weak" is actually decided: an old 32-bit ARM phone and a modern 64-bit one both report `"Android"`, but only one of them is stuck inside a 32-bit address space.

```lua
local arch = Settings.GetArch()
-- Returns one of: "x64", "x86", "arm64", "arm32", "wasm32", "wasm64"
-- ("Unknown" only if compiled for an unrecognized target)

-- Architecture string constants (handy for switch-style logic and comparisons):
Settings.ARCH_X86     -- "x86"     32-bit Intel/AMD
Settings.ARCH_X64     -- "x64"     64-bit Intel/AMD
Settings.ARCH_ARM32   -- "arm32"   32-bit ARM (Android armeabi-v7a)
Settings.ARCH_ARM64   -- "arm64"   64-bit ARM (Apple Silicon, Android arm64-v8a, Windows/Linux on ARM)
Settings.ARCH_WASM32  -- "wasm32"  WebAssembly with 32-bit pointers (heap tops out at 2 GB)
Settings.ARCH_WASM64  -- "wasm64"  WebAssembly with 64-bit pointers (-sMEMORY64 build)

-- Pointer width — the shortest "how much memory can this build even address" check:
local is64 = Settings.Is64Bit()   -- true on x64, arm64, wasm64
local is32 = Settings.Is32Bit()   -- true on x86, arm32, wasm32
```

What the 6 platforms can report:

| Platform | Architectures the engine builds | `Settings.GetArch()` returns |
|----------|---------------------------------|------------------------------|
| Windows  | x64, x86, arm64 | `"x64"`, `"x86"`, `"arm64"` |
| Linux    | x64, x86, arm64 | `"x64"`, `"x86"`, `"arm64"` |
| macOS    | arm64 (Apple Silicon), x64 (Intel) | `"arm64"`, `"x64"` |
| iOS      | arm64 (device and simulator) | `"arm64"` |
| Android  | `arm64-v8a`, `armeabi-v7a`, `x86_64`, `x86` | `"arm64"`, `"arm32"`, `"x64"`, `"x86"` |
| Web      | wasm32, wasm64 (`-sMEMORY64` build) | `"wasm32"`, `"wasm64"` |

> **Android ABI names are normalized** so one comparison works everywhere: `armeabi-v7a` → `"arm32"`, `arm64-v8a` → `"arm64"`, `x86_64` → `"x64"`, `x86` → `"x86"`. Write `arch == Settings.ARCH_ARM64` once instead of matching per-platform ABI spellings.

The value is a compile-time property of **the build that is running**, not a probe of the hardware: an x64 build executing on an ARM machine through emulation still reports `"x64"`, because that is the code actually running — which is exactly what you want when you are budgeting for it. The call is cheap (no syscalls, no I/O), so it is fine to use in `OnStart` or in a settings menu.

Patterns:

```lua
-- 1) One gate for everything that is address-space limited
if Settings.Is32Bit() then
    Settings.SetMaxTextureSize(2048)
    Settings.SetAtlasSize(2048)
    Settings.SetMaxPointLights(8)
    Settings.SetAAMode(Settings.AA_MODE_OFF)
end

-- 2) Switch-style on the architecture name
local a = Settings.GetArch()
if a == Settings.ARCH_ARM32 then
    Settings.SetRenderScale(0.75)
    Settings.SetAudioQuality(Settings.AUDIO_LOW)
    Settings.SetFPSLimit(30)
elseif a == Settings.ARCH_ARM64 then
    Settings.SetRenderScale(1.0)
    Settings.SetFPSLimit(60)
elseif a == Settings.ARCH_WASM32 then
    -- browser build without -sMEMORY64: everything has to fit under a 2 GB heap
    Settings.SetMaxTextureSize(4096)
end

-- 3) Platform + architecture together — the precise "weak device" gate
if Settings.IsAndroid() and Settings.GetArch() == Settings.ARCH_ARM32 then
    -- old 32-bit phone: lowest tier
elseif Settings.IsMobile() then
    -- modern arm64 phone/tablet: normal mobile tier
end

-- 4) Or hand the budget to the engine instead of hardcoding tiers
if Settings.Is32Bit() then
    Settings.SetAdaptiveQuality(true, 30)
else
    Settings.SetAdaptiveQuality(true, 60)
end
```

### Accessibility

```lua
Settings.SetAccessibilityEnabled(true)
local on = Settings.IsAccessibilityEnabled()

Settings.SetGamma(1.0)            -- 0.3 .. 3.0
local gamma = Settings.GetGamma()
Settings.SetContrast(1.0)         -- 0.0 .. 3.0
local contrast = Settings.GetContrast()
Settings.SetBrightness(0.0)       -- -1.0 .. 1.0
local brightness = Settings.GetBrightness()
Settings.SetSaturation(1.0)       -- 0.0 .. 3.0
local saturation = Settings.GetSaturation()

-- Field of View lens
Settings.SetFOVEnabled(true)
local fovOn = Settings.IsFOVEnabled()
Settings.SetFOV(105)              -- 60 .. 120 degrees, 90 = neutral
local fov = Settings.GetFOV()
Settings.FOV_MIN                  -- 60
Settings.FOV_MAX                  -- 120
Settings.FOV_NEUTRAL              -- 90

-- Colorblind filters
Settings.COLORBLIND_OFF
Settings.COLORBLIND_PROTANOPIA
Settings.COLORBLIND_DEUTERANOPIA
Settings.COLORBLIND_TRITANOPIA
Settings.COLORBLIND_ACHROMATOPSIA
Settings.SetColorblindMode(Settings.COLORBLIND_DEUTERANOPIA)
Settings.SetColorblindStrength(1.0)  -- 0.0 .. 1.0
local mode = Settings.GetColorblindMode()
local strength = Settings.GetColorblindStrength()

Settings.SetDyslexiaFriendlyFont(true)
local dyslexic = Settings.IsDyslexiaFriendlyFont()
Settings.SetDyslexiaFontPath("Content/Fonts/OpenDyslexic-Regular.ttf")
local fp = Settings.GetDyslexiaFontPath()
```

`DyslexiaFontPath` accepts a path relative to the project `Content/` folder
(e.g. `"Fonts/OpenDyslexic-Regular.ttf"` or `"Content/Fonts/OpenDyslexic-Regular.ttf"`)
as well as an absolute path; packaged VFS builds are supported. While the override
is active (`AccessibilityEnabled` + `DyslexiaFriendlyFont` + non-empty path), it
replaces **every** font in game text rendering — widget elements with their own
fonts, elements without a font, tooltips and dropdowns — and the editor rebuilds
its UI font with the dyslexia font as primary and the regular editor font merged
in as a glyph fallback. If the file cannot be found, a single warning is logged
and the regular fonts keep working.

### Field of View (accessibility lens)

`SetFOV` is a **lens**, not a camera frustum. The engine renders through
`glm::ortho` and has no perspective projection anywhere, so there is no frustum
angle to widen: culling, lighting, physics, screen-to-world math and what the
player can reach are all untouched. What the slider drives is a screen-space lens
warp applied as one full-screen pass at the end of the post-process chain.

`90` is neutral. Above `90` the frame bulges outward (wide angle, barrel
distortion); below `90` it pinches inward (telephoto, pincushion). Values are
clamped to `[60, 120]`. The warp is aspect-corrected, so the same value looks the
same at 16:9, 21:9 and portrait mobile, and the sampling window is scaled so the
frame stays completely filled — no smeared edges, no black bars, at any angle.

The pass activates the post-process path on its own, exactly like the colour
accessibility pass, so it applies in a level with no post-process volume, in the
editor viewport, in Play mode and in the packaged game, on every render backend.
While `FOVEnabled` is off (or the angle is neutral) the pass is skipped entirely
and costs nothing.

Two things worth knowing:

- Widget elements are composited **after** post-processing unless they are marked
  *Post Processed*, so the HUD stays undistorted by default.
- It is a screen-space warp, so mouse and touch coordinates are not re-projected.
  At high angles the cursor and the picked world point drift apart near the frame
  corners — the same behaviour as the `HeatHaze` and `Underwater` post effects.

```lua
Settings.SetAccessibilityEnabled(true)
Settings.SetFOVEnabled(true)
Settings.SetFOV(Settings.FOV_NEUTRAL + 20)   -- 110, wide angle
Settings.SetFOV(Settings.FOV_NEUTRAL)        -- back to neutral
```

### Text-to-Speech (TTS)

When `Accessibility.TTSEnabled` is on, every `Settings.SpeakHint(text)` call is
routed to a native speech engine — Windows SAPI 5, Web Speech API on
emscripten, **AVSpeechSynthesizer** on macOS/iOS, **espeak-ng** on Linux when
available, **android.speech.tts.TextToSpeech** on Android, stub elsewhere.
Voices configured in the OS (Narrator, Vocalizer, NVDA voices, etc.) are reused
automatically.

TTS also works fully automatically, without any Lua code: when accessibility and
TTS are enabled (in `Config/Engine.json`, via the editor Preferences, or through
this API), the widget runtime speaks the text of any widget element under the
mouse cursor (including plain Text/Image labels that are not interactable),
reads tooltips when they appear, announces hovered dropdown options, and
announces the focused element during gamepad/keyboard UI navigation. Sliders
and progress bars report their label together with the current value.
`SpeakHint` remains available for custom game-driven speech cues.

Install notes:
- **Windows**: nothing to install — SAPI 5 is always used. Installing espeak-ng
  separately is harmless, but the engine will keep using SAPI.
- **Linux**: install the development package, e.g. `sudo apt install libespeak-ng-dev`
  (Debian/Ubuntu). CMake auto-detects it via `find_package` / `find_library`.
  Without headers the backend gracefully falls back to a stub.
- **Android**: nothing to compile. The engine talks to whichever TTS engine the
  user has selected in *Settings → Language & input → Text-to-speech output*.
  Installing the **eSpeak NG APK** (or any other system TTS engine) and picking
  it as the preferred engine is enough — the bridge picks it up automatically.
  Manifest `<queries>` for `intent.action.TTS_SERVICE` is already added so
  Android 11+ can enumerate engines.
- **Web (emscripten)**: nothing to install — the browser's Web Speech API is used.

```lua
Settings.SetTTSEnabled(true)
local on = Settings.IsTTSEnabled()
Settings.SpeakHint("Menu opened")    -- speak text through the TTS engine
Settings.SetTTSRate(0.0)              -- -1..+1 (slow..fast)
local rate = Settings.GetTTSRate()
Settings.SetTTSVolume(1.0)            -- 0..1
local vol = Settings.GetTTSVolume()
Settings.SetTTSPitch(0.0)             -- -1..+1
local pitch = Settings.GetTTSPitch()
Settings.StopSpeech()                 -- interrupt the current utterance
local speaking = Settings.IsSpeaking()
local backend = Settings.GetTTSBackend()       -- "SAPI 5" | "Web Speech API" | "AVSpeechSynthesizer" | "espeak-ng" | "Android TextToSpeech (<engine>)" | "stub"
local available = Settings.IsTTSAvailable()
local sr_active = Settings.IsScreenReaderActive() -- true if Narrator/JAWS/NVDA running
```

### Game speed (accessibility slow-mo)

`Settings.SetGameSpeed(s)` is independent of `SetTimeScale()`. It multiplies
the engine `GetDeltaTime()` so `IsPaused()` becomes true at `0.0`. Range
clamped to `[0.1, 2.0]`.

```lua
Settings.SetGameSpeed(0.5)            -- 50% speed
Settings.ResetGameSpeed()             -- back to 1.0
local s = Settings.GetGameSpeed()
```

### Force mono audio

```lua
Audio.SetForceMono(true)                     -- one-sided hearing loss helper
local mono = Audio.IsForceMono()

-- Accessibility-owned equivalent: persisted with the other accessibility
-- settings and re-applied on load, whereas the Audio.* pair only flips the
-- current mixer state.
Settings.SetForceMonoAudio(true)
local monoSetting = Settings.IsForceMonoAudio()
```

### Save/load settings

```lua
Settings.Save()             -- Save to GameSettings.json (writable user config path)
Settings.Load()             -- Load from GameSettings.json
Settings.Apply()            -- Apply all current settings to the engine
                            --   (window, VSync, quality, audio, AA, lighting, HDR10)
                            --   Window-related changes are skipped with a warning if
                            --   the engine SDL window is not yet available.

Settings.ResetDefaults()    -- Reset all settings to defaults and Apply()
                            --   Defaults: 1920x1080 windowed, VSync on, HDR10 off,
                            --   60 FPS, RenderScale=1.0, Audio=High, AA=Off,
                            --   AdaptiveQuality off (target 60),
                            --   all volumes=1.0, not muted. Lighting, shadows and
                            --   every other render setting go back to the
                            --   Config/Engine.json project defaults.

local path = Settings.GetSettingsPath()  -- Absolute path to GameSettings.json
```

`GameSettings.json` holds **only what the game changed**, never a copy of
`Config/Engine.json`. Render settings the game touched through
[section 13.3](#133-lighting-and-shadows--global-settings) (`SetShadowsEnabled`,
`SetCollidersBlockShadows`, `SetDirectionalLight`, `Settings.SetLightingEnabled`, …) are
written into a `Rendering` block containing just those keys, and re-applied on the next
startup — and when Play mode starts in the editor, so in-editor testing matches the
shipped game. Anything the game never touched keeps following `Config/Engine.json` →
`Rendering` and the level's [World Settings](Editor-EN-DOC.md#8-world-settings), which
means adding a new option to `Engine.json` later reaches players who already have a saved
`GameSettings.json`.

### Auto-detect hardware

Scores the machine from RAM, logical CPU cores and primary display resolution
(with penalties for mobile, web and GLES-class backends), then applies a
matching preset for render scale, audio quality, anti-aliasing, VSync, HDR10
and the FPS limit:

| Tier | Render scale | Audio | AA | FPS limit |
| ---- | ------------ | ----- | -- | --------- |
| High-end | 1.5 | Very High | MSAA 4x | display Hz (min 144) |
| Upper | 1.0 | High | MSAA 2x | display Hz (max 120) |
| Middle | 0.75 | Medium | FXAA | display Hz (max 90) |
| Low | 0.5 | Medium-Low | Off | display Hz (max 60) |

Notes:
- VSync is always enabled by the preset.
- HDR10 is only enabled on the high-end tier with a 1440p+ display on a
  desktop Vulkan / Direct3D 12 / Metal / Metal (MoltenVK) backend — the only
  backends that can deliver a real HDR10 (ST.2084) swapchain. If the display turns out not to
  support HDR10, the renderer silently stays in SDR.
- On mobile and web, MSAA/SSAA presets are downgraded to FXAA (those windows
  are created without multisample buffers) and HDR10 stays off.
- On web, device RAM is estimated via `navigator.deviceMemory` when the
  browser exposes it.
- Resolution and window mode are never touched.

```lua
Settings.AutoDetect()
Settings.Save()             -- persist the detected preset if desired
```

### Adaptive Quality (dynamic FPS-based quality)

When enabled, the engine continuously measures a smoothed (time-based EMA)
average FPS and automatically degrades quality when FPS drops below the
target, then restores it once performance recovers. The quality of the moment
the first downgrade happens is captured as the *base*; recovery always returns
exactly to that base. Degradation levels (relative to the base):
1. AA mode is stepped down (SSAA 4x → SSAA 2x → FXAA; any MSAA → FXAA).
2. Same AA step-down plus render scale −0.15.
3. AA off plus render scale −0.30 (render scale never goes below 0.25).

Levels that would not change anything (e.g. base AA is already FXAA or Off)
are skipped automatically.

Details:
- A downgrade happens when the average FPS falls below `85%` of the effective
  target; recovery happens when it exceeds `97%`.
- The effective target is automatically clamped by the FPS limit, and by the
  display refresh rate while VSync is on (always on mobile/web), so an
  unreachable target does not force minimum quality.
- Cooldowns: 2 s after a downgrade, 4 s after a recovery. If the system starts
  oscillating (downgrade soon after a recovery), the recovery cooldown backs
  off exponentially up to 32 s.
- Frame hitches longer than 0.5 s (loading, alt-tab) are ignored.
- Changing AA mode or render scale manually (Lua, editor, `AutoDetect`) while
  a downgrade is active re-bases the system on your new values instead of
  reverting them later.
- `Settings.Save()` always persists the base quality, never the temporarily
  downgraded one.

```lua
-- Enable with a target FPS (omitting targetFPS keeps the current target)
Settings.SetAdaptiveQuality(true, 60)
Settings.SetAdaptiveQuality(false)            -- disable (target is preserved)

local enabled = Settings.IsAdaptiveQualityEnabled()
local target  = Settings.GetAdaptiveQualityTargetFPS()
```

### Low Latency Mode (reduced input-to-display lag)

Caps the CPU from running more than one frame ahead of the GPU. On the
OpenGL-family backends (OpenGL 4.6/3.3, GLES 3.2, ANGLE) this works by
inserting a GPU fence at the end of each frame and waiting for it at the
start of the next; when VSync is also on, the engine requests **adaptive
VSync** from the driver (`SDL_GL_SetSwapInterval(-1)`) and silently falls
back to standard VSync if the platform does not support it.

On the Vulkan / MoltenVK backends the same cap is enforced natively: the
frame loop waits for **all** in-flight frame fences before starting a new
frame (one frame in flight instead of two), and the swapchain present mode
follows the setting — with VSync on it prefers `FIFO_RELAXED` (the Vulkan
equivalent of adaptive VSync, falling back to `FIFO`), with VSync off it
prefers `IMMEDIATE` over `MAILBOX` for the lowest possible latency.
Toggling the mode recreates the swapchain automatically.

Direct3D 12 works the same way: the frame loop waits for the newest of all
in-flight frame fences instead of only the one belonging to the frame being
started, and presentation uses sync interval 1 with VSync on, or sync
interval 0 with `DXGI_PRESENT_ALLOW_TEARING` (when the adapter and the
display allow tearing) with VSync off.

Metal works the same way: the frame pacing semaphore is drained down to a
single frame in flight, and the `CAMetalLayer` toggles `displaySyncEnabled`
together with a smaller maximum drawable count so the CPU cannot queue ahead.

In web builds (WebGL2 / WebGPU) the browser drives frame pacing and blocking
waits are not allowed, so this setting has no effect there.

Effect at 144 FPS, VSync ON:
- Default: CPU is up to 3 frames ahead → ~21 ms input latency.
- Low Latency ON: CPU is at most 1 frame ahead → ~7 ms input latency.

Trade-off: on very weak GPUs the CPU and GPU run more serially so peak
throughput can drop slightly. Recommended for action games at high FPS and
not recommended for cinematic / cutscene-heavy titles where smoothness
matters more than reaction time.

The setting is persisted to both `Config/Engine.json` (editor default) and
the per-user game settings file (`SaveToFile()`).

```lua
Settings.SetLowLatencyMode(true)         -- enable
Settings.SetLowLatencyMode(false)        -- disable (default)

local on = Settings.IsLowLatencyMode()
```

The `Settings.OnSettingChanged` listener fires with the key
`"LowLatencyMode"` when this value changes.

### Suspend (auto-pause when the window is inactive)

When enabled (the default), the engine suspends itself while its window is
unfocused, minimized, hidden or the app is sent to background: update and
render are skipped, game logic is frozen and audio is stopped. The moment
the window becomes active again everything resumes exactly where it left
off. This works on all 6 platforms (Windows, Linux, macOS, iOS, Android,
Web) and matches the editor's behavior as well.

When disabled, the automatic window-driven suspend is turned off entirely —
the game keeps updating, rendering and playing audio in the background, as
if the suspend system did not exist. The manual `SuspendApp()` /
`ResumeApp()` API keeps working regardless of this setting.

Platform notes:
- **Windows / Linux / macOS** — the game simply keeps running while
  unfocused or minimized (a zero-size minimized surface is safely skipped
  by the renderer).
- **Android** — SDL latches the OS-level "block event loop on pause" flag
  once at startup, so background execution for a launch is controlled by
  the value persisted in `Config/Engine.json` → `Optimization.IsSuspended`.
  A runtime `SetIsSuspended` still applies the audio/frame gating
  immediately, but lifting the OS-level block takes effect on the next
  launch. Background execution is best-effort — the OS may still throttle
  or kill the process.
- **iOS** — with the setting off, the app keeps running through transient
  focus losses (control center, notification shade, app switcher preview);
  a full switch to background is still frozen by the OS itself, as with
  any iOS app.
- **Web** — with the setting off, audio keeps playing while the tab is
  hidden and the game keeps running when the window merely loses focus; a
  fully hidden tab still stops rendering because the browser throttles
  `requestAnimationFrame`, which is outside the engine's control.

The setting lives in the editor's **Settings → Optimization** tab, persists
in `Config/Engine.json` (`Optimization.IsSuspended`, default `true`) and in
the per-user game settings file (`SaveToFile()`).

```lua
Settings.SetIsSuspended(true)          -- default: auto-suspend enabled
Settings.SetIsSuspended(false)         -- keep running in background

local sus = Settings.IsSuspended()
```

`OnSettingChanged` fires with key `"IsSuspended"`.

### Project Pre-Warm (load all assets at startup)

When enabled, the engine scans the `Content/` directory at startup and queues
every sprite, flipbook, skeleton, material, material instance, particle FX,
tilemap, tileset, UI widget, font, animation graph and sound for pre-loading.
Each asset is warmed through the exact same loader the game uses at runtime, so
textures land in the correct atlas with their `.ice_texture` import settings
applied — no duplicate standalone copies and no first-use hitch. The loading
happens incrementally over subsequent frames with a small per-frame budget
(~2 ms) so it does not block the first rendered frame.

Use this if your game does **not** have a loading screen between scenes —
once the queue drains, transitions to new scenes will never hit a first-use
asset compile/decode/upload.

If your game DOES have loading screens, leave this OFF and use the
`Prewarm.*` Lua API on the loading screen instead — it's more granular and
only loads what the next scene needs.

The setting persists in `Config/Engine.json` (`Engine.ProjectPrewarmOnStartup`)
and in the per-user game settings file.

```lua
Settings.SetProjectPrewarm(true)
Settings.SetProjectPrewarm(false)             -- default

local on = Settings.IsProjectPrewarm()
```

`OnSettingChanged` fires with key `"ProjectPrewarm"`.

### Prewarm.* — manual pre-loading API

`Prewarm.*` is a globally-available namespace for queuing assets to be
pre-loaded incrementally without blocking the main thread. Typical use is
inside a loading screen scene.

The queue is drained on the engine tick with a ~2 ms per-frame budget. PNG
decoding happens on worker threads; GL upload and shader compilation happen
on the main thread, throttled by the budget.

```lua
-- Queue a single asset (deduplicated — calling twice with the same path is free)
Prewarm.Sprite("Content/Sprites/Player/SP_T_PlayerIdle.ice_sprite")
Prewarm.Flipbook("Content/Anims/FB_Explosion.ice_flipbook")
Prewarm.Skeleton("Content/Skeletons/SK_Hero.ice_skeleton")
Prewarm.Material("Content/Materials/M_Hologram.ice_material")
Prewarm.MaterialInstance("Content/Materials/MI_HologramBlue.ice_matinst")
Prewarm.Decal("Content/Decals/DC_BulletHole.ice_decal")
Prewarm.FX("Content/FX/FX_Explosion.ice_fx")
Prewarm.Tilemap("Content/Levels/Forest/TM_Forest.ice_tm")
Prewarm.Tileset("Content/Tilesets/TS_Grass.ice_ts")
Prewarm.Widget("Content/UI/HUD.ice_widget")
Prewarm.Font("Content/Fonts/Roboto.ttf")
Prewarm.Animation("Content/Anims/AC_Player.ice_animation")
Prewarm.View("Content/Views/V_Underwater.ice_view")
Prewarm.Sound("Content/Audio/sfx_explosion.wav")
Prewarm.Texture("Content/UI/bg_titlescreen.png")  -- raw image (atlas if it fits, else standalone)

-- Queue an entire folder (recursive). Recognised extensions:
--   .ice_sprite, .ice_flipbook, .ice_skeleton, .ice_material, .ice_matinst, .ice_decal,
--   .ice_fx, .ice_tm, .ice_ts, .ice_widget, .ice_animation, .ice_view,
--   .ttf, .otf, .ttc, .wav, .ogg, .mp3, .flac,
--   .png, .jpg, .jpeg, .bmp, .tga
Prewarm.Folder("Content/Levels/Forest")

-- Progress (call from loading screen Update)
local progress = Prewarm.GetProgress()    -- 0.0 ..  1.0 (caps at 0.99 while GPU uploads pending)
local ready    = Prewarm.IsComplete()     -- true when queue drained AND all GL uploads done
local pending  = Prewarm.GetQueueSize()   -- items still queued

-- Detailed stats
local stats = Prewarm.GetStats()
print(stats.queued, stats.completed, stats.failed, stats.pending_uploads)

-- Cancel everything still in the queue (already-loaded assets stay in caches)
Prewarm.Clear()

-- Queue the project's own prewarm list (the assets configured in the project
-- settings) — the same list the engine warms automatically at startup.
Prewarm.RunProjectPrewarm()
```

**Example: Lua loading screen**

```lua
-- Loading screen widget OnReady():
function OnReady()
    Prewarm.Folder("Content/Levels/Forest")
end

function OnUpdate(dt)
    local p = Prewarm.GetProgress()
    SetWidgetText("progress_label", string.format("Loading %d%%", math.floor(p * 100)))
    if Prewarm.IsComplete() then
        LoadLevel("Content/Levels/Forest/TestForestLevel.icemap")
    end
end
```

**Notes**
- All `Prewarm.*` paths are normalized (`\` → `/`) and resolved the same way the
  engine resolves any content path. You can pass `Content/...` paths or just filenames.
- Every asset is warmed through the same loader the game uses at runtime, so the
  warmed cache entry is the exact one a later first-use will hit — including atlas
  packing and `.ice_texture` settings for sprite textures.
- Dependencies are chased automatically and deduplicated:
  - a flipbook queues every frame sprite (and its override material);
  - a sprite queues its referenced material / material instance;
  - a skeleton queues every attachment sprite;
  - a particle FX queues its sprites, materials, sub-FX and event FX;
  - a tilemap queues its tilesets and animated-tile flipbooks;
  - a tileset warms its texture and queues its material;
  - a widget queues its sprites, flipbooks, fonts, sounds, parent and
    sub-widgets;
  - an animation graph queues every state flipbook;
  - a view warms its post-process sky / LUT / bloom-dirt textures (under the
    exact runtime keys) and compiles its custom post-process materials.
- `Prewarm.Texture` (and any raw image picked up by `Prewarm.Folder`) warms the
  image the same way a sprite would: into the shared atlas if it fits (honoring
  `.ice_texture` settings), otherwise as a standalone texture keyed by full
  path. It is idempotent and shares the cache entry with the matching
  sprite/material/tileset, so no duplicate copy is created.
- `Prewarm.Material` compiles the shader and loads the material's textures;
  `Prewarm.MaterialInstance` (or any `.ice_matinst` path) also resolves the
  parent material and applies texture overrides.
- The queue survives scene transitions — `Prewarm.Clear()` is the only way
  to abandon queued (but not-yet-processed) items.

### Settings change events

Subscribe to a callback that is fired whenever a setting is changed (via Lua,
the editor settings panel, AutoDetect, or AdaptiveQuality). The callback
receives the changed key as a string (for example `"AAMode"`, `"FPSLimit"`,
`"RenderScale"`, `"UpscalingMode"`, `"UpscalingQuality"`, `"UpscalingSharpness"`,
`"UpscalingSharpening"`, `"AdaptiveQuality"`, `"AdaptiveQualityLevel"`, ...).

```lua
local id = Settings.OnSettingChanged(function(key)
    print("Setting changed:", key, "→", Settings.GetAAMode())
end)

-- Remove all listeners
Settings.ClearChangeListeners()
```

> **Notes**
> - All volume setters (`SetMasterVolume`, `SetMusicVolume`, `SetSFXVolume`,
>   `SetVoiceVolume`, `SetAmbientVolume`, `SetUIVolume`) clamp the value to `[0..1]`.
> - `SetWindowMode(mode)` accepts only `"windowed"`, `"fullscreen"`, `"borderless"`;
>   any other value is rejected with a warning.
> - HDR10 is automatically disabled on GLES backends regardless of the requested value.

---

## 25. Math — Math and Noise

> **Type:** Global functions

### Data types

```lua
-- 2D vector
local v = Vec2(1.0, 2.0)
v.x = 3.0
v.y = 4.0

-- 3D vector
local v3 = Vec3(1, 2, 3)

-- 4D vector
local v4 = Vec4(1, 2, 3, 4)

-- Color (RGBA)
local c = Color(1, 0, 0)         -- Red (a=1 by default)
local c = Color(1, 0, 0, 0.5)    -- Semi-transparent red

-- Rectangle
local r = Rect(10, 20, 100, 50)  -- x, y, w, h

-- Transform (position, rotation, scale)
local t = Transform(Vec3(0, 0, 0), 0, Vec2(1, 1))
t.position = Vec3(10, 20, 0)
t.rotation = 45
t.scale = Vec2(2, 2)

-- Vector methods
local len2 = v:Length()
local len2sq = v:LengthSq()
local norm2 = v:Normalized()
local dot2 = v:Dot(Vec2(1, 0))
local dist2 = v:Distance(Vec2(0, 0))
local lerp2 = v:Lerp(Vec2(5, 5), 0.5)

local len3 = v3:Length()
local len3sq = v3:LengthSq()
local norm3 = v3:Normalized()
local dot3 = v3:Dot(Vec3(0, 1, 0))
local cross3 = v3:Cross(Vec3(0, 1, 0))
local dist3 = v3:Distance(Vec3(0, 0, 0))
local lerp3 = v3:Lerp(Vec3(5, 5, 5), 0.5)

local len4 = v4:Length()
local dot4 = v4:Dot(Vec4(1, 0, 0, 0))
local lerp4 = v4:Lerp(Vec4(1, 1, 1, 1), 0.5)
```

### Color utilities

| Function | Description |
|---------|----------|
| `ColorFromHex(hex)` | Hex string → `{r, g, b, a}` (0–1) |
| `ColorToHex(r, g, b, a?)` | RGB → Hex string |
| `HSVToRGB(h, s, v)` | HSV → `{r, g, b}` |
| `RGBToHSV(r, g, b)` | RGB → `{h, s, v}` |
| `Clamp01(value)` | Clamp to [0, 1] |

**Preset colors** (`Colors.*`):

```lua
Colors.Red        -- {r=1, g=0, b=0, a=1}
Colors.Green      -- {r=0, g=1, b=0, a=1}
Colors.Blue       -- {r=0, g=0, b=1, a=1}
Colors.White      -- {r=1, g=1, b=1, a=1}
Colors.Black      -- {r=0, g=0, b=0, a=1}
Colors.Yellow     -- {r=1, g=1, b=0, a=1}
Colors.Cyan       -- {r=0, g=1, b=1, a=1}
Colors.Magenta    -- {r=1, g=0, b=1, a=1}
Colors.Orange     -- {r=1, g=0.5, b=0, a=1}
Colors.Purple     -- {r=0.5, g=0, b=0.5, a=1}
Colors.Gray       -- {r=0.5, g=0.5, b=0.5, a=1}
Colors.DarkGray   -- {r=0.25, g=0.25, b=0.25, a=1}
Colors.LightGray  -- {r=0.75, g=0.75, b=0.75, a=1}
Colors.Brown      -- {r=0.6, g=0.3, b=0, a=1}
Colors.Pink       -- {r=1, g=0.4, b=0.7, a=1}
Colors.Transparent -- {r=0, g=0, b=0, a=0}

-- From hex
local c = ColorFromHex("#FF5500")
SetColor(c.r, c.g, c.b, 1)

-- To hex
local hex = ColorToHex(1, 0.5, 0, 1)  -- → "#FF8000FF"

-- HSV
local rgb = HSVToRGB(120, 1, 1)        -- green: h=120°, s=1, v=1
local hsv = RGBToHSV(1, 0, 0)          -- red: h=0°, s=1, v=1

-- Preset
SetColor(Colors.Red.r, Colors.Red.g, Colors.Red.b, 1)

-- Clamp01
local t = Clamp01(1.5)  -- → 1.0
```

### Math functions

```lua
-- Clamp
local v = Clamp(value, 0, 100)

-- Interpolation
local v = Lerp(0, 100, 0.5)  -- = 50

-- Sign (-1, 0, 1)
local s = Sign(-5)  -- = -1

-- Absolute value
local a = Abs(-5)  -- = 5

-- Min/Max
local mi = Min(a, b)
local ma = Max(a, b)

-- Rounding
local f = Floor(3.7)   -- = 3
local c = Ceil(3.2)    -- = 4
local r = Round(3.5)   -- = 4

-- Root and power
local sq = Sqrt(16)     -- = 4
local pw = Pow(2, 10)   -- = 1024

-- Inverse lerp and remap
local t = InverseLerp(0, 100, 25)           -- → 0.25
local v = Remap(25, 0, 100, 0, 1)           -- → 0.25

-- Vector functions (float)
local len = Length(x, y)
local dot = Dot(x1, y1, x2, y2)
local dist = Distance(x1, y1, x2, y2)
local distSq = DistanceSq(x1, y1, x2, y2)
local n = Normalize(x, y)                   -- → {x, y}

-- Movement and smoothing
local v = MoveTowards(current, target, 5)
local v = SmoothDamp(current, target, 0.2, dt)
local v = Approach(current, target, 2)
local a = WrapAngle(angle)

-- Trigonometry and angles (degrees)
local s = Sin(45)
local c = Cos(90)
local t = Tan(30)
local a = Asin(0.5)
local a = Acos(0.5)
local a = Atan2(y, x)
local ang = AngleBetween(x1, y1, x2, y2)
local dir = AngleToDirection(90)            -- → {x, y}
local ang = DirectionToAngle(x, y)
local p = RotatePoint(x, y, 45)             -- → {x, y}
local p = RotatePointAround(x, y, cx, cy, 45)
local ang = LerpAngle(a, b, 0.5)
```

### Random numbers

```lua
local r = Random()                   -- 0..1 (float)
local r = RandomRange(10.0, 50.0)    -- 10..50 (float)
local r = RandomInt(1, 6)            -- 1..6 (int, like dice)
local b = RandomBool()               -- true/false (50/50)
local b = RandomBool(0.3)            -- true with 30% chance

-- Pick random element from array
local item = RandomChoice({"Sword", "Shield", "Potion"})

-- Weighted random pick
local index = RandomWeighted({10, 5, 1})  -- First is 10x more likely than third

-- Set seed (repeatable)
SetRandomSeed(42)
```

> **Determinism (new):** all of the functions above now run on the engine's deterministic
> generator. Call `SetRandomSeed(n)` (or `RNG.SetSeed`) once and **every** `Random*` call, loot
> roll, array shuffle and AI random selector becomes fully reproducible — the foundation for
> seeded roguelike runs and replays. `SetRandomSeed` also reseeds the noise tables by default.
> For named sub-streams, string seeds and exact save/load, see
> [RNG — Deterministic Random Streams](#rng--deterministic-random-streams-seeded).

> **Standard `math.random` is routed here too.** `math.random()`, `math.random(n)`,
> `math.random(m, n)` and `math.randomseed(s)` run on the same deterministic generator, keeping
> their usual Lua semantics (no args → float in `[0,1)`, otherwise an integer in range). This
> matters for rollback netcode: a raw Lua RNG would not be part of a rollback snapshot and would
> desync on the first resimulated frame. Nothing to change in your code — existing calls simply
> became reproducible.

### Perlin noise and others

```lua
-- Perlin Noise (1D and 2D)
local n = Noise(x)             -- 1D
local n = Noise(x, y)          -- 2D
local n = PerlinNoise(x, y)    -- Alias for Noise

-- Simplex Noise (2D)
local n = SimplexNoise(x, y)

-- FBM (Fractal Brownian Motion) — landscapes and clouds
local n = FBMNoise(x, y)
local n = FBMNoise(x, y, 6, 2.0, 0.5)  -- octaves, lacunarity, gain

-- Ridged Noise — mountain ridges
local n = RidgedNoise(x, y)
local n = RidgedNoise(x, y, 6, 2.0, 0.5)

-- Turbulence Noise
local n = TurbulenceNoise(x, y)
local n = TurbulenceNoise(x, y, 6, 2.0, 0.5)

-- Voronoi Noise — cells (default = euclidean F1 distance)
local n = VoronoiNoise(x, y)

-- Voronoi Noise (extended) — pass a table of options
-- metric:     "euclidean" (default) | "manhattan" | "chebyshev"
-- returnMode: "F1" (default) | "F2" | "F2_F1" | "cellId"
local f1     = VoronoiNoise(x, y, { metric = "manhattan" })
local edge   = VoronoiNoise(x, y, { returnMode = "F2_F1" })  -- crystal/cell-edge mask
local cellId = VoronoiNoise(x, y, { returnMode = "cellId" }) -- integer per-cell id
local f2     = VoronoiNoise(x, y, { metric = "chebyshev", returnMode = "F2" })

-- NoiseMap — 2D noise map (table of values 0..1)
-- result is a Lua table: result[1..height][1..width], row 1 = bottom (y=0), row H = top
local map = NoiseMap(64, 64, 50.0)
local map = NoiseMap(64, 64, 50.0, 6, 2.0, 0.5, 123)

-- Reseed the noise permutation table (affects ALL noise functions:
-- Noise/PerlinNoise/SimplexNoise/FBMNoise/RidgedNoise/TurbulenceNoise/VoronoiNoise/NoiseMap/NoiseBuffer)
ReseedNoise(12345)
SetNoiseSeed(12345)  -- alias

-- Domain warping — distort coordinates with Perlin noise to break grid alignment
-- Returns { x = warpedX, y = warpedY }
local w = DomainWarp(x, y, 4.0)
local val = PerlinNoise(w.x, w.y)

-- Curl noise (2D, divergence-free vector field) — perfect for fluid/smoke flow
-- Returns { x = vx, y = vy }; optional eps controls finite-difference step (default 0.01)
local v = CurlNoise2D(x, y)
local v = CurlNoise2D(x, y, 0.005)

-- NoiseBuffer — native float buffer (much faster than NoiseMap, 0-based indexing)
-- Use for procedural generation where you need to sample/modify large grids without per-cell Lua tables.
local buf = NoiseBuffer.new(256, 256)
buf:Fill(40.0)                              -- fBm with default octaves=6, lac=2, gain=0.5, seed=0
buf:Fill(40.0, 8, 2.0, 0.5, 12345)          -- scale, octaves, lacunarity, gain, seed
local w = buf:Width()
local h = buf:Height()
local v = buf:Get(x, y)                     -- 0-based, out-of-range returns 0
buf:Set(x, y, 0.5)                          -- 0-based, out-of-range is a no-op
buf:Clear(0.0)
local lo, hi = buf:Min(), buf:Max()
buf:Normalize()                             -- remap min..max → 0..1 in place
local tbl = buf:ToTable()                   -- export as Lua table[1..h][1..w] (row 1 = bottom, row H = top)
```

### Additional math functions

```lua
-- Approximate comparison
IsNearlyEqual(3.0001, 3.0, 0.001)  -- → true
IsNearlyZero(0.00001)              -- → true
IsNearlyZero(0.1, 0.5)            -- → true (with custom threshold)

-- Snap to grid
SnapToGrid(47, 32)                 -- → 48 (nearest multiple of 32)
local pos = SnapToGrid2D(47, 63, 32)  -- → {x=48, y=64}

-- Remap with clamp (stay in range)
MapRangeClamped(150, 0, 100, 0, 1)  -- → 1.0 (clamp)

-- Radians/degrees
local rad = DegToRad(180)
local deg = RadToDeg(3.14159)

-- Modular functions
local r = Fmod(10, 3)              -- → 1
local p = PingPong(time, 2.0)      -- 0..2..0
local s = SmoothStep(0, 1, 0.5)    -- → 0.5

-- Geometric checks
local cross = Cross2D(x1, y1, x2, y2)
local refl = Reflect(x, y, nx, ny)        -- → {x, y}
local inside = PointInBox(px, py, bx, by, bw, bh)
local inside = PointInCircle(px, py, cx, cy, radius)
local hit = BoxOverlapsBox(ax, ay, aw, ah, bx, by, bw, bh)
local hit = CircleOverlapsCircle(ax, ay, ar, bx, by, br)
local c = LerpColor(r1, g1, b1, a1, r2, g2, b2, a2, t)

-- Bezier curves
local p = QuadraticBezier(0.5, 0,0, 50,100, 100,0)  -- t, p0, p1, p2
-- p.x, p.y

local p = CubicBezier(0.5, 0,0, 30,80, 70,80, 100,0)  -- t, p0, p1, p2, p3
-- p.x, p.y

-- Catmull-Rom spline (passes through points)
local p = CatmullRom(0.5, 0,0, 10,50, 60,50, 100,0)
-- p.x, p.y

-- Easing functions
EaseIn(0.5)         -- → 0.25 (quadratic)
EaseOut(0.5)        -- → 0.75
EaseInOut(0.5)      -- → 0.5
EaseInCubic(0.5)    -- → 0.125
EaseOutCubic(0.5)   -- → 0.875
EaseInOutCubic(0.5)
EaseInElastic(0.5)
EaseOutElastic(0.5)
EaseOutBounce(0.5)

-- Cyclic wrap
Wrap(370, 0, 360)    -- → 10
Wrap(-10, 0, 360)    -- → 350

-- Spring physics
local result = Spring(current, target, velocity, stiffness, damping, dt)
-- result.value    = new value
-- result.velocity = new velocity

-- Integer/fractional parts
Truncate(3.7)  -- → 3.0
Frac(3.7)      -- → 0.7

-- Logs and exponent
Log(2.718)    -- → ~1.0 (natural)
Log2(8)       -- → 3.0
Log10(1000)   -- → 3.0
Exp(1)        -- → ~2.718
```

### Spatial random

```lua
-- Random point INSIDE a circle (uniform distribution)
local p = RandomPointInCircle(cx, cy, radius)
-- p.x, p.y

-- Random point ON a circle (perimeter)
local p = RandomPointOnCircle(cx, cy, radius)
-- p.x, p.y

-- Random point in a rectangle
local p = RandomPointInBox(x, y, width, height)
-- p.x, p.y

-- Random unit vector (direction)
local dir = RandomUnitVector()
-- dir.x, dir.y (length = 1)
```

### 2D interpolation

```lua
-- Linear interpolation between two points
local p = Lerp2D(x1, y1, x2, y2, 0.5)
-- p.x, p.y

-- MoveTowards for 2D (step-limited)
local p = MoveTowards2D(curX, curY, targetX, targetY, maxDelta)
-- p.x, p.y

-- SmoothDamp for 2D (smooth approach)
local p = SmoothDamp2D(curX, curY, targetX, targetY, smoothTime, dt)
-- p.x, p.y

-- FPS-independent interpolation
local val = InterpTo(current, target, dt, speed)
-- speed = approach speed (5.0 = fast, 0.5 = slow)

-- 2D version of InterpTo
local p = InterpTo2D(curX, curY, targetX, targetY, dt, speed)
-- p.x, p.y
```

### 2D geometry

```lua
-- Closest point on segment AB to point P
local closest = ClosestPointOnLine(px, py, ax, ay, bx, by)
-- closest.x, closest.y

-- Distance from point to segment
local dist = DistanceToLine(px, py, ax, ay, bx, by)

-- Project vector onto another vector
local proj = ProjectOnto(vx, vy, ax, ay)
-- proj.x, proj.y

-- Perpendicular (rotate by 90°)
local perp = Perpendicular(x, y)
-- perp.x, perp.y → (-y, x)

-- Intersection of segments AB and CD
local hit = LineIntersection(ax, ay, bx, by, cx, cy, dx, dy)
-- hit.hit = true/false
-- hit.x, hit.y = intersection point
-- hit.t = parameter on AB (0..1)
-- hit.u = parameter on CD (0..1)

-- Signed angle between two vectors (positive = counter-clockwise)
local angle = SignedAngle(x1, y1, x2, y2)

-- Smoothly rotate angle toward target (clamped speed)
local newAngle = RotateTowards(currentAngle, targetAngle, maxDeltaDeg)
```

### Integer utilities

```lua
CeilToInt(3.2)    -- → 4
FloorToInt(3.8)   -- → 3
RoundToInt(3.5)   -- → 4

IsPowerOfTwo(8)       -- → true
IsPowerOfTwo(12)      -- → false
NextPowerOfTwo(12)    -- → 16
NextPowerOfTwo(64)    -- → 64
```

### RNG — Deterministic Random Streams (seeded)

> **Type:** Global module `RNG.*` + `RandomStream` objects

`RNG` is the engine-wide **deterministic** random generator (xoshiro256\*\* with SplitMix64
seeding). Once a master seed is set, every run is fully reproducible: the same seed always
produces the same dungeon, loot, and AI decisions. This is the backbone for seeded roguelike
runs, daily challenges and replays.

> All legacy helpers (`Random()`, `RandomRange`, `RandomInt`, `RandomBool`, `RandomChoice`,
> `RandomWeighted`, `RandomPointInCircle`, … as well as `Array.Shuffle`, loot-table rolls and
> Behavior-Tree random selectors) route through this same generator, so a single
> `RNG.SetSeed(...)` makes **all** of them reproducible.

#### Seeding

```lua
-- Numeric seed (good for an "enter a seed" UI; integer values up to 2^53 are exact in Lua)
RNG.SetSeed(12345)

-- String seed (full 64-bit entropy — best for shareable seed codes / daily seeds)
RNG.SetSeed("ICEBOX-DAILY-2026-06-10")

-- 2nd argument (default = true) also reseeds the Perlin/Simplex noise tables,
-- so noise-based terrain is reproducible from the same master seed.
RNG.SetSeed(12345, true)    -- reseed RNG + noise (default)
RNG.SetSeed(12345, false)   -- reseed RNG only, leave noise tables untouched

local seed   = RNG.GetSeed()    -- internal 64-bit master seed as a number (may lose precision
                                -- above 2^53 — to SHARE a seed, keep the original value/string
                                -- you passed to SetSeed)
local seeded = RNG.IsSeeded()   -- true once a master seed has been set
RNG.Reset()                     -- forget the master seed and all streams (back to non-deterministic)
```

#### Default stream (quick draws)

```lua
RNG.Value()                 -- float 0..1
RNG.Range(10.0, 50.0)       -- float in [min, max]
RNG.RangeInt(1, 6)          -- int in [min, max], unbiased (no modulo bias) — like a d6
RNG.Int(1, 6)               -- alias of RangeInt
RNG.Bool()                  -- true/false (50%)
RNG.Bool(0.25)              -- true with 25% chance
RNG.Sign()                  -- -1.0 or +1.0
RNG.Index(count)            -- 1..count (1-based — safe as a Lua array index)
RNG.Angle()                 -- 0..360 (degrees)

RNG.Choice({"a", "b", "c"})            -- random array element
RNG.Weighted({10, 5, 1})               -- weighted index (1 is 10x likelier than 3)
RNG.Shuffle(myArray)                   -- in-place Fisher–Yates shuffle
local dir = RNG.UnitVector()           -- {x, y}, length 1
local p   = RNG.PointInCircle(radius)  -- {x, y}, uniform inside a circle
```

#### Named streams (independent, decoupled sub-generators)

Named streams are the key trick for stable seeds: give each subsystem its own stream so that
drawing one extra loot roll never shifts monster spawns or level layout. Each named stream is
deterministically derived from the master seed.

```lua
local loot   = RNG.Stream("loot")
local mobs   = RNG.Stream("monsters")
local layout = RNG.Stream("layout")

local item  = loot:Choice(itemPool)
local hp    = mobs:RangeInt(10, 20)
local rooms = layout:RangeInt(5, 9)

-- A stream object exposes the same set of methods as the RNG.* default stream:
--   s:Value()  s:Range(a,b)  s:RangeInt(a,b)  s:Int(a,b)  s:Bool(p)  s:Sign()
--   s:Index(n) s:Angle()  s:Choice(t)  s:Weighted(t)  s:Shuffle(t)
--   s:UnitVector()  s:PointInCircle(r)  s:GetSeed()  s:Name()  s:Reset()

RNG.HasStream("loot")      -- has this stream been created yet?
RNG.ResetStream("loot")    -- rewind a single stream back to its derived start
```

> Stream handles are safe to keep in locals or fields — they resolve by name internally, so they
> keep working across `RNG.LoadState`, `RNG.Reset` and `RNG.SetSeed`.

#### Save / load (exact resume)

```lua
local blob = RNG.SaveState()    -- serialize master seed + every stream's exact position
-- ... store blob in your save file ...
RNG.LoadState(blob)             -- restore — the next draw continues bit-for-bit
```

#### Example: a reproducible roguelike floor generator

```lua
function GenerateFloor(depth)
    -- one reproducible stream per floor; including depth keeps floors independent
    local r = RNG.Stream("floor." .. depth)
    local roomCount = r:RangeInt(6, 10)
    for i = 1, roomCount do
        local w     = r:RangeInt(4, 9)
        local h     = r:RangeInt(4, 9)
        local theme = r:Weighted({60, 30, 10})   -- 1 = normal, 2 = treasure, 3 = shop
        SpawnRoom(w, h, theme)
    end
end
```

---

## 26. Events — Event System

> **Type:** Global functions
>
> Lets entities communicate without direct references.

```lua
-- Subscribe to event
local listenerId = On("PlayerDied", function(...)
    Print("Player died!")
end)

-- Emit event (all listeners receive)
Emit("PlayerDied")
Emit("DamageDealt", 50, "fire")  -- With args

-- Unsubscribe
Off(listenerId)                   -- By listener ID
Off("PlayerDied")                 -- All listeners for this event
Off("PlayerDied", listenerId)     -- Specific listener
```

### Example: damage system

```lua
-- Enemy.ice_class
function OnCreate()
    health = 100
    On("DamageAll", function(amount)
        health = health - amount
        if health <= 0 then DestroySelf() end
    end)
end

-- Player.ice_class
function OnUpdate(dt)
    if IsKeyJustPressed("q") then
        Emit("DamageAll", 25)  -- Damage all subscribers
    end
end
```

---

## 27. Gameplay — Gameplay Systems

> **Type:** Global functions

### Object Pool

Optimization pattern: reuse objects instead of constantly creating/destroying them.

```lua
local bulletPool = ObjectPool({
    create = function()
        return SpawnEntity("Content/Classes/Bullet.ice_class", -1000, -1000)
    end,
    onGet = function(obj)
        SetEntityEnabled(obj, true)
    end,
    onRelease = function(obj)
        SetEntityEnabled(obj, false)
        SetEntityPosition(obj, -1000, -1000)
    end,
    onDestroy = function(obj)
        DestroyEntity(obj)
    end,
    maxSize = 100,
    preload = 10    -- Pre-create 10
})

-- Get from pool
local bullet = bulletPool.Get()
SetEntityPosition(bullet, x, y)
SetEntityVelocity(bullet, 500, 0)

-- Return to pool
bulletPool.Release(bullet)

-- Info
local active = bulletPool.GetActiveCount()
local inactive = bulletPool.GetInactiveCount()
local total = bulletPool.GetTotalCount()

-- Clear pool
bulletPool.Clear()

-- Iterate active
bulletPool.ForEachActive(function(obj)
    -- handle
end)
```

### Wave System (enemy waves)

```lua
local waves = WaveSystem({
    betweenWaveDelay = 3.0,   -- Delay between waves
    waves = {
        { count = 5,  interval = 0.5 },   -- Wave 1: 5 enemies with 0.5s interval
        { count = 10, interval = 0.3 },   -- Wave 2: 10 enemies with 0.3s interval
        { count = 20, interval = 0.2 }    -- Wave 3: 20 enemies
    },
    onWaveStart = function(waveNum)
        Print("Wave " .. waveNum .. " starts!")
    end,
    onSpawn = function(waveNum, spawnIndex, waveConfig)
        SpawnEntity("Content/Classes/Enemy.ice_class",
            RandomRange(-300, 300), -500)
    end,
    onWaveEnd = function(waveNum)
        Print("Wave " .. waveNum .. " finished!")
    end,
    onAllComplete = function()
        Print("All waves completed!")
    end
})

waves.Start()

function OnUpdate(dt)
    waves.Update(dt)

    local currentWave = waves.GetCurrentWave()
    local active = waves.IsActive()
    -- waves.SkipToWave(3)
    -- waves.Stop()
end
```

### Cooldown

```lua
local dashCooldown = Cooldown(2.0)  -- 2 seconds cooldown

function OnUpdate(dt)
    dashCooldown.Update(dt)

    if IsKeyJustPressed("shift") then
        if dashCooldown.Use() then  -- true if cooldown passed
            -- Execute dash!
            AddImpulse(500, 0)
            Print("Dash!")
        else
            Print("On cooldown! Remaining: " .. dashCooldown.GetRemaining() .. "s")
        end
    end

    local ready = dashCooldown.IsReady()
    local progress = dashCooldown.GetProgress()  -- 0..1
    dashCooldown.Reset()  -- Reset cooldown
    dashCooldown.ForceUse()  -- Force-start the cooldown
    dashCooldown.SetDuration(3.0)  -- Change duration
end
```

### AchievementSystem (achievements)

Local achievement system with persistence. Supports two types: simple (`simple`) and incremental (`incremental`), as well as hidden achievements, auto-save, and unlock timestamps.

Data is saved in `Saves/` as JSON via `PlatformPaths` — works on all platforms (Windows, Linux, Android, Web).

```lua
-- Create system
local achievements = AchievementSystem({
    autoSave = true,                        -- Auto-save on Unlock/AddProgress
    savePath = "achievements.json",          -- Path in Saves/
    onUnlock = function(id, achievement)
        Print("Unlocked: " .. achievement.title)
    end,
    onProgress = function(id, current, target, achievement)
        Print(id .. ": " .. current .. "/" .. target)
    end
})

-- Define achievements
achievements.Define({
    id = "first_kill",
    title = "First Blood",
    description = "Kill your first enemy",
    icon = "Content/Icons/first_kill.png",
    type = "simple"
})

achievements.Define({
    id = "kill_100",
    title = "Mass Destruction",
    description = "Kill 100 enemies",
    type = "incremental",
    target = 100
})

achievements.Define({
    id = "secret_room",
    title = "???",
    description = "Find the secret room",
    type = "simple",
    hidden = true
})

-- Batch define multiple achievements at once
achievements.DefineBatch({
    { id = "level_5", title = "Level 5", type = "simple" },
    { id = "coins_500", title = "Rich", type = "incremental", target = 500 }
})

-- Load saved progress (call after Define/DefineBatch)
achievements.Load()

-- Unlock
achievements.Unlock("first_kill")  -- Returns true if first time

-- Incremental progress
achievements.AddProgress("kill_100", 1)   -- +1 (auto-unlocks when >= target)
achievements.AddProgress("kill_100")       -- +1 (default)
achievements.SetProgress("kill_100", 50)   -- Set directly

-- Queries
local unlocked = achievements.IsUnlocked("first_kill")   -- true/false
local progress = achievements.GetProgress("kill_100")    -- current progress
local target = achievements.GetTarget("kill_100")        -- target value
local pct = achievements.GetProgressPercent("kill_100")  -- 0.0..1.0
local ach = achievements.Get("first_kill")               -- achievement table

-- Lists (in definition order)
local all = achievements.GetAll()
local done = achievements.GetUnlocked()
local left = achievements.GetLocked()

-- Statistics
local total = achievements.GetCount()
local doneCount = achievements.GetUnlockedCount()
local completion = achievements.GetCompletionPercent()  -- 0.0..1.0

-- Reset
achievements.Reset("first_kill")   -- Reset one
achievements.ResetAll()            -- Reset all

-- Persistence
achievements.Save()                -- Save to Saves/achievements.json
achievements.Save("slot2.json")    -- Save to different file
achievements.Load()                -- Load
local exists = achievements.HasSave()     -- Does file exist
achievements.DeleteSave()                 -- Delete file

-- Callbacks can be set/updated after creation
achievements.SetCallbacks({
    onUnlock = function(id, ach) end,
    onProgress = function(id, cur, target, ach) end
})
```

**All AchievementSystem methods:**

| Method | Returns | Description |
|--------|---------|-------------|
| `Define(def)` | — | Define an achievement |
| `DefineBatch(defs)` | — | Define multiple achievements |
| `Unlock(id)` | bool | Unlock (true if first time) |
| `AddProgress(id, amount?)` | bool | Add progress (true if unlocked) |
| `SetProgress(id, value)` | bool | Set progress (true if unlocked) |
| `IsUnlocked(id)` | bool | Check if unlocked |
| `GetProgress(id)` | int | Current progress (incremental only) |
| `GetTarget(id)` | int | Target value (incremental only) |
| `GetProgressPercent(id)` | float | Progress 0.0..1.0 |
| `Get(id)` | table/nil | Achievement table |
| `GetAll()` | table | All achievements (in Define order) |
| `GetUnlocked()` | table | Unlocked achievements |
| `GetLocked()` | table | Locked achievements |
| `GetCount()` | int | Total count |
| `GetUnlockedCount()` | int | Unlocked count |
| `GetCompletionPercent()` | float | Overall percent 0.0..1.0 |
| `Reset(id)` | — | Reset an achievement |
| `ResetAll()` | — | Reset all achievements |
| `Save(path?)` | bool | Save to file |
| `Load(path?)` | bool | Load from file |
| `HasSave(path?)` | bool | Does file exist |
| `DeleteSave(path?)` | bool | Delete file |
| `SetCallbacks(cbs)` | — | Set callbacks |

**Achievement fields** (table from `Get(id)`):

| Field | Type | Description |
|-------|------|-------------|
| `id` | string | Unique identifier |
| `title` | string | Display name |
| `description` | string | Description |
| `icon` | string | Icon path |
| `type` | string | `"simple"` or `"incremental"` |
| `hidden` | bool | Hidden achievement |
| `unlocked` | bool | Whether unlocked |
| `unlockTime` | string | Unlock date/time |
| `current` | int | Current progress (incremental only) |
| `target` | int | Target value (incremental only) |

**JSON format** (`Saves/achievements.json`):

```json
{
  "first_kill": {
    "unlocked": true,
    "unlockTime": "2025-01-15 14:30:00"
  },
  "kill_100": {
    "unlocked": false,
    "unlockTime": "",
    "current": 47
  }
}
```

### SoftRef — Lazy / Async Asset Reference

Create a reference to an asset without loading it.
Type is detected automatically from the file extension.

```lua
-- Create soft references (nothing loaded yet)
local bossTexture = SoftRef("Content/Textures/Boss.png")           -- auto: "texture"
local bossSound   = SoftRef("Content/Sounds/BossRoar.ogg")         -- auto: "sound"
local config      = SoftRef("Content/Data/config.txt", "file")     -- explicit type

-- Start async loading
bossTexture.Load()
bossSound.Load()

-- In OnUpdate:
function OnUpdate(dt)
    if bossTexture.IsLoaded() and bossSound.IsLoaded() then
        -- Safe to use
        SetSpriteTexture(bossTexture.Get())
        PlaySound(bossSound.Get())
    end
end

-- Other methods:
bossTexture.IsLoading()   -- true while in flight
bossTexture.IsValid()     -- true if path is not empty
bossTexture.GetPath()     -- "Content/Textures/Boss.png"
bossTexture.GetType()     -- "texture"
bossTexture.Unload()      -- release the resource
```

| Method | Returns | Description |
|--------|---------|-------------|
| `SoftRef(path, type?)` | table | Create a soft reference (type auto-detected from extension) |
| `:Load()` | — | Start loading the asset |
| `:IsLoaded()` | bool | True when fully loaded |
| `:IsLoading()` | bool | True while loading |
| `:IsValid()` | bool | True if path is non-empty |
| `:Get()` | string | Resource name for use with other APIs |
| `:GetPath()` | string | Original file path |
| `:GetType()` | string | `"texture"`, `"sound"`, or `"file"` |
| `:Unload()` | — | Release the resource |

### Batch Texture Preloading

```lua
-- Preload multiple textures at once
PreloadTextures({
    "Content/Textures/Boss.png",
    "Content/Textures/Arena.png",
    "Content/Textures/Effects.png"
})

-- Check individual texture
local ready = IsTextureLoaded("Content/Textures/Boss.png")

-- Check all at once
local allDone = AreAllTexturesLoaded({
    "Content/Textures/Boss.png",
    "Content/Textures/Arena.png"
})
```

| Function | Returns | Description |
|----------|---------|-------------|
| `PreloadTextures(paths)` | — | Start async loading for all listed textures |
| `IsTextureLoaded(name)` | bool | True if the texture is ready |
| `AreAllTexturesLoaded(paths)` | bool | True if all listed textures are ready |

### PersistentTable — Auto-Persisting Data Store

A key-value store that can be saved/loaded to disk. Ideal for stats, settings, achievements.

```lua
local stats = PersistentTable("game_stats", "stats.json")
stats.Load()  -- load from disk if exists

-- Set / Get
stats.Set("total_kills", 0)
local kills = stats.Get("total_kills", 0)    -- 0 = default
stats.Has("total_kills")                      -- true
stats.Remove("temp_key")

-- Arithmetic
stats.Increment("total_kills", 1)             -- +1 (int or float)
stats.GetMax("best_score", newScore)          -- store only if higher
stats.GetMin("best_time", newTime)            -- store only if lower

-- Persistence
stats.Save()                                   -- save to disk
stats.Save("backup.json")                     -- to specific file
stats.IsDirty()                                -- unsaved changes?
stats.Exists()                                 -- save file exists?
stats.Delete()                                 -- delete save file

-- Auto-save: saves every N seconds if dirty
stats.AutoSave(30.0)
function OnUpdate(dt)
    stats.Update(dt)  -- required for auto-save
end

-- Utility
local data = stats.ToTable()                   -- get all as Lua table
stats.Clear()                                  -- clear all data
```

| Method | Returns | Description |
|--------|---------|-------------|
| `PersistentTable(name, path?)` | table | Create a persistent store |
| `:Set(key, value)` | — | Set a value (string, number, bool) |
| `:Get(key, default?)` | any | Get a value with optional default |
| `:Has(key)` | bool | Check if key exists |
| `:Remove(key)` | — | Remove a key |
| `:Increment(key, amount?)` | — | Add to a numeric value |
| `:GetMax(key, value)` | — | Store only if greater than current |
| `:GetMin(key, value)` | — | Store only if less than current |
| `:Save(path?)` | bool | Save to disk |
| `:Load(path?)` | bool | Load from disk |
| `:AutoSave(seconds)` | — | Enable auto-save interval |
| `:Update(dt)` | — | Tick for auto-save (call in OnUpdate) |
| `:IsDirty()` | bool | Has unsaved changes |
| `:ToTable()` | table | Export all data as Lua table |
| `:Clear()` | — | Clear all entries |
| `:Exists()` | bool | Does save file exist on disk |
| `:Delete()` | bool | Delete the save file |

---

## 28. Localization — Localization

> **Type:** Global functions

```lua
-- Load localization file
LoadLocalization("Content/Localization/strings.json")

-- Set language
SetGameLanguage("ru")
local lang = GetGameLanguage()

-- Get localized string
local text = Localize("menu_play")   -- → "Play"

-- With formatting args
local text = LocalizeFmt("damage_dealt", 50, "fire")
-- Template: "Dealt {0} damage of type {1}" → "Dealt 50 damage of type fire"

-- Available languages
local langs = GetAvailableLanguages()  -- → {"en", "ru", "zh"}

-- Check key
local has = HasLocalizationKey("menu_play")
```

> When the language changes, `OnLanguageChanged(newLang, oldLang)` is called in all entity scripts.

---

## 29. Debug — Debugging

> **Type:** Global functions

```lua
-- Output to engine console
Print("Hello!")                     -- Info (white)
PrintWarning("Careful!")           -- Warning (yellow)
PrintError("Error!")                -- Error (red)

-- On-screen output (overlay)
PrintScreen("FPS: " .. GetFPS())
PrintScreen("Debug", 1, 0, 0, 1, 5.0)
-- text, r, g, b, a, duration, key, scale
-- duration: > 0 seconds, == 0 single frame, < 0 persistent
-- key: >= 0 to overwrite same-key message instead of stacking (ideal for tick)
-- scale: text size multiplier (1.0 default)
PrintScreen("HP: 100", 1, 1, 0, 1, 0.0, 1, 1.5)  -- per-frame, key=1, scale 1.5x
PrintScreen("Boss spawned!", 1, 0, 0, 1, -1)     -- persistent until ClearScreenMessages
PrintScreenEx{ text = "Score: 42", color = {0,1,0,1}, duration = 0, key = 2, scale = 2.0 }
RemoveScreenMessage(1)                           -- remove keyed message

-- World text
DrawWorldText("Hello", 100, 200)
DrawWorldText("Alert!", 100, 200, 1, 0, 0, 1, 2.0, 1.5)
-- text, worldX, worldY, r, g, b, a, duration, scale, key
DrawWorldText("HP", 100, 200, 1, 1, 0, 1, 0, 1.0, 7)  -- per-frame, key=7
DrawWorldTextEx{ text = "Boss", x = 100, y = 240, color = {1,0,0,1}, duration = 0, key = 8 }
RemoveWorldText(7)                               -- remove keyed world text

-- Clear
ClearScreenMessages()
ClearWorldText()
```

#### On-screen debug print — full reference

| Function | Signature | Description |
|----------|-----------|-------------|
| `PrintScreen` | `PrintScreen(msg, r?, g?, b?, a?, duration?, key?, scale?)` | Adds a message to the on-screen overlay. Defaults: color `1,1,1,1`, `duration = 5.0`, `key = -1`, `scale = 1.0`. |
| `PrintScreenEx` | `PrintScreenEx{ text=, msg=, color={r,g,b,a}, r=, g=, b=, a=, duration=, key=, scale=, size= }` | Table form with named arguments. `text` and `msg` are aliases; `scale` and `size` are aliases. `color` overrides individual `r,g,b,a`. |
| `RemoveScreenMessage` | `RemoveScreenMessage(key)` | Removes a previously added message with the given non-negative `key`. No-op for `key < 0`. |
| `ClearScreenMessages` | `ClearScreenMessages()` | Clears all on-screen debug messages (both keyed and non-keyed). |
| `SetScreenEnabled` | `SetScreenEnabled(enabled)` | Globally shows/hides the on-screen overlay and world text without clearing queued messages. Honored identically in the editor viewport and in a build. Enabled by default. |
| `IsScreenEnabled` | `IsScreenEnabled()` | Returns whether the on-screen overlay is currently enabled. |

#### World text — full reference

| Function | Signature | Description |
|----------|-----------|-------------|
| `DrawWorldText` | `DrawWorldText(text, worldX, worldY, r?, g?, b?, a?, duration?, scale?, key?)` | Draws text at a world position. Defaults: color `1,1,1,1`, `duration = 0` (single frame), `scale = 1.0`, `key = -1`. |
| `DrawWorldTextEx` | `DrawWorldTextEx{ text=, msg=, x=, y=, color={r,g,b,a}, r=, g=, b=, a=, duration=, key=, scale=, size= }` | Table form with named arguments, mirroring `PrintScreenEx`. |
| `RemoveWorldText` | `RemoveWorldText(key)` | Removes world text with the given non-negative `key`. No-op for `key < 0`. |
| `ClearWorldText` | `ClearWorldText()` | Clears all world text. |

`duration` and `key` behave exactly as for `PrintScreen`: `key >= 0` overwrites the entry with the same key instead of stacking a new one, which is what you want when drawing a label every tick. Off-screen world text is culled and costs nothing to draw.

**Fonts & platforms:** The overlay and world text use the operating-system UI font resolved at runtime (Segoe UI on Windows, San Francisco on macOS/iOS, DejaVu/Noto/Liberation on Linux, Roboto on Android) — no font asset is shipped with the game for this. On Web the browser does not expose OS font files, so overlay text is not drawn there. The overlay is drawn on top of the scene and game UI with depth testing off, honoring the window pixel size; world text follows the engine's Y-up / X-right coordinate system.

**Parameters:**

| Argument | Type | Default | Notes |
|----------|------|---------|-------|
| `msg` / `text` | `string` | — | Message text. Newlines (`\n`) are not supported in a single call — use multiple calls. |
| `r`, `g`, `b`, `a` | `number` | `1.0` | Color channels in `[0..1]`. `a` controls opacity. Non-persistent entries fade out over the last `min(0.5 s, duration / 2)` — so short durations are shown at full opacity first and still fade, instead of being dimmed for their whole life. Single-frame entries never fade. |
| `duration` | `number` | `5.0` | `> 0` — seconds (real, unscaled time); `== 0` — **exactly one rendered frame** at any framerate (perfect for per-tick logging without a key); `< 0` — persistent (stays until `ClearScreenMessages` or `RemoveScreenMessage`). |
| `key` | `integer` | `-1` | `>= 0` — overwrites an existing entry with the same key. `-1` — always appends. |
| `scale` / `size` | `number` | `1.0` | Text size multiplier. `0` or negative falls back to `1.0`. |

**Patterns:**

```lua
function OnUpdate(dt)
    PrintScreen("FPS: " .. math.floor(1.0 / dt), 0, 1, 0, 1, 0.0, 1)
    PrintScreen("HP: " .. self.hp,             1, 1, 0, 1, 0.0, 2, 1.5)
    PrintScreen("Pos: " .. self.x .. ", " .. self.y, 0, 1, 1, 1, 0.0, 3)
end

PrintScreen("Connected to server", 0, 1, 0, 1, 4.0)

PrintScreen("Boss in arena", 1, 0, 0, 1, -1, 999)
RemoveScreenMessage(999)

PrintScreenEx{
    text     = "Score: " .. score,
    color    = { 1.0, 0.85, 0.0, 1.0 },
    duration = 0,
    key      = 10,
    scale    = 2.0,
}
```

### Debug Draw — debug primitives

Functions for drawing debug lines, circles, rectangles, arrows, and advanced shapes.
They run through `PhysicsDebugDraw` and are useful for tracing, AI, and collider visualization.
They are drawn in **world pixels** and are available in the editor viewport, in Debug builds and in Release builds alike.

#### Lifetime (`duration`) — identical rules for every debug primitive

| `duration` | Meaning |
|------------|---------|
| `== 0` (default) | **Exactly one rendered frame**, at any framerate. Call it every tick from `OnUpdate`/`OnLateUpdate` for a continuous overlay — nothing accumulates. |
| `> 0` | Seconds of real (unscaled) time. Use it for one-shot events you need to actually see, e.g. an impact marker at `0.4`. |
| `< 0` | Persistent — stays until `ClearDebugDraw()` or until Play stops. |

> `duration` is real time and keeps ticking while the game is paused or time-scaled; it matches `PrintScreen` exactly.

```lua
-- Basic primitives
DrawLine(0, 0, 100, 100, 1, 0, 0, 2.0)         -- line
DrawCircle(50, 50, 25, 0, 1, 0)                 -- circle
DrawRect(10, 10, 80, 40, 0, 0, 1, 1.0)          -- rectangle
DrawArrow(0, 0, 50, 50, 1, 1, 0, 0.5, 8)         -- arrow
DrawFilledRect(10, 10, 80, 40, 0, 1, 0, 0.3, 2.0) -- filled rect (x, y, w, h, r, g, b, a, duration)
DrawSelectionRect(10, 10, 90, 50, 0, 1, 0, 0.15, 2.0)  -- selection rect with border + fill

-- Advanced primitives
DrawDebugPoint(100, 100, 8.0, 1, 1, 0, 2.0)
DrawDebugArrow(120, 120, 200, 150, 12.0, 0, 1, 0, 2.0)
DrawDebugCapsule(200, 200, 30, 10, 0, 1, 0, 2.0)
DrawDebugCross(250, 200, 15.0, 1, 0, 0, 2.0)
DrawDebugPolygon({
    {x = 0, y = 0},
    {x = 50, y = 0},
    {x = 50, y = 50},
    {x = 0, y = 50}
}, 0, 1, 1, 2.0)
DrawDebugGrid(400, 300, 32, 10, 10, 0.3, 0.3, 0.3, 0.0)   -- duration 0 = one frame
DrawDebugCoordinateSystem(0, 0, 50.0, 2.0)
```

> **Careful with argument order:** `DrawArrow(x1, y1, x2, y2, r, g, b, duration, headSize)` takes `headSize` **last**, while `DrawDebugArrow(x1, y1, x2, y2, headSize, r, g, b, duration)` takes it **first**. The two are kept as-is for backwards compatibility.

### Debug Draw (Physics) — extended aliases

```lua
DrawDebugLine(x1, y1, x2, y2, 1, 0, 0, 2.0)
DrawDebugCircle(x, y, radius, 0, 1, 0, 2.0)
DrawDebugBox(cx, cy, halfW, halfH, 0, 0, 1, 2.0)
DrawDebugPoint(x, y, 8.0, 1, 1, 0, 2.0)
DrawDebugArrow(x1, y1, x2, y2, 12.0, 1, 0, 0, 2.0)
DrawDebugCapsule(cx, cy, halfHeight, radius, 0, 1, 0, 2.0)
DrawDebugCross(cx, cy, 15.0, 1, 0, 0, 2.0)
DrawDebugPolygon({{x=0,y=0},{x=50,y=0},{x=50,y=50}}, 0, 1, 1, 2.0)
DrawDebugGrid(cx, cy, 32, 10, 10, 0.3, 0.3, 0.3, 0.0)
DrawDebugCoordinateSystem(0, 0, 50.0, 2.0)
ClearDebugDraw()
```

### Lua Script Debugger (Text and Visual)

IceBox ships **two source-level debuggers** that attach to the live Lua VM during Play Mode. They share one runtime backend but are **mutually exclusive** — only one is ever attached, so they never fight over the VM:

| Debugger | For projects in… | Breakpoints on… | Where it lives |
|----------|------------------|-----------------|----------------|
| **Text debugger** | Code mode (hand-written Lua) | source **lines** | the `Lua Script Debugger` panel |
| **Visual debugger** | Visual mode (node graphs) | **nodes** | directly inside the node graph editor |

> The coding mode is chosen per project in the launcher, so a project is either **Code** or **Visual** and you normally use just one. The engine guards against both attaching at once regardless of mode.

#### Text debugger — the `Lua Script Debugger` panel

Open the panel from the editor's window menu. It debugs the hand-written Lua embedded in your assets:

- **Pick an asset** — the left list shows every `.ice_class`, `.ice_widget` and `.icemap`. `Rescan Assets` rebuilds the list; the filter box narrows it.
- **Attach / detach** — `Start Debug` installs the line hook, `Stop Debug` removes it. You can attach before or during Play.
- **Breakpoints** — click the line gutter to toggle one. Each can be **enabled/disabled**, carry a **conditional expression** (it breaks only when the expression is truthy) and tracks a **hit count**. Double-click a breakpoint in the list to edit its condition.
- **Execution control** — `Continue`, `Step Over`, `Step Into`, `Step Out`, and `Pause` (breaks on the next executed Lua line).
- **Inspect while paused** — `Variables` shows Locals, upvalues, enclosing scopes and Globals (tables expand on demand); `Watch` evaluates arbitrary expressions; `Call Stack` lists the frames; the log records every hit and step. The paused line is marked in the source view.
- **Live values** — even without pausing, the panel samples local values a few times per second so you can watch them change in real time.

> **Generated Lua is read-only.** Open a **visual** asset here and you'll see the Lua compiled from its node graph. The panel labels it as generated and **won't place line breakpoints** on it — debug the graph instead (below). Line breakpoints persist to `Config/DebugBreakpoints.json`.

> **Level scripts** execute under the chunk name `LevelScript`, so breakpoints set in an `.icemap` resolve against the running level script.

#### Visual debugger — debugging inside the node graph

In a **Visual Scripting** project you debug the **graph itself** — there is no need to read the generated Lua. Everything happens on the canvas of the Class, Widget or Level graph editor:

- **Node breakpoints** — click the red dot at a node's top-left corner, or right-click the node → `Add Breakpoint` / `Remove Breakpoint`. They persist per asset to `Config/VSBreakpoints.json` and survive restarts.
- **Active-node highlight** — when execution stops, the current node **pulses**; with `Follow` enabled the view re-centers on it automatically.
- **Execution flow** — exec wires that just carried control **animate** with travelling pulses, so you can watch the path your logic took.
- **Pin values** — while paused, output pins display their **live runtime values** as inline badges next to the pin.
- **Debug toolbar** (top of the canvas during Play) — `Continue`, `Step Over`, `Step Into`, `Step Out`, `Pause`, `Stop`, `Focus`, a `Follow` toggle and a status indicator.
- **Debug tab** (bottom, next to `Problems`) — `Call Stack` (click a frame to jump to its node), `Watches` (variable names or expressions) and the `Breakpoints` list (click to focus a node, or clear them all).

> **Compile before you debug.** The runtime runs the **saved** Lua and the node→line mapping is rebuilt from the current graph, so **save the graph before pressing Play** — the same "compile first" rule. The debug hook only attaches when at least one node breakpoint exists (or you press `Pause`), so a graph with no breakpoints runs at full speed.

The visual debugger works across all three graph surfaces — **Class**, **Widget** and **Level** — and reuses the same step engine as the text debugger, so `Step Over/Into/Out` behave identically.

### Debug functions: how to use

`Print*` and `PrintScreen` are handy for quick diagnostics during Play Mode:

- `Print`, `PrintWarning`, `PrintError` write to the editor console.
- `PrintScreen` draws text over the game (useful for FPS, states, values).
- `DrawWorldText` draws text in the world with position and scale.
- `ClearScreenMessages` and `ClearWorldText` clear debug output.

### Debug Build Mode (runtime)

When you package your game in the **Debug** configuration, the runtime includes a full debug
overlay system. This happens automatically on every platform (Windows, Linux, Android, Web);
**Release** builds ship without it.

#### Driven entirely from Lua

The runtime debug system exposes a small, focused Lua API: a getter that reports whether
an overlay is currently visible, and a toggle that flips its visibility. There are no
built-in keyboard shortcuts — every project picks its own keys, gamepad buttons, console
commands or UI widgets to drive the overlays:

```lua
function OnUpdate(dt)
    if IsKeyJustPressed("f4") then ToggleDebugColliders()      end
    if IsKeyJustPressed("f5") then ToggleDebugEntityMarkers()  end
    if IsKeyJustPressed("f6") then ToggleDebugNavGrid()        end
    if IsKeyJustPressed("f3") then ToggleDebugProfiler()       end
    if IsKeyJustPressed("f9") then NetworkProfiler.Toggle()    end

    if IsKeyJustPressed("f7") then
        if IsProfilerTracing() then StopProfilerTrace()
        else StartProfilerTrace("RuntimeTrace") end
    end
    if IsKeyJustPressed("f8") then SaveChromeTrace() end
end
```

> The Lua API itself works in **both Debug and Release** builds and on every platform.
> The actual overlay rendering is only compiled into Debug builds, so in Release
> builds the toggle still flips the underlying flag but no debug visuals are drawn —
> perfect for development menus you ship in internal packages.

#### Lua API — Debug Build Control

```lua
local isDebug = IsDebugBuild()

local on = GetDebugColliders()
ToggleDebugColliders()

local on = GetDebugEntityMarkers()
ToggleDebugEntityMarkers()

local on = GetDebugNavGrid()
ToggleDebugNavGrid()

local on = GetDebugProfilerVisible()
ToggleDebugProfiler()
```

| Function | Signature | Description |
|----------|-----------|-------------|
| `IsDebugBuild` | `IsDebugBuild()` → `bool` | `true` in Debug builds, `false` in Release. Always available on every platform. |
| `GetDebugColliders` / `ToggleDebugColliders` | `() → bool` / `()` | Read or flip the physics collider wireframe overlay (Box, Sphere, Capsule, Polygon). |
| `GetDebugEntityMarkers` / `ToggleDebugEntityMarkers` | `() → bool` / `()` | Read or flip the entity marker overlay (Rigidbody, Lights, Camera, Audio, FX). |
| `GetDebugNavGrid` / `ToggleDebugNavGrid` | `() → bool` / `()` | Read or flip the navigation grid + AI debug overlay (paths, patrol points, perception cones). |
| `GetDebugProfilerVisible` / `ToggleDebugProfiler` | `() → bool` / `()` | Read or flip the runtime profiler overlay (FPS, frame time, CPU/RAM/VRAM/GPU, render passes, hot scopes). |
| `NetworkProfiler.IsVisible` / `NetworkProfiler.Toggle` | `() → bool` / `()` | Same pair for the network profiler overlay. |
| `GetDebugFlag(name)` | `(string) → bool` | Generic getter for any viewport debug flag (see `GetDebugFlagNames`). |
| `SetDebugFlag(name, value)` | `(string, bool) → bool` | Generic setter for any viewport debug flag. Returns `true` if the name is valid. |
| `ToggleDebugFlag(name)` | `(string) → bool` | Generic toggle for any viewport debug flag. Returns the new value. |
| `ClearDebugFlags` | `()` | Reset every viewport debug flag to `false`. |
| `GetDebugFlagNames` | `() → table` | Returns the array of all 21 supported flag names. |

Supported flag names for `GetDebugFlag` / `SetDebugFlag` / `ToggleDebugFlag`:
`ShowColliders`, `ShowNavGrid`, `ShowEntityMarkers`, `ShowLightRadius`, `ShowAudioRange`,
`ShowCameraFrustum`, `ShowJoints`, `ShowPhysicsContacts`, `ShowSleepingBodies`,
`ShowVelocityVectors`, `ShowTilemapGrid`, `ShowFXBounds`, `ShowWidgetBounds`,
`ShowZDepthColor`, `WireframeMode`, `FreezeCulling`, `ShowShadowMaps`, `ShowShadowEdges`,
`ShowLightHeatmap`, `ShowNavGridHeatmap`, `ShowAIStateOverlay`.

> **Build-time gating:** every debug overlay (collider wireframes, entity markers, nav grid,
> velocity/contact vectors, light/audio/FX/widget overlays, wireframe & freeze-culling, shadow
> visualisation, AI state overlay, profiler/network overlays, etc.) is rendered in
> **editor builds (any configuration)** and **standalone-game Debug builds**. In a **shipping
> game build** (`ICE_RUNTIME_BUILD=ON` + Release) the rendering is fully compiled out, but
> the Lua API still exists as no-ops so cross-build scripts keep working without `pcall`.

#### Example: conditional debug rendering

```lua
function OnUpdate(dt)
    if IsDebugBuild() then
        PrintScreen("HP: " .. self.hp, 1, 1, 0, 1, 0)
        DrawCircle(self.x, self.y, self.detectionRadius, 1, 0, 0)
    end
end

function OnBeginPlay()
    if IsDebugBuild() then
        if not GetDebugColliders() then ToggleDebugColliders() end
        if not GetDebugNavGrid()   then ToggleDebugNavGrid()   end
    end
end
```

#### Lua API — Profiler Traces (Chrome Trace Event Format)

The profiler can record CPU/GPU scopes, render passes and counters into a trace and export
it to the **Chrome Trace Event Format** (`.json`). The resulting file opens without any
plugins in:

- `chrome://tracing` (all Chromium browsers — Chrome, Edge, Opera, Brave, Vivaldi);
- [ui.perfetto.dev](https://ui.perfetto.dev) — the modern Google viewer (recommended);
- [profiler.firefox.com](https://profiler.firefox.com) — Firefox Profiler (Import JSON);
- [speedscope.app](https://speedscope.app) — flame-graph viewer.

The API is cross-platform and works in both Debug and Release builds (the trace viewer
format is an open standard, so there is no vendor lock-in).

```lua
-- Start recording a named trace (returns true if a new trace started).
StartProfilerTrace()                     -- default name "LuaTrace"
StartProfilerTrace("BossFight")          -- custom name

-- Stop the active trace and push it into the profiler history.
StopProfilerTrace()

-- Query whether a trace is currently being recorded.
if IsProfilerTracing() then
    PrintScreen("REC", 1, 1, 0, 1, 0)
end

-- Export the most recent trace (or a live snapshot when none is finished yet)
-- to Chrome Trace Event Format. Without an argument the file name is generated
-- automatically inside the profiler output folder. On Web the browser will
-- automatically offer the .json file as a download.
SaveChromeTrace()                        -- auto filename in Profiles/
SaveChromeTrace("boss_fight.json")       -- explicit file name / path
```

| Function | Signature | Description |
|----------|-----------|-------------|
| `StartProfilerTrace` | `StartProfilerTrace([name])` → `bool` | Begins a new trace. `name` is optional — leave it out and the profiler assigns one. Returns `true` if recording actually started. |
| `StopProfilerTrace` | `StopProfilerTrace()` | Stops the active trace and stores it in the trace history (visible in the editor Profiler panel). No-op if nothing is being recorded. |
| `IsProfilerTracing` | `IsProfilerTracing()` → `bool` | Returns `true` while a trace is being recorded. |
| `SaveChromeTrace` | `SaveChromeTrace([filename])` → `bool` | Writes the latest finished trace (or the live one, or a single-frame snapshot if no trace exists) as Chrome Trace Event JSON. On Emscripten (Web) additionally triggers a browser download. Returns `true` on success. |

> **Bind your own keys.** Wire the trace API up to whatever input fits your project — for
> example `IsKeyJustPressed("f7")` for start/stop and `IsKeyJustPressed("f8")` for export,
> a gamepad chord, a developer console command, or a debug UI button. The Lua API works
> on every platform and in every configuration; the JSON export is available everywhere
> while the live in-engine overlay requires a Debug build.

```lua
-- Example: record a trace of a specific gameplay window and auto-export it.
function OnBeginPlay()
    StartProfilerTrace("IntroCutscene")
end

function OnCutsceneFinished()
    if IsProfilerTracing() then
        StopProfilerTrace()
        SaveChromeTrace("intro_cutscene.json")
    end
end
```

#### Lua API — Custom Profile Scopes

Wrap any block of Lua code in a named profile scope. Scopes are visible in:

- the **runtime profiler overlay** (`ToggleDebugProfiler()` / `GetDebugProfilerVisible()`);
- the editor **Profiler** panel (per-frame breakdown);
- exported **Chrome Trace Event** JSON files (`SaveChromeTrace`).

Works in both Debug and Release builds and on every platform (Windows, Linux, Web, Android).
Scope names passed from Lua are interned internally, so `ProfileBegin`/`ProfileEnd` pairs are matched by pointer equality just like native `IB_PROFILE_SCOPE` macros.

```lua
-- Manual pairing (must match by name, can be nested).
ProfileBegin("AI.Update")
    UpdateEnemies(dt)
    ProfileBegin("AI.Pathfinding")
        RecalculatePaths()
    ProfileEnd("AI.Pathfinding")
ProfileEnd("AI.Update")

-- Scoped variant — automatically calls Begin/End around the function body,
-- even if the body throws a Lua error. Forwards the function's return value.
local result = ProfileScope("Boss.HeavyAttack", function()
    return ComputeAttackDamage()
end)
```

| Function | Signature | Description |
|----------|-----------|-------------|
| `ProfileBegin` | `ProfileBegin(name)` | Opens a CPU profile scope with the given name. Must be paired with a matching `ProfileEnd(name)` later in the same frame. Scopes can be nested. |
| `ProfileEnd` | `ProfileEnd(name)` | Closes the most recent scope opened with `ProfileBegin(name)`. The `name` must match the corresponding `ProfileBegin`. |
| `ProfileScope` | `ProfileScope(name, fn)` → `any` | RAII-style helper. Calls `ProfileBegin(name)`, invokes `fn()`, then guarantees `ProfileEnd(name)` is called. Returns whatever `fn` returns. |

> **Tip.** Use a hierarchical naming convention (`"AI.Update"`, `"AI.Pathfinding"`, `"Render.HUD"`) — the editor profiler and Chrome Trace viewers will group them visually.

#### Lua API — Custom Counters

Publish any gameplay or system number as a profiler **counter**. Counters need no registration: they appear immediately in the editor Profiler's **Counters** tab (grouped by the group name, colored against their budget), are recorded into every frame of a trace, are exported as Chrome Trace counter tracks, and become Tracy plots in instrumented builds.

```lua
-- A plain count.
ProfilerSetCounter("Gameplay", "Alive Enemies", #enemies)

-- With a unit and a budget: the value turns yellow/orange/red as it approaches
-- and passes 8 ms.
ProfilerSetCounter("Gameplay", "AI Think", thinkMs, "ms", 8.0)

-- Accumulate over a frame (resets automatically on the next frame).
ProfilerAddCounter("Gameplay", "Projectiles Spawned", 1)

-- Read back any counter, including the engine's own.
local contacts = ProfilerGetCounter("Physics", "Contacts")
local budgetMs = GetProfilerFrameBudgetMs()
```

| Function | Signature | Description |
|----------|-----------|-------------|
| `ProfilerSetCounter` | `ProfilerSetCounter(group, name, value[, unit[, budget]])` | Sets a counter's value for this frame. `unit` is `"ms"`, `"b"`, `"kb"`, `"mb"`, `"%"` or `"/s"`; omit it for a plain count. `budget` (optional) drives the color coding. |
| `ProfilerAddCounter` | `ProfilerAddCounter(group, name, delta)` | Adds `delta` to the counter for the current frame; the value restarts from the first `delta` on the next frame. |
| `ProfilerGetCounter` | `ProfilerGetCounter(group, name)` → `number` | Reads a counter back. Works for engine counters too (`"Physics"`, `"Renderer"`, `"FX"`, `"Audio"`, `"Assets"`, `"Shadows"`, `"Memory"`, `"Network"`, `"Lua"`, `"Components"`, `"Instances"`, `"Scene"`). |
| `GetProfilerFrameBudgetMs` | `GetProfilerFrameBudgetMs()` → `number` | The frame budget in milliseconds derived from the project's target frame rate. |

#### Lua API — Script Profiler

The engine measures every Lua callback it invokes, per script and per callback type (`OnUpdate`, `OnLateUpdate`, `OnFixedUpdate`, collision, sensor, hit, joint-break, lifecycle, behavior-tree, level and mod callbacks, plus widget scripts). These functions read and control that data from gameplay code — useful for shipping builds where the editor panel is not available.

```lua
if GetScriptProfilerTimeMs() > GetProfilerFrameBudgetMs() * 0.3 then
    for _, row in ipairs(GetScriptProfilerRows()) do
        if row.frameMs > 0.5 then
            PrintScreen(string.format("%s: %.2f ms x%d", row.script, row.frameMs, row.instances), 2.0)
        end
    end
end
```

| Function | Signature | Description |
|----------|-----------|-------------|
| `SetScriptProfilerEnabled` | `SetScriptProfilerEnabled(enabled)` | Turns per-script measurement on or off. Disabled, it costs nothing. |
| `IsScriptProfilerEnabled` | `IsScriptProfilerEnabled()` → `bool` | Whether per-script measurement is running. |
| `ResetScriptProfiler` | `ResetScriptProfiler()` | Clears accumulated per-script statistics without restarting the game. |
| `GetScriptProfilerTimeMs` | `GetScriptProfilerTimeMs()` → `number` | Total Lua time spent in engine-invoked callbacks during the last frame. |
| `GetScriptProfilerCalls` | `GetScriptProfilerCalls()` → `number` | Number of measured Lua callback invocations during the last frame. |
| `GetLuaMemoryKB` | `GetLuaMemoryKB()` → `number` | Current Lua heap size in KB. |
| `GetLuaAllocRateKBps` | `GetLuaAllocRateKBps()` → `number` | Smoothed Lua allocation rate in KB/s — a growing value means script memory churn. |
| `GetScriptProfilerRows` | `GetScriptProfilerRows()` → `table` | Array of per-script rows, sorted by last-frame cost. Each entry has `script`, `path`, `frameMs`, `avgMs`, `maxMs`, `totalMs`, `calls`, `instances`, `errors`. |

#### Lua API — Hitch Detection

The profiler automatically captures frames that stall — a frame that exceeds the threshold *and* takes more than twice the running average is stored with its complete scope tree for later inspection in the editor's **Hitches** tab.

```lua
SetProfilerHitchThreshold(33.0)
SetProfilerHitchDetection(true)

if GetProfilerHitchCount() > 0 then
    PrintScreen("Hitches: " .. GetProfilerHitchCount(), 1.0)
end
```

| Function | Signature | Description |
|----------|-----------|-------------|
| `SetProfilerHitchDetection` | `SetProfilerHitchDetection(enabled)` | Turns automatic hitch capture on or off. |
| `SetProfilerHitchThreshold` | `SetProfilerHitchThreshold(ms)` | Absolute millisecond trigger. A frame must also exceed twice the running average to count. |
| `GetProfilerHitchCount` | `GetProfilerHitchCount()` → `number` | How many hitches are currently stored (up to 32, oldest dropped first). |
| `ClearProfilerHitches` | `ClearProfilerHitches()` | Empties the hitch list. |

#### Lua API — Profiler Overlay Toggle

Controls the on-screen profiler overlay (FPS, frame time, CPU/RAM/VRAM/GPU, render passes, hot scopes).
Bind any key you like via `IsKeyJustPressed` (or a gamepad button, a console command, a UI widget) and call `ToggleDebugProfiler()`.
These functions work in **every build configuration** (Debug and Release) and on every platform, so you can expose the overlay through your own in-game menus or developer consoles.

```lua
local visible = GetDebugProfilerVisible()

ToggleDebugProfiler()
```

| Function | Signature | Description |
|----------|-----------|-------------|
| `GetDebugProfilerVisible` | `GetDebugProfilerVisible()` → `bool` | Returns `true` while the runtime profiler overlay is visible. |
| `ToggleDebugProfiler` | `ToggleDebugProfiler()` | Inverts the current overlay visibility. Bind to any hotkey via `IsKeyJustPressed`. |

> The overlay's visibility is a single engine-wide flag, so any script or user-bound hotkey calling `ToggleDebugProfiler` toggles it consistently.

---

## 30. Tilemap — Tile Maps

> **Type:** Entity-bound. Requires **TilemapRendererComponent**.
> Tilemaps are a grid of tiles for building levels, with selectable **orthogonal**, **isometric**, or **hexagonal** projection (chosen per map in the Tilemap editor — see **Projection** below). Every tile function respects the active projection automatically.
> An entity can have multiple tilemap instances. `index` = 0 is the first tilemap.

### Basic properties

```lua
-- Tilemap count
local count = GetTilemapCount()

-- Tilemap size → {width, height, tileSize, cellWidth, cellHeight, projection}
local size = GetTilemapSize()
local size = GetTilemapSize(1)  -- Second tilemap
-- size.width = width in tiles
-- size.height = height in tiles
-- size.tileSize = tile (art) size in pixels
-- size.cellWidth = footprint cell width in pixels
-- size.cellHeight = footprint cell height in pixels
-- size.projection = "orthogonal" | "isometric" | "hexagonal"

-- Layer count
local layers = GetTilemapLayerCount()
local layers = GetTilemapLayerCount(1)  -- Second tilemap

-- Visibility
SetTilemapVisible(true)
SetTilemapVisible(false, 1)  -- Second tilemap
local vis = IsTilemapVisible()

-- Flip
SetTilemapFlipX(true)
SetTilemapFlipY(false)
local fx = GetTilemapFlipX()
local fy = GetTilemapFlipY()

-- Render in game
SetTilemapRenderInGame(true)
local render = GetTilemapRenderInGame()
SetTilemapRenderInGame(true, 0)  -- optional trailing tilemap instance index (default 0)

-- Per-tile shadows. Each tile type owns its own shadow settings. Pass a tile
-- value (as returned by GetTileAt); the optional trailing argument is the
-- tilemap instance index (default 0).
local tile = GetTileAt(worldX, worldY)

-- Whether the tile casts a shadow
SetTileCastShadow(tile, true)
local casts = GetTileCastShadow(tile)              -- → bool

-- Shadow shape: 0 = Colliders (tile collider), 1 = Contour (texture silhouette)
SetTileCastShadowMode(tile, 1)
local mode = GetTileCastShadowMode(tile)           -- → int

-- Shadow origin: 0 = Bottom, 1 = Center, 2 = Top
SetTileShadowOrigin(tile, 0)
local origin = GetTileShadowOrigin(tile)           -- → int

-- Shadow edge fade [0..1]
SetTileShadowEdgeFade(tile, 0.25)
local fade = GetTileShadowEdgeFade(tile)           -- → float

-- Shadow Z-order: negative = toward background, positive = toward foreground, 0 = caster plane
SetTileShadowZOrder(tile, 1)
local zo = GetTileShadowZOrder(tile)               -- → float

-- Whether the tile lets light pass through instead of blocking it
SetTileDontBlockShadows(tile, true)
local dontBlock = GetTileDontBlockShadows(tile)    -- → bool

-- Tilemap file path
local path = GetTilemapFilePath()
```

### Position and transform

```lua
-- Position (global)
SetTilemapPosition(100, 200)
local pos = GetTilemapPosition()  -- → {x, y}

-- Local position (offset within entity)
SetTilemapLocalPosition(10, 5)
local lp = GetTilemapLocalPosition()        -- → {x, y}
SetTilemapLocalPosition(10, 5, 1)           -- For specific tilemap instance
local lp = GetTilemapLocalPosition(1)

-- Scale
SetTilemapLocalScale(2, 2)
local ls = GetTilemapLocalScale()  -- → {x, y}

-- Rotation
SetTilemapLocalRotation(45)
local lr = GetTilemapLocalRotation()

-- World transform (entity transform already applied — see the Sprite section).
-- Note: SetTilemapPosition/GetTilemapPosition are older aliases of the
-- Local position accessors and behave identically.
SetTilemapWorldPosition(120, 64, 0)
local twp = GetTilemapWorldPosition(0)      -- → {x, y, z}
SetTilemapWorldRotation(30, 0)
local twr = GetTilemapWorldRotation(0)      -- → number
local tws = GetTilemapWorldScale(0)         -- → {x, y}, read-only

-- Swap the tilemap asset. Call RebuildTilemapPhysics afterwards so the
-- collision chains match the new grid.
local ok = SetTilemapAsset("Content/Levels/Cave.ice_tilemap", 0)
RebuildTilemapPhysics(0)
local tmPath = GetTilemapFilePath(0)        -- → current .ice_tilemap path

-- Render order (Z-depth)
SetTilemapOrder(5)                          -- Set render order for entity
SetTilemapOrder(3, 1)                       -- Set render order for specific instance
local order = GetTilemapOrder()             -- Get render order
local order = GetTilemapOrder(1)            -- Get render order for specific instance

-- Tile size
SetTilemapTileSize(32)
```

### Working with tiles

```lua
-- Get tile ID by world coordinates
local tileId = GetTileAt(worldX, worldY)
local tileId = GetTileAt(worldX, worldY, 0, 1)  -- instance 0, layer 1

-- Get tile ID by grid coordinates
local tileId = GetTileGrid(tileX, tileY)
local tileId = GetTileGrid(tileX, tileY, 1, 0)  -- layer 1, instance 0

-- Set tile by grid coordinates (also updates physics colliders)
SetTileAt(tileX, tileY, tileId)
SetTileAt(tileX, tileY, tileId, 0, 1)  -- instance 0, layer 1

-- Set tile (grid only, no physics)
SetTileGrid(tileX, tileY, tileId)
SetTileGrid(tileX, tileY, tileId, 1, 0)  -- layer 1, instance 0

-- Check if a tile exists at point (any layer)
if IsTileSolid(worldX, worldY) then ... end
```

### Coordinate conversion

```lua
-- World coordinates → grid coordinates
local tile = WorldToTile(worldX, worldY)  -- → {x, y}

-- Grid coordinates → world (tile center)
local world = TileToWorld(tileX, tileY)  -- → {x, y}
```

### Projection (orthogonal / isometric / hexagonal)

> Projection is a **map-level** property set in the Tilemap editor. Tilesets stay regular square sheets.
> **Coordinate convention:** for **orthogonal** maps tile coordinates use the classic layout (`y = 0` at the bottom). For **isometric** and **hexagonal** maps tile coordinates are the **raw grid coordinates** `(x, y)` — `(0, 0)` is the top cell, `x` grows toward the lower-right, `y` toward the lower-left. All tile functions (`GetTileAt`, `SetTileAt`, `WorldToTile`, `TileToWorld`, `IsTileSolid`, …) respect the active projection automatically.

```lua
-- Active projection → "orthogonal" | "isometric" | "hexagonal"
local proj = GetTilemapProjection()
local proj = GetTilemapProjection(1)        -- Second tilemap

-- Footprint cell size + projection are also reported by GetTilemapSize:
local size = GetTilemapSize()
-- size.cellWidth, size.cellHeight, size.projection

-- Projection-correct neighbours of a cell → array of {x, y}
-- 8 for orthogonal, 4 for isometric (diamond), 6 for hexagonal.
-- Only in-bounds cells are returned.
local neighbours = GetTileNeighbors(tileX, tileY)
local neighbours = GetTileNeighbors(tileX, tileY, 1)  -- Second tilemap
for _, n in ipairs(neighbours) do
    -- n.x, n.y
end

-- Grid distance between two cells (projection-correct):
-- Chebyshev (orthogonal), Manhattan (isometric), hex distance (hexagonal).
local d = TileDistance(ax, ay, bx, by)
local d = TileDistance(ax, ay, bx, by, 1)   -- Second tilemap

-- World-space vertices of a cell's footprint → array of {x, y}
-- 4 verts (orthogonal / isometric), 6 verts (hexagonal). Useful for highlighting a cell.
local poly = GetTileFootprintWorld(tileX, tileY)
for _, v in ipairs(poly) do
    -- v.x, v.y
end
```

> **Tip (grid games):** `GetTileNeighbors` + `TileDistance` give you ready-made adjacency and range math for turn-based / roguelike / tactics movement on any projection — no need to special-case hexagonal offsets yourself.

> **Y sorting and tilemaps.** Turn `SetSpriteYSort(true)` on for characters and props, and give the ground tilemap a
> fixed order well behind them. The sprites then interleave with each other correctly while the floor stays flat.

### Fill and clear

```lua
-- Fill a rectangle region
FillRect(startX, startY, width, height, tileId)
FillRect(startX, startY, width, height, tileId, 0, 0)  -- layer 0, instance 0

-- Fill entire layer with one tile
FillAll(tileId)
FillAll(tileId, 0, 0)  -- layer 0, instance 0

-- Clear layer (all tiles = -1)
ClearLayer()
ClearLayer(0, 0)  -- layer 0, instance 0

-- Resize tilemap
ResizeTilemap(64, 32)  -- width, height
```

### Layers

```lua
-- Add new layer → index of new layer
local layerIdx = AddTilemapLayer("Decoration")

-- Remove layer (cannot remove the last)
RemoveTilemapLayer(2)  -- layerIndex

-- Layer name
local name = GetLayerName(0)

-- Rename layer
SetLayerName(0, "Background")

-- Layer visibility
SetLayerVisible(0, true)
local vis = IsLayerVisible(0)

-- Layer locking
SetLayerLocked(0, true)
local locked = IsLayerLocked(0)

-- Swap layers
SwapLayerOrder(0, 1)

-- Copy layer → returns new index
local copyIdx = CopyLayer(0, "Background Copy")
```

### Tilesets (multi-tilesets)

```lua
-- Path of main tileset
local path = GetTilesetPath()

-- Tileset count
local count = GetTilesetCount()

-- Path of additional tileset
local path = GetAdditionalTilesetPath(0)

-- Encode/decode tiles for multi-tilesets
local encoded = EncodeTile(1, 5)        -- tilesetIndex=1, tileId=5
local rotated = EncodeTile(1, 5, 1)     -- + rotation step 1 (90° clockwise)
local rotated = EncodeTile(1, 5, 1, 0)  -- instance 0 (decides 4 or 6 steps)
local decoded = DecodeTile(encoded)     -- → {tilesetIndex, tileId, rotation}
local isEncoded = IsEncodedTile(value)  -- true/false
```

> A tilemap can reference up to **128 tilesets** (1 primary + 127 additional).

### Tileset paths and management (runtime)

```lua
-- Primary tileset
SetPrimaryTilesetPath("Content/Tilesets/Stone.ice_tileset")
local primary = GetPrimaryTilesetPath()

-- Additional tilesets (returned index is for use with EncodeTile/DecodeTile,
-- where tilesetIndex 0 = primary, 1+ = additional)
local idx = AddAdditionalTileset("Content/Tilesets/Wood.ice_tileset")  -- → new index (0-based among additional)
local count = GetAdditionalTilesetCount()
local p = GetAdditionalTilesetPath(0)
SetAdditionalTilesetPath(0, "Content/Tilesets/Sand.ice_tileset")
RemoveAdditionalTileset(0)

-- Per-layer tileset override (overrides primary tileset for tiles on this layer)
SetLayerTilesetPath(1, "Content/Tilesets/Foreground.ice_tileset")
local layerTs = GetLayerTilesetPath(1)

-- All functions accept an optional last instance index argument
SetPrimaryTilesetPath("...", 0)
```

### Tileset introspection

```lua
-- tilesetIdx: 0 = primary, 1+ = additional (matches EncodeTile/DecodeTile)
local tileSize  = GetTilesetTileSize(tilesetIdx)       -- e.g. 32 (px per tile)
local cols      = GetTilesetColumns(tilesetIdx)        -- texture width / tileSize
local rows      = GetTilesetRows(tilesetIdx)
local total     = GetTilesetTileCount(tilesetIdx)      -- cols * rows
local texPath   = GetTilesetTexturePath(tilesetIdx)

-- Per-tile metadata from the tileset asset (TileData)
local meta = GetTileMeta(tilesetIdx, tileId)
-- meta = {
--   hasCollider, isSensor, isOneWay,
--   enableContactEvents, enableSensorEvents, enableHitEvents,
--   enablePreSolveEvents,
--   density, friction, restitution,
--   colliderMode,           -- 0 = Polygon, 1 = Chain
--   collisionGroupIndex,
--   collisionEnabled,       -- 0 disabled, 1 sensor, 2 collide-no-events, 3 full
--   dataName,
-- }

-- All take an optional trailing instance index
local total = GetTilesetTileCount(0, 0)
```

### Bulk tile editing

```lua
-- All bulk ops use Lua-Y coordinates (y=0 is the top row), respect layers
-- and rebuild chunk emptiness when chunking is enabled.

-- 2D table → whole layer. tbl[1..H][1..W], row 1 = bottom (y=0), row H = top, column 1 = left.
-- Cells that aren't a number are skipped (kept as-is).
local map = NoiseMap(GetTilemapSize().width, GetTilemapSize().height, 30.0)
local grid = {}
for y = 1, #map do
    grid[y] = {}
    for x = 1, #map[y] do
        grid[y][x] = (map[y][x] > 0.55) and 1 or -1
    end
end
SetTilesBulk(grid)              -- layer 0
SetTilesBulk(grid, 1)           -- layer 1
SetTilesBulk(grid, 0, 0)        -- layer 0, instance 0

-- Flat array → rectangular region. Array length = w*h, row-major (row 1 first).
BlitFromArray(x, y, w, h, { 1, -1, 1, -1, 2, 2, 2, 2 }, 0)

-- Copy a rectangle from one layer to another (or same layer)
CopyRect(srcLayer, srcX, srcY, w, h, dstLayer, dstX, dstY)
CopyRect(0, 5, 5, 8, 8, 1, 20, 20, 0)  -- with instance index
```

### Region helpers (procedural editing)

```lua
-- Flood fill (4-connected). Replaces the connected region of tiles with the
-- same id starting at (x, y) with newId. Returns the number of cells filled.
local n = FloodFill(startX, startY, newId)
local n = FloodFill(startX, startY, newId, layerIdx)
local n = FloodFill(startX, startY, newId, layerIdx, instanceIdx)

-- Stamp a 2D pattern at (x, y). pattern[1][1] is placed at (x, y); pattern row 1 = bottom of stamp
-- in world (since Y+ is up), pattern row H = top.
-- skipNegative (default true): cells with tileId < 0 in the pattern are kept transparent
-- and don't overwrite the destination — perfect for rooms/decals with empty space.
local room = {
    { 1, 1, 1, 1, 1 },
    { 1,-1,-1,-1, 1 },
    { 1,-1, 5,-1, 1 },
    { 1,-1,-1,-1, 1 },
    { 1, 1, 1, 1, 1 },
}
StampPattern(10, 10, room)
StampPattern(10, 10, room, 0, 0, false)  -- write -1 too (clear cells)

-- Rotate a rectangular region in place
-- direction:  1 = 90° clockwise, -1 = 90° counter-clockwise, 2 = 180°
-- 90° rotations require w == h.
RotateRect(x, y, w, h, 1)
RotateRect(x, y, 8, 8, -1, 0, 0)

-- The optional 8th argument also spins each individual tile so the region rotates as a
-- whole (default false — cells are only moved, tile orientation is left untouched).
-- Ignored on hexagonal maps, where 90° steps do not exist.
RotateRect(x, y, 8, 8, 1, 0, 0, true)

-- Mirror a rectangular region in place. Tile rotation is left untouched (there is no
-- mirrored tile state — only rotation).
FlipRect(x, y, w, h, "x")  -- horizontal flip (left ↔ right)
FlipRect(x, y, w, h, "y")  -- vertical flip (top ↔ bottom)
```

### Auto-tile / bitmask helpers

```lua
-- 4-neighbor mask. Bits: N=1, E=2, S=4, W=8.
-- Coordinate convention: tile (0,0) is at the bottom-left, last tile at top-right.
-- N (north) = +Y (up), S = -Y (down), E = +X (right), W = -X (left).
-- solidId: -1 (or omit) → any non-empty tile counts as "solid"
--          >=0          → only cells with that exact tileId count as "solid"
local m = GetNeighborMask4(x, y)             -- "any non-empty"
local m = GetNeighborMask4(x, y, 1)          -- "only id 1 is solid"
local m = GetNeighborMask4(x, y, -1, 0, 0)   -- layer 0, instance 0

-- 8-neighbor mask. Bits:
--   N=1, NE=2, E=4, SE=8, S=16, SW=32, W=64, NW=128
local m = GetNeighborMask8(x, y, -1)

-- Example: pick a sprite from a 16-tile auto-tile sheet
local autoTile = { [0]=0, [1]=1, [2]=2, ..., [15]=15 }
local mask = GetNeighborMask4(x, y, -1)
SetTileAt(x, y, autoTile[mask])
```

### Iteration and search

> An error inside an `IterateLayer` / `IterateNonEmpty` callback stops the iteration and is written
> to the log; the rest of the script keeps running.

```lua
-- Iterate every cell (including empty) — coordinates are in Lua-Y.
-- tileId is the raw value (rotation bits included); rotation is passed separately.
IterateLayer(function(x, y, tileId, rotation)
    if StripTileRotation(tileId) == 5 then SetTileAt(x, y, 7) end
end)
IterateLayer(callback, layerIdx, instanceIdx)

-- Iterate only non-empty cells (chunking-aware: skips empty chunks → very fast on sparse maps)
IterateNonEmpty(function(x, y, tileId, rotation)
    print(x, y, tileId, rotation)
end, layerIdx, instanceIdx)

-- Count cells with a specific tile id. layerIdx omitted/-1 → count across all layers.
local n = CountTiles(1)
local n = CountTiles(1, 0)        -- layer 0 only
local n = CountTiles(1, 0, 0)     -- layer 0, instance 0

-- Find every cell with the given tile id (returns array of {x, y, rotation} in Lua-Y)
local cells = FindTile(1)
for _, c in ipairs(cells) do print(c.x, c.y, c.rotation) end
```

### Tile rotation

Every placed tile carries a **rotation step** stored inside its grid value, so one tile
graphic can be used for all four corners (or all six hex orientations) instead of drawing
a separate tile per direction. The rotation applies to *everything* the tile owns: the
sprite (or flipbook frame), the physics collider, the 2D shadow caster, the nav-grid
footprint and the destruction fragments all rotate together.

* **Orthogonal / isometric** maps use **4 steps** of 90°.
* **Hexagonal** maps use **6 steps** of 60°.
* Step `0` is the unrotated tile; positive steps rotate **clockwise**, following the
  engine convention (`X+` right, `Y+` up, rotation clockwise-positive) — a rotation step
  of `1` on an orthogonal map is exactly `Transform.Rotation += 90` for that one tile.
* Colliders are rotated inside the tile's own unit square (the same square the Tileset
  collider editor shows), so a collider drawn over the art stays glued to the art.
  A tile that uses the plain full-tile collider (or the cell footprint on isometric /
  hexagonal maps) is unaffected by rotation, because those shapes are rotation-symmetric.

In the Tilemap editor: **Q** rotates the brush counter-clockwise, **E** clockwise, and
**Ctrl+Q / Ctrl+E** rotate the tile already under the cursor.

```lua
-- Steps available for this map: 4 (orthogonal/isometric) or 6 (hexagonal)
local steps = GetTileRotationStepCount()
local steps = GetTileRotationStepCount(0)          -- instance 0

-- Convert a step count into degrees for the active projection
local deg = GetTileRotationDegrees(1)              -- 90 on ortho, 60 on hex

-- Rotation of the tile at grid coordinates
local rot = GetTileRotation(tileX, tileY)          -- 0 .. steps-1
local rot = GetTileRotation(tileX, tileY, 0, 0)    -- layer 0, instance 0

-- Set an absolute rotation (rebuilds that tile's collider)
SetTileRotation(tileX, tileY, 2)
SetTileRotation(tileX, tileY, 2, 0, 0)             -- layer 0, instance 0

-- Rotate by a delta, returns the resulting step (rebuilds that tile's collider)
local newRot = RotateTile(tileX, tileY, 1)         -- one step clockwise (E)
local newRot = RotateTile(tileX, tileY, -1)        -- one step counter-clockwise (Q)

-- Rotate every non-empty tile in a rectangle, returns how many changed
local n = RotateRegionTiles(x, y, w, h, 1)         -- one step clockwise
local n = RotateRegionTiles(x, y, w, h, -1, 0, 0)  -- CCW, layer 0, instance 0

-- Pure helpers on raw tile values (no map access)
local r  = GetTileValueRotation(value)             -- rotation stored in a tile value
local id = StripTileRotation(value)                -- value without the rotation bits
local v  = SetTileValueRotation(value, 3)          -- absolute rotation on a value
local v  = RotateTileValue(value, 1)               -- relative rotation on a value
```

> **Round-trips are safe:** `GetTileAt` / `GetTileGrid` return the full tile value including
> its rotation, so feeding that value straight back into `SetTileAt`, `SetTileGrid`,
> `StampPattern`, `CopyRect`, `BlitFromArray` or the clipboard preserves the rotation.
> When you compare tile values yourself, use `StripTileRotation(value)` to compare tile
> identity regardless of orientation.
>
> `CountTiles`, `FindTile`, `FloodFill` and `GetNeighborMask4/8` already match on tile
> identity (rotation is ignored) — pass a value that *has* rotation bits set to
> `CountTiles` / `FindTile` when you want an orientation-exact match.

### Tile span (multi-cell tiles)

A placed tile can cover **more than one cell**. The block grows **right (`+X`)** and
**up (`+Y`)** from its *anchor* — the cell that actually holds the tile value — so the
anchor never moves when the tile is resized. Everything the tile owns scales with the
block: the sprite (or flipbook frame) is stretched over it, custom collider polygons and
chains are stretched with it, default box colliders cover the whole footprint (and still
merge with their neighbours), and the 2D shadow caster, the nav-grid footprint, particle
collision and the destruction fragments all follow.

* Spans are **orthogonal-only**. On isometric and hexagonal maps every setter returns
  `false` / `1 x 1` and `SupportsTileSpans()` is `false`.
* The maximum span is `GetMaxTileSpan()` (`64`) per axis, additionally clamped by the map
  bounds — a block can never stick out of the map.
* The cells a block covers are **empty in the grid**, but every per-cell call accepts **any**
  cell of a block and acts on the whole block: `GetTileAt`, `GetTileGrid`, `GetTileDataName`,
  `GetTileDataNameGrid`, `IsTileSolid`, `IsTileDestructible`, `SetTileDestructible`,
  `Get/SetTileFragmentSettings`, `GetTileRotation`, `SetTileRotation`, `RotateTile`,
  `GetNeighborMask4/8`, the tile-collider helpers, and the span calls below. Writing a tile
  into a covered cell breaks the block first, so the grid can never end up with two tiles in
  the same place.
* `CountTiles`, `FindTile`, `IterateLayer` and `IterateNonEmpty` walk raw grid cells, so a
  multi-cell tile shows up **once**, at its anchor.
* A `1 x 1` tile is the minimum — shrinking it does nothing.
* Growing a block over occupied cells **erases** whatever is there.
* In the Tilemap editor: **`Shift+D` / `Shift+A`** grow / shrink the tile under the cursor
  to the right, **`Shift+W` / `Shift+S`** grow / shrink it upwards, and **`Shift+Wheel`**
  does both axes at once.

```lua
-- Is this map able to use multi-cell tiles at all?
local ok = SupportsTileSpans()
local ok = SupportsTileSpans(0)                    -- instance 0
local maxSpan = GetMaxTileSpan()                   -- 64

-- Span of the block that owns this cell (any cell of the block works)
local s = GetTileSpan(tileX, tileY)                -- { w = 2, h = 1 }
local s = GetTileSpan(tileX, tileY, 0, 0)          -- layer 0, instance 0

-- Anchor (bottom-left cell, in Lua-Y) of the block that owns this cell
local o = GetTileSpanOrigin(tileX, tileY)          -- { x = 5, y = 3 }

-- Is this cell covered by a neighbouring block rather than owning a tile itself?
local covered = IsTileSpanCovered(tileX, tileY)

-- Absolute resize (rebuilds that block's collider). Works from any cell of the block.
local ok = SetTileSpan(tileX, tileY, 2, 1)
local ok = SetTileSpan(tileX, tileY, 2, 1, 0, 0)   -- layer 0, instance 0

-- Relative resize, returns the resulting span (this is what Shift+D / Shift+W do)
local s = ExpandTileSpan(tileX, tileY, 1, 0)       -- one cell wider
local s = ExpandTileSpan(tileX, tileY, -1, -1)     -- one cell narrower and shorter
print(s.w, s.h)

-- Place a tile and its span in one call (clears whatever the footprint covers)
SetTileSpanAt(tileX, tileY, tileId, 2, 2)
SetTileSpanAt(tileX, tileY, tileId, 2, 2, 0, 0)    -- layer 0, instance 0

-- Erase the whole block that owns this cell (all of its cells + its collider)
EraseTileBlock(tileX, tileY)

-- Every multi-cell tile on a layer (omit layerIndex for all layers)
for _, b in ipairs(GetTileSpanList()) do
    print(b.x, b.y, b.w, b.h, b.layer, b.tileId)
end
local n = CountSpannedTiles()                      -- how many blocks are larger than 1x1
local n = CountSpannedTiles(0)                     -- layer 0 only

-- Flatten every block on a layer back to 1 x 1, returns how many were reset
local reset = ClearAllTileSpans(0)
```

> **Rotation and span combine.** A rotation step still rotates the quad around the block's
> **centre**, so square blocks (`2 x 2`, `3 x 3`, …) rotate exactly like single tiles —
> sprite, collider and shadow all line up. A non-square block rotated by 90° or 270° keeps
> the cells it occupies and its sprite overhangs them, which is the same thing
> `Transform.Rotation` does to any non-square sprite; its collider is rotated inside the
> tile's unit square first and then stretched over the block, so it stays within the
> occupied cells.
>
> `SetTileAt`, `SetTileGrid`, `FillRect`, `FillAll`, `ClearLayer`, `FloodFill`,
> `StampPattern`, `BlitFromArray`, `CopyRect` and `SetTilesBulk` **remove** any block they
> overwrite — a block cannot survive being half-painted over — while `RotateRect` and
> `FlipRect` **flatten** the blocks inside the region to single tiles and then move them, so
> nothing is lost. Either way a bulk edit can never leave a half-erased block behind. Use
> `SetTileSpanAt` when you want the tiles you write to stay multi-cell.

### Tile colliders

```lua
-- Remove collider for a tile
DestroyTileCollider(tileX, tileY)

-- Create a tile collider manually
CreateTileCollider(tileX, tileY, false)

-- Check collider presence
local has = HasTileCollider(tileX, tileY)

-- Check if a tile collider is a sensor
local isSensor = IsTileColliderSensor(tileX, tileY)
local isSensor = IsTileColliderSensor(tileX, tileY, 0)  -- instance 0

-- Set tile collider sensor mode at runtime
SetTileColliderSensor(tileX, tileY, true)                -- Make sensor
SetTileColliderSensor(tileX, tileY, false)               -- Make solid
SetTileColliderSensor(tileX, tileY, true, 0)             -- instance 0

-- Sensors trigger OnSensorEnter/OnSensorExit but don't block movement
-- Works for both tileset tiles and animated (flipbook) tiles

-- Full tilemap physics rebuild (colliders)
RebuildTilemapPhysics()
```

### Tile collider events and properties

```lua
-- One-way platform
local oneWay = IsTileColliderOneWay(tileX, tileY)
SetTileColliderOneWay(tileX, tileY, true)
SetTileColliderOneWay(tileX, tileY, false, 0)  -- instance 0

-- One-way direction: 1 = Up (default), 2 = Down, 3 = Left, 4 = Right
-- Direction is the side from which the tile is solid (cannot pass through)
local dir = GetTileColliderOneWayDirection(tileX, tileY)
local dir = GetTileColliderOneWayDirection(tileX, tileY, 0)  -- instance 0
SetTileColliderOneWayDirection(tileX, tileY, 1)
SetTileColliderOneWayDirection(tileX, tileY, 2, 0)  -- instance 0

-- Contact events
local contacts = AreTileContactEventsEnabled(tileX, tileY)
SetTileContactEventsEnabled(tileX, tileY, true)

-- Sensor events
local sensors = AreTileSensorEventsEnabled(tileX, tileY)
SetTileSensorEventsEnabled(tileX, tileY, true)

-- Hit events
local hits = AreTileHitEventsEnabled(tileX, tileY)
SetTileHitEventsEnabled(tileX, tileY, true)

-- Pre-solve events (called before collision response is calculated)
local preSolve = AreTilePreSolveEventsEnabled(tileX, tileY)
SetTilePreSolveEventsEnabled(tileX, tileY, true)

-- Check if tile uses chain collider (edge-merged, read-only)
local isChain = IsTileChainCollider(tileX, tileY)

-- All functions accept an optional instance index as the last parameter
```

### Tile destructibility (used by ExplodeTiles)

```lua
-- Check whether the tile at (tileX, tileY) is marked as destructible
local destructible = IsTileDestructible(tileX, tileY)
local destructible = IsTileDestructible(tileX, tileY, 0)  -- instance 0

-- Set/clear the destructible flag on the tile at (tileX, tileY)
-- For regular tileset tiles the change is also persisted to the .tileset asset on disk.
-- For animated (flipbook) tiles the change is applied to the runtime tilemap only.
local changed = SetTileDestructible(tileX, tileY, true)
SetTileDestructible(tileX, tileY, false, 0)              -- instance 0

-- Query/modify destructibility by tile ID (works without coordinates)
local destructible = IsTileDestructibleById(tileId)
local destructible = IsTileDestructibleById(tileId, 0)   -- tileset index 0
SetTileDestructibleById(tileId, true)
SetTileDestructibleById(tileId, true, 1)                 -- additional tileset 1
```

### Per-tile fragment settings

Every destructible tile carries its own Fragment settings (the same set the Tileset / Tilemap
editors expose). `ExplodeTiles` uses these as the base for spawned debris. Read or modify them
at runtime as a table with the same keys accepted by the `ExplodeTiles` `opts` override
(`count`, `pattern`, `lifetime`, `fadeTime`, `gravityScale`, `density`, `friction`,
`restitution`, `isSensor`, `contactEvents`, `sensorEvents`, `hitEvents`, `preSolveEvents`,
`collisionGroup`, `castShadow`, `dontBlockShadows`, `shadowOrigin`, `shadowEdgeFade`,
`shadowZOrder`). `Set*` merges only the keys you provide.

```lua
-- Read the current fragment settings of the tile at (tileX, tileY); nil if no tile there
local frag = GetTileFragmentSettings(tileX, tileY)
local frag = GetTileFragmentSettings(tileX, tileY, 0)     -- instance 0

-- Merge new fragment settings into the tile at (tileX, tileY).
-- Regular tileset tiles are persisted to the .tileset asset; animated tiles apply at runtime.
local changed = SetTileFragmentSettings(tileX, tileY, { density = 2.0, count = 4, pattern = 2 })
SetTileFragmentSettings(tileX, tileY, { castShadow = true }, 0)

-- By tile ID (works without coordinates)
local frag = GetTileFragmentSettingsById(tileId)
local frag = GetTileFragmentSettingsById(tileId, 0)      -- tileset index 0
SetTileFragmentSettingsById(tileId, { lifetime = 5.0, restitution = 0.5 })
SetTileFragmentSettingsById(tileId, { gravityScale = 0.3 }, 1)  -- additional tileset 1
```

### Tile collider physics material

```lua
-- Get tile collider density
local density = GetTileColliderDensity(tileX, tileY)
local density = GetTileColliderDensity(tileX, tileY, 0)  -- instance 0

-- Set tile collider density at runtime
SetTileColliderDensity(tileX, tileY, 1.0)                -- Set density
SetTileColliderDensity(tileX, tileY, 1.0, 0)             -- instance 0

-- Get tile collider friction
local friction = GetTileColliderFriction(tileX, tileY)
local friction = GetTileColliderFriction(tileX, tileY, 0)  -- instance 0

-- Set tile collider friction at runtime
SetTileColliderFriction(tileX, tileY, 0.3)                -- Set friction
SetTileColliderFriction(tileX, tileY, 0.3, 0)             -- instance 0

-- Get tile collider restitution (bounciness)
local restitution = GetTileColliderRestitution(tileX, tileY)
local restitution = GetTileColliderRestitution(tileX, tileY, 0)  -- instance 0

-- Set tile collider restitution at runtime
SetTileColliderRestitution(tileX, tileY, 0.5)                -- Set restitution
SetTileColliderRestitution(tileX, tileY, 0.5, 0)             -- instance 0

-- Default values: density = 0.0, friction = 0.6, restitution = 0.0
-- Works for both tileset tiles and animated (flipbook) tiles
-- All functions accept an optional instance index as the last parameter
```

### Atomic tile collider properties (one call, multiple fields)

```lua
-- Update many tile collider properties in a single call. Avoids the cost of
-- multiple Set* roundtrips and the partial-state risk between them.
-- All fields are optional — only fields present in the table are applied.
SetTileColliderProperties(tileX, tileY, {
    density            = 1.5,
    friction           = 0.2,
    restitution        = 0.8,
    enableContactEvents = true,
    enableSensorEvents  = false,
    enableHitEvents     = true,
    enablePreSolveEvents = false,
})

-- With instance index
SetTileColliderProperties(tileX, tileY, { friction = 0.0 }, 0)
```

### Tile data name

```lua
-- Get tile data name by world coordinates (all layers or specific)
local name = GetTileDataName(worldX, worldY)
local name = GetTileDataName(worldX, worldY, 0)      -- instance 0
local name = GetTileDataName(worldX, worldY, 0, 1)    -- instance 0, layer 1

-- Get tile data name by grid coordinates
local name = GetTileDataNameGrid(tileX, tileY)
local name = GetTileDataNameGrid(tileX, tileY, 0)     -- instance 0
local name = GetTileDataNameGrid(tileX, tileY, 0, 1)  -- instance 0, layer 1

-- Works for regular tilesets and animated (flipbook) tiles
-- Returns "" if no data name is set
-- Example: surface-based gameplay
if GetTileDataName(px, py) == "lava" then
    DealDamage(10)
elseif GetTileDataName(px, py) == "dirt" then
    speed = speed * 0.7
end
```

### Animated tiles (Flipbook)

```lua
local count = GetAnimatedTileCount()
local info = GetAnimatedTileInfo(0)
-- info = { placeholderID, flipbookPath, speed, hasCollider, isSensor, density, friction, restitution, dataName }

local id = GetAnimatedTilePlaceholderID(0)

SetAnimatedTileSpeed(0, 1.5)
SetAnimatedTileData(0, "lava")
SetAnimatedTileCollider(0, true, false)

local created = AddAnimatedTile("Content/Flipbooks/Water.ice_flipbook")
-- created.index, created.placeholderID

RemoveAnimatedTile(0)
RemoveAnimatedTile(0, true)  -- Clear placeholderID from the grid
```

### Chunking (tilemap optimization)

```lua
SetTilemapChunking(true)
SetTilemapChunking(true, 32)  -- Enable and set chunk size
local enabled = IsTilemapChunkingEnabled()
local chunkSize = GetTilemapChunkSize()
```

---

## 31. Component — Component Checks and Management

> **Type:** Entity-bound + global. Lets you check component presence on any entity,
> add/remove components at runtime, and control properties of other entities.

### Check component presence on an entity

```lua
-- Check by entity ID
local hasSprite = HasSprite(entityId)
local hasRb = HasRigidbody(entityId)
local hasColl = HasCollider(entityId)
local hasAnim = HasAnimator(entityId)
local hasSkel = HasSkeleton(entityId)  -- entityId is optional; omit for the current entity
local hasFb = HasFlipbook(entityId)
local hasAud = HasAudio(entityId)
local hasFx = HasFX(entityId)
local hasLight = HasPointLight(entityId)
local hasTm = HasTilemap(entityId)
local hasWgt = HasWidget(entityId)
local hasCam = HasCamera(entityId)
local hasAI = HasAI(entityId)
local hasSL = HasSpotLight(entityId)
local hasJnt = HasJoint(entityId)
local hasPM = HasPointMarker(entityId)
local hasDestr = HasDestructible(entityId)
local hasStencil = HasStencil(entityId)
local hasHier = HasHierarchy(entityId)
local hasIface = HasInterfaceComponent(entityId)
local hasGT = HasGameplayTagComponent(entityId)

-- Check by component name (current entity)
local has = HasComponent("Sprite")
local has = HasComponent("Rigidbody")
local has = HasComponent("Collider")
local has = HasComponent("Animator")
local has = HasComponent("Skeleton")
-- etc.: "Flipbook", "Audio", "FX", "PointLight", "Widget",
-- "Camera", "Tilemap", "SpotLight", "Joint", "PointMarker", "AI", "Destructible", "ClassComponent", "Hierarchy", "Interface", "GameplayTag"

-- Check by component name on another entity
local has = EntityHasComponent(entityId, "Sprite")
```

### Add and remove components

```lua
-- Add component to current entity
AddComponent("Sprite")         -- SpriteRendererComponent
AddComponent("Rigidbody")      -- RigidbodyComponent
AddComponent("Collider")       -- ColliderComponent
AddComponent("BoxCollider")    -- Add box collider (creates ColliderComponent if missing)
AddComponent("CircleCollider") -- Add circle collider
AddComponent("SphereCollider") -- Alias for CircleCollider
AddComponent("CapsuleCollider") -- Add capsule collider
AddComponent("Animator")
AddComponent("Skeleton")
AddComponent("Flipbook")
AddComponent("Audio")
AddComponent("FX")
AddComponent("PointLight")
AddComponent("SpotLight")
AddComponent("Widget")
AddComponent("Camera")
AddComponent("Joint")
AddComponent("PointMarker")
AddComponent("Tilemap")
AddComponent("AI")
AddComponent("Destructible")
AddComponent("ClassComponent")
AddComponent("Stencil")

-- Remove component
RemoveComponent("Sprite")
RemoveComponent("Rigidbody")
RemoveComponent("ClassComponent")
-- etc. for all optional types

-- Core components exist on EVERY entity and cannot be removed: Transform, Tag, ID,
-- Stencil and Replication. RemoveComponent returns false and logs a warning for them.
-- Disable them through their own flags instead (Stencil.Enabled, Replication.Replicate).
-- AddComponent for a core component is a harmless no-op that returns false.

-- Add/remove component on another entity
AddEntityComponent(entityId, "Sprite")
AddEntityComponent(entityId, "Rigidbody")
AddEntityComponent(entityId, "Collider")
AddEntityComponent(entityId, "Animator")
AddEntityComponent(entityId, "Skeleton")
AddEntityComponent(entityId, "Flipbook")
AddEntityComponent(entityId, "Audio")
AddEntityComponent(entityId, "FX")
AddEntityComponent(entityId, "PointLight")
AddEntityComponent(entityId, "SpotLight")
AddEntityComponent(entityId, "Widget")
AddEntityComponent(entityId, "Camera")
AddEntityComponent(entityId, "Tilemap")
AddEntityComponent(entityId, "AI")
AddEntityComponent(entityId, "Destructible")
AddEntityComponent(entityId, "ClassComponent")
AddEntityComponent(entityId, "Joint")
AddEntityComponent(entityId, "PointMarker")
AddEntityComponent(entityId, "Stencil")

RemoveEntityComponent(entityId, "Sprite")
RemoveEntityComponent(entityId, "Rigidbody")
RemoveEntityComponent(entityId, "Collider")
RemoveEntityComponent(entityId, "Animator")
RemoveEntityComponent(entityId, "Skeleton")
RemoveEntityComponent(entityId, "Flipbook")
RemoveEntityComponent(entityId, "Audio")
RemoveEntityComponent(entityId, "FX")
RemoveEntityComponent(entityId, "PointLight")
RemoveEntityComponent(entityId, "SpotLight")
RemoveEntityComponent(entityId, "Widget")
RemoveEntityComponent(entityId, "Camera")
RemoveEntityComponent(entityId, "Tilemap")
RemoveEntityComponent(entityId, "AI")
RemoveEntityComponent(entityId, "Destructible")
RemoveEntityComponent(entityId, "ClassComponent")
RemoveEntityComponent(entityId, "Joint")
RemoveEntityComponent(entityId, "PointMarker")
RemoveEntityComponent(entityId, "Stencil")
```

### ClassComponent (class components)

#### By entity ID

```lua
-- Check ClassComponent presence
local has = EntityHasComponent(entityId, "ClassComponent")

-- Class component count
local count = GetClassComponentCount(entityId)

-- Name and path by index
local path = GetClassComponentPath(entityId, 0)
local name = GetClassComponentName(entityId, 0)

-- Search by name
local exists = HasClassComponentByName(entityId, "MyComponent")
local index = FindClassComponentIndex(entityId, "MyComponent")

-- Add/remove a class component instance on another entity
-- AddEntityClassComponentInstance(entityId, name [, classPath]) → instance index, or -1 on failure
-- If the target entity has no ClassComponentComponent yet, it will be created automatically.
local idx = AddEntityClassComponentInstance(entityId, "Weapon")
local idx = AddEntityClassComponentInstance(entityId, "Shield", "Content/Classes/Shield.ice_class")

-- RemoveEntityClassComponentInstance(entityId, index) → bool (true on success)
local ok = RemoveEntityClassComponentInstance(entityId, idx)

-- Change class path / display name of an existing instance
SetEntityClassComponentInstancePath(entityId, 0, "Content/Classes/NewWeapon.ice_class")
SetEntityClassComponentInstanceName(entityId, 0, "MainWeapon")

-- Runtime instantiation: add AND build the referenced class live.
-- AddEntityClassComponentInstance only stores metadata (the class's components are
-- merged when the level loads / the entity is spawned). Instantiate merges the class's
-- sprites / flipbooks / colliders / lights / markers / etc. onto the entity immediately
-- and creates collider shapes on its runtime physics body.
-- InstantiateEntityClassComponent(entityId, name [, classPath]) → instance index, or -1
local liveIdx = InstantiateEntityClassComponent(entityId, "Shield", "Content/Classes/Shield.ice_class")

-- Build the components of an already-added (metadata-only) instance at runtime.
-- ResolveEntityClassComponentInstance(entityId, index) → bool
ResolveEntityClassComponentInstance(entityId, idx)
```

#### Local transform of a class component on another entity

```lua
-- Position (relative to the entity transform)
local pos = GetEntityClassComponentLocalPosition(entityId, 0)       -- → {x, y, z}
SetEntityClassComponentLocalPosition(entityId, 0, 10, -5)           -- (entityId, index, x, y)
SetEntityClassComponentLocalPosition(entityId, 0, 10, -5, 0.5)      -- (entityId, index, x, y, z)

-- Rotation (degrees)
local rot = GetEntityClassComponentLocalRotation(entityId, 0)       -- → float
SetEntityClassComponentLocalRotation(entityId, 0, 45)

-- Scale
local scale = GetEntityClassComponentLocalScale(entityId, 0)        -- → {x, y}
SetEntityClassComponentLocalScale(entityId, 0, 2, 2)
```

#### World transform of a class component on another entity

World transform = entity transform + instance local transform
(accounts for the entity's rotation and scale via `CombineTransforms`).
These getters are read-only — to move a class component, change its **local** transform.

```lua
local wp = GetEntityClassComponentWorldPosition(entityId, 0)        -- → {x, y, z}
local wr = GetEntityClassComponentWorldRotation(entityId, 0)        -- → float (degrees)
local ws = GetEntityClassComponentWorldScale(entityId, 0)           -- → {x, y}
```

#### For current entity (self)

```lua
-- Count, name, path
local count = GetMyClassComponentCount()
local path = GetMyClassComponentPath(0)
local name = GetMyClassComponentName(0)

-- Search by name
local exists = HasMyClassComponentByName("Weapon")
local index = FindMyClassComponentIndex("Weapon")

-- Add/remove class component instance
local idx = AddClassComponentInstance("Weapon")                               -- Just name
local idx = AddClassComponentInstance("Shield", "Content/Classes/Shield.ice_class") -- With path
RemoveClassComponentInstance(idx)

-- Set class path for an instance
SetClassComponentInstancePath(0, "Content/Classes/NewWeapon.ice_class")

-- Runtime instantiation (self): add AND build the referenced class live (see notes above).
-- InstantiateClassComponent(name [, classPath]) → instance index, or -1
local liveIdx = InstantiateClassComponent("Shield", "Content/Classes/Shield.ice_class")
-- Build an already-added (metadata-only) instance: ResolveClassComponentInstance(index) → bool
ResolveClassComponentInstance(liveIdx)
```

#### Class component local transform

```lua
-- Position (relative to entity)
local pos = GetClassComponentLocalPosition(0)   -- → {x, y, z}
SetClassComponentLocalPosition(0, 10, -5)       -- (index, x, y)
SetClassComponentLocalPosition(0, 10, -5, 0.5)  -- (index, x, y, z)

-- Rotation
local rot = GetClassComponentLocalRotation(0)   -- → float (degrees)
SetClassComponentLocalRotation(0, 45)

-- Scale
local scale = GetClassComponentLocalScale(0)    -- → {x, y}
SetClassComponentLocalScale(0, 2, 2)
```

#### Class component world transform

World transform = entity transform + component local transform
(accounts for entity rotation and scale via `CombineTransforms`).

```lua
-- World position
local wp = GetClassComponentWorldPosition(0)    -- → {x, y, z}

-- World rotation
local wr = GetClassComponentWorldRotation(0)    -- → float

-- World scale
local ws = GetClassComponentWorldScale(0)       -- → {x, y}
```

**Example: Class component as a weapon:**

```lua
function OnCreate()
    -- Find "Sword" component and offset it to the right
    local idx = FindMyClassComponentIndex("Sword")
    if idx >= 0 then
        SetClassComponentLocalPosition(idx, 30, 0)
        SetClassComponentLocalRotation(idx, -15)
    end
end

function OnUpdate(dt)
    -- Get world position of the sword tip
    local idx = FindMyClassComponentIndex("Sword")
    if idx >= 0 then
        local tip = GetClassComponentWorldPosition(idx)
        -- tip.x, tip.y — absolute position accounting for entity rotation and scale
    end
end
```

### Control properties of other entities

```lua
-- Transform
SetEntityRotation(entityId, 45)
local rot = GetEntityRotation(entityId)
SetEntityScale(entityId, 2, 2)
local scale = GetEntityScale(entityId)  -- → {x, y}

-- Incremental operations on other entities
TranslateEntity(entityId, 10, 5)       -- Shift position by (dx, dy)
RotateEntity(entityId, 15)             -- Add to rotation (degrees)
ScaleEntity(entityId, 0.5, 0.5)        -- Add to scale (dx, dy)

-- Sprite
SetEntitySpriteFlipX(entityId, true)
SetEntitySpriteFlipY(entityId, false)
SetEntitySpriteColor(entityId, 1, 0, 0, 1)  -- r, g, b, a
SetEntitySpriteVisible(entityId, true)
SetEntitySpriteAlpha(entityId, 0.5)
SetEntitySpriteOrder(entityId, 5)
local order = GetEntitySpriteOrder(entityId)

-- Physics
SetEntityGravityScale(entityId, 0)
FreezeEntity(entityId)        -- Make static
UnfreezeEntity(entityId)      -- Make dynamic
StopEntityMovement(entityId)  -- Zero velocity

-- Animator
SetEntityAnimBool(entityId, "isRunning", true)
SetEntityAnimInt(entityId, "direction", 2)
SetEntityAnimFloat(entityId, "speed", 150)
SetEntityAnimTrigger(entityId, "attack")

-- Animator target sprite
SetAnimTargetSprite("Body")         -- Set target sprite by name
local target = GetAnimTargetSprite() -- Get current target sprite name

-- Light (Point lights inside an entity's LightComponent; pass an optional `index` to address a specific instance — defaults to 0 = the first one)
SetEntityLightEnabled(entityId, true)
SetEntityLightEnabled(entityId, true, 1)         -- target the second point light on this entity
SetEntityLightColor(entityId, 1, 0.8, 0.3)
SetEntityLightIntensity(entityId, 2.0)
SetEntityLightRadius(entityId, 200)
SetEntityLightFalloff(entityId, 2.0)              -- 1=linear, 2=quadratic (realistic), higher=sharper
SetEntityLightCastShadows(entityId, true)         -- per-light shadow toggle (still requires global shadows enabled)
SetEntityLightLocalPosition(entityId, 0, 0)       -- local offset within the LightComponent

local n        = GetEntityLightCount(entityId)               -- number of point lights on this entity
local enabled  = GetEntityLightEnabled(entityId)             -- bool
local cast     = GetEntityLightCastShadows(entityId)         -- bool
local i        = GetEntityLightIntensity(entityId)
local r        = GetEntityLightRadius(entityId)
local f        = GetEntityLightFalloff(entityId)
local c        = GetEntityLightColor(entityId)               -- → {r, g, b}

-- Spot Light (per-entity spotlight instances inside the same LightComponent — separate index space from PointLights)
local sn = GetEntitySpotLightCount(entityId)
SetEntitySpotLightEnabled(entityId, true)
SetEntitySpotLightColor(entityId, 1, 0.95, 0.85)
SetEntitySpotLightIntensity(entityId, 1.5)
SetEntitySpotLightRadius(entityId, 400)
SetEntitySpotLightDirection(entityId, 0, -1)               -- auto-normalized; (0,-1)=down (Y-up world)
SetEntitySpotLightAngles(entityId, 15, 30)                 -- innerDeg, outerDeg (smooth cone)
SetEntitySpotLightFalloff(entityId, 2.0)
SetEntitySpotLightCastShadows(entityId, true)
SetEntitySpotLightLocalPosition(entityId, 0, 0)

local se   = GetEntitySpotLightEnabled(entityId)
local scs  = GetEntitySpotLightCastShadows(entityId)
local si   = GetEntitySpotLightIntensity(entityId)
local sr   = GetEntitySpotLightRadius(entityId)
local sf   = GetEntitySpotLightFalloff(entityId)
local scol = GetEntitySpotLightColor(entityId)             -- → {r, g, b}
local sdir = GetEntitySpotLightDirection(entityId)         -- → {x, y}
local sang = GetEntitySpotLightAngles(entityId)            -- → {inner, outer}

-- All `*Spot*` and second-light variants accept an optional last `index` argument (defaults to 0).
-- These functions configure component instances; for the project-wide sun and shadow settings
-- see section 13.3 (`SetDirectionalLight`, `SetShadow*`, `SetAmbientLight`, …).

-- FX
PlayEntityFX(entityId)
StopEntityFX(entityId)

-- Flipbook
SetEntityFlipbookPlaying(entityId, true)
SetEntityFlipbookSpeed(entityId, 2.0)

-- Widget
SetEntityWidgetVisible(entityId, true)
SetEntityWidgetScale(entityId, 1.0)
SetEntityWidgetInteractable(entityId, true)
SetEntityWidgetText(entityId, "Label", "Hello", 0)

-- Stencil Mask (controls per-entity stencil masking)
SetStencilEnabled(entityId, true)
local enabled = GetStencilEnabled(entityId)
SetStencilMode(entityId, 1)        -- 0=Off, 1=Write (mask writer), 2=Read (mask reader)
local mode = GetStencilMode(entityId)
SetStencilID(entityId, 1)          -- Stencil ID (1–255), used to match writers and readers
local id = GetStencilID(entityId)
SetStencilCompareFunc(entityId, 0) -- 0=Equal (draw inside mask), 1=NotEqual (draw outside mask)
local cmp = GetStencilCompareFunc(entityId)

-- Self variants (current entity)
SetMyStencilEnabled(true)
local enabled = GetMyStencilEnabled()
SetMyStencilMode(1)
local mode = GetMyStencilMode()
SetMyStencilID(1)
local id = GetMyStencilID()
SetMyStencilCompareFunc(0)
local cmp = GetMyStencilCompareFunc()
```

### Tag-based mass operations

```lua
-- Set velocity for ALL entities with a tag
SetTagVelocity("Enemy", 0, 0)

-- Set FlipX/FlipY for all sprites with a tag
SetTagFlipX("Enemy", true)
SetTagFlipY("Enemy", true)

-- Set animator parameter for all with a tag
SetTagAnimBool("Enemy", "isDead", true)

-- Destroy ALL entities with a tag
DestroyAllByTag("Bullet")

-- Count entities with a tag
local count = CountEntitiesByTag("Enemy")
```

---

## 32. Network — Multiplayer (Network)

> **Type:** Global functions. `Network` table.
>
> Built-in multiplayer networking: client-server architecture,
> rooms, RPC, entity sync, voice chat, variables, interpolation.

### Start server and connect

```lua
-- Start server
Network.StartServer(7777, 16)              -- port, max players
Network.StartServer(7777, 16, "password")  -- with password
Network.StartServer(7777, 16, "password", 8080) -- 4th arg: WebSocket port so browser (web) clients can join; 0/omitted = native (ENet) only

-- Stop server
Network.StopServer()

-- Connect to server
Network.Connect("127.0.0.1", 7777)
Network.Connect("127.0.0.1", 7777, "password")

-- Disconnect
Network.Disconnect()

-- State checks
local connected = Network.IsConnected()
local server = Network.IsServer()
local connecting = Network.IsConnecting()
local host = Network.IsHost()
```

### Player information

```lua
-- Local player
local myId = Network.GetLocalPlayerID()
local myName = Network.GetLocalPlayerName()
Network.SetPlayerName("Hero")

-- All players
local count = Network.GetPlayerCount()
local ids = Network.GetAllPlayerIDs()  -- ID table
local name = Network.GetPlayerName(playerId)
local isHost = Network.IsPlayerHost(playerId)

-- Host
local hostId = Network.GetHostPlayerID()

-- Detailed player info (returns table with all fields)
local info = Network.GetPlayerInfo(playerId)
-- → {id, name, isHost, isLocal, ping, bytesSent, bytesReceived,
--    voiceMuted, textMuted, voiceEnabled, voiceVolume,
--    isTransmitting, connectDuration, chatChannels}

-- Check if player is local
local isLocal = Network.IsPlayerLocal(playerId)

-- Time since player connected (seconds)
local duration = Network.GetPlayerConnectDuration(playerId)
```

### Chat and messages

```lua
-- Send chat to all
Network.SendChatMessage("Hello everyone!")

-- Private message
Network.SendPrivateMessage(playerId, "Secret!")

-- Callback
Network.OnChatReceived(function(playerId, message)
    Print(Network.GetPlayerName(playerId) .. ": " .. message)
end)

-- Channel chat
Network.SendChannelMessage("global", "Hello channel!")
Network.JoinChannel("trade")
Network.LeaveChannel("trade")
local channels = Network.GetJoinedChannels()  -- table of channel names

-- Chat history
local history = Network.GetChatHistory()
-- → array of {senderID, targetID, channel, content, isPrivate, isSystem}

-- Clear chat history
Network.ClearChatHistory()

-- System message (server-only, broadcasts to all clients)
Network.SendSystemMessage("Server is restarting in 5 minutes!")

-- Chat history filtered by channel
local channelHistory = Network.GetChatHistoryForChannel("global")
-- → array of {senderID, targetID, channel, content, isPrivate, isSystem}

-- Chat rate limiting
Network.SetChatCooldown(1.5)               -- minimum delay between messages (seconds)
local cd = Network.GetChatCooldown()       -- current cooldown value
Network.SetChatBurstLimit(5)               -- max messages allowed in a burst
local burst = Network.GetChatBurstLimit()  -- current burst limit
Network.SetChatRateWindow(10.0)            -- time window for rate limiting (seconds)
local window = Network.GetChatRateWindow() -- current rate window
```

### Sending data

```lua
-- String data
Network.SendData(playerId, "hello", true)        -- reliable
Network.BroadcastData("gameStart", true)         -- to all
Network.SendDataToServer("myInput", true)        -- to server
Network.BroadcastDataExcept(playerId, "data")    -- to all except one

-- Short forms
Network.SendDataReliable(playerId, "data")
Network.SendDataUnreliable(playerId, "data")
Network.BroadcastDataReliable("data")
Network.BroadcastDataUnreliable("data")

-- Tables (auto JSON serialization)
Network.SendTable(playerId, { hp = 100, pos = { x = 10, y = 20 } })
Network.BroadcastTable({ event = "explosion", x = 100, y = 200 })
Network.SendTableToServer({ input = "jump" })
Network.BroadcastTableExcept(playerId, { msg = "update" })

-- JSON manually
local json = Network.JsonEncode({ key = "value" })
local tbl = Network.JsonDecode(json)
```

Client-side `BroadcastData`/`BroadcastTable`/`BroadcastDataExcept`/`BroadcastTableExcept`,
as well as targeted `SendData`/`SendTable`, are automatically relayed through the host:
receivers get the real sender `playerId` in `OnDataReceived`/`OnTableReceived`.

### Callbacks (events)

```lua
Network.OnConnected(function(msg) Print("Connected!") end)
Network.OnConnectionFailed(function(msg) Print("Error: " .. msg) end)
Network.OnDisconnected(function(msg) Print("Disconnected!") end)
Network.OnPlayerJoined(function(playerId, name) Print(name .. " joined") end)
Network.OnPlayerLeft(function(playerId, name) Print(name .. " left") end)
Network.OnDataReceived(function(playerId, data) Print("Data: " .. data) end)
Network.OnTableReceived(function(playerId, tbl) Print("HP: " .. tbl.hp) end)
Network.OnInputReceived(function(playerId, data) Print("Input from " .. playerId) end)
Network.OnPrivateChatReceived(function(playerId, message) Print("PM: " .. message) end)
Network.OnChannelChatReceived(function(playerId, channel, content) Print(channel .. ": " .. content) end)
Network.OnVoiceReceived(function(playerId, voiceData) ... end)
Network.OnPlayerVarChanged(function(playerId, key, value)
    Print("Player " .. playerId .. ": " .. key .. " = " .. value)
end)

-- Clear all callbacks
Network.ClearCallbacks()
```

### RPC (Remote Procedure Call)

```lua
-- Register RPC
Network.RegisterRPC("DealDamage", function(playerId, data)
    Print("Damage from " .. playerId .. ": " .. data)
end)

-- Call RPC
Network.CallRPC("DealDamage", "50")            -- Everyone
Network.CallRPCOnServer("DealDamage", "50")    -- Server
Network.CallRPCOnPlayer(playerId, "DealDamage", "50")  -- Specific player

-- RPC with tables
Network.RegisterTableRPC("SyncState", function(playerId, tbl)
    Print("State: hp=" .. tbl.hp)
end)
Network.CallTableRPC("SyncState", { hp = 100, ammo = 30 })
Network.CallTableRPCOnServer("SyncState", { hp = 80 })
Network.CallTableRPCOnPlayer(playerId, "SyncState", { hp = 50 })
```

### Network variables

```lua
-- Global variables (synced for all)
Network.SetNetVar("gameMode", "deathmatch")
local mode = Network.GetNetVar("gameMode", "default")
local has = Network.HasNetVar("gameMode")
local all = Network.GetAllNetVars()  -- table key=value
Network.ClearNetVars()

-- Server-authoritative variables: only the host/server may write them.
-- A client write to such a key is rejected by the server and the client is
-- corrected back to the authoritative value. Use for trusted state that
-- clients must not be able to forge (score, pot, whose turn it is, ...).
Network.SetAuthoritativeNetVar("pot", "1500")        -- host/server only
local isAuth = Network.IsAuthoritativeNetVar("pot")  -- true
Network.ClearAuthoritativeNetVar("pot")              -- host/server only; stops protecting the key

-- Player variables
Network.SetPlayerVar(playerId, "team", "red")
local team = Network.GetPlayerVar(playerId, "team", "none")
local has = Network.HasPlayerVar(playerId, "team")
local all = Network.GetAllPlayerVars(playerId)
Network.ClearPlayerVars(playerId)

-- Change callbacks
Network.OnNetVarChanged(function(key, value)
    Print(key .. " = " .. value)
end)
Network.OnPlayerVarChanged(function(playerId, key, value)
    Print("Player " .. playerId .. ": " .. key .. " = " .. value)
end)
```

### Rooms

```lua
-- Create room
Network.CreateRoom("MyRoom", 8, "password")   -- name, max players, password

-- Join/leave
Network.JoinRoom("MyRoom")
Network.JoinRoom("MyRoom", "password")
Network.LeaveRoom()

-- Info
local room = Network.GetCurrentRoom()
local inRoom = Network.IsInRoom()
local players = Network.GetRoomPlayers("MyRoom")
local count = Network.GetRoomPlayerCount("MyRoom")
local max = Network.GetRoomMaxPlayers("MyRoom")
local full = Network.IsRoomFull("MyRoom")
local isHost = Network.IsRoomHost()

-- All rooms
local rooms = Network.GetAllRoomNames()
local roomCount = Network.GetRoomCount()

-- Room properties
Network.SetRoomProperty("MyRoom", "map", "Forest")
local map = Network.GetRoomProperty("MyRoom", "map", "Default")
local allProps = Network.GetAllRoomProperties("MyRoom")  -- table of all key-value properties

-- Room host
local hostId = Network.GetRoomHost("MyRoom")  -- player ID of the room host

-- Room password check
local hasPwd = Network.HasRoomPassword("MyRoom")

-- Set room max players
Network.SetRoomMaxPlayers("MyRoom", 12)

-- Full room info (returns table with all details)
local roomInfo = Network.GetRoomInfo("MyRoom")
-- → {name, maxPlayers, playerCount, hostPlayerID, hasPassword, isFull, players, properties}

-- Destroy room
Network.DestroyRoom("MyRoom")

-- Callbacks
Network.OnRoomJoined(function(roomName) ... end)
Network.OnRoomJoinFailed(function(roomName, reason) ... end)  -- reason: "password" | "full" | "notfound"
Network.OnRoomLeft(function(roomName) ... end)
Network.OnRoomPlayerJoined(function(roomName, playerId) ... end)
Network.OnRoomPlayerLeft(function(roomName, playerId) ... end)
```

> **Room passwords are server-authoritative.** The password hash is kept only on the
> server (and the room creator) and is never sent to other clients — a peer can only
> learn *that* a room is protected (`Network.HasRoomPassword`), never the hash itself.
> For a remote client, `Network.JoinRoom(name, password)` is **asynchronous**: it sends
> the request and returns immediately; the actual result arrives via `OnRoomJoined`
> (success) or `OnRoomJoinFailed` (wrong password / full / not found). When you are the
> host, the join is validated locally and `OnRoomJoined` fires synchronously.

### Automatic replication (recommended)

Mark an entity once and the engine keeps it in sync for every connected client
automatically — transform, velocity, animation and all visual components — and
spawns/despawns a matching entity on every client. This is the high-level,
"set-and-forget" layer; the manual `SyncEntity*` calls below are the low-level API.

```lua
-- Host/server only. Mark an entity for automatic replication.
-- Returns a stable network id (netId) shared by all machines.
local netId = Network.Replicate(entityId)
local netId = Network.Replicate(entityId, { owner = playerId })          -- assign an owning player (metadata)
local netId = Network.Replicate(spawnedId, { prefab = "Content/Bullet.ice_class" })

-- Stop replicating (level entities stay, runtime-spawned copies are removed on clients).
Network.StopReplicating(entityId)

-- Queries (any peer)
local on   = Network.IsReplicated(entityId)
local id   = Network.GetNetId(entityId)        -- 0 if not replicated
local ent  = Network.GetEntityByNetId(netId)   -- local entity id, or 0
local count= Network.GetReplicatedCount()
Network.SetReplicationOwner(entityId, playerId)

-- After changing a *config* component at runtime on the host (sprite swap, light
-- colour, tilemap, widget, AI tuning, ...), push a one-shot full-state update:
Network.ReplicateFullState(entityId)

-- Tune how often config full-state changes are checked/sent (default 8 Hz).
Network.SetReplicationRate(8)

-- Declarative replication (recommended): tick "Replicate" on the entity's
-- Replication component in the Properties panel (level entity) or the Class Editor
-- (whole class). The host then registers/announces it automatically — no Lua needed,
-- and the settings travel with class inheritance, copy/paste, clone and spawn.
-- The same settings are readable and writable from Lua:
local rep = Network.GetReplicationSettings(entityId)
-- rep.replicate, rep.ownerMode ("server"|"player"), rep.owner,
-- rep.syncTransform, rep.syncVelocity, rep.syncVisuals, rep.syncFullState,
-- rep.fullStateRate, rep.scriptMode ("auto"|"always"|"never"),
-- rep.relevancy ("aoi"|"always"), rep.kinematicOnClients, rep.prefab

Network.SetReplicationSettings(entityId, {
    replicate = true,
    ownerMode = "player",      -- "server" (host authority) or "player"
    owner = playerId,          -- setting a non-zero owner implies ownerMode "player"
    syncTransform = true,      -- position/rotation/scale every tick
    syncVelocity = true,       -- rigidbody linear velocity every tick
    syncVisuals = true,        -- sprite/flipbook/skeleton/animator params every tick
    syncFullState = true,      -- periodic config components (lights, tilemap, AI, ...)
    fullStateRate = 0,         -- Hz; 0 = use the global replication rate
    scriptMode = "auto",       -- "auto" | "always" | "never" (Lua callbacks on replicas)
    relevancy = "aoi",         -- "aoi" (culled by Area Of Interest) | "always"
    kinematicOnClients = true, -- keep client physics from fighting the network
    prefab = "Content/Enemies/CL_Bat.ice_class", -- only for runtime-spawned entities
})

-- Shorthands
Network.SetEntityReplicated(entityId, true)
local on = Network.IsEntityReplicated(entityId)

-- The imperative Network.Replicate() below still works and now writes the same
-- component, so the Properties panel always reflects the live state.

-- Replica script gating (default: enabled). While enabled, a client does NOT run
-- Lua lifecycle callbacks (OnUpdate/OnFixedUpdate/OnLateUpdate, collision/sensor/hit
-- events, behavior trees) for replicated entities owned by the server or another
-- player — the host simulates them and the client only applies the replicated state.
-- Interface calls (Interfaces.TryCall etc.) still work on such entities, so the host
-- can trigger client-side visual functions. Disable only if your game intentionally
-- runs replica scripts on clients.
Network.SetReplicaScriptGating(true)
local gated = Network.IsReplicaScriptGating()

-- Level replication: host changes the level for everyone at once.
Network.LoadNetworkLevel("Content/Arena.icemap")
-- Clients auto-load the same level. To customise client-side loading:
Network.OnNetworkLevelLoad(function(path) LoadLevel(path) end)
```

**Model.** Replication is **host-authoritative**: the host simulates everything and
clients apply the result. What is automatic, once an entity is marked:

- **Spawn / despawn + id coordination** — the host assigns a stable `netId`. Entities
  placed in a level are bound on clients by their level UUID; entities created at runtime
  carry a `prefab` path and are instantiated on clients via `SpawnEntity`. Destroying the
  entity on the host removes it on all clients.
- **Every tick (smooth):** position, rotation, scale, velocity, animator parameters and
  the active flipbook frame. Remote bodies are made kinematic so client physics never
  fights the network.
- **On change (throttled):** all other visual/config components (sprite, light, tilemap,
  widget, destructible, AI, tags, ...), detected automatically and sent only when they
  actually change.
- Event-driven things (audio, particles/FX, one-shot effects) are intentionally **not**
  auto-replicated — drive them with RPC (`Network.CallRPC`) so they fire exactly once.

Clients send **input** (e.g. `Network.SendInput`, RPC); the host moves the entities; the
results replicate back. For fine-grained or custom needs the manual `SyncEntity*` API
below remains available.

### Entity synchronization

```lua
-- Simple position/velocity sync
Network.SyncEntityPosition(entityId, x, y)
Network.SyncEntityPosition(entityId, x, y, z)
Network.SyncEntityVelocity(entityId, vx, vy)
Network.SyncEntityTransform(entityId, x, y, rotation)
Network.SyncEntityTransform(entityId, x, y, rotation, vx, vy)

-- Custom state sync
Network.SyncEntityState(entityId, { hp = 100, state = "idle" })

-- Authoritative sync
Network.SyncEntityAuth(entityId, x, y)
Network.SyncEntityAuth(entityId, x, y, z, rotation)

-- Network spawn/destroy
Network.NetSpawnEntity("Content/Classes/Bullet.ice_class", x, y)
Network.NetSpawnEntity("Content/Classes/Bullet.ice_class", x, y, ownerPlayerId)
Network.NetDestroyEntity(entityId)

-- Entity owner
Network.SetEntityOwner(entityId, playerId)
local owner = Network.GetEntityOwner(entityId)
local mine = Network.IsEntityMine(entityId)
local isOwner = Network.IsEntityOwner(entityId, playerId)
Network.ClearEntityOwner(entityId)

-- Advanced entity system
Network.RegisterEntity(entityId, ownerPlayerId)
Network.UnregisterEntity(entityId)
Network.UpdateEntityState(entityId, x, y, z, rotation, vx, vy)
Network.UpdateEntityProperty(entityId, "hp", "100")
local state = Network.GetEntityState(entityId)
-- Base fields: x, y, z, rotation, vx, vy, scaleX, scaleY, flipX, flipY,
--   pivotX, pivotY, owner, timestamp, properties
-- Animator: animatorState, animatorFrame, animatorStateTime, animatorPath,
--   animatorTargetSprite, animatorIsTransitioning
-- AnimatorParams: animatorParamBools, animatorParamInts,
--   animatorParamFloats, animatorParamTriggers
-- Flipbook: flipbookFrame, flipbookTimer, flipbookPlaying, flipbookSpeed,
--   flipbookColorR/G/B/A, flipbookVisible, flipbookFlipX/Y, flipbookPath
-- Sprite: spriteColorR/G/B/A, spriteVisible, spriteFlipX/Y, spritePath
-- Audio: audioVolume, audioPitch, audioPlaying, audioLoop,
--   audioSpatial, audioMinDistance, audioMaxDistance, audioRolloff, audioPath
-- FX: fxPlaying, fxLoop, fxSpeed, fxFlipX/Y, fxVisible, fxPath
-- Rigidbody: rigidbodyType, rigidbodyGravityScale, rigidbodyFixedRotation,
--   rigidbodyLinearDamping, rigidbodyAngularDamping, rigidbodyIsBullet, rigidbodyAllowSleep,
--   ragdollEnabled, ragdollGravityScale, ragdollAngularDamping
-- PointLight: lightColorR/G/B, lightIntensity, lightRadius,
--   lightFalloffExponent, lightCastShadows, lightEnabled
-- SpotLight: spotLightColorR/G/B, spotLightIntensity, spotLightRadius,
--   spotLightFalloffExponent, spotLightDirX/Y,
--   spotLightInnerAngle, spotLightOuterAngle, spotLightCastShadows, spotLightEnabled
-- Tilemap: tilemapFlipX/Y, tilemapVisible, tilemapPath
-- Widget: widgetVisible, widgetScreenSpace, widgetScale,
--   widgetRenderOrder, widgetInteractable, widgetFlipX/Y, widgetPath
-- Destructible: destructEnabled, destructHealth, destructFragmentCount,
--   destructPattern, destructExplosionForce, destructOnStart, destructImpactThreshold,
--   destructFragmentLifetime, destructFragmentFadeTime, destructFragmentGravityScale,
--   destructFragmentDensity, destructFragmentFriction, destructFragmentRestitution,
--   destructFragmentIsSensor, destructFragmentEnableContactEvents,
--   destructFragmentEnableSensorEvents, destructFragmentEnableHitEvents,
--   destructFragmentEnablePreSolveEvents,
--   destructMaxDebrisCount, destructDestroyOriginal,
--   destructFragmentCastShadow, destructFragmentShadowOrigin, destructFragmentShadowEdgeFade,
--   destructFragmentShadowZOrder
-- AI: aiMoveSpeed, aiDetectionRadius, aiAttackRadius, aiPerceptionEnabled,
--   aiAssetPath, aiPerceptionSightRadius, aiPerceptionSightAngle,
--   aiPerceptionHearingRadius, aiPerceptionRequireLOS, aiPerceptionForgetTime,
--   aiPatrolPoints (array of {x, y}), aiPerceptionTargetTags (array of strings)
-- ClassComponent: classComponentInstances (array of {name, classPath}),
--   classComponentSelectedIndex
local isOwnerAuth = Network.IsEntityOwnerAuth(entityId, playerId)
Network.TransferEntityOwnership(entityId, newOwnerId)
local allIds = Network.GetAllEntityIDs()
local count = Network.GetEntityCount()
```

### Entity visuals

```lua
-- Update local visual state (scale, flip, pivot)
Network.UpdateEntityVisuals(entityId, scaleX, scaleY, flipX, flipY, pivotX, pivotY)

-- Sync visuals over network (updates locally + sends to all peers)
Network.SyncEntityVisuals(entityId, scaleX, scaleY, flipX, flipY, pivotX, pivotY)
Network.SyncEntityVisuals(entityId, scaleX, scaleY, flipX, flipY, pivotX, pivotY, reliable)
-- All parameters except entityId are optional (defaults: scale=1, flip=false, pivot=0.5)
```

### Entity properties

```lua
-- Set property (string key-value)
Network.UpdateEntityProperty(entityId, "hp", "100")

-- Read single property
local hp = Network.GetEntityProperty(entityId, "hp", "0")  -- key, default

-- Read all properties as table
local props = Network.GetEntityProperties(entityId)  -- → {hp = "100", armor = "50"}

-- Check if property exists
local has = Network.HasEntityProperty(entityId, "hp")
```

### Component synchronization

```lua
-- Animator (state machine animation)
Network.SyncEntityAnimator(entityId, "Run", 3)              -- state, frame
Network.SyncEntityAnimator(entityId, "Run", 3, 0.5)         -- + stateTime
Network.SyncEntityAnimator(entityId, "Run", 3, 0.5, true)   -- + reliable

-- Animator config (path, target sprite, transitioning)
Network.SyncEntityAnimatorConfig(entityId, "Content/Animators/Hero.ice_animator")
Network.SyncEntityAnimatorConfig(entityId, "Content/Animators/Hero.ice_animator", "idle_0", false, true)

-- Flipbook (frame animation playback)
Network.SyncEntityFlipbook(entityId, 5)                      -- frame
Network.SyncEntityFlipbook(entityId, 5, 0.1, true, 1.0)     -- frame, timer, playing, speed
Network.SyncEntityFlipbook(entityId, 5, 0.1, true, 1.0, true) -- + reliable

-- Flipbook visuals (color, visibility, flip, path)
Network.SyncEntityFlipbookVisuals(entityId, 1.0, 1.0, 1.0)           -- r, g, b
Network.SyncEntityFlipbookVisuals(entityId, 1.0, 1.0, 1.0, 1.0, true, false, false, true)
-- r, g, b, a, visible, flipX, flipY, reliable
Network.SyncEntityFlipbookVisuals(entityId, 1.0, 1.0, 1.0, 1.0, true, false, false, true, "Content/Flipbooks/Hero.ice_flipbook")
-- r, g, b, a, visible, flipX, flipY, reliable, path

-- Skeleton (2D skeletal animation playback)
Network.SyncEntitySkeleton(entityId, "Run")                  -- animation
Network.SyncEntitySkeleton(entityId, "Run", "default", 0.5, 1.0, true, true, false, true)
-- animation, skin, time, speed, playing, loop, flipX, flipY
Network.SyncEntitySkeleton(entityId, "Run", "default", 0.5, 1.0, true, true, false, true, "Content/Skeletons/Hero.ice_skeleton", true)
-- animation, skin, time, speed, playing, loop, flipX, flipY, path, reliable

-- Skeleton visuals (color, visibility, shadow, material, ragdoll, slot attachments)
Network.SyncEntitySkeletonVisuals(entityId, 1.0, 1.0, 1.0)           -- r, g, b
Network.SyncEntitySkeletonVisuals(entityId, 1.0, 1.0, 1.0, 1.0, true, true, false, 0, 0.0, 0.0, 1, 0, 0.5, "Content/Materials/Hero.ice_material", false, false, 0.5, 1.0)
-- r, g, b, a, visible, renderInGame, castShadow, shadowOrigin, shadowEdgeFade, shadowZOrder,
-- shadingMode, blendMode, alphaClipThreshold, materialPath, ragdollEnabled, ragdollAutoOnStart,
-- ragdollAngularDamping, ragdollGravityScale
Network.SyncEntitySkeletonVisuals(entityId, 1.0, 1.0, 1.0, 1.0, true, true, false, 0, 0.0, 0.0, 1, 0, 0.5, "", false, false, 0.5, 1.0, { [2] = "sword" }, true)
-- + slotAttachments ({ [slotIndex] = "attachmentName" }), reliable

-- Sprite (color, visibility, flip, path)
Network.SyncEntitySprite(entityId, 1.0, 0.0, 0.0)                    -- r, g, b
Network.SyncEntitySprite(entityId, 1.0, 0.0, 0.0, 1.0, true, false, false, true)
-- r, g, b, a, visible, flipX, flipY, reliable
Network.SyncEntitySprite(entityId, 1.0, 0.0, 0.0, 1.0, true, false, false, true, "Content/Sprites/Hero.ice_sprite")
-- r, g, b, a, visible, flipX, flipY, reliable, path

-- Audio (volume, pitch, playback, spatial, path)
Network.SyncEntityAudio(entityId)                                     -- defaults
Network.SyncEntityAudio(entityId, 0.8, 1.0, true, false, true, 1.0, 100.0, 1.0, true)
-- volume, pitch, playing, loop, spatial, minDistance, maxDistance, rolloff, reliable
Network.SyncEntityAudio(entityId, 0.8, 1.0, true, false, true, 1.0, 100.0, 1.0, true, "Content/Audio/Shot.ice_audio")
-- volume, pitch, playing, loop, spatial, minDistance, maxDistance, rolloff, reliable, path

-- FX / Particles (playback, visibility, path)
Network.SyncEntityFX(entityId)                                        -- defaults
Network.SyncEntityFX(entityId, true, true, 1.0, false, false, true, true)
-- playing, loop, speed, flipX, flipY, visible, reliable
Network.SyncEntityFX(entityId, true, true, 1.0, false, false, true, true, "Content/FX/Explosion.ice_fx")
-- playing, loop, speed, flipX, flipY, visible, reliable, path

-- Rigidbody (body type, physics properties, ragdoll)
Network.SyncEntityRigidbody(entityId, 2)                              -- bodyType (0=static, 1=kinematic, 2=dynamic)
Network.SyncEntityRigidbody(entityId, 2, 1.0, false, 0.0, 0.0, false, true, true)
-- bodyType, gravityScale, fixedRotation, linearDamping, angularDamping, isBullet, allowSleep, reliable
Network.SyncEntityRigidbody(entityId, 2, 1.0, false, 0.0, 0.0, false, true, true, true, 1.0, 0.5)
-- bodyType, gravityScale, fixedRotation, linearDamping, angularDamping, isBullet, allowSleep, reliable,
-- ragdollEnabled, ragdollGravityScale, ragdollAngularDamping

-- Point Light (color, intensity, radius)
Network.SyncEntityLight(entityId, 1.0, 0.9, 0.7)                     -- r, g, b
Network.SyncEntityLight(entityId, 1.0, 0.9, 0.7, 1.0, 100.0, true, 2.0, false, true)
-- r, g, b, intensity, radius, enabled, falloffExponent, castShadows, reliable

-- Spot Light (color, intensity, direction, angles)
Network.SyncEntitySpotLight(entityId, 1.0, 1.0, 1.0, 1.0, 100.0, 1.0, 0.0, 30.0, 45.0)
-- r, g, b, intensity, radius, dirX, dirY, innerAngle, outerAngle
Network.SyncEntitySpotLight(entityId, 1.0, 1.0, 1.0, 1.0, 100.0, 1.0, 0.0, 30.0, 45.0, true, 2.0, false, true)
-- + enabled, falloffExponent, castShadows, reliable

-- Tilemap (flip, visibility, path)
Network.SyncEntityTilemap(entityId)                                   -- defaults
Network.SyncEntityTilemap(entityId, false, false, true, true)         -- flipX, flipY, visible, reliable
Network.SyncEntityTilemap(entityId, false, false, true, true, "Content/Tilemaps/Level.ice_tm")
-- flipX, flipY, visible, reliable, path

-- Widget (UI component, path)
Network.SyncEntityWidget(entityId)                                    -- defaults
Network.SyncEntityWidget(entityId, true, true, 1.0, 0, true, false, false, true)
-- visible, screenSpace, scale, renderOrder, interactable, flipX, flipY, reliable
Network.SyncEntityWidget(entityId, true, true, 1.0, 0, true, false, false, true, "Content/Widgets/HUD.ice_widget")
-- visible, screenSpace, scale, renderOrder, interactable, flipX, flipY, reliable, path

-- Destructible (destruction component, extended)
Network.SyncEntityDestructible(entityId)                              -- defaults
Network.SyncEntityDestructible(entityId, true, 100.0, 8, 0, 300.0, true)
-- enabled, health, fragmentCount, pattern, explosionForce, reliable
Network.SyncEntityDestructible(entityId, true, 100.0, 8, 0, 300.0, true,
    false, 0.0, 3.0, 1.0, 1.0, 1.0, 0.3, 0.3, false, true, false, true, false, false, 256, true,
    true, 0, 0.0, 0)
-- enabled, health, fragmentCount, pattern, explosionForce, reliable,
-- destructOnStart, impactThreshold, fragmentLifetime, fragmentFadeTime,
-- fragmentGravityScale, fragmentDensity, fragmentFriction, fragmentRestitution,
-- fragmentIsSensor, fragmentEnableContactEvents, fragmentEnableSensorEvents,
-- fragmentEnableHitEvents, fragmentEnablePreSolveEvents,
-- maxDebrisCount, destroyOriginal,
-- fragmentCastShadow, fragmentShadowOrigin, fragmentShadowEdgeFade, fragmentShadowZOrder

-- AI (movement, perception, extended)
Network.SyncEntityAI(entityId)                                        -- defaults
Network.SyncEntityAI(entityId, 100.0, 200.0, 50.0, false, true)
-- moveSpeed, detectionRadius, attackRadius, perceptionEnabled, reliable
Network.SyncEntityAI(entityId, 100.0, 200.0, 50.0, true, true,
    "Content/AI/EnemyBT.ice_ai", 300.0, 120.0, 500.0, false, 5.0,
    { {x = 100, y = 200}, {x = 300, y = 400} },
    { "Player", "Ally" })
-- moveSpeed, detectionRadius, attackRadius, perceptionEnabled, reliable,
-- aiAssetPath, perceptionSightRadius, perceptionSightAngle,
-- perceptionHearingRadius, perceptionRequireLOS, perceptionForgetTime,
-- patrolPoints (array of {x, y}), targetTags (array of strings)

-- ClassComponent (class instances sync)
Network.SyncEntityClassComponent(entityId, {
    { name = "WeaponScript", classPath = "Content/Classes/Weapon.ice_class" },
    { name = "HealthScript", classPath = "Content/Classes/Health.ice_class" }
})
-- instances (array of {name, classPath})
Network.SyncEntityClassComponent(entityId, {
    { name = "WeaponScript", classPath = "Content/Classes/Weapon.ice_class" }
}, 0, true)
-- instances, selectedIndex, reliable

-- AnimatorParams (animator parameters sync)
Network.SyncEntityAnimatorParams(entityId,
    { isRunning = true, isGrounded = false },   -- bools
    { comboCount = 3 },                          -- ints
    { speed = 1.5 },                             -- floats
    { attack = true })                           -- triggers
-- bools, ints, floats, triggers (all optional key-value tables)
Network.SyncEntityAnimatorParams(entityId, nil, nil, { speed = 2.0 }, nil, true)
-- bools, ints, floats, triggers, reliable
```

### Interpolation and lag compensation

```lua
-- Position interpolation (includes visuals)
local snap = Network.InterpolateEntity(entityId, renderTime)
-- → {x, y, z, rotation, vx, vy, scaleX, scaleY, flipX, flipY, pivotX, pivotY}
Network.SetInterpolationDelay(0.1)  -- seconds
local delay = Network.GetInterpolationDelay()

-- Lag compensation (includes visuals)
Network.EnableLagCompensation(true)
local enabled = Network.IsLagCompensationEnabled()
local pastState = Network.GetEntityAtTime(entityId, timestamp)
-- → {x, y, z, rotation, vx, vy, scaleX, scaleY, flipX, flipY, pivotX, pivotY}
Network.SetMaxHistoryDuration(2.0)
local maxHistory = Network.GetMaxHistoryDuration()  -- current max history duration
```

#### Server-side rewind (authoritative hit detection)

`EnableLagCompensation(true)` makes the **server** record a position history for every replicated
entity each tick. To validate a shot the way the shooter actually saw the world, rewind the world
to that client's view, run your hit test, then restore. Rewinding moves both the transform and the
Box2D body, so raycasts and overlap queries hit the historical positions.

```lua
Network.EnableLagCompensation(true)
Network.SetLagCompensationWindow(1.0)      -- how many seconds of history to keep (0.05 .. 10)
local window = Network.GetLagCompensationWindow()   -- → number (seconds)

-- Inside a server RPC handling "player X fired":
if Network.RewindForPlayer(shooterId) then
    local hit = Raycast(originX, originY, dirX, dirY, range)
    Network.RestoreRewind()                 -- ALWAYS restore, even if nothing was hit
    if hit and hit.hit then ApplyDamage(hit.entityId) end
end
```

`RewindForPlayer` picks the time from that player's ping plus the interpolation delay. For full
control use `Network.GetRewindTimeForPlayer(id)` and `Network.RewindToTime(t)`.
`Network.IsRewound()` reports whether the world is currently rewound — never leave it rewound
across a frame.

#### Reconciliation with input replay

By default a predicted entity is corrected by **smoothing** toward the server position, which suits
co-op and action games. For competitive movement you want the classic model instead: snap to the
authoritative state, then re-apply every input the server has not acknowledged yet.

```lua
Network.SetReplayReconciliation(true)
local replayMode = Network.IsReplayReconciliation()   -- → bool (false = smoothing)
Network.OnReplayInput(function(input, dt)
    ApplyMovementInput(input, dt)          -- the same function your normal update calls
end)
```

The engine snaps the entity to the server state (position, rotation and velocity), then calls your
callback once per unacknowledged input frame in order, with that frame's real delta time. Send
inputs with `Network.SendInput` so they enter the pending queue. Without a registered callback the
engine keeps using smoothing, so enabling this is opt-in and safe.

```lua
-- Time sync
local serverTime = Network.GetServerTimeSync()
local clientTime = Network.GetClientTimeSync()
local offset = Network.GetTimeSyncOffset()
```

### Player readiness

```lua
Network.SetReady(true)
local ready = Network.IsPlayerReady(playerId)
local allReady = Network.AreAllPlayersReady()
local readyCount = Network.GetReadyCount()
Network.ClearReadyStates()
```

### Network input

```lua
Network.SendInput("jump")
Network.SendInputTable({ jump = true, moveX = 0.5 })
local lastAck = Network.GetLastAcknowledgedInput()
local pending = Network.GetPendingInputCount()

-- On the host: receive input from clients
Network.OnInputReceived(function(playerId, data)
    local input = Network.JsonDecode(data)
    -- apply the input to playerId's entity
end)
```

### Voice chat

```lua
Network.EnableVoiceChat(true)
local enabled = Network.IsVoiceChatEnabled()
Network.SetVoiceMuted(true)   -- mutes YOUR microphone only; you still hear others
local muted = Network.IsVoiceMuted()

-- Push-to-talk pattern: keep the mic muted and unmute while a key is held.
-- Network.SetVoiceMuted(not IsActionPressed("VoiceTalk"))

-- Per-player voice control
Network.SetPlayerVoiceVolume(playerId, 0.5)
local vol = Network.GetPlayerVoiceVolume(playerId)
Network.SetPlayerVoiceMuted(playerId, true)
local playerMuted = Network.IsPlayerVoiceMuted(playerId)
local talking = Network.IsPlayerTransmitting(playerId)

-- Voice proximity (spatial voice chat)
Network.SetVoiceProximity(true, 500.0)  -- enable, range
local proxEnabled = Network.IsVoiceProximityEnabled()
local proxRange = Network.GetVoiceProximityRange()

-- Voice relay limit
Network.SetMaxVoiceRelayPlayers(16)              -- max players for voice relay
local maxRelay = Network.GetMaxVoiceRelayPlayers() -- current limit
```

### Administration

```lua
Network.KickPlayer(playerId)
Network.BanPlayer(playerId)
Network.UnbanPlayer(playerId)
local banned = Network.IsPlayerBanned(playerId)
Network.SetMaxPlayers(32)   -- clamped to 2..256
Network.SetPassword("secret")
local hasPwd = Network.HasPassword()
local pwd = Network.GetPassword()  -- current server password

-- Ban by name
Network.BanPlayerByName("Cheater123")
Network.UnbanByName("Cheater123")
local bannedNames = Network.GetBannedNames()  -- table of banned names

-- Ban list persistence
local saved = Network.SaveBanList("bans.json")
local loaded = Network.LoadBanList("bans.json")

-- Whitelist
Network.EnableWhitelist(true)
local wlEnabled = Network.IsWhitelistEnabled()
Network.AddToWhitelist("TrustedPlayer")
Network.RemoveFromWhitelist("TrustedPlayer")
local wl = Network.GetWhitelist()  -- table of whitelisted names
local isWl = Network.IsWhitelisted("TrustedPlayer")

-- Whitelist persistence
local savedWl = Network.SaveWhitelist("whitelist.json")
local loadedWl = Network.LoadWhitelist("whitelist.json")

-- Text chat mute
Network.SetPlayerTextMuted(playerId, true)
local isMuted = Network.IsPlayerTextMuted(playerId)

-- Profanity filter
Network.EnableProfanityFilter(true)
Network.AddProfanityWord("badword")
Network.ClearProfanityWords()
local filtered = Network.FilterProfanity("some badword text")
```

### Settings and stats

```lua
-- Configuration
Network.SetTickRate(60)
local tickRate = Network.GetTickRate()
local maxPlayers = Network.GetMaxPlayers()
local port = Network.GetPort()
local ip = Network.GetServerIP()

-- Ping
local ping = Network.GetPing()
local playerPing = Network.GetPlayerPing(playerId)
local avgPing = Network.GetAveragePing()

-- Traffic
local sent = Network.GetBytesSent()
local recv = Network.GetBytesReceived()
local pSent = Network.GetPlayerBytesSent(playerId)
local pRecv = Network.GetPlayerBytesReceived(playerId)

-- Uptime
local uptime = Network.GetUptime()

-- Network time
Network.UpdateNetworkTime(dt)
local netTime = Network.GetNetworkTime()
Network.SetServerTimeDelta(0.01)
local delta = Network.GetServerTimeDelta()

-- Limits and security
Network.SetRateLimit(100, 65536)         -- max packets/sec, max bytes/sec
Network.SetSnapshotRate(20)
local sr = Network.GetSnapshotRate()
Network.SetMaxEntitySpeed(1000)
local maxSpeed = Network.GetMaxEntitySpeed()  -- current max entity speed
Network.SetValidationEnabled(true)
local valEnabled = Network.IsValidationEnabled()

-- Trust model: how much the authoritative server trusts clients.
-- "competitive" (default) enforces entity ownership + speed/anti-cheat validation;
-- "coop" trusts clients (lighter validation, lower overhead) for friends / co-op games.
Network.SetTrustModel("competitive")     -- or "coop"
local trust = Network.GetTrustModel()    -- "competitive" | "coop"

-- Server world snapshot (broadcast full state to all clients)
Network.BroadcastWorldSnapshot()  -- server-only

-- Area of Interest (relevancy culling). When enabled, the server sends each player
-- only the entities within their interest radius (plus entities they own), instead of
-- the entire world. This is what makes large player counts (e.g. 100-player battle
-- royale) feasible on bandwidth. Off by default — behaviour is unchanged unless enabled.
Network.SetAreaOfInterest(true, 2000)              -- enable, radius in world units
local aoiOn = Network.IsAreaOfInterestEnabled()
local aoiRadius = Network.GetAreaOfInterestRadius()
Network.SetPlayerInterestPosition(playerId, x, y)  -- server: set a player's focus point (camera/avatar) each tick

-- Reconnect
Network.EnableReconnect(true, 5, 2.0)  -- enable, maxAttempts, interval
local reconnecting = Network.IsReconnecting()
local attempt = Network.GetReconnectAttempt()
local connState = Network.GetConnectionState()

-- Full reset
Network.ResetNetworkState()

-- Dedicated server
Network.SetDedicatedServer(true)
local isDedicated = Network.IsDedicatedServer()

-- Client-side prediction
Network.SetPredictionEnabled(true)
local predEnabled = Network.IsPredictionEnabled()

-- Prediction reconciliation tuning (auto-replication, owned entities only). With
-- prediction on, an entity you own keeps simulating locally and is nudged toward the
-- host's authoritative state each tick instead of being snapped to it outright.
Network.SetPredictionSmoothing(0.2)        -- blend factor 0..1 toward the server position per tick (default 0.2; 0 = pure prediction, 1 = snap every tick)
local smoothing = Network.GetPredictionSmoothing()
Network.SetPredictionSnapDistance(500)     -- if prediction drifts further than this (world units) from the server, hard-snap instead of smoothing (default 500; 0 = never snap)
local snapDist = Network.GetPredictionSnapDistance()

-- Delta compression (field level: only changed fields go on the wire)
Network.SetDeltaCompressionEnabled(true)
local deltaEnabled = Network.IsDeltaCompressionEnabled()

-- Packet compression (whole-packet adaptive range coder, ON by default)
Network.SetPacketCompressionEnabled(true)
local packetEnabled = Network.IsPacketCompressionEnabled()

-- Server/client time (direct access)
local srvTime = Network.GetServerTime()
local cliTime = Network.GetClientTime()

-- Traffic encryption (XChaCha20-Poly1305 AEAD via libsodium): confidentiality + integrity.
-- Both ends must call EnableEncryption with the SAME key before connecting. Forged or
-- tampered packets fail authentication and are dropped. Use a strong, high-entropy key.
Network.EnableEncryption("my_secret_key")
local encEnabled = Network.IsEncryptionEnabled()

-- NAT traversal (peer-to-peer connectivity)
Network.EnableNATTraversal(true)                                    -- defaults: stun.l.google.com:19302
Network.EnableNATTraversal(true, "stun.l.google.com", 19302)       -- custom STUN server
local natEnabled = Network.IsNATEnabled()
local externalIP = Network.GetExternalIP()
local externalPort = Network.GetExternalPort()
local discovered = Network.DiscoverExternalAddress()

-- Async NAT discovery
Network.DiscoverExternalAddressAsync()
local pending = Network.IsDiscoveryPending()
local result = Network.PollDiscoveryResult()  -- returns discovery result or nil if still pending
```

> **The two compressions are different layers and stack.** Delta compression decides *what* goes
> on the wire (only changed fields); packet compression shrinks *the bytes themselves* with an
> adaptive range coder over every outgoing packet. Packet compression is on by default and costs
> a little CPU per packet — on a 256-player server that is a real trade, so measure before
> changing it: `NetworkProfiler.GetTotalBytesSent()` and the Network Profiler show the actual effect on
> your traffic, which depends entirely on your payloads.
>
> **Both ends must agree.** A host with compression on and a client with it off cannot decode
> each other's packets. Leave it at the default unless you are profiling, and change it on both
> sides before connecting.
>
> **Turn it off when you enable encryption.** Payloads are encrypted before they reach ENet, and
> ciphertext is high-entropy — the range coder cannot shrink it, so you pay the CPU for nothing.
> With `Network.EnableEncryption(...)` active, call
> `Network.SetPacketCompressionEnabled(false)` on both sides before connecting.

### Server discovery (LAN & master server)

Let players find games without typing an IP. A host **advertises** its server and clients
**discover** the list. It works over the **LAN** (UDP broadcast) and/or through a **master
server** — a lightweight registry you can run anywhere on the internet. The three roles are
independent: a server may advertise on LAN and a master at the same time, a client may browse
both at once, and any machine can host the master registry.

```lua
-- HOST: start advertising this server so others can find it. The options table is
-- optional; every field is optional too (defaults shown).
local ok = Network.AdvertiseServer({
    name        = "My Server",   -- display name shown in the browser
    port        = 7777,          -- game port clients will Connect() to
    maxPlayers  = 16,
    players     = 0,             -- current player count
    hasPassword = false,
    gameMode    = "deathmatch",  -- free-form tag
    lan         = true,          -- broadcast on the local network
    masterIp    = "",            -- also register with this master server (empty = LAN only)
    masterPort  = 0,
    lanPort     = 7778,          -- UDP discovery port (must match the clients' lanPort)
})                               -- → true if advertising started

-- Refresh the advertised info while running (e.g. when players join/leave). Pass every
-- field you care about — omitted fields fall back to their defaults, they are NOT kept
-- from the original AdvertiseServer call.
Network.UpdateAdvertise({ name = "My Server", port = 7777, maxPlayers = 16, players = 5 })

Network.StopAdvertise()
local advertising = Network.IsAdvertising()

-- CLIENT: scan for servers. Same lan / masterIp / masterPort / lanPort options as
-- AdvertiseServer (the options table and all its fields are optional).
local ok = Network.DiscoverServers({ lan = true, masterIp = "", masterPort = 0, lanPort = 7778 })  -- → true if the scan started
local scanning = Network.IsDiscovering()
local count = Network.GetDiscoveredServerCount()
local servers = Network.GetDiscoveredServers()
-- → array of { name, ip, port, players, maxPlayers, hasPassword, gameMode, isLAN }
for _, s in ipairs(servers) do
    print(s.name, s.ip .. ":" .. s.port, s.players .. "/" .. s.maxPlayers, s.isLAN and "LAN" or "Internet")
end
Network.ClearDiscoveredServers()   -- empty the list now (stale entries also expire on their own)
Network.StopDiscovery()

-- Connect to a chosen entry like any other server:
-- Network.Connect(servers[1].ip, servers[1].port)

-- MASTER: host the registry that internet servers register with and clients query.
local ok = Network.StartMasterServer(7779)             -- → true if the master started on this port
local isMaster = Network.IsMasterServer()
local registered = Network.GetMasterRegisteredCount()  -- servers currently registered
Network.StopMasterServer()
```

> `lanPort` is the UDP port used for LAN broadcast discovery and must be identical on the
> host and every client; it is separate from the game `port` passed to `Network.Connect`.
> Discovered servers expire automatically a few seconds after they stop advertising, so the
> browser list stays current without manual cleanup.

> **Android 17 (API 37) and the local network permission.** In a build whose **Target SDK is
> 37 or higher**, Android refuses every LAN socket until `ACCESS_LOCAL_NETWORK` is granted:
> the broadcast beacon, the discovery scan and `Network.Connect` to a LAN address all fail,
> while internet servers and loopback keep working. Enable **Local Network (LAN)** in
> Build Game → Android (CLI: `--enable-local-network`) so the manifest declares it, then ask
> for it before you start discovering:
>
> ```lua
> if not Permissions.Has(Permissions.ACCESS_LOCAL_NETWORK) then
>     Permissions.Request(Permissions.ACCESS_LOCAL_NETWORK)
> end
> ```
>
> `Permissions.Has()` already returns `true` on Android 16 and older and in builds that
> target API 36 or lower, so the same code is correct everywhere.

### NetworkProfiler — runtime network profiler (debug only)

`NetworkProfiler` is a global Lua table registered by the engine for inspecting real network traffic. The engine instruments every send/receive path (ENet on desktop, WebSocket on Web) and aggregates traffic statistics per message type with EWMA-smoothed rates and a 120-second rolling history.

- **Active only in Debug builds** of the engine. In Release the toggle is a no-op and every getter returns `0` / empty tables. Use `NetworkProfiler.IsDebug()` to check at runtime.
- The overlay is rendered only in the standalone Debug runtime (no editor). Toggle it programmatically via `NetworkProfiler.Toggle()` and bind it to any key, gamepad button, console command or UI widget with `IsKeyJustPressed`.
- All functions are thread-safe and cheap to call from game scripts every frame.

#### Overlay control

```lua
local visible = NetworkProfiler.IsVisible()
NetworkProfiler.Toggle()
NetworkProfiler.Reset()
local debugBuild = NetworkProfiler.IsDebug()
```

#### Totals and rates

```lua
local bs = NetworkProfiler.GetTotalBytesSent()        -- integer, bytes since start
local br = NetworkProfiler.GetTotalBytesReceived()
local ps = NetworkProfiler.GetTotalPacketsSent()
local pr = NetworkProfiler.GetTotalPacketsReceived()

local kbpsOut = NetworkProfiler.GetKBpsSent()         -- number (KB/s), EWMA-smoothed
local kbpsIn  = NetworkProfiler.GetKBpsReceived()
local ppsOut  = NetworkProfiler.GetPPSSent()          -- number (packets/s)
local ppsIn   = NetworkProfiler.GetPPSReceived()

local pingMs   = NetworkProfiler.GetPing()            -- integer ms (last measured)
local players  = NetworkProfiler.GetPlayerCount()     -- integer
```

#### Per-message-type statistics

Use the `NetworkProfiler.MSG_*` constants, which mirror the engine `NetMessageType` enum:

```
MSG_PlayerJoin       MSG_PlayerLeave       MSG_Ping              MSG_Pong
MSG_TextChat         MSG_VoiceData         MSG_PrivateChat       MSG_ChannelChat
MSG_ChannelJoin      MSG_ChannelLeave      MSG_EntitySync        MSG_EntitySpawn
MSG_EntityDestroy    MSG_TransformUpdate   MSG_Snapshot          MSG_EntityOwnership
MSG_InputState       MSG_InputAck          MSG_RemoteCall        MSG_Reconnect
MSG_RateExceeded     MSG_DeltaSnapshot     MSG_AuthChallenge     MSG_ShutdownNotice
MSG_Custom
```

```lua
local s = NetworkProfiler.GetTypeStats(NetworkProfiler.MSG_Snapshot)
-- s = { packetsSent = ..., packetsReceived = ..., bytesSent = ..., bytesReceived = ... }
print(s.packetsSent, s.bytesReceived)
```

#### History ring buffer

`GetHistory()` returns an array of per-second samples (oldest → newest). The buffer capacity is fixed:

```lua
local cap = NetworkProfiler.GetHistoryCapacitySeconds()   -- 120
local h   = NetworkProfiler.GetHistory()
for i, s in ipairs(h) do
    -- s = { t=<seconds_since_start>, bytesSent=..., bytesReceived=...,
    --       packetsSent=..., packetsReceived=..., ping=..., playerCount=... }
end
```

#### Saving a report

`SaveReport(path?)` writes a JSON file with totals, per-type stats and full history, and returns the actual output path (or an empty string in Release / on failure).

```lua
-- Default location (picked by the engine)
local path = NetworkProfiler.SaveReport()

-- Explicit path
local path2 = NetworkProfiler.SaveReport("Logs/netreport.json")
print("Network report saved to:", path)
```

#### Full example

```lua
-- Toggle overlay and periodically save a traffic report
if IsKeyJustPressed("f9") then
    NetworkProfiler.Toggle()
end

if NetworkProfiler.IsDebug() and GetTime() % 60.0 < GetDeltaTime() then
    local kb = NetworkProfiler.GetKBpsSent() + NetworkProfiler.GetKBpsReceived()
    if kb > 256.0 then
        NetworkProfiler.SaveReport(string.format("Logs/net-%d.json", os.time()))
    end
end
```

---

## 32.5. Rollback — Deterministic Rollback Netcode (Rollback)

> **Type:** Global (`Rollback` table).
>
> GGPO-style **rollback netcode** for frame-perfect online games (fighting games, 1v1/2v2 action, lockstep). It runs on top of the authoritative `Network` transport (ENet) and the deterministic `Random` service.

### When to use this instead of `Network`

`Network.*` is **authoritative + snapshot** netcode — the right tool for co-op, shooters, MMO-lite and MOBA, where the server owns the world and clients interpolate. `Rollback.*` is a different model for the small class of games that need **frame-perfect input sync**: every peer simulates *all* players locally, predicts remote inputs, and silently rolls back + re-simulates the moment a real input contradicts a prediction. There is no perceived input latency for the local player.

### Hard requirement: a deterministic, fixed-step simulation

Rollback only works if your gameplay is **deterministic**: the same starting state + the same inputs must always produce the exact same next state, on every machine running the same build. You provide that simulation through three callbacks:

| Callback | Contract |
|---|---|
| `Rollback.OnSaveState(fn)` | `fn()` returns a **string** that fully captures the game state of the current frame. |
| `Rollback.OnLoadState(fn)` | `fn(state, frame)` restores the simulation from a string previously returned by save. |
| `Rollback.OnAdvanceFrame(fn)` | `fn(inputs, frame)` advances the simulation **exactly one fixed step** using `inputs`. |

`inputs` is an array indexed `1..PlayerCount`; each entry is `{ bits = <string>, predicted = <bool> }`.

**Determinism rules** (follow all of them):
- Inside `OnAdvanceFrame`, drive the simulation **only** from `inputs` — never read live input (`IsKeyPressed`, `GetMousePosition`, …) or per-frame globals.
- For randomness use `RNG.*` or the global `Random*()` helpers — both draw from the engine `RandomService`. Its state is saved and restored **automatically** every frame (toggle with the session config), so dice rolls replay identically through a rollback. Plain `math.random` is **not** covered.
- Do **not** use wall-clock time (`GetTime()`, `GetDeltaTime()`, `os.time()`), un-ordered iteration (`pairs` over a hash whose order you depend on), or any value that differs run-to-run.
- Box2D physics is deterministic for the **same binary on the same platform** — both peers must run the same build. Cross-platform float determinism is not guaranteed; ship identical executables for ranked play.

### The frame loop

Every fixed tick (typically 60 Hz), sample the local input and call `Rollback.Tick(input)`. The engine:
1. stores your local input (with optional input delay) and sends it to the other peers,
2. predicts any remote inputs that have not arrived yet and advances the present frame via `OnAdvanceFrame`,
3. when a real remote input arrives that differs from the prediction, calls `OnLoadState` at the mispredicted frame and re-runs `OnAdvanceFrame` forward to the present.

`Rollback.Tick(input)` returns a status string:

| Result | Meaning |
|---|---|
| `"ok"` | The simulation advanced one frame. Render the current state. |
| `"stall"` | Do **not** advance this tick — the prediction window is full or time-sync asked you to wait for the remote peer. Re-render the previous frame. |
| `"notready"` | Still synchronizing with peers. |
| `"error"` | No active session. |

### API reference

| Function | Description |
|---|---|
| `Rollback.StartSession(numPlayers, inputSize [, frameDelay [, maxRollback]])` | Start a P2P rollback session. Requires an active `Network` connection with all players present. `inputSize` = bytes of input per player per frame. `frameDelay` (default 2) trades a little latency for fewer rollbacks. `maxRollback` (default 8) is the prediction window. Returns `bool`. |
| `Rollback.StartSyncTest(inputSize, checkDistance [, numPlayers])` | Start an **offline** determinism test: every frame it rolls back `checkDistance` frames and re-simulates, firing a `desync` event if the checksum changes. Use this during development to catch non-determinism. Returns `bool`. |
| `Rollback.Stop()` | End the session. |
| `Rollback.IsRunning()` | `bool` — a session is active. |
| `Rollback.IsSynchronized()` | `bool` — all peers handshaked and simulation has started. |
| `Rollback.Tick(input)` | Advance one frame using the local `input` string. Returns `"ok"` / `"stall"` / `"notready"` / `"error"`. |
| `Rollback.GetLocalHandle()` | `int` — this peer's player index (`0..numPlayers-1`). |
| `Rollback.GetPlayerCount()` | `int`. |
| `Rollback.GetCurrentFrame()` | `int` — the next frame to simulate. |
| `Rollback.GetConfirmedFrame()` | `int` — the last frame for which all inputs are confirmed (no prediction). |
| `Rollback.RecommendStallFrames()` | `int` — time-sync hint; how many frames you are ahead of the remote peer. |
| `Rollback.SetPlayerHandle(handle, netPlayerId, local)` | Manually map a player index to a network player id (auto-assigned by sorted id otherwise). |
| `Rollback.GetInputs()` | Table of the inputs used for the current frame (`{ bits, predicted }`). |
| `Rollback.GetStats()` | Table: `frame`, `confirmed_frame`, `predicted_frames`, `rollbacks_per_second`, `max_rollback_frames`, `avg_rollback_frames`, `frame_advantage`, `ping`, `synchronized`. |
| `Rollback.OnSaveState(fn)` / `OnLoadState(fn)` / `OnAdvanceFrame(fn)` / `OnEvent(fn)` | Register the simulation and event callbacks. |
| `Rollback.ClearCallbacks()` | Remove all registered callbacks. |

`OnEvent(fn)` receives a table with `type` (`"synchronizing"`, `"synchronized"`, `"disconnected"`, `"timesync"`, `"desync"`, …), plus `player`, `frame`, `count`, `total`, `frames_ahead`, `local_checksum`, `remote_checksum`.

### Complete example (1-byte input bitmask)

```lua
-- Deterministic state for two fighters: { x, vx } each. Keep it in plain Lua.
local sim = { p = { {x=-100,vx=0}, {x=100,vx=0} } }

local function encodeState()
    return string.format("%d|%d|%d|%d",
        sim.p[1].x, sim.p[1].vx, sim.p[2].x, sim.p[2].vx)
end
local function decodeState(s)
    local a,b,c,d = s:match("(-?%d+)|(-?%d+)|(-?%d+)|(-?%d+)")
    sim.p[1].x, sim.p[1].vx = tonumber(a), tonumber(b)
    sim.p[2].x, sim.p[2].vx = tonumber(c), tonumber(d)
end

local LEFT, RIGHT = 1, 2
local function sampleLocalInput()
    local b = 0
    if IsKeyPressed("left")  then b = b | LEFT  end
    if IsKeyPressed("right") then b = b | RIGHT end
    return string.char(b)
end

Rollback.OnSaveState(function() return encodeState() end)
Rollback.OnLoadState(function(state, frame) decodeState(state) end)
Rollback.OnAdvanceFrame(function(inputs, frame)
    for i = 1, #inputs do
        local b = string.byte(inputs[i].bits) or 0
        local move = 0
        if b & LEFT  ~= 0 then move = move - 5 end
        if b & RIGHT ~= 0 then move = move + 5 end
        sim.p[i].vx = move
        sim.p[i].x  = sim.p[i].x + move
    end
end)
Rollback.OnEvent(function(e)
    if e.type == "synchronized" then print("Match start!") end
    if e.type == "desync" then print("DESYNC at frame "..e.frame) end
end)

-- After Network.Connect / Network.StartServer and both players are in:
Rollback.StartSession(2, 1, 2, 8)   -- 2 players, 1-byte input, delay 2, window 8

-- In your fixed 60 Hz update:
function OnFixedUpdate()
    if not Rollback.IsSynchronized() then return end
    local result = Rollback.Tick(sampleLocalInput())
    if result == "ok" then
        -- push sim.p[i].x into the visible entities and render
    end
    -- "stall" / "notready": just keep showing the last frame
end
```

> **Tip:** Before going online, run `Rollback.StartSyncTest(1, 7)` and play locally. If you ever see a `desync` event, some state is escaping your `OnSaveState` (or you used non-deterministic data) — fix it before shipping.

---

## 33. Navigation — Navigation and Pathfinding

> **Type:** Global (`Nav` table) + entity-bound (for AIComponent).
>
> A* navigation on a grid (NavGrid). The grid is built from tilemaps and scene colliders.

### Global navigation functions (`Nav.*`)

```lua
-- Build/rebuild navigation grid from current scene
Nav.RebuildGrid()

-- Has grid?
local has = Nav.HasGrid()

-- Grid info → {width, height, cellSize}
local size = Nav.GetGridSize()
local cellSize = Nav.GetCellSize()
local diagonal = Nav.GetAllowDiagonal()
local origin = Nav.GetOrigin()  -- → {x, y}

-- Check point walkability (AI can stand/move there)
local walkable = Nav.IsWalkable(worldX, worldY)

-- Check whether a point sits inside a real obstacle/wall (raw collision data,
-- ignores AgentRadius padding and side-view "no floor" cells).
-- Use this for line-of-sight, ray-tests and visibility — not for pathfinding.
local solid = Nav.IsSolid(worldX, worldY)

-- Ray-march line-of-sight between two world points.
-- Returns true if no Solid cell intercepts the segment.
local clear = Nav.LineOfSight(ax, ay, bx, by)

-- Find path (returns table of points {x, y})
local path = Nav.FindPath(startX, startY, endX, endY)
local path = Nav.FindPath(startX, startY, endX, endY, true)  -- diagonal

-- Coordinate conversion
local grid = Nav.WorldToGrid(worldX, worldY)   -- → {x, y}
local world = Nav.GridToWorld(gridX, gridY)     -- → {x, y}

-- Distance between two points
local dist = Nav.GetDistance(x1, y1, x2, y2)

-- Navigation mode of the grid under a world point.
-- Returns 0 = Top-Down, 1 = Side-View, -1 = no grid there.
local mode = Nav.GetMode(worldX, worldY)
```

#### Script-defined grids

`RebuildGrid` derives walkability from tilemaps and scene colliders. When the world is *logical* rather than physical —
a roguelike dungeon, a colony-sim map, a board game, a strategic province map — you can hand the navigation system your
own grid instead, and everything above (`FindPath`, `IsWalkable`, `LineOfSight`, flow fields) works on it.

```lua
-- width, height in cells; cellSize in world units; origin = world position of cell (0,0)'s corner.
-- blocked is an optional flat array, width*height entries, row-major, 1-based:
-- true / non-zero = wall, false / 0 / nil = walkable. Omit it for an all-walkable grid.
local blocked = {}
for y = 0, H - 1 do
    for x = 0, W - 1 do
        blocked[y * W + x + 1] = IsWall(x, y) and 1 or 0
    end
end
Nav.SetGrid(W, H, 32.0, 0.0, 0.0, blocked)     -- → bool; limit 16 000 000 cells

Nav.SetWalkable(gridX, gridY, false)           -- carve or seal one cell (dig a tunnel, close a door)
Nav.IsWalkableCell(gridX, gridY)               -- read one cell by grid index
Nav.ClearGrid()                                -- drop the grid and the flow field
```

> `Nav.SetGrid` replaces the first navigation grid, so call it *after* `Nav.RebuildGrid` if you use both, and call it
> again whenever your world changes shape in bulk. Single-cell edits are cheaper through `Nav.SetWalkable`.

#### Flow fields (many agents, one target)

`Nav.FindPath` runs a full A* per call, which is the right tool for a handful of agents. When hundreds or thousands of
units share one destination — a horde converging on the player, a colony hauling to a stockpile, creeps walking a tower-
defense lane — build the field **once** and let every agent read the gradient in constant time.

```lua
Nav.BuildFlowField(targetX, targetY)                 -- Dijkstra outward from the target → bool
Nav.BuildFlowField(targetX, targetY, true, 60.0)     -- diagonal, and stop expanding past cost 60
Nav.HasFlowField()                                   -- bool
Nav.ClearFlowField()

Nav.GetFlowCost(worldX, worldY)      -- steps to the target (diagonals cost ~1.41); -1 = unreachable
Nav.GetFlowDirection(worldX, worldY) -- {x, y} unit step downhill; {0, 0} at the target or off-field
Nav.GetFlowNextPoint(worldX, worldY) -- world centre of the next cell, or nil
```

The cost is also a ready-made **influence / threat map**: `GetFlowCost` tells you how far anything is from the target,
so you can pick cover, flee uphill, or gate spawns by distance without any extra search.

```lua
-- One field per frame, read by every enemy.
function OnUpdate(dt)
    local p = GetPlayerWorldPos()
    Nav.BuildFlowField(p.x, p.y)

    for _, id in ipairs(FindEntitiesByTag("Enemy")) do
        local e = GetEntityPosition(id)
        local dir = Nav.GetFlowDirection(e.x, e.y)
        if dir.x ~= 0 or dir.y ~= 0 then
            SetEntityVelocity(id, dir.x * SPEED, dir.y * SPEED)
        end
    end
end
```

> Rebuild the field when the target moves to another cell or the map changes; a static target needs one build ever.
> `maxCost` bounds the search, so a field around the player can stay cheap on a very large map.

#### Terrain cost

Walkability answers *can I go here*. Cost answers *how much do I want to*. Roads, mud, shallow water, a guarded corridor,
difficult terrain in a tactics game — all of it is one number per cell, honoured by `FindPath`, `FindPathAsync` **and**
the flow field.

```lua
Nav.SetCost(gridX, gridY, 3.0)                  -- three times as expensive to cross
Nav.GetCost(gridX, gridY)                       -- → 1.0 when nothing was set
Nav.SetCostRect(gridX, gridY, w, h, 0.5)        -- a road: half price
Nav.HasCosts()                                  -- has any cost been set at all?
Nav.ClearCosts()                                -- back to uniform cost
```

- The baseline is **1.0**; a cell you never touch stays 1.0 and costs nothing in memory — the cost array is only
  allocated the first time you write to it.
- Values are clamped to a small positive minimum, so a cell can be cheap but never free or negative.
- Costs below 1.0 are fully supported: the A* heuristic is scaled by the cheapest cell in the grid, so paths stay
  optimal even with discounted roads.
- `Nav.SetGrid` and `Nav.ClearGrid` drop the costs along with the grid they belonged to.

```lua
-- Roads are fast, swamp is slow; the same call sites keep working.
for _, tile in ipairs(roadTiles)  do Nav.SetCost(tile.x, tile.y, 0.5) end
for _, tile in ipairs(swampTiles) do Nav.SetCost(tile.x, tile.y, 4.0) end
local path = Nav.FindPath(ax, ay, bx, by)     -- now prefers the road, avoids the swamp
```

#### Asynchronous paths

`Nav.FindPath` blocks until it finishes. That is fine for one agent and wrong for two hundred with individual
destinations, where a flow field does not apply. `Nav.FindPathAsync` hands the search to the engine's worker threads and
calls you back on the main thread when it lands, so the frame never stalls.

```lua
local id = Nav.FindPathAsync(startX, startY, endX, endY, function(path, ok, requestId)
    if not ok then return end                  -- unreachable, or no grid
    for _, p in ipairs(path) do
        -- p.x, p.y - same shape Nav.FindPath returns
    end
end)

Nav.CancelPathAsync(id)        -- the search still finishes, the callback is dropped
Nav.GetPendingPathCount()      -- searches in flight right now
```

- The search runs against an immutable **snapshot** of the navigation grid, taken when you call. Editing the grid with
  `Nav.SetWalkable` or `Nav.SetCost` while searches are in flight is safe — running searches keep the world they
  started with, and the next request picks up the new one.
- The snapshot is shared, not copied per request: a hundred requests in the same frame share one copy.
- Callbacks fire during the script update, one per completed request, so they can safely touch entities and Lua state.
- The callback receives `(path, ok, requestId)`. `path` is an empty table when `ok` is false.
- Everything is cleared on level change, so no callback outlives the scene that started it.

```lua
-- Two hundred units, each with its own destination, without a frame spike.
for _, unit in ipairs(units) do
    if unit.needsPath then
        unit.needsPath = false
        local from = GetEntityPosition(unit.id)
        Nav.FindPathAsync(from.x, from.y, unit.goalX, unit.goalY, function(path, ok)
            if ok then unit.path, unit.step = path, 1 end
        end)
    end
end
```

> Rule of thumb: **one shared destination → flow field**, **many individual destinations → `FindPathAsync`**,
> **one agent, right now → `FindPath`**.

> **`IsWalkable` vs `IsSolid`.** `IsWalkable` answers *"can a path go through this cell?"*
> and reflects the post-processed grid (AgentRadius expansion, side-view stand-on-floor filter).
> `IsSolid` answers *"is there a physical wall here?"* and uses the raw obstacle grid.
> In side-view this is critical: empty air between two platforms is **not** walkable but is
> also **not** solid, so sight and sound correctly pass through gaps.

### Entity-bound navigation (requires AIComponent)

```lua
-- AI speed
SetAIMoveSpeed(200)
local speed = GetAIMoveSpeed()

-- Detection and attack radii
SetAIDetectionRadius(300)
local dr = GetAIDetectionRadius()
SetAIAttackRadius(50)
local ar = GetAIAttackRadius()

-- Movement mode: 0 = Auto, 1 = Transform (kinematic), 2 = Physics (velocity-driven)
SetAIMovementMode(2)
local mode = GetAIMovementMode()

-- Physics arrival radius (accept distance when moving in Physics/Auto-physics mode)
SetAIPhysicsArrivalRadius(6.0)
local arrivalR = GetAIPhysicsArrivalRadius()

-- Navigate to point (A*)
local found = NavigateTo(targetX, targetY)
local found = NavigateTo(targetX, targetY, false)  -- no diagonal

-- Follow path (call every frame). true = path complete.
local done = FollowPath(dt)

-- Navigate + follow in one call
-- Returns: 0 = in progress, 1 = reached target, -1 = path not found
local status = NavigateAndFollow(targetX, targetY, dt)
local status = NavigateAndFollow(targetX, targetY, dt, true)  -- diagonal

-- Steer directly toward a point (no A*). Returns true once within arrival radius.
-- Optional speed (defaults to AIComponent MoveSpeed) and accept radius.
local arrived = MoveTowardPoint(targetX, targetY, dt)
local arrived = MoveTowardPoint(targetX, targetY, dt, 120, 6)

-- Side-scroller helper: steer only along X toward targetX (gravity handles Y).
local arrived = MoveTowardX(targetX, dt)
local arrived = MoveTowardX(targetX, dt, 120, 4)

-- Stop movement (zeroes physics velocity; preserves gravity in side-view).
StopMovement()

-- Set the AI facing direction used by facing-aware perception (see below).
SetAIFacing(1)    -- face +X (right)
SetAIFacing(-1)   -- face -X (left)

-- Path info
local path = GetCurrentPath()            -- table of points {x, y}
local next = GetNextPathPoint()          -- → {x, y} or nil
local dir = GetPathDirection()           -- → {x, y} normalized vector
local len = GetPathLength()              -- remaining path length
local remaining = GetRemainingPathPoints()
local has = HasPath()                    -- has active path?
local done = IsPathComplete()            -- path complete?

-- Path control
AdvancePath()   -- Move to next point
ClearPath()     -- Clear path

-- Additional utilities
local dist = GetDistanceTo(targetX, targetY)
local dist = GetDistanceToEntity(otherId)

-- Patrol points
AddPatrolPoint(100, 200)
local patrol = GetNextPatrolPoint()      -- → {x, y} or nil
AdvancePatrol()
local count = GetPatrolPointCount()
local idx = GetPatrolIndex()
SetPatrolIndex(0)
ClearPatrolPoints()

-- Agent separation
ApplySeparation(32.0, 1.0)
```

> **Physics-aware movement.** All of the movement functions above (`FollowPath`,
> `NavigateAndFollow`, `MoveTowardPoint`, `MoveTowardX`, `StopMovement`, `ApplySeparation`)
> respect the AIComponent's **Movement Mode**:
> - **Auto** (default) — if the entity has a dynamic/kinematic Rigidbody with a runtime body,
>   movement is driven through the physics body (velocity), otherwise the Transform is moved directly.
> - **Transform** — always move the Transform directly (legacy behavior; not for physics bodies).
> - **Physics** — always drive the Rigidbody velocity.
>
> When the entity stands on a **Side-View** nav grid, physics movement only sets horizontal (X)
> velocity and leaves gravity to control Y — exactly what a platformer character needs. On a
> **Top-Down** grid it sets full 2D velocity. This makes the built-in `MoveTo` behavior-tree task
> and the navigation follow-functions usable by physics-driven enemies without fighting Box2D.

### Distances

```lua
-- Distance from current entity to point
local dist = GetDistanceTo(worldX, worldY)

-- Distance to another entity
local dist = GetDistanceToEntity(entityId)
```

### Patrolling

```lua
-- Add patrol point
AddPatrolPoint(100, 200)
AddPatrolPoint(300, 200)
AddPatrolPoint(300, 400)

-- Next patrol point
local point = GetNextPatrolPoint()  -- → {x, y} or nil

-- Advance to next (loop)
AdvancePatrol()

-- Info
local count = GetPatrolPointCount()
local idx = GetPatrolIndex()
SetPatrolIndex(0)  -- Reset

-- Clear
ClearPatrolPoints()
```

### Separation

```lua
-- Push AI entities apart to avoid crowding
ApplySeparation()               -- radius=32, strength=1
ApplySeparation(50, 1.5)        -- radius, strength
```

### Example: chasing enemy

```lua
function OnConstruct()
    SetAIMoveSpeed(150)
    SetAIDetectionRadius(400)
end

function OnUpdate(dt)
    local player = FindEntityByTag("Player")
    if not player then return end

    local dist = GetDistanceToEntity(player)
    if dist < GetAIDetectionRadius() then
        local status = NavigateAndFollow(
            GetEntityPosition(player).x,
            GetEntityPosition(player).y, dt)

        if status == 0 then
            SetAnimBool("isWalking", true)
            local dir = GetPathDirection()
            if dir then SetFlipX(dir.x < 0) end
        end
    else
        ClearPath()
        SetAnimBool("isWalking", false)
    end

    ApplySeparation(40, 1.0)
end
```

---

## 33.5. Fog of War — Vision and Visibility

> **Type:** Global module `FogOfWar.*`

A grid-based **field-of-view / fog-of-war** system built on symmetric recursive shadowcasting —
the classic roguelike vision model (top-down dungeon crawlers). It tracks three
states per cell — **unseen**, **explored** (seen before, remembered, dimmed) and **visible**
(currently in view) — and serves both gameplay queries (stealth, line-of-sight, "can the monster
see the player?") and an animated fog overlay.

It integrates with the [NavGrid](#33-navigation--navigation-and-pathfinding): sight-blocking walls
can be pulled straight from the navigation solids via `SetOpacityFromNavGrid()`.

> The engine calls `FogOfWar.Reset()` automatically on every level load. You drive the per-frame
> visibility from your script (see the loop below).

#### State constants

| Constant | Value | Meaning |
|---|---|---|
| `FogOfWar.UNSEEN`   | 0 | never seen |
| `FogOfWar.EXPLORED` | 1 | seen before, not currently visible |
| `FogOfWar.VISIBLE`  | 2 | currently in view |

#### Setup

```lua
-- width, height = grid size in cells; cellSize = world units per cell;
-- originX, originY = world position of cell (0,0)'s corner (optional, default 0,0)
FogOfWar.Configure(64, 64, 32.0, 0.0, 0.0)
FogOfWar.SetEnabled(true)        -- enable the fog overlay
FogOfWar.IsEnabled()             -- bool — is the fog system currently on?

-- Mark which cells block sight (walls):
FogOfWar.SetOpacity(x, y, true)               -- a single cell
FogOfWar.GetOpacity(x, y)                     -- bool — does this cell block sight?
FogOfWar.SetOpacityRect(x, y, w, h, true)     -- a rectangle of cells
FogOfWar.FillOpacity(false)                   -- clear all opacity
FogOfWar.SetOpacityFromNavGrid()              -- pull walls from the NavGrid automatically

FogOfWar.IsConfigured()                       -- bool
FogOfWar.GetWidth(); FogOfWar.GetHeight(); FogOfWar.GetCellSize()
```

#### Per-frame visibility loop

```lua
function OnUpdate(dt)
    FogOfWar.BeginFrame()                       -- clear "currently visible" for this frame
    local p = GetPlayerWorldPos()
    FogOfWar.ComputeFOV(p.x, p.y, 256.0)        -- reveal around the player (radius in world units)

    -- Multiple viewers (party members, torches) — each call ADDS to the visible set:
    FogOfWar.AddViewer(torchX, torchY, 160.0)   -- AddViewer is an alias of ComputeFOV
end
```

#### Queries (gameplay)

```lua
-- By world position
FogOfWar.IsVisible(worldX, worldY)    -- currently in view?
FogOfWar.IsExplored(worldX, worldY)   -- ever seen?

-- By cell
FogOfWar.IsCellVisible(cx, cy)
FogOfWar.IsCellExplored(cx, cy)
FogOfWar.GetState(cx, cy)             -- FogOfWar.UNSEEN / EXPLORED / VISIBLE

-- Conversions
local c = FogOfWar.WorldToCell(worldX, worldY)   -- {x, y} cell index
local w = FogOfWar.CellToWorld(cx, cy)           -- {x, y} cell-center world position
```

> FOV queries work even when rendering is disabled — use the fog purely as a stealth / AI
> line-of-sight oracle if you like (just skip `SetEnabled(true)` or call `SetRenderEnabled(false)`).

#### Appearance

```lua
FogOfWar.SetFogColor(0, 0, 0)            -- RGB of the fog (default black)
FogOfWar.SetAlphas(1.0, 0.55, 0.0)       -- opacity for unseen / explored / visible
FogOfWar.SetFadeSpeed(12.0)              -- how fast cells fade between states
FogOfWar.SetSmoothFade(true)             -- false = instant pop, true = animated fade
FogOfWar.SetRevealExplored(true)         -- false = explored-but-not-visible cells go fully dark
                                         --         again (no map memory — blind / amnesia runs)
FogOfWar.SetRenderEnabled(true)          -- draw the overlay (FOV is still computed if false)
FogOfWar.IsRenderEnabled()               -- bool — is the overlay currently being drawn?
FogOfWar.SetRenderZ(5000.0)              -- draw order (above the world, below the UI)
```

#### Map control & persistence

```lua
FogOfWar.Reset()          -- clear visible + explored + fade (fresh level)
FogOfWar.ClearExplored()  -- forget the explored map, keep configuration
FogOfWar.RevealAll()      -- mark everything explored + visible (debug / full-map item)
FogOfWar.HideAll()        -- hide everything again

local blob = FogOfWar.SaveState()   -- serialize the explored map (compact, bit-packed)
FogOfWar.LoadState(blob)            -- restore on load (re-applies size / cell / origin)
```

---

## 34. AI — Artificial Intelligence

> **Type:** Entity-bound (Blackboard, BT) + Global (Perception, EQS).
> Requires **AIComponent**.
>
> AI system includes: Blackboard, Behavior Tree,
> AI Perception, EQS (Environment Query System).

### Blackboard

Blackboard is AI memory storage.

```lua
-- Bool
SetBlackboardBool("playerSeen", true)
local seen = GetBlackboardBool("playerSeen", false)  -- false = default

-- Int
SetBlackboardInt("alertLevel", 2)
local alert = GetBlackboardInt("alertLevel", 0)

-- Float
SetBlackboardFloat("lastSeenTime", 3.5)
local t = GetBlackboardFloat("lastSeenTime", 0.0)

-- String
SetBlackboardString("state", "patrol")
local state = GetBlackboardString("state", "idle")

-- Vec2
SetBlackboardVec2("lastKnownPos", 100, 200)
local pos = GetBlackboardVec2("lastKnownPos")  -- → {x, y} or nil

-- Entity ID
SetBlackboardEntity("target", playerId)
local target = GetBlackboardEntity("target")

-- Management
local has = HasBlackboardKey("playerSeen")
ClearBlackboardKey("playerSeen")
ClearBlackboard()  -- Clear all
```

### Perception

```lua
-- Enable/disable
SetPerceptionEnabled(true)
local enabled = IsPerceptionEnabled()

-- Sight config
SetSightConfig(400, 120)        -- radius, angle (degrees)
SetSightConfig(400, 120, true)  -- + line-of-sight check

-- Awareness radius: a 360° "sixth sense" ring around the observer. Targets
-- inside it are perceived even when outside the sight cone. Still capped by the
-- sight radius above; 0 (default) disables the ring.
SetPerceptionAwarenessRadius(80)

-- Forward-vector source: true = derive it from the AI facing set by
-- SetAIFacing(fx) (pure ±X), false (default) = entity rotation + sprite flip.
SetPerceptionUseFacingX(true)

-- Hearing config
SetHearingRadius(500)

-- Forget time
SetForgetTime(5.0)  -- forget target after 5 sec

-- Which tags to perceive
SetPerceptionTargetTags({"Player", "Ally"})

-- Check visibility of a specific entity
if CanSeeEntity(playerId) then ... end

-- Get all perceived actors
local actors = GetPerceivedActors()
for _, actor in ipairs(actors) do
    -- actor.entityId   = entity ID
    -- actor.x, actor.y = position
    -- actor.seen       = visible right now?
    -- actor.timeSinceLastSeen = time since last seen
    -- actor.strength   = signal strength
    -- actor.sense      = "sight" / "hearing" / "damage" / "custom"
end

-- Get highest priority target
local target = GetHighestPriorityTarget()
if target then
    Print("Target: " .. target.entityId .. " at " .. target.x .. ", " .. target.y)
end
```

> **Facing-aware sight (rotation + flip).** By default the sight cone follows the entity's
> transform rotation **and** its sprite flip (FlipX mirrors the cone), so a flip-facing enemy
> automatically looks the correct way with no extra code. For a facing that is decoupled from the
> sprite (an entity with no sprite, or AI that "looks" a different way than it is drawn), enable
> **Use Facing X** on the AIComponent and drive it with `SetAIFacing(1)` / `SetAIFacing(-1)`.
> Set an **Awareness Radius** (> 0) to detect targets in any direction within that radius (still
> subject to line-of-sight), e.g. an enemy noticing something right behind it. All of this applies
> identically to the perception update, `CanSeeEntity`, **and the in-viewport perception debug draw**.

### Global stimuli (`Perception.*`)

```lua
-- Report noise at a point
Perception.ReportNoise(x, y, 1.0)        -- loudness
Perception.ReportNoise(x, y, 1.0, 3.0)   -- + maxAge (sec)
Perception.ReportNoise(x, y, 1.0, 3.0, "Player")  -- + source tag

-- Report custom stimulus
Perception.ReportStimulusAt("sight", x, y)
Perception.ReportStimulusAt("hearing", x, y, 0.8, 5.0, "Enemy")

-- Stimulus from a specific entity
Perception.ReportStimulusFrom("damage", entityId)
Perception.ReportStimulusFrom("sight", entityId, 1.0, 5.0, "Player")
```

### EQS — Environment Query System (`EQS.*`)

EQS generates points around AI and scores them by criteria.

```lua
-- Register query
EQS.RegisterQuery("FindCover", {
    maxResults = 3,
    resultKey = "CoverPos",
    generator = {
        type = "grid",          -- "grid", "navigableGrid", "circle", "donut", "aroundEntity"
        halfExtent = 300,       -- half grid size
        spacing = 50,           -- point spacing
        radius = 200,           -- for circle
        points = 16,            -- number of points for circle
        innerRadius = 50,       -- for donut
        outerRadius = 200,      -- for donut
        donutPoints = 16,       -- number of points for donut
        entityKey = "Target"   -- Blackboard key for aroundEntity
    },
    tests = {
        {
            type = "distance",         -- "distance", "dot", "pathExists", "isWalkable", "visibility", "distanceToEntity", "custom"
            scoring = "inverseLinear", -- "linear", "inverseLinear", "constant", "curve"
            weight = 1.0,
            filter = false,            -- use as filter?
            filterType = "range",      -- "min", "max", "range"
            filterMin = 100,
            filterMax = 500,
            referenceKey = "Target",
            luaFunction = "MyCustomTest"
        },
        {
            type = "isWalkable",
            filter = true              -- filter out non-walkable points
        }
    }
})

-- Remove query
EQS.RemoveQuery("FindCover")

-- Run query (entity-bound)
local results = RunEQS("FindCover")
-- Override the key used to write the best result into Blackboard
local results = RunEQS("FindCover", "CoverPosOverride")
-- → table {x, y, score} sorted by score desc
-- Best result is auto-written to Blackboard by resultKey

for _, pt in ipairs(results) do
    Print("Point: " .. pt.x .. ", " .. pt.y .. " (score=" .. pt.score .. ")")
end
```

### Behavior Tree

```lua
-- Check if behavior tree is active
local active = IsBehaviorTreeActive()

-- Enable/disable
SetBehaviorTreeActive(true)
SetBehaviorTreeActive(false)

-- Reset tree (restart)
ResetBehaviorTree()
```

> Behavior trees are created in the `.ice_ai` visual editor and run automatically.
> Lua conditions and actions in nodes access the Blackboard via the functions above.

---

## 35. Joint — Physics Joints (Box2D)

> **Type:** Entity-bound. Requires **JointComponent** (read/set) or **RigidbodyComponent** (create/destroy).
> Joints connect two physics bodies. Types: Revolute,
> Distance, Prismatic, Wheel, Weld, Motor.

### Basic properties

```lua
-- Joint count
local count = GetJointCount()

-- Joint type (int). -1 = none
local type = GetJointType()
local type = GetJointType(1)  -- Second joint

-- Name
local name = GetJointName()

-- Position
SetJointPosition(10, 5)
local pos = GetJointPosition()  -- → {x, y}

-- Local scale
SetJointLocalScale(1.5, 1.5)
local scale = GetJointLocalScale()  -- → {x, y}

-- Local rotation (degrees)
SetJointLocalRotation(45)
local rot = GetJointLocalRotation()  -- → float

-- World transform (entity transform already applied — see the Sprite section).
-- Note: the local position accessors are named SetJointPosition/GetJointPosition.
SetJointWorldPosition(120, 64, 0)
local jwp = GetJointWorldPosition(0)  -- → {x, y, z}
SetJointWorldRotation(30, 0)
local jwr = GetJointWorldRotation(0)  -- → number
local jws = GetJointWorldScale(0)     -- → {x, y}, read-only
```

### Motor

```lua
-- Enable/disable motor
SetJointMotorEnabled(true)
local enabled = IsJointMotorEnabled()

-- Motor speed
SetJointMotorSpeed(5.0)
local speed = GetJointMotorSpeed()

-- Max motor torque (Revolute)
SetJointMaxMotorTorque(100)
local torque = GetJointMaxMotorTorque()

-- Max motor force (Prismatic)
SetJointMaxMotorForce(200)
local force = GetJointMaxMotorForce()
```

### Limits

```lua
-- Enable/disable limits
SetJointLimitsEnabled(true)
local enabled = AreJointLimitsEnabled()

-- Set/get limits
SetJointLimits(-45, 45)            -- lower, upper (degrees for Revolute)
local limits = GetJointLimits()    -- → {lower, upper}
```

### Spring

```lua
-- Enable/disable spring
SetJointSpringEnabled(true)
local enabled = IsJointSpringEnabled()

-- Frequency (Hz)
SetJointSpringHertz(4.0)
local hz = GetJointSpringHertz()

-- Damping
SetJointSpringDamping(0.7)
local damp = GetJointSpringDamping()
```

### Anchors

```lua
-- Anchors are attachment points on both bodies
SetJointAnchorA(10, 5)
SetJointAnchorB(-10, 0)
local a = GetJointAnchorA()  -- → {x, y}
local b = GetJointAnchorB()  -- → {x, y}
```

### Other

```lua
-- Target entity
local targetUUID = GetJointTargetEntity()
local targetTag = GetJointTargetTag()

-- Strength (for breakable joints)
SetJointBreakForce(500)
SetJointBreakTorque(200)
local breakForce = GetJointBreakForce()
local breakTorque = GetJointBreakTorque()

-- All functions accept an optional index for multiple joints:
SetJointMotorSpeed(5.0, 1)  -- Second joint
```

### Motor Joint type parameters

> Functions for controlling **Motor** joint type properties.
> Motor joints smoothly drive one body toward a target position/angle.

```lua
-- Linear offset (target position, pixels)
SetMotorJointLinearOffset(10, 5)
local offset = GetMotorJointLinearOffset()  -- → {x, y}

-- Angular offset (target angle, degrees)
SetMotorJointAngularOffset(45)
local angle = GetMotorJointAngularOffset()

-- Max force (pixels)
SetMotorJointMaxForce(100)
local force = GetMotorJointMaxForce()

-- Max torque
SetMotorJointMaxTorque(50)
local torque = GetMotorJointMaxTorque()

-- Correction factor (0..1, how fast to converge)
SetMotorJointCorrectionFactor(0.3)
local factor = GetMotorJointCorrectionFactor()

-- All functions accept an optional joint index:
SetMotorJointLinearOffset(10, 5, 1)  -- Second joint
```

### Runtime joint creation (by tag)

> These functions are bound to the **current entity** (require RigidbodyComponent).
> The target entity is found by its **Tag**. If multiple entities share the same tag, the first one found is used and a warning is logged — prefer unique tags or the `*ToEntity` variants.
> Successfully created joints also remember the target's UUID, so they survive tag renames.
> All return the joint index (`int`) or `-1` on failure (a warning is logged with the reason).

```lua
-- Revolute (hinge) — rotation around a point
local idx = CreateRevoluteJoint("TargetTag", anchorAx, anchorAy, anchorBx, anchorBy)

-- Distance (rope/rod) — maintains distance between two anchors
local idx = CreateDistanceJoint("TargetTag", anchorAx, anchorAy, anchorBx, anchorBy)

-- Weld (rigid) — glues two bodies together
local idx = CreateWeldJoint("TargetTag", anchorAx, anchorAy, anchorBx, anchorBy)

-- Prismatic (slider) — movement along an axis (elevators, doors)
local idx = CreatePrismaticJoint("TargetTag", axisX, axisY, anchorAx, anchorAy, anchorBx, anchorBy)

-- Wheel (suspension) — wheel with spring along an axis (vehicles)
local idx = CreateWheelJoint("TargetTag", axisX, axisY, anchorAx, anchorAy, anchorBx, anchorBy)

-- Motor — smoothly drives one body toward another
local idx = CreateMotorJoint("TargetTag", maxForce, maxTorque, correctionFactor)
```

### Runtime joint creation (by entity ID)

> Same as above, but the target is specified by **entity ID** instead of tag.
> This is essential when multiple entities share the same tag (e.g. ragdoll limbs, spawned parts).

```lua
-- Revolute (hinge)
local idx = CreateRevoluteJointToEntity(entityId, anchorAx, anchorAy, anchorBx, anchorBy)

-- Distance (rope/rod)
local idx = CreateDistanceJointToEntity(entityId, anchorAx, anchorAy, anchorBx, anchorBy)

-- Weld (rigid)
local idx = CreateWeldJointToEntity(entityId, anchorAx, anchorAy, anchorBx, anchorBy)

-- Prismatic (slider) — movement along an axis (elevators, doors)
-- axisX, axisY = direction of allowed movement (normalized automatically)
local idx = CreatePrismaticJointToEntity(entityId, axisX, axisY, anchorAx, anchorAy, anchorBx, anchorBy)

-- Wheel (suspension) — wheel with spring along an axis (vehicles)
local idx = CreateWheelJointToEntity(entityId, axisX, axisY, anchorAx, anchorAy, anchorBx, anchorBy)

-- Motor — smoothly drives one body toward another
local idx = CreateMotorJointToEntity(entityId, maxForce, maxTorque, correctionFactor)
```

**Anchors** are in pixels, relative to each body's center. All optional (default `0, 0`).

### Runtime joint creation example

```lua
function OnBeginPlay()
    -- Spawn ragdoll parts
    local torso = SpawnEntity("Classes/Torso.json", 100, 100)
    local head  = SpawnEntity("Classes/Head.json",  100, 80)
    local armL  = SpawnEntity("Classes/Arm.json",   85, 100)
    local armR  = SpawnEntity("Classes/Arm.json",  115, 100)

    -- Connect with revolute joints (by entity ID — safe with duplicate tags)
    local neck = CreateRevoluteJointToEntity(head, 0, 10, 0, -8)
    EnableJointLimit(neck, true)
    SetJointLimits(-30, 30, neck)

    local shoulderL = CreateRevoluteJointToEntity(armL, -12, -5, 0, -10)
    EnableJointLimit(shoulderL, true)
    SetJointLimits(-90, 90, shoulderL)
end
```

### Runtime joint destruction and queries

> `DestroyJoint`/`DestroyAllJoints` sever the physical connection but keep the instance slot,
> so joint indices never shift. A severed joint can be brought back with `RecreateJoint`.
> Use `RemoveJointInstance` to delete the slot entirely (indices of later joints shift down by one).

```lua
-- Sever a specific joint by index (slot stays, index stable)
DestroyJoint(0)

-- Sever all joints on this entity
DestroyAllJoints()

-- Re-create a severed/broken joint from its stored settings → true on success
local ok = RecreateJoint(0)

-- Delete the joint slot entirely (shifts later indices)
RemoveJointInstance(0)

-- Check if joint is still valid (not severed/broken)
local valid = IsJointValid(0)

-- Get constraint forces acting on a joint → {x, y} (pixels)
local force = GetJointReactionForce(0)

-- Get constraint torque acting on a joint
local torque = GetJointReactionTorque(0)
```

### Runtime joint parameter control (by index)

> Value-setters take the **value first** and an optional joint index **last** (defaults to 0).
> Enable-functions take the **index first**. All setters wake the connected bodies automatically.

```lua
-- Motor
EnableJointMotor(0, true)            -- (jointIndex, enabled)
SetJointMotorSpeed(90.0, 0)          -- (speed [, jointIndex]) — degrees/sec (Revolute, Wheel) or px/sec (Prismatic, Distance)
SetJointMaxMotorTorque(50, 0)        -- (torque [, jointIndex]) — Revolute, Wheel
SetJointMaxMotorForce(200, 0)        -- (force [, jointIndex]) — Prismatic, Distance

-- Limits
EnableJointLimit(0, true)            -- (jointIndex, enabled)
SetJointLimits(-45, 45, 0)           -- (lower, upper [, jointIndex]) — degrees (Revolute) or px (Prismatic, Wheel, Distance)

-- Spring
EnableJointSpring(0, true)           -- (jointIndex, enabled)
SetJointSpringHertz(4.0, 0)          -- (hertz [, jointIndex])
SetJointSpringDamping(0.7, 0)        -- (damping [, jointIndex])
SetJointSpringDampingRatio(0, 0.7)   -- (jointIndex, damping) — legacy alias of SetJointSpringDamping
```

### Joint break event

> When a joint with `BreakForce`/`BreakTorque` exceeds its threshold, the engine destroys the
> runtime joint, keeps the instance in the list (so joint indices never shift) and calls
> `OnJointBreak` on the owner entity's script. `IsJointValid(index)` returns `false` for broken joints.

```lua
function OnJointBreak(jointIndex, jointName, targetTag)
    PlaySound("snap.wav")
    Print("Joint " .. jointName .. " (#" .. jointIndex .. " -> " .. targetTag .. ") broke!")
end
```

### Physics Parts — multi-body classes (Separate Body)

> A collider instance with **Separate Body** enabled is simulated as its **own dynamic body**
> ("part") instead of a shape on the entity body. This lets a single class contain a whole
> physical machine: chassis + wheels, a catapult arm, etc.
>
> * The part inherits GravityScale/Damping/Bullet/Sleep from the entity's Rigidbody.
> * A joint instance with **Target Part** set connects the entity body to that part
>   (Target Part overrides Target Entity Tag).
> * A sprite or flipbook instance with **Attach To Collider** set follows the part's position and rotation.
> * If an attached sprite/flipbook has a collision polygon, its polygon shapes are created **on the part's body**
>   and move with it (keep the attached instance un-rotated relative to the part for exact polygon placement).
> * Contact/sensor/hit events from part shapes are reported for the owner entity with the collider's name.
> * Collision groups and `CollideConnected` work for parts exactly like for regular colliders.
> * Flipping the entity (`SetEntityFlipX`) mirrors everything physically: part bodies, their velocities,
>   joint anchors, axes, angular limits, reference angles and authored motor speeds — a catapult flips
>   left/right perfectly. Note: motor speeds set at runtime via `SetJointMotorSpeed` stay in world
>   convention (positive = clockwise); multiply by the facing sign yourself if you want "forward" semantics.
> * Teleports (`SetPosition`, `SetEntityPosition`, network snaps) move all parts together with the body.

Car in a single class — components:

```text
Rigidbody (Dynamic)                        — the chassis body
Collider: Box "Body"                       — chassis shape
Collider: Sphere "WheelL"  [Separate Body] — left wheel (circle, at (-45, -20), Friction ~0.9)
Collider: Sphere "WheelR"  [Separate Body] — right wheel (circle, at (45, -20), Friction ~0.9)
Sprite "Chassis"                           — car body sprite
Sprite "SpriteL" [Attach To Collider: WheelL] — wheel sprite, follows the part
Sprite "SpriteR" [Attach To Collider: WheelR] — wheel sprite, follows the part
Joint 0: Wheel, Target Part = WheelL, AnchorA = (-45, -20), Axis = (0, 1),
         Spring (Hertz 4, Damping 0.7), Motor ON, MaxMotorTorque 100
Joint 1: Wheel, Target Part = WheelR, AnchorA = (45, -20), Axis = (0, 1),
         Spring (Hertz 4, Damping 0.7), Motor ON, MaxMotorTorque 100
```

Car driving script (A/D):

```lua
local SPEED = 720

function OnUpdate(dt)
    local axis = GetAxis("A", "D")
    SetJointMotorSpeed(axis * SPEED, 0)
    SetJointMotorSpeed(axis * SPEED, 1)
end
```

> `AnchorA` is the wheel mount point in pixels relative to the chassis center; `AnchorB` defaults
> to `(0, 0)` — the part's center. Positive motor speed spins clockwise, driving the car to the right.
> Wheels don't collide with the chassis while `Collide Connected` is off (default).

---

## 36. PointMarker — Point Markers

> **Type:** Entity-bound. Requires **PointMarkerComponent**.
> Markers are helper points on an entity (spawn points, weapon positions, etc.).

```lua
-- Marker count
local count = GetPointMarkerCount()

-- Name
local name = GetPointMarkerName()
local name = GetPointMarkerName(1)  -- Second marker

-- Position (local)
SetPointMarkerPosition(10, 5)
local pos = GetPointMarkerPosition()  -- → {x, y}

-- Scale
SetPointMarkerScale(2, 2)
local scale = GetPointMarkerScale()  -- → {x, y}

-- Rotation
SetPointMarkerRotation(45)
local rot = GetPointMarkerRotation()

-- World transform (entity transform already applied — see the Sprite section).
-- Note: the local accessors are named SetPointMarkerPosition/Rotation/Scale.
SetPointMarkerWorldPosition(120, 64, 0)
local mwp = GetPointMarkerWorldPosition(0)  -- → {x, y, z}
SetPointMarkerWorldRotation(30, 0)
local mwr = GetPointMarkerWorldRotation(0)  -- → number
local mws = GetPointMarkerWorldScale(0)     -- → {x, y}, read-only

-- Visibility
SetPointMarkerVisible(true)
local vis = IsPointMarkerVisible()

-- Color (RGB, for editor visualization)
SetPointMarkerColor(1, 0, 0)
local c = GetPointMarkerColor()  -- → {r, g, b}

-- Size (visual)
SetPointMarkerSize(48)
local size = GetPointMarkerSize()

-- Render in game (usually false — editor-only)
SetPointMarkerRenderInGame(true)
local render = GetPointMarkerRenderInGame()

-- Shape: 0 = Arrow, 1 = Line, 2 = Circle, 3 = Square
SetPointMarkerShape(0)
local shape = GetPointMarkerShape()

-- Line / border thickness
SetPointMarkerThickness(2)
local th = GetPointMarkerThickness()

-- Arrow head size (Arrow shape)
SetPointMarkerArrowHeadSize(10)
local ah = GetPointMarkerArrowHeadSize()

-- Arrow direction in degrees (Arrow shape)
SetPointMarkerArrowDirection(90)
local ad = GetPointMarkerArrowDirection()

-- Line end offset (Line shape) — endpoint relative to the marker
SetPointMarkerLineEndOffset(50, 0)
local off = GetPointMarkerLineEndOffset()  -- → {x, y}

-- All functions accept an optional index:
SetPointMarkerPosition(10, 5, 1)  -- Second marker
```

---

## 37. DataUtils — Data Structures and Utilities

> **Type:** Global functions.
>
> Utilities for data handling: arrays (Array), maps (Map), sets (Set),
> enums (Enum), structs (Struct), data tables (DataTable).

### Enum

```lua
-- Create enum from strings
local Direction = Enum("Up", "Down", "Left", "Right")
-- Direction.Up = 0, Direction.Down = 1, Direction.Left = 2, Direction.Right = 3

-- Methods
local name = Direction.Name(0)          -- "Up"
local valid = Direction.IsValid(5)      -- false
local count = Direction.Count()         -- 4
local values = Direction.Values()       -- {0, 1, 2, 3}
local names = Direction.Names()         -- {"Up", "Down", "Left", "Right"}

-- Usage
local dir = Direction.Left
if dir == Direction.Left then
    Print("Direction: " .. Direction.Name(dir))
end
```

### Array

```lua
local arr = {10, 20, 30, 40, 50}

-- Add/remove
Array.Push(arr, 60)                  -- Add to end
local last = Array.Pop(arr)         -- Remove and return last
Array.InsertAt(arr, 2, 15)          -- Insert at index
Array.Remove(arr, 3)                -- Remove by index
Array.RemoveValue(arr, 30)          -- Remove by value
Array.Clear(arr)                    -- Clear

-- Search
local has = Array.Contains(arr, 20)  -- true
local idx = Array.Find(arr, 20)      -- Key
local idx = Array.IndexOf(arr, 20)   -- Index (1-based)
local idx = Array.LastIndexOf(arr, 20)

-- Info
local len = Array.Length(arr)
local count = Array.Count(arr)
local count = Array.Count(arr, function(v) return v > 25 end)

-- Math
local sum = Array.Sum(arr)
local min = Array.Min(arr)
local max = Array.Max(arr)
local avg = Array.Average(arr)

-- Transformations
local filtered = Array.Filter(arr, function(v) return v > 25 end)
local mapped = Array.Map(arr, function(v) return v * 2 end)
local reduced = Array.Reduce(arr, function(acc, v) return acc + v end, 0)
Array.ForEach(arr, function(v, i) Print(i .. ": " .. v) end)

-- Checks
local any = Array.Any(arr, function(v) return v > 40 end)   -- true
local all = Array.All(arr, function(v) return v > 0 end)    -- true

-- Selection
local first = Array.First(arr)
local first = Array.First(arr, function(v) return v > 25 end)
local last = Array.Last(arr)
local taken = Array.Take(arr, 3)        -- First 3
local skipped = Array.Skip(arr, 2)      -- Skip first 2
local slice = Array.Slice(arr, 2, 4)    -- From 2 to 4

-- Combining
local joined = Array.Join(arr, ", ")     -- "10, 20, 30"
local zipped = Array.Zip(arr1, arr2)     -- {{a1,b1}, {a2,b2}, ...}
local groups = Array.GroupBy(arr, function(v) return v > 25 end)

-- Conversions
Array.Sort(arr)                          -- Sort (numbers/strings)
Array.Sort(arr, function(a, b) return a > b end)  -- Custom
Array.Reverse(arr)
Array.Shuffle(arr)
local unique = Array.Unique(arr)          -- Unique
local flat = Array.Flatten({{1,2},{3,4}}) -- → {1,2,3,4}
```

### Map

```lua
local data = { name = "Hero", hp = 100, level = 5 }

-- Basic operations
Map.Set(data, "mana", 50)
local val = Map.Get(data, "hp", 0)       -- 100 (or 0 default)
Map.Remove(data, "mana")
Map.Clear(data)

-- Info
local keys = Map.Keys(data)              -- {"name", "hp", "level"}
local values = Map.Values(data)          -- {"Hero", 100, 5}
local has = Map.HasKey(data, "hp")       -- true
local count = Map.Count(data)            -- 3
local entries = Map.Entries(data)        -- {{key="name", value="Hero"}, ...}

-- Copy
local copy = Map.Copy(data)             -- Shallow copy
local deep = Map.DeepCopy(data)         -- Deep copy
local merged = Map.Merge(base, overrides) -- Merge

-- Transformations
local filtered = Map.Filter(data, function(k, v) return type(v) == "number" end)
local mapped = Map.MapValues(data, function(k, v) return v * 2 end)
local inverted = Map.Invert(data)       -- Keys ↔ Values
Map.ForEach(data, function(k, v) Print(k .. "=" .. tostring(v)) end)

-- Checks
local any = Map.Any(data, function(k, v) return v > 50 end)
local all = Map.All(data, function(k, v) return v ~= nil end)
```

### Set

```lua
-- Create set
local s = Set({"a", "b", "c"})
local s = Set()  -- Empty

-- Operations
s.Add("d")
s.Remove("a")
local has = s.Has("b")           -- true
local count = s.Count()          -- 3
s.Clear()

-- Convert
local arr = s.ToArray()          -- {"b", "c", "d"}

-- Iterate
s.ForEach(function(value)
    Print(value)
end)

-- Set operations (return arrays)
local union = s.Union(otherSet)          -- Union
local intersect = s.Intersect(otherSet)  -- Intersection

-- Difference and comparison
local diff = s.Difference(otherSet)      -- Elements not in otherSet
local subset = s.IsSubsetOf(otherSet)
local equals = s.Equals(otherSet)

-- Filtering and checks
local filtered = s.Filter(function(v) return v ~= "a" end)
local any = s.Any(function(v) return v == "b" end)
local all = s.All(function(v) return v ~= "x" end)
```

### Struct — struct factory

```lua
-- Define a “struct” with default fields
local Vector = Struct({ x = 0, y = 0 })

-- Create instance
local v1 = Vector()                     -- {x=0, y=0}
local v2 = Vector({ x = 10, y = 20 })  -- {x=10, y=20}
local v3 = Vector({ x = 5 })           -- {x=5, y=0}
```

### DataTable

```lua
-- Create data table (like a mini database)
local items = DataTable({
    { id = 1, name = "Sword",  damage = 10, type = "weapon" },
    { id = 2, name = "Shield", armor = 5,   type = "armor" },
    { id = 3, name = "Potion", heal = 50,   type = "consumable" },
    { id = 4, name = "Axe",    damage = 15, type = "weapon" }
})

-- Methods
local count = items.Count()                         -- 4
local row = items.GetRow(2)                         -- {id=2, name="Shield", ...}
local sword = items.FindByField("name", "Sword")   -- First row with name="Sword"
local weapons = items.FindAllByField("type", "weapon")  -- All with type="weapon"
local expensive = items.Where(function(row) return (row.damage or 0) > 12 end)
local names = items.Column("name")                  -- {"Sword","Shield","Potion","Axe"}
```

### Type converters

```lua
-- Basic types
local i = ToInt(3.7)        -- → 3
local i = ToInt("42")       -- → 42
local i = ToInt(true)       -- → 1

local f = ToFloat(42)       -- → 42.0
local f = ToFloat("3.14")   -- → 3.14

local s = ToString(42)      -- → "42"
local s = ToString(true)    -- → "true"
local s = ToString(nil)     -- → "nil"

local b = ToBool(1)         -- → true
local b = ToBool(0)         -- → false
local b = ToBool("")        -- → false
local b = ToBool("hello")   -- → true

-- Determine type
local t = TypeOf(42)        -- → "number"
local t = TypeOf("hi")      -- → "string"
local t = TypeOf(true)      -- → "bool"
local t = TypeOf({})        -- → "table"
local t = TypeOf(nil)       -- → "nil"
```

### Vector converters

```lua
-- From numbers
local v = ToVec2(10, 20)            -- → Vec2(10, 20)
local v = ToVec2(5)                 -- → Vec2(5, 5)

-- From table
local v = ToVec2({x = 10, y = 20})  -- → Vec2(10, 20)
local v = ToVec2({10, 20})          -- → Vec2(10, 20)

-- From Vec3 (drop z)
local v = ToVec2(someVec3)          -- → Vec2(x, y)

-- ToVec3
local v = ToVec3(10, 20, 30)
local v = ToVec3({x=1, y=2, z=3})
local v = ToVec3(someVec2)          -- → Vec3(x, y, 0)

-- ToVec4
local v = ToVec4(1, 0, 0, 1)
local v = ToVec4({r=1, g=0, b=0, a=1})  -- Table with r/g/b/a
local v = ToVec4(someVec3)               -- → Vec4(x, y, z, 1)

-- ToColor
local c = ToColor(1, 0, 0, 1)           -- → Color(r=1, g=0, b=0, a=1)
local c = ToColor({r=1, g=0.5, b=0})    -- From table
local c = ToColor(someVec4)             -- From Vec4

-- Any userdata → table
local t = ToTable(someVec2)  -- → {x=..., y=...}
local t = ToTable(someVec3)  -- → {x=..., y=..., z=...}
local t = ToTable(someColor) -- → {r=..., g=..., b=..., a=...}
local t = ToTable(someTransform) -- → {position={x,y,z}, rotation=..., scale={x,y}}
local t = ToTable(someRect)      -- → {x=..., y=..., w=..., h=...}
```

### Select and Coalesce

```lua
-- Select — ternary / Branch analog
local speed = Select(isSprinting, 400, 200)
-- If isSprinting == true → 400, else → 200

-- Coalesce — return first non-nil arg
local name = Coalesce(customName, defaultName, "Unknown")
-- Returns customName, or defaultName if nil, or "Unknown"
```

### String utilities

> **Type:** Global functions (namespace `String`)
>
> Extended string library (from `DataUtilsLua`). Includes everything `Str.*` has plus additional functions: `TrimLeft`, `TrimRight`, `IsEmpty`, `IsBlank`, `ReplaceFirst`, `Reverse`, `Find`, `Count`, `CharAt`, `ToNumber`, `Byte`, `Char`, `Join`. For a quick-reference lightweight variant, see [`Str.*` in Section 1](#str--string-utilities).
>
> ⚠️ **Bytes vs characters.** `Length`, `Sub`, `CharAt`, `Reverse`, `PadLeft`, `PadRight`, `Upper`
> and `Lower` work on **bytes** and are safe for ASCII only. On Russian, Ukrainian, Arabic,
> Hebrew, Hindi, Japanese or Chinese text they will count wrong and can cut a character in half.
> For anything the player can see, use the `String.Utf8*` functions below (or the `utf8`
> standard library).

```lua
-- Split string
local parts = String.Split("a,b,c", ",")  -- → {"a", "b", "c"}
local chars = String.Split("hello", "")    -- → {"h", "e", "l", "l", "o"}

-- Trim spaces
local s = String.Trim("  hello  ")       -- → "hello"
local s = String.TrimLeft("  hello  ")   -- → "hello  "
local s = String.TrimRight("  hello  ")  -- → "  hello"

-- Checks
String.StartsWith("hello world", "hello")  -- → true
String.EndsWith("hello world", "world")    -- → true
String.Contains("hello world", "lo wo")    -- → true
String.IsEmpty("")                         -- → true
String.IsBlank("   ")                      -- → true

-- Replace
String.Replace("aabbcc", "bb", "XX")          -- → "aaXXcc"
String.ReplaceFirst("aabbaabb", "bb", "XX")   -- → "aaXXaabb"

-- Case
String.Upper("hello")  -- → "HELLO"
String.Lower("HELLO")  -- → "hello"

-- Padding
String.PadLeft("42", 5, "0")   -- → "00042"
String.PadRight("hi", 10, ".")  -- → "hi........"

-- Repeat and reverse
String.Repeat("ab", 3)   -- → "ababab"
String.Reverse("hello")  -- → "olleh"

-- Substring (1-based)
String.Sub("hello", 2, 4)  -- → "ell"
String.Length("hello")      -- → 5
String.CharAt("hello", 1)  -- → "h"

-- Search
String.Find("hello world", "world")  -- → 7 (1-based, or -1 if not found)
String.Count("abcabc", "abc")        -- → 2

-- Conversion
local n = String.ToNumber("42.5")  -- → 42.5 (or nil)
local code = String.Byte("A")      -- → 65
local ch = String.Char(65)          -- → "A"

-- Join array into string
String.Join({"a", "b", "c"}, ", ")  -- → "a, b, c"
```

#### UTF-8 aware variants

Same idea as the functions above, but they count **characters (codepoints)**, not bytes — use
these for any text the player sees.

```lua
local text = "Привет"

String.Utf8Length(text)          -- → 6   (String.Length gives 12)
String.Utf8Sub(text, 1, 3)       -- → "При"
String.Utf8Sub(text, -3)         -- → "вет"  (negative indices count from the end)
String.Utf8CharAt(text, 2)       -- → "р"
String.Utf8Reverse(text)         -- → "тевирП"
String.Utf8IsValid(text)         -- → true

-- Cut a name to fit a UI field; the suffix defaults to "..."
String.Utf8Truncate("Длинное имя игрока", 10)        -- → "Длинное..."
String.Utf8Truncate("Длинное имя игрока", 10, "…")   -- → "Длинное и…"

-- Codepoints back and forth
local points = String.Utf8Codepoints("ok")       -- → { 111, 107 }
local back = String.Utf8FromCodepoints(points)   -- → "ok"
```

| Function | Returns |
| -------- | ------- |
| `String.Utf8Length(s)` | Number of characters. |
| `String.Utf8Sub(s, from [, to])` | Substring by character index, 1-based; negative indices count from the end; `to` defaults to `-1`. |
| `String.Utf8CharAt(s, index)` | One character at a 1-based index (`""` when out of range). |
| `String.Utf8Reverse(s)` | The string reversed by characters. |
| `String.Utf8Truncate(s, maxChars [, suffix])` | `s` shortened to `maxChars` characters **including** the suffix (default `"..."`); returns `s` unchanged when it already fits. |
| `String.Utf8IsValid(s)` | `false` if the bytes are not valid UTF-8. |
| `String.Utf8Codepoints(s)` | Array of codepoint numbers. |
| `String.Utf8FromCodepoints(t)` | Builds a string from an array of codepoints. |

### Flow Control — execution flow utilities

> **Type:** Global functions
>
> Lua objects for controlling logic flow (Gate, MultiGate, etc.).
> **These are standard Lua objects**, not visual nodes.

#### Gate

```lua
local gate = Gate()          -- Create closed
local gate = Gate(true)      -- Create open

gate.Open()
gate.Close()
gate.Toggle()

-- Execute calls callback only if gate is open
gate.Execute(function()
    Print("Gate is open!")
end)

local isOpen = gate.IsOpen()
```

#### MultiGate — sequentially calls multiple functions

```lua
local mg = MultiGate({
    function(idx) Print("Output 1") end,
    function(idx) Print("Output 2") end,
    function(idx) Print("Output 3") end
})

-- Each Execute runs next output: 1 → 2 → 3 → 1 → ...
mg.Execute()
mg.Execute()
mg.Execute()

-- With parameters:
local mg = MultiGate({fn1, fn2, fn3}, true)   -- random = true
local mg = MultiGate({fn1, fn2, fn3}, false, false)  -- loop = false (stops after 3rd)

mg.Reset()         -- Reset index
mg.GetIndex()      -- Current index
```

#### DoOnce — execute only once

```lua
local once = DoOnce()

-- Executes only the first time
once.Execute(function()
    Print("This will show once!")
end)

-- Subsequent calls do nothing
once.Execute(function() Print("Won't run") end)

once.Reset()   -- Reset (can run again)
once.IsDone()  -- → true/false
```

#### DoN — execute N times

```lua
local fiveShots = DoN(5)

function OnUpdate(dt)
    if IsKeyJustPressed("space") then
        fiveShots.Execute(function(count)
            Print("Shot #" .. count)
            SpawnProjectile()
        end)
    end
end

fiveShots.GetCount()      -- How many executed
fiveShots.GetRemaining()  -- How many left
fiveShots.IsDone()        -- All N used?
fiveShots.Reset()         -- Reset count
```

#### FlipFlop — alternate between two paths

```lua
local ff = FlipFlop()

function OnUpdate(dt)
    if IsKeyJustPressed("e") then
        ff.Execute(
            function() Print("Path A — OPEN") end,
            function() Print("Path B — CLOSED") end
        )
        -- First call → A, second → B, third → A, ...
    end
end

ff.IsA()    -- true if next call is A
ff.Reset()  -- Reset to A
```

---

## 38. Material — Materials and Shaders

> **Type:** Entity-bound (sprite/flipbook/tilemap props) + Global (`Material.*`, `MPC.*`).
>
> Materials control rendering: shading mode (Lit/Unlit), blend mode (Masked/Additive/Translucent/Opaque),
> alpha clip, custom shaders, textures.
> Created in `.ice_mat` and assigned at runtime.

### Sprite materials (Entity-bound)

```lua
-- Assign material from file (.ice_mat or .ice_matinst)
SetSpriteMaterial("Content/Materials/Glow.ice_mat")
SetSpriteMaterial("Content/Materials/Custom.ice_matinst")  -- Instance
local mat = GetSpriteMaterial()          -- Material name (or "")
ClearSpriteMaterial()                    -- Reset to default

-- Shading mode: "Lit" or "Unlit"
SetSpriteShadingMode("Unlit")
local mode = GetSpriteShadingMode()

-- Blend mode: "Masked", "Additive", "Translucent", "Opaque"
SetSpriteBlendMode("Additive")
local blend = GetSpriteBlendMode()

-- Alpha clip (threshold 0.0 – 1.0)
SetSpriteAlphaClip(0.3)
local clip = GetSpriteAlphaClip()

-- All functions accept optional index for multiple sprites:
SetSpriteShadingMode("Unlit", 1)         -- Second sprite (index 0 by default)
SetSpriteBlendMode("Additive", 1)
SetSpriteAlphaClip(0.5, 1)
SetSpriteMaterial("Content/Materials/Glow.ice_mat", 1)
local mat1 = GetSpriteMaterial(1)
ClearSpriteMaterial(1)
```

### Flipbook materials (Entity-bound)

```lua
-- Assign material (.ice_mat or .ice_matinst)
SetFlipbookMaterial("Content/Materials/Fire.ice_mat")
local mat = GetFlipbookMaterial()
ClearFlipbookMaterial()

-- Shading mode
SetFlipbookShadingMode("Lit")
local mode = GetFlipbookShadingMode()

-- Blend mode
SetFlipbookBlendMode("Translucent")
local blend = GetFlipbookBlendMode()

-- Alpha clip
SetFlipbookAlphaClip(0.5)
local clip = GetFlipbookAlphaClip()

-- Optional index (like sprites)
SetFlipbookMaterial("Content/Materials/Fire.ice_mat", 1)
SetFlipbookShadingMode("Unlit", 1)
```

### Tilemap materials (Entity-bound)

```lua
-- Assign tilemap material (.ice_mat or .ice_matinst)
SetTilemapMaterial("Content/Materials/Water.ice_mat")
local mat = GetTilemapMaterial()
ClearTilemapMaterial()

-- Properties (require material assigned)
SetTilemapShadingMode("Unlit")
local mode = GetTilemapShadingMode()

SetTilemapBlendMode("Additive")
local blend = GetTilemapBlendMode()

SetTilemapAlphaClip(0.5)
local clip = GetTilemapAlphaClip()

-- Optional index
SetTilemapMaterial("Content/Materials/Lava.ice_mat", 1)
```

> **Note:** When assigning a material with `SetSpriteMaterial` / `SetFlipbookMaterial` / `SetTilemapMaterial`,
> shading mode, blend mode, and alpha clip are automatically applied from the material file.
> Subsequent calls to `SetSpriteShadingMode`, etc. override these values.

### Dynamic materials (`Material.*`) — global

> **Type:** Global `MaterialInstanceDynamic`.
> Create runtime instances with overrideable parameters (Scalar, Vector, Texture).
> Parameters are defined via **Scalar Param**, **Vector Param**, **Texture Param** nodes.

```lua
-- Create dynamic instance
local name = Material.CreateDynamic("Content/Materials/Base.ice_mat")          -- Auto name
local name = Material.CreateDynamic("Content/Materials/Base.ice_mat", "MyGlow") -- Custom name

-- Load instance from .ice_matinst
local name = Material.LoadInstance("Content/Materials/Custom.ice_matinst")

-- Destroy instance
Material.DestroyDynamic("MyGlow")

-- Scalar params (float)
Material.SetScalar("MyGlow", "Intensity", 2.0)
local val = Material.GetScalar("MyGlow", "Intensity")
local has = Material.HasScalar("MyGlow", "Intensity")   -- true/false
Material.ClearScalar("MyGlow", "Intensity")              -- Reset to default

-- Vector params (color / vec4)
Material.SetVector("MyGlow", "EmissiveColor", 1, 0.5, 0)       -- RGB (A=1)
Material.SetVector("MyGlow", "EmissiveColor", 1, 0.5, 0, 1)    -- RGBA
local v = Material.GetVector("MyGlow", "EmissiveColor")         -- → {r, g, b, a}
local has = Material.HasVector("MyGlow", "EmissiveColor")
Material.ClearVector("MyGlow", "EmissiveColor")

-- Textures
Material.SetTexture("MyGlow", "NormalMap", "Content/Textures/normal.png")
Material.ClearTexture("MyGlow", "NormalMap")

-- Alpha clip
Material.SetAlphaClip("MyGlow", 0.3)

-- Clear all overrides
Material.ClearOverrides("MyGlow")
```

### MPC — Material Parameter Collection (`MPC.*`)

> **Type:** Global `MaterialParameterCollection`.
> Global parameter sets referenced by any material via **Collection Param**.
> Changes apply immediately to all materials using the collection.

```lua
-- Load from file
MPC.Load("Content/Materials/GlobalParams.ice_mpc")

-- Or create at runtime
MPC.Create("WorldParams")

-- Add parameters (name, default value)
MPC.AddScalar("WorldParams", "WindStrength", 1.0)
MPC.AddVector("WorldParams", "FogColor", 0.5, 0.5, 0.7, 1.0)

-- Set / get
MPC.SetScalar("WorldParams", "WindStrength", 3.5)
local wind = MPC.GetScalar("WorldParams", "WindStrength")

MPC.SetVector("WorldParams", "FogColor", 0.8, 0.8, 0.9)
local fog = MPC.GetVector("WorldParams", "FogColor")   -- → {r, g, b, a}

-- Reset all params to defaults
MPC.Reset("WorldParams")

-- Unload collection
MPC.Unload("WorldParams")
```

### Parameter nodes in Material Editor

| Node | Description | Output types |
|------|-------------|-------------|
| **Scalar Param** | Float param, override via `Material.SetScalar()` | `Value` (float) |
| **Vector Param** | Color/vector, override via `Material.SetVector()` | `RGBA`, `RGB`, `R`, `G`, `B`, `A` |
| **Texture Param** | Texture param, override via `Material.SetTexture()` | `RGB`, `R`, `G`, `B`, `A` |
| **Collection Param** | Reference to MPC param | `Value` (float), `Vector` (vec4) |
| **Material Function** | Reusable function call from `.ice_matfunc` | Depends on function |

### Material Function (`.ice_matfunc`)

> **Material Function** is a reusable subgraph.
> Created as a `.ice_matfunc` file with **Input** and **Output** nodes.
> Using **Material Function** node inlines the graph into the main shader.

**Inside the function graph you can use:**

| Node | Description |
|------|-------------|
| **Input** | Function input. `FunctionInputIndex` defines input number (0, 1, 2...) |
| **Output** | Function output. `FunctionInputIndex` defines output number (0, 1, 2...) |

> Max nesting depth — 8 (a function can call other functions).

### Example: dynamic hit flash

```lua
-- HitFlashEntity.ice_class
local hitMat = nil

function OnConstruct()
    hitMat = Material.CreateDynamic("Content/Materials/HitFlash.ice_mat", "HitFlash")
    SetSpriteMaterial(hitMat)
    Material.SetScalar(hitMat, "FlashAmount", 0)
end

function TakeDamage(amount)
    Material.SetScalar(hitMat, "FlashAmount", 1.0)
    Material.SetVector(hitMat, "FlashColor", 1, 0, 0)
    Tween(1.0, 0.0, 0.3, "easeOut", function(v)
        Material.SetScalar(hitMat, "FlashAmount", v)
    end)
end
```

### Example: day/night cycle via MPC

```lua
-- DayNightCycle.ice_class (level script)

function OnConstruct()
    timeOfDay = 12.0   -- Noon
    daySpeed = 1.0     -- 1 second = 1 in-game hour
end

function OnCreate()
    MPC.Load("Content/Materials/WorldParams.ice_mpc")
end

function OnUpdate(dt)
    timeOfDay = timeOfDay + dt * daySpeed
    if timeOfDay >= 24 then timeOfDay = timeOfDay - 24 end

    MPC.SetScalar("WorldParams", "TimeOfDay", timeOfDay)

    -- Ambient light depends on time of day
    local night = math.abs(timeOfDay - 12) / 12  -- 0 = noon, 1 = midnight
    local ambient = 0.8 - night * 0.6
    SetAmbientIntensity(ambient)
    SetAmbientLight(
        0.4 + (1 - night) * 0.6,
        0.4 + (1 - night) * 0.5,
        0.5 + (1 - night) * 0.3
    )

    -- Sun color: warm by day, cold at night
    MPC.SetVector("WorldParams", "SunColor",
        1.0 - night * 0.7,
        0.9 - night * 0.6,
        0.7 - night * 0.2
    )
end
```

### Example: interactive glowing pickup

```lua
-- GlowPickup.ice_class
local glowMat = nil
local baseGlow = 0.5
local pulseTime = 0

function OnConstruct()
    glowMat = Material.CreateDynamic("Content/Materials/Glow.ice_mat", "GlowPickup_" .. GetEntityId())
    SetSpriteMaterial(glowMat)
    Material.SetVector(glowMat, "GlowColor", 0.2, 0.8, 1.0)
end

function OnUpdate(dt)
    pulseTime = pulseTime + dt
    local intensity = baseGlow + math.sin(pulseTime * 3) * 0.3
    Material.SetScalar(glowMat, "GlowIntensity", intensity)
end

function OnSensorEnter(tag, id)
    if tag == "Player" then
        -- Flash on pickup
        Material.SetScalar(glowMat, "GlowIntensity", 3.0)
        Material.SetVector(glowMat, "GlowColor", 1, 1, 1)
        PlaySound("pickup_glow")
        Delay(0.15, function()
            DestroySelf()
        end)
    end
end

function OnDestroy()
    if glowMat then
        Material.DestroyDynamic(glowMat)
    end
end
```

---

## 39. Destruction — Destruction

> **Type:** Entity-bound + global. Requires the **DestructibleComponent** for entities.
> Allows fracturing sprites/flipbooks and tilemaps, creating physical debris.

### Fracture — break the current entity

```lua
-- impactX/impactY, force, fragments, pattern (0..2), opts
local result = Fracture(impactX, impactY, 350, 12, 1, {
    lifetime = 3.0,
    fadeTime = 1.0,
    gravityScale = 1.0,
    density = 1.0,
    friction = 0.3,
    restitution = 0.3,
    destroy = true,
    preSolveEvents = false,
    castShadow = true,
    shadowOrigin = 0,
    shadowEdgeFade = 0.0,
    shadowZOrder = 0
})

-- result.success = true/false
-- result.fragments = { entityId, ... }
-- result.count = fragment count
```

If `impactX/impactY` are not provided, the entity position is used.

### FractureEntity — break entity by ID

```lua
local result = FractureEntity(entityId, impactX, impactY, 350, 8, 0, {
    lifetime = 3.0,
    fadeTime = 1.0,
    gravityScale = 1.0,
    density = 1.0,
    friction = 0.3,
    restitution = 0.3,
    destroy = true,
    preSolveEvents = false,
    castShadow = true,
    shadowOrigin = 0,
    shadowEdgeFade = 0.0,
    shadowZOrder = 0
})
```

### ExplodeTiles — explode the current tilemap

Each destroyed tile spawns debris using the **per-tile Fragment settings** configured in the
Tileset editor (static tiles) or the Tilemap editor's animated-tile panel (animated tiles).
The `opts` table is an optional runtime override: any field you pass overrides that tile's
configured value; fields you omit fall back to the per-tile settings. This mirrors how the
Destructible component works for entities. Animated destructible tiles now also spawn debris
from their current flipbook frame.

```lua
-- Uses each tile's own configured Fragment settings:
local result = ExplodeTiles(x, y, 120, 400)

-- Override specific fields for this explosion only:
local result = ExplodeTiles(x, y, 120, 400, 0, {
    count = 4,            -- shatter each tile into 4 sub-fragments (1 = whole tile)
    pattern = 0,          -- 0 Grid, 1 Radial, 2 Random (only when count > 1)
    lifetime = 3.0,
    fadeTime = 1.0,
    gravityScale = 1.0,
    density = 0.5,
    friction = 0.4,
    restitution = 0.2,
    isSensor = false,
    contactEvents = true,
    sensorEvents = false,
    hitEvents = true,
    preSolveEvents = false,
    collisionGroup = "Debris", -- name or index; default = built-in Debris group
    castShadow = true,
    dontBlockShadows = true,
    shadowOrigin = 0,
    shadowEdgeFade = 0.0,
    shadowZOrder = 0
})
```

### ExplodeTilesOnEntity — explode tilemap by ID

Same per-tile-settings + `opts`-override behavior as `ExplodeTiles`, but targets a tilemap
entity by id.

```lua
local result = ExplodeTilesOnEntity(entityId, x, y, 120, 400, 0, {
    count = 1,
    lifetime = 3.0,
    fadeTime = 1.0,
    gravityScale = 1.0,
    density = 0.5,
    friction = 0.4,
    restitution = 0.2,
    preSolveEvents = false,
    collisionGroup = "Debris",
    castShadow = true,
    shadowOrigin = 0,
    shadowEdgeFade = 0.0,
    shadowZOrder = 0
})
```

### Destructible entity control

```lua
SetDestructible(true)
local enabled = IsDestructible()

SetDestructibleHealth(100)
local hp = GetDestructibleHealth()

local result = DamageDestructible(25, impactX, impactY)
-- result.destroyed = true/false
-- result.health = current HP
```

### Fragment shadow defaults

```lua
SetDestructibleFragmentCastShadow(true)
local cast = GetDestructibleFragmentCastShadow()

-- Don't block shadows (default true): while the global "Colliders Block Shadows" option is on, debris fragments still let shadows pass through. Turn off so fragments block shadows.
SetDestructibleFragmentDontBlockShadows(true)
local dontBlock = GetDestructibleFragmentDontBlockShadows()

SetDestructibleFragmentShadowOrigin(1)            -- 0 = Center, 1 = Top, 2 = Bottom
local origin = GetDestructibleFragmentShadowOrigin()

SetDestructibleFragmentShadowEdgeFade(0.25)
local fade = GetDestructibleFragmentShadowEdgeFade()

SetDestructibleFragmentShadowZOrder(1)            -- negative = toward background, positive = toward foreground, 0 = caster plane (default)
local zo = GetDestructibleFragmentShadowZOrder()  -- → float
```

### Fragment collision group

```lua
SetDestructibleFragmentCollisionGroup(CollisionGroups.GetIndex("Debris"))  -- collision group index for spawned fragments
local group = GetDestructibleFragmentCollisionGroup()                      -- → int (−1 = default: "Debris" group if defined, else 0)
```

### Fracture configuration

```lua
-- How the object breaks apart (all persist on the DestructibleComponent)
SetDestructibleDestructOnStart(false)          -- fracture immediately when the level starts
local onStart = GetDestructibleDestructOnStart()

SetDestructibleFragmentCount(8)                -- number of fragments (clamped 2..64)
local n = GetDestructibleFragmentCount()

SetDestructiblePattern(0)                      -- 0 = Grid, 1 = Radial, 2 = Random
local pattern = GetDestructiblePattern()

SetDestructibleExplosionForce(300)             -- outward impulse applied to fragments
local force = GetDestructibleExplosionForce()

SetDestructibleImpactThreshold(0)              -- min collision impulse to auto-fracture (0 = off)
local threshold = GetDestructibleImpactThreshold()

SetDestructibleMaxDebrisCount(256)             -- global cap on live debris (clamped 1..2048)
local maxDebris = GetDestructibleMaxDebrisCount()

SetDestructibleDestroyOriginal(true)           -- remove the source entity after fracturing
local destroyOrig = GetDestructibleDestroyOriginal()

-- Fragment lifetime / fade (seconds)
SetDestructibleFragmentLifetime(3.0)
local life = GetDestructibleFragmentLifetime()
SetDestructibleFragmentFadeTime(1.0)
local fadeTime = GetDestructibleFragmentFadeTime()

-- Fragment physics material
SetDestructibleFragmentGravityScale(1.0)
local grav = GetDestructibleFragmentGravityScale()
SetDestructibleFragmentDensity(1.0)
local density = GetDestructibleFragmentDensity()
SetDestructibleFragmentFriction(0.3)           -- clamped 0..1
local friction = GetDestructibleFragmentFriction()
SetDestructibleFragmentRestitution(0.3)        -- clamped 0..1
local restitution = GetDestructibleFragmentRestitution()

-- Fragment collision events
SetDestructibleFragmentSensor(false)
local isSensor = GetDestructibleFragmentSensor()
SetDestructibleFragmentContactEvents(true)
local contact = GetDestructibleFragmentContactEvents()
SetDestructibleFragmentSensorEvents(false)
local sensorEv = GetDestructibleFragmentSensorEvents()
SetDestructibleFragmentHitEvents(true)
local hit = GetDestructibleFragmentHitEvents()
SetDestructibleFragmentPreSolveEvents(true)
local preSolve = GetDestructibleFragmentPreSolveEvents()
```

### Debris

```lua
SetFragmentLifetime(fragmentId, 2.0, 0.5)
local isDebris = IsDebris(fragmentId)
ClearAllDebris()
```

---

## 40. Practical examples

Below are 10 small, simple examples for beginners. Each example is a separate script.

### 1) 🚶 Character: walk, jump, crouch

```lua
-- PlayerBasic.ice_class

function OnConstruct()
    speed = 200
    jumpForce = 500
end

function OnUpdate(dt)
    local vx = 0
    if IsKeyPressed("d") then vx = speed end
    if IsKeyPressed("a") then vx = -speed end
    SetVelocityX(vx)

    if IsKeyJustPressed("space") and IsGrounded() then
        Jump(jumpForce)
    end

    if IsKeyPressed("s") then
        Crouch(0.5)
    else
        UnCrouch()
    end
end
```

### 2) 🏊 Platform back and forth

```lua
-- MovingPlatformBasic.ice_class

function OnCreate()
    startX = GetPositionX()
    endX = startX + 200
    speed = 1.0
    time = 0
end

function OnUpdate(dt)
    time = time + dt * speed
    local t = math.sin(time) * 0.5 + 0.5
    SetPositionX(startX + (endX - startX) * t)
end
```

### 3) 🚪 Door: hint and open on `E`

```lua
-- Door.ice_class

function OnConstruct()
    isOpen = false
    playerInZone = false
    SetWidgetVisible(false)
    SetWidgetText("HintText", "Press E")
end

function OnUpdate(dt)
    if playerInZone and not isOpen and IsKeyJustPressed("e") then
        isOpen = true
        SetWidgetVisible(false)
        local y = GetPositionY()
        Tween(y, y + 96, 0.6, "outQuad", function(v)
            SetPositionY(v)
        end)
    end
end

function OnSensorEnter(tag, id)
    if tag == "Player" and not isOpen then
        playerInZone = true
        SetWidgetVisible(true)
    end
end

function OnSensorExit(tag, id)
    if tag == "Player" then
        playerInZone = false
        SetWidgetVisible(false)
    end
end
```

### 4) 🪙 Collecting coins

```lua
-- PlayerCoins.ice_class

function OnConstruct()
    coins = 0
end

function OnSensorEnter(tag, id)
    if tag == "Coin" then
        DestroyEntity(id)
        coins = coins + 1
        Print("Coins: " .. coins)
    end
end
```

### 5) ❤️ Health and damage

```lua
-- HealthBasic.ice_class

function OnConstruct()
    hp = 100
end

function TakeDamage(amount)
    hp = hp - amount
    Print("HP: " .. hp)
    if hp <= 0 then
        DestroySelf()
    end
end

function OnCollisionEnter(tag, id)
    if tag == "Spike" then
        TakeDamage(25)
    end
end
```

### 6) 👾 Simple enemy: patrol

```lua
-- SimplePatrolEnemy.ice_class

function OnConstruct()
    speed = 120
    direction = 1
    timer = 0
end

function OnUpdate(dt)
    SetVelocityX(speed * direction)
    timer = timer + dt
    if timer >= 2.5 then
        timer = 0
        direction = -direction
    end
end
```

### 7) ➡️ Load next level

```lua
-- LevelExit.ice_class

function OnSensorEnter(tag, id)
    if tag == "Player" then
        LoadLevel("Content/Maps/NextLevel.icemap")
    end
end
```

### 8) 💾 Checkpoint: save/load

```lua
-- CheckpointSaveLoad.ice_class

function OnSensorEnter(tag, id)
    if tag == "Checkpoint" then
        local pos = GetPosition()
        SetGameFloat("checkpoint_x", pos.x)
        SetGameFloat("checkpoint_y", pos.y)
        Print("Checkpoint saved")
    end
end

function OnUpdate(dt)
    if IsKeyJustPressed("r") then
        local x = GetGameFloat("checkpoint_x", GetPositionX())
        local y = GetGameFloat("checkpoint_y", GetPositionY())
        SetPosition(x, y)
    end
end
```

### 9) 🧾 HUD: HP and coins

```lua
-- HUDSimple.ice_class

function OnCreate()
    UpdateHUD(100, 0)
end

function UpdateHUD(hp, coins)
    SetWidgetProgress("HealthBar", hp / 100)
    SetWidgetText("CoinsText", "Coins: " .. coins)
end
```

### 10) 🔫 Simple mouse shooting

```lua
-- ShooterBasic.ice_class

function OnUpdate(dt)
    if IsMouseJustPressed(1) then
        local pos = GetPosition()
        local mpos = GetMouseWorldPosition()
        local dir = GetDirectionTo(mpos.x, mpos.y)
        local bullet = SpawnEntity("Content/Classes/Bullet.ice_class", pos.x, pos.y)
        SetEntityVelocity(bullet, dir.x * 600, dir.y * 600)
    end
end
```

---

## 41. Mods — Mod System

The mod system allows players and developers to extend the game **without recompiling** the engine. Mods are Lua scripts that are loaded **only at runtime** (when the game starts) and have full access to the engine's Lua API.

> **Important:** mods only work during gameplay (Play mode), not in the editor. To extend the engine itself, use the plugin system (C++ DLL/SO) — see the Engine Architecture documentation.

### 41.1 Mod structure

Each mod is a folder inside the `Mods/` directory:

```
Mods/
├── MyMod/
│   ├── mod.json          ← mod descriptor (required)
│   ├── main.lua          ← entry point (default)
│   └── modules/
│       └── helpers.lua   ← additional modules
```

### 41.2 Descriptor `mod.json`

```json
{
    "Name": "MyMod",
    "Description": "My first IceBox mod",
    "Author": "PlayerName",
    "Version": "1.0.0",
    "Icon": "icon.png",
    "EntryScript": "main.lua",
    "LoadOrder": 100,
    "APIVersion": 1,
    "Dependencies": ["CoreLibMod"]
}
```

| Field | Type | Default | Description |
|---|---|---|---|
| `Name` | string | folder name | Mod name |
| `Description` | string | `""` | Description |
| `Author` | string | `""` | Author |
| `Version` | string | `"1.0.0"` | Mod version |
| `Icon` | string | `""` | Optional path (relative to mod folder) to a `.png` / `.jpg` / `.jpeg` icon shown in the editor and launcher panels. If omitted, the engine auto-detects `icon.png`/`icon.jpg`/`icon.jpeg` in the mod folder. When no icon is available a gray square with a `?` is shown. |
| `EntryScript` | string | `"main.lua"` | Main script file |
| `LoadOrder` | int | `100` | Load order (lower = earlier) |
| `APIVersion` | int | `1` | Minimum engine API version required |
| `Dependencies` | string[] | `[]` | List of mods that must be loaded first |

### 41.3 Sandbox

Each mod runs in an **isolated environment**. This means:

- The mod **can see** the entire engine Lua API (all functions from this documentation).
- The mod **cannot** overwrite global functions or variables of another mod.
- Variables `MOD_NAME`, `MOD_VERSION`, `MOD_DIR` are unique per mod.

### 41.4 Available mod variables

| Variable | Type | Description |
|---|---|---|
| `MOD_NAME` | string | Name of the current mod |
| `MOD_VERSION` | string | Version of the current mod |
| `MOD_DIR` | string | Absolute path to the mod folder |

### 41.5 `ModRequire` — loading modules

Use `ModRequire` to load additional Lua files from the mod folder:

```lua
-- main.lua
local helpers = ModRequire("modules.helpers")

helpers.sayHello()
```

```lua
-- modules/helpers.lua
local M = {}

function M.sayHello()
    Print("Hello from mod: " .. MOD_NAME)
end

return M
```

`ModRequire` replaces `.` with `/` in the module name and looks for the file relative to `MOD_DIR`. The loaded file runs in the same sandbox environment as the mod.

### 41.6 Mod lifecycle

1. **Discover** — the engine scans `Mods/` and reads `mod.json` at startup.
2. **Enable** — `Config/Mods.json` marks which mods are enabled.
3. **Load** — when the game starts (OnRuntimeStart), enabled mods are topologically sorted by their `Dependencies` (falling back to `LoadOrder`) and loaded. Each mod's `APIVersion` must be ≤ the engine API version.
4. **Execute** — `main.lua` runs in its sandboxed environment. The mod may declare any of the lifecycle callbacks below.
5. **Unload** — when the game stops (OnRuntimeStop), `OnLevelEnd` and `OnModUnload` are invoked, then the sandbox is destroyed and Lua GC frees its resources.

#### Lifecycle callbacks (all optional)

Declare any of these as **global functions** in your mod's `main.lua` (or files loaded via `ModRequire`) and the engine will call them automatically:

| Function | When it fires |
|----------|---------------|
| `OnModLoad()`            | Once, immediately after `main.lua` finishes loading at runtime start. |
| `OnLevelStart()`         | After the level, its entities and their scripts are fully initialized. Safe place to spawn entities, read tags, register timers. |
| `OnLevelUpdate(dt)`      | Every frame while the level is running. |
| `OnLevelFixedUpdate(dt)` | Every fixed-timestep tick — use this if you touch physics. |
| `OnLevelLateUpdate(dt)`  | Every frame, after the main update pass — runs later than `OnLevelUpdate`. |
| `OnLevelEnd()`           | Right before the level is torn down (when play mode stops). |
| `OnModUnload()`          | Last chance before the mod's sandbox is destroyed — cancel timers, flush state. |

Errors thrown inside a callback are logged with the mod name (`[Mod:<name>] <fn> error: ...`) and do **not** stop other mods from receiving the same event.

#### `ModRequire` soft-fail behavior

If the file requested via `ModRequire("folder.file")` is missing or has a syntax/runtime error, the function returns `nil` and logs a warning/error with the mod name. The rest of the mod keeps loading — you can safely check the result:

```lua
local settings = ModRequire("modules.settings")
if settings then
    settings.Apply()
end
```

### 41.7 Configuration `Config/Mods.json`

```json
{
    "SearchDirectory": "Mods",
    "Enabled": {
        "MyMod": true,
        "AnotherMod": false
    }
}
```

This file is automatically managed through the **Plugins** panel in the editor (**Mods** tab).

### 41.8 Mod examples

#### Simple mod: greeting on level start

```lua
-- Mods/HelloMod/main.lua
On("LevelStart", function()
    Print("[" .. MOD_NAME .. "] Level started!")
end)
```

#### Mod: modify player health

```lua
-- Mods/DoubleHP/main.lua
On("EntitySpawned", function(entityId, tag)
    if tag == "Player" then
        local hp = GetEntityData(entityId, "HP")
        SetEntityData(entityId, "HP", hp * 2)
        Print("[" .. MOD_NAME .. "] Player HP doubled!")
    end
end)
```

#### Mod with modules

```lua
-- Mods/QuestMod/main.lua
local quests = ModRequire("quests.manager")

On("LevelStart", function()
    quests.init()
    Print("[" .. MOD_NAME .. "] Quests loaded: " .. quests.count())
end)
```

```lua
-- Mods/QuestMod/quests/manager.lua
local M = {}
local questList = {}

function M.init()
    questList = {
        { name = "Find the sword", done = false },
        { name = "Defeat the boss", done = false }
    }
end

function M.count()
    return #questList
end

return M
```

### 41.9 Managing mods in the editor

The **Plugins** panel (**Mods** tab) provides:

- Enable/disable mods via checkbox.
- Search by name.
- Status display: gray = disabled, yellow = enabled (awaiting launch), green = loaded.
- Author, version, and `LoadOrder` display.
- **Refresh** button to rescan the directory.

### 41.10 `Mods` — Lua API for Mod Management

The global **`Mods`** table allows scripts (including mods themselves) to query and manage mods **at runtime**. This is the foundation for building in-game mod menus.

#### `Mods.GetAll()` → table

Returns an array of all discovered mods.

```lua
local allMods = Mods.GetAll()
for _, mod in ipairs(allMods) do
    print(mod.name .. " v" .. mod.version .. " — " .. (mod.enabled and "ON" or "OFF"))
end
```

Each element is a table with the following fields:

| Field | Type | Description |
|---|---|---|
| `name` | string | Mod name |
| `description` | string | Mod description |
| `author` | string | Author |
| `version` | string | Version |
| `enabled` | bool | Whether the mod is enabled in the config |
| `loaded` | bool | Whether the mod is currently loaded and running |
| `loadOrder` | int | Load priority (lower = earlier) |

#### `Mods.GetCount()` → int

Returns the total number of discovered mods.

```lua
print("Total mods: " .. Mods.GetCount())
```

#### `Mods.GetEnabledCount()` → int

Returns how many mods are currently enabled.

```lua
print("Enabled: " .. Mods.GetEnabledCount() .. " / " .. Mods.GetCount())
```

#### `Mods.IsEnabled(name)` → bool

Checks if a mod is enabled.

```lua
if Mods.IsEnabled("SuperWeapons") then
    print("SuperWeapons mod is ON")
end
```

#### `Mods.IsLoaded(name)` → bool

Checks if a mod is currently loaded and running.

```lua
if Mods.IsLoaded("SuperWeapons") then
    print("SuperWeapons is active")
end
```

#### `Mods.SetEnabled(name, enabled)`

Enables or disables a mod. The change takes effect **immediately** (the mod is loaded or unloaded) and is **automatically saved** to `Config/Mods.json`.

```lua
-- Enable a mod
Mods.SetEnabled("SuperWeapons", true)

-- Disable a mod
Mods.SetEnabled("SuperWeapons", false)
```

> **Note:** enabling a mod at runtime will load its `main.lua` immediately. Disabling will destroy its sandbox environment.

#### `Mods.GetInfo(name)` → table | nil

Returns detailed information about a specific mod, or `nil` if not found.

```lua
local info = Mods.GetInfo("SuperWeapons")
if info then
    print("Name: " .. info.name)
    print("Author: " .. info.author)
    print("Description: " .. info.description)
    print("Version: " .. info.version)
    print("Enabled: " .. tostring(info.enabled))
    print("Loaded: " .. tostring(info.loaded))
    print("Load Order: " .. info.loadOrder)
    print("Entry Script: " .. info.entryScript)
    print("Folder: " .. info.folderPath)
end
```

| Field | Type | Description |
|---|---|---|
| `name` | string | Mod name |
| `description` | string | Description |
| `author` | string | Author |
| `version` | string | Version |
| `enabled` | bool | Whether enabled |
| `loaded` | bool | Whether currently loaded |
| `loadOrder` | int | Load priority |
| `entryScript` | string | Entry script filename (e.g. `"main.lua"`) |
| `folderPath` | string | Absolute path to the mod folder |

#### `Mods.Refresh()`

Unloads all mods, rescans the `Mods/` directory, reloads config, and loads enabled mods.

```lua
Mods.Refresh()
print("Mods refreshed. Found: " .. Mods.GetCount())
```

#### Complete example: in-game mod menu

```lua
-- Level script: simple mod menu using widgets
function OnLevelStart()
    local mods = Mods.GetAll()

    for i, mod in ipairs(mods) do
        -- Create a toggle button for each mod
        local label = mod.name .. " v" .. mod.version
        if mod.enabled then
            label = "[ON] " .. label
        else
            label = "[OFF] " .. label
        end

        -- Use your widget system to display buttons
        -- When clicked:
        -- Mods.SetEnabled(mod.name, not mod.enabled)
    end
end
```

---

## 42. DLC — Downloadable Content

### Overview

The **`DLC`** table provides a Lua API for working with downloadable content (DLC). A DLC consists of a manifest (`.json`) placed under `DLC/` next to the game executable, plus its content — either packed into a `.icepak` archive or shipped as loose files.

On game startup the engine automatically:
1. Scans the `DLC/` folder for manifests
2. Mounts every `.icepak` file found in `DLC/` into the VFS (including split `_0.icepak`, `_1.icepak`, ...)
3. Loose DLC files placed inside their `contentPrefix` directory (typically under `Content/`) are picked up by the normal content file index

All DLC files become available through their regular content paths, so Lua code just calls e.g. `LoadLevel("Content/DLC/DarkForest/Levels/Forest.icemap")` as if it were base content.

### Two packaging modes

**Packed (`.icepak`)** — recommended for shipping:
- Single archive file per DLC (or split archives if a size limit is set)
- Protects assets from casual modification, reduces file clutter
- Mounted read-only into the VFS at runtime

**Loose** — convenient for iteration, modding, or stores that prefer unpacked depots:
- Content files are copied to the `contentPrefix` folder as-is
- No archive, easy to diff / patch individual files

### DLC structure in a build

Packed:

```
MyGame/
├── MyGame.exe
├── Content/                     ← base game content
├── DLC/
│   ├── expansion01.json         ← DLC manifest
│   ├── expansion01.icepak       ← DLC content archive
│   ├── skins_pack.json
│   └── skins_pack.icepak
└── game.json
```

Loose:

```
MyGame/
├── MyGame.exe
├── Content/
│   └── DLC/
│       └── DarkForest/          ← DLC files live here (contentPrefix)
│           ├── Levels/Forest.icemap
│           └── Textures/dark_trees.png
├── DLC/
│   └── expansion01.json         ← manifest only, no .icepak
└── game.json
```

### DLC manifest format

```json
{
    "dlcId": "expansion01",
    "dlcName": "Dark Forest Expansion",
    "version": "1.0.0",
    "gameVersionMin": "1.0.0",
    "contentPrefix": "Content/DLC/DarkForest",
    "packed": true,
    "fileCount": 15,
    "totalSize": 5242880,
    "buildDate": "2025-07-15T10:30:00Z",
    "files": [
        {
            "path": "Content/DLC/DarkForest/Levels/Forest.icemap",
            "sha256": "a1b2c3d4...",
            "size": 1024
        }
    ]
}
```

`"packed": false` marks a loose DLC. The installed check for loose DLC validates that the `contentPrefix` directory exists at runtime root.

### Packaging DLC in the editor

1. Create a DLC content folder inside `Content/`, e.g.: `Content/DLC/DarkForest/`
2. Place levels, textures, scripts and other assets inside it
3. Open **Tools → DLC Packager**
4. Fill in the fields:
   - **DLC ID** — unique identifier (`expansion01`, `dark_forest`)
   - **DLC Name** — display name (`Dark Forest Expansion`)
   - **DLC Version** — DLC version (`1.0.0`)
   - **Min Game Version** — minimum game version required (`1.0.0`)
   - **Content Folder** — select the DLC content folder inside `Content/`
   - **Pack as .icepak** — check for packed mode, uncheck for loose
   - **Max DLC Pak Size (MB)** — optional, split archive above this limit
5. Click **Package DLC**

The output folder will contain either `DLC/<dlcId>.icepak` + `DLC/<dlcId>.json`, or the loose `contentPrefix/` tree + `DLC/<dlcId>.json`.

### For distribution stores

- **Base game** = one depot/upload
- **Each DLC** = separate depot containing its `DLC/<id>.json` plus either `DLC/<id>.icepak` (packed) or the `contentPrefix/` files (loose)
- The store manages downloading/removing DLC files for players
- Lua scripts check availability via `DLC.IsInstalled()` before loading DLC content

---

### 42.1 DLC.IsInstalled

```lua
DLC.IsInstalled(dlcId) -> bool
```

Checks whether a DLC is installed (its `.icepak` archive is present, or for loose DLCs, its `contentPrefix` directory exists).

| Parameter | Type | Description |
|-----------|------|-------------|
| `dlcId` | `string` | Unique DLC identifier |

**Returns:** `true` if the DLC is installed, `false` otherwise.

```lua
if DLC.IsInstalled("expansion01") then
    Print("DLC 'Dark Forest' is available!")
    LoadLevel("Content/DLC/DarkForest/Levels/Forest.icemap")
else
    Print("DLC not installed")
end
```

---

### 42.2 DLC.GetAll

```lua
DLC.GetAll() -> table
```

Returns an array of tables with all known DLCs (both installed and not — if a manifest is present).

**Returns:** 1-indexed array of tables with the following fields:

| Field | Type | Description |
|-------|------|-------------|
| `id` | `string` | DLC identifier |
| `name` | `string` | Display name |
| `version` | `string` | DLC version |
| `gameVersionMin` | `string` | Minimum game version required |
| `installed` | `bool` | Whether installed (archive or loose folder present) |
| `packed` | `bool` | `true` if the DLC ships as `.icepak`, `false` if loose |
| `fileCount` | `int` | Number of files |
| `contentPrefix` | `string` | Path to DLC content |

```lua
local allDLC = DLC.GetAll()
for _, dlc in ipairs(allDLC) do
    local status = dlc.installed and "Installed" or "Not installed"
    Print(dlc.name .. " v" .. dlc.version .. " — " .. status)
end
```

---

### 42.3 DLC.GetInstalledIds

```lua
DLC.GetInstalledIds() -> table
```

Returns an array of strings — identifiers of all installed DLCs.

```lua
local ids = DLC.GetInstalledIds()
Print("Installed DLCs: " .. #ids)
for _, id in ipairs(ids) do
    Print("  - " .. id)
end
```

---

### 42.4 DLC.GetInfo

```lua
DLC.GetInfo(dlcId) -> table | nil
```

Returns detailed information about a specific DLC.

| Parameter | Type | Description |
|-----------|------|-------------|
| `dlcId` | `string` | DLC identifier |

**Returns:** a table with fields, or `nil` if the DLC is not found.

| Field | Type | Description |
|-------|------|-------------|
| `id` | `string` | DLC identifier |
| `name` | `string` | Display name |
| `version` | `string` | DLC version |
| `gameVersionMin` | `string` | Minimum game version required |
| `installed` | `bool` | Whether installed |
| `packed` | `bool` | `true` if packaged as `.icepak`, `false` if loose |
| `fileCount` | `int` | Number of files |
| `totalSize` | `int` | Total size in bytes |
| `contentPrefix` | `string` | Content path |

```lua
local info = DLC.GetInfo("expansion01")
if info then
    Print("DLC: " .. info.name)
    Print("Version: " .. info.version)
    Print("Min game version: " .. info.gameVersionMin)
    Print("Files: " .. info.fileCount)
    Print("Size: " .. math.floor(info.totalSize / 1024 / 1024) .. " MB")
    Print("Content: " .. info.contentPrefix)
    Print("Status: " .. (info.installed and "installed" or "not installed"))
else
    Print("DLC expansion01 not found")
end
```

---

### 42.5 DLC.GetCount

```lua
DLC.GetCount() -> int
```

Returns the total number of DLCs (including uninstalled ones that have a manifest).

```lua
Print("Total DLCs: " .. DLC.GetCount())
```

---

### 42.6 DLC.GetInstalledCount

```lua
DLC.GetInstalledCount() -> int
```

Returns the number of installed DLCs.

```lua
Print("Installed DLCs: " .. DLC.GetInstalledCount() .. " of " .. DLC.GetCount())
```

---

### 42.7 Practical examples

#### Main menu with DLC content

```lua
function OnCreate()
    -- Show DLC button only if installed
    if DLC.IsInstalled("expansion01") then
        if HasWidgetElement("DLCButton") then
            SetWidgetElementVisible("DLCButton", true)
            SetWidgetText("DLCButton", "Dark Forest")
        end
    end
end
```

#### Loading a DLC level

```lua
function StartDLCCampaign()
    if not DLC.IsInstalled("expansion01") then
        Print("You need to install the 'Dark Forest Expansion' DLC")
        -- Can show a DLC purchase dialog
        return
    end

    LoadLevel("Content/DLC/DarkForest/Levels/Forest.icemap")
end
```

#### DLC list in settings

```lua
function ShowDLCList()
    local allDLC = DLC.GetAll()
    if #allDLC == 0 then
        Print("No DLCs found")
        return
    end

    Print("=== Downloadable Content ===")
    for i, dlc in ipairs(allDLC) do
        local status
        if dlc.installed then
            status = dlc.packed and "Ready (packed)" or "Ready (loose)"
        else
            status = "Not installed"
        end
        Print(i .. ". " .. dlc.name .. " v" .. dlc.version .. " - " .. status)
    end
    Print("Installed: " .. DLC.GetInstalledCount() .. "/" .. DLC.GetCount())
end
```

#### Conditional spawning of DLC content

```lua
function OnCreate()
    -- Spawn DLC enemies if DLC is installed
    if DLC.IsInstalled("dark_forest") then
        local info = DLC.GetInfo("dark_forest")
        if info then
            SpawnEntity(info.contentPrefix .. "/Scripts/ForestEnemy.ice_class", 100, 200)
            SpawnEntity(info.contentPrefix .. "/Scripts/ForestBoss.ice_class", 500, 200)
        end
    end
end
```

#### Save games with DLC awareness

```lua
function SaveGame()
    SetGameInt("hasDLC_expansion01", DLC.IsInstalled("expansion01") and 1 or 0)
    SaveGameState("savegame.json")
end

function LoadGame()
    LoadGameState("savegame.json")

    local hadDLC = GetGameInt("hasDLC_expansion01", 0) == 1
    local hasDLC = DLC.IsInstalled("expansion01")

    if hadDLC and not hasDLC then
        Print("⚠ This save uses a DLC that is no longer installed!")
    end
end
```

---

## 43. Ads — Advertising (Google AdMob)

### Overview

The **`Ads`** table provides a Lua API for displaying advertisements in your game via **Google AdMob**.
Supports **banner**, **interstitial** (full-screen between screens), and **rewarded** (watch ad for reward) ad types.

> **Platform — ad *serving*:** Android and iOS. Both run the same Google Mobile Ads SDK behind
> the identical `Ads.*` calls and the identical event strings, so no branching is needed.
> `Ads.IsSupported()` reports whether the SDK is actually present in the running binary.
>
> **Platform — everything else in this chapter:** the advertising identifier works on Android **and** iOS, and Apple's own attribution and store-promotion frameworks (SKAdNetwork, Apple Search Ads, SKOverlay) are fully supported with no third-party SDK — see [43.10](#4310-advertising-id-attribution-and-app-store-promotion).
>
> **Build requirement — Android:** enable "Google AdMob (Ads)" in the Build Game popup and
> provide your AdMob App ID. Gradle pulls `play-services-ads` automatically. Leaving the AdMob
> App ID empty while ads are enabled is a hard build error, exactly as on iOS: the SDK reads
> `com.google.android.gms.ads.APPLICATION_ID` from the manifest at process start and refuses to
> serve anything without a real one, so the APK would look fine and never show an ad.
>
> **Build requirement — iOS:** the Google Mobile Ads SDK is not redistributed with the engine,
> so download it from Google once per machine and put `GoogleMobileAds.xcframework` into
> `Tools/BuildSystem/Vendor/GoogleMobileAds/` inside the engine folder.
> `Tools/BuildSystem/BuildEngine/fetch_googlemobileads.sh` does that for you and defaults to
> SDK 13.7.0, which Google builds with **Xcode 26.2** — the engine itself still configures on
> Xcode 15+, so this raised floor applies only to iOS builds that actually link the ads SDK.
> Pass `--version=X.Y.Z` (or set `ICE_ADMOB_VERSION`) to vendor an older SDK if your Xcode is
> older; 11.x is the last line that builds on Xcode 15.
>
> Then enable **Ads & Attribution** in Build Game → iOS and fill in the **AdMob App ID**.
> CMake finds `Tools/BuildSystem/Vendor/GoogleMobileAds/GoogleMobileAds.xcframework`, defines
> `ICE_HAS_ADMOB` and links it; the build log prints the SDK version it picked up. Without the
> vendored SDK every `Ads.*` call stays a no-op and logs why. Leaving the AdMob App ID empty
> while the SDK *is* linked is a hard configure error, because the SDK aborts at launch when
> `GADApplicationIdentifier` is missing from `Info.plist`.
>
> The SDK is licensed to you directly by Google under the Android SDK Licence Agreement and the
> Google APIs Terms of Service, not by IceBoxCrew Studio — see `THIRD_PARTY_NOTICES.txt`
> section 14. Shipping it makes you the data controller for what it collects: you own the
> consent flow, the App Privacy answers and the privacy policy.
>
> The engine declares exactly one `SKAdNetworkIdentifier` — Google's own
> `cstr6suwn9.skadnetwork` — because declaring networks the binary never contacts makes your
> App Privacy answers inaccurate. If you enable AdMob **mediation**, add each partner network's
> identifier to the `SKAdNetworkItems` array in the exported Xcode project's `Info.plist`
> (*Extra Usage Descriptions* only injects string keys, not arrays).

---

### 43.1 Ads.IsSupported

```lua
Ads.IsSupported() -> bool
```

Returns `true` when the Google Mobile Ads SDK is actually present in the running binary —
Android with "Google AdMob (Ads)" enabled, or iOS with the vendored `GoogleMobileAds.xcframework`
linked. `false` everywhere else, where every `Ads.*` call is a no-op.

```lua
if Ads.IsSupported() then
    Ads.Init()
end
```

---

### 43.2 Ads.Init

```lua
Ads.Init()
```

Initializes the AdMob SDK. Must be called once before any other `Ads` function.
The result is delivered asynchronously via `Ads.OnInitialized`. `Ads.IsInitialized()` returns
the same state synchronously, and calling `Ads.Init()` again once initialized simply re-fires
`Ads.OnInitialized(true)` instead of re-initializing.

```lua
Ads.Init()
Ads.OnInitialized(function(success)
    if success then
        Print("AdMob ready!")
    end
end)
```

---

### 43.3 Ad request configuration (policy and testing)

Call these **before** `Ads.Init()` — Google requires the child-directed and under-age tags to be
set before the SDK starts. They apply to every banner, interstitial and rewarded request from
then on, and they behave identically on Android and iOS.

```lua
Ads.SetTestDeviceIds({ "33BE2250B43518CCDA7DE426D04EE231" })  -- test ads on your own devices
Ads.SetChildDirected(true)          -- COPPA: true / false / nil (= unspecified)
Ads.SetUnderAgeOfConsent(true)      -- GDPR under-16: true / false / nil (= unspecified)
Ads.SetMaxAdContentRating("G")      -- "G", "PG", "T", "MA", or "" for unspecified
Ads.SetNonPersonalizedAds(true)     -- request non-personalised ads only
Ads.Init()
```

| Function | Description |
|----------|-------------|
| `Ads.SetTestDeviceIds(list)` | Array of device IDs that should receive test ads. The device ID is printed in logcat / the Xcode console on the first ad request. **Never tap your own live ads** — it is an AdMob ban. |
| `Ads.SetChildDirected(enabled?)` | Tags requests for child-directed treatment (COPPA). Omit the argument for "unspecified". |
| `Ads.SetUnderAgeOfConsent(enabled?)` | Tags the user as under the age of consent (GDPR). Omit the argument for "unspecified". |
| `Ads.SetMaxAdContentRating(rating)` | Caps ad content rating: `"G"`, `"PG"`, `"T"`, `"MA"`. Anything else means unspecified. |
| `Ads.SetNonPersonalizedAds(flag)` | When `true`, every request carries `npa=1`. Use it when `Consent.CanShowAds()` is true but the user refused personalisation. |

> Non-personalised ads earn less than personalised ones. Only force the flag when consent
> actually requires it — the [Consent](#49-consent--gdpr-consent-ump) chapter drives that decision.

---

### 43.4 Banner Ads

```lua
Ads.SetBannerUnitId(unitId)       -- Set the AdMob banner unit ID
Ads.ShowBanner(position?)          -- Show banner (0 = top, 1 = bottom; default bottom)
Ads.HideBanner()                   -- Hide banner (keeps it loaded)
Ads.DestroyBanner()                -- Destroy banner completely
Ads.IsBannerVisible() -> bool      -- Check if banner is currently visible
Ads.GetBannerHeight() -> int       -- Height of the visible banner in device pixels (0 when hidden)
```

**Constants:** `Ads.BANNER_TOP` (0), `Ads.BANNER_BOTTOM` (1)

```lua
Ads.SetBannerUnitId("ca-app-pub-XXXXX/YYYYY")
Ads.ShowBanner(Ads.BANNER_BOTTOM)

Ads.OnBannerEvent(function(event)
    Print("Banner event: " .. event) -- "loaded", "failed:X", "impression", "clicked", "opened", "closed"
    if event == "loaded" then
        -- Keep HUD buttons clear of the banner strip:
        SetHudBottomInset(Ads.GetBannerHeight())
    end
end)
```

Both platforms use an **anchored adaptive banner** sized to the current screen width and inset
out of the display cutout / system bars, so the banner never overlaps a notch and never gets
letterboxed. That is why `Ads.GetBannerHeight()` is the only correct way to reserve space for
it — the height is device- and orientation-dependent, not a fixed 50 dp.

The banner is paused and resumed automatically with the app, and re-sized and re-requested on
rotation, on both platforms; there is no lifecycle bookkeeping to do from Lua. Re-read
`Ads.GetBannerHeight()` after a rotation — an auto-orientation game gets a different height in
landscape than in portrait.

Calling `Ads.ShowBanner(position)` again while a banner already exists moves it to the new
position and makes it visible again without re-requesting an ad — unless you changed
`Ads.SetBannerUnitId()` in between, in which case the old banner is torn down and the new unit
is requested.

---

### 43.5 Interstitial Ads

```lua
Ads.SetInterstitialUnitId(unitId)  -- Set interstitial unit ID
Ads.LoadInterstitial()              -- Pre-load interstitial
Ads.ShowInterstitial()              -- Show pre-loaded interstitial
Ads.IsInterstitialReady() -> bool   -- Check if interstitial is loaded
```

An AdMob full-screen ad object is **single-use**. `Ads.ShowInterstitial()` consumes the loaded
ad immediately, so `Ads.IsInterstitialReady()` turns `false` the moment you show it — pre-load
the next one when you get `closed`. `Ads.LoadInterstitial()` is a no-op while a load is already
in flight or an ad is already cached, so it is safe to call it defensively.

```lua
Ads.SetInterstitialUnitId("ca-app-pub-XXXXX/ZZZZZ")
Ads.LoadInterstitial()

Ads.OnInterstitialEvent(function(event)
    if event == "loaded" then
        Print("Interstitial ready")
    elseif event == "closed" then
        Print("Interstitial closed, pre-loading next")
        Ads.LoadInterstitial()
    end
end)

-- Show between levels:
function OnLevelComplete()
    if Ads.IsInterstitialReady() then
        Ads.ShowInterstitial()
    end
end
```

---

### 43.6 Rewarded Ads

```lua
Ads.SetRewardedUnitId(unitId)          -- Set rewarded ad unit ID
Ads.SetRewardedUserId(userId)          -- Server-side verification: your user ID
Ads.SetRewardedCustomData(customData)  -- Server-side verification: opaque payload
Ads.LoadRewarded()                      -- Pre-load rewarded ad
Ads.ShowRewarded()                      -- Show rewarded ad
Ads.IsRewardedReady() -> bool           -- Check if rewarded ad is loaded
```

```lua
Ads.SetRewardedUnitId("ca-app-pub-XXXXX/WWWWW")
Ads.LoadRewarded()

Ads.OnRewardEarned(function(rewardType, rewardAmount)
    Print("Reward earned: " .. rewardType .. " x" .. rewardAmount)
    AddCoins(rewardAmount)
end)

Ads.OnRewardedEvent(function(event)
    if event == "closed" then
        Ads.LoadRewarded() -- Pre-load next
    end
end)

-- "Watch ad for 50 coins" button:
function OnWatchAdButton()
    if Ads.IsRewardedReady() then
        Ads.ShowRewarded()
    else
        Print("Ad not ready yet")
    end
end
```

Like interstitials, a rewarded ad is single-use: `Ads.IsRewardedReady()` goes `false` as soon as
you call `Ads.ShowRewarded()`, so pre-load the next one on `closed`.

**Server-side verification (SSV).** `Ads.OnRewardEarned` fires on the device, so a modified
client can fake it. For anything that costs you real money — currency in a donation-driven
economy, premium unlocks — turn on SSV in the AdMob console for the rewarded unit, point it at
your server, and tag each impression:

```lua
Ads.SetRewardedUserId(myAccountId)                   -- arrives as user_id in the SSV callback
Ads.SetRewardedCustomData("quest=daily_bonus")       -- arrives as custom_data
Ads.LoadRewarded()
```

Both values are attached to the **next** ad you load, so set them before `Ads.LoadRewarded()`.
Grant the reward on your server when Google's verified callback arrives, and treat the client's
`Ads.OnRewardEarned` as a UI cue only. Leaving both unset keeps the previous behaviour (no SSV
options attached).

---

### 43.7 Ad revenue (impression-level)

```lua
Ads.OnPaidEvent(function(info)
    -- info.format      "banner" | "interstitial" | "rewarded"
    -- info.unitId      the ad unit that earned it
    -- info.valueMicros integer, 1 000 000 micros = 1.0 of info.currency
    -- info.value       the same amount as a float
    -- info.currency    ISO-4217 code, e.g. "USD"
    -- info.precision   "estimated" | "publisher_provided" | "precise" | "unknown"
    Analytics.TrackRevenue(info.value, info.currency)
end)
```

Fires once per paid impression on both platforms. This is what you feed into ROAS/LTV
reporting; it is the only way to attribute ad revenue to an individual player.

---

### 43.8 Ads.Destroy

```lua
Ads.Destroy()
```

Cleans up all ad resources and callbacks.

---

### 43.9 Callbacks summary

| Callback | Parameters | Description |
|----------|------------|-------------|
| `Ads.OnInitialized(fn)` | `(success: bool)` | AdMob SDK initialized |
| `Ads.OnBannerEvent(fn)` | `(event: string)` | Banner events: `loaded`, `failed:CODE`, `failed:no_unit_id`, `failed:not_initialized`, `impression`, `clicked`, `opened`, `closed` |
| `Ads.OnInterstitialEvent(fn)` | `(event: string)` | Interstitial events: `loaded`, `failed:CODE`, `failed:no_unit_id`, `failed:not_initialized`, `shown`, `impression`, `clicked`, `show_failed:CODE`, `closed`, `not_ready` |
| `Ads.OnRewardedEvent(fn)` | `(event: string)` | Rewarded events: `loaded`, `failed:CODE`, `failed:no_unit_id`, `failed:not_initialized`, `shown`, `impression`, `clicked`, `show_failed:CODE`, `earned`, `closed`, `not_ready` |
| `Ads.OnRewardEarned(fn)` | `(type: string, amount: int)` | User earned a reward |
| `Ads.OnPaidEvent(fn)` | `(info: table)` | Impression-level ad revenue — see [43.7](#437-ad-revenue-impression-level) |
| `Ads.ClearCallbacks()` | — | Remove all callbacks |

---

### 43.10 Advertising ID, attribution and App Store promotion

Beyond ad serving, both platforms expose an **advertising identifier**, and iOS additionally
ships Apple's own attribution and store-promotion frameworks. These work with **no third-party
SDK at all**.

```lua
-- Advertising identifier (Android: GAID, iOS: IDFA) -- always ask for consent first
Consent.ShowForm()                       -- iOS: the ATT prompt; Android: the UMP form
Ads.OnAdvertisingId(function(id, limited)
    print("ad id:", id, "limit ad tracking:", limited)
end)
Ads.RequestAdvertisingId()               -- async on Android, immediate on iOS
local id = Ads.GetAdvertisingId()        -- "" until fetched / when the user opted out
```

| Function | Description |
|---|---|
| `Ads.RequestAdvertisingId()` | Fetches the identifier and fires `Ads.OnAdvertisingId(id, limitAdTracking)` |
| `Ads.GetAdvertisingId()` | Last known identifier, or `""` when unavailable or the user opted out |
| `Ads.IsLimitAdTrackingEnabled()` | `true` when the user declined tracking (iOS: ATT not authorized) |

> **iOS build requirement:** enable **Ads & Attribution** in Build Game → iOS. It links
> `AdSupport` (IDFA) and `AdServices` (Apple Search Ads attribution). With the switch off
> `AdSupport` is not linked, `ASIdentifierManager` never resolves at runtime, and
> `Ads.GetAdvertisingId()` returns `""`.
>
> **Answer the App Store Connect IDFA question from what your build actually does**, not from
> this switch alone. `AppTrackingTransparency` is linked into *every* iOS build, because
> `Consent.ShowForm()` and `Permissions.Request("TRACKING")` are part of the public API on
> every build. If your game calls either of them, or serves ads, it uses the IDFA and the
> honest answer is "Yes".
>
> **ATT purpose string:** the iOS build declares `NSUserTrackingUsageDescription`
> automatically, because iOS terminates an app that calls `requestTrackingAuthorization`
> without it. The default wording is deliberately generic — replace it with text that matches
> your game by adding `NSUserTrackingUsageDescription=<your reason>` to Build Game → iOS →
> *Extra Usage Descriptions*.

**Apple-only — attribution and store promotion** (`Ads.IsAttributionSupported()` /
`Ads.IsStorePromotionSupported()` return `false` on Android, where the calls are no-ops):

| Function | Description |
|---|---|
| `Ads.UpdateConversionValue(value)` | SKAdNetwork fine conversion value `0..63` |
| `Ads.UpdatePostbackConversionValue(fine, coarse)` | Fine value plus a coarse bucket — `"low"` / `"medium"` / `"high"` (iOS 16.1+; falls back to the fine value alone below that) |
| `Ads.GetAttributionToken()` | Apple Search Ads attribution token — POST it to `https://api-adservices.apple.com/api/v1/` from your server to resolve the campaign |
| `Ads.ShowStoreOverlay(appStoreId)` | Presents an `SKOverlay` App Store card — Apple's own cross-promotion surface, no ad SDK needed |
| `Ads.DismissStoreOverlay()` | Dismisses it |
| `Ads.ShowStoreProduct(appStoreId)` | Full App Store product page (`SKStoreProductViewController`) inside your game |

```lua
if Ads.IsAttributionSupported() then
    Ads.UpdatePostbackConversionValue(12, "medium")   -- after a meaningful post-install event
end

if Ads.IsStorePromotionSupported() then
    Ads.OnStorePromotionEvent(function(event, message, ok)
        print(event, message, ok)   -- overlay_shown / overlay_dismissed / product_shown / ..._failed
    end)
    Ads.ShowStoreOverlay("1234567890")                -- promote your other game
end
```


## 44. IAP — In-App Purchases (Google Play Billing)

### Overview

The **`IAP`** table provides a Lua API for **in-app purchases** and **subscriptions** via Google Play Billing.
Supports one-time purchases (consumable items like coins/gems, non-consumable like "remove ads") and subscriptions (battle pass, VIP).

> **Platform:** Android (Google Play Billing) and iOS (StoreKit). On other platforms `IAP.IsSupported()` returns `false` and all calls are no-ops.
>
> **Cross-store note:** product IDs are configured per store — Google Play Console for Android, App Store Connect for iOS. On iOS `IAP.Consume()` and `IAP.Acknowledge()` are no-ops that still fire their events (StoreKit finalizes transactions automatically); grant the item on `IAP.OnPurchaseComplete`.
>
> **One product schema, both stores:** `IAP.OnProductsQueried` hands you the same field names on
> Android and iOS (`id`, `name`, `title`, `description`, `price`, `priceMicros`, `currency`,
> `billingPeriod`, `type`), so a shop screen written once runs on both.
>
> **Build requirement:** Enable "Google Play Billing (IAP)" in the Build Game popup.

---

### 44.1 IAP.IsSupported

```lua
IAP.IsSupported() -> bool
```

Returns `true` if the current platform supports in-app purchases.

---

### 44.2 IAP.Init

```lua
IAP.Init()
```

Connects to the store. Must be called once before any other `IAP` function.

```lua
IAP.Init()
IAP.OnInitialized(function(success)
    if success then
        Print("Billing service connected")
        IAP.QueryProducts({"coins_100", "coins_500", "remove_ads"}, false)
    end
end)
```

On Android the connection is **self-healing**: if Google Play drops the billing service you get
`IAP.OnEvent("disconnected")` and the engine reconnects on its own with exponential backoff
(1 s → 60 s). Any call made while disconnected fails loudly instead of vanishing — you always
get a `failed:service_unavailable` purchase result, an empty `IAP.OnProductsQueried`, or a
`consume_failed:service_unavailable` / `acknowledge_failed:service_unavailable` event — so a
shop screen can never sit on a spinner forever.

The engine also **reconciles purchases automatically**: on every successful connection and every
time the app returns to the foreground it re-queries Google Play and replays any `PURCHASED`
purchase that was never acknowledged as a normal `IAP.OnPurchaseComplete{status="purchased"}`.
That is what catches a purchase that completed while your game was closed, a pending
(slow-card / cash) payment that cleared later, or a promo code redeemed in the Play Store app.
**Make your grant logic idempotent.**

---

### 44.3 IAP.IsConnected

```lua
IAP.IsConnected() -> bool
```

Returns `true` if the store is reachable — the Play Billing connection on Android,
`canMakePayments` on iOS.

---

### 44.3a IAP.SetUserId

```lua
IAP.SetUserId(userId)
```

Ties every subsequent purchase to your own account ID. Google Play receives it as the
*obfuscated account ID* (the engine sends a SHA-256 digest, never the raw value); StoreKit
receives it as `applicationUsername`. Both stores use it for fraud detection, and it lets your
server match a receipt to a player. Set it once you know who is playing, before `IAP.Purchase`.

---

### 44.4 IAP.QueryProducts

```lua
IAP.QueryProducts(productIds, isSubscription?)
```

Queries product details from Google Play. Results are returned via `IAP.OnProductsQueried`.

| Parameter | Type | Description |
|-----------|------|-------------|
| `productIds` | `table` | Array of product ID strings |
| `isSubscription` | `bool` | `true` for subscriptions, `false` for one-time (default: `false`) |

```lua
-- Query one-time products
IAP.QueryProducts({"coins_100", "coins_500", "remove_ads"}, false)

-- Query subscriptions
IAP.QueryProducts({"vip_monthly", "battle_pass"}, true)

IAP.OnProductsQueried(function(products)
    for _, p in ipairs(products) do
        Print(p.id .. " — " .. p.name .. " — " .. p.price)
    end
end)
```

**Product fields returned** — identical on Android and iOS:

| Field | Type | Description |
|-------|------|-------------|
| `id` | `string` | Product ID |
| `name` | `string` | Product name |
| `title` | `string` | Product title |
| `description` | `string` | Product description |
| `price` | `string` | Formatted price in the store's locale (e.g. "$0.99") |
| `priceMicros` | `int` | Price in micros (990000 = $0.99) |
| `currency` | `string` | Currency code ("USD", "EUR") |
| `billingPeriod` | `string` | Subscription period ("P1M" = monthly), `""` for one-time products |
| `type` | `string` | `"inapp"` or `"subs"` |

`productId`, `priceAmountMicros` and `priceCurrencyCode` are also present as aliases of `id`,
`priceMicros` and `currency`.

**Android subscriptions** additionally carry the offer catalogue, because one subscription
product can have several base plans and intro/free-trial offers:

| Field | Type | Description |
|-------|------|-------------|
| `offerToken` | `string` | Token of the default (first) offer — what `IAP.Purchase` uses when you pass nothing |
| `offers` | `table` | Array of `{ offerToken, basePlanId, offerId, phases }` |
| `offers[i].phases` | `table` | Array of `{ price, priceMicros, currency, billingPeriod, billingCycleCount, recurrenceMode }` — a free trial is a phase with `priceMicros == 0` |

The top-level `price` / `billingPeriod` of a subscription describe the **final recurring phase**,
not the intro offer, so a "£0 for 7 days then £4.99/month" plan shows £4.99/month in the shop.

`IAP.OnProductsQueried` always fires — with an empty table when the query fails — so a shop
screen never hangs. A failing query also emits `IAP.OnEvent("query_failed:CODE")`.

---

### 44.5 IAP.Purchase

```lua
IAP.Purchase(productId, offerToken?)
```

Launches the store purchase flow for the given product. `offerToken` is Android-only and picks a
specific subscription offer from `product.offers`; omit it to use the default offer. It is
ignored on iOS, where App Store Connect owns the intro-offer logic.

```lua
IAP.Purchase("coins_100")

IAP.OnPurchaseComplete(function(info)
    if info.status == "purchased" then
        Print("Purchased: " .. info.productId)
        -- For consumable items, consume to allow re-purchase:
        IAP.Consume(info.token)
        AddCoins(100)
    elseif info.status == "cancelled" then
        Print("Purchase cancelled")
    elseif info.status == "pending" then
        Print("Purchase pending approval")   -- do NOT grant anything yet
    else
        Print("Purchase failed: " .. info.status .. " " .. info.message)
    end
end)
```

A user backing out of the store sheet reports `status == "cancelled"` on **both** platforms —
do not show an error for it.

---

### 44.6 IAP.Consume and IAP.Acknowledge

```lua
IAP.Consume(purchaseToken)
IAP.Acknowledge(purchaseToken)
```

`IAP.Consume` consumes a purchase so the user can buy the same product again. Use it for
consumables (coins, gems, lives) and call it **after** you have granted the item. Non-consumables
("remove ads") must not be consumed.

`IAP.Acknowledge` explicitly acknowledges a purchase. The engine already acknowledges every
`PURCHASED` purchase it sees, including ones found by `IAP.RestorePurchases()`, so you rarely
need this — it exists for flows that acknowledge only after a server has validated the receipt.
Acknowledging twice is harmless; the engine drops duplicate in-flight requests.

> Google **auto-refunds any purchase left unacknowledged for three days**. That is why restore
> and reconciliation acknowledge as well as report.

---

### 44.7 IAP.RestorePurchases

```lua
IAP.RestorePurchases()
```

Restores previously purchased non-consumable items and active subscriptions, and acknowledges
any of them the store still considers unacknowledged. Results are delivered via
`IAP.OnPurchaseRestored`. Call it at startup (and behind a "Restore purchases" button, which
Apple requires) to re-apply entitlements after a reinstall or on a new device.

```lua
IAP.RestorePurchases()

IAP.OnPurchaseRestored(function(info)
    if info.status == "restored" then
        Print("Restored: " .. info.productId)
        UnlockContent(info.productId)
    elseif info.status == "subscription_active" then
        Print("Active subscription: " .. info.productId)
        EnableVIP()
    end
end)
```

Android reports one-time products as `restored` and subscriptions as `subscription_active`, then
`IAP.OnEvent("restore_complete")` and `IAP.OnEvent("restore_subs_complete")`. iOS reports
everything as `restored` followed by `IAP.OnEvent("restore_complete")`.

---

### 44.7a IAP.QueryPurchases

```lua
IAP.QueryPurchases()
```

Forces the reconciliation pass described in [44.2](#442-iapinit) right now, without the
`restored` events. Useful after returning from an external flow. It runs automatically on
connect and on app resume, so most games never call it.

---

### 44.7b Server-side receipt validation

For a donation-driven economy, validate on your server before granting anything valuable —
`IAP.OnPurchaseComplete` runs on a device the player controls. Every purchase callback carries
what your backend needs:

| Field | Platform | Description |
|-------|----------|-------------|
| `token` | both | Android purchase token / iOS transaction identifier |
| `orderId` | both | Google Play order ID / iOS original transaction identifier |
| `signature` | Android | RSA signature of `originalJson`, verified against your Play Console public key |
| `originalJson` | Android | The exact payload that `signature` signs — send both verbatim, never re-serialise |
| `receipt` | iOS | Base-64 App Store receipt (also available any time from `IAP.GetReceipt()`) |
| `purchaseTime` | both | Unix time in milliseconds |
| `quantity` | both | Units bought |
| `acknowledged` | Android | Whether Google Play already has an acknowledgement |
| `autoRenewing` | Android | Subscription auto-renew state |
| `success` | both | `false` for cancelled/failed results |
| `message` | both | Human-readable reason on failure |

`status`, `productId`, `success`, `quantity`, `acknowledged` and `autoRenewing` are always
present. The string fields and `purchaseTime` are **absent (`nil`)** when the platform or the
status does not provide them — `signature`/`originalJson` never appear on iOS, `receipt` never
on Android, and `token`/`orderId` are missing on a `cancelled` or `failed` result. Test them
with `if info.signature then`, not against `""`.

```lua
IAP.OnPurchaseComplete(function(info)
    if info.status ~= "purchased" then return end

    local payload = {
        platform     = Settings.GetPlatform(),
        productId    = info.productId,
        token        = info.token,
        orderId      = info.orderId,
        signature    = info.signature,      -- Android
        originalJson = info.originalJson,   -- Android
        receipt      = info.receipt,        -- iOS
    }
    SendToVerificationBackend(Network.JsonEncode(payload))   -- your own transport
end)
```

---

### 44.8 IAP.Destroy

```lua
IAP.Destroy()
```

Disconnects from the store, stops the reconnect loop, and clears callbacks.

---

### 44.9 Callbacks summary

| Callback | Parameters | Description |
|----------|------------|-------------|
| `IAP.OnInitialized(fn)` | `(success: bool)` | Store connected/failed |
| `IAP.OnEvent(fn)` | `(event: string)` | General events: `disconnected`, `consumed:TOKEN`, `consume_failed:CODE`, `acknowledged`, `acknowledge_failed:CODE`, `query_failed:CODE`, `restore_complete`, `restore_subs_complete`, `store_promotion:PRODUCT_ID` (iOS) |
| `IAP.OnProductsQueried(fn)` | `(products: table)` | Product details received (empty table on failure) |
| `IAP.OnPurchaseComplete(fn)` | `(info: table)` | Purchase result — statuses `purchased`, `pending`, `cancelled`, `failed:CODE` |
| `IAP.OnPurchaseRestored(fn)` | `(info: table)` | Restored purchase — statuses `restored`, `subscription_active` |
| `IAP.ClearCallbacks()` | — | Remove all callbacks |

Both `info` tables carry the full field set from [44.7b](#447b-server-side-receipt-validation).

> **iOS promoted in-app purchases.** When a player buys one of your products straight from the
> App Store product page, StoreKit hands the payment to the running game. The engine accepts it
> and raises `IAP.OnEvent("store_promotion:PRODUCT_ID")` first, then the normal
> `IAP.OnPurchaseComplete` when the transaction settles.

---

### 44.10 Practical example — a soft-currency shop

```lua
local shopProducts = {}

function OnCreate()
    if not IAP.IsSupported() then return end

    IAP.Init()
    IAP.OnInitialized(function(success)
        if success then
            IAP.QueryProducts({"gems_80", "gems_500", "gems_2000", "arena_pass"}, false)
            IAP.QueryProducts({"royale_pass_monthly"}, true)
            IAP.RestorePurchases()
        end
    end)

    IAP.OnProductsQueried(function(products)
        for _, p in ipairs(products) do
            shopProducts[p.id] = p
            Print(p.name .. ": " .. p.price)
        end
    end)

    IAP.OnPurchaseComplete(function(info)
        if info.status == "purchased" then
            if info.productId == "gems_80" then
                IAP.Consume(info.token)
                AddGems(80)
            elseif info.productId == "gems_500" then
                IAP.Consume(info.token)
                AddGems(500)
            elseif info.productId == "gems_2000" then
                IAP.Consume(info.token)
                AddGems(2000)
            elseif info.productId == "arena_pass" then
                UnlockArenaPass()
            end
        end
    end)

    IAP.OnPurchaseRestored(function(info)
        if info.productId == "arena_pass" then
            UnlockArenaPass()
        elseif info.status == "subscription_active" and info.productId == "royale_pass_monthly" then
            EnableRoyalePass()
        end
    end)
end

function BuyGems(amount)
    local productId = "gems_" .. amount
    if shopProducts[productId] then
        IAP.Purchase(productId)
    end
end
```

---

## 45. PlayGames — Google Play Games Services

### Overview

The **`PlayGames`** table provides a Lua API for **Google Play Games Services** —
sign-in, achievements, and leaderboards.

> **Platform:** Android (Google Play Games) and iOS (Game Center, via GameKit). On other platforms `PlayGames.IsSupported()` returns `false` and all calls are no-ops.
>
> **Cross-store note:** the `PlayGames` table maps to Game Center on iOS. Achievement and leaderboard IDs are configured per store — Google Play Console for Android, App Store Connect / Game Center for iOS. On iOS `IsSupported()` reports whether the app is signed with the `com.apple.developer.game-center` entitlement (it returns `true` in the Simulator and for unsigned builds, where no provisioning profile is embedded).
>
> **Achievement steps on iOS:** Game Center models progress as a 0–100 percentage, not as discrete steps. `IncrementAchievement(id, steps)` therefore adds `steps` percentage points to the achievement's current progress (clamped to 100), tracking the value locally and refreshing it from Game Center on sign-in.
>
> **Build requirement:** Enable "Google Play Games" in the Build Game popup and provide your Play Games App ID.

---

### 45.1 PlayGames.IsSupported

```lua
PlayGames.IsSupported() -> bool
```

Returns `true` if the current platform supports Google Play Games.

---

### 45.2 PlayGames.Init

```lua
PlayGames.Init()
PlayGames.IsInitialized() -> bool
```

Initializes the Play Games SDK and attempts automatic sign-in.
`PlayGames.IsInitialized()` reports whether the SDK finished initializing — unlike
`PlayGames.IsSupported()`, which only tells you the platform *could* run it.

```lua
PlayGames.Init()

PlayGames.OnSignIn(function(success, message)
    if success then
        Print("Signed in to Google Play Games!")
    else
        Print("Sign-in failed: " .. message)
    end
end)

PlayGames.OnPlayerInfo(function(playerId, playerName)
    Print("Player: " .. playerName .. " (" .. playerId .. ")")
end)
```

---

### 45.3 Sign-In

```lua
PlayGames.SignIn()                  -- Trigger manual sign-in
PlayGames.IsSignedIn() -> bool      -- Check sign-in status
PlayGames.GetPlayerId() -> string   -- Get signed-in player ID
PlayGames.GetPlayerName() -> string -- Get signed-in player display name
```

---

### 45.4 Achievements

```lua
PlayGames.UnlockAchievement(achievementId)             -- Unlock an achievement
PlayGames.IncrementAchievement(achievementId, steps)   -- Increment progress
PlayGames.RevealAchievement(achievementId)             -- Reveal a hidden achievement
PlayGames.ShowAchievements()                           -- Show achievements UI
```

```lua
-- Player defeated 100 enemies
enemyKills = enemyKills + 1
if enemyKills >= 10 then
    PlayGames.UnlockAchievement("CgkI...AQ") -- "First Blood"
end
PlayGames.IncrementAchievement("CgkI...BQ", 1) -- "Slayer" (incremental)

-- Show all achievements
function OnAchievementsButton()
    PlayGames.ShowAchievements()
end
```

---

### 45.5 Leaderboards

```lua
PlayGames.SubmitScore(leaderboardId, score)    -- Submit a score (supports large values)
PlayGames.ShowLeaderboard(leaderboardId)       -- Show specific leaderboard
PlayGames.ShowAllLeaderboards()                -- Show all leaderboards
```

```lua
function OnGameOver()
    PlayGames.SubmitScore("CgkI...CQ", totalScore)
end

function OnLeaderboardButton()
    PlayGames.ShowLeaderboard("CgkI...CQ")
end
```

---

### 45.6 PlayGames.Destroy

```lua
PlayGames.Destroy()
```

Cleans up Play Games resources and callbacks.

---

### 45.7 Callbacks summary

| Callback | Parameters | Description |
|----------|------------|-------------|
| `PlayGames.OnSignIn(fn)` | `(success: bool, message: string)` | Sign-in result |
| `PlayGames.OnPlayerInfo(fn)` | `(playerId: string, playerName: string)` | Player info received after sign-in |
| `PlayGames.OnEvent(fn)` | `(event: string)` | General events: `achievement_unlocked:ID`, `score_submitted:ID` |
| `PlayGames.ClearCallbacks()` | — | Remove all callbacks |

---

### 45.8 Practical example — endless-runner integration

```lua
local highScore = 0
local runCount = 0


function OnCreate()
    if PlayGames.IsSupported() then
        PlayGames.Init()
        PlayGames.OnSignIn(function(success, msg)
            if success then
                Print("Welcome back, " .. PlayGames.GetPlayerName() .. "!")
            end
        end)
    end

    if Ads.IsSupported() then
        Ads.Init()
        Ads.OnInitialized(function(ok)
            if ok then
                Ads.SetRewardedUnitId("ca-app-pub-XXXXX/RRRRR")
                Ads.LoadRewarded()
                Ads.SetInterstitialUnitId("ca-app-pub-XXXXX/IIIII")
                Ads.LoadInterstitial()
            end
        end)
        Ads.OnRewardEarned(function(type, amount)
            ContinueRun()
        end)
    end
end

function OnGameOver()
    if PlayGames.IsSignedIn() then
        PlayGames.SubmitScore("CgkI...HIGHSCORE", highScore)
    end
    if highScore >= 1000 then
        PlayGames.UnlockAchievement("CgkI...RUN1000")
    end

    runCount = runCount + 1
    if runCount % 3 == 0 and Ads.IsInterstitialReady() then
        Ads.ShowInterstitial()
    end
end

function OnContinueButton()
    if Ads.IsRewardedReady() then
        Ads.ShowRewarded()
    else
        Print("No ad available")
    end
end
```

---

## 46. SavedGames — Google Play Saved Games

### Overview

The **`SavedGames`** table provides a Lua API for **cloud saves** via **Google Play Games Services Saved Games**.
Allows saving and loading game data to the cloud, deleting saves, and showing the built-in saved games UI.

> **Platform:** Android (Google Play Saved Games) and iOS (GameKit / iCloud). On other platforms `SavedGames.IsSupported()` returns `false` and all calls are no-ops.
>
> **iOS note:** saves go through GameKit, so the player must be signed in to Game Center — `SavedGames.Init()` raises `OnError("", "not_signed_in")` when they are not. `SavedGames.ShowUI()` has no native iOS equivalent and raises `OnError("", "no_system_saved_games_ui_on_ios")`; build your own slot picker from the data you saved.
>
> **Build requirement:** Enable "Google Play Games" with Saved Games support in the Build Game popup.

---

### 46.1 SavedGames.IsSupported

```lua
SavedGames.IsSupported() -> bool
```

Returns `true` if the current platform supports Saved Games (Android with saved games enabled).

```lua
if SavedGames.IsSupported() then
    SavedGames.Init()
end
```

---

### 46.2 SavedGames.Init

```lua
SavedGames.Init()
```

Initializes the Saved Games service. Must be called once before other `SavedGames` functions.

---

### 46.3 SavedGames.Save

```lua
SavedGames.Save(slotName, data, description?)
```

Saves data to a named slot in the cloud.

| Parameter | Type | Description |
|-----------|------|-------------|
| `slotName` | `string` | Unique slot name (e.g. `"save_1"`) |
| `data` | `string` | Data to save (JSON string, serialized state, etc.) |
| `description` | `string` | Optional description shown in the UI (default: `""`) |

```lua
local saveData = '{"level":5,"coins":1200,"hp":80}'
SavedGames.Save("save_main", saveData, "Level 5 — 1200 coins")

SavedGames.OnSaved(function(slotName)
    Print("Saved to cloud: " .. slotName)
end)
```

---

### 46.4 SavedGames.Load

```lua
SavedGames.Load(slotName)
```

Loads data from a named slot. The result is delivered via `SavedGames.OnLoaded`.

```lua
SavedGames.Load("save_main")

SavedGames.OnLoaded(function(slotName, data)
    Print("Loaded from " .. slotName .. ": " .. data)
    local save = ParseJson(data)
    RestoreGameState(save)
end)
```

---

### 46.5 SavedGames.Delete

```lua
SavedGames.Delete(slotName)
```

Deletes a saved game slot from the cloud.

```lua
SavedGames.Delete("save_main")

SavedGames.OnDeleted(function(slotName)
    Print("Deleted: " .. slotName)
end)
```

---

### 46.6 SavedGames.ShowUI

```lua
SavedGames.ShowUI()
```

Opens the built-in Google Play Saved Games UI where the player can view and manage their saves.

---

### 46.7 SavedGames.Destroy

```lua
SavedGames.Destroy()
```

Cleans up Saved Games resources and clears all callbacks.

---

### 46.8 Callbacks summary

| Callback | Parameters | Description |
|----------|------------|-------------|
| `SavedGames.OnLoaded(fn)` | `(slotName: string, data: string)` | Data loaded from a slot |
| `SavedGames.OnSaved(fn)` | `(slotName: string)` | Data saved to a slot |
| `SavedGames.OnDeleted(fn)` | `(slotName: string)` | Slot deleted |
| `SavedGames.OnError(fn)` | `(slotName: string, message: string)` | Error occurred |
| `SavedGames.ClearCallbacks()` | — | Remove all callbacks |

---

### 46.9 Practical example — RPG cloud save

```lua
function OnCreate()
    if not SavedGames.IsSupported() then return end

    SavedGames.Init()

    SavedGames.OnLoaded(function(slot, data)
        local save = ParseJson(data)
        playerLevel = save.level or 1
        playerCoins = save.coins or 0
        Print("Cloud save loaded: Level " .. playerLevel)
    end)

    SavedGames.OnSaved(function(slot)
        Print("Progress saved to cloud!")
    end)

    SavedGames.OnError(function(slot, msg)
        Print("SavedGames error [" .. slot .. "]: " .. msg)
    end)

    -- Load existing save on start
    SavedGames.Load("rpg_save")
end

function SaveProgress()
    local data = '{"level":' .. playerLevel .. ',"coins":' .. playerCoins .. '}'
    SavedGames.Save("rpg_save", data, "Level " .. playerLevel)
end
```

---

## 47. Firebase — Firebase Analytics

### Overview

The **`Firebase`** table provides a Lua API for **Firebase Analytics** — event logging, user properties, and screen tracking.

> **Platform:** Android. The engine bundles no Firebase SDK for iOS, so `Firebase.IsSupported()` returns `false` there and all calls are no-ops. On desktop and Web it returns `false` too.
>
> **Build requirement:** Enable "Firebase" in the Build Game popup and place `google-services.json` in the project.

---

### 47.1 Firebase.IsSupported

```lua
Firebase.IsSupported() -> bool
```

Returns `true` if the current platform supports Firebase (Android with Firebase enabled).

```lua
if Firebase.IsSupported() then
    Firebase.Init()
end
```

---

### 47.2 Firebase.Init

```lua
Firebase.Init()
```

Initializes the Firebase SDK. Must be called once before other `Firebase` functions.

---

### 47.3 Firebase.LogEvent

```lua
Firebase.LogEvent(name, params?)
```

Logs an analytics event with optional parameters.

| Parameter | Type | Description |
|-----------|------|-------------|
| `name` | `string` | Event name (e.g. `"level_complete"`) |
| `params` | `table` | Optional key-value table of event parameters |

Parameter values can be `string`, `int`, `double`, or `bool`.

```lua
-- Simple event
Firebase.LogEvent("game_start")

-- Event with parameters
Firebase.LogEvent("level_complete", {
    level = 5,
    score = 12500,
    time_seconds = 45.3,
    used_hint = false
})

-- Purchase tracking
Firebase.LogEvent("purchase", {
    item_id = "sword_of_fire",
    item_name = "Sword of Fire",
    price = 2.99,
    currency = "USD"
})
```

---

### 47.4 Firebase.SetUserId

```lua
Firebase.SetUserId(userId)
```

Sets the user ID for analytics. Useful for cross-device tracking.

```lua
Firebase.SetUserId("player_12345")
```

---

### 47.5 Firebase.SetUserProperty

```lua
Firebase.SetUserProperty(name, value)
```

Sets a custom user property.

```lua
Firebase.SetUserProperty("favorite_class", "warrior")
Firebase.SetUserProperty("vip_status", "gold")
```

---

### 47.6 Firebase.SetScreenName

```lua
Firebase.SetScreenName(screenName)
```

Sets the current screen name for screen-view tracking.

```lua
Firebase.SetScreenName("MainMenu")
Firebase.SetScreenName("Shop")
Firebase.SetScreenName("Level_5")
```

---

### 47.7 Firebase.Destroy

```lua
Firebase.Destroy()
```

Cleans up Firebase resources.

---

### 47.8 API reference

| Function | Returns | Description |
|----------|---------|-------------|
| `Firebase.IsSupported()` | `bool` | Check if Firebase is available |
| `Firebase.Init()` | — | Initialize Firebase SDK |
| `Firebase.LogEvent(name, params?)` | — | Log an analytics event |
| `Firebase.SetUserId(userId)` | — | Set user ID for analytics |
| `Firebase.SetUserProperty(name, value)` | — | Set a custom user property |
| `Firebase.SetScreenName(screenName)` | — | Set current screen name |
| `Firebase.Destroy()` | — | Clean up Firebase resources |

---

### 47.9 Practical example — game analytics

```lua
function OnCreate()
    if not Firebase.IsSupported() then return end

    Firebase.Init()
    Firebase.SetScreenName("MainMenu")
    Firebase.LogEvent("app_open")
end

function OnLevelStart(level)
    Firebase.SetScreenName("Level_" .. level)
    Firebase.LogEvent("level_start", { level = level })
end

function OnLevelComplete(level, score, time)
    Firebase.LogEvent("level_complete", {
        level = level,
        score = score,
        time_seconds = time,
        stars = CalculateStars(score)
    })
end

function OnPlayerDeath(level, cause)
    Firebase.LogEvent("player_death", {
        level = level,
        cause = cause
    })
end

function OnPurchase(itemId, price)
    Firebase.LogEvent("in_app_purchase", {
        item_id = itemId,
        price = price,
        currency = "USD"
    })
end
```

---

## 48. Notifications — Push and Local Notifications

### Overview

The **`Notifications`** table provides a Lua API for **local notifications** and **push notification** tokens via Firebase Cloud Messaging (FCM).
Schedule local notifications with optional delay, cancel them, and request the notification permission on Android 13+.

> **Platform:** Android and iOS (local notifications via UserNotifications). On other platforms `Notifications.IsSupported()` returns `false` and all calls are no-ops.
>
> **iOS note:** local notifications are fully supported, including `OnShown` (foreground presentation) and `OnClicked`. `Notifications.GetToken()` returns the APNs device token as a hex string once `Notifications.Init()` has registered for remote notifications — that only happens when the app is signed with the `aps-environment` entitlement (Build Game → iOS → *Push Notifications*); without it the token stays `""`. Note that `delaySec = 0` fires immediately on iOS instead of being clamped to one second.
>
> **Build requirement:** Enable "Notifications" in the Build Game popup.

---

### 48.1 Notifications.IsSupported

```lua
Notifications.IsSupported() -> bool
```

Returns `true` if the current platform supports notifications.

---

### 48.2 Notifications.Init

```lua
Notifications.Init()
```

Initializes the notifications system. Must be called once before other `Notifications` functions.

---

### 48.3 Notifications.RequestPermission

```lua
Notifications.RequestPermission()
```

Requests the `POST_NOTIFICATIONS` permission (required on Android 13+).
The result is delivered via `Notifications.OnPermissionResult`.

```lua
Notifications.RequestPermission()

Notifications.OnPermissionResult(function(granted)
    if granted then
        Print("Notification permission granted!")
    else
        Print("Notification permission denied")
    end
end)
```

---

### 48.4 Notifications.ShowLocal

```lua
Notifications.ShowLocal(title, message, delaySec?, id?)
```

Shows or schedules a local notification.

| Parameter | Type | Description |
|-----------|------|-------------|
| `title` | `string` | Notification title |
| `message` | `string` | Notification body text |
| `delaySec` | `int` | Delay in seconds before showing (default: `0` = immediate) |
| `id` | `int` | Notification ID for cancellation (default: `0`) |

```lua
-- Show immediately
Notifications.ShowLocal("Game", "Your energy is full!")

-- Schedule in 30 minutes
Notifications.ShowLocal("Game", "Come back and play!", 1800, 1)

-- Schedule daily reminder (86400 seconds)
Notifications.ShowLocal("Game", "Daily reward is waiting!", 86400, 2)
```

---

### 48.5 Notifications.CancelLocal

```lua
Notifications.CancelLocal(id)
```

Cancels a scheduled local notification by its ID.

```lua
Notifications.CancelLocal(1) -- Cancel notification with ID 1
```

---

### 48.6 Notifications.CancelAll

```lua
Notifications.CancelAll()
```

Cancels all scheduled local notifications.

---

### 48.7 Notifications.GetToken

```lua
Notifications.GetToken() -> string
```

Returns the FCM push notification token. Returns `""` if not yet available.

```lua
local token = Notifications.GetToken()
if token ~= "" then
    Print("FCM token: " .. token)
end
```

---

### 48.8 Notifications.Destroy

```lua
Notifications.Destroy()
```

Cleans up notification resources and clears all callbacks.

---

### 48.9 Callbacks summary

| Callback | Parameters | Description |
|----------|------------|-------------|
| `Notifications.OnPermissionResult(fn)` | `(granted: bool)` | Permission request result |
| `Notifications.OnShown(fn)` | `(data: string)` | Notification was shown |
| `Notifications.OnClicked(fn)` | `(data: string)` | User clicked a notification |
| `Notifications.OnTokenReceived(fn)` | `(token: string)` | FCM token received/refreshed |
| `Notifications.ClearCallbacks()` | — | Remove all callbacks |

---

### 48.10 Practical example — engagement notifications

```lua
function OnCreate()
    if not Notifications.IsSupported() then return end

    Notifications.Init()
    Notifications.RequestPermission()

    Notifications.OnPermissionResult(function(granted)
        if granted then
            -- Schedule a "come back" reminder in 24 hours
            Notifications.ShowLocal("Adventure Awaits!", "Your heroes miss you!", 86400, 100)
        end
    end)

    Notifications.OnClicked(function(data)
        Print("Player opened game from notification")
        GiveLoginBonus()
    end)

    Notifications.OnTokenReceived(function(token)
        Print("FCM token: " .. token)
        -- Send token to your server for push notifications
    end)
end

function OnEnergyFull()
    Notifications.ShowLocal("Energy Full!", "Your energy is fully recharged!", 0, 50)
end
```

---

## 49. Consent — GDPR Consent (UMP)

### Overview

The **`Consent`** table provides a Lua API for **GDPR consent management** via Google's **User Messaging Platform (UMP)**.
Shows consent forms, checks consent status, and determines whether ads can be shown with personalization.

> **Platform:** Android (Google UMP) and iOS (App Tracking Transparency). On iOS `Consent.ShowForm()` presents the system ATT prompt and `Consent.GetStatus()` maps the ATT status onto the same vocabulary as UMP: `"required"` (not yet asked), `"obtained"` (user answered), `"not_required"` (restricted / pre-iOS 14). `Consent.Reset()` has no iOS equivalent and raises an `OnError` with `"reset_unsupported_on_ios"`. On desktop and Web `Consent.IsSupported()` returns `false`.
>
> **Build requirement:** Enable "Consent (UMP)" in the Build Game popup.

---

### 49.1 Consent.IsSupported

```lua
Consent.IsSupported() -> bool
```

Returns `true` if the current platform supports consent management.

---

### 49.2 Consent.Init

```lua
Consent.Init(debugGeography?)
```

Initializes the UMP SDK and requests consent information.

| Parameter | Type | Description |
|-----------|------|-------------|
| `debugGeography` | `bool` | If `true`, simulates EEA geography for testing (default: `false`) |

```lua
Consent.Init()

Consent.OnInfoUpdated(function(status, canShowForm)
    Print("Consent status: " .. status)
    if canShowForm then
        Consent.ShowForm()
    end
end)
```

---

### 49.3 Consent.ShowForm

```lua
Consent.ShowForm()
```

Shows the consent form to the user. The result is delivered via `Consent.OnFormDismissed`.

```lua
Consent.ShowForm()

Consent.OnFormDismissed(function(status)
    Print("Form dismissed, status: " .. status)
    if Consent.CanShowAds() then
        Ads.Init()
    end
end)
```

---

### 49.4 Consent.GetStatus

```lua
Consent.GetStatus() -> string
```

Returns the current consent status string (e.g. `"OBTAINED"`, `"REQUIRED"`, `"NOT_REQUIRED"`, `"UNKNOWN"`).

---

### 49.5 Consent.CanShowAds

```lua
Consent.CanShowAds() -> bool
```

Returns `true` if the user has given consent to show ads (personalized or non-personalized).

```lua
if Consent.CanShowAds() then
    Ads.Init()
end
```

---

### 49.6 Consent.Reset

```lua
Consent.Reset()
```

Resets the consent state. Useful for testing or allowing the user to change their consent choice.

---

### 49.7 Consent.Destroy

```lua
Consent.Destroy()
```

Cleans up consent resources and clears all callbacks.

---

### 49.8 Callbacks summary

| Callback | Parameters | Description |
|----------|------------|-------------|
| `Consent.OnInfoUpdated(fn)` | `(status: string, canShowForm: bool)` | Consent info updated |
| `Consent.OnFormDismissed(fn)` | `(status: string)` | Consent form dismissed by user |
| `Consent.OnError(fn)` | `(message: string)` | Error occurred |
| `Consent.ClearCallbacks()` | — | Remove all callbacks |

---

### 49.9 Practical example — GDPR-compliant ad initialization

```lua
function OnCreate()
    if Consent.IsSupported() then
        Consent.Init()

        Consent.OnInfoUpdated(function(status, canShowForm)
            if canShowForm then
                Consent.ShowForm()
            elseif Consent.CanShowAds() then
                InitAds()
            end
        end)

        Consent.OnFormDismissed(function(status)
            if Consent.CanShowAds() then
                InitAds()
            end
        end)

        Consent.OnError(function(msg)
            Print("Consent error: " .. msg)
            -- Fallback: init ads without personalization
            InitAds()
        end)
    else
        -- Non-Android platform, just init ads
        InitAds()
    end
end

function InitAds()
    if Ads.IsSupported() then
        Ads.Init()
        Ads.OnInitialized(function(ok)
            if ok then
                Ads.SetBannerUnitId("ca-app-pub-XXXXX/YYYYY")
                Ads.ShowBanner(Ads.BANNER_BOTTOM)
            end
        end)
    end
end
```

---

## 50. Review — In-App Review

### Overview

The **`Review`** table provides a Lua API for **in-app reviews** via the Google Play In-App Review API.
Prompts the user to rate the game without leaving the app.

> **Platform:** Android (In-App Review) and iOS (`SKStoreReviewController`). On other platforms `Review.IsSupported()` returns `false` and all calls are no-ops.
>
> **Build requirement:** Enable "In-App Review" in the Build Game popup.
>
> **Note:** Google controls when the review dialog is actually shown. The API request may be silently ignored
> if the user has already reviewed or if too many requests were made recently.

---

### 50.1 Review.IsSupported

```lua
Review.IsSupported() -> bool
```

Returns `true` if the current platform supports in-app review.

---

### 50.2 Review.Request

```lua
Review.Request()
```

Requests the in-app review flow. Google may or may not show the dialog.

```lua
Review.Request()

Review.OnLaunched(function()
    Print("Review dialog shown")
end)

Review.OnCompleted(function()
    Print("Review flow completed")
end)

Review.OnError(function(msg)
    Print("Review error: " .. msg)
end)
```

---

### 50.3 Review.Destroy

```lua
Review.Destroy()
```

Clears all callbacks.

---

### 50.4 Callbacks summary

| Callback | Parameters | Description |
|----------|------------|-------------|
| `Review.OnLaunched(fn)` | — | Review dialog was shown |
| `Review.OnCompleted(fn)` | — | Review flow completed |
| `Review.OnError(fn)` | `(message: string)` | Error occurred |
| `Review.ClearCallbacks()` | — | Remove all callbacks |

---

### 50.5 Practical example — ask for review after level 10

```lua
local hasAskedReview = false

function OnLevelComplete(level)
    if not hasAskedReview and level >= 10 and Review.IsSupported() then
        hasAskedReview = true

        Review.OnCompleted(function()
            Print("Thanks for the review!")
        end)

        Review.Request()
    end
end
```

---

## 51. Bluetooth — Bluetooth Communication

### Overview

The **`Bluetooth`** table provides a Lua API for **Bluetooth Classic** communication —
device discovery, connection, and data transfer between devices.

> **Platform:** Android (RFCOMM / Bluetooth Classic) and iOS (Bluetooth LE via CoreBluetooth). The Lua API is identical on both; the transport is not. On iOS a host advertises a GATT service and peers connect as centrals, so `address` is a CoreBluetooth peripheral/central UUID string rather than a MAC address, and `Bluetooth.RequestEnable()` cannot toggle the radio — it opens the app's Settings page and raises an error event. iOS frames every `Send` with a length prefix, so one `Send` always arrives as exactly one `OnDataReceived`; Android streams RFCOMM bytes and may split or coalesce. **Android and iOS peers cannot talk to each other.** On desktop and Web `Bluetooth.IsSupported()` returns `false`.
>
> **Build requirement:** Enable "Bluetooth" in the Build Game popup.
>
> **Permissions:** Requires `BLUETOOTH_CONNECT`, `BLUETOOTH_SCAN`, and `BLUETOOTH_ADVERTISE` (for hosting) permissions (request via `Permissions` API on Android 12+).

---

### 51.1 Bluetooth.IsSupported

```lua
Bluetooth.IsSupported() -> bool
```

Returns `true` if the current platform supports Bluetooth.

---

### 51.2 Bluetooth.Init

```lua
Bluetooth.Init()
```

Initializes the Bluetooth adapter. Must be called once before other `Bluetooth` functions.

---

### 51.3 Bluetooth availability

```lua
Bluetooth.IsAvailable() -> bool    -- Check if Bluetooth hardware is available
Bluetooth.IsEnabled() -> bool      -- Check if Bluetooth is currently turned on
Bluetooth.RequestEnable()          -- Request the user to enable Bluetooth
```

---

### 51.4 Device discovery

```lua
Bluetooth.StartDiscovery()         -- Start scanning for nearby devices
Bluetooth.StopDiscovery()          -- Stop scanning
```

Discovered devices are reported via `Bluetooth.OnDeviceFound`.

```lua
Bluetooth.StartDiscovery()

Bluetooth.OnDeviceFound(function(deviceName, deviceAddress)
    Print("Found: " .. deviceName .. " [" .. deviceAddress .. "]")
end)
```

---

### 51.5 Connection

```lua
Bluetooth.Connect(address)        -- Connect to a device by MAC address
Bluetooth.Disconnect()            -- Disconnect all peers
Bluetooth.IsConnected() -> bool   -- Check if any peer is connected
```

```lua
Bluetooth.Connect("AA:BB:CC:DD:EE:FF")

Bluetooth.OnConnected(function(deviceName, deviceAddress)
    Print("Connected to " .. deviceName)
end)

Bluetooth.OnDisconnected(function(deviceName, deviceAddress)
    Print("Disconnected from " .. deviceName)
end)
```

---

### 51.6 Multi-peer host mode

Bluetooth supports **multi-peer** connections via a host/client model.
The host opens a server socket and accepts multiple incoming connections.

```lua
Bluetooth.StartHost()             -- Start hosting (accept incoming connections)
Bluetooth.StopHost()              -- Stop hosting (close server socket)
Bluetooth.IsHosting() -> bool     -- Check if currently hosting
```

```lua
-- Host device
Bluetooth.StartHost()

Bluetooth.OnHostStarted(function()
    Print("Hosting started — waiting for players...")
end)

Bluetooth.OnHostStopped(function()
    Print("Hosting stopped")
end)

Bluetooth.OnConnected(function(name, address)
    Print("Player joined: " .. name .. " [" .. address .. "]")
    Print("Total players: " .. Bluetooth.GetPeerCount())
end)
```

---

### 51.7 Data transfer

```lua
Bluetooth.Send(data)              -- Send a string to all connected peers (legacy alias)
Bluetooth.SendTo(address, data)   -- Send a string to a specific peer
Bluetooth.SendToAll(data)         -- Send a string to all connected peers
```

```lua
-- Send to everyone
Bluetooth.SendToAll("Hello from IceBox!")

-- Send to a specific peer
Bluetooth.SendTo("AA:BB:CC:DD:EE:FF", "Private message")

Bluetooth.OnDataReceived(function(data)
    Print("Received: " .. data)
end)
```

---

### 51.8 Peer management

```lua
Bluetooth.DisconnectPeer(address)              -- Disconnect a specific peer
Bluetooth.IsPeerConnected(address) -> bool     -- Check if a specific peer is connected
Bluetooth.GetPeerCount() -> int                -- Get the number of connected peers
Bluetooth.GetConnectedAddresses() -> string    -- Get JSON array of connected MAC addresses
```

```lua
local count = Bluetooth.GetPeerCount()
Print("Connected peers: " .. count)

local json = Bluetooth.GetConnectedAddresses()
Print("Addresses: " .. json)  -- e.g. ["AA:BB:CC:DD:EE:FF","11:22:33:44:55:66"]

if Bluetooth.IsPeerConnected("AA:BB:CC:DD:EE:FF") then
    Bluetooth.DisconnectPeer("AA:BB:CC:DD:EE:FF")
end
```

---

### 51.9 Bluetooth.Destroy

```lua
Bluetooth.Destroy()
```

Disconnects, cleans up Bluetooth resources, and clears all callbacks.

---

### 51.10 Callbacks summary

| Callback | Parameters | Description |
|----------|------------|-------------|
| `Bluetooth.OnStateChanged(fn)` | `(state: string)` | Bluetooth adapter state changed |
| `Bluetooth.OnDeviceFound(fn)` | `(deviceName: string, deviceAddress: string)` | Device discovered |
| `Bluetooth.OnConnected(fn)` | `(deviceName: string, deviceAddress: string)` | Peer connected |
| `Bluetooth.OnDisconnected(fn)` | `(deviceName: string, deviceAddress: string)` | Peer disconnected |
| `Bluetooth.OnDataReceived(fn)` | `(data: string)` | Data received from a peer |
| `Bluetooth.OnHostStarted(fn)` | — | Host mode started successfully |
| `Bluetooth.OnHostStopped(fn)` | — | Host mode stopped |
| `Bluetooth.OnError(fn)` | `(message: string)` | Error occurred |
| `Bluetooth.ClearCallbacks()` | — | Remove all callbacks |

---

### 51.11 Practical example — Bluetooth multiplayer tic-tac-toe (multi-peer)

```lua
local isHost = false
local peers = 0

function OnCreate()
    if not Bluetooth.IsSupported() then return end

    -- Request Bluetooth permissions first
    Permissions.Request(Permissions.BLUETOOTH_CONNECT)
    Permissions.Request(Permissions.BLUETOOTH_SCAN)
    Permissions.Request(Permissions.BLUETOOTH_ADVERTISE)

    Bluetooth.Init()

    Bluetooth.OnDeviceFound(function(name, address)
        Print("Found device: " .. name)
        if not isHost then
            Bluetooth.Connect(address)
        end
    end)

    Bluetooth.OnConnected(function(name, address)
        peers = Bluetooth.GetPeerCount()
        Print("Player joined: " .. name .. " (total: " .. peers .. ")")
    end)

    Bluetooth.OnDisconnected(function(name, address)
        peers = Bluetooth.GetPeerCount()
        Print("Player left: " .. name .. " (remaining: " .. peers .. ")")
    end)

    Bluetooth.OnHostStarted(function()
        Print("Hosting started — waiting for players...")
    end)

    Bluetooth.OnHostStopped(function()
        Print("Hosting stopped")
    end)

    Bluetooth.OnDataReceived(function(data)
        local cell = tonumber(data:match("move:(%d+)"))
        if cell then
            PlaceOpponentMark(cell)
        end
    end)

    Bluetooth.OnError(function(msg)
        Print("Bluetooth error: " .. msg)
    end)
end

function OnCellClicked(cellIndex)
    if Bluetooth.IsConnected() then
        Bluetooth.SendToAll("move:" .. cellIndex)
        PlaceMyMark(cellIndex)
    end
end

function StartHosting()
    isHost = true
    Bluetooth.StartHost()
end

function JoinGame()
    isHost = false
    Bluetooth.StartDiscovery()
end
```

---

## 52. DeepLinks — Deep Linking

### Overview

The **`DeepLinks`** table provides a Lua API for **deep linking** (App Links / Intent URIs) —
allows your game to be opened via custom URLs and react to the incoming data.

> **Platform:** Android (intent filter) and iOS (custom URL scheme). Set the scheme in Build Game → iOS → *Deep Link URL Scheme*; it is registered in `CFBundleURLTypes` and opening `yourscheme://...` delivers the full URI to `OnReceived` (the second argument carries the query string). On desktop and Web `DeepLinks.IsSupported()` returns `false`.
>
> **Configuration:** Register your deep link scheme/host in `AndroidManifest.xml` via the Build Game settings.

---

### 52.1 DeepLinks.IsSupported

```lua
DeepLinks.IsSupported() -> bool
```

Returns `true` if the current platform supports deep links.

---

### 52.2 DeepLinks.Init

```lua
DeepLinks.Init()
```

Initializes the deep links listener. Must be called once before other `DeepLinks` functions.

---

### 52.3 DeepLinks.GetLastUri

```lua
DeepLinks.GetLastUri() -> string
```

Returns the URI that launched the app (or the last received deep link). Returns `""` if none.

```lua
local uri = DeepLinks.GetLastUri()
if uri ~= "" then
    Print("App opened via: " .. uri)
end
```

---

### 52.4 Callbacks

```lua
DeepLinks.OnReceived(function(uri, data)
    Print("Deep link received: " .. uri)
    Print("Data: " .. data)
end)
```

| Callback | Parameters | Description |
|----------|------------|-------------|
| `DeepLinks.OnReceived(fn)` | `(uri: string, data: string)` | Deep link received |
| `DeepLinks.ClearCallbacks()` | — | Remove all callbacks |

---

### 52.5 Practical example — referral system

```lua
function OnCreate()
    if not DeepLinks.IsSupported() then return end

    DeepLinks.Init()

    -- Check if app was opened via a deep link
    local uri = DeepLinks.GetLastUri()
    if uri ~= "" then
        HandleDeepLink(uri)
    end

    -- Listen for deep links while app is running
    DeepLinks.OnReceived(function(uri, data)
        HandleDeepLink(uri)
    end)
end

function HandleDeepLink(uri)
    -- Example: mygame://invite?code=ABC123
    local code = uri:match("code=(%w+)")
    if code then
        Print("Referral code: " .. code)
        ApplyReferralBonus(code)
    end

    -- Example: mygame://level/5
    local level = uri:match("level/(%d+)")
    if level then
        LoadLevel(tonumber(level))
    end
end
```

---

## 53. Permissions — Android Runtime Permissions

### Overview

The **`Permissions`** table provides a Lua API for requesting **Android runtime permissions**, opening system settings screens for "special" permissions, checking notification and storage state, and resolving common Android storage paths.
Includes built-in constants for every permission declared by the engine in `AndroidManifest.xml`, plus a `Permissions.Dirs` subtable with public storage directory names.

> **Platform:** Android and iOS. The same `Permissions.CAMERA`, `Permissions.RECORD_AUDIO`, … constants work on both — on iOS the `android.permission.` prefix is stripped and the name is mapped onto the matching iOS authorization API (AVFoundation, Photos, CoreLocation, UserNotifications, CoreBluetooth, CoreMotion, App Tracking Transparency, Contacts, EventKit), so cross-platform scripts need no branching. Android-only concepts (`HasAllFilesAccess`, `CanDrawOverlays`, `CanWriteSettings`, `CanRequestInstallPackages`, `IsIgnoringBatteryOptimizations`, `IsNotificationPolicyAccessGranted`) return `false` on iOS and their request calls are no-ops; `ShouldShowRationale()` on iOS means "the user denied this once — explain why and send them to Settings", because iOS never re-prompts. Every permission you actually request needs a matching purpose string in Build Game → iOS → *Extra Usage Descriptions*. On desktop and Web `Permissions.IsSupported()` returns `false` and all calls are no-ops (numeric getters return `0`, string getters return `""`, boolean checks return `false`).

---

### 53.1 Permissions.IsSupported

```lua
Permissions.IsSupported() -> bool
```

Returns `true` if the current platform supports runtime permissions.

---

### 53.2 Permissions.Request

```lua
Permissions.Request(permission)
```

Requests a single permission. For "normal" and "dangerous" permissions the standard system dialog is shown and the result is delivered via `Permissions.OnResult`. For "special" permissions (`MANAGE_EXTERNAL_STORAGE`, `SYSTEM_ALERT_WINDOW`, `WRITE_SETTINGS`, `REQUEST_INSTALL_PACKAGES`, `SCHEDULE_EXACT_ALARM`, `USE_EXACT_ALARM`, `REQUEST_IGNORE_BATTERY_OPTIMIZATIONS`, `ACCESS_NOTIFICATION_POLICY`) the system settings screen for that permission is opened instead — there is no `OnResult` callback; check the current state with `Permissions.Has()` or with the dedicated `Can*`/`Has*`/`Is*` accessors after the user returns from settings.

```lua
Permissions.Request(Permissions.CAMERA)

Permissions.OnResult(function(permission, granted)
    if granted then
        Print(permission .. " granted!")
    else
        Print(permission .. " denied")
    end
end)
```

---

### 53.3 Permissions.RequestMultiple

```lua
Permissions.RequestMultiple(permissions)
```

Requests multiple permissions at once. Already-granted entries fire `OnResult(permission, true)` immediately; "special" permissions are dispatched one-by-one to their settings screens; the remaining runtime permissions go into a single batched system dialog.

```lua
Permissions.RequestMultiple({
    Permissions.CAMERA,
    Permissions.RECORD_AUDIO,
    Permissions.ACCESS_FINE_LOCATION
})
```

---

### 53.4 Permissions.Has

```lua
Permissions.Has(permission) -> bool
```

Returns `true` if the permission is currently granted. Understands "special" permissions: e.g. `Permissions.Has(Permissions.MANAGE_EXTERNAL_STORAGE)` returns the result of `Environment.isExternalStorageManager()` on Android 11+. Also handles `POST_NOTIFICATIONS` on pre-Android 13 by falling back to `NotificationManagerCompat.areNotificationsEnabled()`.

```lua
if Permissions.Has(Permissions.CAMERA) then
    StartCamera()
else
    Permissions.Request(Permissions.CAMERA)
end
```

---

### 53.5 Permissions.ShouldShowRationale

```lua
Permissions.ShouldShowRationale(permission) -> bool
```

Returns `true` if the system recommends showing an explanation before requesting the permission again
(e.g. the user previously denied it without "Don't ask again"). Always returns `false` for "special" permissions — they don't use the standard dialog.

```lua
if Permissions.ShouldShowRationale(Permissions.CAMERA) then
    ShowDialog("Camera is needed to scan QR codes")
end
```

---

### 53.6 Permissions.OpenAppSettings

```lua
Permissions.OpenAppSettings()
```

Opens the **App info** screen for the current application in the system Settings app (where the user can manage all permissions, storage, notifications, battery, etc.). Useful as a fallback when a "dangerous" permission was denied with "Don't ask again" — `Permissions.Request()` becomes a no-op and the only way to grant it is from the settings screen.

```lua
if not Permissions.ShouldShowRationale(Permissions.CAMERA)
   and not Permissions.Has(Permissions.CAMERA) then
    ShowDialog("Camera was permanently denied. Open settings?", function()
        Permissions.OpenAppSettings()
    end)
end
```

---

### 53.7 Permissions.OpenNotificationSettings

```lua
Permissions.OpenNotificationSettings()
```

Opens the system **App notifications** screen for the current app (channels, sounds, importance). Uses `Settings.ACTION_APP_NOTIFICATION_SETTINGS` on Android 8+ (API 26+) and falls back to the App info screen on older versions.

```lua
if not Permissions.AreNotificationsEnabled() then
    ShowDialog("Enable notifications to receive in-game alerts?", function()
        Permissions.OpenNotificationSettings()
    end)
end
```

---

### 53.8 Permissions.AreNotificationsEnabled

```lua
Permissions.AreNotificationsEnabled() -> bool
```

Returns `true` if the user has not disabled notifications for this app at the OS level (independent of `POST_NOTIFICATIONS` — works on every Android version). Use this to decide whether to schedule reminders or to prompt the user to re-enable notifications via `Permissions.OpenNotificationSettings()`.

```lua
if not Permissions.AreNotificationsEnabled() then
    HideNotificationBoundFeatures()
end
```

---

### 53.9 Special-access permissions

"Special" Android permissions don't use the runtime dialog — the user must toggle them on a dedicated system settings screen. Each one has a `Has*` / `Can*` / `Is*` accessor (current state) and a matching `Request*` method (opens the settings screen). They are also supported transparently by `Permissions.Has()` and `Permissions.Request()` using the corresponding manifest string constants.

#### `MANAGE_EXTERNAL_STORAGE` — All Files Access (Android 11+)

```lua
Permissions.HasAllFilesAccess() -> bool
Permissions.RequestAllFilesAccess()
```

Whether the app holds the **All files access** permission (broad read/write to shared storage). On Android 10 and below returns `true` automatically — scoped storage is not enforced. `RequestAllFilesAccess()` opens `ACTION_MANAGE_APP_ALL_FILES_ACCESS_PERMISSION`, falling back to the global list if the per-app intent is unavailable.

#### `SYSTEM_ALERT_WINDOW` — Draw over other apps

```lua
Permissions.CanDrawOverlays() -> bool
Permissions.RequestOverlayPermission()
```

Whether the app can show overlays on top of other apps. `RequestOverlayPermission()` opens `ACTION_MANAGE_OVERLAY_PERMISSION` for the current package. Always `true` on Android 5.x and below.

#### `WRITE_SETTINGS` — Modify system settings

```lua
Permissions.CanWriteSettings() -> bool
Permissions.RequestWriteSettings()
```

Whether the app can change system settings (brightness, ringtone, etc.). `RequestWriteSettings()` opens `ACTION_MANAGE_WRITE_SETTINGS`.

#### `REQUEST_INSTALL_PACKAGES` — Install unknown apps

```lua
Permissions.CanRequestInstallPackages() -> bool
Permissions.RequestInstallPackagesPermission()
```

Whether the app is allowed to trigger an APK installation (Android 8+). `RequestInstallPackagesPermission()` opens `ACTION_MANAGE_UNKNOWN_APP_SOURCES` for the current package.

#### `SCHEDULE_EXACT_ALARM` — Exact alarms (Android 12+)

```lua
Permissions.CanScheduleExactAlarms() -> bool
Permissions.RequestExactAlarmPermission()
```

Whether the app can call `AlarmManager.setExact*` / `setAlarmClock`. `RequestExactAlarmPermission()` opens `ACTION_REQUEST_SCHEDULE_EXACT_ALARM`. On Android 11 and below the check is implicit and returns `true`. The non-revocable `USE_EXACT_ALARM` permission (Android 13+) doesn't need this gate.

#### `REQUEST_IGNORE_BATTERY_OPTIMIZATIONS` — Battery optimization exemption

```lua
Permissions.IsIgnoringBatteryOptimizations() -> bool
Permissions.RequestIgnoreBatteryOptimizations()
```

Whether the app is excluded from Doze / App Standby for the current user. `RequestIgnoreBatteryOptimizations()` opens the per-app battery-optimization prompt, falling back to the global list if the targeted intent is rejected by the OEM.

#### `ACCESS_NOTIFICATION_POLICY` — Do Not Disturb access

```lua
Permissions.IsNotificationPolicyAccessGranted() -> bool
Permissions.RequestNotificationPolicyAccess()
```

Whether the app can read or modify the device-wide Do Not Disturb policy. `RequestNotificationPolicyAccess()` opens `ACTION_NOTIFICATION_POLICY_ACCESS_SETTINGS`.

> **Settings round-trip:** these `Request*` calls launch an external Activity — there is no `OnResult` callback. After the user returns to the game, re-check the corresponding `Has*` / `Can*` / `Is*` accessor (a good place is `OnResume` or the first frame back from background).

---

### 53.10 App-private storage paths

These paths are owned by the app, require no permissions, and are wiped when the app is uninstalled.

```lua
Permissions.GetInternalFilesPath() -> string  -- context.getFilesDir()
Permissions.GetInternalCachePath() -> string  -- context.getCacheDir()
Permissions.GetExternalFilesPath() -> string  -- context.getExternalFilesDir(null)
Permissions.GetExternalCachePath() -> string  -- context.getExternalCacheDir()
```

* **Internal** paths sit on the device's primary storage partition (always available, count toward the app's storage quota in Settings).
* **External** paths sit on shared storage and may live on an SD card — they can be temporarily unavailable; check `Permissions.IsExternalStorageAvailable()` before writing.
* **Cache** paths may be cleared by the OS under low-storage conditions — never store anything you cannot regenerate.

```lua
local saveDir = Permissions.GetInternalFilesPath() .. "/saves"
local tempDir = Permissions.GetExternalCachePath() .. "/temp_export"
```

---

### 53.11 Public storage paths

```lua
Permissions.GetPublicStoragePath(type) -> string
```

Returns the absolute path of a **public** shared-storage directory (`Environment.getExternalStoragePublicDirectory(type)`). With no argument (or `""`) returns the root of the user-visible external storage (`Environment.getExternalStorageDirectory()`).
Writing here requires `WRITE_EXTERNAL_STORAGE` (≤ Android 9) or `MANAGE_EXTERNAL_STORAGE` / Storage Access Framework on newer versions — reading via `MediaStore` typically only needs the granular `READ_MEDIA_*` permission.

Use the `Permissions.Dirs` subtable for the `type` argument:

| Constant | Value | Maps to |
|----------|-------|---------|
| `Permissions.Dirs.DOWNLOADS` | `"Download"` | `Environment.DIRECTORY_DOWNLOADS` |
| `Permissions.Dirs.DOCUMENTS` | `"Documents"` | `Environment.DIRECTORY_DOCUMENTS` |
| `Permissions.Dirs.PICTURES` | `"Pictures"` | `Environment.DIRECTORY_PICTURES` |
| `Permissions.Dirs.MOVIES` | `"Movies"` | `Environment.DIRECTORY_MOVIES` |
| `Permissions.Dirs.MUSIC` | `"Music"` | `Environment.DIRECTORY_MUSIC` |
| `Permissions.Dirs.DCIM` | `"DCIM"` | `Environment.DIRECTORY_DCIM` |
| `Permissions.Dirs.RINGTONES` | `"Ringtones"` | `Environment.DIRECTORY_RINGTONES` |
| `Permissions.Dirs.ALARMS` | `"Alarms"` | `Environment.DIRECTORY_ALARMS` |
| `Permissions.Dirs.NOTIFICATIONS` | `"Notifications"` | `Environment.DIRECTORY_NOTIFICATIONS` |
| `Permissions.Dirs.PODCASTS` | `"Podcasts"` | `Environment.DIRECTORY_PODCASTS` |
| `Permissions.Dirs.AUDIOBOOKS` | `"Audiobooks"` | `Environment.DIRECTORY_AUDIOBOOKS` |
| `Permissions.Dirs.RECORDINGS` | `"Recordings"` | `Environment.DIRECTORY_RECORDINGS` |
| `Permissions.Dirs.SCREENSHOTS` | `"Pictures/Screenshots"` | `Environment.DIRECTORY_SCREENSHOTS` |

```lua
local downloads = Permissions.GetPublicStoragePath(Permissions.Dirs.DOWNLOADS)
Print("Downloads dir: " .. downloads)

local sharedRoot = Permissions.GetPublicStoragePath()  -- /storage/emulated/0
```

---

### 53.12 Storage availability & free space

```lua
Permissions.GetInternalStorageFreeBytes() -> integer   -- bytes usable in filesDir
Permissions.GetExternalStorageFreeBytes() -> integer   -- bytes usable in externalFilesDir
Permissions.IsExternalStorageAvailable() -> bool       -- true if MEDIA_MOUNTED
Permissions.IsExternalStorageReadOnly() -> bool        -- true if MEDIA_MOUNTED_READ_ONLY
```

Free space is reported via `File.getUsableSpace()` — the amount the app can actually write right now (already accounts for system reserves). On error or unsupported platforms the integers are `0` and the booleans are `false`.

```lua
local needed = 200 * 1024 * 1024   -- 200 MB
if Permissions.GetInternalStorageFreeBytes() < needed then
    ShowDialog("Not enough free space to download the update.")
end

if not Permissions.IsExternalStorageAvailable() or Permissions.IsExternalStorageReadOnly() then
    SwitchToInternalStorageMode()
end
```

---

### 53.13 Permission constants

All permissions declared by the engine in `AndroidManifest.xml` are exposed as Lua constants and can be requested at runtime. Grouped by category for readability.

#### Camera & Microphone

| Constant | Value |
|----------|-------|
| `Permissions.CAMERA` | `"android.permission.CAMERA"` |
| `Permissions.RECORD_AUDIO` | `"android.permission.RECORD_AUDIO"` |

#### Location

| Constant | Value |
|----------|-------|
| `Permissions.ACCESS_FINE_LOCATION` | `"android.permission.ACCESS_FINE_LOCATION"` |
| `Permissions.ACCESS_COARSE_LOCATION` | `"android.permission.ACCESS_COARSE_LOCATION"` |
| `Permissions.ACCESS_BACKGROUND_LOCATION` | `"android.permission.ACCESS_BACKGROUND_LOCATION"` |
| `Permissions.ACCESS_LOCATION_EXTRA_COMMANDS` | `"android.permission.ACCESS_LOCATION_EXTRA_COMMANDS"` |
| `Permissions.ACCESS_MEDIA_LOCATION` | `"android.permission.ACCESS_MEDIA_LOCATION"` |

#### Storage & Media

| Constant | Value |
|----------|-------|
| `Permissions.READ_EXTERNAL_STORAGE` | `"android.permission.READ_EXTERNAL_STORAGE"` |
| `Permissions.WRITE_EXTERNAL_STORAGE` | `"android.permission.WRITE_EXTERNAL_STORAGE"` |
| `Permissions.MANAGE_EXTERNAL_STORAGE` | `"android.permission.MANAGE_EXTERNAL_STORAGE"` |
| `Permissions.READ_MEDIA_IMAGES` | `"android.permission.READ_MEDIA_IMAGES"` |
| `Permissions.READ_MEDIA_VIDEO` | `"android.permission.READ_MEDIA_VIDEO"` |
| `Permissions.READ_MEDIA_AUDIO` | `"android.permission.READ_MEDIA_AUDIO"` |
| `Permissions.READ_MEDIA_VISUAL_USER_SELECTED` | `"android.permission.READ_MEDIA_VISUAL_USER_SELECTED"` |

> **Storage on modern Android:** since Android 13 (API 33) `READ_EXTERNAL_STORAGE` is no longer granted — use the granular `READ_MEDIA_IMAGES` / `READ_MEDIA_VIDEO` / `READ_MEDIA_AUDIO`. `READ_MEDIA_VISUAL_USER_SELECTED` (Android 14+) lets the user pick specific photos instead of granting access to all images. `ACCESS_MEDIA_LOCATION` is needed to read EXIF GPS data from photos returned by `MediaStore`.

#### Notifications

| Constant | Value |
|----------|-------|
| `Permissions.POST_NOTIFICATIONS` | `"android.permission.POST_NOTIFICATIONS"` |
| `Permissions.USE_FULL_SCREEN_INTENT` | `"android.permission.USE_FULL_SCREEN_INTENT"` |
| `Permissions.ACCESS_NOTIFICATION_POLICY` | `"android.permission.ACCESS_NOTIFICATION_POLICY"` |

#### Bluetooth & Nearby

| Constant | Value |
|----------|-------|
| `Permissions.BLUETOOTH` | `"android.permission.BLUETOOTH"` |
| `Permissions.BLUETOOTH_ADMIN` | `"android.permission.BLUETOOTH_ADMIN"` |
| `Permissions.BLUETOOTH_CONNECT` | `"android.permission.BLUETOOTH_CONNECT"` |
| `Permissions.BLUETOOTH_SCAN` | `"android.permission.BLUETOOTH_SCAN"` |
| `Permissions.BLUETOOTH_ADVERTISE` | `"android.permission.BLUETOOTH_ADVERTISE"` |
| `Permissions.NEARBY_WIFI_DEVICES` | `"android.permission.NEARBY_WIFI_DEVICES"` |
| `Permissions.ACCESS_LOCAL_NETWORK` | `"android.permission.ACCESS_LOCAL_NETWORK"` |

#### Contacts & Accounts

| Constant | Value |
|----------|-------|
| `Permissions.READ_CONTACTS` | `"android.permission.READ_CONTACTS"` |
| `Permissions.WRITE_CONTACTS` | `"android.permission.WRITE_CONTACTS"` |
| `Permissions.GET_ACCOUNTS` | `"android.permission.GET_ACCOUNTS"` |

#### Calendar

| Constant | Value |
|----------|-------|
| `Permissions.READ_CALENDAR` | `"android.permission.READ_CALENDAR"` |
| `Permissions.WRITE_CALENDAR` | `"android.permission.WRITE_CALENDAR"` |

#### Sensors & Activity

| Constant | Value |
|----------|-------|
| `Permissions.BODY_SENSORS` | `"android.permission.BODY_SENSORS"` |
| `Permissions.BODY_SENSORS_BACKGROUND` | `"android.permission.BODY_SENSORS_BACKGROUND"` |
| `Permissions.ACTIVITY_RECOGNITION` | `"android.permission.ACTIVITY_RECOGNITION"` |
| `Permissions.HIGH_SAMPLING_RATE_SENSORS` | `"android.permission.HIGH_SAMPLING_RATE_SENSORS"` |

#### Phone & Calls

| Constant | Value |
|----------|-------|
| `Permissions.READ_PHONE_STATE` | `"android.permission.READ_PHONE_STATE"` |
| `Permissions.READ_PHONE_NUMBERS` | `"android.permission.READ_PHONE_NUMBERS"` |
| `Permissions.CALL_PHONE` | `"android.permission.CALL_PHONE"` |
| `Permissions.ANSWER_PHONE_CALLS` | `"android.permission.ANSWER_PHONE_CALLS"` |
| `Permissions.READ_CALL_LOG` | `"android.permission.READ_CALL_LOG"` |
| `Permissions.WRITE_CALL_LOG` | `"android.permission.WRITE_CALL_LOG"` |
| `Permissions.ADD_VOICEMAIL` | `"android.permission.ADD_VOICEMAIL"` |

#### SMS & MMS

| Constant | Value |
|----------|-------|
| `Permissions.SEND_SMS` | `"android.permission.SEND_SMS"` |
| `Permissions.RECEIVE_SMS` | `"android.permission.RECEIVE_SMS"` |
| `Permissions.READ_SMS` | `"android.permission.READ_SMS"` |
| `Permissions.RECEIVE_MMS` | `"android.permission.RECEIVE_MMS"` |
| `Permissions.RECEIVE_WAP_PUSH` | `"android.permission.RECEIVE_WAP_PUSH"` |

#### Biometrics

| Constant | Value |
|----------|-------|
| `Permissions.USE_BIOMETRIC` | `"android.permission.USE_BIOMETRIC"` |
| `Permissions.USE_FINGERPRINT` | `"android.permission.USE_FINGERPRINT"` |

#### Foreground Services

| Constant | Value |
|----------|-------|
| `Permissions.FOREGROUND_SERVICE` | `"android.permission.FOREGROUND_SERVICE"` |
| `Permissions.FOREGROUND_SERVICE_DATA_SYNC` | `"android.permission.FOREGROUND_SERVICE_DATA_SYNC"` |
| `Permissions.FOREGROUND_SERVICE_MEDIA_PLAYBACK` | `"android.permission.FOREGROUND_SERVICE_MEDIA_PLAYBACK"` |
| `Permissions.FOREGROUND_SERVICE_LOCATION` | `"android.permission.FOREGROUND_SERVICE_LOCATION"` |
| `Permissions.FOREGROUND_SERVICE_CONNECTED_DEVICE` | `"android.permission.FOREGROUND_SERVICE_CONNECTED_DEVICE"` |
| `Permissions.FOREGROUND_SERVICE_MICROPHONE` | `"android.permission.FOREGROUND_SERVICE_MICROPHONE"` |
| `Permissions.FOREGROUND_SERVICE_CAMERA` | `"android.permission.FOREGROUND_SERVICE_CAMERA"` |
| `Permissions.FOREGROUND_SERVICE_HEALTH` | `"android.permission.FOREGROUND_SERVICE_HEALTH"` |
| `Permissions.FOREGROUND_SERVICE_PHONE_CALL` | `"android.permission.FOREGROUND_SERVICE_PHONE_CALL"` |
| `Permissions.FOREGROUND_SERVICE_REMOTE_MESSAGING` | `"android.permission.FOREGROUND_SERVICE_REMOTE_MESSAGING"` |
| `Permissions.FOREGROUND_SERVICE_SPECIAL_USE` | `"android.permission.FOREGROUND_SERVICE_SPECIAL_USE"` |
| `Permissions.FOREGROUND_SERVICE_SYSTEM_EXEMPTED` | `"android.permission.FOREGROUND_SERVICE_SYSTEM_EXEMPTED"` |

> **Granular foreground-service types (Android 14+):** declaring `FOREGROUND_SERVICE` alone is no longer enough — also declare the matching type (e.g. `FOREGROUND_SERVICE_MEDIA_PLAYBACK` for a music player) and set `android:foregroundServiceType` on the service. All of these are normal permissions, granted at install.

#### Alarms & Background

| Constant | Value |
|----------|-------|
| `Permissions.REQUEST_IGNORE_BATTERY_OPTIMIZATIONS` | `"android.permission.REQUEST_IGNORE_BATTERY_OPTIMIZATIONS"` |
| `Permissions.SCHEDULE_EXACT_ALARM` | `"android.permission.SCHEDULE_EXACT_ALARM"` |
| `Permissions.USE_EXACT_ALARM` | `"android.permission.USE_EXACT_ALARM"` |
| `Permissions.RECEIVE_BOOT_COMPLETED` | `"android.permission.RECEIVE_BOOT_COMPLETED"` |
| `Permissions.RUN_USER_INITIATED_JOBS` | `"android.permission.RUN_USER_INITIATED_JOBS"` |

#### Special-access permissions

| Constant | Value |
|----------|-------|
| `Permissions.SYSTEM_ALERT_WINDOW` | `"android.permission.SYSTEM_ALERT_WINDOW"` |
| `Permissions.WRITE_SETTINGS` | `"android.permission.WRITE_SETTINGS"` |
| `Permissions.REQUEST_INSTALL_PACKAGES` | `"android.permission.REQUEST_INSTALL_PACKAGES"` |
| `Permissions.REQUEST_DELETE_PACKAGES` | `"android.permission.REQUEST_DELETE_PACKAGES"` |
| `Permissions.QUERY_ALL_PACKAGES` | `"android.permission.QUERY_ALL_PACKAGES"` |

> `Permissions.Request(Permissions.SYSTEM_ALERT_WINDOW)` (and the other special-access constants in this group) opens the corresponding system settings screen — there is no runtime dialog. See section **53.9** for the dedicated accessor pairs.

#### Network

| Constant | Value |
|----------|-------|
| `Permissions.INTERNET` | `"android.permission.INTERNET"` |
| `Permissions.ACCESS_NETWORK_STATE` | `"android.permission.ACCESS_NETWORK_STATE"` |
| `Permissions.ACCESS_WIFI_STATE` | `"android.permission.ACCESS_WIFI_STATE"` |
| `Permissions.CHANGE_WIFI_STATE` | `"android.permission.CHANGE_WIFI_STATE"` |
| `Permissions.CHANGE_NETWORK_STATE` | `"android.permission.CHANGE_NETWORK_STATE"` |

#### Audio & Hardware

| Constant | Value |
|----------|-------|
| `Permissions.VIBRATE` | `"android.permission.VIBRATE"` |
| `Permissions.WAKE_LOCK` | `"android.permission.WAKE_LOCK"` |
| `Permissions.MODIFY_AUDIO_SETTINGS` | `"android.permission.MODIFY_AUDIO_SETTINGS"` |

#### System UI, Wallpaper & Tasks

| Constant | Value |
|----------|-------|
| `Permissions.EXPAND_STATUS_BAR` | `"android.permission.EXPAND_STATUS_BAR"` |
| `Permissions.SET_WALLPAPER` | `"android.permission.SET_WALLPAPER"` |
| `Permissions.SET_WALLPAPER_HINTS` | `"android.permission.SET_WALLPAPER_HINTS"` |
| `Permissions.KILL_BACKGROUND_PROCESSES` | `"android.permission.KILL_BACKGROUND_PROCESSES"` |
| `Permissions.REORDER_TASKS` | `"android.permission.REORDER_TASKS"` |

#### Screen Capture Detection

| Constant | Value |
|----------|-------|
| `Permissions.DETECT_SCREEN_CAPTURE` | `"android.permission.DETECT_SCREEN_CAPTURE"` |
| `Permissions.DETECT_SCREEN_RECORDING` | `"android.permission.DETECT_SCREEN_RECORDING"` |

#### Advertising ID

| Constant | Value |
|----------|-------|
| `Permissions.AD_ID` | `"com.google.android.gms.permission.AD_ID"` |

> Note the non-`android.permission.*` prefix — this one is declared by Google Play Services and is required on Android 13+ for apps that read the Advertising ID.

> You can also pass any Android permission string directly: `Permissions.Request("android.permission.VIBRATE")`

> **Normal vs Dangerous vs Special permissions:** "normal" permissions (`INTERNET`, `ACCESS_NETWORK_STATE`, `ACCESS_WIFI_STATE`, `VIBRATE`, `WAKE_LOCK`, `MODIFY_AUDIO_SETTINGS`, `FOREGROUND_SERVICE*`, `USE_EXACT_ALARM`, `RUN_USER_INITIATED_JOBS`, `RECEIVE_BOOT_COMPLETED`, `USE_BIOMETRIC`, `USE_FINGERPRINT`, `EXPAND_STATUS_BAR`, `SET_WALLPAPER`, `SET_WALLPAPER_HINTS`, `KILL_BACKGROUND_PROCESSES`, `REORDER_TASKS`, `USE_FULL_SCREEN_INTENT`, `ACCESS_LOCATION_EXTRA_COMMANDS`, `RECEIVE_WAP_PUSH`, `AD_ID`, `QUERY_ALL_PACKAGES`, `DETECT_SCREEN_CAPTURE`, `DETECT_SCREEN_RECORDING`) are granted automatically at install — `Permissions.Has()` always returns `true` and `Permissions.Request()` is a no-op. "Dangerous" permissions (camera, mic, location, contacts, calendar, body sensors, phone, SMS, modern media storage, `BLUETOOTH_CONNECT/SCAN/ADVERTISE`, `NEARBY_WIFI_DEVICES`, `ACCESS_LOCAL_NETWORK`, `POST_NOTIFICATIONS`, `ACTIVITY_RECOGNITION`, `ACCESS_MEDIA_LOCATION`) require runtime grant from the user via `Permissions.Request()`. "Special" permissions (`MANAGE_EXTERNAL_STORAGE`, `SYSTEM_ALERT_WINDOW`, `WRITE_SETTINGS`, `REQUEST_INSTALL_PACKAGES`, `SCHEDULE_EXACT_ALARM`, `REQUEST_IGNORE_BATTERY_OPTIMIZATIONS`, `ACCESS_NOTIFICATION_POLICY`, `ACCESS_BACKGROUND_LOCATION`) require navigating the user to a system settings screen — they cannot be granted via the standard permission dialog; use the matching accessors from section **53.9** (or just call `Permissions.Has()` / `Permissions.Request()` with the constant — they understand special permissions transparently).

> **API level notes:** `BLUETOOTH_CONNECT/SCAN/ADVERTISE` apply on Android 12+ (API 31+); on older versions, `BLUETOOTH` and `BLUETOOTH_ADMIN` are used automatically (declared in the manifest with `maxSdkVersion="30"`). `POST_NOTIFICATIONS` and `READ_MEDIA_*` apply on Android 13+ (API 33+). `NEARBY_WIFI_DEVICES` applies on Android 13+. `ACCESS_LOCAL_NETWORK` applies on Android 17+ (API 37+) and only to builds whose **Target SDK is 37 or higher** — those are the builds Android stops from reaching LAN addresses until the permission is granted, so tick **Local Network (LAN)** in Build Game → Android (or pass `--enable-local-network`) to have it declared. On anything older, or in a build that targets 36 or lower, `Permissions.Has()` reports `true` because local network access is implicit there. `READ_MEDIA_VISUAL_USER_SELECTED` applies on Android 14+ (API 34+). `BODY_SENSORS_BACKGROUND` applies on Android 13+. `USE_EXACT_ALARM` applies on Android 13+ (API 33+) — for calendar/alarm-clock category apps; otherwise use `SCHEDULE_EXACT_ALARM`. `FOREGROUND_SERVICE_*` type-specific permissions are required on Android 14+ (API 34+). `RUN_USER_INITIATED_JOBS`, `DETECT_SCREEN_CAPTURE` and `DETECT_SCREEN_RECORDING` apply on Android 14+ / 15+. `USE_FULL_SCREEN_INTENT` is automatically granted only for call / alarm category apps on Android 14+; for other apps the user must enable it manually under the per-app permission screen. On lower SDK levels the constants exist but `Permissions.Has()` will report `true` automatically (the permission is implicit) or `false` (unavailable platform feature).

---

### 53.14 Callbacks summary

| Callback | Parameters | Description |
|----------|------------|-------------|
| `Permissions.OnResult(fn)` | `(permission: string, granted: bool)` | Permission request result (runtime permissions only — special-access permissions don't fire it) |
| `Permissions.ClearCallbacks()` | — | Remove all callbacks |

---

### 53.15 Practical example — permission-aware feature gates

```lua
function OnCreate()
    if not Permissions.IsSupported() then return end

    Permissions.OnResult(function(permission, granted)
        if permission == Permissions.CAMERA and granted then
            StartARMode()
        elseif permission == Permissions.RECORD_AUDIO and granted then
            StartVoiceChat()
        elseif permission == Permissions.POST_NOTIFICATIONS and granted then
            ScheduleReminders()
        elseif not granted then
            Print("Permission denied: " .. permission)
        end
    end)
end

function OnARButtonPressed()
    if Permissions.Has(Permissions.CAMERA) then
        StartARMode()
    elseif Permissions.ShouldShowRationale(Permissions.CAMERA) then
        ShowDialog("Camera access is required for AR mode", function()
            Permissions.Request(Permissions.CAMERA)
        end)
    else
        Permissions.Request(Permissions.CAMERA)
    end
end

function OnVoiceChatButton()
    if Permissions.Has(Permissions.RECORD_AUDIO) then
        StartVoiceChat()
    else
        Permissions.Request(Permissions.RECORD_AUDIO)
    end
end

function OnEnableModInstalls()
    if Permissions.CanRequestInstallPackages() then
        StartInstallFlow()
    else
        ShowDialog("Allow installing mods from this app?", function()
            Permissions.RequestInstallPackagesPermission()
        end)
    end
end

function OnReturnFromBackground()
    -- re-check special-access permissions after the user might
    -- have toggled them on the system settings screen
    if Permissions.CanDrawOverlays() then EnableMiniPlayerOverlay() end
    if Permissions.IsIgnoringBatteryOptimizations() then EnableBackgroundSync() end
end

function OnExportSaveToDownloads()
    if Permissions.IsExternalStorageReadOnly() then
        ShowDialog("Shared storage is read-only on this device.")
        return
    end
    local dir = Permissions.GetPublicStoragePath(Permissions.Dirs.DOWNLOADS)
    WriteSaveTo(dir .. "/MyGameSave.json")
end
```

---

## 54. Web3 — Blockchain Integration (Ethereum / BNB Smart Chain)

The `Web3` module provides MetaMask wallet connection, native coin and ERC-20 token balance queries, transaction signing, and smart contract interaction — all directly from Lua scripts. Available only on the **Web (WASM)** platform with the **Web3** build option enabled.

The Web3 library (`ethers.js`) is embedded into the game page at build time, so a Web3 build **loads no script from any third party** — it works offline, from `file://`, behind a strict CSP, and on game portals that forbid external requests, and no player IP is handed to a CDN. The only hosts the game contacts are the blockchain RPC endpoints and the IPFS/Arweave gateway you actually use.

Every asynchronous call returns a `requestId` and always finishes in exactly one callback. If the Web3 bridge is missing — the build has Web3 disabled, the page does not ship the bridge, or you are running on Windows/Linux/macOS/Android/iOS — the call still completes through `onError` (and through `Web3.OnError`) instead of silently hanging, so the same script runs unchanged on every platform.

Table parameters accept Lua **numbers** as well as strings (`tokenId = 42` and `tokenId = "42"` behave identically), and `abi` / `args` / `owners` / `tokenIds` accept a Lua **table** in place of a JSON string.

### Platform Check

| Function | Returns | Description |
|----------|---------|-------------|
| `Web3.IsSupported()` | `bool` | `true` only when the page really can do Web3: a Web build with the bridge present and the `ethers` library loaded. Checked at runtime, so it also returns `false` if the library failed to download |
| `Web3.IsConnected()` | `bool` | `true` if a wallet is currently connected |

### Wallet

| Function | Returns | Description |
|----------|---------|-------------|
| `Web3.ConnectWallet(callback?)` | — | Opens MetaMask connection prompt. Optional `callback(address)` fires on success |
| `Web3.DisconnectWallet()` | — | Disconnects current wallet session |
| `Web3.GetAddress()` | `string` | Current connected wallet address (`0x...`) |
| `Web3.GetChainId()` | `string` | Current chain ID in hex (`"0x1"`, `"0x38"`, …) |

### Chain Management

| Function | Returns | Description |
|----------|---------|-------------|
| `Web3.SwitchChain(chain)` | — | Switch to a chain by hex ID or name (`"ethereum"`, `"bsc"`, `"sepolia"`, …). Auto-adds unknown chains via MetaMask |
| `Web3.AddChain(config)` | — | Register a custom chain. `config` is a table: `{ chainId, name, rpcUrl, symbol, explorerUrl }`. `chainId` also accepts a chain name shortcut |
| `Web3.SetReadOnlyChain(chain, rpcUrl?)` | — | Enable **read-only mode**: every read function works before (or without) a wallet connection. `chain` is a hex ID or name; `rpcUrl` is optional and defaults to the built-in endpoints for known chains. Call `Web3.SetReadOnlyChain("", "")` to turn it off |

**Supported chain name shortcuts:** `ethereum` / `eth` / `mainnet`, `holesky`, `sepolia`, `bsc` / `bnb`, `bsc_testnet` / `bnb_testnet`, `polygon`, `polygon_amoy` / `amoy`, `arbitrum`, `arbitrum_sepolia`, `optimism`, `optimism_sepolia`, `avalanche`, `avalanche_fuji` / `fuji`, `base`, `base_sepolia`, `linea`, `scroll`, `zksync`, `blast`, `gnosis`, `celo`, `mantle`.

**Read-only mode.** A connected wallet is not required to read the chain. After `Web3.SetReadOnlyChain("base")`, calls such as `GetBalance`, `GetTokenBalance`, `CallContract`, `GetNFTOwner`, `GetTokenURI`, `GetOwnedTokens`, `GetAllowance`, `EstimateGas`, `GetTransactionReceipt`, `SubscribeEvent` and `Multicall` run against a public RPC — useful for leaderboards, NFT galleries and shop previews shown before the player connects. Every built-in chain ships two independent RPC endpoints and automatically falls over to the second one if the first is unreachable. Once a wallet is connected, reads go through the wallet and its current chain — the read-only RPC is used only while no wallet is connected, and it survives `Web3.DisconnectWallet()`. Signing (`SendTransaction`, `WriteContract`, `TransferToken`, `TransferNFT`, `ApproveToken`, `SetApprovalForAll`, `SignMessage`, `SignTypedData`) always requires a connected wallet.

### Chain ID Constants

| Constant | Value | Chain |
|----------|-------|-------|
| `Web3.CHAIN_ETHEREUM` | `"0x1"` | Ethereum Mainnet |
| `Web3.CHAIN_HOLESKY` | `"0x4268"` | Holesky Testnet |
| `Web3.CHAIN_SEPOLIA` | `"0xaa36a7"` | Sepolia Testnet |
| `Web3.CHAIN_BSC` | `"0x38"` | BNB Smart Chain |
| `Web3.CHAIN_BSC_TESTNET` | `"0x61"` | BSC Testnet |
| `Web3.CHAIN_POLYGON` | `"0x89"` | Polygon |
| `Web3.CHAIN_POLYGON_AMOY` | `"0x13882"` | Polygon Amoy Testnet |
| `Web3.CHAIN_ARBITRUM` | `"0xa4b1"` | Arbitrum One |
| `Web3.CHAIN_ARBITRUM_SEPOLIA` | `"0x66eee"` | Arbitrum Sepolia Testnet |
| `Web3.CHAIN_OPTIMISM` | `"0xa"` | Optimism |
| `Web3.CHAIN_OPTIMISM_SEPOLIA` | `"0xaa37dc"` | Optimism Sepolia Testnet |
| `Web3.CHAIN_AVALANCHE` | `"0xa86a"` | Avalanche C-Chain |
| `Web3.CHAIN_AVALANCHE_FUJI` | `"0xa869"` | Avalanche Fuji Testnet |
| `Web3.CHAIN_BASE` | `"0x2105"` | Base |
| `Web3.CHAIN_BASE_SEPOLIA` | `"0x14a34"` | Base Sepolia Testnet |
| `Web3.CHAIN_LINEA` | `"0xe708"` | Linea |
| `Web3.CHAIN_SCROLL` | `"0x82750"` | Scroll |
| `Web3.CHAIN_ZKSYNC` | `"0x144"` | zkSync Era |
| `Web3.CHAIN_BLAST` | `"0x13e31"` | Blast |
| `Web3.CHAIN_GNOSIS` | `"0x64"` | Gnosis |
| `Web3.CHAIN_CELO` | `"0xa4ec"` | Celo |
| `Web3.CHAIN_MANTLE` | `"0x1388"` | Mantle |

### Transactions

| Function | Returns | Description |
|----------|---------|-------------|
| `Web3.SendTransaction(params, onSent?, onConfirmed?, onFailed?)` | `requestId` | Send native coin. `params = { to, value, data? }`. `value` is in ETH/BNB (e.g. `"0.1"`) |
| `Web3.SignMessage(message, callback?, onError?)` | `requestId` | Personal sign. `callback(signature)`. `onError(errorMessage)` on failure |

### Balance

| Function | Returns | Description |
|----------|---------|-------------|
| `Web3.GetBalance(address?, callback?, onError?)` | `requestId` | Native coin balance in ETH/BNB. Defaults to connected wallet |
| `Web3.GetTokenBalance(tokenAddress, ownerAddress?, callback?, onError?)` | `requestId` | ERC-20 token balance, already scaled by the token's `decimals()`. Defaults to the connected wallet. Tokens that do not expose `decimals()` are treated as 18-decimal, the same assumption wallets make |

### Smart Contracts

| Function | Returns | Description |
|----------|---------|-------------|
| `Web3.CallContract(params, callback?, onError?)` | `requestId` | Read-only call. `params = { address, abi, method, args? }`. `abi` and `args` are JSON strings |
| `Web3.WriteContract(params, onSent?, onConfirmed?, onFailed?)` | `requestId` | State-changing call. `params = { address, abi, method, args?, value? }` |

### NFT (ERC-721 / ERC-1155)

| Function | Returns | Description |
|----------|---------|-------------|
| `Web3.GetNFTBalance(params, callback?, onError?)` | `requestId` | Token count. `params = { address, owner?, tokenId? }`. Without `tokenId` → ERC-721 `balanceOf(address)`. With `tokenId` → ERC-1155 `balanceOf(address, tokenId)`. Defaults owner to connected wallet. `callback(count)` |
| `Web3.GetNFTOwner(params, callback?, onError?)` | `requestId` | Owner of a specific ERC-721 token. `params = { address, tokenId }`. `callback(ownerAddress)` |
| `Web3.GetTokenURI(params, callback?, onError?)` | `requestId` | Token metadata URI. `params = { address, tokenId }`. Tries ERC-721 `tokenURI()` first, falls back to ERC-1155 `uri()`. An ERC-1155 `{id}` placeholder is replaced with the 64-character zero-padded lowercase hex token ID, as the standard requires. `callback(uri)` |
| `Web3.GetNFTBalanceBatch(params, callback?, onError?)` | `requestId` | ERC-1155 batch balance. `params = { address, owners, tokenIds }`. `owners` and `tokenIds` are JSON arrays. `callback(balancesJson)` |
| `Web3.GetOwnedTokens(params, callback?, onError?)` | `requestId` | Enumerate all NFTs owned (requires ERC721Enumerable). `params = { address, owner? }`. `callback(tokenIdsJson)` |

### NFT Transfers

| Function | Returns | Description |
|----------|---------|-------------|
| `Web3.TransferNFT(params, onSent?, onConfirmed?, onFailed?)` | `requestId` | Transfer an NFT via `safeTransferFrom`. `params = { address, to, tokenId, from?, standard?, amount? }`. `standard` is `"ERC721"` (default) or `"ERC1155"`. `amount` defaults to `"1"` (ERC-1155 only). `from` defaults to connected wallet |

### ERC-20 Approve & Allowance

| Function | Returns | Description |
|----------|---------|-------------|
| `Web3.ApproveToken(params, onSent?, onConfirmed?, onFailed?)` | `requestId` | Approve a spender to use your tokens. `params = { token, spender, amount }`. `amount` is in human-readable units (e.g. `"100"` for 100 tokens) — decimals are queried automatically. Use `"max"` or `"unlimited"` for unlimited approval (`MaxUint256`). If the contract does not expose `decimals()`, `amount` is treated as raw wei |
| `Web3.GetAllowance(params, callback?, onError?)` | `requestId` | Check how many tokens a spender can use. `params = { token, spender, owner? }`. `owner` defaults to connected wallet. `callback(allowance)` |

### ERC-20 Transfer

| Function | Returns | Description |
|----------|---------|-------------|
| `Web3.TransferToken(params, onSent?, onConfirmed?, onFailed?)` | `requestId` | Transfer ERC-20 tokens. `params = { token, to, amount }`. `amount` is in human-readable units (e.g. `"50"` for 50 tokens) — decimals are queried automatically. If the contract does not expose `decimals()`, `amount` is treated as raw wei |

### NFT Approval (ERC-721 / ERC-1155)

| Function | Returns | Description |
|----------|---------|-------------|
| `Web3.SetApprovalForAll(params, onSent?, onConfirmed?, onFailed?)` | `requestId` | Set or revoke operator approval for all tokens in a contract. Works with ERC-721 and ERC-1155. `params = { address, operator, approved? }`. `approved` defaults to `true`. Calls `setApprovalForAll(operator, approved)` on the contract |

### Transaction Watching

| Function | Returns | Description |
|----------|---------|-------------|
| `Web3.WatchTransaction(txHash, onConfirmed?, onFailed?, timeoutMs?)` | `requestId` | Watch any on-chain transaction (including ones not sent by you). `onConfirmed(txHash)` on success, `onFailed(error)` on revert/failure/timeout. `timeoutMs` defaults to `120000` (2 minutes) |

### EIP-712 Typed Data Signing

| Function | Returns | Description |
|----------|---------|-------------|
| `Web3.SignTypedData(typedDataJson, callback?, onError?)` | `requestId` | Sign structured data per EIP-712 (`eth_signTypedData_v4`). `typedDataJson` is the full EIP-712 JSON string with `types`, `primaryType`, `domain`, and `message`. `callback(signature)` |

### Contract Event Listening

| Function | Returns | Description |
|----------|---------|-------------|
| `Web3.SubscribeEvent(params, callback?, onError?)` | `subscriptionId` | Subscribe to contract events in real-time. `params = { address, abi, event }`. `abi` is JSON ABI containing the event definition. `callback(eventDataJson)` fires for each event. Returns `subscriptionId` for unsubscribing |
| `Web3.UnsubscribeEvent(subscriptionId)` | — | Stop listening to a previously subscribed contract event |

### Gas Estimation

| Function | Returns | Description |
|----------|---------|-------------|
| `Web3.EstimateGas(params, callback?, onError?)` | `requestId` | Estimate gas for a transaction. `params = { to, value?, data? }`. `callback(gasJson)` where `gasJson` contains `gasLimit`, `gasPrice`, `maxFeePerGas`, `maxPriorityFeePerGas` |

### Transaction Receipt

| Function | Returns | Description |
|----------|---------|-------------|
| `Web3.GetTransactionReceipt(txHash, callback?, onError?)` | `requestId` | Get full receipt details. `callback(receiptJson)` with `status`, `gasUsed`, `effectiveGasPrice`, `blockNumber`, `blockHash`, `from`, `to`, `contractAddress`, `logs` |

### Multi-Provider (EIP-6963)

| Function | Returns | Description |
|----------|---------|-------------|
| `Web3.GetProviders()` | `string` | Returns JSON array of detected browser wallet providers. Each entry: `{ rdns, name, icon, uuid }`. E.g. MetaMask = `"io.metamask"`, Coinbase = `"com.coinbase.wallet"` |
| `Web3.ConnectWithProvider(rdns, callback?)` | — | Connect using a specific wallet provider by its RDNS identifier. `callback(address)` on success |

### Metadata Resolution

| Function | Returns | Description |
|----------|---------|-------------|
| `Web3.ResolveMetadata(uri, callback?, onError?)` | `requestId` | Fetch and parse NFT metadata from a URI. Automatically resolves the `ipfs://`, `ipfs://ipfs/` and `ar://` schemes, and resolves them again inside the `image`, `image_url`, `animation_url` and `external_url` fields. The request is aborted after 30 seconds with an error rather than hanging. `callback(metadataJson)` |
| `Web3.SetIPFSGateway(url)` | — | Set a custom IPFS gateway URL for `ResolveMetadata`. Default is `"https://ipfs.io/ipfs/"`. Example: `Web3.SetIPFSGateway("https://gateway.pinata.cloud/ipfs/")` |

### Multicall (Batch Reads)

| Function | Returns | Description |
|----------|---------|-------------|
| `Web3.Multicall(calls, callback?, onError?)` | `requestId` | Execute multiple read-only contract calls in a single RPC request using Multicall3. `calls` is a Lua array of `{ address, abi, method, args? }` and call order is preserved in the result. `callback(resultsJson)` returns an array of `{ success, decoded, data }`: `success` is the on-chain call result, `decoded` says whether the return value could be decoded with the supplied ABI (`false` leaves the raw hex in `data`). Available on all major EVM chains; zkSync Era uses its own Multicall3 deployment automatically |

### Event Callbacks

| Function | Description |
|----------|-------------|
| `Web3.OnWalletConnected(callback)` | `callback(address)` — wallet connected |
| `Web3.OnWalletDisconnected(callback)` | `callback()` — wallet disconnected |
| `Web3.OnChainChanged(callback)` | `callback(chainId)` — user switched chain in MetaMask |
| `Web3.OnAccountChanged(callback)` | `callback(address)` — user switched account in MetaMask |
| `Web3.OnError(callback)` | `callback(errorMessage, requestId)` — any error |
| `Web3.ClearCallbacks()` | Remove all registered callbacks and cancel every live event subscription |

Callbacks and subscriptions are also cleared automatically when the game stops **and on every level change**, exactly like `Input`, `Network` and the other script subsystems — the old level's closures must not keep firing after its scene is gone. Re-register the handlers you need in the new level's `OnLevelLoaded`. The wallet connection itself is untouched: `Web3.IsConnected()`, `Web3.GetAddress()` and `Web3.GetChainId()` keep working across levels, and read-only mode stays configured. If a transaction may still be in flight while you change level, remember its `txHash` and pick it back up with `Web3.WatchTransaction(txHash, …)` after the new level loads.

### Example — Read the chain before the player connects a wallet

```lua
function OnLevelLoaded()
    if not Web3.IsSupported() then return end

    -- No wallet needed: read straight from a public RPC
    Web3.SetReadOnlyChain("base")

    Web3.GetTokenBalance(
        "0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913",
        "0x1111111111111111111111111111111111111111",
        function(balance)
            Print("USDC held by the treasury: " .. balance)
        end,
        function(err)
            PrintError("Read failed: " .. err)
        end
    )
end
```

### Example — Connect wallet and show BNB balance

```lua
function OnLevelLoaded()
    if not Web3.IsSupported() then
        Print("Web3 is not available on this platform")
        return
    end

    Web3.OnWalletConnected(function(address)
        Print("Wallet connected: " .. address)

        -- Switch to BSC and get the balance
        Web3.SwitchChain("bsc")
        Web3.GetBalance(address, function(balance)
            Print("BNB Balance: " .. balance)
        end)
    end)

    Web3.OnChainChanged(function(chainId)
        Print("Chain changed to: " .. chainId)
    end)

    Web3.OnError(function(err, reqId)
        PrintError("Web3 Error: " .. err)
    end)
end

function OnConnectButtonPressed()
    Web3.ConnectWallet()
end
```

### Example — Send ETH transaction

```lua
function SendETH(toAddress, amount)
    Web3.SwitchChain("ethereum")

    Web3.SendTransaction(
        { to = toAddress, value = amount },
        function(txHash)
            Print("Transaction sent: " .. txHash)
        end,
        function(txHash)
            Print("Transaction confirmed: " .. txHash)
        end,
        function(err)
            PrintError("Transaction failed: " .. err)
        end
    )
end
```

### Example — Read ERC-20 token balance

```lua
local USDT_BSC = "0x55d398326f99059fF775485246999027B3197955"

function CheckUSDTBalance()
    Web3.SwitchChain("bsc")
    Web3.GetTokenBalance(USDT_BSC, nil, function(balance)
        Print("USDT Balance: " .. balance)
    end)
end
```

### Example — Call a smart contract (read-only)

```lua
local contractAddr = "0xYourContractAddress"
local abi = '[{"inputs":[{"internalType":"address","name":"player","type":"address"}],"name":"getScore","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"stateMutability":"view","type":"function"}]'

function GetPlayerScore()
    local addr = Web3.GetAddress()
    Web3.CallContract(
        { address = contractAddr, abi = abi, method = "getScore", args = '["' .. addr .. '"]' },
        function(result)
            Print("Player score: " .. result)
        end
    )
end
```

### Example — Write to a smart contract

```lua
local contractAddr = "0xYourContractAddress"
local abi = '[{"inputs":[{"internalType":"uint256","name":"score","type":"uint256"}],"name":"submitScore","outputs":[],"stateMutability":"nonpayable","type":"function"}]'

function SubmitScore(score)
    Web3.WriteContract(
        { address = contractAddr, abi = abi, method = "submitScore", args = '[' .. score .. ']' },
        function(txHash)
            Print("Score submitted, tx: " .. txHash)
        end,
        function(txHash)
            Print("Score confirmed on-chain!")
        end,
        function(err)
            PrintError("Failed to submit score: " .. err)
        end
    )
end
```

### Example — Check NFT ownership

```lua
local NFT_CONTRACT = "0xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D" -- BAYC

function CheckMyNFTs()
    Web3.GetNFTBalance(
        { address = NFT_CONTRACT },
        function(count)
            Print("You own " .. count .. " NFTs")
        end,
        function(err)
            PrintError("NFT query failed: " .. err)
        end
    )
end

function CheckNFTOwner(tokenId)
    Web3.GetNFTOwner(
        { address = NFT_CONTRACT, tokenId = tostring(tokenId) },
        function(owner)
            Print("Token #" .. tokenId .. " owned by: " .. owner)
        end
    )
end

function GetNFTMetadata(tokenId)
    Web3.GetTokenURI(
        { address = NFT_CONTRACT, tokenId = tostring(tokenId) },
        function(uri)
            Print("Token URI: " .. uri)
        end
    )
end
```

### Example — Watch an external transaction

```lua
function WatchPayment(txHash)
    Web3.WatchTransaction(txHash,
        function(hash)
            Print("Transaction confirmed: " .. hash)
        end,
        function(err)
            PrintError("Transaction failed: " .. err)
        end
        -- timeout defaults to 120000ms (2 minutes)
    )
end
```

### Example — ERC-1155 game item balance

```lua
local ITEMS_CONTRACT = "0xYourERC1155Contract"
local SWORD_TOKEN_ID = "1"

function CheckSwordCount()
    Web3.GetNFTBalance(
        { address = ITEMS_CONTRACT, tokenId = SWORD_TOKEN_ID },
        function(count)
            Print("You have " .. count .. " swords")
        end
    )
end
```

### Example — EIP-712 typed data signing (permit pattern)

```lua
function SignPermit(spender, value, nonce, deadline)
    local typedData = '{"types":{"EIP712Domain":[{"name":"name","type":"string"},{"name":"version","type":"string"},{"name":"chainId","type":"uint256"},{"name":"verifyingContract","type":"address"}],"Permit":[{"name":"owner","type":"address"},{"name":"spender","type":"address"},{"name":"value","type":"uint256"},{"name":"nonce","type":"uint256"},{"name":"deadline","type":"uint256"}]},"primaryType":"Permit","domain":{"name":"MyToken","version":"1","chainId":1,"verifyingContract":"0xTokenAddress"},"message":{"owner":"' .. Web3.GetAddress() .. '","spender":"' .. spender .. '","value":"' .. value .. '","nonce":"' .. nonce .. '","deadline":"' .. deadline .. '"}}'

    Web3.SignTypedData(typedData,
        function(signature)
            Print("Permit signature: " .. signature)
        end,
        function(err)
            PrintError("Signing failed: " .. err)
        end
    )
end
```

### Example — Listen to contract events

```lua
local subId = nil

function StartListening()
    local abi = '[{"type":"event","name":"Transfer","inputs":[{"indexed":true,"name":"from","type":"address"},{"indexed":true,"name":"to","type":"address"},{"indexed":true,"name":"tokenId","type":"uint256"}]}]'

    subId = Web3.SubscribeEvent(
        { address = "0xNFTContract", abi = abi, event = "Transfer" },
        function(eventJson)
            Print("Transfer event: " .. eventJson)
        end
    )
end

function StopListening()
    if subId then
        Web3.UnsubscribeEvent(subId)
        subId = nil
    end
end
```

### Example — Transfer an NFT

```lua
function TransferNFT(contractAddr, toAddr, tokenId)
    Web3.TransferNFT(
        { address = contractAddr, to = toAddr, tokenId = tostring(tokenId) },
        function(txHash) Print("Transfer sent: " .. txHash) end,
        function(txHash) Print("Transfer confirmed!") end,
        function(err) PrintError("Transfer failed: " .. err) end
    )
end

-- ERC-1155 transfer (5 potions)
function TransferPotions(contractAddr, toAddr)
    Web3.TransferNFT({
        address = contractAddr,
        to = toAddr,
        tokenId = "3",
        standard = "ERC1155",
        amount = "5"
    })
end
```

### Example — Approve and check allowance

```lua
local TOKEN = "0xTokenAddress"
local MARKETPLACE = "0xMarketplaceAddress"

-- Approve 100 tokens (decimals queried automatically)
function ApproveMarketplace()
    Web3.ApproveToken(
        { token = TOKEN, spender = MARKETPLACE, amount = "100" },
        function(txHash) Print("Approve tx sent: " .. txHash) end,
        function(txHash) Print("Approved!") end,
        function(err) PrintError("Approve failed: " .. err) end
    )
end

-- Unlimited approval
function ApproveUnlimited()
    Web3.ApproveToken(
        { token = TOKEN, spender = MARKETPLACE, amount = "max" },
        nil,
        function(txHash) Print("Unlimited approval confirmed!") end
    )
end

function CheckAllowance()
    Web3.GetAllowance(
        { token = TOKEN, spender = MARKETPLACE },
        function(allowance) Print("Allowance: " .. allowance) end
    )
end
```

### Example — Transfer ERC-20 tokens

```lua
local USDT = "0xdAC17F958D2ee523a2206206994597C13D831ec7"

function SendTokens(toAddr, amount)
    Web3.TransferToken(
        { token = USDT, to = toAddr, amount = amount },
        function(txHash) Print("Transfer sent: " .. txHash) end,
        function(txHash) Print("Transfer confirmed!") end,
        function(err) PrintError("Transfer failed: " .. err) end
    )
end
```

### Example — Set approval for all NFTs

```lua
local NFT_CONTRACT = "0xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D"
local MARKETPLACE = "0xMarketplaceAddress"

function ApproveAllNFTs()
    Web3.SetApprovalForAll(
        { address = NFT_CONTRACT, operator = MARKETPLACE, approved = true },
        function(txHash) Print("Approval tx sent: " .. txHash) end,
        function(txHash) Print("All NFTs approved for marketplace!") end,
        function(err) PrintError("Approval failed: " .. err) end
    )
end

function RevokeAllNFTs()
    Web3.SetApprovalForAll(
        { address = NFT_CONTRACT, operator = MARKETPLACE, approved = false },
        nil,
        function(txHash) Print("Approval revoked!") end
    )
end
```

### Example — Watch transaction with custom timeout

```lua
function WatchWithTimeout(txHash)
    Web3.WatchTransaction(txHash,
        function(hash)
            Print("Transaction confirmed: " .. hash)
        end,
        function(err)
            PrintError("Transaction failed or timed out: " .. err)
        end,
        60000  -- 60 second timeout
    )
end
```

### Example — Custom IPFS gateway

```lua
-- Use Pinata gateway instead of default ipfs.io
Web3.SetIPFSGateway("https://gateway.pinata.cloud/ipfs/")

function ShowNFTWithCustomGateway(contractAddr, tokenId)
    Web3.GetTokenURI(
        { address = contractAddr, tokenId = tostring(tokenId) },
        function(uri)
            Web3.ResolveMetadata(uri, function(metadataJson)
                Print("NFT metadata: " .. metadataJson)
            end)
        end
    )
end
```

### Example — Estimate gas before sending

```lua
function EstimateTransferCost(toAddr, amount)
    Web3.EstimateGas(
        { to = toAddr, value = amount },
        function(gasJson)
            Print("Gas estimation: " .. gasJson)
        end,
        function(err) PrintError("Estimation failed: " .. err) end
    )
end
```

### Example — Get full transaction receipt

```lua
function GetReceipt(txHash)
    Web3.GetTransactionReceipt(txHash,
        function(receiptJson)
            Print("Receipt: " .. receiptJson)
        end
    )
end
```

### Example — Multi-provider wallet selection

```lua
function ListWallets()
    local providersJson = Web3.GetProviders()
    Print("Available wallets: " .. providersJson)
end

function ConnectCoinbaseWallet()
    Web3.ConnectWithProvider("com.coinbase.wallet", function(address)
        Print("Connected via Coinbase: " .. address)
    end)
end
```

### Example — Resolve NFT metadata from IPFS

```lua
function ShowNFTMetadata(contractAddr, tokenId)
    Web3.GetTokenURI(
        { address = contractAddr, tokenId = tostring(tokenId) },
        function(uri)
            Web3.ResolveMetadata(uri, function(metadataJson)
                Print("NFT metadata: " .. metadataJson)
            end)
        end
    )
end
```

### Example — Multicall batch read

```lua
local NFT = "0xNFTContract"

function BatchCheckOwnership(tokenIds)
    local calls = {}
    local abi = '[{"inputs":[{"name":"tokenId","type":"uint256"}],"name":"ownerOf","outputs":[{"name":"","type":"address"}],"stateMutability":"view","type":"function"}]'
    for _, id in ipairs(tokenIds) do
        table.insert(calls, { address = NFT, abi = abi, method = "ownerOf", args = '[' .. id .. ']' })
    end
    Web3.Multicall(calls, function(resultsJson)
        Print("Owners: " .. resultsJson)
    end)
end
```

---

## 55. LocalPlayer — Local Multiplayer and Split-Screen

> **Type:** Global functions. For couch co-op / local multiplayer with up to 4 players.

### Player registration

```lua
-- Register a player with a device
RegisterLocalPlayer(0, "keyboard")           -- Player 0 on keyboard+mouse
RegisterLocalPlayer(1, "gamepad", 0)         -- Player 1 on gamepad 0
RegisterLocalPlayer(2, "gamepad", 1)         -- Player 2 on gamepad 1
RegisterLocalPlayer(3, "controller", 2)      -- Player 3 on gamepad 2 ("controller"/"pad" also work)

-- Unregister
UnregisterLocalPlayer(2)
UnregisterAllLocalPlayers()

-- Query
local registered = IsLocalPlayerRegistered(0)  -- → bool
local count = GetLocalPlayerCount()            -- → int (0-4)

-- Device info
local device = GetLocalPlayerDevice(1)
-- → { type = "gamepad", index = 0, active = true }

-- Auto-assign all connected gamepads
AutoAssignLocalPlayers()
```

**Device type strings:** `"keyboard"`, `"keyboard_mouse"`, `"kb"`, `"gamepad"`, `"controller"`, `"pad"`

### Player input

```lua
-- Movement (normalized -1..1). For keyboard player, default keys: WASD
local move = GetPlayerMovement(0)               -- → {x, y}
local move = GetPlayerMovement(0, "left", "right", "down", "up")  -- Custom keys
SetVelocity(move.x * speed, move.y * speed)

-- Button state (abstract names or raw buttons)
if IsPlayerButtonPressed(0, "jump") then Jump() end
if IsPlayerButtonJustPressed(1, "attack") then Attack() end
if IsPlayerButtonJustReleased(0, "confirm") then Confirm() end

-- Right stick (gamepad) / mouse delta (keyboard player)
local aim = GetPlayerAimStick(0)  -- → {x, y}

-- Triggers (gamepad only, 0.0-1.0)
local lt = GetPlayerTrigger(0, "left")   -- also "lt" or "l2"
local rt = GetPlayerTrigger(0, "right")  -- also "rt" or "r2"
```

**Abstract button names:**

| Name | Keyboard | Gamepad |
|------|----------|---------|
| `confirm` | Enter | A |
| `cancel` | Escape | B |
| `jump` | Space | A |
| `attack` | J | X |
| `interact` | E | Y |
| `pause` | Escape | Start |

**Raw gamepad buttons:** `a`, `b`, `x`, `y`, `start`, `back`, `lb`, `rb`, `leftshoulder`, `rightshoulder`, `dpadup`, `dpaddown`, `dpadleft`, `dpadright`

### Rumble and device info

```lua
-- Vibration (gamepad only)
SetPlayerRumble(0, 0.5, 0.8, 300)   -- playerIndex, lowFreq, highFreq, durationMs
StopPlayerRumble(0)

-- Controller name and type
local name = GetPlayerGamepadName(1)   -- → "Xbox Controller" or "Keyboard & Mouse"
local type = GetPlayerGamepadType(1)   -- → "xbox", "ps", "switch", "keyboard_mouse", "none"
```

### Split-screen layout

```lua
-- Set layout
SetSplitScreenLayout("horizontal")   -- Top/bottom 2-player
SetSplitScreenLayout("vertical")     -- Left/right 2-player
SetSplitScreenLayout("quad")         -- 4-player grid
SetSplitScreenLayout("three_top1_bottom2")  -- 1 top + 2 bottom
SetSplitScreenLayout("three_left1_right2")  -- 1 left + 2 right
SetSplitScreenLayout("auto")         -- Auto-pick layout from current player count
SetSplitScreenLayout("none")         -- Disable

-- Shortcut: force Auto mode (equivalent to SetSplitScreenLayout("auto"))
ApplyAutoSplitLayout()

local layout = GetSplitScreenLayout()  -- → "horizontal", "vertical", "auto", etc.
local active = IsSplitScreenActive()   -- → bool

-- Resolved layout after "auto" has been evaluated (never returns "auto")
local eff = GetEffectiveSplitScreenLayout()  -- → "none", "vertical", "quad", ...

-- True when local multiplayer has 2+ players registered
local mp = IsLocalMultiplayerActive()  -- → bool

-- Orientation used by "auto" layout when there are exactly 2 players
SetTwoPlayerOrientation("horizontal")  -- "horizontal" (top/bottom) or "vertical" (left/right)
local orient = GetTwoPlayerOrientation()  -- → "horizontal" or "vertical"

-- Divider between split viewports (pixels + RGBA color)
SetSplitScreenDivider(2, 0.0, 0.0, 0.0, 1.0)  -- thicknessPx, r, g, b, a (color args optional, default black)
SetSplitScreenDivider(0)                      -- disable divider
local px = GetSplitScreenDividerPx()          -- → current thickness in pixels

-- Get computed viewport rect for a player slot
local rect = GetPlayerViewportRect(0)  -- → {x, y, width, height} (normalized 0-1)

-- Same rect in pixel coordinates (y is top-down, useful for HUD / scissor)
local pxRect = GetPlayerViewportPixels(0, windowW, windowH)
                                       -- → {x, y, width, height} (integers)
```

**What each player gets in their viewport.** With split screen active, every
player view runs the full engine pipeline independently: its own frustum and
culling, its own shadow maps, sprites/flipbooks/skeletons/tilemaps (including
animated tiles and stencil effects), FX, fog of war, GI, video and cinema fades.
Post-processing is per player too: volumes are evaluated at each player camera's
position and every view owns its own effect stack (bloom, grading, vignette,
motion blur, auto-exposure, FXAA) with independent temporal state.

**Widgets.** A widget instance has a `Player Index`: `-1` renders on every
player's screen, `0-3` only on that player's screen. Mouse and touch input is
automatically routed to the viewport under the cursor: hit-testing happens in
that viewport's local coordinates and only against that player's widgets (plus
shared `-1` ones). Gamepad UI navigation is driven by the device of the widget's
owning player; shared (`-1`) widgets can be navigated by any registered player.

**Cameras.** The primary camera resolves its own split-screen slot: its
`Player Index` if set, otherwise the first registered player. Camera follow
centers on that viewport's size. Non-primary cameras with `Player Index >= 0`
render their players' viewports (the player must be registered). Layout slots
are assigned in registered-player order, so a sparse registration (e.g. players
0 and 2) still packs the viewports tightly.

**Audio.** Each player camera becomes a spatial-audio listener (up to 4):
positional sounds attenuate relative to the nearest player's screen.

### Camera management

```lua
-- Apply split-screen layout to all cameras with PlayerIndex >= 0
ApplySplitScreenCameras()

-- Manually set viewport for a specific player's camera
SetPlayerCameraViewport(0, 0.0, 0.0, 0.5, 1.0)  -- Left half

-- Find the camera entity for a player
local camId = GetPlayerCameraEntity(1)  -- → entityId or nil
```

### Input detection (join screen)

```lua
-- Detect any new input (for "Press Start to join" screens)
function OnUpdate(dt)
    local input = DetectPlayerInput()
    if input then
        -- input = { type = "gamepad", index = 0, input = "a" }
        -- or:     { type = "keyboard_mouse", index = 0, input = "space" }
        RegisterLocalPlayer(GetLocalPlayerCount(), input.type, input.index)
    end
end

-- Auto-assign both keyboard (slot 0) and all connected gamepads (remaining slots)
AutoAssignLocalDevices(true)   -- preferKeyboard = true (keyboard goes to slot 0)
AutoAssignLocalDevices(false)  -- gamepads only
```

### Complete example — 2-player couch co-op

```lua
function OnLevelStart()
    -- Player 1 on keyboard, Player 2 on first gamepad
    RegisterLocalPlayer(0, "keyboard")
    RegisterLocalPlayer(1, "gamepad", 0)

    -- Set up vertical split-screen
    SetSplitScreenLayout("vertical")
    ApplySplitScreenCameras()
end

function OnUpdate(dt)
    -- Player 1 movement
    local move1 = GetPlayerMovement(0)
    -- Player 2 movement
    local move2 = GetPlayerMovement(1)

    -- Player 1 actions
    if IsPlayerButtonJustPressed(0, "jump") then
        -- Player 1 jumps
    end

    -- Player 2 actions
    if IsPlayerButtonJustPressed(1, "attack") then
        -- Player 2 attacks
    end
end
```

### Quick reference

| Function | Description |
|----------|-------------|
| `RegisterLocalPlayer(idx, device, [devIdx])` | Register player with device |
| `UnregisterLocalPlayer(idx)` | Unregister player |
| `UnregisterAllLocalPlayers()` | Remove all players |
| `IsLocalPlayerRegistered(idx)` | Check if registered |
| `GetLocalPlayerCount()` | Active player count |
| `GetLocalPlayerDevice(idx)` | Device info table |
| `AutoAssignLocalPlayers()` | Auto-assign gamepads |
| `AutoAssignLocalDevices([preferKeyboard])` | Auto-assign keyboard + gamepads |
| `IsLocalMultiplayerActive()` | 2+ players registered |
| `GetPlayerMovement(idx, [l,r,d,u])` | Movement vector `{x,y}` |
| `IsPlayerButtonPressed(idx, btn)` | Button held |
| `IsPlayerButtonJustPressed(idx, btn)` | Button just pressed |
| `IsPlayerButtonJustReleased(idx, btn)` | Button just released |
| `GetPlayerAimStick(idx)` | Right stick / mouse delta |
| `GetPlayerTrigger(idx, side)` | Trigger value (0-1) |
| `SetPlayerRumble(idx, lo, hi, ms)` | Start rumble |
| `StopPlayerRumble(idx)` | Stop rumble |
| `GetPlayerGamepadName(idx)` | Controller name |
| `GetPlayerGamepadType(idx)` | Controller type |
| `SetSplitScreenLayout(layout)` | Set split layout (incl. `"auto"`) |
| `GetSplitScreenLayout()` | Current layout name |
| `GetEffectiveSplitScreenLayout()` | Resolved layout (after `"auto"`) |
| `ApplyAutoSplitLayout()` | Shortcut for `"auto"` |
| `SetTwoPlayerOrientation(mode)` | Orientation used by `"auto"` for 2 players |
| `GetTwoPlayerOrientation()` | `"horizontal"` / `"vertical"` |
| `SetSplitScreenDivider(px, [r,g,b,a])` | Divider thickness + color |
| `GetSplitScreenDividerPx()` | Current divider thickness |
| `IsSplitScreenActive()` | Is split-screen on |
| `GetPlayerViewportRect(idx)` | Viewport rect for player (0..1) |
| `GetPlayerViewportPixels(idx, w, h)` | Viewport rect in pixels |
| `ApplySplitScreenCameras()` | Apply layout to cameras |
| `SetPlayerCameraViewport(idx, x,y,w,h)` | Manual viewport |
| `GetPlayerCameraEntity(idx)` | Camera entity ID |
| `DetectPlayerInput()` | Detect any new input |

---

## 56. Video — Runtime Video Playback

> **Type:** Global functions. `Video` table.
>
> Full-screen video playback for intros, cutscenes, and in-game cinematics.
> The video overlays the entire game screen with correct aspect ratio (letterbox/pillarbox).
> Supports skip input (ESC / Space / Enter / Gamepad A / Start), audio, volume control, looping,
> and the `VideoFinished` Lua event for scene transitions.
>
> Video files are played directly (`.mp4`, `.webm`, `.avi`, `.mkv` — any format supported by FFmpeg).
> Works with both loose files on disk and files packed in `.ICEPAK` archives via VFS.
>
> **Note:** Available on all six platforms. **Windows**, **Linux**, **macOS** and **Android** decode through
> FFmpeg; **iOS** plays through AVFoundation (H.264/HEVC only — WebM/VP9 is not decodable there, so video
> cooking falls back to PassThrough for iOS builds); **Web** plays through the browser's `<video>` element.

### Playback

```lua
-- Play a video (path relative to Content/)
local ok = Video.Play("Videos/intro.mp4")

-- Stop the current video
Video.Stop()

-- Pause / Resume
Video.Pause()
Video.Resume()

-- Skip the video (triggers VideoFinished event)
Video.Skip()
```

### State checks

```lua
local playing  = Video.IsPlaying()    -- true while video is playing
local paused   = Video.IsPaused()     -- true if paused
local finished = Video.IsFinished()   -- true after video ends or is skipped
local active   = Video.HasActiveVideo() -- true if playing or paused
local path     = Video.GetPath()      -- current video file path
```

### Time and progress

```lua
local t   = Video.GetTime()       -- current playback time (seconds)
local dur = Video.GetDuration()   -- total duration (seconds)
local pct = Video.GetProgress()   -- progress 0.0–1.0
```

### Volume

```lua
Video.SetVolume(0.8)              -- set audio volume (0.0–1.0)
local vol = Video.GetVolume()     -- get current volume
```

### Skip and looping settings

```lua
-- Allow player to skip with ESC/Space/Enter/Gamepad
Video.SetSkippable(true)
local canSkip = Video.IsSkippable()

-- Loop the video
Video.SetLooping(true)
local loops = Video.IsLooping()
```

### VideoFinished event

When a video finishes naturally or is skipped, the engine emits the `VideoFinished` event.
Use the event system to listen for it and trigger scene transitions:

```lua
function OnInit()
    On("VideoFinished", function(videoPath)
        Print("Video finished: " .. videoPath)
        LoadLevel("Content/Maps/Level1.icemap")
    end)
end

function OnStart()
    Video.Play("Videos/intro.mp4")
    Video.SetSkippable(true)
end
```

### Practical example — intro with fade transition

```lua
function OnInit()
    On("VideoFinished", function(path)
        Cinema.FadeOut(0.5)
        Delay(0.5, function()
            LoadLevel("Content/Maps/MainMenu.icemap")
        end)
    end)
end

function OnStart()
    Video.Play("Videos/studio_logo.mp4")
    Video.SetSkippable(true)
    Video.SetVolume(1.0)
end
```

### Practical example — in-game cutscene

```lua
function PlayCutscene(videoFile)
    Video.Play("Videos/Cutscenes/" .. videoFile)
    Video.SetSkippable(true)
    Video.SetLooping(false)
end

function OnInit()
    On("VideoFinished", function(path)
        Print("Cutscene ended, resuming gameplay")
    end)
end

function OnTriggerEnter(other)
    local tag = Entity.GetTag(other)
    if tag == "CutsceneTrigger" then
        PlayCutscene("boss_intro.mp4")
    end
end
```

### API reference

| Function | Returns | Description |
|----------|---------|-------------|
| `Video.Play(path)` | `bool` | Start video playback. Path is relative to `Content/`. Returns `true` on success |
| `Video.Stop()` | — | Stop and reset the current video |
| `Video.Pause()` | — | Pause playback |
| `Video.Resume()` | — | Resume playback |
| `Video.Skip()` | — | Skip the video (emits `VideoFinished` event) |
| `Video.IsPlaying()` | `bool` | `true` while video is actively playing |
| `Video.IsPaused()` | `bool` | `true` if video is paused |
| `Video.IsFinished()` | `bool` | `true` after the video ends or is skipped |
| `Video.GetTime()` | `float` | Current playback time in seconds |
| `Video.GetDuration()` | `float` | Total video duration in seconds |
| `Video.GetProgress()` | `float` | Playback progress (0.0–1.0) |
| `Video.SetVolume(volume)` | — | Set audio volume (0.0–1.0) |
| `Video.GetVolume()` | `float` | Get current audio volume |
| `Video.SetSkippable(bool)` | — | Allow/disallow skip via input (ESC, Space, Enter, Gamepad A/Start) |
| `Video.IsSkippable()` | `bool` | Check if the video can be skipped |
| `Video.SetLooping(bool)` | — | Enable/disable video looping |
| `Video.IsLooping()` | `bool` | Check if looping is enabled |
| `Video.GetPath()` | `string` | Get the path of the current video |
| `Video.HasActiveVideo()` | `bool` | `true` if a video is currently playing or paused |

### Events

| Event | Parameters | Description |
|-------|------------|-------------|
| `VideoFinished` | `path` (string) | Emitted when a video finishes playback or is skipped |

---

## 57. Voice — Microphone, Opus, Voice Analysis

> **Type:** Global. Single-player friendly — does **not** require networking. Works for any game that wants to scream, shout, sing or detect spoken words via the microphone.
> **Opus:** encode/decode needs the Opus codec — check for it at runtime with `Voice.HasOpus()`. Capture, playback, recording and volume analysis work either way.

The Voice API gives Lua direct access to:

- **Microphone capture** (SDL3 default recording device, 16-bit PCM).
- **Opus encoding/decoding** (the same codec used by `Network.EnableVoiceChat` — but available to any local game).
- **Audio playback** of raw PCM or Opus-encoded data.
- **Volume analysis** — real-time RMS, peak, dB, exponentially smoothed loudness.
- **Voice detection** — IsSpeaking() / IsScreaming() against configurable thresholds.
- **WAV recording** — write the live mic stream to a 16-bit PCM `.wav` file.
- **Loopback** — pipe the mic straight to the speakers (stream-at-yourself effect).

Internally a frame queue is filled by `Voice.Update()` (call it once per frame, e.g. inside `OnUpdate`). One frame = `sampleRate / 50` samples (20 ms; 960 @ 48 kHz).

### Capture / Playback / Codec lifecycle

| Function | Returns | Description |
|----------|---------|-------------|
| Voice.StartCapture(sampleRate?, channels?) | bool | Open the default mic. Defaults: 48000 Hz, 1 channel |
| Voice.StopCapture() | — | Stop the mic |
| Voice.IsCapturing() | bool | Is the mic running? |
| Voice.StartPlayback(sampleRate?, channels?) | bool | Open the default playback device |
| Voice.StopPlayback() | — | Close playback |
| Voice.IsPlaybackActive() | bool | Is playback initialized? |
| Voice.SetPlaybackVolume(volume) | — | 0.0–2.0, applied to all decoded/written PCM |
| Voice.GetPlaybackVolume() | float | Current playback volume |
| Voice.InitCodec(sampleRate?, channels?, bitrate?) | bool | Create Opus encoder+decoder. Defaults: 48000 Hz, 1 ch, 32000 bps |
| Voice.ShutdownCodec() | — | Destroy the codec |
| Voice.IsCodecReady() | bool | true if Opus is initialized and valid |
| Voice.HasOpus() | bool | true if Opus encode/decode is available in this build |
| Voice.Shutdown() | — | Stop everything (capture, playback, recording, codec) |

### Frame stream (microphone → Lua)

| Function | Returns | Description |
|----------|---------|-------------|
| Voice.Update() | — | Pump captured frames; updates RMS/peak/smoothed; writes WAV/loopback. Call every frame |
| Voice.HasFrame() | bool | True if at least one captured frame is queued |
| Voice.QueuedFrameCount() | int | Number of queued frames |
| Voice.SetMaxQueuedFrames(n) | — | Drop oldest frames beyond this limit (default 32) |
| Voice.ReadFrame() | string \| nil | Pop oldest frame as binary PCM string (int16 LE) — fast path |
| Voice.ReadFrameTable() | table \| nil | Pop oldest frame as a Lua array of integers |
| Voice.ClearFrames() | — | Discard all queued frames |
| Voice.GetSampleRate() | int | Current capture/codec sample rate |
| Voice.GetChannels() | int | Current channel count |
| Voice.GetFrameSize() | int | Samples per frame |
| Voice.SetFrameSize(n) | — | Override frame size (advanced) |

### Opus encode / decode

| Function | Returns | Description |
|----------|---------|-------------|
| Voice.Encode(pcmString) | string \| nil | Encode one PCM frame (binary string from `ReadFrame`) |
| Voice.EncodeTable(samples) | string \| nil | Encode from a Lua array of int16 samples |
| Voice.Decode(opusString) | string \| nil | Decode an Opus packet to a PCM binary string |
| Voice.DecodeTable(opusString) | table \| nil | Decode an Opus packet to a Lua array |

### Direct playback

| Function | Returns | Description |
|----------|---------|-------------|
| Voice.PlayPCM(pcmString) | — | Push raw PCM (binary string) to the speakers |
| Voice.PlayPCMTable(samples) | — | Push raw PCM (Lua array) |
| Voice.PlayEncoded(opusString) | bool | Decode Opus and push to speakers in one step |

### Volume analysis & voice detection

| Function | Returns | Description |
|----------|---------|-------------|
| Voice.GetVolume() | float | RMS of the last frame, normalized 0..1 |
| Voice.GetVolumePeak() | float | Absolute peak of the last frame, 0..1 |
| Voice.GetVolumeDB() | float | RMS in decibels (clamped to `-120 dB` floor) |
| Voice.GetSmoothedVolume() | float | Exponentially smoothed RMS — best for UI meters |
| Voice.SetSmoothing(factor) | — | 0..0.9999 (default 0.85). Higher = smoother / slower |
| Voice.GetSmoothing() | float | Current smoothing factor |
| Voice.SetInputGain(g) | — | Multiply mic samples (0..32). Default 1.0 |
| Voice.GetInputGain() | float | Current input gain |
| Voice.SetVoiceThreshold(t) | — | Default threshold for `IsSpeaking` (0..1) |
| Voice.GetVoiceThreshold() | float | Current voice threshold |
| Voice.IsSpeaking(threshold?) | bool | `smoothed >= threshold` (default uses `VoiceThreshold`) |
| Voice.IsScreaming(threshold?) | bool | `peak >= threshold` (default 0.45) |

### Loopback (mic → speakers)

| Function | Returns | Description |
|----------|---------|-------------|
| Voice.SetLoopback(enabled) | — | While `true` and capture is running, every captured frame is auto-played |
| Voice.IsLoopback() | bool | Loopback state |

### WAV recording

Writes a 16-bit PCM `.wav` using the current capture rate/channels. The file's RIFF/data sizes are finalized on `StopRecording` or `Shutdown`.

| Function | Returns | Description |
|----------|---------|-------------|
| Voice.StartRecording(filePath) | bool | Begin writing the live mic to a WAV file. Creates parent dirs |
| Voice.StopRecording() | bool | Finalize and close the WAV file |
| Voice.IsRecording() | bool | Is recording active? |
| Voice.GetRecordedDuration() | float | Duration recorded so far, in seconds |

### Constants

| Constant | Value | Description |
|----------|-------|-------------|
| Voice.DEFAULT_SAMPLE_RATE | 48000 | Default Opus / capture rate |
| Voice.DEFAULT_CHANNELS | 1 | Mono mic by default |
| Voice.DEFAULT_BITRATE | 32000 | Default Opus bitrate |

### Example: "Scream higher to jump higher"

```lua
function OnStart()
    Voice.StartCapture()
    Voice.SetSmoothing(0.7)
    Voice.SetInputGain(1.5)
end

function OnUpdate(dt)
    Voice.Update()
    local loudness = Voice.GetSmoothedVolume()
    self.JumpForce = 200 + loudness * 1500
    if Voice.IsScreaming(0.6) then
        TriggerScreamAttack()
    end
end

function OnDestroy()
    Voice.Shutdown()
end
```

### Example: "Record a voice clip and play it back"

```lua
function OnStart()
    Voice.StartCapture()
    Voice.StartPlayback()
end

function StartRecord()
    Voice.StartRecording("Saved/voice_clip.wav")
end

function StopRecord()
    Voice.StopRecording()
end

function PlayBackLive()
    Voice.SetLoopback(true)
end

function OnUpdate(dt)
    Voice.Update()
end
```

### Example: Local Opus round-trip (encode → decode → play)

```lua
function OnStart()
    Voice.StartCapture()
    Voice.StartPlayback()
    Voice.InitCodec(48000, 1, 32000)
end

function OnUpdate(dt)
    Voice.Update()
    while Voice.HasFrame() do
        local pcm = Voice.ReadFrame()
        local opus = Voice.Encode(pcm)
        if opus then
            Voice.PlayEncoded(opus)
        end
    end
end
```

### Notes

- Frame format: signed 16-bit PCM, little-endian, interleaved channels. `Voice.ReadFrame()` returns a Lua **string** containing raw bytes — passing it directly into `Voice.Encode` is the cheapest path.
- `Voice.IsScreaming` uses peak amplitude (instant), `Voice.IsSpeaking` uses smoothed RMS (stable) — pick the right one for your gameplay.
- The `Voice` system is fully independent of `Network`. It is safe to use both at the same time.
- `Voice.Update()` is thread-safe and idempotent; calling it from `OnUpdate` is enough for all features (loopback, WAV recording, volume meters, frame queue).

---

## 58. Replay — Recording, Playback, Killcams

The `Replay` module records the runtime state of the scene (entity transforms, physics velocities, custom values) into a timeline and plays it back later. It is designed for **killcams**, **death replays**, **match recording**, **ghost runs**, **trailer captures** and **debug rewind**.

**Highlights**

- Records `TransformComponent` (Position / Scale / Rotation / Visible / Enabled) for every entity that has an `IDComponent`.
- Records linear and angular velocity for entities with a `RigidbodyComponent`.
- Records arbitrary numeric and string values per frame (`Replay.RecordValue`, `Replay.RecordString`) — useful for HUD: score, ammo, state.
- Two recording modes: **full session recording** (`StartRecording` / `StopRecording`) and **circular killcam buffer** (`StartBuffer` / `CaptureBuffer`).
- Playback with **interpolation**, configurable **speed** (slow-mo / fast-forward), **loop**, **seek**, **pause/resume**.
- Save / Load to JSON inside the user's `Saves/Replays/` folder.
- Identifies entities by **UUID** (`IDComponent.ID`), so replays survive editor reloads and entt-handle changes.

> Replays are updated automatically every script tick after `TimeLua / Coroutine / Tween` and before entity `OnUpdate`. The system is reset together with the runtime when you stop play.

### Lifecycle and modes

```lua
Replay.GetMode()        -- 0 = Idle, 1 = Recording, 2 = Playing
Replay.Mode.Idle        -- 0
Replay.Mode.Recording   -- 1
Replay.Mode.Playing     -- 2
```

A replay can only be in **one** mode at a time. Calling `Play()` while recording stops recording first; calling `StartRecording()` while playing stops playback first.

The **killcam buffer** is *independent* of the recording/playing mode: you can have a circular buffer running in the background while playing back a previously captured replay.

### Recording API

| Function | Description |
|---|---|
| `Replay.StartRecording(name?, rate?)` | Start recording. `name` (string, optional) — label saved with the file. `rate` (Hz, default `60`) — sampling rate. |
| `Replay.StopRecording()` | Stop recording and copy the recorded data into the active replay (ready to `Play()` or `Save()`). |
| `Replay.IsRecording()` | `true` while recording. |
| `Replay.GetRecordedDuration()` | Length of the in-progress recording, in seconds. |
| `Replay.GetRecordedFrameCount()` | Number of frames recorded so far. |

```lua
Replay.StartRecording("match01", 30)  -- record 30 frames per second
-- ... gameplay runs ...
Replay.StopRecording()
Replay.Save("match01")
```

### Killcam (circular buffer) API

The buffer continuously samples the last *N* seconds. When something dramatic happens (death, kill, goal), call `CaptureBuffer()` to freeze the buffer into a playable replay.

| Function | Description |
|---|---|
| `Replay.StartBuffer(seconds, rate?)` | Start the circular buffer. `seconds` — how much history to keep. `rate` (Hz, default `60`). |
| `Replay.StopBuffer()` | Stop the buffer and discard its contents. |
| `Replay.IsBuffering()` | `true` while the buffer is running. |
| `Replay.GetBufferDuration()` | Configured buffer length in seconds. |
| `Replay.CaptureBuffer(name?)` | Convert the current buffer contents into the active replay. Returns `true` on success. |

```lua
function OnLevelStart()
    Replay.StartBuffer(8.0, 60)   -- always keep last 8 seconds
end

function OnPlayerDeath()
    if Replay.CaptureBuffer("death_killcam") then
        Replay.SetSpeed(0.4)      -- slow-mo
        Replay.Play()
    end
end
```

### Playback API

| Function | Description |
|---|---|
| `Replay.Play()` | Start playback from `t = 0`. Returns `true` if there is something to play. |
| `Replay.Pause()` | Pause playback (time freezes, scene stays at current frame). |
| `Replay.Resume()` | Resume playback. |
| `Replay.Stop()` | Stop playback and reset playhead to `0`. |
| `Replay.IsPlaying()` | `true` if `Play()` was called and `Stop()` has not yet been called. |
| `Replay.IsPaused()` | `true` if currently playing **and** paused. |
| `Replay.GetTime()` | Current playhead time, in seconds. |
| `Replay.SetTime(t)` | Seek to time `t` (clamped to `[0, GetDuration()]`). |
| `Replay.GetDuration()` | Total length of the active replay, in seconds. |
| `Replay.GetFrameCount()` | Number of frames in the active replay. |
| `Replay.GetProgress()` | `0..1` progress (`time / duration`). |
| `Replay.SetSpeed(s)` / `GetSpeed()` | Playback speed: `0.5` = slow-mo, `2.0` = fast, `-1.0` = backwards. |
| `Replay.SetLoop(b)` / `GetLoop()` | Loop the replay automatically. |
| `Replay.SetInterpolation(b)` / `GetInterpolation()` | Linear interpolation between sampled frames (default `true`). Disable for snapshot-accurate playback. |

During playback, the engine **overwrites** `TransformComponent` and (when present) the Box2D body transform / linear velocity / angular velocity of every recorded entity. Other systems (sprites, animator, camera) read from these components, so visuals follow automatically.

```lua
Replay.Load("last_match")
Replay.SetSpeed(0.5)
Replay.SetLoop(false)
Replay.Play()

function OnLevelUpdate(dt)
    if Replay.IsPlaying() then
        ShowReplayHUD(Replay.GetTime(), Replay.GetDuration(), Replay.GetProgress())
    end
end
```

### Tracking which entities to record

By default the system records **every** entity that has an `IDComponent` and a `TransformComponent`. For huge scenes you can limit recording to a whitelist.

| Function | Description |
|---|---|
| `Replay.TrackAll(true\|false)` | `true` (default) — record every entity. `false` — record only those passed to `TrackEntity`. |
| `Replay.TrackEntity(entityOrUUID)` | Add an entity to the whitelist. Accepts an entity handle (number) or a UUID string. |
| `Replay.UntrackEntity(entityOrUUID)` | Remove from the whitelist. |
| `Replay.ClearTracked()` | Clear the whitelist. |
| `Replay.IsTracked(entityOrUUID)` | `true` if the entity will be recorded. |

```lua
Replay.TrackAll(false)
Replay.TrackEntity(player)
Replay.TrackEntity(boss)
Replay.StartRecording("duel")
```

### Custom values per frame

Great for replay HUDs (score, ammo, phase) and analytics.

| Function | Description |
|---|---|
| `Replay.RecordValue(key, number)` | Stamp a number for the next captured frame. |
| `Replay.RecordString(key, string)` | Stamp a string for the next captured frame. |
| `Replay.GetValue(key)` | During playback: number value at the current playhead time, or `nil` if no value was recorded yet. Carries the last recorded value forward. |
| `Replay.GetString(key)` | Same, for string values. |

```lua
function OnLevelUpdate(dt)
    if Replay.IsRecording() then
        Replay.RecordValue("score", currentScore)
        Replay.RecordString("phase", currentPhase)
    end
    if Replay.IsPlaying() then
        local s = Replay.GetValue("score") or 0
        local p = Replay.GetString("phase") or ""
        DrawHUD(s, p)
    end
end
```

### Save / Load

Replays are written to `Saves/Replays/<name>.replay.json` inside the platform's writable storage. The path is sanitised: `..` and absolute paths are rejected.

| Function | Description |
|---|---|
| `Replay.Save(name)` | Save the **active** replay (the one created by `StopRecording` / `CaptureBuffer` / `Load`). Returns `true` on success. |
| `Replay.Load(name)` | Load a replay file into the active replay. Returns `true` on success. |
| `Replay.HasReplay()` | `true` if there is an active replay. |
| `Replay.Clear()` | Drop the active replay. |
| `Replay.Reset()` | Reset all replay state (recording, buffer, playback, tracking, custom values). |

```lua
Replay.Save("match01")        -- -> Saves/Replays/match01.replay.json
Replay.Load("match01")
Replay.Play()
```

### Inspecting samples manually (custom rendering / ghosts)

Useful for ghost replays, mini-map traces, or rendering trails over a still scene without overwriting it.

| Function | Description |
|---|---|
| `Replay.GetEntitySample(entityOrUUID, time?)` | Returns a table at the given time (default = current playhead). Fields: `x, y, z, scaleX, scaleY, rotation, velocityX, velocityY, angularVelocity, visible, enabled, hasBody`. Returns `nil` if no data. |
| `Replay.GetEntityPosition(entityOrUUID, time?)` | Shortcut returning `{x, y, z}` or `nil`. |

```lua
local s = Replay.GetEntitySample(player, Replay.GetTime() - 0.5)
if s then
    DrawGhost(s.x, s.y, s.rotation, s.scaleX, s.scaleY)
end
```

### End-to-end example: match recording with replay viewer

```lua
function OnLevelStart()
    Replay.TrackAll(true)
    Replay.StartRecording("match", 30)
end

function OnLevelUpdate(dt)
    if Replay.IsRecording() then
        Replay.RecordValue("score", GetScore())
    end
end

function OnMatchEnd()
    Replay.StopRecording()
    Replay.Save("last_match")
end

function WatchLastMatch()
    if Replay.Load("last_match") then
        Replay.SetSpeed(1.0)
        Replay.SetLoop(false)
        Replay.SetInterpolation(true)
        Replay.Play()
    end
end
```

### End-to-end example: 8-second death killcam in slow-mo

```lua
function OnLevelStart()
    Replay.StartBuffer(8.0, 60)
end

function OnPlayerKilled()
    PauseGame()
    if Replay.CaptureBuffer("killcam") then
        Replay.SetSpeed(0.35)
        Replay.Play()
    end
end

function OnLevelUpdate(dt)
    if Replay.IsPlaying() and Replay.GetTime() >= Replay.GetDuration() then
        Replay.Stop()
        ShowDeathScreen()
    end
end
```

### Notes & best practices

- The **sampling rate** controls only how often frames are recorded; playback can still run at any FPS thanks to interpolation. `30 Hz` is a great default for long matches; `60 Hz` for fast-paced action / killcams.
- Recording **only stamps state** — it does **not** re-run game logic. During playback your scripts (`OnUpdate`, AI, physics) keep running normally; the replay overrides transforms/velocities **on top** of them. Disable scripts during playback if you want a pure spectator view.
- An entity is matched between recording and playback by its **UUID**, not its entt-handle. Entities deleted during the recording will simply stop being driven once their last frame has passed.
- `Replay.Save/Load` paths are sandboxed inside `Saves/Replays/`. Do not use absolute paths or `..`.
- The **active replay** persists across `Stop()` and `Pause()` — only `Clear()` / `Reset()` / loading another file will discard it.
- The killcam buffer keeps copies of every sampled frame, so memory use is `~ rate * seconds * tracked_entities`. Use `TrackAll(false)` + a whitelist on huge open-world scenes.

---

## 59. Matchmaking — Player Matchmaking

The `Matchmaking` module provides a deterministic, in-engine **skill-based matchmaker** that runs entirely on the Lua side. It is independent of the `Network` module: you feed it a candidate pool (real players from any backend, peers from `Network`, or local bots), define the match shape, and call `StartSearch`. The matchmaker expands its skill window over time, fires progress / found / failed events, and returns a balanced match team.

**Highlights**

- **Skill anchored on Self**: every search is centered on `Matchmaking.SetSelf{...}`'s skill; closest-skill candidates are picked first.
- **Expanding skill window**: starts at `skillRange` and grows by `expandRate` per second up to `maxSkillRange` until enough candidates qualify.
- **Region & attribute filters**: `region` + `requiredAttributes` (string key/value map) for game-mode, language, party-id, etc.
- **Bot fillers**: `Matchmaking.AddBots(N, { minSkill, maxSkill })` to fill empty slots with synthetic candidates.
- **Event-driven**: `OnSearchStarted`, `OnSearchProgress`, `OnMatchFound`, `OnSearchCancelled`, `OnSearchFailed`, `OnSearchTimeout`, `OnPlayerAdded`, `OnPlayerRemoved`.
- **Lifecycle**: `idle → searching → found | cancelled | failed`. State is queryable any time via `GetState()`.
- **Ticked automatically** every script update tick; no need to call any update yourself.

> Matchmaking does **not** open sockets or talk to a remote service — it is a local matcher. Combine it with `Network.*` (lobby creation / direct connect) once `OnMatchFound` fires.

### Player descriptor

Every entry in the candidate pool — including `Self` — is a Lua table with these fields:

```lua
{
    id         = "p_42",        -- unique string id (required)
    name       = "Alice",       -- optional display name (defaults to id)
    skill      = 1500,          -- numeric MMR / ELO / rating (default 1000)
    region     = "eu",          -- optional region tag
    attributes = {              -- optional string key/value map
        mode  = "ranked",
        party = "abc123",
    },
}
```

### Quick start

```lua
Matchmaking.SetSelf{ id = "me", name = "You", skill = 1500, region = "eu" }
Matchmaking.AddBots(20, { minSkill = 1200, maxSkill = 1800, region = "eu" })

Matchmaking.OnMatchFound(function(match)
    print("Match found:", match.id, "avg skill", match.averageSkill)
    for _, p in ipairs(match.players) do
        print("  ", p.name, p.skill)
    end
end)

local ticket = Matchmaking.StartSearch{
    mode             = "ranked",
    teamSize         = 4,
    skillRange       = 50,
    maxSkillRange    = 500,
    expandRate       = 25,    -- skill range grows by 25 per second
    region           = "eu",
    requireRegion    = true,
    timeout          = 30,
    progressInterval = 0.5,
}
```

### Self / candidate pool

| Function | Description |
|---|---|
| `Matchmaking.SetSelf(player)` | Set the local player descriptor. The search is anchored on this player's `skill`. |
| `Matchmaking.GetSelf()` | Returns the local player table or `nil`. |
| `Matchmaking.HasSelf()` | `true` if `SetSelf` has been called. |
| `Matchmaking.ClearSelf()` | Drop the local player. |
| `Matchmaking.AddPlayer(player)` → `bool` | Add / replace a single candidate. Returns `false` if `id` is empty. |
| `Matchmaking.AddPlayers(arrayOfPlayers)` → `int` | Bulk add. Returns the number of **newly inserted** candidates. |
| `Matchmaking.AddBots(count, options?)` → `int` | Insert `count` synthetic candidates. `options = { minSkill=800, maxSkill=1200, region="", prefix="bot_" }`. Skills are spread linearly between `minSkill` and `maxSkill`. |
| `Matchmaking.RemovePlayer(id)` → `bool` | Remove by id. Fires `OnPlayerRemoved`. |
| `Matchmaking.HasPlayer(id)` → `bool` | Lookup. |
| `Matchmaking.GetPlayer(id)` → `table\|nil` | Fetch a candidate. |
| `Matchmaking.GetPlayers()` → `array` | Snapshot of all candidates. |
| `Matchmaking.GetPlayerCount()` → `int` | Pool size. |
| `Matchmaking.ClearPlayers()` | Empty the pool (does not affect `Self`). |

### Search lifecycle

| Function | Description |
|---|---|
| `Matchmaking.StartSearch(options?)` → `string` | Begin a search and return a **ticket id**. Stops any ongoing search. Fires `OnSearchStarted`. |
| `Matchmaking.CancelSearch()` → `bool` | Cancel the active search. Fires `OnSearchCancelled`. Returns `true` if a search was actually running. |
| `Matchmaking.Fail(reason?)` → `bool` | Manually fail the active search (e.g. backend rejected). Fires `OnSearchFailed`. |
| `Matchmaking.IsSearching()` → `bool` | Convenience for `GetState() == "searching"`. |
| `Matchmaking.GetState()` → `string` | One of `"idle"`, `"searching"`, `"found"`, `"cancelled"`, `"failed"`. |
| `Matchmaking.GetTicketId()` → `string` | Ticket id of the current/last search. |
| `Matchmaking.GetElapsed()` → `number` | Seconds since `StartSearch`. |
| `Matchmaking.GetCurrentSkillRange()` → `number` | The currently-applied skill window (grows over time). |
| `Matchmaking.GetCandidateCount()` → `int` | Number of candidates that *currently* satisfy all filters. |
| `Matchmaking.GetMatch()` → `table\|nil` | The last successful match (see schema below). |
| `Matchmaking.GetLastFailReason()` → `string` | `"timeout"`, `"manual"` or whatever was passed to `Fail`. |
| `Matchmaking.GetOptions()` → `table` | Snapshot of the active search options. |
| `Matchmaking.SetExpandRate(rate)` | Adjust the skill-range growth rate while searching. |
| `Matchmaking.SetMaxSkillRange(range)` | Adjust the maximum skill range while searching. |
| `Matchmaking.Reset()` | Wipe everything: pool, self, callbacks, search state. |
| `Matchmaking.ClearCallbacks()` | Drop all registered callbacks (keeps the pool). |

### `StartSearch` options

```lua
Matchmaking.StartSearch{
    mode               = "quick",         -- free-form label, copied into the match
    teamSize           = 2,               -- minimum number of players in the result (alias: minPlayers)
    skillRange         = 100,             -- initial ± skill window around Self
    maxSkillRange      = 5000,            -- upper cap of the growing window
    expandRate         = 50,              -- skill-window growth per second (0 = no expansion)
    region             = "",              -- optional region filter
    requireRegion      = false,           -- if true, candidates must match `region` exactly
    requiredAttributes = { mode = "ranked", lang = "en" },
    timeout            = 0,               -- 0 = no timeout (seconds)
    progressInterval   = 0.5,             -- seconds between OnSearchProgress fires (0 = disabled)
    includeSelf        = true,            -- include Self as one of the picked players
}
```

A candidate qualifies when **all** of these hold:

1. `|candidate.skill - self.skill| <= currentSkillRange`
2. If `requireRegion = true` and `region ~= ""`: `candidate.region == region`
3. Every key/value in `requiredAttributes` is present and equal in `candidate.attributes`

### Match result schema

```lua
match = {
    id           = "match_3",
    mode         = "ranked",
    teamSize     = 4,
    averageSkill = 1487.5,
    finalRange   = 175.0,        -- the skill window that finally produced the match
    elapsed      = 4.3,           -- seconds spent searching
    players      = {              -- length == teamSize, sorted by closeness to Self.skill
        { id = "me",   name = "You",   skill = 1500, region = "eu", attributes = {...} },
        { id = "p_7",  name = "Bob",   skill = 1480, region = "eu", attributes = {...} },
        { id = "p_15", name = "Cara",  skill = 1525, region = "eu", attributes = {...} },
        { id = "p_22", name = "Dale",  skill = 1445, region = "eu", attributes = {...} },
    },
}
```

### Callbacks

All callbacks accept a `function`. Re-registering replaces the previous one. They are invoked on the Lua thread during the regular runtime tick, so it is safe to touch any other Lua API from inside them.

| Callback | Signature | When |
|---|---|---|
| `Matchmaking.OnSearchStarted(fn)` | `fn(ticketId)` | Right after `StartSearch`. |
| `Matchmaking.OnSearchProgress(fn)` | `fn(elapsed, candidateCount, currentSkillRange)` | Every `progressInterval` seconds while searching. |
| `Matchmaking.OnMatchFound(fn)` | `fn(match)` | When enough qualified candidates were found. |
| `Matchmaking.OnSearchCancelled(fn)` | `fn(ticketId)` | After `CancelSearch`. |
| `Matchmaking.OnSearchFailed(fn)` | `fn(ticketId, reason)` | After `Fail(reason)` or on `timeout`. |
| `Matchmaking.OnSearchTimeout(fn)` | `fn(ticketId)` | When `timeout > 0` and the search ran out. Always followed by `OnSearchFailed(_, "timeout")`. |
| `Matchmaking.OnPlayerAdded(fn)` | `fn(id)` | A new candidate entered the pool. |
| `Matchmaking.OnPlayerRemoved(fn)` | `fn(id)` | A candidate was removed. |

### Full example: ranked match with bot filler and timeout

```lua
function StartRankedSearch()
    Matchmaking.SetSelf{ id = "me", name = "You", skill = GetMyMMR(), region = "eu" }
    Matchmaking.ClearPlayers()

    -- pull live candidates from your backend / Network module
    for _, p in ipairs(Backend.FetchOnlinePlayers()) do
        Matchmaking.AddPlayer(p)
    end

    -- top up with bots so search always succeeds in QA / offline mode
    Matchmaking.AddBots(8, { minSkill = GetMyMMR() - 200, maxSkill = GetMyMMR() + 200, region = "eu" })

    Matchmaking.OnSearchProgress(function(t, candidates, range)
        UpdateUI(string.format("Searching... %.1fs, %d candidates, ±%.0f MMR", t, candidates, range))
    end)

    Matchmaking.OnMatchFound(function(m)
        UpdateUI(string.format("Match found! avg %.0f MMR", m.averageSkill))
        -- Matchmaking only picks the players; hosting is still yours to drive.
        -- Convention here: the host creates a room named after the match id.
        Network.JoinRoom(m.id)
    end)

    Matchmaking.OnSearchTimeout(function()
        UpdateUI("No match found, please try again")
    end)

    Matchmaking.StartSearch{
        mode             = "ranked",
        teamSize         = 4,
        skillRange       = 50,
        maxSkillRange    = 500,
        expandRate       = 25,
        region           = "eu",
        requireRegion    = true,
        timeout          = 30,
        progressInterval = 0.25,
    }
end

function CancelRankedSearch()
    Matchmaking.CancelSearch()
end
```

### Notes & best practices

- The matcher is **deterministic** with respect to its inputs: same pool + same options → same match. Useful for tests.
- Use `requiredAttributes` to keep parties together (`party = "<id>"`), to enforce a game mode (`mode = "ranked"`), or to keep languages aligned (`lang = "en"`).
- `expandRate` and `maxSkillRange` are the two main quality-vs-speed knobs. Small expand rate → tighter skill match, longer waits. Large `maxSkillRange` → guaranteed match eventually, looser skill quality.
- `Matchmaking.AddBots` is ideal during development and for "always-on" PvE / co-op modes that want a few warm bodies.
- The pool is **not** automatically synchronised with `Network` peers — feed it explicitly from your network or backend layer.
- After `OnMatchFound`, the state stays at `"found"`. Call `Matchmaking.Reset()` (or `StartSearch` again) to start a new round.
- All callbacks survive `CancelSearch` and `Fail`; only `ClearCallbacks` / `Reset` remove them.

---

## 60. Console — Developer Console & Command System

The **Developer Console** is a drop-down in-game overlay for debugging, cheat codes, and runtime configuration. The **Command System** provides a framework for registering console commands and variables (CVars) from both C++ and Lua.

### 60.1 Console — Developer Console Control

| Function | Returns | Description |
|----------|---------|-------------|
| `Console.Show()` | — | Open the developer console overlay |
| `Console.Hide()` | — | Close the developer console overlay |
| `Console.Toggle()` | — | Toggle the console open/closed |
| `Console.IsOpen()` | `bool` | Returns whether the console is currently open |
| `Console.SetEnabled(enabled)` | — | Enable or disable the console entirely |
| `Console.IsEnabled()` | `bool` | Returns whether the console is enabled |
| `Console.SetToggleKey(scancode)` | — | Set the SDL scancode used to toggle the console (default: grave accent `` ` ``) |
| `Console.GetToggleKey()` | `int` | Get the current toggle key scancode |
| `Console.SetStyle(style)` | — | Customize console appearance |

```lua
Console.Show()
Console.Toggle()
print("Console open:", Console.IsOpen())

Console.SetStyle({
    backgroundAlpha = 0.9,
    fontScale = 0.5,
    lineHeight = 16.0,
    padding = 8.0,
    heightFraction = 0.5,
    animationSpeed = 10.0,
    textColor = {0.9, 0.9, 0.9},
    backgroundColor = {0.05, 0.05, 0.1},
    warningColor = {1.0, 0.8, 0.2},
    errorColor = {1.0, 0.3, 0.3},
    successColor = {0.3, 1.0, 0.3},
    commandColor = {0.5, 0.8, 1.0},
    inputBgColor = {0.08, 0.08, 0.12},
    cursorColor = {0.9, 0.9, 0.9},
    completionBgColor = {0.1, 0.1, 0.15},
    completionSelectedColor = {0.2, 0.3, 0.5}
})
```

### 60.2 Console — Command Registration

| Function | Returns | Description |
|----------|---------|-------------|
| `Console.RegisterCommand(name, callback, description?, category?, usage?, opts?)` | — | Register a new console command |
| `Console.UnregisterCommand(name)` | — | Remove a registered command |
| `Console.HasCommand(name)` | `bool` | Check if a command exists |
| `Console.GetAllCommands()` | `table` | Get all registered commands |

```lua
Console.RegisterCommand("godmode", function(args)
    local enabled = not isGodMode
    isGodMode = enabled
    Console.Print("God mode " .. (enabled and "ON" or "OFF"), 3)
end, "Toggle god mode", "Cheats", "godmode")

Console.RegisterCommand("spawn", function(args)
    if #args < 1 then
        Console.Print("Usage: spawn <class> [x] [y]", 1)
        return
    end
    local class = args[1]
    local x = tonumber(args[2]) or 0
    local y = tonumber(args[3]) or 0
    SpawnEntity(class, x, y, 0)
    Console.Print("Spawned " .. class, 3)
end, "Spawn an entity", "Debug", "spawn <class> [x] [y]")
```

**Command options table** (optional 6th argument):

| Field | Type | Description |
|-------|------|-------------|
| `cheat` | `bool` | Requires cheats enabled to run |
| `hidden` | `bool` | Hide from help/list/autocomplete |
| `devOnly` | `bool` | Only available in developer mode |
| `autocomplete` | `function` | `function(partial) -> table` returning candidate strings for Tab completion |

```lua
Console.RegisterCommand("warp", function(args)
    if #args >= 1 then LoadLevel(args[1]) end
end, "Warp to a level", "Debug", "warp <level>", {
    devOnly = true,
    autocomplete = function(partial)
        return { "level1", "level2", "boss_arena" }
    end
})
```

### 60.3 Console — CVar (Console Variables)

| Function | Returns | Description |
|----------|---------|-------------|
| `Console.RegisterCVar(name, defaultValue, description?, category?, opts?)` | — | Register a console variable |
| `Console.UnregisterCVar(name)` | — | Remove a CVar |
| `Console.HasCVar(name)` | `bool` | Check if a CVar exists |
| `Console.GetCVar(name)` | `any` | Get CVar value (auto-typed) |
| `Console.SetCVar(name, value)` | — | Set CVar value (auto-typed) |
| `Console.GetCVarBool(name)` | `bool` | Get CVar as boolean |
| `Console.GetCVarInt(name)` | `int` | Get CVar as integer |
| `Console.GetCVarFloat(name)` | `float` | Get CVar as float |
| `Console.GetCVarString(name)` | `string` | Get CVar as string |
| `Console.SetCVarBool(name, value)` | — | Set CVar as boolean |
| `Console.SetCVarInt(name, value)` | — | Set CVar as integer |
| `Console.SetCVarFloat(name, value)` | — | Set CVar as float |
| `Console.SetCVarString(name, value)` | — | Set CVar as string |
| `Console.GetAllCVars()` | `table` | Get all registered CVars |

**CVar options table:**

| Field | Type | Description |
|-------|------|-------------|
| `readOnly` | `bool` | Prevent modification from console |
| `hidden` | `bool` | Hide from help/list commands |
| `archive` | `bool` | Save to config file |
| `cheat` | `bool` | Requires cheats enabled |
| `devOnly` | `bool` | Only available in developer mode (hidden/blocked otherwise) |
| `noLog` | `bool` | Suppress the value-change echo in the console |
| `min` | `number` | Minimum value (int/float only) |
| `max` | `number` | Maximum value (int/float only) |
| `onChange` | `function` | Callback when value changes |

```lua
Console.RegisterCVar("player_speed", 200.0, "Player movement speed", "Gameplay", {
    min = 10.0,
    max = 1000.0,
    archive = true,
    onChange = function(oldVal, newVal)
        Console.Print("Speed changed to " .. tostring(newVal))
    end
})

Console.RegisterCVar("god_mode", false, "Enable god mode", "Cheats", { cheat = true })
Console.RegisterCVar("game_version", "1.0.0", "Game version", "Info", { readOnly = true })

local speed = Console.GetCVarFloat("player_speed")
Console.SetCVarFloat("player_speed", speed * 1.5)
```

### 60.4 Console — Execution and Logging

| Function | Returns | Description |
|----------|---------|-------------|
| `Console.Execute(commandLine)` | — | Execute a console command string |
| `Console.Print(message, level?)` | — | Print a message to the console |
| `Console.Clear()` | — | Clear the console log |
| `Console.GetLog()` | `table` | Get all log entries |
| `Console.GetInputHistory()` | `table` | Get command input history |

**Log levels:** `0` = Info, `1` = Warning, `2` = Error, `3` = Success

```lua
Console.Execute("god_mode true")
Console.Execute("player_speed 300")

Console.Print("Hello from Lua!", 0)
Console.Print("Something is wrong!", 1)
Console.Print("Critical error!", 2)
Console.Print("Operation successful!", 3)
```

### 60.5 Console — Cheats and Aliases

| Function | Returns | Description |
|----------|---------|-------------|
| `Console.SetCheatsEnabled(enabled)` | — | Enable/disable cheat commands |
| `Console.AreCheatsEnabled()` | `bool` | Check if cheats are enabled |
| `Console.SetDeveloperMode(enabled)` | — | Enable/disable developer-only commands and CVars |
| `Console.IsDeveloperMode()` | `bool` | Check if developer mode is enabled (defaults on in debug builds, off in release) |
| `Console.RegisterAlias(alias, target)` | — | Create a command alias |
| `Console.GetCategories()` | `table` | Get all command/CVar categories |

```lua
Console.SetCheatsEnabled(true)
Console.RegisterAlias("gm", "godmode")
Console.RegisterAlias("sp", "spawn")
```

### 60.6 Console — Persistence

| Function | Returns | Description |
|----------|---------|-------------|
| `Console.ArchiveCVars(filePath)` | — | Save all archived CVars to a JSON file |
| `Console.LoadArchivedCVars(filePath)` | — | Load archived CVars from a JSON file |

```lua
function OnGameStart()
    Console.LoadArchivedCVars("Config/Console.cfg")
end

function OnGameExit()
    Console.ArchiveCVars("Config/Console.cfg")
end
```

> **Paths are resolved relative to `Content/` and sandboxed.** `exec`, `ArchiveCVars`, and `LoadArchivedCVars` reject absolute paths and `..` traversal; a name like `"Config/Console.cfg"` resolves under `Content/`.

### 60.7 Built-in Commands

| Command | Usage | Description |
|---------|-------|-------------|
| `help` | `help [name]` | List all commands or show help for a specific one |
| `clear` | `clear` | Clear the console log |
| `echo` | `echo <message>` | Print a message |
| `find` | `find <pattern>` | Search commands and CVars by name |
| `exec` | `exec <file>` | Execute commands from a config file |
| `alias` | `alias <name> <command>` | Create a command alias |
| `cheats` | `cheats <on\|off>` | Enable/disable cheat commands |
| `developer` | `developer <on\|off>` | Enable/disable developer-only commands and CVars |
| `cvarlist` | `cvarlist [category]` | List all CVars |
| `cmdlist` | `cmdlist [category]` | List all commands |
| `reset` | `reset <cvar>` | Reset a CVar to its default value |
| `toggle` | `toggle <cvar>` | Toggle a boolean CVar |

### 60.8 Console Keyboard Shortcuts

| Key | Action |
|-----|--------|
| `` ` `` (grave) | Toggle console |
| `Enter` | Execute command |
| `Tab` | Autocomplete |
| `Up/Down` | Navigate command history |
| `PageUp/PageDown` | Scroll log |
| `Mouse wheel` | Scroll log |
| `Escape` | Close completions / close console |
| `Ctrl+A` | Select all text |
| `Ctrl+C` | Copy selected text |
| `Ctrl+V` | Paste from clipboard |
| `Ctrl+X` | Cut selected text |

### 60.9 Complete Example

```lua
local debugEnabled = false

function OnConstruct()
    Console.RegisterCVar("debug_overlay", false, "Show debug overlay", "Debug", {
        archive = true,
        onChange = function(old, new)
            debugEnabled = new
        end
    })

    Console.RegisterCVar("spawn_count", 10, "Number of entities to spawn", "Debug", {
        min = 1, max = 100
    })

    Console.RegisterCommand("spawn_wave", function(args)
        local count = Console.GetCVarInt("spawn_count")
        for i = 1, count do
            local x = math.random(-200, 200)
            local y = math.random(-200, 200)
            SpawnEntity("Enemy", x, y, 0)
        end
        Console.Print("Spawned " .. count .. " enemies", 3)
    end, "Spawn a wave of enemies", "Debug", "spawn_wave")

    Console.RegisterCommand("fps_max", function(args)
        if #args < 1 then
            Console.Print("Usage: fps_max <value>", 1)
            return
        end
        Console.Print("FPS limit set to " .. args[1], 3)
    end, "Set FPS limit", "Graphics")

    Console.LoadArchivedCVars("Config/Console.cfg")
end

function OnDestroy()
    Console.ArchiveCVars("Config/Console.cfg")
end
```

---

## 61. Draw — Immediate-Mode Rendering (Draw / Texture / RenderTarget)

> `Draw` submits textured quads and arbitrary triangle meshes straight into the batch renderer, without creating entities.
> Everything you submit during a frame is drawn once and discarded — call it again every frame from `OnUpdate`.
>
> This is the fastest way to push large amounts of generated geometry: **one Lua call can submit thousands of quads**, whereas
> driving the same number of sprite entities would cost several Lua-to-C++ calls each.
>
> Typical uses: procedural graphics, custom particles, in-game level editors, minimaps, debug overlays, tile/column renderers,
> and pseudo-3D techniques (raycasting, projected ground planes, perspective roads) that stay entirely 2D.

### Coordinate conventions

The `Draw` API follows the engine conventions exactly:

- **X+** is right, **X-** is left.
- **Y+** is up, **Y-** is down.
- **Rotation** is in degrees; **positive is clockwise**, negative is counter-clockwise (same as `SetSpriteLocalRotation`).
- **Z+** is toward the viewer (foreground), **Z-** is away (background) — same as `SetSpriteOrder`.
  World-space draws are depth-tested against sprites, tilemaps and everything else in the scene, so Z gives you
  correct per-pixel occlusion for free.
- **Pivot** `px, py` is normalized `0..1`, and **`py` is measured from the top** — identical to `SetSpritePivot`.
  The default pivot is `0.5, 0.5` (centre), so `x, y` is the centre of the quad unless you change it.
- **UV** `u, v, uw, vh` is normalized `0..1` in exactly the same space as `SetSpriteRegion` divided by the texture size:
  `v = 0` is the **top** of the image and the image lands upright, identical to a sprite.
  Passing `sx, sy, sw, sh` instead lets you specify the source rectangle **in pixels**.
  `Draw.Mesh` is different on purpose — there each vertex carries a raw sampling coordinate, so `v = 0` samples
  the top row of the image and you decide which vertex it belongs to.

### Spaces

```lua
Draw.SetSpace("world")   -- default: world units, depth-tested, moves with the camera
Draw.SetSpace("screen")  -- screen pixels, origin at the BOTTOM-LEFT, no depth test, painter order
local space = Draw.GetSpace()
```

`world` geometry is rendered together with the scene (after particles, before fog of war and widgets), so it is affected by
post-processing and participates in the depth buffer.
`screen` geometry ignores the camera and is drawn in submission order.

**Depth in screen space.** By default `world` geometry is depth-tested and `screen` geometry is not — that is what
`Draw.SetDepthTest("auto")` means. `Draw.SetDepthTest(true)` turns the depth buffer on for screen-space geometry too,
which is exactly what a raycaster or a sector-based pseudo-3D renderer wants: give every column, floor span and sprite
its real distance in `z` and the GPU sorts them per pixel, with no manual painter ordering and no software z-buffer.
`Draw.SetDepthTest(false)` forces it off even in world space (useful for an overlay that must never be occluded).
The setting is part of the draw state, so `Draw.Push` / `Draw.Pop` / `Draw.Reset` save and restore it.

> Screen space uses its own depth range (`-100000 .. 100000`), which is not the same scale as the camera's. Enable
> screen-space depth for a self-contained pseudo-3D view where **every** primitive goes through `Draw`; do not rely on it
> to sort script geometry against ordinary scene sprites.

### Draw state

The draw state is global and persists between calls, so you set it once and submit many primitives.

```lua
Draw.SetTexture("Content/Textures/wall.png")  -- default texture for later calls; nil = white
Draw.SetColor(1, 1, 1, 1)                     -- default tint
Draw.SetZ(0)                                  -- default depth
Draw.SetPivot(0.5, 0.5)                       -- default pivot (py measured from the top)
Draw.SetBlend("masked")                       -- "masked" | "additive" | "translucent" | "opaque"
Draw.SetShading("unlit")                      -- "unlit" (default) | "lit"
Draw.SetAlphaClip(0.5)                        -- alpha cutout threshold used by "masked"
Draw.SetTarget(nil)                           -- nil = screen, or a RenderTarget name
Draw.SetDepthTest("auto")                     -- "auto" (default) | true / "on" | false / "off"
Draw.SetMaterial(nil)                         -- nil = none, or a material asset / dynamic instance name

Draw.GetTexture(); Draw.GetColor(); Draw.GetZ(); Draw.GetPivot()
Draw.GetBlend(); Draw.GetShading(); Draw.GetAlphaClip(); Draw.GetTarget()
Draw.GetDepthTest(); Draw.GetMaterial()

Draw.Push()   -- save the whole draw state (max depth 64)
Draw.Pop()    -- restore it
Draw.Reset()  -- back to defaults and clear the state stack
```

### Materials on immediate-mode geometry

`Draw.SetMaterial` runs a node-based material — a real fragment shader — over everything you submit afterwards. This is
what turns `Draw` from a textured-quad pusher into a programmable surface: distance fog computed per pixel, perspective
correction, water, heat shimmer, palette swaps, scanlines on one specific mesh rather than the whole screen.

```lua
-- A material asset, or a dynamic instance created with Material.CreateDynamic:
Draw.SetMaterial("Content/Materials/Fog.ice_mat")

local dyn = Material.CreateDynamic("Content/Materials/Fog.ice_mat", "GroundFog")
Material.SetScalar("GroundFog", "Density", 0.4)
Draw.SetMaterial("GroundFog")     -- parameters stay live: set them any frame

Draw.Mesh(...)                    -- drawn through the material
Draw.SetMaterial(nil)             -- back to the built-in shader
Draw.ClearMaterial()              -- same thing
```

Returns `false` and logs a warning if the name matches neither a loaded dynamic instance nor a material asset, and the
state is left cleared, so a typo degrades to the default shader instead of drawing nothing.

> **Cost.** A material draw sets up its own shader and flushes immediately — it does not batch with the surrounding
> geometry. One material over one large mesh is free; one material over ten thousand separate quads is ten thousand draw
> calls. Group geometry that shares a material, submit it as a mesh where you can, and keep `Draw.SetMaterial(nil)` on
> for the bulk quads.

### Text

`Draw.Text` puts a string through the engine's own font stack — the same glyph atlas, shaping and bidirectional layout
the widget system uses, so Arabic, Hebrew, Japanese and Cyrillic all render correctly. It needs no widget asset and no
entity, which makes it the right tool for HUDs, damage numbers, debug overlays and text-grid games.

```lua
Draw.Text("Score: 1200", x, y, 24)              -- text, position, pixel size
Draw.Text("HP", x, y, 18, {
    r = 1, g = 0.3, b = 0.3, a = 1,             -- colour (defaults to the draw-state colour)
    align = "center",                            -- "left" (default) | "center" | "right"
    font = "Content/Fonts/Pixel.ttf",            -- default engine font when omitted
    z = 10,
    rtl = false,                                 -- force right-to-left layout
    lit = false,                                 -- take scene lighting
})

-- Measure before you place. Returns width, height and the font's line height,
-- all already scaled to the pixel size you pass in.
local m = Draw.MeasureText("Score: 1200", 24)
Draw.Text("Score: 1200", (Draw.GetViewportSize().width - m.width) * 0.5, y, 24)

Draw.GetTextCount()          -- strings submitted this frame
Draw.SetMaxTexts(20000)      -- per-frame budget (default 20000)
Draw.GetMaxTexts()
```

- `x, y` is the **baseline** of the first line, in the current space (`Draw.SetSpace`).
- In `world` space the text rolls and scales with the camera; in `screen` space it is fixed to the viewport.
- Text is drawn in submission order and is **not** depth-tested, so it always lands on top of the geometry submitted
  before it in the same space.
- One call per line: split on `\n` yourself, using `MeasureText(...).lineHeight` for the step.

```lua
-- A text-grid renderer: one call per row, whole screen in 50 calls.
local COLS, ROWS, CELL = 80, 50, 16
function OnUpdate(dt)
    Draw.SetSpace("screen")
    local h = Draw.GetViewportSize().height
    for row = 0, ROWS - 1 do
        Draw.Text(RowString(row), 0, h - (row + 1) * CELL, CELL, { font = GRID_FONT })
    end
end
```

> For tens of thousands of individually coloured glyphs, a `Draw.QuadsPacked` call over a font-atlas texture is still
> faster — one Lua call for the whole screen. `Draw.Text` is the convenient path; the packed-quad path is the extreme one.

### Drawing primitives

```lua
-- One quad, fully specified. Any field may be omitted.
Draw.Quad{
    x = 100, y = 200,      -- pivot point (centre by default)
    w = 64,  h = 64,       -- size in world/screen units
    z = 0,                 -- depth (Z+ = front)
    rot = 0,               -- degrees, clockwise positive
    px = 0.5, py = 0.5,    -- pivot, py from the top
    u = 0, v = 0, uw = 1, vh = 1,       -- normalized UV rect
    -- or a pixel source rect instead of u/v/uw/vh:
    -- sx = 0, sy = 0, sw = 16, sh = 16,
    r = 1, g = 1, b = 1, a = 1,         -- tint
    texture = "Content/Textures/x.png", -- per-quad texture override
}

-- Solid untextured rectangle. x, y is the BOTTOM-LEFT corner.
Draw.Rect(x, y, w, h, r, g, b, a, z)

-- Textured sprite; w/h default to the texture size, pivot comes from the draw state.
Draw.Sprite("Content/Textures/hero.png", x, y, w, h, rotation, z)

-- Textured quad using a pixel source rectangle from the texture.
Draw.Region("Content/Textures/atlas.png", x, y, w, h, sx, sy, sw, sh, rotation, z)

-- Line drawn as a rotated quad.
Draw.Line(x1, y1, x2, y2, thickness, r, g, b, a, z)
```

### Bulk submission

```lua
-- Array of quad tables. Returns how many were submitted.
local n = Draw.Quads("Content/Textures/wall.png", {
    { x = 0, y = 0, w = 1, h = 100 },
    { x = 1, y = 0, w = 1, h = 120 },
})

-- Same, using the texture from Draw.SetTexture:
Draw.Quads(quadList)
```

`Draw.QuadsPacked` is the fastest path: a **flat array of numbers**, 14 per quad, in this exact order:

```
x, y, w, h, z, rot, u, v, uw, vh, r, g, b, a
```

```lua
local data = {}
local i = 0
for col = 0, 319 do
    local h = ColumnHeight(col)
    data[i+1]  = col        -- x
    data[i+2]  = 180        -- y (centre)
    data[i+3]  = 1          -- w
    data[i+4]  = h          -- h
    data[i+5]  = 0          -- z
    data[i+6]  = 0          -- rot
    data[i+7]  = TexU(col)  -- u
    data[i+8]  = 0          -- v
    data[i+9]  = 1 / 64     -- uw (one texel column of a 64 px texture)
    data[i+10] = 1          -- vh
    data[i+11] = 1; data[i+12] = 1; data[i+13] = 1; data[i+14] = 1  -- rgba
    i = i + 14
end
Draw.QuadsPacked("Content/Textures/wall.png", data)   -- 320 columns, one Lua call
```

An optional third argument limits how many quads are read: `Draw.QuadsPacked(path, data, count)`.

### Meshes

`Draw.Mesh` submits an arbitrary triangle mesh with per-vertex UVs — this is what lets you build perspective trapezoids,
projected ground planes, warped roads and free-form deformations without leaving 2D.

```lua
Draw.Mesh(texturePath, positions, uvs, indices, options)
```

- `positions` — flat array `{x1, y1, x2, y2, ...}` (at least 3 vertices, at most 65535).
- `uvs` — flat array `{u1, v1, u2, v2, ...}` with the same vertex count. Optional; omitted means all zero.
- `indices` — flat array of **1-based** vertex indices, a multiple of 3. Optional; omitted builds a triangle fan.
- `options` — optional table: `{ r, g, b, a, z, blend, shading, alphaClip, colors, zs }`.

#### Per-vertex colour and depth

`options.colors` and `options.zs` turn a flat mesh into a properly shaded, properly depth-sorted one. This is what makes
projected ground planes, distance fog, per-distance light falloff and Gouraud shading possible without splitting the
surface into hundreds of separate draws.

- `colors` — flat array `{r1, g1, b1, a1, r2, ...}`, **4 numbers per vertex**, `0..1`. Multiplied by the mesh tint
  (`options.r/g/b/a`, or the `Draw.SetColor` state), so leave the tint at white to use the vertex colours as-is.
- `zs` — flat array `{z1, z2, ...}`, **1 number per vertex**. Overrides `options.z` per vertex, so a single triangle can
  span near and far and still be occluded correctly per pixel.

Either array is ignored (with a log warning) if it is shorter than the vertex count requires.

```lua
-- A floor span that fades to black with distance and carries real per-vertex depth.
Draw.Mesh("Content/Textures/floor.png",
    { -160, 0,  160, 0,  90, 70,  -90, 70 },       -- positions
    {    0, 1,    1, 1,   1, 0,     0, 0 },        -- uvs
    { 1, 2, 3,  1, 3, 4 },                         -- indices
    {
        colors = {                                  -- near = bright, far = dark
            1, 1, 1, 1,   1, 1, 1, 1,
            0.25, 0.25, 0.3, 1,   0.25, 0.25, 0.3, 1,
        },
        zs = { -2, -2, -40, -40 },                  -- near vertices in front, far ones behind
    })
```

```lua
-- A textured trapezoid: wide at the bottom, narrow at the top (a road segment).
Draw.Mesh("Content/Textures/road.png",
    { -100, 0,  100, 0,  40, 60,  -40, 60 },   -- positions
    {    0, 1,    1, 1,   1, 0,     0, 0 },    -- uvs
    { 1, 2, 3,  1, 3, 4 },                     -- indices (1-based)
    { z = -10 })
```

Consecutive meshes that share the same texture and state are batched into a single draw call.

### Budgets and statistics

```lua
Draw.GetViewportSize()     -- { width, height } of the surface Draw renders into,
                           -- in the same units screen space uses. Use this for HUD
                           -- layout rather than the window resolution.
Draw.GetQuadCount()        -- quads submitted this frame
Draw.GetCommandCount()     -- batched commands this frame
Draw.GetMeshVertexCount()  -- mesh vertices submitted this frame
Draw.DidOverflow()         -- true if a budget was exceeded and geometry was dropped

Draw.SetMaxQuads(200000)        -- per-frame quad budget (default 200000)
Draw.SetMaxMeshVertices(400000) -- per-frame mesh vertex budget (default 400000)
Draw.GetMaxQuads(); Draw.GetMaxMeshVertices()

Draw.Clear()  -- drop everything submitted so far this frame
```

The list is cleared automatically at the start of every frame, and whenever the scene is torn down.

### Texture — script-created textures

Textures created here are registered under their name, so **every API that takes a texture path accepts the name**:
`SetSpriteTexture`, `Material.SetTexture`, `PP.SetCustomMaterialTexture`, `Draw.SetTexture`, widgets, and so on.

```lua
Texture.Create("minimap", 256, 256, {
    r = 0, g = 0, b = 0, a = 0,   -- initial fill colour
    filter = "nearest",           -- "nearest" (default) | "linear"
    wrap = "clamp",               -- "clamp" (default) | "repeat"
})

Texture.Exists("minimap")
Texture.GetSize("minimap")            -- returns { width, height }
Texture.Destroy("minimap")
Texture.GetCount()

Texture.Fill("minimap", 0, 0, 0, 1)                    -- fill the whole texture
Texture.SetPixel("minimap", x, y, r, g, b, a)          -- one pixel, components 0..1
Texture.SetPixels("minimap", x, y, w, h, floats)       -- RGBA floats 0..1, w*h*4 values
Texture.SetPixelBytes("minimap", x, y, w, h, source)   -- table | binary string | PixelBuffer
Texture.SetPixelBuffer("minimap", x, y, buffer)        -- upload a whole PixelBuffer at (x, y)

Texture.SetFilter("minimap", "linear")
Texture.SetWrap("minimap", "repeat")
Texture.GenerateMipmaps("minimap")
```

`SetPixels` and `SetPixelBytes` expect the region row by row, 4 components per pixel, starting at `x, y`.
This is the classic software-renderer path: build a pixel buffer in Lua, upload it once, draw it as a single quad.

`SetPixelBytes` accepts **three** kinds of source, in increasing order of speed:

| Source | When to use |
| ------ | ----------- |
| Lua table of numbers `0..255` | Small regions, one-off edits. Every element costs a Lua table lookup. |
| **Binary string** (`string.char`, `table.concat`, `string.pack`) | Whole frames built in Lua. Uploaded with one memcpy. |
| **`PixelBuffer`** | Per-frame simulation. No conversion at all — the bytes are already in engine memory. |

### PixelBuffer — native RGBA8 canvas

`PixelBuffer` is a native byte buffer, the pixel-level counterpart to `NoiseBuffer`. Reads and writes go straight to
C++ memory instead of a Lua table, and uploading it costs one memcpy, so a falling-sand simulation, a software
rasteriser or a procedural texture can run per frame without the table overhead that would otherwise dominate.

```lua
local buf = PixelBuffer.new(320, 240)          -- RGBA8, cleared to 0,0,0,0
                                               -- limit: 67 108 864 pixels, 16384 per side

buf:Width(); buf:Height()

buf:SetPixel(x, y, r, g, b, a)                 -- components 0..255; a defaults to 255
local r, g, b, a = buf:GetPixel(x, y)          -- four return values, no table allocated

buf:SetValue(x, y, packed)                     -- one 32-bit value: r | g<<8 | b<<16 | a<<24
local v = buf:GetValue(x, y)                   -- read it back

buf:Swap(x1, y1, x2, y2)                       -- exchange two pixels in place
buf:Fill(r, g, b, a)                           -- fill everything
buf:FillRect(x, y, w, h, r, g, b, a)           -- fill a rectangle (clipped)
buf:Clear()                                    -- all zeros
buf:Blit(otherBuffer, dstX, dstY)              -- copy another buffer in (clipped)
buf:CopyFrom(otherBuffer)                      -- exact copy; sizes must match

local blob = buf:ToString()                    -- raw bytes, ready for WriteFile
buf:FromString(blob)                           -- restore (needs at least as many bytes)

Texture.SetPixelBuffer("canvas", 0, 0, buf)    -- upload; then draw "canvas" as a quad
```

Out-of-range coordinates are ignored on write and read as zero, so a simulation can walk past the edges safely.

`SetValue` / `GetValue` are the fast path for grid simulations: one number per cell carries both the material id and its
colour, and `Swap` is the whole move step of a falling-sand cell.

```lua
-- Falling sand: one PixelBuffer is both the material grid and the framebuffer.
local W, H = 320, 240
local sand = PixelBuffer.new(W, H)
local EMPTY = 0

Texture.Create("sandCanvas", W, H, { filter = "nearest" })

function OnUpdate(dt)
    for y = 1, H - 1 do                          -- bottom-up so a grain moves one cell per step
        for x = 0, W - 1 do
            if sand:GetValue(x, y) ~= EMPTY then
                if sand:GetValue(x, y - 1) == EMPTY then
                    sand:Swap(x, y, x, y - 1)
                elseif sand:GetValue(x - 1, y - 1) == EMPTY then
                    sand:Swap(x, y, x - 1, y - 1)
                elseif sand:GetValue(x + 1, y - 1) == EMPTY then
                    sand:Swap(x, y, x + 1, y - 1)
                end
            end
        end
    end

    Texture.SetPixelBuffer("sandCanvas", 0, 0, sand)

    Draw.SetSpace("screen")
    Draw.SetBlend("opaque")
    Draw.Sprite("sandCanvas", 0, 0, W * 2, H * 2)
end
```

> A full-screen cellular automaton is still Lua work: budget roughly a few hundred thousand cell updates per frame at
> 60 FPS and simulate only the active region (dirty rectangles / chunks that contain moving cells), which is what
> pixel-simulation games do anyway.

### RenderTarget — offscreen rendering

A render target is a texture you can draw into. It is registered by name too, so it can be sampled by sprites and materials.

```lua
RenderTarget.Create("mirror", 512, 512, { filter = "linear", wrap = "clamp" })
RenderTarget.Exists("mirror")
RenderTarget.GetSize("mirror")
RenderTarget.Destroy("mirror")

RenderTarget.Clear("mirror", r, g, b, a, clearDepth)   -- clearDepth defaults to true
RenderTarget.CaptureScene("mirror")                    -- copy this frame's rendered scene into it
RenderTarget.ReadPixels("mirror", x, y, w, h)          -- flat array of RGBA bytes (slow, stalls the GPU)

-- Route immediate-mode geometry into the target:
Draw.SetTarget("mirror")
Draw.Rect(0, 0, 512, 512, 0, 0, 0, 1)
Draw.SetTarget(nil)
```

Render-target geometry is rendered **before** the main scene each frame, so a target you fill this frame can already be
sampled by sprites and materials in the same frame. In screen space the target's own pixel rectangle is used
(origin bottom-left); in world space the current camera projection is stretched over the target.

The depth buffer of a render target is **not** cleared automatically — call `RenderTarget.Clear` each frame if you draw
depth-tested world-space geometry into it.

### Complete example — raycast textured columns

```lua
local COLUMNS = 320
local WALL_TEX = "Content/Textures/wall.png"

function OnUpdate(dt)
    local px, py = GetPlayerPos()
    local angle  = GetPlayerAngle()

    local data = {}
    local i = 0
    for col = 0, COLUMNS - 1 do
        local rayAngle = angle + (col / COLUMNS - 0.5) * 60
        local dist, texU = CastRay(px, py, rayAngle)   -- your own DDA, in pure Lua
        local height = 12000 / math.max(dist, 0.1)
        local shade  = math.max(0.2, 1 - dist / 20)

        data[i+1]  = col; data[i+2] = 180
        data[i+3]  = 1;   data[i+4] = height
        data[i+5]  = -dist            -- Z: farther walls sit further back
        data[i+6]  = 0
        data[i+7]  = texU; data[i+8] = 0
        data[i+9]  = 1 / 64; data[i+10] = 1
        data[i+11] = shade; data[i+12] = shade; data[i+13] = shade; data[i+14] = 1
        i = i + 14
    end

    Draw.SetSpace("screen")
    Draw.SetBlend("opaque")
    Draw.QuadsPacked(WALL_TEX, data)
end
```

---

## 62. Decal — Bullet Holes, Blood Splatter, Scorch Marks

> **Type:** Global (`Decal.*`) + Entity-bound (`DecalComponent` accessors).
>
> Decals are surface marks the game leaves behind at runtime: bullet holes, blood splatter, scorch marks,
> footprints, cracks. They are described by a `.ice_decal` asset, spawned from script, and managed by the
> engine — lifetime, fade in / fade out, sorting, budget and pooling are all handled for you.

### The `.ice_decal` asset

Create it in the Content Browser (**Materials → Create Decal**), or right-click a texture and pick **Create Decal**.
Double-click to open the Decal Editor. An asset holds:

| Group | What it controls |
| ----- | ---------------- |
| Texture Variants | One or more textures with weights. A random one is picked per spawn — this is what makes ten bullet holes look different. |
| Material | Optional material with **Domain = Decal** (or a material instance of one). Replaces the plain textured draw. |
| Appearance | Size, size variance, pivot, tint, tint variance, shading mode, blend mode, alpha clip. |
| Rotation | Fixed, random, or aligned to the surface normal of the hit; random horizontal / vertical flip. |
| Lifetime | Lifetime, fade in, fade out, and the per-asset instance limit. |
| Placement | Z offset, sort order, normal offset, follow receiver, clip to receiver. |

### Spawning

```lua
-- Simplest form: place a decal in world space
local h = Decal.Spawn("Content/Decals/DC_BulletHole.ice_decal", x, y)

-- From a trace hit, aligned to the surface normal
local hit = LineTrace(fromX, fromY, toX, toY, true)
if hit.hit then
    Decal.SpawnOnHit("Content/Decals/DC_BulletHole.ice_decal",
                     hit.x, hit.y, hit.normalX, hit.normalY)
end

-- Attached to an entity: the decal moves and rotates with it
Decal.SpawnAttached("Content/Decals/DC_Blood.ice_decal", enemyId, hit.x, hit.y)

-- Attached AND clipped to a sprite's bounds, so blood never hangs off the character
Decal.SpawnOnSprite("Content/Decals/DC_Blood.ice_decal", enemyId, 0, hit.x, hit.y)
```

All spawn functions return a **handle** (a number). `0` means the spawn failed — a missing asset, an empty
budget, or a decal with no texture and no material.

### Spawn options

Every spawn function takes an optional table as the last argument. Anything you leave out comes from the asset.

```lua
Decal.Spawn("Content/Decals/DC_Scorch.ice_decal", x, y, {
    rotation   = 45,        -- explicit angle in degrees, overrides the asset rotation mode
    normalX    = 0,         -- surface normal, used by the Align To Normal rotation modes
    normalY    = 1,
    sizeX      = 64,        -- explicit size in pixels (both must be > 0 to take effect)
    sizeY      = 64,
    scale      = 1.5,       -- multiplier applied on top of the resolved size
    r = 1, g = 0.2, b = 0.2, a = 1,   -- tint override
    lifetime   = 8.0,       -- seconds, 0 = forever
    fadeIn     = 0.1,
    fadeOut    = 1.5,
    z          = 2.0,       -- depth of the surface; the asset Z offset is added on top
    sortOrder  = 10,        -- draw order among decals
    variant    = 2,         -- force a texture variant instead of picking at random
    seed       = 1337,      -- fixed seed (any integer): same variant, size, rotation and tint every time
    clipX      = wallX,     -- clip rectangle in world space
    clipY      = wallY,
    clipHalfW  = 128,
    clipHalfH  = 16,
    clipRotation = 0,
    clip       = true,      -- set false to pass a rectangle but keep clipping off
})
```

Clipping only takes effect when the asset has **Clip To Receiver** enabled. `Decal.SpawnOnSprite` fills the
clip rectangle for you from the sprite's own bounds.

### Managing spawned decals

```lua
Decal.IsAlive(h)                  -- false once it expired or was destroyed
Decal.Destroy(h)                  -- remove immediately
Decal.FadeOut(h, 0.5)             -- fade out over 0.5 s, then remove

Decal.Clear()                     -- remove every runtime decal
Decal.ClearByAsset("Content/Decals/DC_Blood.ice_decal")
Decal.ClearInRadius(x, y, 200)    -- returns how many were removed
Decal.ClearAttachedTo(entityId)

Decal.Preload("Content/Decals/DC_BulletHole.ice_decal")   -- load textures ahead of the first shot
Decal.Unload("Content/Decals/DC_BulletHole.ice_decal")    -- drop it from the cache; its live decals are removed
```

### Reading and changing a live decal

```lua
Decal.SetPosition(h, x, y)        local p = Decal.GetPosition(h)   -- {x, y}
Decal.SetZ(h, 3)                  local z = Decal.GetZ(h)
Decal.SetRotation(h, 90)          local r = Decal.GetRotation(h)
Decal.SetSize(h, 48, 48)          local s = Decal.GetSize(h)       -- {x, y}
Decal.SetColor(h, 1, 0, 0, 0.8)   local c = Decal.GetColor(h)      -- {r, g, b, a}
Decal.SetVisible(h, false)        local v = Decal.IsVisible(h)
Decal.SetSortOrder(h, 5)          local o = Decal.GetSortOrder(h)
Decal.SetLifetime(h, 20)          local l = Decal.GetLifetime(h)

local age  = Decal.GetAge(h)      -- seconds since spawn
local fade = Decal.GetFade(h)     -- current fade factor, 0..1
```

For an attached decal, `SetPosition` and `SetRotation` work in the space of the entity it follows, while
`GetPosition`, `GetZ`, `GetRotation` and `GetSize` always report the final world values.

### Budget and global switches

```lua
Decal.SetBudget(512)     -- max live decals; the oldest fade out when it is exceeded
local budget = Decal.GetBudget()
local count  = Decal.GetCount()          -- live decals in total
local blood  = Decal.GetCount("Content/Decals/DC_Blood.ice_decal")   -- live decals from one asset

Decal.SetEnabled(false)  -- stop accepting new spawns, e.g. on the lowest quality preset
local on = Decal.IsEnabled()
```

The budget is a soft cap: exceeding it starts fading the oldest decals instead of popping them. A hard cap of
twice the budget removes them outright, so a runaway spawn loop can never grow without bound. A per-asset
**Max Instances** limit works the same way, scoped to one `.ice_decal`.

### DecalComponent (Entity-bound)

A `Decal` component holds decals placed by hand in the Class Editor — graffiti, cracks, stains that are part
of the level. They render in both the editor and play mode and have no lifetime.

A placed instance still uses the asset's base rotation, size variance, tint variance and random mirroring.
Those random parts come from a hash of the instance **name** and its index, so they stay identical across
sessions and every instance of the same asset still looks a little different — rename an instance to reroll
its variation.

```lua
local n = GetDecalCount()
local i = FindDecalIndex("Graffiti")

SetDecalAsset("Content/Decals/DC_Crack.ice_decal", i)
local path = GetDecalAsset(i)

SetDecalPosition(x, y, z, i)      local p = GetDecalPosition(i)    -- {x, y, z}
SetDecalRotation(30, i)           local r = GetDecalRotation(i)
SetDecalScale(2, 2, i)            local s = GetDecalScale(i)
SetDecalSize(128, 64, i)          local sz = GetDecalSize(i)       -- size override, 0 = from asset
SetDecalColor(1, 1, 1, 0.5, i)    local c = GetDecalColor(i)
SetDecalVisible(true, i)          local v = IsDecalVisible(i)
SetDecalFlip(true, false, i)      local f = GetDecalFlip(i)        -- {x, y}
SetDecalSortOrder(3, i)           local o = GetDecalSortOrder(i)
ClearDecalSortOrder(i)            -- stop overriding, fall back to the asset's sort order
SetDecalVariant(1, i)             local v = GetDecalVariant(i)
SetDecalRenderInGame(true, i)     local g = IsDecalRenderInGame(i)
local name = GetDecalName(i)

-- Spawn a runtime decal at this entity's position
local h = SpawnDecalAtSelf("Content/Decals/DC_Blood.ice_decal", { lifetime = 6 })
```

The index argument is optional everywhere and defaults to `0`.
`HasDecal(entityId)`, `AddComponent("Decal")` and `AddEntityComponent(entityId, "Decal")` work through the usual component API (section 31).

### Decal materials

Give a material **Domain = Decal** in the Material Editor and it becomes assignable to a decal asset. Such a
material sees the decal texture as its entity texture, so the whole node graph — texture samples, parameters,
material functions, parameter collections — works exactly as it does for a surface material.

The **Decal Data** node exposes the state of the decal being drawn:

| Output | Meaning |
| ------ | ------- |
| Fade | Current fade factor, 0..1, from fade in / fade out and the budget |
| Normalized Age | Age divided by lifetime, 0..1 (0 when the lifetime is infinite) |
| Age | Seconds since spawn |
| Lifetime | The decal's lifetime in seconds |
| Random | Per-decal random value, 0..1, stable for the life of that decal |

Drive `Random` into a hue shift to make every splatter slightly different, or `Normalized Age` into a
lerp between fresh and dried blood.

### Complete example — a shooter's impact reaction

```lua
local DECAL_HOLE  = "Content/Decals/DC_BulletHole.ice_decal"
local DECAL_BLOOD = "Content/Decals/DC_Blood.ice_decal"

function OnStart()
    Decal.Preload(DECAL_HOLE)
    Decal.Preload(DECAL_BLOOD)
    Decal.SetBudget(384)
end

function Shoot(fromX, fromY, dirX, dirY)
    local hit = LineTrace(fromX, fromY, fromX + dirX * 2000, fromY + dirY * 2000, true)
    if not hit.hit then return end

    if hit.tag == "Enemy" and hit.entityId then
        Decal.SpawnOnSprite(DECAL_BLOOD, hit.entityId, 0, hit.x, hit.y, {
            lifetime = 12, fadeOut = 3, scale = 1.2
        })
    else
        Decal.SpawnOnHit(DECAL_HOLE, hit.x, hit.y, hit.normalX, hit.normalY, {
            lifetime = 30, fadeOut = 2
        })
    end
end
```

---
