# 🐍 IceBox Engine — Python API

## Full documentation in English

### Actual for B-0.8.3 Version

> **IceBox Engine** integrates **Python** via **pybind11** for editor scripting.
> The Python API lets you automate work in the editor, manage scenes, entities,
> components, project files, panels, and much more — right from the built-in Python console.
>
> This documentation describes **all** available Python functions with examples.

---

## 📑 Table of Contents

1. [Introduction and architecture](#1-introduction-and-architecture)
2. [Quick start](#2-quick-start)
   - [Opening the "Run Python Script" window](#opening-the-run-python-script-window)
   - [First commands](#first-commands)
   - [Core concepts](#core-concepts)
3. [Python basics — Guide for beginners](#3-python-basics--guide-for-beginners)
   - 3.1 [Variables and data types](#31-variables-and-data-types)
   - 3.2 [Arithmetic operations](#32-arithmetic-operations)
   - 3.3 [Comparison operations and logical operators](#33-comparison-operations-and-logical-operators)
   - 3.4 [Strings (str)](#34-strings-str)
   - 3.5 [Lists (list)](#35-lists-list)
   - 3.6 [Tuples (tuple)](#36-tuples-tuple)
   - 3.7 [Dictionaries (dict)](#37-dictionaries-dict)
   - 3.8 [Conditional statements (if / elif / else)](#38-conditional-statements-if--elif--else)
   - 3.9 [Loops (for / while)](#39-loops-for--while)
   - 3.10 [Functions (def)](#310-functions-def)
   - 3.11 [Importing modules](#311-importing-modules)
   - 3.12 [Error handling (try / except)](#312-error-handling-try--except)
   - 3.13 [Python built-in functions](#313-python-built-in-functions)
   - 3.14 [List and dictionary comprehensions](#314-list-and-dictionary-comprehensions)
   - 3.15 [Helpful techniques](#315-helpful-techniques)
4. [API modules — overview](#4-api-modules--overview)
5. [`editor` module — Working with the editor](#5-editor-module--working-with-the-editor)
   - 5.1 [Entities (Entity)](#51-entities-entity)
   - 5.2 [Selection](#52-selection)
   - 5.3 [Transforms (Transform)](#53-transforms-transform)
   - 5.4 [Components](#54-components)
   - 5.4.1 [Individual colliders](#541-individual-colliders)
   - 5.5 [Sprites (Sprite)](#55-sprites-sprite)
   - 5.6 [Folders](#56-folders)
   - 5.7 [Batch operations](#57-batch-operations)
   - 5.8 [Camera & viewport](#58-camera--viewport)
   - 5.9 [Editor panels](#59-editor-panels)
   - 5.10 [Play mode](#510-play-mode)
   - 5.11 [Undo / Redo](#511-undo--redo)
   - 5.12 [Clipboard](#512-clipboard)
   - 5.13 [Multi-instance components](#513-multi-instance-components)
   - 5.14 [World physics settings](#514-world-physics-settings)
   - 5.14.1 [Level world overrides](#5141-level-world-overrides)
   - 5.14.2 [Level world assets](#5142-level-world-assets)
   - 5.14.3 [Level script](#5143-level-script)
   - 5.15 [Editor settings](#515-editor-settings)
   - 5.16 [Localization](#516-localization)
   - 5.17 [Spatial queries](#517-spatial-queries)
   - 5.18 [Classes & export](#518-classes--export)
   - 5.19 [Events](#519-events)
   - 5.20 [Timers](#520-timers)
   - 5.21 [Logging](#521-logging)
   - 5.22 [User scripts & script execution](#522-user-scripts--script-execution)
6. [`scene` module — Working with scenes](#6-scene-module--working-with-scenes)
7. [`engine` module — Engine information](#7-engine-module--engine-information)
   - 7.1 [General info](#71-general-info)
   - 7.2 [Performance and statistics](#72-performance-and-statistics)
   - 7.3 [Resources](#73-resources)
   - 7.4 [Audio](#74-audio)
8. [`browser` module — Content browser](#8-browser-module--content-browser)
   - 8.1 [Navigation](#81-navigation)
   - 8.2 [Files and folders](#82-files-and-folders)
   - 8.3 [Reading and writing files](#83-reading-and-writing-files)
   - 8.4 [Assets](#84-assets)
   - 8.5 [Browser selection](#85-browser-selection)
   - 8.6 [Panel state, search and refresh](#86-panel-state-search-and-refresh)
   - 8.7 [Browser clipboard](#87-browser-clipboard)
   - 8.8 [Path utilities](#88-path-utilities)
   - 8.9 [Batch file operations](#89-batch-file-operations)
9. [`icebox.log` module — System logging](#9-iceboxlog-module--system-logging)
10. [Component types — Full reference](#10-component-types--full-reference)
11. [Supported asset types](#11-supported-asset-types)
12. [Practical examples](#12-practical-examples)
13. [FAQ and troubleshooting](#13-faq-and-troubleshooting)

---

## 1. Introduction and architecture

### What is the Python API in IceBox Engine?

**Python API** is a scripting interface for the **IceBox Engine editor**. Unlike the Lua API, which is used for game logic (object scripts, widgets, cinematics), the Python API is designed for:

- **Automating routine tasks** in the editor
- **Batch operations** on entities (bulk rename, move, delete)
- **Scene management** (create, save, load)
- **Working with project files** (content browser)
- **Getting diagnostic information** (FPS, memory, resources)
- **Extending editor functionality** without rebuilding the engine

### How does it work?

```
┌─────────────────────────────────────────────┐
│              IceBox Editor                   │
│                                             │
│  ┌──────────────────────────────────────┐   │
│  │         Python Console               │   │
│  │  >>> editor.create_entity('Player')  │   │
│  │  >>> engine.fps()                    │   │
│  └──────────────────────────────────────┘   │
│                    │                         │
│                    ▼                         │
│  ┌──────────────────────────────────────┐   │
│  │            Python Runtime            │   │
│  │  • Python initialization (pybind11)  │   │
│  │  • stdout/stderr capture             │   │
│  │  • Module management                 │   │
│  │  • Event system                      │   │
│  │  • Timer system                      │   │
│  └──────────────────────────────────────┘   │
│                    │                         │
│       ┌────────────┼────────────┐            │
│       ▼            ▼            ▼            │
│  ┌────────┐  ┌──────────┐  ┌─────────┐      │
│  │ editor │  │  engine   │  │ browser │      │
│  │ scene  │  │           │  │         │      │
│  └────────┘  └──────────┘  └─────────┘      │
│   Entities    Engine       Files            │
│   Components  Resources    Assets           │
│   Folders     Audio        Navigation       │
│   Panels      Stats        Operations       │
└─────────────────────────────────────────────┘
```

### Key technologies

| Technology | Description |
|-----------|-------------|
| **pybind11** | C++ ↔ Python bindings that allow calling C++ functions from Python |
| **Modular system** | API split into modules: `editor`, `scene`, `engine`, `browser` |
| **ECS (EnTT)** | Entity Component System — entity data stored in components |

### Python API vs Lua API

| Feature | Python API | Lua API |
|---------|-----------|---------|
| **Purpose** | Editor automation | Game logic |
| **When it works** | Editor only | Play mode |
| **Access to** | Scene, editor, files | Physics, input, camera, AI |
| **Undo/Redo** | ✅ Supported | No |
| **File operations** | ✅ Full access | Limited |
| **Batch operations** | ✅ Batch functions | No |

---

## 2. Quick start

### Opening the "Run Python Script" window

All Python tooling lives in a single dockable panel. Open it from the main menu:

> **`Tools → Run Python Script`**

This opens the panel titled **Python Console** — the central surface for everything in this guide. From here you write and run Python, pull in ready-made snippets, save and load script files, and read the built-in API reference. In practice, almost all day-to-day Python work in IceBox happens in this one window.

#### Window layout

The panel is split into four areas, top to bottom:

| Area | What it is |
|------|-----------|
| **Menu bar** | Four menus: *Run Python Script*, *Show Console*, *Snippets*, *Help* (detailed below). |
| **Script editor** | A multi-line code editor with Python syntax highlighting, a small toolbar, and optional live error markers. Executed with the **Run Script** button. |
| **Quick command** | A single-line input for one-off commands, with Tab-completion and Up/Down history. Executed with **Execute** or `Enter`. |
| **Status bar** | The last execution status, plus a reminder of the available modules and `help()`. |

When a file is loaded, its name is shown in parentheses next to the **Script Editor:** label.

#### Menu bar reference

**`Run Python Script`** — manage script files and the on-disk tool scripts:

| Item | Action |
|------|--------|
| **Save Script…** (`Ctrl+S`) | Save the editor contents. Prompts for a path the first time, then overwrites it. |
| **Save Script As…** (`Ctrl+Shift+S`) | Save the editor contents to a new file. |
| **Load Script…** (`Ctrl+O`) | Load a `.py` file into the editor. |
| **Export History…** | Write the quick-command history to `Tools/PythonScripts/history.txt`. |
| *(discovered scripts)* | Every `.py` file under `Tools/PythonScripts/` is listed here, grouped into submenus by folder. Click one to run it; hover to see its full path. |
| **Run Python Script…** | Pick one or more `.py` files from disk and run them right away. |
| **Reload Scripts** | Re-scan `Tools/PythonScripts/` after you add, rename, or delete files. |
| **Reload API Autocomplete** | Rebuild the editor's completion index (e.g. after new imports or bindings). |
| **Run Startup Scripts** | Run everything in `Tools/PythonScripts/Startup/` again. |
| **Open Scripts Folder** | Open `Tools/PythonScripts/` in the OS file browser. |

> If no scripts are found, the menu shows *"No scripts found. Drop .py files in Tools/PythonScripts"*.

**`Show Console`** — actions on the interactive console environment:

| Item | Action |
|------|--------|
| **Reset Environment** | Wipe everything you defined in the interactive console (variables, functions, imports) and recreate a clean namespace with `help()`. |
| **Clear History** | Clear the quick-command history. |

**`Snippets`** — one-click code templates grouped by topic: entities, content browser, diagnostics (FPS / memory / render), components, batch operations, camera & scene, and events & timers. Picking a snippet loads it into either the quick-command field or the script editor, ready to run or edit.

**`Help`** — print API references straight into the log:

| Item | Action |
|------|--------|
| **Show API Reference** | Run `help()` — the full module overview. |
| **Show Editor API** | List every `editor` function, grouped by category. |
| **Show Browser API** | List every `browser` function. |
| **Show Scene API** | List every `scene` function. |
| **Engine API** | List every `engine` function. |

#### The script editor

The editor is a real code editor, not just a text box. Its toolbar (above the editor) and the shortcuts that work while editing:

| Button | Shortcut | Action |
|--------|----------|--------|
| **Un** / **Re** | — | Undo / redo edits. |
| **Se** | `Ctrl+F` | Find. `Ctrl+H` opens find **and** replace. `Aa` toggles case sensitivity; `<` / `>` step through matches. |
| **Ln** | `Ctrl+G` | Go to a line number. |
| **fn** | — | Function navigator — jump to any `def` / `class` in the script (callbacks are highlighted). |
| **+T** | — | Insert a code template (control flow, functions, classes, editor loops, events…). |
| **Auto** | — | Auto-compile: while ticked, the script is syntax-checked ~0.6 s after you stop typing and the offending line is flagged. |

**Autocomplete** pops up automatically once you have typed two or more characters. It draws from the built-in API, live runtime bindings, and the functions you have defined in the current script. Use `↑` / `↓` to choose, `Tab` or `Enter` to accept, `Esc` to dismiss.

Press **Run Script** to execute the whole editor, or **Clear** to reset it to an empty buffer.

#### Quick command field

For quick one-liners, use the **Quick Command** input at the bottom: type a command and press `Enter` (or click **Execute**). `Tab` completes the current word; `↑` / `↓` walk through your last 100 commands.

#### Console vs. global execution scope

There are two namespaces, and it is worth knowing which one your code lands in:

- The **interactive console** — the **Run Script** button *and* the **Quick Command** field — runs in a **persistent console namespace**. Variables, functions, and imports you create there stay available for later commands until you choose **Show Console → Reset Environment**.
- **Script files** — discovered tool scripts, **Run Python Script…**, **Run Startup Scripts**, and `editor.execute_file()` — run in the main (`__main__`) namespace.
- Either way, each run is wrapped as **one undo step**: a single `editor.undo()` reverts everything the script changed. **Events and timers** registered from any scope persist (they are owned by the engine), so a `Startup` script can install handlers that keep working for the whole session.

#### Tool script folders

Two editor-only folders back this window. They are tooling, **not** game content, and are never shipped with the game:

- `Tools/PythonScripts/` — your tool scripts. Each subfolder becomes a category submenu.
- `Tools/PythonScripts/Startup/` — scripts that run once automatically when the editor starts.

See [5.22 User scripts & script execution](#522-user-scripts--script-execution) for the matching API (`run_user_script`, `rediscover_user_scripts`, `execute_file`, …).

### First commands

```python
# Show help
help

# Get FPS
engine.fps()

# Create an entity
uuid = editor.create_entity('MyEntity')
print(f'Created entity with UUID: {uuid}')

# Move the entity
editor.set_position(uuid, 100, 200)

# Get a list of all entities
names = editor.get_entity_names()
print(names)
```

### Core concepts

- **UUID** — unique 64-bit entity identifier (`uint64`). Each entity has its own UUID.
- **Component** — a data block attached to an entity (Transform, SpriteRenderer, Rigidbody, etc.)
- **Instance** — an instance of a multi-instance component (for example, SpriteRenderer can have multiple sprites).
- **Module** — group of related functions (`editor`, `scene`, `engine`, `browser`).

---

## 3. Python basics — Guide for beginners

> This section is a short but complete reference of basic Python. If you already know Python — feel free to jump to [section 4](#4-api-modules--overview). If not — read this section, and you will be able to confidently write scripts in the IceBox Engine Python console.

---

### 3.1 Variables and data types

In Python you do not need to declare a variable type — it is inferred automatically from the assigned value.

```python
# Integer (int)
health = 100
score = 0
entity_count = 42

# Floating point number (float)
speed = 3.5
zoom = 1.0
pi = 3.14159

# String (str) — text in quotes
name = 'Player'
path = "Content/Textures/hero.png"
greeting = "Hello, world!"

# Boolean value (bool) — True or False
is_alive = True
is_paused = False

# Empty value (None) — means "nothing"
result = None
```

#### Type check

```python
x = 42
print(type(x))     # <class 'int'>

y = 3.14
print(type(y))     # <class 'float'>

name = 'Player'
print(type(name))  # <class 'str'>
```

#### Type conversion

```python
# String → number
x = int('42')       # 42
y = float('3.14')   # 3.14

# Number → string
s = str(100)         # '100'
s = str(3.14)        # '3.14'

# Float → int (drops the fractional part)
n = int(3.7)         # 3

# Int → float
f = float(5)         # 5.0

# Anything → boolean
bool(0)       # False
bool(1)       # True
bool('')      # False  (empty string)
bool('hello') # True   (non-empty string)
bool([])      # False  (empty list)
bool([1, 2])  # True   (non-empty list)
```

---

### 3.2 Arithmetic operations

```python
a = 10
b = 3

a + b     # 13    — addition
a - b     # 7     — subtraction
a * b     # 30    — multiplication
a / b     # 3.333 — division (always returns float)
a // b    # 3     — integer division (drops remainder)
a % b     # 1     — modulo
a ** b    # 1000  — exponentiation (10³)
```

#### Shorthand notation

```python
x = 10
x += 5    # x = x + 5 → 15
x -= 3    # x = x - 3 → 12
x *= 2    # x = x * 2 → 24
x /= 4    # x = x / 4 → 6.0
```

---

### 3.3 Comparison operations and logical operators

#### Comparison

```python
a = 10
b = 20

a == b    # False — equal
a != b    # True  — not equal
a > b     # False — greater than
a < b     # True  — less than
a >= b    # False — greater or equal
a <= b    # True  — less or equal
```

#### Logical operators

```python
x = True
y = False

x and y   # False — AND (both must be True)
x or y    # True  — OR (at least one True)
not x     # False — NOT (invert)

# Combinations
a = 5
(a > 0) and (a < 10)   # True — a in range (0, 10)
(a == 5) or (a == 10)  # True — a equals 5 or 10
not (a > 100)          # True — a is NOT greater than 100
```

---

### 3.4 Strings (str)

Strings are one of the most used types. In the IceBox console you constantly work with strings (entity names, file paths, components).

#### Creating strings

```python
s1 = 'Single quotes'
s2 = "Double quotes"
s3 = '''Multiline
string'''
```

#### Basic operations

```python
name = 'Player'

len(name)              # 6 — string length
name.upper()           # 'PLAYER' — uppercase
name.lower()           # 'player' — lowercase
name.startswith('P')   # True
name.endswith('er')    # True
name.replace('Player', 'Enemy')  # 'Enemy'

# Concatenation
first = 'Ice'
second = 'Box'
full = first + second       # 'IceBox'
full = first + ' ' + second # 'Ice Box'

# Repetition
line = '-' * 20   # '--------------------'
```

#### String formatting (f-strings)

F-strings are the most convenient way to insert values into text. Put `f` before quotes and use `{}` to insert expressions.

```python
name = 'Player'
health = 100
x, y = 150.5, 200.3

# Simple insertion
print(f'Name: {name}')               # Name: Player
print(f'Health: {health}')           # Health: 100

# Expressions inside {}
print(f'Sum: {2 + 3}')               # Sum: 5
print(f'Position: ({x}, {y})')       # Position: (150.5, 200.3)

# Number formatting
pi = 3.14159
print(f'Pi = {pi:.2f}')              # Pi = 3.14 (2 digits after the dot)

fps = 59.7
print(f'FPS: {fps:.0f}')             # FPS: 60 (no decimals)

size = 15667.2
print(f'Size: {size/1024:.1f} KB')   # Size: 15.3 KB
```

#### Indexing and slicing

```python
s = 'Hello World'

s[0]      # 'H'     — first character (0-based)
s[1]      # 'e'     — second character
s[-1]     # 'd'     — last character
s[-2]     # 'l'     — second to last

s[0:5]    # 'Hello' — slice from 0 to 5 (excluding 5)
s[6:]     # 'World' — from 6 to end
s[:5]     # 'Hello' — from start to 5
```

#### Splitting and joining

```python
path = 'Content/Textures/player.png'

parts = path.split('/')   # ['Content', 'Textures', 'player.png']
folder = parts[0]          # 'Content'
filename = parts[-1]       # 'player.png'

# Reverse operation — join
'/'.join(['Content', 'Textures', 'hero.png'])
# 'Content/Textures/hero.png'

# Membership check
'player' in path   # True
'enemy' in path    # False
```

---

### 3.5 Lists (list)

List is an ordered collection of elements. Elements can be added, removed, and modified.

#### Creation and basic operations

```python
# Creation
enemies = ['Slime', 'Bat', 'Skeleton']
numbers = [1, 2, 3, 4, 5]
empty = []

# Length
len(enemies)         # 3

# Access by index (0-based)
enemies[0]           # 'Slime'
enemies[1]           # 'Bat'
enemies[-1]          # 'Skeleton' (last)

# Modify an element
enemies[0] = 'BigSlime'

# Membership check
'Bat' in enemies     # True
'Dragon' in enemies  # False
```

#### Adding and removing

```python
fruits = ['apple', 'banana']

fruits.append('cherry')     # Add to end → ['apple', 'banana', 'cherry']
fruits.insert(0, 'mango')   # Insert at index → ['mango', 'apple', 'banana', 'cherry']

fruits.remove('banana')     # Remove by value
fruits.pop()                # Remove and return last item
fruits.pop(0)               # Remove and return item by index

fruits.clear()              # Clear the list
```

#### List slices

```python
numbers = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

numbers[2:5]    # [2, 3, 4]
numbers[:3]     # [0, 1, 2]
numbers[7:]     # [7, 8, 9]
numbers[::2]    # [0, 2, 4, 6, 8] — every second
```

#### Useful methods

```python
numbers = [3, 1, 4, 1, 5, 9]

sorted(numbers)          # [1, 1, 3, 4, 5, 9] — new sorted list
numbers.sort()           # Sorts the list in place
numbers.reverse()        # Reverses the list
numbers.count(1)         # 2 — how many times 1 appears
numbers.index(4)         # 2 — index of first 4

# Min, max, sum
min(numbers)   # 1
max(numbers)   # 9
sum(numbers)   # 23
```

#### Joining lists

```python
a = [1, 2, 3]
b = [4, 5, 6]
c = a + b          # [1, 2, 3, 4, 5, 6]
a.extend(b)        # a is now [1, 2, 3, 4, 5, 6]
```

---

### 3.6 Tuples (tuple)

Tuple is like a list, but **immutable**. Many IceBox API functions return tuples (for example, position `(x, y, z)`).

```python
# Creation
pos = (100.0, 200.0, 0.0)
size = (1920, 1080)

# Access by index
pos[0]    # 100.0
pos[1]    # 200.0

# Unpacking — the most convenient way
x, y, z = pos         # x=100.0, y=200.0, z=0.0
width, height = size  # width=1920, height=1080

# Unpacking from IceBox API functions
x, y, z = editor.get_position(uuid)
sx, sy = editor.get_scale(uuid)
cam_x, cam_y = editor.get_camera_position()
```

> ⚠️ Tuples **cannot be modified** after creation. If you need to change them — create a new tuple or use a list.

---

### 3.7 Dictionaries (dict)

Dictionary is a collection of key-value pairs. IceBox API uses dictionaries heavily for component data.

#### Creation and access

```python
# Creation
player = {
    'name': 'Hero',
    'health': 100,
    'position': (0, 0, 0),
    'alive': True
}

# Access by key
player['name']       # 'Hero'
player['health']     # 100

# Safe access (won't fail if key is missing)
player.get('score', 0)    # 0 (key 'score' not found, returns default)

# Modification
player['health'] = 80

# Add new key
player['score'] = 500

# Deletion
del player['alive']
```

#### Checking and iteration

```python
settings = {'show_grid': True, 'snap_to_grid': False, 'grid_size': 32}

# Key check
'show_grid' in settings    # True
'theme' in settings        # False

# Iterate keys
for key in settings:
    print(key)

# Iterate key-value pairs
for key, value in settings.items():
    print(f'{key} = {value}')

# All keys and values
settings.keys()     # dict_keys(['show_grid', 'snap_to_grid', 'grid_size'])
settings.values()   # dict_values([True, False, 32])
```

#### Using with IceBox API

```python
# Get component data (returns a dict)
data = editor.get_component(uuid, 'Rigidbody')
print(data['body_type'])       # 0 (Static, the default)
print(data['gravity_scale'])   # 1.0

# Set component data (pass a dict)
editor.set_component(uuid, 'Camera', {
    'ortho_width': 1920.0,
    'background_color': (0.1, 0.1, 0.2)
})

# Entity info is also a dict
info = editor.get_entity_info(uuid)
print(info['name'])
print(info['components'])
```

---

### 3.8 Conditional statements (if / elif / else)

Conditions let you execute different code depending on the situation.

> ⚠️ **Important**: in Python blocks are defined by **indentation** (4 spaces). Incorrect indentation is an error.

```python
# Simple condition
health = 50

if health <= 0:
    print('Player is dead')

# Condition with alternative
if health > 50:
    print('Health is fine')
else:
    print('Health is low')

# Multiple conditions
if health > 75:
    print('Excellent health')
elif health > 25:
    print('Average health')
elif health > 0:
    print('Critical health!')
else:
    print('Player is dead')
```

#### Practical examples with IceBox

```python
# Check if entity exists before working with it
uuid = editor.find_entity('Player')
if uuid:
    x, y, z = editor.get_position(uuid)
    print(f'Player at ({x}, {y})')
else:
    print('Player not found in the scene')

# Check play mode
if editor.is_play_mode():
    print('Game is running')
    if editor.is_paused():
        print('(paused)')
else:
    print('Editor mode')

# Check component before modifying
if editor.has_component(uuid, 'Rigidbody'):
    editor.set_component(uuid, 'Rigidbody', {'gravity_scale': 0})
else:
    editor.add_component(uuid, 'Rigidbody')
    print('Rigidbody added')
```

---

### 3.9 Loops (for / while)

#### `for` loop — iterating elements

```python
# Iterate a list
fruits = ['apple', 'banana', 'cherry']
for fruit in fruits:
    print(fruit)

# Iterate with index
for i, fruit in enumerate(fruits):
    print(f'{i}: {fruit}')
# 0: apple
# 1: banana
# 2: cherry

# Iterate a range of numbers
for i in range(5):        # 0, 1, 2, 3, 4
    print(i)

for i in range(2, 8):     # 2, 3, 4, 5, 6, 7
    print(i)

for i in range(0, 20, 5): # 0, 5, 10, 15 (step 5)
    print(i)
```

#### Practical examples with IceBox

```python
# Iterate all entities
names = editor.get_entity_names()
for name in names:
    print(name)

# Iterate and search
uuids = editor.get_entity_uuids()
for uuid in uuids:
    if editor.has_component(uuid, 'Rigidbody'):
        name = editor.get_entity_name(uuid)
        print(f'{name} — has physics')

# Create entities in a loop
for i in range(10):
    uuid = editor.create_entity(f'Enemy_{i}')
    editor.set_position(uuid, i * 100, 0)

# Iterate folders
for folder in editor.get_folders():
    entities = editor.get_entities_in_folder(folder)
    print(f'📁 {folder}: {len(entities)} entities')
```

#### `while` loop — repeat while condition is true

```python
# Simple while loop
count = 0
while count < 5:
    print(count)
    count += 1
# 0, 1, 2, 3, 4
```

#### `break` and `continue`

```python
# break — stop the loop
for name in editor.get_entity_names():
    if name == 'Player':
        print('Player found!')
        break   # Stop searching

# continue — skip iteration
uuids = editor.get_entity_uuids()
for uuid in uuids:
    if not editor.has_component(uuid, 'SpriteRenderer'):
        continue   # Skip entities without a sprite
    path = editor.get_sprite(uuid)
    print(f'{editor.get_entity_name(uuid)}: {path}')
```

---

### 3.10 Functions (def)

Functions are blocks of code that can be called multiple times.

#### Definition and call

```python
# Simple function
def greet():
    print('Hello!')

greet()   # Hello!

# Function with parameters
def greet_player(name):
    print(f'Hello, {name}!')

greet_player('Alex')   # Hello, Alex!

# Function with return value
def add(a, b):
    return a + b

result = add(3, 5)   # 8

# Default parameters
def create_enemy(name='Slime', health=100):
    print(f'Created {name} with HP={health}')

create_enemy()                    # Created Slime with HP=100
create_enemy('Dragon', 500)       # Created Dragon with HP=500
create_enemy(health=200)          # Created Slime with HP=200
```

#### Practical functions for IceBox

```python
# Function to create a configured entity
def spawn_enemy(name, x, y, sprite_path):
    uuid = editor.create_entity(name)
    editor.set_position(uuid, x, y)
    editor.set_sprite(uuid, sprite_path)
    editor.add_component(uuid, 'Rigidbody')
    editor.add_component(uuid, 'Collider')
    return uuid

enemy1 = spawn_enemy('Slime', 100, 0, 'Content/Textures/slime.png')
enemy2 = spawn_enemy('Bat', 300, 0, 'Content/Textures/bat.png')

# Function for a report
def scene_report():
    names = editor.get_entity_names()
    print(f'Entities: {len(names)}')
    print(f'FPS: {engine.fps():.0f}')
    print(f'Scene: {scene.get_path()}')

scene_report()
```

#### Lambda functions

Lambda is a one-line function. Often used for timers and events.

```python
# Regular function
def square(x):
    return x * x

# Equivalent lambda
square = lambda x: x * x

print(square(5))   # 25

# Use with IceBox timers
editor.set_timer(3.0, lambda: print('3 seconds passed!'))
editor.set_timer(5.0, lambda: editor.log_info('Timer fired'))
```

---

### 3.11 Importing modules

Python has a rich standard library. The most useful modules for IceBox:

```python
# Math
import math

math.pi                  # 3.14159...
math.sqrt(16)            # 4.0
math.sin(math.pi / 2)   # 1.0
math.cos(0)              # 1.0
math.degrees(math.pi)   # 180.0
math.radians(90)         # 1.5707...
math.ceil(3.2)           # 4
math.floor(3.8)          # 3

# Random numbers
import random

random.randint(1, 100)                       # Random integer from 1 to 100
random.uniform(0.5, 2.0)                     # Random float from 0.5 to 2.0
random.choice(['red', 'blue', 'green'])      # Random element from a list
random.shuffle(my_list)                      # Shuffle list in place

# Working with JSON
import json

data = {'name': 'Player', 'health': 100}
text = json.dumps(data)      # Dict → JSON string
back = json.loads(text)      # JSON string → dict

# Time
import time

time.time()       # Current time (Unix timestamp)

# Regular expressions
import re

matches = re.findall(r'\d+', 'Tree_01 and Tree_02')  # ['01', '02']
```

#### Example: random entity placement

```python
import random

for i in range(20):
    uuid = editor.create_entity(f'Star_{i}')
    x = random.uniform(-500, 500)
    y = random.uniform(-500, 500)
    editor.set_position(uuid, x, y)
    editor.set_rotation(uuid, random.uniform(0, 360))
    scale = random.uniform(0.5, 1.5)
    editor.set_scale(uuid, scale, scale)
```

---

### 3.12 Error handling (try / except)

If code can fail, wrap it in `try/except` to handle errors gracefully.

```python
# Basic syntax
try:
    result = 10 / 0
except ZeroDivisionError:
    print('Division by zero!')

# Catch any error
try:
    uuid = editor.find_entity('Player')
    x, y, z = editor.get_position(uuid)
except Exception as e:
    print(f'Error: {e}')

# try / except / finally
try:
    data = editor.get_component(uuid, 'Camera')
    print(data['ortho_width'])
except Exception as e:
    editor.log_error(f'Failed to get component: {e}')
finally:
    print('This code runs always')
```

---

### 3.13 Python built-in functions

The most useful built-in functions that are **always available** without imports:

| Function | Description | Example |
|---------|----------|--------|
| `print(x)` | Output to console | `print('Hello')` |
| `len(x)` | Length (string, list, dict) | `len([1, 2, 3])` → `3` |
| `type(x)` | Object type | `type(42)` → `<class 'int'>` |
| `int(x)` | Convert to integer | `int('42')` → `42` |
| `float(x)` | Convert to float | `float('3.14')` → `3.14` |
| `str(x)` | Convert to string | `str(100)` → `'100'` |
| `bool(x)` | Convert to boolean | `bool(0)` → `False` |
| `abs(x)` | Absolute value | `abs(-5)` → `5` |
| `round(x, n)` | Round to n digits | `round(3.14159, 2)` → `3.14` |
| `min(...)` | Minimum | `min(3, 1, 4)` → `1` |
| `max(...)` | Maximum | `max(3, 1, 4)` → `4` |
| `sum(list)` | Sum of elements | `sum([1, 2, 3])` → `6` |
| `range(n)` | Sequence of numbers | `list(range(5))` → `[0,1,2,3,4]` |
| `enumerate(list)` | Elements with indices | `list(enumerate(['a','b']))` → `[(0,'a'),(1,'b')]` |
| `zip(a, b)` | Pairing | `list(zip([1,2], ['a','b']))` → `[(1,'a'),(2,'b')]` |
| `sorted(list)` | Sorted copy | `sorted([3,1,2])` → `[1,2,3]` |
| `reversed(list)` | Reversed order | `list(reversed([1,2,3]))` → `[3,2,1]` |
| `isinstance(x, T)` | Type check | `isinstance(42, int)` → `True` |
| `any(list)` | Any True? | `any([False, True, False])` → `True` |
| `all(list)` | All True? | `all([True, True, False])` → `False` |

---

### 3.14 List and dictionary comprehensions

Compact way to build lists and dicts from loops.

#### List comprehension

```python
# Classic way
squares = []
for i in range(10):
    squares.append(i ** 2)

# List comprehension — same, but one line
squares = [i ** 2 for i in range(10)]
# [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]

# With condition (filtering)
even = [i for i in range(20) if i % 2 == 0]
# [0, 2, 4, 6, 8, 10, 12, 14, 16, 18]
```

#### Using with IceBox

```python
# Get names of all entities with physics
physics_uuids = editor.find_entities_by_component('Rigidbody')
physics_names = [editor.get_entity_name(u) for u in physics_uuids]
print(physics_names)

# Filter entities by position (only those to the right of x=0)
uuids = editor.get_entity_uuids()
right_side = [u for u in uuids if editor.get_position(u)[0] > 0]
print(f'To the right of center: {len(right_side)} entities')

# Dict: name → position for all entities
positions = {
    editor.get_entity_name(u): editor.get_position(u)
    for u in editor.get_entity_uuids()
}
print(positions)
```

---

### 3.15 Helpful techniques

#### Multiple assignment

```python
x, y, z = 100, 200, 0
a = b = c = 0
```

#### Ternary operator

```python
status = 'alive' if health > 0 else 'dead'
label = f'Player ({status})'
```

#### Unpacking collections

```python
first, *rest = [1, 2, 3, 4, 5]
# first = 1, rest = [2, 3, 4, 5]

head, *middle, tail = [1, 2, 3, 4, 5]
# head = 1, middle = [2, 3, 4], tail = 5
```

#### Check for None

```python
result = editor.find_entity('Boss')

if result is None:
    print('Not found')

# Or shorter (0 and None are both "false")
if not result:
    print('Not found')
```

#### Call chaining

```python
# Find and act immediately
uuid = editor.find_entity('Player')
if uuid:
    editor.set_position(uuid, 0, 0)
```

#### Final cheat sheet

```
Types:       int, float, str, bool, None, list, tuple, dict
Operators:   + - * / // % **  ==  !=  > < >= <=  and or not  in
Control:     if / elif / else / for / while / break / continue
Functions:   def name(parameters): ... return value
Lambda:      lambda x: expression
Import:      import module
Errors:      try: ... except: ... finally: ...
f-strings:   f'text {variable} text {expression:.2f}'
Lists:       [item for item in iterable if condition]
```

---

## 4. API modules — overview

| Module | Description | Example |
|--------|----------|--------|
| `editor` | Entities, components, transforms, panels, camera, events | `editor.create_entity('Box')` |
| `scene` | Saving/loading scenes, exporting/importing entities | `scene.save('Content/level.icemap')` |
| `engine` | Engine version, FPS, memory, resources, audio | `engine.fps()` |
| `browser` | Content browser, files, folders, assets | `browser.list_files()` |
| `icebox.log` | System logging to engine console | `icebox.log.info('Hello')` |

---

## 5. `editor` module — Working with the editor

The largest and most important module. Provides full control over the scene, entities, and the editor.

---

### 5.1 Entities (Entity)

#### `editor.create_entity(name='Entity')` → `int`

Creates a new empty entity with the specified name. Returns UUID.

```python
uuid = editor.create_entity('Player')
print(f'Created entity: {uuid}')

# Without name — will be "Entity"
uuid2 = editor.create_entity()
```

> ✅ Automatically records state for Undo.
> 🔔 Generates `entity_created` event.

#### `editor.delete_entity(uuid)` → `bool`

Deletes an entity by UUID. Returns `True` on success.

```python
success = editor.delete_entity(uuid)
if success:
    print('Entity deleted')
```

> 🔔 Generates `entity_deleted` event.

#### `editor.delete_entity_by_name(name)` → `bool`

Deletes the first found entity with the specified name.

```python
editor.delete_entity_by_name('TempEntity')
```

#### `editor.duplicate_entity(uuid)` → `int`

Duplicates an entity with all components. The copy is offset 50 pixels to the right. Returns UUID of the copy.

```python
copy_uuid = editor.duplicate_entity(original_uuid)
editor.set_entity_name(copy_uuid, 'Player_Copy')
```

#### `editor.instantiate(class_path, x=0, y=0, z=0)` → `int`

Creates an entity from a class file (`.ice_class`) at the specified position. Returns UUID.

```python
enemy = editor.instantiate('Content/Enemies/Slime.ice_class', 500, 300)
```

#### `editor.get_entity_names()` → `list[str]`

Returns list of names of all entities in the current scene.

```python
names = editor.get_entity_names()
for name in names:
    print(name)
```

#### `editor.get_entity_uuids()` → `list[int]`

Returns list of UUIDs of all entities.

```python
uuids = editor.get_entity_uuids()
print(f'Total entities: {len(uuids)}')
```

#### `editor.find_entity(name)` → `int`

Finds entity UUID by exact name. Returns `0` if not found.

```python
uuid = editor.find_entity('Player')
if uuid != 0:
    print(f'Found Player: {uuid}')
else:
    print('Player not found')
```

#### `editor.find_entities(pattern)` → `list[int]`

Finds all entities whose names match a regular expression (regex). Case-insensitive.

```python
# Find all entities starting with "Enemy"
enemies = editor.find_entities('^Enemy')

# Find entities containing "wall" (any case)
walls = editor.find_entities('wall')

# Find entities like "Tree_01", "Tree_02", ...
trees = editor.find_entities(r'Tree_\d+')
```

#### `editor.entity_exists(uuid)` → `bool`

Checks if an entity with the given UUID exists.

```python
if editor.entity_exists(uuid):
    print('Entity exists')
```

#### `editor.get_entity_name(uuid)` → `str`

Returns entity name by UUID. Empty string if not found.

```python
name = editor.get_entity_name(uuid)
```

#### `editor.set_entity_name(uuid, name)` → `bool`

Sets a new name for the entity.

```python
editor.set_entity_name(uuid, 'NewName')
```

#### `editor.get_entity_info(uuid)` → `dict`

Returns full info about an entity: name, position, scale, rotation, folder, component list.

```python
info = editor.get_entity_info(uuid)
# Result:
# {
#     'uuid': 123456789,
#     'name': 'Player',
#     'position': (100.0, 200.0, 0.0),
#     'scale': (1.0, 1.0),
#     'rotation': 0.0,
#     'folder': 'Characters',
#     'components': ['Transform', 'SpriteRenderer', 'Rigidbody', 'Script']
# }
```

#### `editor.get_entity_components(uuid)` → `list[str]`

Returns list of component names attached to the entity.

```python
comps = editor.get_entity_components(uuid)
print(comps)  # ['Transform', 'SpriteRenderer', 'Rigidbody']
```

#### `editor.get_component_types()` → `list[str]`

Returns list of **all** supported component types in the engine.

```python
types = editor.get_component_types()
# ['Transform', 'SpriteRenderer', 'Camera', 'Rigidbody', 'Collider',
#  'Flipbook', 'Audio', 'Animator', 'Skeleton', 'Tilemap', 'FX',
#  'Widget', 'PointLight', 'SpotLight', 'PointMarker', 'Script', 'AI', 'Joint',
#  'Destructible', 'ClassComponent', 'Stencil', 'GameplayTag', 'Interface',
#  'Replication', 'Hierarchy']
```

> 25 types. See the [add / remove support matrix](#add--remove-support-matrix) — `Transform` and `Hierarchy` are in this list but cannot be added or removed.

---

### 5.2 Selection

#### `editor.select_entity(name)` → `bool`

Selects an entity by name in the Level Outliner.

```python
editor.select_entity('Player')
```

> 🔔 Generates `selection_changed` event.

#### `editor.select_entity_by_uuid(uuid)` → `bool`

Selects an entity by UUID.

```python
editor.select_entity_by_uuid(uuid)
```

#### `editor.select_entities(uuids)` → `bool`

Selects multiple entities by UUID list (previous selection cleared).

```python
editor.select_entities([uuid1, uuid2, uuid3])
```

#### `editor.select_all()` → `bool`

Selects all entities in the scene.

```python
editor.select_all()
```

#### `editor.clear_selection()`

Clears current selection.

```python
editor.clear_selection()
```

> 🔔 Generates `selection_changed` event.

#### `editor.get_selected()` → `list[str]`

Returns names of all selected entities.

```python
selected = editor.get_selected()
print(f'Selected: {selected}')
```

#### `editor.get_selected_uuids()` → `list[int]`

Returns UUIDs of all selected entities.

```python
selected_uuids = editor.get_selected_uuids()
```

---

### 5.3 Transforms (Transform)

Each entity has a `Transform` component with position, scale, and rotation.

#### `editor.get_position(uuid)` → `tuple(x, y, z)`

Returns entity position as `(x, y, z)`.

```python
x, y, z = editor.get_position(uuid)
print(f'Position: ({x}, {y}, {z})')
```

#### `editor.set_position(uuid, x, y, z=0)` → `bool`

Sets absolute entity position.

```python
editor.set_position(uuid, 100, 200)       # z defaults to 0
editor.set_position(uuid, 100, 200, 5)    # with z (layer)
```

#### `editor.translate(uuid, dx, dy, dz=0)` → `bool`

Moves entity by the specified delta (relative move).

```python
# Move right by 50 pixels
editor.translate(uuid, 50, 0)

# Move up and raise layer
editor.translate(uuid, 0, -100, 1)
```

#### `editor.get_scale(uuid)` → `tuple(x, y)`

Returns entity scale as `(x, y)`.

```python
sx, sy = editor.get_scale(uuid)
```

#### `editor.set_scale(uuid, x, y)` → `bool`

Sets entity scale.

```python
editor.set_scale(uuid, 2.0, 2.0)  # Scale up 2x
editor.set_scale(uuid, -1, 1)     # Flip horizontally
```

#### `editor.get_rotation(uuid)` → `float`

Returns entity rotation in **degrees**.

```python
angle = editor.get_rotation(uuid)
```

#### `editor.set_rotation(uuid, rotation)` → `bool`

Sets rotation in degrees.

```python
editor.set_rotation(uuid, 45.0)   # Rotate 45°
editor.set_rotation(uuid, 0.0)    # Reset rotation
```

---

### 5.4 Components

#### `editor.has_component(uuid, component_type)` → `bool`

Checks if the entity has a component of the given type.

```python
if editor.has_component(uuid, 'Rigidbody'):
    print('Has physics body')
```

**Supported types**: `Transform`, `SpriteRenderer`, `Camera`, `Rigidbody`, `Collider`, `Flipbook`, `Audio`, `Animator`, `Skeleton`, `Tilemap`, `FX`, `Widget`, `PointLight`, `SpotLight`, `PointMarker`, `Script`, `AI`, `Joint`, `Destructible`, `ClassComponent`, `Stencil`, `GameplayTag`, `Interface`, `Replication`, `Hierarchy`

> ℹ️ `PointLight` and `SpotLight` are two faces of one underlying light component, so an entity carrying either will report `True` for `has_component` on both. Use `get_instance_count(uuid, 'PointLight')` / `get_instance_count(uuid, 'SpotLight')` to tell which kind of lights it actually holds.

#### `editor.add_component(uuid, component_type)` → `bool`

Adds a component to the entity. Returns `False` if already present.

```python
editor.add_component(uuid, 'Rigidbody')
editor.add_component(uuid, 'Collider')
editor.add_component(uuid, 'SpriteRenderer')
```

#### `editor.remove_component(uuid, component_type)` → `bool`

Removes a component from the entity.

```python
editor.remove_component(uuid, 'Rigidbody')
```

#### Add / remove support matrix

`get_component_types()` returns all 25 types the API understands, but not every type can be created or deleted:

| Type | `add_component` | `remove_component` | Why |
|------|:---:|:---:|-----|
| `Transform` | ❌ | ❌ | Every entity always has one |
| `Hierarchy` | ❌ | ❌ | Created and destroyed by `set_parent` / `clear_parent` |
| `Stencil` | ✅ | ❌ | Treated as a core component once added |
| `Replication` | ✅ | ❌ | Treated as a core component once added |
| all other 21 types | ✅ | ✅ | |

`add_component` returns `False` if the component is already present, or if the type cannot be added.
`remove_component` returns `False` for the blocked types above and logs a warning.

> ℹ️ `Tag` and `ID` are internal identity components. They are not in `get_component_types()` and cannot be touched from Python at all.

#### `editor.get_component(uuid, component_type)` → `dict`

Returns component data as a Python dictionary.

**Example for Transform:**
```python
data = editor.get_component(uuid, 'Transform')
# {
#     'position': (100.0, 200.0, 0.0),
#     'scale': (1.0, 1.0),
#     'rotation': 0.0,
#     'enabled': True,          # entity is active at all
#     'visible': True,          # drawn in the editor viewport
#     'render_in_game': True    # drawn in play mode / the shipped game
# }
```

> All six fields are writable. `enabled` is the master switch for the whole entity; `visible` and `render_in_game` control editor and game rendering independently.

```python
# Hide an entity in the editor but keep it in the game
editor.set_component(uuid, 'Transform', {'visible': False, 'render_in_game': True})
```

**Example for Camera:**
```python
cam = editor.get_component(uuid, 'Camera')
# {
#     'primary': True,
#     'ortho_width': 1920.0,
#     'offset': (0.0, 0.0),
#     'background_color': (0.1, 0.1, 0.15),
#     'near_plane': -1.0,
#     'far_plane': 1.0,
#     'player_index': -1,              # -1 = all players, 0..3 = split-screen slot
#     'viewport_rect': (0.0, 0.0, 1.0, 1.0),   # x, y, width, height in 0..1
#     'shake_intensity': 0.0,
#     'shake_duration': 0.0
# }
```

> All of the above are writable. Writing `shake_intensity` + `shake_duration` arms a camera shake; `player_index` is clamped to `-1..3`.

**Example for Rigidbody:**
```python
rb = editor.get_component(uuid, 'Rigidbody')
# {
#     'body_type': 0,         # 0 = Static, 1 = Dynamic, 2 = Kinematic
#     'fixed_rotation': False,
#     'gravity_scale': 1.0,
#     'linear_damping': 0.0,
#     'angular_damping': 0.01,
#     'is_bullet': False,
#     'allow_sleep': True,
#     'ragdoll_enabled': False,
#     'ragdoll_gravity_scale': 1.0,
#     'ragdoll_angular_damping': 0.5
# }
```

**Example for Collider:**
```python
col = editor.get_component(uuid, 'Collider')
# {
#     'box_count': 1,
#     'sphere_count': 0,
#     'capsule_count': 1,
#     'boxes':    [ { ...shared fields..., 'size': (50.0, 50.0) } ],
#     'spheres':  [ ],
#     'capsules': [ { ...shared fields..., 'radius': 10.0, 'length': 40.0 } ]
# }
```

Every entry in `boxes` / `spheres` / `capsules` carries the same **shared field set**, plus its shape-specific size fields:

| Field | Type | Description |
|-------|------|-------------|
| `name` | `str` | Collider name |
| `density` | `float` | Mass density |
| `friction` | `float` | Surface friction |
| `restitution` | `float` | Bounciness |
| `is_sensor` | `bool` | Sensor (no collision response, still reports overlap) |
| `is_one_way` | `bool` | One-way platform behaviour |
| `one_way_dir` | `int` | `0` None, `1` Up, `2` Down, `3` Left, `4` Right |
| `enable_contact_events` | `bool` | Report begin/end contact |
| `enable_sensor_events` | `bool` | Report sensor overlap |
| `enable_hit_events` | `bool` | Report hard hits |
| `enable_presolve_events` | `bool` | Report pre-solve callbacks |
| `cast_shadow` | `bool` | Collider casts a shadow |
| `shadow_origin` | `int` | Shadow origin mode |
| `shadow_edge_fade` | `float` | Shadow edge fade |
| `shadow_z_order` | `float` | Shadow Z order |
| `dont_block_shadows` | `bool` | Do not occlude other shadows |
| `collision_group` | `int` | Collision group index |
| `collision_enabled` | `int` | Collision mask bits, `0..3` |
| `separate_body` | `bool` | Give this collider its own physics body |
| `position` | `(x, y, z)` | Local offset from the entity |
| `scale` | `(x, y)` | Local scale |
| `rotation` | `float` | Local rotation in degrees |
| `size` | `(w, h)` | **box only** |
| `radius` | `float` | **sphere and capsule only** |
| `length` | `float` | **capsule only** |

```python
# Read one box collider in full
box = editor.get_component(uuid, 'Collider')['boxes'][0]
print(box['is_one_way'], box['collision_group'])
```

> ℹ️ `set_component(uuid, 'Collider', {...})` edits colliders **already present**, matching list entries by index — it never creates or deletes them. To change the collider count use `add_collider` / `remove_collider` (see [5.4.1](#541-individual-colliders)).

**Example for SpriteRenderer:**
```python
sr = editor.get_component(uuid, 'SpriteRenderer')
# {
#     'instance_count': 1,
#     'sprite_path': 'Content/Textures/player.png',
#     'attach_to_collider': '',   # name of the collider this sprite follows ('' = none)
#     'attach_to_socket': '',     # name of the socket this sprite follows ('' = none)
#     'color': (1.0, 1.0, 1.0, 1.0),
#     'flip_x': False,
#     'flip_y': False,
#     'visible': True,
#     'render_in_game': True
# }
```

**Example for Audio:**
```python
audio = editor.get_component(uuid, 'Audio')
# {
#     'instance_count': 1,
#     'sound_path': 'Content/Sounds/jump.ice_sound',
#     'group': 0,
#     'volume': 1.0,
#     'pitch': 1.0,
#     'loop': False,
#     'play_on_wake': False,
#     'spatial': False,
#     'min_distance': 100.0,
#     'max_distance': 1000.0
# }
```

**Example for Flipbook:**
```python
fb = editor.get_component(uuid, 'Flipbook')
# {
#     'instance_count': 1,
#     'flipbook_path': 'Content/Flipbooks/run.ice_flipbook',
#     'playing': True,
#     'speed': 1.0,
#     'color': (1.0, 1.0, 1.0, 1.0),
#     'flip_x': False,
#     'flip_y': False,
#     'visible': True
# }
```

**Example for Animator:**
```python
anim = editor.get_component(uuid, 'Animator')
# {
#     'animation_path': 'Content/Animations/player.ice_animation',
#     'target_sprite_name': '',     # which SpriteRenderer instance the animator drives
#     'current_state': 'Idle',
#     'state_time': 0.5,
#     'current_frame': 3,
#     'is_transitioning': False
# }
```

> ℹ️ `set_component` accepts `animation_path` and `target_sprite_name` for `Animator` — `current_state`, `state_time`, `current_frame`, and `is_transitioning` are runtime playback state and are read-only.

**Example for Skeleton:**
```python
sk = editor.get_component(uuid, 'Skeleton')
# {
#     'skeleton_path': 'Content/Skeletons/hero.ice_skeleton',
#     'current_animation': 'run',
#     'current_skin': 'default',
#     'playing': True,
#     'loop': True,
#     'speed': 1.0,
#     'color': (1.0, 1.0, 1.0, 1.0),
#     'flip_x': False,
#     'flip_y': False,
#     'visible': True,
#     'render_in_game': True,
#     'cast_shadow': False,
#     'dont_block_shadows': False,
#     'cast_shadow_mode': 0,          # 0 Colliders, 1 Contour
#     'shadow_origin': 0,             # 0 Bottom, 1 Center, 2 Top
#     'shadow_edge_fade': 0.0,
#     'shadow_z_order': 0.0,
#     'shading_mode': 1,              # 0 Lit, 1 Unlit
#     'blend_mode': 0,                # 0 Masked, 1 Additive, 2 Translucent, 3 Opaque
#     'alpha_clip_threshold': 0.5,
#     'material_path': '',
#     'ragdoll_enabled': False,
#     'ragdoll_auto_on_start': False,
#     'ragdoll_angular_damping': 0.5,
#     'ragdoll_gravity_scale': 1.0,
#     'bone_colliders_enabled': True
# }
```

> Every Skeleton field above is writable via `set_component`. `cast_shadow_mode` and `shading_mode` are clamped to `0..1`, `shadow_origin` to `0..2`, `blend_mode` to `0..3`.

**Example for PointLight:**
```python
light = editor.get_component(uuid, 'PointLight')
# {
#     'instance_count': 1,
#     'color': (1.0, 0.9, 0.7),
#     'intensity': 1.0,
#     'radius': 300.0,
#     'falloff': 2.0,
#     'cast_shadows': True,
#     'enabled': True
# }
```

**Example for SpotLight:**
```python
spot = editor.get_component(uuid, 'SpotLight')
# {
#     'instance_count': 1,
#     'color': (1.0, 1.0, 1.0),
#     'intensity': 1.0,
#     'radius': 500.0,
#     'falloff': 2.0,
#     'direction': (0.0, -1.0),
#     'inner_angle': 15.0,
#     'outer_angle': 30.0,
#     'cast_shadows': True,
#     'enabled': True
# }
```

**Example for Script:**
```python
sc = editor.get_component(uuid, 'Script')
# {
#     'file_name': 'PlayerScript',
#     'class_path': 'Content/Scripts/PlayerScript.lua',
#     'override_class_defaults': False,
#     'override_lua_script': False,
#     'lua_script_override': '',
#     'visual_graph_override': ''
# }
```

> All six fields are writable.

**Example for AI:**
```python
ai = editor.get_component(uuid, 'AI')
# {
#     'ai_asset_path': 'Content/AI/enemy_ai.ice_ai',
#     'move_speed': 100.0,
#     'detection_radius': 200.0,
#     'attack_radius': 50.0,
#     'movement_mode': 0,              # 0 = Auto, 1 = Transform, 2 = Physics
#     'physics_arrival_radius': 6.0,
#     'perception_enabled': False,
#     'perception_sight_radius': 300.0,
#     'perception_sight_angle': 120.0,
#     'perception_hearing_radius': 500.0,
#     'perception_require_los': False,
#     'perception_forget_time': 5.0,
#     'perception_awareness_radius': 0.0,
#     'perception_use_facing_x': False,
#     'facing_x': 1.0,
#     'perception_target_tags': ['Player'],
#     'patrol_points': [(100.0, 200.0), (300.0, 200.0)],
#     'current_patrol_index': 0
# }
```

Everything except `current_patrol_index` is writable:

```python
editor.set_component(uuid, 'AI', {
    'move_speed': 180.0,
    'movement_mode': 2,
    'perception_enabled': True,
    'perception_require_los': True,
    'perception_target_tags': ['Player', 'Ally'],
    'patrol_points': [(0, 0), (400, 0), (400, 300)]
})
```

> Writing `patrol_points` replaces the whole route and resets `current_patrol_index` to `0`. Writing `perception_target_tags` replaces the whole tag list. `movement_mode` is clamped to `0..2`.

**Example for FX:**
```python
fx = editor.get_component(uuid, 'FX')
# {
#     'instance_count': 1,
#     'fx_path': 'Content/FX/explosion.ice_fx',
#     'playing': False,
#     'loop': False,
#     'speed': 1.0,
#     'play_on_wake': False,
#     'visible': True
# }
```

**Example for Widget:**
```python
widget = editor.get_component(uuid, 'Widget')
# {
#     'instance_count': 1,
#     'widget_path': 'Content/UI/health_bar.ice_widget',
#     'visible': True,
#     'render_in_game': True,
#     'screen_space': True,
#     'scale': 1.0,
#     'render_order': 0,
#     'flip_x': False,
#     'flip_y': False,
#     'interactable': True,
#     'player_index': -1    # -1 = all players, 0..3 = specific player
# }
```

**Example for Tilemap:**
```python
tm = editor.get_component(uuid, 'Tilemap')
# {
#     'instance_count': 1,
#     'tilemap_path': 'Content/Maps/forest.ice_tm',
#     'visible': True,
#     'flip_x': False,
#     'flip_y': False
# }
```

**Example for PointMarker:**
```python
pm = editor.get_component(uuid, 'PointMarker')
# {
#     'instance_count': 1,
#     'color': (1.0, 0.0, 0.0),
#     'shape': 0,    # 0 = Circle, 1 = Square, ...
#     'size': 10.0,
#     'visible': True
# }
```

**Example for Joint:**
```python
joint = editor.get_component(uuid, 'Joint')
# {
#     'instance_count': 1,
#     'joint_type': 0,
#     'target_entity_tag': 'Ground',
#     'target_entity_uuid': 987654321,
#     'target_part_name': '',
#     'anchor_a': (0.0, 0.0),
#     'anchor_b': (0.0, 0.0),
#     'collide_connected': False,
#     'enable_spring': False,
#     'hertz': 1.0,
#     'damping_ratio': 0.7,
#     'enable_limit': False,
#     'enable_motor': False,
#     'motor_speed': 0.0,
#     'max_motor_torque': 0.0
# }
```

> ℹ️ Limits and break thresholds (`lower_limit`, `upper_limit`, `max_motor_force`, `break_force`, `break_torque`) are only available through `get_instance` / `set_instance` — see [5.13](#513-multi-instance-components).

**Example for Destructible:**
```python
d = editor.get_component(uuid, 'Destructible')
# {
#     'enabled': True,
#     'destruct_on_start': False,
#     'health': 100.0,
#     'fragment_count': 8,
#     'pattern': 0,
#     'explosion_force': 200.0,
#     'impact_threshold': 5.0,
#     'fragment_lifetime': 5.0,
#     'fragment_fade_time': 1.0,
#     'fragment_gravity_scale': 1.0,
#     'fragment_density': 1.0,
#     'fragment_friction': 0.3,
#     'fragment_restitution': 0.0,
#     'fragment_is_sensor': False,
#     'fragment_enable_contact_events': False,
#     'fragment_enable_sensor_events': False,
#     'fragment_enable_hit_events': False,
#     'fragment_enable_presolve_events': False,
#     'fragment_collision_group': 0,
#     'max_debris_count': 100,
#     'fragment_cast_shadow': False,
#     'fragment_dont_block_shadows': False,
#     'fragment_shadow_origin': 0,
#     'fragment_shadow_edge_fade': 0.0,
#     'fragment_shadow_z_order': 0.0,
#     'destroy_original': True
# }
```

> Every Destructible field above is writable via `set_component`.

**Example for ClassComponent:**
```python
cc = editor.get_component(uuid, 'ClassComponent')
# {
#     'instance_count': 2,
#     'class_path': 'Content/Classes/Turret.ice_class'    # first instance only
# }
```

> Use `get_instance` / `set_instance` to reach the other nested class instances and their local transforms.

**Example for Stencil:**
```python
st = editor.get_component(uuid, 'Stencil')
# {
#     'enabled': True,
#     'mode': 0,           # 0..2
#     'stencil_id': 1,     # 1..255
#     'compare_func': 0    # 0..1
# }
```

> On write, `mode`, `stencil_id`, and `compare_func` are clamped to the ranges above.

**Example for GameplayTag:**
```python
gt = editor.get_component(uuid, 'GameplayTag')
# {'tags': ['Enemy.Boss', 'Faction.Undead']}

# Writing replaces the whole set
editor.set_component(uuid, 'GameplayTag', {'tags': ['Enemy.Elite']})
```

**Example for Interface:**
```python
ic = editor.get_component(uuid, 'Interface')
# {'interfaces': ['IDamageable', 'IInteractable']}

# Writing replaces the whole set
editor.set_component(uuid, 'Interface', {'interfaces': ['IDamageable']})
```

**Example for Replication (network):**
```python
rep = editor.get_component(uuid, 'Replication')
# {
#     'replicate': False,
#     'owner_mode': 0,                 # 0 = Server, 1 = Player
#     'owner_player_id': 0,
#     'sync_transform': True,
#     'sync_velocity': True,
#     'sync_visuals': True,
#     'sync_full_state': True,
#     'full_state_rate_hz': 0.0,       # 0 = use the server default
#     'script_mode': 0,                # 0 = Auto, 1 = AlwaysRun, 2 = NeverRun
#     'relevancy': 0,                  # 0 = AreaOfInterest, 1 = AlwaysRelevant
#     'make_kinematic_on_clients': True,
#     'runtime_prefab_path': '',
#     'is_default': True               # read-only: True while every field is at its default
# }

# Make an entity a player-owned replicated actor
editor.add_component(uuid, 'Replication')
editor.set_component(uuid, 'Replication', {
    'replicate': True,
    'owner_mode': 1,
    'owner_player_id': 0,
    'relevancy': 1
})
```

> Every field except `is_default` is writable. `owner_mode` is clamped to `0..1`, `script_mode` to `0..2`, `relevancy` to `0..1`. Like `Stencil`, `Replication` cannot be removed with `remove_component`.

**Example for Hierarchy:**
```python
h = editor.get_component(uuid, 'Hierarchy')
# {
#     'parent_uuid': 123456789,        # 0 if the entity has no parent
#     'children': [111, 222, 333],
#     'local_position': (10.0, 0.0, 0.0),
#     'local_scale': (1.0, 1.0),
#     'local_rotation': 0.0
# }

# The local transform is writable
editor.set_component(uuid, 'Hierarchy', {
    'local_position': (0.0, 40.0, 0.0),
    'local_rotation': 15.0
})
```

> ⚠️ `parent_uuid` and `children` are **read-only** — change the links with `set_parent` / `clear_parent` (see [5.6](#56-folders)). Only `local_position`, `local_scale`, and `local_rotation` can be written.

`get_instance(uuid, type, index)` — see section [5.13](#513-multi-instance-components).

#### `editor.set_component(uuid, component_type, data)` → `bool`

Sets component data from a Python dictionary. Pass **only** fields you want to change — others remain unchanged.

```python
# Change the camera's orthographic width only
editor.set_component(uuid, 'Camera', {'ortho_width': 1920.0})

# Configure rigidbody
editor.set_component(uuid, 'Rigidbody', {
    'body_type': 1,           # Dynamic
    'gravity_scale': 1.5,
    'fixed_rotation': True
})

# Change sprite and color
editor.set_component(uuid, 'SpriteRenderer', {
    'sprite_path': 'Content/Textures/enemy.png',
    'color': (1.0, 0.0, 0.0, 1.0),  # Red
    'flip_x': True
})

# Configure point light
editor.set_component(uuid, 'PointLight', {
    'color': (1.0, 0.8, 0.3),
    'intensity': 2.0,
    'radius': 500.0
})

# Configure audio
editor.set_component(uuid, 'Audio', {
    'sound_path': 'Content/Sounds/bgm.ice_sound',
    'volume': 0.7,
    'loop': True,
    'play_on_wake': True
})

# Configure collider — change properties of existing box colliders
editor.set_component(uuid, 'Collider', {
    'boxes': [
        {'size': (100, 50), 'density': 2.0, 'friction': 0.5}
    ]
})
```

#### `editor.find_entities_by_component(component_type)` → `list[int]`

Finds UUIDs of all entities that have the specified component.

```python
# Find all entities with a physics body
physics_entities = editor.find_entities_by_component('Rigidbody')
print(f'Physics objects: {len(physics_entities)}')

# Find all cameras
cameras = editor.find_entities_by_component('Camera')

# Find all lights
lights = editor.find_entities_by_component('PointLight')
```

---

### 5.4.1 Individual colliders

`get_component` / `set_component` treat the `Collider` component as one blob and cannot change how many colliders an entity has. These five functions work on a single collider at a time and can add and remove them.

The `shape` argument is `'box'`, `'sphere'`, or `'capsule'` (case-insensitive). An unknown shape logs a warning and returns `0` / `{}` / `False`.

#### `editor.get_collider_count(uuid, shape)` → `int`

Number of colliders of that shape on the entity. `0` if the entity has no `Collider` component.

```python
print(editor.get_collider_count(uuid, 'box'))       # 2
print(editor.get_collider_count(uuid, 'capsule'))   # 1
```

#### `editor.get_collider(uuid, shape, index)` → `dict`

One collider's full field set — the shared fields listed in [5.4](#54-components) plus its shape-specific size fields. Returns an empty dict if the index is out of range.

```python
for i in range(editor.get_collider_count(uuid, 'box')):
    box = editor.get_collider(uuid, 'box', i)
    print(box['name'], box['size'], box['is_sensor'])
```

#### `editor.set_collider(uuid, shape, index, data)` → `bool`

Changes one collider. Pass only the fields you want to change.

```python
# Turn the second box into a one-way platform facing up
editor.set_collider(uuid, 'box', 1, {
    'is_one_way': True,
    'one_way_dir': 1,
    'is_sensor': False
})

# Resize and offset a capsule
editor.set_collider(uuid, 'capsule', 0, {
    'radius': 12.0,
    'length': 48.0,
    'position': (0.0, 24.0, 0.0),
    'rotation': 0.0
})
```

#### `editor.add_collider(uuid, shape, name='')` → `bool`

Appends a new collider of that shape. **Creates the `Collider` component if the entity does not have one.** With no name, the engine default is used (`Box Collider`, `Sphere Collider`, `Capsule Collider`).

```python
editor.add_collider(uuid, 'box', 'Feet')
editor.add_collider(uuid, 'capsule', 'Body')
editor.add_collider(uuid, 'sphere')
```

#### `editor.remove_collider(uuid, shape, index)` → `bool`

Removes one collider by index and destroys its runtime physics shape.

```python
editor.remove_collider(uuid, 'box', 0)

# Clear every sphere collider
while editor.get_collider_count(uuid, 'sphere') > 0:
    editor.remove_collider(uuid, 'sphere', 0)
```

> ✅ All five record Undo state.

---

### 5.5 Sprites (Sprite)

Convenient short functions for working with sprites (first SpriteRenderer instance).

#### `editor.set_sprite(uuid, sprite_path)` → `bool`

Sets sprite texture path. If SpriteRenderer is missing — it is created automatically.

```python
editor.set_sprite(uuid, 'Content/Textures/player.png')
```

#### `editor.get_sprite(uuid)` → `str`

Returns sprite texture path.

```python
path = editor.get_sprite(uuid)
```

#### `editor.set_sprite_color(uuid, r, g, b)` → `bool`

Sets sprite tint color (RGB, 0.0–1.0).

```python
editor.set_sprite_color(uuid, 1.0, 0.0, 0.0)  # Red
editor.set_sprite_color(uuid, 1.0, 1.0, 1.0)  # White (no tint)
```

#### `editor.set_sprite_alpha(uuid, alpha)` → `bool`

Sets sprite transparency (0.0 — fully transparent, 1.0 — opaque).

```python
editor.set_sprite_alpha(uuid, 0.5)  # Semi-transparent
```

---

### 5.6 Folders

Folders are used in the Level Outliner to organize entities into groups.

#### `editor.get_folders()` → `list[str]`

Returns list of all folder names.

```python
folders = editor.get_folders()
# ['Characters', 'Environment', 'UI']
```

#### `editor.create_folder(name)` → `bool`

Creates a new folder in the Level Outliner.

```python
editor.create_folder('Enemies')
```

#### `editor.delete_folder(name)` → `bool`

Deletes a folder.

```python
editor.delete_folder('OldFolder')
```

#### `editor.rename_folder(old_name, new_name)` → `bool`

Renames a folder.

```python
editor.rename_folder('Mobs', 'Enemies')
```

#### `editor.move_to_folder(uuid, folder_name)` → `bool`

Moves an entity to the specified folder.

```python
editor.move_to_folder(uuid, 'Characters')
```

#### `editor.move_to_root(uuid)` → `bool`

Moves an entity from a folder to the root.

```python
editor.move_to_root(uuid)
```

#### `editor.get_entity_folder(uuid)` → `str`

Returns the folder name that the entity is in. Empty string if in root.

```python
folder = editor.get_entity_folder(uuid)
```

#### `editor.get_entities_in_folder(folder_name)` → `list[int]`

Returns UUIDs of all entities in the specified folder.

```python
chars = editor.get_entities_in_folder('Characters')
```

#### `editor.get_root_entities()` → `list[int]`

Returns UUIDs of all entities in root (not in any folder).

```python
root = editor.get_root_entities()
```

#### Transform parenting

Attach/detach entities in the transform hierarchy (separate from outliner folders).

#### `editor.set_parent(child_uuid, parent_uuid)` → `bool`

Makes `child_uuid` a child of `parent_uuid`. Returns `False` and does nothing if either UUID is invalid, if they are the same, or if it would create a cycle. Creates a `Hierarchy` component on both entities as needed.

```python
sword = editor.find_entity('Sword')
hero = editor.find_entity('Hero')
editor.set_parent(sword, hero)
```

#### `editor.clear_parent(uuid)` → `bool`

Detaches `uuid` from its parent. Returns `True` if it had one.

```python
editor.clear_parent(sword)
```

#### `editor.get_parent(uuid)` → `int`

Returns the parent UUID, or `0` if the entity has no parent.

#### `editor.get_children(uuid)` → `list[int]`

Returns the UUIDs of the direct children.

```python
for child in editor.get_children(hero):
    print(editor.get_entity_name(child))
```

> ℹ️ You can also read the full link via `editor.get_component(uuid, 'Hierarchy')` → `{'parent_uuid', 'children', 'local_position', 'local_scale', 'local_rotation'}`.

---

### 5.7 Batch operations

Batch operations apply an action to many entities in one call. Return the number of successfully processed entities (except `batch_duplicate`).

#### `editor.batch_set_position(uuids, x, y, z=0)` → `int`

Sets the same position for all entities.

```python
uuids = editor.get_entity_uuids()
count = editor.batch_set_position(uuids, 0, 0)
print(f'Moved: {count}')
```

#### `editor.batch_translate(uuids, dx, dy, dz=0)` → `int`

Moves all entities by the specified delta.

```python
enemies = editor.find_entities('^Enemy')
editor.batch_translate(enemies, 100, 0)  # Move all enemies right
```

#### `editor.batch_delete(uuids)` → `int`

Deletes multiple entities.

```python
temp = editor.find_entities('^Temp')
deleted = editor.batch_delete(temp)
print(f'Deleted: {deleted}')
```

#### `editor.batch_scale(uuids, sx, sy)` → `int`

Sets scale for multiple entities.

```python
all_uuids = editor.get_entity_uuids()
editor.batch_scale(all_uuids, 2.0, 2.0)  # Scale up 2x
```

#### `editor.batch_rotate(uuids, rotation)` → `int`

Sets rotation for multiple entities (degrees).

```python
editor.batch_rotate(uuids, 45.0)
```

#### `editor.batch_move_to_folder(uuids, folder_name)` → `int`

Moves multiple entities to a folder.

```python
enemies = editor.find_entities('^Enemy')
editor.batch_move_to_folder(enemies, 'Enemies')
```

#### `editor.batch_duplicate(uuids)` → `list[int]`

Duplicates multiple entities. Returns list of UUIDs of copies.

```python
originals = editor.get_selected_uuids()
copies = editor.batch_duplicate(originals)
print(f'Created {len(copies)} copies')
```

#### `editor.batch_add_component(uuids, component_type)` → `int`

Adds a component to multiple entities.

```python
all_uuids = editor.get_entity_uuids()
editor.batch_add_component(all_uuids, 'Rigidbody')
```

#### `editor.batch_remove_component(uuids, component_type)` → `int`

Removes a component from multiple entities.

```python
editor.batch_remove_component(uuids, 'Collider')
```

#### `editor.batch_set_sprite(uuids, sprite_path)` → `int`

Sets sprite for multiple entities.

```python
trees = editor.find_entities('^Tree')
editor.batch_set_sprite(trees, 'Content/Textures/oak.png')
```

#### `editor.batch_set_sprite_color(uuids, r, g, b)` → `int`

Sets sprite color for multiple entities.

```python
editor.batch_set_sprite_color(uuids, 0.5, 0.5, 0.5)  # Darken
```

#### `editor.batch_rename(uuids, base_name)` → `int`

Renames entities with automatic numbering: `base_name_0`, `base_name_1`, ...

```python
trees = editor.find_entities('^Tree')
editor.batch_rename(trees, 'Tree')
# Result: Tree_0, Tree_1, Tree_2, ...
```

---

### 5.8 Camera & viewport

#### `editor.get_camera_position()` → `tuple(x, y)`

Returns editor camera position.

```python
x, y = editor.get_camera_position()
```

#### `editor.get_camera_zoom()` → `float`

Returns current camera zoom.

```python
zoom = editor.get_camera_zoom()
```

#### `editor.set_camera(x, y, zoom=-1)`

Sets editor camera position and zoom. If `zoom` is not specified or ≤ 0, zoom is unchanged.

```python
editor.set_camera(0, 0, 1.0)       # Center, default zoom
editor.set_camera(500, 300)        # Move camera, keep zoom
editor.set_camera(0, 0, 0.5)       # Zoom out
```

#### `editor.focus_entity(uuid)`

Focuses editor camera on the specified entity position.

```python
uuid = editor.find_entity('Player')
editor.focus_entity(uuid)
```

#### `editor.zoom_to_fit()`

Automatically adjusts camera to show all entities in the scene.

```python
editor.zoom_to_fit()
```

#### `editor.zoom_to_selection()`

Adjusts camera so all selected entities are visible.

```python
editor.select_entities(some_uuids)
editor.zoom_to_selection()
```

#### `editor.get_viewport_size()` → `tuple(width, height)`

Returns viewport size in pixels.

```python
w, h = editor.get_viewport_size()
print(f'Viewport: {w}x{h}')
```

#### `editor.screen_to_world(screen_x, screen_y)` → `tuple(x, y)`

Converts screen coordinates to world coordinates.

```python
world_x, world_y = editor.screen_to_world(400, 300)
```

#### `editor.is_viewport_focused()` → `bool`

Checks if the viewport is focused.

#### `editor.is_viewport_hovered()` → `bool`

Checks if the mouse is over the viewport.

---

### 5.9 Editor panels

#### `editor.get_panel_names()` → `list[str]`

Returns list of all available panel names.

```python
panels = editor.get_panel_names()
for p in panels:
    print(f'{p}: {"✅" if editor.is_panel_visible(p) else "❌"}')
```

**Dockable panels** — can be shown and hidden freely:

`Hierarchy`, `Properties`, `Stats`, `ContentBrowser`, `Settings`, `NetworkPanel`, `Profiler`, `WorldSettings`, `HotKeys`, `Documentation`, `LuaDebugger`, `Plugins`, `Console`, `PythonConsole`, `About`, `PropertyMatrix`, `LevelScriptEditor`, `TextNoteEditor`, `RemotePreview`

**One-shot dialog triggers** — setting them to `True` opens a modal; they reset themselves to `False` immediately, so `is_panel_visible` on them is almost always `False`:

`BuildGame`, `DLCPackager`

**Asset editor panels** — opened by `open_asset`, and can only be *closed* by name:

`TilesetEditor`, `TilemapEditor`, `ClassEditor`, `TextureSettings`, `SpriteEditor`, `FlipbookEditor`, `SkeletonEditor`, `SoundSettings`, `SpritesheetSlicer`, `AnimationEditor`, `MaterialEditor`, `MaterialInstanceEditor`, `MaterialFunctionEditor`, `MPCEditor`, `FXEditor`, `FontEditor`, `WidgetEditor`, `ViewEditor`, `CinemaEditor`, `Localization`, `AIEditor`, `VideoPlayer`

#### `editor.set_panel_visible(panel_name, visible)`

Shows or hides a panel.

```python
editor.set_panel_visible('Console', True)
editor.set_panel_visible('Properties', False)
```

> ⚠️ For **asset editor panels** only `visible=False` works — it closes every open instance of that panel. `visible=True` cannot work, because an asset panel needs a file to open: use `editor.open_asset(path)` instead. An unknown panel name is logged as a warning and otherwise ignored.

#### `editor.is_panel_visible(panel_name)` → `bool`

Checks if a panel is visible. For asset editor panels this returns `True` while at least one instance of that panel is open.

#### `editor.open_asset(asset_path)` → `bool`

Opens an asset file in the corresponding editor (Class Editor, Sprite Editor, etc.)

```python
editor.open_asset('Content/Classes/Player.ice_class')
editor.open_asset('Content/Sprites/hero.ice_sprite')
```

#### `editor.save_panel(panel_name)` → `bool`

Saves the currently open asset in the panel. If several instances of that panel are open, every dirty one is saved.

```python
editor.save_panel('ClassEditor')
```

> Only asset editor panels can be saved. `SpritesheetSlicer` and `VideoPlayer` have nothing to save, so they return `False`, as does any name that is not an asset panel.

#### `editor.close_panel(panel_name)` → `bool`

Saves and closes the panel.

```python
editor.close_panel('SpriteEditor')
```

#### `editor.is_panel_dirty(panel_name)` → `bool`

Checks if the panel has unsaved changes.

```python
if editor.is_panel_dirty('FXEditor'):
    editor.save_panel('FXEditor')
```

#### `editor.save_all_panels()`

Saves all panels with unsaved changes.

```python
editor.save_all_panels()
```

#### `editor.refresh_browser()`

Refreshes the content browser (usually updates automatically).

---

### 5.10 Play mode

#### `editor.is_play_mode()` → `bool`

Checks if play mode is running.

#### `editor.is_paused()` → `bool`

Checks if the game is paused.

#### `editor.play()`

Starts play mode.

```python
if not editor.is_play_mode():
    editor.play()
```

#### `editor.stop()`

Stops play mode.

#### `editor.pause()`

Pauses the game.

#### `editor.resume()`

Resumes the game.

```python
# Example of automated testing
editor.play()
editor.set_timer(5.0, lambda: editor.stop())  # Stop after 5 seconds
```

---

### 5.11 Undo / Redo

#### `editor.undo()`

Undo last action.

```python
editor.undo()
```

#### `editor.redo()`

Redo last undone action.

```python
editor.redo()
```

> ℹ️ Most API functions (create, delete, move, modify components) **automatically** record state for Undo. No need to call undo before each operation.

#### `editor.begin_undo()` / `editor.end_undo()`

Group many edits into a **single** undo step (an editor transaction). Every console command, script run, tool script, timer callback, and event callback is **already wrapped automatically**, so a whole script collapses into one `Ctrl+Z` — you rarely need these directly. Use them only for manual fine-grained control, and always pair them (calls may nest):

```python
editor.begin_undo()
try:
    for uuid in editor.get_entity_uuids():
        editor.translate(uuid, 100, 0)   # 1000 moves...
finally:
    editor.end_undo()                    # ...become ONE undo step
```

> ℹ️ Without grouping, each individual edit records its own undo snapshot. Because the undo history is capped, a large batch could otherwise evict your earlier history — transactions prevent that and are far faster on big scenes.

---

### 5.12 Clipboard

#### `editor.copy_entities(uuids)` → `bool`

Copies entities to the clipboard.

```python
editor.copy_entities([uuid1, uuid2])
```

#### `editor.cut_entities(uuids)` → `bool`

Cuts entities to the clipboard (removed on paste).

```python
editor.cut_entities([uuid1])
```

#### `editor.paste_entities()` → `list[int]`

Pastes entities from clipboard. Returns UUIDs of new entities. Copies are offset 50 pixels.

```python
new_uuids = editor.paste_entities()
```

#### `editor.has_clipboard()` → `bool`

Checks if clipboard has entities.

#### `editor.copy_selected()` → `bool`

Copies current selection to clipboard.

#### `editor.cut_selected()` → `bool`

Cuts current selection to clipboard.

---

### 5.13 Multi-instance components

Some components support multiple instances on one entity. For example, SpriteRenderer can have multiple sprite layers, PointLight — multiple light sources.

**Multi-instance components**: `SpriteRenderer`, `Flipbook`, `Audio`, `FX`, `Widget`, `PointLight`, `SpotLight`, `PointMarker`, `Tilemap`, `Joint`, `ClassComponent`

#### `editor.get_instance_count(uuid, component_type)` → `int`

Returns the number of component instances.

```python
count = editor.get_instance_count(uuid, 'SpriteRenderer')
print(f'Sprites: {count}')
```

#### `editor.get_instance(uuid, component_type, index)` → `dict`

Returns data of a specific instance. Unlike `get_component`, returns **full information** including name, local position, scale, and rotation.

**SpriteRenderer:**
```python
sprite_data = editor.get_instance(uuid, 'SpriteRenderer', 1)
# {
#     'name': 'Shadow',
#     'sprite_path': 'Content/Textures/shadow.png',
#     'attach_to_collider': '',
#     'attach_to_socket': '',           # socket name this instance follows ('' = none)
#     'attach_socket_source': 0,        # 0=Auto, 1=Sprite, 2=Flipbook, 3=Skeleton
#     'attach_socket_source_index': -1, # source instance index, -1 = any
#     'attach_socket_inherit_flip_x': True,
#     'attach_socket_offset': (0.0, 0.0),
#     'attach_socket_offset_rotation': 0.0,
#     'color': (0.0, 0.0, 0.0, 0.5),
#     'flip_x': False,
#     'flip_y': False,
#     'visible': True,
#     'render_in_game': True,
#     'cast_shadow': False,
#     'cast_shadow_mode': 0,
#     'shadow_origin': 0,
#     'shadow_edge_fade': 0.0,
#     'shadow_z_order': 0.0,
#     'dont_block_shadows': True,
#     'vertex_effects': {...},          # see "Vertex effects" below
#     'position': (0.0, -10.0, 0.0),
#     'scale': (1.2, 0.3),
#     'rotation': 0.0
# }
```

**Flipbook:**
```python
fb_data = editor.get_instance(uuid, 'Flipbook', 0)
# {
#     'name': 'RunAnimation',
#     'flipbook_path': 'Content/Flipbooks/run.ice_flipbook',
#     'attach_to_collider': '',
#     'attach_to_socket': '',           # socket name this instance follows ('' = none)
#     'attach_socket_source': 0,        # 0=Auto, 1=Sprite, 2=Flipbook, 3=Skeleton
#     'attach_socket_source_index': -1, # source instance index, -1 = any
#     'attach_socket_inherit_flip_x': True,
#     'attach_socket_offset': (0.0, 0.0),
#     'attach_socket_offset_rotation': 0.0,
#     'playing': True,
#     'speed': 1.0,
#     'color': (1.0, 1.0, 1.0, 1.0),
#     'flip_x': False,
#     'flip_y': False,
#     'visible': True,
#     'render_in_game': True,
#     'cast_shadow': False,
#     'cast_shadow_mode': 0,
#     'shadow_origin': 0,
#     'shadow_edge_fade': 0.0,
#     'shadow_z_order': 0.0,
#     'dont_block_shadows': True,
#     'vertex_effects': {...},          # see "Vertex effects" below
#     'current_frame': 0,
#     'position': (0.0, 0.0, 0.0),
#     'scale': (1.0, 1.0),
#     'rotation': 0.0
# }
```

#### Vertex effects

`SpriteRenderer` and `Flipbook` instances carry a nested `vertex_effects` dict — GPU vertex-shader animation applied to the quad. Read it whole, write only the keys you want:

```python
fx = editor.get_instance(uuid, 'SpriteRenderer', 0)['vertex_effects']
# {
#     'parallax_x': 0.0,
#     'parallax_y': 0.0,
#     'sway_amplitude': 0.0,
#     'sway_speed': 0.0,
#     'sway_phase_offset': 0.0,
#     'sway_gradient': 0.0,
#     'wind_strength': 0.0,
#     'wind_speed': 1.5
# }

# Make foliage sway in the wind
editor.set_instance(uuid, 'SpriteRenderer', 0, {
    'vertex_effects': {
        'sway_amplitude': 4.0,
        'sway_speed': 1.2,
        'sway_gradient': 1.0,
        'wind_strength': 0.6
    }
})
```

**Audio:**
```python
audio_data = editor.get_instance(uuid, 'Audio', 0)
# {
#     'name': 'FootstepSound',
#     'sound_path': 'Content/Sounds/step.ice_sound',
#     'group': 0,
#     'volume': 1.0,
#     'pitch': 1.0,
#     'loop': False,
#     'play_on_wake': False,
#     'spatial': False,
#     'min_distance': 100.0,
#     'max_distance': 1000.0,
#     'rolloff': 1.0,
#     'override_loop': False,
#     'override_spatial': False,
#     'position': (0.0, 0.0, 0.0)
# }
```

> `loop` and `spatial` only take effect once their override flag is on. Writing `loop` or `spatial` through `set_instance` / `set_component` sets the matching override flag for you; write `override_loop` / `override_spatial` directly to fall back to the sound asset's own setting.

**FX:**
```python
fx_data = editor.get_instance(uuid, 'FX', 0)
# {
#     'name': 'Explosion',
#     'fx_path': 'Content/FX/explosion.ice_fx',
#     'playing': False,
#     'loop': False,
#     'speed': 1.0,
#     'play_on_wake': False,
#     'flip_x': False,
#     'flip_y': False,
#     'visible': True,
#     'render_in_game': True,
#     'cast_shadow': False,
#     'shadow_origin': 0,
#     'shadow_edge_fade': 0.0,
#     'shadow_z_order': 0.0,
#     'dont_block_shadows': True,
#     'position': (0.0, 0.0, 0.0),
#     'scale': (1.0, 1.0),
#     'rotation': 0.0
# }
```

**Widget:**
```python
widget_data = editor.get_instance(uuid, 'Widget', 0)
# {
#     'name': 'HealthBar',
#     'widget_path': 'Content/UI/health_bar.ice_widget',
#     'visible': True,
#     'render_in_game': True,
#     'screen_space': True,
#     'scale': 1.0,
#     'render_order': 0,
#     'flip_x': False,
#     'flip_y': False,
#     'interactable': True,
#     'player_index': -1,
#     'position': (0.0, 0.0, 0.0)
# }
```

**PointLight:**
```python
light_data = editor.get_instance(uuid, 'PointLight', 0)
# {
#     'name': 'MainLight',
#     'color': (1.0, 0.9, 0.7),
#     'intensity': 1.0,
#     'radius': 300.0,
#     'falloff': 2.0,
#     'cast_shadows': True,
#     'enabled': True,
#     'cookie_texture': '',
#     'cookie_intensity': 1.0,
#     'cookie_rotation': 0.0,
#     'position': (0.0, 0.0, 0.0)
# }
```

> ℹ️ Lights have no `visible` / `render_in_game` fields — use `enabled` to switch a light instance on and off.

**SpotLight:**
```python
spot_data = editor.get_instance(uuid, 'SpotLight', 0)
# {
#     'name': 'Spotlight1',
#     'color': (1.0, 1.0, 1.0),
#     'intensity': 1.0,
#     'radius': 500.0,
#     'falloff': 2.0,
#     'direction': (0.0, -1.0),
#     'inner_angle': 15.0,
#     'outer_angle': 30.0,
#     'cast_shadows': True,
#     'enabled': True,
#     'cookie_texture': '',
#     'cookie_intensity': 1.0,
#     'cookie_rotation': 0.0,
#     'position': (0.0, 0.0, 0.0)
# }
```

**PointMarker:**
```python
marker_data = editor.get_instance(uuid, 'PointMarker', 0)
# {
#     'name': 'SpawnPoint',
#     'color': (1.0, 0.0, 0.0),
#     'shape': 0,                      # 0 Arrow, 1 Line, 2 Circle, 3 Square
#     'size': 10.0,
#     'thickness': 2.0,
#     'arrow_head_size': 10.0,         # Arrow shape only
#     'arrow_direction': 0.0,          # Arrow shape only, degrees
#     'line_end_offset': (50.0, 0.0),  # Line shape only
#     'visible': True,
#     'render_in_game': False,
#     'position': (0.0, 0.0, 0.0)
# }
```

**Tilemap:**
```python
tm_data = editor.get_instance(uuid, 'Tilemap', 0)
# {
#     'name': 'ForestMap',
#     'tilemap_path': 'Content/Maps/forest.ice_tm',
#     'visible': True,
#     'render_in_game': True,
#     'flip_x': False,
#     'flip_y': False,
#     'position': (0.0, 0.0, 0.0),
#     'scale': (1.0, 1.0),
#     'rotation': 0.0
# }
```

**Joint:**
```python
joint_data = editor.get_instance(uuid, 'Joint', 0)
# {
#     'name': 'HingeJoint',
#     'joint_type': 0,
#     'target_entity_tag': 'Ground',
#     'target_entity_uuid': 987654321,
#     'target_part_name': '',
#     'anchor_a': (0.0, 0.0),
#     'anchor_b': (0.0, 0.0),
#     'collide_connected': False,
#     'enable_spring': False,
#     'hertz': 1.0,
#     'damping_ratio': 0.7,
#     'enable_limit': False,
#     'lower_limit': 0.0,
#     'upper_limit': 0.0,
#     'enable_motor': False,
#     'motor_speed': 0.0,
#     'max_motor_torque': 0.0,
#     'max_motor_force': 0.0,
#     'break_force': 0.0,
#     'break_torque': 0.0,
#     'reference_angle': 0.0,
#     'local_axis_a': (1.0, 0.0),         # Prismatic / Wheel axis
#     'linear_hertz': 0.0,                # Motor joint
#     'angular_hertz': 0.0,               # Motor joint
#     'linear_damping_ratio': 1.0,        # Motor joint
#     'angular_damping_ratio': 1.0,       # Motor joint
#     'max_force': 1.0,                   # Motor joint
#     'max_torque': 1.0,                  # Motor joint
#     'correction_factor': 0.3,           # Motor joint
#     'linear_offset': (0.0, 0.0),        # Motor joint
#     'position': (0.0, 0.0, 0.0)
# }
```

`joint_type` values: `0` Revolute, `1` Distance, `2` Weld, `3` Prismatic, `4` Wheel, `5` Motor.

**ClassComponent:**
```python
cc_data = editor.get_instance(uuid, 'ClassComponent', 0)
# {
#     'name': 'Turret',
#     'class_path': 'Content/Classes/Turret.ice_class',
#     'overrides_json': '',            # per-instance property overrides, raw JSON
#     'position': (0.0, 40.0, 0.0),
#     'scale': (1.0, 1.0),
#     'rotation': 0.0
# }
```

#### `editor.set_instance(uuid, component_type, index, data)` → `bool`

Changes instance data. Pass only fields to change. For each instance you can also change `name`, `position`, `scale`, `rotation` (if supported by the component).

```python
# Change sprite and its local position
editor.set_instance(uuid, 'SpriteRenderer', 0, {
    'name': 'MainSprite',
    'sprite_path': 'Content/Textures/hero_alt.png',
    'color': (1.0, 0.8, 0.8, 1.0),
    'position': (10.0, 0.0, 0.0),
    'scale': (2.0, 2.0),
    'rotation': 15.0
})

# Configure light source
editor.set_instance(uuid, 'PointLight', 0, {
    'color': (1.0, 0.0, 0.0),
    'radius': 200.0,
    'intensity': 3.0,
    'position': (50.0, 0.0, 0.0)
})

# Configure joint with parameters
editor.set_instance(uuid, 'Joint', 0, {
    'joint_type': 1,
    'enable_limit': True,
    'lower_limit': -45.0,
    'upper_limit': 45.0,
    'enable_motor': True,
    'motor_speed': 5.0,
    'max_motor_torque': 100.0,
    'max_motor_force': 50.0,
    'break_force': 1000.0,
    'break_torque': 500.0
})
```

#### `editor.add_instance(uuid, component_type, name='')` → `bool`

Adds a new instance. If name is not specified, uses the component type name.

```python
editor.add_instance(uuid, 'SpriteRenderer', 'Shadow')
editor.add_instance(uuid, 'PointLight', 'RedLight')
editor.add_instance(uuid, 'Audio', 'FootstepSound')
```

#### `editor.remove_instance(uuid, component_type, index)` → `bool`

Removes an instance by index.

```python
editor.remove_instance(uuid, 'PointLight', 1)  # Remove second light source
```

---

### 5.14 World physics settings

#### `editor.get_world_settings()` → `dict`

Returns physics settings of the current scene.

```python
ws = editor.get_world_settings()
# {
#     'gravity_x': 0.0,
#     'gravity_y': -9.8,
#     'ppm': 100.0,               # Pixels Per Meter
#     'sub_step_count': 4,
#     'restitution_threshold': 1.0,
#     'hit_event_threshold': 1.0,
#     'contact_hertz': 30.0,
#     'contact_damping_ratio': 10.0,
#     'max_contact_push_speed': 3.0,
#     'maximum_linear_speed': 400.0,
#     'enable_sleep': True,
#     'enable_continuous': True
# }
```

#### `editor.set_world_settings(data)` → `bool`

Changes physics settings. Pass only fields you want to change.

```python
# Disable gravity (space)
editor.set_world_settings({'gravity_x': 0, 'gravity_y': 0})

# Increase gravity
editor.set_world_settings({'gravity_y': -20.0})

# Change scale (PPM)
editor.set_world_settings({'ppm': 50.0})

# Fixed physics step — write-only, not returned by get_world_settings()
editor.set_world_settings({'fixed_timestep': 1.0 / 60.0})
```

> ℹ️ `fixed_timestep` is the one key `set_world_settings` accepts that `get_world_settings` does not return. Every other key is symmetric.

---

### 5.14.1 Level world overrides

`get_world_settings` / `set_world_settings` above touch the **live** physics state of the scene. A level can additionally carry its own **world override** — a saved block inside the `.icemap` that replaces the project-wide physics and rendering settings whenever that level is loaded. This is what the **World Settings** panel edits.

The override has two independent halves, each with its own `enabled` switch:

| Half | Turns on with | Replaces |
|------|---------------|----------|
| `physics` | `physics.enabled = True` | Gravity, PPM, solver and sleep settings |
| `rendering` | `rendering.enabled = True` | Lighting mode, ambient, clear colour, shadows, raytracing, directional light |

> ⚠️ While `enabled` is `False` for a half, its values are still stored and saved — they simply have no effect. Turning it on later brings them back.

#### `editor.get_world_override()` → `dict`

Returns the level's override block as two nested dicts.

```python
wo = editor.get_world_override()
print(wo['physics']['enabled'], wo['rendering']['enabled'])

# wo == {
#   'physics': {
#       'enabled': False,
#       'ppm': 100.0,
#       'gravity_x': 0.0,
#       'gravity_y': -9.8,
#       'sub_step_count': 4,
#       'fixed_timestep': 0.016666,
#       'restitution_threshold': 1.0,
#       'hit_event_threshold': 1.0,
#       'contact_hertz': 30.0,
#       'contact_damping_ratio': 10.0,
#       'max_contact_push_speed': 3.0,
#       'maximum_linear_speed': 400.0,
#       'enable_sleep': True,
#       'enable_continuous': True
#   },
#   'rendering': {
#       'enabled': False,
#       'near_plane': -1.0,
#       'far_plane': 1.0,
#       'lighting_mode': 0,
#       'ambient_intensity': 0.0,
#       'ambient_color': (0.1, 0.1, 0.15),
#       'clear_color': (0.1, 0.1, 0.1),
#       'shadows_enabled': False,
#       'shadow_quality': 2,
#       'shadow_softness': 0.0,
#       'shadow_intensity': 1.0,
#       'shadow_bias': (0.0, 0.0),
#       'shadow_pcf_samples': 5,
#       'shadow_directional_length': 0.0,
#       'shadow_directional_depth_fade': 0.0,
#       'colliders_block_shadows': False,
#       'raytracing_enabled': False,
#       'raytracing_quality': 1,
#       'raytracing_intensity': 1.0,
#       'raytracing_bounce': 1.0,
#       'raytracing_max_bounces': 2,
#       'raytracing_reflection': 0.0,
#       'raytracing_max_distance': 512.0,
#       'raytracing_denoise': True,
#       'raytracing_shadows': True,
#       'raytracing_ao_intensity': 0.65,
#       'raytracing_ao_radius': 96.0,
#       'raytracing_albedo_response': 0.75,
#       'raytracing_sky_intensity': 1.0,
#       'raytracing_sharpness': 0.5,
#       'raytracing_screen_radiance': True,
#       'dir_light_enabled': False,
#       'dir_light_cast_shadows': False,
#       'dir_light_dir_x': 0.0,
#       'dir_light_dir_y': -1.0,
#       'dir_light_color': (1.0, 1.0, 0.9),
#       'dir_light_intensity': 0.0
#   }
# }
```

#### `editor.set_world_override(data)` → `bool`

Writes into the override block. Pass only the halves and keys you want to change — everything else keeps its value. Changes are applied to the live scene immediately and the scene is marked dirty, so a normal level save persists them.

```python
# Low-gravity moon level
editor.set_world_override({
    'physics': {'enabled': True, 'gravity_y': -1.6}
})

# Dark interior with soft shadows and a warm directional light
editor.set_world_override({
    'rendering': {
        'enabled': True,
        'lighting_mode': 1,
        'ambient_color': (0.02, 0.02, 0.04),
        'ambient_intensity': 0.15,
        'shadows_enabled': True,
        'shadow_quality': 3,
        'shadow_softness': 0.4,
        'dir_light_enabled': True,
        'dir_light_color': (1.0, 0.85, 0.6),
        'dir_light_intensity': 0.8,
        'dir_light_dir_y': -1.0
    }
})
```

Returns `False` if nothing recognisable was passed, or if the World Settings panel is unavailable.

> `ambient_color`, `clear_color`, `dir_light_color` take 3 numbers; `shadow_bias` takes 2. Tuples and lists both work.
>
> ⚠️ **Not undoable.** Like the World Settings panel itself, these functions mark the scene dirty and push a scene snapshot, but the Undo snapshot only covers **entities** — `Ctrl+Z` will not roll an override back. Read the old values with `get_world_override()` first if you need to restore them.

#### `editor.reset_world_override()` → `bool`

Resets **both** halves back to engine defaults, which also switches both `enabled` flags off — the level falls back to the project-wide settings.

```python
editor.reset_world_override()
```

#### `editor.apply_world_override()` → `bool`

Pushes the current override values into the live scene and applies them, without changing any value. `set_world_override` and `reset_world_override` already do this for you; call it directly only if something else changed the World Settings panel and you want the viewport to catch up.

```python
editor.apply_world_override()
```

**Batch example — retune every level in the project:**

```python
for level in scene.list_scenes():
    scene.load(level)
    editor.set_world_override({'physics': {'enabled': True, 'ppm': 64.0}})
    scene.save(level)
```

---

### 5.14.2 Level world assets

Besides entities, a level can hold **world assets** — assets placed directly on the level rather than on an entity, listed in the Level Outliner under the level itself and saved inside the `.icemap`. Typical uses are post-process **View** volumes (`.ice_view`) and **Cinema** sequences (`.ice_cinema`).

Each world asset is a dict:

| Field | Type | Description |
|-------|------|-------------|
| `index` | `int` | Its position in the list (read-only, informational) |
| `name` | `str` | Display name — defaults to the file stem |
| `asset_path` | `str` | Path to the asset file (read-only after adding) |
| `position` | `(x, y, z)` | World position |
| `scale` | `(x, y)` | World scale |
| `expanded` | `bool` | Outliner tree expanded state |
| `cinema_auto_play` | `bool` | Cinema: start automatically |
| `cinema_play_once` | `bool` | Cinema: play only once |
| `cinema_trigger_on_overlap` | `bool` | Cinema: start when a tagged entity overlaps |
| `cinema_trigger_tag` | `str` | Cinema: which gameplay tag triggers it |

#### `editor.get_world_asset_count()` → `int`

Number of world assets on the current level.

#### `editor.get_world_assets()` → `list[dict]`

All world assets, in outliner order.

```python
for wa in editor.get_world_assets():
    print(wa['index'], wa['name'], wa['asset_path'], wa['position'])
```

#### `editor.get_world_asset(index)` → `dict`

One world asset by index. Empty dict if the index is out of range.

#### `editor.set_world_asset(index, data)` → `bool`

Changes one world asset. Pass only the fields you want to change. `index` and `asset_path` are ignored — to point at a different file, remove and re-add.

```python
# Move a post-process volume and rename it
editor.set_world_asset(0, {
    'name': 'CaveGrade',
    'position': (1200.0, -300.0, 0.0),
    'scale': (2.0, 2.0)
})

# Make a cinema fire when the player walks in
editor.set_world_asset(1, {
    'cinema_auto_play': False,
    'cinema_trigger_on_overlap': True,
    'cinema_trigger_tag': 'Player',
    'cinema_play_once': True
})
```

#### `editor.add_world_asset(asset_path, x=0, y=0, z=0)` → `bool`

Adds a world asset at an exact position. Returns `False` if the file does not exist or that asset path is already on the level (each asset can appear once per level).

```python
editor.add_world_asset('Content/Views/cave_grade.ice_view', 1200, -300)
editor.add_world_asset('Content/Cinema/intro.ice_cinema')
```

> ℹ️ Unlike dragging an asset into the viewport — which centres the volume on the drop point — this places the asset's origin at exactly the coordinates you give.

#### `editor.remove_world_asset(index)` → `bool`

Removes one world asset by index.

#### `editor.remove_world_asset_by_path(asset_path)` → `bool`

Removes every world asset pointing at that file. Returns `False` if nothing matched.

```python
editor.remove_world_asset_by_path('Content/Views/old_grade.ice_view')
```

#### `editor.clear_world_assets()` → `bool`

Removes all world assets from the level.

#### `editor.get_selected_world_asset()` → `int`

Index of the world asset selected in the Level Outliner, or `-1` if none is.

#### `editor.select_world_asset(index)` → `bool`

Selects a world asset in the outliner. Pass `-1` to clear the selection. Selecting a world asset clears any entity and folder selection, mirroring what a click in the outliner does.

```python
editor.select_world_asset(0)
print(editor.get_world_asset(editor.get_selected_world_asset()))
```

> Every function that changes a world asset marks the scene dirty and rebuilds post-process volumes, so the viewport updates immediately. `select_world_asset` and the getters change nothing.
>
> ⚠️ **Not undoable**, for the same reason as world overrides: the Undo snapshot only covers entities. Snapshot the list with `get_world_assets()` before a bulk edit if you may need to restore it.

---

### 5.14.3 Level script

Every level carries its own Lua script, saved inside the `.icemap` and edited in the **Level Script** panel. It can be authored either as Lua text or as a visual graph — in the latter case the Lua is generated by the compiler on save.

#### `editor.get_level_script()` → `str`

Returns the level's Lua script text. Empty string if the level has none.

```python
code = editor.get_level_script()
print(f'{len(code.splitlines())} lines')
```

#### `editor.set_level_script(code)` → `bool`

Replaces the level's Lua script text and marks the level dirty, so a normal level save persists it.

```python
editor.set_level_script('''
function OnLevelStart()
    Log("level started")
end
''')
```

> ⚠️ Returns `False` when the level is authored as a **visual graph** — its Lua is generated from the graph on every save, so writing the text would be discarded. Check with `is_level_using_visual_script()` first.

#### `editor.is_level_using_visual_script()` → `bool`

`True` when the level's script is authored as a visual graph rather than Lua text.

```python
if editor.is_level_using_visual_script():
    editor.log_warn('Level uses a visual graph — script text is read-only')
else:
    editor.set_level_script(new_code)
```

**Batch example — audit level scripts across the project:**

```python
for level in scene.list_scenes():
    scene.load(level)
    code = editor.get_level_script()
    kind = 'graph' if editor.is_level_using_visual_script() else 'lua'
    print(f'{level}: {kind}, {len(code)} chars')
```

---

### 5.15 Editor settings

#### `editor.get_editor_settings()` → `dict`

Returns current editor settings.

```python
es = editor.get_editor_settings()
# {
#     'show_grid': True,
#     'show_physics_colliders': False,
#     'show_nav_grid': False,
#     'viewport_width': 1280.0,
#     'viewport_height': 720.0,
#     'is_viewport_focused': True,
#     'is_viewport_hovered': True,
#     'last_opened_level': 'Content/Levels/main.icemap',
#     'current_level_path': 'Content/Levels/main.icemap',
#     'is_level_dirty': False,
#     'grid_size': 32.0,
#     'snap_to_grid': True
# }
```

`show_grid` is the effective grid visibility of the main viewport: the **Show Viewport Grid** setting AND the grid toggle on the toolbar. It is `False` if either of them is off. The class editor has its own grid toggle stored inside the `.ice_class` asset, and it is not reported here.

#### `editor.set_editor_settings(data)` → `bool`

Changes editor settings.

```python
editor.set_editor_settings({'level_dirty': True})
```

#### `editor.mark_dirty()` → `bool`

Marks the scene as modified (asterisk `*` in the title).

#### `editor.is_dirty()` → `bool`

Checks if there are unsaved changes.

```python
if editor.is_dirty():
    print('There are unsaved changes!')
```

---

### 5.16 Localization

#### `editor.get_locale()` → `str`

Returns current language code (e.g., `'en'`, `'ru'`).

```python
lang = editor.get_locale()  # 'ru'
```

#### `editor.set_locale(lang_code)`

Sets UI language.

```python
editor.set_locale('en')
editor.set_locale('ru')
```

#### `editor.get_localized_string(key)` → `str`

Returns localized string by key.

```python
text = editor.get_localized_string('menu.file.save')
```

---

### 5.17 Spatial queries

#### `editor.distance_between(uuid1, uuid2)` → `float`

Calculates distance between two entities (2D). Returns `-1` if one entity is not found.

```python
dist = editor.distance_between(player_uuid, enemy_uuid)
print(f'Distance: {dist:.1f} px')
```

#### `editor.get_entities_in_radius(x, y, radius)` → `list[int]`

Finds all entities within radius from a point (world coordinates).

```python
# Find all entities within 500 px from center
nearby = editor.get_entities_in_radius(0, 0, 500)
print(f'In radius: {len(nearby)}')
```

#### `editor.get_closest_entity(x, y)` → `int`

Finds the closest entity to a point. Returns `0` if no entities.

```python
closest = editor.get_closest_entity(100, 200)
if closest:
    name = editor.get_entity_name(closest)
    print(f'Closest: {name}')
```

---

### 5.18 Classes & export

#### `editor.list_classes(directory='Content', recursive=True)` → `list[str]`

Finds all `.ice_class` files in the specified directory.

```python
classes = editor.list_classes()
for c in classes:
    print(c)
```

#### `editor.save_as_class(uuid, filepath)` → `bool`

Saves an entity as a class file (`.ice_class`).

```python
uuid = editor.find_entity('Player')
editor.save_as_class(uuid, 'Content/Classes/Player.ice_class')
```

---

### 5.19 Events

The event system lets you subscribe to editor change notifications.

#### Available events

| Event | Description | Callback args |
|-------|-------------|---------------|
| `entity_created` | New entity created | `uuid, name` |
| `entity_deleted` | Entity deleted | `uuid` |
| `selection_changed` | Selection changed | — |
| `scene_saved` | Scene saved to file | `path` |
| `scene_loaded` | Scene loaded from file | `path` |
| `scene_new` | New empty scene created | — |

> **All events are source-agnostic.** `entity_created`, `entity_deleted`, and `selection_changed` fire no matter how the change happened — a Python call, the level outliner, the viewport, paste, undo/redo, or a plugin. The `scene_*` events fire from the editor's Save/Open/New flows as well. Subscribe once at startup and you are notified of every change.
>
> Entity and selection events are dispatched once per frame by diffing editor state, so a script that creates 100 entities triggers 100 `entity_created` callbacks at the end of that frame (transient entities created and destroyed within a single frame are not reported). Loading or creating a scene re-baselines the tracker instead of emitting one event per entity.

#### `editor.on(event_name, callback)`

Subscribes to an event.

```python
def on_entity_created(uuid, name):
    print(f'Entity "{name}" created (UUID: {uuid})')

editor.on('entity_created', on_entity_created)

def on_selection():
    selected = editor.get_selected()
    print(f'Selected: {selected}')

editor.on('selection_changed', on_selection)
```

#### `editor.off(event_name)`

Unsubscribes **all** callbacks for the event.

```python
editor.off('entity_created')
```

#### `editor.get_events()` → `list[str]`

Returns list of events with active subscriptions.

```python
events = editor.get_events()
print(events)  # ['entity_created', 'selection_changed']
```

---

### 5.20 Timers

#### `editor.set_timer(delay_sec, callback)` → `int`

Creates a one-shot timer. Callback is called after `delay_sec` seconds. Returns timer ID.

```python
def delayed_action():
    print('3 seconds passed!')

timer_id = editor.set_timer(3.0, delayed_action)
```

#### `editor.set_interval(interval_sec, callback)` → `int`

Creates a repeating timer. Callback is called every `interval_sec` seconds. Returns timer ID.

```python
def periodic_check():
    fps = engine.fps()
    if fps < 30:
        editor.log_warn(f'Low FPS: {fps:.0f}')

interval_id = editor.set_interval(5.0, periodic_check)
```

#### `editor.cancel_timer(timer_id)` → `bool`

Cancels a timer by ID.

```python
editor.cancel_timer(timer_id)
```

#### `editor.clear_timers()`

Cancels **all** active timers.

```python
editor.clear_timers()
```

---

### 5.21 Logging

#### `editor.log(message, level=1)`

Prints a message in the editor console. Levels: `1` — Info, `2` — Warning, `3` — Error.

```python
editor.log('Information')
editor.log('Warning', 2)
editor.log('Error!', 3)
```

#### `editor.log_info(message)`

Short form for info messages.

```python
editor.log_info('All good')
```

#### `editor.log_warn(message)`

Short form for warnings.

```python
editor.log_warn('Something looks suspicious')
```

#### `editor.log_error(message)`

Short form for errors.

```python
editor.log_error('Something went wrong')
```

---

### 5.22 User scripts & script execution

This subsection covers the Python tooling surface: executing script files on disk, controlling the Python console autocomplete, and integrating with the Python Console panel's **Run Python Script** menu, which is backed by `Tools/PythonScripts/`.

> 📁 **Script locations (editor-only — NOT game content):**
> - `Tools/PythonScripts/` — user tool scripts, discovered recursively. Each subfolder becomes a **category** (submenu) in the panel's *Run Python Script* menu.
> - `Tools/PythonScripts/Startup/` — scripts executed automatically once, right after the editor initializes the Python interpreter. Use them to register event handlers, timers, custom tools, etc.

#### `editor.execute_file(filepath)` → `dict`

Executes a Python script file in the main (`__main__`) namespace — the same scope shared by all file runs, startup scripts, and `run_user_script`. Variables, imports, and functions it defines persist there afterwards; any event handlers or timers it registers stay active too. This is a separate namespace from the interactive console (the **Run Script** button and **Quick Command** field) — see [Console vs. global execution scope](#console-vs-global-execution-scope).

Returns a dictionary:

| Key | Type | Description |
|-----|------|-------------|
| `success` | `bool` | `True` if the script finished without raising. |
| `output` | `str` | Captured `stdout` contents. |
| `errors` | `str` | Captured `stderr` / traceback (empty if `success`). |

```python
result = editor.execute_file('Tools/PythonScripts/cleanup.py')
if result['success']:
    editor.log_info(result['output'])
else:
    editor.log_error(result['errors'])
```

> ⚠️ Path is resolved relative to the project working directory. Absolute paths are also accepted.

#### `editor.list_user_scripts()` → `list[str]`

Returns display names of every `.py` file discovered under `Tools/PythonScripts/` — a recursive scan that **also includes** `Startup/`. The display name is the script's **bare file stem** (no folder prefix). Results are sorted by category, then by name; the category is what becomes a submenu in the panel's *Run Python Script* menu.

```python
for name in editor.list_user_scripts():
    print(name)
# cleanup_scene
# spawn_grid
# spawn_circle
```

> ⚠️ Two scripts with the same file name in different subfolders produce the same display name. To target one unambiguously, pass its file path to `run_user_script` instead.

#### `editor.run_user_script(name)` → `bool`

Runs a previously discovered user script by its **display name** (as returned by `list_user_scripts`) or by the **file path it was discovered under** (e.g. `Tools/PythonScripts/level_gen/spawn_grid.py`). The first entry that matches wins. Returns `True` on successful execution. Logs a warning if the name is not found.

```python
if editor.run_user_script('spawn_grid'):
    editor.log_info('Grid spawned')

# Unambiguous form — by path
editor.run_user_script('Tools/PythonScripts/level_gen/spawn_grid.py')
```

> This is the same entry point the panel's **Run Python Script → <category> → <script>** menu items use.

#### `editor.rediscover_user_scripts()` → `int`

Re-scans `Tools/PythonScripts/` and rebuilds the script list used by both `list_user_scripts` and the Tools menu. Returns the total number of scripts found. Call this after you add, remove, or rename files on disk while the editor is running.

```python
count = editor.rediscover_user_scripts()
print(f'Found {count} user scripts')
```

#### `editor.reload_autocomplete()`

Rebuilds the Python console autocomplete index from the currently loaded modules. Useful after you `import` new modules, define new functions/classes in the console, or register new bindings from a script.

```python
import math
def my_helper(x):
    return x * 2
editor.reload_autocomplete()
# `math.` and `my_helper` now appear in Tab-completion
```

#### `editor.get_autocomplete_items()` → `list[str]`

Returns the current list of autocomplete entries used by the Python console. Handy for building your own fuzzy-search tools or inspecting what symbols are exposed.

```python
items = editor.get_autocomplete_items()
print(f'{len(items)} autocomplete entries')
print([i for i in items if i.startswith('editor.batch_')])
```

---

## 6. `scene` module — Working with scenes

#### `scene.get_entity_count()` → `int`

Returns number of entities in the current scene.

```python
count = scene.get_entity_count()
print(f'Entities: {count}')
```

#### `scene.get_path()` → `str`

Returns path of the currently loaded scene.

```python
path = scene.get_path()
print(f'Current scene: {path}')
```

#### `scene.save(path='')` → `bool`

Saves the current scene. If path is not specified, saves to the current path.

```python
scene.save()                                    # Save current
scene.save('Content/Levels/backup.icemap')   # Save as
```

> 🔔 Generates `scene_saved` event.

#### `scene.load(path)` → `bool`

Loads a scene from a file.

```python
scene.load('Content/Levels/level2.icemap')
```

> 🔔 Generates `scene_loaded` event.

#### `scene.new()`

Creates a new empty scene.

```python
scene.new()
```

#### `scene.list_scenes(directory='Content', recursive=True)` → `list[str]`

Finds all `.icemap` files in the specified directory.

```python
scenes = scene.list_scenes()
for s in scenes:
    print(s)
# Content/Levels/main.icemap
# Content/Levels/boss.icemap
```

#### `scene.export_entity(uuid)` → `str`

Exports an entity to a JSON string.

```python
json_str = scene.export_entity(uuid)
print(json_str)
```

#### `scene.import_entity(json_str)` → `int`

Imports an entity from a JSON string. Returns UUID of the new entity.

```python
json_data = scene.export_entity(old_uuid)
new_uuid = scene.import_entity(json_data)
```

#### `scene.save_entity(uuid, filepath)` → `bool`

Saves an entity to a file.

```python
scene.save_entity(uuid, 'Content/Templates/player_template.json')
```

#### `scene.load_entity(filepath)` → `int`

Loads an entity from a file. Returns UUID.

```python
uuid = scene.load_entity('Content/Templates/player_template.json')
```

---

## 7. `engine` module — Engine information

### 7.1 General info

#### `engine.version()` → `str`

Returns engine version.

```python
print(engine.version())  # '1.0.0'
```

#### `engine.working_dir()` → `str`

Returns current working directory.

#### `engine.project_path()` → `str`

Returns project root path.

#### `engine.content_path()` → `str`

Returns `Content` directory path.

#### `engine.uptime()` → `float`

Returns engine uptime in seconds since launch.

```python
hours = engine.uptime() / 3600
print(f'Engine has been running for {hours:.1f} hours')
```

#### `engine.has_scene()` → `bool`

Checks if there is an active scene.

#### `engine.entity_count()` → `int`

Returns entity count in the active scene.

---

### 7.2 Performance and statistics

#### `engine.fps()` → `float`

Returns current FPS (frames per second).

```python
fps = engine.fps()
print(f'FPS: {fps:.0f}')
```

#### `engine.frame_time()` → `float`

Returns current frame time in **milliseconds**.

```python
ms = engine.frame_time()
print(f'Frame time: {ms:.2f} ms')
```

#### `engine.delta_time()` → `float`

Returns delta time in **seconds** (time between frames).

#### `engine.render_stats()` → `dict`

Returns detailed rendering statistics.

```python
stats = engine.render_stats()
# {
#     'entity_count': 150,
#     'sprite_count': 120,
#     'draw_calls': 45,
#     'quad_count': 300,
#     'physics_bodies': 30,
#     'active_scripts': 10,
#     'flipbook_count': 5,
#     'audio_count': 8,
#     'fx_count': 3,
#     'light_count': 12,
#     'camera_count': 1,
#     'widget_count': 4,
#     'tilemap_count': 2,
#     'animator_count': 6,
#     'collider_count': 25
# }
```

#### `engine.memory_stats()` → `dict`

Returns memory usage information.

```python
mem = engine.memory_stats()
# {
#     'ram_mb': 256,
#     'ram_peak_mb': 310,
#     'ram_total_mb': 16384,
#     'vram_mb': 128,
#     'vram_peak_mb': 200,
#     'vram_total_mb': 8192,
#     'cpu_usage': 15.5,
#     'gpu_time': 3.2
# }
```

#### `engine.system_info()` → `dict`

Returns system information.

```python
sys = engine.system_info()
# {
#     'cpu_cores': 8,
#     'cpu_logical': 16,
#     'cpu_freq_ghz': 3.6
# }
```

---

### 7.3 Resources

#### `engine.get_loaded_textures()` → `list[str]`

Returns names of all loaded textures.

```python
textures = engine.get_loaded_textures()
print(f'Loaded textures: {len(textures)}')
```

#### `engine.get_loaded_shaders()` → `list[str]`

Returns names of all loaded shaders.

#### `engine.get_loaded_sounds()` → `list[str]`

Returns names of all loaded sounds.

#### `engine.texture_count()` → `int`

Number of loaded textures.

#### `engine.shader_count()` → `int`

Number of loaded shaders.

#### `engine.sound_count()` → `int`

Number of loaded sounds.

#### `engine.is_texture_loaded(name)` → `bool`

Checks if a texture is loaded.

```python
if engine.is_texture_loaded('player_idle'):
    print('Texture loaded')
```

#### `engine.is_sound_loaded(name)` → `bool`

Checks if a sound is loaded.

#### `engine.reload_texture(name)`

Reloads a texture from disk.

```python
engine.reload_texture('player_idle')
```

#### `engine.collect_unused()`

Unloads unused resources from memory.

```python
engine.collect_unused()
```

---

### 7.4 Audio

#### `engine.play_sound(name)`

Plays a sound by name.

```python
engine.play_sound('explosion')
```

#### `engine.stop_sound(name)`

Stops a sound.

#### `engine.pause_sound(name)`

Pauses a sound.

#### `engine.resume_sound(name)`

Resumes a sound.

#### `engine.is_sound_playing(name)` → `bool`

Checks if a sound is playing.

```python
if engine.is_sound_playing('bgm'):
    print('Music is playing')
```

#### `engine.set_sound_volume(name, volume)`

Sets sound volume (0.0–1.0).

```python
engine.set_sound_volume('bgm', 0.5)
```

#### `engine.set_sound_pitch(name, pitch)`

Sets sound pitch.

```python
engine.set_sound_pitch('engine_sound', 1.5)  # Speed up
```

#### `engine.set_sound_loop(name, loop)`

Enables/disables sound looping.

```python
engine.set_sound_loop('bgm', True)
```

#### `engine.set_master_volume(volume)`

Sets master volume (0.0–1.0).

```python
engine.set_master_volume(0.8)
```

#### `engine.get_master_volume()` → `float`

Returns current master volume.

#### `engine.stop_all_sounds()`

Stops all sounds.

```python
engine.stop_all_sounds()
```

#### `engine.audio_info()` → `dict`

Returns audio device info.

```python
info = engine.audio_info()
# {
#     'device_name': 'Speakers (Realtek)',
#     'sample_rate': 44100,
#     'channels': 2,
#     'master_volume': 1.0,
#     'quality': 'High'
# }
```

---

## 8. `browser` module — Content browser

The `browser` module lets you programmatically manage project files and folders via the content browser.

### 8.1 Navigation

#### `browser.get_current_dir()` → `str`

Returns current directory in the content browser.

```python
current = browser.get_current_dir()
print(f'Current folder: {current}')
```

#### `browser.set_current_dir(path)` → `bool`

Navigates to the specified directory.

```python
browser.set_current_dir('Content/Textures')
```

#### `browser.navigate(path)` → `bool`

Alias for `set_current_dir`.

```python
browser.navigate('Content/Levels')
```

#### `browser.get_content_root()` → `str`

Returns root `Content` directory.

---

### 8.2 Files and folders

#### `browser.list_files(path='')` → `list[str]`

List of files in a directory. Defaults to current directory.

```python
files = browser.list_files('Content/Textures')
for f in files:
    print(f)
```

#### `browser.list_folders(path='')` → `list[str]`

List of folders in a directory.

```python
folders = browser.list_folders('Content')
```

#### `browser.list_all(path='')` → `list[str]`

List of files and folders.

#### `browser.find_files(pattern, recursive=True)` → `list[str]`

Finds files by regex pattern in `Content`.

```python
# Find all PNG files
pngs = browser.find_files(r'\.png$')

# Find files containing "player"
player_files = browser.find_files('player')
```

#### `browser.find_by_extension(extension, recursive=True)` → `list[str]`

Finds files by extension.

```python
classes = browser.find_by_extension('.ice_class')
textures = browser.find_by_extension('png')  # Dot added automatically
```

#### `browser.create_folder(name, parent_path='')` → `bool`

Creates a folder. If `parent_path` is not specified, creates in current directory.

```python
browser.create_folder('NewLevel', 'Content/Levels')
```

#### `browser.create_file(name, content='', parent_path='')` → `bool`

Creates a file with the specified content.

```python
browser.create_file('notes.txt', 'Level notes', 'Content')
```

#### `browser.delete(path)` → `bool`

Deletes a file or folder (recursive).

```python
browser.delete('Content/Temp')
browser.delete('Content/old_texture.png')
```

#### `browser.rename(path, new_name)` → `bool`

Renames a file or folder.

```python
browser.rename('Content/Textures/old.png', 'new.png')
```

#### `browser.move(source_path, dest_folder)` → `bool`

Moves a file or folder to another directory.

```python
browser.move('Content/player.png', 'Content/Textures')
```

#### `browser.copy(source_path, dest_folder)` → `bool`

Copies a file or folder.

```python
browser.copy('Content/Textures/player.png', 'Content/Backup')
```

#### `browser.exists(path)` → `bool`

Checks if path exists.

```python
if browser.exists('Content/Levels/boss.icemap'):
    print('Boss level exists!')
```

#### `browser.is_dir(path)` → `bool`

Checks if path is a directory.

#### `browser.is_file(path)` → `bool`

Checks if path is a file.

---

### 8.3 Reading and writing files

#### `browser.read_file(path)` → `str`

Reads text file contents.

```python
content = browser.read_file('Content/Scripts/notes.txt')
print(content)
```

#### `browser.write_file(path, content)` → `bool`

Writes text content to a file.

```python
browser.write_file('Content/config.txt', 'difficulty=hard\nvolume=0.8')
```

#### `browser.get_file_size(path)` → `int`

Returns file size in bytes. Returns `-1` if file not found.

```python
size = browser.get_file_size('Content/Textures/player.png')
print(f'Size: {size / 1024:.1f} KB')
```

---

### 8.4 Assets

#### `browser.get_asset_type(path)` → `str`

Determines asset type by file extension.

```python
t = browser.get_asset_type('Content/player.ice_class')
print(t)  # 'Class'

t = browser.get_asset_type('Content/map.icemap')
print(t)  # 'Level'
```

**Asset types:**

| Extension | Type |
|-----------|-----|
| `.ice_class` | Class |
| `.ice_ts` | Tileset |
| `.ice_tm` | Tilemap |
| `.ice_flipbook` | Flipbook |
| `.ice_animation` | Animation |
| `.ice_sprite` | Sprite |
| `.ice_material` | Material |
| `.ice_sound` | Sound |
| `.ice_fx` | FX |
| `.ice_widget` | Widget |
| `.ice_view` | View |
| `.ice_cinema` | Cinema |
| `.icemap` | Level |
| `.png`, `.jpg`, `.jpeg` | Texture |
| `.wav`, `.mp3`, `.ogg`, `.flac` | Audio |
| `.ttf`, `.otf` | Font |
| `.lua` | Script |
| `.txt` | Text file |

#### `browser.get_asset_extension(asset_type)` → `str`

Returns extension for asset type.

```python
ext = browser.get_asset_extension('Class')  # '.ice_class'
ext = browser.get_asset_extension('Level')  # '.icemap'
```

#### `browser.get_supported_types()` → `list[str]`

Returns list of all supported asset types.

```python
types = browser.get_supported_types()
# ['Class', 'Tileset', 'Tilemap', 'Flipbook', 'Animation',
#  'Sprite', 'Material', 'Sound', 'FX', 'Widget', 'View',
#  'Cinema', 'Level', 'Texture', 'Audio', 'Font', 'Script', 'Text']
```

#### `browser.import_file(external_path, dest_folder='')` → `bool`

Imports an external file into the `Content` folder.

```python
browser.import_file('C:/Downloads/texture.png', 'Content/Textures')
```

---

### 8.5 Browser selection

#### `browser.get_selected()` → `list[str]`

Returns paths of selected items in the content browser.

```python
selected = browser.get_selected()
```

#### `browser.select(path)` → `bool`

Selects an item in the content browser.

```python
browser.select('Content/Textures/player.png')
```

#### `browser.select_multiple(paths)` → `bool`

Selects multiple items.

```python
browser.select_multiple([
    'Content/Textures/player.png',
    'Content/Textures/enemy.png'
])
```

#### `browser.clear_selection()`

Clears selection.

#### `browser.has_selection()` → `bool`

`True` if anything is selected. Cheaper than `len(browser.get_selected()) > 0`.

```python
if browser.has_selection():
    print(browser.get_selected())
```

---

### 8.6 Panel state, search and refresh

#### `browser.refresh()`

Rebuilds the content browser view. Call it after creating, deleting, or moving files on disk from Python so the panel picks up the change.

```python
browser.write_file('Content/Data/levels.json', '{}')
browser.refresh()
```

> `editor.refresh_browser()` does the same thing from the `editor` module.

#### `browser.is_focused()` → `bool`

`True` while the content browser panel has keyboard focus.

```python
if browser.is_focused():
    editor.log_info('Content browser is focused')
```

#### `browser.get_search_filter()` → `str`

Current text in the browser's search box (`''` when empty).

#### `browser.set_search_filter(filter)`

Types text into the browser's search box, exactly as if the user had. Pass `''` to clear it.

```python
# Show only files whose name contains "player"
browser.set_search_filter('player')
print(browser.get_visible_paths())

browser.set_search_filter('')
```

> While a search filter is active the browser switches to a **recursive** view, so results come from subfolders too.

#### `browser.get_visible_paths()` → `list[str]`

Paths currently shown in the browser, after the search box and the type filters are applied — i.e. exactly what the user sees. Compare with `browser.list_all()`, which ignores filters.

```python
browser.set_search_filter('.ice_class')
for p in browser.get_visible_paths():
    print(p)
```

---

### 8.7 Browser clipboard

#### `browser.copy_to_clipboard(paths)` → `bool`

Copies paths to browser clipboard.

```python
browser.copy_to_clipboard(['Content/Textures/player.png'])
```

#### `browser.cut_to_clipboard(paths)` → `bool`

Cuts paths to clipboard (moved on paste).

#### `browser.paste(dest_folder='')` → `bool`

Pastes from clipboard. Defaults to current directory.

```python
browser.paste('Content/Backup')
```

#### `browser.get_clipboard()` → `list[str]`

Returns clipboard contents.

---

### 8.8 Path utilities

#### `browser.abs_path(relative_path)` → `str`

Converts a relative path to an absolute path.

```python
full = browser.abs_path('Content/Textures/player.png')
# 'C:/Projects/MyGame/Content/Textures/player.png'
```

#### `browser.rel_path(absolute_path)` → `str`

Converts an absolute path to a relative path.

#### `browser.join(base, child)` → `str`

Joins path components.

```python
path = browser.join('Content/Textures', 'player.png')
# 'Content/Textures/player.png'
```

#### `browser.parent(path)` → `str`

Returns parent directory.

```python
parent = browser.parent('Content/Textures/player.png')
# 'Content/Textures'
```

#### `browser.filename(path)` → `str`

Returns filename from a path.

```python
name = browser.filename('Content/Textures/player.png')
# 'player.png'
```

#### `browser.stem(path)` → `str`

Returns filename without extension.

```python
stem = browser.stem('Content/Textures/player.png')
# 'player'
```

#### `browser.extension(path)` → `str`

Returns file extension.

```python
ext = browser.extension('Content/Textures/player.png')
# '.png'
```

---

### 8.9 Batch file operations

#### `browser.batch_delete(paths)` → `int`

Deletes multiple files/folders. Returns number deleted.

```python
count = browser.batch_delete(['Content/temp1.txt', 'Content/temp2.txt'])
```

#### `browser.batch_move(paths, dest_folder)` → `int`

Moves multiple files. Returns number moved.

```python
files = browser.find_by_extension('.png')
browser.batch_move(files, 'Content/Textures')
```

#### `browser.batch_copy(paths, dest_folder)` → `int`

Copies multiple files. Returns number copied.

---

## 9. `icebox.log` module — System logging

The `icebox.log` module writes messages directly to the engine log (output window), not the editor Python console.

#### `icebox.log.info(msg)`

Info message.

```python
icebox.log.info('Script started')
```

#### `icebox.log.warn(msg)`

Warning message.

```python
icebox.log.warn('Texture not found')
```

#### `icebox.log.error(msg)`

Error message.

```python
icebox.log.error('Critical error!')
```

> ℹ️ Unlike `editor.log_info()`, which writes to the editor console (visible to the user), `icebox.log.info()` writes to the engine system log. Both are useful for different purposes.

---

## 10. Component types — Full reference

### Table of all components

| Type | Multi-instance | Description |
|-----|:-:|----------|
| `Transform` | ❌ | Position (x, y, z), scale (x, y), rotation (degrees). **Every entity has it.** |
| `SpriteRenderer` | ✅ | Sprite rendering. Texture path, color, flip, visibility. |
| `Camera` | ❌ | Scene camera. Orthographic width, offset, background color, primary flag. |
| `Rigidbody` | ❌ | Physics body (Box2D). Body type, gravity, damping, bullet, sleep. |
| `Collider` | ❌ | Physics colliders. Array of Box, Sphere, and Capsule colliders with size, density, friction. |
| `Flipbook` | ✅ | Frame animation. Flipbook path, speed, color, playback. |
| `Audio` | ✅ | Sound sources. Sound path, volume, pitch, loop, spatial sound. |
| `Animator` | ❌ | State Machine animation. Animation path, current state, frame. |
| `Skeleton` | ❌ | Skeletal (2D bone) animation. Skeleton path, current animation/skin, playback, color, ragdoll. |
| `Tilemap` | ✅ | Tilemaps. Tilemap path, visibility, flip. |
| `FX` | ✅ | Particle systems. FX path, playback, loop, speed. |
| `Widget` | ✅ | UI widgets. Widget path, visibility, screen space, scale, order, flip (x/y), interactable, player index. |
| `PointLight` | ✅ | Point light. Color, intensity, radius, falloff, shadows. |
| `SpotLight` | ✅ | Spotlight. Color, direction, cone angles, shadows. |
| `PointMarker` | ✅ | Point marker (editor). Color, shape, size. |
| `Script` | ❌ | Lua script. File name and class path. |
| `AI` | ❌ | AI behaviour. AI asset path, move speed, movement mode, detection/attack radius, perception settings, patrol route. |
| `Joint` | ✅ | Physics joints. Type, anchors, springs, motors, limits, break forces. |
| `Destructible` | ❌ | Destruction. Health, fragment count/pattern, explosion force, fragment physics, debris cap. |
| `ClassComponent` | ✅ | Nested class component instances. Each instance has a `class_path` and local transform. |
| `Stencil` | ❌ | Stencil masking. `enabled`, `mode`, `stencil_id`, `compare_func`. |
| `GameplayTag` | ❌ | Gameplay tag set. `{'tags': ['Enemy.Boss', ...]}` — read/replace the whole set. |
| `Interface` | ❌ | Implemented interface names. `{'interfaces': ['IDamageable', ...]}`. |
| `Replication` | ❌ | Network replication: ownership, what to sync, script mode, relevancy. Cannot be removed. |
| `Hierarchy` | ❌ | Parent/child links: `parent_uuid`, `children`, `local_position`, `local_scale`, `local_rotation`. **Read** via `get_component`; **edit** via `set_parent` / `clear_parent` (see 5.6). |

### Enum reference

All enum-valued component fields, with their integer meanings.

**Rigidbody `body_type`**

| Value | Type | Description |
|:-----:|------|-------------|
| `0` | Static | Static body (does not move) |
| `1` | Dynamic | Dynamic body (responds to physics) |
| `2` | Kinematic | Kinematic body (moved by script, no forces) |

**Collider `one_way_dir`**

| Value | Direction |
|:-----:|-----------|
| `0` | None |
| `1` | Up |
| `2` | Down |
| `3` | Left |
| `4` | Right |

**Collider `collision_enabled`** — bit mask, `0..3`. `3` (both bits) is the default "fully enabled".

**Joint `joint_type`**

| Value | Type |
|:-----:|------|
| `0` | Revolute |
| `1` | Distance |
| `2` | Weld |
| `3` | Prismatic |
| `4` | Wheel |
| `5` | Motor |

**PointMarker `shape`**

| Value | Shape |
|:-----:|-------|
| `0` | Arrow |
| `1` | Line |
| `2` | Circle |
| `3` | Square |

**AI `movement_mode`**

| Value | Mode |
|:-----:|------|
| `0` | Auto |
| `1` | Transform |
| `2` | Physics |

**Replication `owner_mode`** — `0` Server, `1` Player.
**Replication `script_mode`** — `0` Auto, `1` AlwaysRun, `2` NeverRun.
**Replication `relevancy`** — `0` AreaOfInterest, `1` AlwaysRelevant.

**Stencil `mode`** — `0..2`. **Stencil `compare_func`** — `0..1`. **Stencil `stencil_id`** — `1..255`.

**Camera / Widget `player_index`** — `-1` all players, `0..3` a specific split-screen slot.

**Sprite / Flipbook / Skeleton `cast_shadow_mode`** — `0` Colliders, `1` Contour.

**Skeleton `shadow_origin`** — `0` Bottom, `1` Center, `2` Top.
**Skeleton `shading_mode`** — `0` Lit, `1` Unlit.
**Skeleton `blend_mode`** — `0` Masked, `1` Additive, `2` Translucent, `3` Opaque.

**Transform `enabled` / `visible` / `render_in_game`** — booleans, not enums: `enabled` disables the whole entity, `visible` hides it in the editor viewport, `render_in_game` hides it in play mode and the shipped game.

### Full instance fields

When using `get_instance` / `set_instance` for multi-instance components, **additional fields** are available that are not returned by `get_component`:

| Field | Component types | Description |
|------|:---:|-------------|
| `name` | All multi-instance | Instance name (`str`) |
| `position` | All multi-instance | Local position `(x, y, z)` relative to entity |
| `scale` | SpriteRenderer, Flipbook, FX, Tilemap, ClassComponent | Local scale `(x, y)` |
| `rotation` | SpriteRenderer, Flipbook, FX, Tilemap, ClassComponent | Local rotation in degrees |
| `render_in_game` | SpriteRenderer, Flipbook, FX, Widget, PointMarker | Render in game mode |
| `attach_to_collider` | SpriteRenderer, Flipbook | Name of the collider the instance follows (`''` = none) |
| `attach_to_socket` | SpriteRenderer, Flipbook | Name of the socket (attach point) the instance follows (`''` = none). The engine keeps the instance glued to it every frame, correct under entity rotation, entity scale and Flip X. |
| `attach_socket_source` | SpriteRenderer, Flipbook | Which component provides the socket: `0` Auto, `1` Sprite, `2` Flipbook, `3` Skeleton |
| `attach_socket_source_index` | SpriteRenderer, Flipbook | Instance index of the source component, `-1` = search all instances |
| `attach_socket_inherit_flip_x` | SpriteRenderer, Flipbook | Copy Flip X from the socket owner so the attached art mirrors with it |
| `attach_socket_offset` | SpriteRenderer, Flipbook | Extra offset `(x, y)` layered on top of the socket, in socket space |
| `attach_socket_offset_rotation` | SpriteRenderer, Flipbook | Extra rotation in degrees layered on top of the socket angle |
| `cast_shadow` | SpriteRenderer, Flipbook, FX | Instance casts a shadow |
| `cast_shadow_mode` | SpriteRenderer, Flipbook | Shadow mode, `0..1` |
| `shadow_origin` | SpriteRenderer, Flipbook, FX | Shadow origin mode |
| `shadow_edge_fade` | SpriteRenderer, Flipbook, FX | Shadow edge fade |
| `shadow_z_order` | SpriteRenderer, Flipbook, FX | Shadow Z order |
| `dont_block_shadows` | SpriteRenderer, Flipbook, FX | Do not occlude other shadows |
| `vertex_effects` | SpriteRenderer, Flipbook | Nested dict of vertex-shader effects (see 5.13) |
| `current_frame` | Flipbook | Current animation frame |
| `flip_x` / `flip_y` | Widget, FX | Mirror horizontally / vertically |
| `player_index` | Widget | Owning player: `-1` = all, `0..3` = specific player (clamped) |
| `rolloff` | Audio | Spatial distance rolloff |
| `override_loop` / `override_spatial` | Audio | Use the instance's own `loop` / `spatial` instead of the sound asset's |
| `thickness` | PointMarker | Line thickness |
| `arrow_head_size` | PointMarker | Arrow head size (Arrow shape) |
| `arrow_direction` | PointMarker | Arrow direction in degrees (Arrow shape) |
| `line_end_offset` | PointMarker | Line end offset `(x, y)` (Line shape) |
| `cookie_texture` | PointLight, SpotLight | Light cookie (gobo) texture path |
| `cookie_intensity` | PointLight, SpotLight | Light cookie intensity |
| `cookie_rotation` | PointLight, SpotLight | Light cookie rotation in degrees |
| `overrides_json` | ClassComponent | Per-instance property overrides, raw JSON string |
| `target_part_name` | Joint | Name of the target skeleton/ragdoll part |
| `lower_limit` | Joint | Lower limit |
| `upper_limit` | Joint | Upper limit |
| `max_motor_force` | Joint | Max motor force |
| `break_force` | Joint | Break force |
| `break_torque` | Joint | Break torque |
| `reference_angle` | Joint | Reference angle |
| `local_axis_a` | Joint | Local axis `(x, y)` for Prismatic / Wheel |
| `linear_hertz` / `angular_hertz` | Joint | Motor joint spring frequencies |
| `linear_damping_ratio` / `angular_damping_ratio` | Joint | Motor joint damping ratios |
| `max_force` / `max_torque` | Joint | Motor joint force / torque caps |
| `correction_factor` | Joint | Motor joint correction factor |
| `linear_offset` | Joint | Motor joint linear offset `(x, y)` |

> ⚠️ `PointLight` and `SpotLight` instances have **no** `visible` or `render_in_game` field — use `enabled` instead.

---

## 11. Supported asset types

| Type | Extension | Description |
|-----|-----------|----------|
| Class | `.ice_class` | Entity class template |
| Level | `.icemap` | Level/scene |
| Tileset | `.ice_ts` | Tileset |
| Tilemap | `.ice_tm` | Tilemap |
| Flipbook | `.ice_flipbook` | Frame animation |
| Animation | `.ice_animation` | Animation state machine |
| Skeleton | `.ice_skeleton` | Skeletal animation rig |
| Sprite | `.ice_sprite` | Sprite |
| Material | `.ice_material` | Material |
| MaterialInstance | `.ice_matinst` | Material instance |
| MaterialFunction | `.ice_matfunc` | Material function |
| MaterialCollection | `.ice_mpc` | Material parameter collection |
| Sound | `.ice_sound` | Sound asset (sidecar for audio files) |
| FX | `.ice_fx` | FX system |
| Widget | `.ice_widget` | UI widget |
| View | `.ice_view` | View |
| Cinema | `.ice_cinema` | Cinematic |
| AI | `.ice_ai` | AI asset |
| TextureSettings | `.ice_texture` | Texture settings (sidecar for images) |
| FontAsset | `.ice_font` | Font settings (sidecar for fonts) |
| Localization | `.ice_localization` | Localization table |
| VideoSettings | `.ice_video` | Video settings (sidecar for video) |
| Texture | `.png`, `.jpg`, `.jpeg` | Raster images |
| Audio | `.wav`, `.mp3`, `.ogg`, `.flac` | Audio files |
| Font | `.ttf`, `.otf` | Fonts |
| Video | `.mp4`, `.avi`, `.mkv`, `.mov`, `.webm` | Video files |
| Script | `.lua` | Lua script |
| Text | `.txt` | Text file |

> ℹ️ Sidecar `.ice_sound` / `.ice_texture` / `.ice_font` / `.ice_video` files store import settings next to their source media. `open_asset` on a sidecar opens the editor for its source file.

---

## 12. Practical examples

### Example 1: Mass entity creation

Create a 10×10 grid of trees with equal spacing.

```python
folder_name = 'Forest'
editor.create_folder(folder_name)

for row in range(10):
    for col in range(10):
        uuid = editor.create_entity(f'Tree_{row}_{col}')
        editor.set_position(uuid, col * 150, row * 150)
        editor.set_sprite(uuid, 'Content/Textures/tree.png')
        editor.move_to_folder(uuid, folder_name)

print(f'Created {10*10} trees')
```

### Example 2: Find and replace sprites

Replace texture on all entities containing "Coin".

```python
coins = editor.find_entities('Coin')
for uuid in coins:
    editor.set_sprite(uuid, 'Content/Textures/gold_coin.png')
    editor.set_sprite_color(uuid, 1.0, 0.9, 0.0)

print(f'Updated {len(coins)} coins')
```

### Example 3: Organize entities into folders

Automatically organize entities into folders based on components.

```python
# Create folders
for folder in ['Physics', 'Lights', 'Cameras', 'UI']:
    editor.create_folder(folder)

# Distribute to folders
physics = editor.find_entities_by_component('Rigidbody')
editor.batch_move_to_folder(physics, 'Physics')

lights = editor.find_entities_by_component('PointLight')
lights += editor.find_entities_by_component('SpotLight')
editor.batch_move_to_folder(lights, 'Lights')

cameras = editor.find_entities_by_component('Camera')
editor.batch_move_to_folder(cameras, 'Cameras')

widgets = editor.find_entities_by_component('Widget')
editor.batch_move_to_folder(widgets, 'UI')

print('Entities organized!')
```

### Example 4: Scene report

Generate a text report about scene contents.

```python
names = editor.get_entity_names()
report = f'=== Scene Report ===\n'
report += f'Path: {scene.get_path()}\n'
report += f'Entities: {len(names)}\n'
report += f'FPS: {engine.fps():.0f}\n\n'

# Stats by component
types = editor.get_component_types()
for t in types:
    entities = editor.find_entities_by_component(t)
    if entities:
        report += f'  {t}: {len(entities)}\n'

report += f'\n--- Folders ---\n'
for folder in editor.get_folders():
    entities = editor.get_entities_in_folder(folder)
    report += f'  📁 {folder} ({len(entities)} entities)\n'

root = editor.get_root_entities()
report += f'  📄 Root ({len(root)} entities)\n'

print(report)

# Save to file
browser.write_file('Content/scene_report.txt', report)
```

### Example 5: Autosave by timer

```python
def autosave():
    if editor.is_dirty():
        scene.save()
        editor.log_info('Autosave done!')
    else:
        editor.log_info('No changes — skip autosave')

# Autosave every 5 minutes
autosave_id = editor.set_interval(300.0, autosave)
print(f'Autosave enabled (ID: {autosave_id})')

# To disable:
# editor.cancel_timer(autosave_id)
```

### Example 6: Performance monitoring

```python
def perf_monitor():
    fps = engine.fps()
    mem = engine.memory_stats()
    stats = engine.render_stats()

    if fps < 30:
        editor.log_warn(f'⚠️ Low FPS: {fps:.0f}')

    editor.log_info(
        f'FPS: {fps:.0f} | RAM: {mem["ram_mb"]}MB | '
        f'Draw Calls: {stats["draw_calls"]} | '
        f'Entities: {stats["entity_count"]}'
    )

# Monitor every 10 seconds
monitor_id = editor.set_interval(10.0, perf_monitor)
```

### Example 7: Arrange entities in a circle

```python
import math

center_x, center_y = 0, 0
radius = 300
count = 12

enemies = []
for i in range(count):
    angle = (2 * math.pi / count) * i
    x = center_x + radius * math.cos(angle)
    y = center_y + radius * math.sin(angle)

    uuid = editor.create_entity(f'Enemy_{i}')
    editor.set_position(uuid, x, y)
    editor.set_rotation(uuid, math.degrees(angle) + 90)
    enemies.append(uuid)

print(f'Created {len(enemies)} enemies in a circle')
```

### Example 8: Duplicate with modification

```python
# Duplicate entity and create mirrored version
original = editor.find_entity('Player')
if original:
    copy = editor.duplicate_entity(original)
    editor.set_entity_name(copy, 'Player_Mirror')

    # Mirror horizontally
    ox, oy, oz = editor.get_position(original)
    editor.set_position(copy, -ox, oy, oz)
    editor.set_scale(copy, -1, 1)
```

### Example 9: Lighting setup for entire scene

```python
# Find all point lights
lights = editor.find_entities_by_component('PointLight')

for uuid in lights:
    count = editor.get_instance_count(uuid, 'PointLight')
    for i in range(count):
        editor.set_instance(uuid, 'PointLight', i, {
            'color': (1.0, 0.9, 0.7),    # Warm light
            'intensity': 1.5,
            'radius': 400.0,
            'falloff': 2.5,
            'cast_shadows': True
        })

print(f'Updated {len(lights)} light sources')
```

### Example 10: Subscribe to events and logging

```python
def log_creation(uuid, name):
    editor.log_info(f'✨ Created: "{name}" (UUID: {uuid})')

def log_deletion(uuid):
    editor.log_warn(f'🗑️ Deleted: UUID {uuid}')

def log_selection():
    sel = editor.get_selected()
    if sel:
        editor.log_info(f'🔵 Selected: {", ".join(sel)}')
    else:
        editor.log_info('🔵 Selection cleared')

def log_save():
    editor.log_info(f'💾 Scene saved: {scene.get_path()}')

editor.on('entity_created', log_creation)
editor.on('entity_deleted', log_deletion)
editor.on('selection_changed', log_selection)
editor.on('scene_saved', log_save)

print('Event logging enabled!')
# To disable: editor.off('entity_created') etc.
```

### Example 11: Project asset inventory

```python
types_to_check = ['Class', 'Level', 'Flipbook', 'Sound', 'FX', 'Widget', 'Sprite']

report = '=== Asset Inventory ===\n\n'

for asset_type in types_to_check:
    ext = browser.get_asset_extension(asset_type)
    files = browser.find_by_extension(ext)
    report += f'{asset_type} ({ext}): {len(files)} files\n'
    for f in files[:5]:  # Show first 5
        size = browser.get_file_size(f)
        report += f'  • {browser.filename(f)} ({size/1024:.1f} KB)\n'
    if len(files) > 5:
        report += f'  ... and {len(files)-5} more\n'
    report += '\n'

print(report)
```

### Example 12: Export and move entity between scenes

```python
# Step 1: Export entity from current scene
player_uuid = editor.find_entity('Player')
json_data = scene.export_entity(player_uuid)

# Save to file
scene.save_entity(player_uuid, 'Content/Templates/player.json')

# Step 2: Load in another scene
scene.load('Content/Levels/level2.icemap')

# Step 3: Import
new_uuid = scene.load_entity('Content/Templates/player.json')
editor.set_position(new_uuid, 100, 100)
print(f'Player imported: {new_uuid}')
```

---

## 13. FAQ and troubleshooting

### Frequently asked questions

**Q: Where is the Python console?**
A: In the IceBox Engine editor, open it via **`Tools → Run Python Script`**. See [Opening the "Run Python Script" window](#opening-the-run-python-script-window) for a full tour of the panel.

**Q: Why does `editor.create_entity()` return 0?**
A: Most likely there is no active scene. Check with `engine.has_scene()`.

**Q: Why does `editor.set_position()` return `False`?**
A: The entity with the specified UUID was not found. Check UUID with `editor.entity_exists(uuid)`.

**Q: Can I undo operations from Python?**
A: Yes! Most functions automatically record state for Undo. Use `editor.undo()`.

**Q: How do I get an entity UUID?**
A: Use `editor.find_entity('Name')` or `editor.get_entity_uuids()`.

**Q: What's the difference between `editor.log_info()` and `icebox.log.info()`?**
A: `editor.log_info()` writes to the editor console (visible to the user). `icebox.log.info()` writes to the engine system log.

**Q: How do I use standard Python libraries?**
A: Standard modules (math, os, sys, re, etc.) are available via `import`:
```python
import math
import os
import re
```

**Q: Why doesn’t my timer fire?**
A: Timers are processed in the editor main loop. Make sure the editor is not blocked and the console is running.

**Q: How do I reset the console environment?**
A: Open **Show Console → Reset Environment**. That clears every variable, import and registered callback without restarting the editor.

### Tips

- 💡 Use `help` in the console for quick help
- 💡 UUIDs are big numbers. Don’t try to memorize them — use variables
- 💡 Batch operations (`batch_*`) are faster than calling single functions in a loop (Undo records one state per call, not per element)
- 💡 Use `editor.find_entities(regex)` for flexible search
- 💡 Subscribe to events (`editor.on()`) for reactive programming
- 💡 Autosave via `editor.set_interval()` is a great way to avoid losing work

### Common mistakes

```python
# ❌ Wrong — UUID as string
editor.set_position('12345', 100, 200)

# ✅ Right — UUID as number
editor.set_position(12345, 100, 200)

# ❌ Wrong — forgot to check return of find_entity
uuid = editor.find_entity('NonExistent')
editor.set_position(uuid, 100, 200)  # uuid = 0, entity not found

# ✅ Right
uuid = editor.find_entity('Player')
if uuid:
    editor.set_position(uuid, 100, 200)

# ❌ Wrong — wrong component name
editor.has_component(uuid, 'sprite_renderer')

# ✅ Right — case and name must match
editor.has_component(uuid, 'SpriteRenderer')
```

---

> 📌 **Available only in the editor**
