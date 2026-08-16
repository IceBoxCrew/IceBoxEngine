# 🧊 IceBox Engine — Lua API (Ice Scripting)

## Полная документация на русском языке

### Актуальная для версии B-0.8.3

> **IceBox Engine** использует **Lua** через библиотеку **sol2** для скриптинга игровой логики.
> Скрипты встраиваются в файлы `.ice_class` (классы объектов), `.icemap` (скрипт уровня),
> `.ice_widget` (виджеты UI).
>
> Эта документация описывает **абсолютно все** доступные Lua-функции с примерами.

---

## 📑 Содержание

1. [Архитектура и основы](#1-архитектура-и-основы)
   - [Режимы скриптинга: код и визуальный](#режимы-скриптинга-код-и-визуальный)
   - [Редактор классов: компоненты `.ice_class`](#редактор-классов-компоненты-ice_class)
   - [Редактор Lua-скриптов](#редактор-lua-скриптов)
2. [Основы языка Lua — Полный курс для новичков](#2-основы-языка-lua--полный-курс-для-новичков)
   - [Что такое Lua?](#что-такое-lua)
   - [Комментарии](#комментарии)
   - [Переменные и типы данных](#переменные-и-типы-данных)
   - [Операторы](#операторы)
   - [Условия (if / elseif / else)](#условия-if--elseif--else)
   - [Циклы (for, while, repeat)](#циклы-for-while-repeat)
   - [Функции](#функции)
   - [Таблицы — сердце Lua](#таблицы--сердце-lua)
   - [Строки и работа с ними](#строки-и-работа-с-ними)
   - [Стандартная библиотека math](#стандартная-библиотека-math)
   - [Стандартная библиотека string](#стандартная-библиотека-string)
   - [Стандартная библиотека table](#стандартная-библиотека-table)
   - [Область видимости и замыкания](#область-видимости-и-замыкания)
   - [Метатаблицы и ООП](#метатаблицы-и-ооп)
   - [Обработка ошибок (pcall, xpcall)](#обработка-ошибок-pcall-xpcall)
   - [Полезные паттерны и идиомы](#полезные-паттерны-и-идиомы)
   - [Модули и require](#модули-и-require)
   - [Итераторы и обобщённый for](#итераторы-и-обобщённый-for)
   - [Сборщик мусора](#сборщик-мусора)
   - [Стандартные библиотеки coroutine/io/os (если доступны)](#стандартные-библиотеки-coroutineioos-если-доступны)
3. [Жизненный цикл скриптов](#3-жизненный-цикл-скриптов)
4. [Transform — Позиция, масштаб, поворот](#4-transform--позиция-масштаб-поворот)
5. [Physics — Физика (Box2D)](#5-physics--физика-box2d)
6. [Input — Ввод (клавиатура, мышь, геймпад, тач)](#6-input--ввод-клавиатура-мышь-геймпад-тач)
7. [Entity — Сущности](#7-entity--сущности)
8. [Sprite — Спрайты](#8-sprite--спрайты)
9. [Flipbook — Покадровая анимация](#9-flipbook--покадровая-анимация)
10. [Animation — Аниматор (State Machine)](#10-animation--аниматор-state-machine)
   - [Skeleton — Костная анимация и рэгдолл](#105-skeleton--костная-анимация-и-рэгдолл)
11. [Camera — Камера](#11-camera--камера)
12. [Audio — Звук и музыка](#12-audio--звук-и-музыка)
13. [FX — Визуальные эффекты частиц](#13-fx--визуальные-эффекты-частиц)
   - [Point Light — Точечные источники света](#131-point-light--точечные-источники-света)
   - [Spot Light — Прожектор (направленный конус)](#132-spot-light--прожектор-направленный-конус)
   - [Освещение и тени — Глобальные настройки](#133-освещение-и-тени--глобальные-настройки)
   - [World Override — Постоянное переопределение уровня (Physics + Rendering)](#134-world-override--постоянное-переопределение-уровня-physics--rendering)
14. [Collision — Столкновения (AABB)](#14-collision--столкновения-aabb)
15. [Traces — Трассировка (Raycast и Shape Sweep)](#15-traces--трассировка-raycast-и-shape-sweep)
16. [Time — Время и таймеры](#16-time--время-и-таймеры)
17. [Tween — Плавные анимации значений](#17-tween--плавные-анимации-значений)
18. [Coroutine — Корутины](#18-coroutine--корутины)
19. [FSM — Конечный автомат состояний](#19-fsm--конечный-автомат-состояний)
20. [Scene — Сцены, сохранения, файлы](#20-scene--сцены-сохранения-файлы)
21. [Widget — UI виджеты](#21-widget--ui-виджеты)
22. [PostProcess — Пост-обработка](#22-postprocess--пост-обработка)
23. [Cinema — Кинематики](#23-cinema--кинематики)
24. [Settings — Настройки игры](#24-settings--настройки-игры)
25. [Math — Математика и шум](#25-math--математика-и-шум)
   - [RNG — Детерминированные потоки случайных чисел](#rng--детерминированные-потоки-случайных-чисел)
26. [Events — Система событий](#26-events--система-событий)
27. [Gameplay — Игровые системы](#27-gameplay--игровые-системы)
28. [Localization — Локализация](#28-localization--локализация)
29. [Debug — Отладка](#29-debug--отладка)
   - [Lua Script Debugger (текстовый и визуальный)](#lua-script-debugger-текстовый-и-визуальный)
   - [Отладочные функции: как использовать](#отладочные-функции-как-использовать)
30. [Tilemap — Тайловые карты](#30-tilemap--тайловые-карты)
31. [Component — Проверка и управление компонентами](#31-component--проверка-и-управление-компонентами)
32. [Network — Мультиплеер (Сеть)](#32-network--мультиплеер-сеть)
   - [Rollback — Детерминированный rollback-неткод (Rollback)](#325-rollback--детерминированный-rollback-неткод-rollback)
33. [Navigation — Навигация и поиск пути](#33-navigation--навигация-и-поиск-пути)
   - [Fog of War — Обзор и видимость](#335-fog-of-war--обзор-и-видимость)
34. [AI — Искусственный интеллект](#34-ai--искусственный-интеллект)
35. [Joint — Физические соединения (Box2D)](#35-joint--физические-соединения-box2d)
36. [PointMarker — Точечные маркеры](#36-pointmarker--точечные-маркеры)
37. [DataUtils — Структуры данных и утилиты](#37-datautils--структуры-данных-и-утилиты)
38. [Material — Материалы и шейдеры](#38-material--материалы-и-шейдеры)
39. [Destruction — Разрушения](#39-destruction--разрушения)
40. [Практические примеры](#40-практические-примеры)
41. [Mods — Система модов](#41-mods--система-модов)
   - [Mods Lua API (`Mods.*`)](#4110-mods--lua-api-для-управления-модами)
42. [DLC — Дополнительный контент](#42-dlc--дополнительный-контент)
43. [Ads — Реклама (Google AdMob)](#43-ads--реклама-google-admob)
44. [IAP — Внутриигровые покупки (Google Play Billing)](#44-iap--внутриигровые-покупки-google-play-billing)
45. [PlayGames — Google Play Games Services](#45-playgames--google-play-games-services)
46. [SavedGames — Облачные сохранения (Google Play)](#46-savedgames--облачные-сохранения-google-play)
47. [Firebase — Firebase Analytics](#47-firebase--firebase-analytics)
48. [Notifications — Push- и локальные уведомления](#48-notifications--push--и-локальные-уведомления)
49. [Consent — Согласие GDPR (UMP)](#49-consent--согласие-gdpr-ump)
50. [Review — Внутриигровой отзыв](#50-review--внутриигровой-отзыв)
51. [Bluetooth — Bluetooth-коммуникация](#51-bluetooth--bluetooth-коммуникация)
52. [DeepLinks — Глубокие ссылки](#52-deeplinks--глубокие-ссылки)
53. [Permissions — Разрешения Android](#53-permissions--разрешения-android)
54. [Web3 — Интеграция с блокчейном (Ethereum / BNB Smart Chain)](#54-web3--интеграция-с-блокчейном-ethereum--bnb-smart-chain)
55. [LocalPlayer — Локальный мультиплеер и сплит-скрин](#55-localplayer--локальный-мультиплеер-и-сплит-скрин)
56. [Video — Воспроизведение видео](#56-video--воспроизведение-видео)
57. [Voice — Микрофон, Opus, анализ голоса](#57-voice--микрофон-opus-анализ-голоса)
58. [Replay — Запись, воспроизведение, killcam](#58-replay--запись-воспроизведение-killcam)
59. [Matchmaking — Подбор игроков](#59-matchmaking--подбор-игроков)
60. [Console — Консоль разработчика и система команд](#60-console--консоль-разработчика-и-система-команд)
61. [Draw — Немедленная отрисовка (Draw / Texture / RenderTarget)](#61-draw--немедленная-отрисовка-draw--texture--rendertarget)

---

## 1. Архитектура и основы

### Что такое Ice Scripting?

**Ice Scripting** — визуальная + текстовая система скриптинга IceBox Engine. Логика пишется на **Lua** — легком и быстром языке, либо собирается визуально как нодовый граф.

### Режимы скриптинга: код и визуальный

IceBox предлагает два способа создавать игровую логику. Режим выбирается **один раз при создании проекта** (в лаунчере) и применяется ко всем скриптуемым ассетам — `.ice_class`, `.ice_widget` и `.icemap`.

| Режим | Что вы делаете | Редактор |
|-------|----------------|----------|
| **Кодовый скриптинг** | Пишете Lua напрямую | Встроенный текстовый редактор кода |
| **Визуальный скриптинг** | Соединяете ноды на графе | Редактор нодового графа |

**Кодовый скриптинг** — классический подход, описанный во всём этом документе: вы печатаете Lua в редакторе кода.

**Визуальный скриптинг** заменяет редактор кода **нодовым графом**. Вместо набора текста вы перетаскиваете ноды и соединяете их:

- **Ноды-события** — точки входа (`On Create`, `On Update`, `On Collision Enter`, …) — те же колбэки жизненного цикла, что перечислены в разделе [Жизненный цикл скриптов](#3-жизненный-цикл-скриптов).
- **Ноды действий / значений** оборачивают Lua-функции движка (`Set Position`, `Is Key Pressed`, `Add Force`, `Play Sound`, …). Каждая глобальная функция из этого документа доступна как нода, а для самых частых категорий (Transform, Physics, Input, Entity, Audio, Camera и др.) есть типизированные ноды.
- **Ноды управления потоком** добавляют структуру, нужную визуальному графу: `Branch` (условие), `Sequence`, `For Loop`, `While`, `For Each`, `Do Once`, `Flip Flop`, `Do N`, `Delay`.
- **Ноды переменных** (`Get` / `Set`) и **ноды математики и логики** (`+`, `-`, `>`, `AND`, `Make Vec2`, …) позволяют вычислять и хранить значения.

Под капотом граф **компилируется в точно такой же Lua** и исполняется тем же движком. Это значит:

- Оба режима используют **один API и один жизненный цикл** — всё в этой документации применимо к обоим. Нода просто представляет вызов Lua.
- **Разницы в рантайме нет**; визуальный проект ведёт себя идентично эквивалентному кодовому.

Выбранный режим хранится как `"ScriptingMode": "Code"` или `"Visual"` в файле проекта `.iceproject`. Визуальные графы сохраняются внутри ассета рядом со сгенерированным скриптом, поэтому ассет всегда открывается в том редакторе, в котором вы его создавали.

**Работа в нодовом графе:**
- **Правый клик** по холсту открывает палитру нод с поиском — выберите ноду, чтобы разместить её.
- **Потяните от пина** к другому пину, чтобы соединить ноды; отпустите на пустом месте, чтобы выбрать и сразу подключить новую ноду.
- **Белые пины/провода** — поток исполнения (порядок выполнения); **цветные пины/провода** — данные (значения), окрашенные по типу.
- Добавляйте переменные в панели **Variables**; выделите ноду, чтобы отредактировать её параметры справа.

> Заголовки и категории нод намеренно на английском.

### Типы файлов

| Файл | Расширение | Описание |
|------|-----------|----------|
| **Класс (Class)** | `.ice_class` | Объект на сцене (игрок, враг, предмет). Содержит компоненты + Lua-скрипт |
| **Карта (Map)** | `.icemap` | Уровень/сцена. Может содержать скрипт уровня (Level Script) |
| **Виджет (Widget)** | `.ice_widget` | UI-элемент (HUD, меню, инвентарь) |

### Как это работает?

1. Вы создаёте **`.ice_class`** (класс) в редакторе
2. Добавляете **компоненты** (спрайт, физика, коллайдер и т.д.)
3. Пишете **Lua-скрипт** в встроенном редакторе кода
4. Размещаете класс на сцене (`.icemap`) — **`OnConstruct`** выполняется сразу в редакторе
5. Нажимаете **Play** — движок запускает `OnConstruct` → `OnCreate` → `OnUpdate` каждый кадр

### Два типа API

| Тип | Доступ | Пример |
|-----|--------|--------|
| **Глобальные функции** | Доступны отовсюду | `IsKeyPressed("space")`, `GetDeltaTime()` |
| **Функции сущности (Entity)** | Только внутри скрипта конкретной сущности | `SetPosition(x, y, z)`, `GetVelocity()` |

> **Важно:** Функции сущности (Entity-bound) работают с «самим собой» — то есть с объектом, к которому привязан скрипт. Например, `SetPosition(100, 200, 0)` сдвинет именно ту сущность, в чьём скрипте написана эта строка.

### Наследование классов

IceBox поддерживает **наследование классов**. Если в `.ice_class` указано поле `ParentClass`, то:
- Компоненты родителя наследуются
- Lua-функции родителя доступны через таблицу `Parent`

```lua
-- Дочерний класс
function OnCreate()
    -- Вызвать OnCreate родительского класса
    if Parent and Parent.OnCreate then
        Parent.OnCreate()
    end
    -- Собственная инициализация
    health = 100
end
```

### Дочерние сущности (Children)

`.ice_class` может содержать **дочерние сущности** (children). Такие сущности создаются и живут вместе с родителем как часть одного класса:

- Дочерние сущности автоматически наследуют трансформацию родителя и следуют за ним.
- Их можно использовать для оружия, эффектов, маркеров, UI, дополнительных коллайдеров и т.д.
- В рантайме ими можно управлять через API иерархии (`AttachToEntity`, `GetChildEntities`, `GetParentEntity`, `SetLocalOffset`).

### Компонент из класса (ClassComponent)

`.ice_class` может быть подключён как **компонент** другого класса (ClassComponent). Это позволяет переиспользовать готовые наборы компонентов и логики:

- Компонент-класс добавляется как обычный компонент в редакторе класса.
- Такой компонент считается частью сущности и наследует её жизненный цикл.
- В рантайме доступна информация о подключённых ClassComponent через API (`GetClassComponentCount`, `GetClassComponentPath`, `FindClassComponentIndex`).

### Health — Система здоровья

Готовая логика HP с неуязвимостью, щитом и коллбеками.

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

### Inventory — Инвентарь

```lua
local inv = Inventory(20)   -- максимум слотов (0 = без лимита)

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

-- Перенос предметов между инвентарями
local other = Inventory(10)
local moved = inv.TransferTo(other, "potion", 2)
```

### Dialog — Диалоги

```lua
local dlg = Dialog({
    start = { text = "Привет!", next = "q1" },
    q1 = {
        speaker = "NPC",
        text = "Куда идём?",
        choices = {
            { text = "В город", next = "city" },
            { text = "В лес", next = "forest" }
        }
    },
    city = { text = "Пойдём в город.", next = "end" },
    forest = { text = "Пойдём в лес.", next = "end" },
    end = { text = "Удачи!" }
})

dlg.SetCallbacks({
    onText = function(speaker, text) Print(speaker .. ": " .. text) end,
    onChoice = function(speaker, text, choices) Print(text) end,
    onEnd = function() Print("Диалог завершён") end,
    onEvent = function(eventName) Print("Событие: " .. eventName) end
})

dlg.Start("start")
dlg.Next(1)  -- выбрать вариант
local active = dlg.IsActive()
local current = dlg.GetCurrent()
local history = dlg.GetHistory()
dlg.SetVar("reputation", 10)
local rep = dlg.GetVar("reputation")
dlg.Skip()
```

### QuestLog — Журнал квестов

```lua
local ql = QuestLog()
ql.SetCallbacks({
    onQuestAdded = function(id, quest) Print("Квест: " .. id) end,
    onQuestComplete = function(id) Print("Квест завершён: " .. id) end,
    onQuestFailed = function(id) Print("Квест провален: " .. id) end,
    onObjectiveUpdate = function(qid, oid, cur, target)
        Print(qid .. ": " .. oid .. " " .. cur .. "/" .. target)
    end
})

ql.AddQuest({
    id = "find_artifact",
    title = "Найти артефакт",
    description = "Исследуйте руины",
    objectives = {
        { id = "enter_ruins", text = "Войти в руины", target = 1 },
        { id = "collect", text = "Собрать артефакт", target = 1 }
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

### ScoreSystem — Очки и комбо

```lua
local score = ScoreSystem({
    highScore = 1000,
    comboTimeout = 2.0,
    comboMultiplierStep = 0.1
})

score.SetCallbacks({
    onScoreChanged = function(total, delta) Print("+" .. delta) end,
    onComboChanged = function(combo) Print("Combo: " .. combo) end,
    onComboBroken = function(combo) Print("Сброшено: " .. combo) end,
    onHighScore = function(value) Print("Новый рекорд: " .. value) end
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

### Grid — 2D сетка

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

### LootTable — Таблица лута

```lua
local loot = LootTable({
    { id = "gold", weight = 10, minAmount = 1, maxAmount = 5 },
    { id = "potion", weight = 2, minAmount = 1, maxAmount = 1 }
})

local drops = loot.Roll(3)       -- 3 броска (могут повторяться)
local unique = loot.RollUnique(2) -- 2 уникальных награды
```

### StatusEffects — Эффекты статуса

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

### StatBlock — Блок характеристик

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

### TurnManager — Пошаговые бои

```lua
local tm = TurnManager({
    onTurnStart = function(actor, idx) Print("Ход", idx) end,
    onTurnEnd = function(actor, idx) Print("Конец хода", idx) end,
    onRoundStart = function(round) Print("Раунд", round) end,
    onRoundEnd = function(round) Print("Раунд конец", round) end
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

> **Примечание:** достижения полностью документированы в [Секции 27 — Игровые системы](#27-gameplay--игровые-системы).

### Str — Строковые утилиты

> **Лёгкие строковые хелперы.** Для расширенных операций (`TrimLeft`, `TrimRight`, `IsEmpty`, `IsBlank`, `ReplaceFirst`, `Reverse`, `Find`, `Count`, `CharAt`, `ToNumber`, `Byte`, `Char`, `Join`) см. [`String.*` в секции 37](#string--строковые-утилиты).

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

### Редактор классов: компоненты `.ice_class`

В редакторе классов есть вкладка `Components`, где настраиваются компоненты класса. Основные возможности:

- Добавление компонентов через кнопку `Добавить компонент`.
- Удаление компонента кнопкой `X` в заголовке компонента.
- Поддержка множественных инстансов у компонентов (например `SpriteRenderer`, `Flipbook`, `Audio`, `FX`, `Widget`, `Light`, `PointMarker`, `Tilemap`, `Joint`).
- Выбор активного инстанса кликом по элементу списка.
- Наследуемые компоненты отмечаются как `(P)` и отображаются в отдельной вкладке `Inheritance`.

Список доступных компонентов зависит от типа класса, но в `Class Editor` доступны базовые: `Transform`, `SpriteRenderer`, `Collider`, `TilemapRenderer`, `Rigidbody`, `Animator`, `Flipbook`, `Camera`, `Audio`, `FX`, `Widget`, `PointLight`, `SpotLight`, `PointMarker`, `AI`, `Joint`.

### Редактор Lua-скриптов

Внутри `Class Editor` встроен редактор Lua-скриптов для `.ice_class`:

- `Save`/`Ctrl+S` сохраняет класс целиком.
- `Compile` проверяет скрипт на ошибки и **выполняет `OnConstruct`** в viewport класса.
- Отображение строки/колонки курсора и количества строк.
- Просмотр родительского скрипта в режиме только чтения (если есть наследование).
- Автодополнение на основе базы Lua API.
- Поиск/замена, переход к строке, список функций.
- Быстрые вставки (snippets) для типовых коллбеков.
- Сворачивание блоков кода.

---

## 2. Основы языка Lua — Полный курс для новичков

> **Эта секция** описывает сам **язык Lua** — его синтаксис, типы данных, управляющие конструкции, функции, таблицы и стандартную библиотеку.
> Если вы **никогда не программировали** или переходите с другого языка — прочитайте эту секцию **целиком**, прежде чем переходить к API движка.
> Всё, что описано здесь — **базовый Lua**, который работает в любом Lua-приложении, не только в IceBox Engine.

---

### Что такое Lua?

**Lua** (от португальского «луна») — лёгкий, быстрый и простой скриптовый язык. Его используют во многих коммерческих играх, встраиваемых системах и, конечно, в IceBox Engine.

**Почему Lua?**
- Простой синтаксис — можно выучить за день
- Динамическая типизация — не нужно указывать типы
- Таблицы — один тип данных заменяет массивы, словари и объекты
- Быстрый — один из самых быстрых скриптовых языков
- Легко встраивается в C/C++ движки

---

### Комментарии

Комментарии — текст, который движок полностью игнорирует. Нужны для пояснений.

```lua
-- Это однострочный комментарий (всё после -- игнорируется)

--[[
    Это многострочный комментарий.
    Можно писать сколько угодно строк.
    Удобно для временного отключения кода.
]]

-- Совет: используйте комментарии, чтобы объяснять ЗАЧЕМ, а не ЧТО делает код
speed = 200  -- Скорость движения игрока (пикс/сек)
```

---

### Переменные и типы данных

Переменная — это **имя**, которому вы присваиваете **значение**. В Lua не нужно указывать тип — язык определяет его автоматически.

#### Создание переменных

```lua
-- Глобальная переменная (видна отовсюду в текущем скрипте)
health = 100
name = "Игрок"
isAlive = true

-- Локальная переменная (видна только внутри текущего блока/функции)
local speed = 200
local score = 0
local playerName = "Герой"
```

> **Правило:** Всегда используйте `local`, если переменная нужна только внутри функции. Глобальные переменные (без `local`) видны везде в текущем скрипте `.ice_class` и сохраняются между вызовами `OnUpdate`.

#### Типы данных

В Lua есть **8 типов данных**. Вот основные, которые нужны для игр:

| Тип | Описание | Пример |
|-----|----------|--------|
| **nil** | Пустота, отсутствие значения | `local x = nil` |
| **boolean** | Логический: `true` или `false` | `local alive = true` |
| **number** | Число (целое или дробное) | `local hp = 100`, `local pi = 3.14` |
| **string** | Строка текста | `local name = "Герой"` |
| **table** | Таблица (массив, словарь, объект) | `local pos = {x = 10, y = 20}` |
| **function** | Функция (блок кода) | `local fn = function() end` |

```lua
-- nil — значит «ничего», «не существует»
local weapon = nil         -- У игрока нет оружия
if weapon == nil then
    Print("Оружие не выбрано!")
end

-- boolean — логические значения
local isAlive = true
local isDead = false

-- number — числа (Lua не различает int и float)
local health = 100         -- Целое число
local speed = 250.5        -- Дробное число
local negative = -42       -- Отрицательное
local big = 1e6            -- 1000000 (научная нотация)

-- string — текст (в одинарных или двойных кавычках)
local name = "Игрок"
local dialog = 'Привет, мир!'
local multiline = [[
    Это многострочная строка.
    Можно писать на нескольких строках
    без специальных символов.
]]

-- Определить тип переменной
Print(type(42))        -- "number"
Print(type("hello"))   -- "string"
Print(type(true))      -- "boolean"
Print(type(nil))       -- "nil"
Print(type({}))        -- "table"
```

#### Множественное присваивание

```lua
-- Lua позволяет присваивать несколько переменных за раз
local x, y, z = 100, 200, 0
local a, b = 1, 2

-- Обмен значениями (swap) — без временной переменной!
a, b = b, a  -- Теперь a=2, b=1

-- Если значений меньше чем переменных — остальные будут nil
local p, q, r = 1, 2
-- p=1, q=2, r=nil
```

#### Правила именования переменных

```lua
-- ✅ Допустимо:
local health = 100
local playerName = "Hero"      -- camelCase (рекомендуется)
local max_speed = 300           -- snake_case (тоже нормально)
local _privateVar = 42          -- Начинается с _
local item1 = "Sword"          -- Буквы + цифры

-- ❌ Недопустимо:
-- local 1item = "Sword"       -- Нельзя начинать с цифры
-- local my-var = 10            -- Нельзя использовать дефис
-- local my var = 10            -- Нельзя использовать пробелы
-- local function = 10          -- Нельзя использовать зарезервированные слова
```

**Зарезервированные слова** (нельзя использовать как имена переменных):
`and`, `break`, `do`, `else`, `elseif`, `end`, `false`, `for`, `function`, `goto`, `if`, `in`, `local`, `nil`, `not`, `or`, `repeat`, `return`, `then`, `true`, `until`, `while`

---

### Операторы

#### Арифметические операторы

```lua
local a = 10
local b = 3

Print(a + b)    -- 13     Сложение
Print(a - b)    -- 7      Вычитание
Print(a * b)    -- 30     Умножение
Print(a / b)    -- 3.333  Деление (всегда дробное!)
Print(a % b)    -- 1      Остаток от деления (модуль)
Print(a ^ b)    -- 1000   Возведение в степень (10³)
Print(-a)       -- -10    Унарный минус

-- Целочисленное деление (Lua 5.3+)
Print(a // b)   -- 3      Отбрасывает дробную часть
```

#### Операторы сравнения

```lua
-- Все операторы возвращают true или false

Print(10 == 10)    -- true    Равно
Print(10 ~= 5)     -- true    НЕ равно (внимание: ~= а не !=)
Print(10 > 5)      -- true    Больше
Print(10 < 5)      -- false   Меньше
Print(10 >= 10)    -- true    Больше или равно
Print(10 <= 5)     -- false   Меньше или равно

-- ⚠️ Важно: в Lua оператор «не равно» пишется как ~= (тильда + равно), а не != 
```

#### Логические операторы

```lua
local a = true
local b = false

Print(a and b)     -- false   И (оба true)
Print(a or b)      -- true    ИЛИ (хотя бы одно true)
Print(not a)       -- false   НЕ (инверсия)

-- Сложные условия
local hp = 50
local hasPotion = true
if hp < 100 and hasPotion then
    Print("Можно лечиться!")
end

-- ═══════════════════════════════════
-- ВАЖНАЯ ОСОБЕННОСТЬ LUA:
-- and и or возвращают НЕ true/false, а одно из значений!
-- ═══════════════════════════════════

-- and: если первое ложно → возвращает первое, иначе → второе
Print(nil and "hello")     -- nil
Print("hi" and "hello")   -- "hello"

-- or: если первое истинно → возвращает первое, иначе → второе
Print(nil or "default")    -- "default"
Print("hi" or "default")  -- "hi"

-- Это даёт мощный паттерн «значение по умолчанию»:
local name = playerName or "Безымянный"
-- Если playerName == nil → name будет "Безымянный"

-- Эмуляция тернарного оператора (a ? b : c из других языков):
local label = (hp > 50) and "Здоров" or "Ранен"
-- hp=80 → "Здоров", hp=20 → "Ранен"
```

#### Оператор конкатенации строк

```lua
-- Две точки (..) соединяют строки
local firstName = "Ледяной"
local lastName = "Боксёр"
local fullName = firstName .. " " .. lastName
Print(fullName)  -- "Ледяной Боксёр"

-- Числа автоматически конвертируются в строки при конкатенации
local score = 42
Print("Очки: " .. score)     -- "Очки: 42"
Print("HP: " .. 100 .. "/100")  -- "HP: 100/100"
```

#### Оператор длины

```lua
-- # возвращает длину строки или массива
local name = "Hello"
Print(#name)  -- 5

local items = {"Sword", "Shield", "Potion"}
Print(#items) -- 3
```

---

### Условия (if / elseif / else)

Условия позволяют выполнять разный код в зависимости от ситуации.

#### Базовый if

```lua
local hp = 75

if hp <= 0 then
    Print("Персонаж мёртв!")
end
```

#### if / else

```lua
local hp = 75

if hp <= 0 then
    Print("Мёртв!")
else
    Print("Жив! HP: " .. hp)
end
```

#### if / elseif / else

```lua
local hp = 75

if hp <= 0 then
    Print("Мёртв!")
elseif hp < 25 then
    Print("Критическое состояние!")
elseif hp < 50 then
    Print("Ранен")
elseif hp < 75 then
    Print("Слегка ранен")
else
    Print("Здоров!")
end
```

#### Вложенные условия

```lua
local hasKey = true
local doorLocked = true

if doorLocked then
    if hasKey then
        Print("Открываю дверь ключом!")
    else
        Print("Дверь заперта, нужен ключ!")
    end
else
    Print("Дверь открыта, прохожу!")
end
```

#### Что считается true и false?

```lua
-- В Lua только ДВА значения ложные: nil и false
-- ВСЁ ОСТАЛЬНОЕ — true (даже 0, пустая строка "" и пустая таблица {})

if 0 then Print("0 — это true в Lua!") end           -- Выведет!
if "" then Print("Пустая строка — true!") end         -- Выведет!
if {} then Print("Пустая таблица — true!") end        -- Выведет!

if nil then Print("Не выведется") end                 -- НЕ выведет
if false then Print("Не выведется") end               -- НЕ выведет

-- ⚠️ Это ОТЛИЧИЕ от многих других языков! В Python, C++, JavaScript 
-- число 0 и пустая строка считаются ложными, в Lua — нет!
```

---

### Циклы (for, while, repeat)

Циклы выполняют код многократно.

#### Числовой for

```lua
-- for переменная = старт, конец, шаг do ... end
-- Шаг по умолчанию = 1

-- Считаем от 1 до 5
for i = 1, 5 do
    Print("Итерация: " .. i)
end
-- Выведет: 1, 2, 3, 4, 5

-- С шагом 2
for i = 0, 10, 2 do
    Print(i)
end
-- Выведет: 0, 2, 4, 6, 8, 10

-- Обратный отсчёт (шаг -1)
for i = 5, 1, -1 do
    Print(i .. "...")
end
Print("Пуск!")
-- Выведет: 5... 4... 3... 2... 1... Пуск!
```

#### Обобщённый for (для таблиц)

```lua
-- ipairs — для массивов (по порядку, от 1)
local fruits = {"Яблоко", "Банан", "Вишня"}
for index, value in ipairs(fruits) do
    Print(index .. ": " .. value)
end
-- 1: Яблоко
-- 2: Банан
-- 3: Вишня

-- pairs — для словарей (порядок НЕ гарантирован)
local player = { name = "Герой", hp = 100, level = 5 }
for key, value in pairs(player) do
    Print(key .. " = " .. tostring(value))
end
-- name = Герой
-- hp = 100
-- level = 5
```

#### while

```lua
-- while условие do ... end
-- Выполняется ПОКА условие истинно

local countdown = 5
while countdown > 0 do
    Print(countdown)
    countdown = countdown - 1   -- В Lua нет ++, --, +=
end
Print("Поехали!")

-- ⚠️ Осторожно! Если условие никогда не станет false — бесконечный цикл!
-- В IceBox это заморозит игру!
```

#### repeat...until

```lua
-- repeat ... until условие
-- Как while, но проверяет условие ПОСЛЕ выполнения (гарантирует минимум 1 итерацию)

local input = ""
repeat
    Print("Введите 'да' для продолжения")
    input = "да"  -- В реальности тут был бы ввод пользователя
until input == "да"
```

#### break — досрочный выход из цикла

```lua
-- break прерывает ближайший цикл

local enemies = {"Гоблин", "Орк", "Дракон", "Слайм"}
local found = nil

for i, enemy in ipairs(enemies) do
    if enemy == "Дракон" then
        found = enemy
        Print("Нашёл дракона на позиции " .. i .. "!")
        break  -- Выходим из цикла, не проверяя остальных
    end
end
```

#### Практический пример: Спавн врагов

```lua
function SpawnWave(count)
    for i = 1, count do
        local x = RandomRange(-400, 400)
        local y = -300
        SpawnEntity("Content/Classes/Enemy.ice_class", x, y)
    end
    Print("Волна: " .. count .. " врагов!")
end
```

---

### Функции

Функция — это **именованный блок кода**, который можно вызывать многократно.

#### Объявление и вызов

```lua
-- Объявление функции
function SayHello()
    Print("Привет, мир!")
end

-- Вызов функции
SayHello()  -- Выведет: Привет, мир!
SayHello()  -- Можно вызывать сколько угодно раз
```

#### Параметры (аргументы)

```lua
-- Функция с параметрами
function Greet(name)
    Print("Привет, " .. name .. "!")
end

Greet("Игрок")   -- Привет, Игрок!
Greet("Враг")    -- Привет, Враг!

-- Несколько параметров
function DealDamage(target, amount, damageType)
    Print(target .. " получил " .. amount .. " урона (" .. damageType .. ")")
end

DealDamage("Гоблин", 50, "огонь")
```

#### Значения по умолчанию

```lua
-- В Lua нет встроенных дефолтных параметров, но есть паттерн:
function CreateEnemy(name, hp, speed)
    name = name or "Враг"       -- Если nil → "Враг"
    hp = hp or 100              -- Если nil → 100
    speed = speed or 150        -- Если nil → 150
    Print(name .. " создан: HP=" .. hp .. " Speed=" .. speed)
end

CreateEnemy("Дракон", 500, 80)   -- Дракон создан: HP=500 Speed=80
CreateEnemy("Слайм")              -- Слайм создан: HP=100 Speed=150
CreateEnemy()                     -- Враг создан: HP=100 Speed=150
```

#### Возврат значений (return)

```lua
-- Одно возвращаемое значение
function Add(a, b)
    return a + b
end

local sum = Add(10, 20)
Print(sum)  -- 30

-- Несколько возвращаемых значений (фишка Lua!)
function GetPlayerInfo()
    return "Герой", 100, 5  -- имя, HP, уровень
end

local name, hp, level = GetPlayerInfo()
Print(name .. " Lvl " .. level)  -- Герой Lvl 5

-- Пример: функция расчёта урона
function CalculateDamage(baseDmg, armor)
    local reduction = armor * 0.5
    local finalDmg = math.max(baseDmg - reduction, 1)
    local isCrit = math.random() < 0.2  -- 20% шанс крита
    if isCrit then
        finalDmg = finalDmg * 2
    end
    return finalDmg, isCrit
end

local dmg, crit = CalculateDamage(50, 20)
if crit then
    Print("КРИТ! " .. dmg .. " урона!")
else
    Print(dmg .. " урона")
end
```

#### Локальные функции

```lua
-- Глобальная функция (видна отовсюду в скрипте)
function Attack()
    Print("Атака!")
end

-- Локальная функция (видна только в текущей области)
local function CalculateBonus(level)
    return level * 10
end

-- Альтернативный синтаксис локальной функции
local Heal = function(amount)
    Print("Лечение на " .. amount)
end
```

#### Анонимные функции (лямбды)

```lua
-- Функция без имени — передаётся как аргумент
Delay(2.0, function()
    Print("Прошло 2 секунды!")
end)

-- Подписка на событие
On("PlayerDied", function()
    Print("Игрок погиб!")
end)

-- Сортировка с пользовательским сравнением
local scores = {40, 10, 80, 25}
table.sort(scores, function(a, b)
    return a > b  -- По убыванию
end)
-- scores = {80, 40, 25, 10}
```

#### Переменное число аргументов (varargs)

```lua
-- ... принимает любое количество аргументов
function PrintAll(...)
    local args = {...}  -- Собрать в таблицу
    for i, v in ipairs(args) do
        Print(tostring(v))
    end
end

PrintAll("Привет", 42, true, "Мир")
-- Привет
-- 42
-- true
-- Мир

-- Полезный пример: форматированный лог
function Log(prefix, ...)
    local args = {...}
    local msg = prefix .. ": "
    for i, v in ipairs(args) do
        if i > 1 then msg = msg .. ", " end
        msg = msg .. tostring(v)
    end
    Print(msg)
end

Log("INFO", "Игрок создан", "HP=100")
-- INFO: Игрок создан, HP=100
```

---

### Таблицы — сердце Lua

Таблица (`table`) — **единственная структура данных** в Lua. Она заменяет массивы, словари (hash map), списки, множества и даже объекты.

#### Массив (последовательная таблица)

```lua
-- Массив — таблица с числовыми индексами (начиная с 1, НЕ с 0!)
local inventory = {"Меч", "Щит", "Зелье"}

-- Доступ по индексу (1-based!)
Print(inventory[1])   -- "Меч"
Print(inventory[2])   -- "Щит"
Print(inventory[3])   -- "Зелье"
Print(inventory[4])   -- nil (нет элемента)

-- Длина массива
Print(#inventory)     -- 3

-- Добавить в конец
table.insert(inventory, "Лук")
Print(#inventory)     -- 4

-- Добавить на позицию
table.insert(inventory, 2, "Шлем")
-- {"Меч", "Шлем", "Щит", "Зелье", "Лук"}

-- Удалить по позиции (и получить удалённый элемент)
local removed = table.remove(inventory, 3)
Print(removed)  -- "Щит"

-- Удалить последний
local last = table.remove(inventory)

-- Перебор массива
for i, item in ipairs(inventory) do
    Print(i .. ") " .. item)
end
```

> **⚠️ Важно!** В Lua массивы начинаются с индекса **1**, а не с 0. Это одно из главных отличий от C, C++, JavaScript, Python и т.д.

#### Словарь (ассоциативная таблица)

```lua
-- Словарь — таблица со строковыми ключами
local player = {
    name = "Герой",
    hp = 100,
    maxHp = 100,
    level = 1,
    isAlive = true
}

-- Доступ через точку
Print(player.name)      -- "Герой"
Print(player.hp)        -- 100

-- Доступ через квадратные скобки (для динамических ключей)
Print(player["name"])   -- "Герой"
local key = "hp"
Print(player[key])      -- 100

-- Изменение значений
player.hp = 75
player.level = player.level + 1

-- Добавление нового поля
player.weapon = "Меч"
player.armor = 20

-- Удаление поля (присвоить nil)
player.weapon = nil

-- Перебор словаря
for key, value in pairs(player) do
    Print(key .. " = " .. tostring(value))
end
```

#### Смешанная таблица

```lua
-- Таблица может содержать и массивную, и словарную часть
local item = {
    -- Словарная часть
    name = "Огненный меч",
    damage = 50,
    type = "weapon",

    -- Массивная часть
    "enchant_fire",      -- [1]
    "enchant_glow",      -- [2]
}

Print(item.name)    -- "Огненный меч"
Print(item[1])      -- "enchant_fire"
```

#### Вложенные таблицы

```lua
-- Таблица может содержать другие таблицы (как вложенные объекты)
local party = {
    {
        name = "Воин",
        hp = 200,
        skills = {"Удар", "Защита", "Провокация"}
    },
    {
        name = "Маг",
        hp = 80,
        skills = {"Огненный шар", "Лечение", "Телепорт"}
    },
    {
        name = "Лучник",
        hp = 120,
        skills = {"Выстрел", "Мультивыстрел", "Уклонение"}
    }
}

-- Доступ к вложенным данным
Print(party[1].name)           -- "Воин"
Print(party[2].skills[1])     -- "Огненный шар"
Print(#party)                   -- 3 (три персонажа)

-- Перебор группы
for i, member in ipairs(party) do
    Print(member.name .. " (HP: " .. member.hp .. ")")
    for _, skill in ipairs(member.skills) do
        Print("  - " .. skill)
    end
end
```

#### Таблица как конфигурация

```lua
-- Таблицы идеальны для передачи настроек в функции
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

#### Проверка существования ключа

```lua
local data = { name = "Герой", hp = 100 }

-- Проверка: есть ли ключ?
if data.name ~= nil then
    Print("Имя: " .. data.name)
end

-- Короткая запись (idiomatic Lua)
if data.name then
    Print("Имя: " .. data.name)
end

-- Безопасный доступ к вложенным данным
local weapon = data.equipment and data.equipment.weapon
-- Если data.equipment == nil, weapon будет nil (без ошибки)
```

---

### Строки и работа с ними

#### Объявление строк

```lua
local s1 = "Двойные кавычки"
local s2 = 'Одинарные кавычки'
local s3 = [[Многострочная
строка без экранирования]]

-- Экранирование спецсимволов
local s4 = "Он сказал: \"Привет!\""
local s5 = "Строка 1\nСтрока 2"     -- \n = перенос строки
local s6 = "Табуляция:\tзначение"     -- \t = таб
local s7 = "Обратный слеш: \\"       -- \\ = один \
```

#### Конкатенация (соединение)

```lua
local first = "Ледяной"
local last = "Движок"
local full = first .. " " .. last  -- "Ледяной Движок"

-- Числа автоматически конвертируются
local msg = "HP: " .. 100 .. "/" .. 100  -- "HP: 100/100"
```

#### Длина строки

```lua
local s = "Hello"
Print(#s)              -- 5
Print(string.len(s))   -- 5 (то же самое)
```

#### Основные функции string

```lua
local s = "Hello, World!"

-- Верхний / нижний регистр
Print(string.upper(s))    -- "HELLO, WORLD!"
Print(string.lower(s))    -- "hello, world!"

-- Подстрока (начало, конец) — индексация с 1
Print(string.sub(s, 1, 5))    -- "Hello"
Print(string.sub(s, 8))       -- "World!"
Print(string.sub(s, -6))      -- "orld!" (с конца)

-- Поиск подстроки → начало, конец (или nil)
local start, finish = string.find(s, "World")
Print(start)   -- 8
Print(finish)  -- 12

-- Замена
local replaced = string.gsub(s, "World", "Lua")
Print(replaced)  -- "Hello, Lua!"

-- Замена с подсчётом
local result, count = string.gsub("aabbaabb", "bb", "XX")
Print(result)  -- "aaXXaaXX"
Print(count)   -- 2

-- Повтор
Print(string.rep("Ha", 3))   -- "HaHaHa"
Print(string.rep("-", 20))   -- "--------------------"

-- Реверс
Print(string.reverse("Hello"))  -- "olleH"

-- Код символа и символ по коду
Print(string.byte("A"))       -- 65
Print(string.char(65))        -- "A"
```

#### Форматирование строк

```lua
-- string.format — как printf в C
local name = "Герой"
local hp = 75
local maxHp = 100

local msg = string.format("%s: %d/%d HP", name, hp, maxHp)
Print(msg)  -- "Герой: 75/100 HP"

-- Спецификаторы:
-- %s = строка
-- %d = целое число
-- %f = дробное число
-- %.2f = дробное с 2 знаками после точки
-- %% = символ %

Print(string.format("Координаты: (%.1f, %.1f)", 123.456, 789.012))
-- "Координаты: (123.5, 789.0)"

Print(string.format("Прогресс: %d%%", 75))
-- "Прогресс: 75%"

Print(string.format("Предмет: [%20s]", "Меч"))
-- "Предмет: [                 Меч]" (выравнивание)
```

#### Конвертация строка ↔ число

```lua
-- Строка → число
local n = tonumber("42")       -- 42
local f = tonumber("3.14")     -- 3.14
local bad = tonumber("abc")    -- nil (невозможно)

-- Число → строка
local s = tostring(42)         -- "42"
local s2 = tostring(3.14)     -- "3.14"
local s3 = tostring(true)     -- "true"
local s4 = tostring(nil)      -- "nil"

-- При конкатенации числа конвертируются автоматически
Print("Score: " .. 100)       -- "Score: 100" (автоконвертация)
```

---

### Стандартная библиотека math

```lua
-- ═══════════════════════════════════
-- КОНСТАНТЫ
-- ═══════════════════════════════════

math.pi          -- 3.14159265358979 (число Пи)
math.huge        -- бесконечность (infinity)
math.maxinteger  -- максимальное целое число
math.mininteger  -- минимальное целое число

-- ═══════════════════════════════════
-- БАЗОВЫЕ ФУНКЦИИ
-- ═══════════════════════════════════

math.abs(-5)            -- 5        (модуль / абсолютное значение)
math.max(10, 20, 5)     -- 20       (максимум)
math.min(10, 20, 5)     -- 5        (минимум)
math.floor(3.7)         -- 3        (округление вниз)
math.ceil(3.2)          -- 4        (округление вверх)
math.sqrt(16)           -- 4.0      (квадратный корень)
math.log(2.718)         -- ~1.0     (натуральный логарифм)

-- ═══════════════════════════════════
-- ТРИГОНОМЕТРИЯ (углы в РАДИАНАХ!)
-- ═══════════════════════════════════

math.sin(math.pi / 2)   -- 1.0
math.cos(0)              -- 1.0
math.tan(math.pi / 4)   -- ~1.0
math.asin(1)             -- ~1.5708 (π/2)
math.acos(0)             -- ~1.5708
math.atan(1, 1)          -- ~0.7854 (π/4)  — atan2(y, x)

-- Конвертация градусы ↔ радианы
math.rad(180)            -- π (градусы → радианы)
math.deg(math.pi)        -- 180 (радианы → градусы)

-- ═══════════════════════════════════
-- СЛУЧАЙНЫЕ ЧИСЛА
-- ═══════════════════════════════════

math.random()            -- Дробное 0.0 — 1.0
math.random(6)           -- Целое 1 — 6 (как кубик)
math.random(10, 20)      -- Целое 10 — 20
math.randomseed(42)      -- Задать зерно (для воспроизводимости)

-- Типичное использование:
math.randomseed(os.time())  -- Инициализировать рандом текущим временем
```

#### Полезные математические паттерны для игр

```lua
-- Ограничение значения (clamp)
function Clamp(value, min, max)
    return math.max(min, math.min(max, value))
end
local hp = Clamp(hp - damage, 0, maxHp)

-- Линейная интерполяция (lerp)
function Lerp(a, b, t)
    return a + (b - a) * t
end
-- Плавное перемещение (t от 0 до 1)
local x = Lerp(startX, endX, 0.5)  -- Середина

-- Расстояние между двумя точками
function Distance(x1, y1, x2, y2)
    local dx = x2 - x1
    local dy = y2 - y1
    return math.sqrt(dx * dx + dy * dy)
end

-- Нормализация вектора (сделать длину = 1)
function Normalize(x, y)
    local len = math.sqrt(x * x + y * y)
    if len > 0 then
        return x / len, y / len
    end
    return 0, 0
end

-- Угол между двумя точками (в градусах)
function AngleBetween(x1, y1, x2, y2)
    return math.deg(math.atan(y2 - y1, x2 - x1))
end
```

---

### Стандартная библиотека string

> Все функции string можно вызывать двумя способами:

```lua
local s = "Hello World"

-- Способ 1: через библиотеку
string.upper(s)          -- "HELLO WORLD"

-- Способ 2: через метод строки (синтаксический сахар)
s:upper()                -- "HELLO WORLD"

-- Оба способа идентичны. Второй короче.
```

#### Полная таблица функций string

| Функция | Описание | Пример |
|---------|----------|--------|
| `string.byte(s, i?)` | Код символа на позиции i | `string.byte("A")` → `65` |
| `string.char(n...)` | Символ по коду | `string.char(65)` → `"A"` |
| `string.find(s, pattern)` | Поиск → start, end или nil | `string.find("hello", "ll")` → `3, 4` |
| `string.format(fmt, ...)` | Форматирование | `string.format("%d HP", 100)` → `"100 HP"` |
| `string.gmatch(s, pattern)` | Итератор по совпадениям | см. ниже |
| `string.gsub(s, pattern, repl)` | Замена всех совпадений | `string.gsub("aa", "a", "b")` → `"bb"` |
| `string.len(s)` | Длина строки | `string.len("abc")` → `3` |
| `string.lower(s)` | В нижний регистр | `string.lower("ABC")` → `"abc"` |
| `string.upper(s)` | В верхний регистр | `string.upper("abc")` → `"ABC"` |
| `string.match(s, pattern)` | Первое совпадение | `string.match("abc123", "%d+")` → `"123"` |
| `string.rep(s, n)` | Повторить n раз | `string.rep("ab", 3)` → `"ababab"` |
| `string.reverse(s)` | Перевернуть | `string.reverse("abc")` → `"cba"` |
| `string.sub(s, i, j?)` | Подстрока | `string.sub("hello", 2, 4)` → `"ell"` |

#### Паттерны (шаблоны поиска)

```lua
-- Lua использует собственные паттерны (не регулярные выражения, но похожие)

-- Основные спецсимволы:
-- %d = цифра (0-9)
-- %a = буква (a-z, A-Z)
-- %l = строчная буква
-- %u = заглавная буква
-- %s = пробельный символ
-- %p = знак пунктуации
-- %w = буква или цифра
-- .  = любой символ
-- +  = 1 или более повторений
-- *  = 0 или более повторений
-- -  = 0 или более (ленивый)
-- ?  = 0 или 1 повторение

-- Извлечение чисел из строки
local text = "Урон: 50, Защита: 30"
for num in string.gmatch(text, "%d+") do
    Print(num)  -- "50", "30"
end

-- Извлечение с захватом
local date = "2024-01-15"
local year, month, day = string.match(date, "(%d+)-(%d+)-(%d+)")
Print(year .. "/" .. month .. "/" .. day)  -- "2024/01/15"

-- Разбиение строки по разделителю
function Split(str, sep)
    local result = {}
    for part in string.gmatch(str, "([^" .. sep .. "]+)") do
        table.insert(result, part)
    end
    return result
end

local parts = Split("Меч,Щит,Зелье", ",")
-- parts = {"Меч", "Щит", "Зелье"}
```

---

### Стандартная библиотека table

Библиотека `table` работает с **массивной частью** таблиц (числовые индексы от 1).

```lua
local items = {"Меч", "Щит", "Зелье"}

-- ═══════════════════════════════════
-- ДОБАВЛЕНИЕ И УДАЛЕНИЕ
-- ═══════════════════════════════════

-- Добавить в конец
table.insert(items, "Лук")
-- {"Меч", "Щит", "Зелье", "Лук"}

-- Добавить на позицию (сдвигает остальные)
table.insert(items, 2, "Шлем")
-- {"Меч", "Шлем", "Щит", "Зелье", "Лук"}

-- Удалить по позиции (возвращает удалённый элемент)
local removed = table.remove(items, 3)  -- "Щит"
-- {"Меч", "Шлем", "Зелье", "Лук"}

-- Удалить последний
local last = table.remove(items)  -- "Лук"
-- {"Меч", "Шлем", "Зелье"}

-- ═══════════════════════════════════
-- СОРТИРОВКА
-- ═══════════════════════════════════

-- По возрастанию (числа / строки)
local numbers = {30, 10, 50, 20}
table.sort(numbers)
-- {10, 20, 30, 50}

-- По убыванию (пользовательская функция сравнения)
table.sort(numbers, function(a, b)
    return a > b
end)
-- {50, 30, 20, 10}

-- Сортировка таблицы объектов по полю
local enemies = {
    { name = "Орк", hp = 150 },
    { name = "Гоблин", hp = 50 },
    { name = "Дракон", hp = 500 },
}
table.sort(enemies, function(a, b)
    return a.hp < b.hp  -- По HP (от слабого к сильному)
end)

-- ═══════════════════════════════════
-- СОЕДИНЕНИЕ
-- ═══════════════════════════════════

local words = {"Привет", "мир", "Lua"}
local sentence = table.concat(words, " ")
Print(sentence)  -- "Привет мир Lua"

local csv = table.concat({1, 2, 3, 4}, ",")
Print(csv)  -- "1,2,3,4"

-- ═══════════════════════════════════
-- ПЕРЕМЕЩЕНИЕ (Lua 5.3+)
-- ═══════════════════════════════════

-- table.move(src, from, to, dest_start, dest_table?)
-- Копирует элементы из одной позиции в другую
local a = {1, 2, 3, 4, 5}
local b = {}
table.move(a, 1, 3, 1, b)  -- Копировать a[1..3] в b
-- b = {1, 2, 3}
```

#### Полезные функции для работы с таблицами

```lua
-- Проверка: массив пуст?
function IsEmpty(t)
    return next(t) == nil
end

-- Длина словаря (# работает только для массивной части)
function CountKeys(t)
    local count = 0
    for _ in pairs(t) do
        count = count + 1
    end
    return count
end

-- Содержит ли массив значение?
function Contains(arr, value)
    for _, v in ipairs(arr) do
        if v == value then return true end
    end
    return false
end

-- Поверхностная копия таблицы
function ShallowCopy(t)
    local copy = {}
    for k, v in pairs(t) do
        copy[k] = v
    end
    return copy
end

-- Глубокая копия (включая вложенные таблицы)
function DeepCopy(t)
    if type(t) ~= "table" then return t end
    local copy = {}
    for k, v in pairs(t) do
        copy[DeepCopy(k)] = DeepCopy(v)
    end
    return copy
end

-- Слияние двух таблиц (второй перезаписывает первый)
function Merge(base, override)
    local result = ShallowCopy(base)
    for k, v in pairs(override) do
        result[k] = v
    end
    return result
end

-- Фильтрация массива
function Filter(arr, predicate)
    local result = {}
    for _, v in ipairs(arr) do
        if predicate(v) then
            table.insert(result, v)
        end
    end
    return result
end

-- Маппинг массива
function Map(arr, transform)
    local result = {}
    for i, v in ipairs(arr) do
        result[i] = transform(v)
    end
    return result
end

-- Примеры использования:
local numbers = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10}
local evens = Filter(numbers, function(n) return n % 2 == 0 end)
-- {2, 4, 6, 8, 10}

local doubled = Map(numbers, function(n) return n * 2 end)
-- {2, 4, 6, 8, 10, 12, 14, 16, 18, 20}
```

> **Примечание:** IceBox Engine предоставляет продвинутые утилиты `Array.*`, `Map.*`, `Set()` и другие в секции [DataUtils](#37-datautils--структуры-данных-и-утилиты), которые делают всё вышеперечисленное и многое другое.

---

### Область видимости и замыкания

#### Глобальные vs локальные переменные

```lua
-- Глобальная переменная — видна ВЕЗДЕ в скрипте
health = 100

function TakeDamage(amount)
    health = health - amount  -- Изменяет глобальную health
end

-- Локальная переменная — видна только в своём блоке
function CreateBullet()
    local speed = 500         -- Видна только внутри CreateBullet
    local damage = 10         -- Видна только внутри CreateBullet
    Print(speed)
end

-- Print(speed) -- ОШИБКА! speed не определена здесь
```

#### Блоки и области видимости

```lua
-- local внутри if/for/while видна только внутри этого блока
if true then
    local temp = 42
    Print(temp)     -- 42 (OK)
end
-- Print(temp)      -- nil! (temp не существует вне if)

-- Вложенные области: внутренний блок видит внешние переменные
local outerVar = "я снаружи"

function Inner()
    Print(outerVar)  -- "я снаружи" (видит переменную из внешней области)

    local innerVar = "я внутри"
    Print(innerVar)  -- "я внутри" (OK)
end

-- Print(innerVar)   -- nil! (не видна снаружи)
```

#### Замыкания (closures)

Замыкание — функция, которая «запоминает» переменные из своей области создания.

```lua
-- Простое замыкание
function CreateCounter()
    local count = 0  -- Эта переменная «захвачена» замыканием

    return function()
        count = count + 1
        return count
    end
end

local counter = CreateCounter()
Print(counter())  -- 1
Print(counter())  -- 2
Print(counter())  -- 3

-- Каждый вызов CreateCounter создаёт НЕЗАВИСИМЫЙ счётчик
local counterA = CreateCounter()
local counterB = CreateCounter()
Print(counterA())  -- 1
Print(counterA())  -- 2
Print(counterB())  -- 1 (независимый!)

-- Практический пример: фабрика урона
function CreateDamageDealer(baseDamage, critChance)
    return function(target)
        local dmg = baseDamage
        if math.random() < critChance then
            dmg = dmg * 2
            Print("КРИТ!")
        end
        Print(target .. " получил " .. dmg .. " урона")
        return dmg
    end
end

local fireBall = CreateDamageDealer(50, 0.3)
local iceSpike = CreateDamageDealer(30, 0.5)

fireBall("Гоблин")   -- Гоблин получил 50 (или 100) урона
iceSpike("Орк")      -- Орк получил 30 (или 60) урона
```

---

### Метатаблицы и ООП

Метатаблицы — механизм, позволяющий переопределить поведение таблиц. С их помощью в Lua реализуется объектно-ориентированное программирование.

#### Базовые метаметоды

```lua
-- Метатаблица — обычная таблица со специальными ключами (__add, __index и т.д.)
local vec1 = { x = 1, y = 2 }
local vec2 = { x = 3, y = 4 }

-- Без метатаблицы: vec1 + vec2 = ОШИБКА
-- С метатаблицей: можно определить, что означает "+"

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

#### ООП: Классы через метатаблицы

```lua
-- ═══════════════════════════════════
-- СОЗДАНИЕ «КЛАССА» В LUA
-- ═══════════════════════════════════

-- Определяем «класс» Character
local Character = {}
Character.__index = Character

-- Конструктор
function Character.new(name, hp, damage)
    local self = setmetatable({}, Character)
    self.name = name
    self.hp = hp
    self.maxHp = hp
    self.damage = damage
    self.isAlive = true
    return self
end

-- Методы
function Character:TakeDamage(amount)
    self.hp = math.max(self.hp - amount, 0)
    Print(self.name .. " получил " .. amount .. " урона! HP: " .. self.hp)
    if self.hp <= 0 then
        self.isAlive = false
        Print(self.name .. " погиб!")
    end
end

function Character:Heal(amount)
    self.hp = math.min(self.hp + amount, self.maxHp)
    Print(self.name .. " вылечен на " .. amount .. ". HP: " .. self.hp)
end

function Character:Attack(target)
    Print(self.name .. " атакует " .. target.name .. "!")
    target:TakeDamage(self.damage)
end

-- Использование
local hero = Character.new("Герой", 100, 25)
local goblin = Character.new("Гоблин", 50, 10)

hero:Attack(goblin)     -- Герой атакует Гоблин! → Гоблин получил 25 урона!
goblin:Attack(hero)     -- Гоблин атакует Герой! → Герой получил 10 урона!
hero:Heal(15)           -- Герой вылечен на 15. HP: 105 → 100 (max)
```

> **Важно:** `:` (двоеточие) при определении и вызове метода — синтаксический сахар.
> `Character:Attack(target)` то же самое, что `Character.Attack(self, target)`.
> При вызове `hero:Attack(goblin)` Lua автоматически передаёт `hero` как `self`.

#### ООП: Наследование

```lua
-- Базовый класс
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

-- Дочерний класс (наследуется от Entity)
local Enemy = setmetatable({}, { __index = Entity })
Enemy.__index = Enemy

function Enemy.new(name, x, y, damage)
    local self = Entity.new(name, x, y)  -- Вызов родительского конструктора
    setmetatable(self, Enemy)
    self.damage = damage
    self.isAggressive = true
    return self
end

function Enemy:Attack()
    Print(self.name .. " наносит " .. self.damage .. " урона!")
end

-- Использование
local orc = Enemy.new("Орк", 100, 200, 30)
orc:MoveTo(150, 250)        -- Метод наследован от Entity
orc:Attack()                 -- Метод Enemy
local x, y = orc:GetPosition()  -- Метод наследован от Entity
Print(orc.name .. " на позиции " .. x .. ", " .. y)
```

> **Примечание:** В IceBox Engine ООП обычно не нужен в скриптах сущностей, так как движок уже предоставляет компонентную систему. Но ООП полезен для создания своих утилитарных модулей (системы инвентаря, диалогов, квестов и т.д.).

---

### Обработка ошибок (pcall, xpcall)

В обычном режиме ошибка в Lua **останавливает весь скрипт**. Функция `pcall` позволяет «поймать» ошибку.

```lua
-- ═══════════════════════════════════
-- pcall — Protected Call (защищённый вызов)
-- ═══════════════════════════════════

-- Опасный код (может упасть)
local success, result = pcall(function()
    local x = 10 / 0       -- Не ошибка в Lua (= inf)
    local t = nil
    return t.field          -- ОШИБКА! Попытка обратиться к полю nil
end)

if success then
    Print("Результат: " .. tostring(result))
else
    Print("Ошибка: " .. result)  -- result содержит сообщение об ошибке
end

-- ═══════════════════════════════════
-- Практический пример: безопасная загрузка данных
-- ═══════════════════════════════════

function SafeLoadData(key)
    local success, data = pcall(function()
        local raw = ReadFile(key .. ".json")
        if not raw then
            error("Файл не найден: " .. key)
        end
        return raw
    end)

    if success then
        return data
    else
        PrintWarning("Не удалось загрузить " .. key .. ": " .. data)
        return nil
    end
end

-- ═══════════════════════════════════
-- xpcall — с пользовательским обработчиком ошибок
-- ═══════════════════════════════════

local function errorHandler(err)
    return "ОШИБКА СКРИПТА: " .. err
end

local ok, msg = xpcall(function()
    error("что-то пошло не так")
end, errorHandler)

Print(msg)  -- "ОШИБКА СКРИПТА: что-то пошло не так"

-- ═══════════════════════════════════
-- error() — генерация собственных ошибок
-- ═══════════════════════════════════

function SetHealth(value)
    if type(value) ~= "number" then
        error("SetHealth ожидает число, получено: " .. type(value))
    end
    if value < 0 then
        error("HP не может быть отрицательным: " .. value)
    end
    health = value
end

-- assert — проверка условия + ошибка если false
function DivideScore(score, divisor)
    assert(divisor ~= 0, "Деление на ноль!")
    return score / divisor
end
```

---

### Полезные паттерны и идиомы

Здесь собраны типичные приёмы Lua, которые вы будете использовать в игровых скриптах.

#### Значение по умолчанию

```lua
function CreateEnemy(config)
    config = config or {}
    local name = config.name or "Враг"
    local hp = config.hp or 100
    local speed = config.speed or 150
end
```

#### Тернарный оператор

```lua
-- Lua не имеет тернарного оператора a ? b : c, но можно эмулировать:
local status = (hp > 0) and "Жив" or "Мёртв"
local direction = (vx > 0) and 1 or -1

-- ⚠️ Осторожно! Не работает, если «true-значение» может быть false/nil:
-- local val = condition and false or "default"  — всегда вернёт "default"!
```

#### Безопасный доступ к вложенным данным

```lua
-- Проблема: если промежуточный ключ = nil → ошибка
-- local name = data.player.inventory.weapon.name  -- ОШИБКА если player = nil

-- Безопасная цепочка:
local weapon = data and data.player and data.player.inventory
                and data.player.inventory.weapon
local name = weapon and weapon.name or "Нет оружия"
```

#### Кэширование результатов

```lua
-- Если функция дорогая — вызывайте один раз и сохраняйте результат
function OnUpdate(dt)
    -- ❌ Плохо: вызывается каждый кадр (60+ раз в секунду)
    -- for _, enemy in ipairs(FindEntitiesByTag("Enemy")) do ... end

    -- ✅ Хорошо: поиск + кэш (обновлять по необходимости)
    if not cachedEnemies or refreshTimer <= 0 then
        cachedEnemies = FindEntitiesByTag("Enemy")
        refreshTimer = 0.5  -- Обновлять каждые 0.5 секунды
    end
    refreshTimer = refreshTimer - dt

    for _, enemy in ipairs(cachedEnemies) do
        -- ...
    end
end
```

#### Паттерн «Lookup Table» вместо длинного if/elseif

```lua
-- ❌ Длинная цепочка if/elseif:
function GetDamageMultiplier(element, target)
    if element == "fire" and target == "ice" then return 2.0
    elseif element == "fire" and target == "fire" then return 0.5
    elseif element == "water" and target == "fire" then return 2.0
    -- ... десятки строк
    else return 1.0 end
end

-- ✅ Таблица поиска (lookup table) — чище, быстрее, легче расширять:
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
Print(GetDamageMultiplier("fire", "rock"))  -- 1.0 (по умолчанию)
```

#### Паттерн «Command Table» вместо switch/case

```lua
-- Lua не имеет switch/case, но таблица функций — мощнее:
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
        PrintWarning("Неизвестная команда: " .. name)
    end
end

-- Использование:
ExecuteCommand("attack")
ExecuteCommand("heal")
```

#### Глобальные переменные скрипта для OnUpdate

```lua
-- Переменные без local ВНУТРИ OnConstruct/OnCreate сохраняются между кадрами
-- Это основной способ хранения состояния в IceBox скриптах
--
-- ВАЖНО: OnConstruct выполняется и в редакторе тоже.
-- Переменные заданные в OnConstruct настроят сущность прямо в edit mode.
-- В runtime: OnConstruct → OnCreate → OnUpdate каждый кадр.

function OnConstruct()
    -- Эти переменные живут всё время существования сущности
    -- И применяются в viewport редактора сразу
    speed = 250
    health = 100
    maxHealth = 100
    isAlive = true
    attackCooldown = 0
    facingRight = true
end

function OnUpdate(dt)
    -- Все переменные выше доступны здесь
    attackCooldown = attackCooldown - dt
    if attackCooldown < 0 then attackCooldown = 0 end

    if IsKeyJustPressed("space") and attackCooldown <= 0 then
        attackCooldown = 0.5  -- Полсекунды откат
        -- Атака!
    end
end
```

#### Итерация с удалением (безопасная)

```lua
-- ❌ ОПАСНО: удаление элементов во время ipairs
-- for i, v in ipairs(list) do
--     if v.dead then table.remove(list, i) end  -- Пропустит элементы!
-- end

-- ✅ Безопасно: итерация с конца
for i = #list, 1, -1 do
    if list[i].dead then
        table.remove(list, i)
    end
end

-- ✅ Или: создать новый массив (filter)
local alive = {}
for _, v in ipairs(list) do
    if not v.dead then
        table.insert(alive, v)
    end
end
list = alive
```

#### Полезные однострочники

```lua
-- Обмен значениями
a, b = b, a

-- Максимум из трёх
local max3 = math.max(a, math.max(b, c))

-- Знак числа (-1, 0, 1)
local sign = x > 0 and 1 or (x < 0 and -1 or 0)

-- Проверка, что значение в диапазоне
local inRange = x >= min and x <= max

-- Случайный элемент массива
local item = list[math.random(#list)]

-- Проверка — таблица пуста?
local empty = next(t) == nil
```

---

### Модули и require

Lua поддерживает разбиение кода на модули. Модуль — это обычный файл `.lua`, который возвращает таблицу.

```lua
-- content/Utils/Math.lua
local M = {}

function M.Clamp(v, min, max)
    return math.max(min, math.min(max, v))
end

return M
```

```lua
-- В другом файле
local Math = require("content.Utils.Math")
local hp = Math.Clamp(150, 0, 100)  -- 100
```

> Путь в `require` задаётся через точки, не через слеши. Доступность `require` зависит от сборки движка.

---

### Итераторы и обобщённый for

Обобщённый `for` может работать с любым итератором, который возвращает `nextValue`.

```lua
-- Простейший кастомный итератор: числа от 1 до n
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

### Сборщик мусора

Lua использует автоматический сборщик мусора (GC). Обычно не требуется ручное управление, но оно возможно:

```lua
collectgarbage("collect")   -- принудительный проход GC
local kb = collectgarbage("count")  -- объём памяти (КБ)
collectgarbage("stop")      -- остановить GC
collectgarbage("restart")   -- включить GC обратно
```

---

### Стандартные библиотеки coroutine/io/os (если доступны)

В обычном Lua есть библиотеки `coroutine`, `io`, `os`, `package`. В некоторых движках они могут быть отключены.

```lua
-- coroutine: базовый пример
local co = coroutine.create(function()
    Print("Шаг 1")
    coroutine.yield()
    Print("Шаг 2")
end)

coroutine.resume(co)  -- Шаг 1
coroutine.resume(co)  -- Шаг 2
```

```lua
-- os: базовые утилиты времени
local now = os.time()
local t = os.date("%Y-%m-%d", now)
```

```lua
-- io: чтение файла (если доступно)
local f = io.open("data.txt", "r")
if f then
    local text = f:read("*a")
    f:close()
end
```

---

> **Вы изучили все основы языка Lua!** Теперь вы готовы использовать API IceBox Engine. Переходите к [секции 3 (Жизненный цикл скриптов)](#3-жизненный-цикл-скриптов) и далее.

---

## 3. Жизненный цикл скриптов

### Скрипт класса (Entity Script)

Каждый `.ice_class` может содержать следующие callback-функции:

```lua
-- ═══════════════════════════════════
-- ЖИЗНЕННЫЙ ЦИКЛ СУЩНОСТИ
-- ═══════════════════════════════════

function OnConstruct()
    -- СКРИПТ КОНСТРУИРОВАНИЯ
    --
    -- Вызывается В РЕЖИМЕ РЕДАКТОРА:
    --   • При перетаскивании класса на viewport (drag-and-drop)
    --   • При нажатии "Compile" в редакторе класса
    --   • При открытии уровня (для всех сущностей со скриптами)
    --   • При hot-reload классов
    --
    -- Вызывается В RUNTIME (Play Mode):
    --   • Перед OnCreate, самым первым callback'ом
    --
    -- Используйте для настройки визуала, генерации тайлмапов,
    -- конфигурации компонентов — всего, что вы хотите ВИДЕТЬ
    -- в редакторе без нажатия Play.
    --
    -- Доступна глобальная переменная IsEditorMode (bool):
    --   true  = выполняется в viewport редактора (edit mode)
    --   false / nil = выполняется в play mode (runtime)
    --
    speed = 200
    health = 100
    isAlive = true

    if IsEditorMode then
        -- Логика только для превью в редакторе (например, отладочная визуализация)
        Print("[Редактор] Сущность сконструирована")
    end
end

function OnCreate()
    -- Вызывается ТОЛЬКО в runtime (Play Mode), после OnConstruct.
    -- Используйте для runtime-логики: биндинг ввода,
    -- спавн помощников, запуск корутин и т.д.
    -- НИКОГДА не вызывается в редакторе.
    Print("Игрок создан!")
end

function OnUpdate(dt)
    -- Вызывается КАЖДЫЙ КАДР. dt = время между кадрами (дельта-время)
    -- Сюда пишется основная логика: движение, проверки, ИИ
    local vx = 0
    if IsKeyPressed("d") then vx = speed end
    if IsKeyPressed("a") then vx = -speed end
    SetVelocityX(vx)
end

function OnFixedUpdate(dt)
    -- Вызывается с ФИКСИРОВАННЫМ шагом (для физики)
    -- Используйте для физических расчётов
end

function OnLateUpdate(dt)
    -- Вызывается каждый кадр ПОСЛЕ обновления анимаций (Animator/смена
    -- кадра флипбука), непосредственно перед рендером.
    -- Используйте для всего, что должно следовать за ТЕКУЩИМ кадром анимации:
    -- следование камеры, look-at, ручная работа с сокетами.
    --
    -- Для привязки графики к сокету лучше AttachSpriteToSocket("ArmSocket", 1)
    -- (настроил один раз — движок держит привязку каждый кадр). Ручной вариант
    -- ниже нужен только если требуется своя логика на кадр — и всегда через
    -- World-сеттеры, а не SetSpriteLocalPosition с мировой дельтой.
    local sock = GetSpriteAttachPointWorld("ArmSocket", 0)
    if sock and sock.found then
        SetSpriteWorldPosition(sock.x, sock.y, 1)
        SetSpriteWorldRotation(sock.rotation, 1)
        SetFlipX(GetFlipX(0), 1)
    end
end

function OnDestroy()
    -- Вызывается при удалении сущности
    Print("Сущность удалена")
end

function OnEnable()
    -- Вызывается при включении сущности
end

function OnDisable()
    -- Вызывается при выключении сущности
end

-- ═══════════════════════════════════
-- СТОЛКНОВЕНИЯ (ФИЗИКА Box2D)
-- ═══════════════════════════════════

function OnCollisionEnter(otherTag, otherEntityId)
    -- Столкновение с физическим телом началось
    if otherTag == "Enemy" then
        health = health - 10
    end
end

function OnCollisionExit(otherTag, otherEntityId)
    -- Столкновение закончилось
end

-- ═══════════════════════════════════
-- СЕНСОРЫ (ТРИГГЕРЫ)
-- ═══════════════════════════════════

function OnSensorEnter(otherTag, otherEntityId)
    -- Вход в зону сенсора (коллайдер с флагом isSensor)
    if otherTag == "Coin" then
        DestroyEntity(otherEntityId)
        score = score + 1
    end
end

function OnSensorExit(otherTag, otherEntityId)
    -- Выход из зоны сенсора
end

-- ═══════════════════════════════════
-- УДАР (HIT)
-- ═══════════════════════════════════

function OnHit(otherTag, otherEntityId, speed)
    -- Вызывается при ударе с определённой скоростью
end

-- ═══════════════════════════════════
-- ПРИЗЕМЛЕНИЕ / ПАДЕНИЕ
-- ═══════════════════════════════════

function OnLanded()
    -- Вызывается когда сущность приземляется (переход из воздуха на землю)
    -- Требуется RigidbodyComponent
end

function OnStartFalling()
    -- Вызывается когда сущность начинает падать (переход с земли в воздух)
    -- Требуется RigidbodyComponent
end

-- ═══════════════════════════════════
-- ПАУЗА / ВОЗОБНОВЛЕНИЕ
-- ═══════════════════════════════════

function OnPause()
    -- Вызывается при SetTimeScale(0) / PauseGame()
end

function OnResume()
    -- Вызывается при ResumeGame()
end

-- ═══════════════════════════════════
-- ЛОКАЛИЗАЦИЯ
-- ═══════════════════════════════════

function OnLanguageChanged(newLang, oldLang)
    -- Язык игры сменился
    Print("Язык: " .. oldLang .. " -> " .. newLang)
end
```

### Скрипт уровня (Level Script)

Пишется в `.icemap`. Имеет собственные callback-функции:

```lua
function OnLevelStart()
    -- Вызывается при запуске уровня
    Print("Уровень начался!")
end

function OnLevelUpdate(dt)
    -- Каждый кадр (уровень)
end

function OnLevelLateUpdate(dt)
    -- Каждый кадр после обновления анимаций, непосредственно перед рендером
end

function OnLevelEnd()
    -- При завершении уровня
end
```

> Из скрипта уровня доступны: `FindEntityByTag`, `SetCameraPosition`, `GetCameraPosition`, `SpawnEntity`, `LineTrace` и другие глобальные + EntityLua/CameraLua/ComponentLua функции.

### OnConstruct vs OnCreate — порядок выполнения

```
┌─────────────────────────────────────────────────────────┐
│                    РЕЖИМ РЕДАКТОРА                       │
│                                                         │
│  Размещение класса на сцене ──► OnConstruct()           │
│  Открытие уровня ────────────► OnConstruct() для всех   │
│  Нажатие Compile ────────────► OnConstruct() в viewport  │
│  Hot-reload класса ──────────► OnConstruct()            │
│                                                         │
│  OnCreate НИКОГДА не вызывается в редакторе             │
│  IsEditorMode = true                                    │
└─────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────┐
│                   RUNTIME (Play Mode)                   │
│                                                         │
│  OnConstruct()  ← первым, настройка дефолтов/компонент  │
│       ↓                                                 │
│  OnCreate()     ← runtime инициализация (ввод, корутины)│
│       ↓                                                 │
│  OnUpdate(dt)   ← каждый кадр                           │
│  OnFixedUpdate(dt) ← фиксированный физический шаг       │
│  OnLateUpdate(dt) ← после анимаций, перед рендером      │
│       ↓                                                 │
│  OnDestroy()    ← очистка                               │
│                                                         │
│  IsEditorMode = nil (не задана)                         │
└─────────────────────────────────────────────────────────┘
```

**Правило:**
- **`OnConstruct`** = визуальная настройка, конфигурация компонентов, процедурная генерация (тайлмапы, декорации). Работает в редакторе И в runtime.
- **`OnCreate`** = только runtime-логика: запуск корутин, биндинг ввода, воспроизведение звуков, настройка ИИ. Вызывается только в Play Mode.

| Переменная | В редакторе | В runtime |
|------------|-------------|-----------|
| `IsEditorMode` | `true` | `nil` / не задана |

```lua
-- Пример: генерация тайлмапы видна прямо в редакторе
function OnConstruct()
    SetTileAt(0, 0, 0, 1)  -- видно сразу в viewport
    SetTileAt(1, 0, 0, 2)

    if not IsEditorMode then
        -- Инициализация только для runtime внутри OnConstruct
        Print("Заспавнен в рантайме")
    end
end

function OnCreate()
    -- Это выполнится только в Play Mode
    StartCoroutine(function()
        coroutine.yield(WaitSeconds(2))
        Print("Готов!")
    end)
end
```

---

## 4. Transform — Позиция, масштаб, поворот

> **Тип:** Entity-bound (работают с текущей сущностью)

### Позиция

```lua
-- Установить позицию (x, y, z)
SetPosition(100, 200, 0)

-- Получить позицию → {x, y, z}
local pos = GetPosition()
Print("X=" .. pos.x .. " Y=" .. pos.y)

-- Получить компонент Transform (userdata, может быть nil)
local t = GetTransform()

-- Получить/установить отдельные оси
local x = GetPositionX()
local y = GetPositionY()
local z = GetPositionZ()
SetPositionX(100)
SetPositionY(200)
SetPositionZ(0)  -- Z = порядок отрисовки (больше = дальше)

-- Сдвинуть на величину (dx, dy)
Translate(5, 0)  -- Сдвинуть вправо на 5 пикселей
```

### Масштаб

```lua
-- Установить масштаб
SetScale(2, 2)  -- Увеличить в 2 раза

-- Получить масштаб → {x, y}
local scale = GetScale()
```

### Поворот

```lua
-- Установить поворот (в градусах)
SetRotation(45)

-- Получить поворот
local rot = GetRotation()

-- Добавить поворот
Rotate(10)  -- Повернуть на +10 градусов
```

### Утилиты

```lua
-- Двигаться к точке с максимальной скоростью. Возвращает true, если достигнута.
local reached = MoveTowards(targetX, targetY, speed * dt)

-- Расстояние до точки
local dist = DistanceTo(x, y)

-- Повернуться в направлении вектора
LookAtDirection(dx, dy)

```

---

### GLSLua — GPU-эффекты вершинного шейдера

GLSLua — система per-instance GPU вершинных эффектов. Эффекты (параллакс, покачивание, ветер)
применяются напрямую в вершинном шейдере на GPU — без манипуляции трансформами на CPU.
Каждый экземпляр спрайта/флипбука имеет 8 float-параметров, упакованных в 2×vec4, которые
управляют эффектом.

Эта система **независима** от системы Материалов. Можно использовать GLSLua для вершинных
эффектов на сущностях со стандартным пайплайном рендеринга, или реализовать свои вершинные
эффекты через кастомные шейдеры Материалов — они не конфликтуют.

#### Раскладка параметров

| Индекс | Имя            | По умолч. | Описание                                           |
|--------|----------------|-----------|-----------------------------------------------------|
| 0      | ParallaxX      | 0.0       | Горизонтальный параллакс-фактор (относительно камеры)|
| 1      | ParallaxY      | 0.0       | Вертикальный параллакс-фактор (относительно камеры)  |
| 2      | SwayAmplitude  | 0.0       | Амплитуда покачивания (мировые единицы)              |
| 3      | SwaySpeed      | 0.0       | Скорость покачивания (рад/сек)                       |
| 4      | SwayPhaseOffset| 0.0       | Сдвиг фазы (для десинхронизации)                     |
| 5      | SwayGradient   | 0.0       | Градиент: 0=равномерно, 1=низ зафиксирован           |
| 6      | WindStrength   | 0.0       | Сила эффекта ветра                                   |
| 7      | WindSpeed      | 1.5       | Скорость эффекта ветра                               |

#### Высокоуровневые функции

```lua
-- Параллакс (автоопределение спрайта или флипбука)
GLSL_SetParallax(factorX, factorY [, instanceIndex])
local px, py = GLSL_GetParallax([instanceIndex])

-- Покачивание (phaseOffset, gradient опциональны — сохраняют предыдущее если не указаны)
GLSL_SetSway(amplitude, speed [, phaseOffset] [, gradient] [, instanceIndex])
local amp, spd, phase, grad = GLSL_GetSway([instanceIndex])

-- Ветер
GLSL_SetWind(strength [, speed] [, instanceIndex])
local str, spd = GLSL_GetWind([instanceIndex])

-- Сбросить все эффекты к значениям по умолчанию
GLSL_ClearEffects([instanceIndex])
```

#### Низкоуровневый доступ к параметрам

```lua
-- Установить/получить любой параметр по индексу (0-7), см. таблицу выше
GLSL_SetParam(paramIndex, value [, instanceIndex])
local val = GLSL_GetParam(paramIndex [, instanceIndex])
```

#### Функции для конкретных компонентов

Применяются ко всем экземплярам (index = nil) или к конкретному по индексу:

```lua
-- Только спрайты
GLSL_SpriteSetParallax(factorX, factorY [, index])
GLSL_SpriteSetSway(amplitude, speed [, phaseOffset] [, gradient] [, index])
GLSL_SpriteSetWind(strength [, speed] [, index])

-- Только флипбуки
GLSL_FlipbookSetParallax(factorX, factorY [, index])
GLSL_FlipbookSetSway(amplitude, speed [, phaseOffset] [, gradient] [, index])
GLSL_FlipbookSetWind(strength [, speed] [, index])
```

#### Примеры

```lua
-- Параллакс фона: двигается с половинной скоростью камеры
function OnBegin()
    GLSL_SetParallax(0.5, 0.5)
end

-- Покачивание дерева: низ зафиксирован, верх качается, с ветром
function OnBegin()
    SetSpritePivot(0.5, 1.0)  -- pivot на нижний центр
    GLSL_SetSway(3.0, 2.0, math.random() * 6.28, 1.0)  -- gradient=1 → низ зафиксирован
    GLSL_SetWind(1.5, 1.2)
end

-- Поле травы: у каждого экземпляра уникальная фаза
function OnBegin()
    for i = 0, GetSpriteCount() - 1 do
        GLSL_SpriteSetSway(2.0, 3.0, math.random() * 6.28, 0.8, i)
        GLSL_SpriteSetWind(0.5, 1.0, i)
    end
end

-- Прямой доступ к параметрам для кастомных эффектов
GLSL_SetParam(2, 5.0)   -- SwayAmplitude = 5.0
GLSL_SetParam(6, 2.0)   -- WindStrength = 2.0
local wind = GLSL_GetParam(6)
```

#### UV-скролл (Авто-параллакс)

`SetSpriteUVScroll` / `GetSpriteUVScroll` позволяют непрерывно смещать текстурные
координаты спрайта в рантайме. В сочетании с `GLSL_SpriteSetParallax` это создаёт
**авто-скроллящийся параллакс** — текстура бесконечно тайлится через GPU (GL_REPEAT)
и при этом автоматически плывёт со временем. Идеально для облаков, воды, тумана или
любого фонового слоя, который должен двигаться сам по себе в дополнение к параллаксу камеры.

```lua
-- Установить UV-смещение для экземпляра спрайта (в нормализованном UV-пространстве, 0..1 = одна полная текстура)
SetSpriteUVScroll(offsetX, offsetY [, instanceIndex])

-- Получить текущее UV-смещение → {x, y}
local scroll = GetSpriteUVScroll([instanceIndex])
```

**Пример — авто-скроллящиеся облака с бесконечным параллаксом:**

```lua
local IDX_CLOUDS = 2
cloudScrollSpeed = 6.0   -- мировых единиц в секунду
cloudScrollU = 0.0

function OnCreate()
    -- GPU-параллакс для бесконечного тайлинга при движении камеры
    GLSL_SpriteSetParallax(-0.04, -0.03, IDX_CLOUDS)

    -- Перевод мировой скорости в UV-скорость
    local size = GetSpriteSize(IDX_CLOUDS)
    local scale = GetSpriteLocalScale(IDX_CLOUDS)
    cloudUVSpeed = cloudScrollSpeed / (size.width * scale.x)
end

function OnUpdate(dt)
    -- Накапливаем UV-смещение — текстура оборачивается бесшовно через GL_REPEAT
    cloudScrollU = cloudScrollU + cloudUVSpeed * dt
    SetSpriteUVScroll(cloudScrollU, 0.0, IDX_CLOUDS)
end
```

#### UV-масштаб (Тайлинг)

`SetSpriteUVScale` / `GetSpriteUVScale` управляют количеством повторений (тайлингом)
текстуры спрайта на квадре. По умолчанию `(1.0, 1.0)` — текстура отображается 1:1.
Значение `(2.0, 1.0)` повторит текстуру дважды по горизонтали.

В сочетании с `SetSpriteUVScroll` это необходимо для **авто-скроллящихся слоёв,
которые шире области видимости**: увеличьте спрайт так, чтобы его края были за
пределами экрана, а затем установите UV-масштаб, чтобы текстура тайлилась
корректно, а не растягивалась.

```lua
-- Установить UV-тайлинг для экземпляра спрайта (1.0 = по умолчанию, 2.0 = повтор дважды и т.д.)
SetSpriteUVScale(scaleX, scaleY [, instanceIndex])

-- Получить текущий UV-масштаб → {x, y}
local uvs = GetSpriteUVScale([instanceIndex])
```

**Пример — скроллящееся облако шире области видимости (без видимого шва):**

```lua
local IDX_CLOUDS = 2

function OnCreate()
    -- Спрайт облака в 2× шире стандартного — края за пределами экрана
    SetSpriteLocalScale(0.5, 0.25, IDX_CLOUDS)  -- 2× шире чем (0.25, 0.25)

    -- Тайлинг текстуры 2× по горизонтали для компенсации более широкого спрайта
    SetSpriteUVScale(2.0, 1.0, IDX_CLOUDS)

    -- ...
end
```

### Прицеливание и направление

```lua
-- Повернуть сущность к точке
LookAt(mouseX, mouseY)

-- Повернуть сущность к другой сущности
LookAtEntity(enemyId)

-- Угол от себя к точке (градусы)
local angle = GetAngleTo(targetX, targetY)

-- Угол от себя к сущности (градусы)
local angle = GetAngleToEntity(entityId)

-- Вектор направления из текущего поворота → {x, y}
local fwd = GetForwardVector()

-- Правый вектор (перпендикулярный forward) → {x, y}
local right = GetRightVector()
```

### Перемещение

```lua
-- Двигаться в направлении угла
MoveInDirection(angleDeg, speed, dt)

-- Двигаться вперёд (по текущему вращению)
MoveForward(speed, dt)

-- Повернуть позицию вокруг точки
RotateAround(cx, cy, angleDeg)

-- Встать на орбиту вокруг точки
OrbitAround(cx, cy, radius, angleDeg)

-- Flip спрайта по направлению скорости (для платформеров)
FaceMovementDirection()

-- Flip спрайта по знаку dirX
FaceDirection(dirX)
```

### Пример: Top-Down шутер — прицеливание и движение

```lua
function OnUpdate(dt)
    -- Прицеливание к мыши
    local mpos = GetMouseWorldPosition()
    LookAt(mpos.x, mpos.y)

    -- Движение вперёд по направлению взгляда
    if IsKeyPressed("w") then
        MoveForward(200, dt)
    end

    -- Движение по углу (стрейф)
    local angle = GetAngleTo(mpos.x, mpos.y)
    if IsKeyPressed("a") then
        MoveInDirection(angle - 90, 150, dt)
    end
    if IsKeyPressed("d") then
        MoveInDirection(angle + 90, 150, dt)
    end
end
```

### Пример: Орбитальный спутник

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

## 5. Physics — Физика (Box2D)

> **Тип:** Entity-bound. Требует компонент **RigidbodyComponent**.

### Скорость

```lua
-- Установить скорость (пиксели/сек)
SetVelocity(200, 0)
SetVelocityX(200)
SetVelocityY(-300)

-- Получить скорость → {x, y}
local vel = GetVelocity()
local vx = GetVelocityX()
local vy = GetVelocityY()

-- Скорость (модуль)
local speed = GetSpeed()

-- Ограничить скорость
ClampVelocity(500)     -- Общая максимальная скорость
ClampVelocityX(300)    -- Максимальная по X
ClampVelocityY(300)    -- Максимальная по Y

-- Остановить движение (обнулить скорость + угловую скорость)
StopMovement()
```

> У сущности с **AI-компонентом** `StopMovement()` вместо этого останавливает
> AI-локомоцию (см. раздел Navigation) — в сайд-скроллере обнуляется только
> горизонтальная скорость, чтобы гравитация продолжала работать. Без
> AI-компонента это обычная физическая остановка, показанная выше.

### Силы и импульсы

```lua
-- Импульс — мгновенное изменение скорости (прыжок, удар)
AddImpulse(0, 500)  -- Прыжок вверх

-- Сила — постоянное ускорение (двигатель, ветер)
AddForce(100, 0)     -- Толкать вправо

-- Сила в точке (создаёт вращение)
AddForceAtPoint(100, 0, pointX, pointY)

-- Импульс в точке
AddImpulseAtPoint(100, 200, pointX, pointY)

-- Импульс/сила по направлению угла
AddImpulseDirection(45, 300)
AddForceDirection(90, 50)

-- Торк (вращающий момент)
AddTorque(50)

-- Угловой импульс
AddAngularImpulse(10)
```

### Гравитация и состояние

```lua
-- Масштаб гравитации (0 = без гравитации, 2 = двойная)
SetGravityScale(0)
local gs = GetGravityScale()

-- Гравитация мира (глобально)
SetGravity(0, -9.81)
local gravity = GetGravity()  -- → {x, y}

-- Проверки состояния
local falling = IsFalling()    -- Падает вниз?
local rising  = IsRising()     -- Летит вверх?
local ground  = IsGrounded()   -- На земле? (raycast вниз)
local onWall = IsOnWall()       -- Есть стена рядом
local onWallRight = IsOnWall(1) -- Проверить справа
local onWallLeft = IsOnWall(-1) -- Проверить слева
local ceiling = IsCeiling()     -- Потолок над головой
```

> `IsGrounded`, `IsInAir`, `IsOnWall` и `IsCeiling` прощупывают пространство короткими лучами и учитывают только **твёрдую** геометрию. Сенсоры и собственное тело сущности пропускаются и **не** перекрывают луч — триггер-зона, монета или чекпоинт, лежащие на полу, не помешают `IsGrounded()` найти пол под ними. `IsInAir()` — это в точности `not IsGrounded()`, обе функции выполняют одну и ту же проверку.

### Свойства тела

```lua
-- Тип тела: "Dynamic", "Static", "Kinematic"
SetBodyType("Dynamic")
local type = GetBodyType()

-- Масса и инерция
local mass = GetMass()
local inertia = GetInertia()

-- Демпфирование (сопротивление движению, как вязкость)
SetLinearDamping(2.0)
SetAngularDamping(1.0)
local ld = GetLinearDamping()
local ad = GetAngularDamping()

-- Фиксированное вращение (без вращения от физики)
SetFixedRotation(true)
local fixed = IsFixedRotation()

-- Bullet-режим (непрерывное обнаружение столкновений для быстрых объектов)
SetBullet(true)
local isBullet = IsBullet()

-- Сон (оптимизация для неподвижных тел)
SetAwake(true)
EnableSleep(true)
local awake = IsAwake()
local sleepEnabled = IsSleepEnabled()

-- Включить/выключить физическое тело
SetBodyEnabled(true)
local bodyEnabled = IsBodyEnabled()

-- Угловая скорость (градусы/сек)
SetAngularVelocity(90)
local av = GetAngularVelocity()
```

### Заморозка и Ragdoll

```lua
-- Заморозить (сделать статическим)
Freeze()

-- Разморозить (сделать динамическим)
Unfreeze()

-- Ragdoll-физика (тряпичная кукла)
EnableRagdoll()
DisableRagdoll()
local ragdoll = IsRagdoll()

-- Ragdoll с начальным импульсом (для эффекта удара)
EnableRagdollWithImpulse(300, -200, 5.0)
-- ix, iy = линейный импульс, angularImpulse = вращательный
```

### Прыжок и приседание

```lua
-- Прыжок (только если на земле, задаёт скорость вверх)
Jump(500)                        -- Прыжок с силой 500 (работает только при IsGrounded)

-- Принудительный прыжок (игнорирует проверку земли — для двойного прыжка, прыжка от стены и т.д.)
JumpForce(400)                   -- Работает всегда, даже в воздухе

-- Остановка прыжка (обрезает вертикальную скорость — для контроля высоты прыжка)
-- Вызывать при отпускании кнопки прыжка. Умножает скорость вверх на 0.25.
StopJump()

-- Состояние прыжка
local jumping = IsJumping()      -- true если в воздухе после Jump/JumpForce (сбрасывается при приземлении)

-- Счётчик прыжков (авто-сброс при приземлении)
local count = GetJumpCount()     -- 0 на земле, 1 после первого, 2 после двойного...
ResetJumpCount()                 -- Принудительный сброс (напр. прыжок от стены даёт доп. прыжок)

-- Приседание (уменьшает коллайдеры, ноги остаются на месте)
Crouch()                         -- По умолчанию: 50% высоты
Crouch(0.6)                      -- 60% от оригинальной высоты коллайдера

-- Встать (восстановить оригинальный размер коллайдера)
UnCrouch()

-- Проверить состояние приседания
local crouching = IsCrouching()
```

### Типичный пример прыжка + приседания

```lua
local MAX_JUMPS = 2  -- Двойной прыжок

function OnUpdate(dt)
    -- Прыжок с контролем высоты
    if IsKeyJustPressed("space") and GetJumpCount() < MAX_JUMPS then
        if GetJumpCount() == 0 then
            Jump(600)            -- Первый прыжок (с земли)
        else
            JumpForce(500)       -- Прыжок в воздухе (двойной)
        end
    end

    -- Отпускание = обрезка прыжка (разная высота)
    if IsKeyJustReleased("space") then
        StopJump()
    end

    -- Приседание
    if IsKeyPressed("s") or IsKeyPressed("down") then
        Crouch(0.5)
    else
        UnCrouch()
    end
end
```

### Состояние движения

```lua
-- Двигается ли сущность? (скорость > порога, по умолчанию 1.0)
local moving = IsMoving()
local moving = IsMoving(5.0)  -- Свой порог

-- Нормализованный вектор направления скорости → {x, y}
local dir = GetMovementDirection()

-- Не на земле? (алиас для not IsGrounded())
local airborne = IsInAir()
```

### Формы, телепорт и физическое положение

```lua
-- Количество коллайдер-шейпов тела
local shapeCount = GetShapeCount()

-- Настройки форм (по индексу)
SetShapeDensity(1.0, 0)
SetShapeFriction(0.3, 0)
SetShapeRestitution(0.2, 0)
SetShapeSensor(true, 0)

-- Получить настройки форм (по индексу)
local density = GetShapeDensity(0)
local friction = GetShapeFriction(0)
local restitution = GetShapeRestitution(0)
local isSensor = IsShapeSensor(0)

-- Односторонняя платформа (форма пропускает с одной стороны)
SetShapeOneWay(true, 0)
local oneWay = IsShapeOneWay(0)

-- Направление одностороннего прохода: 1 = Вверх (по умолчанию, твёрдая сверху),
-- 2 = Вниз, 3 = Влево, 4 = Вправо. Направление — это сторона, с которой платформа ТВЁРДАЯ
-- (сторона, с которой тела не проходят сквозь). Учитывает поворот тела.
SetShapeOneWayDirection(1, 0)
local dir = GetShapeOneWayDirection(0)  -- возвращает 0, если форма не односторонняя

-- События контакта (колбэки начала/конца контакта)
SetShapeContactEvents(true, 0)
local contacts = AreShapeContactEventsEnabled(0)

-- События сенсора (колбэки входа/выхода из сенсора)
SetShapeSensorEvents(true, 0)
local sensors = AreShapeSensorEventsEnabled(0)

-- События удара (колбэки столкновений)
SetShapeHitEvents(true, 0)
local hits = AreShapeHitEventsEnabled(0)

-- События пре-солва (вызываются до расчёта ответа на столкновение)
SetShapePreSolveEvents(true, 0)
local preSolve = AreShapePreSolveEventsEnabled(0)

-- Телепорт (обновляет и Transform)
TeleportTo(100, 200)

-- Позиция/поворот из физики
local physPos = GetPhysicsPosition()  -- → {x, y}
local physRot = GetPhysicsRotation()
```

### Фильтры столкновений (Box2D)

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

-- Для другой сущности
SetEntityCollisionFilter(entityId, 0x0002, 0xFFFF, 0)
```

### Именованные группы столкновений (для текущей сущности)

> Применить **именованную** группу столкновений (задаётся в **Настройки → Коллизия**) ко всем шейпам `RigidbodyComponent` этой сущности в рантайме. `categoryBits` / `maskBits` группы берутся из матрицы столкновений автоматически.

```lua
-- Применить именованную группу ко всем шейпам тела.
-- Возвращает true при успехе, false если нет rigidbody / нет runtime-тела / группа не найдена.
local ok = SetCollisionGroupByName("Player")

-- Имя группы, назначенной на первый шейп этого тела
-- (ищется по его categoryBits). Возвращает "" если не найдено / нет runtime-тела.
local groupName = GetCollisionGroupName()

-- Сменить режим столкновений для всех шейпов тела.
-- 0 = NoCollision     (maskBits = 0, никаких контактных/сенсорных/hit-событий)
-- 1 = QueryOnly       (принудительно sensor; только sensor-события)
-- 2 = PhysicsOnly     (твёрдое тело; contact + hit события, без sensor-событий)
-- 3 = QueryAndPhysics (твёрдое тело; все события включены — по умолчанию)
SetCollisionEnabled(2)
```

> Эти функции меняют только **рантайм**-фильтр/флаги Box2D — сериализованные поля `CollisionGroupIndex` / `CollisionEnabled` у компонентов коллайдера не изменяются. Используйте их в `OnUpdate` / обработчиках событий для смены поведения на лету.

### Группы столкновений (глобально — `CollisionGroups.*`)

> **Тип:** Глобально. Чтение / редактирование проектной матрицы столкновений из Lua. Группы и попарные флаги `collide?` задаются в **Настройки → Коллизия** (сохраняются в `Config/CollisionGroups.json`).

```lua
-- Поиск
local idx     = CollisionGroups.GetIndex("Player")     -- → int (−1 если не найдена)
local name    = CollisionGroups.GetName(idx)           -- → string ("" если слот пуст)
local count   = CollisionGroups.GetCount()             -- → int (число слотов, макс 64)
local names   = CollisionGroups.GetAllNames()          -- → массив имён непустых групп

-- Сырые биты фильтра Box2D для индекса группы
local catBits  = CollisionGroups.GetCategoryBits(idx)  -- → uint64
local maskBits = CollisionGroups.GetMaskBits(idx)      -- → uint64

-- Попарная матрица столкновений
local a = CollisionGroups.GetIndex("Player")
local b = CollisionGroups.GetIndex("Enemy")
local doesCollide = CollisionGroups.DoCollide(a, b)    -- → bool
CollisionGroups.SetCollide(a, b, true)                 -- симметрично; также обновляет mask bits

-- Построить 64-битную маску слоёв из имён групп (или индексов). Удобно
-- передавать в фильтрацию LineTrace / Overlap*.
local mask = CollisionGroups.LayerMaskFromNames({"Enemy", "Environment"})
local all  = CollisionGroups.LayerMaskAll()           -- → uint64 (~0)
local none = CollisionGroups.LayerMaskNone()          -- → uint64 (0)

-- Постоянный trace-layer override (thread-local). Пока установлен
-- ненулевой mask, каждый вызов LineTrace / CircleTrace / BoxTrace /
-- CapsuleTrace / Overlap* использует его как query-фильтр. 0 — сброс.
CollisionGroups.SetTraceLayerMask(mask)
local cur = CollisionGroups.GetTraceLayerMask()        -- → uint64

-- Scope-хелпер: выполняет `fn` с временно установленной trace-маской,
-- после чего автоматически восстанавливает предыдущее значение
-- (в т.ч. при ошибке в Lua). Возвращает первое значение, которое вернул `fn`.
local hit = CollisionGroups.WithLayerMask(mask, function()
    return LineTrace(x1, y1, x2, y2)
end)
```

### Управление физикой других сущностей

```lua
-- Установить скорость другой сущности
SetEntityVelocity(entityId, 150, 0)

-- Добавить импульс другой сущности
AddEntityImpulse(entityId, 0, -400)

-- Масштаб гравитации другой сущности
SetEntityGravityScale(entityId, 0.5)
```

### Mouse Joint (перетаскивание физического тела)

```lua
CreateMouseJoint(targetX, targetY)
CreateMouseJoint(targetX, targetY, 1000, 5.0, 0.7)  -- maxForce, hertz, dampingRatio

SetMouseJointTarget(targetX, targetY)
DestroyMouseJoint()
local has = HasMouseJoint()
```

### Настройки физики мира

> Эти функции управляют параметрами физики на уровне сцены. Доступны из любой сущности с **RigidbodyComponent**.

```lua
-- Пикселей на метр (только чтение)
local ppm = GetPPM()

-- Количество подшагов (итерации солвера за шаг, 1..64)
SetSubStepCount(8)
local sub = GetSubStepCount()

-- Фиксированный шаг времени (размер шага физики)
SetFixedTimestep(1/60)
local step = GetFixedTimestep()

-- Порог реституции (минимальная скорость для отскока)
SetRestitutionThreshold(1.0)
local rest = GetRestitutionThreshold()

-- Порог скорости для событий удара
SetHitEventThreshold(5.0)
local hit = GetHitEventThreshold()

-- Настройка контактов (hertz, dampingRatio, maxPushSpeed)
SetContactTuning(60, 0.5, 3.0)
local ct = GetContactTuning()  -- → {hertz, dampingRatio, pushSpeed}

-- Максимальная линейная скорость
SetMaxLinearSpeed(200)
local maxSpeed = GetMaxLinearSpeed()

-- Включить/выключить засыпание тел
SetWorldSleeping(true)
local sleep = IsWorldSleepingEnabled()

-- Включить/выключить непрерывное обнаружение столкновений
SetWorldContinuous(true)
local continuous = IsWorldContinuousEnabled()
```

---

## 6. Input — Ввод (клавиатура, мышь, геймпад, тач)

> **Тип:** Глобальные функции

### Клавиатура

```lua
-- Клавиша зажата?
if IsKeyPressed("space") then Jump() end
if IsKeyPressed("w") then MoveUp() end

-- Клавиша ТОЛЬКО ЧТО нажата? (один кадр)
if IsKeyJustPressed("e") then Interact() end

-- Клавиша ТОЛЬКО ЧТО отпущена?
if IsKeyJustReleased("shift") then StopSprint() end

-- Любая из клавиш нажата?
if IsAnyKeyPressed("w", "up") then MoveUp() end

-- ВСЕ клавиши нажаты?
if IsAllKeysPressed("ctrl", "s") then Save() end
```

**Доступные имена клавиш:**

| Категория | Клавиши |
|-----------|---------|
| Буквы | `a`-`z` |
| Цифры | `0`-`9` |
| Стрелки | `up`, `down`, `left`, `right` |
| Управление | `space`, `enter`/`return`, `escape`/`esc`, `tab`, `backspace`, `delete` |
| Модификаторы | `shift`/`lshift`/`rshift`, `ctrl`/`lctrl`/`rctrl`, `alt`/`lalt`/`ralt` |
| Функциональные | `f1`-`f12` |
| Numpad | `numpad0`-`numpad9`, `numpad_enter`, `numpad_plus`, `numpad_minus` и т.д. |
| Прочие | `capslock`, `numlock`, `scrolllock`, `printscreen`, `pause`, `insert`, `home`, `end`, `pageup`, `pagedown`, `grave`, `semicolon`, `comma`, `period`, `slash`, `backslash`, `minus`, `equals`, `leftbracket`, `rightbracket`, `apostrophe` |

### Оси (для управления движением)

```lua
-- Одна ось: -1 (negative), 0, или +1 (positive)
local moveX = GetAxis("a", "d")     -- A = -1, D = +1

-- 2D ось (нормализованная!) → {x, y}
local move = GetAxis2D("a", "d", "s", "w")
SetVelocity(move.x * speed, move.y * speed)

-- Универсальное движение (клавиатура + геймпад автоматически)
local input = GetUniversalMovement("a", "d", "s", "w")
```

### Мышь

```lua
-- Кнопки мыши (1 = левая, 2 = средняя, 3 = правая)
if IsMousePressed(1) then Shoot() end
if IsMouseJustPressed(3) then Aim() end
if IsMouseJustReleased(1) then StopShoot() end

-- Позиция мыши (экранные координаты)
local mx = GetMouseX()
local my = GetMouseY()
local mpos = GetMousePosition()  -- → {x, y}

-- Позиция мыши в мировых координатах
local wpos = GetMouseWorldPosition()  -- → {x, y}

-- Конвертация координат экран ↔ мир
local world = ScreenToWorld(400, 300)  -- → {x, y}
local screen = WorldToScreen(10, 20)   -- → {x, y}

-- Прокрутка колёсика
local scroll = GetMouseScroll()          -- вертикальная
local scrollX = GetMouseScrollX()        -- горизонтальная (трекпад, тилт-колёсико)

-- Дельта мыши за кадр (для камеры, drag и т.д.)
local delta = GetMouseDelta()  -- → {x, y} пикселей

-- Относительный режим мыши (FPS — захватывает и прячет мышь, отдаёт только дельты)
SetRelativeMouseMode(true)               -- включить
SetRelativeMouseMode(false)              -- выключить
local relative = GetRelativeMouseMode()  -- включён ли?

-- Курсор (только .ice_sprite и .ice_flipbook ассеты)
ShowCursor()
HideCursor()
local visible = IsCursorVisible()
SetCursor("Content/Sprites/cursor.ice_sprite", 0, 0, 1.0)              -- Авто-детект по расширению
SetCursorSprite("Content/Sprites/cursor.ice_sprite", 0, 0, 1.0)        -- Явно из спрайта
SetCursorFlipbook("Content/Flipbooks/cursor.ice_flipbook", 0, 0, 1.0)  -- Анимированный
SetCursorHotspot(8, 8)              -- Поменять точку нажатия (pivot)
SetCursorScale(2.0)                 -- Поменять размер
local hx, hy = GetCursorHotspot()
local s = GetCursorScale()
ResetCursor()
local hasCursor = HasCustomCursor()
local cursorPath = GetCustomCursorPath()
local animated = IsCursorAnimated()
local cursorType = GetCursorType()  -- "None" | "Sprite" | "Flipbook"
SetCursorPosition(400, 300)
```

### Геймпад

```lua
-- Подключён?
if IsGamepadConnected() then ... end
if IsGamepadConnected(1) then ... end  -- Второй геймпад
local count = GetGamepadCount()
local name = GetGamepadName()

-- Кнопки (строки или PlayStation-названия)
if IsGamepadButtonPressed("a") then Jump() end       -- Xbox: A
if IsGamepadButtonPressed("cross") then Jump() end    -- PS: ×
if IsGamepadButtonJustPressed("x") then Attack() end  -- Xbox: X / PS: □
if IsGamepadButtonJustReleased("b") then ... end

-- Стики → {x, y} (от -1 до 1)
local left = GetGamepadLeftStick()
local right = GetGamepadRightStick()

-- Триггеры (0..1)
local lt = GetGamepadTriggerLeft()
local rt = GetGamepadTriggerRight()

-- Универсальная ось
local axis = GetGamepadAxis("leftx")  -- "leftx", "lefty", "rightx", "righty", "lt"/"l2"/"triggerleft", "rt"/"r2"/"triggerright"

-- Вибрация
SetGamepadRumble(0.5, 0.8, 200)  -- lowFreq, highFreq, миллисекунды
StopGamepadRumble()

-- Вибрация триггеров (Xbox/PS5 DualSense)
SetGamepadTriggerRumble(0.3, 0.7, 200)  -- left, right, миллисекунды
StopGamepadTriggerRumble()

-- LED (PS4/PS5 лайтбар)
SetGamepadLED(255, 0, 0)           -- красный
SetGamepadLED(0, 255, 0, 1)       -- зелёный, второй геймпад

-- Тип контроллера
local gtype = GetGamepadType()  -- "xbox360"/"xboxone"/"ps3"/"ps4"/"ps5"/"switch_pro"/"joycon_left"/"joycon_right"/"joycon_pair"/"gamecube"/"standard"/"unknown"

-- Метки кнопок (реальное название кнопки в зависимости от типа контроллера)
local label = GetGamepadButtonLabel("a")  -- "a" на Xbox, "cross" на PlayStation

-- Батарея / питание
local percent = GetGamepadPowerPercent()  -- 0-100 или -1 если неизвестно
local pstate = GetGamepadPowerState()     -- "battery" / "charging" / "charged" / "no_battery" / "unknown"

-- Тачпад (DualSense / DualShock 4)
if GamepadHasTouchpad() then
    local maxFingers = GetGamepadTouchpadFingerCount()  -- макс. поддерживаемых пальцев
    local finger = GetGamepadTouchpadFinger(0)          -- → {down, x, y, pressure}
end

-- Сенсоры геймпада (DualShock 4 / DualSense / Joy-Con)
if GamepadHasSensor("gyro") then
    SetGamepadSensorEnabled("gyro", true)
    local gyro = GetGamepadSensorData("gyro")  -- → {x, y, z} рад/сек
    local rate = GetGamepadSensorDataRate("gyro")
end
if GamepadHasSensor("accel") then
    SetGamepadSensorEnabled("accel", true)
    local accel = GetGamepadSensorData("accel")  -- → {x, y, z} м/с²
end
local enabled = IsGamepadSensorEnabled("gyro")

-- Мёртвая зона стиков
SetGamepadDeadzone(0.15)
local dz = GetGamepadDeadzone()

-- Проверка возможностей геймпада (особенно важно в Web — браузеры обычно отключают вибро)
if IsGamepadRumbleSupported() then SetGamepadRumble(0.5, 0.5, 200) end
if IsGamepadTriggerRumbleSupported() then SetGamepadTriggerRumble(0.3, 0.7, 200) end
if IsGamepadLEDSupported() then SetGamepadLED(0, 255, 0) end

-- Для конкретного геймпада (0-3)
if IsGamepadRumbleSupported(1) then SetGamepadRumble(0.5, 0.5, 200, 1) end
```

| Функция | Описание |
|---|---|
| `IsGamepadRumbleSupported(idx?)` | Поддерживает ли геймпад low/high frequency вибро (в большинстве браузеров вернёт false) |
| `IsGamepadTriggerRumbleSupported(idx?)` | Поддерживает ли геймпад вибро адаптивных триггеров (DualSense / Xbox Elite) |
| `IsGamepadLEDSupported(idx?)` | Есть ли у геймпада RGB-LED, которым может управлять `SetGamepadLED` |

**Типы сенсоров геймпада:**

| Строка | Описание |
|---|---|
| `"accel"` / `"accelerometer"` | Акселерометр |
| `"gyro"` / `"gyroscope"` | Гироскоп |
| `"accel_l"` | Акселерометр левого Joy-Con |
| `"gyro_l"` | Гироскоп левого Joy-Con |
| `"accel_r"` | Акселерометр правого Joy-Con |
| `"gyro_r"` | Гироскоп правого Joy-Con |

**Кнопки геймпада:**

| Xbox | PlayStation | Строка |
|------|-----------|--------|
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

### Тач-скрин (мобильные)

```lua
-- Поддержка
local supported = IsTouchSupported()
local count = GetTouchCount()

-- Нажатия
if IsTouchPressed() then ... end
if IsTouchJustPressed(0) then ... end  -- Палец 0
if IsTouchJustReleased(0) then ... end

-- Позиция пальца (нормализованная 0..1)
local pos = GetTouchPosition(0)  -- → {x, y}

-- Позиция пальца (пиксели)
local pix = GetTouchPositionPixels(0)  -- → {x, y}

-- Дельта (смещение)
local delta = GetTouchDelta(0)  -- → {x, y}

-- Давление
local pressure = GetTouchPressure(0)

-- Мультитач жесты
local pinching = IsPinching()
local scale = GetPinchScale()
local rotation = GetPinchRotation()
```

### Свайпы

```lua
-- Был ли свайп в этом кадре?
if IsSwipe() then
    local dir = GetSwipeDirection()  -- "left", "right", "up", "down"
    Log("Свайп: " .. dir)
end

-- Проверка конкретного направления
if IsSwipeLeft() then ... end
if IsSwipeRight() then ... end
if IsSwipeUp() then ... end
if IsSwipeDown() then ... end

-- Количество свайпов (мультитач — несколько пальцев одновременно)
local count = GetSwipeCount()
for i = 0, count - 1 do
    local dir = GetSwipeDirection(i)
    local delta = GetSwipeDelta(i)         -- → {x, y} (нормализованная дельта)
    local velocity = GetSwipeVelocity(i)   -- скорость (расстояние/секунда)
    local distance = GetSwipeDistance(i)    -- пройденное расстояние (нормализованное)
end

-- Настройка чувствительности
SetSwipeMinDistance(0.05)   -- мин. расстояние для срабатывания (по умолчанию 0.05 = 5% экрана)
SetSwipeMaxDuration(0.5)    -- макс. время свайпа в секундах (по умолчанию 0.5)
local minDist = GetSwipeMinDistance()
local maxDur = GetSwipeMaxDuration()
```

| Функция | Описание |
|---|---|
| `IsSwipe()` | Был ли свайп в этом кадре (любое направление) |
| `IsSwipeLeft()` / `Right()` / `Up()` / `Down()` | Свайп в конкретном направлении |
| `GetSwipeCount()` | Количество свайпов в этом кадре |
| `GetSwipeDirection(index?)` | Направление: `"left"`, `"right"`, `"up"`, `"down"` |
| `GetSwipeDelta(index?)` | `{x, y}` — нормализованное смещение пальца |
| `GetSwipeVelocity(index?)` | Скорость свайпа (расстояние/секунда) |
| `GetSwipeDistance(index?)` | Длина свайпа (нормализованная, 0..1) |
| `SetSwipeMinDistance(float)` | Мин. расстояние для срабатывания (по умолчанию `0.05`) |
| `GetSwipeMinDistance()` | Текущее мин. расстояние |
| `SetSwipeMaxDuration(float)` | Макс. длительность свайпа в секундах (по умолчанию `0.5`) |
| `GetSwipeMaxDuration()` | Текущая макс. длительность |

### Вибрация устройства (Haptic)

```lua
-- Поддержка
if IsHapticSupported() then
    PlayHaptic(0.7, 300)    -- сила (0..1), длительность в мс
    StopHaptic()             -- остановить вибрацию
end

-- Чёткий отклик интерфейса: один системный тап вместо самодельного «бззз»
PlayHapticPreset("tick")          -- "tick" | "click" | "doubleclick" | "heavyclick"

-- Своя форма волны: длительности отрезков + сила каждого отрезка
PlayHapticPattern({ 0, 40, 80, 120 }, { 0.0, 0.4, 0.0, 1.0 })

-- Зацикленный паттерн: повтор с отрезка 1 -> 60 мс вибрации, 60 мс паузы, ...
PlayHapticPattern({ 60, 60 }, { 1.0, 0.0 }, 1)
StopHaptic()                      -- зацикленный паттерн иначе не остановить
```

| Функция | Описание |
|---|---|
| `IsHapticSupported()` | Есть ли haptic-устройство (мобильная вибрация, вибромотор и т.д.) |
| `IsHapticAmplitudeControlSupported()` | Учитывает ли устройство `strength`. Если `false`, любая вибрация играется на системной громкости и форму задаёт только `durationMs` |
| `PlayHaptic(strength, durationMs)` | Запустить вибрацию (`strength` 0..1, `durationMs` — миллисекунды) |
| `PlayHapticPreset(name?)` | Короткий системный эффект: `"tick"` (по умолчанию), `"click"`, `"doubleclick"`, `"heavyclick"` |
| `PlayHapticPattern(timingsMs, amplitudes?, repeatIndex?)` | Проиграть форму волны. `timingsMs[i]` — длина отрезка `i` в мс, `amplitudes[i]` — его сила `0..1` (`0` = пауза). Без `amplitudes` отрезки чередуются вкл/выкл, начиная с **вкл**. `repeatIndex` — индекс таблицы (с 1), на который паттерн зацикливается до `StopHaptic()`; без него (или `-1`) играется один раз |
| `StopHaptic()` | Остановить текущую вибрацию |

> **Бэкенды по платформам:**
> • **Windows / Linux / macOS** — первое SDL force-feedback устройство (FFB-руль или джойстик). Без такого железа возвращается `false`. Пресеты сводятся к короткому `PlayHaptic`, паттерны — к одной вибрации длиной суммы «включённых» отрезков на пиковой силе.
> • **Android** — движок нативно управляет `android.os.Vibrator` через `IceBoxHaptics` (на Android 12+ — через `VibratorManager`), поэтому `strength` превращается в настоящую амплитуду `VibrationEffect` там, где мотор это умеет, `PlayHapticPreset` берёт подобранные вендором `EFFECT_TICK / CLICK / DOUBLE_CLICK / HEAVY_CLICK`, а `PlayHapticPattern` — это `VibrationEffect`-волна с зацикливанием. Каждая вибрация помечается игровыми/медийными usage-атрибутами, поэтому ей управляет громкость отклика самой игры, а не системный тумблер «отклик при касании». Нужно разрешение `VIBRATE` — оно уже объявлено в шаблоне Android-сборки. Если нативного класса в проекте нет (старый сгенерированный проект), движок молча откатывается на haptic-устройство SDL.
> • **iOS** — Core Haptics (`CHHapticEngine`) с запасным путём через `UIImpactFeedbackGenerator`: в самом SDL на iOS собирается только dummy-драйвер haptic, поэтому Taptic Engine движок дёргает нативно. Длительность 60 мс и меньше играется одиночным коротким тапом, длиннее — непрерывным событием; паттерн превращается в `CHHapticPattern` из нескольких событий. `repeatIndex` игнорируется — у Core Haptics нет зацикливания паттерна, перезапускайте его из Lua. У iPad / iPod touch вибромотора нет, там `false`.
> • **Web** — `navigator.vibrate` в мобильных браузерах (Chrome, Firefox, Edge, Samsung Internet на Android). Десктопные браузеры дают `false`. **Safari никогда не реализовывал Vibration API, поэтому браузер на iOS/iPadOS завибрировать не может в принципе** — это ограничение платформы, а не движка. Управления силой в Web API нет: `strength` там игнорируется, работает только `durationMs` (`IsHapticAmplitudeControlSupported()` вернёт `false`); паттерн переводится в массив вкл/выкл для `navigator.vibrate`, `repeatIndex` игнорируется. Chrome вдобавок гасит вызовы вибрации до первого взаимодействия пользователя со страницей.
>
> `PlayHaptic` работает с *устройством*, а не с геймпадом. Для моторов геймпада всегда используйте `SetGamepadRumble(...)` — он в браузере **работает** (Chromium отдаёт `gamepad.vibrationActuator`), так что веб-сборка может трясти пад даже там, где `IsHapticSupported()` возвращает `false`.
>
> На мобильных устройствах системный бэкенд всегда имеет приоритет над haptic-устройством SDL, а при уходе приложения в фон вибрация останавливается сама — зацикленный паттерн не сможет жужжать за домашним экраном.
>
> **В удалённом предпросмотре редактора** все вызовы из этого раздела уходят на подключённое Android-устройство, а не на десктоп, и `IsHapticSupported()` возвращает `true` — мобильную вибрацию можно почувствовать и настроить без полной сборки. См. *Редактор → Удалённый предпросмотр*.

### Force-Feedback эффекты (расширенный haptic)

Кроме простого rumble движок даёт доступ ко всему конвейеру SDL3 `SDL_HapticEffect`: **постоянная сила**, **периодические волны** (sine, triangle, sawtooth), **рампы** (плавный рост / спад), **leftright** (независимые низко- и высокочастотный моторы консольных геймпадов). Эффекты проигрываются на **первом открытом haptic-устройстве** (FFB-руль, джойстик, геймпад с FFB). Мобильные вибромоторы обычно поддерживают только `"leftright"`.

Каждый эффект описывается Lua-таблицей, общие поля:

| Поле | Смысл | По умолчанию |
|---|---|---|
| `type` | `"constant"` │ `"sine"` │ `"triangle"` │ `"sawtoothup"` │ `"sawtoothdown"` │ `"ramp"` │ `"leftright"` | `"sine"` |
| `length` (или `duration`) | Длительность эффекта в мс | `1000` |
| `delay` | Задержка перед стартом, мс | `0` |
| `magnitude` (или `level`, `start`) | Сила 0..1 (постоянный уровень / пик волны / старт рампы) | `1.0` |
| `finish` (или `end`) | Конечная сила рампы 0..1 | `0.0` |
| `period` | Период одного цикла, мс (только периодические) | `100` |
| `direction` | Полярное направление в градусах (0 = вперёд, 90 = вправо) | `0` |
| `attack_length` / `attack_level` | Огибающая нарастания: длина (мс) и пик (0..1) | `0` / `0` |
| `fade_length` / `fade_level` | Огибающая затухания: длина (мс) и конечная сила (0..1) | `0` / `0` |
| `large` / `small` | Низкомотор и высокомотор 0..1 (только `leftright`) | `0.5` / `0.5` |

```lua
-- Проверка возможностей устройства (всегда делайте это на Web/мобильных)
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
    -- id можно переиспользовать / остановить / удалить
end

-- Однократный «удар» постоянной силы
PlayHapticEffect{ type = "constant", level = 0.9, length = 120, direction = 90 }

-- Растущая рампа (раскрутка двигателя)
PlayHapticEffect{ type = "ramp", start = 0.0, finish = 1.0, length = 800 }

-- Консольный двухмоторный рамбл (Xbox / DualSense)
PlayHapticEffect{ type = "leftright", large = 0.8, small = 0.3, length = 250 }

-- Ручной жизненный цикл: создаём раз, запускаем многократно, обновляем на лету
local sineId = CreateHapticEffect{ type = "sine", magnitude = 0.4, period = 60, length = 9999999 }
RunHapticEffect(sineId, 1)              -- 1 итерация; 0 = бесконечно
-- ...позже меняем параметры без перезапуска:
UpdateHapticEffect(sineId, { type = "sine", magnitude = 0.9, period = 30, length = 9999999 })
if IsHapticEffectPlaying(sineId) then ... end
StopHapticEffect(sineId)
DestroyHapticEffect(sineId)             -- освободить слот на устройстве

-- Остановить всё (удобно при паузе / смене сцены)
StopAllHapticEffects()

-- Запросы
local feats = GetHapticFeatures()  -- { constant=true, sine=true, ramp=false, leftright=true, ... }
local slots = GetHapticMaxEffects()         -- напр. 32 (сколько эффектов можно загрузить)
local slotsPlaying = GetHapticMaxEffectsPlaying()  -- напр. 16 (сколько может играть одновременно)
```

| Функция | Описание |
|---|---|
| `PlayHapticEffect(cfg, iterations?)` | Создать + запустить эффект. Возвращает `effectId` (>=0) или `-1`. `iterations` по умолчанию `1`, `0` — бесконечно |
| `CreateHapticEffect(cfg)` | Загрузить эффект на устройство. Возвращает `effectId` (>=0) или `-1`. **Не** запускает его |
| `RunHapticEffect(id, iterations?)` | Запустить существующий эффект (`0` — бесконечно) |
| `UpdateHapticEffect(id, cfg)` | Изменить параметры играющего/загруженного эффекта на лету |
| `StopHapticEffect(id)` | Остановить один эффект сразу |
| `DestroyHapticEffect(id)` | Освободить слот на устройстве |
| `StopAllHapticEffects()` | Остановить все играющие эффекты |
| `IsHapticEffectPlaying(id)` | Играет ли эффект сейчас |
| `IsHapticEffectTypeSupported(typeName)` | Поддерживает ли устройство данный тип эффекта |
| `GetHapticFeatures()` | Таблица boolean'ов: `constant, sine, triangle, sawtoothup, sawtoothdown, ramp, leftright, spring, damper, inertia, friction, custom` |
| `GetHapticMaxEffects()` | Сколько всего слотов на устройстве |
| `GetHapticMaxEffectsPlaying()` | Сколько эффектов может играть одновременно |

> **Платформы:**
> • FFB-рули (Logitech G29/G923, Thrustmaster T-серия, Fanatec) обычно поддерживают все типы: `constant`, `sine`, `ramp`, `friction`, `damper`.
> • Xbox / DualSense обычно дают только `leftright` (плюс вибро триггеров — см. `SetGamepadTriggerRumble`).
> • На мобильных и в Web SDL haptic-эффекты **не** поддерживаются — всегда сначала проверяйте `IsHapticEffectTypeSupported(...)`; для вибро телефона используйте `PlayHaptic(...)`.

### Сенсоры устройства (акселерометр / гироскоп телефона)

```lua
-- Проверить наличие встроенного сенсора (телефон/планшет)
if IsDeviceSensorAvailable("accel") then
    SetDeviceSensorEnabled("accel", true)          -- нужно включить перед чтением
    local accel = GetDeviceSensorData("accel")      -- → {x, y, z} м/с²
    local enabled = IsDeviceSensorEnabled("accel")
end

if IsDeviceSensorAvailable("gyro") then
    SetDeviceSensorEnabled("gyro", true)
    local gyro = GetDeviceSensorData("gyro")        -- → {x, y, z} рад/с
end

-- Выключить когда больше не нужно
SetDeviceSensorEnabled("accel", false)
SetDeviceSensorEnabled("gyro", false)
```

| Функция | Описание |
|---|---|
| `IsDeviceSensorAvailable(type)` | Есть ли сенсор физически на устройстве |
| `SetDeviceSensorEnabled(type, bool)` | Открыть/закрыть сенсор (нужно включить перед чтением) |
| `IsDeviceSensorEnabled(type)` | Открыт ли сенсор в данный момент |
| `GetDeviceSensorData(type)` | `{x, y, z}` — данные сенсора (акселерометр: м/с², гироскоп: рад/с) |

**Типы сенсоров:**

| Строка | Смысл | Единицы | Платформы |
|---|---|---|---|
| `"accel"` / `"accelerometer"` | Акселерометр | м/с² (`{x,y,z}`) | Все (SDL3) |
| `"gyro"` / `"gyroscope"` | Гироскоп | рад/с (`{x,y,z}`) | Все (SDL3) |
| `"mag"` / `"magnetometer"` | Магнитное поле | µТл (`{x,y,z}`) | Android (JNI), iOS (CoreMotion) |
| `"compass"` / `"heading"` | Синтетический — включает источник курса для `GetCompassHeading()` | — | Android (JNI), iOS (CoreMotion) |
| `"baro"` / `"barometer"` / `"pressure"` | Атмосферное давление | гПа (поле `x`) | Android (JNI), iOS (CMAltimeter) |
| `"gravity"` | Вектор гравитации | м/с² (`{x,y,z}`) | Android (JNI), iOS (CoreMotion) |
| `"linearaccel"` | Линейное ускорение (без гравитации) | м/с² (`{x,y,z}`) | Android (JNI), iOS (CoreMotion) |
| `"rotationvector"` | Объединённый вектор вращения | компоненты един. кватерниона | Android (JNI), iOS (CoreMotion) |
| `"proximity"` | Датчик приближения | см (`x`) — на iOS `0` (близко) или `5` (далеко) | Android (JNI), iOS (UIDevice) |
| `"light"` | Освещённость | люкс (`x`) | **Только Android** |
| `"stepcounter"` / `"step"` | Счётчик шагов | шт (`x`) | Android (JNI), iOS (CMPedometer) — см. примечание про точку отсчёта ниже |
| `"ambienttemperature"` / `"temperature"` | Температура воздуха | °C (`x`) | **Только Android** |
| `"humidity"` | Относительная влажность | % (`x`) | **Только Android** |

> **Примечание:** Это **встроенные** сенсоры устройства (телефон/планшет), а не сенсоры геймпада.
> Для сенсоров геймпада (DualSense, Joy-Con) используйте `GamepadHasSensor()` / `GetGamepadSensorData()`.
>
> Имена сенсоров сравниваются без учёта регистра на всех платформах.
>
> **Счётчик шагов — точка отсчёта и разрешения.** У платформ разные точки отсчёта, поэтому считайте значение **монотонным счётчиком и работайте с разницей**, а не с абсолютным числом, которое везде значит одно и то же:
> • **Android** — `TYPE_STEP_COUNTER`, счёт с момента загрузки устройства. На Android 10+ нужно runtime-разрешение `ACTIVITY_RECOGNITION`. `IsDeviceSensorAvailable("stepcounter")` сообщает о *железе*, поэтому может вернуть `true`, пока счётчик остаётся `0` — запросите разрешение через `Permissions.Request(Permissions.ACTIVITY_RECOGNITION)` и добавьте его в дополнительные разрешения сборки.
> • **iOS** — `CMPedometer`, счёт с полуночи текущего дня. `SetDeviceSensorEnabled("stepcounter", true)` сразу подтягивает уже сделанные за сегодня шаги разовым запросом и дальше держит значение живым, поэтому первое чтение сразу корректное, а не `0`. Первое включение вызывает системный запрос «Движение и фитнес»; движок сам объявляет `NSMotionUsageDescription` в Info.plist (текст переопределяется через `NSMotionUsageDescription=<причина>` в Extra Usage Descriptions). В отличие от Android, `IsDeviceSensorAvailable("stepcounter")` вернёт `false`, если пользователь запретил или ограничил «Движение и фитнес», — то есть отказ виден игре.
>
> **Особенности Web:** SDL регистрирует акселерометр и гироскоп в *любом* браузере, поэтому `IsDeviceSensorAvailable("accel")` вернёт `true` даже в десктопном браузере без датчиков — значения просто остаются `0`. В Safari на iOS/iPadOS браузер вдобавок требует отдельное разрешение на движение: `SetDeviceSensorEnabled("accel", true)` запрашивает его сразу и, если вызов был не внутри пользовательского жеста, повторяет запрос один раз при следующем касании или клике — значения пойдут после того, как игрок тронет страницу.

### Компас и барометр (Android / iOS)

Акселерометр и гироскоп работают везде, а **магнитометр** (компас) и **барометр** (атмосферное давление / высота) есть только на Android и iOS, где движок обращается к ним через нативный мост. На десктопе и в Web эти вызовы возвращают `0` / `false`.

```lua
-- Компас (градусы, 0 = магнитный север, 90 = восток)
if IsDeviceSensorAvailable("compass") then
    SetDeviceSensorEnabled("compass", true)   -- внутри включает accel + magnetometer
    local heading = GetCompassHeading()        -- 0..360
end

-- Сырой вектор магнитного поля, если нужен
if IsDeviceSensorAvailable("magnetometer") then
    SetDeviceSensorEnabled("magnetometer", true)
    local mag = GetDeviceSensorData("magnetometer")  -- {x, y, z} µТл
end

-- Барометр
if IsDeviceSensorAvailable("barometer") then
    SetDeviceSensorEnabled("barometer", true)
    local p   = GetDeviceSensorData("pressure").x  -- гПа
    local alt = GetBarometricAltitude()             -- метры над уровнем моря (станд. атм.)
end

-- Освещённость + proximity (адаптивный UI)
SetDeviceSensorEnabled("light", true)
local lux = GetDeviceSensorData("light").x
SetDeviceSensorEnabled("proximity", true)
local nearCm = GetDeviceSensorData("proximity").x
```

| Функция | Описание |
|---|---|
| `GetCompassHeading()` | Магнитный курс в градусах (0..360, 0 = север). Требует включённый `"compass"`. **Android / iOS**, иначе `0` |
| `GetBarometricAltitude()` | Высота в метрах по текущему давлению относительно стандартной атмосферы (1013.25 гПа). Требует включённый `"barometer"`. **Android / iOS**, иначе `0` |

> Выключайте сенсоры, которые больше не нужны — они сажают батарею.
> **Десктоп / Web:** SDL3 не даёт там магнитометр и барометр; вызовы возвращают `0` и `false`, так что их безопасно вызывать всюду без `#ifdef`.

### Ввод текста

```lua
StartTextInput()               -- Включить режим ввода текста (открывает клавиатуру на мобильных)
local text = GetTextInput()    -- Получить введённый текст
ClearTextInput()               -- Очистить буфер
StopTextInput()                -- Выключить ввод
local active = IsTextInputActive()
```

### Экранная клавиатура (мобильная IME)

На Android / iOS вызов `StartTextInput()` поднимает программную клавиатуру. Эти хелперы помогают узнать, есть ли вообще софт-клавиатура на платформе и видна ли она сейчас — удобно для сдвига UI, чтобы клавиатура не закрывала поле ввода.

```lua
if HasScreenKeyboardSupport() then
    StartTextInput()
end

if IsScreenKeyboardShown() then
    -- Поднимаем UI вверх, чтобы экранная клавиатура не закрывала поле ввода
end
```

| Функция | Описание |
|---|---|
| `HasScreenKeyboardSupport()` | На платформе есть программная клавиатура (Android/iOS — true; десктоп обычно — false) |
| `IsScreenKeyboardShown()` | Программная клавиатура сейчас показана на активном окне |

### Joystick (raw HID — рули, HOTAS, аркадные стики, флайт-контроллеры)

В отличие от `Gamepad.*`, таблица `Joystick` даёт доступ к «сырым» HID-устройствам, которые SDL3 **не** маппит в стандартный gamepad-layout — игровые рули, секции газа, аркадные стики, танцевальные коврики, обычные USB-контроллеры. Поддерживается до 8 устройств одновременно. Те устройства, которые SDL уже опознаёт как геймпад, отфильтровываются (доступны только через Gamepad-API, без двойного учёта).

```lua
if Joystick.IsConnected() then
    Print("Руль/HOTAS: " .. Joystick.GetName() .. " (GUID " .. Joystick.GetGUID() .. ")")
    Print("Кнопок: " .. Joystick.GetButtonCount() .. ", осей: " .. Joystick.GetAxisCount())
end

local n = Joystick.GetCount()                       -- сколько raw-джойстиков подключено

-- Кнопки (нумерация с 0)
if Joystick.IsButtonPressed(0) then ... end
if Joystick.IsButtonJustPressed(3) then Fire() end
if Joystick.IsButtonJustReleased(3) then Reload() end

-- Оси — нормализованы в -1..1, с движковой deadzone
local steer    = Joystick.GetAxis(0)
local throttle = Joystick.GetAxis(1)
local brake    = Joystick.GetAxis(2)

-- Hat (POV) — 8-направленная строка
local hat = Joystick.GetHat(0)   -- "centered" | "up" | "down" | "left" | "right" | "leftup" | "rightup" | "leftdown" | "rightdown"

-- Трекбол — дельта (редко, старые джойстики)
local ball = Joystick.GetBall(0)  -- → {x, y}

-- Force feedback (только если железо рапортует поддержку)
Joystick.Rumble(0.7, 0.7, 250)    -- low, high, длительность мс
Joystick.SetLED(255, 0, 0)         -- у некоторых стиков есть RGB

-- Второе устройство
if Joystick.IsConnected(1) then
    local pedalsBrake = Joystick.GetAxis(0, 1)
end
```

| Функция | Описание |
|---|---|
| `Joystick.IsConnected(idx?)` | Слот занят (`idx` 0..7, по умолчанию 0) |
| `Joystick.GetCount()` | Сколько raw-джойстиков подключено |
| `Joystick.GetName(idx?)` | Имя устройства (читаемое) |
| `Joystick.GetGUID(idx?)` | Стабильный hex GUID — для сохранения настроек на устройство |
| `Joystick.GetButtonCount(idx?)` | Сколько кнопок |
| `Joystick.GetAxisCount(idx?)` | Сколько аналоговых осей |
| `Joystick.GetHatCount(idx?)` | Сколько hat (POV) |
| `Joystick.GetBallCount(idx?)` | Сколько трекболов |
| `Joystick.IsButtonPressed(b, idx?)` | Кнопка зажата сейчас |
| `Joystick.IsButtonJustPressed(b, idx?)` | Кнопка нажата в этом кадре |
| `Joystick.IsButtonJustReleased(b, idx?)` | Кнопка отпущена в этом кадре |
| `Joystick.GetAxis(a, idx?)` | Значение оси -1..+1 (deadzone применена) |
| `Joystick.GetHat(h, idx?)` | Направление hat (строка) |
| `Joystick.GetBall(b, idx?)` | `{x, y}` дельта трекбола |
| `Joystick.Rumble(low, high, durationMs, idx?)` | Force feedback 0..1 |
| `Joystick.SetLED(r, g, b, idx?)` | RGB-LED 0..255 |

### Перо / Стилус (Wacom, Apple Pencil, S-Pen, Surface Pen)

Полная поддержка перьев с давлением: давление пера, наклон по X/Y, расстояние до поверхности, поворот корпуса, конец-ластик и боковые кнопки. Работает на Windows (Wintab/Pointer Events), macOS, Linux, Android (S-Pen), iPadOS (Apple Pencil) и в Web (PointerEvent `pointerType="pen"`).

```lua
if Pen.IsActive() then
    local pos = Pen.GetPosition()           -- → {x, y} в пикселях
    local p   = Pen.GetPressure()           -- 0..1
    local tilt = Pen.GetTilt()              -- → {x, y} в градусах, -90..+90
    local rot  = Pen.GetRotation()          -- градусы (поворот корпуса)
    local dist = Pen.GetDistance()          -- 0..1 (1 = касание)

    if Pen.IsEraser() then
        EraseAt(pos.x, pos.y, p)
    elseif Pen.IsDown() then
        DrawAt(pos.x, pos.y, p)
    end
end

if Pen.IsJustPressed()  then BeginStroke() end
if Pen.IsJustReleased() then EndStroke() end

-- Боковые кнопки (нумерация с 1; обычно 1 = основная, 2 = переключение в ластик)
if Pen.IsButtonJustPressed(1) then OpenColorPicker() end
if Pen.IsButtonPressed(2)     then PanCanvas() end
```

| Функция | Описание |
|---|---|
| `Pen.IsSupported()` | Сборка движка поддерживает события пера |
| `Pen.IsActive()` | Перо в зоне видимости (висит над поверхностью или касается её) |
| `Pen.IsDown()` | Кончик касается поверхности |
| `Pen.IsJustPressed()` / `IsJustReleased()` | Edge-события за этот кадр |
| `Pen.GetPosition()` | `{x, y}` в пикселях |
| `Pen.GetPressure()` | 0..1 |
| `Pen.GetTilt()` | `{x, y}` градусы |
| `Pen.GetDistance()` | 0..1 (1 = касание) |
| `Pen.GetRotation()` | поворот корпуса в градусах |
| `Pen.IsEraser()` | Используется конец-ластик |
| `Pen.IsButtonPressed(n)` | Боковая кнопка `n` (1..32) зажата сейчас |
| `Pen.IsButtonJustPressed(n)` / `IsButtonJustReleased(n)` | Edge-события для боковой кнопки `n` |

### Drag & Drop (файлы / текст, перетянутые в окно)

Рантайм ловит файлы и текст, которые пользователь дропает на окно движка из ОС. Полезно для встроенных редакторов уровней, импорта ассетов на лету, кастомных скинов/карт и т.п. Каждый кадр `Update()` чистит списки, поэтому потреблять их нужно **в том же кадре**, что и дроп.

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
        DragDrop.Clear()  -- необязательно: следующий кадр сам зачистит
    end
end
```

| Функция | Описание |
|---|---|
| `DragDrop.HasItems()` | В этот кадр был дроп файла или текста |
| `DragDrop.GetFiles()` | Массив абсолютных путей |
| `DragDrop.GetTexts()` | Массив текстовых строк, перетянутых из другого приложения |
| `DragDrop.GetPosition()` | `{x, y}` точка дропа в координатах окна |
| `DragDrop.Clear()` | Принудительно опустошить буферы |

### Буфер обмена (Clipboard)

Текстовый буфер обмена работает на Windows / macOS / Linux / Android / Web (в Web для `SetText` нужен пользовательский жест). Primary-selection (`GetPrimarySelection` / `SetPrimarySelection`) — это X11-буфер по средней кнопке мыши, имеет смысл только на Linux.

```lua
if Clipboard.HasText() then
    local s = Clipboard.GetText()
end

Clipboard.SetText("Сохранённая строка из игры")

-- Только Linux/X11 — на других платформах безопасно отдаст пустоту
local sel = Clipboard.GetPrimarySelection()
Clipboard.SetPrimarySelection("primary")
```

| Функция | Описание |
|---|---|
| `Clipboard.GetText()` | Текущая строка буфера (`""` если пусто) |
| `Clipboard.SetText(text)` | Заменить содержимое буфера (в Web — нужен жест пользователя) |
| `Clipboard.HasText()` | В буфере есть текст |
| `Clipboard.GetPrimarySelection()` | X11 primary-selection (Linux); пусто на других платформах |
| `Clipboard.SetPrimarySelection(text)` | Установить X11 primary-selection |

### Веб-камера / Camera

Лёгкий доступ к системным камерам через SDL3. Работает на Windows, macOS, Linux, Android, iOS и в Web (в Web нужны HTTPS и подтверждение пользователя — первые 100–300 мс кадры могут быть недоступны). Подсистема камеры инициализируется лениво при первом вызове.

```lua
-- Перечислить устройства
for _, dev in ipairs(Webcam.GetDevices()) do
    Print(dev.id .. ": " .. dev.name)
end

-- Открыть камеру по умолчанию (или по конкретному id)
if Webcam.Open() then
    local fmt = Webcam.GetFormat()        -- → {width, height}
    Print("Камера готова: " .. fmt.width .. "x" .. fmt.height)
end

-- Покадровая проверка
function OnUpdate(dt)
    if Webcam.IsOpen() and Webcam.HasNewFrame() then
        Webcam.SaveFrameToPNG("saves/photo.png")
    end
end

Webcam.Close()
```

| Функция | Описание |
|---|---|
| `Webcam.GetDevices()` | Массив `{id, name}` |
| `Webcam.Open(deviceId?)` | Открывает устройство по id; без аргумента — первое доступное. `true` при успехе |
| `Webcam.Close()` | Закрывает активную камеру |
| `Webcam.IsOpen()` | Камера сейчас открыта |
| `Webcam.GetFormat()` | `{width, height}` согласованного потока |
| `Webcam.HasNewFrame()` | На этот опрос пришёл свежий кадр |
| `Webcam.SaveFrameToPNG(path)` | Берёт последний кадр и пишет его в PNG. `true` при успехе |

> **Web:** браузеры закрывают камеру за prompt'ом разрешения; пока пользователь не подтвердил, `IsOpen()` может быть `true`, но `HasNewFrame()` несколько кадров будет возвращать `false`.

### Action System (Привязка действий)

```lua
-- Привязать действие к кнопкам
BindAction("Jump", "space")
BindAction("Jump", "gamepad_a")
BindAction("Jump", "touch_0")
BindAction("Shoot", "mouse_1")

-- Мультиплеер: привязка к конкретному геймпаду (0-3)
BindAction("P2_Jump", "gamepad_1_a")     -- второй геймпад, кнопка A
BindAction("P2_Attack", "gamepad_1_x")   -- второй геймпад, кнопка X
BindAction("P3_Jump", "gamepad_2_a")     -- третий геймпад

-- Проверить действие (любая из привязанных кнопок)
if IsActionPressed("Jump") then Jump() end
if IsActionJustPressed("Shoot") then Fire() end
if IsActionJustReleased("Shoot") then StopFire() end

-- Удалить привязку
UnbindAction("Jump")
ClearAllActions()

-- Удобные универсальные проверки
if IsConfirmPressed() then ... end   -- Space/Enter/Gamepad A/Touch
if IsCancelPressed() then ... end    -- Escape/Gamepad B
```

**Формат биндинга действий:**

| Формат | Пример | Описание |
|---|---|---|
| `"клавиша"` | `"space"`, `"w"`, `"f1"` | Клавиша клавиатуры |
| `"mouse_N"` | `"mouse_1"`, `"mouse_3"` | Кнопка мыши (1-5) |
| `"gamepad_кнопка"` | `"gamepad_a"`, `"gamepad_lb"` | Геймпад 0, кнопка |
| `"gamepad_N_кнопка"` | `"gamepad_1_a"`, `"gamepad_2_x"` | Геймпад N (0-3), кнопка |
| `"touch"` / `"touch_N"` | `"touch_0"`, `"touch_1"` | Палец N |

### Осевые действия (аналоговый 1D)

Привязка аналоговых входов к одному числу float (газ, руль, зум).

```lua
-- Оси геймпада
BindAxisAction("Throttle", "gamepad_triggerright")                 -- правый триггер
BindAxisAction("Steer", "gamepad_leftx", { deadzone = 0.15 })     -- левый стик X
BindAxisAction("Look", "gamepad_1_rightx")                        -- геймпад 1, правый X
BindAxisAction("Zoom", "scroll_y")                                -- колесо мыши

-- Пара клавиш (отрицательная + положительная → -1 / 0 / +1)
BindAxisActionKeys("MoveX", "a", "d")
BindAxisActionKeys("MoveX", "left", "right", { scale = 0.5 })

-- Дельта мыши
BindAxisAction("LookX", "mouse_deltax")
BindAxisAction("LookY", "mouse_deltay", { scale = -1.0 })

-- Чтение значения
local val = GetActionValue("Throttle")   -- float (с дедзоной + масштабом)

-- Удалить
UnbindAxisAction("Throttle")
```

**Форматы источников осей:**

| Формат | Пример | Описание |
|---|---|---|
| `"gamepad_ОСЬ"` | `"gamepad_leftx"`, `"gamepad_triggerleft"` | Ось геймпада 0 |
| `"gamepad_N_ОСЬ"` | `"gamepad_1_righty"` | Ось геймпада N |
| `"mouse_deltax"` | — | Дельта мыши X |
| `"mouse_deltay"` | — | Дельта мыши Y |
| `"scroll_x"` | — | Колесо прокрутки X |
| `"scroll_y"` | — | Колесо прокрутки Y |

**Имена осей:** `leftx`, `lefty`, `rightx`, `righty`, `triggerleft` (`lt`, `l2`), `triggerright` (`rt`, `r2`)

**Таблица опций (необязательно):** `{ scale = 1.0, deadzone = 0.0 }`

### 2D осевые действия (аналоговый 2D)

Привязка 2D входов к таблице `{x, y}` (перемещение, камера).

```lua
-- Стик геймпада
BindAxis2DAction("Move", "gamepad_leftstick", { deadzone = 0.15 })
BindAxis2DAction("Look", "gamepad_rightstick")
BindAxis2DAction("P2Move", "gamepad_1_leftstick")   -- второй геймпад

-- Дельта мыши (для FPS камеры)
BindAxis2DAction("MouseLook", "mouse_delta", { scale = 0.1 })

-- Четвёрка клавиш (лево, право, вниз, вверх)
BindAxis2DActionKeys("Move", "a", "d", "s", "w")

-- Чтение значения
local move = GetActionAxis2D("Move")   -- → { x = ..., y = ... }
entity.x = entity.x + move.x * speed * dt
entity.y = entity.y + move.y * speed * dt

-- Удалить
UnbindAxis2DAction("Move")
```

**Форматы 2D источников:**

| Формат | Пример | Описание |
|---|---|---|
| `"gamepad_leftstick"` | — | Левый стик геймпада 0 |
| `"gamepad_rightstick"` | — | Правый стик геймпада 0 |
| `"gamepad_N_leftstick"` | `"gamepad_1_leftstick"` | Левый стик геймпада N |
| `"mouse_delta"` | — | Дельта движения мыши |

### Контексты ввода

Группировка действий в контексты с возможностью включения/отключения (геймплей vs меню).

```lua
-- Создать контексты
CreateInputContext("Gameplay")        -- включён по умолчанию
CreateInputContext("Menu", false)     -- создан выключенным

-- Назначить действия контекстам
SetActionContext("Jump", "Gameplay")
SetActionContext("Shoot", "Gameplay")
SetActionContext("MenuSelect", "Menu")

-- Переключить контексты
DisableInputContext("Gameplay")
EnableInputContext("Menu")

-- Проверка
if IsInputContextEnabled("Gameplay") then ... end

-- Удалить контекст (также отвязывает все его действия)
RemoveInputContext("Menu")
```

> Действия без контекста всегда активны.

### Триггеры (Hold / Tap / DoubleTap / Pulse)

Добавляют условия срабатывания к существующим булевым действиям.

```lua
-- Hold: срабатывает после удержания 0.5с
BindAction("Interact", "e")
SetActionTrigger("Interact", "Hold", 0.5)

-- Tap: срабатывает при быстром нажатии-отпускании (< 0.3с)
BindAction("Dash", "shift")
SetActionTrigger("Dash", "Tap", 0.3)

-- DoubleTap: два быстрых нажатия за 0.4с
BindAction("Roll", "space")
SetActionTrigger("Roll", "DoubleTap", 0.4)

-- Pulse: срабатывает сразу при нажатии, затем повторяется каждые 0.2с пока удерживается
BindAction("Fire", "mouse_left")
SetActionTrigger("Fire", "Pulse", 0.2)

-- ВАЖНО: вызывать каждый кадр с delta time!
UpdateActionTriggers(dt)

-- Проверить результат триггера
if IsActionTriggered("Interact") then OpenDoor() end
if IsActionTriggered("Dash") then DoDash() end
if IsActionTriggered("Fire") then ShootBullet() end

-- Прогресс удержания (0.0 → 1.0)
local progress = GetActionHoldProgress("Interact")
DrawProgressBar(progress)

-- Убрать триггер (действие продолжает работать нормально)
ClearActionTrigger("Interact")
```

### Перепривязка (для меню настроек)

Обнаружение любого ввода для UI перепривязки клавиш.

```lua
-- Вызывать каждый кадр в режиме перепривязки
local binding = DetectInputBinding()
if binding then
    -- binding = "w", "space", "mouse_1", "gamepad_a", "touch_0", и т.д.
    UnbindAction("Jump")
    BindAction("Jump", binding)
end
```

### Запрос и Экспорт/Импорт (сохранение)

Система привязок охватывает **все** типы action — цифровые (`BindAction`),
1D-оси (`BindAxisAction` / `BindAxisActionKeys`) и 2D-оси (`BindAxis2DAction` /
`BindAxis2DActionKeys`). Save/Load сохраняет все три категории вместе с
параметрами `scale` и `deadzone`.

```lua
-- Получить все привязки действия (только цифровые actions)
local bindings = GetActionBindings("Jump")    -- → {"space", "gamepad_a"}

-- Получить все имена зарегистрированных действий (цифровые + 1D + 2D)
local names = GetAllActionNames()             -- → {"Jump", "Shoot", "Move", ...}

-- Найти, какому action уже назначена кнопка (для проверки конфликтов в UI)
local owner = GetActionByBinding("space")     -- → "Jump" или nil
if owner and owner ~= "NewAction" then
    UnbindAction(owner)                       -- разрешить конфликт
end

-- Встроенное сохранение в JSON (рекомендуемый способ)
SaveInputBindings()                           -- → InputBindings.json в пользовательском конфиге
SaveInputBindings("profiles/wasd.json")       -- свой путь / несколько профилей
LoadInputBindings()                           -- возвращает true при успехе, иначе false
LoadInputBindings("profiles/wasd.json")
local path = GetInputBindingsPath()           -- абсолютный путь к файлу по умолчанию

-- Ручной экспорт/импорт (если хочешь встроить привязки в свой save-файл)
local data = ExportInputBindings()
-- data = {
--   actions = { Jump = { "space", "gamepad_a" }, ... },
--   axes    = { Throttle = { { type="source", source="gamepad_triggerright",
--                              scale=1.0, deadzone=0.0 } }, ... },
--   axes2d  = { Move = { { type="keys", left="a", right="d",
--                          down="s", up="w", scale=1.0, deadzone=0.0 } }, ... }
-- }
ImportInputBindings(data)                     -- заменяет текущие привязки
```

**Типичный сценарий меню настроек**

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

### Виртуальные элементы управления (для мобильных)

```lua
-- Виртуальный стик
CreateVirtualStick("move", 0.15, 0.7, 0.12)  -- name, centerX, centerY, radius
UpdateVirtualSticks()  -- Вызывать каждый кадр!

local axis = GetVirtualStickAxis("move")  -- → {x, y}
local knob = GetVirtualStickKnob("move")  -- → {x, y} позиция ручки
local active = IsVirtualStickActive("move")
SetVirtualStickCenter("move", 0.15, 0.7)  -- Переместить центр
RemoveVirtualStick("move")

-- Виртуальная кнопка
CreateVirtualButton("jump", 0.8, 0.7, 0.1, 0.1)  -- name, x, y, w, h
UpdateVirtualButtons()  -- Вызывать каждый кадр!

if IsVirtualButtonPressed("jump") then ... end
if IsVirtualButtonJustPressed("jump") then ... end
if IsVirtualButtonJustReleased("jump") then ... end
SetVirtualButtonRect("jump", 0.8, 0.7, 0.12, 0.12)
RemoveVirtualButton("jump")

-- Удалить все виртуальные элементы
ClearVirtualControls()
```

### Курсор геймпада (виртуальный курсор через стик)

```lua
-- Включить виртуальный курсор, управляемый стиком геймпада
EnableGamepadCursor()                              -- по умолчанию: speed=400, правый стик, геймпад 0
EnableGamepadCursor(600)                           -- своя скорость
EnableGamepadCursor(400, true, 0)                  -- speed, useRightStick, gamepadIndex

-- Выключить виртуальный курсор
DisableGamepadCursor()

-- Запрос состояния
local enabled = IsGamepadCursorEnabled()
local pos = GetGamepadCursorPosition()  -- → {x, y}

-- Установить позицию / скорость вручную
SetGamepadCursorPosition(960, 540)
SetGamepadCursorSpeed(800)

-- Обновлять каждый кадр (двигает курсор по стику)
UpdateGamepadCursor(dt)
```

### Универсальный указатель (мышь / тач / курсор геймпада)

Эти функции автоматически выбирают активный источник ввода:
курсор геймпада → тач → мышь (в таком приоритете).

```lua
-- Экранная позиция (в пикселях)
local ptr = GetPointerScreenPosition()
-- ptr.x, ptr.y   = экранные координаты
-- ptr.source      = "mouse" | "touch" | "gamepad"

-- Мировая позиция (использует основную камеру)
local wptr = GetPointerWorldPosition()
-- wptr.x, wptr.y  = мировые координаты
-- wptr.source      = "mouse" | "touch" | "gamepad"
```

---

## 7. Entity — Сущности

> **Тип:** Entity-bound

### Поиск сущностей

```lua
-- Найти сущность по тегу (первую)
local enemy = FindEntityByTag("Enemy")

-- Найти ВСЕ сущности с тегом → таблица ID
local enemies = FindEntitiesByTag("Enemy")
for _, id in ipairs(enemies) do
    -- обработка каждого врага
end

-- Получить тег сущности
local tag = GetEntityTag(entityId)

-- Ближайшая сущность с тегом
local closest = GetClosestEntityByTag("Coin")

-- Сущности в радиусе
local nearby = GetEntitiesInRadius(100, 200, 500)           -- x, y, radius
local coins  = GetEntitiesInRadius(100, 200, 500, "Coin")   -- с фильтром тега
local nearMe = GetEntitiesInMyRadius(300, "Enemy")           -- от текущей сущности

-- Сущности в прямоугольнике
local enemies = GetEntitiesInRect(0, 0, 500, 300, "Enemy")  -- x, y, w, h, filterTag?
for _, id in ipairs(enemies) do
    DestroyEntity(id)
end

-- Все сущности на сцене
local all = GetAllEntities()
local count = GetEntityCount()
local enemyCount = GetEntityCountByTag("Enemy")
```

### Итерация

```lua
-- Перебрать все сущности
ForEachEntity(function(id)
    local tag = GetEntityTag(id)
    -- return false для прерывания
end)

-- Перебрать сущности с тегом
ForEachEntityByTag("Enemy", function(id)
    DestroyEntity(id)
end)
```

### Создание и удаление

```lua
-- Создать сущность из класса
local id = SpawnEntity("Content/Classes/Bullet.ice_class", x, y)
local id = SpawnEntity("Content/Classes/Bullet.ice_class", x, y, z)  -- с Z

-- Создать с начальной скоростью
local id = SpawnEntityWithVelocity("Content/Classes/Bullet.ice_class", x, y, vx, vy)

-- Создать пустую сущность
local id = CreateEmptyEntity()  -- tag="Entity", x=y=z=0
local id = CreateEmptyEntity("Player", 100, 200, 0)

-- Клонировать существующую сущность (со всеми компонентами и скриптом)
local cloneId = CloneEntity(entityId, x, y)  -- С новой позицией
local cloneId = CloneEntity(entityId)         -- С оригинальной позицией

-- Удалить сущность
DestroyEntity(entityId)

-- Удалить себя
DestroySelf()

-- Проверить существование
if EntityExists(entityId) then ... end
```

### Свойства

```lua
-- Получить свой ID
local myId = GetEntityId()

-- Тег
local tag = GetTag()
SetTag("Player")
SetEntityTag(entityId, "Dead")
local hasTag = EntityHasTag(entityId, "Enemy")

-- Включить/выключить сущность
SetEnabled(false)
SetEntityEnabled(entityId, false)
local enabled = IsEnabled()
local enabled = IsEntityEnabled(entityId)

-- Видимость сущности (глобальный флаг — влияет на все компоненты)
SetVisible(false)
SetEntityVisible(entityId, false)
local vis = IsVisible()
local vis = IsEntityVisible(entityId)

-- Рендер в игре (глобальный флаг — если false, сущность полностью отключается в рантайме)
SetRenderInGame(false)
SetEntityRenderInGame(entityId, false)
local rig = GetRenderInGame()
local rig = GetEntityRenderInGame(entityId)
```

### Работа с другими сущностями

```lua
-- Позиция другой сущности
local pos = GetEntityPosition(entityId)  -- → {x, y, z}
SetEntityPosition(entityId, 100, 200)
SetEntityPosition(entityId, 100, 200, 0)  -- с Z

-- Скорость другой сущности
local vel = GetEntityVelocity(entityId)  -- → {x, y}
SetEntityVelocity(entityId, 200, 0)
AddEntityImpulse(entityId, 0, -500)

-- Флип спрайта другой сущности
SetEntityFlipX(entityId, true)
SetEntityFlipY(entityId, true)

-- Расстояние до другой сущности
local dist = DistanceToEntity(targetId)

-- Направление к сущности (нормализованный вектор)
local dir = GetDirectionToEntity(targetId)  -- → {x, y}

-- Направление к точке
local dir = GetDirectionTo(targetX, targetY)  -- → {x, y}

-- Быстрая проверка дальности (без вычисления точного расстояния)
local inRange = IsEntityInRange(targetId, 200)  -- В радиусе 200?

-- Сущности в конусе обзора (от текущей сущности)
local seen = GetEntitiesInCone(dirAngle, halfAngle, radius)
local seen = GetEntitiesInCone(dirAngle, halfAngle, radius, "Enemy")  -- С фильтром тега
```

### Entity Data (Пользовательские данные)

```lua
-- Привязать данные к себе
SetData("health", 100)
local hp = GetData("health")
local has = HasData("health")
ClearData()

-- Привязать данные к любой сущности
SetEntityData(entityId, "health", 100)
local hp = GetEntityData(entityId, "health")
local has = HasEntityData(entityId, "health")
ClearEntityData(entityId)
```

### Иерархия (Привязка к родителю)

Иерархия хранится как EnTT компонент (`HierarchyComponent`). Создаётся автоматически
при вызове `AttachToEntity` и уничтожается автоматически при уничтожении сущности
(дети отвязываются, родитель уведомляется). Ручная очистка не требуется.

```lua
-- Привязать к родительской сущности (следовать за ней)
-- При привязке запоминается полный локальный трансформ (позиция, поворот, масштаб)
AttachToEntity(parentId)

-- Отвязать
DetachFromParent()

-- Явные варианты (для любой сущности — удобно из level-скрипта или для управления другими):
AttachEntityToParent(childId, parentId)   -- привязать произвольного ребёнка к родителю
AttachChildEntity(childId)                -- привязать ребёнка к текущей (self) сущности
DetachEntityFromParent(childId)           -- отвязать произвольную сущность от родителя

-- Заспавнить инстанс класса сразу привязанным к родителю, в локальной от родителя позиции.
-- SpawnEntityAsChild(classPath, parentId [, localX, localY, z]) → id новой сущности, либо nil
local turretId = SpawnEntityAsChild("Content/Classes/Turret.ice_class", GetEntityId(), 0, 24)

-- Получить родителя
local parentId = GetParentEntity()  -- nil если нет

-- Получить дочерние сущности
local children = GetChildEntities()  -- таблица ID

-- Проверки
local hasParent = HasParent()
local childCount = GetChildCount()

-- Локальный оффсет (устанавливает LocalPosition.x, LocalPosition.y в HierarchyComponent)
SetLocalOffset(10, 5)
local offset = GetLocalOffset()  -- → {x, y}

-- Принудительно обновить позицию от родителя (с учётом поворота и масштаба)
FollowParent()

-- Обновить всю иерархию сущности и всех потомков рекурсивно
FollowParentHierarchy()

-- Очистить всю иерархию (удаляет HierarchyComponent у всех сущностей)
ClearHierarchy()

-- Проверить наличие компонента
local has = HasHierarchy(entityId)
local has = HasComponent("Hierarchy")
local has = EntityHasComponent(entityId, "Hierarchy")
```

### Локальный и мировой трансформ (Entity)

Когда сущность привязана к родителю через `AttachToEntity`, доступны
отдельные функции для **локального** (относительно родителя) и **мирового** (абсолютного) трансформа.
Если родителя нет — локальный и мировой трансформ совпадают.

```lua
-- Локальная позиция (относительно родителя, или абсолютная если нет родителя)
local lp = GetLocalPosition()      -- → {x, y, z}
SetLocalPosition(10, 20)           -- Установить (x, y)
SetLocalPosition(10, 20, 0.5)     -- Установить (x, y, z)

-- Локальный поворот
local lr = GetLocalRotation()      -- → float (градусы)
SetLocalRotation(45)

-- Локальный масштаб
local ls = GetLocalScale()         -- → {x, y}
SetLocalScale(2, 2)

-- Мировая позиция (абсолютная, из TransformComponent)
local wp = GetWorldPosition()      -- → {x, y, z}
SetWorldPosition(100, 200)         -- Установить (x, y), пересчитывает локальный трансформ
SetWorldPosition(100, 200, 1.0)   -- Установить (x, y, z)

-- Мировой поворот
local wr = GetWorldRotation()      -- → float (градусы)
SetWorldRotation(90)               -- Пересчитывает локальный поворот

-- Мировой масштаб
local ws = GetWorldScale()         -- → {x, y}
SetWorldScale(3, 3)                -- Пересчитывает локальный масштаб

-- Инкрементальный сдвиг (добавление к текущему значению)
AddWorldOffset(10, 5)              -- Сдвинуть мировую позицию на (dx, dy), пересчитывает локальный трансформ
AddWorldOffset(10, 5, 1.0)         -- Сдвинуть на (dx, dy, dz)
AddLocalOffset(3, -2)              -- Сдвинуть локальную позицию на (dx, dy)
AddLocalOffset(3, -2, 0.5)         -- Сдвинуть на (dx, dy, dz)

-- Инкрементальный поворот
AddWorldRotation(15)               -- Добавить к мировому повороту (градусы), пересчитывает локальный
AddLocalRotation(15)               -- Добавить к локальному повороту (градусы)

-- Инкрементальный масштаб
AddWorldScale(0.5, 0.5)            -- Добавить к мировому масштабу (dx, dy), пересчитывает локальный
AddLocalScale(0.5, 0.5)            -- Добавить к локальному масштабу (dx, dy)
```

**Пример: Привязка оружия к персонажу:**

```lua
function OnCreate()
    local weaponId = SpawnEntity("Content/Classes/Sword.ice_class", 0, 0)
    AttachToEntity(GetEntityId())  -- Привязать к текущей сущности
    SetLocalPosition(20, -10)      -- Оружие справа и чуть ниже
    SetLocalRotation(0)
end

function OnUpdate(dt)
    FollowParent()  -- Автоматически следовать с учётом поворота и масштаба
end
```

### Наследование классов

```lua
-- Имя класса сущности
local className = GetEntityClassName(entityId)  -- "Player"

-- Имя/путь класса текущей сущности
local myClass = GetClassName()
local myPath = GetClassPath()
local parentName = GetParentClassName()
local hasParent = HasParentClass()

-- Проверить, является ли сущность экземпляром класса (с учётом наследования)
if EntityIsA(entityId, "Character") then
    -- Сущность — Character или наследуется от Character
end

-- Проверки наследования текущей сущности
if IsA("Character") then ... end
if IsChildOf("Character") then ... end

-- Получить цепочку наследования
local chain = GetInheritanceChain()  -- {"Player", "Character", "Entity"}
```

### Интерфейсы (Interfaces)

Интерфейсы — механизм для объявления «контрактов» между сущностями.
Сущность объявляет, что реализует интерфейс, и другие сущности могут вызывать её функции.
Интерфейсы хранятся как EnTT компонент (`InterfaceComponent`). Компонент создаётся
автоматически при вызове `ImplementInterface` и очищается при уничтожении сущности.

```lua
-- Объявить, что сущность реализует интерфейс
ImplementInterface("Damageable")
ImplementInterfaces({"Damageable", "Interactable"})

-- Проверить свои интерфейсы
local has = HasInterface("Damageable")       -- true
local list = GetInterfaces()                  -- {"Damageable", "Interactable"}

-- Проверить интерфейс другой сущности
local has = EntityHasInterface(entityId, "Damageable")

-- Вызвать функцию через интерфейс
CallInterface(entityId, "TakeDamage", 50)
-- Вызывает TakeDamage(50) в скрипте сущности entityId

-- Вызвать функцию на ВСЕХ сущностях с тегом, у которых есть InterfaceComponent
CallInterfaceOnTag("Enemy", "OnAlert")

-- Найти все сущности с интерфейсом
local healables = FindEntitiesWithInterface("Healable")

-- Управление
RemoveInterface("Damageable")
ClearInterfaces()                -- Удалить все интерфейсы текущей сущности
ClearAllInterfaces()             -- Удалить все интерфейсы всех сущностей

-- Проверить наличие компонента
local has = HasInterfaceComponent(entityId)
local has = HasComponent("Interface")
local has = EntityHasComponent(entityId, "Interface")
```

### Gameplay Tags (игровые теги)

Gameplay Tags — система иерархических тегов, хранимая как EnTT-компонент (`GameplayTagComponent`).
Теги используют точечную иерархию (например `"Status.Buff.Speed"`) — запрос `"Status.Buff"` совпадёт с `"Status.Buff.Speed"`.

```lua
-- Добавить / удалить теги
AddGameplayTag("Status.Buff.Speed")
AddGameplayTags({"Status.Buff.Speed", "Team.Ally"})  -- пакетное добавление из таблицы
RemoveGameplayTag("Status.Buff.Speed")
ClearGameplayTags()

-- Точное совпадение (без иерархии)
local has = HasExactGameplayTag("Status.Buff.Speed")

-- Иерархическое совпадение ("Status.Buff" совпадает с "Status.Buff.Speed")
local has = HasGameplayTag("Status.Buff")

-- Проверить несколько тегов (из таблицы)
local any = HasAnyGameplayTag({"Status.Buff", "Status.Debuff"})
local all = HasAllGameplayTags({"Status.Buff.Speed", "Team.Ally"})

-- Получить все теги
local tags = GetGameplayTags()  -- таблица строк

-- Запросы по другим сущностям
local has = EntityHasGameplayTag(entityId, "Status.Buff")
local has = EntityHasExactGameplayTag(entityId, "Status.Buff.Speed")

-- Найти сущности по тегу
local entities = FindEntitiesWithGameplayTag("Status.Buff")
local entities = FindEntitiesWithExactGameplayTag("Status.Buff.Speed")

-- Проверить наличие компонента
local has = HasGameplayTagComponent(entityId)
local has = HasComponent("GameplayTag")
local has = EntityHasComponent(entityId, "GameplayTag")
```

### Глобальный модуль `GameplayTags`

Глобальная таблица `GameplayTags` — это система тегов поверх
`GameplayTagComponent`. Добавлены динамический реестр тегов, утилиты иерархии, помощники
для контейнеров, билдер запросов (`any`/`all`/`none`, exact или иерархический), поиск
по сцене и механизм событий/сообщений.

Все мутации (как per-entity функции `AddGameplayTag`/`RemoveGameplayTag`/`ClearGameplayTags`,
так и `GameplayTags.*`) вызывают зарегистрированные слушатели и рассылки сообщений.

```lua
-- ---------- Реестр известных тегов ----------
GameplayTags.Register("Status.Buff.Speed")             -- также регистрирует "Status" и "Status.Buff"
GameplayTags.RegisterMany({"Team.Ally", "Team.Enemy"}) -- возвращает кол-во новых
GameplayTags.IsRegistered("Status.Buff")
local all = GameplayTags.GetAllRegistered()
GameplayTags.Unregister("Team.Enemy")
GameplayTags.ClearRegistry()

-- ---------- Утилиты формата / иерархии ----------
GameplayTags.IsValid("Status.Buff.Speed")   -- true
GameplayTags.Split("A.B.C")                 -- {"A","B","C"}
GameplayTags.Join({"A","B","C"})            -- "A.B.C"
GameplayTags.GetParent("A.B.C")             -- "A.B"
GameplayTags.GetParents("A.B.C")            -- {"A.B","A"}
GameplayTags.GetDepth("A.B.C")              -- 3
GameplayTags.IsChildOf("A.B.C", "A.B")      -- true (строго: тег не является ребёнком сам себе)
GameplayTags.MatchesTag("A.B.C", "A.B")     -- true (иерархически)
GameplayTags.GetChildren("Status")          -- прямые зарегистрированные потомки
GameplayTags.GetDescendants("Status")       -- все зарегистрированные потомки

-- ---------- Помощники контейнеров (таблица строк) ----------
local c = {"Status.Buff.Speed", "Team.Ally"}
GameplayTags.ContainerHasTag(c, "Status")          -- true  (иерархически)
GameplayTags.ContainerHasExact(c, "Status")        -- false
GameplayTags.ContainerHasAny(c, {"Foo","Status"})  -- true
GameplayTags.ContainerHasAll(c, {"Team","Status"}) -- true
GameplayTags.ContainerHasAnyExact(c, {"Team.Ally"})-- true
GameplayTags.ContainerHasAllExact(c, {"Team.Ally","Status.Buff.Speed"}) -- true
GameplayTags.ContainerFilter(c, "Status")          -- {"Status.Buff.Speed"}
GameplayTags.ContainerMerge(c, {"Team.Ally","NewTag"}) -- объединение без дубликатов

-- ---------- Per-entity ----------
GameplayTags.AddTag(entityId, "Status.Buff.Speed")            -- вызывает OnTagAdded
GameplayTags.AddTags(entityId, {"Status.Buff.Speed","Team.Ally"})
GameplayTags.RemoveTag(entityId, "Status.Buff.Speed")         -- вызывает OnTagRemoved
GameplayTags.RemoveTags(entityId, {"Status.Buff.Speed"})
GameplayTags.Clear(entityId)

GameplayTags.HasTag(entityId, "Status")            -- иерархически
GameplayTags.HasExactTag(entityId, "Status.Buff")  -- точно
GameplayTags.HasAny(entityId, {"A","B"})
GameplayTags.HasAll(entityId, {"A","B"})
GameplayTags.HasAnyExact(entityId, {"A","B"})
GameplayTags.HasAllExact(entityId, {"A","B"})
local tags  = GameplayTags.GetTags(entityId)
local count = GameplayTags.CountTags(entityId)
local has   = GameplayTags.HasTagComponent(entityId)

-- ---------- Поиск по сцене ----------
local e1 = GameplayTags.FindByTag("Status.Buff")
local e2 = GameplayTags.FindByExactTag("Status.Buff.Speed")
local e3 = GameplayTags.FindAnyOf({"Team.Ally","Team.Enemy"})
local e4 = GameplayTags.FindAllOf({"Status.Buff","Team.Ally"})
local n  = GameplayTags.CountEntitiesWith("Status.Buff")
local all= GameplayTags.GetAllTagsInScene()

GameplayTags.AddTagToAll({id1, id2, id3}, "Team.Enemy")
GameplayTags.RemoveTagFromAll({id1, id2}, "Team.Enemy")

-- ---------- Запросы (GameplayTagQuery-lite) ----------
local q = GameplayTags.MakeQuery({
    all   = { "Status.Buff" },                -- каждый должен совпасть
    any   = { "Team.Ally", "Team.Neutral" },  -- хотя бы один
    none  = { "Status.Debuff" },              -- ни один не должен совпасть
    exact = false,                            -- false = иерархически (по умолчанию)
})

GameplayTags.EvaluateQuery(q, {"Status.Buff.Speed","Team.Ally"})
q:Matches({"Status.Buff.Speed","Team.Ally"})
GameplayTags.EntityMatchesQuery(entityId, q)
local matches = GameplayTags.FindEntitiesMatchingQuery(q)

-- ---------- События (добавление / удаление) ----------
-- Иерархические слушатели: подписка на "Status" также сработает для "Status.Buff.Speed".
local l1 = GameplayTags.OnTagAdded("Status", function(entityId, tag) end)
local l2 = GameplayTags.OnTagAddedExact("Status.Buff.Speed", function(eid, tag) end)
local l3 = GameplayTags.OnAnyTagAdded(function(eid, tag) end)
local l4 = GameplayTags.OnTagAddedForEntity(entityId, "Status", function(eid, tag) end)

local r1 = GameplayTags.OnTagRemoved("Status", function(eid, tag) end)
local r2 = GameplayTags.OnTagRemovedExact("Status", function(eid, tag) end)
local r3 = GameplayTags.OnAnyTagRemoved(function(eid, tag) end)
local r4 = GameplayTags.OnTagRemovedForEntity(entityId, "Status", function(eid, tag) end)

GameplayTags.RemoveListener(l1)
GameplayTags.RemoveAllListeners()          -- также очищаются автоматически при остановке рантайма

-- ---------- Рассылка сообщений (GameplayMessageSubsystem-lite) ----------
local m1 = GameplayTags.Listen("Combat", function(tag, payload) end)
local m2 = GameplayTags.ListenExact("Combat.Damage.Fire", function(tag, payload) end)
GameplayTags.Broadcast("Combat.Damage.Fire", { amount = 25, source = myId })
GameplayTags.Unlisten(m1)                   -- алиас RemoveListener
```

### Глобальный модуль `Interfaces`

Глобальная таблица `Interfaces` — система диспетчеризации интерфейсов
поверх `InterfaceComponent`. Добавлены централизованный реестр, массовые операции, безопасные
вызовы с возвратом значений (в том числе multi-return), broadcast-диспетчеризация и хуки
жизненного цикла.

Все мутации (как per-entity `ImplementInterface`/`RemoveInterface`/`ClearInterfaces`, так и
`Interfaces.*`) вызывают зарегистрированные слушатели.

```lua
-- ---------- Объявление интерфейсов ----------
Interfaces.Declare("Damageable", {"TakeDamage", "Heal"})
Interfaces.Declare("Interactable")                     -- список функций опционален
Interfaces.AddFunction("Damageable", "Die")
Interfaces.HasDeclaredFunction("Damageable", "Heal")
local list  = Interfaces.GetFunctions("Damageable")
local all   = Interfaces.GetDeclared()
local known = Interfaces.IsDeclared("Damageable")
Interfaces.Undeclare("Damageable")

-- ---------- Per-entity ----------
Interfaces.Implement(entityId, "Damageable")           -- вызывает OnImplemented
Interfaces.ImplementMany(entityId, {"Damageable","Interactable"})
Interfaces.Remove(entityId, "Damageable")              -- вызывает OnRemoved
Interfaces.Clear(entityId)
Interfaces.ClearAll()                                  -- все сущности на сцене

Interfaces.Implements(entityId, "Damageable")
Interfaces.ImplementsAny(entityId, {"A","B"})
Interfaces.ImplementsAll(entityId, {"A","B"})
local ifs   = Interfaces.Get(entityId)
local count = Interfaces.Count(entityId)
local has   = Interfaces.HasInterfaceComponent(entityId)

-- ---------- Поиск по сцене ----------
local impls = Interfaces.FindImplementers("Damageable")
local any   = Interfaces.FindImplementersOfAny({"Damageable","Healable"})
local all   = Interfaces.FindImplementersOfAll({"Damageable","Interactable"})
local n     = Interfaces.CountImplementers("Damageable")

-- ---------- Диспетчеризация ----------
-- Call возвращает (ok:bool, возвращаемые значения...) — multi-return.
local ok, result, extra = Interfaces.Call(entityId, "TakeDamage", 50, "fire")

-- TryCall: тихо возвращает nil при отсутствии функции (только первое значение).
local healed = Interfaces.TryCall(entityId, "Heal", 10)

-- CallIfImplements: вызов только если интерфейс реально объявлен у сущности.
local x = Interfaces.CallIfImplements(entityId, "Damageable", "TakeDamage", 50)

-- Broadcast: вызов на всех реализующих интерфейс (fire-and-forget). Возвращает кол-во успешных.
local n = Interfaces.Broadcast("Damageable", "TakeDamage", 10)

-- Broadcast и сбор значений: { { entity=id, value=result }, ... }
local rows = Interfaces.BroadcastWithResults("Damageable", "GetHealth")
for _, row in ipairs(rows) do print(row.entity, row.value) end

-- Диспетчеризация по TagComponent (display-name)
Interfaces.CallOnTag("Enemy", "OnAlert")

-- Диспетчеризация по GameplayTagComponent (иерархически!)
Interfaces.CallOnGameplayTag("Team.Enemy", "OnAlert")

-- На ВСЕ сущности, у которых в окружении есть эта функция (интерфейс не обязателен)
Interfaces.CallOnAll("OnGamePaused", true)

-- Рефлексия
Interfaces.HasFunction(entityId, "TakeDamage")   -- true если в env есть такая функция
local fns = Interfaces.ListFunctions(entityId)   -- все функции-значения в env

-- ---------- Переменные другой сущности (чтение/запись её скрипт-глобалов) ----------
-- Аналог Interfaces.Call для переменных: достучаться до переменной в env другой живой сущности
-- (например, до глобала `health`, объявленного в её OnConstruct).
Interfaces.SetVar(entityId, "health", 80)          -- записать глобал в env цели → bool
local hp   = Interfaces.GetVar(entityId, "health") -- прочитать (nil если отсутствует)
local known = Interfaces.HasVar(entityId, "health")-- true если переменная задана (не nil)

-- ---------- События жизненного цикла ----------
local l1 = Interfaces.OnImplemented("Damageable", function(entityId, name) end)
local l2 = Interfaces.OnAnyImplemented(function(eid, name) end)
local l3 = Interfaces.OnRemoved("Damageable", function(eid, name) end)
local l4 = Interfaces.OnAnyRemoved(function(eid, name) end)

Interfaces.RemoveListener(l1)
Interfaces.RemoveAllListeners()                  -- также очищаются автоматически при остановке рантайма
```

### Скрипты уровня (Level Scripts)

Level Script — глобальный скрипт, привязанный к сцене (уровню).
Сущности могут вызывать функции уровневого скрипта и обмениваться данными.

```lua
-- Вызвать функцию из Level Script
CallLevelFunction("OnBossDeath")
CallLevelFunction("SpawnWave", 3)  -- С аргументами
local result = CallLevelFunction("GetDifficulty")

-- Проверить наличие функции
if HasLevelFunction("OnBossDeath") then
    CallLevelFunction("OnBossDeath")
end

-- Общие данные уровня (доступны из любого скрипта)
SetLevelData("score", 1000)
local score = GetLevelData("score")
```

---

## 8. Sprite — Спрайты

> **Тип:** Entity-bound. Требует компонент **SpriteRendererComponent**.
> У сущности может быть несколько спрайтов (слоёв). `index` = 0 — первый спрайт.

### Базовые свойства

```lua
-- Количество спрайтов
local count = GetSpriteCount()

-- Отзеркалить
SetFlipX(true)     -- По горизонтали
SetFlipY(true)     -- По вертикали
local fx = GetFlipX()
local fy = GetFlipY()

-- Цвет (RGBA, каждый компонент 0..1)
SetColor(1, 0, 0, 1)         -- Красный
SetColor(1, 1, 1, 0.5)       -- Полупрозрачный
local r, g, b, a = GetColor()

-- Прозрачность
SetSpriteAlpha(0.5)           -- Полупрозрачный
local alpha = GetSpriteAlpha()

-- Видимость
SetSpriteVisible(false)
local visible = IsSpriteVisible()
```

### Текстура и регион

```lua
-- Сменить текстуру в рантайме
SetSpriteTexture("Content/Textures/hero_damaged.png")
local path = GetSpriteTexturePath()

-- Регион (для спрайт-листов / атласов)
SetSpriteRegion(0, 0, 32, 32)     -- x, y, w, h в текстуре
local reg = GetSpriteRegion()       -- → {x, y, w, h}

-- Размер текущей текстуры/региона
local size = GetSpriteSize()        -- → {width, height}
```

Регион, заданный через `SetSpriteRegion`, можно безопасно применять уже в `OnCreate`: он сохраняется при отложенной загрузке текстуры и не сбрасывается обратно к дефолтному ректу спрайт-ассета. `SetSpriteTexture` очищает кастомный регион и возвращает спрайт к региону из ассета.

### Локальная трансформация (сдвиг спрайта относительно сущности)

```lua
SetSpriteLocalPosition(10, 5)           -- Сдвиг
local lp = GetSpriteLocalPosition()     -- → {x, y}

SetSpriteLocalScale(2, 2)              -- Масштаб слоя
local ls = GetSpriteLocalScale()        -- → {x, y}

SetSpriteLocalRotation(45)             -- Локальный поворот
local lr = GetSpriteLocalRotation()

-- UV-смещение (для авто-скроллинга текстур, см. раздел GLSLua)
SetSpriteUVScroll(0.5, 0.0)            -- Сдвиг текстурных координат
local uv = GetSpriteUVScroll()          -- → {x, y}

-- UV-масштаб / тайлинг (сколько раз текстура повторяется на спрайте)
SetSpriteUVScale(2.0, 1.0)             -- Тайлинг 2× по горизонтали
local uvs = GetSpriteUVScale()          -- → {x, y}
```

### Мировая трансформация (спрайт в мировых координатах)

Локальные значения задаются относительно сущности: движок поворачивает и
масштабирует их трансформацией сущности перед рендером. Если у вас уже есть
**мировая** координата (сокет, попадание луча, позиция мыши), никогда не
присваивайте её как локальную — она будет повёрнута и отмасштабирована второй
раз. Используйте World-аксессоры, они делают обратное преобразование сами:

```lua
SetSpriteWorldPosition(120, 64, 1)      -- Поставить спрайт 1 в мировую точку
local wp = GetSpriteWorldPosition(1)    -- → {x, y, z}

SetSpriteWorldRotation(30, 1)           -- Мировой угол в градусах (плюс = по часовой)
local wr = GetSpriteWorldRotation(1)    -- → число

local ws = GetSpriteWorldScale(1)       -- → {x, y} (масштаб сущности × локальный)
```

Мировой масштаб только на чтение — меняйте масштаб сущности или локальный.
`SetSpriteWorldPosition` трогает только x/y; z (порядок отрисовки) остаётся
там, куда его поставил `SetSpriteOrder`.

### Ассет спрайта

```lua
-- Подменить весь .ice_sprite: текстура, пивот, регион, точки привязки,
-- полигон коллизии, шейдинг/блендинг и материал перезагружаются.
local ok = SetSpriteAsset("Content/Weapons/SP_Pistol.ice_sprite", 1)
local path = GetSpritePath(1)           -- → текущий путь .ice_sprite

-- SetSpriteTexture меняет только сырое изображение. Он очищает точки привязки
-- (они принадлежат старому спрайту) и сохраняет пивот и полигон коллизии.
SetSpriteTexture("Content/Weapons/T_Pistol.png", 1)
```

### Порядок отрисовки и прочее

```lua
-- Z-порядок (рендер)
SetSpriteOrder(5)
local order = GetSpriteOrder()

-- Pivot (точка привязки, 0..1)
SetSpritePivot(0.5, 0)         -- Центр-верх
local piv = GetSpritePivot()    -- → {x, y}

-- Имя / путь
local name = GetSpriteName()
local path = GetSpritePath()

-- Отображение в игре
SetSpriteRenderInGame(true)
local render = GetSpriteRenderInGame()

-- Отбрасывание теней (без коллайдера)
SetSpriteCastShadow(true)
local shadow = GetSpriteCastShadow()

-- Не блокировать тени (по умолчанию true): пока глобально включено «Коллайдеры блокируют тени», этот спрайт всё равно пропускает тени сквозь себя. Отключите, чтобы он блокировал тени как ландшафт.
SetSpriteDontBlockShadows(true)
local dontBlock = GetSpriteDontBlockShadows()  -- → bool

-- Режим тени: 0 = Коллайдеры (формы коллизии ассета), 1 = Контур (силуэт по альфе текстуры)
SetSpriteCastShadowMode(1)
local mode = GetSpriteCastShadowMode()       -- → int

-- Начало тени: 0 = Center, 1 = Top, 2 = Bottom
SetSpriteShadowOrigin(1)
local origin = GetSpriteShadowOrigin()       -- → int

-- Смягчение краёв тени [0..1]
SetSpriteShadowEdgeFade(0.25)
local fade = GetSpriteShadowEdgeFade()       -- → float

-- Z-порядок тени: отрицательное = на задний план, положительное = на передний план, 0 = плоскость кастера (по умолчанию)
SetSpriteShadowZOrder(1)
local zo = GetSpriteShadowZOrder()           -- → float

-- Опциональный второй аргумент — индекс спрайта (по умолчанию 0).
SetSpriteShadowZOrder(1, 0)
```

### Точки привязки (Attach Points)

```lua
-- Локальная точка
local ap = GetSpriteAttachPoint("Weapon")
-- → {found=true/false, x, y, rotation}

-- Мировая точка (с учётом трансформации сущности)
local wp = GetSpriteAttachPointWorld("Weapon")
-- → {found=true/false, x, y, rotation}
-- rotation в градусах (плюс = по часовой) и совпадает со стрелкой сокета,
-- заданной в Sprite Editor; FlipX/FlipY зеркалят и позицию, и угол

-- Список всех attach point имён
local names = GetSpriteAttachPointNames()   -- → {"Weapon", "Shield", ...}
local count = GetSpriteAttachPointCount()   -- Количество точек
```

### Привязка к сокету (движковая)

Вместо того чтобы каждый кадр самому переставлять приаттаченный спрайт,
скажите движку, за каким сокетом он должен следовать. Движок пересчитывает
позицию и поворот после анимаций и до `OnLateUpdate`, корректно учитывая
поворот сущности, её масштаб и FlipX.

```lua
-- Спрайт 1 следует за сокетом "ArmSocket", найденным где угодно на этой сущности
AttachSpriteToSocket("ArmSocket", 1)

-- Явный источник. source = "auto" | "sprite" | "flipbook" | "skeleton"
-- sourceIndex = индекс экземпляра компонента, -1 = искать во всех экземплярах
-- inheritFlipX = копировать FlipX у владельца сокета (по умолчанию true)
AttachSpriteToSocketFrom("hand_point", "skeleton", -1, true, 1)

DetachSpriteFromSocket(1)               -- Вернуть обычную локальную трансформацию

local a = GetSpriteSocketAttach(1)
-- → {socket="ArmSocket", source="auto", sourceIndex=-1, inheritFlipX=true,
--    offsetX=0, offsetY=0, offsetRotation=0, attached=true/false}

-- attached = false, если на текущем кадре анимации такого сокета нет —
-- обычно именно этим и удобно управлять видимостью:
SetSpriteVisible(IsSpriteSocketAttached(1), 1)

-- Смещение поверх сокета: пространство сокета, пиксели, масштабируется вместе
-- с сущностью и зеркалится по Flip X. Точная подгонка или анимация отдачи:
SetSpriteSocketOffset(-3, 0, -8, 1)     -- на 3 px назад, ствол на 8° вверх
local off = GetSpriteSocketOffset(1)    -- → {x, y, rotation}
```

Локальной трансформацией приаттаченного экземпляра владеет движок — пишите в
`AttachSocketOffset` (через `SetSpriteSocketOffset`), а не в
`SetSpriteLocalPosition`, иначе значение перезапишется на следующем проходе
привязки. Проход выполняется дважды за кадр — до и после `OnLateUpdate`, —
поэтому поворот прицеливания, заданный в `OnLateUpdate`, попадает в тот же
кадр. Локальный масштаб и порядок отрисовки (`SetSpriteOrder`) остаются вашими.

При `source = "auto"` источники ищутся в порядке: скелет → экземпляры спрайтов
→ экземпляры флипбуков. Сам приаттаченный экземпляр пропускается, поэтому
спрайт никогда не привяжется к собственному сокету. Те же поля есть у
экземпляров Sprite и Flipbook в Class Editor и в панели Properties
(*Прикрепить к сокету*) — можно настроить вообще без скрипта.

*Прикрепить к коллайдеру* имеет приоритет над *Прикрепить к сокету*: экземпляр,
уже привязанный к коллайдеру с отдельным телом, продолжает следовать за этим
телом, а привязка к сокету игнорируется (`IsSpriteSocketAttached` вернёт
`false`). Сначала снимите привязку к коллайдеру, если хотите управление сокетом.

Так как привязка выводится из реплицируемого состояния анимации, она
разрешается одинаково на всех пирах в мультиплеере — дополнительная репликация
не нужна.

### Полигон коллизии

```lua
-- Есть ли у спрайта полигон коллизии (>= 3 точки)
local has = HasSpriteCollisionPolygon()          -- true/false
local has = HasSpriteCollisionPolygon(1)          -- Второй спрайт

-- Количество точек полигона коллизии
local count = GetSpriteCollisionPointCount()      -- 0 если нет полигона

-- Получить все точки полигона коллизии (нормализованные 0-1)
local poly = GetSpriteCollisionPolygon()
-- → { {x=0.1, y=0.2}, {x=0.9, y=0.2}, {x=0.9, y=0.9}, ... }
for i, pt in ipairs(poly) do
    print(pt.x, pt.y)
end
```

Сгенерированные Box2D-формы следуют **полной локальной трансформации**
экземпляра — позиции, повороту и масштабу, — а также трансформации сущности,
поэтому физическая форма всегда оказывается ровно там, где нарисован спрайт, и
совпадает с дебаг-просмотром коллайдеров. Отрицательный масштаб зеркалит полигон
относительно пивота так же, как зеркалит графику. Это касается и приаттаченных к
сокету экземпляров: пистолет, приклеенный к вращающейся руке, получает корректно
размещённый и повёрнутый коллайдер. Пока меняется только
трансформация, формы обновляются на месте — контакты и состояние сенсоров
сохраняются; полное пересоздание происходит лишь при изменении самого полигона,
флипов, пивота или свойства формы (например, при смене кадра флипбука).

### Свойства форм коллизии

```lua
-- Количество рантайм-форм коллизии
local count = GetSpriteCollisionShapeCount()

-- Плотность
SetSpriteCollisionDensity(1.0)
local density = GetSpriteCollisionDensity()

-- Трение
SetSpriteCollisionFriction(0.5)
local friction = GetSpriteCollisionFriction()

-- Упругость (отскок)
SetSpriteCollisionRestitution(0.3)
local rest = GetSpriteCollisionRestitution()

-- Сенсор (вызывает события перекрытия вместо физического столкновения)
SetSpriteCollisionSensor(true)
local sensor = IsSpriteCollisionSensor()

-- Односторонняя платформа (объекты проходят снизу, сталкиваются сверху)
SetSpriteCollisionOneWay(true)
local oneWay = IsSpriteCollisionOneWay()

-- Направление одностороннего прохода: 1 = Вверх (по умолчанию), 2 = Вниз, 3 = Влево, 4 = Вправо
-- Направление — это сторона, с которой платформа ТВЁРДАЯ (тела не проходят сквозь)
SetSpriteCollisionOneWayDirection(1)
local dir = GetSpriteCollisionOneWayDirection()

-- Контактные события (колбэки OnCollisionEnter / OnCollisionExit)
SetSpriteContactEventsEnabled(true)
local contact = AreSpriteContactEventsEnabled()

-- Сенсорные события (колбэки OnSensorEnter / OnSensorExit)
SetSpriteSensorEventsEnabled(true)
local sensorEvt = AreSpriteSensorEventsEnabled()

-- События ударов (колбэк OnCollisionHit с силой удара)
SetSpriteHitEventsEnabled(true)
local hit = AreSpriteHitEventsEnabled()

-- События пре-солва (вызываются до расчёта ответа на столкновение)
SetSpritePreSolveEventsEnabled(true)
local preSolve = AreSpritePreSolveEventsEnabled()


-- Все функции принимают необязательный индекс спрайта:
SetSpriteCollisionDensity(2.0, 1)  -- Второй спрайт
SetSpriteCollisionOneWay(true, 1)  -- Второй спрайт
```

### Работа с несколькими спрайтами

```lua
-- Все функции принимают необязательный index:
SetFlipX(true, 1)              -- Второй спрайт
SetColor(1, 0, 0, 1, 2)       -- Третий спрайт
SetSpriteVisible(false, 1)
```

---

## 9. Flipbook — Покадровая анимация

> **Тип:** Entity-bound. Требует компонент **FlipbookComponent**.
> Flipbook — покадровая анимация (spritesheet). Не путать с Animator!

```lua
-- Управление воспроизведением
SetFlipbookPlaying(true)
SetFlipbookPlaying(false)
local playing = IsFlipbookPlaying()

-- Скорость
SetFlipbookSpeed(2.0)           -- Двойная скорость
local speed = GetFlipbookSpeed()

-- Кадры
SetFlipbookFrame(5)             -- Перейти на кадр 5
local frame = GetFlipbookFrame()
local total = GetFlipbookFrameCount()
ResetFlipbook()                 -- На кадр 0

-- Таймер
local timer = GetFlipbookTimer()

-- Визуальные свойства
SetFlipbookColor(1, 0, 0, 1)
local r, g, b, a = GetFlipbookColor()
SetFlipbookFlipX(true)
local flipX = GetFlipbookFlipX()     -- опционально: GetFlipbookFlipX(index)
SetFlipbookFlipY(false)
local flipY = GetFlipbookFlipY()     -- опционально: GetFlipbookFlipY(index)
SetFlipbookAlpha(0.5)
local alpha = GetFlipbookAlpha()
SetFlipbookVisible(true)
local vis = IsFlipbookVisible()
SetFlipbookRenderInGame(true)
local render = GetFlipbookRenderInGame()

-- Отбрасывание теней (без коллайдера)
SetFlipbookCastShadow(true)
local shadow = GetFlipbookCastShadow()

-- Не блокировать тени (по умолчанию true): пока глобально включено «Коллайдеры блокируют тени», этот флипбук всё равно пропускает тени сквозь себя. Отключите, чтобы он блокировал тени как ландшафт.
SetFlipbookDontBlockShadows(true)
local dontBlock = GetFlipbookDontBlockShadows()  -- → bool

-- Режим тени: 0 = Коллайдеры (формы коллизии ассета), 1 = Контур (силуэт по альфе текстуры)
SetFlipbookCastShadowMode(1)
local mode = GetFlipbookCastShadowMode()     -- → int

-- Начало тени: 0 = Center, 1 = Top, 2 = Bottom
SetFlipbookShadowOrigin(1)
local origin = GetFlipbookShadowOrigin()     -- → int

-- Смягчение краёв тени [0..1]
SetFlipbookShadowEdgeFade(0.25)
local fade = GetFlipbookShadowEdgeFade()     -- → float

-- Z-порядок тени: отрицательное = на задний план, положительное = на передний план, 0 = плоскость кастера (по умолчанию)
SetFlipbookShadowZOrder(1)
local zo = GetFlipbookShadowZOrder()         -- → float

-- Опциональный второй аргумент — индекс flipbook'а (по умолчанию 0).
SetFlipbookShadowZOrder(1, 0)

-- Количество и имена
local count = GetFlipbookCount()
local name = GetFlipbookName(0)
local path = GetFlipbookPath(0)

-- Сменить флипбук в рантайме
SetFlipbookPath("Content/Flipbooks/Run.ice_flipbook")

-- Порядок отрисовки (Z-глубина)
SetFlipbookOrder(5.0)                -- без индекса — устанавливает Z сущности
SetFlipbookOrder(5.0, 1)             -- с индексом — устанавливает Z экземпляра флипбука
local order = GetFlipbookOrder()     -- опционально: GetFlipbookOrder(index)

-- Локальная трансформация
SetFlipbookLocalPosition(5, 0)
local lp = GetFlipbookLocalPosition()
SetFlipbookLocalScale(1, 1)
local ls = GetFlipbookLocalScale()
SetFlipbookLocalRotation(0)
local lr = GetFlipbookLocalRotation()

-- UV-смещение (для авто-скроллинга текстур флипбука)
SetFlipbookUVScroll(0.5, 0.0)
local uv = GetFlipbookUVScroll()          -- → {x, y}

-- UV-масштаб / тайлинг (сколько раз текстура повторяется на флипбуке)
SetFlipbookUVScale(2.0, 1.0)
local uvs = GetFlipbookUVScale()          -- → {x, y}

-- Мировая трансформация (см. раздел Sprite — те же правила и то же преобразование)
SetFlipbookWorldPosition(120, 64, 0)
local fwp = GetFlipbookWorldPosition(0)   -- → {x, y, z}
SetFlipbookWorldRotation(30, 0)
local fwr = GetFlipbookWorldRotation(0)   -- → число
local fws = GetFlipbookWorldScale(0)      -- → {x, y}, только чтение

-- Точки привязки. У флипбука нет своих сокетов — он отдаёт сокеты спрайта
-- ТЕКУЩЕГО кадра, поэтому они анимируются покадрово.
local ap = GetFlipbookAttachPoint("Hand")
local wp = GetFlipbookAttachPointWorld("Hand")
-- → {found, x, y, rotation}
-- rotation в градусах (плюс = по часовой) и совпадает со стрелкой сокета,
-- заданной в Sprite Editor; FlipX/FlipY зеркалят и позицию, и угол
local names = GetFlipbookAttachPointNames()   -- → {"Hand", "Foot", ...}
local apCount = GetFlipbookAttachPointCount()

-- Привязка к сокету (семантика идентична спрайтовой)
AttachFlipbookToSocket("ArmSocket", 1)
AttachFlipbookToSocketFrom("hand_point", "skeleton", -1, true, 1)
DetachFlipbookFromSocket(1)
SetFlipbookSocketOffset(-3, 0, -8, 1)
local fOff = GetFlipbookSocketOffset(1)   -- → {x, y, rotation}
local fa = GetFlipbookSocketAttach(1)
local isAttached = IsFlipbookSocketAttached(1)
```

### Полигон коллизии

```lua
-- Есть ли у текущего кадра флипбука полигон коллизии
local has = HasFlipbookCollisionPolygon()          -- true/false
local has = HasFlipbookCollisionPolygon(1)          -- Второй флипбук

-- Количество точек полигона коллизии
local count = GetFlipbookCollisionPointCount()

-- Получить все точки полигона коллизии (нормализованные 0-1)
local poly = GetFlipbookCollisionPolygon()
-- → { {x=0.1, y=0.2}, {x=0.9, y=0.2}, ... }
-- Примечание: полигон меняется от кадра к кадру, т.к. каждый кадр — отдельный спрайт
```

### Свойства форм коллизии

```lua
-- Количество рантайм-форм коллизии
local count = GetFlipbookCollisionShapeCount()

-- Плотность
SetFlipbookCollisionDensity(1.0)
local density = GetFlipbookCollisionDensity()

-- Трение
SetFlipbookCollisionFriction(0.5)
local friction = GetFlipbookCollisionFriction()

-- Упругость (отскок)
SetFlipbookCollisionRestitution(0.3)
local rest = GetFlipbookCollisionRestitution()

-- Сенсор (вызывает события перекрытия вместо физического столкновения)
SetFlipbookCollisionSensor(true)
local sensor = IsFlipbookCollisionSensor()

-- Односторонняя платформа (объекты проходят снизу, сталкиваются сверху)
SetFlipbookCollisionOneWay(true)
local oneWay = IsFlipbookCollisionOneWay()

-- Направление одностороннего прохода: 1 = Вверх (по умолчанию), 2 = Вниз, 3 = Влево, 4 = Вправо
-- Направление — это сторона, с которой платформа ТВЁРДАЯ (тела не проходят сквозь)
SetFlipbookCollisionOneWayDirection(1)
local dir = GetFlipbookCollisionOneWayDirection()

-- Контактные события (колбэки OnCollisionEnter / OnCollisionExit)
SetFlipbookContactEventsEnabled(true)
local contact = AreFlipbookContactEventsEnabled()

-- Сенсорные события (колбэки OnSensorEnter / OnSensorExit)
SetFlipbookSensorEventsEnabled(true)
local sensorEvt = AreFlipbookSensorEventsEnabled()

-- События ударов (колбэк OnCollisionHit с силой удара)
SetFlipbookHitEventsEnabled(true)
local hit = AreFlipbookHitEventsEnabled()

-- События пре-солва (вызываются до расчёта ответа на столкновение)
SetFlipbookPreSolveEventsEnabled(true)
local preSolve = AreFlipbookPreSolveEventsEnabled()


-- Все функции принимают необязательный индекс флипбука:
SetFlipbookCollisionDensity(2.0, 1)  -- Второй флипбук
SetFlipbookCollisionOneWay(true, 1)  -- Второй флипбук
```

### Состояние анимации

```lua
-- Зацикливание
SetFlipbookLoop(false)            -- Отключить зацикливание
local loop = GetFlipbookLoop()    -- true/false

-- Прогресс (0.0 — 1.0)
local progress = GetFlipbookProgress()

-- Завершена ли анимация? (для не-зацикленных)
local finished = IsFlipbookFinished()
local justFinished = DidFlipbookJustFinish()  -- true только в кадр завершения

-- Отслеживание кадров
local changed = HasFrameChanged()             -- Кадр сменился в этом фрейме?
local prevFrame = GetPreviousFrame()          -- Предыдущий кадр

-- Проверить, достигнут ли конкретный кадр (в этом фрейме)
if WasFrameReached(3) then
    PlaySound("footstep")
end

-- Проверить, активен ли диапазон кадров
local inRange = WasFrameRangeActive(2, 5)     -- Кадр между 2 и 5?
local rangeStarted = DidFrameRangeStart(2, 5) -- Только что вошли в диапазон?
```

### Пример: Атака с событиями по кадрам

```lua
function OnUpdate(dt)
    if IsKeyJustPressed("space") then
        SetFlipbookPath("Content/Flipbooks/Attack.ice_flipbook")
        SetFlipbookLoop(false)
        SetFlipbookPlaying(true)
    end

    -- Кадр 3 — нанести урон
    if WasFrameReached(3) then
        DealDamage()
    end

    -- Анимация закончилась — вернуться к Idle
    if DidFlipbookJustFinish() then
        SetFlipbookPath("Content/Flipbooks/Idle.ice_flipbook")
        SetFlipbookLoop(true)
        SetFlipbookPlaying(true)
    end
end
```

---

## 10. Animation — Аниматор (State Machine)

> **Тип:** Entity-bound. Требует компонент **AnimatorComponent**.
> Аниматор — система состояний (State Machine) с переходами, управляемая параметрами.

### Параметры

```lua
-- Установить параметры (используются в переходах анимации)
SetAnimBool("isRunning", true)
SetAnimInt("direction", 2)
SetAnimFloat("speed", 150.5)
SetAnimTrigger("attack")        -- Триггер — одноразовый флаг

-- Получить
local running = GetAnimBool("isRunning")
local dir = GetAnimInt("direction")
local speed = GetAnimFloat("speed")
```

### Журнал триггеров (репликация)

Триггер — одноразовое событие: аниматор гасит его в том же кадре, в котором сработал переход,
поэтому опросом (как у `GetAnimBool`) поймать его нельзя. Аниматор ведёт журнал триггеров,
которые были подняты, но ещё не прочитаны скриптом.

```lua
local epoch = GetAnimTriggerEpoch()   -- Монотонный счётчик, +1 на каждое новое имя в журнале

local log = ConsumeAnimTriggers()     -- Забирает и очищает журнал
-- log.epoch          -- Значение счётчика на момент чтения (число)
-- log.names          -- Массив имён триггеров, поднятых с прошлого чтения
for i = 1, #log.names do
    print(log.names[i])
end
```

`ConsumeAnimTriggers` очищает журнал, но никогда не сбрасывает `epoch` — счётчик только растёт,
пока жив компонент. Именно это делает триггеры реплицируемыми по сети: отправляйте `epoch`
и последнюю пачку имён в **каждом** пакете состояния, а получатель проигрывает пачку ровно один
раз — когда пришедший `epoch` больше применённого ранее. Так как данные повторяются в каждом
пакете, потеря пакета или промах окна выборки больше не съедает анимацию.

```lua
local log = ConsumeAnimTriggers()
if #log.names > 0 then
    lastEpoch, lastNames = log.epoch, log.names
end

-- аргумент triggers — это карта { name = true }, а не массив
local trigSet = {}
for _, name in ipairs(lastNames or {}) do trigSet[name] = true end

Network.SyncEntityAnimatorParams(key, bools, { trigEpoch = lastEpoch }, nil, trigSet, false)
```

### Состояние

```lua
-- Текущее состояние
local state = GetAnimState()        -- Имя состояния (строка)
local stateTime = GetAnimStateTime() -- Время в текущем состоянии

-- Текущий кадр
local frame = GetAnimFrame()

-- Принудительный переход в состояние
ForceAnimState("Jump")
ForceAnimState("Jump", 0.2)         -- С длительностью перехода

-- Переход
local transitioning = IsAnimTransitioning()
local progress = GetAnimTransitionProgress()  -- 0..1

-- Сбросить все триггеры
ResetAnimTriggers()

-- Есть ли аниматор?
local has = HasAnimator()
local other = HasAnimator(entityId)   -- entityId опционален; без него — текущая сущность

-- Загружен ли сам ассет анимации (а не только наличие компонента)?
local loaded = IsAnimationLoaded()
```

### Целевой спрайт

```lua
-- Указать какой спрайт-инстанс будет целью аниматора (по имени)
-- Когда у сущности несколько спрайтов, это выбирает какой получит текстуры анимации
SetAnimTargetSprite("Body")

-- Получить имя текущего целевого спрайта (пустая строка = авто / первый спрайт)
local target = GetAnimTargetSprite()

-- Сбросить на стандартный (первый спрайт)
SetAnimTargetSprite("")
```

### Типичный пример

```lua
function OnUpdate(dt)
    local vx = GetVelocityX()
    local vy = GetVelocityY()

    SetAnimBool("isRunning", math.abs(vx) > 10)
    SetAnimBool("isFalling", IsFalling())
    SetAnimBool("isGrounded", IsGrounded())

    -- Направление взгляда
    if vx > 0 then SetFlipX(false) end
    if vx < 0 then SetFlipX(true) end

    -- Атака по нажатию
    if IsKeyJustPressed("space") then
        SetAnimTrigger("attack")
    end
end
```

### Ассет анимации

```lua
local path = GetAnimationPath()                        -- → текущий .ice_animation
local ok = SetAnimationAsset("Content/Anim/Boss.ice_animation")
-- Сбрасывает состояние, кадр, переход и блендинг, затем грузит новый граф.
-- После этого заново выставьте параметры (SetAnimBool/Int/Float).
```

---

## 10.5. Skeleton — Костная анимация и рэгдолл

> **Тип:** Entity-bound. Требует компонент **SkeletonComponent** (ассет `.ice_skeleton`).
> Скелет — это иерархия костей со спрайтовыми/сетчатыми аттачментами, скинами, кадрированными анимациями (включая деформацию меша и IK), событиями анимации и встроенным гибридным активным рэгдоллом. Мировые координаты следуют конвенции движка: **X+ вправо, Y+ вверх, поворот по часовой стрелке — положительный (в градусах)**.

### Проигрывание

```lua
-- Запустить анимацию по имени (loop по умолчанию true, blendDuration по умолчанию 0)
PlaySkeletonAnimation("run")
PlaySkeletonAnimation("attack", false)         -- однократно
PlaySkeletonAnimation("walk", true, 0.15)      -- кроссфейд за 0.15с

StopSkeletonAnimation()

-- Время / скорость
SetSkeletonAnimationTime(0.0)
local t = GetSkeletonAnimationTime()
local name = GetSkeletonAnimation()
local playing = IsSkeletonPlaying()
SetSkeletonSpeed(1.5)
local s = GetSkeletonSpeed()

local has = HasSkeleton()

-- Управление зацикливанием и интроспекция
SetSkeletonLoop(true)
local looping = IsSkeletonLooping()
local dur  = GetSkeletonDuration()                  -- длительность (с) текущей анимации
local d2   = GetSkeletonAnimationDuration("attack") -- длительность (с) указанной анимации
local ex   = HasSkeletonAnimation("attack")
local list = GetSkeletonAnimationList()             -- массив имён всех анимаций
local nt   = GetSkeletonNormalizedTime()            -- фаза 0..1 текущей анимации
SetSkeletonNormalizedTime(0.5)                      -- перейти к 50% текущей анимации
local done = IsSkeletonAnimationFinished()          -- true, когда незациклённая анимация завершилась
```

> Во время кроссфейда предыдущая анимация продолжает проигрываться (её время идёт дальше), поэтому переход остаётся плавным, а не затухает из замороженной позы.

### Слои анимаций (блендинг / смешивание)

Слои позволяют проигрывать несколько анимаций **одновременно** на одном скелете — бежать ногами и бить руками, присесть и стрелять, аддитивно дышать поверх всего. Каждый слой — независимый трек, накладываемый **поверх** базовой анимации (той, что управляется `PlaySkeletonAnimation`). Треки с бóльшим номером применяются позже (побеждают там, где пересекаются).

```lua
-- PlaySkeletonLayerAnimation(track, name [, loop=false, fade=0, weight=1, rootBone="", additive=false])
--   track    — любое целое >= 1; на треке одновременно одна анимация
--   loop     — one-shot анимации сами затухают по завершении (см. SetSkeletonLayerHold)
--   fade     — секунды для fade-in, fade-out и кроссфейдов внутри этого трека
--   weight   — сила слоя 0..1
--   rootBone — ограничить слой костью и ВСЕМИ её потомками ("" = весь скелет)
--   additive — добавлять дельты анимации поверх позы вместо замещения

-- Удар верхней частью тела, пока базовый слой продолжает бег:
PlaySkeletonAnimation("run", true, 0.12)
PlaySkeletonLayerAnimation(1, "attack", false, 0.08, 1.0, "chest")

-- Удержание прицела только на руке, сила 70%:
PlaySkeletonLayerAnimation(2, "aim", true, 0.15, 0.7, "arm_up_near")

-- Аддитивное дыхание поверх всего:
PlaySkeletonLayerAnimation(3, "breath", true, 0.3, 1.0, "", true)

StopSkeletonLayerAnimation(1)            -- мгновенно
StopSkeletonLayerAnimation(1, 0.1)       -- затухание за 0.1с
StopAllSkeletonLayerAnimations(0.1)

-- Интроспекция / управление
local name = GetSkeletonLayerAnimation(1)       -- "" если трек пуст
local on   = IsSkeletonLayerActive(1)           -- true пока играет или затухает
local pl   = IsSkeletonLayerPlaying(1)
local fin  = IsSkeletonLayerFinished(1)         -- незациклённая дошла до конца
local nt   = GetSkeletonLayerNormalizedTime(1)  -- 0..1
local t    = GetSkeletonLayerTime(1)
SetSkeletonLayerTime(1, 0.0)
SetSkeletonLayerWeight(1, 0.5)
local w    = GetSkeletonLayerWeight(1)
SetSkeletonLayerSpeed(1, 1.5)
local sp   = GetSkeletonLayerSpeed(1)
SetSkeletonLayerHold(1, true)                   -- держать последний кадр вместо авто-затухания
SetSkeletonLayerAdditive(1, true)
SetSkeletonLayerRootBone(1, "chest")
local tracks = GetSkeletonActiveLayerTracks()   -- массив активных номеров треков
```

Примечания:

- Слой трогает только те каналы, по которым в его анимации реально есть ключи (как в Spine), поэтому удар, где прокеены только руки, не убивает покачивание торса от базового бега, даже если `rootBone` покрывает весь верх тела.
- Запуск новой анимации на занятом треке даёт кроссфейд внутри трека через `fade`; повторный запуск той же — плавный рестарт, спамить атаку безопасно.
- Слотовые таймлайны слоя (цвета, смена аттачментов, деформы) применяются только к слотам, чья кость входит в маску слоя; смена аттачментов и порядка отрисовки срабатывает, пока слой доминирует (вес ≥ 0.5).
- **События анимаций работают и на слоях** — кладите событие `hit` в анимацию удара и читайте через `GetSkeletonEvents()` как обычно.
- Время слоя масштабируется и `SetSkeletonLayerSpeed`, и глобальной `SetSkeletonSpeed`.
- Оверрайды костей (`SetSkeletonBoneOverride`) применяются после всех слоёв; моторы рэгдолла отслеживают итоговую слоёную позу.

### Скины и аттачменты

```lua
-- Сменить весь скин (комплекты брони, варианты персонажа)
SetSkeletonSkin("gold_armor")
local skin = GetSkeletonSkin()

-- Переопределить активный аттач одного слота (per-entity, не разделяется между экземплярами)
SetSkeletonAttachment("weapon", "axe")
SetSkeletonAttachment("weapon", "")            -- пусто = скрыть слот

-- Цвет и направление (FlipX — горизонтальное направление, FlipY — конвенция текстуры по V, по умолчанию вкл.)
SetSkeletonColor(1, 0.5, 0.5)                  -- r, g, b [, a]
SetSkeletonFlip(true, true)                    -- flipX, flipY

-- Перечисление слотов / скинов
local slots = GetSkeletonSlotNames()           -- массив имён слотов
local skins = GetSkeletonSkinNames()           -- массив имён скинов

-- Переопределение цвета отдельного слота (например, вспышка одной конечности
-- белым при попадании). Заменяет анимационный цвет слота до сброса.
SetSkeletonSlotColor("left_arm", 1, 1, 1)      -- r, g, b [, a]
ClearSkeletonSlotColor("left_arm")

-- Явно скрыть / показать один слот (независимо от его активного аттача)
HideSkeletonSlot("left_arm")
ShowSkeletonSlot("left_arm")

-- Видимость всего скелета
SetSkeletonVisible(true)
local vis = IsSkeletonVisible()
```

### Тени

```lua
-- Отбрасывание теней (по слотам)
SetSkeletonCastShadow(true)
local shadow = GetSkeletonCastShadow()

-- Не блокировать тени (по умолчанию true): пока глобально включено «Коллайдеры блокируют тени», этот скелет всё равно пропускает тени сквозь себя. Отключите, чтобы он блокировал тени как ландшафт.
SetSkeletonDontBlockShadows(true)
local dontBlock = GetSkeletonDontBlockShadows()  -- → bool

-- Режим тени: 0 = Коллайдеры (квады слотов), 1 = Контур (силуэт по альфе текстуры каждого слота)
SetSkeletonCastShadowMode(1)
local mode = GetSkeletonCastShadowMode()       -- → int

-- Начало тени: 0 = Center, 1 = Top, 2 = Bottom
SetSkeletonShadowOrigin(1)
local origin = GetSkeletonShadowOrigin()       -- → int

-- Смягчение краёв тени [0..1]
SetSkeletonShadowEdgeFade(0.25)
local fade = GetSkeletonShadowEdgeFade()       -- → float

-- Z-порядок тени: отрицательный = к фону, положительный = к переднему плану, 0 = плоскость кастера (по умолчанию)
SetSkeletonShadowZOrder(1)
local zo = GetSkeletonShadowZOrder()           -- → float
```

### Кости

```lua
-- Мировой трансформ кости (уже учитывает трансформ entity)
local b = GetSkeletonBoneWorld("head")
-- b = { found, x, y, rotation, scaleX, scaleY }
if b.found then DrawDebugCircle(b.x, b.y, 4) end

-- Процедурное переопределение (look-at, отдача и т.п.). weight 0..1 смешивает поверх анимации.
SetSkeletonBoneOverride("head", x, y, rot)            -- по умолчанию sx=sy=1, weight=1
SetSkeletonBoneOverride("head", x, y, rot, 1, 1, 0.5) -- 50% смешивания
ClearSkeletonBoneOverride("head")

-- Перечисление и локальная (относительно родителя) поза
local bones   = GetSkeletonBoneNames()                -- массив имён костей
local count   = GetSkeletonBoneCount()
local hasBone = HasSkeletonBone("head")
local l = GetSkeletonBoneLocal("head")
-- l = { found, x, y, rotation, scaleX, scaleY, shearX, shearY }
```

### Точки привязки (оружие / VFX)

```lua
-- Point-аттачменты в мировых координатах (учитывают трансформ entity)
local p = GetSkeletonAttachPointWorld("muzzle")
-- p = { found, x, y, rotation }
if p.found then SpawnBullet(p.x, p.y, p.rotation) end

-- Та же точка в локальном пространстве скелета (без трансформа сущности и FlipX)
local lp = GetSkeletonAttachPoint("muzzle")    -- → {found, x, y, rotation}

local names = GetSkeletonAttachPointNames()    -- массив строк
local count = GetSkeletonAttachPointCount()    -- → int
```

Позиции сокетов следуют за живой позой, поэтому учитывают и
`SetSkeletonBoneOverride` (прицеливание мышью, look-at), и IK-констрейнты. При
рэгдолле они берутся напрямую из физических тел. Чтобы приклеить спрайт оружия
к сокету, используйте движковую привязку, а не ручное перемещение:

```lua
AttachSpriteToSocketFrom("hand_point", "skeleton", -1, true, 1)
```

### Ассет скелета

```lua
local path = GetSkeletonPath()                       -- → текущий .ice_skeleton
local ok = SetSkeletonAsset("Content/SK_Boss.ice_skeleton")
-- Уничтожает рэгдолл/костные тела, очищает позу, скины, слои и оверрайды,
-- затем загружает новый скелет. После этого вызовите PlaySkeletonAnimation.
```

### Коллайдеры костей (анимированные хит-тела)

Пока скелет анимируется (рэгдолл выключен), каждая физическая кость получает **кинематический коллайдер, следующий за позой** каждый кадр — с учётом трансформа сущности, FlipX и масштаба. Они толкают динамические объекты (ящики, чужие рэгдоллы), принимают попадания, видны в дебаг-просмотре коллайдеров и никогда не сталкиваются с собственным Rigidbody-контроллером сущности. При включении рэгдолла те же тела переключаются в динамические и наследуют накопленные скорости — импульс движения переносится бесплатно.

```lua
SetSkeletonBoneColliders(true)                  -- включено по умолчанию (чекбокс "Коллайдеры костей")
local on = AreSkeletonBoneCollidersEnabled()
```

### Рэгдолл (гибридный активный рэгдолл)

```lua
-- Строит тела + суставы из физических костей и переключает на физику.
-- blend: 0 = жёсткие моторы (следует анимации, но реагирует на столкновения), 1 = полностью безвольно.
EnableSkeletonRagdoll()        -- по умолчанию 1 (безвольно)
EnableSkeletonRagdoll(0.85)    -- почти безвольно, немного управляется

DisableSkeletonRagdoll()       -- уничтожить тела, вернуться к анимации
local r = IsSkeletonRagdoll()

SetSkeletonRagdollBlend(0.4)   -- живое смешивание; плавно к 0 для вставания
local b = GetSkeletonRagdollBlend()

-- Направленная хит-реакция по конкретной кости
ApplySkeletonBoneImpulse("torso", dirX * 400, dirY * 400)

-- Ручное включение/выключение работает и через флаг компонента (чекбокс
-- в редакторе / Lua): рантайм держит RagdollEnabled и живое состояние синхронно.

-- Расширенное управление физикой тел костей (рэгдолл должен быть активен)
ApplySkeletonBoneImpulseAtPoint("torso", ix, iy, px, py) -- импульс в мировой точке (добавляет вращение)
ApplySkeletonBoneTorque("torso", 5000)                   -- постоянный момент (по часовой = плюс)
ApplySkeletonBoneAngularImpulse("torso", 200)            -- мгновенный угловой толчок
local v = GetSkeletonBoneVelocity("torso")               -- { found, x, y } в px/с

ApplySkeletonRagdollImpulse(dirX * 500, dirY * 500)      -- толкнуть все тела (взрывная волна по всему телу)
SetSkeletonRagdollGravityScale(1.0)                      -- живой масштаб гравитации для всех тел
SetSkeletonRagdollAngularDamping(0.5)                    -- живое угловое демпфирование для всех тел
```

### Расчленение (отрыв конечностей)

```lua
-- Разорвать сустав кости, чтобы конечность (и всё, что ниже неё в иерархии)
-- оторвалась и отлетела как свободные обломки. Рэгдолл должен быть активен —
-- отрезанные кости продолжают симулироваться как свободные тела.
local cut  = SeverSkeletonBone("left_forearm")    -- → true, если сустав был разорван
local gone = IsSkeletonBoneSevered("left_forearm")
local all  = GetSeveredSkeletonBones()            -- массив имён отрезанных костей

-- Порог разрыва для кости: сустав авто-рвётся, когда сила в нём превышает порог.
-- Задаётся в редакторе скелета (вкладка физики) или переопределяется на экземпляре
-- в рантайме. 0 = не рвётся автоматически.
SetSkeletonBoneBreakForce("neck", 800)

-- Кости, оторванные силой в этом кадре (авто-разрыв) — здесь запускаем гор-VFX / звук
for _, boneName in ipairs(GetSkeletonSeverEvents()) do
    SpawnBloodFountain(boneName)
    HideSkeletonSlot(boneName .. "_skin")         -- либо сменить на аттач кровавой культи
end
```

### Хитбоксы (bounding box)

```lua
-- Bounding-box аттачменты, заданные по слотам, каждый кадр пересчитываются
-- в мировые координаты вслед за живой позой / рэгдоллом — идеально для
-- по-конечностных хёртбоксов.
local boxes = GetSkeletonBoundingBoxNames()                  -- массив имён боксов
local hb = GetSkeletonBoundingBoxWorld("head_hurtbox")
-- hb = { found, points = { {x = .., y = ..}, ... } }
if SkeletonBoundingBoxContainsPoint("head_hurtbox", hitX, hitY) then
    SeverSkeletonBone("head")
end
```

### События

```lua
-- События, пересечённые в этом кадре (задаются в таймлайне анимации в редакторе скелета)
for _, e in ipairs(GetSkeletonEvents()) do
    -- e = { name, int, float, string, time }
    if e.name == "footstep" then PlaySound("step") end
    if e.name == "hit" then DealDamage(e.int) end
end
```

### Типичный пример

```lua
function OnUpdate(dt)
    if IsSkeletonRagdoll() then
        -- плавно вернуть управление анимации после нокдауна
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

### Материал, затенение и смешивание

```lua
-- Затенение и режим смешивания на уровне сущности (независимо от ассета скелета)
SetSkeletonShadingMode("Unlit")            -- "Lit" | "Unlit"
local sm = GetSkeletonShadingMode()
SetSkeletonBlendMode("Additive")           -- "Masked" | "Additive" | "Translucent" | "Opaque"
local bm = GetSkeletonBlendMode()
SetSkeletonAlphaClip(0.5)                  -- порог отсечения (masked) 0..1
local ac = GetSkeletonAlphaClip()

-- Назначить пользовательский материал / экземпляр материала (.ice_material или .ice_matinst)
SetSkeletonMaterial("Materials/Hero.ice_matinst")
local mat = GetSkeletonMaterial()          -- "" если не задан / не кастомный шейдер
ClearSkeletonMaterial()
```

### Вершинные эффекты (GLSL)

```lua
-- Функции специально для скелета
GLSL_SkeletonSetParallax(0.1, 0.05)
GLSL_SkeletonSetSway(2.0, 1.5)             -- амплитуда, скорость [, сдвиг фазы, градиент]
GLSL_SkeletonSetWind(0.3)                  -- сила [, скорость]

-- Общие функции GLSL_* также применяются к скелету, если у сущности
-- нет компонента спрайта / флипбука:
GLSL_SetParallax(0.1, 0.05)
GLSL_ClearEffects()
```

---

## 11. Camera — Камера

> **Тип:** Entity-bound + глобальные. Работает с **основной камерой** (Primary = true).

### Позиция

```lua
SetCameraPosition(100, 200)
local pos = GetCameraPosition()  -- → {x, y}
MoveCamera(5, 0)                 -- Сдвинуть
```

### Ортографическая ширина (масштаб камеры)

```lua
SetCameraOrthoWidth(2.0)               -- Увеличить масштаб (приблизить)
local ortho = GetCameraOrthoWidth()    -- Получить текущую орто-ширину
```

### Следование за персонажем

```lua
-- Мгновенно
CameraFollowMe()

-- Плавно (lerp)
CameraFollowSmooth(0.1)         -- lerpFactor (0..1). Меньше = плавнее.
```

### Запаздывание камеры (Camera Lag)

```lua
-- Camera lag — глобальная настройка. Вызывай ОДИН раз (например в OnCreate),
-- чтобы настроить, как движок плавно догоняет primary-камерой её цель.
-- Скорость задаётся в пикселях/секунду.
SetCameraLag(600, 120)          -- скорость (пикс/сек), макс. дистанция (пикс)

-- Алиас SetCameraLag для обратной совместимости (та же stateful-семантика).
CameraFollowWithLag(600, 120)

-- Чтение / отключение.
local lag = GetCameraLag()      -- → {speed, maxDistance, enabled}
DisableCameraLag()              -- Выключить лаг
```

### Типичный пример Camera Lag

```lua
function OnCreate()
    -- Настраиваем один раз. Движок сам применяет лаг каждый кадр.
    SetCameraLag(600, 120)
end
```

### Тряска камеры

```lua
ShakeCamera(5.0, 0.3)           -- intensity, duration (сек)
StopCameraShake()
local shaking = IsCameraShaking()
```

### Дополнительно

```lua
-- Смещение камеры (для cutscene и т.д.)
SetCameraOffset(50, 0)
local offset = GetCameraOffset()  -- → {x, y}

-- Цвет фона
SetCameraBackgroundColor(0.1, 0.1, 0.2)
local bg = GetCameraBackgroundColor()  -- → {r, g, b}

-- Мировые границы видимости
local bounds = GetCameraWorldBounds()
-- → {left, right, top, bottom, width, height}

-- Проверка видимости на экране
local visible = IsOnScreen(worldX, worldY)
local visible = IsOnScreen(worldX, worldY, 50)  -- С запасом

-- Размер окна
local vp = GetViewportSize()  -- → {width, height}

-- Ближняя / дальняя плоскости отсечения
SetCameraNearPlane(0.1)
local near = GetCameraNearPlane()  -- → float (по умолчанию 0.1)
SetCameraFarPlane(1000.0)
local far = GetCameraFarPlane()    -- → float (по умолчанию 1000.0)
```

### Ограничения камеры (Bounds)

Устанавливает границы мира, за которые камера не может выйти. Автоматически применяется при `SetCameraPosition`, `CameraFollowMe`, `CameraFollowSmooth`.

```lua
-- Установить границы уровня
SetCameraBounds(-500, -300, 2000, 600)  -- minX, minY, maxX, maxY

-- Камера будет автоматически ограничена
CameraFollowSmooth(0.1)

-- Проверить
if HasCameraBounds() then
    local bounds = GetCameraBounds()  -- → {enabled, minX, minY, maxX, maxY}
    Print("MinX: " .. bounds.minX)
end

-- Убрать ограничения
ClearCameraBounds()
```

### Сплит-скрин камера

Функции для сплит-скрин мультиплеера на уровне сущности. `PlayerIndex` привязывает камеру к слоту локального игрока, `ViewportRect` задаёт область экрана.

```lua
-- Назначить индекс игрока камере этой сущности (-1 = не назначен)
SetCameraPlayerIndex(1)
local idx = GetCameraPlayerIndex()  -- → int

-- Задать область обзора (x, y, ширина, высота в нормализованных координатах 0-1)
SetCameraViewportRect(0.5, 0.0, 0.5, 1.0)  -- Правая половина экрана
local vr = GetCameraViewportRect()  -- → {x, y, width, height}

-- То же, но для другой сущности
SetCameraPlayerIndexByEntity(2)  -- Назначить игрока 2 камере этой сущности
SetCameraViewportRectByEntity(0.0, 0.5, 1.0, 0.5)  -- Нижняя половина

-- Главная камера (с неё рендерит движок). Назначение одной главной снимает флаг с остальных.
SetCameraPrimary(true)                 -- сделать камеру этой сущности главной
SetCameraPrimary(false)                -- снять флаг
local isPrimary = IsCameraPrimary()    -- → bool
-- Переключить активную камеру на конкретную сущность (удобно для катсцен / смены вида):
SetCameraPrimaryByEntity(otherCameraId, true)
local p = GetCameraPrimaryByEntity(otherCameraId)  -- → bool

-- Получить все камеры на сцене
local cameras = GetAllCameras()
-- → массив { entityId, primary, orthoWidth, playerIndex, viewportX, viewportY, viewportW, viewportH }
for _, cam in ipairs(cameras) do
    Print("Entity " .. cam.entityId .. " player=" .. cam.playerIndex)
end
```

---

## 12. Audio — Звук и музыка

> **Тип:** Глобальные функции. Таблица `Audio`.

### Инициализация

```lua
local ok = Audio.Initialize()
Audio.Shutdown()
local ready = Audio.IsInitialized()
```

### Звуки (SFX)

```lua
-- Быстрые функции (без таблицы Audio)
LoadSound("Content/Audio/jump.wav", "jump")
PlaySound("jump")

-- Через таблицу Audio
Audio.LoadSound("Content/Audio/shoot.wav", "shoot")
Audio.PlaySound("shoot")
Audio.StopSound("shoot")
Audio.PauseSound("shoot")
Audio.ResumeSound("shoot")

-- Проверки
local playing = Audio.IsSoundPlaying("shoot")
local finished = Audio.IsSoundFinished("shoot")
local has = Audio.HasSound("shoot")

-- Свойства звука
Audio.SetSoundVolume("shoot", 0.8)
Audio.SetSoundPitch("shoot", 1.2)     -- Высота тона (1.0 = нормальная)
Audio.SetSoundLoop("shoot", false)
Audio.SetSoundPan("shoot", -0.5)      -- Панорама (-1..1: лево..право)

-- Получить
local vol = Audio.GetSoundVolume("shoot")
local pitch = Audio.GetSoundPitch("shoot")
local time = Audio.GetSoundCurrentTime("shoot")
local dur = Audio.GetSoundDuration("shoot")

-- Перемотка
Audio.SeekSound("shoot", 1.5)         -- Перейти на 1.5 секунд

-- Удалить
Audio.UnloadSound("shoot")
```

### Музыка

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

Быстрые функции (без таблицы `Audio`):

```lua
PlayMusic("Content/Audio/bg_music.ogg")
StopMusic()
```

### Пространственный звук (3D Audio)

```lua
-- Загрузить пространственный звук
Audio.LoadSoundSpatial("Content/Audio/explosion.wav", "explosion", 1.0, 100.0, 1.0)
-- filePath, name, minDistance, maxDistance, rolloff

-- Позиция/скорость/направление источника
Audio.SetSoundPosition("explosion", x, y, z)
Audio.SetSoundVelocity("explosion", vx, vy, vz)
Audio.SetSoundDirection("explosion", dx, dy, dz)
Audio.SetSoundMinDistance("explosion", 2.0)
Audio.SetSoundMaxDistance("explosion", 200.0)
Audio.SetSoundRolloff("explosion", 1.5)
Audio.SetSoundDopplerFactor("explosion", 1.0)
Audio.SetSoundCone("explosion", 90, 180, 0.3)  -- innerAngle, outerAngle, outerVolume

-- Слушатель (обычно камера или игрок)
Audio.SetListenerPosition(x, y, z)
Audio.SetListenerVelocity(vx, vy, vz)
Audio.SetListenerDirection(dx, dy, dz)
Audio.SetListenerWorldUp(0, 1, 0)
```

### Группы звуков

```lua
-- Константы групп
Audio.GROUP_MASTER   -- 0
Audio.GROUP_MUSIC    -- 1
Audio.GROUP_SFX      -- 2
Audio.GROUP_VOICE    -- 3
Audio.GROUP_AMBIENT  -- 4
Audio.GROUP_UI       -- 5

-- Громкость группы
Audio.SetGroupVolume(Audio.GROUP_SFX, 0.8)
local vol = Audio.GetGroupVolume(Audio.GROUP_SFX)

-- Заглушить группу
Audio.SetGroupMuted(Audio.GROUP_MUSIC, true)
local muted = Audio.IsGroupMuted(Audio.GROUP_MUSIC)

-- Мастер-громкость
Audio.SetMasterVolume(1.0)
local masterVol = Audio.GetMasterVolume()
Audio.SetMasterMuted(false)
local masterMuted = Audio.IsMasterMuted()
```

### Эффекты обработки звука

```lua
-- Фильтры
Audio.SetSoundLowPassFilter("music", true, 5000)      -- cutoffHz
Audio.SetSoundHighPassFilter("music", true, 200)
Audio.SetSoundLoShelf("music", true, 200, -6.0)       -- freqHz, gainDB
Audio.SetSoundHiShelf("music", true, 4000, 3.0)

-- Задержка (эхо)
Audio.SetSoundDelay("music", true, 0.25, 0.5, 0.5, 1.0)
-- enabled, delaySec, decay, wet, dry

-- Реверберация
Audio.SetSoundReverb("music", true, 0.7, 0.3, 0.5, 0.5)
-- enabled, decay, wet, roomSize, damping
```

### Затухание (Fade)

```lua
Audio.FadeSound("bgm", 0.0, 2.0)      -- Затухание за 2 секунды
Audio.FadeIn("bgm", 1.0)              -- Плавное появление за 1 секунду
Audio.FadeOut("bgm", 2.0)             -- Плавное затухание за 2 секунды
```

### Управление всеми звуками

```lua
Audio.StopAllSounds()
Audio.PauseAllSounds()
Audio.ResumeAllSounds()
```

### Звуки на сущности (AudioComponent)

```lua
PlayEntitySound()           -- Проиграть первый инстанс
PlayEntitySound(1)          -- Проиграть второй инстанс
StopEntitySound(0)
PauseEntitySound(0)
ResumeEntitySound(0)
local playing = IsEntitySoundPlaying(0)

SetEntitySoundVolume(0.8, 0)
local vol = GetEntitySoundVolume(0)        -- прочитать громкость живого канала
SetEntitySoundPitch(1.1, 0)
local pitch = GetEntitySoundPitch(0)       -- прочитать pitch живого канала
SetEntitySoundLoop(true, 0)                -- зациклить играющий инстанс
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

-- Мировая трансформация (трансформ сущности уже учтён — см. раздел Sprite)
SetAudioWorldPosition(120, 64, 0)
local awp = GetAudioWorldPosition(0)       -- → {x, y, z}
SetAudioWorldRotation(30, 0)
local awr = GetAudioWorldRotation(0)       -- → число
local aws = GetAudioWorldScale(0)          -- → {x, y}, только чтение

-- Подмена звукового ассета (останавливает и выгружает предыдущий у этого инстанса)
local ok = SetAudioAsset("Content/Audio/Explosion.wav", 0)
local sndPath = GetAudioPath(0)            -- → текущий путь звука

-- Конфиг инстанса (хранится в AudioInstance, применяется при следующем воспроизведении)
SetAudioGroup(2, 0)                        -- шина: 0=Master,1=Music,2=SFX,3=Voice,4=Ambient,5=UI
local group = GetAudioGroup(0)             -- → int (по умолчанию 2 = SFX)
SetAudioSpatial(true, 0)                   -- включить 3D-позиционный звук для этого инстанса
local spatial = IsAudioSpatial(0)          -- → bool
SetAudioMinDistance(1.0, 0)                -- также применяется вживую, если инстанс играет
local minD = GetAudioMinDistance(0)
SetAudioMaxDistance(100.0, 0)              -- также применяется вживую, если инстанс играет
local maxD = GetAudioMaxDistance(0)
SetAudioRolloff(1.0, 0)                    -- также применяется вживую, если инстанс играет
local rolloff = GetAudioRolloff(0)
SetAudioPlayOnWake(false, 0)               -- авто-проигрывание при пробуждении сущности
local playOnWake = GetAudioPlayOnWake(0)
SetAudioOverrideLoop(true, 0)              -- если true, Loop инстанса переопределяет ассет звука
local ovLoop = GetAudioOverrideLoop(0)
SetAudioOverrideSpatial(true, 0)           -- если true, spatial-поля инстанса переопределяют ассет
local ovSpatial = GetAudioOverrideSpatial(0)
```

### Глобальные настройки

```lua
Audio.SetGlobalGain(1.0)
local gain = Audio.GetGlobalGain()
Audio.SetGlobalDopplerFactor(1.0)
local doppler = Audio.GetGlobalDopplerFactor()
Audio.SetSpeedOfSound(343.0)
local speed = Audio.GetSpeedOfSound()
Audio.SetSpatialAudioEnabled(true)
local spatial = Audio.IsSpatialAudioEnabled()
Audio.SetGlobalVolume(1.0)  -- alias для мастер-громкости
local name = Audio.GetDeviceName()
local rate = Audio.GetSampleRate()
```

---

## 13. FX — Визуальные эффекты частиц

> **Тип:** Entity-bound. Требует компонент **FXComponent**.

### Воспроизведение

```lua
PlayFX()              -- Запустить первый эффект (сброс времени, снятие паузы)
PlayFX(1)             -- Запустить второй эффект
StopFX()              -- Прекратить эмиссию и убрать существующие частицы
PauseFX()             -- Заморозить эффект на месте, сохранив частицы
ResumeFX()            -- Разморозить без сброса времени
ResetFX()             -- Сбросить время в 0

-- Все эффекты разом
PlayAllFX()           -- Запустить все (со сбросом времени)
StopAllFX()           -- Остановить все
PauseAllFX()          -- Заморозить все на месте
ResumeAllFX()         -- Разморозить все
```

**Stop против Pause.** `StopFX` прекращает эмиссию **и** убирает уже живые частицы.
`PauseFX` замораживает всё как есть — частицы остаются на экране и продолжают
с того же состояния при `ResumeFX`.

**Конец не-зацикленного эффекта.** Когда эмиттер доходит до своей Duration, он
прекращает эмиссию, но уже созданные частицы досимулировываются до конца своего
времени жизни. Только после этого `IsFXFinished` становится true, и только тогда
освобождаются не-компонентные эффекты (запущенные через `SpawnFXAtPosition` и подобные).
Ставьте Duration эмиттера в 0 для эффекта, который должен работать вечно — например,
для стоячего бассейна воды.

### Проверки и информация

```lua
local playing = IsFXPlaying()           -- Играет ли?
local finished = IsFXFinished()         -- Закончился ли (не играет + нет живых частиц)?
local count = GetFXCount()              -- Кол-во FX-инстансов на сущности
local emitters = GetFXEmitterCount()    -- Кол-во рантайм-эмиттеров в инстансе
local time = GetFXElapsedTime()         -- Прошедшее время
local duration = GetFXDuration()        -- Длительность из ассета (Duration эмиттера)
local particles = GetFXParticleCount()  -- Всего живых частиц
```

### Имя и путь

```lua
local name = GetFXName()
local path = GetFXPath()
```

### Скорость, цикл и автостарт

```lua
SetFXSpeed(2.0)
local speed = GetFXSpeed()

SetFXLoop(true)
local loop = GetFXLoop()

SetFXPlayOnWake(true)
local pow = GetFXPlayOnWake()
```

### Видимость и рендер

```lua
SetFXVisible(true)
local vis = IsFXVisible()

SetFXRenderInGame(true)
local rig = GetFXRenderInGame()

SetFXCastShadow(false)
local shadow = GetFXCastShadow()

-- Не блокировать тени (по умолчанию true): пока глобально включено «Коллайдеры блокируют тени», этот FX всё равно пропускает тени сквозь себя. Отключите, чтобы он блокировал тени как ландшафт.
SetFXDontBlockShadows(true)
local dontBlock = GetFXDontBlockShadows()  -- → bool

SetFXShadowOrigin(1)                -- 0 = Center, 1 = Top, 2 = Bottom
local origin = GetFXShadowOrigin()  -- → int

SetFXShadowEdgeFade(0.25)
local fade = GetFXShadowEdgeFade()  -- → float [0..1]

SetFXShadowZOrder(1)                -- отрицательное = на задний план, положительное = на передний план, 0 = плоскость кастера (по умолчанию)
local zo = GetFXShadowZOrder()      -- → float

-- Опциональный второй аргумент — индекс FX-инстанса (по умолчанию 0).
SetFXShadowZOrder(1, 0)
```

### Порядок рендеринга

```lua
SetFXOrder(5)                -- Задать порядок рендеринга (Z-глубина) для сущности
SetFXOrder(3, 1)             -- Задать порядок рендеринга для конкретного FX-экземпляра
local order = GetFXOrder()   -- Получить порядок рендеринга
local order = GetFXOrder(1)  -- Получить порядок рендеринга для конкретного экземпляра
```

### Флип

```lua
SetFXFlipX(true)
local flipX = GetFXFlipX()
SetFXFlipY(false)
local flipY = GetFXFlipY()
```

### Локальная трансформация

```lua
SetFXLocalPosition(10, 5)
local lp = GetFXLocalPosition()   -- → {x, y}

SetFXLocalScale(2, 2)
local ls = GetFXLocalScale()      -- → {x, y}

SetFXLocalRotation(45)
local lr = GetFXLocalRotation()

-- Мировая трансформация (трансформ сущности уже учтён — см. раздел Sprite)
SetFXWorldPosition(120, 64, 0)
local fwp = GetFXWorldPosition(0)          -- → {x, y, z}
SetFXWorldRotation(30, 0)
local fwr = GetFXWorldRotation(0)          -- → число
local fws = GetFXWorldScale(0)             -- → {x, y}, только чтение

-- Подмена FX-ассета (уничтожает текущие эмиттеры, они пересоздадутся при отрисовке)
local ok = SetFXAsset("Content/FXs/MuzzleFlash.ice_fx", 0)
local fxPath = GetFXPath(0)                -- → текущий путь .ice_fx
```

Вспышки выстрела и VFX попаданий удобнее всего ставить через world-сеттеры:

```lua
function OnLateUpdate(dt)
    local muzzle = GetSpriteAttachPointWorld("Muzzle", 1)
    if muzzle.found then
        SetFXWorldPosition(muzzle.x, muzzle.y, 0)
        SetFXWorldRotation(muzzle.rotation, 0)
    end
end
```

### Управление частицами

```lua
ClearFXParticles()      -- Убить все частицы (без остановки спавна)
ClearFXParticles(1)     -- Для второго инстанса
```

### Глобальные настройки FX-системы

```lua
-- Глобальный масштаб времени (влияет на все FX в сцене)
SetFXGlobalTimeScale(0.5)              -- Замедлить в 2 раза
local ts = GetFXGlobalTimeScale()

-- Глобальный масштаб качества (множитель кол-ва частиц)
SetFXQualityScale(0.5)                 -- Уменьшить вдвое
local qs = GetFXQualityScale()
```

### Колбэки событий FX

Регистрация колбэков, которые срабатывают при определённых событиях частиц.  
`OnFXDeath` и `OnFXSpawn` получают позицию и скорость частицы `(px, py, vx, vy)`.
`OnFXCollision` получает те же четыре значения плюс id сущности, в которую попала частица.

```lua
-- Вызывается при столкновении частицы с коллайдером
OnFXCollision(function(px, py, vx, vy, entityId)
    -- px, py     — позиция частицы в момент столкновения
    -- vx, vy     — скорость частицы в момент столкновения
    -- entityId   — сущность, которой принадлежит коллайдер (0xFFFFFFFF, если неизвестна)
    SpawnFXAtPosition("Content/FX/Sparks.ice_fx", px, py)
end)

-- Вызывается при гибели частицы (age >= lifetime)
OnFXDeath(function(px, py, vx, vy)
    SpawnFXAtPosition("Content/FX/Smoke.ice_fx", px, py)
end)

-- Вызывается при спавне частицы
OnFXSpawn(function(px, py, vx, vy)
    Print("Заспавнена на " .. px .. ", " .. py)
end)
```

**Необязательные параметры** — индекс FX-инстанса и индекс эмиттера:

```lua
-- Второй FX-инстанс (индекс 1), все эмиттеры
OnFXCollision(callback, 1)

-- Первый FX-инстанс (индекс 0), второй эмиттер (индекс 1)
OnFXDeath(callback, 0, 1)
```

| Функция | Аргументы | Описание |
|---|---|---|
| `OnFXCollision(fn [, index [, emitterIdx]])` | `fn(px, py, vx, vy, entityId)` | Срабатывает при столкновении частицы |
| `OnFXDeath(fn [, index [, emitterIdx]])` | `fn(px, py, vx, vy)` | Срабатывает при гибели частицы |
| `OnFXSpawn(fn [, index [, emitterIdx]])` | `fn(px, py, vx, vy)` | Срабатывает при спавне частицы |
| `OnFXSensorOverlap(fn [, index [, emitterIdx]])` | `fn(entityId, count, px, py)` | Срабатывает раз за кадр на каждую пересечённую сущность |

### События сенсоров — `OnFXSensorOverlap`

Включите **События сенсоров** в модуле Collision эмиттера и зарегистрируйте колбэк.
Каждый кадр частицы проверяются на пересечение со всеми сенсор-коллайдерами сцены, а результат
агрегируется по сущностям: `count` — сколько частиц касается этой сущности в этом кадре,
`px, py` — их средняя позиция. Само событие никогда не даёт физической реакции; отскакивают ли
частицы от сенсоров — отдельно управляется галкой **Игнорировать сенсоры** (включена по
умолчанию, так что частицы пролетают насквозь). Каждая частица считается не более одного
раза на сущность за кадр.

```lua
-- Лава, которая наносит урон каждой частицей
OnFXSensorOverlap(function(entityId, count, px, py)
    local dmg = count * 4.0 * GetDeltaTime()
    CallInterface(entityId, "TakeDamage", dmg)
end)
```

Требуется модуль **Collision** на эмиттере (он поддерживает кэш коллайдеров).
Сообщается владелец сенсор-коллайдера, а не сам FX.

### Запросы к жидкости — `GetFluidDensityAt`

```lua
local d = GetFluidDensityAt(x, y)   -- 0 = жидкости нет, ~1 = внутри устоявшейся жидкости
```

Опрашивает все не-превью эмиттеры с модулем Fluid в точке мира и возвращает наибольшую
нормализованную плотность (локальная SPH-плотность, делённая на Rest Density эмиттера).
Используйте для проверки «точка под водой» — таймеры утопления, режим плавания, триггеры брызг.

```lua
local underwater = 0.0

function OnUpdate(dt)
    local p = GetPosition()
    if GetFluidDensityAt(p.x, p.y + 24) > 0.4 then   -- голова под водой
        underwater = underwater + dt
        if underwater > 8.0 then
            Health = Health - 10 * dt                -- ваша логика утопления
        end
    else
        underwater = 0.0
    end
end
```

Спящая жидкость по-прежнему отвечает на этот запрос, по-прежнему сообщает о пересечениях
с сенсорами и по-прежнему рисуется — сон пропускает только симуляцию, но не результаты.

### Детерминизм и rollback

`FluidAffectBodies`, `OnFXSensorOverlap`, `OnFXCollision`/`OnFXDeath`/`OnFXSpawn` и
`GetFluidDensityAt` — единственные части FX, способные влиять на геймплейное состояние.
Всё остальное чисто визуальное.

Случайность частиц детерминирована: у каждого эмиттера свой поток случайных чисел, засеянный
по его порядковому номеру создания в сцене, и все случайные величины переносимы между
платформами. Два запуска одной сцены и две машины с одной сборкой дают идентичные частицы.

Тем не менее FX **не входит в состояние отката** — `Rollback.*` намеренно не сохраняет,
не восстанавливает и не пересимулирует частицы (откатывать тысячи частиц дороже, чем оно
стоит). Поэтому в детерминированном rollback-мультиплеере держите эти четыре функции вне
всего, что попадает в контрольную сумму отката, либо стройте такую логику на обычном
Box2D-сенсоре. Серверную репликацию `NetworkReplication` это не затрагивает.

### Simulation Mode и жидкости

Режим **Simulation Mode** (CPU / GPU) у эмиттера относится к общему обновлению частиц.
Модуль **Fluid** всегда считается на CPU независимо от этой настройки — его решатель на
пространственной сетке быстрее, чем был бы GPU-проход при тех количествах частиц, с
которыми работают жидкости.

### Засыпание жидкости

При включённой галке **Разрешить засыпание** (по умолчанию) жидкость прекращает
симуляцию, когда все частицы примерно полсекунды двигались медленнее, чем
**Скорость засыпания**, и просыпается сама. Просыпается она, если: появилась или умерла
частица, эмиттер сдвинулся, его параметры пересобраны или записаны из Lua, изменился набор
коллайдеров сцены, коллайдер движущегося тела оказался рядом с жидкостью, или рядом
оказалась бодрствующая взаимодействующая жидкость. Сон автоматически отключается при
наличии модуля силы (Wind, Vortex, Attractor, Curl Noise, Noise, Spring Force,
Particle Collision, Conditional Module, Custom Script, Size/Color By Speed) — они постоянно
меняли бы состояние.

В сценах, где коллайдеры непрерывно создаются и удаляются, жидкость будет просыпаться
каждый кадр; поставьте `SetFXEmitterFlag("FluidAllowSleep", false)`, если предпочитаете
честно платить за симуляцию, чем дёргать сон каждый кадр.

### Взаимодействие жидкости с жидкостью

Эмиттеры с галкой **Взаимодействие с другими жидкостями** отталкивают частицы друг друга,
поэтому вода и лава держат границу, а не проходят насквозь. Толчок взвешивается отношением
масс, так что жидкость с меньшей **Particle Mass** отталкивается сильнее и оказывается
сверху более тяжёлой. Эмиттеры без этой галки друг друга не видят — ровно как раньше.

```lua
SetFXEmitterFlag("FluidInteract", true)
SetFXEmitterParam("FluidInteractStrength", 60.0)
```

`WakeFXFluid([index])` принудительно будит спящую жидкость. Нужна ровно в одном случае:
тело телепортировали в устоявшуюся жидкость с нулевой скоростью — такое ни одно из
автоматических условий не увидит. Без индекса будит все инстансы на сущности.

```lua
SetPosition(poolX, poolY)
WakeFXFluid()          -- на сущности, которой принадлежит FX воды
```

### Параметры эмиттера — `SetFXEmitterParam` / `GetFXEmitterParam`

Позволяют менять и читать параметры `CachedEmitterParams` в рантайме по строковому имени.

```lua
SetFXEmitterParam("paramName", value)          -- Первый инстанс
SetFXEmitterParam("paramName", value, 1)       -- Второй инстанс
local val = GetFXEmitterParam("paramName")     -- Чтение (первый эмиттер)
local val = GetFXEmitterParam("paramName", 1)  -- Второй инстанс
```

#### Полный список доступных параметров:

**Spawn — спавн:**
| Параметр | Тип | Описание |
|---|---|---|
| `"SpawnRate"` | float | Скорость спавна (частиц/сек) |
| `"ShapeRadius"` | float | Радиус формы спавна |
| `"ShapeInnerRadius"` | float | Внутренний радиус (Ring) |
| `"ShapeSize.X"` | float | Размер формы спавна по X |
| `"ShapeSize.Y"` | float | Размер формы спавна по Y |
| `"SpawnPerUnitDistance"` | float | Дистанция для Spawn Per Unit |
| `"SpawnPerUnitCount"` | int→float | Кол-во при Spawn Per Unit |
| `"SpawnTextureThreshold"` | float | Порог яркости для Spawn From Texture |

**Init — начальные параметры частицы:**
| Параметр | Тип | Описание |
|---|---|---|
| `"LifetimeMin"` | float | Мин. время жизни |
| `"LifetimeMax"` | float | Макс. время жизни |
| `"VelocityMin.X"` | float | Мин. скорость X |
| `"VelocityMin.Y"` | float | Мин. скорость Y |
| `"VelocityMin.Z"` | float | Мин. скорость Z |
| `"VelocityMax.X"` | float | Макс. скорость X |
| `"VelocityMax.Y"` | float | Макс. скорость Y |
| `"VelocityMax.Z"` | float | Макс. скорость Z |
| `"InheritEmitterVelocityScale"` | float | Множитель наследования скорости эмиттера |
| `"SizeMin"` | float | Мин. размер (uniform) |
| `"SizeMax"` | float | Макс. размер (uniform) |
| `"SizeMinXY.X"` | float | Мин. размер X (non-uniform) |
| `"SizeMinXY.Y"` | float | Мин. размер Y (non-uniform) |
| `"SizeMaxXY.X"` | float | Макс. размер X (non-uniform) |
| `"SizeMaxXY.Y"` | float | Макс. размер Y (non-uniform) |
| `"ColorMin.R"` | float | Мин. цвет — красный (0..1) |
| `"ColorMin.G"` | float | Мин. цвет — зелёный |
| `"ColorMin.B"` | float | Мин. цвет — синий |
| `"ColorMin.A"` | float | Мин. цвет — альфа |
| `"ColorMax.R"` | float | Макс. цвет — красный |
| `"ColorMax.G"` | float | Макс. цвет — зелёный |
| `"ColorMax.B"` | float | Макс. цвет — синий |
| `"ColorMax.A"` | float | Макс. цвет — альфа |
| `"RotationMin"` | float | Мин. начальный угол (градусы) |
| `"RotationMax"` | float | Макс. начальный угол |
| `"RotationSpeedMin"` | float | Мин. скорость вращения |
| `"RotationSpeedMax"` | float | Макс. скорость вращения |

**Forces — силы и физика:**
| Параметр | Тип | Описание |
|---|---|---|
| `"Gravity.X"` | float | Гравитация X |
| `"Gravity.Y"` | float | Гравитация Y |
| `"Gravity.Z"` | float | Гравитация Z |
| `"Drag"` | float | Коэффициент сопротивления |
| `"Acceleration.X"` | float | Постоянное ускорение X |
| `"Acceleration.Y"` | float | Постоянное ускорение Y |
| `"Acceleration.Z"` | float | Постоянное ускорение Z |
| `"OrbitSpeed"` | float | Скорость орбитального движения |
| `"OrbitRadius"` | float | Радиус орбиты |
| `"NoiseStrength"` | float | Сила шума |
| `"NoiseFrequency"` | float | Частота шума |

**Curl Noise:**
| Параметр | Тип | Описание |
|---|---|---|
| `"CurlNoiseStrength"` | float | Сила curl-шума |
| `"CurlNoiseFrequency"` | float | Частота curl-шума |
| `"CurlNoiseSpeed"` | float | Скорость анимации curl-шума |
| `"CurlNoiseOctaves"` | int→float | Октавы |
| `"CurlNoiseLacunarity"` | float | Лакунарность |
| `"CurlNoisePersistence"` | float | Персистенция |

**Vortex — вихрь:**
| Параметр | Тип | Описание |
|---|---|---|
| `"VortexCenter.X"` | float | Центр вихря X |
| `"VortexCenter.Y"` | float | Центр вихря Y |
| `"VortexCenter.Z"` | float | Центр вихря Z |
| `"VortexStrength"` | float | Сила вихря |
| `"VortexRadius"` | float | Радиус вихря |
| `"VortexFalloff"` | float | Затухание вихря |
| `"VortexInwardPull"` | float | Сила притяжения к центру |

**Wind — ветер:**
| Параметр | Тип | Описание |
|---|---|---|
| `"WindDirection.X"` | float | Направление ветра X |
| `"WindDirection.Y"` | float | Направление ветра Y |
| `"WindStrength"` | float | Сила ветра |
| `"WindTurbulence"` | float | Турбулентность |
| `"WindTurbulenceFrequency"` | float | Частота турбулентности |
| `"WindGustStrength"` | float | Сила порывов |
| `"WindGustFrequency"` | float | Частота порывов |
| `"WindDrag"` | float | Сопротивление ветру |

**Attractor — аттрактор:**
| Параметр | Тип | Описание |
|---|---|---|
| `"AttractorPosition.X"` | float | Позиция аттрактора X |
| `"AttractorPosition.Y"` | float | Позиция аттрактора Y |
| `"AttractorPosition.Z"` | float | Позиция аттрактора Z |
| `"AttractorStrength"` | float | Сила притяжения |
| `"AttractorRadius"` | float | Радиус действия |
| `"AttractorFalloff"` | float | Затухание |

**Collision — столкновения с миром:**
| Параметр | Тип | Описание |
|---|---|---|
| `"CollisionBounceFactor"` | float | Коэффициент отскока (0..1) |
| `"CollisionFriction"` | float | Трение при столкновении |
| `"CollisionRadiusScale"` | float | Масштаб радиуса коллизии |

**Light — свет от частиц:**
| Параметр | Тип | Описание |
|---|---|---|
| `"LightColor.R"` | float | Цвет света — красный |
| `"LightColor.G"` | float | Цвет света — зелёный |
| `"LightColor.B"` | float | Цвет света — синий |
| `"LightIntensity"` | float | Интенсивность |
| `"LightRadius"` | float | Радиус |
| `"LightFalloff"` | float | Затухание |

**Size/Color By Speed:**
| Параметр | Тип | Описание |
|---|---|---|
| `"SizeBySpeedMin"` | float | Мин. множитель размера от скорости |
| `"SizeBySpeedMax"` | float | Макс. множитель размера от скорости |
| `"SizeBySpeedRange"` | float | Диапазон скорости |
| `"ColorBySpeedRange"` | float | Диапазон скорости для цвета |

**Scale By Density:**
| Параметр | Тип | Описание |
|---|---|---|
| `"ScaleByDensityMin"` | float | Мин. масштаб от плотности |
| `"ScaleByDensityMax"` | float | Макс. масштаб от плотности |

**Spring — пружина:**
| Параметр | Тип | Описание |
|---|---|---|
| `"SpringStiffness"` | float | Жёсткость пружины |
| `"SpringDamping"` | float | Демпфирование пружины |

**Particle Collision — межчастичные столкновения:**
| Параметр | Тип | Описание |
|---|---|---|
| `"ParticleCollisionRadius"` | float | Радиус столкновения |
| `"ParticleCollisionBounce"` | float | Отскок |
| `"ParticleCollisionRepulsion"` | float | Сила отталкивания |

**Stretch / Animation / Ribbon:**
| Параметр | Тип | Описание |
|---|---|---|
| `"StretchVelocityScale"` | float | Масштаб растяжения от скорости |
| `"SpriteAnimationSpeed"` | float | Скорость анимации спрайта |
| `"RibbonWidth"` | float | Ширина ленты |
| `"RibbonMinVertexDistance"` | float | Мин. расстояние между сегментами ленты |

**Kill Condition — зона уничтожения:**
| Параметр | Тип | Описание |
|---|---|---|
| `"KillZoneCenter.X"` | float | Центр зоны X |
| `"KillZoneCenter.Y"` | float | Центр зоны Y |
| `"KillZoneCenter.Z"` | float | Центр зоны Z |
| `"KillZoneSize.X"` | float | Размер зоны X |
| `"KillZoneSize.Y"` | float | Размер зоны Y |
| `"KillZoneRadius"` | float | Радиус зоны (для Sphere) |
| `"KillSpeedThreshold"` | float | Порог скорости |

**Fluid — физика жидкости (SPH):**
| Параметр | Тип | Описание |
|---|---|---|
| `"FluidRestDensity"` | float | Плотность покоя |
| `"FluidStiffness"` | float | Жёсткость |
| `"FluidViscosity"` | float | Вязкость |
| `"FluidSurfaceTension"` | float | Поверхностное натяжение |
| `"FluidInteractionRadius"` | float | Радиус взаимодействия |
| `"FluidParticleMass"` | float | Масса частицы |
| `"FluidNearStiffness"` | float | Жёсткость ближнего поля |
| `"FluidDamping"` | float | Демпфирование |
| `"FluidMaxSpeed"` | float | Макс. скорость |
| `"FluidCollisionDamping"` | float | Демпфирование при столкновении |
| `"FluidSubSteps"` | int→float | Кол-во подшагов |
| `"FluidMetaballThreshold"` | float | Порог метаболлов |
| `"FluidMetaballScale"` | float | Масштаб метаболлов |
| `"FluidBodyPushScale"` | float | Импульс, передаваемый физическим телам |
| `"FluidBodyDrag"` | float | Вязкое сопротивление для погружённых тел (1/сек) |
| `"FluidSleepSpeed"` | float | Скорость, ниже которой жидкость может заснуть |
| `"FluidInteractStrength"` | float | Сила разделения с другими жидкостями |
| `"FluidInteractViscosity"` | float | Сцепление между разными жидкостями |

**Conditional / Event:**
| Параметр | Тип | Описание |
|---|---|---|
| `"CondModuleThreshold"` | float | Порог условия |
| `"CondModuleValueTrue"` | float | Значение при true |
| `"CondModuleValueFalse"` | float | Значение при false |
| `"EventSpawnCount"` | int→float | Кол-во спавна по событию |
| `"EventInheritVelocityScale"` | float | Множитель наследования скорости |

### Модули, флаги и режимы — структурное управление в рантайме

`SetFXEmitterParam` достаёт только скалярные (`float`/`int`) поля. Функции ниже покрывают
всё остальное в `CachedEmitterParams` в рантайме — включение/выключение целых модулей,
булевы флаги и enum/int-поля «режимов» — применяется вживую ко всем эмиттерам инстанса
(без переавторинга). Все принимают необязательный индекс инстанса в конце (по умолчанию `0`).

```lua
-- Включить / выключить целый модуль (переключает его рантайм-гейт)
SetFXModuleEnabled("Collision", true)
SetFXModuleEnabled("Light", false, 1)          -- второй инстанс
local on = IsFXModuleEnabled("Fluid")

-- Булевы параметры
SetFXEmitterFlag("RandomColor", true)
SetFXEmitterFlag("AlignToVelocity", true)
local immortal = GetFXEmitterFlag("Immortal")

-- Enum / int параметры «режимов» (задаются целым числом)
SetFXEmitterMode("Shape", 1)                   -- 0=Point,1=Circle,2=Box,3=Edge,4=Ring
SetFXEmitterMode("BlendMode", 1)               -- 0=Alpha,1=Additive,2=Multiply
local blend = GetFXEmitterMode("BlendMode")
```

**Переключаемые модули (`SetFXModuleEnabled` / `IsFXModuleEnabled`):**
`"SpawnRate"`, `"SpawnPerUnit"`, `"VelocityOverLife"`, `"SizeOverLife"`, `"ColorOverLife"`,
`"OpacityOverLife"`, `"StretchOverLife"`, `"SubUVOverLife"`, `"CurlNoise"`, `"VortexForce"`,
`"WindForce"`, `"SizeBySpeed"`, `"ColorBySpeed"`, `"ScaleByDensity"`, `"SpringForce"`,
`"ParticleCollision"`, `"Attractor"`, `"Collision"`, `"Light"`, `"Fluid"`, `"KillCondition"`,
`"EventHandler"`, `"RibbonRenderer"`, `"ConditionalModule"`, `"CustomScript"`.

**Булевы флаги (`SetFXEmitterFlag` / `GetFXEmitterFlag`):**
`"Immortal"`, `"VelocityInLocalSpace"`, `"UniformSize"`, `"RandomColor"`, `"AlignToVelocity"`,
`"SpawnOnEdge"`, `"RandomDirection"`, `"LightInheritParticleColor"`, `"LightCastShadows"`,
`"CollisionKillOnSecondHit"`, `"CollisionIgnoreSensors"`, `"CollisionSensorEvents"`,
`"FluidPreFill"`, `"FluidAffectBodies"`, `"FluidAllowSleep"`, `"FluidInteract"`, `"AnimateSprite"`,
`"UseCustomMaterial"`, `"InheritEmitterVelocity"`, `"VortexRelativeToEmitter"`,
`"AttractorRelativeToEmitter"`, `"KillZoneRelativeToEmitter"`, `"EventInheritVelocity"`.

**Enum / int режимы (`SetFXEmitterMode` / `GetFXEmitterMode`):**
| Имя | Значения |
|---|---|
| `"Shape"` | 0=Point, 1=Circle, 2=Box, 3=Edge, 4=Ring |
| `"CollisionResponse"` | 0=None, 1=Destroy, 2=Bounce, 3=Slide |
| `"BlendMode"` | 0=Alpha, 1=Additive, 2=Multiply |
| `"FluidRenderMode"` | 0=Particles, 1=Metaball |
| `"KillCondType"` | 0=Box, 1=Sphere, 2=SpeedMin, 3=SpeedMax |
| `"KillBoxMode"` | 0=Inside, 1=Outside, 2=Contain |
| `"EventTrigger"` | 0=OnCollision, 1=OnDeath, 2=OnSpawn |
| `"ShadingMode"` | индекс режима шейдинга |
| `"SpriteSubImageH"` / `"SpriteSubImageV"` | столбцы / строки Sub-UV сетки |
| `"RibbonMaxSegments"` | Макс. число сегментов ленты |
| `"RibbonTextureMode"` | Режим текстуры ленты |
| `"FluidPreFillCount"` | Кол-во частиц предзаполнения жидкости |
| `"CondModuleSourceType"` / `"CondModuleComparison"` | Источник / сравнение условия |
| `"CondModuleActionTrue"` / `"CondModuleActionFalse"` | Действия условия |

> **Примечание:** эти функции меняют живой рантайм-кэш инстанса эффекта. Они не изменяют
> `.ice_fx` ассет на диске и сбрасываются при пере-кэшировании параметров эмиттера (например,
> после перезагрузки ассета). Включение модуля, которого не было в авторинге, активирует его
> со значениями по умолчанию — их затем можно настроить через `SetFXEmitterParam` /
> `SetFXEmitterFlag` / `SetFXEmitterMode`.

### Пример комплексного использования

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
    -- Динамическое изменение параметров
    local rate = GetFXEmitterParam("SpawnRate")
    if GetFXParticleCount() > 500 then
        SetFXEmitterParam("SpawnRate", rate * 0.5)
    end

    -- Проверка завершения одноразового эффекта
    if IsFXFinished(1) then
        PlayFX(1)
    end
end
```

---

## 13.1. Point Light — Точечные источники света

> **Тип:** Entity-bound. Требует компонент **PointLightComponent**.

### Базовые свойства

```lua
-- Количество источников
local count = GetLightCount()

-- Включить/выключить свет
SetLightEnabled(true)
SetLightEnabled(false, 1)       -- Второй источник
local on = IsLightEnabled()
ToggleLight()                    -- Переключить

-- Включить/выключить все разом
SetAllLightsEnabled(true)
```

### Цвет, интенсивность, радиус

```lua
SetLightColor(1.0, 0.8, 0.3)           -- Тёплый свет
local c = GetLightColor()               -- → {r, g, b}

SetLightIntensity(2.0)
local i = GetLightIntensity()

SetLightRadius(200.0)
local r = GetLightRadius()

SetLightFalloff(2.0)                    -- Экспонента затухания
local f = GetLightFalloff()

-- Батч-установка (цвет + интенсивность + радиус)
SetLight(1, 0.9, 0.7, 2.0, 150)
```

### Позиция и трансформация

```lua
SetLightPosition(100, 200)
local pos = GetLightPosition()          -- → {x, y}

SetLightLocalScale(2, 2)
local ls = GetLightLocalScale()         -- → {x, y}

SetLightLocalRotation(45)
local lr = GetLightLocalRotation()

-- Мировая трансформация (трансформ сущности уже учтён — см. раздел Sprite).
-- Учтите: локальная позиция исторически называется SetLightPosition/GetLightPosition
-- и SetSpotLightPosition/GetSpotLightPosition.
SetLightWorldPosition(120, 64, 0)
local lwp = GetLightWorldPosition(0)         -- → {x, y, z}
SetLightWorldRotation(30, 0)
local lwr = GetLightWorldRotation(0)         -- → число
local lws = GetLightWorldScale(0)            -- → {x, y}, только чтение

SetSpotLightWorldPosition(120, 64, 0)
local swp = GetSpotLightWorldPosition(0)     -- → {x, y, z}
SetSpotLightWorldRotation(30, 0)
local swr = GetSpotLightWorldRotation(0)     -- → число
local sws = GetSpotLightWorldScale(0)        -- → {x, y}, только чтение
```

### Тени и видимость

```lua
SetLightCastShadows(true)
local shadows = GetLightCastShadows()

-- SetLightVisible / GetLightVisible управляют Enabled (обратная совместимость)
SetLightVisible(true)                   -- то же что SetLightEnabled(true)
local vis = GetLightVisible()           -- то же что IsLightEnabled()
```

---

## 13.2. Spot Light — Прожектор (направленный конус)

> **Тип:** Entity-bound. Требует компонент **SpotLightComponent**.

```lua
-- Количество
local count = GetSpotLightCount()

-- Включение
SetSpotLightEnabled(true)
local on = IsSpotLightEnabled()
ToggleSpotLight()                    -- Переключить
SetAllSpotLightsEnabled(true)        -- Включить/выключить все сразу

-- Цвет и интенсивность
SetSpotLightColor(1, 1, 0.9)
local c = GetSpotLightColor()           -- → {r, g, b}
SetSpotLightIntensity(3.0)
local i = GetSpotLightIntensity()

-- Радиус
SetSpotLightRadius(300)
local r = GetSpotLightRadius()

-- Направление
SetSpotLightDirection(0, -1)
local dir = GetSpotLightDirection()     -- → {x, y}

-- Углы конуса (внутренний и внешний, 1°–90°)
SetSpotLightAngles(15, 30)
local a = GetSpotLightAngles()          -- → {inner, outer}

-- Затухание
SetSpotLightFalloff(2.0)
local f = GetSpotLightFalloff()

-- Позиция и трансформ
SetSpotLightPosition(100, 200)
local pos = GetSpotLightPosition()      -- → {x, y}

SetSpotLightLocalScale(2, 2)
local ls = GetSpotLightLocalScale()     -- → {x, y}

SetSpotLightLocalRotation(45)
local lr = GetSpotLightLocalRotation()

-- Тени и видимость
SetSpotLightCastShadows(true)
local sh = GetSpotLightCastShadows()

-- SetSpotLightVisible / GetSpotLightVisible управляют Enabled (обратная совместимость)
SetSpotLightVisible(true)               -- то же что SetSpotLightEnabled(true)
local vis = GetSpotLightVisible()       -- то же что IsSpotLightEnabled()

-- Батч-установка (цвет r,g,b + intensity + radius + dirX,dirY + innerAngle + outerAngle)
SetSpotLight(1, 0.9, 0.7, 2.0, 300, 0, -1, 15, 30)
```

### Текстуры-кукиз (проекция текстур через свет)

**PointLight кукиз** (работают с текущим выбранным PointLight):

```lua
SetLightCookie("Content/cookies/iris.png")
local p = GetLightCookie()              -- путь к текстуре

SetLightCookieIntensity(0.75)           -- 0..4, по умолчанию 1.0
local i = GetLightCookieIntensity()

SetLightCookieRotation(45.0)            -- в градусах
local r = GetLightCookieRotation()
```

**SpotLight кукиз**:

```lua
SetSpotLightCookie("Content/cookies/gobo.png", 0)
local p = GetSpotLightCookie(0)

SetSpotLightCookieIntensity(1.5, 0)
local sci = GetSpotLightCookieIntensity(0)
SetSpotLightCookieRotation(90.0, 0)
local scr = GetSpotLightCookieRotation(0)
```

Все Set/Get принимают необязательный параметр `index` (по умолчанию 0) для адресации
конкретных источников, если у сущности их несколько.

---

## 13.3. Освещение и тени — Глобальные настройки

> **Тип:** Глобальные функции (не привязаны к сущности)

### Режим освещения

```lua
SetLightingMode("Lit")                  -- Включить освещение
SetLightingMode("Unlit")                -- Выключить
local mode = GetLightingMode()          -- → "Lit" или "Unlit"
```

### Ambient (окружающий свет)

```lua
SetAmbientLight(0.1, 0.1, 0.15)        -- Цвет (RGB)
SetAmbientLight(0.1, 0.1, 0.15, 0.2)   -- Цвет + интенсивность
SetAmbientIntensity(0.1)
local ai = GetAmbientIntensity()
local ac = GetAmbientColor()            -- → {r, g, b}
```

### Directional Light (направленный свет / солнце)

```lua
-- dirX, dirY, r, g, b, intensity [, enabled [, castShadows]]
SetDirectionalLight(0.5, -1, 1, 1, 0.9, 0.5)
SetDirectionalLight(0.5, -1, 1, 1, 0.9, 0.5, true)
SetDirectionalLight(0.5, -1, 1, 1, 0.9, 0.5, true, true) -- с тенями

SetDirectionalLightEnabled(true)
local on = IsDirectionalLightEnabled()

SetDirectionalLightCastShadows(true)
local cs = IsDirectionalLightCastShadows()

local dl = GetDirectionalLight()
-- → {dirX, dirY, r, g, b, intensity, enabled, castShadows}
```

### Тени

```lua
SetShadowsEnabled(true)
local on = AreShadowsEnabled()

-- Качество: 0=Off, 1=Low, 2=Medium, 3=High, 4=Ultra
SetShadowQuality(3)
local q = GetShadowQuality()

SetShadowSoftness(0.5)                 -- 0.0–1.0
local s = GetShadowSoftness()

SetShadowIntensity(1.0)                -- 0.0–1.0
local i = GetShadowIntensity()

SetShadowBias(2.0, 0.0)                -- (x, y) смещение в мировых координатах, [-1000, 1000]
local b = GetShadowBias()              -- → { x, y }

SetShadowPCFSamples(5)                 -- 1–7
local p = GetShadowPCFSamples()

-- Длина теней Directional-света (солнца) в мировых юнитах; 0 = без ограничения.
-- Тень плавно затухает после этой дистанции, чтобы высоко висящие объекты не
-- тянули тень через платформы по всему уровню.
SetShadowDirectionalLength(256.0)
local dl = GetShadowDirectionalLength()

-- Затухание тени Directional по Z-глубине в юнитах слоёв; 0 = выкл. Тень слабеет
-- по мере удаления приёмника за кастер по Z (не пускает тень на дальние слои).
SetShadowDirectionalDepthFade(3.0)
local df = GetShadowDirectionalDepthFade()

-- Если true, любой сплошной коллайдер блокирует тени: тень кастера обрывается на
-- следующем коллайдере, а не проходит сквозь него (жёсткое реалистичное перекрытие, дороже).
SetCollidersBlockShadows(true)
local cb = GetCollidersBlockShadows()
```

### Трассировка лучей (только Vulkan)

```lua
-- 2D-глобальное освещение с трассировкой лучей в реальном времени (мягкий
-- непрямой свет, растекание цвета, ray-traced ambient occlusion). Работает
-- ТОЛЬКО на устройствах Vulkan с аппаратной трассировкой лучей; на любом другом
-- бэкенде/устройстве вызовы запоминаются, но не дают визуального эффекта
-- (фича ведёт себя так, будто её нет).
local supported = IsRaytracingSupported()   -- true только на Vulkan с RT

SetRaytracingEnabled(true)
local on = IsRaytracingEnabled()

-- Качество: 0=Low, 1=Medium, 2=High, 3=Ultra (лучей на пиксель + внутреннее разрешение)
SetRaytracingQuality(2)
local q = GetRaytracingQuality()

SetRaytracingIntensity(1.0)            -- общая сила GI (0.0–8.0)
local i = GetRaytracingIntensity()

SetRaytracingBounce(1.0)               -- сила отражённого света / растекания цвета (0.0–4.0)
local b = GetRaytracingBounce()

SetRaytracingMaxBounces(2)             -- число отскоков света / глубина пути, мультибаунс GI (1–8)
local mb = GetRaytracingMaxBounces()

SetRaytracingReflection(0.0)           -- сила зеркальных отражений (0 = диффузный GI, 1 = зеркало)
local rf = GetRaytracingReflection()

SetRaytracingMaxDistance(512.0)        -- макс. дистанция луча в мировых единицах
local d = GetRaytracingMaxDistance()

SetRaytracingDenoise(true)             -- временное + пространственное шумоподавление
local dn = GetRaytracingDenoise()

SetRaytracingShadows(true)             -- теневые лучи: свет перекрывается препятствиями
local sh = GetRaytracingShadows()

-- Трассируемое затенение окружения: контактное затемнение на стыках геометрии (0.0–1.0, 0 = выкл.)
SetRaytracingAOIntensity(0.65)
local ao = GetRaytracingAOIntensity()

SetRaytracingAORadius(96.0)            -- радиус AO в мировых единицах (1.0–8192.0)
local aor = GetRaytracingAORadius()

-- Как применяется непрямой свет: 0 — добавляется поверх изображения,
-- 1 — умножается на цвет поверхности, и отражённый свет ведёт себя как настоящий
SetRaytracingAlbedoResponse(0.75)
local ar = GetRaytracingAlbedoResponse()

-- Свет неба, который подхватывают ушедшие из сцены лучи; умножает цвет
-- окружающего света и складывается с его интенсивностью (0.0–8.0)
SetRaytracingSkyIntensity(1.0)
local sk = GetRaytracingSkyIntensity()

-- Резкость шумоподавителя: выше — чётче контактные тени, но больше шума (0.0–1.0)
SetRaytracingSharpness(0.5)
local sp = GetRaytracingSharpness()

-- Отражённый свет берёт цвет из отрисованной сцены (полноценное растекание
-- цвета и свечение эмиссии); вне экрана — аналитическое освещение
SetRaytracingScreenRadiance(true)
local sr = GetRaytracingScreenRadiance()
```

### Цвет фона (clear color)

```lua
SetClearColor(0.1, 0.1, 0.15)          -- RGB (0.0–1.0)
local c = GetClearColor()              -- → {r, g, b}
```

> `SetClearColor` включает World Override сцены (см. раздел 13.4), поэтому значение сохраняется на уровне и переживает Play/Stop. Для возврата к умолчаниям проекта вызовите `ResetWorldOverride()`.

---

## 13.4. World Override — Постоянное переопределение уровня (Physics + Rendering)

> **Тип:** Глобальные функции. Работают с **World Override** активной сцены — это снимок физики и рендера, который сохраняется внутри `.icemap` и автоматически применяется при загрузке уровня (как в редакторе, так и в runtime-сборке).
>
> Используется, когда Lua-скрипт должен на постоянной основе изменить параметры физики или рендера для текущего уровня. Изменения применяются вживую (Box2D-мир переконфигурируется, обновляется освещение, near/far/clear-color вступают в силу со следующего кадра).

### Сброс и чтение

```lua
ResetWorldOverride()                   -- Отключить override и вернуть умолчания
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

### Применение override (любое подмножество полей)

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

> Таблицы `physics` и `rendering` обе опциональны, и любое поле внутри них тоже опционально. Записываются только перечисленные поля; остальные сохраняют текущее значение. Вызов `ApplyWorldOverride` автоматически включает режим override для той секции, что передана. Изменения физики проталкиваются в живой Box2D-мир; изменения рендера — в систему освещения, и используются для проекции и clear color на следующем кадре.

---

## 14. Collision — Столкновения (AABB)

> **Тип:** Entity-bound. Требует **ColliderComponent**.
> Это *не* физические столкновения Box2D, а проверки пересечений AABB (прямоугольники).

```lua
-- Пересечение с тегом?
if IsOverlappingTag("Coin") then CollectCoin() end
-- Теги поддерживают префикс: "Enemy" совпадает с "EnemyBoss"

-- Получить пересекающуюся сущность
local coin = GetOverlappingEntity("Coin")  -- Первая найденная (или nil)

-- Все пересекающиеся
local coins = GetAllOverlappingEntities("Coin")

-- Количество пересечений
local count = GetOverlappingCount("Enemy")

-- Пересечение с конкретной сущностью
if IsOverlappingEntity(targetId) then ... end

-- Сторона столкновения
local side = GetCollisionSide("Ground")
-- → {left=false, right=false, top=false, bottom=true}
if side.bottom then onGround = true end

-- Близко ли к тегу? (по расстоянию, без коллайдера)
if IsNearTag("Enemy", 200) then Alert() end

-- Ближайший по тегу
local nearest = GetNearestByTag("Enemy")

-- Расстояние до ближайшего с тегом
local dist = DistanceToTag("Checkpoint")
```

### Свойства коллайдеров

#### Отбрасывание теней

```lua
-- Box коллайдер
SetBoxColliderCastShadow(true)
local shadow = GetBoxColliderCastShadow()  -- → bool

-- Sphere коллайдер
SetSphereColliderCastShadow(true)
local shadow = GetSphereColliderCastShadow()

-- Capsule коллайдер
SetCapsuleColliderCastShadow(true)
local shadow = GetCapsuleColliderCastShadow()

-- Не блокировать тени (по умолчанию true): пока глобально включено «Коллайдеры блокируют тени»,
-- эти коллайдеры всё равно пропускают тени сквозь себя. Отключите, чтобы коллайдер блокировал тени как ландшафт.
SetBoxColliderDontBlockShadows(true)
local boxDontBlock = GetBoxColliderDontBlockShadows()      -- → bool
SetSphereColliderDontBlockShadows(true)
local sphDontBlock = GetSphereColliderDontBlockShadows()   -- → bool
SetCapsuleColliderDontBlockShadows(true)
local capDontBlock = GetCapsuleColliderDontBlockShadows()  -- → bool
```

#### Начало тени и смягчение краёв

```lua
-- Начало (origin): 0 = Center (по умолчанию), 1 = Top, 2 = Bottom
SetBoxColliderShadowOrigin(1)
local origin = GetBoxColliderShadowOrigin()       -- → int

SetSphereColliderShadowOrigin(2)
local origin = GetSphereColliderShadowOrigin()

SetCapsuleColliderShadowOrigin(0)
local origin = GetCapsuleColliderShadowOrigin()

-- Смягчение краёв: 0.0 = чёткий силуэт, 1.0 = полностью схлопнут
SetBoxColliderShadowEdgeFade(0.25)
local fade = GetBoxColliderShadowEdgeFade()       -- → float [0..1]

SetSphereColliderShadowEdgeFade(0.5)
local fade = GetSphereColliderShadowEdgeFade()

SetCapsuleColliderShadowEdgeFade(0.0)
local fade = GetCapsuleColliderShadowEdgeFade()

-- Z-порядок: отрицательное = на задний план, положительное = на передний план, 0 = плоскость кастера (по умолчанию)
SetBoxColliderShadowZOrder(1)
local zo = GetBoxColliderShadowZOrder()           -- → float

SetSphereColliderShadowZOrder(0)
local zo = GetSphereColliderShadowZOrder()

SetCapsuleColliderShadowZOrder(1)
local zo = GetCapsuleColliderShadowZOrder()

-- Опциональный второй аргумент — индекс коллайдера (по умолчанию 0).
SetBoxColliderShadowOrigin(1, 0)
SetBoxColliderShadowEdgeFade(0.3, 0)
SetBoxColliderShadowZOrder(1, 0)
```

#### Локальный трансформ

```lua
-- Box коллайдер
SetBoxColliderLocalPosition(5, 10)
local pos = GetBoxColliderLocalPosition()    -- → {x, y}
SetBoxColliderLocalScale(2, 2)
local scale = GetBoxColliderLocalScale()     -- → {x, y}
SetBoxColliderLocalRotation(45)
local rot = GetBoxColliderLocalRotation()    -- → float (градусы)

-- Sphere коллайдер
SetSphereColliderLocalPosition(5, 10)
local pos = GetSphereColliderLocalPosition() -- → {x, y}
SetSphereColliderLocalScale(2, 2)
local scale = GetSphereColliderLocalScale()  -- → {x, y}
SetSphereColliderLocalRotation(45)
local rot = GetSphereColliderLocalRotation() -- → float (градусы)

-- Capsule коллайдер
SetCapsuleColliderLocalPosition(5, 10)
local pos = GetCapsuleColliderLocalPosition() -- → {x, y}
SetCapsuleColliderLocalScale(2, 2)
local scale = GetCapsuleColliderLocalScale()  -- → {x, y}
SetCapsuleColliderLocalRotation(45)
local rot = GetCapsuleColliderLocalRotation() -- → float (градусы)

-- Все функции принимают опциональный index для нескольких коллайдеров:
SetBoxColliderLocalPosition(5, 10, 1)  -- Второй box коллайдер
```

#### Мировая трансформация

Те же правила преобразования, что и у спрайтов — трансформ сущности уже учтён.
Доступно для всех трёх типов коллайдеров (`Box`, `Sphere`, `Capsule`):

```lua
-- Box
SetBoxColliderWorldPosition(120, 64, 0)
local cwp = GetBoxColliderWorldPosition(0)          -- → {x, y, z}
SetBoxColliderWorldRotation(30, 0)
local cwr = GetBoxColliderWorldRotation(0)          -- → число
local cws = GetBoxColliderWorldScale(0)             -- → {x, y}, только чтение

-- Sphere
SetSphereColliderWorldPosition(120, 64, 0)
local swp = GetSphereColliderWorldPosition(0)       -- → {x, y, z}
SetSphereColliderWorldRotation(30, 0)
local swr = GetSphereColliderWorldRotation(0)       -- → число
local sws = GetSphereColliderWorldScale(0)          -- → {x, y}, только чтение

-- Capsule
SetCapsuleColliderWorldPosition(120, 64, 0)
local pwp = GetCapsuleColliderWorldPosition(0)      -- → {x, y, z}
SetCapsuleColliderWorldRotation(30, 0)
local pwr = GetCapsuleColliderWorldRotation(0)      -- → число
local pws = GetCapsuleColliderWorldScale(0)         -- → {x, y}, только чтение
```

> Они двигают трансформ-смещение коллайдера. Как и локальные сеттеры, сами по
> себе они не пересоздают Box2D-шейп — если нужно, чтобы физический шейп
> последовал за ними, используйте функции геометрии шейпа в рантайме ниже.

#### Геометрия шейпа в рантайме (изменение размера / пересоздание)

> Эти функции изменяют сам Box2D-шейп **во время игры**: они обновляют сериализованные поля геометрии в компоненте, после чего уничтожают и заново создают `b2Shape` на уже существующем теле с теми же материалом, фильтром, флагами sensor и one-way. Используйте их, чтобы уменьшать капсулу игрока при приседании, увеличивать хитбокс на кадре удара или менять радиус сенсора-сферы.
>
> Все сеттеры и функции `Rebuild*` возвращают `true` при успехе и принимают опциональный аргумент `index` (по умолчанию `0`), чтобы выбрать конкретный коллайдер внутри компонента, если их несколько. После пересборки кэшированный `b2ShapeId` становится недействительным — запрашивайте его заново через соответствующий `Get*` или храните своё состояние.
> Размеры указываются в **спрайт-юнитах** (`1.0` = один полный тайл спрайта, `DEFAULT_SPRITE_SIZE` пикселей). Итоговый размер в мире также учитывает масштаб сущности и локальный масштаб коллайдера.

```lua
-- Box коллайдер — Size это 2D-вектор в спрайт-юнитах
SetBoxColliderSize(2.0, 1.0)            -- ширина = 2 тайла, высота = 1 тайл
SetBoxColliderSize(2.0, 1.0, 1)         -- второй Box коллайдер
local size = GetBoxColliderSize()       -- → {x = width, y = height}
RebuildBoxColliderShape()               -- пересоздать b2Shape без изменения Size
RebuildBoxColliderShape(0)

-- Sphere коллайдер — Radius это скаляр в спрайт-юнитах
SetSphereColliderRadius(0.75)
SetSphereColliderRadius(0.75, 1)
local r = GetSphereColliderRadius()     -- → float
RebuildSphereColliderShape()
RebuildSphereColliderShape(0)

-- Capsule коллайдер — Length это длина отрезка между центрами полусфер, Radius это радиус скруглений (всё в спрайт-юнитах)
SetCapsuleColliderLength(0.5)
SetCapsuleColliderLength(0.5, 1)
local len = GetCapsuleColliderLength()  -- → float

SetCapsuleColliderRadius(0.25)
SetCapsuleColliderRadius(0.25, 1)
local cr  = GetCapsuleColliderRadius()  -- → float

RebuildCapsuleColliderShape()
RebuildCapsuleColliderShape(0)
```

> **Шейпы коллизий тайлмапов и флипбуков** генерируются из исходного тайлсета / спрайт-ассета. Для тайлмапа после изменения флагов коллизии или замены тайлов вызовите `RebuildTilemapPhysics()`, чтобы перегенерировать статическое тело. Полигоны коллизии флипбука автоматически следуют за активным кадром — материальные свойства (плотность / трение / упругость / sensor / one-way) при этом можно менять в рантайме через функции `SetFlipbookCollision*`.

#### Группа столкновений и режим — для отдельного коллайдера

> В отличие от `SetCollisionGroupByName` / `SetCollisionEnabled` (которые работают со **всем** телом сразу), эти функции позволяют настраивать **каждый** коллайдер компонента независимо. Опциональный `index` (0 по умолчанию) выбирает шейп внутри компонента, если их несколько.
> Все сеттеры возвращают `true` при успехе и `false`, если компонент / шейп / группа не найдены. Изменения применяются к рантайм-фильтру Box2D **и** записываются в соответствующие сериализованные поля компонента (`CollisionGroupIndex` / `CollisionEnabled`).

```lua
-- Box коллайдер
SetBoxColliderGroup(2)                          -- по индексу группы
SetBoxColliderGroup(2, 1)                       -- второй Box коллайдер
SetBoxColliderGroupByName("Player")             -- по имени группы
SetBoxColliderGroupByName("Enemy", 0)
local gi   = GetBoxColliderGroup()              -- → int (−1 если нет)
local name = GetBoxColliderGroupName()          -- → string
SetBoxColliderCollisionEnabled(2)               -- 0..3 (см. значения ниже)
local mode = GetBoxColliderCollisionEnabled()   -- → int

-- Sphere коллайдер
SetSphereColliderGroup(2)
SetSphereColliderGroupByName("Player")
local gi   = GetSphereColliderGroup()
local name = GetSphereColliderGroupName()
SetSphereColliderCollisionEnabled(2)
local mode = GetSphereColliderCollisionEnabled()

-- Capsule коллайдер
SetCapsuleColliderGroup(2)
SetCapsuleColliderGroupByName("Player")
local gi   = GetCapsuleColliderGroup()
local name = GetCapsuleColliderGroupName()
SetCapsuleColliderCollisionEnabled(2)
local mode = GetCapsuleColliderCollisionEnabled()
```

> Режимы `CollisionEnabled`:
> - `0` — **NoCollision** (maskBits = 0; нет контактных / сенсорных / hit-событий)
> - `1` — **QueryOnly** (принудительно sensor; только sensor-события)
> - `2` — **PhysicsOnly** (твёрдое тело; contact + hit события, без sensor-событий)
> - `3` — **QueryAndPhysics** (твёрдое тело; все события включены — по умолчанию)

---

## 15. Traces — Трассировка (Raycast и Shape Sweep)

> **Тип:** Entity-bound / Level. Требует физический мир (Box2D).

### LineTrace — Луч (Raycast)

> **Фильтрация:** собственное тело кастера пропускается всегда, а при `ignoreSensors = true` (по умолчанию) пропускаются и сенсоры. Пропущенные шейпы **не** блокируют луч — он идёт дальше и возвращает ближайший шейп, прошедший фильтр. Поэтому сенсор перед стеной не мешает увидеть стену. То же правило действует для `LineTraceDirection`, `LineTraceAtCursor`, `LineTraceMulti` и всех sweep-функций (`CircleTrace`, `BoxTrace`, `CapsuleTrace`, …).

```lua
-- Бросить луч
local hit = LineTrace(startX, startY, endX, endY)
local hit = LineTrace(startX, startY, endX, endY, true)        -- ignoreSensors
local hit = LineTrace(startX, startY, endX, endY, true, true)  -- debugDraw
local hit = LineTrace(startX, startY, endX, endY, true, true, 2.0)  -- debugDuration

-- Луч по направлению
local hit = LineTraceDirection(startX, startY, dirX, dirY, distance, true)

-- Луч с фильтром по тегу (tag также поддерживает префикс)
-- Семантика линии видимости: тег должен быть у ближайшего шейпа, иначе попадания нет.
-- Собственное тело кастера пропускается; всё остальное (стены, сенсоры) перекрывает луч.
local hit = LineTraceTag(startX, startY, endX, endY, "Enemy", true, 0.5)

-- Результат:
-- hit.hit       = true/false
-- hit.x, hit.y  = точка попадания (мировые координаты)
-- hit.normalX, hit.normalY = нормаль поверхности
-- hit.fraction   = доля пути (0..1)
-- hit.distance   = расстояние до точки
-- hit.entityId   = ID сущности (или nil)
-- hit.tag         = тег сущности
-- hit.isSensor    = является ли коллайдер сенсором

-- Пример: проверка линии зрения
local target = FindEntityByTag("Player")
if target then
    local tpos = GetEntityPosition(target)
    local myPos = GetPosition()
    local hit = LineTrace(myPos.x, myPos.y, tpos.x, tpos.y, true)
    if hit.hit and hit.tag == "Player" then
        -- Видим игрока!
        canSeePlayer = true
    end
end
```

### CircleTrace — Круговая трассировка

> В отличие от `LineTrace` (луч), `CircleTrace` «протягивает» круг вдоль пути и находит первое столкновение.

```lua
-- Протянуть круг радиусом 16 от точки A до точки B
local hit = CircleTrace(startX, startY, endX, endY, 16)
local hit = CircleTrace(startX, startY, endX, endY, 16, true)         -- ignoreSensors
local hit = CircleTrace(startX, startY, endX, endY, 16, true, true)   -- debugDraw
local hit = CircleTrace(startX, startY, endX, endY, 16, true, true, 2.0) -- debugDuration

-- Результат аналогичен LineTrace:
-- hit.hit, hit.x, hit.y, hit.normalX, hit.normalY, hit.fraction,
-- hit.distance, hit.entityId, hit.tag, hit.isSensor
```

### BoxTrace — Прямоугольная трассировка

```lua
-- Протянуть бокс halfW×halfH от точки A до точки B
local hit = BoxTrace(startX, startY, endX, endY, 20, 10)
local hit = BoxTrace(startX, startY, endX, endY, 20, 10, true, true, 2.0)
-- startX, startY, endX, endY, halfWidth, halfHeight, ignoreSensors?, debugDraw?, debugDuration?

-- Результат аналогичен LineTrace
```

### LineTraceMulti — Несколько попаданий

```lua
local hits = LineTraceMulti(startX, startY, endX, endY, 10, true, true, 0.5)
for _, hit in ipairs(hits) do
    Print(hit.entityId)
end
```

### OverlapBox / OverlapCircle — пересечения по области

Обе принимают тот же необязательный «хвост» `debugDraw` / `debugDuration`, что и все остальные trace- и overlap-функции. Цвет рамки: зелёный при попадании, красный если пусто.

```lua
local entities = OverlapBox(cx, cy, halfW, halfH, true)
local entities = OverlapCircle(cx, cy, radius, true)

local entities = OverlapBox(cx, cy, halfW, halfH, true, true, 0)
-- cx, cy, halfW, halfH, ignoreSensors?, debugDraw?, debugDuration?

local entities = OverlapCircle(cx, cy, radius, true, true, 0)
-- cx, cy, radius, ignoreSensors?, debugDraw?, debugDuration?
```

### OverlapBoxDebug / OverlapCircleDebug — overlap с визуализацией

> Оставлены ради обратной совместимости. `OverlapBoxDebug` полностью эквивалентен `OverlapBox` с теми же аргументами; в новом коде используйте `OverlapBox` / `OverlapCircle`. `OverlapCircleDebug` по-прежнему отличается одним: он выполняет **точную** проверку через `b2World_OverlapShape`, тогда как `OverlapCircle` использует широкую фазу по AABB плюс проверку расстояния до центра. Цвет рамки: зелёный при попадании, красный если пусто.

```lua
local entities = OverlapBoxDebug(cx, cy, halfW, halfH, true, true, 2.0)
-- cx, cy, halfW, halfH, ignoreSensors?, debugDraw?, debugDuration?

local entities = OverlapCircleDebug(cx, cy, radius, true, true, 2.0)
-- cx, cy, radius, ignoreSensors?, debugDraw?, debugDuration?
```

### OverlapCapsule — пересечение капсулой

> Находит все сущности внутри капсулы, заданной двумя точками-центрами полусфер (`A`, `B`) и радиусом. Точная проверка через `b2World_OverlapShape`.

```lua
local entities = OverlapCapsule(ax, ay, bx, by, radius, true, true, 2.0)
-- ax, ay, bx, by, radius, ignoreSensors?, debugDraw?, debugDuration?
```

### OverlapBoxRotated — пересечение поворотным OBB

> В отличие от `OverlapBox` (AABB), эта функция задаёт **ориентированный** прямоугольник (OBB) с углом поворота `angleRad` (радианы, против часовой стрелки). Точная проверка через `b2World_OverlapShape`.

```lua
local entities = OverlapBoxRotated(cx, cy, halfW, halfH, angleRad, true, true, 2.0)
-- cx, cy, halfW, halfH, angleRad, ignoreSensors?, debugDraw?, debugDuration?
```

### CircleTraceMulti / BoxTraceMulti — мульти-hit shape sweeps

> Собирают **все** пересечения вдоль пути (как `LineTraceMulti`, но для круга / бокса). Результаты уже отсортированы по `fraction`, а дубликаты по одному и тому же телу устранены (оставляется ближайший хит).

```lua
local hits = CircleTraceMulti(startX, startY, endX, endY, radius, 10, true, true, 0.5)
-- startX, startY, endX, endY, radius, maxHits?, ignoreSensors?, debugDraw?, debugDuration?

local hits = BoxTraceMulti(startX, startY, endX, endY, halfW, halfH, 10, true, true, 0.5)
-- startX, startY, endX, endY, halfW, halfH, maxHits?, ignoreSensors?, debugDraw?, debugDuration?

for _, h in ipairs(hits) do
    Print(h.entityId, h.fraction)
end
```

### CapsuleTrace / CapsuleTraceMulti — трассировка капсулой

> «Протягивает» капсулу из стартовой позы (`sAx,sAy` → `sBx,sBy`) в конечную (`eAx,eAy` → `eBx,eBy`). `CapsuleTrace` возвращает первое попадание, `CapsuleTraceMulti` — все.

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

### BoxTraceRotated / BoxTraceRotatedMulti — трассировка OBB

> Протягивает ориентированный прямоугольник (OBB) с заданным углом поворота `angleRad` из `(startX,startY)` в `(endX,endY)`. Угол фиксированный на протяжении всего пути.

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

> **Важно:** все trace- и overlap-функции уважают текущий `CollisionGroups.WithLayerMask(...)`, `selfBody` (автоматически исключают коллайдеры собственной сущности) и `ignoreSensors`. Поля возвращаемых структур хита те же, что у `LineTrace`.

### Трассировка по курсору и экрану

Удобные функции, автоматически конвертирующие позицию экрана/курсора в мировые координаты.
Используют мышь, тач или курсор геймпада — что активно.

```lua
-- Overlap в позиции курсора (мышь или тач)
local ids = OverlapAtCursor()                    -- radius=4, ignoreSensors=true
local ids = OverlapAtCursor(8.0, false)          -- свой радиус, включая сенсоры
for _, entityId in ipairs(ids) do
    Print(entityId)
end

-- Overlap в произвольной точке экрана
local ids = OverlapAtScreenPoint(400, 300)       -- screenX, screenY
local ids = OverlapAtScreenPoint(400, 300, 8.0, false)

-- Все сущности в экранном прямоугольнике
local ids = GetEntitiesInScreenRect(100, 100, 500, 400)  -- sx1, sy1, sx2, sy2
local ids = GetEntitiesInScreenRect(100, 100, 500, 400, false)  -- включая сенсоры

-- Raycast в позиции курсора (вертикальный луч по мировой точке)
local hit = LineTraceAtCursor()                  -- radius=1, ignoreSensors=true
local hit = LineTraceAtCursor(2.0, true, true, 1.0)  -- radius, ignoreSensors, debugDraw, debugDuration
-- Результат: аналогичен LineTrace (hit.hit, hit.x, hit.y, hit.entityId, hit.tag и т.д.)
```

### DrawFilledRect / DrawSelectionRect

```lua
-- Заполненный прямоугольник (мировые координаты)
DrawFilledRect(x, y, w, h)                           -- по умолчанию: зелёный, alpha 0.3
DrawFilledRect(x, y, w, h, 1, 0, 0, 0.5, 2.0)       -- r, g, b, a, duration

-- Прямоугольник выделения с рамкой и полупрозрачной заливкой
DrawSelectionRect(x1, y1, x2, y2)                    -- по умолчанию: зелёный, fillAlpha 0.15
DrawSelectionRect(x1, y1, x2, y2, 0, 0, 1, 0.2, 1.0)  -- r, g, b, fillAlpha, duration
```

### Пример: Область атаки ближнего боя

```lua
function MeleeAttack()
    local pos = GetPosition()
    local fwd = GetForwardVector()
    local endX = pos.x + fwd.x * 80
    local endY = pos.y + fwd.y * 80

    -- Широкая атака прямоугольником
    local hit = BoxTrace(pos.x, pos.y, endX, endY, 30, 15, true, true, 0.5)
    if hit.hit and hit.tag == "Enemy" then
        CallInterface(hit.entityId, "TakeDamage", 25)
    end
end
```

---

## 16. Time — Время и таймеры

> **Тип:** Глобальные функции

### Дельта-время

```lua
-- Время с прошлого кадра (учитывает TimeScale)
local dt = GetDeltaTime()

-- Общее игровое время
local t = GetTime()

-- Реальное время (не зависит от паузы)
local rt = GetRealTime()

-- Дельта без масштаба
local udt = GetUnscaledDeltaTime()
```

### Масштаб времени (Slow-Mo, Пауза)

```lua
-- Замедление
SetTimeScale(0.5)   -- Замедлить в 2 раза
SetTimeScale(2.0)   -- Ускорить в 2 раза
ResetTimeScale()    -- Вернуть к 1.0
local ts = GetTimeScale()

-- Пауза
PauseGame()                 -- SetTimeScale(0)
ResumeGame()                -- SetTimeScale(1)
ResumeGame(0.5)             -- SetTimeScale(0.5)
local paused = IsPaused()
```

### Индивидуальный масштаб времени сущности

```lua
SetEntityTimeScale(entityId, 0.5)
local ets = GetEntityTimeScale(entityId)
local edt = GetEntityDeltaTime(entityId)  -- dt * entityTimeScale
```

### FPS

```lua
local fps = GetFPS()
local frameTime = GetFrameTime()  -- В миллисекундах
```

### Таймеры (простые утилиты)

```lua
-- Проверка: прошло ли время?
local startTime = GetTime()
-- ...позже:
if TimerElapsed(startTime, 3.0) then
    -- Прошло 3 секунды
end

-- Оставшееся время
local remaining = TimerRemaining(startTime, 3.0)

-- Прогресс (0..1)
local progress = TimerProgress(startTime, 3.0)
```

### Delay и SetInterval (продвинутые таймеры)

```lua
-- Вызвать функцию через N секунд
local timerId = Delay(2.0, function()
    Print("Прошло 2 секунды!")
end)

-- Повторять каждые N секунд
local intervalId = SetInterval(1.0, function()
    Print("Каждую секунду")
end)

-- Управление таймерами
CancelTimer(timerId)
CancelAllTimers()
local active = IsTimerActive(timerId)
local count = GetActiveTimerCount()
```

### RetriggerableDelay — Перезапускаемая задержка

```lua
-- Обычный Delay — каждый вызов создаёт новый таймер
-- RetriggerableDelay — сбрасывает таймер если вызван повторно с тем же ключом

-- Пример: показать UI hint на 3 секунды, каждый раз обновляя таймер
RetriggerableDelay("hide_hint", 3.0, function()
    SetWidgetVisible(false)
end)

-- Если вызвать снова до истечения 3с — таймер сбросится
RetriggerableDelay("hide_hint", 3.0, function()
    SetWidgetVisible(false)
end)

-- Пример: дебаунс (debounce) ввода
function OnUpdate(dt)
    if IsKeyPressed("e") then
        RetriggerableDelay("interact_cooldown", 0.5, function()
            Interact()
        end)
    end
end
```

---

## 17. Tween — Плавные анимации значений

> **Тип:** Глобальные функции
>
> Tween — плавное изменение числа от A до B за определённое время. Идеально для UI-анимаций, перемещений, эффектов.

### Простой Tween

```lua
-- Простой: от 0 до 100 за 1 секунду
local id = Tween(0, 100, 1.0, function(value, t)
    -- value = текущее значение (0..100)
    -- t = прогресс с easing (0..1)
    SetSpriteAlpha(value / 100)
end, "outQuad")  -- Тип easing (необязательно)
```

### Расширенный TweenEx

```lua
local id = TweenEx({
    from = 0,
    to = 255,
    duration = 2.0,
    delay = 0.5,               -- Задержка перед стартом
    easing = "outElastic",     -- Тип сглаживания
    loops = 3,                 -- Количество повторений (0 = бесконечно)
    yoyo = true,               -- Туда-обратно
    onUpdate = function(value, t)
        SetSpriteAlpha(value / 255)
    end,
    onComplete = function()
        Print("Tween завершён!")
    end
})
```

### Управление

```lua
StopTween(id)
PauseTween(id)
ResumeTween(id)
local running = IsTweenRunning(id)
StopAllTweens()
local count = GetActiveTweenCount()
```

### Применить easing к значению вручную

```lua
local easedT = Ease(0.5, "outBounce")
```

### Последовательность Tween-ов

```lua
local id = TweenSequence({
    { from = 0, to = 1, duration = 0.5, easing = "inQuad",
      onUpdate = function(v) SetSpriteAlpha(v) end },
    { from = 1, to = 0, duration = 0.5, easing = "outQuad",
      onUpdate = function(v) SetSpriteAlpha(v) end }
})
```

### Доступные типы Easing

| Категория | Виды |
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

> `in` = медленный старт, `out` = медленный финиш, `inOut` = оба

### Timeline — Мультитрек-таймлайн с Keyframes

> Timeline позволяет одновременно интерполировать несколько значений по ключевым кадрам с разным easing.

```lua
local tl = Timeline({
    duration = 3.0,          -- Общая длительность (секунды)
    loops = 0,               -- 0 = бесконечно, 1 = один раз
    yoyo = true,             -- Проиграть туда-обратно
    playRate = 1.0,          -- Скорость воспроизведения
    autoPlay = true,         -- Начать сразу

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
        Print("Таймлайн завершён!")
    end,
    onLoop = function(loopNum)
        Print("Цикл #" .. loopNum)
    end
})

-- В OnUpdate обязательно вызывать Update:
function OnUpdate(dt)
    tl.Update(dt)
end

-- Управление:
tl.Play()
tl.Pause()
tl.Stop()
tl.Reverse()               -- Развернуть направление
tl.SetPlayRate(2.0)         -- Ускорить в 2 раза
local rate = tl.GetPlayRate()   -- текущая скорость воспроизведения
tl.SetTime(1.5)             -- Перемотать на 1.5с
local p = tl.GetProgress()  -- 0..1
local e = tl.GetElapsed()
local d = tl.GetDuration()
local playing = tl.IsPlaying()
local done = tl.IsFinished()
```

---

## 18. Coroutine — Корутины

> **Тип:** Глобальные функции
>
> Корутины позволяют писать **последовательную** логику, которая растягивается на несколько кадров.

```lua
-- Запустить корутину
local id = StartCoroutine(function()
    Print("Старт!")

    -- Подождать 2 секунды
    coroutine.yield(WaitSeconds(2.0))
    Print("Прошло 2 секунды!")

    -- Подождать 3 кадра
    coroutine.yield(WaitFrames(3))
    Print("Прошло 3 кадра!")

    -- Ждать условие
    coroutine.yield(WaitUntil(function()
        return IsKeyJustPressed("space")
    end))
    Print("Нажали Space!")
end, "myCoroutine")  -- Имя (необязательно)

-- Управление
StopCoroutine(id)
StopAllCoroutines()
local running = IsCoroutineRunning(id)
local count = GetCoroutineCount()
```

### Пример: Диалог

```lua
StartCoroutine(function()
    SetWidgetText("dialog", "Привет, путник!")
    SetWidgetVisible(true)
    coroutine.yield(WaitSeconds(2.0))

    SetWidgetText("dialog", "Тебя ждёт опасное приключение...")
    coroutine.yield(WaitSeconds(2.0))

    SetWidgetText("dialog", "Удачи!")
    coroutine.yield(WaitSeconds(1.5))

    SetWidgetVisible(false)
end)
```

---

## 19. FSM — Конечный автомат состояний

> **Тип:** Глобальные функции
>
> FSM (Finite State Machine) — паттерн для управления состояниями ИИ, игрока и т.д.

```lua
local sm = StateMachine({
    initial = "Idle",  -- Начальное состояние
    states = {
        Idle = {
            onEnter = function(args)
                Print("Вошёл в Idle")
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
                Print("Вышел из Idle")
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

### Методы FSM

```lua
sm:SetState("Run")                  -- Переключить состояние
sm:SetState("Run", { speed = 200 }) -- С аргументами (передаются в onEnter/onExit)
local state = sm:GetState()         -- Текущее состояние (строка)
local prev = sm:GetPreviousState()  -- Предыдущее
local is = sm:IsState("Idle")       -- Проверка
local has = sm:HasState("Fly")      -- Существует ли состояние?

-- Динамическое добавление/удаление
sm:AddState("Fly", { onEnter = ..., onUpdate = ..., onExit = ... })
sm:RemoveState("Fly")

-- Данные FSM
sm:SetData("jumpCount", 0)
local jc = sm:GetData("jumpCount")
```

---

## 20. Scene — Сцены, сохранения, файлы

> **Тип:** Глобальные функции

### Управление сценами

```lua
-- Загрузить уровень
LoadLevel("Content/Maps/Level2.icemap")

-- Перезагрузить текущий
ReloadLevel()

-- Путь текущего уровня
local path = GetCurrentLevel()

-- Выйти из игры
QuitGame()
local quit = IsQuitRequested()
```

### Приостановка приложения (пауза всего и вся)

Замораживает update, render и аудио на всех 6 платформах (Windows, Linux, macOS, iOS, Android, Web). Удобно для меню паузы при сворачивании окна, ухода в фон на мобилках или когда нужно вручную поставить всё приложение на паузу из скрипта.

```lua
-- Приостановить приложение вручную (звук выключается, update/render пропускается)
SuspendApp()

-- Возобновить после ручной приостановки
ResumeApp()

-- Узнать, активна ли сейчас ручная приостановка
local suspended = IsAppSuspended()
```

> **Примечание:** Движок также автоматически приостанавливается при сворачивании/скрытии окна и при уходе в фон на мобильных платформах; Lua API накладывается поверх этого — это отдельный флаг `RequestManualSuspend`, независимый от состояния окна, управляемого ОС. Эту автоматическую приостановку можно отключить через `Settings.SetIsSuspended(false)` (см. раздел "Приостановка (Suspend)" в Settings) — ручное API отсюда продолжает работать в любом случае.

### Глобальное состояние игры (Game State)

Данные, которые **сохраняются между уровнями** (пока не закрыта игра):

```lua
-- Простые типы
SetGameInt("score", 100)
local score = GetGameInt("score", 0)       -- 0 = значение по умолчанию
AddToGameInt("score", 10)                  -- score += 10

SetGameFloat("time", 99.5)
local t = GetGameFloat("time", 0.0)
AddToGameFloat("time", -1.0)

SetGameString("playerName", "Hero")
local name = GetGameString("playerName", "Unknown")

SetGameBool("hasKey", true)
local key = GetGameBool("hasKey", false)

-- Векторы
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

-- Очистить всё
ClearGameState()
```

### Снапшот состояния сцены (сейв-стейты, чекпоинты, роллбэк)

Game state выше хранит значения, которые вы записали руками. **Снапшот состояния сцены** — другое:
он снимает живую симуляцию всех сущностей в один бинарный блоб, нативно в C++ — трансформ,
тело Box2D (позиция, поворот, линейная и угловая скорость, флаги awake и enabled), аниматор
(состояние, кадр, переход и все параметры вместе с журналом триггеров), скелет (анимация, время,
блендинг, флаг рагдолла **и физическое тело каждой кости рагдолла**), а также проигрывание флипбука.

```lua
local state = SaveSceneState()      -- Бинарный блоб как строка Lua; пустая строка при ошибке
-- ... играем дальше ...
LoadSceneState(state)               -- Восстанавливает всё; возвращает true при успехе
```

Сущности адресуются по UUID, поэтому снапшот восстанавливается корректно независимо от порядка
создания, а отсутствующие при загрузке сущности пропускаются, не ломая остальное.

Именно это делает детерминированный роллбэк-нетокод практичным: `Rollback` снимает этот снапшот на
**каждом** сохраняемом кадре, так что сериализовать физику руками из Lua не нужно никогда.

`Rollback.OnSaveState` / `Rollback.OnLoadState` теперь **дополняют**, а не заменяют — используйте их
только для состояния, которого движок не видит, например собственных таблиц Lua. Возвращённая строка
дописывается к нативному снапшоту и в том же виде приходит обратно в `OnLoadState`; физика, аниматор
и скелет в любом случае остаются заботой движка.

```lua
Rollback.OnSaveState(function(frame) return SaveTableToString(myCombatState) end)
Rollback.OnLoadState(function(extra, frame) myCombatState = LoadTableFromString(extra) end)
```

### Таблицы (сложные данные)

```lua
-- Сохранить таблицу в состояние
SaveTable("inventory", { "Sword", "Shield", gold = 500 })

-- Загрузить
local inv = LoadTable("inventory")
if inv then
    Print("Gold: " .. inv.gold)
end

-- Проверить / удалить
local has = HasSaveTable("inventory")
RemoveSaveTable("inventory")
```

### Сохранение на диск

```lua
-- Сохранить всё состояние в файл (в папку Saves)
SaveGameState()                    -- savegame.json
SaveGameState("slot1.json")

-- Загрузить
LoadGameState()
LoadGameState("slot1.json")
```

### Работа с файлами

```lua
-- Запись (в папку сохранений)
WriteFile("notes.txt", "Hello World!")

-- Чтение
local content = ReadFile("notes.txt")  -- nil если не существует

-- Проверить существование
local exists = SaveFileExists("notes.txt")

-- Удалить
DeleteSaveFile("notes.txt")

-- Список файлов
local files = GetSaveFiles()           -- → таблица имён
local files = GetSaveFiles("saves/")   -- В подпапке

-- Метаданные файла (размер + время изменения, удобно для UI слотов сохранений)
local info = GetSaveFileInfo("slot1.json")
--   → { name = "slot1.json", size = 1024, mtime = 1714816800 }
--   mtime — unix-таймстамп в секундах; пустая таблица если файла нет

local infos = GetSaveFilesInfo()           -- вся папка saves
local infos = GetSaveFilesInfo("slots/")   -- подпапка
-- → { { name=..., size=..., mtime=... }, ... }
for _, e in ipairs(infos) do
    Print(e.name .. "  " .. e.size .. " байт  ts=" .. e.mtime)
end
```

### Мир и FX

```lua
-- FX
SpawnFXAtPosition("Content/FX/Explosion.ice_fx", 100, 200)
SpawnFXAtPosition("Content/FX/Explosion.ice_fx", 100, 200, 10)
SetFXTimeScale(0.5)
local fxScale = GetFXTimeScale()

-- Параметры физического мира
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

-- Фиксированный шаг физики (размер шага симуляции)
SetFixedTimestep(1/60)               -- по умолчанию 1/60 ≈ 0.0167 сек
local step = GetFixedTimestep()       -- → 0.01667

-- Пиксели на метр (только чтение, задаётся в Settings/World Override)
local ppm = GetPPM()                  -- → 100.0 (по умолчанию)

-- Количество под-шагов (итерации солвера за один шаг физики)
local sub = GetSubStepCount()         -- → 4 (по умолчанию)
SetSubStepCount(8)                    -- диапазон 1..16
```

### Глобальные физические запросы

Это **глобальные** (уровневые) функции запросов, работающие без привязки к конкретной сущности.
Для entity-bound трассировок см. [Раздел 15 — Traces](#15-traces--трассировка-raycast-и-shape-sweep).

```lua
-- Raycast — луч, находит ближайшее попадание
-- Все координаты в пикселях; конвертация PPM автоматическая
local hit = Raycast(originX, originY, dirX, dirY)          -- maxDist = 1000
local hit = Raycast(originX, originY, dirX, dirY, 500)     -- свой maxDist

-- Результат:
-- hit.hit        = true / false
-- hit.x, hit.y   = точка попадания (мировые пиксели)
-- hit.normalX, hit.normalY = нормаль поверхности
-- hit.fraction    = доля пути (0..1)
-- hit.entityId    = ID сущности (или nil)
-- hit.tag         = тег сущности (или nil)
-- hit.isSensor    = является ли shape сенсором

-- OverlapCircle — найти все сущности в круге
local entities = OverlapCircle(cx, cy, radius)
for _, e in ipairs(entities) do
    Print(e.entityId .. " tag=" .. (e.tag or "") .. " sensor=" .. tostring(e.isSensor))
end

-- OverlapBox — найти все сущности в осе-выровненном прямоугольнике
local entities = OverlapBox(cx, cy, halfW, halfH)
for _, e in ipairs(entities) do
    Print(e.entityId .. " tag=" .. (e.tag or ""))
end
```

> **Примечание:** Entity-bound `OverlapBox(cx, cy, halfW, halfH, ignoreSensors)` и
> `OverlapCircle(cx, cy, radius, ignoreSensors)` в скриптах сущностей (Раздел 15)
> имеют дополнительный параметр `ignoreSensors` и возвращают более полный результат.
> Глобальные версии выше — облегчённые и доступны в скриптах уровня.

### Асинхронная загрузка уровней

Загрузка уровня в фоне, пока текущая сцена продолжает отрисовываться.
Идеально для экранов загрузки с реальным прогрессом.

```lua
-- Запустить фоновую загрузку (сцена продолжает работать)
LoadLevelAsync("Content/Maps/Level2.icemap")

-- Запустить с авто-применением (сменит сразу когда готов)
LoadLevelAsync("Content/Maps/Level2.icemap", true)

-- В OnUpdate / OnLevelUpdate — опрашивать прогресс:
function OnLevelUpdate(dt)
    if IsAsyncLoading() then
        local progress = GetAsyncLoadProgress()   -- 0.0 до 1.0
        local status   = GetAsyncLoadStatus()     -- "Loading assets (3/7)..."
        local phase    = GetAsyncLoadPhase()       -- 0=Idle,1=Parsing,2=Preloading,3=Ready,4=Complete,5=Failed

        -- Обновляем UI загрузочного экрана
        SetWidgetProgress("LoadBar", progress)
        SetWidgetText("StatusText", status)
    end

    if IsAsyncLoadReady() then
        -- Все ассеты предзагружены, применяем уровень
        ApplyAsyncLevel()
    end

    if IsAsyncLoadFailed() then
        Print("Ошибка загрузки: " .. GetAsyncLoadError())
    end
end

-- Отменить загрузку
CancelAsyncLoad()
```

| Функция | Возврат | Описание |
|---------|---------|----------|
| `LoadLevelAsync(path)` | — | Запустить фоновую загрузку уровня |
| `LoadLevelAsync(path, true)` | — | Запустить с авто-применением |
| `IsAsyncLoading()` | bool | true пока идёт загрузка |
| `IsAsyncLoadReady()` | bool | true когда всё предзагружено |
| `IsAsyncLoadFailed()` | bool | true при ошибке |
| `GetAsyncLoadProgress()` | float | Прогресс 0.0–1.0 |
| `GetAsyncLoadStatus()` | string | Человекочитаемый статус |
| `GetAsyncLoadPhase()` | int | 0=Idle, 1=Parsing, 2=Preloading, 3=Ready, 4=Complete, 5=Failed |
| `GetAsyncLoadError()` | string | Текст ошибки (при неудаче) |
| `ApplyAsyncLevel()` | — | Применить предзагруженный уровень |
| `CancelAsyncLoad()` | — | Отменить загрузку |

---

## 21. Widget — UI виджеты

> **Тип:** Entity-bound. Требует компонент **WidgetComponent**.
> Виджеты создаются в редакторе `.ice_widget` и привязываются к сущности.

`.ice_widget` — это иерархический UI-ассет: виджет может содержать **дочерние элементы** и выступать **вложенным элементом** в другом виджете. Это позволяет строить составные интерфейсы из переиспользуемых частей.

### Управление видимостью

```lua
SetWidgetVisible(true)
SetWidgetVisible(false, 1)     -- Второй виджет
local vis = IsWidgetVisible()
ToggleWidget()                 -- Переключить
ShowAllWidgets()
HideAllWidgets()
```

### Свойства

```lua
-- Масштаб
SetWidgetScale(2.0)
local scale = GetWidgetScale()

-- Позиция
SetWidgetPosition(100, 50)
local pos = GetWidgetPosition()  -- → {x, y}

-- Порядок рендеринга: виджеты сортируются глобально по RenderOrder (больше = выше).
-- При равном RenderOrder world-space виджеты всегда рендерятся ПОД
-- screen-space виджетами — внутримировой UI остаётся с миром, экранный UI
-- перекрывает его. Чтобы явно поднять виджет выше, задайте больший RenderOrder.
SetWidgetRenderOrder(10)

-- Интерактивность
SetWidgetInteractable(true)
local interactable = IsWidgetInteractable()

-- Индекс игрока (видимость в сплит-скрине, для каждого экземпляра в WidgetComponent)
-- -1 = общий для всех игроков (HUD), 0..3 = виден только в области обзора этого локального игрока
SetWidgetPlayerIndex(-1)         -- первый экземпляр (индекс 0)
SetWidgetPlayerIndex(1, 2)       -- третий экземпляр (индекс 2), только для игрока 1
local pi = GetWidgetPlayerIndex()      -- первый экземпляр
local pi2 = GetWidgetPlayerIndex(1)    -- второй экземпляр

-- Screen Space (привязка к экрану, а не к миру)
SetWidgetScreenSpace(true)

-- Рендеринг в игре (видимость в режиме игры)
SetWidgetRenderInGame(true)
SetWidgetRenderInGame(false, 1)    -- Второй виджет
local render = GetWidgetRenderInGame()  -- → bool (по умолчанию true)

-- Количество виджетов
local count = GetWidgetCount()
```

### Работа с элементами виджета

```lua
-- Текст
SetWidgetText("ScoreLabel", "Очки: 100")
local text = GetWidgetText("ScoreLabel")

-- Прогресс-бар (0..1)
SetWidgetProgress("HealthBar", 0.75)
local progress = GetWidgetProgress("HealthBar")

-- Видимость элемента
SetWidgetElementVisible("Hint", true)
local vis = IsWidgetElementVisible("Hint")

-- Локализация
SetWidgetLocalizationKey("Title", "menu_title")
local key = GetWidgetLocalizationKey("Title")

-- Изображение элемента
SetWidgetElementImage("Icon", "Content/Textures/sword.png")
local img = GetWidgetElementImage("Icon")

-- Lit/Unlit (участие элемента в системе освещения и теней; по умолчанию Unlit)
SetWidgetElementLit("Background", true)
local lit = GetWidgetElementLit("Background")

-- Приёмник теней (для каждого элемента; по умолчанию ВЫКЛ). Когда ВКЛ — динамические
-- тени падают на этот Lit-элемент. Работает и в мировом, и в экранном пространстве.
SetWidgetElementShadowReceiver("Background", true)
local recv = GetWidgetElementShadowReceiver("Background")

-- Размытие фона панели (матовое стекло; имеет значение только для элементов панели,
-- по умолчанию ВЫКЛ). Уменьшите значение альфа-канала цвета панели, чтобы отобразить размытый фон.
SetWidgetElementPanelBlur("GlassPanel", true)
local blurOn = GetWidgetElementPanelBlur("GlassPanel")
SetWidgetElementPanelBlurStrength("GlassPanel", 16.0)   -- радиус размытия в пикселях холста
local blurStrength = GetWidgetElementPanelBlurStrength("GlassPanel")

-- Флипбук (анимированное изображение)
SetWidgetElementFlipbook("Anim", "Content/Flipbooks/fire.ice_flipbook")
local fb = GetWidgetElementFlipbook("Anim")
SetWidgetFlipbookFrame("Anim", 3)
local frame = GetWidgetFlipbookFrame("Anim")

-- Цвет элемента
SetWidgetElementColor("Background", 0.2, 0.2, 0.2, 0.8)

-- Слайдер
local val = GetSliderValue("Volume")
SetSliderValue("Volume", 0.5)

-- Checkbox (переключатель)
local on = IsCheckboxChecked("Fullscreen")
SetCheckboxChecked("Fullscreen", true)

-- Dropdown (выпадающий список)
local sel = GetDropdownSelected("Language")
local text = GetDropdownSelectedText("Language")
SetDropdownSelected("Language", 2)

-- InputField (поле ввода)
local input = GetInputText("PlayerName")
SetInputText("PlayerName", "Hero")
```

### Дополнительные свойства элементов

```lua
-- Тип элемента
local type = GetElementType("Title")

-- Позиция и размер (set и get)
SetWidgetElementPosition("Title", 10, 20)
local pos = GetWidgetElementPosition("Title")   -- → {x, y}
SetWidgetElementSize("Title", 200, 40)
local size = GetWidgetElementSize("Title")       -- → {x, y}

-- Желаемый размер (подгонка под содержимое). Пока включён, элемент сам
-- подстраивает размер под содержимое (текст, спрайт/флипбук, дочерние элементы,
-- поток бокса), а поле Size становится только для чтения.
SetWidgetElementUseDesiredSize("Title", true)
local uses = GetWidgetElementUseDesiredSize("Title")
local desired = GetWidgetElementDesiredSize("Title")   -- → {x, y} (вычисляется даже когда выключено)

-- Цвет элемента (set и get)
SetWidgetElementColor("Background", 0.2, 0.2, 0.2, 0.8)
local color = GetWidgetElementColor("Background")  -- → {r, g, b, a}

-- Прозрачность
SetWidgetElementOpacity("Title", 0.75)
local opacity = GetWidgetElementOpacity("Title")

-- Масштаб и поворот (set и get)
SetWidgetElementScale("Icon", 1.2, 1.2)
local scale = GetWidgetElementScale("Icon")      -- → {x, y}
SetWidgetElementRotation("Icon", 15)
local rot = GetWidgetElementRotation("Icon")     -- → float

-- Pivot
SetWidgetElementPivot("Icon", 0.5, 0.5)
local pivot = GetWidgetElementPivot("Icon")  -- → {x, y}

-- Якорь (Anchor)
SetWidgetElementAnchor("Title", "MiddleCenter")
local anchor = GetWidgetElementAnchor("Title")  -- → строка
-- Доступные значения: "TopLeft", "TopCenter", "TopRight",
-- "MiddleLeft", "MiddleCenter", "MiddleRight",
-- "BottomLeft", "BottomCenter", "BottomRight",
-- "StretchLeft", "StretchCenter", "StretchRight",
-- "StretchTop", "StretchMiddle", "StretchBottom", "StretchAll"

-- Взаимодействие
SetWidgetElementInteractable("Button", true)
local interactable = IsWidgetElementInteractable("Button")

-- Z-порядок
SetWidgetElementZOrder("Panel", 5)
local z = GetWidgetElementZOrder("Panel")

-- Глобальный Z: когда true, ZOrder этого элемента участвует в глобальной Z-сортировке
-- виджетов (между виджетами и даже сценой энтити). При ZOrder < 0 элемент рендерится
-- ДО сущностей сцены (тайлмапов/спрайтов/флипбуков/FX) — удобно для фона HUD, который
-- должен быть позади геймплея. При ZOrder >= 0 элемент рендерится вместе с другими
-- виджетами, сортируясь глобально по ZOrder против RenderOrder виджетов.
-- По умолчанию false (элемент сортируется только внутри своего виджета).
-- При равных значениях действует правило пространств: в оверлей-пассе (ZOrder >= 0)
-- world-space записи рендерятся под screen-space; в фоновом пассе (ZOrder < 0)
-- screen-space задники рендерятся первыми (дальше всех), поэтому world-space
-- задники оказываются ближе к слою геймплея.
SetWidgetElementGlobalZ("Background", true)
local isGlobal = GetWidgetElementGlobalZ("Background")  -- → bool

-- Участие в пост-обработке: когда true, элемент рисуется внутри сцены и получает
-- пост-обработку (bloom, экспозиция, глубина резкости, цветокоррекция и т.д.).
-- Когда false (по умолчанию) элемент рисуется как оверлей ПОСЛЕ цепочки
-- пост-обработки, оставаясь чётким и незатронутым — правильный выбор для иконок,
-- полос и текста HUD. Фоновые элементы (GlobalZ с ZOrder < 0) всегда являются
-- частью сцены и игнорируют этот флаг.
SetWidgetElementIsPostProcessed("BackgroundSky", true)
local isPP = GetWidgetElementIsPostProcessed("BackgroundSky")  -- → bool

-- Шрифт и цвет текста
SetWidgetElementFontSize("Title", 32)
local size = GetWidgetElementFontSize("Title")
SetWidgetElementTextColor("Title", 1, 1, 1, 1)
local color = GetWidgetElementTextColor("Title")  -- → {r, g, b, a}

-- Тултип
SetWidgetElementTooltip("Button", "Нажмите для продолжения")
local tooltip = GetWidgetElementTooltip("Button")

-- Цвет заполнения (ProgressBar/Slider)
SetFillColor("HealthBar", 0.8, 0.1, 0.1, 1)

-- Состояния кнопки (Hovered/Pressed)
-- Первый аргумент после имени элемента — флаг включения. Если false (по умолчанию
-- у нового элемента), элементы НЕ меняют цвет автоматически на hover/press. Включи в
-- true, чтобы использовать встроенные цвета, или управляй сменой цвета сам из
-- коллбэков OnHover/OnUnhover/OnPressed/OnReleased.
SetWidgetStateColors("Button", true, 1, 0.8, 0.8, 1, 0.8, 0.2, 0.2, 1)

-- Звуки состояний кнопки (Hovered/Pressed)
-- Первый аргумент после имени элемента — флаг включения. Если false (по умолчанию
-- у нового элемента), элементы НЕ воспроизводят звуки автоматически на hover/press.
-- Включи в true, чтобы использовать встроенные звуки.
SetWidgetStateSounds("Button", true, "Content/Audio/hover.wav", "Content/Audio/click.wav")

-- Перенос текста (Text, Button)
SetWidgetTextWrap("Description", true)
local wrap = IsWidgetTextWrap("Description")

-- Пользовательские якоря (растяжение/компоновка)
SetWidgetCustomAnchors("Panel", 0, 0, 1, 1)    -- minX, minY, maxX, maxY
ClearWidgetCustomAnchors("Panel")

-- Спрайт галочки чекбокса
SetWidgetElementCheckedSprite("Toggle", "Content/Textures/checked.png")
```

### Анимации виджетов

```lua
PlayWidgetAnimation("Intro")
PauseWidgetAnimation("Intro")
ResumeWidgetAnimation("Intro")
StopWidgetAnimation("Intro", true)

local playing = IsWidgetAnimationPlaying("Intro")
SetWidgetAnimationTime("Intro", 0.5)
local t = GetWidgetAnimationTime("Intro")

SetAnimationCompleteCallback("Intro", "OnIntroFinished")

-- Событие анимации — вызвать callback в определённый момент времени
AddAnimationEvent("Intro", 0.5, "OnHalfway")               -- animName, time, callback
AddAnimationEvent("Intro", 1.0, "OnFinish", "myParam")     -- с необязательным параметром
```

### Локальные трансформации и инстансы

```lua
SetWidgetLocalPosition(10, 20)
local lp = GetWidgetLocalPosition()

SetWidgetLocalScale(1.5, 1.5)
local ls = GetWidgetLocalScale()

SetWidgetLocalRotation(15)
local lr = GetWidgetLocalRotation()

-- Мировая трансформация (трансформ сущности уже учтён — см. раздел Sprite)
SetWidgetWorldPosition(120, 64, 0)
local wwp = GetWidgetWorldPosition(0)      -- → {x, y, z}
SetWidgetWorldRotation(30, 0)
local wwr = GetWidgetWorldRotation(0)      -- → число
local wws = GetWidgetWorldScale(0)         -- → {x, y}, только чтение

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

### Дополнительные настройки UI

```lua
-- Максимальная длина текста (InputField)
SetWidgetMaxLength("NameInput", 16)
local maxLen = GetWidgetMaxLength("NameInput")

-- Отступы и spacing (HorizontalBox/VerticalBox)
SetWidgetSpacing("Row", 8)
local spacing = GetWidgetSpacing("Row")

SetWidgetPadding("Row", 4, 4, 4, 4)

-- Safe area (вырезы мобильных / скруглённые углы)
SetWidgetSafeArea(true)                              -- включить
SetWidgetSafeArea(true, 10, 20, 10, 20)              -- включить, left, top, right, bottom
SetWidgetSafeArea(false)                             -- выключить

-- Прокрутка перетаскиванием (ScrollView)
SetWidgetDragScroll("Inventory", true)

-- Смещение прокрутки (ScrollView)
SetWidgetScrollOffset("Inventory", 0, 100)           -- x, y
local offset = GetWidgetScrollOffset("Inventory")    -- → {x, y}
```

### Callbacks элементов

```lua
-- Базовое взаимодействие
SetWidgetCallback("PlayButton", "OnClick", "OnPlayClicked")
local cb = GetWidgetCallback("PlayButton", "OnClick")

-- Наведение (когда курсор входит в элемент)
SetWidgetCallback("PlayButton", "OnHover", "OnPlayHover")
-- Уход курсора (когда курсор покидает элемент) — срабатывает один раз при потере hover
SetWidgetCallback("PlayButton", "OnUnhover", "OnPlayUnhover")

-- Нажатие / отпускание кнопки мыши на элементе
-- OnPressed срабатывает в момент нажатия кнопки мыши на этом элементе
-- OnReleased срабатывает при отпускании — даже если отпустили вне элемента
SetWidgetCallback("PlayButton", "OnPressed", "OnPlayPressed")
SetWidgetCallback("PlayButton", "OnReleased", "OnPlayReleased")

-- События значений / фокуса
SetWidgetCallback("NameInput", "OnValueChanged", "OnNameChanged")
SetWidgetCallback("NameInput", "OnFocusGained", "OnNameFocus")
SetWidgetCallback("NameInput", "OnFocusLost", "OnNameBlur")
```

> **Важно.** По умолчанию элементы НЕ меняют цвет автоматически на hover/press. Чтобы использовать встроенные цвета состояний, включи `UseCustomStateColors` через `SetWidgetStateColors(...)`. Иначе управляй сменой цвета сам из коллбэков `OnHover` / `OnUnhover` / `OnPressed` / `OnReleased` через `SetWidgetElementColor`.

### Throbber (спиннер)

```lua
SetThrobberClockwise("Loading", true)
local cw = IsThrobberClockwise("Loading")

SetThrobberSpeed("Loading", 2.0)
local speed = GetThrobberSpeed("Loading")

-- Пауза / возобновление анимации
SetThrobberPaused("Loading", true)
local paused = IsThrobberPaused("Loading")
PauseThrobber("Loading")
ResumeThrobber("Loading")

-- Сброс времени к нулю (вернуть к первой точке)
ResetThrobber("Loading")
```

### Toggle и WidgetSwitcher

```lua
SetToggleState("MusicToggle", true)
local on = GetToggleState("MusicToggle")

SetSwitcherActiveChild("Tabs", 1)
local idx = GetSwitcherActiveChild("Tabs")
```

### Динамические инстансы виджета

```lua
local idx = AddWidgetInstance("Content/UI/HUD.ice_widget", true)
RemoveWidgetInstance(idx)
```

### Динамические элементы

```lua
-- typeName: "Text", "Button", "Image", "ProgressBar", "Slider", "InputField",
-- "Checkbox", "Dropdown", "ScrollView", "HorizontalBox", "VerticalBox",
-- "SizeBox", "Overlay", "Throbber", "WidgetSwitcher", "Spacer", "Toggle"
local elemId = AddWidgetElement("Text", "HintText", "Root")
RemoveWidgetElement("HintText")
```

### Иерархия элементов виджета

```lua
-- Проверить существование элемента
local exists = HasWidgetElement("Title")

-- Дочерние элементы
local children = GetWidgetElementChildren("Panel")   -- → таблица имён
-- Пример: {"Label", "Button", "Icon"}

-- Корневые элементы виджета
local roots = GetWidgetRootElements()                 -- → таблица имён

-- Родитель элемента
local parentName = GetWidgetElementParent("Button")   -- → строка или ""

-- Переместить элемент к другому родителю
MoveWidgetElement("Button", "NewPanel")               -- Переместить в "NewPanel"
MoveWidgetElement("Button", "")                       -- Переместить в корень
```

### Dropdown (расширенное управление)

```lua
-- Установить/получить список опций
SetDropdownOptions("Language", {"English", "Русский", "日本語"})
local options = GetDropdownOptions("Language")   -- → таблица строк
local count = GetDropdownOptionCount("Language") -- → число
```

### Свойства инстанса виджета

```lua
-- Имя инстанса
local name = GetWidgetInstanceName(0)

-- Путь к .ice_widget (можно менять в рантайме)
local path = GetWidgetInstancePath(0)
SetWidgetInstancePath("Content/UI/NewHUD.ice_widget", 0)

-- Порядок рендеринга
local order = GetWidgetRenderOrder(0)
```

### Расширенные свойства элементов (entity-bound)

Полный набор геттеров/сеттеров на элемент для исчерпывающего контроля из скриптов. Все принимают необязательный последний `instanceIndex` (по умолчанию `0`).

```lua
-- Выравнивание текста (Text / Button / InputField)
SetWidgetElementTextAlign("Title", "Center")    -- "Left" | "Center" | "Right"
local ha = GetWidgetElementTextAlign("Title")
SetWidgetElementTextVAlign("Title", "Middle")    -- "Top" | "Middle" | "Bottom"
local va = GetWidgetElementTextVAlign("Title")

-- Шрифт (путь к .ice_font / шрифтовому ассету; "" = шрифт по умолчанию)
SetWidgetElementFont("Title", "Content/Fonts/Title.ice_font")
local font = GetWidgetElementFont("Title")

-- Произвольное значение (сырое CurrentValue, без ограничения)
SetWidgetElementValue("Bar", 42)
local v = GetWidgetElementValue("Bar")

-- Диапазон значений (Slider / ProgressBar) — CurrentValue переограничивается в диапазон
SetWidgetValueRange("Volume", 0, 100)
local minV = GetWidgetMinValue("Volume")
local maxV = GetWidgetMaxValue("Volume")

-- Геттер цвета заливки (ProgressBar / Slider) — сеттер это SetFillColor
local fill = GetFillColor("HealthBar")           -- → {r, g, b, a}

-- Nine-slice (растягиваемые границы спрайта)
SetWidgetNineSlice("Panel", true, 12, 12, 12, 12)   -- enable, left, top, right, bottom
local ns = GetWidgetNineSlice("Panel")           -- → {enabled, left, top, right, bottom}

-- Обрезка дочерних по прямоугольнику элемента
SetWidgetClipChildren("Panel", true)
local clip = GetWidgetClipChildren("Panel")

-- Соседи навигации геймпада для элемента (имена элементов; "" / nil сбрасывает)
SetWidgetNavigation("PlayBtn", "TitleLabel", "OptionsBtn", nil, nil)   -- up, down, left, right
local nav = GetWidgetNavigation("PlayBtn")       -- → {up, down, left, right} (имена)

-- Геометрия Throbber
SetThrobberRadius("Loading", 30)
local rad = GetThrobberRadius("Loading")
SetThrobberDots("Loading", 12)
local dots = GetThrobberDots("Loading")

-- Цвета состояний Hover / Pressed (индивидуальный доступ + геттеры)
SetWidgetUseStateColors("Button", true)
local useState = GetWidgetUseStateColors("Button")
SetWidgetHoveredColor("Button", 1, 0.8, 0.8, 1)
local hov = GetWidgetHoveredColor("Button")      -- → {r, g, b, a}
SetWidgetPressedColor("Button", 0.8, 0.2, 0.2, 1)
local prs = GetWidgetPressedColor("Button")      -- → {r, g, b, a}

-- Звуки состояний Hover / Pressed (индивидуальный доступ + геттеры)
SetWidgetUseStateSounds("Button", true)
local useSnd = GetWidgetUseStateSounds("Button")
SetWidgetHoveredSound("Button", "Content/Audio/hover.wav")
local hovSnd = GetWidgetHoveredSound("Button")   -- → строка-путь
SetWidgetPressedSound("Button", "Content/Audio/click.wav")
local prsSnd = GetWidgetPressedSound("Button")   -- → строка-путь

-- Геттеры для ранее «только-сеттер» свойств
local pad = GetWidgetPadding("Row")              -- → {left, top, right, bottom}
local drag = GetWidgetDragScroll("Inventory")    -- → bool
local checkSprite = GetWidgetElementCheckedSprite("Toggle")
local anchors = GetWidgetCustomAnchors("Panel")  -- → {enabled, minX, minY, maxX, maxY}

-- Задержка тултипа (секунды до появления)
SetWidgetElementTooltipDelay("Button", 0.5)
local td = GetWidgetElementTooltipDelay("Button")

-- Макс. высота списка Dropdown (px)
SetWidgetDropdownMaxHeight("Language", 240)
local dh = GetWidgetDropdownMaxHeight("Language")

-- Полосы прокрутки ScrollView + размер содержимого
SetWidgetScrollbars("Inventory", true, false)    -- showVertical, showHorizontal
local sb = GetWidgetScrollbars("Inventory")      -- → {vertical, horizontal}
SetWidgetContentSize("Inventory", 300, 1200)
local cs = GetWidgetContentSize("Inventory")     -- → {x, y}

-- Принудительные размеры SizeBox
SetWidgetSizeOverride("Box", true, true, 200, 120)  -- overrideW, overrideH, width, height
local so = GetWidgetSizeOverride("Box")          -- → {overrideWidth, overrideHeight, width, height}

-- Арт ползунка Slider
SetWidgetSliderThumbImage("Volume", "Content/UI/knob.png")
local thumb = GetWidgetSliderThumbImage("Volume")
SetWidgetSliderThumbFlipbook("Volume", "Content/UI/knob.ice_flipbook")
local thumbFb = GetWidgetSliderThumbFlipbook("Volume")

-- Цвета / ручка Toggle
SetWidgetToggleColors("MusicToggle", 0.3, 0.7, 0.4, 1, 0.5, 0.5, 0.55, 1)  -- on rgba, off rgba
local onCol = GetWidgetToggleOnColor("MusicToggle")    -- → {r, g, b, a}
local offCol = GetWidgetToggleOffColor("MusicToggle")  -- → {r, g, b, a}
SetWidgetToggleHandleRatio("MusicToggle", 0.45)
local hr = GetWidgetToggleHandleRatio("MusicToggle")
SetWidgetToggleHandleImage("MusicToggle", "Content/UI/handle.png")
local hImg = GetWidgetToggleHandleImage("MusicToggle")
```

### Холст, масштабирование и безопасная зона виджета (entity-bound)

```lua
-- Дизайн-размер холста инстанса виджета
SetWidgetCanvasSize(1920, 1080)
local canvas = GetWidgetCanvasSize()             -- → {x, y}

-- Желаемый размер для самого холста: холст обтягивает корневые элементы,
-- а Canvas Size становится только для чтения (пересчитывается каждый кадр).
SetWidgetCanvasUseDesiredSize(true)
local canvasAuto = GetWidgetCanvasUseDesiredSize()
local canvasDesired = GetWidgetCanvasDesiredSize()  -- → {x, y} (вычисляется даже когда выключено)

-- Масштабировать содержимое виджета с экраном (true) или сохранять размер в пикселях (false)
SetWidgetScaleWithScreen(true)
local sws = GetWidgetScaleWithScreen()

-- Прочитать конфигурацию безопасной зоны (сеттер — SetWidgetSafeArea)
local sa = GetWidgetSafeArea()                   -- → {enabled, left, top, right, bottom}
```

### Переупорядочивание и переименование элементов (entity-bound)

```lua
-- Сдвинуть элемент вверх/вниз среди соседей (влияет на порядок отрисовки внутри родителя)
ReorderWidgetElementUp("Icon")
ReorderWidgetElementDown("Icon")

-- Переупорядочить относительно опорного соседа
MoveWidgetElementBefore("Icon", "Label")
MoveWidgetElementAfter("Icon", "Label")

-- Переименовать элемент. Возвращает фактически применённое (уникальное) имя.
local applied = SetWidgetElementName("OldName", "NewName")
```

### SubWidget — вложенные виджеты (аналог ClassComponent для UI)

SubWidget — элемент типа `SubWidget`, который ссылается на другой `.ice_widget`.
Это позволяет строить составные интерфейсы из переиспользуемых частей,
аналогично тому, как ClassComponent работает для `.ice_class`.

```lua
-- Путь к вложенному виджету
local path = GetSubWidgetPath("HealthBar")
SetSubWidgetPath("HealthBar", "Content/UI/NewHealthBar.ice_widget")

-- Работа с элементами внутри SubWidget
-- Первый параметр — имя SubWidget-элемента, второй — имя элемента внутри вложенного виджета

-- Текст
SetSubWidgetText("HealthBar", "Label", "100 HP")
local text = GetSubWidgetText("HealthBar", "Label")

-- Видимость элемента
SetSubWidgetElementVisible("HealthBar", "Warning", true)
local vis = IsSubWidgetElementVisible("HealthBar", "Warning")

-- Цвет
SetSubWidgetElementColor("HealthBar", "Fill", 1, 0, 0, 1)
local color = GetSubWidgetElementColor("HealthBar", "Fill")  -- → {r, g, b, a}

-- Позиция и размер
SetSubWidgetElementPosition("HealthBar", "Icon", 10, 5)
local pos = GetSubWidgetElementPosition("HealthBar", "Icon")  -- → {x, y}
SetSubWidgetElementSize("HealthBar", "Icon", 32, 32)
local sz = GetSubWidgetElementSize("HealthBar", "Icon")       -- → {x, y}

-- Желаемый размер для элемента вложенного виджета
SetSubWidgetElementUseDesiredSize("HealthBar", "Icon", true)
local subUses = GetSubWidgetElementUseDesiredSize("HealthBar", "Icon")
local subDesired = GetSubWidgetElementDesiredSize("HealthBar", "Icon")  -- → {x, y}

-- Прогресс-бар
SetSubWidgetProgress("HealthBar", "Bar", 0.75)
local progress = GetSubWidgetProgress("HealthBar", "Bar")

-- Изображение
SetSubWidgetElementImage("HealthBar", "Icon", "Content/Textures/heart.png")

-- Прозрачность
SetSubWidgetElementOpacity("HealthBar", "Label", 0.5)
local opacity = GetSubWidgetElementOpacity("HealthBar", "Label")

-- Поворот (в градусах)
SetSubWidgetElementRotation("HealthBar", "Icon", 45)
local rot = GetSubWidgetElementRotation("HealthBar", "Icon")

-- Масштаб
SetSubWidgetElementScale("HealthBar", "Icon", 1.5, 1.5)
local scale = GetSubWidgetElementScale("HealthBar", "Icon")  -- → {x, y}

-- Точка опоры (пивот)
SetSubWidgetElementPivot("HealthBar", "Icon", 0.5, 0.5)
local pivot = GetSubWidgetElementPivot("HealthBar", "Icon")  -- → {x, y}

-- Интерактивность
SetSubWidgetElementInteractable("Menu", "Button", false)

-- Callback
SetSubWidgetCallback("Menu", "PlayButton", "OnClick", "OnPlayClicked")
-- callbackType: "OnClick", "OnHover", "OnValueChanged", "OnFocusGained", "OnFocusLost"
```

#### SubWidget — расширенный API

Все функции ниже принимают **необязательный последний аргумент `instanceIndex`** (по умолчанию `0`) — индекс `WidgetInstance` на сущности, семантика совпадает с остальными `GetWidget*`/`SetWidget*`. Первый аргумент всегда — имя **SubWidget-элемента** во внешнем виджете; второй — имя **внутреннего элемента** во вложенном `.ice_widget`.

```lua
-- ── Структура / интроспекция ──────────────────────────────
HasSubWidgetElement("HealthBar", "Label")                    -- → bool
GetSubWidgetElementType("HealthBar", "Label")                -- → "Text" | "Button" | "Image" | ...
GetSubWidgetRootElements("HealthBar")                        -- → { "Root1", "Root2", ... }
GetSubWidgetElementChildren("HealthBar", "Panel")            -- → { "Child1", "Child2", ... }
GetSubWidgetElementParent("HealthBar", "Icon")               -- → имя родителя или ""

-- ── Свойства текста ───────────────────────────────────────
SetSubWidgetElementText("HealthBar", "Label", "100 HP")
local t   = GetSubWidgetElementText("HealthBar", "Label")    -- → string
SetSubWidgetElementFontSize("HealthBar", "Label", 18)
local fs  = GetSubWidgetElementFontSize("HealthBar", "Label")-- → number
SetSubWidgetElementTextColor("HealthBar", "Label", 1, 1, 1, 1)
local tc  = GetSubWidgetElementTextColor("HealthBar", "Label") -- → {r, g, b, a}

-- ── Image / Flipbook ──────────────────────────────────────
SetSubWidgetElementFlipbook("HealthBar", "Fx", "Content/FX/spin.ice_flipbook")
local fb  = GetSubWidgetElementFlipbook("HealthBar", "Fx")   -- → string
local img = GetSubWidgetElementImage("HealthBar", "Icon")    -- → string (путь к спрайту)

-- ── Макет ─────────────────────────────────────────────────
SetSubWidgetElementZOrder("HealthBar", "Icon", 5)
local z   = GetSubWidgetElementZOrder("HealthBar", "Icon")   -- → int
-- GlobalZ для внутреннего элемента саб-виджета: семантика та же, что
-- у SetWidgetElementGlobalZ — когда true, ZOrder элемента используется как
-- глобальный Z-ключ сортировки (отрицательный ZOrder рендерится позади сцены).
SetSubWidgetElementGlobalZ("HealthBar", "Background", true)
local sg  = GetSubWidgetElementGlobalZ("HealthBar", "Background") -- → bool
-- Примечание: вложенный виджет рендерится как единое целое — проход определяется
-- флагом IsPostProcessed самого элемента SubWidget в виджете-хосте.
-- Флаги внутренних элементов применяются, когда этот ассет используется отдельно.
SetSubWidgetElementIsPostProcessed("HealthBar", "Background", true)
local spp = GetSubWidgetElementIsPostProcessed("HealthBar", "Background") -- → bool
SetSubWidgetElementTooltip("HealthBar", "Icon", "Здоровье")
local tip = GetSubWidgetElementTooltip("HealthBar", "Icon")  -- → string
SetSubWidgetElementAnchor("HealthBar", "Icon", "TopLeft")
local a   = GetSubWidgetElementAnchor("HealthBar", "Icon")   -- → имя якоря

-- ── Интерактивные элементы ────────────────────────────────
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
GetSubWidgetDropdownSelected("Settings", "Quality")          -- → int (с нуля)

SetSubWidgetSwitcherActiveChild("Tabs", "Pages", 1)
GetSubWidgetSwitcherActiveChild("Tabs", "Pages")             -- → int

-- ── Анимации (во вложенном виджете) ───────────────────────
PlaySubWidgetAnimation("HealthBar", "Pulse")                 -- запуск по имени
StopSubWidgetAnimation("HealthBar", "Pulse")
PauseSubWidgetAnimation("HealthBar", "Pulse")
ResumeSubWidgetAnimation("HealthBar", "Pulse")
IsSubWidgetAnimationPlaying("HealthBar", "Pulse")            -- → bool

-- Необязательный индекс экземпляра (если на сущности несколько WidgetInstance):
SetSubWidgetElementText("HealthBar", "Label", "0/100", 1)
PlaySubWidgetAnimation("HealthBar", "Pulse", 1)
```

> **Примечание.** Расширенный API работает рекурсивно — корректно разрешает внутренние элементы через любую глубину вложенных SubWidget. Обращение к ещё не загруженному виджету вызывает `LoadWidget` по требованию, после чего анимации и flipbook вложенного виджета тикают автоматически каждый кадр вместе с остальным UI.

#### SubWidget — полный набор свойств внутренних элементов

Полный набор свойств для внутренних элементов, зеркально повторяющий API элементов верхнего уровня. Первый аргумент — имя элемента SubWidget, второй — имя внутреннего элемента; необязательный последний `instanceIndex`.

```lua
-- Освещение и блюр панели
SetSubWidgetElementLit("HealthBar", "Bg", true);             GetSubWidgetElementLit("HealthBar", "Bg")
SetSubWidgetElementShadowReceiver("HealthBar", "Bg", true);  GetSubWidgetElementShadowReceiver("HealthBar", "Bg")
SetSubWidgetElementPanelBlur("HealthBar", "Glass", true);    GetSubWidgetElementPanelBlur("HealthBar", "Glass")
SetSubWidgetElementPanelBlurStrength("HealthBar", "Glass", 16.0); GetSubWidgetElementPanelBlurStrength("HealthBar", "Glass")

-- Выравнивание текста / шрифт / перенос / локализация
SetSubWidgetElementTextAlign("HealthBar", "Label", "Center"); GetSubWidgetElementTextAlign("HealthBar", "Label")
SetSubWidgetElementTextVAlign("HealthBar", "Label", "Middle");GetSubWidgetElementTextVAlign("HealthBar", "Label")
SetSubWidgetElementFont("HealthBar", "Label", "Content/Fonts/F.ice_font"); GetSubWidgetElementFont("HealthBar", "Label")
SetSubWidgetElementTextWrap("HealthBar", "Label", true);     IsSubWidgetElementTextWrap("HealthBar", "Label")
SetSubWidgetElementLocalizationKey("HealthBar", "Label", "hp_text"); GetSubWidgetElementLocalizationKey("HealthBar", "Label")
SetSubWidgetElementMaxLength("Form", "Name", 16);            GetSubWidgetElementMaxLength("Form", "Name")

-- Значения, диапазон, заливка, содержимое
SetSubWidgetElementValue("HealthBar", "Bar", 42);            GetSubWidgetElementValue("HealthBar", "Bar")
SetSubWidgetElementValueRange("HealthBar", "Bar", 0, 100)
GetSubWidgetElementMinValue("HealthBar", "Bar");             GetSubWidgetElementMaxValue("HealthBar", "Bar")
SetSubWidgetElementFillColor("HealthBar", "Bar", 0.8, 0.1, 0.1, 1); GetSubWidgetElementFillColor("HealthBar", "Bar")
SetSubWidgetElementContentSize("Menu", "Scroll", 300, 1200); GetSubWidgetElementContentSize("Menu", "Scroll")

-- Цвета состояний / интерактивность / чтение callback
SetSubWidgetElementUseStateColors("Menu", "Btn", true);      GetSubWidgetElementUseStateColors("Menu", "Btn")
SetSubWidgetElementHoveredColor("Menu", "Btn", 1, 0.9, 0.6, 1)
SetSubWidgetElementPressedColor("Menu", "Btn", 0.7, 0.5, 0.3, 1)
SetSubWidgetElementUseStateSounds("Menu", "Btn", true);      GetSubWidgetElementUseStateSounds("Menu", "Btn")
SetSubWidgetElementHoveredSound("Menu", "Btn", "Content/Audio/hover.wav")
SetSubWidgetElementPressedSound("Menu", "Btn", "Content/Audio/click.wav")
IsSubWidgetElementInteractable("Menu", "Btn")
GetSubWidgetCallback("Menu", "Btn", "OnClick")

-- Раскладка: spacing / padding / nine-slice / clip / anchors
SetSubWidgetElementSpacing("Menu", "Row", 8);                GetSubWidgetElementSpacing("Menu", "Row")
SetSubWidgetElementPadding("Menu", "Row", 4, 4, 4, 4);       GetSubWidgetElementPadding("Menu", "Row")
SetSubWidgetElementNineSlice("Menu", "Panel", true, 12, 12, 12, 12)
SetSubWidgetElementClipChildren("Menu", "Panel", true);      GetSubWidgetElementClipChildren("Menu", "Panel")
SetSubWidgetElementCustomAnchors("Menu", "Panel", 0, 0, 1, 1); ClearSubWidgetElementCustomAnchors("Menu", "Panel")

-- ScrollView / SizeBox / ползунок Slider
SetSubWidgetElementScrollOffset("Menu", "Scroll", 0, 100);   GetSubWidgetElementScrollOffset("Menu", "Scroll")
SetSubWidgetElementDragScroll("Menu", "Scroll", true)
SetSubWidgetElementSizeOverride("Menu", "Box", true, true, 200, 120)
SetSubWidgetElementSliderThumbImage("Settings", "Vol", "Content/UI/knob.png")
SetSubWidgetElementSliderThumbFlipbook("Settings", "Vol", "Content/UI/knob.ice_flipbook")

-- Цвета / ручка Toggle, спрайт чекбокса, задержка тултипа, высота dropdown
SetSubWidgetElementToggleColors("Menu", "Music", 0.3,0.7,0.4,1, 0.5,0.5,0.55,1)
SetSubWidgetElementToggleHandleRatio("Menu", "Music", 0.45)
SetSubWidgetElementToggleHandleImage("Menu", "Music", "Content/UI/handle.png")
SetSubWidgetElementCheckedSprite("Menu", "Sound", "Content/UI/checked.png")
SetSubWidgetElementTooltipDelay("Menu", "Btn", 0.5);        GetSubWidgetElementTooltipDelay("Menu", "Btn")
SetSubWidgetElementDropdownMaxHeight("Settings", "Quality", 240)
GetSubWidgetDropdownSelectedText("Settings", "Quality");    GetSubWidgetDropdownOptionCount("Settings", "Quality")

-- Управление Throbber
SetSubWidgetThrobberSpeed("HUD", "Spinner", 2.0);           GetSubWidgetThrobberSpeed("HUD", "Spinner")
SetSubWidgetThrobberClockwise("HUD", "Spinner", true)
SetSubWidgetThrobberRadius("HUD", "Spinner", 30)
SetSubWidgetThrobberDots("HUD", "Spinner", 12)
SetSubWidgetThrobberPaused("HUD", "Spinner", true);         IsSubWidgetThrobberPaused("HUD", "Spinner")

-- Навигация и кадр flipbook для внутреннего элемента
SetSubWidgetElementNavigation("Menu", "PlayBtn", "Title", "OptionsBtn", nil, nil)
SetSubWidgetFlipbookFrame("HUD", "Anim", 3);                GetSubWidgetFlipbookFrame("HUD", "Anim")

-- ── Структурное редактирование (рантайм) ──────────────────
-- Построение/изменение дерева внутренних элементов вложенного виджета в рантайме —
-- SubWidget-аналог AddWidgetElement / RemoveWidgetElement / MoveWidgetElement.
-- typeName — любой тип виджета ("Panel","Text","Button","Image","ProgressBar",
-- "Slider","InputField","Checkbox","Dropdown","ScrollView","HorizontalBox",
-- "VerticalBox","SizeBox","Overlay","Throbber","WidgetSwitcher","Spacer","Toggle","SubWidget").
local id = AddSubWidgetElement("HealthBar", "Text", "NewLabel")            -- → ID нового элемента
AddSubWidgetElement("HealthBar", "Image", "Icon", "Root")                  -- с внутренним родителем
RemoveSubWidgetElement("HealthBar", "NewLabel")
MoveSubWidgetElement("HealthBar", "Icon", "OtherPanel")                    -- сменить родителя ("" = корень)
ReorderSubWidgetElementUp("HealthBar", "Icon")                            -- → bool
ReorderSubWidgetElementDown("HealthBar", "Icon")                         -- → bool
MoveSubWidgetElementBefore("HealthBar", "Icon", "Label")                 -- → bool
MoveSubWidgetElementAfter("HealthBar", "Icon", "Label")                  -- → bool
local unique = SetSubWidgetElementName("HealthBar", "Icon", "HeartIcon") -- → применённое уникальное имя
```

### Фокус и состояние взаимодействия

```lua
-- Установить фокус на элемент
FocusWidgetElement("NameInput")
ClearWidgetFocus()

-- Запрос текущего состояния взаимодействия (глобально)
local focused = GetWidgetFocusedElement()   -- → имя элемента или ""
local hovered = GetWidgetHoveredElement()   -- → имя элемента или ""
local pressed = GetWidgetPressedElement()   -- → имя элемента или ""

-- Проверка состояния для конкретного элемента (привязано к сущности)
local isFocused = IsWidgetElementFocused("NameInput")
local isHovered = IsWidgetElementHovered("PlayButton")
local isPressed = IsWidgetElementPressed("PlayButton")
```

### Навигация геймпадом (настраиваемая)

Виджет-система поддерживает полную навигацию геймпадом: DPad + аналоговый стик
для перемещения фокуса между интерактивными элементами, кнопка Activate для
клика / переключения / открытия дропдауна / фокуса на input field, кнопка
Cancel для сброса фокуса. Слайдеры также реагируют на влево/вправо на DPad и
стике. Все настройки ниже — **глобальные**, они влияют на навигацию по всем
виджетам на сцене.

```lua
-- Включить / выключить навигацию геймпадом полностью
SetGamepadEnabled(true)
local on = IsGamepadEnabled()

-- Перенастроить кнопки действий / направлений.
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

-- Чувствительность
SetGamepadNavCooldown(0.2)            -- секунды между авто-повторами шагов на стике
SetGamepadStickThreshold(0.5)         -- мёртвая зона стика для навигации (0..1)
SetGamepadSliderStickThreshold(0.3)   -- мёртвая зона стика для слайдеров
SetGamepadSliderStepSize(0.05)        -- шаг на одно нажатие DPad на сфокусированном слайдере (нормализованный)
SetGamepadSliderStickSpeed(0.02)      -- скорость в кадр от стика на сфокусированном слайдере

-- Какой стик управляет навигацией
SetGamepadUseLeftStick(true)          -- false = правый стик

-- Рамка фокуса (выделение вокруг сфокусированного элемента)
SetGamepadFocusBorder(true)                                 -- показать / скрыть
SetGamepadFocusBorder(true, 1.0, 0.9, 0.2, 1.0)             -- показать + цвет (r,g,b,a)
SetGamepadFocusBorder(true, 1.0, 0.9, 0.2, 1.0, 3.0)        -- показать + цвет + толщина

-- Программно задать / прочитать сфокусированный элемент
SetGamepadFocusedElement("PlayButton")      -- фокус по имени (виджет текущей сущности)
ClearGamepadFocus()
local name = GetGamepadFocusedElement()     -- → string или ""

-- Режим навигации (видимость рамки фокуса клавиатуры/геймпада)
local navOn = IsUINavigationActive()        -- → bool: true пока показана рамка фокуса
SetUINavigationActive(false)                -- принудительно включить (true) / выключить (false) режим навигации
```

> **Маршрутизация навигации по элементам.** У каждого элемента также есть явные
> `NavUpID` / `NavDownID` / `NavLeftID` / `NavRightID`, задаваемые в редакторе
> или из скрипта виджета через `SetNavigation(...)`. Если они не заданы,
> runtime использует пространственный поиск (ближайший интерактивный элемент
> в нужном направлении).

> **Навигация по SubWidget.** Навигация клавиатурой/геймпадом заходит внутрь
> элементов `SubWidget`: интерактивные элементы саб-виджета (и вложенных в него
> саб-виджетов) входят в тот же набор фокуса, что и собственные элементы
> хост-виджета, а пространственный поиск сравнивает их реальные экранные
> позиции, поэтому фокус естественно входит в саб-виджет и выходит из него.
> Активация, коллбэки hover/unhover, звуки состояний и TTS выполняются в
> скриптовом окружении того виджета, которому принадлежит элемент. Явные связи
> `NavUpID` / `NavDownID` / `NavLeftID` / `NavRightID` остаются локальными для
> виджета — используйте `SetSubElementNavigation(...)` (или
> `SetSubWidgetElementNavigation(...)` из скрипта сущности), чтобы задать
> маршруты внутри саб-виджета, и оставляйте связи незаданными там, где фокус
> должен пересекать границу саб-виджета через пространственный поиск.
>
> Учтите, что все элементы `SubWidget`, ссылающиеся на один и тот же файл
> виджета, разделяют одно рантайм-состояние, поэтому повторение одного
> саб-виджета несколько раз внутри одного хоста даёт одну фокусируемую копию
> каждого его элемента.

**Пример: Составной HUD с переиспользуемой полоской здоровья:**

```lua
-- В виджете HUD.ice_widget есть SubWidget элемент "PlayerHealth",
-- который ссылается на HealthBar.ice_widget.
-- HealthBar.ice_widget содержит: ProgressBar "Bar", Text "Label", Image "Icon"

function OnCreate()
    -- Установить начальное здоровье
    SetSubWidgetProgress("PlayerHealth", "Bar", 1.0)
    SetSubWidgetText("PlayerHealth", "Label", "100/100")
end

function OnUpdate(dt)
    local hp = GetData("health") or 100
    local maxHp = GetData("maxHealth") or 100
    SetSubWidgetProgress("PlayerHealth", "Bar", hp / maxHp)
    SetSubWidgetText("PlayerHealth", "Label", hp .. "/" .. maxHp)

    -- Подсветить красным при низком здоровье
    if hp < 30 then
        SetSubWidgetElementColor("PlayerHealth", "Bar", 1, 0.2, 0.2, 1)
    else
        SetSubWidgetElementColor("PlayerHealth", "Bar", 0.2, 0.8, 0.2, 1)
    end
end
```

### Widget-Internal API (скрипты `.ice_widget`)

Внутри Lua-скриптов `.ice_widget` доступны укороченные имена функций. Они работают с элементами **текущего виджета** по имени, без префикса `Widget` и без индекса экземпляра.

> Это **алиасы** — каждая функция соответствует длинному entity-bound эквиваленту, описанному выше. Используйте их **только внутри скриптов `.ice_widget`**.

#### Свойства элементов (короткие имена)

```lua
-- Трансформация
SetElementPosition("Title", 100, 50)
local pos = GetElementPosition("Title")         -- → {x, y}
SetElementSize("Title", 200, 40)
local sz = GetElementSize("Title")              -- → {width, height}
SetElementUseDesiredSize("Title", true)         -- подгонка под содержимое; Size только для чтения
local usesDesired = GetElementUseDesiredSize("Title")
local desired = GetElementDesiredSize("Title")  -- → {width, height} (вычисляется даже когда выключено)
SetElementRotation("Title", 45)
local rot = GetElementRotation("Title")
SetElementScale("Title", 1.5, 1.5)
local sc = GetElementScale("Title")             -- → {x, y}
SetElementPivot("Title", 0.5, 0.5)
local pv = GetElementPivot("Title")             -- → {x, y}

-- Внешний вид
SetElementColor("Title", 1, 0.5, 0, 1)
local col = GetElementColor("Title")            -- → {r, g, b, a}
SetElementOpacity("Title", 0.8)
local op = GetElementOpacity("Title")
SetElementFontSize("Title", 24)
SetElementVisible("Title", true)
local vis = IsElementVisible("Title")
SetElementLit("Background", true)
local lit = IsElementLit("Background")
SetElementShadowReceiver("Background", true)   -- тени падают на этот Lit-элемент (по умолчанию ВЫКЛ)
local recv = IsElementShadowReceiver("Background")

-- Текст и значения
SetElementText("Title", "Привет")
local txt = GetElementText("Title")
SetElementValue("Slider", 0.5)
local val = GetElementValue("Slider")

-- Интерактивность
SetElementInteractable("Button", true)
local ia = IsElementInteractable("Button")
```

#### Фокус и состояние взаимодействия

```lua
FocusElement("NameInput")
ClearFocus()
local focused = GetFocusedElement()              -- → имя элемента или ""
local hovered = GetHoveredElement()
local pressed = GetPressedElement()
local isFocused = IsElementFocused("NameInput")
local isHovered = IsElementHovered("Button")
local isPressed = IsElementPressed("Button")
```

#### Специфичные типы виджетов

```lua
-- Toggle (переключатель)
SetToggled("SoundToggle", true)
local on = IsToggled("SoundToggle")

-- Switcher (контейнер вкладок)
SetActiveChild("TabPanel", 2)
local idx = GetActiveChild("TabPanel")

-- Dropdown (выпадающий список)
SetDropdownSelected("Language", 1)
local sel = GetDropdownSelected("Language")
local text = GetDropdownSelectedText("Language")

-- Прогресс-бар
SetProgressValue("HealthBar", 0.75)
local pv = GetProgressValue("HealthBar")

-- Throbber (спиннер загрузки)
SetThrobberSpeed("Loader", 2.0)
local spd = GetThrobberSpeed("Loader")
SetThrobberRadius("Loader", 30)
local rad = GetThrobberRadius("Loader")
SetThrobberDots("Loader", 8)
local dots = GetThrobberDots("Loader")
SetThrobberClockwise("Loader", false)
local cw = IsThrobberClockwise("Loader")
-- Пауза / возобновление / сброс
SetThrobberPaused("Loader", true)
local paused = IsThrobberPaused("Loader")
PauseThrobber("Loader")
ResumeThrobber("Loader")
ResetThrobber("Loader")                          -- сбросить время в 0

-- Диапазон значений слайдера (также для ProgressBar)
SetValueRange("Volume", 0, 100)                  -- min, max
local minV = GetMinValue("Volume")
local maxV = GetMaxValue("Volume")

-- Состояние чекбокса
SetChecked("Fullscreen", true)
local on2 = IsChecked("Fullscreen")

-- Текст InputField
SetInputText("PlayerName", "Hero")
local typed = GetInputText("PlayerName")

-- Цвет заливки (ProgressBar / Slider — область заполнения)
SetFillColor("HealthBar", 0.8, 0.1, 0.1, 1)
local fillCol = GetFillColor("HealthBar")        -- → {r, g, b, a}

-- Спрайт галочки чекбокса
SetCheckedSprite("Toggle", "Content/UI/checked.png")

-- Опции дропдауна (массив строк)
SetDropdownOptions("Language", {"English", "Русский", "日本語"})
local opts = GetDropdownOptions("Language")       -- → таблица строк
local optCount = GetDropdownOptionCount("Language")
```

#### Внешний вид и стили элементов (короткие имена)

```lua
-- Спрайт / Flipbook
SetElementImage("Icon", "Content/Textures/sword.png")
local img = GetElementImage("Icon")
SetElementFlipbook("Anim", "Content/Flipbooks/fire.ice_flipbook")
local fb = GetElementFlipbook("Anim")

-- Цвет текста
SetElementTextColor("Title", 1, 1, 1, 1)
local tc = GetElementTextColor("Title")          -- → {r, g, b, a}

-- Z-порядок и глобальный Z
SetElementZOrder("Panel", 5)
local z = GetElementZOrder("Panel")
SetElementGlobalZ("Background", true)            -- участвовать в глобальной Z-сортировке
local gz = GetElementGlobalZ("Background")

-- Участие в пост-обработке (по умолчанию false = чёткий оверлей после пост-обработки)
SetElementIsPostProcessed("BackgroundSky", true)
local pp = GetElementIsPostProcessed("BackgroundSky")

-- Якорный пресет
SetElementAnchor("Title", "MiddleCenter")
local a = GetElementAnchor("Title")
-- Значения: "TopLeft", "TopCenter", "TopRight",
--           "MiddleLeft", "MiddleCenter", "MiddleRight",
--           "BottomLeft", "BottomCenter", "BottomRight",
--           "StretchLeft", "StretchCenter", "StretchRight",
--           "StretchTop", "StretchMiddle", "StretchBottom", "StretchAll"

-- Тултип
SetElementTooltip("Button", "Нажми чтобы продолжить")
SetElementTooltip("Button", "Нажми чтобы продолжить", 0.5)   -- + задержка (секунды)
local tip = GetElementTooltip("Button")

-- Цвета Hovered / Pressed. Без этого элементы НЕ меняют цвет автоматически
-- при наведении/нажатии — управляй переходами сам из коллбэков.
SetStateColors("PlayButton", true,
    1.0, 0.9, 0.6, 1.0,    -- HoveredColor (r,g,b,a)
    0.7, 0.5, 0.3, 1.0)    -- PressedColor (r,g,b,a)
SetUseStateColors("PlayButton", true)
SetHoveredColor("PlayButton", 1.0, 0.9, 0.6)
SetPressedColor("PlayButton", 0.7, 0.5, 0.3)

-- Звуки Hovered / Pressed (та же схема, что и цвета выше)
SetStateSounds("PlayButton", true,
    "Content/Audio/hover.wav",     -- HoveredSound
    "Content/Audio/click.wav")     -- PressedSound
SetUseStateSounds("PlayButton", true)
local useSnd = GetUseStateSounds("PlayButton")   -- → bool
SetHoveredSound("PlayButton", "Content/Audio/hover.wav")
local hovSnd = GetHoveredSound("PlayButton")     -- → строка-путь
SetPressedSound("PlayButton", "Content/Audio/click.wav")
local prsSnd = GetPressedSound("PlayButton")     -- → строка-путь

-- Nine-Slice для растягиваемых рамок спрайтов
SetNineSlice("Panel", true)                                 -- включить
SetNineSlice("Panel", true, 12, 12, 12, 12)                 -- + бордюр (left, top, right, bottom)

-- Обрезка дочерних элементов по прямоугольнику родителя
SetClipChildren("ScrollPanel", true)

-- Путь к SubWidget (вложенному .ice_widget)
SetSubWidgetPath("HealthBar", "Content/UI/NewHealthBar.ice_widget")
local p = GetSubWidgetPath("HealthBar")
```

#### Иерархия и интроспекция (короткие имена)

```lua
-- Проверка существования
local exists = HasElement("Title")

-- Тип элемента строкой ("Panel", "Text", "Button", ..., "SubWidget")
local typeName = GetElementType("Title")

-- Навигация по дереву
local children = GetElementChildren("Panel")     -- → { "Child1", "Child2", ... }
local parentName = GetElementParent("Button")    -- → строка или ""
local roots = GetRootElements()                  -- → { "Root1", "Root2", ... }

-- Перепривязать элемент (пустое имя родителя = переместить в корень)
MoveElement("Button", "OtherPanel")
MoveElement("Button", "")                        -- в корень
```

#### Коллбэки (короткие имена)

```lua
-- Установить коллбэк по типу. callbackType — один из:
-- "OnClick", "OnHover", "OnUnhover", "OnPressed", "OnReleased",
-- "OnValueChanged", "OnFocusGained", "OnFocusLost"
SetCallback("PlayButton", "OnClick",    "OnPlayClicked")
SetCallback("PlayButton", "OnHover",    "OnPlayHover")
SetCallback("PlayButton", "OnUnhover",  "OnPlayUnhover")
SetCallback("PlayButton", "OnPressed",  "OnPlayPressed")
SetCallback("PlayButton", "OnReleased", "OnPlayReleased")

-- Прочитать текущее имя привязанного коллбэка
local cb = GetCallback("PlayButton", "OnClick")
```

#### Навигация геймпадом: ID и настройки (короткие имена)

```lua
-- Установить соседей навигации для элемента (имена; "" или nil очищает)
SetNavigation("PlayBtn",
    "TitleLabel",       -- вверх
    "OptionsBtn",       -- вниз
    nil,                -- влево (не меняется при nil)
    nil)                -- вправо (не меняется при nil)

-- Тот же глобальный конфиг геймпада, что и на стороне сущности — доступен внутри виджет-скриптов
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

SetGamepadFocusBorder(true, 1.0, 0.9, 0.2, 1.0, 2.0)   -- показать, r, g, b, a, толщина

SetGamepadFocusedElement("PlayBtn")
ClearGamepadFocus()
local focused = GetGamepadFocusedElement()             -- → строка или ""

local navOn = IsUINavigationActive()                   -- → bool: режим навигации клавиатуры/геймпада активен
SetUINavigationActive(false)                           -- принудительно вкл (true) / выкл (false) режим навигации
```

#### Доступ к элементам SubWidget (короткие имена)

Это удобные эквиваленты `Get/SetSubWidgetElement*`, доступные внутри скрипта виджета. Первый аргумент — имя **SubWidget элемента** в текущем виджете; второй — имя **внутреннего элемента** во вложенном `.ice_widget`.

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

-- Привязать коллбэк к внутреннему элементу SubWidget
-- callbackType: тот же набор, что и в SetCallback
SetSubElementCallback("Menu", "PlayButton", "OnClick", "OnPlayClicked")
```

#### Раскладка и прокрутка

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

#### Динамические элементы и анимации

```lua
-- Создание/удаление элементов в рантайме
local id = CreateElement("Text", "DynamicLabel", "Root")  -- тип, имя, родитель
RemoveElement("DynamicLabel")

-- Информация об элементе
local info = GetElement("Title")                 -- → {name, visible, text, ...}

-- Анимации
PlayAnimation("FadeIn")
PlayAnimation("FadeIn", 2.0)                     -- со скоростью
StopAnimation("FadeIn")
StopAnimation("FadeIn", true)                    -- сброс в начало
PauseAnimation("FadeIn")
ResumeAnimation("FadeIn")
local playing = IsAnimationPlaying("FadeIn")
AddAnimationEvent("FadeIn", 0.5, "OnHalfway")   -- коллбэк в момент 0.5
AddAnimationEvent("FadeIn", 1.0, "OnDone", "param")
```

#### Расширенные свойства элементов (короткие имена)

```lua
-- Геттер размера шрифта (сеттер — SetElementFontSize)
local fs = GetElementFontSize("Title")

-- Выравнивание текста / шрифт
SetElementTextAlign("Title", "Center");   local ha = GetElementTextAlign("Title")
SetElementTextVAlign("Title", "Middle");  local va = GetElementTextVAlign("Title")
SetElementFont("Title", "Content/Fonts/Title.ice_font"); local f = GetElementFont("Title")

-- Блюр фона панели
SetElementPanelBlur("Glass", true);       local pb = GetElementPanelBlur("Glass")
SetElementPanelBlurStrength("Glass", 16); local pbs = GetElementPanelBlurStrength("Glass")

-- Геттеры для ранее «только-сеттер» свойств
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

-- Задержка тултипа
SetElementTooltipDelay("Btn", 0.5);       local td = GetElementTooltipDelay("Btn")

-- Высота списка Dropdown, полосы прокрутки, размер содержимого
SetDropdownMaxHeight("Lang", 240);        local dh = GetDropdownMaxHeight("Lang")
SetScrollbars("Scroll", true, false);     local sb = GetScrollbars("Scroll")   -- → {vertical, horizontal}
SetContentSize("Scroll", 300, 1200);      local cs = GetContentSize("Scroll")  -- → {x, y}

-- SizeBox / ползунок Slider
SetSizeOverride("Box", true, true, 200, 120); local so = GetSizeOverride("Box")
SetSliderThumbImage("Vol", "Content/UI/knob.png");    local th = GetSliderThumbImage("Vol")
SetSliderThumbFlipbook("Vol", "Content/UI/knob.ice_flipbook"); local tf = GetSliderThumbFlipbook("Vol")

-- Цвета / ручка Toggle
SetToggleColors("Music", 0.3,0.7,0.4,1, 0.5,0.5,0.55,1)
local onc = GetToggleOnColor("Music");    local offc = GetToggleOffColor("Music")
SetToggleHandleRatio("Music", 0.45);      local thr = GetToggleHandleRatio("Music")
SetToggleHandleImage("Music", "Content/UI/handle.png"); local thi = GetToggleHandleImage("Music")

-- Текущий кадр flipbook
SetFlipbookFrame("Anim", 3);              local fr = GetFlipbookFrame("Anim")
```

#### Холст, масштабирование и безопасная зона (короткие имена)

```lua
SetCanvasSize(1920, 1080);     local canvas = GetCanvasSize()       -- → {x, y}
SetCanvasUseDesiredSize(true);     local canvasAuto = GetCanvasUseDesiredSize()
local canvasDesired = GetCanvasDesiredSize()                        -- → {x, y}
SetScaleWithScreen(true);      local sws = GetScaleWithScreen()
SetStretchMode("Letterbox");   local sm = GetStretchMode()          -- "Stretch"|"Letterbox"|"MatchWidth"|"MatchHeight"
SetSafeArea(true, 10, 20, 10, 20); local sa = GetSafeArea()         -- → {enabled, left, top, right, bottom}
```

#### Переупорядочивание и переименование элементов (короткие имена)

```lua
ReorderElementUp("Icon")
ReorderElementDown("Icon")
MoveElementBefore("Icon", "Label")
MoveElementAfter("Icon", "Label")
local applied = SetElementName("OldName", "NewName")   -- возвращает уникализированное имя
```

#### Полный доступ к внутренним элементам SubWidget (короткие имена)

Полный контроль внутренних элементов вложенных SubWidget из `.ice_widget`. Первый аргумент — имя элемента SubWidget, второй — имя внутреннего элемента.

```lua
-- Интроспекция
GetSubElementType("HealthBar", "Label")
GetSubRootElements("HealthBar")
GetSubElementChildren("HealthBar", "Panel")
GetSubElementParent("HealthBar", "Icon")

-- Геттеры/сеттеры трансформа
SetSubElementRotation("HealthBar", "Icon", 45);  GetSubElementRotation("HealthBar", "Icon")
SetSubElementScale("HealthBar", "Icon", 1.5, 1.5); GetSubElementScale("HealthBar", "Icon")
SetSubElementPivot("HealthBar", "Icon", 0.5, 0.5); GetSubElementPivot("HealthBar", "Icon")
SetSubElementOpacity("HealthBar", "Label", 0.5); GetSubElementOpacity("HealthBar", "Label")
GetSubElementPosition("HealthBar", "Icon")       -- → {x, y}
GetSubElementSize("HealthBar", "Icon")           -- → {width, height}
GetSubElementColor("HealthBar", "Icon")          -- → {r, g, b, a}

-- Внешний вид
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

-- Интерактивность и цвета состояний
SetSubElementInteractable("Menu", "Btn", false); IsSubElementInteractable("Menu", "Btn")
GetSubElementCallback("Menu", "Btn", "OnClick")
SetSubElementUseStateColors("Menu", "Btn", true)
SetSubElementHoveredColor("Menu", "Btn", 1,0.9,0.6,1)
SetSubElementPressedColor("Menu", "Btn", 0.7,0.5,0.3,1)
SetSubElementUseStateSounds("Menu", "Btn", true)
SetSubElementHoveredSound("Menu", "Btn", "Content/Audio/hover.wav")
SetSubElementPressedSound("Menu", "Btn", "Content/Audio/click.wav")

-- Значения / заливка / диапазон / прогресс
SetSubElementFillColor("HealthBar", "Bar", 0.8,0.1,0.1,1); GetSubElementFillColor("HealthBar", "Bar")
SetSubElementValueRange("HealthBar", "Bar", 0, 100)
GetSubElementMinValue("HealthBar", "Bar");       GetSubElementMaxValue("HealthBar", "Bar")
SetSubProgressValue("HealthBar", "Bar", 0.75);   GetSubProgressValue("HealthBar", "Bar")

-- Интерактивные контролы
SetSubChecked("Menu", "Sound", true);            IsSubChecked("Menu", "Sound")
SetSubToggled("Menu", "Music", true);            IsSubToggled("Menu", "Music")
SetSubSliderValue("Settings", "Vol", 0.5);       GetSubSliderValue("Settings", "Vol")
SetSubInputText("Form", "Name", "Player1");      GetSubInputText("Form", "Name")
SetSubDropdownOptions("Settings", "Quality", {"Low","Medium","High"})
GetSubDropdownOptions("Settings", "Quality");    SetSubDropdownSelected("Settings", "Quality", 2)
GetSubDropdownSelected("Settings", "Quality");   GetSubDropdownSelectedText("Settings", "Quality")
SetSubSwitcherActiveChild("Tabs", "Pages", 1);   GetSubSwitcherActiveChild("Tabs", "Pages")

-- Раскладка / скролл / sizebox / ползунок / toggle
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

-- Throbber / навигация / кадр flipbook
SetSubThrobberSpeed("HUD", "Spinner", 2.0);      SetSubThrobberClockwise("HUD", "Spinner", true)
SetSubThrobberRadius("HUD", "Spinner", 30);      SetSubThrobberDots("HUD", "Spinner", 12)
SetSubThrobberPaused("HUD", "Spinner", true)
SetSubElementNavigation("Menu", "PlayBtn", "Title", "OptionsBtn", nil, nil)
SetSubFlipbookFrame("HUD", "Anim", 3);           GetSubFlipbookFrame("HUD", "Anim")

-- Анимации вложенного виджета
PlaySubAnimation("HealthBar", "Pulse");          PlaySubAnimation("HealthBar", "Pulse", 2.0)
StopSubAnimation("HealthBar", "Pulse");          PauseSubAnimation("HealthBar", "Pulse")
ResumeSubAnimation("HealthBar", "Pulse");        IsSubAnimationPlaying("HealthBar", "Pulse")
```

**Соответствие коротких имён и entity-bound эквивалентов:**

| Короткое (`.ice_widget`) | Entity-bound (`.ice_class`) |
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
| `SetNavigation` | *(индивидуальные NavUp/Down/Left/Right ID)* |
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
| `SetGamepadEnabled` *(и все хелперы `SetGamepad*`)* | то же имя на стороне сущности |
| `SetSubElementText` | `SetSubWidgetText` |
| `SetSubElementColor` | `SetSubWidgetElementColor` |

---

## 22. PostProcess — Пост-обработка

> **Тип:** Глобальные функции. Таблица `PP` + функции `PP_*`.
> Каждая функция доступна в двух формах: `PP.FunctionName(...)` (метод таблицы) и `PP_FunctionName(...)` (глобальная функция). Они идентичны.

### Общее управление

```lua
PP.SetEnabled(true)
PP_SetEnabled(true)
local enabled = PP.IsEnabled()
local enabled2 = PP_IsEnabled()
PP_Reset()  -- Сбросить все настройки
```

### Bloom (Свечение)

```lua
PP.SetBloomEnabled(true)       -- или PP_SetBloomEnabled(true)
local bloomOn = PP.IsBloomEnabled()
local bloomOn2 = PP_IsBloomEnabled()
PP.SetBloomIntensity(1.5)      -- или PP_SetBloomIntensity(1.5)
local bloomIntensity = PP.GetBloomIntensity()
local bloomIntensity2 = PP_GetBloomIntensity()
PP.SetBloomThreshold(0.8)      -- или PP_SetBloomThreshold(0.8)
local bloomThreshold = PP.GetBloomThreshold()
local bloomThreshold2 = PP_GetBloomThreshold()
PP.SetBloomRadius(5.0)         -- или PP_SetBloomRadius(5.0)
local bloomRadius = PP.GetBloomRadius()
local bloomRadius2 = PP_GetBloomRadius()
```

### Color Grading (Цветокоррекция)

```lua
PP.SetColorGradingEnabled(true) -- или PP_SetColorGradingEnabled(true)
local gradingOn = PP.IsColorGradingEnabled()
local gradingOn2 = PP_IsColorGradingEnabled()
PP.SetSaturation(1.2)          -- или PP_SetSaturation(1.2) — >1 = насыщенней, <1 = бледнее
local saturation = PP.GetSaturation()
local saturation2 = PP_GetSaturation()
PP.SetContrast(1.1)            -- или PP_SetContrast(1.1)
local contrast = PP.GetContrast()
local contrast2 = PP_GetContrast()
PP.SetGamma(1.0)               -- или PP_SetGamma(1.0)
local gamma = PP.GetGamma()
local gamma2 = PP_GetGamma()
PP.SetColorTint(1.0, 0.9, 0.8) -- или PP_SetColorTint(1.0, 0.9, 0.8) — Тёплый оттенок
local tint = PP.GetColorTint()
local tint2 = PP_GetColorTint()
```

### Виньетка

```lua
PP.SetVignetteEnabled(true)    -- или PP_SetVignetteEnabled(true)
local vignetteOn = PP.IsVignetteEnabled()
local vignetteOn2 = PP_IsVignetteEnabled()
PP.SetVignetteIntensity(0.5)   -- или PP_SetVignetteIntensity(0.5)
local vignetteIntensity = PP.GetVignetteIntensity()
local vignetteIntensity2 = PP_GetVignetteIntensity()
PP.SetVignetteRadius(0.8)      -- или PP_SetVignetteRadius(0.8)
local vignetteRadius = PP.GetVignetteRadius()
local vignetteRadius2 = PP_GetVignetteRadius()
PP.SetVignetteSoftness(0.5)    -- или PP_SetVignetteSoftness(0.5)
local vignetteSoftness = PP.GetVignetteSoftness()
local vignetteSoftness2 = PP_GetVignetteSoftness()
```

### Плёночное зерно

```lua
PP_SetFilmGrainEnabled(true)
local filmGrainOn = PP_IsFilmGrainEnabled()
PP_SetFilmGrainIntensity(0.1)
local filmGrainIntensity = PP_GetFilmGrainIntensity()
```

### Хроматическая аберрация

```lua
PP_SetChromaticAberrationEnabled(true)
local chromaOn = PP_IsChromaticAberrationEnabled()
PP_SetChromaticAberrationIntensity(0.02)
local chromaIntensity = PP_GetChromaticAberrationIntensity()
```

### Фоновое затенение (Ambient Occlusion)

```lua
PP_SetAmbientOcclusionEnabled(true)
local aoOn = PP_IsAmbientOcclusionEnabled()
PP_SetAmbientOcclusionIntensity(0.5)   -- сила затенения (0 = нет, 1 = макс.)
local aoIntensity = PP_GetAmbientOcclusionIntensity()
PP_SetAmbientOcclusionRadius(32.0)     -- радиус выборки в пикселях
local aoRadius = PP_GetAmbientOcclusionRadius()
```

### Отражения в экранном пространстве (SSR)

Использует G-буфер (нормали, шероховатость, металличность) и буфер глубины сцены для ray-march отражений отрисованного кадра на металлических/глянцевых поверхностях. Автоматически активируется на любом материале с `Metallic > 0.05` и `Roughness < RoughnessCutoff` (зеркала, полированный металл, мокрый асфальт, лёд, стекло).

```lua
PP.SetSSREnabled(true)                 -- или PP_SetSSREnabled(true)
local ssrOn = PP.IsSSREnabled()        -- или PP_IsSSREnabled()
PP.SetSSRIntensity(0.8)                -- общая сила отражений (0..2)
local ssrI = PP.GetSSRIntensity()
PP.SetSSRMaxDistance(256.0)            -- длина луча в пикселях (1..2048)
local ssrDist = PP.GetSSRMaxDistance()
PP.SetSSRMaxSteps(48)                  -- число шагов трассировки (1..96, жёсткий потолок)
local ssrSteps = PP.GetSSRMaxSteps()
PP.SetSSRThickness(2.0)                -- толщина depth-теста (0.1..32)
local ssrThk = PP.GetSSRThickness()
PP.SetSSRRoughnessCutoff(0.7)          -- поверхности грубее этого значения не отражают (0..1)
local ssrCut = PP.GetSSRRoughnessCutoff()
PP.SetSSRFadeEdge(0.1)                 -- затухание у края экрана в UV (0..0.5)
local ssrFade = PP.GetSSRFadeEdge()
```

Все сеттеры также блендятся per-volume через post-process volumes в `ViewAsset` (для каждого поля есть соответствующая запись `SSR*` рядом с уже существующими блендами AO/Bloom).

### Волюметрические световые лучи (Godrays)

Радиальный блур от настраиваемой экранной позиции источника света, модулированный emissive-яркостью из G-буфера (`GBufferMaterial.g`) и закрываемый depth-буфером. Создаёт объёмные световые лучи за силуэтами — идеально для солнечных лучей через деревья, света из окон, свечения магических порталов.

```lua
PP.SetGodraysEnabled(true)                  -- или PP_SetGodraysEnabled(true)
local grOn = PP.IsGodraysEnabled()
PP.SetGodraysIntensity(1.5)                 -- общая сила (0..4)
PP.SetGodraysSamples(96)                    -- число шагов ray-march (4..192)
PP.SetGodraysDecay(0.96)                    -- затухание на шаг (0.5..1)
PP.SetGodraysWeight(0.4)                    -- вес сэмпла (0..2)
PP.SetGodraysExposure(0.6)                  -- финальный множитель (0..4)
PP.SetGodraysLightScreenPos(0.5, 0.2)       -- UV источника света (солнце в верхней части)

local gIntensity = PP.GetGodraysIntensity()
local gSamples  = PP.GetGodraysSamples()
local gDecay    = PP.GetGodraysDecay()
local gWeight   = PP.GetGodraysWeight()
local gExposure = PP.GetGodraysExposure()
local gLx       = PP.GetGodraysLightScreenX()   -- UV.x источника
local gLy       = PP.GetGodraysLightScreenY()   -- UV.y источника
```

### Процедурное небо (1D градиент + время суток)

Рисует фоновое небо ДО рендера сцены, сэмплируя 2D градиентную текстуру как 1D LUT (X = время суток, Y = вертикальная позиция от горизонта к зениту). G-буферы normal/material при этом не трогаются (SSR/AO/GI продолжают работать корректно).

```lua
PP.SetSkyEnabled(true)
PP.SetSkyTexturePath("Content/Textures/sky_timecycle.png")
PP.SetSkyTimeOfDay(0.5)                     -- 0=полночь, 0.25=рассвет, 0.5=полдень, 0.75=закат
PP.SetSkyIntensity(1.0)                     -- множитель цвета (0..4)
PP.SetSkyHorizonOffset(0.0)                 -- сдвиг сэмплинга градиента по Y (-1..1)

local skyOn     = PP.IsSkyEnabled()
local skyPath   = PP.GetSkyTexturePath()
local timeOfDay = PP.GetSkyTimeOfDay()
local skyI      = PP.GetSkyIntensity()
local horizon   = PP.GetSkyHorizonOffset()
```

### Глобальное освещение — Radiance Cascades 2D

Многокаскадное probe-based GI: из каждого пикселя пускаются N равномерных лучей, которые ray-marchат освещённый emissive G-буфер (`GBufferMaterial.g` + scene color). Каждый следующий каскад имеет вдвое большую дальность, но вдвое меньший вес — даёт мягкое непрямое освещение в 2D сценах (эмиссивные кристаллы освещают стены, огонь подсвечивает пол и т.д.).

```lua
PP.SetGIEnabled(true)
PP.SetGIIntensity(1.0)                      -- сила отраженного света (0..4)
PP.SetGICascadeCount(4)                     -- глубина пирамиды каскадов (1..8)
PP.SetGIBaseRayCount(16)                    -- лучей на каскад (4..64)
PP.SetGIMaxDistance(512.0)                  -- дальность каскада-0 в пикселях (16..2048)

local giOn       = PP.IsGIEnabled()
local giIntensity = PP.GetGIIntensity()
local giCascades = PP.GetGICascadeCount()
local giRays     = PP.GetGIBaseRayCount()
local giDistance = PP.GetGIMaxDistance()
```

Все три эффекта поддерживают per-volume блендинг в `ViewAsset`. `Sky` требует текстуры градиента (например 256x256 PNG с градиентом горизонт→небо по Y и переходом времени суток по X).

### Авто-экспозиция

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

### Глубина резкости (Depth of Field)

```lua
PP.SetDepthOfFieldEnabled(true)       -- или PP_SetDepthOfFieldEnabled(true)
local dofOn = PP.IsDepthOfFieldEnabled()  -- или PP_IsDepthOfFieldEnabled()
PP.SetFocusDistance(500.0)       -- расстояние до фокуса; или PP_SetFocusDistance(500.0)
local focusDist = PP.GetFocusDistance()   -- или PP_GetFocusDistance()
PP.SetFocusRange(200.0)          -- диапазон фокуса; или PP_SetFocusRange(200.0)
local focusRange = PP.GetFocusRange()     -- или PP_GetFocusRange()
PP.SetBlurAmount(1.0)            -- сила размытия; или PP_SetBlurAmount(1.0)
local blur = PP.GetBlurAmount()           -- или PP_GetBlurAmount()
```

### LUT (Таблица цветокоррекции)

```lua
PP.SetLUTTexturePath("Content/LUT/cinematic.png")  -- или PP_SetLUTTexturePath(...)
local lutPath = PP.GetLUTTexturePath()              -- или PP_GetLUTTexturePath()
PP.SetLUTIntensity(1.0)          -- 0 = оригинал, 1 = полный LUT; или PP_SetLUTIntensity(1.0)
local lutIntensity = PP.GetLUTIntensity()            -- или PP_GetLUTIntensity()
```

### Tonemap (LDR-путь)

> Режим тональной компрессии на LDR-композите. На пути HDR10 тонмап пропускается (динамический диапазон кодирует PQ).

```lua
-- Константы режимов
PP.TONEMAP_NONE      -- 0: линейное обрезание в [0,1]
PP.TONEMAP_REINHARD  -- 1: x / (1 + x)
PP.TONEMAP_ACES      -- 2: ACES filmic (по умолчанию)

PP.SetTonemap(PP.TONEMAP_ACES)     -- или PP_SetTonemap(2)
local mode = PP.GetTonemap()        -- → 0 / 1 / 2; или PP_GetTonemap()
```

### Motion Blur (размытие в движении)

> Экранное motion blur по векторам репроекции камеры.

```lua
PP.SetMotionBlurEnabled(true)              -- или PP_SetMotionBlurEnabled(true)
local on = PP.IsMotionBlurEnabled()         -- или PP_IsMotionBlurEnabled()

PP.SetMotionBlurIntensity(0.5)              -- 0..1; или PP_SetMotionBlurIntensity(0.5)
local intensity = PP.GetMotionBlurIntensity()

PP.SetMotionBlurSamples(8)                  -- количество сэмплов (целое); или PP_SetMotionBlurSamples(8)
local samples = PP.GetMotionBlurSamples()

PP.SetMotionBlurMaxBlur(0.05)               -- максимальный радиус размытия (в UV); или PP_SetMotionBlurMaxBlur(0.05)
local maxBlur = PP.GetMotionBlurMaxBlur()
```

### CAS (Contrast Adaptive Sharpening)

> Адаптивная резкость по контрасту (AMD-style) на финальном изображении.

```lua
PP.SetCASEnabled(true)                      -- или PP_SetCASEnabled(true)
local on = PP.IsCASEnabled()

PP.SetCASSharpness(0.5)                     -- 0..1; или PP_SetCASSharpness(0.5)
local sharpness = PP.GetCASSharpness()
```

### Lens Sharpen

> Резкость в стиле unsharp-mask с радиусом, сосредоточенная вокруг фокальной зоны.

```lua
PP.SetLensSharpenEnabled(true)              -- или PP_SetLensSharpenEnabled(true)
local on = PP.IsLensSharpenEnabled()

PP.SetLensSharpenIntensity(0.5)             -- 0..1
local intensity = PP.GetLensSharpenIntensity()

PP.SetLensSharpenRadius(1.0)                -- в пикселях
local radius = PP.GetLensSharpenRadius()
```

### Bloom Dirt (грязь линзы)

> Текстура «грязи» линзы, умножается на сигнал bloom. Требует текстуру.

```lua
PP.SetBloomDirtEnabled(true)                -- или PP_SetBloomDirtEnabled(true)
local on = PP.IsBloomDirtEnabled()

PP.SetBloomDirtTexturePath("Content/Textures/lens_dirt.png")
local path = PP.GetBloomDirtTexturePath()

PP.SetBloomDirtIntensity(1.0)               -- множитель
local intensity = PP.GetBloomDirtIntensity()
```

### Lens Flare (блики)

> Процедурные «призраки» и гало от ярких пикселей.

```lua
PP.SetLensFlareEnabled(true)                -- или PP_SetLensFlareEnabled(true)
local on = PP.IsLensFlareEnabled()

PP.SetLensFlareIntensity(0.5)
local intensity = PP.GetLensFlareIntensity()

PP.SetLensFlareThreshold(1.0)               -- HDR-порог исходных пикселей
local threshold = PP.GetLensFlareThreshold()

PP.SetLensFlareGhostCount(4)                -- целое число «призраков»
local count = PP.GetLensFlareGhostCount()

PP.SetLensFlareGhostDispersal(0.3)          -- расстояние между «призраками»
local dispersal = PP.GetLensFlareGhostDispersal()

PP.SetLensFlareHaloWidth(0.45)              -- радиус гало (в UV)
local halo = PP.GetLensFlareHaloWidth()

PP.SetLensFlareChromaDistortion(2.0)        -- хроматическое разделение «призраков»
local chroma = PP.GetLensFlareChromaDistortion()
```

### Fog (туман)

> Туман по расстоянию: линейный / экспоненциальный / квадратично-экспоненциальный, плюс опциональное затухание по высоте.

```lua
-- Константы режимов
PP.FOG_LINEAR  -- 0
PP.FOG_EXP     -- 1
PP.FOG_EXP2    -- 2

PP.SetFogEnabled(true)                      -- или PP_SetFogEnabled(true)
local on = PP.IsFogEnabled()

PP.SetFogColor(0.6, 0.7, 0.8)               -- RGB 0..1
local r, g, b = PP.GetFogColor()

PP.SetFogDensity(0.02)
local density = PP.GetFogDensity()

PP.SetFogStart(10.0)                        -- начало (только для линейного режима)
local s = PP.GetFogStart()

PP.SetFogEnd(200.0)                         -- конец (только для линейного режима)
local e = PP.GetFogEnd()

PP.SetFogHeightFalloff(0.1)                 -- 0 — отключить затухание по высоте
local falloff = PP.GetFogHeightFalloff()

PP.SetFogHeightOffset(0.0)                  -- мировой Y, относительно которого считается высота
local offset = PP.GetFogHeightOffset()

PP.SetFogMode(PP.FOG_EXP2)                  -- 0 / 1 / 2
local mode = PP.GetFogMode()
```

### Volumetric Fog (объёмный туман)

> Лучевой марш одиночного рассеяния от направленного света.

```lua
PP.SetVolumetricFogEnabled(true)            -- или PP_SetVolumetricFogEnabled(true)
local on = PP.IsVolumetricFogEnabled()

PP.SetVolumetricFogIntensity(1.0)
local intensity = PP.GetVolumetricFogIntensity()

PP.SetVolumetricFogSteps(32)                -- шагов лучевого марша (целое)
local steps = PP.GetVolumetricFogSteps()

PP.SetVolumetricFogScattering(0.5)          -- Henyey-Greenstein g, -1..1
local scattering = PP.GetVolumetricFogScattering()

PP.SetVolumetricFogDensity(0.1)
local density = PP.GetVolumetricFogDensity()

PP.SetVolumetricFogColor(1.0, 0.95, 0.85)
local r, g, b = PP.GetVolumetricFogColor()

PP.SetVolumetricFogLightPos(0.5, 0.5)       -- экранная позиция источника god-rays (UV 0..1)
local x, y = PP.GetVolumetricFogLightPos()
```

### Heat Haze (марево от жары)

> Анимированное смещение UV, имитирующее «дрожание воздуха», маскируется вертикальным градиентом.

```lua
PP.SetHeatHazeEnabled(true)                 -- или PP_SetHeatHazeEnabled(true)
local on = PP.IsHeatHazeEnabled()

PP.SetHeatHazeIntensity(0.02)               -- сила смещения (в UV)
local intensity = PP.GetHeatHazeIntensity()

PP.SetHeatHazeSpeed(1.0)                    -- скорость анимации
local speed = PP.GetHeatHazeSpeed()

PP.SetHeatHazeScale(20.0)                   -- частота шума
local scale = PP.GetHeatHazeScale()

PP.SetHeatHazeMaskTop(0.6)                  -- верх полосы марева (UV.y)
local top = PP.GetHeatHazeMaskTop()

PP.SetHeatHazeMaskBottom(0.0)               -- низ полосы марева (UV.y)
local bottom = PP.GetHeatHazeMaskBottom()
```

### Underwater (под водой)

> Подводный вид: тонировка, искажение UV и анимированные каустики.

```lua
PP.SetUnderwaterEnabled(true)               -- или PP_SetUnderwaterEnabled(true)
local on = PP.IsUnderwaterEnabled()

PP.SetUnderwaterTint(0.1, 0.4, 0.6)         -- RGB 0..1
local r, g, b = PP.GetUnderwaterTint()

PP.SetUnderwaterTintIntensity(0.6)          -- 0..1 — степень смешения с тонировкой
local tintIntensity = PP.GetUnderwaterTintIntensity()

PP.SetUnderwaterDistortion(0.01)            -- смещение UV
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

### Кастомные пост-процесс материалы

> Навешивание нодовых материалов с доменом **Post Process** (Редактор материалов → Domain = Post Process) на активный вид. Каждый материал выполняется как полноэкранный проход, сэмплит сцену и смешивается по индивидуальной силе (0..1). Индивидуальный **порядок** задаёт, выполняется ли материал **до** всех встроенных эффектов (тогда bloom, цветокоррекция, виньетка и т.д. накладываются поверх него) или **после** них (по умолчанию). Применяются только материалы с доменом **Post Process**; обычные (Surface) материалы игнорируются. Пути ведут на ассеты `.ice_material` и сопоставляются после нормализации, поэтому работает адресация и по пути, и по индексу.

```lua
-- Добавить материал (сила по умолчанию 1.0, порядок по умолчанию 0 = после). Возвращает новое количество материалов.
local count = PP.AddCustomMaterial("Content/PP_Invert.ice_material", 0.75)
-- Необязательный 3-й аргумент = порядок: 0 = после всех встроенных эффектов (по умолчанию), 1 = до всех встроенных эффектов.
local count2 = PP.AddCustomMaterial("Content/PP_Grade.ice_material", 1.0, 1)

-- Запросы
local n        = PP.GetCustomMaterialCount()
local has      = PP.HasCustomMaterial("Content/PP_Invert.ice_material")
local path     = PP.GetCustomMaterialPath(1)        -- индекс с 1, "" если вне диапазона
local strength = PP.GetCustomMaterialStrength("Content/PP_Invert.ice_material")
local enabled  = PP.IsCustomMaterialEnabled("Content/PP_Invert.ice_material")
local location = PP.GetCustomMaterialLocation("Content/PP_Invert.ice_material")  -- 0 = после всех эффектов, 1 = до всех эффектов

-- Настройка по пути...
PP.SetCustomMaterialStrength("Content/PP_Invert.ice_material", 0.5)  -- зажимается в 0..1
PP.SetCustomMaterialEnabled("Content/PP_Invert.ice_material", false)
PP.SetCustomMaterialLocation("Content/PP_Invert.ice_material", 1)    -- 0 = после всех эффектов, 1 = до всех эффектов

-- ...или по индексу с 1 (учитывает дубликаты и перебор)
PP.SetCustomMaterialStrengthAt(1, 0.5)
PP.SetCustomMaterialEnabledAt(1, true)
PP.SetCustomMaterialLocationAt(1, 0)

-- Удаление
PP.RemoveCustomMaterial("Content/PP_Invert.ice_material")  -- true, если удалён
PP.RemoveCustomMaterialAt(1)                               -- индекс с 1, true если удалён
PP.ClearCustomMaterials()
```

> Примечание: изменения применяются к **активным** настройкам пост-процесса (как и остальные функции `PP.*`) и не сохраняются в ассет `.ice_view`. Чтобы материал стал постоянным, добавьте его в Редакторе вида → Post Process → Custom Post Process.


### Параметры кастомных пост-обработочных материалов

> Пост-обработочные материалы могут содержать узлы **Scalar Parameter**, **Vector Parameter** и **Texture Parameter**, как и
> поверхностные материалы. Эти функции задают такие параметры для конкретного материала каждый кадр прямо из Lua — коллекция
> параметров материалов больше не нужна. Переопределения привязаны к пути материала и сохраняются при смене силы,
> включении/выключении и смешивании объёмов.

```lua
local MAT = "Content/PP_Raycast.ice_material"

PP.SetCustomMaterialScalar(MAT, "PlayerAngle", angle)          -- параметр float
PP.SetCustomMaterialVector(MAT, "PlayerPos", x, y, 0, 1)       -- параметр vec4 (a по умолчанию 1)
PP.SetCustomMaterialTexture(MAT, "MapData", "levelmap")        -- параметр-текстура; принимает путь
                                                               -- к файлу или имя из Texture.Create

local angleNow = PP.GetCustomMaterialScalar(MAT, "PlayerAngle")
local posNow   = PP.GetCustomMaterialVector(MAT, "PlayerPos")  -- возвращает { r, g, b, a }

PP.ClearCustomMaterialParams(MAT)      -- сбросить переопределения одного материала
PP.ClearAllCustomMaterialParams()      -- сбросить все переопределения
```

> Сеттеры загружают материал, если он ещё не загружен, и возвращают `false`, если материал не найден.
> Имена параметров должны совпадать с полем **Parameter Name** узла в редакторе материалов.
> Коллекции параметров материалов (`MPC.*`) продолжают работать и остаются правильным инструментом для значений,
> общих для многих материалов.

### Колбэки post-process volume

> Подписка Lua-колбэков на вход/выход из конкретного post-process volume. Том идентифицируется по имени (как задано в сцене). В колбэк передаётся имя тома.

```lua
PP.OnVolumeEnter("CaveVolume", function(name)
    print("Вошли:", name)
end)

PP.OnVolumeExit("CaveVolume", function(name)
    print("Вышли:", name)
end)

-- Передайте nil, чтобы убрать колбэк одного направления:
PP.OnVolumeEnter("CaveVolume", nil)

-- Удалить оба колбэка (вход и выход) для одного тома:
PP.RemoveVolumeCallback("CaveVolume")

-- Удалить все зарегистрированные колбэки:
PP.ClearVolumeCallbacks()
```

---

## 23. Cinema — Кинематики

> **Тип:** Глобальные функции. Таблица `Cinema`.
>
> Кинематики создаются в `.ice_cinema` файлах с таймлайном и могут вызывать Lua-функции.

```lua
-- Воспроизведение
Cinema.Play("Content/Cinema/intro.ice_cinema")
Cinema.Stop("Content/Cinema/intro.ice_cinema")
Cinema.Pause("Content/Cinema/intro.ice_cinema")
Cinema.Resume("Content/Cinema/intro.ice_cinema")

-- С плавным входом
Cinema.PlayWithBlend("Content/Cinema/intro.ice_cinema", 0.5)

-- Проверки
local playing = Cinema.IsPlaying("Content/Cinema/intro.ice_cinema")
local paused = Cinema.IsPaused("Content/Cinema/intro.ice_cinema")
local controlling = Cinema.IsControllingCamera()
local anyPlaying = Cinema.IsAnyPlaying()

-- Время
local t = Cinema.GetTime("Content/Cinema/intro.ice_cinema")
Cinema.SetTime("Content/Cinema/intro.ice_cinema", 5.0)
local dur = Cinema.GetDuration("Content/Cinema/intro.ice_cinema")

-- Прогресс (0.0 — 1.0)
local progress = Cinema.GetProgress("Content/Cinema/intro.ice_cinema")

-- Скорость воспроизведения
Cinema.SetPlaybackRate("Content/Cinema/intro.ice_cinema", 2.0) -- 2x скорость
local rate = Cinema.GetPlaybackRate("Content/Cinema/intro.ice_cinema")

-- Пропустить
Cinema.Skip("Content/Cinema/intro.ice_cinema")

-- Остановить все кинематики
Cinema.StopAll()

-- Перезагрузить
Cinema.Reload("Content/Cinema/intro.ice_cinema")

-- Вес смешивания
Cinema.SetBlendWeight("Content/Cinema/intro.ice_cinema", 0.5)

-- Управление зацикливанием
Cinema.SetLoop("Content/Cinema/intro.ice_cinema", true)
local looping = Cinema.GetLoop("Content/Cinema/intro.ice_cinema")

-- Имя текущей кинематики
local name = Cinema.GetName()

-- Эффекты затемнения (независимо от ключевых кадров кинематики)
Cinema.FadeOut(1.0)              -- затемнение в чёрный за 1 секунду
Cinema.FadeOut(1.0, 1, 0, 0)    -- затемнение в красный за 1 секунду
Cinema.FadeIn(0.5)               -- выход из чёрного за 0.5 секунды
Cinema.FadeIn(0.5, 1, 1, 1)     -- выход из белого за 0.5 секунды

-- Запросы состояния затемнения
local fading = Cinema.IsFading()
local alpha = Cinema.GetFadeAlpha()       -- 0.0 (прозрачный) до 1.0 (непрозрачный)
local color = Cinema.GetFadeColor()       -- → {r, g, b, a}

-- Информация о камере (во время воспроизведения кинематики)
local camPos = Cinema.GetCameraPosition()  -- → {x, y, z}
local camZoom = Cinema.GetCameraZoom()

-- Тряска камеры (независимо от ключевых кадров)
Cinema.ShakeCamera(5.0, 10.0, 0.5)  -- интенсивность, частота, длительность

-- Список воспроизводимых кинематик
local list = Cinema.GetPlayingList()  -- → {"path1", "path2", ...}

-- Колбэк завершения
Cinema.OnFinished("Content/Cinema/intro.ice_cinema", function(path)
    print("Кинематика завершена: " .. path)
end)
Cinema.ClearOnFinished("Content/Cinema/intro.ice_cinema")
```

> В `.ice_cinema` можно добавлять **Lua Callback** keyframes — они вызывают глобальные Lua-функции в определённый момент времени. Эти функции определяются в скрипте уровня или глобально (в class/widget/AI скриптах).
>
> Также движок автоматически вызывает две глобальные функции:
> - `OnCinemaStart()` — срабатывает сразу после `Cinema.Play` (или autoplay/триггера world-asset). Используйте `Cinema.GetName()`, если может играть несколько катсцен.
> - `OnCinemaEnd()` — срабатывает когда катсцена завершается естественно, по `Skip` или при обратном проигрывании.
>
> Для логики завершения по конкретному пути предпочтительнее `Cinema.OnFinished(path, callback)` — это чище, чем сравнивать имена внутри `OnCinemaEnd`.

| Функция | Описание |
|---|---|
| `Cinema.Play(path)` | Начать воспроизведение кинематики |
| `Cinema.Stop(path)` | Остановить и сбросить кинематику |
| `Cinema.Pause(path)` | Поставить на паузу |
| `Cinema.Resume(path)` | Возобновить воспроизведение |
| `Cinema.PlayWithBlend(path, blendIn)` | Воспроизвести с плавным входом |
| `Cinema.Skip(path)` | Пропустить до конца |
| `Cinema.StopAll()` | Остановить все воспроизводимые кинематики |
| `Cinema.Reload(path)` | Перезагрузить кинематику с диска |
| `Cinema.IsPlaying(path)` | Проверить, воспроизводится ли |
| `Cinema.IsPaused(path)` | Проверить, на паузе ли |
| `Cinema.IsAnyPlaying()` | Проверить, воспроизводится ли хоть одна кинематика |
| `Cinema.IsControllingCamera()` | Проверить, управляет ли кинематика камерой |
| `Cinema.GetTime(path)` | Получить текущее время воспроизведения |
| `Cinema.SetTime(path, time)` | Установить время воспроизведения |
| `Cinema.GetDuration(path)` | Получить общую продолжительность |
| `Cinema.GetProgress(path)` | Получить прогресс 0.0–1.0 |
| `Cinema.SetPlaybackRate(path, rate)` | Установить скорость воспроизведения (-10.0–10.0, отрицательное = реверс) |
| `Cinema.GetPlaybackRate(path)` | Получить текущую скорость воспроизведения |
| `Cinema.SetBlendWeight(path, weight)` | Установить вес смешивания (0.0–1.0) |
| `Cinema.SetLoop(path, loop)` | Включить/выключить зацикливание во время выполнения |
| `Cinema.GetLoop(path)` | Проверить, включено ли зацикливание |
| `Cinema.GetName()` | Получить имя текущей кинематики |
| `Cinema.FadeIn(duration, r?, g?, b?)` | Плавный переход из цвета (по умолчанию чёрный) |
| `Cinema.FadeOut(duration, r?, g?, b?)` | Плавный переход в цвет (по умолчанию чёрный) |
| `Cinema.IsFading()` | Проверить, активен ли эффект затемнения |
| `Cinema.GetFadeAlpha()` | Получить текущую непрозрачность затемнения (0.0–1.0) |
| `Cinema.GetFadeColor()` | Получить цвет затемнения как таблицу `{r, g, b, a}` |
| `Cinema.GetCameraPosition()` | Получить позицию камеры кинематики `{x, y, z}` |
| `Cinema.GetCameraZoom()` | Получить зум камеры кинематики |
| `Cinema.ShakeCamera(intensity, frequency, duration)` | Запустить тряску камеры (независимо от ключевых кадров) |
| `Cinema.GetPlayingList()` | Получить список путей воспроизводимых кинематик |
| `Cinema.OnFinished(path, callback)` | Зарегистрировать колбэк завершения кинематики |
| `Cinema.ClearOnFinished(path)` | Удалить колбэк завершения кинематики |

---

## 24. Settings — Настройки игры

> **Тип:** Глобальные функции. Таблица `Settings`.

### Графика

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

Settings.SetLowLatencyMode(false)            -- см. раздел "Режим низкой задержки"
local ll = Settings.IsLowLatencyMode()

Settings.SetProjectPrewarm(false)            -- см. раздел "Прогрев проекта"
local pp = Settings.IsProjectPrewarm()

Settings.SetIsSuspended(true)                -- см. раздел "Приостановка (Suspend)"
local sus = Settings.IsSuspended()

Settings.SetHDR10(true)
local hdr = Settings.IsHDR10()

Settings.SetHDR10PaperWhiteNits(200.0)
local pw = Settings.GetHDR10PaperWhiteNits()

Settings.SetHDR10MaxLuminanceNits(1000.0)
local maxNits = Settings.GetHDR10MaxLuminanceNits()

-- Размер экрана (физический монитор)
local screen = Settings.GetScreenSize()  -- → {width, height}
```

### Качество

```lua
-- Универсальный масштаб рендера (диапазон: 0.01 .. 2.0, т.е. 1% .. 200%)
-- Управляет одновременно разрешением оффскрин-рендера и качеством партиклов.
-- Перемножается с режимами SSAA (ssaa_2x ~1.41x, ssaa_4x 2.0x).
Settings.SetRenderScale(1.0)
local rs = Settings.GetRenderScale()  -- float

-- Режим AA (рекомендуется): строковый API для всех техник сглаживания
-- Константы:
--   Settings.AA_MODE_OFF      -- "off"      (без сглаживания)
--   Settings.AA_MODE_FXAA     -- "fxaa"     (пост-процесс, быстро)
--   Settings.AA_MODE_MSAA_2X  -- "msaa_2x"  (мультисэмплинг, требует перезапуска)
--   Settings.AA_MODE_MSAA_4X  -- "msaa_4x"  (мультисэмплинг, требует перезапуска)
--   Settings.AA_MODE_MSAA_8X  -- "msaa_8x"  (мультисэмплинг, требует перезапуска)
--   Settings.AA_MODE_SSAA_2X  -- "ssaa_2x"  (суперсэмплинг, ~1.41x масштаб рендера)
--   Settings.AA_MODE_SSAA_4X  -- "ssaa_4x"  (суперсэмплинг, 2.0x масштаб рендера)
Settings.SetAAMode(Settings.AA_MODE_FXAA)
local mode = Settings.GetAAMode()  -- → "off" | "fxaa" | "msaa_*" | "ssaa_*"

-- MSAA Alpha-To-Coverage (работает только при выбранном msaa_*)
Settings.SetMSAAAlphaToCoverage(true)
local a2c = Settings.IsMSAAAlphaToCoverage()

-- Качество аудио (частота дискретизации):
--   AUDIO_VERY_LOW(0)    = 8000 Гц
--   AUDIO_LOW(1)         = 16000 Гц
--   AUDIO_MEDIUM_LOW(2)  = 22050 Гц
--   AUDIO_MEDIUM(3)      = 44100 Гц
--   AUDIO_HIGH(4)        = 48000 Гц   (по умолчанию)
--   AUDIO_VERY_HIGH(5)   = 96000 Гц
Settings.SetAudioQuality(Settings.AUDIO_HIGH)
local aq = Settings.GetAudioQuality()

-- Освещение
Settings.SetLightingEnabled(true)    -- true = Lit, false = Unlit
local lit = Settings.IsLightingEnabled()

-- Константы освещения
Settings.LIGHTING_LIT    -- true
Settings.LIGHTING_UNLIT  -- false
```

### Масштабирование (FSR / NIS)

> Рендерит сцену во внутреннем разрешении ниже выходного и восстанавливает её до полного в самом конце кадра, **после** постобработки. **Только Vulkan и только на совместимой видеокарте** — ровно как трассировка лучей. На любом другом бэкенде или неподдерживаемом устройстве настройка сохраняется, но не действует, и кадр масштабируется обычной линейной фильтрацией.
>
> - **`"fsr"`** — AMD FidelityFX Super Resolution 1.0: EASU (пространственный апсемплинг с адаптацией к границам) и затем RCAS (устойчивое контрастно-адаптивное повышение резкости). Работает на **любой** видеокарте с Vulkan.
> - **`"nis"`** — NVIDIA Image Scaling: направленное масштабирование по структурному тензору с адаптивной нерезкой маской, за один проход. Требует видеокарту **NVIDIA**.
>
> Пресет качества задаёт внутреннее разрешение рендера как долю от выходного и **перемножается** с `SetRenderScale()` и режимами SSAA, поэтому оставьте `RenderScale` равным `1.0`, чтобы разрешением управлял только пресет. По умолчанию — `"off"`.

```lua
-- Константы апскейлера
--   Settings.UPSCALING_OFF   -- "off"   (обычная линейная фильтрация, по умолчанию)
--   Settings.UPSCALING_FSR   -- "fsr"   (AMD FidelityFX Super Resolution 1.0)
--   Settings.UPSCALING_NIS   -- "nis"   (NVIDIA Image Scaling, только видеокарты NVIDIA)
Settings.SetUpscalingMode(Settings.UPSCALING_FSR)
local upscaler = Settings.GetUpscalingMode()   -- → "off" | "fsr" | "nis"

-- Константы пресетов качества (внутреннее разрешение рендера от выходного)
--   Settings.UPSCALING_ULTRA_PERFORMANCE  -- "ultra_performance"   33%
--   Settings.UPSCALING_PERFORMANCE        -- "performance"         50%
--   Settings.UPSCALING_BALANCED           -- "balanced"            59%
--   Settings.UPSCALING_QUALITY            -- "quality"             67%  (по умолчанию)
--   Settings.UPSCALING_ULTRA_QUALITY      -- "ultra_quality"       77%
--   Settings.UPSCALING_NATIVE             -- "native"             100%  (только проход резкости)
Settings.SetUpscalingQuality(Settings.UPSCALING_QUALITY)
local preset = Settings.GetUpscalingQuality()  -- → "quality"

-- Проход повышения резкости (FSR → RCAS, NIS → адаптивная нерезкая маска)
Settings.SetUpscalingSharpening(true)
local sharpening = Settings.IsUpscalingSharpening()

Settings.SetUpscalingSharpness(0.5)            -- 0.0 .. 1.0 (по умолчанию 0.5)
local sharpness = Settings.GetUpscalingSharpness()

-- Запрос поддержки
local anySupported = Settings.IsUpscalingSupported()                        -- любой апскейлер на этом устройстве
local fsrSupported = Settings.IsUpscalingSupported(Settings.UPSCALING_FSR)  -- конкретный
local nisSupported = Settings.IsUpscalingSupported(Settings.UPSCALING_NIS)

local active = Settings.IsUpscalingActive()       -- выбран И поддерживается прямо сейчас
local factor = Settings.GetUpscalingRenderScale() -- 1.0 если неактивен, иначе коэффициент пресета
```

```lua
-- Типичное графическое меню игры: выбрать лучший доступный апскейлер
if Settings.IsUpscalingSupported(Settings.UPSCALING_NIS) then
    Settings.SetUpscalingMode(Settings.UPSCALING_NIS)
elseif Settings.IsUpscalingSupported(Settings.UPSCALING_FSR) then
    Settings.SetUpscalingMode(Settings.UPSCALING_FSR)
end

if Settings.IsUpscalingActive() then
    Settings.SetRenderScale(1.0)                              -- разрешением управляет пресет
    Settings.SetUpscalingQuality(Settings.UPSCALING_BALANCED)
    Settings.SetUpscalingSharpness(0.6)
    Settings.Save()
end
```

### Рендерер / Графический API

> Узнать активный графический бэкенд или запросить другой. `GetRenderer()` работает на **любой платформе** и возвращает используемый сейчас рендерер. `SetRenderer(name)` записывает выбранный бэкенд в `Config/Engine.json` → `Rendering.RenderBackend` и **применяется при следующем запуске** — бэкенд выбирается один раз при старте, когда создаются окно и графический контекст, поэтому переключение «на лету» не выполняется. На платформах, где бэкенд задан сборкой (Web → WebGPU или WebGL 2.0, выбирается при сборке с автоматическим откатом к WebGL 2.0, если браузер не поддерживает WebGPU; macOS/iOS → Metal через ANGLE; Android → тот бэкенд, с которым собран APK), значение всё равно сохраняется, но активный рендерер остаётся тем, что предоставляет платформа; бэкенд, не вкомпилированный в сборку, аккуратно откатывается при старте.
>
> Имена принимаются без учёта регистра и пробелов: `Vulkan`, `OpenGL 4.6`, `OpenGL 3.3`, `OpenGL ES 3.2`, `WebGL 2.0`, `WebGPU`, `Metal`. Бэкенд Vulkan в рантайме согласует версию **1.4 → 1.3 → 1.2 → 1.1** и откатывается к OpenGL (десктоп) / OpenGL ES (Android), если совместимого устройства нет. На Web `WebGPU` при старте откатывается к `WebGL 2.0`, если браузер не поддерживает WebGPU.

```lua
-- Узнать активный рендерер (любая платформа)
local renderer = Settings.GetRenderer()    -- напр. "Vulkan", "OpenGL 4.6", "OpenGL ES 3.2", "WebGL 2.0", "WebGPU", "Metal (ANGLE)"

-- Запросить рендерер; сохраняется в конфиг, применяется при следующем запуске. Возвращает true при успехе.
local ok = Settings.SetRenderer("Vulkan")
Settings.SetRenderer("OpenGL 4.6")          -- явный OpenGL на десктопе
```

### Оптимизация / Производительность

> Глобальные параметры настройки рендера, соответствующие вкладке **Optimization** в редакторе. Каждый сеттер ограничивает значение безопасным диапазоном и сохраняется в `GameSettings.json`; значения из `Config/Engine.json` → `Optimization` применяются при старте **до** инициализации рендера.
>
> - **Применяются сразу:** `CullPadding`, `CullingMode`, `IsSuspended`.
> - **Требуют перезапуска** (GPU-буферы/шейдеры, текстурные атласы и кэшированный лимит размера текстуры устройства строятся один раз при старте из этих значений): `MaxPointLights`, `MaxTextureSlotsGL`, `MaxTextureSlotsGLES`, `MaxQuadsPerBatch`, `MaxParticlesPerBatch`, `MaxInstancesPerBatch`, `DebugVertexBufferSize`, `AtlasSize`, `MaxSpriteSize`, `MaxTextureSize`.

```lua
-- Отсечение (culling)
Settings.SetCullPadding(256.0)            -- запас отсечения за экраном в px (диапазон: 0 .. 16384)
local pad = Settings.GetCullPadding()

Settings.SetCullingMode(0)                -- 0 = AABB (быстро), 1 = PerPixel (строго)
local cm = Settings.GetCullingMode()

-- Автоприостановка, когда окно не в фокусе / свернуто (см. раздел "Приостановка (Suspend)")
Settings.SetIsSuspended(true)             -- по умолчанию: true
local sus = Settings.IsSuspended()

-- Батчинг: GPU-буферы выделяются ровно под заданное значение (без перерасхода),
-- поэтому увеличение требует больше VRAM, а уменьшение — экономит. Требуют перезапуска.
Settings.SetMaxQuadsPerBatch(50000)       -- диапазон: 1 .. 1000000
local quads = Settings.GetMaxQuadsPerBatch()
Settings.SetMaxParticlesPerBatch(10000)   -- диапазон: 1 .. 1000000
local parts = Settings.GetMaxParticlesPerBatch()
Settings.SetMaxInstancesPerBatch(100000)  -- диапазон: 1 .. 1000000
local inst = Settings.GetMaxInstancesPerBatch()

-- Текстурные слоты на батч (требуют перезапуска; эффективное число дополнительно ограничивается при старте лимитом GPU/рендера)
Settings.SetMaxTextureSlotsGL(14)         -- десктопный OpenGL, диапазон: 1 .. 32
local slGL = Settings.GetMaxTextureSlotsGL()
Settings.SetMaxTextureSlotsGLES(12)       -- GLES / WebGL2 / Metal-ANGLE, диапазон: 1 .. 32
local slES = Settings.GetMaxTextureSlotsGLES()

-- Лимит активных точечных источников света (требует перезапуска; жёсткий потолок 128 = размер UBO-массива)
Settings.SetMaxPointLights(32)            -- диапазон: 1 .. 128
local mpl = Settings.GetMaxPointLights()

-- Текстурный атлас / текстуры (требуют перезапуска)
Settings.SetAtlasSize(4096)               -- размер страницы атласа в px (диапазон: 256 .. 16384)
local atl = Settings.GetAtlasSize()
Settings.SetMaxSpriteSize(2048)           -- максимальный спрайт, упаковываемый в общий атлас (диапазон: 64 .. 16384)
local mss = Settings.GetMaxSpriteSize()
Settings.SetMaxTextureSize(0)             -- верхний потолок макс. размера текстуры GPU; 0 = реальный максимум устройства (диапазон: 0 .. 65536)
local mts = Settings.GetMaxTextureSize()

-- Буфер вершин отладочного рендера (требует перезапуска)
Settings.SetDebugVertexBufferSize(128)    -- диапазон: 16 .. 4096
local dvb = Settings.GetDebugVertexBufferSize()
```

### Звук (через Settings)

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

### Платформа

Движок поддерживает 6 платформ: **Windows**, **Linux**, **macOS**, **iOS**, **Android**, **Web**. Из Lua можно определить текущую платформу и писать как платформо-специфичный код, так и общий код для всех платформ сразу. Про архитектуру процессора, под которую собрана эта же сборка, см. раздел **Архитектура** сразу ниже.

```lua
local platform = Settings.GetPlatform()
-- Возвращает одно из: "Windows", "Linux", "macOS", "iOS", "Android", "Web"
-- ("Unknown" — только если собрано под неизвестную цель)

-- Проверки конкретной платформы (ровно одна вернёт true в текущей сборке):
local isWindows = Settings.IsWindows()
local isLinux   = Settings.IsLinux()
local isMacOS   = Settings.IsMacOS()
local isIOS     = Settings.IsIOS()
local isAndroid = Settings.IsAndroid()
local isWeb     = Settings.IsWeb()

-- Групповые проверки:
local isApple     = Settings.IsApple()      -- macOS или iOS
local isMobile    = Settings.IsMobile()     -- iOS или Android (нативная сборка)
local isMobileWeb = Settings.IsMobileWeb()  -- true ТОЛЬКО на Web-сборке, если браузер запущен на мобильном устройстве (UA + touch + pointer:coarse). На всех остальных платформах всегда false.
local isDesktop   = Settings.IsDesktop()    -- Windows, Linux или macOS

-- Строковые константы (удобно для switch-логики и сравнений):
Settings.PLATFORM_WINDOWS  -- "Windows"
Settings.PLATFORM_LINUX    -- "Linux"
Settings.PLATFORM_MACOS    -- "macOS"
Settings.PLATFORM_IOS      -- "iOS"
Settings.PLATFORM_ANDROID  -- "Android"
Settings.PLATFORM_WEB      -- "Web"

-- Шаблоны использования:

-- 1) Код только под одну платформу
if Settings.IsAndroid() then
    -- логика только под Android (реклама, IAP, разрешения и т.п.)
end

-- 2) Разный код под разные платформы
if Settings.IsMobile() or Settings.IsMobileWeb() then
    -- тач-ввод, более крупный UI, экономия батареи
    -- ВАЖНО: используйте именно эту пару, чтобы виртуальные стики/кнопки
    -- появлялись и в нативной мобильной сборке, и при заходе с телефона
    -- через браузер на ту же Web-сборку.
elseif Settings.IsWeb() then
    -- десктопный браузер (мышь+клава)
else
    -- ветка десктопа (Windows / Linux / macOS)
end

-- 3) switch-стиль по имени платформы
local p = Settings.GetPlatform()
if p == Settings.PLATFORM_WINDOWS then
    -- ...
elseif p == Settings.PLATFORM_MACOS or p == Settings.PLATFORM_LINUX then
    -- общая ветка для десктопного Unix
elseif p == Settings.PLATFORM_IOS then
    -- ...
end

-- 4) Код «под все 6 платформ сразу» — просто не оборачивайте его в
--    проверки: один и тот же скрипт работает везде, так как все 6
--    платформ предоставляют одинаковый Lua API.
```

### Архитектура

`Settings.GetPlatform()` отвечает на вопрос *какая ОС*, а `Settings.GetArch()` — *под какой процессор собрана эта сборка*. Именно на этом уровне и решается «устройство слабое»: старый 32-битный ARM-телефон и современный 64-битный оба вернут `"Android"`, но в 32-битное адресное пространство заперт только один из них.

```lua
local arch = Settings.GetArch()
-- Возвращает одно из: "x64", "x86", "arm64", "arm32", "wasm32", "wasm64"
-- ("Unknown" — только если собрано под неизвестную цель)

-- Строковые константы архитектур (удобно для switch-логики и сравнений):
Settings.ARCH_X86     -- "x86"     32-битный Intel/AMD
Settings.ARCH_X64     -- "x64"     64-битный Intel/AMD
Settings.ARCH_ARM32   -- "arm32"   32-битный ARM (Android armeabi-v7a)
Settings.ARCH_ARM64   -- "arm64"   64-битный ARM (Apple Silicon, Android arm64-v8a, Windows/Linux на ARM)
Settings.ARCH_WASM32  -- "wasm32"  WebAssembly с 32-битными указателями (куча упирается в 2 ГБ)
Settings.ARCH_WASM64  -- "wasm64"  WebAssembly с 64-битными указателями (сборка с -sMEMORY64)

-- Разрядность указателя — самая короткая проверка «сколько памяти сборка вообще может адресовать»:
local is64 = Settings.Is64Bit()   -- true на x64, arm64, wasm64
local is32 = Settings.Is32Bit()   -- true на x86, arm32, wasm32
```

Что могут вернуть все 6 платформ:

| Платформа | Архитектуры, под которые собирается движок | Что вернёт `Settings.GetArch()` |
|-----------|--------------------------------------------|---------------------------------|
| Windows   | x64, x86, arm64 | `"x64"`, `"x86"`, `"arm64"` |
| Linux     | x64, x86, arm64 | `"x64"`, `"x86"`, `"arm64"` |
| macOS     | arm64 (Apple Silicon), x64 (Intel) | `"arm64"`, `"x64"` |
| iOS       | arm64 (устройство и симулятор); x64 — только симулятор на Intel-маке | `"arm64"`, `"x64"` |
| Android   | `arm64-v8a`, `armeabi-v7a`, `x86_64`, `x86` | `"arm64"`, `"arm32"`, `"x64"`, `"x86"` |
| Web       | wasm32, wasm64 (сборка с `-sMEMORY64`) | `"wasm32"`, `"wasm64"` |

> **Имена Android-ABI нормализованы**, поэтому одно сравнение работает везде: `armeabi-v7a` → `"arm32"`, `arm64-v8a` → `"arm64"`, `x86_64` → `"x64"`, `x86` → `"x86"`. Пишите `arch == Settings.ARCH_ARM64` один раз вместо разбора платформенных написаний ABI.

Значение — это свойство **самой запущенной сборки**, определённое на этапе компиляции, а не опрос железа: x64-сборка, запущенная на ARM-машине через эмуляцию, всё равно вернёт `"x64"`, потому что выполняется именно этот код — и именно под него и нужно закладывать бюджет. Вызов дешёвый (без системных вызовов и обращений к диску), так что его спокойно можно использовать в `OnStart` или в меню настроек.

Шаблоны использования:

```lua
-- 1) Одна проверка на всё, что ограничено адресным пространством
if Settings.Is32Bit() then
    Settings.SetMaxTextureSize(2048)
    Settings.SetAtlasSize(2048)
    Settings.SetMaxPointLights(8)
    Settings.SetAAMode(Settings.AA_MODE_OFF)
end

-- 2) switch-стиль по имени архитектуры
local a = Settings.GetArch()
if a == Settings.ARCH_ARM32 then
    Settings.SetRenderScale(0.75)
    Settings.SetAudioQuality(Settings.AUDIO_LOW)
    Settings.SetFPSLimit(30)
elseif a == Settings.ARCH_ARM64 then
    Settings.SetRenderScale(1.0)
    Settings.SetFPSLimit(60)
elseif a == Settings.ARCH_WASM32 then
    -- браузерная сборка без -sMEMORY64: всё должно уместиться в 2 ГБ кучи
    Settings.SetMaxTextureSize(4096)
end

-- 3) Платформа + архитектура вместе — точная проверка «слабого устройства»
if Settings.IsAndroid() and Settings.GetArch() == Settings.ARCH_ARM32 then
    -- старый 32-битный телефон: самый низкий пресет
elseif Settings.IsMobile() then
    -- современный arm64-телефон/планшет: обычный мобильный пресет
end

-- 4) Либо отдайте бюджет движку вместо ручных пресетов
if Settings.Is32Bit() then
    Settings.SetAdaptiveQuality(true, 30)
else
    Settings.SetAdaptiveQuality(true, 60)
end
```

### Специальные возможности (Accessibility)

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

-- Фильтры дальтонизма
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

`DyslexiaFontPath` принимает путь относительно папки `Content/` проекта
(например `"Fonts/OpenDyslexic-Regular.ttf"` или `"Content/Fonts/OpenDyslexic-Regular.ttf"`),
а также абсолютный путь; упакованные VFS-сборки поддерживаются. Пока замена
активна (`AccessibilityEnabled` + `DyslexiaFriendlyFont` + непустой путь), она
подменяет **все** шрифты в игровом рендере текста — элементы виджетов со своими
шрифтами, элементы без шрифта, тултипы и выпадающие списки — а редактор
пересобирает свой UI-шрифт: дислексия-шрифт становится основным, а обычный
шрифт редактора подключается как резерв для недостающих глифов. Если файл не
найден, в лог выводится одно предупреждение, и обычные шрифты продолжают
работать.

### Синтез речи (TTS)

При включённом `Accessibility.TTSEnabled` каждый вызов `Settings.SpeakHint(text)`
отправляется в системный голосовой движок — Windows SAPI 5,
Web Speech API в emscripten-сборке, **AVSpeechSynthesizer** на macOS/iOS,
**espeak-ng** на Linux при наличии библиотеки,
**android.speech.tts.TextToSpeech** на Android, заглушка на
остальных. На Windows голоса, установленные в ОС (Narrator, Vocalizer,
голоса NVDA и т.п.) подхватываются автоматически.

TTS работает и полностью автоматически, без единой строки Lua: когда специальные
возможности и TTS включены (в `Config/Engine.json`, через настройки редактора
или через этот API), рантайм виджетов озвучивает текст элемента под курсором
мыши (включая неинтерактивные Text/Image-подписи), читает тултипы при их
появлении, проговаривает наведённые пункты выпадающих списков и объявляет
сфокусированный элемент при навигации по UI с геймпада/клавиатуры. Слайдеры и
прогресс-бары озвучивают подпись вместе с текущим значением. `SpeakHint`
остаётся для собственных игровых подсказок.

Как установить:
- **Windows**: ничего ставить не нужно — всегда используется SAPI 5. Установка
  espeak-ng отдельным приложением не мешает, но движком не используется.
- **Linux**: поставьте dev-пакет, например `sudo apt install libespeak-ng-dev`
  (Debian/Ubuntu). CMake находит его через `find_package` / `find_library`.
  Без заголовков бэкенд автоматически переключается на stub.
- **Android**: ничего компилировать не нужно. Движок разговаривает с тем TTS-
  движком, который выбран в *Настройки → Язык и ввод → Синтез речи*. Установите
  **eSpeak NG APK** (или любой другой системный TTS) и выберите его в качестве
  предпочтительного — бридж подхватит его автоматически. Блок `<queries>` для
  `intent.action.TTS_SERVICE` уже в manifest, чтобы Android 11+ видел движки.
- **Web (emscripten)**: ничего ставить не нужно — используется Web Speech API
  браузера.

```lua
Settings.SetTTSEnabled(true)
local on = Settings.IsTTSEnabled()
Settings.SpeakHint("Меню открыто")    -- произнести текст через TTS-движок
Settings.SetTTSRate(0.0)              -- -1..+1 (медленно..быстро)
local rate = Settings.GetTTSRate()
Settings.SetTTSVolume(1.0)            -- 0..1
local vol = Settings.GetTTSVolume()
Settings.SetTTSPitch(0.0)             -- -1..+1
local pitch = Settings.GetTTSPitch()
Settings.StopSpeech()                 -- прервать текущую фразу
local speaking = Settings.IsSpeaking()
local backend = Settings.GetTTSBackend()       -- "SAPI 5" | "Web Speech API" | "AVSpeechSynthesizer" | "espeak-ng" | "Android TextToSpeech (<engine>)" | "stub"
local available = Settings.IsTTSAvailable()
local sr_active = Settings.IsScreenReaderActive() -- true если запущен Narrator/JAWS/NVDA
```

### Скорость игры (accessibility slow-mo)

`Settings.SetGameSpeed(s)` независим от `SetTimeScale()`. Он умножает
`GetDeltaTime()`, поэтому при `0.0` `IsPaused()` становится true. Диапазон
ограничен `[0.1, 2.0]`.

```lua
Settings.SetGameSpeed(0.5)            -- 50% скорости
Settings.ResetGameSpeed()             -- вернуть 1.0
local s = Settings.GetGameSpeed()
```

### Принудительное моно

```lua
Audio.SetForceMono(true)                     -- помощь при односторонней потере слуха
local mono = Audio.IsForceMono()

-- Эквивалент со стороны accessibility: сохраняется вместе с остальными
-- настройками доступности и применяется заново при загрузке, тогда как пара
-- Audio.* только переключает текущее состояние микшера.
Settings.SetForceMonoAudio(true)
local monoSetting = Settings.IsForceMonoAudio()
```

### Сохранение/загрузка настроек

```lua
Settings.Save()             -- Сохранить в GameSettings.json (в пользовательский конфиг)
Settings.Load()             -- Загрузить из GameSettings.json
Settings.Apply()            -- Применить все текущие настройки к движку
                            --   (окно, VSync, качество, звук, AA, освещение, HDR10)
                            --   Если SDL-окно ещё не создано — оконные настройки
                            --   пропускаются с предупреждением, остальные применяются.

Settings.ResetDefaults()    -- Сбросить все настройки на значения по умолчанию + Apply()
                            --   По умолчанию: 1920x1080 windowed, VSync вкл., HDR10 выкл.,
                            --   60 FPS, RenderScale=1.0, Audio=High, AA=Off,
                            --   AdaptiveQuality выкл. (цель 60), Lighting=Lit,
                            --   все громкости=1.0, не замьючено.

local path = Settings.GetSettingsPath()  -- Абсолютный путь к GameSettings.json
```

### Авто-определение характеристик

Оценивает машину по ОЗУ, числу логических ядер CPU и разрешению основного
дисплея (со штрафами для мобильных платформ, веба и GLES-бэкендов), затем
применяет соответствующий пресет: масштаб рендера, качество аудио,
сглаживание, VSync, HDR10 и лимит FPS:

| Уровень | Масштаб рендера | Аудио | AA | Лимит FPS |
| ------- | --------------- | ----- | -- | --------- |
| Топовый | 1.5 | Very High | MSAA 4x | частота дисплея (мин. 144) |
| Высокий | 1.0 | High | MSAA 2x | частота дисплея (макс. 120) |
| Средний | 0.75 | Medium | FXAA | частота дисплея (макс. 90) |
| Низкий | 0.5 | Medium-Low | Off | частота дисплея (макс. 60) |

Примечания:
- Пресет всегда включает VSync.
- HDR10 включается только на топовом уровне с дисплеем 1440p+ на десктопном
  бэкенде Vulkan / Metal (MoltenVK) — только они умеют отдавать настоящий
  HDR10 (ST.2084) swapchain. Если дисплей не поддерживает HDR10, рендер
  тихо остаётся в SDR.
- На мобильных платформах и в вебе пресеты MSAA/SSAA понижаются до FXAA
  (окна там создаются без мультисэмпл-буферов), а HDR10 остаётся выключенным.
- В вебе объём ОЗУ устройства оценивается через `navigator.deviceMemory`,
  если браузер его предоставляет.
- Разрешение и режим окна никогда не изменяются.

```lua
Settings.AutoDetect()
Settings.Save()             -- при желании сохранить подобранный пресет
```

### Адаптивное качество (динамическая подстройка под FPS)

Когда включено, движок постоянно измеряет сглаженный (EMA по времени)
средний FPS и автоматически снижает качество, если FPS падает ниже целевого,
и восстанавливает его, когда производительность нормализуется. Качество на
момент первого снижения фиксируется как *база*; восстановление всегда
возвращает ровно к этой базе. Уровни деградации (относительно базы):
1. Понижение режима AA на шаг (SSAA 4x → SSAA 2x → FXAA; любой MSAA → FXAA).
2. То же понижение AA плюс масштаб рендера −0.15.
3. AA выключается, масштаб рендера −0.30 (масштаб не опускается ниже 0.25).

Уровни, которые ничего бы не изменили (например, база уже FXAA или Off),
пропускаются автоматически.

Детали:
- Снижение происходит, когда средний FPS падает ниже `85%` эффективной цели;
  восстановление — когда превышает `97%`.
- Эффективная цель автоматически ограничивается лимитом FPS, а при включённом
  VSync (на мобильных и в вебе — всегда) ещё и частотой обновления дисплея,
  поэтому недостижимая цель не загоняет качество в минимум.
- Кулдауны: 2 с после снижения, 4 с после восстановления. При осцилляции
  (снижение вскоре после восстановления) кулдаун восстановления
  экспоненциально растёт до 32 с.
- Подвисания кадра дольше 0.5 с (загрузка, alt-tab) игнорируются.
- Ручное изменение AA или масштаба рендера (Lua, редактор, `AutoDetect`) при
  активной деградации перебазирует систему на новые значения, а не
  откатывает их позже.
- `Settings.Save()` всегда сохраняет базовое качество, а не временно
  сниженное.

```lua
-- Включить с целевым FPS (без targetFPS сохраняется текущая цель)
Settings.SetAdaptiveQuality(true, 60)
Settings.SetAdaptiveQuality(false)            -- выключить (цель сохраняется)

local enabled = Settings.IsAdaptiveQualityEnabled()
local target  = Settings.GetAdaptiveQualityTargetFPS()
```

### Режим низкой задержки (Low Latency Mode)

Ограничивает опережение CPU перед GPU одним кадром. На бэкендах семейства
OpenGL (OpenGL 4.6/3.3, GLES 3.2, ANGLE) это реализовано так: в конце каждого
кадра ставится GPU-фенс, ожидание которого происходит в начале следующего
кадра. Если VSync тоже включён, движок запрашивает у драйвера **адаптивный
VSync** (`SDL_GL_SetSwapInterval(-1)`) и тихо откатывается к обычному VSync,
если платформа адаптивный режим не поддерживает.

На бэкендах Vulkan / MoltenVK то же ограничение работает нативно: цикл кадра
ждёт **все** фенсы кадров в полёте перед началом нового кадра (один кадр в
полёте вместо двух), а режим презентации свапчейна следует настройке — при
включённом VSync предпочитается `FIFO_RELAXED` (Vulkan-эквивалент адаптивного
VSync, с откатом к `FIFO`), при выключенном VSync предпочитается `IMMEDIATE`
вместо `MAILBOX` ради минимальной задержки. Переключение режима автоматически
пересоздаёт свапчейн.

В веб-сборках (WebGL2 / WebGPU) темпом кадров управляет браузер и блокирующие
ожидания запрещены, поэтому там эта настройка не имеет эффекта.

Эффект на 144 FPS с включенным VSync:
- По умолчанию: CPU может уйти вперёд на 3 кадра → ~21 мс задержки ввода.
- Low Latency ON: CPU опережает максимум на 1 кадр → ~7 мс задержки ввода.

Компромисс: на очень слабых GPU CPU и GPU выполняются более последовательно,
из-за чего пиковая пропускная способность может слегка снизиться. Включать
рекомендуется для динамичных игр на высоком FPS; для кинематографичных игр
или игр с большим количеством катсцен плавность важнее времени реакции.

Значение сохраняется и в `Config/Engine.json` (значение по умолчанию редактора),
и в пользовательском файле настроек игры (`SaveToFile()`).

```lua
Settings.SetLowLatencyMode(true)         -- включить
Settings.SetLowLatencyMode(false)        -- выключить (по умолчанию)

local on = Settings.IsLowLatencyMode()
```

Слушатель `Settings.OnSettingChanged` срабатывает с ключом
`"LowLatencyMode"`, когда это значение меняется.

### Приостановка (Suspend) — автопауза, когда окно неактивно

Если включено (по умолчанию), движок приостанавливает себя, пока его окно
не в фокусе, свернуто, скрыто или приложение ушло в фон: update и render
пропускаются, игровая логика замораживается, звук останавливается. Как
только окно снова становится активным, всё возобновляется ровно с того же
места. Так это работает на всех 6 платформах (Windows, Linux, macOS, iOS,
Android, Web), и редактор ведёт себя так же.

Если выключено, автоматическая приостановка по состоянию окна полностью
отключается — игра продолжает обновляться, рендериться и воспроизводить
звук в фоне, как будто системы приостановки не существует. Ручное API
`SuspendApp()` / `ResumeApp()` при этом продолжает работать независимо от
этой настройки.

Платформенные заметки:
- **Windows / Linux / macOS** — игра просто продолжает работать без фокуса
  или в свернутом виде (кадр в свернутое окно нулевого размера безопасно
  пропускается рендером).
- **Android** — SDL фиксирует OS-уровневый флаг «блокировать цикл событий
  при паузе» один раз при старте, поэтому фоновое выполнение для текущего
  запуска определяется значением, сохранённым в `Config/Engine.json` →
  `Optimization.IsSuspended`. Рантайм-вызов `SetIsSuspended` сразу же
  применяет гейтинг звука/кадров, но снятие OS-уровневой блокировки
  вступит в силу со следующего запуска. Фоновое выполнение — best-effort:
  ОС всё равно может тормозить или убить процесс.
- **iOS** — при выключенной настройке приложение продолжает работать при
  кратковременных потерях фокуса (пункт управления, шторка уведомлений,
  предпросмотр в переключателе приложений); полный уход в фон по-прежнему
  замораживается самой ОС, как у любого iOS-приложения.
- **Web** — при выключенной настройке звук продолжает играть при скрытой
  вкладке, а игра продолжает работать, когда окно просто теряет фокус;
  полностью скрытая вкладка всё же перестаёт рендериться, потому что
  браузер троттлит `requestAnimationFrame` — это вне контроля движка.

Настройка живёт во вкладке **Настройки → Оптимизация** редактора,
сохраняется в `Config/Engine.json` (`Optimization.IsSuspended`, по
умолчанию `true`) и в пользовательском файле настроек игры (`SaveToFile()`).

```lua
Settings.SetIsSuspended(true)          -- по умолчанию: автоприостановка включена
Settings.SetIsSuspended(false)         -- продолжать работать в фоне

local sus = Settings.IsSuspended()
```

`OnSettingChanged` срабатывает с ключом `"IsSuspended"`.

### Прогрев проекта (загрузка всех ассетов на старте)

Если включено, движок при старте сканирует папку `Content/` и ставит в очередь
все спрайты, флипбуки, скелеты, материалы, инстансы материалов, эффекты частиц,
тайлмапы, тайлсеты, UI-виджеты, шрифты, графы анимаций и звуки на предзагрузку.
Каждый ассет прогревается через тот же загрузчик, что использует игра в
рантайме, поэтому текстуры попадают в правильный атлас с применёнными
настройками импорта `.ice_texture` — без дублирующих standalone-копий и без
рывка при первом использовании. Загрузка происходит постепенно по последующим
кадрам с малым бюджетом (~2 мс на кадр), поэтому не блокирует первый
отрисованный кадр.

Используй если у тебя **нет** загрузочных экранов между сценами — как только
очередь обработается, переходы на новые сцены никогда больше не словят первый
компайл/декод/загрузку ассета.

Если у тебя ЕСТЬ загрузочные экраны — оставь ВЫКЛ и используй Lua API
`Prewarm.*` на экране загрузки. Это более точечный подход и грузит только то
что нужно следующей сцене.

Значение сохраняется в `Config/Engine.json` (`Engine.ProjectPrewarmOnStartup`)
и в пользовательском файле настроек игры.

```lua
Settings.SetProjectPrewarm(true)
Settings.SetProjectPrewarm(false)             -- по умолчанию

local on = Settings.IsProjectPrewarm()
```

`OnSettingChanged` срабатывает с ключом `"ProjectPrewarm"`.

### Prewarm.* — ручное API предзагрузки

`Prewarm.*` — глобально доступное пространство имён для постановки ассетов в
очередь на постепенную предзагрузку без блокирования главного потока.
Типичное использование — внутри сцены экрана загрузки.

Очередь обрабатывается на тике движка с бюджетом ~2 мс на кадр. Декодинг
PNG идёт на воркер-потоках; GL upload и компиляция шейдеров — на главном
потоке, ограничены бюджетом.

```lua
-- Поставить один ассет в очередь (дедупликация — два одинаковых пути бесплатно)
Prewarm.Sprite("Content/Sprites/Player/SP_T_PlayerIdle.ice_sprite")
Prewarm.Flipbook("Content/Anims/FB_Explosion.ice_flipbook")
Prewarm.Skeleton("Content/Skeletons/SK_Hero.ice_skeleton")
Prewarm.Material("Content/Materials/M_Hologram.ice_material")
Prewarm.MaterialInstance("Content/Materials/MI_HologramBlue.ice_matinst")
Prewarm.FX("Content/FX/FX_Explosion.ice_fx")
Prewarm.Tilemap("Content/Levels/Forest/TM_Forest.ice_tm")
Prewarm.Tileset("Content/Tilesets/TS_Grass.ice_ts")
Prewarm.Widget("Content/UI/HUD.ice_widget")
Prewarm.Font("Content/Fonts/Roboto.ttf")
Prewarm.Animation("Content/Anims/AC_Player.ice_animation")
Prewarm.View("Content/Views/V_Underwater.ice_view")
Prewarm.Sound("Content/Audio/sfx_explosion.wav")
Prewarm.Texture("Content/UI/bg_titlescreen.png")  -- сырое изображение (в атлас если влезает, иначе standalone)

-- Поставить в очередь целую папку (рекурсивно). Распознаются расширения:
--   .ice_sprite, .ice_flipbook, .ice_skeleton, .ice_material, .ice_matinst,
--   .ice_fx, .ice_tm, .ice_ts, .ice_widget, .ice_animation, .ice_view,
--   .ttf, .otf, .ttc, .wav, .ogg, .mp3, .flac,
--   .png, .jpg, .jpeg, .bmp, .tga
Prewarm.Folder("Content/Levels/Forest")

-- Прогресс (вызывай из Update экрана загрузки)
local progress = Prewarm.GetProgress()    -- 0.0 .. 1.0 (стопится на 0.99 пока есть GPU uploads)
local ready    = Prewarm.IsComplete()     -- true когда очередь пуста И все GL uploads готовы
local pending  = Prewarm.GetQueueSize()   -- сколько ещё в очереди

-- Детальная статистика
local stats = Prewarm.GetStats()
print(stats.queued, stats.completed, stats.failed, stats.pending_uploads)

-- Отменить всё что ещё не обработано (уже загруженные ассеты остаются в кеше)
Prewarm.Clear()

-- Поставить в очередь собственный список прогрева проекта (ассеты, заданные
-- в настройках проекта) — тот же список, который движок греет автоматически при старте.
Prewarm.RunProjectPrewarm()
```

**Пример: Lua экран загрузки**

```lua
-- В виджете загрузочного экрана OnReady():
function OnReady()
    Prewarm.Folder("Content/Levels/Forest")
end

function OnUpdate(dt)
    local p = Prewarm.GetProgress()
    SetWidgetText("progress_label", string.format("Загрузка %d%%", math.floor(p * 100)))
    if Prewarm.IsComplete() then
        LoadLevel("Content/Levels/Forest/TestForestLevel.icemap")
    end
end
```

**Примечания**
- Все пути в `Prewarm.*` нормализуются (`\` → `/`) и резолвятся так же, как движок
  резолвит любой путь к контенту. Можно передавать `Content/...` пути или просто
  имена файлов.
- Каждый ассет прогревается через тот же загрузчик, что использует игра в рантайме,
  поэтому прогретая запись кеша — ровно та, в которую попадёт будущее первое
  использование, включая упаковку в атлас и настройки `.ice_texture` для текстур
  спрайтов.
- Зависимости подтягиваются автоматически и дедуплицируются:
  - флипбук ставит в очередь все кадровые спрайты (и override-материал);
  - спрайт ставит в очередь свой материал / инстанс материала;
  - скелет ставит в очередь спрайты всех аттачментов;
  - эффект частиц ставит в очередь свои спрайты, материалы, sub-FX и event-FX;
  - тайлмап ставит в очередь свои тайлсеты и флипбуки анимированных тайлов;
  - тайлсет прогревает свою текстуру и ставит в очередь материал;
  - виджет ставит в очередь свои спрайты, флипбуки, шрифты, звуки, родителя и
    под-виджеты;
  - граф анимации ставит в очередь флипбук каждого состояния;
  - view прогревает текстуры пост-процесса (небо / LUT / bloom-dirt) под теми же
    ключами, что и рантайм, и компилирует свои кастомные материалы пост-процесса.
- `Prewarm.Texture` (и любое сырое изображение, найденное `Prewarm.Folder`)
  прогревает картинку так же, как это делает спрайт: в общий атлас если она
  влезает (с учётом настроек `.ice_texture`), иначе как standalone-текстуру по
  ключу полного пути. Операция идемпотентна и делит запись кеша с
  соответствующим спрайтом/материалом/тайлсетом, поэтому дублирующая копия не
  создаётся.
- `Prewarm.Material` компилирует шейдер и грузит текстуры материала;
  `Prewarm.MaterialInstance` (или любой путь `.ice_matinst`) дополнительно
  резолвит родительский материал и применяет override-текстуры.
- Очередь переживает переход между сценами — `Prewarm.Clear()` это
  единственный способ отбросить ещё необработанные элементы.

### События изменения настроек

Подпишитесь на колбэк, который вызывается при любом изменении настройки
(через Lua, панель настроек редактора, AutoDetect или AdaptiveQuality).
Колбэк получает имя изменённого ключа (например `"AAMode"`, `"FPSLimit"`,
`"RenderScale"`, `"UpscalingMode"`, `"UpscalingQuality"`, `"UpscalingSharpness"`,
`"UpscalingSharpening"`, `"AdaptiveQuality"`, `"AdaptiveQualityLevel"`, ...).

```lua
local id = Settings.OnSettingChanged(function(key)
    print("Настройка изменена:", key, "→", Settings.GetAAMode())
end)

-- Удалить всех слушателей
Settings.ClearChangeListeners()
```

> **Примечания**
> - Все сеттеры громкости (`SetMasterVolume`, `SetMusicVolume`, `SetSFXVolume`,
>   `SetVoiceVolume`, `SetAmbientVolume`, `SetUIVolume`) ограничивают значение `[0..1]`.
> - `SetWindowMode(mode)` принимает только `"windowed"`, `"fullscreen"`, `"borderless"`;
>   любое другое значение отклоняется с предупреждением.
> - HDR10 автоматически отключается на GLES-бэкендах независимо от запрошенного значения.

---

## 25. Math — Математика и шум

> **Тип:** Глобальные функции

### Типы данных

```lua
-- Вектор 2D
local v = Vec2(1.0, 2.0)
v.x = 3.0
v.y = 4.0

-- Вектор 3D
local v3 = Vec3(1, 2, 3)

-- Вектор 4D
local v4 = Vec4(1, 2, 3, 4)

-- Цвет (RGBA)
local c = Color(1, 0, 0)         -- Красный (a=1 по умолчанию)
local c = Color(1, 0, 0, 0.5)    -- Полупрозрачный красный

-- Прямоугольник
local r = Rect(10, 20, 100, 50)  -- x, y, w, h

-- Transform (позиция, поворот, масштаб)
local t = Transform(Vec3(0, 0, 0), 0, Vec2(1, 1))
t.position = Vec3(10, 20, 0)
t.rotation = 45
t.scale = Vec2(2, 2)

-- Методы векторов
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

### Цветовые утилиты

| Функция | Описание |
|---------|----------|
| `ColorFromHex(hex)` | Hex-строка → `{r, g, b, a}` (0–1) |
| `ColorToHex(r, g, b, a?)` | RGB → Hex-строка |
| `HSVToRGB(h, s, v)` | HSV → `{r, g, b}` |
| `RGBToHSV(r, g, b)` | RGB → `{h, s, v}` |
| `Clamp01(value)` | Ограничить значение в диапазоне [0, 1] |

**Предустановленные цвета** (`Colors.*`):

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

-- Из Hex
local c = ColorFromHex("#FF5500")
SetColor(c.r, c.g, c.b, 1)

-- В Hex
local hex = ColorToHex(1, 0.5, 0, 1)  -- → "#FF8000FF"

-- HSV
local rgb = HSVToRGB(120, 1, 1)        -- зелёный: h=120°, s=1, v=1
local hsv = RGBToHSV(1, 0, 0)          -- красный: h=0°, s=1, v=1

-- Preset
SetColor(Colors.Red.r, Colors.Red.g, Colors.Red.b, 1)

-- Clamp01
local t = Clamp01(1.5)  -- → 1.0
```

### Математические функции

```lua
-- Ограничение
local v = Clamp(value, 0, 100)

-- Интерполяция
local v = Lerp(0, 100, 0.5)  -- = 50

-- Знак (-1, 0, 1)
local s = Sign(-5)  -- = -1

-- Модуль
local a = Abs(-5)  -- = 5

-- Мин/Макс
local mi = Min(a, b)
local ma = Max(a, b)

-- Округление
local f = Floor(3.7)   -- = 3
local c = Ceil(3.2)    -- = 4
local r = Round(3.5)   -- = 4

-- Корень и степень
local sq = Sqrt(16)     -- = 4
local pw = Pow(2, 10)   -- = 1024

-- Обратная интерполяция и remap
local t = InverseLerp(0, 100, 25)           -- → 0.25
local v = Remap(25, 0, 100, 0, 1)           -- → 0.25

-- Векторные функции (float)
local len = Length(x, y)
local dot = Dot(x1, y1, x2, y2)
local dist = Distance(x1, y1, x2, y2)
local distSq = DistanceSq(x1, y1, x2, y2)
local n = Normalize(x, y)                   -- → {x, y}

-- Движение и сглаживание
local v = MoveTowards(current, target, 5)
local v = SmoothDamp(current, target, 0.2, dt)
local v = Approach(current, target, 2)
local a = WrapAngle(angle)

-- Тригонометрия и углы (градусы)
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

### Случайные числа

```lua
local r = Random()                   -- 0..1 (float)
local r = RandomRange(10.0, 50.0)    -- 10..50 (float)
local r = RandomInt(1, 6)            -- 1..6 (int, как кубик)
local b = RandomBool()               -- true/false (50/50)
local b = RandomBool(0.3)            -- true с шансом 30%

-- Выбрать случайный элемент из массива
local item = RandomChoice({"Sword", "Shield", "Potion"})

-- Взвешенный случайный выбор (по весам)
local index = RandomWeighted({10, 5, 1})  -- Первый в 10 раз вероятнее третьего

-- Задать seed (для воспроизводимости)
SetRandomSeed(42)
```

> **Детерминизм (новое):** все функции выше теперь работают на детерминированном генераторе
> движка. Достаточно один раз вызвать `SetRandomSeed(n)` (или `RNG.SetSeed`) — и **каждый** вызов
> `Random*`, выпадение лута, перемешивание массива и случайный выбор в Behavior Tree станут
> полностью воспроизводимыми. Это фундамент для seed-забегов рогаликов и реплеев.
> `SetRandomSeed` по умолчанию также пересевает таблицы шума. Про именованные под-потоки,
> строковые сиды и точное сохранение/загрузку см.
> [RNG — Детерминированные потоки случайных чисел](#rng--детерминированные-потоки-случайных-чисел).

> **Стандартный `math.random` теперь тоже идёт сюда.** `math.random()`, `math.random(n)`,
> `math.random(m, n)` и `math.randomseed(s)` работают на том же детерминированном генераторе и
> сохраняют привычную семантику Lua (без аргументов — float в `[0,1)`, иначе целое в диапазоне).
> Для роллбэк-нетокода это принципиально: собственный генератор Lua не попадал бы в снапшот
> роллбэка и разъехался бы на первом же пересимулированном кадре. В коде менять ничего не нужно —
> существующие вызовы просто стали воспроизводимыми.

### Шум Перлина и другие

```lua
-- Perlin Noise (1D и 2D)
local n = Noise(x)             -- 1D
local n = Noise(x, y)          -- 2D
local n = PerlinNoise(x, y)    -- Аналог Noise

-- Simplex Noise (2D)
local n = SimplexNoise(x, y)

-- FBM (Fractal Brownian Motion) — для ландшафтов и облаков
local n = FBMNoise(x, y)
local n = FBMNoise(x, y, 6, 2.0, 0.5)  -- octaves, lacunarity, gain

-- Ridged Noise — горные хребты
local n = RidgedNoise(x, y)
local n = RidgedNoise(x, y, 6, 2.0, 0.5)

-- Turbulence Noise — турбулентность
local n = TurbulenceNoise(x, y)
local n = TurbulenceNoise(x, y, 6, 2.0, 0.5)

-- Voronoi Noise — ячейки (по умолчанию = евклидова F1-дистанция)
local n = VoronoiNoise(x, y)

-- Voronoi Noise (расширенный) — таблица опций
-- metric:     "euclidean" (по умолчанию) | "manhattan" | "chebyshev"
-- returnMode: "F1" (по умолчанию) | "F2" | "F2_F1" | "cellId"
local f1     = VoronoiNoise(x, y, { metric = "manhattan" })
local edge   = VoronoiNoise(x, y, { returnMode = "F2_F1" })  -- маска граней ячеек/кристаллов
local cellId = VoronoiNoise(x, y, { returnMode = "cellId" }) -- целочисленный ID ячейки
local f2     = VoronoiNoise(x, y, { metric = "chebyshev", returnMode = "F2" })

-- NoiseMap — 2D карта шума (таблица значений 0..1)
-- результат — Lua-таблица: result[1..height][1..width], строка 1 = низ (y=0), строка H = верх
local map = NoiseMap(64, 64, 50.0)
local map = NoiseMap(64, 64, 50.0, 6, 2.0, 0.5, 123)

-- Реседирование таблицы перестановок (влияет на ВСЕ функции шума:
-- Noise/PerlinNoise/SimplexNoise/FBMNoise/RidgedNoise/TurbulenceNoise/VoronoiNoise/NoiseMap/NoiseBuffer)
ReseedNoise(12345)
SetNoiseSeed(12345)  -- алиас

-- Domain warping — искажение координат шумом Perlin, чтобы убрать «решёточные» артефакты
-- Возвращает { x = warpedX, y = warpedY }
local w = DomainWarp(x, y, 4.0)
local val = PerlinNoise(w.x, w.y)

-- Curl-шум (2D, бездивергентное векторное поле) — идеален для жидкости/дыма
-- Возвращает { x = vx, y = vy }; необязательный eps управляет шагом производной (по умолчанию 0.01)
local v = CurlNoise2D(x, y)
local v = CurlNoise2D(x, y, 0.005)

-- NoiseBuffer — нативный буфер float'ов (быстрее NoiseMap, индексация с 0)
-- Использовать для процедурной генерации, когда нужно семплировать/менять большие сетки
-- без создания Lua-таблицы под каждую ячейку.
local buf = NoiseBuffer.new(256, 256)
buf:Fill(40.0)                              -- fBm с дефолтами octaves=6, lac=2, gain=0.5, seed=0
buf:Fill(40.0, 8, 2.0, 0.5, 12345)          -- scale, octaves, lacunarity, gain, seed
local w = buf:Width()
local h = buf:Height()
local v = buf:Get(x, y)                     -- 0-based, выход за границы → 0
buf:Set(x, y, 0.5)                          -- 0-based, выход за границы — no-op
buf:Clear(0.0)
local lo, hi = buf:Min(), buf:Max()
buf:Normalize()                             -- min..max → 0..1 на месте
local tbl = buf:ToTable()                   -- экспорт в Lua-таблицу[1..h][1..w] (строка 1 = низ, строка H = верх)
```

### Дополнительные math-функции

```lua
-- Приблизительное сравнение
IsNearlyEqual(3.0001, 3.0, 0.001)  -- → true
IsNearlyZero(0.00001)              -- → true
IsNearlyZero(0.1, 0.5)            -- → true (с указанием порога)

-- Привязка к сетке
SnapToGrid(47, 32)                 -- → 48 (ближайшее кратное 32)
local pos = SnapToGrid2D(47, 63, 32)  -- → {x=48, y=64}

-- Remap с clamp (не выходит за диапазон)
MapRangeClamped(150, 0, 100, 0, 1)  -- → 1.0 (clamp)

-- Радианы/градусы
local rad = DegToRad(180)
local deg = RadToDeg(3.14159)

-- Модулярные функции
local r = Fmod(10, 3)              -- → 1
local p = PingPong(time, 2.0)      -- 0..2..0
local s = SmoothStep(0, 1, 0.5)    -- → 0.5

-- Геометрические проверки
local cross = Cross2D(x1, y1, x2, y2)
local refl = Reflect(x, y, nx, ny)        -- → {x, y}
local inside = PointInBox(px, py, bx, by, bw, bh)
local inside = PointInCircle(px, py, cx, cy, radius)
local hit = BoxOverlapsBox(ax, ay, aw, ah, bx, by, bw, bh)
local hit = CircleOverlapsCircle(ax, ay, ar, bx, by, br)
local c = LerpColor(r1, g1, b1, a1, r2, g2, b2, a2, t)

-- Кривые Безье
local p = QuadraticBezier(0.5, 0,0, 50,100, 100,0)  -- t, p0, p1, p2
-- p.x, p.y

local p = CubicBezier(0.5, 0,0, 30,80, 70,80, 100,0)  -- t, p0, p1, p2, p3
-- p.x, p.y

-- Сплайн Catmull-Rom (проходит через все точки)
local p = CatmullRom(0.5, 0,0, 10,50, 60,50, 100,0)
-- p.x, p.y

-- Easing функции
EaseIn(0.5)         -- → 0.25 (квадратичная)
EaseOut(0.5)        -- → 0.75
EaseInOut(0.5)      -- → 0.5
EaseInCubic(0.5)    -- → 0.125
EaseOutCubic(0.5)   -- → 0.875
EaseInOutCubic(0.5)
EaseInElastic(0.5)
EaseOutElastic(0.5)
EaseOutBounce(0.5)

-- Циклический wrap
Wrap(370, 0, 360)    -- → 10
Wrap(-10, 0, 360)    -- → 350

-- Пружинная физика
local result = Spring(current, target, velocity, stiffness, damping, dt)
-- result.value    = новое значение
-- result.velocity = новая скорость

-- Целая/дробная часть
Truncate(3.7)  -- → 3.0
Frac(3.7)      -- → 0.7

-- Логарифмы и экспонента
Log(2.718)    -- → ~1.0 (натуральный)
Log2(8)       -- → 3.0
Log10(1000)   -- → 3.0
Exp(1)        -- → ~2.718
```

### Пространственный рандом

```lua
-- Случайная точка ВНУТРИ круга (равномерное распределение)
local p = RandomPointInCircle(cx, cy, radius)
-- p.x, p.y

-- Случайная точка НА окружности (периметр)
local p = RandomPointOnCircle(cx, cy, radius)
-- p.x, p.y

-- Случайная точка в прямоугольнике
local p = RandomPointInBox(x, y, width, height)
-- p.x, p.y

-- Случайный единичный вектор (направление)
local dir = RandomUnitVector()
-- dir.x, dir.y (длина = 1)
```

### 2D Интерполяция

```lua
-- Линейная интерполяция двух точек
local p = Lerp2D(x1, y1, x2, y2, 0.5)
-- p.x, p.y

-- MoveTowards для 2D (ограничение шага)
local p = MoveTowards2D(curX, curY, targetX, targetY, maxDelta)
-- p.x, p.y

-- SmoothDamp для 2D (плавное приближение)
local p = SmoothDamp2D(curX, curY, targetX, targetY, smoothTime, dt)
-- p.x, p.y

-- Независимая от FPS интерполяция
local val = InterpTo(current, target, dt, speed)
-- speed = скорость приближения (5.0 = быстро, 0.5 = медленно)

-- 2D версия InterpTo
local p = InterpTo2D(curX, curY, targetX, targetY, dt, speed)
-- p.x, p.y
```

### 2D Геометрия

```lua
-- Ближайшая точка на отрезке AB к точке P
local closest = ClosestPointOnLine(px, py, ax, ay, bx, by)
-- closest.x, closest.y

-- Расстояние от точки до отрезка
local dist = DistanceToLine(px, py, ax, ay, bx, by)

-- Проекция вектора на другой вектор
local proj = ProjectOnto(vx, vy, ax, ay)
-- proj.x, proj.y

-- Перпендикуляр (поворот на 90°)
local perp = Perpendicular(x, y)
-- perp.x, perp.y → (-y, x)

-- Пересечение двух отрезков AB и CD
local hit = LineIntersection(ax, ay, bx, by, cx, cy, dx, dy)
-- hit.hit = true/false
-- hit.x, hit.y = точка пересечения
-- hit.t = параметр на отрезке AB (0..1)
-- hit.u = параметр на отрезке CD (0..1)

-- Знаковый угол между двумя векторами (положительный = против часовой)
local angle = SignedAngle(x1, y1, x2, y2)

-- Плавный поворот угла к цели (с ограничением скорости)
local newAngle = RotateTowards(currentAngle, targetAngle, maxDeltaDeg)
```

### Целочисленные утилиты

```lua
CeilToInt(3.2)    -- → 4
FloorToInt(3.8)   -- → 3
RoundToInt(3.5)   -- → 4

IsPowerOfTwo(8)       -- → true
IsPowerOfTwo(12)      -- → false
NextPowerOfTwo(12)    -- → 16
NextPowerOfTwo(64)    -- → 64
```

### RNG — Детерминированные потоки случайных чисел

> **Тип:** Глобальный модуль `RNG.*` + объекты `RandomStream`

`RNG` — это общедвижковый **детерминированный** генератор случайных чисел (xoshiro256\*\* с
рассевом через SplitMix64). Как только задан мастер-сид, каждый забег полностью воспроизводим:
один и тот же сид всегда даёт одно и то же подземелье, лут и решения ИИ. Это основа для
seed-забегов рогаликов, ежедневных испытаний (daily challenge) и реплеев.

> Все старые помощники (`Random()`, `RandomRange`, `RandomInt`, `RandomBool`, `RandomChoice`,
> `RandomWeighted`, `RandomPointInCircle`, … а также `Array.Shuffle`, выпадение лута из
> лут-таблиц и случайные селекторы Behavior Tree) теперь идут через этот же генератор — поэтому
> один `RNG.SetSeed(...)` делает воспроизводимым **всё** сразу.

#### Установка сида

```lua
-- Числовой сид (удобно для UI «введите сид»; целые до 2^53 точны в Lua)
RNG.SetSeed(12345)

-- Строковый сид (полные 64 бита энтропии — лучший вариант для общих кодов-сидов / daily)
RNG.SetSeed("ICEBOX-DAILY-2026-06-10")

-- 2-й аргумент (по умолчанию = true) также пересевает таблицы шума Перлина/Симплекс,
-- поэтому террейн на шуме воспроизводим от того же мастер-сида.
RNG.SetSeed(12345, true)    -- пересев RNG + шум (по умолчанию)
RNG.SetSeed(12345, false)   -- пересев только RNG, таблицы шума не трогаются

local seed   = RNG.GetSeed()    -- внутренний 64-битный мастер-сид числом (может терять точность
                                -- выше 2^53 — чтобы ПОДЕЛИТЬСЯ сидом, храните исходное
                                -- значение/строку, которые передали в SetSeed)
local seeded = RNG.IsSeeded()   -- true, как только мастер-сид задан
RNG.Reset()                     -- забыть мастер-сид и все потоки (снова недетерминированно)
```

#### Поток по умолчанию (быстрые броски)

```lua
RNG.Value()                 -- float 0..1
RNG.Range(10.0, 50.0)       -- float в [min, max]
RNG.RangeInt(1, 6)          -- int в [min, max], без смещения (no modulo bias) — как d6
RNG.Int(1, 6)               -- псевдоним RangeInt
RNG.Bool()                  -- true/false (50%)
RNG.Bool(0.25)              -- true с шансом 25%
RNG.Sign()                  -- -1.0 или +1.0
RNG.Index(count)            -- 1..count (с единицы — безопасно как индекс массива Lua)
RNG.Angle()                 -- 0..360 (градусы)

RNG.Choice({"a", "b", "c"})            -- случайный элемент массива
RNG.Weighted({10, 5, 1})               -- взвешенный индекс (1 в 10 раз вероятнее 3)
RNG.Shuffle(myArray)                   -- перемешивание на месте (Фишер–Йетс)
local dir = RNG.UnitVector()           -- {x, y}, длина 1
local p   = RNG.PointInCircle(radius)  -- {x, y}, равномерно внутри круга
```

#### Именованные потоки (независимые, развязанные под-генераторы)

Именованные потоки — ключевой приём для стабильных сидов: дайте каждой подсистеме свой поток,
чтобы один лишний бросок лута никогда не сдвигал спавн монстров или планировку уровня. Каждый
именованный поток детерминированно выводится из мастер-сида.

```lua
local loot   = RNG.Stream("loot")
local mobs   = RNG.Stream("monsters")
local layout = RNG.Stream("layout")

local item  = loot:Choice(itemPool)
local hp    = mobs:RangeInt(10, 20)
local rooms = layout:RangeInt(5, 9)

-- У объекта-потока тот же набор методов, что и у потока по умолчанию RNG.*:
--   s:Value()  s:Range(a,b)  s:RangeInt(a,b)  s:Int(a,b)  s:Bool(p)  s:Sign()
--   s:Index(n) s:Angle()  s:Choice(t)  s:Weighted(t)  s:Shuffle(t)
--   s:UnitVector()  s:PointInCircle(r)  s:GetSeed()  s:Name()  s:Reset()

RNG.HasStream("loot")      -- создан ли уже этот поток?
RNG.ResetStream("loot")    -- перемотать один поток к его выведенному началу
```

> Хэндлы потоков безопасно хранить в локальных переменных и полях — внутри они резолвятся по
> имени, поэтому продолжают работать после `RNG.LoadState`, `RNG.Reset` и `RNG.SetSeed`.

#### Сохранение / загрузка (точное возобновление)

```lua
local blob = RNG.SaveState()    -- сериализовать мастер-сид + точную позицию каждого потока
-- ... сохранить blob в файл сохранения ...
RNG.LoadState(blob)             -- восстановить — следующий бросок продолжится бит-в-бит
```

#### Пример: воспроизводимый генератор этажа рогалика

```lua
function GenerateFloor(depth)
    -- один воспроизводимый поток на этаж; depth держит этажи независимыми
    local r = RNG.Stream("floor." .. depth)
    local roomCount = r:RangeInt(6, 10)
    for i = 1, roomCount do
        local w     = r:RangeInt(4, 9)
        local h     = r:RangeInt(4, 9)
        local theme = r:Weighted({60, 30, 10})   -- 1 = обычная, 2 = сокровищница, 3 = магазин
        SpawnRoom(w, h, theme)
    end
end
```

---

## 26. Events — Система событий

> **Тип:** Глобальные функции
>
> Позволяет сущностям общаться друг с другом без прямых ссылок.

```lua
-- Подписаться на событие
local listenerId = On("PlayerDied", function(...)
    Print("Игрок погиб!")
end)

-- Отправить событие (все слушатели получат)
Emit("PlayerDied")
Emit("DamageDealt", 50, "fire")  -- С аргументами

-- Отписаться
Off(listenerId)                   -- По ID слушателя
Off("PlayerDied")                 -- Все слушатели этого события
Off("PlayerDied", listenerId)     -- Конкретный слушатель события
```

### Пример: Система урона

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
        Emit("DamageAll", 25)  -- Урон всем подписчикам
    end
end
```

---

## 27. Gameplay — Игровые системы

> **Тип:** Глобальные функции

### Object Pool (Пул объектов)

Паттерн для оптимизации: вместо постоянного создания/удаления объектов, переиспользуем их.

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
    preload = 10    -- Создать 10 заранее
})

-- Получить из пула
local bullet = bulletPool.Get()
SetEntityPosition(bullet, x, y)
SetEntityVelocity(bullet, 500, 0)

-- Вернуть в пул
bulletPool.Release(bullet)

-- Информация
local active = bulletPool.GetActiveCount()
local inactive = bulletPool.GetInactiveCount()
local total = bulletPool.GetTotalCount()

-- Очистить пул
bulletPool.Clear()

-- Перебрать активные
bulletPool.ForEachActive(function(obj)
    -- обработка
end)
```

### Wave System (Волны врагов)

```lua
local waves = WaveSystem({
    betweenWaveDelay = 3.0,   -- Задержка между волнами
    waves = {
        { count = 5,  interval = 0.5 },   -- Волна 1: 5 врагов с интервалом 0.5с
        { count = 10, interval = 0.3 },   -- Волна 2: 10 врагов с интервалом 0.3с
        { count = 20, interval = 0.2 }    -- Волна 3: 20 врагов
    },
    onWaveStart = function(waveNum)
        Print("Волна " .. waveNum .. " начинается!")
    end,
    onSpawn = function(waveNum, spawnIndex, waveConfig)
        SpawnEntity("Content/Classes/Enemy.ice_class",
            RandomRange(-300, 300), -500)
    end,
    onWaveEnd = function(waveNum)
        Print("Волна " .. waveNum .. " завершена!")
    end,
    onAllComplete = function()
        Print("Все волны пройдены!")
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

### Cooldown (Откат способности)

```lua
local dashCooldown = Cooldown(2.0)  -- 2 секунды отката

function OnUpdate(dt)
    dashCooldown.Update(dt)

    if IsKeyJustPressed("shift") then
        if dashCooldown.Use() then  -- true если откат прошёл
            -- Выполнить рывок!
            AddImpulse(500, 0)
            Print("Dash!")
        else
            Print("На откате! Осталось: " .. dashCooldown.GetRemaining() .. "с")
        end
    end

    local ready = dashCooldown.IsReady()
    local progress = dashCooldown.GetProgress()  -- 0..1
    dashCooldown.Reset()  -- Сбросить откат
    dashCooldown.ForceUse()  -- Принудительно запустить откат
    dashCooldown.SetDuration(3.0)  -- Изменить длительность
end
```

### AchievementSystem (Система достижений)

Локальная система достижений с персистентностью. Поддерживает два типа: обычные (`simple`) и инкрементальные (`incremental`), а также скрытые достижения, автосохранение и временные метки разблокировки.

Данные сохраняются в `Saves/` как JSON через `PlatformPaths` — работает на всех платформах (Windows, Linux, Android, Web).

```lua
-- Создание системы
local achievements = AchievementSystem({
    autoSave = true,                        -- Автосохранение при Unlock/AddProgress
    savePath = "achievements.json",          -- Путь в Saves/
    onUnlock = function(id, achievement)
        Print("Открыто: " .. achievement.title)
    end,
    onProgress = function(id, current, target, achievement)
        Print(id .. ": " .. current .. "/" .. target)
    end
})

-- Определение достижений
achievements.Define({
    id = "first_kill",
    title = "Первая кровь",
    description = "Убейте первого врага",
    icon = "Content/Icons/first_kill.png",
    type = "simple"
})

achievements.Define({
    id = "kill_100",
    title = "Массовое уничтожение",
    description = "Убейте 100 врагов",
    type = "incremental",
    target = 100
})

achievements.Define({
    id = "secret_room",
    title = "???",
    description = "Найдите секретную комнату",
    type = "simple",
    hidden = true
})

-- Пакетное определение нескольких достижений сразу
achievements.DefineBatch({
    { id = "level_5", title = "Уровень 5", type = "simple" },
    { id = "coins_500", title = "Богач", type = "incremental", target = 500 }
})

-- Загрузка сохранённого прогресса (вызывать после Define/DefineBatch)
achievements.Load()

-- Разблокировка
achievements.Unlock("first_kill")  -- Возвращает true если впервые

-- Инкрементальный прогресс
achievements.AddProgress("kill_100", 1)   -- +1 (автоматически разблокирует при >= target)
achievements.AddProgress("kill_100")       -- +1 (по умолчанию)
achievements.SetProgress("kill_100", 50)   -- Установить напрямую

-- Запросы
local unlocked = achievements.IsUnlocked("first_kill")   -- true/false
local progress = achievements.GetProgress("kill_100")    -- текущий прогресс
local target = achievements.GetTarget("kill_100")        -- целевое значение
local pct = achievements.GetProgressPercent("kill_100")  -- 0.0..1.0
local ach = achievements.Get("first_kill")               -- таблица достижения

-- Списки (в порядке определения)
local all = achievements.GetAll()
local done = achievements.GetUnlocked()
local left = achievements.GetLocked()

-- Статистика
local total = achievements.GetCount()
local doneCount = achievements.GetUnlockedCount()
local completion = achievements.GetCompletionPercent()  -- 0.0..1.0

-- Сброс
achievements.Reset("first_kill")   -- Сбросить одно
achievements.ResetAll()            -- Сбросить все

-- Персистентность
achievements.Save()                -- Сохранить в Saves/achievements.json
achievements.Save("slot2.json")    -- Сохранить в другой файл
achievements.Load()                -- Загрузить
local exists = achievements.HasSave()     -- Есть ли файл
achievements.DeleteSave()                 -- Удалить файл

-- Коллбэки можно установить/обновить после создания
achievements.SetCallbacks({
    onUnlock = function(id, ach) end,
    onProgress = function(id, cur, target, ach) end
})
```

**Все методы AchievementSystem:**

| Метод | Возвращает | Описание |
|-------|-----------|----------|
| `Define(def)` | — | Определить достижение |
| `DefineBatch(defs)` | — | Определить несколько достижений |
| `Unlock(id)` | bool | Разблокировать (true если впервые) |
| `AddProgress(id, amount?)` | bool | Добавить прогресс (true если разблокировано) |
| `SetProgress(id, value)` | bool | Установить прогресс (true если разблокировано) |
| `IsUnlocked(id)` | bool | Проверка разблокировки |
| `GetProgress(id)` | int | Текущий прогресс (только incremental) |
| `GetTarget(id)` | int | Целевое значение (только incremental) |
| `GetProgressPercent(id)` | float | Прогресс 0.0..1.0 |
| `Get(id)` | table/nil | Таблица достижения |
| `GetAll()` | table | Все достижения (в порядке Define) |
| `GetUnlocked()` | table | Разблокированные |
| `GetLocked()` | table | Заблокированные |
| `GetCount()` | int | Общее количество |
| `GetUnlockedCount()` | int | Количество разблокированных |
| `GetCompletionPercent()` | float | Общий процент 0.0..1.0 |
| `Reset(id)` | — | Сбросить достижение |
| `ResetAll()` | — | Сбросить все достижения |
| `Save(path?)` | bool | Сохранить в файл |
| `Load(path?)` | bool | Загрузить из файла |
| `HasSave(path?)` | bool | Существует ли файл |
| `DeleteSave(path?)` | bool | Удалить файл |
| `SetCallbacks(cbs)` | — | Установить коллбэки |

**Поля достижения** (таблица из `Get(id)`):

| Поле | Тип | Описание |
|------|-----|----------|
| `id` | string | Уникальный идентификатор |
| `title` | string | Название |
| `description` | string | Описание |
| `icon` | string | Путь к иконке |
| `type` | string | `"simple"` или `"incremental"` |
| `hidden` | bool | Скрытое достижение |
| `unlocked` | bool | Разблокировано |
| `unlockTime` | string | Дата/время разблокировки |
| `current` | int | Текущий прогресс (только incremental) |
| `target` | int | Целевое значение (только incremental) |

**Формат JSON** (`Saves/achievements.json`):

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

### SoftRef — Ленивые / Асинхронные ссылки на ассеты

Создаёт ссылку на ассет без немедленной загрузки.
Тип определяется автоматически по расширению файла.

```lua
-- Создать мягкие ссылки (ничего не загружается)
local bossTexture = SoftRef("Content/Textures/Boss.png")           -- авто: "texture"
local bossSound   = SoftRef("Content/Sounds/BossRoar.ogg")         -- авто: "sound"
local config      = SoftRef("Content/Data/config.txt", "file")     -- явный тип

-- Запустить асинхронную загрузку
bossTexture.Load()
bossSound.Load()

-- В OnUpdate:
function OnUpdate(dt)
    if bossTexture.IsLoaded() and bossSound.IsLoaded() then
        -- Безопасно использовать
        SetSpriteTexture(bossTexture.Get())
        PlaySound(bossSound.Get())
    end
end

-- Другие методы:
bossTexture.IsLoading()   -- true пока загружается
bossTexture.IsValid()     -- true если путь не пустой
bossTexture.GetPath()     -- "Content/Textures/Boss.png"
bossTexture.GetType()     -- "texture"
bossTexture.Unload()      -- освободить ресурс
```

| Метод | Возврат | Описание |
|-------|---------|----------|
| `SoftRef(path, type?)` | table | Создать мягкую ссылку (тип определяется по расширению) |
| `:Load()` | — | Начать загрузку ассета |
| `:IsLoaded()` | bool | true когда полностью загружен |
| `:IsLoading()` | bool | true пока загружается |
| `:IsValid()` | bool | true если путь непустой |
| `:Get()` | string | Имя ресурса для использования с API |
| `:GetPath()` | string | Исходный путь к файлу |
| `:GetType()` | string | `"texture"`, `"sound"` или `"file"` |
| `:Unload()` | — | Освободить ресурс |

### Пакетная предзагрузка текстур

```lua
-- Предзагрузить несколько текстур разом
PreloadTextures({
    "Content/Textures/Boss.png",
    "Content/Textures/Arena.png",
    "Content/Textures/Effects.png"
})

-- Проверить конкретную текстуру
local ready = IsTextureLoaded("Content/Textures/Boss.png")

-- Проверить все сразу
local allDone = AreAllTexturesLoaded({
    "Content/Textures/Boss.png",
    "Content/Textures/Arena.png"
})
```

| Функция | Возврат | Описание |
|---------|---------|----------|
| `PreloadTextures(paths)` | — | Запустить асинхронную загрузку всех указанных текстур |
| `IsTextureLoaded(name)` | bool | true если текстура готова |
| `AreAllTexturesLoaded(paths)` | bool | true если все указанные текстуры готовы |

### PersistentTable — Автосохраняемое хранилище данных

Key-value хранилище с сохранением/загрузкой на диск. Идеально для статистики, настроек, достижений.

```lua
local stats = PersistentTable("game_stats", "stats.json")
stats.Load()  -- загрузить с диска если есть

-- Установить / Получить
stats.Set("total_kills", 0)
local kills = stats.Get("total_kills", 0)    -- 0 = значение по умолчанию
stats.Has("total_kills")                      -- true
stats.Remove("temp_key")

-- Арифметика
stats.Increment("total_kills", 1)             -- +1 (int или float)
stats.GetMax("best_score", newScore)          -- записать только если больше
stats.GetMin("best_time", newTime)            -- записать только если меньше

-- Персистентность
stats.Save()                                   -- сохранить на диск
stats.Save("backup.json")                     -- в конкретный файл
stats.IsDirty()                                -- есть несохранённые изменения?
stats.Exists()                                 -- файл сохранения существует?
stats.Delete()                                 -- удалить файл сохранения

-- Авто-сохранение: сохраняет каждые N секунд если есть изменения
stats.AutoSave(30.0)
function OnUpdate(dt)
    stats.Update(dt)  -- необходимо для авто-сохранения
end

-- Утилиты
local data = stats.ToTable()                   -- получить всё как Lua-таблицу
stats.Clear()                                  -- очистить все данные
```

| Метод | Возврат | Описание |
|-------|---------|----------|
| `PersistentTable(name, path?)` | table | Создать персистентное хранилище |
| `:Set(key, value)` | — | Установить значение (string, number, bool) |
| `:Get(key, default?)` | any | Получить значение с дефолтом |
| `:Has(key)` | bool | Проверить наличие ключа |
| `:Remove(key)` | — | Удалить ключ |
| `:Increment(key, amount?)` | — | Добавить к числовому значению |
| `:GetMax(key, value)` | — | Записать только если больше текущего |
| `:GetMin(key, value)` | — | Записать только если меньше текущего |
| `:Save(path?)` | bool | Сохранить на диск |
| `:Load(path?)` | bool | Загрузить с диска |
| `:AutoSave(seconds)` | — | Включить авто-сохранение |
| `:Update(dt)` | — | Тик для авто-сохранения (вызывать в OnUpdate) |
| `:IsDirty()` | bool | Есть несохранённые изменения |
| `:ToTable()` | table | Экспорт всех данных как Lua-таблица |
| `:Clear()` | — | Очистить все записи |
| `:Exists()` | bool | Существует ли файл сохранения |
| `:Delete()` | bool | Удалить файл сохранения |

---

## 28. Localization — Локализация

> **Тип:** Глобальные функции

```lua
-- Загрузить файл локализации
LoadLocalization("Content/Localization/strings.json")

-- Установить язык
SetGameLanguage("ru")
local lang = GetGameLanguage()

-- Получить переведённую строку
local text = Localize("menu_play")   -- → "Играть"

-- С подстановкой аргументов
local text = LocalizeFmt("damage_dealt", 50, "fire")
-- Шаблон: "Нанесено {0} урона типа {1}" → "Нанесено 50 урона типа fire"

-- Доступные языки
local langs = GetAvailableLanguages()  -- → {"en", "ru", "zh"}

-- Проверить ключ
local has = HasLocalizationKey("menu_play")
```

> При смене языка вызывается `OnLanguageChanged(newLang, oldLang)` во всех скриптах сущностей.

---

## 29. Debug — Отладка

> **Тип:** Глобальные функции

```lua
-- Вывод в консоль движка
Print("Привет!")                     -- Информация (белый)
PrintWarning("Осторожно!")           -- Предупреждение (жёлтый)
PrintError("Ошибка!")                -- Ошибка (красный)

-- Вывод на экран (поверх игры)
PrintScreen("FPS: " .. GetFPS())
PrintScreen("Debug", 1, 0, 0, 1, 5.0)
-- text, r, g, b, a, duration, key, scale
-- duration: > 0 секунд, == 0 один кадр, < 0 постоянное сообщение
-- key: >= 0 чтобы перезаписывать запись по ключу (идеально для вызова в тике)
-- scale: множитель размера текста (по умолчанию 1.0)
PrintScreen("HP: 100", 1, 1, 0, 1, 0.0, 1, 1.5)  -- один кадр, key=1, scale 1.5
PrintScreen("Boss spawned!", 1, 0, 0, 1, -1)     -- висит до ClearScreenMessages
PrintScreenEx{ text = "Score: 42", color = {0,1,0,1}, duration = 0, key = 2, scale = 2.0 }
RemoveScreenMessage(1)                           -- удалить по ключу

-- Текст в мире
DrawWorldText("Hello", 100, 200)
DrawWorldText("Alert!", 100, 200, 1, 0, 0, 1, 2.0, 1.5)
-- text, worldX, worldY, r, g, b, a, duration, scale, key
DrawWorldText("HP", 100, 200, 1, 1, 0, 1, 0, 1.0, 7)  -- каждый кадр, key=7
DrawWorldTextEx{ text = "Boss", x = 100, y = 240, color = {1,0,0,1}, duration = 0, key = 8 }
RemoveWorldText(7)                               -- удалить мировой текст по ключу

-- Очистить
ClearScreenMessages()
ClearWorldText()
```

#### Экранный дебаг-принт — полный справочник

| Функция | Сигнатура | Описание |
|---------|-----------|----------|
| `PrintScreen` | `PrintScreen(msg, r?, g?, b?, a?, duration?, key?, scale?)` | Добавляет сообщение в экранный оверлей. По умолчанию: цвет `1,1,1,1`, `duration = 5.0`, `key = -1`, `scale = 1.0`. |
| `PrintScreenEx` | `PrintScreenEx{ text=, msg=, color={r,g,b,a}, r=, g=, b=, a=, duration=, key=, scale=, size= }` | Табличная форма с именованными аргументами. `text` и `msg` — синонимы; `scale` и `size` — синонимы. `color` переопределяет отдельные `r,g,b,a`. |
| `RemoveScreenMessage` | `RemoveScreenMessage(key)` | Удаляет сообщение, добавленное с указанным неотрицательным `key`. Игнорируется при `key < 0`. |
| `ClearScreenMessages` | `ClearScreenMessages()` | Очищает все экранные сообщения (как с ключом, так и без). |
| `SetScreenEnabled` | `SetScreenEnabled(enabled)` | Глобально показывает/скрывает экранный оверлей и мировой текст, не очищая очередь сообщений. Одинаково учитывается во вьюпорте редактора и в билде. По умолчанию включено. |
| `IsScreenEnabled` | `IsScreenEnabled()` | Возвращает, включён ли экранный оверлей сейчас. |

#### Мировой текст — полный справочник

| Функция | Сигнатура | Описание |
|---------|-----------|----------|
| `DrawWorldText` | `DrawWorldText(text, worldX, worldY, r?, g?, b?, a?, duration?, scale?, key?)` | Рисует текст в мировых координатах. По умолчанию: цвет `1,1,1,1`, `duration = 0` (один кадр), `scale = 1.0`, `key = -1`. |
| `DrawWorldTextEx` | `DrawWorldTextEx{ text=, msg=, x=, y=, color={r,g,b,a}, r=, g=, b=, a=, duration=, key=, scale=, size= }` | Табличная форма с именованными аргументами, по аналогии с `PrintScreenEx`. |
| `RemoveWorldText` | `RemoveWorldText(key)` | Удаляет мировой текст с указанным неотрицательным `key`. Игнорируется при `key < 0`. |
| `ClearWorldText` | `ClearWorldText()` | Очищает весь мировой текст. |

`duration` и `key` работают ровно так же, как у `PrintScreen`: `key >= 0` перезаписывает запись с тем же ключом вместо добавления новой — именно это нужно, когда подпись рисуется каждый тик. Мировой текст за пределами экрана отсекается и не стоит ничего.

**Шрифты и платформы:** Оверлей и мировой текст используют системный UI-шрифт ОС, определяемый в рантайме (Segoe UI на Windows, San Francisco на macOS/iOS, DejaVu/Noto/Liberation на Linux, Roboto на Android) — для этого с игрой не поставляется ни один шрифт-ассет. На Web браузер не даёт доступа к файлам шрифтов ОС, поэтому текст оверлея там не рисуется. Оверлей рисуется поверх сцены и игрового UI с выключенным тестом глубины, учитывая размер окна в пикселях; мировой текст следует координатной системе движка Y-вверх / X-вправо.

**Параметры:**

| Аргумент | Тип | По умолчанию | Примечание |
|----------|-----|-------------|------------|
| `msg` / `text` | `string` | — | Текст сообщения. Переносы строк (`\n`) в одном вызове не поддерживаются — используйте несколько вызовов. |
| `r`, `g`, `b`, `a` | `number` | `1.0` | Каналы цвета в `[0..1]`. `a` управляет прозрачностью. Непостоянные записи затухают за последние `min(0.5 с, duration / 2)` — поэтому короткие длительности сначала показываются на полной непрозрачности и всё равно затухают, а не тускнеют всю свою жизнь. Однокадровые записи не затухают никогда. |
| `duration` | `number` | `5.0` | `> 0` — секунды (реального, немасштабированного времени); `== 0` — **ровно один отрисованный кадр** на любом фреймрейте (идеально для каждого тика без ключа); `< 0` — постоянно (висит до `ClearScreenMessages` или `RemoveScreenMessage`). |
| `key` | `integer` | `-1` | `>= 0` — перезаписывает запись с тем же ключом. `-1` — всегда добавляет новую. |
| `scale` / `size` | `number` | `1.0` | Множитель размера текста. `0` или отрицательное значение → `1.0`. |

**Шаблоны:**

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

### Debug Draw — отладочные примитивы

Функции для рисования отладочных линий, кругов, прямоугольников, стрелок и расширенных фигур.
Работают через `PhysicsDebugDraw` и полезны для трассировки, ИИ и визуализации коллайдеров.
Рисуются в **мировых пикселях** и доступны одинаково во вьюпорте редактора, в Debug- и в Release-билдах.

#### Время жизни (`duration`) — одинаковые правила для всех отладочных примитивов

| `duration` | Значение |
|------------|----------|
| `== 0` (по умолчанию) | **Ровно один отрисованный кадр** на любом фреймрейте. Вызывайте каждый тик из `OnUpdate`/`OnLateUpdate` для непрерывного оверлея — ничего не накапливается. |
| `> 0` | Секунды реального (немасштабированного) времени. Подходит для разовых событий, которые нужно успеть увидеть, например маркер попадания на `0.4`. |
| `< 0` | Постоянно — висит до `ClearDebugDraw()` или до остановки Play. |

> `duration` считается в реальном времени и продолжает идти на паузе и при изменённом тайм-скейле; поведение полностью совпадает с `PrintScreen`.

```lua
-- Базовые примитивы
DrawLine(0, 0, 100, 100, 1, 0, 0, 2.0)         -- линия
DrawCircle(50, 50, 25, 0, 1, 0)                 -- круг
DrawRect(10, 10, 80, 40, 0, 0, 1, 1.0)          -- прямоугольник
DrawArrow(0, 0, 50, 50, 1, 1, 0, 0.5, 8)         -- стрелка
DrawFilledRect(10, 10, 80, 40, 0, 1, 0, 0.3, 2.0) -- заполненный прямоугольник (x, y, w, h, r, g, b, a, duration)
DrawSelectionRect(10, 10, 90, 50, 0, 1, 0, 0.15, 2.0)  -- прямоугольник выделения с рамкой + заливкой

-- Расширенные примитивы
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
DrawDebugGrid(400, 300, 32, 10, 10, 0.3, 0.3, 0.3, 0.0)
DrawDebugCoordinateSystem(0, 0, 50.0, 2.0)
```

### Debug Draw (Physics) — расширенные алиасы

```lua
DrawDebugLine(x1, y1, x2, y2, 1, 0, 0, 2.0)
DrawDebugCircle(x, y, radius, 0, 1, 0, 2.0)
DrawDebugBox(cx, cy, halfW, halfH, 0, 0, 1, 2.0)
DrawDebugPoint(x, y, 8.0, 1, 1, 0, 2.0)
DrawDebugArrow(x1, y1, x2, y2, 12.0, 1, 0, 0, 2.0)
DrawDebugCapsule(cx, cy, halfHeight, radius, 0, 1, 0, 2.0)
DrawDebugCross(cx, cy, 15.0, 1, 0, 0, 2.0)
DrawDebugPolygon({{x=0,y=0},{x=50,y=0},{x=50,y=50}}, 0, 1, 1, 2.0)
DrawDebugGrid(cx, cy, 32, 10, 10, 0.3, 0.3, 0.3, 0.0)   -- duration 0 = один кадр
DrawDebugCoordinateSystem(0, 0, 50.0, 2.0)
ClearDebugDraw()
```

> **Внимание к порядку аргументов:** у `DrawArrow(x1, y1, x2, y2, r, g, b, duration, headSize)` параметр `headSize` идёт **последним**, а у `DrawDebugArrow(x1, y1, x2, y2, headSize, r, g, b, duration)` — **первым**. Обе сигнатуры оставлены как есть ради обратной совместимости.

### Lua Script Debugger (текстовый и визуальный)

В IceBox есть **два отладчика уровня исходника**, которые подключаются к живой Lua-ВМ во время Play Mode. У них общий рантайм-бэкенд, но они **взаимоисключающи** — одновременно подключён только один, поэтому за ВМ они не конкурируют:

| Отладчик | Для проектов в режиме… | Точки останова на… | Где находится |
|----------|------------------------|--------------------|---------------|
| **Текстовый** | Code (рукописный Lua) | **строках** кода | панель `Lua Script Debugger` |
| **Визуальный** | Visual (графы узлов) | **узлах** | прямо в редакторе графа |

> Режим кодинга выбирается для проекта в лаунчере, поэтому проект — либо **Code**, либо **Visual**, и обычно используется только один отладчик. Независимо от режима движок не даёт подключиться обоим сразу.

#### Текстовый отладчик — панель `Lua Script Debugger`

Открывается из меню окон редактора. Отлаживает рукописный Lua, встроенный в ассеты:

- **Выбор ассета** — список слева показывает все `.ice_class`, `.ice_widget` и `.icemap`. `Rescan Assets` пересобирает список, поле фильтра сужает его.
- **Подключение/отключение** — `Start Debug` ставит построчный хук, `Stop Debug` снимает его. Подключаться можно до или во время Play.
- **Точки останова** — клик по геттеру строки переключает её. Каждую можно **включить/выключить**, задать **условное выражение** (останов только если выражение истинно), и она считает **число попаданий**. Двойной клик по точке в списке редактирует условие.
- **Управление выполнением** — `Continue`, `Step Over`, `Step Into`, `Step Out` и `Pause` (останов на следующей выполненной строке Lua).
- **Просмотр на паузе** — `Variables` показывает локальные, upvalue, охватывающие области и Globals (таблицы разворачиваются по клику); `Watch` вычисляет произвольные выражения; `Call Stack` перечисляет кадры; журнал фиксирует каждое попадание и шаг. Текущая строка подсвечивается в исходнике.
- **Живые значения** — даже без паузы панель несколько раз в секунду опрашивает локальные значения, чтобы видеть их изменение в реальном времени.

> **Сгенерированный Lua — только для чтения.** Если открыть здесь **визуальный** ассет, вы увидите Lua, скомпилированный из его графа. Панель помечает его как сгенерированный и **не ставит на него построчные точки останова** — отлаживайте граф (ниже). Построчные точки сохраняются в `Config/DebugBreakpoints.json`.

> **Скрипты уровней** выполняются под именем чанка `LevelScript`, поэтому точки останова в `.icemap` сопоставляются с работающим скриптом уровня.

#### Визуальный отладчик — отладка прямо в графе узлов

В проекте на **Visual Scripting** вы отлаживаете **сам граф** — читать сгенерированный Lua не нужно. Всё происходит на холсте редактора графа Class, Widget или Level:

- **Точки останова на узлах** — клик по красной точке в левом верхнем углу узла или ПКМ по узлу → `Add Breakpoint` / `Remove Breakpoint`. Сохраняются по ассетам в `Config/VSBreakpoints.json` и переживают перезапуск.
- **Подсветка активного узла** — при остановке текущий узел **пульсирует**; с включённым `Follow` вид автоматически центрируется на нём.
- **Поток выполнения** — exec-провода, по которым только что прошло управление, **анимируются** бегущими импульсами, и виден путь, который прошла логика.
- **Значения пинов** — на паузе выходные пины показывают свои **живые рантайм-значения** инлайн-бейджами рядом с пином.
- **Тулбар отладки** (сверху холста во время Play) — `Continue`, `Step Over`, `Step Into`, `Step Out`, `Pause`, `Stop`, `Focus`, переключатель `Follow` и индикатор статуса.
- **Вкладка Debug** (снизу, рядом с `Problems`) — `Call Stack` (клик по кадру переходит к его узлу), `Watches` (имена переменных или выражения) и список `Breakpoints` (клик фокусирует узел или очищает все).

> **Компилируйте перед отладкой.** Рантайм исполняет **сохранённый** Lua, а маппинг узел→строка пересобирается из текущего графа — поэтому **сохраните граф перед нажатием Play** (то же правило «сначала compile»). Хук подключается только при наличии хотя бы одной точки останова на узле (или по нажатию `Pause`), поэтому граф без точек работает на полной скорости.

Визуальный отладчик работает на всех трёх поверхностях графа — **Class**, **Widget** и **Level** — и использует тот же движок шагов, что и текстовый, поэтому `Step Over/Into/Out` ведут себя одинаково.

### Отладочные функции: как использовать

Функции `Print*` и `PrintScreen` удобны для быстрой диагностики логики во время Play Mode:

- `Print`, `PrintWarning`, `PrintError` пишут сообщения в консоль редактора.
- `PrintScreen` выводит текст поверх игры (полезно для FPS, состояний и значений).
- `DrawWorldText` рисует текст в мире с позицией и масштабом.
- `ClearScreenMessages` и `ClearWorldText` очищают отладочный вывод.

### Режим Debug Build (рантайм)

Когда вы упаковываете игру в конфигурации **Debug**, рантайм включает полную систему
отладочных оверлеев. Это происходит автоматически на всех платформах (Windows, Linux,
Android, Web); в **Release**-сборках её нет.

#### Полностью управляется из Lua

Система рантайм-отладки предоставляет компактный Lua API: геттер сообщает, виден ли
оверлей сейчас, а toggle инвертирует его видимость. Никаких встроенных клавиатурных
хоткеев нет — каждый проект сам выбирает клавиши, кнопки геймпада, консольные команды
или UI-виджеты для управления оверлеями:

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

> Сам Lua API работает **и в Debug, и в Release** сборках на любой платформе. Отрисовка
> оверлеев компилируется только в Debug-сборки, поэтому в Release toggle
> по-прежнему переключает внутренний флаг, но никакой отладочной графики не появляется —
> это удобно для девелоперских меню, которые остаются во внутренних сборках.

#### Lua API — управление Debug-режимом

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

| Функция | Сигнатура | Описание |
|---------|-----------|----------|
| `IsDebugBuild` | `IsDebugBuild()` → `bool` | `true` в Debug, `false` в Release. Доступна всегда на всех платформах. |
| `GetDebugColliders` / `ToggleDebugColliders` | `() → bool` / `()` | Прочитать или инвертировать оверлей вайрфреймов коллайдеров (Box, Sphere, Capsule, Polygon). |
| `GetDebugEntityMarkers` / `ToggleDebugEntityMarkers` | `() → bool` / `()` | Прочитать или инвертировать оверлей маркеров сущностей (Rigidbody, Lights, Camera, Audio, FX). |
| `GetDebugNavGrid` / `ToggleDebugNavGrid` | `() → bool` / `()` | Прочитать или инвертировать оверлей навигационной сетки и отладки ИИ (пути, патрульные точки, конусы восприятия). |
| `GetDebugProfilerVisible` / `ToggleDebugProfiler` | `() → bool` / `()` | Прочитать или инвертировать рантайм-оверлей профайлера (FPS, кадр, CPU/RAM/VRAM/GPU, render passes, hot scopes). |
| `NetworkProfiler.IsVisible` / `NetworkProfiler.Toggle` | `() → bool` / `()` | Та же пара для сетевого оверлея профайлера. |
| `GetDebugFlag(name)` | `(string) → bool` | Универсальный геттер любого флага вьюпорта (см. `GetDebugFlagNames`). |
| `SetDebugFlag(name, value)` | `(string, bool) → bool` | Универсальный сеттер любого флага вьюпорта. Возвращает `true`, если имя валидно. |
| `ToggleDebugFlag(name)` | `(string) → bool` | Универсальный переключатель. Возвращает новое значение флага. |
| `ClearDebugFlags` | `()` | Сбрасывает все флаги вьюпорта в `false`. |
| `GetDebugFlagNames` | `() → table` | Массив всех 21 поддерживаемого имени флага. |

Поддерживаемые имена для `GetDebugFlag` / `SetDebugFlag` / `ToggleDebugFlag`:
`ShowColliders`, `ShowNavGrid`, `ShowEntityMarkers`, `ShowLightRadius`, `ShowAudioRange`,
`ShowCameraFrustum`, `ShowJoints`, `ShowPhysicsContacts`, `ShowSleepingBodies`,
`ShowVelocityVectors`, `ShowTilemapGrid`, `ShowFXBounds`, `ShowWidgetBounds`,
`ShowZDepthColor`, `WireframeMode`, `FreezeCulling`, `ShowShadowMaps`, `ShowShadowEdges`,
`ShowLightHeatmap`, `ShowNavGridHeatmap`, `ShowAIStateOverlay`.

> **Гейтинг по конфигурации сборки:** все отладочные оверлеи (вайрфреймы коллайдеров,
> маркеры сущностей, навигационная сетка, векторы скорости/контактов, оверлеи света/звука/FX/виджетов,
> wireframe и freeze-culling, визуализация теней, AI-оверлей, профайлер/сетевой оверлей и т.д.)
> рисуются в **редакторе (любая конфигурация)** и в **standalone-Debug сборке игры**.
> В **shipping-сборке** (`ICE_RUNTIME_BUILD=ON` + Release) отрисовка полностью вырезается препроцессором,
> но Lua API остаётся как no-op — кросс-сборочные скрипты работают без `pcall`.

#### Пример: условная отладочная отрисовка

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

#### Lua API — Трейсы профайлера (Chrome Trace Event Format)

Профайлер умеет писать CPU/GPU-скопы, render-passes и счётчики в трейс и экспортировать
его в **Chrome Trace Event Format** (`.json`). Результирующий файл открывается без плагинов в:

- `chrome://tracing` (любые Chromium-браузеры — Chrome, Edge, Opera, Brave, Vivaldi);
- [ui.perfetto.dev](https://ui.perfetto.dev) — современный вьюер от Google (рекомендуется);
- [profiler.firefox.com](https://profiler.firefox.com) — Firefox Profiler (Import JSON);
- [speedscope.app](https://speedscope.app) — flame-граф вьюер.

API кросс-платформенный и работает как в Debug, так и в Release-сборках (формат трейса —
открытый стандарт, никакой привязки к конкретному вьюеру нет).

```lua
-- Начать запись именованного трейса (возвращает true, если запись реально стартовала).
StartProfilerTrace()                     -- имя по умолчанию
StartProfilerTrace("BossFight")          -- своё имя

-- Остановить активный трейс и сохранить его в истории профайлера.
StopProfilerTrace()

-- Проверить, идёт ли сейчас запись.
if IsProfilerTracing() then
    PrintScreen("REC", 1, 1, 0, 1, 0)
end

-- Экспорт последнего готового трейса (или живого, или снапшота одного кадра,
-- если трейсов ещё нет) в Chrome Trace Event Format. Без аргумента имя файла
-- генерируется автоматически в папке вывода профайлера. На Web браузер
-- автоматически предложит скачать получившийся .json.
SaveChromeTrace()                        -- авто-имя в Profiles/
SaveChromeTrace("boss_fight.json")       -- явное имя/путь
```

| Функция | Сигнатура | Описание |
|---------|-----------|----------|
| `StartProfilerTrace` | `StartProfilerTrace([name])` → `bool` | Начинает новый трейс. `name` необязательно — если не передать, профайлер подставит своё имя. Возвращает `true`, если запись реально началась. |
| `StopProfilerTrace` | `StopProfilerTrace()` | Останавливает текущий трейс и кладёт его в историю трейсов (видно в панели Profiler в редакторе). Без эффекта, если запись не шла. |
| `IsProfilerTracing` | `IsProfilerTracing()` → `bool` | Возвращает `true`, пока идёт запись трейса. |
| `SaveChromeTrace` | `SaveChromeTrace([filename])` → `bool` | Сохраняет последний завершённый трейс (или живой, или снапшот одного кадра, если трейсов ещё нет) как Chrome Trace Event JSON. На Emscripten (Web) дополнительно вызывает скачивание через браузер. Возвращает `true` при успехе. |

> **Привяжите свои клавиши.** Подключите трейс-API к любому удобному вводу проекта —
> например `IsKeyJustPressed("f7")` для start/stop и `IsKeyJustPressed("f8")` для
> экспорта, аккорд на геймпаде, команда девелоперской консоли или кнопка отладочного UI.
> Сам Lua API работает на всех платформах и во всех конфигурациях; JSON-экспорт доступен
> везде, а живой оверлей внутри движка требует Debug-сборки.

```lua
-- Пример: запись трейса отдельного игрового отрезка с авто-экспортом.
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

#### Lua API — Пользовательские профайлер-скопы

Оборачивайте любой кусок Lua-кода в именованный профайлер-скоп. Скопы видны:

- в **рантайм-оверлее профайлера** (`ToggleDebugProfiler()` / `GetDebugProfilerVisible()`);
- в редакторской панели **Profiler** (разбивка по кадру);
- в экспортированных файлах **Chrome Trace Event** JSON (`SaveChromeTrace`).

Работает в Debug и Release билдах и на всех платформах (Windows, Linux, Web, Android).
Имена скопов из Lua интернируются внутри движка, поэтому пары `ProfileBegin`/`ProfileEnd` сопоставляются по равенству указателей — так же, как нативные макросы `IB_PROFILE_SCOPE`.

```lua
-- Ручное парное использование (имена должны совпадать, скопы можно вкладывать).
ProfileBegin("AI.Update")
    UpdateEnemies(dt)
    ProfileBegin("AI.Pathfinding")
        RecalculatePaths()
    ProfileEnd("AI.Pathfinding")
ProfileEnd("AI.Update")

-- Scoped-вариант — сам вызывает Begin/End вокруг тела функции,
-- даже если внутри произойдёт Lua-ошибка. Прокидывает возвращаемое значение.
local result = ProfileScope("Boss.HeavyAttack", function()
    return ComputeAttackDamage()
end)
```

| Функция | Сигнатура | Описание |
|---------|-----------|----------|
| `ProfileBegin` | `ProfileBegin(name)` | Открывает CPU-скоп профайлера с указанным именем. Должен быть закрыт парным `ProfileEnd(name)` в том же кадре. Скопы можно вкладывать. |
| `ProfileEnd` | `ProfileEnd(name)` | Закрывает последний открытый `ProfileBegin(name)`. Имя обязано совпадать с соответствующим `ProfileBegin`. |
| `ProfileScope` | `ProfileScope(name, fn)` → `any` | Удобный RAII-аналог. Вызывает `ProfileBegin(name)`, выполняет `fn()`, гарантированно вызывает `ProfileEnd(name)`. Возвращает то, что вернула `fn`. |

> **Совет.** Используйте иерархические имена (`"AI.Update"`, `"AI.Pathfinding"`, `"Render.HUD"`) — редакторский профайлер и Chrome Trace-вьюверы будут группировать их визуально.

#### Lua API — Пользовательские счётчики

Публикуйте любое число геймплея или системы как **счётчик** профайлера. Счётчики не требуют регистрации: они сразу появляются на вкладке **Counters** профайлера редактора (сгруппированные по имени группы, раскрашенные по бюджету), записываются в каждый кадр трейса, экспортируются в Chrome Trace отдельными дорожками и становятся графиками Tracy в инструментированных сборках.

```lua
-- Обычное число.
ProfilerSetCounter("Gameplay", "Alive Enemies", #enemies)

-- С единицей и бюджетом: значение желтеет, оранжевеет и краснеет по мере
-- приближения к 8 мс и их превышения.
ProfilerSetCounter("Gameplay", "AI Think", thinkMs, "ms", 8.0)

-- Накопление за кадр (на следующем кадре сбрасывается автоматически).
ProfilerAddCounter("Gameplay", "Projectiles Spawned", 1)

-- Прочитать любой счётчик, включая счётчики самого движка.
local contacts = ProfilerGetCounter("Physics", "Contacts")
local budgetMs = GetProfilerFrameBudgetMs()
```

| Функция | Сигнатура | Описание |
|---------|-----------|----------|
| `ProfilerSetCounter` | `ProfilerSetCounter(group, name, value[, unit[, budget]])` | Задаёт значение счётчика на этот кадр. `unit` — `"ms"`, `"b"`, `"kb"`, `"mb"`, `"%"` или `"/s"`; опустите для обычного числа. `budget` (необязательно) управляет цветовой индикацией. |
| `ProfilerAddCounter` | `ProfilerAddCounter(group, name, delta)` | Прибавляет `delta` к счётчику за текущий кадр; на следующем кадре отсчёт начинается заново с первого `delta`. |
| `ProfilerGetCounter` | `ProfilerGetCounter(group, name)` → `number` | Читает счётчик. Работает и для счётчиков движка (`"Physics"`, `"Renderer"`, `"FX"`, `"Audio"`, `"Assets"`, `"Shadows"`, `"Memory"`, `"Network"`, `"Lua"`, `"Components"`, `"Instances"`, `"Scene"`). |
| `GetProfilerFrameBudgetMs` | `GetProfilerFrameBudgetMs()` → `number` | Бюджет кадра в миллисекундах, выведенный из целевого FPS проекта. |

#### Lua API — Профайлер скриптов

Движок измеряет каждый Lua-колбэк, который он вызывает, отдельно по скрипту и по типу колбэка (`OnUpdate`, `OnLateUpdate`, `OnFixedUpdate`, коллизии, сенсоры, удары, разрыв шарниров, события жизненного цикла, деревья поведения, колбэки уровня и модов, а также скрипты виджетов). Эти функции читают и настраивают эти данные из игрового кода — полезно в готовых сборках, где панель редактора недоступна.

```lua
if GetScriptProfilerTimeMs() > GetProfilerFrameBudgetMs() * 0.3 then
    for _, row in ipairs(GetScriptProfilerRows()) do
        if row.frameMs > 0.5 then
            PrintScreen(string.format("%s: %.2f ms x%d", row.script, row.frameMs, row.instances), 2.0)
        end
    end
end
```

| Функция | Сигнатура | Описание |
|---------|-----------|----------|
| `SetScriptProfilerEnabled` | `SetScriptProfilerEnabled(enabled)` | Включает или выключает измерение по скриптам. В выключенном состоянии не стоит ничего. |
| `IsScriptProfilerEnabled` | `IsScriptProfilerEnabled()` → `bool` | Идёт ли измерение по скриптам. |
| `ResetScriptProfiler` | `ResetScriptProfiler()` | Сбрасывает накопленную статистику по скриптам без перезапуска игры. |
| `GetScriptProfilerTimeMs` | `GetScriptProfilerTimeMs()` → `number` | Суммарное время Lua в колбэках, вызванных движком, за последний кадр. |
| `GetScriptProfilerCalls` | `GetScriptProfilerCalls()` → `number` | Число измеренных вызовов Lua-колбэков за последний кадр. |
| `GetLuaMemoryKB` | `GetLuaMemoryKB()` → `number` | Текущий размер кучи Lua в КБ. |
| `GetLuaAllocRateKBps` | `GetLuaAllocRateKBps()` → `number` | Сглаженная скорость аллокаций Lua в КБ/с — растущее значение означает расход памяти скриптами. |
| `GetScriptProfilerRows` | `GetScriptProfilerRows()` → `table` | Массив строк по скриптам, отсортированный по стоимости за последний кадр. У каждой записи есть `script`, `path`, `frameMs`, `avgMs`, `maxMs`, `totalMs`, `calls`, `instances`, `errors`. |

#### Lua API — Детектор фризов

Профайлер автоматически захватывает залипшие кадры: кадр, который превысил порог *и* занял больше двойного скользящего среднего, сохраняется вместе с полным деревом скоупов для последующего разбора на вкладке **Hitches** редактора.

```lua
SetProfilerHitchThreshold(33.0)
SetProfilerHitchDetection(true)

if GetProfilerHitchCount() > 0 then
    PrintScreen("Фризы: " .. GetProfilerHitchCount(), 1.0)
end
```

| Функция | Сигнатура | Описание |
|---------|-----------|----------|
| `SetProfilerHitchDetection` | `SetProfilerHitchDetection(enabled)` | Включает или выключает автоматический захват фризов. |
| `SetProfilerHitchThreshold` | `SetProfilerHitchThreshold(ms)` | Абсолютный порог в миллисекундах. Кадр должен ещё и превысить двойное скользящее среднее. |
| `GetProfilerHitchCount` | `GetProfilerHitchCount()` → `number` | Сколько фризов сейчас сохранено (до 32, самые старые вытесняются). |
| `ClearProfilerHitches` | `ClearProfilerHitches()` | Очищает список фризов. |

#### Lua API — Управление видимостью профайлер-оверлея

Управляет экранным профайлер-оверлеем (FPS, кадр, CPU/RAM/VRAM/GPU, render passes, hot scopes).
Привяжите любую клавишу через `IsKeyJustPressed` (или кнопку геймпада, консольную команду, UI-виджет) и вызывайте `ToggleDebugProfiler()`.
Эти функции работают в **любой конфигурации сборки** (Debug и Release) и на всех платформах — оверлей можно вывести через свои внутриигровые меню или девелоперскую консоль.

```lua
local visible = GetDebugProfilerVisible()

ToggleDebugProfiler()
```

| Функция | Сигнатура | Описание |
|---------|-----------|----------|
| `GetDebugProfilerVisible` | `GetDebugProfilerVisible()` → `bool` | Возвращает `true`, пока рантайм-оверлей профайлера виден. |
| `ToggleDebugProfiler` | `ToggleDebugProfiler()` | Инвертирует текущую видимость оверлея. Привяжите к любому хоткею через `IsKeyJustPressed`. |

> Видимость оверлея — единый флаг на весь движок, поэтому любой вызов `ToggleDebugProfiler` — из вашего скрипта или привязанного хоткея — переключает его одинаково.

---

## 30. Tilemap — Тайловые карты

> **Тип:** Entity-bound. Требует компонент **TilemapRendererComponent**.
> Тайлмапы (тайловые карты) — сетка из тайлов (плиток) для построения уровней, с выбором проекции: **ортогональная**, **изометрическая** или **гексагональная** (задаётся для каждой карты в редакторе тайлмапа — см. **Проекцию** ниже). Все тайловые функции учитывают активную проекцию автоматически.
> У сущности может быть несколько тайлмапов (инстансов). `index` = 0 — первый тайлмап.

### Основные свойства

```lua
-- Количество тайлмапов
local count = GetTilemapCount()

-- Размер тайлмапа → {width, height, tileSize, cellWidth, cellHeight, projection}
local size = GetTilemapSize()
local size = GetTilemapSize(1)  -- Второй тайлмап
-- size.width = ширина в тайлах
-- size.height = высота в тайлах
-- size.tileSize = размер тайла (арта) в пикселях
-- size.cellWidth = ширина footprint-ячейки в пикселях
-- size.cellHeight = высота footprint-ячейки в пикселях
-- size.projection = "orthogonal" | "isometric" | "hexagonal"

-- Количество слоёв
local layers = GetTilemapLayerCount()
local layers = GetTilemapLayerCount(1)  -- Второй тайлмап

-- Видимость
SetTilemapVisible(true)
SetTilemapVisible(false, 1)  -- Второй тайлмап
local vis = IsTilemapVisible()

-- Флип
SetTilemapFlipX(true)
SetTilemapFlipY(false)
local fx = GetTilemapFlipX()
local fy = GetTilemapFlipY()

-- Рендер в игре
SetTilemapRenderInGame(true)
local render = GetTilemapRenderInGame()
SetTilemapRenderInGame(true, 0)  -- опциональный индекс инстанса тайлмапа (по умолчанию 0)

-- Потайловые тени. У каждого типа тайла свои настройки тени. Передаётся значение
-- тайла (как возвращает GetTileAt); опциональный последний аргумент — индекс
-- инстанса тайлмапа (по умолчанию 0).
local tile = GetTileAt(worldX, worldY)

-- Отбрасывает ли тайл тень
SetTileCastShadow(tile, true)
local casts = GetTileCastShadow(tile)              -- → bool

-- Форма тени: 0 = Коллайдеры (коллайдер тайла), 1 = Контур (силуэт по текстуре)
SetTileCastShadowMode(tile, 1)
local mode = GetTileCastShadowMode(tile)           -- → int

-- Начало тени: 0 = Bottom, 1 = Center, 2 = Top
SetTileShadowOrigin(tile, 0)
local origin = GetTileShadowOrigin(tile)           -- → int

-- Смягчение краёв тени [0..1]
SetTileShadowEdgeFade(tile, 0.25)
local fade = GetTileShadowEdgeFade(tile)           -- → float

-- Z-порядок тени: отрицательное = на задний план, положительное = на передний план, 0 = плоскость кастера
SetTileShadowZOrder(tile, 1)
local zo = GetTileShadowZOrder(tile)               -- → float

-- Пропускает ли тайл свет вместо блокировки тени
SetTileDontBlockShadows(tile, true)
local dontBlock = GetTileDontBlockShadows(tile)    -- → bool

-- Путь файла тайлмапа
local path = GetTilemapFilePath()
```

### Позиция и трансформация

```lua
-- Позиция (глобальная)
SetTilemapPosition(100, 200)
local pos = GetTilemapPosition()  -- → {x, y}

-- Локальная позиция (смещение внутри сущности)
SetTilemapLocalPosition(10, 5)
local lp = GetTilemapLocalPosition()        -- → {x, y}
SetTilemapLocalPosition(10, 5, 1)           -- Для конкретного экземпляра тайлмапа
local lp = GetTilemapLocalPosition(1)

-- Масштаб
SetTilemapLocalScale(2, 2)
local ls = GetTilemapLocalScale()  -- → {x, y}

-- Поворот
SetTilemapLocalRotation(45)
local lr = GetTilemapLocalRotation()

-- Мировая трансформация (трансформ сущности уже учтён — см. раздел Sprite).
-- Учтите: SetTilemapPosition/GetTilemapPosition — старые псевдонимы локальной
-- позиции и работают идентично Local-варианту.
SetTilemapWorldPosition(120, 64, 0)
local twp = GetTilemapWorldPosition(0)      -- → {x, y, z}
SetTilemapWorldRotation(30, 0)
local twr = GetTilemapWorldRotation(0)      -- → число
local tws = GetTilemapWorldScale(0)         -- → {x, y}, только чтение

-- Подмена ассета тайлмапа. После этого вызовите RebuildTilemapPhysics, чтобы
-- цепочки коллизий соответствовали новой сетке.
local ok = SetTilemapAsset("Content/Levels/Cave.ice_tilemap", 0)
RebuildTilemapPhysics(0)
local tmPath = GetTilemapFilePath(0)        -- → текущий путь .ice_tilemap

-- Порядок рендеринга (Z-глубина)
SetTilemapOrder(5)                          -- Задать порядок рендеринга для сущности
SetTilemapOrder(3, 1)                       -- Задать порядок рендеринга для конкретного экземпляра
local order = GetTilemapOrder()             -- Получить порядок рендеринга
local order = GetTilemapOrder(1)            -- Получить порядок рендеринга для конкретного экземпляра

-- Размер тайла
SetTilemapTileSize(32)
```

### Работа с тайлами

```lua
-- Получить ID тайла по мировым координатам
local tileId = GetTileAt(worldX, worldY)
local tileId = GetTileAt(worldX, worldY, 0, 1)  -- instance 0, layer 1

-- Получить ID тайла по координатам сетки
local tileId = GetTileGrid(tileX, tileY)
local tileId = GetTileGrid(tileX, tileY, 1, 0)  -- layer 1, instance 0

-- Установить тайл по координатам сетки (обновляет также физические коллайдеры)
SetTileAt(tileX, tileY, tileId)
SetTileAt(tileX, tileY, tileId, 0, 1)  -- instance 0, layer 1

-- Установить тайл (только сетка, без физики)
SetTileGrid(tileX, tileY, tileId)
SetTileGrid(tileX, tileY, tileId, 1, 0)  -- layer 1, instance 0

-- Проверить, есть ли тайл в точке (любой слой)
if IsTileSolid(worldX, worldY) then ... end
```

### Конвертация координат

```lua
-- Мировые координаты → координаты сетки
local tile = WorldToTile(worldX, worldY)  -- → {x, y}

-- Координаты сетки → мировые (центр тайла)
local world = TileToWorld(tileX, tileY)  -- → {x, y}
```

### Проекция (orthogonal / isometric / hexagonal)

> Проекция — свойство **уровня карты**, задаётся в редакторе тайлмапа. Тайлсеты остаются обычными квадратными листами.
> **Соглашение о координатах:** для **ортогональных** карт координаты тайлов используют классическую раскладку (`y = 0` снизу). Для **изометрических** и **гексагональных** карт координаты тайлов — это **сырые координаты сетки** `(x, y)`: `(0, 0)` — верхняя ячейка, `x` растёт вправо-вниз, `y` — влево-вниз. Все тайловые функции (`GetTileAt`, `SetTileAt`, `WorldToTile`, `TileToWorld`, `IsTileSolid`, …) учитывают активную проекцию автоматически.

```lua
-- Активная проекция → "orthogonal" | "isometric" | "hexagonal"
local proj = GetTilemapProjection()
local proj = GetTilemapProjection(1)        -- Второй тайлмап

-- Размер footprint-ячейки и проекцию также возвращает GetTilemapSize:
local size = GetTilemapSize()
-- size.cellWidth, size.cellHeight, size.projection

-- Проекционно-корректные соседи ячейки → массив {x, y}
-- 8 для ортогональной, 4 для изометрической (ромб), 6 для гексагональной.
-- Возвращаются только ячейки в пределах карты.
local neighbours = GetTileNeighbors(tileX, tileY)
local neighbours = GetTileNeighbors(tileX, tileY, 1)  -- Второй тайлмап
for _, n in ipairs(neighbours) do
    -- n.x, n.y
end

-- Дистанция по сетке между двумя ячейками (проекционно-корректная):
-- Чебышёв (ортогональная), Манхэттен (изометрическая), гекс-дистанция (гексагональная).
local d = TileDistance(ax, ay, bx, by)
local d = TileDistance(ax, ay, bx, by, 1)   -- Второй тайлмап

-- Мировые вершины footprint ячейки → массив {x, y}
-- 4 вершины (орто / изо), 6 вершин (гекс). Удобно для подсветки ячейки.
local poly = GetTileFootprintWorld(tileX, tileY)
for _, v in ipairs(poly) do
    -- v.x, v.y
end
```

> **Совет (сеточные игры):** `GetTileNeighbors` + `TileDistance` дают готовую математику смежности и дальности для пошаговых / рогалик / тактических игр на любой проекции — не нужно вручную возиться со смещениями гексов.

### Заполнение и очистка

```lua
-- Заполнить прямоугольную область
FillRect(startX, startY, width, height, tileId)
FillRect(startX, startY, width, height, tileId, 0, 0)  -- layer 0, instance 0

-- Заполнить весь слой одним тайлом
FillAll(tileId)
FillAll(tileId, 0, 0)  -- layer 0, instance 0

-- Очистить слой (все тайлы = -1)
ClearLayer()
ClearLayer(0, 0)  -- layer 0, instance 0

-- Изменить размер тайлмапа
ResizeTilemap(64, 32)  -- width, height
```

### Слои

```lua
-- Добавить новый слой → индекс нового слоя
local layerIdx = AddTilemapLayer("Decoration")

-- Удалить слой (нельзя удалить последний)
RemoveTilemapLayer(2)  -- layerIndex

-- Имя слоя
local name = GetLayerName(0)

-- Переименовать слой
SetLayerName(0, "Background")

-- Видимость слоя
SetLayerVisible(0, true)
local vis = IsLayerVisible(0)

-- Блокировка слоя
SetLayerLocked(0, true)
local locked = IsLayerLocked(0)

-- Поменять местами слои
SwapLayerOrder(0, 1)

-- Копировать слой → новый индекс
local copyIdx = CopyLayer(0, "Background Copy")
```

### Тайлсеты (мультитайлсеты)

```lua
-- Путь основного тайлсета
local path = GetTilesetPath()

-- Количество тайлсетов
local count = GetTilesetCount()

-- Путь дополнительного тайлсета
local path = GetAdditionalTilesetPath(0)

-- Кодирование/декодирование тайлов для мультитайлсетов
local encoded = EncodeTile(1, 5)        -- tilesetIndex=1, tileId=5
local decoded = DecodeTile(encoded)     -- → {tilesetIndex, tileId}
local isEncoded = IsEncodedTile(value)  -- true/false
```

### Пути тайлсетов и управление в рантайме

```lua
-- Основной тайлсет
SetPrimaryTilesetPath("Content/Tilesets/Stone.ice_tileset")
local primary = GetPrimaryTilesetPath()

-- Дополнительные тайлсеты (возвращаемый индекс используется в EncodeTile/DecodeTile,
-- где tilesetIndex 0 = основной, 1+ = дополнительные)
local idx = AddAdditionalTileset("Content/Tilesets/Wood.ice_tileset")  -- → новый индекс (0-based среди дополнительных)
local count = GetAdditionalTilesetCount()
local p = GetAdditionalTilesetPath(0)
SetAdditionalTilesetPath(0, "Content/Tilesets/Sand.ice_tileset")
RemoveAdditionalTileset(0)

-- Переопределение тайлсета на конкретном слое (перекрывает основной для тайлов этого слоя)
SetLayerTilesetPath(1, "Content/Tilesets/Foreground.ice_tileset")
local layerTs = GetLayerTilesetPath(1)

-- Все функции принимают необязательный индекс инстанса последним аргументом
SetPrimaryTilesetPath("...", 0)
```

### Интроспекция тайлсета

```lua
-- tilesetIdx: 0 = основной, 1+ = дополнительные (как в EncodeTile/DecodeTile)
local tileSize  = GetTilesetTileSize(tilesetIdx)       -- например 32 (px на тайл)
local cols      = GetTilesetColumns(tilesetIdx)        -- ширина текстуры / tileSize
local rows      = GetTilesetRows(tilesetIdx)
local total     = GetTilesetTileCount(tilesetIdx)      -- cols * rows
local texPath   = GetTilesetTexturePath(tilesetIdx)

-- Метаданные конкретного тайла из ассета тайлсета (TileData)
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

-- Все принимают необязательный индекс инстанса последним аргументом
local total = GetTilesetTileCount(0, 0)
```

### Пакетное редактирование тайлов

```lua
-- Все пакетные операции используют Lua-Y-координаты (y=0 — верхняя строка),
-- учитывают слои и автоматически пересчитывают пустые чанки если включён chunking.

-- 2D-таблица → весь слой. tbl[1..H][1..W], строка 1 = низ (y=0), строка H = верх, столбец 1 = левый.
-- Ячейки, не являющиеся числом, пропускаются (остаются как есть).
local map = NoiseMap(GetTilemapSize().width, GetTilemapSize().height, 30.0)
local grid = {}
for y = 1, #map do
    grid[y] = {}
    for x = 1, #map[y] do
        grid[y][x] = (map[y][x] > 0.55) and 1 or -1
    end
end
SetTilesBulk(grid)              -- слой 0
SetTilesBulk(grid, 1)           -- слой 1
SetTilesBulk(grid, 0, 0)        -- слой 0, инстанс 0

-- Плоский массив → прямоугольный регион. Длина массива = w*h, row-major (первая строка вначале).
BlitFromArray(x, y, w, h, { 1, -1, 1, -1, 2, 2, 2, 2 }, 0)

-- Копирование прямоугольника между слоями (или внутри одного слоя)
CopyRect(srcLayer, srcX, srcY, w, h, dstLayer, dstX, dstY)
CopyRect(0, 5, 5, 8, 8, 1, 20, 20, 0)  -- с индексом инстанса
```

### Региональные операции (процедурное редактирование)

```lua
-- Заливка (4-связная). Заменяет связную область тайлов с тем же id, начиная с (x, y),
-- на newId. Возвращает количество залитых ячеек.
local n = FloodFill(startX, startY, newId)
local n = FloodFill(startX, startY, newId, layerIdx)
local n = FloodFill(startX, startY, newId, layerIdx, instanceIdx)

-- Штамп 2D-паттерна в (x, y). pattern[1][1] попадает в (x, y); строка 1 = низ штампа в мире
-- (т.к. Y+ вверх), строка H = верх.
-- skipNegative (по умолчанию true): ячейки с tileId < 0 в паттерне считаются «прозрачными»
-- и не перезаписывают цель — удобно для комнат/декалей с пустыми участками.
local room = {
    { 1, 1, 1, 1, 1 },
    { 1,-1,-1,-1, 1 },
    { 1,-1, 5,-1, 1 },
    { 1,-1,-1,-1, 1 },
    { 1, 1, 1, 1, 1 },
}
StampPattern(10, 10, room)
StampPattern(10, 10, room, 0, 0, false)  -- писать -1 тоже (очистка ячеек)

-- Поворот прямоугольной области на месте
-- direction:  1 = 90° по часовой, -1 = 90° против часовой, 2 = 180°
-- 90° повороты требуют w == h.
RotateRect(x, y, w, h, 1)
RotateRect(x, y, 8, 8, -1, 0, 0)

-- Зеркальное отражение прямоугольника на месте
FlipRect(x, y, w, h, "x")  -- по горизонтали (лево ↔ право)
FlipRect(x, y, w, h, "y")  -- по вертикали (верх ↔ низ)
```

### Auto-tile / битовые маски соседей

```lua
-- Маска 4 соседей. Биты: N=1, E=2, S=4, W=8.
-- Координаты: тайл (0,0) в левом нижнем углу, последний — в правом верхнем.
-- N (север) = +Y (вверх), S = -Y (вниз), E = +X (вправо), W = -X (влево).
-- solidId: -1 (или не передавать) → любой непустой тайл считается «твёрдым»
--          >=0                    → только ячейки с этим id считаются «твёрдыми»
local m = GetNeighborMask4(x, y)             -- "любой непустой"
local m = GetNeighborMask4(x, y, 1)          -- "только id 1 — твёрдый"
local m = GetNeighborMask4(x, y, -1, 0, 0)   -- слой 0, инстанс 0

-- Маска 8 соседей. Биты:
--   N=1, NE=2, E=4, SE=8, S=16, SW=32, W=64, NW=128
local m = GetNeighborMask8(x, y, -1)

-- Пример: выбор спрайта из 16-тайлового auto-tile-листа
local autoTile = { [0]=0, [1]=1, [2]=2, ..., [15]=15 }
local mask = GetNeighborMask4(x, y, -1)
SetTileAt(x, y, autoTile[mask])
```

### Итерация и поиск

```lua
-- Обход каждой ячейки (включая пустые) — координаты в Lua-Y
IterateLayer(function(x, y, tileId)
    if tileId == 5 then SetTileAt(x, y, 7) end
end)
IterateLayer(callback, layerIdx, instanceIdx)

-- Обход только непустых ячеек (учитывает chunking: пропускает пустые чанки → очень быстро на разреженных картах)
IterateNonEmpty(function(x, y, tileId)
    print(x, y, tileId)
end, layerIdx, instanceIdx)

-- Подсчёт ячеек с конкретным tileId. layerIdx опущен/-1 → подсчёт по всем слоям.
local n = CountTiles(1)
local n = CountTiles(1, 0)        -- только слой 0
local n = CountTiles(1, 0, 0)     -- слой 0, инстанс 0

-- Поиск всех ячеек с указанным tileId (возвращает массив {x, y} в Lua-Y)
local cells = FindTile(1)
for _, c in ipairs(cells) do print(c.x, c.y) end
```

### Коллайдеры тайлов

```lua
-- Удалить коллайдер конкретного тайла
DestroyTileCollider(tileX, tileY)

-- Создать коллайдер тайла вручную
CreateTileCollider(tileX, tileY, false)

-- Проверить наличие коллайдера
local has = HasTileCollider(tileX, tileY)

-- Проверить, является ли коллайдер тайла сенсором
local isSensor = IsTileColliderSensor(tileX, tileY)
local isSensor = IsTileColliderSensor(tileX, tileY, 0)  -- instance 0

-- Установить режим сенсора коллайдера тайла в рантайме
SetTileColliderSensor(tileX, tileY, true)                -- Сделать сенсором
SetTileColliderSensor(tileX, tileY, false)               -- Сделать твёрдым
SetTileColliderSensor(tileX, tileY, true, 0)             -- instance 0

-- Сенсоры вызывают OnSensorEnter/OnSensorExit, но не блокируют движение
-- Работает как для тайлсетных, так и для анимационных (флипбук) тайлов

-- Полная пересборка физики тайлмапа (коллайдеры)
RebuildTilemapPhysics()
```

### События и свойства коллайдеров тайлов

```lua
-- Односторонняя платформа
local oneWay = IsTileColliderOneWay(tileX, tileY)
SetTileColliderOneWay(tileX, tileY, true)
SetTileColliderOneWay(tileX, tileY, false, 0)  -- instance 0

-- Направление одностороннего прохода: 1 = Вверх (по умолчанию), 2 = Вниз, 3 = Влево, 4 = Вправо
-- Направление — это сторона, с которой тайл ТВЁРДЫЙ (тела не проходят сквозь)
local dir = GetTileColliderOneWayDirection(tileX, tileY)
local dir = GetTileColliderOneWayDirection(tileX, tileY, 0)  -- instance 0
SetTileColliderOneWayDirection(tileX, tileY, 1)
SetTileColliderOneWayDirection(tileX, tileY, 2, 0)  -- instance 0

-- События контакта
local contacts = AreTileContactEventsEnabled(tileX, tileY)
SetTileContactEventsEnabled(tileX, tileY, true)

-- События сенсора
local sensors = AreTileSensorEventsEnabled(tileX, tileY)
SetTileSensorEventsEnabled(tileX, tileY, true)

-- События удара
local hits = AreTileHitEventsEnabled(tileX, tileY)
SetTileHitEventsEnabled(tileX, tileY, true)

-- События пре-солва (вызываются до расчёта ответа на столкновение)
local preSolve = AreTilePreSolveEventsEnabled(tileX, tileY)
SetTilePreSolveEventsEnabled(tileX, tileY, true)

-- Проверить, использует ли тайл chain-коллайдер (объединённый контур, только чтение)
local isChain = IsTileChainCollider(tileX, tileY)

-- Все функции принимают необязательный индекс инстанса как последний параметр
```

### Разрушаемость тайлов (используется ExplodeTiles)

```lua
-- Проверить, помечен ли тайл по координатам (tileX, tileY) как разрушаемый
local destructible = IsTileDestructible(tileX, tileY)
local destructible = IsTileDestructible(tileX, tileY, 0)  -- instance 0

-- Установить/снять флаг "разрушаемый" у тайла в (tileX, tileY).
-- Для обычных тайлов тайлсета изменение сохраняется в .tileset на диске.
-- Для анимированных (флипбук) тайлов изменение применяется только к рантайм-тайлмэпу.
local changed = SetTileDestructible(tileX, tileY, true)
SetTileDestructible(tileX, tileY, false, 0)              -- instance 0

-- Запрос/изменение разрушаемости по ID тайла (без координат)
local destructible = IsTileDestructibleById(tileId)
local destructible = IsTileDestructibleById(tileId, 0)   -- индекс тайлсета 0
SetTileDestructibleById(tileId, true)
SetTileDestructibleById(tileId, true, 1)                 -- дополнительный тайлсет 1
```

### Настройки осколков тайла

У каждого разрушаемого тайла есть собственные настройки осколков (Fragment) — тот же набор,
что доступен в редакторах тайлсета и тайлмапа. `ExplodeTiles` использует их как основу для
порождаемых осколков. Читать и менять их во время выполнения можно как таблицу с теми же
ключами, что принимает переопределение `opts` у `ExplodeTiles` (`count`, `pattern`,
`lifetime`, `fadeTime`, `gravityScale`, `density`, `friction`, `restitution`, `isSensor`,
`contactEvents`, `sensorEvents`, `hitEvents`, `preSolveEvents`, `collisionGroup`, `castShadow`,
`dontBlockShadows`, `shadowOrigin`, `shadowEdgeFade`, `shadowZOrder`). `Set*` применяет только
переданные ключи.

```lua
-- Прочитать текущие настройки осколков тайла в (tileX, tileY); nil, если тайла нет
local frag = GetTileFragmentSettings(tileX, tileY)
local frag = GetTileFragmentSettings(tileX, tileY, 0)     -- instance 0

-- Слить новые настройки осколков в тайл (tileX, tileY).
-- Обычные тайлы тайлсета сохраняются в .tileset; анимированные применяются в рантайме.
local changed = SetTileFragmentSettings(tileX, tileY, { density = 2.0, count = 4, pattern = 2 })
SetTileFragmentSettings(tileX, tileY, { castShadow = true }, 0)

-- По ID тайла (без координат)
local frag = GetTileFragmentSettingsById(tileId)
local frag = GetTileFragmentSettingsById(tileId, 0)      -- индекс тайлсета 0
SetTileFragmentSettingsById(tileId, { lifetime = 5.0, restitution = 0.5 })
SetTileFragmentSettingsById(tileId, { gravityScale = 0.3 }, 1)  -- дополнительный тайлсет 1
```

### Физический материал коллайдера тайла

```lua
-- Получить плотность коллайдера тайла
local density = GetTileColliderDensity(tileX, tileY)
local density = GetTileColliderDensity(tileX, tileY, 0)  -- instance 0

-- Установить плотность коллайдера тайла в рантайме
SetTileColliderDensity(tileX, tileY, 1.0)                -- Установить плотность
SetTileColliderDensity(tileX, tileY, 1.0, 0)             -- instance 0

-- Получить трение коллайдера тайла
local friction = GetTileColliderFriction(tileX, tileY)
local friction = GetTileColliderFriction(tileX, tileY, 0)  -- instance 0

-- Установить трение коллайдера тайла в рантайме
SetTileColliderFriction(tileX, tileY, 0.3)                -- Установить трение
SetTileColliderFriction(tileX, tileY, 0.3, 0)             -- instance 0

-- Получить упругость (отскок) коллайдера тайла
local restitution = GetTileColliderRestitution(tileX, tileY)
local restitution = GetTileColliderRestitution(tileX, tileY, 0)  -- instance 0

-- Установить упругость коллайдера тайла в рантайме
SetTileColliderRestitution(tileX, tileY, 0.5)                -- Установить упругость
SetTileColliderRestitution(tileX, tileY, 0.5, 0)             -- instance 0

-- Значения по умолчанию: density = 0.0, friction = 0.6, restitution = 0.0
-- Работает как для тайлсетных, так и для анимационных (флипбук) тайлов
-- Все функции принимают необязательный индекс инстанса как последний параметр
```

### Атомарное обновление свойств коллайдера тайла (одним вызовом)

```lua
-- Обновить сразу несколько свойств коллайдера тайла за один вызов. Дешевле, чем серия Set*,
-- и устраняет риск промежуточного несогласованного состояния.
-- Все поля опциональны — применяются только переданные.
SetTileColliderProperties(tileX, tileY, {
    density            = 1.5,
    friction           = 0.2,
    restitution        = 0.8,
    enableContactEvents = true,
    enableSensorEvents  = false,
    enableHitEvents     = true,
    enablePreSolveEvents = false,
})

-- С индексом инстанса
SetTileColliderProperties(tileX, tileY, { friction = 0.0 }, 0)
```

### Имя данных тайла (Data Name)

```lua
-- Получить имя данных тайла по мировым координатам (все слои или конкретный)
local name = GetTileDataName(worldX, worldY)
local name = GetTileDataName(worldX, worldY, 0)      -- instance 0
local name = GetTileDataName(worldX, worldY, 0, 1)    -- instance 0, слой 1

-- Получить имя данных тайла по координатам сетки
local name = GetTileDataNameGrid(tileX, tileY)
local name = GetTileDataNameGrid(tileX, tileY, 0)     -- instance 0
local name = GetTileDataNameGrid(tileX, tileY, 0, 1)  -- instance 0, слой 1

-- Работает как для обычных тайлов из тайлсета, так и для анимированных (флипбук) тайлов
-- Возвращает "" если имя данных не задано
-- Пример: геймплей на основе поверхности
if GetTileDataName(px, py) == "lava" then
    DealDamage(10)
elseif GetTileDataName(px, py) == "dirt" then
    speed = speed * 0.7
end
```

### Анимированные тайлы (Flipbook)

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
RemoveAnimatedTile(0, true)  -- очистить grid от placeholderID
```

### Chunking (оптимизация тайлмапа)

```lua
SetTilemapChunking(true)
SetTilemapChunking(true, 32)  -- включить и задать размер чанка
local enabled = IsTilemapChunkingEnabled()
local chunkSize = GetTilemapChunkSize()
```

---

## 31. Component — Проверка и управление компонентами

> **Тип:** Entity-bound + глобальные. Позволяют проверять наличие компонентов у любой сущности,
> добавлять и удалять компоненты в рантайме, а также управлять свойствами других сущностей.

### Проверка наличия компонентов у сущности

```lua
-- Проверить по ID сущности
local hasSprite = HasSprite(entityId)
local hasRb = HasRigidbody(entityId)
local hasColl = HasCollider(entityId)
local hasAnim = HasAnimator(entityId)
local hasSkel = HasSkeleton(entityId)  -- entityId необязателен; без него — текущая сущность
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

-- Проверить по имени компонента (у текущей сущности)
local has = HasComponent("Sprite")
local has = HasComponent("Rigidbody")
local has = HasComponent("Collider")
local has = HasComponent("Animator")
local has = HasComponent("Skeleton")
-- и т.д.: "Flipbook", "Audio", "FX", "PointLight", "Widget",
-- "Camera", "Tilemap", "SpotLight", "Joint", "PointMarker", "AI", "Destructible", "ClassComponent", "Hierarchy", "Interface", "GameplayTag"

-- Проверить по имени компонента у другой сущности
local has = EntityHasComponent(entityId, "Sprite")
```

### Добавление и удаление компонентов

```lua
-- Добавить компонент к текущей сущности
AddComponent("Sprite")         -- SpriteRendererComponent
AddComponent("Rigidbody")      -- RigidbodyComponent
AddComponent("Collider")       -- ColliderComponent
AddComponent("BoxCollider")    -- Добавить Box коллайдер (создаст ColliderComponent если нет)
AddComponent("CircleCollider") -- Добавить сферический коллайдер
AddComponent("SphereCollider") -- Алиас для CircleCollider
AddComponent("CapsuleCollider") -- Добавить капсульный коллайдер
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

-- Удалить компонент
RemoveComponent("Sprite")
RemoveComponent("Rigidbody")
RemoveComponent("ClassComponent")
-- и т.д. для всех опциональных типов

-- Базовые компоненты есть у КАЖДОЙ сущности и не удаляются: Transform, Tag, ID,
-- Stencil и Replication. RemoveComponent для них вернёт false и напишет предупреждение.
-- Отключайте их собственными флагами (Stencil.Enabled, Replication.Replicate).
-- AddComponent для базового компонента — безвредная пустая операция, вернёт false.

-- Добавить/удалить компонент у другой сущности
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

### ClassComponent (компоненты класса)

#### По ID сущности

```lua
-- Проверка наличия ClassComponent по имени компонента
local has = EntityHasComponent(entityId, "ClassComponent")

-- Количество компонент класса
local count = GetClassComponentCount(entityId)

-- Имя и путь компонента класса по индексу
local path = GetClassComponentPath(entityId, 0)
local name = GetClassComponentName(entityId, 0)

-- Поиск по имени
local exists = HasClassComponentByName(entityId, "MyComponent")
local index = FindClassComponentIndex(entityId, "MyComponent")

-- Добавить/удалить экземпляр компонента класса у другой сущности
-- AddEntityClassComponentInstance(entityId, name [, classPath]) → индекс добавленного экземпляра, либо -1 при ошибке
-- Если у целевой сущности ещё нет ClassComponentComponent — он будет создан автоматически.
local idx = AddEntityClassComponentInstance(entityId, "Weapon")
local idx = AddEntityClassComponentInstance(entityId, "Shield", "Content/Classes/Shield.ice_class")

-- RemoveEntityClassComponentInstance(entityId, index) → bool (true при успехе)
local ok = RemoveEntityClassComponentInstance(entityId, idx)

-- Изменить путь к классу / отображаемое имя у существующего экземпляра
SetEntityClassComponentInstancePath(entityId, 0, "Content/Classes/NewWeapon.ice_class")
SetEntityClassComponentInstanceName(entityId, 0, "MainWeapon")

-- Инстанцирование в рантайме: добавить И построить компоненты класса вживую.
-- AddEntityClassComponentInstance сохраняет только метаданные (компоненты класса
-- мёржатся при загрузке уровня / спавне сущности). Instantiate сразу добавляет
-- спрайты / флипбуки / коллайдеры / источники света / маркеры класса на сущность
-- и создаёт формы коллайдеров на её рантайм-теле физики.
-- InstantiateEntityClassComponent(entityId, name [, classPath]) → индекс экземпляра, либо -1
local liveIdx = InstantiateEntityClassComponent(entityId, "Shield", "Content/Classes/Shield.ice_class")

-- Построить компоненты уже добавленного (только-метаданные) экземпляра в рантайме.
-- ResolveEntityClassComponentInstance(entityId, index) → bool
ResolveEntityClassComponentInstance(entityId, idx)
```

#### Локальный трансформ компонента класса у другой сущности

```lua
-- Позиция (относительно трансформа сущности)
local pos = GetEntityClassComponentLocalPosition(entityId, 0)       -- → {x, y, z}
SetEntityClassComponentLocalPosition(entityId, 0, 10, -5)           -- (entityId, index, x, y)
SetEntityClassComponentLocalPosition(entityId, 0, 10, -5, 0.5)      -- (entityId, index, x, y, z)

-- Поворот (градусы)
local rot = GetEntityClassComponentLocalRotation(entityId, 0)       -- → float
SetEntityClassComponentLocalRotation(entityId, 0, 45)

-- Масштаб
local scale = GetEntityClassComponentLocalScale(entityId, 0)        -- → {x, y}
SetEntityClassComponentLocalScale(entityId, 0, 2, 2)
```

#### Мировой трансформ компонента класса у другой сущности

Мировой трансформ = трансформ сущности + локальный трансформ экземпляра
(учитывает поворот и масштаб сущности через `CombineTransforms`).
Эти функции только для чтения — чтобы подвинуть компонент класса, изменяйте его **локальный** трансформ.

```lua
local wp = GetEntityClassComponentWorldPosition(entityId, 0)        -- → {x, y, z}
local wr = GetEntityClassComponentWorldRotation(entityId, 0)        -- → float (градусы)
local ws = GetEntityClassComponentWorldScale(entityId, 0)           -- → {x, y}
```

#### Для текущей сущности (self)

```lua
-- Количество, имя, путь
local count = GetMyClassComponentCount()
local path = GetMyClassComponentPath(0)
local name = GetMyClassComponentName(0)

-- Поиск по имени
local exists = HasMyClassComponentByName("Weapon")
local index = FindMyClassComponentIndex("Weapon")

-- Добавить/удалить инстанс класс-компонента
local idx = AddClassComponentInstance("Weapon")                               -- Просто имя
local idx = AddClassComponentInstance("Shield", "Content/Classes/Shield.ice_class") -- С путём
RemoveClassComponentInstance(idx)

-- Установить путь класса для инстанса
SetClassComponentInstancePath(0, "Content/Classes/NewWeapon.ice_class")

-- Инстанцирование в рантайме (self): добавить И построить класс вживую (см. примечания выше).
-- InstantiateClassComponent(name [, classPath]) → индекс экземпляра, либо -1
local liveIdx = InstantiateClassComponent("Shield", "Content/Classes/Shield.ice_class")
-- Построить уже добавленный (только-метаданные) экземпляр: ResolveClassComponentInstance(index) → bool
ResolveClassComponentInstance(liveIdx)
```

#### Локальный трансформ класс-компонента

```lua
-- Позиция (относительно сущности)
local pos = GetClassComponentLocalPosition(0)   -- → {x, y, z}
SetClassComponentLocalPosition(0, 10, -5)       -- (index, x, y)
SetClassComponentLocalPosition(0, 10, -5, 0.5)  -- (index, x, y, z)

-- Поворот
local rot = GetClassComponentLocalRotation(0)   -- → float (градусы)
SetClassComponentLocalRotation(0, 45)

-- Масштаб
local scale = GetClassComponentLocalScale(0)    -- → {x, y}
SetClassComponentLocalScale(0, 2, 2)
```

#### Мировой трансформ класс-компонента

Мировой трансформ = трансформ сущности + локальный трансформ компонента
(с учётом поворота и масштаба сущности, аналог `CombineTransforms`).

```lua
-- Мировая позиция
local wp = GetClassComponentWorldPosition(0)    -- → {x, y, z}

-- Мировой поворот
local wr = GetClassComponentWorldRotation(0)    -- → float

-- Мировой масштаб
local ws = GetClassComponentWorldScale(0)       -- → {x, y}
```

**Пример: Класс-компонент как оружие:**

```lua
function OnCreate()
    -- Найти компонент "Sword" и поставить его правее от сущности
    local idx = FindMyClassComponentIndex("Sword")
    if idx >= 0 then
        SetClassComponentLocalPosition(idx, 30, 0)
        SetClassComponentLocalRotation(idx, -15)
    end
end

function OnUpdate(dt)
    -- Получить мировую позицию кончика меча
    local idx = FindMyClassComponentIndex("Sword")
    if idx >= 0 then
        local tip = GetClassComponentWorldPosition(idx)
        -- tip.x, tip.y — абсолютная позиция с учётом поворота и масштаба сущности
    end
end
```

### Управление свойствами других сущностей

```lua
-- Трансформация
SetEntityRotation(entityId, 45)
local rot = GetEntityRotation(entityId)
SetEntityScale(entityId, 2, 2)
local scale = GetEntityScale(entityId)  -- → {x, y}

-- Инкрементальные операции над другими сущностями
TranslateEntity(entityId, 10, 5)       -- Сдвинуть позицию на (dx, dy)
RotateEntity(entityId, 15)             -- Добавить к повороту (градусы)
ScaleEntity(entityId, 0.5, 0.5)        -- Добавить к масштабу (dx, dy)

-- Спрайт
SetEntitySpriteFlipX(entityId, true)
SetEntitySpriteFlipY(entityId, false)
SetEntitySpriteColor(entityId, 1, 0, 0, 1)  -- r, g, b, a
SetEntitySpriteVisible(entityId, true)
SetEntitySpriteAlpha(entityId, 0.5)
SetEntitySpriteOrder(entityId, 5)
local order = GetEntitySpriteOrder(entityId)

-- Физика
SetEntityGravityScale(entityId, 0)
FreezeEntity(entityId)        -- Сделать статическим
UnfreezeEntity(entityId)      -- Сделать динамическим
StopEntityMovement(entityId)  -- Обнулить скорость

-- Аниматор
SetEntityAnimBool(entityId, "isRunning", true)
SetEntityAnimInt(entityId, "direction", 2)
SetEntityAnimFloat(entityId, "speed", 150)
SetEntityAnimTrigger(entityId, "attack")

-- Целевой спрайт аниматора
SetAnimTargetSprite("Body")         -- Указать целевой спрайт по имени
local target = GetAnimTargetSprite() -- Получить имя целевого спрайта

-- Свет (Point Lights внутри LightComponent сущности; необязательный `index` адресует конкретный экземпляр — по умолчанию 0 = первый)
SetEntityLightEnabled(entityId, true)
SetEntityLightEnabled(entityId, true, 1)         -- адресовать второй point light этой сущности
SetEntityLightColor(entityId, 1, 0.8, 0.3)
SetEntityLightIntensity(entityId, 2.0)
SetEntityLightRadius(entityId, 200)
SetEntityLightFalloff(entityId, 2.0)              -- 1=линейный, 2=квадратичный (реалистично), больше=резче
SetEntityLightCastShadows(entityId, true)         -- переключатель теней для конкретного света (нужны включённые глобальные тени)
SetEntityLightLocalPosition(entityId, 0, 0)       -- локальное смещение внутри LightComponent

local n        = GetEntityLightCount(entityId)               -- кол-во point-лайтов у сущности
local enabled  = GetEntityLightEnabled(entityId)             -- bool
local cast     = GetEntityLightCastShadows(entityId)         -- bool
local i        = GetEntityLightIntensity(entityId)
local r        = GetEntityLightRadius(entityId)
local f        = GetEntityLightFalloff(entityId)
local c        = GetEntityLightColor(entityId)               -- → {r, g, b}

-- Spot Light (прожекторы в том же LightComponent — отдельное пространство индексов от PointLights)
local sn = GetEntitySpotLightCount(entityId)
SetEntitySpotLightEnabled(entityId, true)
SetEntitySpotLightColor(entityId, 1, 0.95, 0.85)
SetEntitySpotLightIntensity(entityId, 1.5)
SetEntitySpotLightRadius(entityId, 400)
SetEntitySpotLightDirection(entityId, 0, -1)               -- нормализуется автоматически; (0,-1)=вниз (мир Y-up)
SetEntitySpotLightAngles(entityId, 15, 30)                 -- innerDeg, outerDeg (мягкий конус)
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

-- Все `*Spot*` и Light* функции принимают необязательный последний аргумент `index` (по умолчанию 0).
-- Эти функции настраивают экземпляры в компоненте; глобальные настройки солнца, теней и ambient
-- см. в секции 13.3 (`SetDirectionalLight`, `SetShadow*`, `SetAmbientLight`, …).

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

-- Маска (Stencil) — управление stencil-маскировкой на уровне сущности
SetStencilEnabled(entityId, true)
local enabled = GetStencilEnabled(entityId)
SetStencilMode(entityId, 1)        -- 0=Off, 1=Write (записывает маску), 2=Read (читает маску)
local mode = GetStencilMode(entityId)
SetStencilID(entityId, 1)          -- Stencil ID (1–255), связывает писателей и читателей
local id = GetStencilID(entityId)
SetStencilCompareFunc(entityId, 0) -- 0=Equal (рисовать внутри маски), 1=NotEqual (рисовать вне маски)
local cmp = GetStencilCompareFunc(entityId)

-- Варианты для текущей сущности (Self)
SetMyStencilEnabled(true)
local enabled = GetMyStencilEnabled()
SetMyStencilMode(1)
local mode = GetMyStencilMode()
SetMyStencilID(1)
local id = GetMyStencilID()
SetMyStencilCompareFunc(0)
local cmp = GetMyStencilCompareFunc()
```

### Массовые операции по тегу

```lua
-- Установить скорость ВСЕМ сущностям с тегом
SetTagVelocity("Enemy", 0, 0)

-- Установить FlipX/FlipY всем спрайтам с тегом
SetTagFlipX("Enemy", true)
SetTagFlipY("Enemy", true)

-- Установить параметр аниматора всем с тегом
SetTagAnimBool("Enemy", "isDead", true)

-- Удалить ВСЕ сущности с тегом
DestroyAllByTag("Bullet")

-- Подсчитать сущности с тегом
local count = CountEntitiesByTag("Enemy")
```

---

## 32. Network — Мультиплеер (Сеть)

> **Тип:** Глобальные функции. Таблица `Network`.
>
> Встроенная сетевая система для мультиплеера: клиент-серверная архитектура,
> комнаты, RPC, синхронизация сущностей, голосовой чат, переменные, интерполяция.

### Запуск сервера и подключение

```lua
-- Запустить сервер
Network.StartServer(7777, 16)              -- порт, макс. игроков
Network.StartServer(7777, 16, "password")  -- с паролем
Network.StartServer(7777, 16, "password", 8080) -- 4-й аргумент: WebSocket-порт, чтобы подключались браузерные (web) клиенты; 0/опущен = только нативный (ENet)

-- Остановить сервер
Network.StopServer()

-- Подключиться к серверу
Network.Connect("127.0.0.1", 7777)
Network.Connect("127.0.0.1", 7777, "password")

-- Отключиться
Network.Disconnect()

-- Проверки состояния
local connected = Network.IsConnected()
local server = Network.IsServer()
local connecting = Network.IsConnecting()
local host = Network.IsHost()
```

### Информация о игроках

```lua
-- Локальный игрок
local myId = Network.GetLocalPlayerID()
local myName = Network.GetLocalPlayerName()
Network.SetPlayerName("Hero")

-- Все игроки
local count = Network.GetPlayerCount()
local ids = Network.GetAllPlayerIDs()  -- таблица ID
local name = Network.GetPlayerName(playerId)
local isHost = Network.IsPlayerHost(playerId)

-- Хост
local hostId = Network.GetHostPlayerID()

-- Подробная информация об игроке (возвращает таблицу со всеми полями)
local info = Network.GetPlayerInfo(playerId)
-- → {id, name, isHost, isLocal, ping, bytesSent, bytesReceived,
--    voiceMuted, textMuted, voiceEnabled, voiceVolume,
--    isTransmitting, connectDuration, chatChannels}

-- Проверить, локальный ли игрок
local isLocal = Network.IsPlayerLocal(playerId)

-- Время с момента подключения игрока (секунды)
local duration = Network.GetPlayerConnectDuration(playerId)
```

### Чат и сообщения

```lua
-- Отправить чат всем
Network.SendChatMessage("Привет всем!")

-- Приватное сообщение
Network.SendPrivateMessage(playerId, "Секрет!")

-- Callback
Network.OnChatReceived(function(playerId, message)
    Print(Network.GetPlayerName(playerId) .. ": " .. message)
end)

-- Каналы чата
Network.SendChannelMessage("global", "Привет каналу!")
Network.JoinChannel("trade")
Network.LeaveChannel("trade")
local channels = Network.GetJoinedChannels()  -- таблица имён каналов

-- История чата
local history = Network.GetChatHistory()
-- → массив {senderID, targetID, channel, content, isPrivate, isSystem}

-- Очистить историю чата
Network.ClearChatHistory()

-- Системное сообщение (только сервер, рассылается всем клиентам)
Network.SendSystemMessage("Сервер перезагрузится через 5 минут!")

-- История чата, отфильтрованная по каналу
local channelHistory = Network.GetChatHistoryForChannel("global")
-- → массив {senderID, targetID, channel, content, isPrivate, isSystem}

-- Ограничение частоты чата
Network.SetChatCooldown(1.5)               -- минимальная задержка между сообщениями (секунды)
local cd = Network.GetChatCooldown()       -- текущее значение задержки
Network.SetChatBurstLimit(5)               -- макс. сообщений за один всплеск
local burst = Network.GetChatBurstLimit()  -- текущий лимит всплеска
Network.SetChatRateWindow(10.0)            -- временное окно ограничения (секунды)
local window = Network.GetChatRateWindow() -- текущее временное окно
```

### Отправка данных

```lua
-- Строковые данные
Network.SendData(playerId, "hello", true)        -- reliable
Network.BroadcastData("gameStart", true)          -- всем
Network.SendDataToServer("myInput", true)         -- серверу
Network.BroadcastDataExcept(playerId, "data")     -- всем, кроме одного

-- Короткие формы
Network.SendDataReliable(playerId, "data")
Network.SendDataUnreliable(playerId, "data")
Network.BroadcastDataReliable("data")
Network.BroadcastDataUnreliable("data")

-- Таблицы (автоматическая сериализация в JSON)
Network.SendTable(playerId, { hp = 100, pos = { x = 10, y = 20 } })
Network.BroadcastTable({ event = "explosion", x = 100, y = 200 })
Network.SendTableToServer({ input = "jump" })
Network.BroadcastTableExcept(playerId, { msg = "update" })

-- JSON вручную
local json = Network.JsonEncode({ key = "value" })
local tbl = Network.JsonDecode(json)
```

Вызовы `BroadcastData`/`BroadcastTable`/`BroadcastDataExcept`/`BroadcastTableExcept`, а также
адресные `SendData`/`SendTable` с клиента автоматически ретранслируются через хост:
получатели видят в `OnDataReceived`/`OnTableReceived` реальный `playerId` отправителя.

### Callbacks (события)

```lua
Network.OnConnected(function(msg) Print("Подключён!") end)
Network.OnConnectionFailed(function(msg) Print("Ошибка: " .. msg) end)
Network.OnDisconnected(function(msg) Print("Отключён!") end)
Network.OnPlayerJoined(function(playerId, name) Print(name .. " вошёл") end)
Network.OnPlayerLeft(function(playerId, name) Print(name .. " вышел") end)
Network.OnDataReceived(function(playerId, data) Print("Данные: " .. data) end)
Network.OnTableReceived(function(playerId, tbl) Print("HP: " .. tbl.hp) end)
Network.OnInputReceived(function(playerId, data) Print("Ввод от " .. playerId) end)
Network.OnPrivateChatReceived(function(playerId, message) Print("ЛС: " .. message) end)
Network.OnChannelChatReceived(function(playerId, channel, content) Print(channel .. ": " .. content) end)
Network.OnVoiceReceived(function(playerId, voiceData) ... end)
Network.OnPlayerVarChanged(function(playerId, key, value)
    Print("Игрок " .. playerId .. ": " .. key .. " = " .. value)
end)

-- Очистить все callback'и
Network.ClearCallbacks()
```

### RPC (Remote Procedure Call)

```lua
-- Зарегистрировать RPC
Network.RegisterRPC("DealDamage", function(playerId, data)
    Print("Урон от " .. playerId .. ": " .. data)
end)

-- Вызвать RPC
Network.CallRPC("DealDamage", "50")            -- Всем
Network.CallRPCOnServer("DealDamage", "50")    -- Серверу
Network.CallRPCOnPlayer(playerId, "DealDamage", "50")  -- Конкретному игроку

-- RPC с таблицами
Network.RegisterTableRPC("SyncState", function(playerId, tbl)
    Print("State: hp=" .. tbl.hp)
end)
Network.CallTableRPC("SyncState", { hp = 100, ammo = 30 })
Network.CallTableRPCOnServer("SyncState", { hp = 80 })
Network.CallTableRPCOnPlayer(playerId, "SyncState", { hp = 50 })
```

### Сетевые переменные

```lua
-- Глобальные переменные (синхронизируются между всеми)
Network.SetNetVar("gameMode", "deathmatch")
local mode = Network.GetNetVar("gameMode", "default")
local has = Network.HasNetVar("gameMode")
local all = Network.GetAllNetVars()  -- таблица key=value
Network.ClearNetVars()

-- Серверно-авторитетные переменные: писать в них может только хост/сервер.
-- Запись от клиента сервер отклоняет и возвращает клиенту истинное значение.
-- Используйте для доверенного состояния, которое клиент не должен подделывать
-- (счёт, банк, чей сейчас ход, ...).
Network.SetAuthoritativeNetVar("pot", "1500")        -- только хост/сервер
local isAuth = Network.IsAuthoritativeNetVar("pot")  -- true
Network.ClearAuthoritativeNetVar("pot")              -- только хост/сервер; снимает защиту ключа

-- Переменные игрока
Network.SetPlayerVar(playerId, "team", "red")
local team = Network.GetPlayerVar(playerId, "team", "none")
local has = Network.HasPlayerVar(playerId, "team")
local all = Network.GetAllPlayerVars(playerId)
Network.ClearPlayerVars(playerId)

-- Callback'и при изменении
Network.OnNetVarChanged(function(key, value)
    Print(key .. " = " .. value)
end)
Network.OnPlayerVarChanged(function(playerId, key, value)
    Print("Игрок " .. playerId .. ": " .. key .. " = " .. value)
end)
```

### Комнаты

```lua
-- Создать комнату
Network.CreateRoom("MyRoom", 8, "password")   -- имя, макс. игроков, пароль

-- Войти/выйти
Network.JoinRoom("MyRoom")
Network.JoinRoom("MyRoom", "password")
Network.LeaveRoom()

-- Информация
local room = Network.GetCurrentRoom()
local inRoom = Network.IsInRoom()
local players = Network.GetRoomPlayers("MyRoom")
local count = Network.GetRoomPlayerCount("MyRoom")
local max = Network.GetRoomMaxPlayers("MyRoom")
local full = Network.IsRoomFull("MyRoom")
local isHost = Network.IsRoomHost()

-- Все комнаты
local rooms = Network.GetAllRoomNames()
local roomCount = Network.GetRoomCount()

-- Свойства комнаты
Network.SetRoomProperty("MyRoom", "map", "Forest")
local map = Network.GetRoomProperty("MyRoom", "map", "Default")
local allProps = Network.GetAllRoomProperties("MyRoom")  -- таблица всех свойств (key-value)

-- Хост комнаты
local hostId = Network.GetRoomHost("MyRoom")  -- ID игрока-хоста комнаты

-- Проверка пароля комнаты
local hasPwd = Network.HasRoomPassword("MyRoom")

-- Установить макс. игроков в комнате
Network.SetRoomMaxPlayers("MyRoom", 12)

-- Полная информация о комнате (возвращает таблицу со всеми деталями)
local roomInfo = Network.GetRoomInfo("MyRoom")
-- → {name, maxPlayers, playerCount, hostPlayerID, hasPassword, isFull, players, properties}

-- Удалить комнату
Network.DestroyRoom("MyRoom")

-- Callbacks
Network.OnRoomJoined(function(roomName) ... end)
Network.OnRoomJoinFailed(function(roomName, reason) ... end)  -- reason: "password" | "full" | "notfound"
Network.OnRoomLeft(function(roomName) ... end)
Network.OnRoomPlayerJoined(function(roomName, playerId) ... end)
Network.OnRoomPlayerLeft(function(roomName, playerId) ... end)
```

> **Пароли комнат серверно-авторитетны.** Хеш пароля хранится только на сервере
> (и у создателя комнаты) и никогда не рассылается другим клиентам — пир может узнать
> лишь *факт* наличия пароля (`Network.HasRoomPassword`), но не сам хеш. Для удалённого
> клиента `Network.JoinRoom(name, password)` **асинхронна**: она отправляет запрос и
> сразу возвращает управление; реальный результат приходит через `OnRoomJoined` (успех)
> или `OnRoomJoinFailed` (неверный пароль / комната полна / не найдена). Если вы — хост,
> вход проверяется локально и `OnRoomJoined` срабатывает синхронно.

### Автоматическая репликация (рекомендуется)

Пометьте сущность один раз — и движок сам держит её синхронизированной у всех
подключённых клиентов: трансформ, скорость, анимация и все визуальные компоненты, плюс
авто-спавн/деспавн соответствующей сущности на каждом клиенте. Это высокоуровневый слой
«пометил и забыл»; ручные `SyncEntity*` ниже — низкоуровневый API.

```lua
-- Только хост/сервер. Пометить сущность для автоматической репликации.
-- Возвращает стабильный сетевой id (netId), общий для всех машин.
local netId = Network.Replicate(entityId)
local netId = Network.Replicate(entityId, { owner = playerId })          -- задать игрока-владельца (метаданные)
local netId = Network.Replicate(spawnedId, { prefab = "Content/Bullet.ice_class" })

-- Прекратить репликацию (level-сущности остаются, спавн-копии удаляются у клиентов).
Network.StopReplicating(entityId)

-- Запросы (любой пир)
local on   = Network.IsReplicated(entityId)
local id   = Network.GetNetId(entityId)        -- 0, если не реплицируется
local ent  = Network.GetEntityByNetId(netId)   -- локальный id сущности или 0
local count= Network.GetReplicatedCount()
Network.SetReplicationOwner(entityId, playerId)

-- После изменения *конфигурационного* компонента в рантайме на хосте (смена спрайта,
-- цвет света, тайлмап, виджет, настройки ИИ, ...) — разовая отправка полного состояния:
Network.ReplicateFullState(entityId)

-- Частота проверки/отправки изменений полного состояния (по умолчанию 8 Гц).
Network.SetReplicationRate(8)

-- Декларативная репликация (рекомендуется): поставьте галочку «Реплицировать» в
-- компоненте Репликация в панели свойств (сущность уровня) или в редакторе классов
-- (весь класс). Хост сам зарегистрирует и анонсирует её — Lua не нужен, а настройки
-- переносятся через наследование классов, копирование/вставку, клонирование и спавн.
-- Те же настройки читаются и пишутся из Lua:
local rep = Network.GetReplicationSettings(entityId)
-- rep.replicate, rep.ownerMode ("server"|"player"), rep.owner,
-- rep.syncTransform, rep.syncVelocity, rep.syncVisuals, rep.syncFullState,
-- rep.fullStateRate, rep.scriptMode ("auto"|"always"|"never"),
-- rep.relevancy ("aoi"|"always"), rep.kinematicOnClients, rep.prefab

Network.SetReplicationSettings(entityId, {
    replicate = true,
    ownerMode = "player",      -- "server" (полномочия хоста) или "player"
    owner = playerId,          -- ненулевой владелец сам включает ownerMode "player"
    syncTransform = true,      -- позиция/поворот/масштаб каждый тик
    syncVelocity = true,       -- линейная скорость физического тела каждый тик
    syncVisuals = true,        -- спрайт/флипбук/скелет/параметры аниматора каждый тик
    syncFullState = true,      -- периодические конфиг-компоненты (свет, тайлмап, ИИ, ...)
    fullStateRate = 0,         -- Гц; 0 = использовать глобальную частоту репликации
    scriptMode = "auto",       -- "auto" | "always" | "never" (Lua-колбэки на репликах)
    relevancy = "aoi",         -- "aoi" (отсекается областью интереса) | "always"
    kinematicOnClients = true, -- чтобы клиентская физика не конфликтовала с сетью
    prefab = "Content/Enemies/CL_Bat.ice_class", -- только для сущностей, созданных в рантайме
})

-- Короткие формы
Network.SetEntityReplicated(entityId, true)
local on = Network.IsEntityReplicated(entityId)

-- Императивный Network.Replicate() ниже продолжает работать и теперь пишет в тот же
-- компонент, поэтому панель свойств всегда отражает актуальное состояние.

-- Гейт скриптов реплик (по умолчанию: включён). Пока включён, клиент НЕ выполняет
-- Lua-колбэки жизненного цикла (OnUpdate/OnFixedUpdate/OnLateUpdate, события
-- коллизий/сенсоров/ударов, деревья поведения) для реплицируемых сущностей,
-- принадлежащих серверу или другому игроку — хост симулирует их сам, а клиент лишь
-- применяет реплицированное состояние. Интерфейсные вызовы (Interfaces.TryCall и т.п.)
-- на таких сущностях продолжают работать, поэтому хост может запускать клиентские
-- визуальные функции. Отключайте, только если игра сознательно выполняет скрипты
-- реплик на клиентах.
Network.SetReplicaScriptGating(true)
local gated = Network.IsReplicaScriptGating()

-- Репликация уровня: хост меняет уровень сразу для всех.
Network.LoadNetworkLevel("Content/Arena.icemap")
-- Клиенты сами грузят тот же уровень. Чтобы кастомизировать загрузку на клиенте:
Network.OnNetworkLevelLoad(function(path) LoadLevel(path) end)
```

**Модель.** Репликация **авторитетна со стороны хоста**: хост симулирует всё, клиенты
применяют результат. Что автоматически после пометки сущности:

- **Спавн / деспавн + координация id** — хост назначает стабильный `netId`. Сущности,
  размещённые в уровне, привязываются на клиентах по их UUID из уровня; созданные в
  рантайме несут путь `prefab` и инстанцируются на клиентах через `SpawnEntity`.
  Уничтожение сущности на хосте удаляет её у всех клиентов.
- **Каждый тик (плавно):** позиция, поворот, масштаб, скорость, параметры аниматора и
  текущий кадр активного флипбука. Удалённые тела делаются kinematic, чтобы клиентская
  физика не конфликтовала с сетью.
- **По изменению (с троттлингом):** все прочие визуальные/конфиг-компоненты (спрайт,
  свет, тайлмап, виджет, разрушаемость, ИИ, теги, ...) — определяется автоматически и
  отправляется только при реальном изменении.
- Событийные вещи (звук, частицы/FX, одноразовые эффекты) намеренно **не** реплицируются
  автоматически — запускайте их через RPC (`Network.CallRPC`), чтобы сработало ровно один раз.

Клиенты шлют **ввод** (например, `Network.SendInput`, RPC); хост двигает сущности;
результат реплицируется обратно. Для тонкого/кастомного контроля остаётся ручной API
`SyncEntity*` ниже.

### Синхронизация сущностей

```lua
-- Простая синхронизация позиции/скорости
Network.SyncEntityPosition(entityId, x, y)
Network.SyncEntityPosition(entityId, x, y, z)
Network.SyncEntityVelocity(entityId, vx, vy)
Network.SyncEntityTransform(entityId, x, y, rotation)
Network.SyncEntityTransform(entityId, x, y, rotation, vx, vy)

-- Синхронизация произвольного состояния
Network.SyncEntityState(entityId, { hp = 100, state = "idle" })

-- Авторитарная синхронизация
Network.SyncEntityAuth(entityId, x, y)
Network.SyncEntityAuth(entityId, x, y, z, rotation)

-- Спавн/удаление по сети
Network.NetSpawnEntity("Content/Classes/Bullet.ice_class", x, y)
Network.NetSpawnEntity("Content/Classes/Bullet.ice_class", x, y, ownerPlayerId)
Network.NetDestroyEntity(entityId)

-- Владелец сущности
Network.SetEntityOwner(entityId, playerId)
local owner = Network.GetEntityOwner(entityId)
local mine = Network.IsEntityMine(entityId)
local isOwner = Network.IsEntityOwner(entityId, playerId)
Network.ClearEntityOwner(entityId)

-- Расширенная система сущностей
Network.RegisterEntity(entityId, ownerPlayerId)
Network.UnregisterEntity(entityId)
Network.UpdateEntityState(entityId, x, y, z, rotation, vx, vy)
Network.UpdateEntityProperty(entityId, "hp", "100")
local state = Network.GetEntityState(entityId)
-- Базовые поля: x, y, z, rotation, vx, vy, scaleX, scaleY, flipX, flipY,
--   pivotX, pivotY, owner, timestamp, properties
-- Аниматор: animatorState, animatorFrame, animatorStateTime, animatorPath,
--   animatorTargetSprite, animatorIsTransitioning
-- Параметры аниматора: animatorParamBools, animatorParamInts,
--   animatorParamFloats, animatorParamTriggers
-- Флипбук: flipbookFrame, flipbookTimer, flipbookPlaying, flipbookSpeed,
--   flipbookColorR/G/B/A, flipbookVisible, flipbookFlipX/Y, flipbookPath
-- Спрайт: spriteColorR/G/B/A, spriteVisible, spriteFlipX/Y, spritePath
-- Аудио: audioVolume, audioPitch, audioPlaying, audioLoop,
--   audioSpatial, audioMinDistance, audioMaxDistance, audioRolloff, audioPath
-- FX: fxPlaying, fxLoop, fxSpeed, fxFlipX/Y, fxVisible, fxPath
-- Rigidbody: rigidbodyType, rigidbodyGravityScale, rigidbodyFixedRotation,
--   rigidbodyLinearDamping, rigidbodyAngularDamping, rigidbodyIsBullet, rigidbodyAllowSleep,
--   ragdollEnabled, ragdollGravityScale, ragdollAngularDamping
-- Точечный свет: lightColorR/G/B, lightIntensity, lightRadius,
--   lightFalloffExponent, lightCastShadows, lightEnabled
-- Прожектор: spotLightColorR/G/B, spotLightIntensity, spotLightRadius,
--   spotLightFalloffExponent, spotLightDirX/Y,
--   spotLightInnerAngle, spotLightOuterAngle, spotLightCastShadows, spotLightEnabled
-- Тайлмап: tilemapFlipX/Y, tilemapVisible, tilemapPath
-- Виджет: widgetVisible, widgetScreenSpace, widgetScale,
--   widgetRenderOrder, widgetInteractable, widgetFlipX/Y, widgetPath
-- Разрушение: destructEnabled, destructHealth, destructFragmentCount,
--   destructPattern, destructExplosionForce, destructOnStart, destructImpactThreshold,
--   destructFragmentLifetime, destructFragmentFadeTime, destructFragmentGravityScale,
--   destructFragmentDensity, destructFragmentFriction, destructFragmentRestitution,
--   destructFragmentIsSensor, destructFragmentEnableContactEvents,
--   destructFragmentEnableSensorEvents, destructFragmentEnableHitEvents,
--   destructFragmentEnablePreSolveEvents,
--   destructMaxDebrisCount, destructDestroyOriginal,
--   destructFragmentCastShadow, destructFragmentShadowOrigin, destructFragmentShadowEdgeFade,
--   destructFragmentShadowZOrder
-- ИИ: aiMoveSpeed, aiDetectionRadius, aiAttackRadius, aiPerceptionEnabled,
--   aiAssetPath, aiPerceptionSightRadius, aiPerceptionSightAngle,
--   aiPerceptionHearingRadius, aiPerceptionRequireLOS, aiPerceptionForgetTime,
--   aiPatrolPoints (массив {x, y}), aiPerceptionTargetTags (массив строк)
-- Компонент классов: classComponentInstances (массив {name, classPath}),
--   classComponentSelectedIndex
local isOwnerAuth = Network.IsEntityOwnerAuth(entityId, playerId)
Network.TransferEntityOwnership(entityId, newOwnerId)
local allIds = Network.GetAllEntityIDs()
local count = Network.GetEntityCount()
```

### Визуал сущностей

```lua
-- Обновить локальное визуальное состояние (масштаб, отражение, пивот)
Network.UpdateEntityVisuals(entityId, scaleX, scaleY, flipX, flipY, pivotX, pivotY)

-- Синхронизировать визуал по сети (обновляет локально + отправляет всем)
Network.SyncEntityVisuals(entityId, scaleX, scaleY, flipX, flipY, pivotX, pivotY)
Network.SyncEntityVisuals(entityId, scaleX, scaleY, flipX, flipY, pivotX, pivotY, reliable)
-- Все параметры кроме entityId опциональны (по умолчанию: scale=1, flip=false, pivot=0.5)
```

### Свойства сущностей

```lua
-- Установить свойство (строковые ключ-значение)
Network.UpdateEntityProperty(entityId, "hp", "100")

-- Прочитать одно свойство
local hp = Network.GetEntityProperty(entityId, "hp", "0")  -- ключ, значение по умолчанию

-- Прочитать все свойства как таблицу
local props = Network.GetEntityProperties(entityId)  -- → {hp = "100", armor = "50"}

-- Проверить наличие свойства
local has = Network.HasEntityProperty(entityId, "hp")
```

### Синхронизация компонентов

```lua
-- Аниматор (анимация через конечный автомат)
Network.SyncEntityAnimator(entityId, "Run", 3)              -- состояние, кадр
Network.SyncEntityAnimator(entityId, "Run", 3, 0.5)         -- + время состояния
Network.SyncEntityAnimator(entityId, "Run", 3, 0.5, true)   -- + reliable

-- Конфигурация аниматора (путь, целевой спрайт, переход)
Network.SyncEntityAnimatorConfig(entityId, "Content/Animators/Hero.ice_animator")
Network.SyncEntityAnimatorConfig(entityId, "Content/Animators/Hero.ice_animator", "idle_0", false, true)

-- Флипбук (покадровая анимация)
Network.SyncEntityFlipbook(entityId, 5)                      -- кадр
Network.SyncEntityFlipbook(entityId, 5, 0.1, true, 1.0)     -- кадр, таймер, воспроизведение, скорость
Network.SyncEntityFlipbook(entityId, 5, 0.1, true, 1.0, true) -- + reliable

-- Визуал флипбука (цвет, видимость, отражение, путь)
Network.SyncEntityFlipbookVisuals(entityId, 1.0, 1.0, 1.0)           -- r, g, b
Network.SyncEntityFlipbookVisuals(entityId, 1.0, 1.0, 1.0, 1.0, true, false, false, true)
-- r, g, b, a, visible, flipX, flipY, reliable
Network.SyncEntityFlipbookVisuals(entityId, 1.0, 1.0, 1.0, 1.0, true, false, false, true, "Content/Flipbooks/Hero.ice_flipbook")
-- r, g, b, a, visible, flipX, flipY, reliable, path

-- Скелет (воспроизведение скелетной 2D-анимации)
Network.SyncEntitySkeleton(entityId, "Run")                  -- анимация
Network.SyncEntitySkeleton(entityId, "Run", "default", 0.5, 1.0, true, true, false, true)
-- анимация, скин, время, скорость, воспроизведение, цикл, flipX, flipY
Network.SyncEntitySkeleton(entityId, "Run", "default", 0.5, 1.0, true, true, false, true, "Content/Skeletons/Hero.ice_skeleton", true)
-- анимация, скин, время, скорость, воспроизведение, цикл, flipX, flipY, путь, reliable

-- Визуал скелета (цвет, видимость, тень, материал, рэгдолл, замены аттачей слотов)
Network.SyncEntitySkeletonVisuals(entityId, 1.0, 1.0, 1.0)           -- r, g, b
Network.SyncEntitySkeletonVisuals(entityId, 1.0, 1.0, 1.0, 1.0, true, true, false, 0, 0.0, 0.0, 1, 0, 0.5, "Content/Materials/Hero.ice_material", false, false, 0.5, 1.0)
-- r, g, b, a, visible, renderInGame, castShadow, shadowOrigin, shadowEdgeFade, shadowZOrder,
-- shadingMode, blendMode, alphaClipThreshold, materialPath, ragdollEnabled, ragdollAutoOnStart,
-- ragdollAngularDamping, ragdollGravityScale
Network.SyncEntitySkeletonVisuals(entityId, 1.0, 1.0, 1.0, 1.0, true, true, false, 0, 0.0, 0.0, 1, 0, 0.5, "", false, false, 0.5, 1.0, { [2] = "sword" }, true)
-- + slotAttachments ({ [индекс слота] = "имя аттача" }), reliable

-- Спрайт (цвет, видимость, отражение, путь)
Network.SyncEntitySprite(entityId, 1.0, 0.0, 0.0)                    -- r, g, b
Network.SyncEntitySprite(entityId, 1.0, 0.0, 0.0, 1.0, true, false, false, true)
-- r, g, b, a, visible, flipX, flipY, reliable
Network.SyncEntitySprite(entityId, 1.0, 0.0, 0.0, 1.0, true, false, false, true, "Content/Sprites/Hero.ice_sprite")
-- r, g, b, a, visible, flipX, flipY, reliable, path

-- Аудио (громкость, высота, воспроизведение, пространственность, путь)
Network.SyncEntityAudio(entityId)                                     -- по умолчанию
Network.SyncEntityAudio(entityId, 0.8, 1.0, true, false, true, 1.0, 100.0, 1.0, true)
-- volume, pitch, playing, loop, spatial, minDistance, maxDistance, rolloff, reliable
Network.SyncEntityAudio(entityId, 0.8, 1.0, true, false, true, 1.0, 100.0, 1.0, true, "Content/Audio/Shot.ice_audio")
-- volume, pitch, playing, loop, spatial, minDistance, maxDistance, rolloff, reliable, path

-- FX / Частицы (воспроизведение, видимость, путь)
Network.SyncEntityFX(entityId)                                        -- по умолчанию
Network.SyncEntityFX(entityId, true, true, 1.0, false, false, true, true)
-- playing, loop, speed, flipX, flipY, visible, reliable
Network.SyncEntityFX(entityId, true, true, 1.0, false, false, true, true, "Content/FX/Explosion.ice_fx")
-- playing, loop, speed, flipX, flipY, visible, reliable, path

-- Rigidbody (тип тела, физические свойства, рэгдолл)
Network.SyncEntityRigidbody(entityId, 2)                              -- bodyType (0=static, 1=kinematic, 2=dynamic)
Network.SyncEntityRigidbody(entityId, 2, 1.0, false, 0.0, 0.0, false, true, true)
-- bodyType, gravityScale, fixedRotation, linearDamping, angularDamping, isBullet, allowSleep, reliable
Network.SyncEntityRigidbody(entityId, 2, 1.0, false, 0.0, 0.0, false, true, true, true, 1.0, 0.5)
-- bodyType, gravityScale, fixedRotation, linearDamping, angularDamping, isBullet, allowSleep, reliable,
-- ragdollEnabled, ragdollGravityScale, ragdollAngularDamping

-- Точечный свет (цвет, интенсивность, радиус)
Network.SyncEntityLight(entityId, 1.0, 0.9, 0.7)                     -- r, g, b
Network.SyncEntityLight(entityId, 1.0, 0.9, 0.7, 1.0, 100.0, true, 2.0, false, true)
-- r, g, b, intensity, radius, enabled, falloffExponent, castShadows, reliable

-- Прожектор (цвет, интенсивность, направление, углы)
Network.SyncEntitySpotLight(entityId, 1.0, 1.0, 1.0, 1.0, 100.0, 1.0, 0.0, 30.0, 45.0)
-- r, g, b, intensity, radius, dirX, dirY, innerAngle, outerAngle
Network.SyncEntitySpotLight(entityId, 1.0, 1.0, 1.0, 1.0, 100.0, 1.0, 0.0, 30.0, 45.0, true, 2.0, false, true)
-- + enabled, falloffExponent, castShadows, reliable

-- Тайлмап (отражение, видимость, путь)
Network.SyncEntityTilemap(entityId)                                   -- по умолчанию
Network.SyncEntityTilemap(entityId, false, false, true, true)         -- flipX, flipY, visible, reliable
Network.SyncEntityTilemap(entityId, false, false, true, true, "Content/Tilemaps/Level.ice_tm")
-- flipX, flipY, visible, reliable, path

-- Виджет (UI-компонент, путь)
Network.SyncEntityWidget(entityId)                                    -- по умолчанию
Network.SyncEntityWidget(entityId, true, true, 1.0, 0, true, false, false, true)
-- visible, screenSpace, scale, renderOrder, interactable, flipX, flipY, reliable
Network.SyncEntityWidget(entityId, true, true, 1.0, 0, true, false, false, true, "Content/Widgets/HUD.ice_widget")
-- visible, screenSpace, scale, renderOrder, interactable, flipX, flipY, reliable, path

-- Разрушение (компонент деструкции, расширенный)
Network.SyncEntityDestructible(entityId)                              -- по умолчанию
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

-- ИИ (движение, восприятие, расширенный)
Network.SyncEntityAI(entityId)                                        -- по умолчанию
Network.SyncEntityAI(entityId, 100.0, 200.0, 50.0, false, true)
-- moveSpeed, detectionRadius, attackRadius, perceptionEnabled, reliable
Network.SyncEntityAI(entityId, 100.0, 200.0, 50.0, true, true,
    "Content/AI/EnemyBT.ice_ai", 300.0, 120.0, 500.0, false, 5.0,
    { {x = 100, y = 200}, {x = 300, y = 400} },
    { "Player", "Ally" })
-- moveSpeed, detectionRadius, attackRadius, perceptionEnabled, reliable,
-- aiAssetPath, perceptionSightRadius, perceptionSightAngle,
-- perceptionHearingRadius, perceptionRequireLOS, perceptionForgetTime,
-- patrolPoints (массив {x, y}), targetTags (массив строк)

-- Компонент классов (синхронизация экземпляров классов)
Network.SyncEntityClassComponent(entityId, {
    { name = "WeaponScript", classPath = "Content/Classes/Weapon.ice_class" },
    { name = "HealthScript", classPath = "Content/Classes/Health.ice_class" }
})
-- instances (массив {name, classPath})
Network.SyncEntityClassComponent(entityId, {
    { name = "WeaponScript", classPath = "Content/Classes/Weapon.ice_class" }
}, 0, true)
-- instances, selectedIndex, reliable

-- Параметры аниматора (синхронизация параметров анимации)
Network.SyncEntityAnimatorParams(entityId,
    { isRunning = true, isGrounded = false },   -- bools
    { comboCount = 3 },                          -- ints
    { speed = 1.5 },                             -- floats
    { attack = true })                           -- triggers
-- bools, ints, floats, triggers (все опциональные таблицы ключ-значение)
Network.SyncEntityAnimatorParams(entityId, nil, nil, { speed = 2.0 }, nil, true)
-- bools, ints, floats, triggers, reliable
```

### Интерполяция и компенсация лагов

```lua
-- Интерполяция позиции (включает визуал)
local snap = Network.InterpolateEntity(entityId, renderTime)
-- → {x, y, z, rotation, vx, vy, scaleX, scaleY, flipX, flipY, pivotX, pivotY}
Network.SetInterpolationDelay(0.1)  -- секунды
local delay = Network.GetInterpolationDelay()

-- Компенсация лагов (включает визуал)
Network.EnableLagCompensation(true)
local enabled = Network.IsLagCompensationEnabled()
local pastState = Network.GetEntityAtTime(entityId, timestamp)
-- → {x, y, z, rotation, vx, vy, scaleX, scaleY, flipX, flipY, pivotX, pivotY}
Network.SetMaxHistoryDuration(2.0)
local maxHistory = Network.GetMaxHistoryDuration()  -- текущая макс. длительность истории
```

#### Серверная перемотка (авторитетное определение попаданий)

`EnableLagCompensation(true)` заставляет **сервер** каждый тик записывать историю позиций всех
реплицируемых сущностей. Чтобы проверить выстрел так, как мир видел сам стрелок, перемотайте мир
в его момент времени, посчитайте попадание и верните всё обратно. Перемотка двигает и трансформ,
и тело Box2D, поэтому лучи и запросы перекрытия попадают в исторические позиции.

```lua
Network.EnableLagCompensation(true)
Network.SetLagCompensationWindow(1.0)      -- сколько секунд истории хранить (0.05 .. 10)
local window = Network.GetLagCompensationWindow()   -- → число (секунды)

-- Внутри серверного RPC «игрок X выстрелил»:
if Network.RewindForPlayer(shooterId) then
    local hit = Raycast(originX, originY, dirX, dirY, range)
    Network.RestoreRewind()                 -- ВСЕГДА восстанавливать, даже если не попали
    if hit and hit.hit then ApplyDamage(hit.entityId) end
end
```

`RewindForPlayer` берёт момент времени из пинга игрока плюс задержка интерполяции. Для полного
контроля есть `Network.GetRewindTimeForPlayer(id)` и `Network.RewindToTime(t)`.
`Network.IsRewound()` показывает, перемотан ли мир сейчас — никогда не оставляйте его перемотанным
между кадрами.

#### Реконсиляция с переигрыванием ввода

По умолчанию предсказанная сущность корректируется **сглаживанием** к позиции сервера — это
подходит коопу и экшенам. Для соревновательного движения нужна классическая модель: привязаться к
авторитетному состоянию и заново применить весь ввод, который сервер ещё не подтвердил.

```lua
Network.SetReplayReconciliation(true)
local replayMode = Network.IsReplayReconciliation()   -- → bool (false = сглаживание)
Network.OnReplayInput(function(input, dt)
    ApplyMovementInput(input, dt)          -- та же функция, что и в обычном апдейте
end)
```

Движок ставит сущность в серверное состояние (позиция, поворот и скорость), затем по порядку
вызывает ваш колбэк по одному разу на каждый неподтверждённый кадр ввода с его настоящей дельтой
времени. Отправляйте ввод через `Network.SendInput`, чтобы он попадал в очередь ожидания. Без
зарегистрированного колбэка движок продолжает сглаживать, так что включение полностью
добровольное и безопасное.

```lua
-- Синхронизация времени
local serverTime = Network.GetServerTimeSync()
local clientTime = Network.GetClientTimeSync()
local offset = Network.GetTimeSyncOffset()
```

### Готовность игроков

```lua
Network.SetReady(true)
local ready = Network.IsPlayerReady(playerId)
local allReady = Network.AreAllPlayersReady()
local readyCount = Network.GetReadyCount()
Network.ClearReadyStates()
```

### Ввод по сети

```lua
Network.SendInput("jump")
Network.SendInputTable({ jump = true, moveX = 0.5 })
local lastAck = Network.GetLastAcknowledgedInput()
local pending = Network.GetPendingInputCount()

-- На хосте: приём ввода от клиентов
Network.OnInputReceived(function(playerId, data)
    local input = Network.JsonDecode(data)
    -- применить ввод к сущности игрока playerId
end)
```

### Голосовой чат

```lua
Network.EnableVoiceChat(true)
local enabled = Network.IsVoiceChatEnabled()
Network.SetVoiceMuted(true)   -- глушит только ВАШ микрофон; остальных вы слышите
local muted = Network.IsVoiceMuted()

-- Паттерн push-to-talk: держите микрофон замьюченным и снимайте мьют, пока зажата клавиша.
-- Network.SetVoiceMuted(not IsActionPressed("VoiceTalk"))

-- Управление голосом отдельных игроков
Network.SetPlayerVoiceVolume(playerId, 0.5)
local vol = Network.GetPlayerVoiceVolume(playerId)
Network.SetPlayerVoiceMuted(playerId, true)
local playerMuted = Network.IsPlayerVoiceMuted(playerId)
local talking = Network.IsPlayerTransmitting(playerId)

-- Голосовая близость (пространственный голосовой чат)
Network.SetVoiceProximity(true, 500.0)  -- включить, радиус
local proxEnabled = Network.IsVoiceProximityEnabled()
local proxRange = Network.GetVoiceProximityRange()

-- Лимит ретрансляции голоса
Network.SetMaxVoiceRelayPlayers(16)              -- макс. игроков для голосовой ретрансляции
local maxRelay = Network.GetMaxVoiceRelayPlayers() -- текущий лимит
```

### Администрирование

```lua
Network.KickPlayer(playerId)
Network.BanPlayer(playerId)
Network.UnbanPlayer(playerId)
local banned = Network.IsPlayerBanned(playerId)
Network.SetMaxPlayers(32)   -- ограничивается диапазоном 2..256
Network.SetPassword("secret")
local hasPwd = Network.HasPassword()
local pwd = Network.GetPassword()  -- текущий пароль сервера

-- Бан по имени
Network.BanPlayerByName("Cheater123")
Network.UnbanByName("Cheater123")
local bannedNames = Network.GetBannedNames()  -- таблица забаненных имён

-- Сохранение/загрузка бан-листа
local saved = Network.SaveBanList("bans.json")
local loaded = Network.LoadBanList("bans.json")

-- Белый список
Network.EnableWhitelist(true)
local wlEnabled = Network.IsWhitelistEnabled()
Network.AddToWhitelist("TrustedPlayer")
Network.RemoveFromWhitelist("TrustedPlayer")
local wl = Network.GetWhitelist()  -- таблица разрешённых имён
local isWl = Network.IsWhitelisted("TrustedPlayer")

-- Сохранение/загрузка белого списка
local savedWl = Network.SaveWhitelist("whitelist.json")
local loadedWl = Network.LoadWhitelist("whitelist.json")

-- Мут текстового чата
Network.SetPlayerTextMuted(playerId, true)
local isMuted = Network.IsPlayerTextMuted(playerId)

-- Фильтр нецензурной лексики
Network.EnableProfanityFilter(true)
Network.AddProfanityWord("плохоеслово")
Network.ClearProfanityWords()
local filtered = Network.FilterProfanity("текст с плохоеслово")
```

### Настройки и статистика

```lua
-- Конфигурация
Network.SetTickRate(60)
local tickRate = Network.GetTickRate()
local maxPlayers = Network.GetMaxPlayers()
local port = Network.GetPort()
local ip = Network.GetServerIP()

-- Пинг
local ping = Network.GetPing()
local playerPing = Network.GetPlayerPing(playerId)
local avgPing = Network.GetAveragePing()

-- Трафик
local sent = Network.GetBytesSent()
local recv = Network.GetBytesReceived()
local pSent = Network.GetPlayerBytesSent(playerId)
local pRecv = Network.GetPlayerBytesReceived(playerId)

-- Время работы
local uptime = Network.GetUptime()

-- Время сети
Network.UpdateNetworkTime(dt)
local netTime = Network.GetNetworkTime()
Network.SetServerTimeDelta(0.01)
local delta = Network.GetServerTimeDelta()

-- Ограничения и безопасность
Network.SetRateLimit(100, 65536)         -- макс. пакетов/сек, макс. байт/сек
Network.SetSnapshotRate(20)
local sr = Network.GetSnapshotRate()
Network.SetMaxEntitySpeed(1000)
local maxSpeed = Network.GetMaxEntitySpeed()  -- текущая макс. скорость сущности
Network.SetValidationEnabled(true)
local valEnabled = Network.IsValidationEnabled()

-- Модель доверия: насколько авторитетный сервер доверяет клиентам.
-- "competitive" (по умолчанию) — строгая проверка владения сущностями и анти-чит (скорость);
-- "coop" — доверие клиентам (лёгкая валидация, меньше накладных расходов) для игр с друзьями / кооператива.
Network.SetTrustModel("competitive")     -- или "coop"
local trust = Network.GetTrustModel()    -- "competitive" | "coop"

-- Снимок мира сервера (рассылает полное состояние всем клиентам)
Network.BroadcastWorldSnapshot()  -- только сервер

-- Area of Interest (отсечение по релевантности). Когда включено, сервер отправляет
-- каждому игроку только сущности в радиусе его точки интереса (плюс его собственные),
-- а не весь мир. Именно это делает реальным большое число игроков (например, батл-рояль
-- на 100 человек) по трафику. По умолчанию выключено — без включения поведение прежнее.
Network.SetAreaOfInterest(true, 2000)              -- включить, радиус в мировых единицах
local aoiOn = Network.IsAreaOfInterestEnabled()
local aoiRadius = Network.GetAreaOfInterestRadius()
Network.SetPlayerInterestPosition(playerId, x, y)  -- сервер: каждый тик задавайте точку фокуса игрока (камера/аватар)

-- Реконнект
Network.EnableReconnect(true, 5, 2.0)  -- enable, maxAttempts, interval
local reconnecting = Network.IsReconnecting()
local attempt = Network.GetReconnectAttempt()
local connState = Network.GetConnectionState()

-- Полный сброс
Network.ResetNetworkState()

-- Выделенный сервер
Network.SetDedicatedServer(true)
local isDedicated = Network.IsDedicatedServer()

-- Клиентское предсказание
Network.SetPredictionEnabled(true)
local predEnabled = Network.IsPredictionEnabled()

-- Подстройка сверки предсказания (авто-репликация, только свои сущности). При включённом
-- предсказании сущность, которой вы владеете, продолжает симулироваться локально и каждый
-- тик мягко подтягивается к авторитетному состоянию хоста, а не привязывается к нему резко.
Network.SetPredictionSmoothing(0.2)        -- коэффициент смешивания 0..1 к позиции сервера за тик (по умолчанию 0.2; 0 = чистое предсказание, 1 = резкая привязка каждый тик)
local smoothing = Network.GetPredictionSmoothing()
Network.SetPredictionSnapDistance(500)     -- если предсказание ушло дальше этого (в мировых единицах) от сервера — резкая привязка вместо сглаживания (по умолчанию 500; 0 = не привязывать)
local snapDist = Network.GetPredictionSnapDistance()

-- Дельта-компрессия (на уровне полей: в провод идут только изменившиеся)
Network.SetDeltaCompressionEnabled(true)
local deltaEnabled = Network.IsDeltaCompressionEnabled()

-- Компрессия пакетов (адаптивный range coder на весь пакет, включена по умолчанию)
Network.SetPacketCompressionEnabled(true)
local packetEnabled = Network.IsPacketCompressionEnabled()

-- Время сервера/клиента (прямой доступ)
local srvTime = Network.GetServerTime()
local cliTime = Network.GetClientTime()

-- Шифрование трафика (XChaCha20-Poly1305 AEAD через libsodium): конфиденциальность + целостность.
-- Обе стороны должны вызвать EnableEncryption с ОДНИМ И ТЕМ ЖЕ ключом до подключения. Поддельные
-- или искажённые пакеты не проходят аутентификацию и отбрасываются. Используйте стойкий ключ с высокой энтропией.
Network.EnableEncryption("my_secret_key")
local encEnabled = Network.IsEncryptionEnabled()

-- NAT-обход (подключение через NAT)
Network.EnableNATTraversal(true)                                    -- по умолчанию: stun.l.google.com:19302
Network.EnableNATTraversal(true, "stun.l.google.com", 19302)       -- свой STUN-сервер
local natEnabled = Network.IsNATEnabled()
local externalIP = Network.GetExternalIP()
local externalPort = Network.GetExternalPort()
local discovered = Network.DiscoverExternalAddress()

-- Асинхронное обнаружение внешнего адреса
Network.DiscoverExternalAddressAsync()
local pending = Network.IsDiscoveryPending()
local result = Network.PollDiscoveryResult()  -- возвращает результат или nil, если ещё выполняется
```

> **Это два разных слоя, и они складываются.** Дельта-компрессия решает, *что* попадёт в провод
> (только изменившиеся поля); компрессия пакетов сжимает *сами байты* адаптивным range coder на
> каждом исходящем пакете. Вторая включена по умолчанию и стоит немного процессорного времени на
> пакет — на сервере с 256 игроками это реальный размен, поэтому измеряйте, прежде чем менять:
> `NetworkProfiler.GetTotalBytesSent()` и Network Profiler покажут фактический эффект на вашем трафике,
> который целиком зависит от ваших данных.
>
> **Обе стороны должны совпадать.** Хост со сжатием и клиент без него не смогут разобрать пакеты
> друг друга. Оставляйте значение по умолчанию, если не профилируете, а меняйте — на обеих
> сторонах и до подключения.
>
> **Выключайте её, когда включаете шифрование.** Данные шифруются до передачи в ENet, а шифротекст
> высокоэнтропийный — range coder его не сожмёт, и процессорное время будет потрачено впустую.
> При активном `Network.EnableEncryption(...)` вызывайте
> `Network.SetPacketCompressionEnabled(false)` на обеих сторонах до подключения.

### Обнаружение серверов (LAN и мастер-сервер)

Позвольте игрокам находить игры без ручного ввода IP. Хост **анонсирует** свой сервер, а
клиенты **обнаруживают** список. Работает по **локальной сети** (UDP-бродкаст) и/или через
**мастер-сервер** — лёгкий реестр, который можно поднять где угодно в интернете. Три роли
независимы: сервер может анонсироваться одновременно в LAN и на мастере, клиент может смотреть
оба источника сразу, а любая машина может хостить мастер-реестр.

```lua
-- ХОСТ: начать анонсировать сервер, чтобы его находили. Таблица опций необязательна;
-- каждое поле тоже необязательно (показаны значения по умолчанию).
local ok = Network.AdvertiseServer({
    name        = "My Server",   -- отображаемое имя в браузере серверов
    port        = 7777,          -- игровой порт, к которому клиенты сделают Connect()
    maxPlayers  = 16,
    players     = 0,             -- текущее число игроков
    hasPassword = false,
    gameMode    = "deathmatch",  -- произвольный тег
    lan         = true,          -- бродкаст по локальной сети
    masterIp    = "",            -- также зарегистрироваться на этом мастер-сервере (пусто = только LAN)
    masterPort  = 0,
    lanPort     = 7778,          -- UDP-порт обнаружения (должен совпадать с lanPort клиентов)
})                               -- → true, если анонсирование запущено

-- Обновить анонсируемую информацию на ходу (например, когда игроки заходят/выходят).
-- Передавайте все нужные поля — пропущенные сбрасываются в значения по умолчанию, они НЕ
-- сохраняются из исходного вызова AdvertiseServer.
Network.UpdateAdvertise({ name = "My Server", port = 7777, maxPlayers = 16, players = 5 })

Network.StopAdvertise()
local advertising = Network.IsAdvertising()

-- КЛИЕНТ: сканировать серверы. Те же опции lan / masterIp / masterPort / lanPort, что и у
-- AdvertiseServer (таблица опций и все её поля необязательны).
local ok = Network.DiscoverServers({ lan = true, masterIp = "", masterPort = 0, lanPort = 7778 })  -- → true, если сканирование запущено
local scanning = Network.IsDiscovering()
local count = Network.GetDiscoveredServerCount()
local servers = Network.GetDiscoveredServers()
-- → массив { name, ip, port, players, maxPlayers, hasPassword, gameMode, isLAN }
for _, s in ipairs(servers) do
    print(s.name, s.ip .. ":" .. s.port, s.players .. "/" .. s.maxPlayers, s.isLAN and "LAN" or "Интернет")
end
Network.ClearDiscoveredServers()   -- очистить список сейчас (устаревшие записи также истекают сами)
Network.StopDiscovery()

-- Подключитесь к выбранной записи как к обычному серверу:
-- Network.Connect(servers[1].ip, servers[1].port)

-- МАСТЕР: поднять реестр, на котором регистрируются интернет-серверы и который опрашивают клиенты.
local ok = Network.StartMasterServer(7779)             -- → true, если мастер запущен на этом порту
local isMaster = Network.IsMasterServer()
local registered = Network.GetMasterRegisteredCount()  -- сколько серверов сейчас зарегистрировано
Network.StopMasterServer()
```

> `lanPort` — это UDP-порт LAN-обнаружения через бродкаст; он должен быть одинаковым на хосте
> и всех клиентах и отличается от игрового `port`, передаваемого в `Network.Connect`.
> Обнаруженные серверы исчезают автоматически через несколько секунд после остановки анонса,
> поэтому список в браузере остаётся актуальным без ручной очистки.

### NetworkProfiler — сетевой профилировщик рантайма (только Debug)

`NetworkProfiler` — глобальная Lua-таблица, регистрируемая движком для анализа реального сетевого трафика. Движок инструментирует все пути отправки/приёма (ENet на десктопе, WebSocket на Web) и агрегирует статистику по типам сообщений со сглаженными (EWMA) скоростями и кольцевой историей на 120 секунд.

- **Активен только в Debug-сборках** движка. В Release toggle — no-op, а геттеры возвращают `0` / пустые таблицы. Во время выполнения проверяйте через `NetworkProfiler.IsDebug()`.
- Оверлей рисуется только в отдельном рантайме без редактора. Переключайте его программно через `NetworkProfiler.Toggle()` и привязывайте к любой клавише, кнопке геймпада, консольной команде или UI-виджету через `IsKeyJustPressed`.
- Все функции потокобезопасны и дешёвы — их можно вызывать из скриптов каждый кадр.

#### Управление оверлеем

```lua
local visible = NetworkProfiler.IsVisible()
NetworkProfiler.Toggle()
NetworkProfiler.Reset()
local debugBuild = NetworkProfiler.IsDebug()
```

#### Суммы и скорости

```lua
local bs = NetworkProfiler.GetTotalBytesSent()        -- целое, байт с момента запуска
local br = NetworkProfiler.GetTotalBytesReceived()
local ps = NetworkProfiler.GetTotalPacketsSent()
local pr = NetworkProfiler.GetTotalPacketsReceived()

local kbpsOut = NetworkProfiler.GetKBpsSent()         -- число (КБ/с), EWMA-сглаживание
local kbpsIn  = NetworkProfiler.GetKBpsReceived()
local ppsOut  = NetworkProfiler.GetPPSSent()          -- число (пакетов/с)
local ppsIn   = NetworkProfiler.GetPPSReceived()

local pingMs   = NetworkProfiler.GetPing()            -- целое, мс (последнее измерение)
local players  = NetworkProfiler.GetPlayerCount()     -- целое
```

#### Статистика по типам сообщений

Используйте константы `NetworkProfiler.MSG_*`, которые соответствуют enum `NetMessageType`:

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

#### Кольцевой буфер истории

`GetHistory()` возвращает массив посекундных сэмплов (от старых к новым). Ёмкость буфера фиксирована:

```lua
local cap = NetworkProfiler.GetHistoryCapacitySeconds()   -- 120
local h   = NetworkProfiler.GetHistory()
for i, s in ipairs(h) do
    -- s = { t=<секунд_с_запуска>, bytesSent=..., bytesReceived=...,
    --       packetsSent=..., packetsReceived=..., ping=..., playerCount=... }
end
```

#### Сохранение отчёта

`SaveReport(path?)` записывает JSON-файл с итогами, статистикой по типам и полной историей и возвращает фактический путь к файлу (пустую строку в Release или при ошибке).

```lua
-- Путь по умолчанию (выбирается движком)
local path = NetworkProfiler.SaveReport()

-- Явный путь
local path2 = NetworkProfiler.SaveReport("Logs/netreport.json")
print("Сетевой отчёт сохранён в:", path)
```

#### Полный пример

```lua
-- Переключение оверлея и периодическое сохранение отчёта о трафике
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

## 32.5. Rollback — Детерминированный rollback-неткод (Rollback)

> **Тип:** Глобальный (таблица `Rollback`).
>
> **Rollback-неткод** в стиле GGPO для онлайн-игр с покадровой точностью (файтинги, 1×1/2×2 экшен, lockstep). Работает поверх авторитарного транспорта `Network` (ENet) и детерминированного сервиса `Random`.

### Когда использовать вместо `Network`

`Network.*` — это **авторитарный снапшотный** неткод, правильный инструмент для кооператива, шутеров, MMO-lite и MOBA, где миром владеет сервер, а клиенты интерполируют. `Rollback.*` — другая модель для узкого класса игр, которым нужна **покадровая синхронизация ввода**: каждый участник локально симулирует *всех* игроков, предсказывает чужой ввод и незаметно откатывается + пересимулирует, как только реальный ввод противоречит предсказанию. Для локального игрока задержки ввода нет.

### Жёсткое требование: детерминированная симуляция с фиксированным шагом

Rollback работает только если геймплей **детерминирован**: одно и то же начальное состояние + одинаковый ввод обязаны всегда давать ровно одно и то же следующее состояние на любой машине с той же сборкой. Симуляцию ты задаёшь тремя колбэками:

| Колбэк | Контракт |
|---|---|
| `Rollback.OnSaveState(fn)` | `fn()` возвращает **строку**, полностью описывающую состояние игры текущего кадра. |
| `Rollback.OnLoadState(fn)` | `fn(state, frame)` восстанавливает симуляцию из строки, ранее возвращённой save. |
| `Rollback.OnAdvanceFrame(fn)` | `fn(inputs, frame)` продвигает симуляцию **ровно на один фиксированный шаг** по `inputs`. |

`inputs` — массив с индексами `1..PlayerCount`; каждый элемент `{ bits = <строка>, predicted = <bool> }`.

**Правила детерминизма** (соблюдать все):
- Внутри `OnAdvanceFrame` управляй симуляцией **только** из `inputs` — никогда не читай живой ввод (`IsKeyPressed`, `GetMousePosition`, …) и покадровые глобалы.
- Для случайности используй `RNG.*` или глобальные хелперы `Random*()` — оба берут числа из движкового `RandomService`. Его состояние сохраняется и восстанавливается **автоматически** каждый кадр (управляется конфигом сессии), поэтому броски кубика воспроизводятся идентично сквозь откат. Обычный `math.random` под это **не** попадает.
- Не используй настенное время (`GetTime()`, `GetDeltaTime()`, `os.time()`), неупорядоченный обход (`pairs` по хешу, на порядок которого ты полагаешься) и любые значения, отличающиеся от запуска к запуску.
- Физика Box2D детерминирована для **одного бинаря на одной платформе** — оба участника должны запускать одну сборку. Кроссплатформенный детерминизм float не гарантируется; для рейтинговых матчей поставляй идентичные исполняемые файлы.

### Кадровый цикл

Каждый фиксированный тик (обычно 60 Гц) сэмплируй локальный ввод и вызывай `Rollback.Tick(input)`. Движок:
1. сохраняет твой локальный ввод (с опциональной задержкой ввода) и отправляет его другим участникам,
2. предсказывает ещё не пришедший чужой ввод и продвигает текущий кадр через `OnAdvanceFrame`,
3. когда приходит реальный чужой ввод, отличающийся от предсказания, вызывает `OnLoadState` на ошибочном кадре и заново прогоняет `OnAdvanceFrame` вперёд до настоящего.

`Rollback.Tick(input)` возвращает строку статуса:

| Результат | Значение |
|---|---|
| `"ok"` | Симуляция продвинулась на кадр. Рендери текущее состояние. |
| `"stall"` | **Не** продвигаться в этом тике — окно предсказания заполнено, либо синхронизация времени просит подождать удалённого участника. Перерисуй предыдущий кадр. |
| `"notready"` | Ещё идёт синхронизация с участниками. |
| `"error"` | Нет активной сессии. |

### Справочник API

| Функция | Описание |
|---|---|
| `Rollback.StartSession(numPlayers, inputSize [, frameDelay [, maxRollback]])` | Старт P2P rollback-сессии. Требует активное соединение `Network` со всеми участниками. `inputSize` = байт ввода на игрока за кадр. `frameDelay` (по умолч. 2) меняет немного задержки на меньшее число откатов. `maxRollback` (по умолч. 8) — окно предсказания. Возвращает `bool`. |
| `Rollback.StartSyncTest(inputSize, checkDistance [, numPlayers])` | Старт **офлайн** теста детерминизма: каждый кадр откатывается на `checkDistance` кадров и пересимулирует, кидая событие `desync`, если контрольная сумма изменилась. Используй при разработке для отлова недетерминизма. Возвращает `bool`. |
| `Rollback.Stop()` | Завершить сессию. |
| `Rollback.IsRunning()` | `bool` — сессия активна. |
| `Rollback.IsSynchronized()` | `bool` — все участники сделали handshake и симуляция началась. |
| `Rollback.Tick(input)` | Продвинуть на кадр по локальной строке `input`. Возвращает `"ok"` / `"stall"` / `"notready"` / `"error"`. |
| `Rollback.GetLocalHandle()` | `int` — индекс игрока этого участника (`0..numPlayers-1`). |
| `Rollback.GetPlayerCount()` | `int`. |
| `Rollback.GetCurrentFrame()` | `int` — следующий кадр для симуляции. |
| `Rollback.GetConfirmedFrame()` | `int` — последний кадр, для которого весь ввод подтверждён (без предсказания). |
| `Rollback.RecommendStallFrames()` | `int` — подсказка синхронизации времени; на сколько кадров ты опережаешь удалённого. |
| `Rollback.SetPlayerHandle(handle, netPlayerId, local)` | Вручную сопоставить индекс игрока с сетевым id (иначе назначается по отсортированным id). |
| `Rollback.GetInputs()` | Таблица ввода, использованного для текущего кадра (`{ bits, predicted }`). |
| `Rollback.GetStats()` | Таблица: `frame`, `confirmed_frame`, `predicted_frames`, `rollbacks_per_second`, `max_rollback_frames`, `avg_rollback_frames`, `frame_advantage`, `ping`, `synchronized`. |
| `Rollback.OnSaveState(fn)` / `OnLoadState(fn)` / `OnAdvanceFrame(fn)` / `OnEvent(fn)` | Регистрация колбэков симуляции и событий. |
| `Rollback.ClearCallbacks()` | Удалить все зарегистрированные колбэки. |

`OnEvent(fn)` получает таблицу с `type` (`"synchronizing"`, `"synchronized"`, `"disconnected"`, `"timesync"`, `"desync"`, …), а также `player`, `frame`, `count`, `total`, `frames_ahead`, `local_checksum`, `remote_checksum`.

### Полный пример (1-байтовая битовая маска ввода)

```lua
-- Детерминированное состояние двух бойцов: { x, vx } у каждого. Храним в чистом Lua.
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
    if e.type == "synchronized" then print("Матч начался!") end
    if e.type == "desync" then print("ДЕСИНК на кадре "..e.frame) end
end)

-- После Network.Connect / Network.StartServer и когда оба игрока в сессии:
Rollback.StartSession(2, 1, 2, 8)   -- 2 игрока, 1 байт ввода, задержка 2, окно 8

-- В фиксированном апдейте 60 Гц:
function OnFixedUpdate()
    if not Rollback.IsSynchronized() then return end
    local result = Rollback.Tick(sampleLocalInput())
    if result == "ok" then
        -- перенести sim.p[i].x в видимые сущности и отрендерить
    end
    -- "stall" / "notready": просто продолжаем показывать прошлый кадр
end
```

> **Совет:** Перед выходом в онлайн запусти `Rollback.StartSyncTest(1, 7)` и поиграй локально. Если увидишь событие `desync` — какое-то состояние ускользает из твоего `OnSaveState` (или ты использовал недетерминированные данные); исправь до релиза.

---

## 33. Navigation — Навигация и поиск пути

> **Тип:** Глобальные (таблица `Nav`) + Entity-bound (для AIComponent).
>
> Система навигации A* по сетке (NavGrid). Сетка строится из тайлмапов и коллайдеров сцены.

### Глобальные функции навигации (`Nav.*`)

```lua
-- Построить/перестроить навигационную сетку из текущей сцены
Nav.RebuildGrid()

-- Есть ли сетка?
local has = Nav.HasGrid()

-- Информация о сетке → {width, height, cellSize}
local size = Nav.GetGridSize()
local cellSize = Nav.GetCellSize()
local diagonal = Nav.GetAllowDiagonal()
local origin = Nav.GetOrigin()  -- → {x, y}

-- Проверить проходимость точки (ИИ может стоять/двигаться здесь)
local walkable = Nav.IsWalkable(worldX, worldY)

-- Проверить, находится ли точка внутри реального препятствия/стены
-- (сырые данные коллизий: без расширения AgentRadius и без side-view
-- фильтра «нет пола»). Используй для LOS, raycast'ов и видимости — не для пути.
local solid = Nav.IsSolid(worldX, worldY)

-- Ray-march проверка линии видимости между двумя мировыми точками.
-- Возвращает true, если ни одна Solid-клетка не пересекает отрезок.
local clear = Nav.LineOfSight(ax, ay, bx, by)

-- Найти путь (возвращает таблицу точек {x, y})
local path = Nav.FindPath(startX, startY, endX, endY)
local path = Nav.FindPath(startX, startY, endX, endY, true)  -- diagonal

-- Конвертация координат
local grid = Nav.WorldToGrid(worldX, worldY)   -- → {x, y}
local world = Nav.GridToWorld(gridX, gridY)     -- → {x, y}

-- Расстояние между двумя точками
local dist = Nav.GetDistance(x1, y1, x2, y2)

-- Режим навигации сетки под мировой точкой.
-- Возвращает 0 = Top-Down, 1 = Side-View, -1 = сетки там нет.
local mode = Nav.GetMode(worldX, worldY)
```

> **`IsWalkable` vs `IsSolid`.** `IsWalkable` отвечает на вопрос *«может ли путь пройти через эту клетку?»*
> и использует пост-обработанную сетку (расширение AgentRadius, side-view фильтр «стоять на полу»).
> `IsSolid` отвечает на вопрос *«есть ли тут физическая стена?»* и работает по сырой obstacle-сетке.
> В side-view это критично: пустой воздух между двумя платформами **не** walkable, но **не** solid —
> поэтому зрение и слух корректно проходят сквозь воздушные гэпы.

### Entity-bound навигация (требует AIComponent)

```lua
-- Скорость ИИ
SetAIMoveSpeed(200)
local speed = GetAIMoveSpeed()

-- Радиусы обнаружения и атаки
SetAIDetectionRadius(300)
local dr = GetAIDetectionRadius()
SetAIAttackRadius(50)
local ar = GetAIAttackRadius()

-- Режим движения: 0 = Auto, 1 = Transform (кинематика), 2 = Physics (через скорость)
SetAIMovementMode(2)
local mode = GetAIMovementMode()

-- Радиус прибытия для физики (дистанция принятия при движении в режиме Physics/Auto-physics)
SetAIPhysicsArrivalRadius(6.0)
local arrivalR = GetAIPhysicsArrivalRadius()

-- Навигация к точке (A*)
local found = NavigateTo(targetX, targetY)
local found = NavigateTo(targetX, targetY, false)  -- без диагонали

-- Следовать по пути (вызывать каждый кадр). true = путь завершён.
local done = FollowPath(dt)

-- Навигация + следование в одном вызове
-- Возвращает: 0 = в процессе, 1 = достиг цели, -1 = путь не найден
local status = NavigateAndFollow(targetX, targetY, dt)
local status = NavigateAndFollow(targetX, targetY, dt, true)  -- diagonal

-- Двигаться напрямую к точке (без A*). true — когда в радиусе прибытия.
-- Необязательные: скорость (по умолчанию MoveSpeed из AIComponent) и радиус прибытия.
local arrived = MoveTowardPoint(targetX, targetY, dt)
local arrived = MoveTowardPoint(targetX, targetY, dt, 120, 6)

-- Помощник для side-scroller: двигаться только по X к targetX (Y отдаётся гравитации).
local arrived = MoveTowardX(targetX, dt)
local arrived = MoveTowardX(targetX, dt, 120, 4)

-- Остановить движение (обнуляет физическую скорость; в side-view сохраняет гравитацию).
StopMovement()

-- Задать направление взгляда ИИ для восприятия с учётом направления (см. ниже).
SetAIFacing(1)    -- смотрит вправо (+X)
SetAIFacing(-1)   -- смотрит влево (-X)

-- Информация о пути
local path = GetCurrentPath()            -- таблица точек {x, y}
local next = GetNextPathPoint()          -- → {x, y} или nil
local dir = GetPathDirection()           -- → {x, y} нормализованный вектор
local len = GetPathLength()              -- оставшаяся длина пути
local remaining = GetRemainingPathPoints()
local has = HasPath()                    -- есть активный путь?
local done = IsPathComplete()            -- путь завершён?

-- Управление путём
AdvancePath()   -- Перейти к следующей точке
ClearPath()     -- Очистить путь

-- Дополнительные утилиты
local dist = GetDistanceTo(targetX, targetY)
local dist = GetDistanceToEntity(otherId)

-- Патрульные точки
AddPatrolPoint(100, 200)
local patrol = GetNextPatrolPoint()      -- → {x, y} или nil
AdvancePatrol()
local count = GetPatrolPointCount()
local idx = GetPatrolIndex()
SetPatrolIndex(0)
ClearPatrolPoints()

-- Разведение агентов (separation)
ApplySeparation(32.0, 1.0)
```

> **Движение с учётом физики.** Все функции движения выше (`FollowPath`,
> `NavigateAndFollow`, `MoveTowardPoint`, `MoveTowardX`, `StopMovement`, `ApplySeparation`)
> учитывают **Режим движения** (Movement Mode) из AIComponent:
> - **Auto** (по умолчанию) — если у сущности есть динамическое/кинематическое Rigidbody-тело,
>   движение идёт через физическое тело (скорость), иначе двигается Transform напрямую.
> - **Transform** — всегда двигать Transform напрямую (старое поведение; не для физических тел).
> - **Physics** — всегда управлять скоростью Rigidbody.
>
> Когда сущность стоит на **Side-View** навигационной сетке, физическое движение задаёт только
> горизонтальную (X) скорость, а Y отдаётся гравитации — ровно то, что нужно платформенному
> персонажу. На **Top-Down** сетке задаётся полная 2D-скорость. Благодаря этому встроенная задача
> `MoveTo` в behavior tree и функции следования по пути пригодны для физических врагов и не
> конфликтуют с Box2D.

### Расстояния

```lua
-- Расстояние от текущей сущности до точки
local dist = GetDistanceTo(worldX, worldY)

-- Расстояние до другой сущности
local dist = GetDistanceToEntity(entityId)
```

### Патрулирование

```lua
-- Добавить точку патрулирования
AddPatrolPoint(100, 200)
AddPatrolPoint(300, 200)
AddPatrolPoint(300, 400)

-- Следующая точка патруля
local point = GetNextPatrolPoint()  -- → {x, y} или nil

-- Перейти к следующей (цикл)
AdvancePatrol()

-- Информация
local count = GetPatrolPointCount()
local idx = GetPatrolIndex()
SetPatrolIndex(0)  -- Сбросить

-- Очистить
ClearPatrolPoints()
```

### Разделение (Separation)

```lua
-- Раздвинуть сущности AI, чтобы не толпились
ApplySeparation()               -- radius=32, strength=1
ApplySeparation(50, 1.5)        -- radius, strength
```

### Пример: Преследующий враг

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

## 33.5. Fog of War — Обзор и видимость

> **Тип:** Глобальный модуль `FogOfWar.*`

Сеточная система **поля зрения / тумана войны** на основе симметричного рекурсивного
shadowcasting'а — классическая модель обзора рогаликов (top-down
данжен-краулеры). Хранит три состояния на клетку — **не видено** (unseen), **исследовано**
(explored — видели раньше, запомнено, затемнено) и **видно** (visible — сейчас в поле зрения) —
и обслуживает как геймплейные запросы (стелс, линия видимости, «видит ли монстр игрока?»), так и
анимированный оверлей тумана.

Интегрируется с [NavGrid](#33-navigation--навигация-и-поиск-пути): стены, блокирующие обзор,
можно взять прямо из навигационных «solid»-клеток через `SetOpacityFromNavGrid()`.

> Движок вызывает `FogOfWar.Reset()` автоматически при каждой загрузке уровня. Покадровую
> видимость вы задаёте из скрипта (см. цикл ниже).

#### Константы состояния

| Константа | Значение | Смысл |
|---|---|---|
| `FogOfWar.UNSEEN`   | 0 | никогда не видели |
| `FogOfWar.EXPLORED` | 1 | видели раньше, сейчас не в обзоре |
| `FogOfWar.VISIBLE`  | 2 | сейчас в обзоре |

#### Настройка

```lua
-- width, height = размер сетки в клетках; cellSize = мировых единиц на клетку;
-- originX, originY = мировая позиция угла клетки (0,0) (необязательно, по умолчанию 0,0)
FogOfWar.Configure(64, 64, 32.0, 0.0, 0.0)
FogOfWar.SetEnabled(true)        -- включить оверлей тумана
FogOfWar.IsEnabled()             -- bool — включена ли сейчас система тумана?

-- Отметить, какие клетки блокируют обзор (стены):
FogOfWar.SetOpacity(x, y, true)               -- одна клетка
FogOfWar.GetOpacity(x, y)                     -- bool — блокирует ли эта клетка обзор?
FogOfWar.SetOpacityRect(x, y, w, h, true)     -- прямоугольник клеток
FogOfWar.FillOpacity(false)                   -- сбросить всю непрозрачность
FogOfWar.SetOpacityFromNavGrid()              -- взять стены из NavGrid автоматически

FogOfWar.IsConfigured()                       -- bool
FogOfWar.GetWidth(); FogOfWar.GetHeight(); FogOfWar.GetCellSize()
```

#### Покадровый цикл видимости

```lua
function OnUpdate(dt)
    FogOfWar.BeginFrame()                       -- очистить «видно сейчас» на этот кадр
    local p = GetPlayerWorldPos()
    FogOfWar.ComputeFOV(p.x, p.y, 256.0)        -- открыть вокруг игрока (радиус в мировых единицах)

    -- Несколько наблюдателей (соратники, факелы) — каждый вызов ДОБАВЛЯЕТ к видимому множеству:
    FogOfWar.AddViewer(torchX, torchY, 160.0)   -- AddViewer — псевдоним ComputeFOV
end
```

#### Запросы (геймплей)

```lua
-- По мировой позиции
FogOfWar.IsVisible(worldX, worldY)    -- сейчас в обзоре?
FogOfWar.IsExplored(worldX, worldY)   -- видели когда-либо?

-- По клетке
FogOfWar.IsCellVisible(cx, cy)
FogOfWar.IsCellExplored(cx, cy)
FogOfWar.GetState(cx, cy)             -- FogOfWar.UNSEEN / EXPLORED / VISIBLE

-- Преобразования
local c = FogOfWar.WorldToCell(worldX, worldY)   -- {x, y} индекс клетки
local w = FogOfWar.CellToWorld(cx, cy)           -- {x, y} мировой центр клетки
```

> Запросы FOV работают даже при выключенном рендере — можно использовать туман чисто как оракул
> линии видимости для стелса / ИИ (просто не вызывайте `SetEnabled(true)` или вызовите
> `SetRenderEnabled(false)`).

#### Внешний вид

```lua
FogOfWar.SetFogColor(0, 0, 0)            -- RGB тумана (по умолчанию чёрный)
FogOfWar.SetAlphas(1.0, 0.55, 0.0)       -- непрозрачность для не видено / исследовано / видно
FogOfWar.SetFadeSpeed(12.0)              -- как быстро клетки перетекают между состояниями
FogOfWar.SetSmoothFade(true)             -- false = мгновенно, true = плавное затухание
FogOfWar.SetRevealExplored(true)         -- false = исследованные, но не видимые клетки снова
                                         --         полностью темнеют (без памяти карты — слепые забеги)
FogOfWar.SetRenderEnabled(true)          -- рисовать оверлей (FOV считается и при false)
FogOfWar.IsRenderEnabled()               -- bool — рисуется ли сейчас оверлей?
FogOfWar.SetRenderZ(5000.0)              -- порядок отрисовки (над миром, под UI)
```

#### Управление картой и сохранение

```lua
FogOfWar.Reset()          -- очистить видно + исследовано + затухание (свежий уровень)
FogOfWar.ClearExplored()  -- забыть исследованную карту, конфигурацию сохранить
FogOfWar.RevealAll()      -- пометить всё исследованным + видимым (дебаг / предмет «вся карта»)
FogOfWar.HideAll()        -- снова всё скрыть

local blob = FogOfWar.SaveState()   -- сериализовать исследованную карту (компактно, упаковка битами)
FogOfWar.LoadState(blob)            -- восстановить при загрузке (заново применяет размер / клетку / origin)
```

---

## 34. AI — Искусственный интеллект

> **Тип:** Entity-bound (Blackboard, BT) + Глобальные (Perception, EQS).
> Требует компонент **AIComponent**.
>
> Система ИИ включает: Blackboard (доска данных), Behavior Tree (дерево поведений),
> AI Perception (система восприятия), EQS (Environment Query System — запросы к окружению).

### Blackboard (Доска данных)

Blackboard — хранилище данных ИИ (как память врага).

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
local pos = GetBlackboardVec2("lastKnownPos")  -- → {x, y} или nil

-- Entity ID
SetBlackboardEntity("target", playerId)
local target = GetBlackboardEntity("target")

-- Управление
local has = HasBlackboardKey("playerSeen")
ClearBlackboardKey("playerSeen")
ClearBlackboard()  -- Очистить всё
```

### Perception (Система восприятия)

```lua
-- Включить/выключить
SetPerceptionEnabled(true)
local enabled = IsPerceptionEnabled()

-- Настройка зрения
SetSightConfig(400, 120)        -- radius, angle (в градусах)
SetSightConfig(400, 120, true)  -- + проверка line-of-sight

-- Радиус осведомлённости: круговое «шестое чувство» на 360° вокруг наблюдателя.
-- Цели внутри него воспринимаются даже вне конуса зрения. Всё равно ограничен
-- радиусом зрения выше; 0 (по умолчанию) отключает кольцо.
SetPerceptionAwarenessRadius(80)

-- Источник вектора forward: true = брать его из направления AI, заданного
-- SetAIFacing(fx) (чистый ±X), false (по умолчанию) = поворот сущности + флип спрайта.
SetPerceptionUseFacingX(true)

-- Настройка слуха
SetHearingRadius(500)

-- Время забывания
SetForgetTime(5.0)  -- Через 5 сек забывает цель

-- Какие теги воспринимать
SetPerceptionTargetTags({"Player", "Ally"})

-- Проверить видимость конкретной сущности
if CanSeeEntity(playerId) then ... end

-- Получить всех воспринятых актёров
local actors = GetPerceivedActors()
for _, actor in ipairs(actors) do
    -- actor.entityId   = ID сущности
    -- actor.x, actor.y = позиция
    -- actor.seen       = видим прямо сейчас?
    -- actor.timeSinceLastSeen = время с последнего обнаружения
    -- actor.strength   = сила сигнала
    -- actor.sense      = "sight" / "hearing" / "damage" / "custom"
end

-- Получить главную цель (с наивысшим приоритетом)
local target = GetHighestPriorityTarget()
if target then
    Print("Цель: " .. target.entityId .. " на " .. target.x .. ", " .. target.y)
end
```

> **Зрение с учётом направления (поворот + флип).** По умолчанию конус зрения следует за поворотом
> (rotation) сущности **и** за флипом спрайта (FlipX зеркалит конус) — поэтому враг, разворачивающийся
> флипом, автоматически смотрит в нужную сторону без единой строчки кода. Если нужно направление,
> отвязанное от спрайта (сущность без спрайта или ИИ, который «смотрит» не туда, куда нарисован),
> включите **Use Facing X** у AIComponent и задавайте его через `SetAIFacing(1)` / `SetAIFacing(-1)`.
> Задайте **Awareness Radius** (> 0), чтобы замечать цели в любом направлении в этом радиусе (с учётом
> линии видимости) — например, врага за спиной. Всё это одинаково действует в обновлении восприятия,
> в `CanSeeEntity` **и в отладочной отрисовке восприятия во вьюпорте**.

### Глобальные стимулы (`Perception.*`)

```lua
-- Сообщить о шуме в точке
Perception.ReportNoise(x, y, 1.0)        -- loudness
Perception.ReportNoise(x, y, 1.0, 3.0)   -- + maxAge (сек)
Perception.ReportNoise(x, y, 1.0, 3.0, "Player")  -- + тег источника

-- Сообщить произвольный стимул
Perception.ReportStimulusAt("sight", x, y)
Perception.ReportStimulusAt("hearing", x, y, 0.8, 5.0, "Enemy")

-- Стимул от конкретной сущности
Perception.ReportStimulusFrom("damage", entityId)
Perception.ReportStimulusFrom("sight", entityId, 1.0, 5.0, "Player")
```

### EQS — Environment Query System (`EQS.*`)

EQS — система запросов к окружению. Генерирует точки вокруг ИИ и оценивает их по критериям.

```lua
-- Зарегистрировать запрос
EQS.RegisterQuery("FindCover", {
    maxResults = 3,
    resultKey = "CoverPos",
    generator = {
        type = "grid",          -- "grid", "navigableGrid", "circle", "donut", "aroundEntity"
        halfExtent = 300,       -- половина размера сетки
        spacing = 50,           -- шаг между точками
        radius = 200,           -- для circle
        points = 16,            -- количество точек для circle
        innerRadius = 50,       -- для donut
        outerRadius = 200,      -- для donut
        donutPoints = 16,       -- количество точек для donut
        entityKey = "Target"   -- ключ Blackboard для aroundEntity
    },
    tests = {
        {
            type = "distance",         -- "distance", "dot", "pathExists", "isWalkable", "visibility", "distanceToEntity", "custom"
            scoring = "inverseLinear", -- "linear", "inverseLinear", "constant", "curve"
            weight = 1.0,
            filter = false,            -- использовать как фильтр?
            filterType = "range",      -- "min", "max", "range"
            filterMin = 100,
            filterMax = 500,
            referenceKey = "Target",
            luaFunction = "MyCustomTest"
        },
        {
            type = "isWalkable",
            filter = true              -- Отсеять непроходимые точки
        }
    }
})

-- Удалить запрос
EQS.RemoveQuery("FindCover")

-- Выполнить запрос (Entity-bound)
local results = RunEQS("FindCover")
-- Можно переопределить ключ записи лучшего результата в Blackboard
local results = RunEQS("FindCover", "CoverPosOverride")
-- → таблица {x, y, score} отсортированная по убыванию score
-- Лучший результат автоматически записывается в Blackboard по ключу resultKey

for _, pt in ipairs(results) do
    Print("Точка: " .. pt.x .. ", " .. pt.y .. " (score=" .. pt.score .. ")")
end
```

### Behavior Tree (Дерево поведений)

```lua
-- Проверить, активно ли дерево поведений
local active = IsBehaviorTreeActive()

-- Включить/выключить
SetBehaviorTreeActive(true)
SetBehaviorTreeActive(false)

-- Сбросить дерево (перезапуск)
ResetBehaviorTree()
```

> Деревья поведений создаются в визуальном редакторе `.ice_ai` и выполняются автоматически.
> Lua-условия и действия в нодах дерева имеют доступ к Blackboard через функции выше.

---

## 35. Joint — Физические соединения (Box2D)

> **Тип:** Entity-bound. Требует компонент **JointComponent** (чтение/настройка) или **RigidbodyComponent** (создание/удаление).
> Joints (соединения) связывают два физических тела. Типы: Revolute (шарнир),
> Distance (расстояние), Prismatic (призматический), Wheel (колесо), Weld (сварка), Motor (мотор).

### Основные свойства

```lua
-- Количество соединений
local count = GetJointCount()

-- Тип соединения (int). -1 = нет
local type = GetJointType()
local type = GetJointType(1)  -- Второе соединение

-- Имя
local name = GetJointName()

-- Позиция
SetJointPosition(10, 5)
local pos = GetJointPosition()  -- → {x, y}

-- Локальный масштаб
SetJointLocalScale(1.5, 1.5)
local scale = GetJointLocalScale()  -- → {x, y}

-- Локальный поворот (градусы)
SetJointLocalRotation(45)
local rot = GetJointLocalRotation()  -- → float

-- Мировая трансформация (трансформ сущности уже учтён — см. раздел Sprite).
-- Учтите: локальная позиция называется SetJointPosition/GetJointPosition.
SetJointWorldPosition(120, 64, 0)
local jwp = GetJointWorldPosition(0)  -- → {x, y, z}
SetJointWorldRotation(30, 0)
local jwr = GetJointWorldRotation(0)  -- → число
local jws = GetJointWorldScale(0)     -- → {x, y}, только чтение
```

### Мотор

```lua
-- Включить/выключить мотор
SetJointMotorEnabled(true)
local enabled = IsJointMotorEnabled()

-- Скорость мотора
SetJointMotorSpeed(5.0)
local speed = GetJointMotorSpeed()

-- Максимальный крутящий момент (Revolute)
SetJointMaxMotorTorque(100)
local torque = GetJointMaxMotorTorque()

-- Максимальная сила мотора (Prismatic)
SetJointMaxMotorForce(200)
local force = GetJointMaxMotorForce()
```

### Ограничения (Limits)

```lua
-- Включить/выключить лимиты
SetJointLimitsEnabled(true)
local enabled = AreJointLimitsEnabled()

-- Установить/получить пределы
SetJointLimits(-45, 45)            -- lower, upper (градусы для Revolute)
local limits = GetJointLimits()    -- → {lower, upper}
```

### Пружина (Spring)

```lua
-- Включить/выключить пружину
SetJointSpringEnabled(true)
local enabled = IsJointSpringEnabled()

-- Частота (Hz)
SetJointSpringHertz(4.0)
local hz = GetJointSpringHertz()

-- Демпфирование
SetJointSpringDamping(0.7)
local damp = GetJointSpringDamping()
```

### Якоря (Anchors)

```lua
-- Якоря — точки привязки соединения на каждом из двух тел
SetJointAnchorA(10, 5)
SetJointAnchorB(-10, 0)
local a = GetJointAnchorA()  -- → {x, y}
local b = GetJointAnchorB()  -- → {x, y}
```

### Прочее

```lua
-- Целевая сущность
local targetUUID = GetJointTargetEntity()
local targetTag = GetJointTargetTag()

-- Прочность (для разрушаемых соединений)
local breakForce = GetJointBreakForce()
local breakTorque = GetJointBreakTorque()
SetJointBreakForce(500)
SetJointBreakTorque(200)

-- Все функции принимают необязательный index для нескольких Joint'ов:
SetJointMotorSpeed(5.0, 1)  -- Второе соединение
```

### Параметры Motor Joint

> Функции управления свойствами соединения типа **Motor**.
> Motor-соединения плавно перемещают одно тело к целевой позиции/углу.

```lua
-- Линейное смещение (целевая позиция, пиксели)
SetMotorJointLinearOffset(10, 5)
local offset = GetMotorJointLinearOffset()  -- → {x, y}

-- Угловое смещение (целевой угол, градусы)
SetMotorJointAngularOffset(45)
local angle = GetMotorJointAngularOffset()

-- Максимальная сила (пиксели)
SetMotorJointMaxForce(100)
local force = GetMotorJointMaxForce()

-- Максимальный крутящий момент
SetMotorJointMaxTorque(50)
local torque = GetMotorJointMaxTorque()

-- Коэффициент коррекции (0..1, скорость сходимости)
SetMotorJointCorrectionFactor(0.3)
local factor = GetMotorJointCorrectionFactor()

-- Все функции принимают необязательный индекс соединения:
SetMotorJointLinearOffset(10, 5, 1)  -- Второе соединение
```

### Создание соединений в рантайме (по тегу)

> Эти функции привязаны к **текущей сущности** (требуют RigidbodyComponent).
> Целевая сущность ищется по **Tag**. Если несколько сущностей с одинаковым тегом — берётся первая найденная и пишется предупреждение в лог; используйте уникальные теги или варианты `*ToEntity`.
> Успешно созданный джоинт запоминает UUID цели, поэтому переживает переименование тега.
> Все возвращают индекс созданного джоинта (`int`) или `-1` при ошибке (причина пишется в лог).

```lua
-- Revolute (шарнир) — вращение вокруг точки
local idx = CreateRevoluteJoint("TargetTag", anchorAx, anchorAy, anchorBx, anchorBy)

-- Distance (трос/стержень) — поддерживает расстояние между двумя якорями
local idx = CreateDistanceJoint("TargetTag", anchorAx, anchorAy, anchorBx, anchorBy)

-- Weld (сварка) — жёстко склеивает два тела
local idx = CreateWeldJoint("TargetTag", anchorAx, anchorAy, anchorBx, anchorBy)

-- Prismatic (слайдер) — движение по оси (лифты, двери)
local idx = CreatePrismaticJoint("TargetTag", axisX, axisY, anchorAx, anchorAy, anchorBx, anchorBy)

-- Wheel (подвеска) — колесо с пружиной по оси (транспорт)
local idx = CreateWheelJoint("TargetTag", axisX, axisY, anchorAx, anchorAy, anchorBx, anchorBy)

-- Motor — плавно притягивает одно тело к другому
local idx = CreateMotorJoint("TargetTag", maxForce, maxTorque, correctionFactor)
```

### Создание соединений в рантайме (по entity ID)

> То же самое, но цель указывается через **entity ID** вместо тега.
> Это необходимо когда несколько сущностей имеют одинаковый тег (например, части ragdoll, заспавненные детали).

```lua
-- Revolute (шарнир)
local idx = CreateRevoluteJointToEntity(entityId, anchorAx, anchorAy, anchorBx, anchorBy)

-- Distance (трос/стержень)
local idx = CreateDistanceJointToEntity(entityId, anchorAx, anchorAy, anchorBx, anchorBy)

-- Weld (сварка)
local idx = CreateWeldJointToEntity(entityId, anchorAx, anchorAy, anchorBx, anchorBy)

-- Prismatic (слайдер) — движение по оси (лифты, двери)
-- axisX, axisY = направление допустимого движения (нормализуется автоматически)
local idx = CreatePrismaticJointToEntity(entityId, axisX, axisY, anchorAx, anchorAy, anchorBx, anchorBy)

-- Wheel (подвеска) — колесо с пружиной по оси (транспорт)
local idx = CreateWheelJointToEntity(entityId, axisX, axisY, anchorAx, anchorAy, anchorBx, anchorBy)

-- Motor — плавно притягивает одно тело к другому
local idx = CreateMotorJointToEntity(entityId, maxForce, maxTorque, correctionFactor)
```

**Якоря** задаются в пикселях, относительно центра каждого тела. Все необязательные (по умолчанию `0, 0`).

### Пример создания соединений в рантайме

```lua
function OnBeginPlay()
    -- Спавним части тела
    local torso = SpawnEntity("Classes/Torso.json", 100, 100)
    local head  = SpawnEntity("Classes/Head.json",  100, 80)
    local armL  = SpawnEntity("Classes/Arm.json",   85, 100)
    local armR  = SpawnEntity("Classes/Arm.json",  115, 100)

    -- Соединяем шарнирами (по entity ID — безопасно при одинаковых тегах)
    local neck = CreateRevoluteJointToEntity(head, 0, 10, 0, -8)
    EnableJointLimit(neck, true)
    SetJointLimits(-30, 30, neck)

    local shoulderL = CreateRevoluteJointToEntity(armL, -12, -5, 0, -10)
    EnableJointLimit(shoulderL, true)
    SetJointLimits(-90, 90, shoulderL)
end
```

### Удаление и запросы соединений в рантайме

> `DestroyJoint`/`DestroyAllJoints` разрывают физическую связь, но сохраняют слот инстанса —
> индексы джоинтов никогда не сдвигаются. Разорванный джоинт можно вернуть через `RecreateJoint`.
> `RemoveJointInstance` удаляет слот полностью (индексы последующих джоинтов сдвигаются на один).

```lua
-- Разорвать конкретное соединение по индексу (слот остаётся, индексы стабильны)
DestroyJoint(0)

-- Разорвать все соединения на этой сущности
DestroyAllJoints()

-- Пересоздать разорванный джоинт из сохранённых настроек → true при успехе
local ok = RecreateJoint(0)

-- Удалить слот джоинта полностью (сдвигает последующие индексы)
RemoveJointInstance(0)

-- Проверить валидность соединения (не разорвано ли)
local valid = IsJointValid(0)

-- Получить силу связи на соединении → {x, y} (пиксели)
local force = GetJointReactionForce(0)

-- Получить крутящий момент связи на соединении
local torque = GetJointReactionTorque(0)
```

### Управление параметрами соединений в рантайме (по индексу)

> Set-функции принимают **сначала значение**, а необязательный индекс джоинта — **последним** (по умолчанию 0).
> Enable-функции принимают **индекс первым**. Все сеттеры автоматически будят связанные тела.

```lua
-- Мотор
EnableJointMotor(0, true)            -- (индексДжоинта, включён)
SetJointMotorSpeed(90.0, 0)          -- (скорость [, индексДжоинта]) — град/сек (Revolute, Wheel) или пикс/сек (Prismatic, Distance)
SetJointMaxMotorTorque(50, 0)        -- (момент [, индексДжоинта]) — Revolute, Wheel
SetJointMaxMotorForce(200, 0)        -- (сила [, индексДжоинта]) — Prismatic, Distance

-- Ограничения
EnableJointLimit(0, true)            -- (индексДжоинта, включён)
SetJointLimits(-45, 45, 0)           -- (нижний, верхний [, индексДжоинта]) — градусы (Revolute) или пиксели (Prismatic, Wheel, Distance)

-- Пружина
EnableJointSpring(0, true)           -- (индексДжоинта, включён)
SetJointSpringHertz(4.0, 0)          -- (герцы [, индексДжоинта])
SetJointSpringDamping(0.7, 0)        -- (затухание [, индексДжоинта])
SetJointSpringDampingRatio(0, 0.7)   -- (индексДжоинта, затухание) — legacy-алиас SetJointSpringDamping
```

### Событие разрыва соединения

> Когда джоинт с `BreakForce`/`BreakTorque` превышает порог, движок уничтожает runtime-джоинт,
> оставляет инстанс в списке (индексы джоинтов никогда не сдвигаются) и вызывает
> `OnJointBreak` в скрипте сущности-владельца. `IsJointValid(index)` для разорванного джоинта вернёт `false`.

```lua
function OnJointBreak(jointIndex, jointName, targetTag)
    PlaySound("snap.wav")
    Print("Джоинт " .. jointName .. " (#" .. jointIndex .. " -> " .. targetTag .. ") разорван!")
end
```

### Физические части — мультитело в одном классе (Отдельное тело)

> Коллайдер-инстанс с включённым флагом **Отдельное тело** симулируется как **собственное
> динамическое тело** («часть»), а не как шейп на теле сущности. Это позволяет собрать целую
> физическую машину в одном классе: кузов + колёса, рычаг катапульты и т.д.
>
> * Часть наследует GravityScale/Damping/Bullet/Sleep от Rigidbody сущности.
> * Джоинт-инстанс с заданной **Целевой частью** соединяет тело сущности с этой частью
>   (Целевая часть имеет приоритет над тегом цели).
> * Спрайт- или флипбук-инстанс с заданным **Привязать к коллайдеру** следует за позицией и поворотом части.
> * Если у привязанного спрайта/флипбука есть полигон коллизии — его полигон-шейпы создаются
>   **на теле части** и движутся вместе с ней (для точного совпадения не поворачивайте привязанный
>   инстанс относительно части).
> * События contact/sensor/hit от шейпов части приходят сущности-владельцу с именем коллайдера.
> * Группы коллизий и `CollideConnected` работают для частей так же, как для обычных коллайдеров.
> * Флип сущности (`SetEntityFlipX`) зеркалит всё физически: тела частей, их скорости, якоря и оси
>   джоинтов, угловые лимиты, референс-углы и авторские скорости моторов — катапульта идеально
>   флипается влево/вправо. Заметь: скорость мотора, заданная в рантайме через `SetJointMotorSpeed`,
>   остаётся в мировой конвенции (положительная = по часовой); для семантики «вперёд» умножай на знак фейсинга сам.
> * Телепорты (`SetPosition`, `SetEntityPosition`, сетевые снапы) переносят части вместе с телом.

Машина в одном классе — компоненты:

```text
Rigidbody (Dynamic)                          — тело кузова
Collider: Box "Body"                         — шейп кузова
Collider: Sphere "WheelL"  [Отдельное тело]  — левое колесо (круг, в (-45, -20), Friction ~0.9)
Collider: Sphere "WheelR"  [Отдельное тело]  — правое колесо (круг, в (45, -20), Friction ~0.9)
Sprite "Chassis"                             — спрайт кузова
Sprite "SpriteL" [Привязать к коллайдеру: WheelL] — спрайт колеса, следует за частью
Sprite "SpriteR" [Привязать к коллайдеру: WheelR] — спрайт колеса, следует за частью
Joint 0: Wheel, Целевая часть = WheelL, AnchorA = (-45, -20), Ось = (0, 1),
         Пружина (Hertz 4, Damping 0.7), Мотор ВКЛ, MaxMotorTorque 100
Joint 1: Wheel, Целевая часть = WheelR, AnchorA = (45, -20), Ось = (0, 1),
         Пружина (Hertz 4, Damping 0.7), Мотор ВКЛ, MaxMotorTorque 100
```

Скрипт управления машиной (A/D):

```lua
local SPEED = 720

function OnUpdate(dt)
    local axis = GetAxis("A", "D")
    SetJointMotorSpeed(axis * SPEED, 0)
    SetJointMotorSpeed(axis * SPEED, 1)
end
```

> `AnchorA` — точка крепления колеса в пикселях относительно центра кузова; `AnchorB` по умолчанию
> `(0, 0)` — центр части. Положительная скорость мотора крутит по часовой стрелке и везёт машину вправо.
> Колёса не сталкиваются с кузовом, пока выключен `Collide Connected` (по умолчанию).

---

## 36. PointMarker — Точечные маркеры

> **Тип:** Entity-bound. Требует компонент **PointMarkerComponent**.
> Маркеры — вспомогательные точки на сущности (точки спавна, позиции оружия и т.д.).

```lua
-- Количество маркеров
local count = GetPointMarkerCount()

-- Имя
local name = GetPointMarkerName()
local name = GetPointMarkerName(1)  -- Второй маркер

-- Позиция (локальная)
SetPointMarkerPosition(10, 5)
local pos = GetPointMarkerPosition()  -- → {x, y}

-- Масштаб
SetPointMarkerScale(2, 2)
local scale = GetPointMarkerScale()  -- → {x, y}

-- Поворот
SetPointMarkerRotation(45)
local rot = GetPointMarkerRotation()

-- Мировая трансформация (трансформ сущности уже учтён — см. раздел Sprite).
-- Учтите: локальные аксессоры называются SetPointMarkerPosition/Rotation/Scale.
SetPointMarkerWorldPosition(120, 64, 0)
local mwp = GetPointMarkerWorldPosition(0)  -- → {x, y, z}
SetPointMarkerWorldRotation(30, 0)
local mwr = GetPointMarkerWorldRotation(0)  -- → число
local mws = GetPointMarkerWorldScale(0)     -- → {x, y}, только чтение

-- Видимость
SetPointMarkerVisible(true)
local vis = IsPointMarkerVisible()

-- Цвет (RGB, для визуализации в редакторе)
SetPointMarkerColor(1, 0, 0)
local c = GetPointMarkerColor()  -- → {r, g, b}

-- Размер (визуальный)
SetPointMarkerSize(48)
local size = GetPointMarkerSize()

-- Отображение в игре (обычно false — только в редакторе)
SetPointMarkerRenderInGame(true)
local render = GetPointMarkerRenderInGame()

-- Форма: 0 = Arrow (стрелка), 1 = Line (линия), 2 = Circle (круг), 3 = Square (квадрат)
SetPointMarkerShape(0)
local shape = GetPointMarkerShape()

-- Толщина линии / контура
SetPointMarkerThickness(2)
local th = GetPointMarkerThickness()

-- Размер наконечника стрелки (форма Arrow)
SetPointMarkerArrowHeadSize(10)
local ah = GetPointMarkerArrowHeadSize()

-- Направление стрелки в градусах (форма Arrow)
SetPointMarkerArrowDirection(90)
local ad = GetPointMarkerArrowDirection()

-- Смещение конца линии (форма Line) — конечная точка относительно маркера
SetPointMarkerLineEndOffset(50, 0)
local off = GetPointMarkerLineEndOffset()  -- → {x, y}

-- Все функции принимают необязательный index:
SetPointMarkerPosition(10, 5, 1)  -- Второй маркер
```

---

## 37. DataUtils — Структуры данных и утилиты

> **Тип:** Глобальные функции.
>
> Утилиты для работы с данными: массивы (Array), словари (Map), множества (Set),
> перечисления (Enum), структуры (Struct), таблицы данных (DataTable).

### Enum — Перечисления

```lua
-- Создать enum из строк
local Direction = Enum("Up", "Down", "Left", "Right")
-- Direction.Up = 0, Direction.Down = 1, Direction.Left = 2, Direction.Right = 3

-- Методы
local name = Direction.Name(0)          -- "Up"
local valid = Direction.IsValid(5)      -- false
local count = Direction.Count()         -- 4
local values = Direction.Values()       -- {0, 1, 2, 3}
local names = Direction.Names()         -- {"Up", "Down", "Left", "Right"}

-- Использование
local dir = Direction.Left
if dir == Direction.Left then
    Print("Направление: " .. Direction.Name(dir))
end
```

### Array — Массив

```lua
local arr = {10, 20, 30, 40, 50}

-- Добавление / удаление
Array.Push(arr, 60)                  -- Добавить в конец
local last = Array.Pop(arr)         -- Удалить и вернуть последний
Array.InsertAt(arr, 2, 15)          -- Вставить на позицию
Array.Remove(arr, 3)                -- Удалить по индексу
Array.RemoveValue(arr, 30)          -- Удалить по значению
Array.Clear(arr)                    -- Очистить

-- Поиск
local has = Array.Contains(arr, 20)  -- true
local idx = Array.Find(arr, 20)      -- Ключ
local idx = Array.IndexOf(arr, 20)   -- Индекс (1-based)
local idx = Array.LastIndexOf(arr, 20)

-- Информация
local len = Array.Length(arr)
local count = Array.Count(arr)
local count = Array.Count(arr, function(v) return v > 25 end)

-- Математика
local sum = Array.Sum(arr)
local min = Array.Min(arr)
local max = Array.Max(arr)
local avg = Array.Average(arr)

-- Трансформации
local filtered = Array.Filter(arr, function(v) return v > 25 end)
local mapped = Array.Map(arr, function(v) return v * 2 end)
local reduced = Array.Reduce(arr, function(acc, v) return acc + v end, 0)
Array.ForEach(arr, function(v, i) Print(i .. ": " .. v) end)

-- Проверки
local any = Array.Any(arr, function(v) return v > 40 end)   -- true
local all = Array.All(arr, function(v) return v > 0 end)    -- true

-- Выборка
local first = Array.First(arr)
local first = Array.First(arr, function(v) return v > 25 end)
local last = Array.Last(arr)
local taken = Array.Take(arr, 3)        -- Первые 3
local skipped = Array.Skip(arr, 2)      -- Пропустить первые 2
local slice = Array.Slice(arr, 2, 4)    -- С 2 по 4

-- Комбинирование
local joined = Array.Join(arr, ", ")     -- "10, 20, 30"
local zipped = Array.Zip(arr1, arr2)     -- {{a1,b1}, {a2,b2}, ...}
local groups = Array.GroupBy(arr, function(v) return v > 25 end)

-- Преобразования
Array.Sort(arr)                          -- Сортировка (числа/строки)
Array.Sort(arr, function(a, b) return a > b end)  -- Кастомная
Array.Reverse(arr)
Array.Shuffle(arr)
local unique = Array.Unique(arr)          -- Уникальные
local flat = Array.Flatten({{1,2},{3,4}}) -- → {1,2,3,4}
```

### Map — Словарь

```lua
local data = { name = "Hero", hp = 100, level = 5 }

-- Базовые операции
Map.Set(data, "mana", 50)
local val = Map.Get(data, "hp", 0)       -- 100 (или 0 default)
Map.Remove(data, "mana")
Map.Clear(data)

-- Информация
local keys = Map.Keys(data)              -- {"name", "hp", "level"}
local values = Map.Values(data)          -- {"Hero", 100, 5}
local has = Map.HasKey(data, "hp")       -- true
local count = Map.Count(data)            -- 3
local entries = Map.Entries(data)        -- {{key="name", value="Hero"}, ...}

-- Копирование
local copy = Map.Copy(data)             -- Поверхностная копия
local deep = Map.DeepCopy(data)         -- Глубокая копия
local merged = Map.Merge(base, overrides) -- Объединение

-- Трансформации
local filtered = Map.Filter(data, function(k, v) return type(v) == "number" end)
local mapped = Map.MapValues(data, function(k, v) return v * 2 end)
local inverted = Map.Invert(data)       -- Ключи ↔ Значения
Map.ForEach(data, function(k, v) Print(k .. "=" .. tostring(v)) end)

-- Проверки
local any = Map.Any(data, function(k, v) return v > 50 end)
local all = Map.All(data, function(k, v) return v ~= nil end)
```

### Set — Множество

```lua
-- Создать множество
local s = Set({"a", "b", "c"})
local s = Set()  -- Пустое

-- Операции
s.Add("d")
s.Remove("a")
local has = s.Has("b")           -- true
local count = s.Count()          -- 3
s.Clear()

-- Конвертация
local arr = s.ToArray()          -- {"b", "c", "d"}

-- Итерация
s.ForEach(function(value)
    Print(value)
end)

-- Множественные операции (возвращают массив)
local union = s.Union(otherSet)          -- Объединение
local intersect = s.Intersect(otherSet)  -- Пересечение

-- Разность и сравнение
local diff = s.Difference(otherSet)      -- Элементы, которых нет в otherSet
local subset = s.IsSubsetOf(otherSet)
local equals = s.Equals(otherSet)

-- Фильтрация и проверки
local filtered = s.Filter(function(v) return v ~= "a" end)
local any = s.Any(function(v) return v == "b" end)
local all = s.All(function(v) return v ~= "x" end)
```

### Struct — Фабрика структур

```lua
-- Определить "структуру" с полями по умолчанию
local Vector = Struct({ x = 0, y = 0 })

-- Создать экземпляр
local v1 = Vector()                     -- {x=0, y=0}
local v2 = Vector({ x = 10, y = 20 })  -- {x=10, y=20}
local v3 = Vector({ x = 5 })           -- {x=5, y=0}
```

### DataTable — Таблица данных

```lua
-- Создать таблицу данных (как база данных)
local items = DataTable({
    { id = 1, name = "Sword",  damage = 10, type = "weapon" },
    { id = 2, name = "Shield", armor = 5,   type = "armor" },
    { id = 3, name = "Potion", heal = 50,   type = "consumable" },
    { id = 4, name = "Axe",    damage = 15, type = "weapon" }
})

-- Методы
local count = items.Count()                         -- 4
local row = items.GetRow(2)                         -- {id=2, name="Shield", ...}
local sword = items.FindByField("name", "Sword")   -- Первая строка с name="Sword"
local weapons = items.FindAllByField("type", "weapon")  -- Все с type="weapon"
local expensive = items.Where(function(row) return (row.damage or 0) > 12 end)
local names = items.Column("name")                  -- {"Sword","Shield","Potion","Axe"}
```

### Конверторы типов

```lua
-- Базовые типы
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

-- Определить тип
local t = TypeOf(42)        -- → "number"
local t = TypeOf("hi")      -- → "string"
local t = TypeOf(true)      -- → "bool"
local t = TypeOf({})        -- → "table"
local t = TypeOf(nil)       -- → "nil"
```

### Конверторы векторов

```lua
-- Из чисел
local v = ToVec2(10, 20)            -- → Vec2(10, 20)
local v = ToVec2(5)                 -- → Vec2(5, 5)

-- Из таблицы
local v = ToVec2({x = 10, y = 20})  -- → Vec2(10, 20)
local v = ToVec2({10, 20})          -- → Vec2(10, 20)

-- Из Vec3 (отбросить z)
local v = ToVec2(someVec3)          -- → Vec2(x, y)

-- ToVec3
local v = ToVec3(10, 20, 30)
local v = ToVec3({x=1, y=2, z=3})
local v = ToVec3(someVec2)          -- → Vec3(x, y, 0)

-- ToVec4
local v = ToVec4(1, 0, 0, 1)
local v = ToVec4({r=1, g=0, b=0, a=1})  -- Таблица с r/g/b/a
local v = ToVec4(someVec3)               -- → Vec4(x, y, z, 1)

-- ToColor
local c = ToColor(1, 0, 0, 1)           -- → Color(r=1, g=0, b=0, a=1)
local c = ToColor({r=1, g=0.5, b=0})    -- Из таблицы
local c = ToColor(someVec4)             -- Из Vec4

-- Любой userdata тип → таблица
local t = ToTable(someVec2)  -- → {x=..., y=...}
local t = ToTable(someVec3)  -- → {x=..., y=..., z=...}
local t = ToTable(someColor) -- → {r=..., g=..., b=..., a=...}
local t = ToTable(someTransform) -- → {position={x,y,z}, rotation=..., scale={x,y}}
local t = ToTable(someRect)      -- → {x=..., y=..., w=..., h=...}
```

### Select и Coalesce — Условный выбор

```lua
-- Select — аналог тернарного оператора / Branch
local speed = Select(isSprinting, 400, 200)
-- Если isSprinting == true → 400, иначе → 200

-- Coalesce — вернуть первый не-nil аргумент
local name = Coalesce(customName, defaultName, "Unknown")
-- Вернёт customName, или defaultName если nil, или "Unknown"
```

### String — Строковые утилиты

> **Тип:** Глобальные функции (namespace `String`)
>
> Расширенная библиотека строк (из `DataUtilsLua`). Включает всё, что есть в `Str.*`, плюс дополнительные функции: `TrimLeft`, `TrimRight`, `IsEmpty`, `IsBlank`, `ReplaceFirst`, `Reverse`, `Find`, `Count`, `CharAt`, `ToNumber`, `Byte`, `Char`, `Join`. Для лёгкого варианта см. [`Str.*` в секции 1](#str--строковые-утилиты).

```lua
-- Разделить строку
local parts = String.Split("a,b,c", ",")  -- → {"a", "b", "c"}
local chars = String.Split("hello", "")    -- → {"h", "e", "l", "l", "o"}

-- Обрезка пробелов
local s = String.Trim("  hello  ")       -- → "hello"
local s = String.TrimLeft("  hello  ")   -- → "hello  "
local s = String.TrimRight("  hello  ")  -- → "  hello"

-- Проверки
String.StartsWith("hello world", "hello")  -- → true
String.EndsWith("hello world", "world")    -- → true
String.Contains("hello world", "lo wo")    -- → true
String.IsEmpty("")                         -- → true
String.IsBlank("   ")                      -- → true

-- Замена
String.Replace("aabbcc", "bb", "XX")          -- → "aaXXcc"
String.ReplaceFirst("aabbaabb", "bb", "XX")   -- → "aaXXaabb"

-- Регистр
String.Upper("hello")  -- → "HELLO"
String.Lower("HELLO")  -- → "hello"

-- Дополнение символами
String.PadLeft("42", 5, "0")   -- → "00042"
String.PadRight("hi", 10, ".")  -- → "hi........"

-- Повтор и реверс
String.Repeat("ab", 3)   -- → "ababab"
String.Reverse("hello")  -- → "olleh"

-- Подстрока (1-based)
String.Sub("hello", 2, 4)  -- → "ell"
String.Length("hello")      -- → 5
String.CharAt("hello", 1)  -- → "h"

-- Поиск
String.Find("hello world", "world")  -- → 7 (1-based, или -1 если не найдено)
String.Count("abcabc", "abc")        -- → 2

-- Конвертация
local n = String.ToNumber("42.5")  -- → 42.5 (или nil)
local code = String.Byte("A")      -- → 65
local ch = String.Char(65)          -- → "A"

-- Объединение массива в строку
String.Join({"a", "b", "c"}, ", ")  -- → "a, b, c"
```

### Flow Control — Управление потоком выполнения

> **Тип:** Глобальные функции
>
> Lua-объекты для управления логикой выполнения (аналоги Gate, MultiGate и т.д.).
> **Это обычные Lua-объекты**, не визуальные ноды.

#### Gate — Ворота (пропускает/блокирует выполнение)

```lua
local gate = Gate()          -- Создать закрытый
local gate = Gate(true)      -- Создать открытый

gate.Open()
gate.Close()
gate.Toggle()

-- Execute вызовет callback только если gate открыт
gate.Execute(function()
    Print("Ворота открыты!")
end)

local isOpen = gate.IsOpen()
```

#### MultiGate — Поочерёдный вызов нескольких функций

```lua
local mg = MultiGate({
    function(idx) Print("Выход 1") end,
    function(idx) Print("Выход 2") end,
    function(idx) Print("Выход 3") end
})

-- Каждый вызов Execute выполняет следующий выход: 1 → 2 → 3 → 1 → ...
mg.Execute()
mg.Execute()
mg.Execute()

-- С параметрами:
local mg = MultiGate({fn1, fn2, fn3}, true)   -- random = true
local mg = MultiGate({fn1, fn2, fn3}, false, false)  -- loop = false (остановится после 3-го)

mg.Reset()         -- Сбросить счётчик
mg.GetIndex()      -- Текущий индекс
```

#### DoOnce — Выполнить только один раз

```lua
local once = DoOnce()

-- Вызовется только первый раз
once.Execute(function()
    Print("Это сообщение покажется один раз!")
end)

-- Повторные вызовы ничего не делают
once.Execute(function() Print("Не вызовется") end)

once.Reset()   -- Сбросить (можно вызвать снова)
once.IsDone()  -- → true/false
```

#### DoN — Выполнить N раз

```lua
local fiveShots = DoN(5)

function OnUpdate(dt)
    if IsKeyJustPressed("space") then
        fiveShots.Execute(function(count)
            Print("Выстрел #" .. count)
            SpawnProjectile()
        end)
    end
end

fiveShots.GetCount()      -- Сколько раз уже выполнено
fiveShots.GetRemaining()  -- Сколько осталось
fiveShots.IsDone()        -- Все N использованы?
fiveShots.Reset()         -- Сбросить счётчик
```

#### FlipFlop — Чередование двух путей

```lua
local ff = FlipFlop()

function OnUpdate(dt)
    if IsKeyJustPressed("e") then
        ff.Execute(
            function() Print("Путь A — ВКРЫТО") end,
            function() Print("Путь B — ЗАКРЫТО") end
        )
        -- Первый вызов → A, второй → B, третий → A, ...
    end
end

ff.IsA()    -- true если следующий вызов — A
ff.Reset()  -- Сбросить на A
```

---

## 38. Material — Материалы и шейдеры

> **Тип:** Entity-bound (свойства спрайтов/флипбуков/тайлмапов) + Глобальные (`Material.*`, `MPC.*`).
>
> Материалы управляют рендерингом: режим затенения (Lit/Unlit), режим смешивания (Masked/Additive/Translucent/Opaque),
> альфа-отсечение, пользовательские шейдеры и текстуры.
> Создаются в нодовом редакторе `.ice_mat` и могут быть назначены в рантайме.

### Материалы спрайтов (Entity-bound)

```lua
-- Назначить материал из файла (.ice_mat или .ice_matinst)
SetSpriteMaterial("Content/Materials/Glow.ice_mat")
SetSpriteMaterial("Content/Materials/Custom.ice_matinst")  -- Готовый инстанс
local mat = GetSpriteMaterial()          -- Имя материала (или "")
ClearSpriteMaterial()                    -- Сбросить к стандартному

-- Режим затенения: "Lit" или "Unlit"
SetSpriteShadingMode("Unlit")
local mode = GetSpriteShadingMode()

-- Режим смешивания: "Masked", "Additive", "Translucent", "Opaque"
SetSpriteBlendMode("Additive")
local blend = GetSpriteBlendMode()

-- Альфа-отсечение (порог 0.0 – 1.0)
SetSpriteAlphaClip(0.3)
local clip = GetSpriteAlphaClip()

-- Все функции принимают опциональный index для нескольких спрайтов:
SetSpriteShadingMode("Unlit", 1)         -- Второй спрайт (индекс 0 по умолчанию)
SetSpriteBlendMode("Additive", 1)
SetSpriteAlphaClip(0.5, 1)
SetSpriteMaterial("Content/Materials/Glow.ice_mat", 1)
local mat1 = GetSpriteMaterial(1)
ClearSpriteMaterial(1)
```

### Материалы флипбуков (Entity-bound)

```lua
-- Назначить материал (.ice_mat или .ice_matinst)
SetFlipbookMaterial("Content/Materials/Fire.ice_mat")
local mat = GetFlipbookMaterial()
ClearFlipbookMaterial()

-- Режим затенения
SetFlipbookShadingMode("Lit")
local mode = GetFlipbookShadingMode()

-- Режим смешивания
SetFlipbookBlendMode("Translucent")
local blend = GetFlipbookBlendMode()

-- Альфа-отсечение
SetFlipbookAlphaClip(0.5)
local clip = GetFlipbookAlphaClip()

-- Опциональный index (как у спрайтов)
SetFlipbookMaterial("Content/Materials/Fire.ice_mat", 1)
SetFlipbookShadingMode("Unlit", 1)
```

### Материалы тайлмапов (Entity-bound)

```lua
-- Назначить материал тайлмапу (.ice_mat или .ice_matinst)
SetTilemapMaterial("Content/Materials/Water.ice_mat")
local mat = GetTilemapMaterial()
ClearTilemapMaterial()

-- Свойства (требуют назначенного материала)
SetTilemapShadingMode("Unlit")
local mode = GetTilemapShadingMode()

SetTilemapBlendMode("Additive")
local blend = GetTilemapBlendMode()

SetTilemapAlphaClip(0.5)
local clip = GetTilemapAlphaClip()

-- Опциональный index
SetTilemapMaterial("Content/Materials/Lava.ice_mat", 1)
```

> **Примечание:** При назначении материала через `SetSpriteMaterial` / `SetFlipbookMaterial` / `SetTilemapMaterial`,
> режим затенения, смешивания и alpha clip автоматически устанавливаются из файла материала.
> Последующие вызовы `SetSpriteShadingMode` и т.д. переопределяют эти значения.

### Динамические материалы (`Material.*`) — Глобальные

> **Тип:** Глобальный `MaterialInstanceDynamic`.
> Позволяет создавать runtime-инстансы материала с переопределяемыми параметрами (Scalar, Vector, Texture).
> Параметры определяются нодами **Scalar Param**, **Vector Param**, **Texture Param** в редакторе материалов.

```lua
-- Создать динамический экземпляр
local name = Material.CreateDynamic("Content/Materials/Base.ice_mat")          -- Авто-имя
local name = Material.CreateDynamic("Content/Materials/Base.ice_mat", "MyGlow") -- Своё имя

-- Загрузить готовый экземпляр из .ice_matinst
local name = Material.LoadInstance("Content/Materials/Custom.ice_matinst")

-- Удалить инстанс
Material.DestroyDynamic("MyGlow")

-- Скалярные параметры (float)
Material.SetScalar("MyGlow", "Intensity", 2.0)
local val = Material.GetScalar("MyGlow", "Intensity")
local has = Material.HasScalar("MyGlow", "Intensity")   -- true/false
Material.ClearScalar("MyGlow", "Intensity")              -- Вернуть к значению по умолчанию

-- Векторные параметры (цвет / vec4)
Material.SetVector("MyGlow", "EmissiveColor", 1, 0.5, 0)       -- RGB (A=1)
Material.SetVector("MyGlow", "EmissiveColor", 1, 0.5, 0, 1)    -- RGBA
local v = Material.GetVector("MyGlow", "EmissiveColor")         -- → {r, g, b, a}
local has = Material.HasVector("MyGlow", "EmissiveColor")
Material.ClearVector("MyGlow", "EmissiveColor")

-- Текстуры
Material.SetTexture("MyGlow", "NormalMap", "Content/Textures/normal.png")
Material.ClearTexture("MyGlow", "NormalMap")

-- Альфа-отсечение
Material.SetAlphaClip("MyGlow", 0.3)

-- Сбросить все переопределения разом
Material.ClearOverrides("MyGlow")
```

### MPC — Material Parameter Collection (`MPC.*`)

> **Тип:** Глобальный `MaterialParameterCollection`.
> Глобальные наборы параметров, на которые может ссылаться любой материал через ноду **Collection Param**.
> Изменения параметров мгновенно влияют на все материалы, использующие эту коллекцию.

```lua
-- Загрузить из файла
MPC.Load("Content/Materials/GlobalParams.ice_mpc")

-- Или создать в рантайме
MPC.Create("WorldParams")

-- Добавить параметры (имя, значение по умолчанию)
MPC.AddScalar("WorldParams", "WindStrength", 1.0)
MPC.AddVector("WorldParams", "FogColor", 0.5, 0.5, 0.7, 1.0)

-- Установить / получить
MPC.SetScalar("WorldParams", "WindStrength", 3.5)
local wind = MPC.GetScalar("WorldParams", "WindStrength")

MPC.SetVector("WorldParams", "FogColor", 0.8, 0.8, 0.9)
local fog = MPC.GetVector("WorldParams", "FogColor")   -- → {r, g, b, a}

-- Сбросить все параметры к значениям по умолчанию
MPC.Reset("WorldParams")

-- Выгрузить коллекцию
MPC.Unload("WorldParams")
```

### Ноды параметров в Material Editor

| Нода | Описание | Тип выходов |
|------|----------|-------------|
| **Scalar Param** | Float-параметр, переопределяемый через `Material.SetScalar()` | `Value` (float) |
| **Vector Param** | Цвет/вектор, переопределяемый через `Material.SetVector()` | `RGBA`, `RGB`, `R`, `G`, `B`, `A` |
| **Texture Param** | Текстура-параметр, переопределяемый через `Material.SetTexture()` | `RGB`, `R`, `G`, `B`, `A` |
| **Collection Param** | Ссылка на параметр из MPC-коллекции | `Value` (float), `Vector` (vec4) |
| **Material Function** | Вызов реюзабельной функции из `.ice_matfunc` | Зависит от функции |

### Material Function (`.ice_matfunc`)

> **Material Function** — реюзабельный подграф нод.
> Создаётся как отдельный файл `.ice_matfunc`, содержит ноды **Input** и **Output** для определения интерфейса.
> При вызове через ноду **Material Function** граф инлайнится в основной шейдер.

**Внутри графа функции используются ноды:**

| Нода | Описание |
|------|----------|
| **Input** | Входной параметр функции. `FunctionInputIndex` определяет номер входа (0, 1, 2...) |
| **Output** | Выходной результат функции. `FunctionInputIndex` определяет номер выхода (0, 1, 2...) |

> Максимальная глубина вложенности — 8 (функция может вызывать другие функции).

### Пример: Динамический эффект попадания

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

### Пример: Цикл дня и ночи через MPC

```lua
-- DayNightCycle.ice_class  (скрипт уровня)

function OnConstruct()
    timeOfDay = 12.0   -- Полдень
    daySpeed = 1.0     -- 1 секунда = 1 игровой час
end

function OnCreate()
    MPC.Load("Content/Materials/WorldParams.ice_mpc")
end

function OnUpdate(dt)
    timeOfDay = timeOfDay + dt * daySpeed
    if timeOfDay >= 24 then timeOfDay = timeOfDay - 24 end

    MPC.SetScalar("WorldParams", "TimeOfDay", timeOfDay)

    -- Ambient свет зависит от времени суток
    local night = math.abs(timeOfDay - 12) / 12  -- 0 = полдень, 1 = полночь
    local ambient = 0.8 - night * 0.6
    SetAmbientIntensity(ambient)
    SetAmbientLight(
        0.4 + (1 - night) * 0.6,
        0.4 + (1 - night) * 0.5,
        0.5 + (1 - night) * 0.3
    )

    -- Цвет солнца: тёплый днём, холодный ночью
    MPC.SetVector("WorldParams", "SunColor",
        1.0 - night * 0.7,
        0.9 - night * 0.6,
        0.7 - night * 0.2
    )
end
```

### Пример: Интерактивный объект со свечением

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
        -- Вспышка при подборе
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

## 39. Destruction — Разрушения

> **Тип:** Entity-bound + глобальные. Требует компонент **DestructibleComponent** для сущностей.
> Позволяет фрагментировать спрайты/флипбуки и тайлмапы, создавать физические обломки.

### Fracture — разрушить текущую сущность

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
-- result.count = количество фрагментов
```

Если `impactX/impactY` не указаны — используются координаты сущности.

### FractureEntity — разрушить сущность по ID

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

### ExplodeTiles — взрыв тайлмапа текущей сущности

Каждый разрушаемый тайл порождает осколки по своим **настройкам осколков (Fragment)**,
заданным в редакторе тайлсета (обычные тайлы) или в панели анимированных тайлов редактора
тайлмапа (анимированные тайлы). Таблица `opts` — необязательное переопределение во время
выполнения: любое переданное поле переопределяет значение этого тайла, а пропущенные поля
берутся из настроек тайла. Это работает так же, как компонент Destructible для сущностей.
Анимированные разрушаемые тайлы теперь тоже порождают осколки из текущего кадра флипбука.

```lua
-- Использует собственные настройки осколков каждого тайла:
local result = ExplodeTiles(x, y, 120, 400)

-- Переопределить отдельные поля только для этого взрыва:
local result = ExplodeTiles(x, y, 120, 400, 0, {
    count = 4,            -- разбить каждый тайл на 4 осколка (1 = весь тайл)
    pattern = 0,          -- 0 Grid, 1 Radial, 2 Random (только при count > 1)
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
    collisionGroup = "Debris", -- имя или индекс; по умолчанию встроенная группа Debris
    castShadow = true,
    dontBlockShadows = true,
    shadowOrigin = 0,
    shadowEdgeFade = 0.0,
    shadowZOrder = 0
})
```

### ExplodeTilesOnEntity — взрыв тайлмапа по ID

То же поведение (настройки тайла + переопределение `opts`), что и у `ExplodeTiles`, но
целью является сущность-тайлмап по её id.

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

### Управление destructible-сущностью

```lua
SetDestructible(true)
local enabled = IsDestructible()

SetDestructibleHealth(100)
local hp = GetDestructibleHealth()

local result = DamageDestructible(25, impactX, impactY)
-- result.destroyed = true/false
-- result.health = текущее HP
```

### Параметры тени фрагментов

```lua
SetDestructibleFragmentCastShadow(true)
local cast = GetDestructibleFragmentCastShadow()

-- Не блокировать тени (по умолчанию true): пока глобально включено «Коллайдеры блокируют тени», осколки всё равно пропускают тени сквозь себя. Отключите, чтобы осколки блокировали тени.
SetDestructibleFragmentDontBlockShadows(true)
local dontBlock = GetDestructibleFragmentDontBlockShadows()

SetDestructibleFragmentShadowOrigin(1)            -- 0 = Center, 1 = Top, 2 = Bottom
local origin = GetDestructibleFragmentShadowOrigin()

SetDestructibleFragmentShadowEdgeFade(0.25)
local fade = GetDestructibleFragmentShadowEdgeFade()

SetDestructibleFragmentShadowZOrder(1)            -- отрицательное = на задний план, положительное = на передний план, 0 = плоскость кастера (по умолчанию)
local zo = GetDestructibleFragmentShadowZOrder()  -- → float
```

### Группа коллизий фрагментов

```lua
SetDestructibleFragmentCollisionGroup(CollisionGroups.GetIndex("Debris"))  -- индекс группы коллизий для создаваемых фрагментов
local group = GetDestructibleFragmentCollisionGroup()                      -- → int (−1 = по умолчанию: группа «Debris», иначе 0)
```

### Конфигурация разрушения

```lua
-- Как объект разбивается на части (всё хранится в DestructibleComponent)
SetDestructibleDestructOnStart(false)          -- разрушить сразу при старте уровня
local onStart = GetDestructibleDestructOnStart()

SetDestructibleFragmentCount(8)                -- число фрагментов (ограничено 2..64)
local n = GetDestructibleFragmentCount()

SetDestructiblePattern(0)                      -- 0 = Grid, 1 = Radial, 2 = Random
local pattern = GetDestructiblePattern()

SetDestructibleExplosionForce(300)             -- импульс разлёта фрагментов
local force = GetDestructibleExplosionForce()

SetDestructibleImpactThreshold(0)              -- мин. импульс удара для авто-разрушения (0 = выкл)
local threshold = GetDestructibleImpactThreshold()

SetDestructibleMaxDebrisCount(256)             -- глобальный лимит живых обломков (ограничено 1..2048)
local maxDebris = GetDestructibleMaxDebrisCount()

SetDestructibleDestroyOriginal(true)           -- удалить исходную сущность после разрушения
local destroyOrig = GetDestructibleDestroyOriginal()

-- Время жизни / затухания фрагментов (секунды)
SetDestructibleFragmentLifetime(3.0)
local life = GetDestructibleFragmentLifetime()
SetDestructibleFragmentFadeTime(1.0)
local fadeTime = GetDestructibleFragmentFadeTime()

-- Физический материал фрагментов
SetDestructibleFragmentGravityScale(1.0)
local grav = GetDestructibleFragmentGravityScale()
SetDestructibleFragmentDensity(1.0)
local density = GetDestructibleFragmentDensity()
SetDestructibleFragmentFriction(0.3)           -- ограничено 0..1
local friction = GetDestructibleFragmentFriction()
SetDestructibleFragmentRestitution(0.3)        -- ограничено 0..1
local restitution = GetDestructibleFragmentRestitution()

-- События коллизий фрагментов
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

### Обломки (debris)

```lua
SetFragmentLifetime(fragmentId, 2.0, 0.5)
local isDebris = IsDebris(fragmentId)
ClearAllDebris()
```

---

## 40. Практические примеры

Ниже — 10 небольших, простых примеров для новичков. Каждый пример — отдельный скрипт.

### 1) 🚶 Персонаж: ходьба, прыжок, приседание

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

### 2) 🏊 Платформа туда-обратно

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

### 3) 🚪 Дверь: подсказка и открытие по `E`

```lua
-- Door.ice_class

function OnConstruct()
    isOpen = false
    playerInZone = false
    SetWidgetVisible(false)
    SetWidgetText("HintText", "Нажмите E")
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

### 4) 🪙 Сбор монет

```lua
-- PlayerCoins.ice_class

function OnConstruct()
    coins = 0
end

function OnSensorEnter(tag, id)
    if tag == "Coin" then
        DestroyEntity(id)
        coins = coins + 1
        Print("Монеты: " .. coins)
    end
end
```

### 5) ❤️ Здоровье и получение урона

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

### 6) 👾 Простой враг: патрулирование

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

### 7) ➡️ Переход на следующий уровень

```lua
-- LevelExit.ice_class

function OnSensorEnter(tag, id)
    if tag == "Player" then
        LoadLevel("Content/Maps/NextLevel.icemap")
    end
end
```

### 8) 💾 Чекпоинт: сохранение/загрузка

```lua
-- CheckpointSaveLoad.ice_class

function OnSensorEnter(tag, id)
    if tag == "Checkpoint" then
        local pos = GetPosition()
        SetGameFloat("checkpoint_x", pos.x)
        SetGameFloat("checkpoint_y", pos.y)
        Print("Чекпоинт сохранён")
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

### 9) 🧾 HUD: HP и монеты

```lua
-- HUDSimple.ice_class

function OnCreate()
    UpdateHUD(100, 0)
end

function UpdateHUD(hp, coins)
    SetWidgetProgress("HealthBar", hp / 100)
    SetWidgetText("CoinsText", "Монеты: " .. coins)
end
```

### 10) 🔫 Простой выстрел по мыши

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

## 41. Mods — Система модов

Система модов позволяет игрокам и разработчикам расширять игру **без перекомпиляции** движка. Моды — это Lua-скрипты, которые загружаются **только в рантайме** (при запуске игры) и имеют полный доступ ко всему Lua API движка.

> **Важно:** моды работают только во время запуска игры (Play), не в редакторе. Для расширения самого движка используется система плагинов (C++ DLL/SO) — см. документацию архитектуры движка.

### 41.1 Структура мода

Каждый мод — это папка внутри директории `Mods/`:

```
Mods/
├── MyMod/
│   ├── mod.json          ← дескриптор мода (обязателен)
│   ├── main.lua          ← точка входа (по умолчанию)
│   └── modules/
│       └── helpers.lua   ← дополнительные модули
```

### 41.2 Дескриптор `mod.json`

```json
{
    "Name": "MyMod",
    "Description": "Мой первый мод для IceBox",
    "Author": "PlayerName",
    "Version": "1.0.0",
    "Icon": "icon.png",
    "EntryScript": "main.lua",
    "LoadOrder": 100,
    "APIVersion": 1,
    "Dependencies": ["CoreLibMod"]
}
```

| Поле | Тип | По умолчанию | Описание |
|---|---|---|---|
| `Name` | string | имя папки | Название мода |
| `Description` | string | `""` | Описание |
| `Author` | string | `""` | Автор |
| `Version` | string | `"1.0.0"` | Версия мода |
| `Icon` | string | `""` | Необязательный путь (относительно папки мода) к иконке `.png` / `.jpg` / `.jpeg`, которая отображается в панелях редактора и лаунчера. Если не указан, движок сам ищет `icon.png` / `icon.jpg` / `icon.jpeg` в папке мода. При отсутствии иконки показывается серый квадрат со знаком `?`. |
| `EntryScript` | string | `"main.lua"` | Главный скрипт |
| `LoadOrder` | int | `100` | Порядок загрузки (меньше = раньше) |
| `APIVersion` | int | `1` | Минимальная версия API движка |
| `Dependencies` | string[] | `[]` | Список модов, которые должны быть загружены раньше |

### 41.3 Песочница (Sandbox)

Каждый мод исполняется в **изолированном окружении**. Это означает:

- Мод **видит** весь глобальный Lua API движка (все функции из этой документации).
- Мод **не может** перезаписать глобальные функции или переменные другого мода.
- Переменные `MOD_NAME`, `MOD_VERSION`, `MOD_DIR` уникальны для каждого мода.

### 41.4 Доступные переменные мода

| Переменная | Тип | Описание |
|---|---|---|
| `MOD_NAME` | string | Название текущего мода |
| `MOD_VERSION` | string | Версия текущего мода |
| `MOD_DIR` | string | Абсолютный путь к папке мода |

### 41.5 `ModRequire` — загрузка модулей

Для подключения дополнительных Lua-файлов из папки мода используйте `ModRequire`:

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

`ModRequire` заменяет `.` на `/` в имени модуля и ищет файл относительно `MOD_DIR`. Загруженный файл исполняется в том же sandbox-окружении мода.

### 41.6 Жизненный цикл мода

1. **Discover** — движок сканирует `Mods/` и читает `mod.json` при запуске.
2. **Enable** — в конфигурации `Config/Mods.json` отмечается, какие моды включены.
3. **Load** — при старте игры (OnRuntimeStart) включённые моды **топологически сортируются** по полю `Dependencies` (с откатом на `LoadOrder` и предупреждением в лог при обнаружении цикла) и последовательно загружаются. `APIVersion` каждого мода должен быть ≤ версии API движка.
4. **Execute** — `main.lua` исполняется в своём изолированном окружении. Мод может объявить любые из колбэков, перечисленных ниже.
5. **Unload** — при остановке игры (OnRuntimeStop) вызываются `OnLevelEnd` и `OnModUnload`, после чего sandbox удаляется и Lua GC освобождает ресурсы. Порядок выгрузки — обратный порядку загрузки.

#### Колбэки жизненного цикла (все необязательны)

Объявите любую из этих функций как **глобальную** в `main.lua` мода (или в файлах, подключённых через `ModRequire`) — движок вызовет их автоматически:

| Функция | Когда срабатывает |
|---------|-------------------|
| `OnModLoad()`            | Один раз, сразу после того как `main.lua` успешно выполнился. Срабатывает и при старте рантайма, и при включении мода во время игры через `Mods.SetEnabled`. |
| `OnLevelStart()`         | После того как уровень и все его сущности со скриптами полностью инициализированы. Безопасное место для спауна сущностей, чтения тегов, регистрации таймеров. |
| `OnLevelUpdate(dt)`      | Каждый кадр, пока уровень активен. |
| `OnLevelFixedUpdate(dt)` | Каждый тик фиксированного шага симуляции — используйте, если работаете с физикой. |
| `OnLevelLateUpdate(dt)`  | Каждый кадр, после основного прохода обновления — вызывается позже, чем `OnLevelUpdate`. |
| `OnLevelEnd()`           | Прямо перед разрушением уровня (когда игра останавливается или мод выключают во время игры). |
| `OnModUnload()`          | Последний шанс перед уничтожением sandbox мода — отменяйте таймеры, сохраняйте состояние. |

Ошибка, возникшая внутри колбэка, логируется с именем мода (`[Mod:<name>] <fn> error: ...`) и **не** мешает остальным модам получить тот же вызов.

#### `ModRequire` и «мягкое» поведение при ошибках

Если файл, запрошенный через `ModRequire("folder.file")`, не найден или содержит ошибку, функция возвращает `nil` и пишет предупреждение/ошибку в лог с именем мода. Это не прерывает загрузку остальной части мода — можно безопасно проверять результат:

```lua
local settings = ModRequire("modules.settings")
if settings then
    settings.Apply()
end
```

### 41.7 Конфигурация `Config/Mods.json`

```json
{
    "SearchDirectory": "Mods",
    "Enabled": {
        "MyMod": true,
        "AnotherMod": false
    }
}
```

Этот файл автоматически управляется через панель **Plugins** в эдиторе (вкладка **Mods**).

### 41.8 Примеры модов

#### Простой мод: приветствие при старте уровня

```lua
-- Mods/HelloMod/main.lua
On("LevelStart", function()
    Print("[" .. MOD_NAME .. "] Уровень начался!")
end)
```

#### Мод: модификация здоровья игрока

```lua
-- Mods/DoubleHP/main.lua
On("EntitySpawned", function(entityId, tag)
    if tag == "Player" then
        local hp = GetEntityData(entityId, "HP")
        SetEntityData(entityId, "HP", hp * 2)
        Print("[" .. MOD_NAME .. "] HP игрока удвоено!")
    end
end)
```

#### Мод с модулями

```lua
-- Mods/QuestMod/main.lua
local quests = ModRequire("quests.manager")

On("LevelStart", function()
    quests.init()
    Print("[" .. MOD_NAME .. "] Квесты загружены: " .. quests.count())
end)
```

```lua
-- Mods/QuestMod/quests/manager.lua
local M = {}
local questList = {}

function M.init()
    questList = {
        { name = "Найди меч", done = false },
        { name = "Победи босса", done = false }
    }
end

function M.count()
    return #questList
end

return M
```

### 41.9 Управление модами в эдиторе

В панели **Plugins** (вкладка **Mods**) доступно:

- Включение/выключение модов чекбоксом.
- Поиск по имени.
- Отображение статуса: серый = выключен, жёлтый = включён (ожидает запуска), зелёный = загружен.
- Отображение автора, версии и `LoadOrder`.
- Кнопка **Refresh** для повторного сканирования директории.

### 41.10 `Mods` — Lua API для управления модами

Глобальная таблица **`Mods`** позволяет скриптам (включая сами моды) запрашивать и управлять модами **во время выполнения**. Это основа для создания внутриигровых меню модов.

#### `Mods.GetAll()` → table

Возвращает массив всех обнаруженных модов.

```lua
local allMods = Mods.GetAll()
for _, mod in ipairs(allMods) do
    print(mod.name .. " v" .. mod.version .. " — " .. (mod.enabled and "ВКЛ" or "ВЫКЛ"))
end
```

Каждый элемент — таблица со следующими полями:

| Поле | Тип | Описание |
|---|---|---|
| `name` | string | Название мода |
| `description` | string | Описание мода |
| `author` | string | Автор |
| `version` | string | Версия |
| `enabled` | bool | Включён ли мод в конфиге |
| `loaded` | bool | Загружен ли мод и работает ли сейчас |
| `loadOrder` | int | Приоритет загрузки (меньше = раньше) |

#### `Mods.GetCount()` → int

Возвращает общее количество обнаруженных модов.

```lua
print("Всего модов: " .. Mods.GetCount())
```

#### `Mods.GetEnabledCount()` → int

Возвращает количество включённых модов.

```lua
print("Включено: " .. Mods.GetEnabledCount() .. " / " .. Mods.GetCount())
```

#### `Mods.IsEnabled(name)` → bool

Проверяет, включён ли мод.

```lua
if Mods.IsEnabled("SuperWeapons") then
    print("Мод SuperWeapons включён")
end
```

#### `Mods.IsLoaded(name)` → bool

Проверяет, загружен ли мод и работает ли он в данный момент.

```lua
if Mods.IsLoaded("SuperWeapons") then
    print("SuperWeapons активен")
end
```

#### `Mods.SetEnabled(name, enabled)`

Включает или выключает мод. Изменение вступает в силу **немедленно** (мод загружается или выгружается) и **автоматически сохраняется** в `Config/Mods.json`.

```lua
-- Включить мод
Mods.SetEnabled("SuperWeapons", true)

-- Выключить мод
Mods.SetEnabled("SuperWeapons", false)
```

> **Примечание:** включение мода во время игры немедленно загрузит его `main.lua`. Выключение уничтожит его песочницу.

#### `Mods.GetInfo(name)` → table | nil

Возвращает подробную информацию о конкретном моде, или `nil` если мод не найден.

```lua
local info = Mods.GetInfo("SuperWeapons")
if info then
    print("Название: " .. info.name)
    print("Автор: " .. info.author)
    print("Описание: " .. info.description)
    print("Версия: " .. info.version)
    print("Включён: " .. tostring(info.enabled))
    print("Загружен: " .. tostring(info.loaded))
    print("Порядок загрузки: " .. info.loadOrder)
    print("Скрипт входа: " .. info.entryScript)
    print("Папка: " .. info.folderPath)
end
```

| Поле | Тип | Описание |
|---|---|---|
| `name` | string | Название мода |
| `description` | string | Описание |
| `author` | string | Автор |
| `version` | string | Версия |
| `enabled` | bool | Включён ли |
| `loaded` | bool | Загружен ли сейчас |
| `loadOrder` | int | Приоритет загрузки |
| `entryScript` | string | Имя файла входного скрипта (напр. `"main.lua"`) |
| `folderPath` | string | Абсолютный путь к папке мода |

#### `Mods.Refresh()`

Выгружает все моды, пересканирует директорию `Mods/`, перечитывает конфиг и загружает включённые моды.

```lua
Mods.Refresh()
print("Моды обновлены. Найдено: " .. Mods.GetCount())
```

#### Полный пример: внутриигровое меню модов

```lua
-- Скрипт уровня: простое меню модов через виджеты
function OnLevelStart()
    local mods = Mods.GetAll()

    for i, mod in ipairs(mods) do
        -- Создать кнопку-переключатель для каждого мода
        local label = mod.name .. " v" .. mod.version
        if mod.enabled then
            label = "[ВКЛ] " .. label
        else
            label = "[ВЫКЛ] " .. label
        end

        -- Используйте систему виджетов для отображения кнопок
        -- При нажатии:
        -- Mods.SetEnabled(mod.name, not mod.enabled)
    end
end
```

---

## 42. DLC — Дополнительный контент

### Обзор

Таблица **`DLC`** предоставляет Lua API для работы с дополнительным контентом (DLC). DLC состоит из манифеста (`.json`) в папке `DLC/` рядом с игрой и самого контента — либо упакованного в `.icepak`, либо в виде лоос-файлов.

При запуске игры движок автоматически:
1. Сканирует папку `DLC/` на наличие манифестов
2. Монтирует все `.icepak` из папки `DLC/` в VFS (включая split-части `_0.icepak`, `_1.icepak`...)
3. Лоос-DLC, установленные в свою `contentPrefix`-папку (чаще всего внутри `Content/`), подхватываются обычным файловым индексом игры

Все DLC-файлы доступны по своим обычным путям, поэтому Lua-код просто вызывает, например, `LoadLevel("Content/DLC/DarkForest/Levels/Forest.icemap")` без разницы — это базовый контент или DLC.

### Два режима упаковки

**Упакованный (`.icepak`)** — рекомендован для релиза:
- Один архив на DLC (или split, если задан лимит размера)
- Защищает ассеты от случайной модификации, уменьшает беспорядок в файлах
- Монтируется в VFS только на чтение

**Лоос** — удобно для итераций, моддинга или магазинов, предпочитающих распакованные depot:
- Файлы контента копируются в папку `contentPrefix` как есть
- Без архивов, легко делать diff/patch отдельных файлов

### Структура DLC в билде

Упакованный:

```
MyGame/
├── MyGame.exe
├── Content/                     ← основной контент игры
├── DLC/
│   ├── expansion01.json         ← манифест DLC
│   ├── expansion01.icepak       ← архив с контентом DLC
│   ├── skins_pack.json
│   └── skins_pack.icepak
└── game.json
```

Лоос:

```
MyGame/
├── MyGame.exe
├── Content/
│   └── DLC/
│       └── DarkForest/          ← файлы DLC (contentPrefix)
│           ├── Levels/Forest.icemap
│           └── Textures/dark_trees.png
├── DLC/
│   └── expansion01.json         ← только манифест, без .icepak
└── game.json
```

### Формат DLC-манифеста

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

`"packed": false` означает лоос-DLC. Для лоос-DLC флаг «installed» ставится по наличию папки `contentPrefix` в корне игры.

### Упаковка DLC в редакторе

1. Создайте папку DLC-контента внутри `Content/`, например: `Content/DLC/DarkForest/`
2. Разместите в ней уровни, текстуры, скрипты и другие ассеты
3. Откройте **Инструменты → Упаковщик DLC**
4. Заполните поля:
   - **DLC ID** — уникальный идентификатор (`expansion01`, `dark_forest`)
   - **DLC Name** — отображаемое имя (`Dark Forest Expansion`)
   - **DLC Version** — версия DLC (`1.0.0`)
   - **Min Game Version** — минимальная версия игры (`1.0.0`)
   - **Content Folder** — выберите папку с контентом DLC внутри `Content/`
   - **Pack as .icepak** — включить для упакованного режима, отключить для лооса
   - **Max DLC Pak Size (MB)** — опциональный лимит, сверх которого архив будет разбит на части
5. Нажмите **Package DLC**

В выходной папке появятся либо `DLC/<dlcId>.icepak` + `DLC/<dlcId>.json`, либо дерево `contentPrefix/` + `DLC/<dlcId>.json`.

### Для дистрибуционных магазинов

- **Основной билд** = один depot/upload
- **Каждый DLC** = отдельный depot с `DLC/<id>.json` и либо `DLC/<id>.icepak`, либо файлами `contentPrefix/`
- Магазин сам управляет загрузкой/удалением DLC у игрока
- Lua-скрипты проверяют наличие через `DLC.IsInstalled()` перед загрузкой DLC-контента

---

### 42.1 DLC.IsInstalled

```lua
DLC.IsInstalled(dlcId) -> bool
```

Проверяет, установлен ли DLC (присутствует `.icepak` архив, или для лоос-DLC — папка `contentPrefix`).

| Параметр | Тип | Описание |
|----------|------|----------|
| `dlcId` | `string` | Уникальный идентификатор DLC |

**Возвращает:** `true` если DLC установлен, `false` если нет.

```lua
if DLC.IsInstalled("expansion01") then
    Print("DLC 'Dark Forest' доступен!")
    LoadLevel("Content/DLC/DarkForest/Levels/Forest.icemap")
else
    Print("DLC не установлен")
end
```

---

### 42.2 DLC.GetAll

```lua
DLC.GetAll() -> table
```

Возвращает массив таблиц со всеми известными DLC (как установленными, так и нет — если манифест присутствует).

**Возвращает:** массив (1-индексированный) таблиц с полями:

| Поле | Тип | Описание |
|------|------|----------|
| `id` | `string` | Идентификатор DLC |
| `name` | `string` | Отображаемое название |
| `version` | `string` | Версия DLC |
| `gameVersionMin` | `string` | Минимальная версия игры |
| `installed` | `bool` | Установлен ли (архив или лоос-папка на месте) |
| `packed` | `bool` | `true` если DLC поставляется как `.icepak`, `false` для лооса |
| `fileCount` | `int` | Количество файлов |
| `contentPrefix` | `string` | Путь к контенту DLC |

```lua
local allDLC = DLC.GetAll()
for _, dlc in ipairs(allDLC) do
    local status = dlc.installed and "Установлен" or "Не установлен"
    Print(dlc.name .. " v" .. dlc.version .. " — " .. status)
end
```

---

### 42.3 DLC.GetInstalledIds

```lua
DLC.GetInstalledIds() -> table
```

Возвращает массив строк — идентификаторы всех установленных DLC.

```lua
local ids = DLC.GetInstalledIds()
Print("Установлено DLC: " .. #ids)
for _, id in ipairs(ids) do
    Print("  - " .. id)
end
```

---

### 42.4 DLC.GetInfo

```lua
DLC.GetInfo(dlcId) -> table | nil
```

Возвращает подробную информацию о конкретном DLC.

| Параметр | Тип | Описание |
|----------|------|----------|
| `dlcId` | `string` | Идентификатор DLC |

**Возвращает:** таблицу с полями или `nil` если DLC не найден.

| Поле | Тип | Описание |
|------|------|----------|
| `id` | `string` | Идентификатор DLC |
| `name` | `string` | Отображаемое название |
| `version` | `string` | Версия DLC |
| `gameVersionMin` | `string` | Минимальная версия игры |
| `installed` | `bool` | Установлен ли |
| `packed` | `bool` | `true` если упакован в `.icepak`, `false` для лооса |
| `fileCount` | `int` | Количество файлов |
| `totalSize` | `int` | Общий размер в байтах |
| `contentPrefix` | `string` | Путь к контенту |

```lua
local info = DLC.GetInfo("expansion01")
if info then
    Print("DLC: " .. info.name)
    Print("Версия: " .. info.version)
    Print("Мин. версия игры: " .. info.gameVersionMin)
    Print("Файлов: " .. info.fileCount)
    Print("Размер: " .. math.floor(info.totalSize / 1024 / 1024) .. " МБ")
    Print("Контент: " .. info.contentPrefix)
    Print("Статус: " .. (info.installed and "установлен" or "не установлен"))
else
    Print("DLC expansion01 не найден")
end
```

---

### 42.5 DLC.GetCount

```lua
DLC.GetCount() -> int
```

Возвращает общее количество DLC (включая неустановленные, но имеющие манифест).

```lua
Print("Всего DLC: " .. DLC.GetCount())
```

---

### 42.6 DLC.GetInstalledCount

```lua
DLC.GetInstalledCount() -> int
```

Возвращает количество установленных DLC.

```lua
Print("Установлено DLC: " .. DLC.GetInstalledCount() .. " из " .. DLC.GetCount())
```

---

### 42.7 Практические примеры

#### Главное меню с DLC-контентом

```lua
function OnCreate()
    -- Показать кнопку DLC только если установлен
    if DLC.IsInstalled("expansion01") then
        if HasWidgetElement("DLCButton") then
            SetWidgetElementVisible("DLCButton", true)
            SetWidgetText("DLCButton", "Dark Forest")
        end
    end
end
```

#### Загрузка DLC-уровня

```lua
function StartDLCCampaign()
    if not DLC.IsInstalled("expansion01") then
        Print("Необходимо установить DLC 'Dark Forest Expansion'")
        -- Можно показать окно покупки DLC
        return
    end

    LoadLevel("Content/DLC/DarkForest/Levels/Forest.icemap")
end
```

#### Список DLC в настройках

```lua
function ShowDLCList()
    local allDLC = DLC.GetAll()
    if #allDLC == 0 then
        Print("DLC не обнаружены")
        return
    end

    Print("=== Дополнительный контент ===")
    for i, dlc in ipairs(allDLC) do
        local status
        if dlc.installed then
            status = dlc.packed and "Готов (packed)" or "Готов (loose)"
        else
            status = "Не установлен"
        end
        Print(i .. ". " .. dlc.name .. " v" .. dlc.version .. " - " .. status)
    end
    Print("Установлено: " .. DLC.GetInstalledCount() .. "/" .. DLC.GetCount())
end
```

#### Условный спавн контента из DLC

```lua
function OnCreate()
    -- Спавн DLC-врагов если DLC установлен
    if DLC.IsInstalled("dark_forest") then
        local info = DLC.GetInfo("dark_forest")
        if info then
            SpawnEntity(info.contentPrefix .. "/Scripts/ForestEnemy.ice_class", 100, 200)
            SpawnEntity(info.contentPrefix .. "/Scripts/ForestBoss.ice_class", 500, 200)
        end
    end
end
```

#### Сохранение с учётом DLC

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
        Print("⚠ Сохранение использует DLC, который больше не установлен!")
    end
end
```

---

## 43. Ads — Реклама (Google AdMob)

### Обзор

Таблица **`Ads`** предоставляет Lua API для отображения рекламы в игре через **Google AdMob**.
Поддерживаются **баннеры**, **межстраничная** (полноэкранная между экранами) и **наградная** (просмотр рекламы за награду) реклама.

> **Платформа — *показ* рекламы:** Android и iOS. Обе платформы работают на одном и том же
> Google Mobile Ads SDK за одинаковыми вызовами `Ads.*` и с одинаковыми строками событий, так
> что ветвления в скрипте не нужны. `Ads.IsSupported()` сообщает, присутствует ли SDK в
> запущенном бинарнике на самом деле.
>
> **Платформа — всё остальное в этой главе:** рекламный идентификатор работает и на Android, и на iOS, а собственные фреймворки Apple для атрибуции и промо в сторе (SKAdNetwork, Apple Search Ads, SKOverlay) поддержаны полностью и без сторонних SDK — см. 43.10.
>
> **Требование при сборке — Android:** включите «Google AdMob (Ads)» в окне Build Game и
> укажите ваш AdMob App ID. Gradle подтянет `play-services-ads` сам. Пустой AdMob App ID при
> включённой рекламе — жёсткая ошибка сборки, ровно как на iOS: SDK читает
> `com.google.android.gms.ads.APPLICATION_ID` из манифеста при старте процесса и без настоящего
> идентификатора ничего не отдаёт, то есть APK выглядел бы рабочим и никогда не показал рекламу.
>
> **Требование при сборке — iOS:** Google Mobile Ads SDK не распространяется вместе с движком,
> поэтому один раз на машину скачайте его у Google и положите
> `GoogleMobileAds.xcframework` в `Tools/BuildSystem/Vendor/GoogleMobileAds/` внутри папки
> движка.
>
> Затем включите **Ads & Attribution** в Build Game → iOS и заполните **Идентификатор приложения Рекламы в приложении**. CMake
> найдёт `Tools/BuildSystem/Vendor/GoogleMobileAds/GoogleMobileAds.xcframework`, определит
> `ICE_HAS_ADMOB` и слинкует его; в логе сборки будет напечатана подхваченная версия SDK. Без
> положенного SDK любой вызов `Ads.*` остаётся no-op и пишет в лог причину. Пустой AdMob App ID
> при слинкованном SDK — жёсткая ошибка конфигурации, потому что SDK падает на старте, если в
> `Info.plist` нет `GADApplicationIdentifier`.
>
> SDK лицензируется вам напрямую Google по Android SDK Licence Agreement и Google APIs Terms of
> Service, а не студией IceBoxCrew — см. `THIRD_PARTY_NOTICES.txt`, раздел 14. Подключив его,
> вы становитесь контролёром данных для всего, что он собирает: на вас согласие пользователя,
> ответы в App Privacy и собственная политика конфиденциальности.
>
> Движок объявляет ровно один `SKAdNetworkIdentifier` — собственный гугловский
> `cstr6suwn9.skadnetwork`, потому что объявлять сети, к которым бинарник никогда не
> обращается, значит сделать ответы в App Privacy недостоверными. Если включаете **медиацию**
> AdMob, добавьте идентификаторы партнёрских сетей в массив `SKAdNetworkItems` в `Info.plist`
> экспортированного Xcode-проекта (*Extra Usage Descriptions* подставляет только строковые
> ключи, не массивы).

---

### 43.1 Ads.IsSupported

```lua
Ads.IsSupported() -> bool
```

Возвращает `true`, когда Google Mobile Ads SDK действительно присутствует в запущенном бинарнике:
Android с включённой «Google AdMob (Ads)» либо iOS со слинкованным
`GoogleMobileAds.xcframework`. Везде остальном — `false`, и любой вызов `Ads.*` ничего не делает.

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

Инициализирует AdMob SDK. Должна быть вызвана один раз перед использованием любых других функций `Ads`.
Результат приходит асинхронно через `Ads.OnInitialized`. `Ads.IsInitialized()` возвращает то же
состояние синхронно, а повторный `Ads.Init()` после успешной инициализации просто ещё раз
вызовет `Ads.OnInitialized(true)`, не переинициализируя SDK.

```lua
Ads.Init()
Ads.OnInitialized(function(success)
    if success then
        Print("AdMob готов!")
    end
end)
```

---

### 43.3 Конфигурация рекламных запросов (политики и тестирование)

Вызывайте это **до** `Ads.Init()` — Google требует, чтобы теги child-directed и under-age были
выставлены до старта SDK. Дальше они применяются ко всем запросам баннеров, межстраничной и
наградной рекламы и работают одинаково на Android и iOS.

```lua
Ads.SetTestDeviceIds({ "33BE2250B43518CCDA7DE426D04EE231" })  -- тестовая реклама на своих устройствах
Ads.SetChildDirected(true)          -- COPPA: true / false / nil (= не указано)
Ads.SetUnderAgeOfConsent(true)      -- GDPR, младше 16: true / false / nil (= не указано)
Ads.SetMaxAdContentRating("G")      -- "G", "PG", "T", "MA" либо "" (не указано)
Ads.SetNonPersonalizedAds(true)     -- запрашивать только неперсонализированную рекламу
Ads.Init()
```

| Функция | Описание |
|---------|----------|
| `Ads.SetTestDeviceIds(list)` | Массив ID устройств, которым отдавать тестовую рекламу. ID печатается в logcat / консоль Xcode при первом рекламном запросе. **Никогда не кликайте свою боевую рекламу** — за это блокируют аккаунт AdMob. |
| `Ads.SetChildDirected(enabled?)` | Помечает запросы как предназначенные детям (COPPA). Без аргумента — «не указано». |
| `Ads.SetUnderAgeOfConsent(enabled?)` | Помечает пользователя как не достигшего возраста согласия (GDPR). Без аргумента — «не указано». |
| `Ads.SetMaxAdContentRating(rating)` | Ограничивает рейтинг контента рекламы: `"G"`, `"PG"`, `"T"`, `"MA"`. Любое другое значение — «не указано». |
| `Ads.SetNonPersonalizedAds(flag)` | При `true` каждый запрос уходит с `npa=1`. Нужно, когда `Consent.CanShowAds()` истинно, но пользователь отказался от персонализации. |

> Неперсонализированная реклама приносит меньше денег, чем персонализированная. Включайте флаг
> только когда этого действительно требует согласие — решение принимается по главе 49 (Consent).

---

### 43.4 Баннерная реклама

```lua
Ads.SetBannerUnitId(unitId)       -- Установить ID рекламного блока баннера
Ads.ShowBanner(position?)          -- Показать баннер (0 = сверху, 1 = снизу; по умолчанию снизу)
Ads.HideBanner()                   -- Скрыть баннер (оставляет загруженным)
Ads.DestroyBanner()                -- Полностью уничтожить баннер
Ads.IsBannerVisible() -> bool      -- Проверить, виден ли баннер
Ads.GetBannerHeight() -> int       -- Высота видимого баннера в пикселях устройства (0, если скрыт)
```

**Константы:** `Ads.BANNER_TOP` (0), `Ads.BANNER_BOTTOM` (1)

```lua
Ads.SetBannerUnitId("ca-app-pub-XXXXX/YYYYY")
Ads.ShowBanner(Ads.BANNER_BOTTOM)

Ads.OnBannerEvent(function(event)
    Print("Событие баннера: " .. event) -- "loaded", "failed:X", "impression", "clicked", "opened", "closed"
    if event == "loaded" then
        -- Держим кнопки HUD подальше от полосы баннера:
        SetHudBottomInset(Ads.GetBannerHeight())
    end
end)
```

Обе платформы используют **адаптивный закреплённый баннер** по текущей ширине экрана, отступая
от выреза дисплея и системных панелей: баннер никогда не наезжает на «чёлку» и не остаётся с
пустыми полями по краям. Именно поэтому `Ads.GetBannerHeight()` — единственный корректный способ
зарезервировать под него место: высота зависит от устройства и ориентации, а не равна
фиксированным 50 dp.

Баннер автоматически ставится на паузу и возобновляется вместе с приложением, а при повороте
экрана пересчитывает размер и перезапрашивает рекламу — на обеих платформах, из Lua ничего для
этого делать не нужно. После поворота перечитывайте `Ads.GetBannerHeight()`: у игры со свободной
ориентацией высота в альбомном режиме отличается от портретной.

Повторный `Ads.ShowBanner(position)` при уже существующем баннере переносит его на новую позицию
и снова показывает, не запрашивая рекламу заново — если только между вызовами вы не поменяли
`Ads.SetBannerUnitId()`: тогда старый баннер сносится и запрашивается новый блок.

---

### 43.5 Межстраничная реклама (Interstitial)

```lua
Ads.SetInterstitialUnitId(unitId)  -- Установить ID рекламного блока
Ads.LoadInterstitial()              -- Предзагрузить межстраничную рекламу
Ads.ShowInterstitial()              -- Показать предзагруженную рекламу
Ads.IsInterstitialReady() -> bool   -- Проверить, загружена ли реклама
```

Полноэкранный объект AdMob **одноразовый**. `Ads.ShowInterstitial()` сразу расходует загруженную
рекламу, поэтому `Ads.IsInterstitialReady()` становится `false` в момент показа — предзагружайте
следующую по событию `closed`. `Ads.LoadInterstitial()` ничего не делает, если загрузка уже идёт
или реклама уже в кэше, так что вызывать её «на всякий случай» безопасно.

```lua
Ads.SetInterstitialUnitId("ca-app-pub-XXXXX/ZZZZZ")
Ads.LoadInterstitial()

Ads.OnInterstitialEvent(function(event)
    if event == "loaded" then
        Print("Межстраничная реклама готова")
    elseif event == "closed" then
        Print("Реклама закрыта, загружаем следующую")
        Ads.LoadInterstitial()
    end
end)

-- Показать между уровнями:
function OnLevelComplete()
    if Ads.IsInterstitialReady() then
        Ads.ShowInterstitial()
    end
end
```

---

### 43.6 Наградная реклама (Rewarded)

```lua
Ads.SetRewardedUnitId(unitId)          -- Установить ID рекламного блока
Ads.SetRewardedUserId(userId)          -- Серверная верификация: ваш ID пользователя
Ads.SetRewardedCustomData(customData)  -- Серверная верификация: произвольные данные
Ads.LoadRewarded()                      -- Предзагрузить наградную рекламу
Ads.ShowRewarded()                      -- Показать наградную рекламу
Ads.IsRewardedReady() -> bool           -- Проверить, загружена ли реклама
```

```lua
Ads.SetRewardedUnitId("ca-app-pub-XXXXX/WWWWW")
Ads.LoadRewarded()

Ads.OnRewardEarned(function(rewardType, rewardAmount)
    Print("Награда получена: " .. rewardType .. " x" .. rewardAmount)
    AddCoins(rewardAmount)
end)

Ads.OnRewardedEvent(function(event)
    if event == "closed" then
        Ads.LoadRewarded() -- Предзагрузить следующую
    end
end)

-- Кнопка «Посмотреть рекламу за 50 монет»:
function OnWatchAdButton()
    if Ads.IsRewardedReady() then
        Ads.ShowRewarded()
    else
        Print("Реклама ещё не готова")
    end
end
```

Как и межстраничная, наградная реклама одноразовая: `Ads.IsRewardedReady()` становится `false`
сразу при вызове `Ads.ShowRewarded()`, поэтому следующую предзагружайте по событию `closed`.

**Серверная верификация (SSV).** `Ads.OnRewardEarned` срабатывает на устройстве, то есть
модифицированный клиент может её подделать. Для всего, что стоит вам реальных денег — валюта в
донатной экономике, премиум-разблокировки — включите SSV в консоли AdMob для наградного блока,
укажите адрес своего сервера и помечайте каждый показ:

```lua
Ads.SetRewardedUserId(myAccountId)                   -- придёт как user_id в SSV-колбэке
Ads.SetRewardedCustomData("quest=daily_bonus")       -- придёт как custom_data
Ads.LoadRewarded()
```

Оба значения прикрепляются к **следующей** загружаемой рекламе, поэтому выставляйте их до
`Ads.LoadRewarded()`. Награду выдавайте на сервере, когда придёт проверенный колбэк от Google, а
клиентский `Ads.OnRewardEarned` считайте только подсказкой для интерфейса. Если оба значения не
заданы, поведение прежнее — опции SSV не прикрепляются.

---

### 43.7 Доход от рекламы (по каждому показу)

```lua
Ads.OnPaidEvent(function(info)
    -- info.format      "banner" | "interstitial" | "rewarded"
    -- info.valueMicros целое, 1 000 000 микро = 1.0 в валюте info.currency
    -- info.unitId      рекламный блок, который принёс доход
    -- info.value       та же сумма числом с плавающей точкой
    -- info.currency    код ISO-4217, например "USD"
    -- info.precision   "estimated" | "publisher_provided" | "precise" | "unknown"
    Analytics.TrackRevenue(info.value, info.currency)
end)
```

Срабатывает один раз на каждый оплаченный показ на обеих платформах. Именно это скармливают в
отчёты ROAS/LTV; другого способа привязать рекламный доход к конкретному игроку нет.

---

### 43.8 Ads.Destroy

```lua
Ads.Destroy()
```

Освобождает все рекламные ресурсы и очищает колбэки.

---

### 43.9 Сводка колбэков

| Колбэк | Параметры | Описание |
|--------|-----------|----------|
| `Ads.OnInitialized(fn)` | `(success: bool)` | AdMob SDK инициализирован |
| `Ads.OnBannerEvent(fn)` | `(event: string)` | События баннера: `loaded`, `failed:CODE`, `failed:no_unit_id`, `failed:not_initialized`, `impression`, `clicked`, `opened`, `closed` |
| `Ads.OnInterstitialEvent(fn)` | `(event: string)` | События межстраничной: `loaded`, `failed:CODE`, `failed:no_unit_id`, `failed:not_initialized`, `shown`, `impression`, `clicked`, `show_failed:CODE`, `closed`, `not_ready` |
| `Ads.OnRewardedEvent(fn)` | `(event: string)` | События наградной: `loaded`, `failed:CODE`, `failed:no_unit_id`, `failed:not_initialized`, `shown`, `impression`, `clicked`, `show_failed:CODE`, `earned`, `closed`, `not_ready` |
| `Ads.OnRewardEarned(fn)` | `(type: string, amount: int)` | Пользователь получил награду |
| `Ads.OnPaidEvent(fn)` | `(info: table)` | Доход по каждому показу — см. 43.7 |
| `Ads.ClearCallbacks()` | — | Удалить все колбэки |

---

### 43.10 Рекламный идентификатор, атрибуция и промо в App Store

Помимо показа рекламы обе платформы дают **рекламный идентификатор**, а iOS вдобавок —
собственные фреймворки атрибуции и промо в App Store. Всё это работает **вообще без
сторонних SDK**.

```lua
-- Рекламный идентификатор (Android: GAID, iOS: IDFA) -- сначала всегда спросите согласие
Consent.ShowForm()                       -- iOS: запрос ATT; Android: форма UMP
Ads.OnAdvertisingId(function(id, limited)
    print("ad id:", id, "ограничение трекинга:", limited)
end)
Ads.RequestAdvertisingId()               -- асинхронно на Android, сразу на iOS
local id = Ads.GetAdvertisingId()        -- "" пока не получен / если пользователь отказался
```

| Функция | Описание |
|---|---|
| `Ads.RequestAdvertisingId()` | Запрашивает идентификатор и вызывает `Ads.OnAdvertisingId(id, limitAdTracking)` |
| `Ads.GetAdvertisingId()` | Последний известный идентификатор или `""`, если недоступен либо пользователь отказался |
| `Ads.IsLimitAdTrackingEnabled()` | `true`, если пользователь запретил трекинг (на iOS — ATT не разрешён) |

> **Требование при сборке iOS:** включите **Реклама и атрибуция** в Build Game → iOS. Это
> подключает `AdSupport` (IDFA) и `AdServices` (атрибуция Apple Search Ads). Если оставить
> выключенным, `AdSupport` не линкуется, `ASIdentifierManager` не резолвится в рантайме, и
> `Ads.GetAdvertisingId()` возвращает `""`.
>
> **На вопрос про IDFA в App Store Connect отвечайте по тому, что реально делает ваша сборка**,
> а не по одному этому переключателю. `AppTrackingTransparency` линкуется в *каждую* iOS-сборку,
> потому что `Consent.ShowForm()` и `Permissions.Request("TRACKING")` входят в публичный API
> всегда. Если игра вызывает любой из них или показывает рекламу — она использует IDFA, и
> честный ответ «Да».
>
> **Строка назначения ATT:** iOS-сборка объявляет `NSUserTrackingUsageDescription`
> автоматически, потому что iOS завершает приложение, вызвавшее `requestTrackingAuthorization`
> без неё. Текст по умолчанию намеренно обобщённый — замените его на свой, добавив
> `NSUserTrackingUsageDescription=<ваша причина>` в Build Game → iOS → *Extra Usage
> Descriptions*.

**Только Apple — атрибуция и промо в сторе** (`Ads.IsAttributionSupported()` и
`Ads.IsStorePromotionSupported()` возвращают `false` на Android, вызовы там ничего не делают):

| Функция | Описание |
|---|---|
| `Ads.UpdateConversionValue(value)` | Точное значение конверсии SKAdNetwork `0..63` |
| `Ads.UpdatePostbackConversionValue(fine, coarse)` | Точное значение плюс грубая корзина — `"low"` / `"medium"` / `"high"` (iOS 16.1+; ниже откатывается только к точному значению) |
| `Ads.GetAttributionToken()` | Токен атрибуции Apple Search Ads — отправьте его POST-ом на `https://api-adservices.apple.com/api/v1/` со своего сервера, чтобы определить кампанию |
| `Ads.ShowStoreOverlay(appStoreId)` | Показывает карточку App Store через `SKOverlay` — родная площадка Apple для кросс-промо, рекламный SDK не нужен |
| `Ads.DismissStoreOverlay()` | Закрывает её |
| `Ads.ShowStoreProduct(appStoreId)` | Полная страница приложения в App Store (`SKStoreProductViewController`) прямо в игре |

```lua
if Ads.IsAttributionSupported() then
    Ads.UpdatePostbackConversionValue(12, "medium")   -- после значимого события после установки
end

if Ads.IsStorePromotionSupported() then
    Ads.OnStorePromotionEvent(function(event, message, ok)
        print(event, message, ok)   -- overlay_shown / overlay_dismissed / product_shown / ..._failed
    end)
    Ads.ShowStoreOverlay("1234567890")                -- прорекламируйте свою другую игру
end
```


## 44. IAP — Внутриигровые покупки (Google Play Billing)

### Обзор

Таблица **`IAP`** предоставляет Lua API для **внутриигровых покупок** и **подписок** через Google Play Billing.
Поддерживаются разовые покупки (расходуемые — монеты/гемы, нерасходуемые — «убрать рекламу») и подписки (боевой пропуск, VIP).

> **Платформа:** Android (Google Play Billing) и iOS (StoreKit). На других платформах `IAP.IsSupported()` возвращает `false`, а все вызовы ничего не делают.
>
> **Про разные сторы:** ID товаров настраиваются отдельно для каждого стора — Google Play Console для Android, App Store Connect для iOS. На iOS `IAP.Consume()` и `IAP.Acknowledge()` ничего не делают, но всё равно присылают свои события (StoreKit финализирует транзакции сам); выдавайте товар в `IAP.OnPurchaseComplete`.
>
> **Одна схема товара на оба стора:** `IAP.OnProductsQueried` отдаёт одинаковые имена полей на
> Android и iOS (`id`, `name`, `title`, `description`, `price`, `priceMicros`, `currency`,
> `billingPeriod`, `type`), поэтому экран магазина, написанный один раз, работает на обеих
> платформах.
>
> **Требование при сборке:** включите «Google Play Billing (IAP)» в окне Build Game.

---

### 44.1 IAP.IsSupported

```lua
IAP.IsSupported() -> bool
```

Возвращает `true`, если текущая платформа поддерживает внутриигровые покупки.

---

### 44.2 IAP.Init

```lua
IAP.Init()
```

Подключается к стору. Должна быть вызвана один раз перед другими функциями `IAP`.

```lua
IAP.Init()
IAP.OnInitialized(function(success)
    if success then
        Print("Сервис покупок подключён")
        IAP.QueryProducts({"coins_100", "coins_500", "remove_ads"}, false)
    end
end)
```

На Android соединение **самовосстанавливающееся**: если Google Play отключит сервис биллинга,
придёт `IAP.OnEvent("disconnected")`, а движок сам переподключится с экспоненциальной задержкой
(1 с → 60 с). Любой вызов, сделанный в отключённом состоянии, завершается явной ошибкой, а не
исчезает молча — вы всегда получите результат покупки `failed:service_unavailable`, пустой
`IAP.OnProductsQueried` либо событие `consume_failed:service_unavailable` /
`acknowledge_failed:service_unavailable`, — так что экран магазина не может зависнуть на
крутилке навсегда.

Движок также **сам сверяет покупки**: при каждом успешном подключении и каждом возврате
приложения на передний план он перезапрашивает Google Play и повторно отдаёт любую покупку в
состоянии `PURCHASED`, которая так и не была подтверждена, обычным
`IAP.OnPurchaseComplete{status="purchased"}`. Именно это ловит покупку, завершённую при закрытой
игре, отложенный платёж (медленная карта / наличные), который прошёл позже, или промокод,
активированный в приложении Play Store. **Делайте выдачу товара идемпотентной.**

---

### 44.3 IAP.IsConnected

```lua
IAP.IsConnected() -> bool
```

Возвращает `true`, если стор доступен: соединение Play Billing на Android, `canMakePayments` на
iOS.

---

### 44.3a IAP.SetUserId

```lua
IAP.SetUserId(userId)
```

Привязывает все последующие покупки к вашему ID аккаунта. Google Play получает его как
*obfuscated account ID* (движок отправляет SHA-256-дайджест, а не исходное значение), StoreKit —
как `applicationUsername`. Оба стора используют это для антифрода, а вашему серверу это позволяет
сопоставить чек с игроком. Выставляйте, как только знаете, кто играет, — до `IAP.Purchase`.

---

### 44.4 IAP.QueryProducts

```lua
IAP.QueryProducts(productIds, isSubscription?)
```

Запрашивает информацию о продуктах из Google Play. Результаты приходят через `IAP.OnProductsQueried`.

| Параметр | Тип | Описание |
|----------|-----|----------|
| `productIds` | `table` | Массив строк — ID продуктов |
| `isSubscription` | `bool` | `true` для подписок, `false` для разовых покупок (по умолчанию `false`) |

```lua
-- Разовые покупки
IAP.QueryProducts({"coins_100", "coins_500", "remove_ads"}, false)

-- Подписки
IAP.QueryProducts({"vip_monthly", "battle_pass"}, true)

IAP.OnProductsQueried(function(products)
    for _, p in ipairs(products) do
        Print(p.id .. " — " .. p.name .. " — " .. p.price)
    end
end)
```

**Поля продукта** — одинаковые на Android и iOS:

| Поле | Тип | Описание |
|------|-----|----------|
| `id` | `string` | ID продукта |
| `name` | `string` | Название продукта |
| `title` | `string` | Заголовок продукта |
| `description` | `string` | Описание продукта |
| `price` | `string` | Форматированная цена в локали стора (напр. "$0.99") |
| `priceMicros` | `int` | Цена в микро (990000 = $0.99) |
| `currency` | `string` | Код валюты ("USD", "EUR", "RUB") |
| `billingPeriod` | `string` | Период подписки ("P1M" = ежемесячно), `""` для разовых товаров |
| `type` | `string` | `"inapp"` или `"subs"` |

`productId`, `priceAmountMicros` и `priceCurrencyCode` также присутствуют как псевдонимы `id`,
`priceMicros` и `currency`.

**Подписки на Android** дополнительно приносят каталог предложений, потому что у одного
подписочного товара может быть несколько базовых планов и вводных/пробных предложений:

| Поле | Тип | Описание |
|------|-----|----------|
| `offerToken` | `string` | Токен предложения по умолчанию (первого) — его использует `IAP.Purchase`, если ничего не передать |
| `offers` | `table` | Массив `{ offerToken, basePlanId, offerId, phases }` |
| `offers[i].phases` | `table` | Массив `{ price, priceMicros, currency, billingPeriod, billingCycleCount, recurrenceMode }` — бесплатный пробный период это фаза с `priceMicros == 0` |

Верхнеуровневые `price` / `billingPeriod` подписки описывают **финальную регулярную фазу**, а не
вводное предложение, поэтому план «0 ₽ 7 дней, далее 499 ₽/мес» покажет в магазине 499 ₽/мес.

`IAP.OnProductsQueried` вызывается всегда — с пустой таблицей, если запрос не удался, — так что
экран магазина не зависает. При неудаче дополнительно приходит
`IAP.OnEvent("query_failed:CODE")`.

---

### 44.5 IAP.Purchase

```lua
IAP.Purchase(productId, offerToken?)
```

Запускает процесс покупки в сторе для указанного продукта. `offerToken` — только для Android,
выбирает конкретное подписочное предложение из `product.offers`; не передавайте его, чтобы
использовать предложение по умолчанию. На iOS параметр игнорируется — там вводными предложениями
управляет App Store Connect.

```lua
IAP.Purchase("coins_100")

IAP.OnPurchaseComplete(function(info)
    if info.status == "purchased" then
        Print("Куплено: " .. info.productId)
        -- Для расходуемых предметов — потребить, чтобы можно было купить снова:
        IAP.Consume(info.token)
        AddCoins(100)
    elseif info.status == "cancelled" then
        Print("Покупка отменена")
    elseif info.status == "pending" then
        Print("Покупка ожидает подтверждения")   -- ничего пока НЕ выдавать
    else
        Print("Ошибка покупки: " .. info.status .. " " .. info.message)
    end
end)
```

Когда пользователь закрывает окно оплаты, на **обеих** платформах приходит
`status == "cancelled"` — не показывайте на это ошибку.

---

### 44.6 IAP.Consume и IAP.Acknowledge

```lua
IAP.Consume(purchaseToken)
IAP.Acknowledge(purchaseToken)
```

`IAP.Consume` потребляет покупку, позволяя пользователю купить тот же продукт снова. Используйте
для расходуемых предметов (монеты, гемы, жизни) и вызывайте **после** того, как выдали товар.
Нерасходуемые предметы («убрать рекламу») потреблять нельзя.

`IAP.Acknowledge` явно подтверждает покупку. Движок и так подтверждает каждую увиденную покупку
в состоянии `PURCHASED`, включая найденные через `IAP.RestorePurchases()`, поэтому эта функция
нужна редко — она существует для сценариев, где подтверждение делается только после проверки чека
на сервере. Подтверждать дважды безопасно: движок отбрасывает дубли запросов «в полёте».

> Google **автоматически возвращает деньги за любую покупку, не подтверждённую в течение трёх
> суток**. Именно поэтому восстановление и сверка не только сообщают о покупках, но и подтверждают их.

---

### 44.7 IAP.RestorePurchases

```lua
IAP.RestorePurchases()
```

Восстанавливает ранее купленные нерасходуемые предметы и активные подписки, а также подтверждает
те из них, которые стор всё ещё считает неподтверждёнными. Результаты приходят через
`IAP.OnPurchaseRestored`. Вызывайте при старте (и по кнопке «Восстановить покупки», которой
требует Apple), чтобы вернуть права после переустановки или на новом устройстве.

```lua
IAP.RestorePurchases()

IAP.OnPurchaseRestored(function(info)
    if info.status == "restored" then
        Print("Восстановлено: " .. info.productId)
        UnlockContent(info.productId)
    elseif info.status == "subscription_active" then
        Print("Активная подписка: " .. info.productId)
        EnableVIP()
    end
end)
```

Android отдаёт разовые товары как `restored`, а подписки как `subscription_active`, затем
`IAP.OnEvent("restore_complete")` и `IAP.OnEvent("restore_subs_complete")`. iOS отдаёт всё как
`restored`, а затем `IAP.OnEvent("restore_complete")`.

---

### 44.7a IAP.QueryPurchases

```lua
IAP.QueryPurchases()
```

Немедленно запускает ту самую сверку из [44.2](#442-iapinit), но без событий `restored`. Полезно
после возврата из внешнего сценария. Сверка и так выполняется при подключении и при возврате
приложения на передний план, поэтому большинству игр вызывать её не нужно.

---

### 44.7b Проверка чеков на сервере

Для донатной экономики проверяйте покупку на своём сервере, прежде чем выдавать что-то ценное:
`IAP.OnPurchaseComplete` выполняется на устройстве, которое контролирует игрок. Каждый колбэк
покупки несёт всё, что нужно бэкенду:

| Поле | Платформа | Описание |
|------|-----------|----------|
| `token` | обе | Purchase token на Android / идентификатор транзакции на iOS |
| `orderId` | обе | Order ID Google Play / original transaction identifier на iOS |
| `signature` | Android | RSA-подпись `originalJson`, проверяется вашим публичным ключом из Play Console |
| `originalJson` | Android | Ровно та строка, которую подписывает `signature` — отправляйте обе как есть, не пересериализуя |
| `receipt` | iOS | Чек App Store в base-64 (доступен также в любой момент через `IAP.GetReceipt()`) |
| `purchaseTime` | обе | Unix-время в миллисекундах |
| `quantity` | обе | Количество купленных единиц |
| `acknowledged` | Android | Есть ли у Google Play подтверждение |
| `autoRenewing` | Android | Состояние автопродления подписки |
| `success` | обе | `false` для отменённых и неуспешных результатов |
| `message` | обе | Человекочитаемая причина ошибки |

`status`, `productId`, `success`, `quantity`, `acknowledged` и `autoRenewing` присутствуют
всегда. Строковые поля и `purchaseTime` **отсутствуют (`nil`)**, если платформа или статус их не
даёт: `signature`/`originalJson` никогда не приходят на iOS, `receipt` — на Android, а
`token`/`orderId` отсутствуют у результата `cancelled` или `failed`. Проверяйте их через
`if info.signature then`, а не сравнением с `""`.

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
    SendToVerificationBackend(Network.JsonEncode(payload))   -- ваш собственный транспорт
end)
```

---

### 44.8 IAP.Destroy

```lua
IAP.Destroy()
```

Отключается от стора, останавливает цикл переподключения и очищает колбэки.

---

### 44.9 Сводка колбэков

| Колбэк | Параметры | Описание |
|--------|-----------|----------|
| `IAP.OnInitialized(fn)` | `(success: bool)` | Стор подключён/ошибка |
| `IAP.OnEvent(fn)` | `(event: string)` | Общие события: `disconnected`, `consumed:TOKEN`, `consume_failed:CODE`, `acknowledged`, `acknowledge_failed:CODE`, `query_failed:CODE`, `restore_complete`, `restore_subs_complete`, `store_promotion:PRODUCT_ID` (iOS) |
| `IAP.OnProductsQueried(fn)` | `(products: table)` | Информация о продуктах получена (пустая таблица при ошибке) |
| `IAP.OnPurchaseComplete(fn)` | `(info: table)` | Результат покупки — статусы `purchased`, `pending`, `cancelled`, `failed:CODE` |
| `IAP.OnPurchaseRestored(fn)` | `(info: table)` | Восстановленная покупка — статусы `restored`, `subscription_active` |
| `IAP.ClearCallbacks()` | — | Удалить все колбэки |

Обе таблицы `info` содержат полный набор полей из [44.7b](#447b-проверка-чеков-на-сервере).

> **Промо-покупки в App Store (iOS).** Когда игрок покупает ваш товар прямо со страницы
> приложения в App Store, StoreKit передаёт платёж запущенной игре. Движок принимает его и
> сначала поднимает `IAP.OnEvent("store_promotion:PRODUCT_ID")`, а затем обычный
> `IAP.OnPurchaseComplete`, когда транзакция завершится.

---

### 44.10 Практический пример — магазин в стиле Clash Royale

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

### Обзор

Таблица **`PlayGames`** предоставляет Lua API для **Google Play Games Services** —
авторизация, достижения и таблицы лидеров.

> **Платформа:** Android (Google Play Games) и iOS (Game Center, через GameKit). На других платформах `PlayGames.IsSupported()` возвращает `false`, а все вызовы ничего не делают.
>
> **Про разные сторы:** на iOS таблица `PlayGames` отображается на Game Center. ID достижений и таблиц лидеров настраиваются отдельно — Google Play Console для Android, App Store Connect / Game Center для iOS. На iOS `IsSupported()` сообщает, подписано ли приложение с entitlement `com.apple.developer.game-center` (в симуляторе и для неподписанных сборок, где нет provisioning-профиля, вернётся `true`).
>
> **Шаги достижений на iOS:** Game Center хранит прогресс как проценты 0–100, а не как дискретные шаги. Поэтому `IncrementAchievement(id, steps)` прибавляет `steps` процентных пунктов к текущему прогрессу (с ограничением до 100), храня значение локально и обновляя его из Game Center при входе.
>
> **Требование при сборке:** включите «Google Play Games» в окне Build Game и укажите Play Games App ID.

---

### 45.1 PlayGames.IsSupported

```lua
PlayGames.IsSupported() -> bool
```

Возвращает `true`, если текущая платформа поддерживает Google Play Games.

---

### 45.2 PlayGames.Init

```lua
PlayGames.Init()
PlayGames.IsInitialized() -> bool
```

Инициализирует Play Games SDK и пытается выполнить автоматическую авторизацию.
`PlayGames.IsInitialized()` сообщает, завершилась ли инициализация SDK — в отличие от
`PlayGames.IsSupported()`, который говорит лишь о том, что платформа *способна* его запустить.

```lua
PlayGames.Init()

PlayGames.OnSignIn(function(success, message)
    if success then
        Print("Авторизован в Google Play Games!")
    else
        Print("Ошибка авторизации: " .. message)
    end
end)

PlayGames.OnPlayerInfo(function(playerId, playerName)
    Print("Игрок: " .. playerName .. " (" .. playerId .. ")")
end)
```

---

### 45.3 Авторизация

```lua
PlayGames.SignIn()                  -- Запустить ручную авторизацию
PlayGames.IsSignedIn() -> bool      -- Проверить статус авторизации
PlayGames.GetPlayerId() -> string   -- Получить ID авторизованного игрока
PlayGames.GetPlayerName() -> string -- Получить отображаемое имя игрока
```

---

### 45.4 Достижения

```lua
PlayGames.UnlockAchievement(achievementId)             -- Разблокировать достижение
PlayGames.IncrementAchievement(achievementId, steps)   -- Увеличить прогресс
PlayGames.RevealAchievement(achievementId)             -- Раскрыть скрытое достижение
PlayGames.ShowAchievements()                           -- Показать UI достижений
```

```lua
-- Игрок убил 100 врагов
enemyKills = enemyKills + 1
if enemyKills >= 10 then
    PlayGames.UnlockAchievement("CgkI...AQ") -- «Первая кровь»
end
PlayGames.IncrementAchievement("CgkI...BQ", 1) -- «Убийца» (инкрементальное)

-- Показать все достижения
function OnAchievementsButton()
    PlayGames.ShowAchievements()
end
```

---

### 45.5 Таблицы лидеров

```lua
PlayGames.SubmitScore(leaderboardId, score)    -- Отправить очки (поддерживает большие значения)
PlayGames.ShowLeaderboard(leaderboardId)       -- Показать конкретную таблицу
PlayGames.ShowAllLeaderboards()                -- Показать все таблицы лидеров
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

Освобождает ресурсы Play Games и очищает колбэки.

---

### 45.7 Сводка колбэков

| Колбэк | Параметры | Описание |
|--------|-----------|----------|
| `PlayGames.OnSignIn(fn)` | `(success: bool, message: string)` | Результат авторизации |
| `PlayGames.OnPlayerInfo(fn)` | `(playerId: string, playerName: string)` | Информация об игроке после авторизации |
| `PlayGames.OnEvent(fn)` | `(event: string)` | Общие события: `achievement_unlocked:ID`, `score_submitted:ID` |
| `PlayGames.ClearCallbacks()` | — | Удалить все колбэки |

---

### 45.8 Практический пример — интеграция в стиле Subway Surfers

```lua
local highScore = 0
local runCount = 0

function OnCreate()
    if PlayGames.IsSupported() then
        PlayGames.Init()
        PlayGames.OnSignIn(function(success, msg)
            if success then
                Print("С возвращением, " .. PlayGames.GetPlayerName() .. "!")
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
    -- Смотришь рекламу — продолжаешь забег (как в Subway Surfers)
    if Ads.IsRewardedReady() then
        Ads.ShowRewarded()
    else
        Print("Реклама ещё не готова")
    end
end
```

---

## 46. SavedGames — Облачные сохранения (Google Play)

### Обзор

Таблица **`SavedGames`** предоставляет Lua API для **облачных сохранений** через **Google Play Games Services Saved Games**.
Позволяет сохранять и загружать игровые данные в облако, удалять сохранения и показывать встроенный UI сохранений.

> **Платформа:** Android (Google Play Saved Games) и iOS (GameKit / iCloud). На других платформах `SavedGames.IsSupported()` возвращает `false`, а все вызовы ничего не делают.
>
> **Про iOS:** сохранения идут через GameKit, поэтому игрок должен быть авторизован в Game Center — иначе `SavedGames.Init()` присылает `OnError("", "not_signed_in")`. У `SavedGames.ShowUI()` нет нативного аналога на iOS: он присылает `OnError("", "no_system_saved_games_ui_on_ios")`, свой выбор слота нужно рисовать самим.
>
> **Требование при сборке:** включите «Google Play Games» с поддержкой Saved Games в окне Build Game.

---

### 46.1 SavedGames.IsSupported

```lua
SavedGames.IsSupported() -> bool
```

Возвращает `true`, если текущая платформа поддерживает облачные сохранения (Android с включённым Saved Games).

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

Инициализирует сервис облачных сохранений. Должна быть вызвана один раз перед другими функциями `SavedGames`.

---

### 46.3 SavedGames.Save

```lua
SavedGames.Save(slotName, data, description?)
```

Сохраняет данные в именованный слот в облаке.

| Параметр | Тип | Описание |
|----------|-----|----------|
| `slotName` | `string` | Уникальное имя слота (напр. `"save_1"`) |
| `data` | `string` | Данные для сохранения (JSON-строка, сериализованное состояние и т.д.) |
| `description` | `string` | Необязательное описание, отображаемое в UI (по умолчанию: `""`) |

```lua
local saveData = '{"level":5,"coins":1200,"hp":80}'
SavedGames.Save("save_main", saveData, "Уровень 5 — 1200 монет")

SavedGames.OnSaved(function(slotName)
    Print("Сохранено в облако: " .. slotName)
end)
```

---

### 46.4 SavedGames.Load

```lua
SavedGames.Load(slotName)
```

Загружает данные из именованного слота. Результат приходит через `SavedGames.OnLoaded`.

```lua
SavedGames.Load("save_main")

SavedGames.OnLoaded(function(slotName, data)
    Print("Загружено из " .. slotName .. ": " .. data)
    local save = ParseJson(data)
    RestoreGameState(save)
end)
```

---

### 46.5 SavedGames.Delete

```lua
SavedGames.Delete(slotName)
```

Удаляет слот сохранения из облака.

```lua
SavedGames.Delete("save_main")

SavedGames.OnDeleted(function(slotName)
    Print("Удалено: " .. slotName)
end)
```

---

### 46.6 SavedGames.ShowUI

```lua
SavedGames.ShowUI()
```

Открывает встроенный UI Google Play Saved Games, где игрок может просматривать и управлять своими сохранениями.

---

### 46.7 SavedGames.Destroy

```lua
SavedGames.Destroy()
```

Освобождает ресурсы Saved Games и очищает все колбэки.

---

### 46.8 Сводка колбэков

| Колбэк | Параметры | Описание |
|--------|-----------|----------|
| `SavedGames.OnLoaded(fn)` | `(slotName: string, data: string)` | Данные загружены из слота |
| `SavedGames.OnSaved(fn)` | `(slotName: string)` | Данные сохранены в слот |
| `SavedGames.OnDeleted(fn)` | `(slotName: string)` | Слот удалён |
| `SavedGames.OnError(fn)` | `(slotName: string, message: string)` | Произошла ошибка |
| `SavedGames.ClearCallbacks()` | — | Удалить все колбэки |

---

### 46.9 Практический пример — облачное сохранение RPG

```lua
function OnCreate()
    if not SavedGames.IsSupported() then return end

    SavedGames.Init()

    SavedGames.OnLoaded(function(slot, data)
        local save = ParseJson(data)
        playerLevel = save.level or 1
        playerCoins = save.coins or 0
        Print("Облачное сохранение загружено: Уровень " .. playerLevel)
    end)

    SavedGames.OnSaved(function(slot)
        Print("Прогресс сохранён в облако!")
    end)

    SavedGames.OnError(function(slot, msg)
        Print("Ошибка SavedGames [" .. slot .. "]: " .. msg)
    end)

    -- Загрузить существующее сохранение при запуске
    SavedGames.Load("rpg_save")
end

function SaveProgress()
    local data = '{"level":' .. playerLevel .. ',"coins":' .. playerCoins .. '}'
    SavedGames.Save("rpg_save", data, "Уровень " .. playerLevel)
end
```

---

## 47. Firebase — Firebase Analytics

### Обзор

Таблица **`Firebase`** предоставляет Lua API для **Firebase Analytics** — логирование событий, пользовательские свойства и отслеживание экранов.

> **Платформа:** Android. Для iOS движок не поставляет Firebase SDK, поэтому там `Firebase.IsSupported()` возвращает `false`, а все вызовы ничего не делают. На десктопе и в Web — тоже `false`.
>
> **Требование при сборке:** включите «Firebase» в окне Build Game и поместите `google-services.json` в проект.

---

### 47.1 Firebase.IsSupported

```lua
Firebase.IsSupported() -> bool
```

Возвращает `true`, если текущая платформа поддерживает Firebase (Android с включённым Firebase).

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

Инициализирует Firebase SDK. Должна быть вызвана один раз перед другими функциями `Firebase`.

---

### 47.3 Firebase.LogEvent

```lua
Firebase.LogEvent(name, params?)
```

Логирует аналитическое событие с необязательными параметрами.

| Параметр | Тип | Описание |
|----------|-----|----------|
| `name` | `string` | Имя события (напр. `"level_complete"`) |
| `params` | `table` | Необязательная таблица пар ключ-значение |

Значения параметров могут быть `string`, `int`, `double` или `bool`.

```lua
-- Простое событие
Firebase.LogEvent("game_start")

-- Событие с параметрами
Firebase.LogEvent("level_complete", {
    level = 5,
    score = 12500,
    time_seconds = 45.3,
    used_hint = false
})

-- Отслеживание покупки
Firebase.LogEvent("purchase", {
    item_id = "sword_of_fire",
    item_name = "Меч огня",
    price = 2.99,
    currency = "USD"
})
```

---

### 47.4 Firebase.SetUserId

```lua
Firebase.SetUserId(userId)
```

Устанавливает ID пользователя для аналитики. Полезно для кросс-девайсного отслеживания.

```lua
Firebase.SetUserId("player_12345")
```

---

### 47.5 Firebase.SetUserProperty

```lua
Firebase.SetUserProperty(name, value)
```

Устанавливает пользовательское свойство.

```lua
Firebase.SetUserProperty("favorite_class", "warrior")
Firebase.SetUserProperty("vip_status", "gold")
```

---

### 47.6 Firebase.SetScreenName

```lua
Firebase.SetScreenName(screenName)
```

Устанавливает имя текущего экрана для отслеживания просмотров.

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

Освобождает ресурсы Firebase.

---

### 47.8 Справка по API

| Функция | Возвращает | Описание |
|---------|-----------|----------|
| `Firebase.IsSupported()` | `bool` | Проверить доступность Firebase |
| `Firebase.Init()` | — | Инициализировать Firebase SDK |
| `Firebase.LogEvent(name, params?)` | — | Залогировать аналитическое событие |
| `Firebase.SetUserId(userId)` | — | Установить ID пользователя |
| `Firebase.SetUserProperty(name, value)` | — | Установить свойство пользователя |
| `Firebase.SetScreenName(screenName)` | — | Установить имя текущего экрана |
| `Firebase.Destroy()` | — | Освободить ресурсы Firebase |

---

### 47.9 Практический пример — игровая аналитика

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

## 48. Notifications — Push- и локальные уведомления

### Обзор

Таблица **`Notifications`** предоставляет Lua API для **локальных уведомлений** и получения **push-токенов** через Firebase Cloud Messaging (FCM).
Планируйте локальные уведомления с задержкой, отменяйте их и запрашивайте разрешение на уведомления на Android 13+.

> **Платформа:** Android и iOS (локальные уведомления через UserNotifications). На других платформах `Notifications.IsSupported()` возвращает `false`, а все вызовы ничего не делают.
>
> **Про iOS:** локальные уведомления поддерживаются полностью, включая `OnShown` (показ поверх работающей игры) и `OnClicked`. `Notifications.GetToken()` возвращает APNs-токен устройства в виде hex-строки после того, как `Notifications.Init()` зарегистрирует приложение для remote-уведомлений — это происходит только если приложение подписано с entitlement `aps-environment` (Build Game → iOS → *Push Notifications*); без него токен останется `""`. Учтите, что `delaySec = 0` на iOS срабатывает сразу, а не округляется до секунды.
>
> **Требование при сборке:** включите «Notifications» в окне Build Game.

---

### 48.1 Notifications.IsSupported

```lua
Notifications.IsSupported() -> bool
```

Возвращает `true`, если текущая платформа поддерживает уведомления.

---

### 48.2 Notifications.Init

```lua
Notifications.Init()
```

Инициализирует систему уведомлений. Должна быть вызвана один раз перед другими функциями `Notifications`.

---

### 48.3 Notifications.RequestPermission

```lua
Notifications.RequestPermission()
```

Запрашивает разрешение `POST_NOTIFICATIONS` (обязательно на Android 13+).
Результат приходит через `Notifications.OnPermissionResult`.

```lua
Notifications.RequestPermission()

Notifications.OnPermissionResult(function(granted)
    if granted then
        Print("Разрешение на уведомления получено!")
    else
        Print("Разрешение на уведомления отклонено")
    end
end)
```

---

### 48.4 Notifications.ShowLocal

```lua
Notifications.ShowLocal(title, message, delaySec?, id?)
```

Показывает или планирует локальное уведомление.

| Параметр | Тип | Описание |
|----------|-----|----------|
| `title` | `string` | Заголовок уведомления |
| `message` | `string` | Текст уведомления |
| `delaySec` | `int` | Задержка в секундах (по умолчанию: `0` = немедленно) |
| `id` | `int` | ID уведомления для отмены (по умолчанию: `0`) |

```lua
-- Показать немедленно
Notifications.ShowLocal("Игра", "Ваша энергия полностью восстановлена!")

-- Запланировать через 30 минут
Notifications.ShowLocal("Игра", "Возвращайтесь играть!", 1800, 1)

-- Ежедневное напоминание (86400 секунд)
Notifications.ShowLocal("Игра", "Ежедневная награда ждёт вас!", 86400, 2)
```

---

### 48.5 Notifications.CancelLocal

```lua
Notifications.CancelLocal(id)
```

Отменяет запланированное уведомление по его ID.

```lua
Notifications.CancelLocal(1) -- Отменить уведомление с ID 1
```

---

### 48.6 Notifications.CancelAll

```lua
Notifications.CancelAll()
```

Отменяет все запланированные локальные уведомления.

---

### 48.7 Notifications.GetToken

```lua
Notifications.GetToken() -> string
```

Возвращает FCM-токен для push-уведомлений. Возвращает `""`, если токен ещё не получен.

```lua
local token = Notifications.GetToken()
if token ~= "" then
    Print("FCM-токен: " .. token)
end
```

---

### 48.8 Notifications.Destroy

```lua
Notifications.Destroy()
```

Освобождает ресурсы уведомлений и очищает все колбэки.

---

### 48.9 Сводка колбэков

| Колбэк | Параметры | Описание |
|--------|-----------|----------|
| `Notifications.OnPermissionResult(fn)` | `(granted: bool)` | Результат запроса разрешения |
| `Notifications.OnShown(fn)` | `(data: string)` | Уведомление было показано |
| `Notifications.OnClicked(fn)` | `(data: string)` | Пользователь нажал на уведомление |
| `Notifications.OnTokenReceived(fn)` | `(token: string)` | FCM-токен получен/обновлён |
| `Notifications.ClearCallbacks()` | — | Удалить все колбэки |

---

### 48.10 Практический пример — уведомления для вовлечения игроков

```lua
function OnCreate()
    if not Notifications.IsSupported() then return end

    Notifications.Init()
    Notifications.RequestPermission()

    Notifications.OnPermissionResult(function(granted)
        if granted then
            -- Запланировать напоминание «вернись» через 24 часа
            Notifications.ShowLocal("Приключения ждут!", "Ваши герои скучают!", 86400, 100)
        end
    end)

    Notifications.OnClicked(function(data)
        Print("Игрок открыл игру из уведомления")
        GiveLoginBonus()
    end)

    Notifications.OnTokenReceived(function(token)
        Print("FCM-токен: " .. token)
        -- Отправить токен на сервер для push-уведомлений
    end)
end

function OnEnergyFull()
    Notifications.ShowLocal("Энергия полная!", "Ваша энергия полностью восстановлена!", 0, 50)
end
```

---

## 49. Consent — Согласие GDPR (UMP)

### Обзор

Таблица **`Consent`** предоставляет Lua API для **управления согласием GDPR** через **User Messaging Platform (UMP)** от Google.
Показывает формы согласия, проверяет статус и определяет, можно ли показывать рекламу с персонализацией.

> **Платформа:** Android (Google UMP) и iOS (App Tracking Transparency). На iOS `Consent.ShowForm()` показывает системный запрос ATT, а `Consent.GetStatus()` отображает статус ATT на тот же словарь, что и UMP: `"required"` (ещё не спрашивали), `"obtained"` (пользователь ответил), `"not_required"` (ограничено / до iOS 14). У `Consent.Reset()` нет аналога на iOS — он вызывает `OnError` с `"reset_unsupported_on_ios"`. На десктопе и в Web `Consent.IsSupported()` возвращает `false`.
>
> **Требование при сборке:** включите «Consent (UMP)» в окне Build Game.

---

### 49.1 Consent.IsSupported

```lua
Consent.IsSupported() -> bool
```

Возвращает `true`, если текущая платформа поддерживает управление согласием.

---

### 49.2 Consent.Init

```lua
Consent.Init(debugGeography?)
```

Инициализирует UMP SDK и запрашивает информацию о согласии.

| Параметр | Тип | Описание |
|----------|-----|----------|
| `debugGeography` | `bool` | Если `true`, симулирует географию EEA для тестирования (по умолчанию: `false`) |

```lua
Consent.Init()

Consent.OnInfoUpdated(function(status, canShowForm)
    Print("Статус согласия: " .. status)
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

Показывает форму согласия пользователю. Результат приходит через `Consent.OnFormDismissed`.

```lua
Consent.ShowForm()

Consent.OnFormDismissed(function(status)
    Print("Форма закрыта, статус: " .. status)
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

Возвращает текущий статус согласия (напр. `"OBTAINED"`, `"REQUIRED"`, `"NOT_REQUIRED"`, `"UNKNOWN"`).

---

### 49.5 Consent.CanShowAds

```lua
Consent.CanShowAds() -> bool
```

Возвращает `true`, если пользователь дал согласие на показ рекламы (персонализированной или нет).

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

Сбрасывает состояние согласия. Полезно для тестирования или предоставления пользователю возможности изменить выбор.

---

### 49.7 Consent.Destroy

```lua
Consent.Destroy()
```

Освобождает ресурсы UMP и очищает все колбэки.

---

### 49.8 Сводка колбэков

| Колбэк | Параметры | Описание |
|--------|-----------|----------|
| `Consent.OnInfoUpdated(fn)` | `(status: string, canShowForm: bool)` | Информация о согласии обновлена |
| `Consent.OnFormDismissed(fn)` | `(status: string)` | Форма согласия закрыта пользователем |
| `Consent.OnError(fn)` | `(message: string)` | Произошла ошибка |
| `Consent.ClearCallbacks()` | — | Удалить все колбэки |

---

### 49.9 Практический пример — GDPR-совместимая инициализация рекламы

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
            Print("Ошибка согласия: " .. msg)
            -- Запасной вариант: инициализация рекламы без персонализации
            InitAds()
        end)
    else
        -- Не Android, просто инициализируем рекламу
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

## 50. Review — Внутриигровой отзыв

### Обзор

Таблица **`Review`** предоставляет Lua API для **внутриигровых отзывов** через Google Play In-App Review API.
Предлагает пользователю оценить игру, не выходя из приложения.

> **Платформа:** Android (In-App Review) и iOS (`SKStoreReviewController`). На других платформах `Review.IsSupported()` возвращает `false`, а все вызовы ничего не делают.
>
> **Требование при сборке:** включите «In-App Review» в окне Build Game.
>
> **Примечание:** Google контролирует, когда диалог отзыва фактически показывается. Запрос может быть
> проигнорирован, если пользователь уже оставил отзыв или было сделано слишком много запросов.

---

### 50.1 Review.IsSupported

```lua
Review.IsSupported() -> bool
```

Возвращает `true`, если текущая платформа поддерживает внутриигровые отзывы.

---

### 50.2 Review.Request

```lua
Review.Request()
```

Запрашивает показ диалога отзыва. Google может показать или не показать диалог.

```lua
Review.Request()

Review.OnLaunched(function()
    Print("Диалог отзыва показан")
end)

Review.OnCompleted(function()
    Print("Процесс отзыва завершён")
end)

Review.OnError(function(msg)
    Print("Ошибка отзыва: " .. msg)
end)
```

---

### 50.3 Review.Destroy

```lua
Review.Destroy()
```

Очищает все колбэки.

---

### 50.4 Сводка колбэков

| Колбэк | Параметры | Описание |
|--------|-----------|----------|
| `Review.OnLaunched(fn)` | — | Диалог отзыва был показан |
| `Review.OnCompleted(fn)` | — | Процесс отзыва завершён |
| `Review.OnError(fn)` | `(message: string)` | Произошла ошибка |
| `Review.ClearCallbacks()` | — | Удалить все колбэки |

---

### 50.5 Практический пример — запрос отзыва после 10-го уровня

```lua
local hasAskedReview = false

function OnLevelComplete(level)
    if not hasAskedReview and level >= 10 and Review.IsSupported() then
        hasAskedReview = true

        Review.OnCompleted(function()
            Print("Спасибо за отзыв!")
        end)

        Review.Request()
    end
end
```

---

## 51. Bluetooth — Bluetooth-коммуникация

### Обзор

Таблица **`Bluetooth`** предоставляет Lua API для **Bluetooth Classic** коммуникации —
обнаружение устройств, подключение и обмен данными между устройствами.

> **Платформа:** Android (RFCOMM / Bluetooth Classic) и iOS (Bluetooth LE через CoreBluetooth). Lua API одинаковый, транспорт — нет. На iOS хост публикует GATT-сервис, а участники подключаются как central, поэтому `address` — это UUID-строка peripheral/central из CoreBluetooth, а не MAC-адрес; `Bluetooth.RequestEnable()` не может включить радиомодуль — он открывает страницу настроек приложения и присылает событие ошибки. На iOS каждый `Send` обрамляется префиксом длины, поэтому один `Send` всегда приходит как ровно один `OnDataReceived`; на Android поток RFCOMM может дробиться и склеиваться. **Устройства Android и iOS между собой не соединяются.** На десктопе и в Web `Bluetooth.IsSupported()` возвращает `false`.
>
> **Требование при сборке:** включите «Bluetooth» в окне Build Game.
>
> **Разрешения:** требуются разрешения `BLUETOOTH_CONNECT`, `BLUETOOTH_SCAN` и `BLUETOOTH_ADVERTISE` (для хостинга) (запросите через API `Permissions` на Android 12+).

---

### 51.1 Bluetooth.IsSupported

```lua
Bluetooth.IsSupported() -> bool
```

Возвращает `true`, если текущая платформа поддерживает Bluetooth.

---

### 51.2 Bluetooth.Init

```lua
Bluetooth.Init()
```

Инициализирует Bluetooth-адаптер. Должна быть вызвана один раз перед другими функциями `Bluetooth`.

---

### 51.3 Доступность Bluetooth

```lua
Bluetooth.IsAvailable() -> bool    -- Проверить, есть ли Bluetooth-модуль
Bluetooth.IsEnabled() -> bool      -- Проверить, включён ли Bluetooth
Bluetooth.RequestEnable()          -- Запросить у пользователя включение Bluetooth
```

---

### 51.4 Обнаружение устройств

```lua
Bluetooth.StartDiscovery()         -- Начать поиск устройств
Bluetooth.StopDiscovery()          -- Остановить поиск
```

Найденные устройства приходят через `Bluetooth.OnDeviceFound`.

```lua
Bluetooth.StartDiscovery()

Bluetooth.OnDeviceFound(function(deviceName, deviceAddress)
    Print("Найдено: " .. deviceName .. " [" .. deviceAddress .. "]")
end)
```

---

### 51.5 Подключение

```lua
Bluetooth.Connect(address)        -- Подключиться к устройству по MAC-адресу
Bluetooth.Disconnect()            -- Отключить всех пиров
Bluetooth.IsConnected() -> bool   -- Проверить, есть ли активные подключения
```

```lua
Bluetooth.Connect("AA:BB:CC:DD:EE:FF")

Bluetooth.OnConnected(function(deviceName, deviceAddress)
    Print("Подключено к " .. deviceName)
end)

Bluetooth.OnDisconnected(function(deviceName, deviceAddress)
    Print("Отключено от " .. deviceName)
end)
```

---

### 51.6 Мульти-пир хост-режим

Bluetooth поддерживает **мульти-пир** соединения через модель хост/клиент.
Хост открывает серверный сокет и принимает несколько входящих подключений.

```lua
Bluetooth.StartHost()             -- Начать хостинг (принимать входящие подключения)
Bluetooth.StopHost()              -- Остановить хостинг (закрыть серверный сокет)
Bluetooth.IsHosting() -> bool     -- Проверить, активен ли хостинг
```

```lua
-- Устройство-хост
Bluetooth.StartHost()

Bluetooth.OnHostStarted(function()
    Print("Хостинг запущен — ожидание игроков...")
end)

Bluetooth.OnHostStopped(function()
    Print("Хостинг остановлен")
end)

Bluetooth.OnConnected(function(name, address)
    Print("Игрок присоединился: " .. name .. " [" .. address .. "]")
    Print("Всего игроков: " .. Bluetooth.GetPeerCount())
end)
```

---

### 51.7 Обмен данными

```lua
Bluetooth.Send(data)              -- Отправить строку всем подключённым пирам (устаревший алиас)
Bluetooth.SendTo(address, data)   -- Отправить строку конкретному пиру
Bluetooth.SendToAll(data)         -- Отправить строку всем подключённым пирам
```

```lua
-- Отправить всем
Bluetooth.SendToAll("Привет от IceBox!")

-- Отправить конкретному пиру
Bluetooth.SendTo("AA:BB:CC:DD:EE:FF", "Личное сообщение")

Bluetooth.OnDataReceived(function(data)
    Print("Получено: " .. data)
end)
```

---

### 51.8 Управление пирами

```lua
Bluetooth.DisconnectPeer(address)              -- Отключить конкретного пира
Bluetooth.IsPeerConnected(address) -> bool     -- Проверить, подключён ли конкретный пир
Bluetooth.GetPeerCount() -> int                -- Получить количество подключённых пиров
Bluetooth.GetConnectedAddresses() -> string    -- Получить JSON-массив MAC-адресов подключённых пиров
```

```lua
local count = Bluetooth.GetPeerCount()
Print("Подключённых пиров: " .. count)

local json = Bluetooth.GetConnectedAddresses()
Print("Адреса: " .. json)  -- напр. ["AA:BB:CC:DD:EE:FF","11:22:33:44:55:66"]

if Bluetooth.IsPeerConnected("AA:BB:CC:DD:EE:FF") then
    Bluetooth.DisconnectPeer("AA:BB:CC:DD:EE:FF")
end
```

---

### 51.9 Bluetooth.Destroy

```lua
Bluetooth.Destroy()
```

Отключает, освобождает ресурсы Bluetooth и очищает все колбэки.

---

### 51.10 Сводка колбэков

| Колбэк | Параметры | Описание |
|--------|-----------|----------|
| `Bluetooth.OnStateChanged(fn)` | `(state: string)` | Состояние Bluetooth-адаптера изменилось |
| `Bluetooth.OnDeviceFound(fn)` | `(deviceName: string, deviceAddress: string)` | Устройство обнаружено |
| `Bluetooth.OnConnected(fn)` | `(deviceName: string, deviceAddress: string)` | Пир подключён |
| `Bluetooth.OnDisconnected(fn)` | `(deviceName: string, deviceAddress: string)` | Пир отключён |
| `Bluetooth.OnDataReceived(fn)` | `(data: string)` | Данные получены от пира |
| `Bluetooth.OnHostStarted(fn)` | — | Хост-режим успешно запущен |
| `Bluetooth.OnHostStopped(fn)` | — | Хост-режим остановлен |
| `Bluetooth.OnError(fn)` | `(message: string)` | Произошла ошибка |
| `Bluetooth.ClearCallbacks()` | — | Удалить все колбэки |

---

### 51.11 Практический пример — Bluetooth-мультиплеер крестики-нолики (мульти-пир)

```lua
local isHost = false
local peers = 0

function OnCreate()
    if not Bluetooth.IsSupported() then return end

    -- Сначала запросить Bluetooth-разрешения
    Permissions.Request(Permissions.BLUETOOTH_CONNECT)
    Permissions.Request(Permissions.BLUETOOTH_SCAN)
    Permissions.Request(Permissions.BLUETOOTH_ADVERTISE)

    Bluetooth.Init()

    Bluetooth.OnDeviceFound(function(name, address)
        Print("Найдено устройство: " .. name)
        if not isHost then
            Bluetooth.Connect(address)
        end
    end)

    Bluetooth.OnConnected(function(name, address)
        peers = Bluetooth.GetPeerCount()
        Print("Игрок присоединился: " .. name .. " (всего: " .. peers .. ")")
    end)

    Bluetooth.OnDisconnected(function(name, address)
        peers = Bluetooth.GetPeerCount()
        Print("Игрок вышел: " .. name .. " (осталось: " .. peers .. ")")
    end)

    Bluetooth.OnHostStarted(function()
        Print("Хостинг запущен — ожидание игроков...")
    end)

    Bluetooth.OnHostStopped(function()
        Print("Хостинг остановлен")
    end)

    Bluetooth.OnDataReceived(function(data)
        local cell = tonumber(data:match("move:(%d+)"))
        if cell then
            PlaceOpponentMark(cell)
        end
    end)

    Bluetooth.OnError(function(msg)
        Print("Ошибка Bluetooth: " .. msg)
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

## 52. DeepLinks — Глубокие ссылки

### Обзор

Таблица **`DeepLinks`** предоставляет Lua API для **глубоких ссылок** (App Links / Intent URI) —
позволяет открывать игру через пользовательские URL и реагировать на входящие данные.

> **Платформа:** Android (intent-фильтр) и iOS (своя URL-схема). Схема задаётся в Build Game → iOS → *URL-схема диплинков*, регистрируется в `CFBundleURLTypes`, и открытие `yourscheme://...` передаёт полный URI в `OnReceived` (вторым аргументом приходит query-строка). На десктопе и в Web `DeepLinks.IsSupported()` возвращает `false`.
>
> **Настройка:** зарегистрируйте свою схему/хост глубоких ссылок в `AndroidManifest.xml` через настройки Build Game.

---

### 52.1 DeepLinks.IsSupported

```lua
DeepLinks.IsSupported() -> bool
```

Возвращает `true`, если текущая платформа поддерживает глубокие ссылки.

---

### 52.2 DeepLinks.Init

```lua
DeepLinks.Init()
```

Инициализирует обработчик глубоких ссылок. Должна быть вызвана один раз перед другими функциями `DeepLinks`.

---

### 52.3 DeepLinks.GetLastUri

```lua
DeepLinks.GetLastUri() -> string
```

Возвращает URI, которым было запущено приложение (или последнюю полученную ссылку). Возвращает `""`, если ссылки не было.

```lua
local uri = DeepLinks.GetLastUri()
if uri ~= "" then
    Print("Приложение открыто через: " .. uri)
end
```

---

### 52.4 Колбэки

```lua
DeepLinks.OnReceived(function(uri, data)
    Print("Получена глубокая ссылка: " .. uri)
    Print("Данные: " .. data)
end)
```

| Колбэк | Параметры | Описание |
|--------|-----------|----------|
| `DeepLinks.OnReceived(fn)` | `(uri: string, data: string)` | Получена глубокая ссылка |
| `DeepLinks.ClearCallbacks()` | — | Удалить все колбэки |

---

### 52.5 Практический пример — реферальная система

```lua
function OnCreate()
    if not DeepLinks.IsSupported() then return end

    DeepLinks.Init()

    -- Проверить, было ли приложение открыто через глубокую ссылку
    local uri = DeepLinks.GetLastUri()
    if uri ~= "" then
        HandleDeepLink(uri)
    end

    -- Слушать ссылки во время работы приложения
    DeepLinks.OnReceived(function(uri, data)
        HandleDeepLink(uri)
    end)
end

function HandleDeepLink(uri)
    -- Пример: mygame://invite?code=ABC123
    local code = uri:match("code=(%w+)")
    if code then
        Print("Реферальный код: " .. code)
        ApplyReferralBonus(code)
    end

    -- Пример: mygame://level/5
    local level = uri:match("level/(%d+)")
    if level then
        LoadLevel(tonumber(level))
    end
end
```

---

## 53. Permissions — Разрешения Android

### Обзор

Таблица **`Permissions`** предоставляет Lua API для запроса **runtime-разрешений Android**, открытия экранов системных настроек для «special»-разрешений, проверки состояния уведомлений и хранилища, а также получения распространённых путей хранилища Android.
Включает встроенные константы для всех разрешений, объявленных движком в `AndroidManifest.xml`, плюс подтаблицу `Permissions.Dirs` с именами публичных каталогов хранилища.

> **Платформа:** Android и iOS. Одни и те же константы `Permissions.CAMERA`, `Permissions.RECORD_AUDIO`, … работают на обеих — на iOS префикс `android.permission.` отбрасывается, а имя отображается на соответствующий iOS-API авторизации (AVFoundation, Photos, CoreLocation, UserNotifications, CoreBluetooth, CoreMotion, App Tracking Transparency, Contacts, EventKit), так что кроссплатформенным скриптам не нужны ветвления. Чисто андроидные понятия (`HasAllFilesAccess`, `CanDrawOverlays`, `CanWriteSettings`, `CanRequestInstallPackages`, `IsIgnoringBatteryOptimizations`, `IsNotificationPolicyAccessGranted`) на iOS возвращают `false`, а их запросы ничего не делают; `ShouldShowRationale()` на iOS означает «пользователь уже отказал — объясните зачем и отправьте его в Настройки», потому что iOS не спрашивает повторно. Для каждого реально запрашиваемого разрешения нужна строка-обоснование в Build Game → iOS → *Доп. описания разрешений*. На десктопе и в Web `Permissions.IsSupported()` возвращает `false`, а все вызовы ничего не делают (числовые геттеры возвращают `0`, строковые — `""`, булевы проверки — `false`).

---

### 53.1 Permissions.IsSupported

```lua
Permissions.IsSupported() -> bool
```

Возвращает `true`, если текущая платформа поддерживает runtime-разрешения.

---

### 53.2 Permissions.Request

```lua
Permissions.Request(permission)
```

Запрашивает одно разрешение. Для «normal»- и «dangerous»-разрешений показывается стандартный системный диалог, а результат приходит через `Permissions.OnResult`. Для «special»-разрешений (`MANAGE_EXTERNAL_STORAGE`, `SYSTEM_ALERT_WINDOW`, `WRITE_SETTINGS`, `REQUEST_INSTALL_PACKAGES`, `SCHEDULE_EXACT_ALARM`, `USE_EXACT_ALARM`, `REQUEST_IGNORE_BATTERY_OPTIMIZATIONS`, `ACCESS_NOTIFICATION_POLICY`) вместо диалога открывается соответствующий экран системных настроек — колбэка `OnResult` не будет; проверьте текущее состояние через `Permissions.Has()` или специализированные `Can*` / `Has*` / `Is*` геттеры после того, как пользователь вернётся из настроек.

```lua
Permissions.Request(Permissions.CAMERA)

Permissions.OnResult(function(permission, granted)
    if granted then
        Print(permission .. " получено!")
    else
        Print(permission .. " отклонено")
    end
end)
```

---

### 53.3 Permissions.RequestMultiple

```lua
Permissions.RequestMultiple(permissions)
```

Запрашивает несколько разрешений одновременно. Уже выданные разрешения тут же вызывают `OnResult(permission, true)`; «special»-разрешения по одному отправляются на свои экраны настроек; оставшиеся runtime-разрешения объединяются в один пакетный системный диалог.

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

Возвращает `true`, если разрешение в данный момент выдано. Понимает «special»-разрешения: например, `Permissions.Has(Permissions.MANAGE_EXTERNAL_STORAGE)` возвращает результат `Environment.isExternalStorageManager()` на Android 11+. Также обрабатывает `POST_NOTIFICATIONS` на Android до 13, фоллбэча на `NotificationManagerCompat.areNotificationsEnabled()`.

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

Возвращает `true`, если система рекомендует показать объяснение перед повторным запросом разрешения
(напр. пользователь ранее отклонил его без «Больше не спрашивать»). Для «special»-разрешений всегда возвращает `false` — они не используют стандартный диалог.

```lua
if Permissions.ShouldShowRationale(Permissions.CAMERA) then
    ShowDialog("Камера нужна для сканирования QR-кодов")
end
```

---

### 53.6 Permissions.OpenAppSettings

```lua
Permissions.OpenAppSettings()
```

Открывает экран **«О приложении»** для текущего приложения в системных настройках (где пользователь может управлять всеми разрешениями, хранилищем, уведомлениями, батареей и т. д.). Полезен как запасной вариант, когда «dangerous»-разрешение было отклонено с «Больше не спрашивать» — `Permissions.Request()` становится no-op, и единственный способ его выдать — через экран настроек.

```lua
if not Permissions.ShouldShowRationale(Permissions.CAMERA)
   and not Permissions.Has(Permissions.CAMERA) then
    ShowDialog("Камера запрещена навсегда. Открыть настройки?", function()
        Permissions.OpenAppSettings()
    end)
end
```

---

### 53.7 Permissions.OpenNotificationSettings

```lua
Permissions.OpenNotificationSettings()
```

Открывает системный экран **«Уведомления приложения»** для текущего приложения (каналы, звуки, важность). Использует `Settings.ACTION_APP_NOTIFICATION_SETTINGS` на Android 8+ (API 26+) и фоллбэчит на экран «О приложении» на более старых версиях.

```lua
if not Permissions.AreNotificationsEnabled() then
    ShowDialog("Включить уведомления, чтобы получать игровые оповещения?", function()
        Permissions.OpenNotificationSettings()
    end)
end
```

---

### 53.8 Permissions.AreNotificationsEnabled

```lua
Permissions.AreNotificationsEnabled() -> bool
```

Возвращает `true`, если пользователь не отключал уведомления для этого приложения на уровне ОС (независимо от `POST_NOTIFICATIONS` — работает на любой версии Android). Используйте, чтобы решить, планировать ли напоминания или предложить пользователю снова включить уведомления через `Permissions.OpenNotificationSettings()`.

```lua
if not Permissions.AreNotificationsEnabled() then
    HideNotificationBoundFeatures()
end
```

---

### 53.9 «Special»-разрешения

«Special»-разрешения Android не используют runtime-диалог — пользователь должен переключить их на отдельном экране системных настроек. У каждого есть `Has*` / `Can*` / `Is*` геттер (текущее состояние) и парный `Request*` метод (открывает экран настроек). Они также прозрачно поддерживаются `Permissions.Has()` и `Permissions.Request()` через соответствующие строковые константы из манифеста.

#### `MANAGE_EXTERNAL_STORAGE` — All Files Access (Android 11+)

```lua
Permissions.HasAllFilesAccess() -> bool
Permissions.RequestAllFilesAccess()
```

Имеет ли приложение разрешение **«Доступ ко всем файлам»** (широкое чтение/запись в общее хранилище). На Android 10 и ниже автоматически возвращает `true` — scoped storage там не действует. `RequestAllFilesAccess()` открывает `ACTION_MANAGE_APP_ALL_FILES_ACCESS_PERMISSION` с фоллбэком на глобальный список, если per-app интент недоступен.

#### `SYSTEM_ALERT_WINDOW` — Поверх других приложений

```lua
Permissions.CanDrawOverlays() -> bool
Permissions.RequestOverlayPermission()
```

Может ли приложение показывать overlay поверх других приложений. `RequestOverlayPermission()` открывает `ACTION_MANAGE_OVERLAY_PERMISSION` для текущего пакета. На Android 5.x и ниже всегда `true`.

#### `WRITE_SETTINGS` — Изменение системных настроек

```lua
Permissions.CanWriteSettings() -> bool
Permissions.RequestWriteSettings()
```

Может ли приложение менять системные настройки (яркость, рингтон и т. п.). `RequestWriteSettings()` открывает `ACTION_MANAGE_WRITE_SETTINGS`.

#### `REQUEST_INSTALL_PACKAGES` — Установка неизвестных приложений

```lua
Permissions.CanRequestInstallPackages() -> bool
Permissions.RequestInstallPackagesPermission()
```

Может ли приложение инициировать установку APK (Android 8+). `RequestInstallPackagesPermission()` открывает `ACTION_MANAGE_UNKNOWN_APP_SOURCES` для текущего пакета.

#### `SCHEDULE_EXACT_ALARM` — Точные будильники (Android 12+)

```lua
Permissions.CanScheduleExactAlarms() -> bool
Permissions.RequestExactAlarmPermission()
```

Может ли приложение вызывать `AlarmManager.setExact*` / `setAlarmClock`. `RequestExactAlarmPermission()` открывает `ACTION_REQUEST_SCHEDULE_EXACT_ALARM`. На Android 11 и ниже проверка неявная и возвращает `true`. Невозможное к отзыву разрешение `USE_EXACT_ALARM` (Android 13+) этот гейт не требует.

#### `REQUEST_IGNORE_BATTERY_OPTIMIZATIONS` — Исключение из оптимизации батареи

```lua
Permissions.IsIgnoringBatteryOptimizations() -> bool
Permissions.RequestIgnoreBatteryOptimizations()
```

Исключено ли приложение из Doze / App Standby для текущего пользователя. `RequestIgnoreBatteryOptimizations()` открывает per-app промпт оптимизации батареи с фоллбэком на глобальный список, если целевой интент отклонён OEM.

#### `ACCESS_NOTIFICATION_POLICY` — Доступ к Do Not Disturb

```lua
Permissions.IsNotificationPolicyAccessGranted() -> bool
Permissions.RequestNotificationPolicyAccess()
```

Может ли приложение читать или менять политику «Не беспокоить» на устройстве. `RequestNotificationPolicyAccess()` открывает `ACTION_NOTIFICATION_POLICY_ACCESS_SETTINGS`.

> **Settings round-trip:** эти `Request*` запускают внешний Activity — `OnResult` колбэка нет. Когда пользователь вернётся в игру, перепроверьте соответствующий `Has*` / `Can*` / `Is*` геттер (хорошее место — `OnResume` или первый кадр после возвращения из фона).

---

### 53.10 Пути приватного хранилища приложения

Эти пути принадлежат приложению, не требуют разрешений и стираются при удалении приложения.

```lua
Permissions.GetInternalFilesPath() -> string  -- context.getFilesDir()
Permissions.GetInternalCachePath() -> string  -- context.getCacheDir()
Permissions.GetExternalFilesPath() -> string  -- context.getExternalFilesDir(null)
Permissions.GetExternalCachePath() -> string  -- context.getExternalCacheDir()
```

* **Internal**-пути лежат на основном разделе хранилища устройства (всегда доступны, учитываются в квоте приложения в настройках).
* **External**-пути лежат на общем хранилище и могут быть на SD-карте — временно могут быть недоступны; проверяйте `Permissions.IsExternalStorageAvailable()` перед записью.
* **Cache**-пути могут быть очищены ОС в условиях нехватки места — никогда не храните то, что нельзя восстановить.

```lua
local saveDir = Permissions.GetInternalFilesPath() .. "/saves"
local tempDir = Permissions.GetExternalCachePath() .. "/temp_export"
```

---

### 53.11 Пути публичного хранилища

```lua
Permissions.GetPublicStoragePath(type) -> string
```

Возвращает абсолютный путь **публичного** каталога общего хранилища (`Environment.getExternalStoragePublicDirectory(type)`). Без аргумента (или с `""`) возвращает корень видимого пользователю внешнего хранилища (`Environment.getExternalStorageDirectory()`).
Запись сюда требует `WRITE_EXTERNAL_STORAGE` (≤ Android 9) или `MANAGE_EXTERNAL_STORAGE` / Storage Access Framework на более новых версиях — для чтения через `MediaStore` обычно достаточно детализированного `READ_MEDIA_*` разрешения.

Используйте подтаблицу `Permissions.Dirs` для аргумента `type`:

| Константа | Значение | Соответствует |
|-----------|----------|---------------|
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
Print("Папка Downloads: " .. downloads)

local sharedRoot = Permissions.GetPublicStoragePath()  -- /storage/emulated/0
```

---

### 53.12 Доступность хранилища и свободное место

```lua
Permissions.GetInternalStorageFreeBytes() -> integer   -- байт доступно в filesDir
Permissions.GetExternalStorageFreeBytes() -> integer   -- байт доступно в externalFilesDir
Permissions.IsExternalStorageAvailable() -> bool       -- true, если MEDIA_MOUNTED
Permissions.IsExternalStorageReadOnly() -> bool        -- true, если MEDIA_MOUNTED_READ_ONLY
```

Свободное место сообщается через `File.getUsableSpace()` — реально доступный для записи приложением объём (уже учитывает системные резервы). При ошибке или на неподдерживаемых платформах целые числа равны `0`, а булевы — `false`.

```lua
local needed = 200 * 1024 * 1024   -- 200 МБ
if Permissions.GetInternalStorageFreeBytes() < needed then
    ShowDialog("Недостаточно свободного места для загрузки обновления.")
end

if not Permissions.IsExternalStorageAvailable() or Permissions.IsExternalStorageReadOnly() then
    SwitchToInternalStorageMode()
end
```

---

### 53.13 Константы разрешений

Все разрешения, объявленные движком в `AndroidManifest.xml`, доступны как константы Lua и могут быть запрошены в рантайме. Сгруппированы по категориям для удобства.

#### Камера и микрофон

| Константа | Значение |
|-----------|----------|
| `Permissions.CAMERA` | `"android.permission.CAMERA"` |
| `Permissions.RECORD_AUDIO` | `"android.permission.RECORD_AUDIO"` |

#### Местоположение

| Константа | Значение |
|-----------|----------|
| `Permissions.ACCESS_FINE_LOCATION` | `"android.permission.ACCESS_FINE_LOCATION"` |
| `Permissions.ACCESS_COARSE_LOCATION` | `"android.permission.ACCESS_COARSE_LOCATION"` |
| `Permissions.ACCESS_BACKGROUND_LOCATION` | `"android.permission.ACCESS_BACKGROUND_LOCATION"` |
| `Permissions.ACCESS_LOCATION_EXTRA_COMMANDS` | `"android.permission.ACCESS_LOCATION_EXTRA_COMMANDS"` |
| `Permissions.ACCESS_MEDIA_LOCATION` | `"android.permission.ACCESS_MEDIA_LOCATION"` |

#### Хранилище и медиа

| Константа | Значение |
|-----------|----------|
| `Permissions.READ_EXTERNAL_STORAGE` | `"android.permission.READ_EXTERNAL_STORAGE"` |
| `Permissions.WRITE_EXTERNAL_STORAGE` | `"android.permission.WRITE_EXTERNAL_STORAGE"` |
| `Permissions.MANAGE_EXTERNAL_STORAGE` | `"android.permission.MANAGE_EXTERNAL_STORAGE"` |
| `Permissions.READ_MEDIA_IMAGES` | `"android.permission.READ_MEDIA_IMAGES"` |
| `Permissions.READ_MEDIA_VIDEO` | `"android.permission.READ_MEDIA_VIDEO"` |
| `Permissions.READ_MEDIA_AUDIO` | `"android.permission.READ_MEDIA_AUDIO"` |
| `Permissions.READ_MEDIA_VISUAL_USER_SELECTED` | `"android.permission.READ_MEDIA_VISUAL_USER_SELECTED"` |

> **Хранилище в современном Android:** начиная с Android 13 (API 33) `READ_EXTERNAL_STORAGE` больше не выдаётся — используйте детализированные `READ_MEDIA_IMAGES` / `READ_MEDIA_VIDEO` / `READ_MEDIA_AUDIO`. `READ_MEDIA_VISUAL_USER_SELECTED` (Android 14+) позволяет пользователю выбрать конкретные фото вместо предоставления доступа ко всем изображениям. `ACCESS_MEDIA_LOCATION` нужен, чтобы читать EXIF-данные GPS из фотографий, возвращаемых `MediaStore`.

#### Уведомления

| Константа | Значение |
|-----------|----------|
| `Permissions.POST_NOTIFICATIONS` | `"android.permission.POST_NOTIFICATIONS"` |
| `Permissions.USE_FULL_SCREEN_INTENT` | `"android.permission.USE_FULL_SCREEN_INTENT"` |
| `Permissions.ACCESS_NOTIFICATION_POLICY` | `"android.permission.ACCESS_NOTIFICATION_POLICY"` |

#### Bluetooth и Nearby

| Константа | Значение |
|-----------|----------|
| `Permissions.BLUETOOTH` | `"android.permission.BLUETOOTH"` |
| `Permissions.BLUETOOTH_ADMIN` | `"android.permission.BLUETOOTH_ADMIN"` |
| `Permissions.BLUETOOTH_CONNECT` | `"android.permission.BLUETOOTH_CONNECT"` |
| `Permissions.BLUETOOTH_SCAN` | `"android.permission.BLUETOOTH_SCAN"` |
| `Permissions.BLUETOOTH_ADVERTISE` | `"android.permission.BLUETOOTH_ADVERTISE"` |
| `Permissions.NEARBY_WIFI_DEVICES` | `"android.permission.NEARBY_WIFI_DEVICES"` |

#### Контакты и аккаунты

| Константа | Значение |
|-----------|----------|
| `Permissions.READ_CONTACTS` | `"android.permission.READ_CONTACTS"` |
| `Permissions.WRITE_CONTACTS` | `"android.permission.WRITE_CONTACTS"` |
| `Permissions.GET_ACCOUNTS` | `"android.permission.GET_ACCOUNTS"` |

#### Календарь

| Константа | Значение |
|-----------|----------|
| `Permissions.READ_CALENDAR` | `"android.permission.READ_CALENDAR"` |
| `Permissions.WRITE_CALENDAR` | `"android.permission.WRITE_CALENDAR"` |

#### Сенсоры и активность

| Константа | Значение |
|-----------|----------|
| `Permissions.BODY_SENSORS` | `"android.permission.BODY_SENSORS"` |
| `Permissions.BODY_SENSORS_BACKGROUND` | `"android.permission.BODY_SENSORS_BACKGROUND"` |
| `Permissions.ACTIVITY_RECOGNITION` | `"android.permission.ACTIVITY_RECOGNITION"` |
| `Permissions.HIGH_SAMPLING_RATE_SENSORS` | `"android.permission.HIGH_SAMPLING_RATE_SENSORS"` |

#### Телефон и звонки

| Константа | Значение |
|-----------|----------|
| `Permissions.READ_PHONE_STATE` | `"android.permission.READ_PHONE_STATE"` |
| `Permissions.READ_PHONE_NUMBERS` | `"android.permission.READ_PHONE_NUMBERS"` |
| `Permissions.CALL_PHONE` | `"android.permission.CALL_PHONE"` |
| `Permissions.ANSWER_PHONE_CALLS` | `"android.permission.ANSWER_PHONE_CALLS"` |
| `Permissions.READ_CALL_LOG` | `"android.permission.READ_CALL_LOG"` |
| `Permissions.WRITE_CALL_LOG` | `"android.permission.WRITE_CALL_LOG"` |
| `Permissions.ADD_VOICEMAIL` | `"android.permission.ADD_VOICEMAIL"` |

#### SMS и MMS

| Константа | Значение |
|-----------|----------|
| `Permissions.SEND_SMS` | `"android.permission.SEND_SMS"` |
| `Permissions.RECEIVE_SMS` | `"android.permission.RECEIVE_SMS"` |
| `Permissions.READ_SMS` | `"android.permission.READ_SMS"` |
| `Permissions.RECEIVE_MMS` | `"android.permission.RECEIVE_MMS"` |
| `Permissions.RECEIVE_WAP_PUSH` | `"android.permission.RECEIVE_WAP_PUSH"` |

#### Биометрия

| Константа | Значение |
|-----------|----------|
| `Permissions.USE_BIOMETRIC` | `"android.permission.USE_BIOMETRIC"` |
| `Permissions.USE_FINGERPRINT` | `"android.permission.USE_FINGERPRINT"` |

#### Foreground-сервисы

| Константа | Значение |
|-----------|----------|
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

> **Гранулярные типы foreground-сервисов (Android 14+):** объявления одного `FOREGROUND_SERVICE` уже недостаточно — также объявите соответствующий тип (напр. `FOREGROUND_SERVICE_MEDIA_PLAYBACK` для музыкального плеера) и укажите `android:foregroundServiceType` у сервиса. Все они — normal-разрешения, выдаются при установке.

#### Будильники и фон

| Константа | Значение |
|-----------|----------|
| `Permissions.REQUEST_IGNORE_BATTERY_OPTIMIZATIONS` | `"android.permission.REQUEST_IGNORE_BATTERY_OPTIMIZATIONS"` |
| `Permissions.SCHEDULE_EXACT_ALARM` | `"android.permission.SCHEDULE_EXACT_ALARM"` |
| `Permissions.USE_EXACT_ALARM` | `"android.permission.USE_EXACT_ALARM"` |
| `Permissions.RECEIVE_BOOT_COMPLETED` | `"android.permission.RECEIVE_BOOT_COMPLETED"` |
| `Permissions.RUN_USER_INITIATED_JOBS` | `"android.permission.RUN_USER_INITIATED_JOBS"` |

#### «Special»-разрешения

| Константа | Значение |
|-----------|----------|
| `Permissions.SYSTEM_ALERT_WINDOW` | `"android.permission.SYSTEM_ALERT_WINDOW"` |
| `Permissions.WRITE_SETTINGS` | `"android.permission.WRITE_SETTINGS"` |
| `Permissions.REQUEST_INSTALL_PACKAGES` | `"android.permission.REQUEST_INSTALL_PACKAGES"` |
| `Permissions.REQUEST_DELETE_PACKAGES` | `"android.permission.REQUEST_DELETE_PACKAGES"` |
| `Permissions.QUERY_ALL_PACKAGES` | `"android.permission.QUERY_ALL_PACKAGES"` |

> `Permissions.Request(Permissions.SYSTEM_ALERT_WINDOW)` (и другие «special»-константы из этой группы) открывают соответствующий экран системных настроек — runtime-диалога не показывается. Парные геттеры см. в разделе **53.9**.

#### Сеть

| Константа | Значение |
|-----------|----------|
| `Permissions.INTERNET` | `"android.permission.INTERNET"` |
| `Permissions.ACCESS_NETWORK_STATE` | `"android.permission.ACCESS_NETWORK_STATE"` |
| `Permissions.ACCESS_WIFI_STATE` | `"android.permission.ACCESS_WIFI_STATE"` |
| `Permissions.CHANGE_WIFI_STATE` | `"android.permission.CHANGE_WIFI_STATE"` |
| `Permissions.CHANGE_NETWORK_STATE` | `"android.permission.CHANGE_NETWORK_STATE"` |

#### Аудио и железо

| Константа | Значение |
|-----------|----------|
| `Permissions.VIBRATE` | `"android.permission.VIBRATE"` |
| `Permissions.WAKE_LOCK` | `"android.permission.WAKE_LOCK"` |
| `Permissions.MODIFY_AUDIO_SETTINGS` | `"android.permission.MODIFY_AUDIO_SETTINGS"` |

#### System UI, обои и задачи

| Константа | Значение |
|-----------|----------|
| `Permissions.EXPAND_STATUS_BAR` | `"android.permission.EXPAND_STATUS_BAR"` |
| `Permissions.SET_WALLPAPER` | `"android.permission.SET_WALLPAPER"` |
| `Permissions.SET_WALLPAPER_HINTS` | `"android.permission.SET_WALLPAPER_HINTS"` |
| `Permissions.KILL_BACKGROUND_PROCESSES` | `"android.permission.KILL_BACKGROUND_PROCESSES"` |
| `Permissions.REORDER_TASKS` | `"android.permission.REORDER_TASKS"` |

#### Обнаружение захвата экрана

| Константа | Значение |
|-----------|----------|
| `Permissions.DETECT_SCREEN_CAPTURE` | `"android.permission.DETECT_SCREEN_CAPTURE"` |
| `Permissions.DETECT_SCREEN_RECORDING` | `"android.permission.DETECT_SCREEN_RECORDING"` |

#### Advertising ID

| Константа | Значение |
|-----------|----------|
| `Permissions.AD_ID` | `"com.google.android.gms.permission.AD_ID"` |

> Обратите внимание на префикс не-`android.permission.*` — это разрешение объявлено Google Play Services и требуется на Android 13+ для приложений, читающих Advertising ID.

> Вы также можете передать любую строку разрешения Android напрямую: `Permissions.Request("android.permission.VIBRATE")`

> **Normal vs Dangerous vs Special разрешения:** «normal»-разрешения (`INTERNET`, `ACCESS_NETWORK_STATE`, `ACCESS_WIFI_STATE`, `VIBRATE`, `WAKE_LOCK`, `MODIFY_AUDIO_SETTINGS`, `FOREGROUND_SERVICE*`, `USE_EXACT_ALARM`, `RUN_USER_INITIATED_JOBS`, `RECEIVE_BOOT_COMPLETED`, `USE_BIOMETRIC`, `USE_FINGERPRINT`, `EXPAND_STATUS_BAR`, `SET_WALLPAPER`, `SET_WALLPAPER_HINTS`, `KILL_BACKGROUND_PROCESSES`, `REORDER_TASKS`, `USE_FULL_SCREEN_INTENT`, `ACCESS_LOCATION_EXTRA_COMMANDS`, `RECEIVE_WAP_PUSH`, `AD_ID`, `QUERY_ALL_PACKAGES`, `DETECT_SCREEN_CAPTURE`, `DETECT_SCREEN_RECORDING`) выдаются автоматически при установке — `Permissions.Has()` всегда возвращает `true`, а `Permissions.Request()` ничего не делает. «Dangerous»-разрешения (камера, микрофон, местоположение, контакты, календарь, body-сенсоры, телефон, SMS, современное хранилище медиа, `BLUETOOTH_CONNECT/SCAN/ADVERTISE`, `NEARBY_WIFI_DEVICES`, `POST_NOTIFICATIONS`, `ACTIVITY_RECOGNITION`, `ACCESS_MEDIA_LOCATION`) требуют разрешения от пользователя через `Permissions.Request()`. «Special»-разрешения (`MANAGE_EXTERNAL_STORAGE`, `SYSTEM_ALERT_WINDOW`, `WRITE_SETTINGS`, `REQUEST_INSTALL_PACKAGES`, `SCHEDULE_EXACT_ALARM`, `REQUEST_IGNORE_BATTERY_OPTIMIZATIONS`, `ACCESS_NOTIFICATION_POLICY`, `ACCESS_BACKGROUND_LOCATION`) требуют перехода пользователя в системные настройки — стандартным диалогом разрешения их не выдать; используйте парные геттеры из раздела **53.9** (или просто вызывайте `Permissions.Has()` / `Permissions.Request()` с константой — они прозрачно понимают «special»-разрешения).

> **Заметки по версиям API:** `BLUETOOTH_CONNECT/SCAN/ADVERTISE` действуют на Android 12+ (API 31+); на старых версиях автоматически используются `BLUETOOTH` и `BLUETOOTH_ADMIN` (объявлены в манифесте с `maxSdkVersion="30"`). `POST_NOTIFICATIONS` и `READ_MEDIA_*` действуют на Android 13+ (API 33+). `NEARBY_WIFI_DEVICES` действует на Android 13+. `READ_MEDIA_VISUAL_USER_SELECTED` действует на Android 14+ (API 34+). `BODY_SENSORS_BACKGROUND` действует на Android 13+. `USE_EXACT_ALARM` действует на Android 13+ (API 33+) — для приложений-будильников и календарей; для остальных используйте `SCHEDULE_EXACT_ALARM`. Гранулярные `FOREGROUND_SERVICE_*` обязательны на Android 14+ (API 34+). `RUN_USER_INITIATED_JOBS`, `DETECT_SCREEN_CAPTURE` и `DETECT_SCREEN_RECORDING` действуют на Android 14+ / 15+. `USE_FULL_SCREEN_INTENT` автоматически выдаётся только приложениям категорий «вызов» / «будильник» на Android 14+; остальным пользователь должен включить его вручную в per-app экране разрешений. На более низких SDK константы существуют, но `Permissions.Has()` либо вернёт `true` автоматически (разрешение неявное), либо `false` (фича недоступна на этой версии платформы).

---

### 53.14 Сводка колбэков

| Колбэк | Параметры | Описание |
|--------|-----------|----------|
| `Permissions.OnResult(fn)` | `(permission: string, granted: bool)` | Результат запроса разрешения (только для runtime-разрешений — «special»-разрешения его не вызывают) |
| `Permissions.ClearCallbacks()` | — | Удалить все колбэки |

---

### 53.15 Практический пример — гейтинг функций по разрешениям

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
            Print("Разрешение отклонено: " .. permission)
        end
    end)
end

function OnARButtonPressed()
    if Permissions.Has(Permissions.CAMERA) then
        StartARMode()
    elseif Permissions.ShouldShowRationale(Permissions.CAMERA) then
        ShowDialog("Для AR-режима нужен доступ к камере", function()
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
        ShowDialog("Разрешить установку модов из этого приложения?", function()
            Permissions.RequestInstallPackagesPermission()
        end)
    end
end

function OnReturnFromBackground()
    -- перепроверяем «special»-разрешения после того, как пользователь
    -- мог их переключить на экране системных настроек
    if Permissions.CanDrawOverlays() then EnableMiniPlayerOverlay() end
    if Permissions.IsIgnoringBatteryOptimizations() then EnableBackgroundSync() end
end

function OnExportSaveToDownloads()
    if Permissions.IsExternalStorageReadOnly() then
        ShowDialog("Общее хранилище на этом устройстве только для чтения.")
        return
    end
    local dir = Permissions.GetPublicStoragePath(Permissions.Dirs.DOWNLOADS)
    WriteSaveTo(dir .. "/MyGameSave.json")
end
```

---

## 54. Web3 — Интеграция с блокчейном (Ethereum / BNB Smart Chain)

Модуль `Web3` предоставляет подключение кошелька MetaMask, запросы баланса нативных монет и ERC-20 токенов, подпись транзакций и взаимодействие со смарт-контрактами — всё прямо из Lua-скриптов. Доступен только на платформе **Web (WASM)** при включённой опции **Web3** в настройках сборки.

Библиотека Web3 (`ethers.js`) встраивается в страницу игры на этапе сборки, поэтому Web3-сборка **не загружает ни одного скрипта со сторонних сайтов** — она работает офлайн, с `file://`, при строгом CSP и на игровых порталах, где внешние запросы запрещены, а IP игрока не уходит ни на какой CDN. Единственные хосты, к которым обращается игра, — это RPC-узлы блокчейна и шлюз IPFS/Arweave, которые вы сами задали.

Каждый асинхронный вызов возвращает `requestId` и всегда завершается ровно одним колбэком. Если мост Web3 недоступен — сборка без опции Web3, страница без моста, либо запуск на Windows/Linux/macOS/Android/iOS — вызов всё равно завершится через `onError` (и через `Web3.OnError`), а не зависнет молча. Благодаря этому один и тот же скрипт работает без изменений на любой платформе.

Поля таблиц принимают не только строки, но и **числа** Lua (`tokenId = 42` и `tokenId = "42"` равнозначны), а `abi` / `args` / `owners` / `tokenIds` принимают Lua-**таблицу** вместо JSON-строки.

### Проверка платформы

| Функция | Возвращает | Описание |
|---------|-----------|----------|
| `Web3.IsSupported()` | `bool` | `true` только если страница действительно может работать с Web3: Web-сборка, мост присутствует и библиотека `ethers` загружена. Проверка выполняется во время работы, поэтому вернёт `false` и в случае, если библиотека не скачалась |
| `Web3.IsConnected()` | `bool` | `true` если кошелёк подключён |

### Кошелёк

| Функция | Возвращает | Описание |
|---------|-----------|----------|
| `Web3.ConnectWallet(callback?)` | — | Открывает окно подключения MetaMask. Опциональный `callback(address)` при успехе |
| `Web3.DisconnectWallet()` | — | Отключает текущую сессию кошелька |
| `Web3.GetAddress()` | `string` | Текущий адрес подключённого кошелька (`0x...`) |
| `Web3.GetChainId()` | `string` | ID текущей сети в hex (`"0x1"`, `"0x38"`, …) |

### Управление сетями

| Функция | Возвращает | Описание |
|---------|-----------|----------|
| `Web3.SwitchChain(chain)` | — | Переключиться на сеть по hex ID или имени (`"ethereum"`, `"bsc"`, `"sepolia"`, …). Автоматически добавляет неизвестные сети через MetaMask |
| `Web3.AddChain(config)` | — | Зарегистрировать пользовательскую сеть. `config` — таблица: `{ chainId, name, rpcUrl, symbol, explorerUrl }`. В `chainId` также принимается сокращённое имя сети |
| `Web3.SetReadOnlyChain(chain, rpcUrl?)` | — | Включить **режим чтения**: все читающие функции работают до подключения кошелька (или вообще без него). `chain` — hex ID или имя сети; `rpcUrl` необязателен и по умолчанию берётся встроенный адрес для известных сетей. Вызов `Web3.SetReadOnlyChain("", "")` отключает режим |

**Поддерживаемые сокращения сетей:** `ethereum` / `eth` / `mainnet`, `holesky`, `sepolia`, `bsc` / `bnb`, `bsc_testnet` / `bnb_testnet`, `polygon`, `polygon_amoy` / `amoy`, `arbitrum`, `arbitrum_sepolia`, `optimism`, `optimism_sepolia`, `avalanche`, `avalanche_fuji` / `fuji`, `base`, `base_sepolia`, `linea`, `scroll`, `zksync`, `blast`, `gnosis`, `celo`, `mantle`.

**Режим чтения.** Подключённый кошелёк не нужен, чтобы читать данные из сети. После `Web3.SetReadOnlyChain("base")` вызовы `GetBalance`, `GetTokenBalance`, `CallContract`, `GetNFTOwner`, `GetTokenURI`, `GetOwnedTokens`, `GetAllowance`, `EstimateGas`, `GetTransactionReceipt`, `SubscribeEvent` и `Multicall` идут через публичный RPC — это удобно для таблиц лидеров, галерей NFT и витрин магазина, которые показываются до подключения игрока. Для каждой встроенной сети задано два независимых RPC-адреса, и при недоступности первого автоматически используется второй. Как только кошелёк подключён, чтение идёт через него и его текущую сеть — RPC режима чтения используется только пока кошелёк не подключён, и сохраняется после `Web3.DisconnectWallet()`. Подпись (`SendTransaction`, `WriteContract`, `TransferToken`, `TransferNFT`, `ApproveToken`, `SetApprovalForAll`, `SignMessage`, `SignTypedData`) всегда требует подключённого кошелька.

### Константы Chain ID

| Константа | Значение | Сеть |
|-----------|---------|------|
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

### Транзакции

| Функция | Возвращает | Описание |
|---------|-----------|----------|
| `Web3.SendTransaction(params, onSent?, onConfirmed?, onFailed?)` | `requestId` | Отправить нативную монету. `params = { to, value, data? }`. `value` в ETH/BNB (например `"0.1"`) |
| `Web3.SignMessage(message, callback?, onError?)` | `requestId` | Личная подпись. `callback(signature)`. `onError(errorMessage)` при ошибке |

### Баланс

| Функция | Возвращает | Описание |
|---------|-----------|----------|
| `Web3.GetBalance(address?, callback?, onError?)` | `requestId` | Баланс нативной монеты в ETH/BNB. По умолчанию — подключённый кошелёк |
| `Web3.GetTokenBalance(tokenAddress, ownerAddress?, callback?, onError?)` | `requestId` | Баланс ERC-20 токена, уже приведённый по `decimals()` токена. По умолчанию — подключённый кошелёк. Для токенов без метода `decimals()` принимается 18 знаков — так же, как это делают кошельки |

### Смарт-контракты

| Функция | Возвращает | Описание |
|---------|-----------|----------|
| `Web3.CallContract(params, callback?, onError?)` | `requestId` | Вызов только для чтения. `params = { address, abi, method, args? }`. `abi` и `args` — JSON-строки |
| `Web3.WriteContract(params, onSent?, onConfirmed?, onFailed?)` | `requestId` | Запись в контракт. `params = { address, abi, method, args?, value? }` |

### NFT (ERC-721 / ERC-1155)

| Функция | Возвращает | Описание |
|---------|-----------|----------|
| `Web3.GetNFTBalance(params, callback?, onError?)` | `requestId` | Количество токенов. `params = { address, owner?, tokenId? }`. Без `tokenId` → ERC-721 `balanceOf(address)`. С `tokenId` → ERC-1155 `balanceOf(address, tokenId)`. По умолчанию owner — подключённый кошелёк. `callback(count)` |
| `Web3.GetNFTOwner(params, callback?, onError?)` | `requestId` | Владелец конкретного ERC-721 токена. `params = { address, tokenId }`. `callback(ownerAddress)` |
| `Web3.GetTokenURI(params, callback?, onError?)` | `requestId` | URI метаданных токена. `params = { address, tokenId }`. Сначала пробует ERC-721 `tokenURI()`, при неудаче — ERC-1155 `uri()`. Плейсхолдер ERC-1155 `{id}` заменяется на 64-символьный hex-идентификатор в нижнем регистре с ведущими нулями, как требует стандарт. `callback(uri)` |
| `Web3.GetNFTBalanceBatch(params, callback?, onError?)` | `requestId` | Пакетный запрос баланса ERC-1155. `params = { address, owners, tokenIds }`. `owners` и `tokenIds` — JSON-массивы. `callback(balancesJson)` |
| `Web3.GetOwnedTokens(params, callback?, onError?)` | `requestId` | Перечисление всех NFT владельца (требуется ERC721Enumerable). `params = { address, owner? }`. `callback(tokenIdsJson)` |

### Передача NFT

| Функция | Возвращает | Описание |
|---------|-----------|----------|
| `Web3.TransferNFT(params, onSent?, onConfirmed?, onFailed?)` | `requestId` | Передача NFT через `safeTransferFrom`. `params = { address, to, tokenId, from?, standard?, amount? }`. `standard` — `"ERC721"` (по умолчанию) или `"ERC1155"`. `amount` по умолчанию `"1"` (только ERC-1155). `from` по умолчанию — подключённый кошелёк |

### ERC-20 Approve и Allowance

| Функция | Возвращает | Описание |
|---------|-----------|----------|
| `Web3.ApproveToken(params, onSent?, onConfirmed?, onFailed?)` | `requestId` | Одобрить использование ваших токенов для spender. `params = { token, spender, amount }`. `amount` в человекочитаемых единицах (например `"100"` для 100 токенов) — decimals запрашиваются автоматически. Используйте `"max"` или `"unlimited"` для безлимитного одобрения (`MaxUint256`). Если контракт не имеет `decimals()`, `amount` трактуется как raw wei |
| `Web3.GetAllowance(params, callback?, onError?)` | `requestId` | Проверить сколько токенов spender может использовать. `params = { token, spender, owner? }`. `owner` по умолчанию — подключённый кошелёк. `callback(allowance)` |

### ERC-20 Трансфер

| Функция | Возвращает | Описание |
|---------|-----------|----------|
| `Web3.TransferToken(params, onSent?, onConfirmed?, onFailed?)` | `requestId` | Перевод ERC-20 токенов. `params = { token, to, amount }`. `amount` в человекочитаемых единицах (например `"50"` для 50 токенов) — decimals запрашиваются автоматически. Если контракт не имеет `decimals()`, `amount` трактуется как raw wei |

### Одобрение NFT (ERC-721 / ERC-1155)

| Функция | Возвращает | Описание |
|---------|-----------|----------|
| `Web3.SetApprovalForAll(params, onSent?, onConfirmed?, onFailed?)` | `requestId` | Установить или отозвать одобрение оператора для всех токенов контракта. Работает с ERC-721 и ERC-1155. `params = { address, operator, approved? }`. `approved` по умолчанию `true`. Вызывает `setApprovalForAll(operator, approved)` на контракте |

### Отслеживание транзакций

| Функция | Возвращает | Описание |
|---------|-----------|----------|
| `Web3.WatchTransaction(txHash, onConfirmed?, onFailed?, timeoutMs?)` | `requestId` | Отслеживание любой транзакции в блокчейне (включая чужие). `onConfirmed(txHash)` при успехе, `onFailed(error)` при откате/ошибке/таймауте. `timeoutMs` по умолчанию `120000` (2 минуты) |

### EIP-712 Подпись типизированных данных

| Функция | Возвращает | Описание |
|---------|-----------|----------|
| `Web3.SignTypedData(typedDataJson, callback?, onError?)` | `requestId` | Подпись структурированных данных по стандарту EIP-712 (`eth_signTypedData_v4`). `typedDataJson` — полная JSON-строка EIP-712 с `types`, `primaryType`, `domain` и `message`. `callback(signature)` |

### Подписка на события контрактов

| Функция | Возвращает | Описание |
|---------|-----------|----------|
| `Web3.SubscribeEvent(params, callback?, onError?)` | `subscriptionId` | Подписка на события контракта в реальном времени. `params = { address, abi, event }`. `abi` — JSON ABI с определением события. `callback(eventDataJson)` срабатывает при каждом событии. Возвращает `subscriptionId` для отписки |
| `Web3.UnsubscribeEvent(subscriptionId)` | — | Прекратить прослушивание ранее подписанного события контракта |

### Оценка газа

| Функция | Возвращает | Описание |
|---------|-----------|----------|
| `Web3.EstimateGas(params, callback?, onError?)` | `requestId` | Оценка газа для транзакции. `params = { to, value?, data? }`. `callback(gasJson)` где `gasJson` содержит `gasLimit`, `gasPrice`, `maxFeePerGas`, `maxPriorityFeePerGas` |

### Квитанция транзакции

| Функция | Возвращает | Описание |
|---------|-----------|----------|
| `Web3.GetTransactionReceipt(txHash, callback?, onError?)` | `requestId` | Получить полные данные квитанции. `callback(receiptJson)` с `status`, `gasUsed`, `effectiveGasPrice`, `blockNumber`, `blockHash`, `from`, `to`, `contractAddress`, `logs` |

### Мульти-провайдер (EIP-6963)

| Функция | Возвращает | Описание |
|---------|-----------|----------|
| `Web3.GetProviders()` | `string` | Возвращает JSON-массив обнаруженных провайдеров кошельков в браузере. Каждая запись: `{ rdns, name, icon, uuid }`. Например MetaMask = `"io.metamask"`, Coinbase = `"com.coinbase.wallet"` |
| `Web3.ConnectWithProvider(rdns, callback?)` | — | Подключиться через конкретный провайдер кошелька по его RDNS-идентификатору. `callback(address)` при успехе |

### Разрешение метаданных

| Функция | Возвращает | Описание |
|---------|-----------|----------|
| `Web3.ResolveMetadata(uri, callback?, onError?)` | `requestId` | Загрузить и распарсить метаданные NFT из URI. Автоматически разрешает схемы `ipfs://`, `ipfs://ipfs/` и `ar://`, в том числе внутри полей `image`, `image_url`, `animation_url` и `external_url`. Запрос прерывается через 30 секунд с ошибкой вместо бесконечного ожидания. `callback(metadataJson)` |
| `Web3.SetIPFSGateway(url)` | — | Установить пользовательский IPFS-шлюз для `ResolveMetadata`. По умолчанию `"https://ipfs.io/ipfs/"`. Пример: `Web3.SetIPFSGateway("https://gateway.pinata.cloud/ipfs/")` |

### Multicall (Пакетное чтение)

| Функция | Возвращает | Описание |
|---------|-----------|----------|
| `Web3.Multicall(calls, callback?, onError?)` | `requestId` | Выполнить несколько read-only вызовов контрактов одним RPC-запросом через Multicall3. `calls` — Lua-массив из `{ address, abi, method, args? }`, порядок вызовов сохраняется в результате. `callback(resultsJson)` возвращает массив `{ success, decoded, data }`: `success` — результат вызова в сети, `decoded` — удалось ли декодировать возвращённое значение по переданному ABI (при `false` в `data` остаётся сырой hex). Доступно на всех основных EVM-сетях; для zkSync Era автоматически используется её собственный контракт Multicall3 |

### Колбэки событий

| Функция | Описание |
|---------|----------|
| `Web3.OnWalletConnected(callback)` | `callback(address)` — кошелёк подключён |
| `Web3.OnWalletDisconnected(callback)` | `callback()` — кошелёк отключён |
| `Web3.OnChainChanged(callback)` | `callback(chainId)` — пользователь сменил сеть в MetaMask |
| `Web3.OnAccountChanged(callback)` | `callback(address)` — пользователь сменил аккаунт в MetaMask |
| `Web3.OnError(callback)` | `callback(errorMessage, requestId)` — любая ошибка |
| `Web3.ClearCallbacks()` | Удалить все зарегистрированные колбэки и отменить все активные подписки на события |

Колбэки и подписки также очищаются автоматически при остановке игры **и при каждой смене уровня** — так же, как у `Input`, `Network` и остальных скриптовых подсистем: замыкания старого уровня не должны продолжать срабатывать после того, как его сцена уничтожена. Нужные обработчики регистрируйте заново в `OnLevelLoaded` нового уровня. Само подключение кошелька при этом не трогается: `Web3.IsConnected()`, `Web3.GetAddress()` и `Web3.GetChainId()` продолжают работать между уровнями, режим чтения тоже сохраняется. Если при смене уровня транзакция ещё может быть в полёте, сохраните её `txHash` и подхватите её через `Web3.WatchTransaction(txHash, …)` после загрузки нового уровня.

### Пример — Чтение сети до подключения кошелька

```lua
function OnLevelLoaded()
    if not Web3.IsSupported() then return end

    -- Кошелёк не нужен: читаем напрямую через публичный RPC
    Web3.SetReadOnlyChain("base")

    Web3.GetTokenBalance(
        "0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913",
        "0x1111111111111111111111111111111111111111",
        function(balance)
            Print("USDC на балансе казны: " .. balance)
        end,
        function(err)
            PrintError("Ошибка чтения: " .. err)
        end
    )
end
```

### Пример — Подключение кошелька и показ баланса BNB

```lua
function OnLevelLoaded()
    if not Web3.IsSupported() then
        Print("Web3 недоступен на этой платформе")
        return
    end

    Web3.OnWalletConnected(function(address)
        Print("Кошелёк подключён: " .. address)

        -- Переключаемся на BSC и получаем баланс
        Web3.SwitchChain("bsc")
        Web3.GetBalance(address, function(balance)
            Print("Баланс BNB: " .. balance)
        end)
    end)

    Web3.OnChainChanged(function(chainId)
        Print("Сеть изменена на: " .. chainId)
    end)

    Web3.OnError(function(err, reqId)
        PrintError("Ошибка Web3: " .. err)
    end)
end

function OnConnectButtonPressed()
    Web3.ConnectWallet()
end
```

### Пример — Отправка ETH

```lua
function SendETH(toAddress, amount)
    Web3.SwitchChain("ethereum")

    Web3.SendTransaction(
        { to = toAddress, value = amount },
        function(txHash)
            Print("Транзакция отправлена: " .. txHash)
        end,
        function(txHash)
            Print("Транзакция подтверждена: " .. txHash)
        end,
        function(err)
            PrintError("Транзакция провалилась: " .. err)
        end
    )
end
```

### Пример — Баланс ERC-20 токена

```lua
local USDT_BSC = "0x55d398326f99059fF775485246999027B3197955"

function CheckUSDTBalance()
    Web3.SwitchChain("bsc")
    Web3.GetTokenBalance(USDT_BSC, nil, function(balance)
        Print("Баланс USDT: " .. balance)
    end)
end
```

### Пример — Чтение из смарт-контракта

```lua
local contractAddr = "0xYourContractAddress"
local abi = '[{"inputs":[{"internalType":"address","name":"player","type":"address"}],"name":"getScore","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"stateMutability":"view","type":"function"}]'

function GetPlayerScore()
    local addr = Web3.GetAddress()
    Web3.CallContract(
        { address = contractAddr, abi = abi, method = "getScore", args = '["' .. addr .. '"]' },
        function(result)
            Print("Счёт игрока: " .. result)
        end
    )
end
```

### Пример — Запись в смарт-контракт

```lua
local contractAddr = "0xYourContractAddress"
local abi = '[{"inputs":[{"internalType":"uint256","name":"score","type":"uint256"}],"name":"submitScore","outputs":[],"stateMutability":"nonpayable","type":"function"}]'

function SubmitScore(score)
    Web3.WriteContract(
        { address = contractAddr, abi = abi, method = "submitScore", args = '[' .. score .. ']' },
        function(txHash)
            Print("Счёт отправлен, tx: " .. txHash)
        end,
        function(txHash)
            Print("Счёт подтверждён в блокчейне!")
        end,
        function(err)
            PrintError("Не удалось отправить счёт: " .. err)
        end
    )
end
```

### Пример — Проверка NFT

```lua
local NFT_CONTRACT = "0xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D" -- BAYC

function CheckMyNFTs()
    Web3.GetNFTBalance(
        { address = NFT_CONTRACT },
        function(count)
            Print("У вас " .. count .. " NFT")
        end,
        function(err)
            PrintError("Ошибка запроса NFT: " .. err)
        end
    )
end

function CheckNFTOwner(tokenId)
    Web3.GetNFTOwner(
        { address = NFT_CONTRACT, tokenId = tostring(tokenId) },
        function(owner)
            Print("Токен #" .. tokenId .. " принадлежит: " .. owner)
        end
    )
end

function GetNFTMetadata(tokenId)
    Web3.GetTokenURI(
        { address = NFT_CONTRACT, tokenId = tostring(tokenId) },
        function(uri)
            Print("URI токена: " .. uri)
        end
    )
end
```

### Пример — Отслеживание внешней транзакции

```lua
function WatchPayment(txHash)
    Web3.WatchTransaction(txHash,
        function(hash)
            Print("Транзакция подтверждена: " .. hash)
        end,
        function(err)
            PrintError("Транзакция провалилась: " .. err)
        end
        -- таймаут по умолчанию 120000мс (2 минуты)
    )
end
```

### Пример — Баланс ERC-1155 игрового предмета

```lua
local ITEMS_CONTRACT = "0xYourERC1155Contract"
local SWORD_TOKEN_ID = "1"

function CheckSwordCount()
    Web3.GetNFTBalance(
        { address = ITEMS_CONTRACT, tokenId = SWORD_TOKEN_ID },
        function(count)
            Print("У вас " .. count .. " мечей")
        end
    )
end
```

### Пример — EIP-712 подпись типизированных данных (паттерн permit)

```lua
function SignPermit(spender, value, nonce, deadline)
    local typedData = '{"types":{"EIP712Domain":[{"name":"name","type":"string"},{"name":"version","type":"string"},{"name":"chainId","type":"uint256"},{"name":"verifyingContract","type":"address"}],"Permit":[{"name":"owner","type":"address"},{"name":"spender","type":"address"},{"name":"value","type":"uint256"},{"name":"nonce","type":"uint256"},{"name":"deadline","type":"uint256"}]},"primaryType":"Permit","domain":{"name":"MyToken","version":"1","chainId":1,"verifyingContract":"0xTokenAddress"},"message":{"owner":"' .. Web3.GetAddress() .. '","spender":"' .. spender .. '","value":"' .. value .. '","nonce":"' .. nonce .. '","deadline":"' .. deadline .. '"}}'

    Web3.SignTypedData(typedData,
        function(signature)
            Print("Подпись permit: " .. signature)
        end,
        function(err)
            PrintError("Ошибка подписи: " .. err)
        end
    )
end
```

### Пример — Подписка на события контракта

```lua
local subId = nil

function StartListening()
    local abi = '[{"type":"event","name":"Transfer","inputs":[{"indexed":true,"name":"from","type":"address"},{"indexed":true,"name":"to","type":"address"},{"indexed":true,"name":"tokenId","type":"uint256"}]}]'

    subId = Web3.SubscribeEvent(
        { address = "0xNFTContract", abi = abi, event = "Transfer" },
        function(eventJson)
            Print("Событие Transfer: " .. eventJson)
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

### Пример — Передача NFT

```lua
function TransferNFT(contractAddr, toAddr, tokenId)
    Web3.TransferNFT(
        { address = contractAddr, to = toAddr, tokenId = tostring(tokenId) },
        function(txHash) Print("Передача отправлена: " .. txHash) end,
        function(txHash) Print("Передача подтверждена!") end,
        function(err) PrintError("Ошибка передачи: " .. err) end
    )
end

-- Передача ERC-1155 (5 зелий)
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

### Пример — Approve и проверка allowance

```lua
local TOKEN = "0xTokenAddress"
local MARKETPLACE = "0xMarketplaceAddress"

-- Одобрить 100 токенов (decimals запрашиваются автоматически)
function ApproveMarketplace()
    Web3.ApproveToken(
        { token = TOKEN, spender = MARKETPLACE, amount = "100" },
        function(txHash) Print("Approve tx отправлена: " .. txHash) end,
        function(txHash) Print("Одобрено!") end,
        function(err) PrintError("Ошибка approve: " .. err) end
    )
end

-- Безлимитное одобрение
function ApproveUnlimited()
    Web3.ApproveToken(
        { token = TOKEN, spender = MARKETPLACE, amount = "max" },
        nil,
        function(txHash) Print("Безлимитное одобрение подтверждено!") end
    )
end

function CheckAllowance()
    Web3.GetAllowance(
        { token = TOKEN, spender = MARKETPLACE },
        function(allowance) Print("Allowance: " .. allowance) end
    )
end
```

### Пример — Перевод ERC-20 токенов

```lua
local USDT = "0xdAC17F958D2ee523a2206206994597C13D831ec7"

function SendTokens(toAddr, amount)
    Web3.TransferToken(
        { token = USDT, to = toAddr, amount = amount },
        function(txHash) Print("Перевод отправлен: " .. txHash) end,
        function(txHash) Print("Перевод подтверждён!") end,
        function(err) PrintError("Ошибка перевода: " .. err) end
    )
end
```

### Пример — Одобрение всех NFT для оператора

```lua
local NFT_CONTRACT = "0xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D"
local MARKETPLACE = "0xMarketplaceAddress"

function ApproveAllNFTs()
    Web3.SetApprovalForAll(
        { address = NFT_CONTRACT, operator = MARKETPLACE, approved = true },
        function(txHash) Print("Tx одобрения отправлена: " .. txHash) end,
        function(txHash) Print("Все NFT одобрены для маркетплейса!") end,
        function(err) PrintError("Ошибка одобрения: " .. err) end
    )
end

function RevokeAllNFTs()
    Web3.SetApprovalForAll(
        { address = NFT_CONTRACT, operator = MARKETPLACE, approved = false },
        nil,
        function(txHash) Print("Одобрение отозвано!") end
    )
end
```

### Пример — Отслеживание транзакции с таймаутом

```lua
function WatchWithTimeout(txHash)
    Web3.WatchTransaction(txHash,
        function(hash)
            Print("Транзакция подтверждена: " .. hash)
        end,
        function(err)
            PrintError("Транзакция не прошла или таймаут: " .. err)
        end,
        60000  -- 60 секунд таймаут
    )
end
```

### Пример — Пользовательский IPFS-шлюз

```lua
-- Использовать Pinata вместо ipfs.io по умолчанию
Web3.SetIPFSGateway("https://gateway.pinata.cloud/ipfs/")

function ShowNFTWithCustomGateway(contractAddr, tokenId)
    Web3.GetTokenURI(
        { address = contractAddr, tokenId = tostring(tokenId) },
        function(uri)
            Web3.ResolveMetadata(uri, function(metadataJson)
                Print("Метаданные NFT: " .. metadataJson)
            end)
        end
    )
end
```

### Пример — Оценка газа перед отправкой

```lua
function EstimateTransferCost(toAddr, amount)
    Web3.EstimateGas(
        { to = toAddr, value = amount },
        function(gasJson)
            Print("Оценка газа: " .. gasJson)
        end,
        function(err) PrintError("Ошибка оценки: " .. err) end
    )
end
```

### Пример — Получение полной квитанции транзакции

```lua
function GetReceipt(txHash)
    Web3.GetTransactionReceipt(txHash,
        function(receiptJson)
            Print("Квитанция: " .. receiptJson)
        end
    )
end
```

### Пример — Выбор кошелька через мульти-провайдер

```lua
function ListWallets()
    local providersJson = Web3.GetProviders()
    Print("Доступные кошельки: " .. providersJson)
end

function ConnectCoinbaseWallet()
    Web3.ConnectWithProvider("com.coinbase.wallet", function(address)
        Print("Подключено через Coinbase: " .. address)
    end)
end
```

### Пример — Разрешение метаданных NFT из IPFS

```lua
function ShowNFTMetadata(contractAddr, tokenId)
    Web3.GetTokenURI(
        { address = contractAddr, tokenId = tostring(tokenId) },
        function(uri)
            Web3.ResolveMetadata(uri, function(metadataJson)
                Print("Метаданные NFT: " .. metadataJson)
            end)
        end
    )
end
```

### Пример — Пакетное чтение через Multicall

```lua
local NFT = "0xNFTContract"

function BatchCheckOwnership(tokenIds)
    local calls = {}
    local abi = '[{"inputs":[{"name":"tokenId","type":"uint256"}],"name":"ownerOf","outputs":[{"name":"","type":"address"}],"stateMutability":"view","type":"function"}]'
    for _, id in ipairs(tokenIds) do
        table.insert(calls, { address = NFT, abi = abi, method = "ownerOf", args = '[' .. id .. ']' })
    end
    Web3.Multicall(calls, function(resultsJson)
        Print("Владельцы: " .. resultsJson)
    end)
end
```

---

## 55. LocalPlayer — Локальный мультиплеер и сплит-скрин

> **Тип:** Глобальные функции. Для кооперативной игры на одном экране (до 4 игроков).

### Регистрация игроков

```lua
-- Зарегистрировать игрока с устройством
RegisterLocalPlayer(0, "keyboard")           -- Игрок 0 на клавиатуре+мыши
RegisterLocalPlayer(1, "gamepad", 0)         -- Игрок 1 на геймпаде 0
RegisterLocalPlayer(2, "gamepad", 1)         -- Игрок 2 на геймпаде 1
RegisterLocalPlayer(3, "controller", 2)      -- Игрок 3 на геймпаде 2 ("controller"/"pad" тоже работают)

-- Отменить регистрацию
UnregisterLocalPlayer(2)
UnregisterAllLocalPlayers()

-- Запросить информацию
local registered = IsLocalPlayerRegistered(0)  -- → bool
local count = GetLocalPlayerCount()            -- → int (0-4)

-- Информация об устройстве
local device = GetLocalPlayerDevice(1)
-- → { type = "gamepad", index = 0, active = true }

-- Автоназначение всех подключённых геймпадов
AutoAssignLocalPlayers()
```

**Строки типа устройства:** `"keyboard"`, `"keyboard_mouse"`, `"kb"`, `"gamepad"`, `"controller"`, `"pad"`

### Ввод игрока

```lua
-- Движение (нормализованное -1..1). Для клавиатурного игрока, клавиши по умолчанию: WASD
local move = GetPlayerMovement(0)               -- → {x, y}
local move = GetPlayerMovement(0, "left", "right", "down", "up")  -- Свои клавиши
SetVelocity(move.x * speed, move.y * speed)

-- Состояние кнопок (абстрактные имена или сырые кнопки)
if IsPlayerButtonPressed(0, "jump") then Jump() end
if IsPlayerButtonJustPressed(1, "attack") then Attack() end
if IsPlayerButtonJustReleased(0, "confirm") then Confirm() end

-- Правый стик (геймпад) / дельта мыши (клавиатурный игрок)
local aim = GetPlayerAimStick(0)  -- → {x, y}

-- Триггеры (только геймпад, 0.0-1.0)
local lt = GetPlayerTrigger(0, "left")   -- также "lt" или "l2"
local rt = GetPlayerTrigger(0, "right")  -- также "rt" или "r2"
```

**Абстрактные имена кнопок:**

| Имя | Клавиатура | Геймпад |
|-----|-----------|---------|
| `confirm` | Enter | A |
| `cancel` | Escape | B |
| `jump` | Space | A |
| `attack` | J | X |
| `interact` | E | Y |
| `pause` | Escape | Start |

**Сырые кнопки геймпада:** `a`, `b`, `x`, `y`, `start`, `back`, `lb`, `rb`, `leftshoulder`, `rightshoulder`, `dpadup`, `dpaddown`, `dpadleft`, `dpadright`

### Вибрация и информация об устройстве

```lua
-- Вибрация (только геймпад)
SetPlayerRumble(0, 0.5, 0.8, 300)   -- playerIndex, lowFreq, highFreq, длительностьMs
StopPlayerRumble(0)

-- Имя и тип контроллера
local name = GetPlayerGamepadName(1)   -- → "Xbox Controller" или "Keyboard & Mouse"
local type = GetPlayerGamepadType(1)   -- → "xbox", "ps", "switch", "keyboard_mouse", "none"
```

### Раскладка сплит-скрина

```lua
-- Установить раскладку
SetSplitScreenLayout("horizontal")   -- Верх/низ на 2 игрока
SetSplitScreenLayout("vertical")     -- Лево/право на 2 игрока
SetSplitScreenLayout("quad")         -- Сетка на 4 игрока
SetSplitScreenLayout("three_top1_bottom2")  -- 1 сверху + 2 снизу
SetSplitScreenLayout("three_left1_right2")  -- 1 слева + 2 справа
SetSplitScreenLayout("auto")         -- Автовыбор раскладки по числу игроков
SetSplitScreenLayout("none")         -- Отключить

-- Ярлык: принудительно включить режим Auto (эквивалент SetSplitScreenLayout("auto"))
ApplyAutoSplitLayout()

local layout = GetSplitScreenLayout()  -- → "horizontal", "vertical", "auto" и т.д.
local active = IsSplitScreenActive()   -- → bool

-- Разрешённая раскладка после вычисления "auto" (никогда не возвращает "auto")
local eff = GetEffectiveSplitScreenLayout()  -- → "none", "vertical", "quad", ...

-- true, если локальный мультиплеер активен (зарегистрированы 2+ игроков)
local mp = IsLocalMultiplayerActive()  -- → bool

-- Ориентация, используемая раскладкой "auto" при ровно 2 игроках
SetTwoPlayerOrientation("horizontal")  -- "horizontal" (верх/низ) или "vertical" (лево/право)
local orient = GetTwoPlayerOrientation()  -- → "horizontal" или "vertical"

-- Разделитель между вьюпортами сплит-скрина (пиксели + RGBA-цвет)
SetSplitScreenDivider(2, 0.0, 0.0, 0.0, 1.0)  -- thicknessPx, r, g, b, a (аргументы цвета опциональны, по умолчанию чёрный)
SetSplitScreenDivider(0)                      -- отключить разделитель
local px = GetSplitScreenDividerPx()          -- → текущая толщина в пикселях

-- Получить вычисленный прямоугольник обзора для слота игрока
local rect = GetPlayerViewportRect(0)  -- → {x, y, width, height} (нормализованные 0-1)

-- Тот же прямоугольник в пиксельных координатах (y отсчитывается сверху, удобно для HUD / scissor)
local pxRect = GetPlayerViewportPixels(0, windowW, windowH)
                                       -- → {x, y, width, height} (целые)
```

**Что каждый игрок получает в своём вьюпорте.** При активном сплит-скрине каждый
вид игрока рендерится через полный пайплайн движка независимо: собственный фрустум
и куллинг, собственные карты теней, спрайты/флипбуки/скелеты/тайлмапы (включая
анимированные тайлы и stencil-эффекты), FX, туман войны, GI, видео и синема-фейд.
Пост-процессинг тоже персональный: волюмы оцениваются по позиции камеры каждого
игрока, и у каждого вида свой стек эффектов (bloom, грейдинг, виньетка, motion blur,
автоэкспозиция, FXAA) с независимым временным состоянием.

**Виджеты.** У инстанса виджета есть `Player Index`: `-1` — рендерится на экране
каждого игрока, `0-3` — только на экране этого игрока. Мышь и тач автоматически
привязываются к вьюпорту, над которым находится курсор: хит-тест идёт в локальных
координатах этого вьюпорта и только по виджетам этого игрока (плюс общие с `-1`).
Геймпадная навигация по UI управляется устройством игрока-владельца виджета;
общие виджеты (`-1`) может навигировать любой зарегистрированный игрок.

**Камеры.** Primary-камера сама определяет свой слот сплит-скрина: если у неё задан
`Player Index` — используется он, иначе берётся первый зарегистрированный игрок.
Центрирование следования учитывает размер именно её вьюпорта. Не-primary камеры с
`Player Index >= 0` рендерят вьюпорты своих игроков (игрок должен быть зарегистрирован).
Слоты раскладки назначаются по порядку зарегистрированных игроков, поэтому
разреженная регистрация (например, игроки 0 и 2) всё равно упаковывает вьюпорты плотно.

**Аудио.** Каждая камера игрока становится слушателем пространственного звука
(до 4 слушателей): позиционные звуки затухают от ближайшего к ним экрана.

### Управление камерами

```lua
-- Применить раскладку сплит-скрина ко всем камерам с PlayerIndex >= 0
ApplySplitScreenCameras()

-- Вручную задать область обзора для камеры конкретного игрока
SetPlayerCameraViewport(0, 0.0, 0.0, 0.5, 1.0)  -- Левая половина

-- Найти сущность камеры для игрока
local camId = GetPlayerCameraEntity(1)  -- → entityId или nil
```

### Детект ввода (экран присоединения)

```lua
-- Обнаружить любой новый ввод (для экранов "Нажмите Start чтобы присоединиться")
function OnUpdate(dt)
    local input = DetectPlayerInput()
    if input then
        -- input = { type = "gamepad", index = 0, input = "a" }
        -- или:   { type = "keyboard_mouse", index = 0, input = "space" }
        RegisterLocalPlayer(GetLocalPlayerCount(), input.type, input.index)
    end
end

-- Автоназначение клавиатуры (слот 0) и всех подключённых геймпадов (остальные слоты)
AutoAssignLocalDevices(true)   -- preferKeyboard = true (клавиатура идёт в слот 0)
AutoAssignLocalDevices(false)  -- только геймпады
```

### Полный пример — кооператив на 2 игрока

```lua
function OnLevelStart()
    -- Игрок 1 на клавиатуре, Игрок 2 на первом геймпаде
    RegisterLocalPlayer(0, "keyboard")
    RegisterLocalPlayer(1, "gamepad", 0)

    -- Установить вертикальный сплит-скрин
    SetSplitScreenLayout("vertical")
    ApplySplitScreenCameras()
end

function OnUpdate(dt)
    -- Движение игрока 1
    local move1 = GetPlayerMovement(0)
    -- Движение игрока 2
    local move2 = GetPlayerMovement(1)

    -- Действия игрока 1
    if IsPlayerButtonJustPressed(0, "jump") then
        -- Игрок 1 прыгает
    end

    -- Действия игрока 2
    if IsPlayerButtonJustPressed(1, "attack") then
        -- Игрок 2 атакует
    end
end
```

### Краткий справочник

| Функция | Описание |
|---------|----------|
| `RegisterLocalPlayer(idx, device, [devIdx])` | Зарегистрировать игрока с устройством |
| `UnregisterLocalPlayer(idx)` | Отменить регистрацию игрока |
| `UnregisterAllLocalPlayers()` | Удалить всех игроков |
| `IsLocalPlayerRegistered(idx)` | Зарегистрирован ли игрок |
| `GetLocalPlayerCount()` | Количество активных игроков |
| `GetLocalPlayerDevice(idx)` | Таблица информации об устройстве |
| `AutoAssignLocalPlayers()` | Автоназначить геймпады |
| `AutoAssignLocalDevices([preferKeyboard])` | Автоназначить клавиатуру + геймпады |
| `IsLocalMultiplayerActive()` | Зарегистрированы 2+ игроков |
| `GetPlayerMovement(idx, [l,r,d,u])` | Вектор движения `{x,y}` |
| `IsPlayerButtonPressed(idx, btn)` | Кнопка удерживается |
| `IsPlayerButtonJustPressed(idx, btn)` | Кнопка только что нажата |
| `IsPlayerButtonJustReleased(idx, btn)` | Кнопка только что отпущена |
| `GetPlayerAimStick(idx)` | Правый стик / дельта мыши |
| `GetPlayerTrigger(idx, side)` | Значение триггера (0-1) |
| `SetPlayerRumble(idx, lo, hi, ms)` | Включить вибрацию |
| `StopPlayerRumble(idx)` | Остановить вибрацию |
| `GetPlayerGamepadName(idx)` | Имя контроллера |
| `GetPlayerGamepadType(idx)` | Тип контроллера |
| `SetSplitScreenLayout(layout)` | Установить раскладку (в т.ч. `"auto"`) |
| `GetSplitScreenLayout()` | Текущее имя раскладки |
| `GetEffectiveSplitScreenLayout()` | Разрешённая раскладка (после `"auto"`) |
| `ApplyAutoSplitLayout()` | Ярлык для режима `"auto"` |
| `SetTwoPlayerOrientation(mode)` | Ориентация для `"auto"` при 2 игроках |
| `GetTwoPlayerOrientation()` | `"horizontal"` / `"vertical"` |
| `SetSplitScreenDivider(px, [r,g,b,a])` | Толщина и цвет разделителя |
| `GetSplitScreenDividerPx()` | Текущая толщина разделителя |
| `IsSplitScreenActive()` | Активен ли сплит-скрин |
| `GetPlayerViewportRect(idx)` | Область обзора игрока (0..1) |
| `GetPlayerViewportPixels(idx, w, h)` | Область обзора в пикселях |
| `ApplySplitScreenCameras()` | Применить раскладку к камерам |
| `SetPlayerCameraViewport(idx, x,y,w,h)` | Ручная область обзора |
| `GetPlayerCameraEntity(idx)` | ID сущности камеры |
| `DetectPlayerInput()` | Обнаружить любой новый ввод |

---

## 56. Video — Воспроизведение видео

> **Тип:** Глобальные функции. Таблица `Video`.
>
> Полноэкранное воспроизведение видео для интро, катсцен и внутриигровых роликов.
> Видео накладывается поверх всего экрана игры с правильным соотношением сторон (letterbox/pillarbox).
> Поддерживает пропуск через ввод (ESC / Space / Enter / Геймпад A / Start), звук, регулировку громкости, зацикливание
> и Lua-событие `VideoFinished` для переходов между сценами.
>
> Видеофайлы воспроизводятся напрямую (`.mp4`, `.webm`, `.avi`, `.mkv` — любой формат, поддерживаемый FFmpeg).
> Работает как с обычными файлами на диске, так и с файлами, запакованными в `.ICEPAK` архивы через VFS.
>
> **Важно:** Доступно на всех шести платформах. **Windows**, **Linux**, **macOS** и **Android** декодируют
> через FFmpeg; **iOS** воспроизводит через AVFoundation (только H.264/HEVC — WebM/VP9 там не декодируется,
> поэтому кукинг видео для iOS-сборок переключается на PassThrough); **Web** воспроизводит через
> браузерный элемент `<video>`.

### Воспроизведение

```lua
-- Запустить видео (путь относительно Content/)
local ok = Video.Play("Videos/intro.mp4")

-- Остановить текущее видео
Video.Stop()

-- Пауза / Продолжить
Video.Pause()
Video.Resume()

-- Пропустить видео (вызывает событие VideoFinished)
Video.Skip()
```

### Проверки состояния

```lua
local playing  = Video.IsPlaying()      -- true пока видео воспроизводится
local paused   = Video.IsPaused()       -- true если на паузе
local finished = Video.IsFinished()     -- true после завершения или пропуска
local active   = Video.HasActiveVideo() -- true если воспроизводится или на паузе
local path     = Video.GetPath()        -- путь к текущему видеофайлу
```

### Время и прогресс

```lua
local t   = Video.GetTime()       -- текущее время воспроизведения (секунды)
local dur = Video.GetDuration()   -- общая продолжительность (секунды)
local pct = Video.GetProgress()   -- прогресс 0.0–1.0
```

### Громкость

```lua
Video.SetVolume(0.8)              -- установить громкость звука (0.0–1.0)
local vol = Video.GetVolume()     -- получить текущую громкость
```

### Настройки пропуска и зацикливания

```lua
-- Разрешить игроку пропускать через ESC/Space/Enter/Геймпад
Video.SetSkippable(true)
local canSkip = Video.IsSkippable()

-- Зациклить видео
Video.SetLooping(true)
local loops = Video.IsLooping()
```

### Событие VideoFinished

Когда видео завершается естественным образом или пропускается, движок генерирует событие `VideoFinished`.
Используйте систему событий для подписки и переходов между сценами:

```lua
function OnInit()
    On("VideoFinished", function(videoPath)
        Print("Видео завершено: " .. videoPath)
        LoadLevel("Content/Maps/Level1.icemap")
    end)
end

function OnStart()
    Video.Play("Videos/intro.mp4")
    Video.SetSkippable(true)
end
```

### Практический пример — интро с плавным переходом

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

### Практический пример — внутриигровая катсцена

```lua
function PlayCutscene(videoFile)
    Video.Play("Videos/Cutscenes/" .. videoFile)
    Video.SetSkippable(true)
    Video.SetLooping(false)
end

function OnInit()
    On("VideoFinished", function(path)
        Print("Катсцена завершена, возврат к игровому процессу")
    end)
end

function OnTriggerEnter(other)
    local tag = Entity.GetTag(other)
    if tag == "CutsceneTrigger" then
        PlayCutscene("boss_intro.mp4")
    end
end
```

### Справочник API

| Функция | Возвращает | Описание |
|---------|-----------|----------|
| `Video.Play(path)` | `bool` | Запустить воспроизведение видео. Путь относительно `Content/`. Возвращает `true` при успехе |
| `Video.Stop()` | — | Остановить и сбросить текущее видео |
| `Video.Pause()` | — | Поставить на паузу |
| `Video.Resume()` | — | Возобновить воспроизведение |
| `Video.Skip()` | — | Пропустить видео (генерирует событие `VideoFinished`) |
| `Video.IsPlaying()` | `bool` | `true` пока видео активно воспроизводится |
| `Video.IsPaused()` | `bool` | `true` если видео на паузе |
| `Video.IsFinished()` | `bool` | `true` после завершения или пропуска видео |
| `Video.GetTime()` | `float` | Текущее время воспроизведения в секундах |
| `Video.GetDuration()` | `float` | Общая продолжительность видео в секундах |
| `Video.GetProgress()` | `float` | Прогресс воспроизведения (0.0–1.0) |
| `Video.SetVolume(volume)` | — | Установить громкость звука (0.0–1.0) |
| `Video.GetVolume()` | `float` | Получить текущую громкость звука |
| `Video.SetSkippable(bool)` | — | Разрешить/запретить пропуск через ввод (ESC, Space, Enter, Геймпад A/Start) |
| `Video.IsSkippable()` | `bool` | Можно ли пропустить видео |
| `Video.SetLooping(bool)` | — | Включить/выключить зацикливание видео |
| `Video.IsLooping()` | `bool` | Включено ли зацикливание |
| `Video.GetPath()` | `string` | Получить путь к текущему видео |
| `Video.HasActiveVideo()` | `bool` | `true` если видео воспроизводится или на паузе |

### События

| Событие | Параметры | Описание |
|---------|-----------|----------|
| `VideoFinished` | `path` (string) | Генерируется при завершении воспроизведения или пропуске видео |

---

## 57. Voice — Микрофон, Opus, анализ голоса

> **Тип:** Глобальный. Подходит для одиночных игр — **не** требует сети. Нужен везде, где нужно кричать, шептать, петь или распознавать произнесённые слова через микрофон.
> **Opus:** кодирование/декодирование требует кодека Opus — проверяйте его наличие в рантайме через `Voice.HasOpus()`. Захват, воспроизведение, запись в WAV и анализ громкости работают в любом случае.

API `Voice` даёт Lua прямой доступ к:

- **Захвату с микрофона** (устройство записи по умолчанию через SDL3, 16-bit PCM).
- **Кодированию/декодированию Opus** (тот же кодек, что и в `Network.EnableVoiceChat`, но доступный любой локальной игре).
- **Воспроизведению** PCM или Opus-данных.
- **Анализу громкости** — RMS, пик, дБ, экспоненциально сглаженная громкость в реальном времени.
- **Детекции голоса** — `IsSpeaking()` / `IsScreaming()` по настраиваемым порогам.
- **Записи в WAV** — пишет живой поток с микрофона в 16-bit PCM `.wav` файл.
- **Луп-бэку** — пускает мик прямо в колонки (эффект «слышу себя»).

Внутри есть очередь кадров, которую заполняет `Voice.Update()` (вызывайте раз в кадр, например в `OnUpdate`). Один кадр = `sampleRate / 50` сэмплов (20 мс; 960 при 48 кГц).

### Жизненный цикл захвата / воспроизведения / кодека

| Функция | Возвращает | Описание |
|---------|------------|----------|
| Voice.StartCapture(sampleRate?, channels?) | bool | Открыть микрофон по умолчанию. По умолчанию: 48000 Гц, 1 канал |
| Voice.StopCapture() | — | Остановить микрофон |
| Voice.IsCapturing() | bool | Микрофон активен? |
| Voice.StartPlayback(sampleRate?, channels?) | bool | Открыть устройство воспроизведения |
| Voice.StopPlayback() | — | Закрыть воспроизведение |
| Voice.IsPlaybackActive() | bool | Воспроизведение инициализировано? |
| Voice.SetPlaybackVolume(volume) | — | 0.0–2.0, применяется ко всем декодированным/выведенным PCM |
| Voice.GetPlaybackVolume() | float | Текущая громкость воспроизведения |
| Voice.InitCodec(sampleRate?, channels?, bitrate?) | bool | Создать Opus encoder+decoder. По умолчанию: 48000 Гц, 1 канал, 32000 bps |
| Voice.ShutdownCodec() | — | Уничтожить кодек |
| Voice.IsCodecReady() | bool | true если Opus инициализирован и валиден |
| Voice.HasOpus() | bool | true если кодирование/декодирование Opus доступно в этой сборке |
| Voice.Shutdown() | — | Остановить всё (захват, воспроизведение, запись, кодек) |

### Поток кадров (микрофон → Lua)

| Функция | Возвращает | Описание |
|---------|------------|----------|
| Voice.Update() | — | Прокачать захваченные кадры; обновить RMS/пик/сглаженный; писать WAV/loopback. Вызывайте каждый кадр |
| Voice.HasFrame() | bool | true если в очереди есть хотя бы один кадр |
| Voice.QueuedFrameCount() | int | Количество кадров в очереди |
| Voice.SetMaxQueuedFrames(n) | — | Сбрасывать самые старые кадры сверх лимита (по умолчанию 32) |
| Voice.ReadFrame() | string \| nil | Извлечь самый старый кадр как бинарную PCM-строку (int16 LE) — быстрый путь |
| Voice.ReadFrameTable() | table \| nil | Извлечь самый старый кадр как Lua-массив целых чисел |
| Voice.ClearFrames() | — | Очистить очередь |
| Voice.GetSampleRate() | int | Текущая частота дискретизации |
| Voice.GetChannels() | int | Текущее число каналов |
| Voice.GetFrameSize() | int | Сэмплов на кадр |
| Voice.SetFrameSize(n) | — | Принудительно задать размер кадра (продвинуто) |

### Кодирование / декодирование Opus

| Функция | Возвращает | Описание |
|---------|------------|----------|
| Voice.Encode(pcmString) | string \| nil | Закодировать один PCM-кадр (бинарная строка из `ReadFrame`) |
| Voice.EncodeTable(samples) | string \| nil | Закодировать из Lua-массива int16 |
| Voice.Decode(opusString) | string \| nil | Декодировать Opus-пакет в бинарную PCM-строку |
| Voice.DecodeTable(opusString) | table \| nil | Декодировать Opus-пакет в Lua-массив |

### Прямое воспроизведение

| Функция | Возвращает | Описание |
|---------|------------|----------|
| Voice.PlayPCM(pcmString) | — | Отправить сырой PCM (бинарная строка) в колонки |
| Voice.PlayPCMTable(samples) | — | Отправить сырой PCM (Lua-массив) |
| Voice.PlayEncoded(opusString) | bool | Декодировать Opus и сразу отправить в колонки |

### Анализ громкости и детекция голоса

| Функция | Возвращает | Описание |
|---------|------------|----------|
| Voice.GetVolume() | float | RMS последнего кадра, нормализован 0..1 |
| Voice.GetVolumePeak() | float | Абсолютный пик последнего кадра, 0..1 |
| Voice.GetVolumeDB() | float | RMS в децибелах (нижний порог `-120 дБ`) |
| Voice.GetSmoothedVolume() | float | Экспоненциально сглаженный RMS — для UI-индикаторов |
| Voice.SetSmoothing(factor) | — | 0..0.9999 (по умолчанию 0.85). Больше = плавнее/медленнее |
| Voice.GetSmoothing() | float | Текущий коэффициент сглаживания |
| Voice.SetInputGain(g) | — | Множитель сэмплов микрофона (0..32). По умолчанию 1.0 |
| Voice.GetInputGain() | float | Текущий input gain |
| Voice.SetVoiceThreshold(t) | — | Порог по умолчанию для `IsSpeaking` (0..1) |
| Voice.GetVoiceThreshold() | float | Текущий порог голоса |
| Voice.IsSpeaking(threshold?) | bool | `smoothed >= threshold` (по умолчанию `VoiceThreshold`) |
| Voice.IsScreaming(threshold?) | bool | `peak >= threshold` (по умолчанию 0.45) |

### Loopback (микрофон → колонки)

| Функция | Возвращает | Описание |
|---------|------------|----------|
| Voice.SetLoopback(enabled) | — | Пока `true` и захват активен — каждый кадр автоматически воспроизводится |
| Voice.IsLoopback() | bool | Состояние loopback |

### Запись в WAV

Пишет 16-bit PCM `.wav` с текущей частотой и числом каналов захвата. Размеры RIFF/data финализируются при `StopRecording` или `Shutdown`.

| Функция | Возвращает | Описание |
|---------|------------|----------|
| Voice.StartRecording(filePath) | bool | Начать запись микрофона в WAV. Создаёт родительские папки |
| Voice.StopRecording() | bool | Завершить и закрыть WAV |
| Voice.IsRecording() | bool | Запись идёт? |
| Voice.GetRecordedDuration() | float | Сколько уже записано, в секундах |

### Константы

| Константа | Значение | Описание |
|-----------|----------|----------|
| Voice.DEFAULT_SAMPLE_RATE | 48000 | Частота по умолчанию для Opus и захвата |
| Voice.DEFAULT_CHANNELS | 1 | Моно-микрофон по умолчанию |
| Voice.DEFAULT_BITRATE | 32000 | Битрейт Opus по умолчанию |

### Пример: «Кричи громче — прыгай выше»

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

### Пример: «Записать голосовой клип и проиграть его»

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

### Пример: Локальный round-trip через Opus (encode → decode → play)

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

### Заметки

- Формат кадра: знаковый 16-bit PCM, little-endian, чередующиеся каналы. `Voice.ReadFrame()` возвращает Lua-**строку** с сырыми байтами — её можно сразу передавать в `Voice.Encode` без копий.
- `Voice.IsScreaming` использует пик (мгновенно), `Voice.IsSpeaking` — сглаженный RMS (стабильно) — выбирайте под свою механику.
- Система `Voice` полностью независима от `Network`. Их можно использовать одновременно.
- `Voice.Update()` потокобезопасен и идемпотентен; вызова из `OnUpdate` достаточно для всех функций (loopback, запись WAV, индикаторы громкости, очередь кадров).

---

## 58. Replay — Запись, воспроизведение, killcam

Модуль `Replay` записывает состояние сцены во время игры (трансформы сущностей, физические скорости, пользовательские значения) в таймлайн и позволяет воспроизвести её позже. Подходит для **killcam**, **реплеев смерти**, **записи матчей**, **ghost-забегов**, **захвата трейлеров** и **отладочного rewind**.

**Ключевые возможности**

- Записывает `TransformComponent` (Position / Scale / Rotation / Visible / Enabled) для каждой сущности с `IDComponent`.
- Записывает линейную и угловую скорость для сущностей с `RigidbodyComponent`.
- Позволяет записывать произвольные числовые и строковые значения покадрово (`Replay.RecordValue`, `Replay.RecordString`) — удобно для HUD: счёт, патроны, фаза.
- Два режима записи: **полная запись сессии** (`StartRecording` / `StopRecording`) и **кольцевой буфер killcam** (`StartBuffer` / `CaptureBuffer`).
- Воспроизведение с **интерполяцией**, настраиваемой **скоростью** (slow-mo / fast-forward), **loop**, **seek**, **pause/resume**.
- Сохранение / загрузка в JSON в пользовательскую папку `Saves/Replays/`.
- Сущности идентифицируются по **UUID** (`IDComponent.ID`), поэтому реплеи выживают перезапуски редактора и смену entt-handle.

> Реплей обновляется автоматически каждый кадр скрипта после `TimeLua / Coroutine / Tween` и до сущностных `OnUpdate`. Система сбрасывается вместе с runtime, когда вы останавливаете Play.

### Жизненный цикл и режимы

```lua
Replay.GetMode()        -- 0 = Idle, 1 = Recording, 2 = Playing
Replay.Mode.Idle        -- 0
Replay.Mode.Recording   -- 1
Replay.Mode.Playing     -- 2
```

Реплей может находиться только в **одном** режиме одновременно. Вызов `Play()` во время записи сначала останавливает запись; вызов `StartRecording()` во время воспроизведения сначала останавливает воспроизведение.

**Буфер killcam** независим от режимов record/play: вы можете держать кольцевой буфер в фоне, пока воспроизводите ранее захваченный реплей.

### API записи

| Функция | Описание |
|---|---|
| `Replay.StartRecording(name?, rate?)` | Начать запись. `name` (string, опц) — ярлык, сохраняемый в файле. `rate` (Гц, по умолчанию `60`) — частота сэмплинга. |
| `Replay.StopRecording()` | Остановить запись и скопировать данные в активный реплей (готов к `Play()` / `Save()`). |
| `Replay.IsRecording()` | `true`, пока идёт запись. |
| `Replay.GetRecordedDuration()` | Длительность текущей записи в секундах. |
| `Replay.GetRecordedFrameCount()` | Число уже записанных кадров. |

```lua
Replay.StartRecording("match01", 30)  -- 30 кадров в секунду
-- ... идёт игра ...
Replay.StopRecording()
Replay.Save("match01")
```

### Killcam (кольцевой буфер)

Буфер постоянно хранит последние *N* секунд. Когда происходит важное событие (смерть, килл, гол), вызовите `CaptureBuffer()`, чтобы зафиксировать буфер как воспроизводимый реплей.

| Функция | Описание |
|---|---|
| `Replay.StartBuffer(seconds, rate?)` | Запустить кольцевой буфер. `seconds` — сколько истории хранить. `rate` (Гц, по умолчанию `60`). |
| `Replay.StopBuffer()` | Остановить буфер и очистить его содержимое. |
| `Replay.IsBuffering()` | `true`, пока буфер работает. |
| `Replay.GetBufferDuration()` | Настроенная длина буфера в секундах. |
| `Replay.CaptureBuffer(name?)` | Превратить текущее содержимое буфера в активный реплей. Возвращает `true` при успехе. |

```lua
function OnLevelStart()
    Replay.StartBuffer(8.0, 60)   -- всегда хранить последние 8 секунд
end

function OnPlayerDeath()
    if Replay.CaptureBuffer("death_killcam") then
        Replay.SetSpeed(0.4)      -- slow-mo
        Replay.Play()
    end
end
```

### API воспроизведения

| Функция | Описание |
|---|---|
| `Replay.Play()` | Начать воспроизведение с `t = 0`. Возвращает `true`, если есть что воспроизводить. |
| `Replay.Pause()` | Приостановить (время замирает, сцена застывает на текущем кадре). |
| `Replay.Resume()` | Продолжить воспроизведение. |
| `Replay.Stop()` | Остановить воспроизведение и сбросить время в `0`. |
| `Replay.IsPlaying()` | `true`, если `Play()` вызван и `Stop()` ещё нет. |
| `Replay.IsPaused()` | `true`, если воспроизведение активно **и** поставлено на паузу. |
| `Replay.GetTime()` | Текущее время курсора в секундах. |
| `Replay.SetTime(t)` | Перейти к времени `t` (ограничивается диапазоном `[0, GetDuration()]`). |
| `Replay.GetDuration()` | Общая длительность активного реплея в секундах. |
| `Replay.GetFrameCount()` | Количество кадров в активном реплее. |
| `Replay.GetProgress()` | Прогресс `0..1` (`time / duration`). |
| `Replay.SetSpeed(s)` / `GetSpeed()` | Скорость: `0.5` = slow-mo, `2.0` = быстро, `-1.0` = в обратную сторону. |
| `Replay.SetLoop(b)` / `GetLoop()` | Автоматический loop реплея. |
| `Replay.SetInterpolation(b)` / `GetInterpolation()` | Линейная интерполяция между сэмплированными кадрами (по умолчанию `true`). Выключите для снимково-точного воспроизведения. |

Во время воспроизведения движок **перезаписывает** `TransformComponent` и (при наличии) трансформ / линейную / угловую скорость тела Box2D для каждой записанной сущности. Остальные системы (спрайты, аниматор, камера) читают эти компоненты — визуальный результат следует автоматически.

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

### Что именно записывать (whitelist)

По умолчанию записываются **все** сущности с `IDComponent` и `TransformComponent`. Для больших сцен можно ограничить запись белым списком.

| Функция | Описание |
|---|---|
| `Replay.TrackAll(true\|false)` | `true` (по умолчанию) — записывать все сущности. `false` — только добавленные через `TrackEntity`. |
| `Replay.TrackEntity(entityOrUUID)` | Добавить сущность в whitelist. Принимает entity-handle (число) или UUID-строку. |
| `Replay.UntrackEntity(entityOrUUID)` | Убрать из whitelist. |
| `Replay.ClearTracked()` | Очистить whitelist. |
| `Replay.IsTracked(entityOrUUID)` | `true`, если сущность будет записана. |

```lua
Replay.TrackAll(false)
Replay.TrackEntity(player)
Replay.TrackEntity(boss)
Replay.StartRecording("duel")
```

### Пользовательские значения покадрово

Удобно для HUD реплеев (счёт, патроны, фаза) и аналитики.

| Функция | Описание |
|---|---|
| `Replay.RecordValue(key, number)` | Записать число в следующий захваченный кадр. |
| `Replay.RecordString(key, string)` | Записать строку в следующий захваченный кадр. |
| `Replay.GetValue(key)` | При воспроизведении: числовое значение на текущем курсоре или `nil`. Сохраняет последнее записанное значение до следующего изменения. |
| `Replay.GetString(key)` | Аналогично для строк. |

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

### Сохранение / загрузка

Реплеи записываются в `Saves/Replays/<name>.replay.json` внутри пользовательского хранилища. Путь санитизируется: `..` и абсолютные пути отклоняются.

| Функция | Описание |
|---|---|
| `Replay.Save(name)` | Сохранить **активный** реплей (созданный `StopRecording` / `CaptureBuffer` / `Load`). Возвращает `true` при успехе. |
| `Replay.Load(name)` | Загрузить реплей из файла в активный слот. Возвращает `true` при успехе. |
| `Replay.HasReplay()` | `true`, если есть активный реплей. |
| `Replay.Clear()` | Удалить активный реплей. |
| `Replay.Reset()` | Сбросить всё состояние (запись, буфер, воспроизведение, tracking, пользовательские значения). |

```lua
Replay.Save("match01")        -- -> Saves/Replays/match01.replay.json
Replay.Load("match01")
Replay.Play()
```

### Ручной доступ к сэмплам (ghost / кастомный рендер)

Полезно для ghost-реплеев, следов на мини-карте или отрисовки трейлов, не изменяя текущую сцену.

| Функция | Описание |
|---|---|
| `Replay.GetEntitySample(entityOrUUID, time?)` | Возвращает таблицу сэмпла на указанное время (по умолчанию — текущий курсор). Поля: `x, y, z, scaleX, scaleY, rotation, velocityX, velocityY, angularVelocity, visible, enabled, hasBody`. Возвращает `nil`, если данных нет. |
| `Replay.GetEntityPosition(entityOrUUID, time?)` | Короткий вариант: `{x, y, z}` или `nil`. |

```lua
local s = Replay.GetEntitySample(player, Replay.GetTime() - 0.5)
if s then
    DrawGhost(s.x, s.y, s.rotation, s.scaleX, s.scaleY)
end
```

### Полный пример: запись матча с просмотром

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

### Полный пример: 8-секундный killcam смерти в slow-mo

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

### Заметки и лучшие практики

- **Частота сэмплирования** определяет только частоту записи кадров; воспроизведение работает на любом FPS благодаря интерполяции. `30 Гц` — хороший выбор для длинных матчей, `60 Гц` — для быстрых экшенов / killcam.
- Запись **только фиксирует состояние** — она **не** перезапускает игровую логику. Во время воспроизведения ваши скрипты (`OnUpdate`, AI, физика) продолжают работать, а реплей перезаписывает трансформы/скорости поверх них. Для чистого режима просмотра отключайте скрипты во время воспроизведения.
- Сущность сопоставляется между записью и воспроизведением по её **UUID**, а не по entt-handle. Сущности, удалённые во время записи, просто перестанут управляться после своего последнего кадра.
- Пути `Replay.Save/Load` изолированы внутри `Saves/Replays/`. Не используйте абсолютные пути и `..`.
- **Активный реплей** живёт после `Stop()` и `Pause()` — только `Clear()` / `Reset()` / загрузка другого файла его удаляют.
- Буфер killcam хранит копии всех сэмплированных кадров, поэтому память ≈ `rate * seconds * tracked_entities`. Для огромных открытых миров используйте `TrackAll(false)` + whitelist.

---

## 59. Matchmaking — Подбор игроков

Модуль `Matchmaking` предоставляет детерминированный встроенный **матчмейкер по уровню навыка (skill-based)**, который полностью работает на стороне Lua. Он не зависит от модуля `Network`: вы наполняете пул кандидатов (реальные игроки из любого бэкенда, пиры из `Network` или локальные боты), задаёте форму матча и вызываете `StartSearch`. Матчмейкер расширяет окно по скиллу со временем, генерирует события прогресса / найденного матча / ошибки и возвращает сбалансированную команду.

**Ключевые возможности**

- **Поиск вокруг Self**: каждый поиск центрируется на скилле `Matchmaking.SetSelf{...}`; в первую очередь подбираются ближайшие по скиллу кандидаты.
- **Расширяющееся окно скилла**: начинается с `skillRange` и растёт со скоростью `expandRate` в секунду до `maxSkillRange`, пока не наберётся нужное число кандидатов.
- **Фильтры по региону и атрибутам**: `region` + `requiredAttributes` (карта строк ключ/значение) для режима игры, языка, пати-id и т.д.
- **Боты-наполнители**: `Matchmaking.AddBots(N, { minSkill, maxSkill })` — заполняют пустые слоты синтетическими кандидатами.
- **Событийная модель**: `OnSearchStarted`, `OnSearchProgress`, `OnMatchFound`, `OnSearchCancelled`, `OnSearchFailed`, `OnSearchTimeout`, `OnPlayerAdded`, `OnPlayerRemoved`.
- **Жизненный цикл**: `idle → searching → found | cancelled | failed`. Состояние можно прочитать в любой момент через `GetState()`.
- **Тикается автоматически** на каждом обновлении скрипт-движка; вызывать апдейт вручную не нужно.

> Matchmaking **не** открывает сокеты и не общается с удалённым сервисом — это локальный матчер. Соединяйте его с `Network.*` (создание лобби / прямое подключение) уже после `OnMatchFound`.

### Описание игрока

Каждая запись в пуле кандидатов — включая `Self` — это Lua-таблица:

```lua
{
    id         = "p_42",        -- уникальный строковый id (обязательно)
    name       = "Alice",       -- отображаемое имя (по умолчанию = id)
    skill      = 1500,          -- числовой MMR / ELO / рейтинг (по умолчанию 1000)
    region     = "eu",          -- регион (опционально)
    attributes = {              -- произвольная карта строк ключ/значение
        mode  = "ranked",
        party = "abc123",
    },
}
```

### Быстрый старт

```lua
Matchmaking.SetSelf{ id = "me", name = "You", skill = 1500, region = "eu" }
Matchmaking.AddBots(20, { minSkill = 1200, maxSkill = 1800, region = "eu" })

Matchmaking.OnMatchFound(function(match)
    print("Матч найден:", match.id, "средний скилл", match.averageSkill)
    for _, p in ipairs(match.players) do
        print("  ", p.name, p.skill)
    end
end)

local ticket = Matchmaking.StartSearch{
    mode             = "ranked",
    teamSize         = 4,
    skillRange       = 50,
    maxSkillRange    = 500,
    expandRate       = 25,    -- окно скилла растёт на 25 в секунду
    region           = "eu",
    requireRegion    = true,
    timeout          = 30,
    progressInterval = 0.5,
}
```

### Self / пул кандидатов

| Функция | Описание |
|---|---|
| `Matchmaking.SetSelf(player)` | Задать локального игрока. Поиск центрируется на его `skill`. |
| `Matchmaking.GetSelf()` | Возвращает таблицу локального игрока или `nil`. |
| `Matchmaking.HasSelf()` | `true`, если `SetSelf` уже был вызван. |
| `Matchmaking.ClearSelf()` | Очистить локального игрока. |
| `Matchmaking.AddPlayer(player)` → `bool` | Добавить / заменить одного кандидата. Вернёт `false`, если `id` пустой. |
| `Matchmaking.AddPlayers(arrayOfPlayers)` → `int` | Массовое добавление. Возвращает число **новых** записей. |
| `Matchmaking.AddBots(count, options?)` → `int` | Создать `count` синтетических кандидатов. `options = { minSkill=800, maxSkill=1200, region="", prefix="bot_" }`. Скиллы распределяются линейно от `minSkill` до `maxSkill`. |
| `Matchmaking.RemovePlayer(id)` → `bool` | Удалить по id. Триггерит `OnPlayerRemoved`. |
| `Matchmaking.HasPlayer(id)` → `bool` | Проверка наличия. |
| `Matchmaking.GetPlayer(id)` → `table\|nil` | Получить кандидата. |
| `Matchmaking.GetPlayers()` → `array` | Снимок всех кандидатов. |
| `Matchmaking.GetPlayerCount()` → `int` | Размер пула. |
| `Matchmaking.ClearPlayers()` | Очистить пул (на `Self` не влияет). |

### Жизненный цикл поиска

| Функция | Описание |
|---|---|
| `Matchmaking.StartSearch(options?)` → `string` | Запустить поиск и вернуть **id тикета**. Останавливает текущий поиск. Триггерит `OnSearchStarted`. |
| `Matchmaking.CancelSearch()` → `bool` | Отменить активный поиск. Триггерит `OnSearchCancelled`. Возвращает `true`, если поиск действительно шёл. |
| `Matchmaking.Fail(reason?)` → `bool` | Принудительно зафейлить активный поиск (например, бэкенд отказал). Триггерит `OnSearchFailed`. |
| `Matchmaking.IsSearching()` → `bool` | Удобный алиас для `GetState() == "searching"`. |
| `Matchmaking.GetState()` → `string` | Одно из `"idle"`, `"searching"`, `"found"`, `"cancelled"`, `"failed"`. |
| `Matchmaking.GetTicketId()` → `string` | Id тикета текущего/последнего поиска. |
| `Matchmaking.GetElapsed()` → `number` | Секунд с момента `StartSearch`. |
| `Matchmaking.GetCurrentSkillRange()` → `number` | Текущее окно скилла (растёт со временем). |
| `Matchmaking.GetCandidateCount()` → `int` | Сколько кандидатов **сейчас** удовлетворяют всем фильтрам. |
| `Matchmaking.GetMatch()` → `table\|nil` | Последний успешный матч (схема ниже). |
| `Matchmaking.GetLastFailReason()` → `string` | `"timeout"`, `"manual"` или то, что было передано в `Fail`. |
| `Matchmaking.GetOptions()` → `table` | Снимок текущих параметров поиска. |
| `Matchmaking.SetExpandRate(rate)` | Подкрутить скорость роста окна скилла прямо во время поиска. |
| `Matchmaking.SetMaxSkillRange(range)` | Подкрутить максимальное окно скилла во время поиска. |
| `Matchmaking.Reset()` | Полный сброс: пул, Self, колбэки, состояние поиска. |
| `Matchmaking.ClearCallbacks()` | Сбросить все колбэки (пул сохраняется). |

### Параметры `StartSearch`

```lua
Matchmaking.StartSearch{
    mode               = "quick",         -- произвольный ярлык, копируется в результат матча
    teamSize           = 2,               -- минимальное число игроков в результате (алиас: minPlayers)
    skillRange         = 100,             -- стартовое окно ± по скиллу вокруг Self
    maxSkillRange      = 5000,            -- верхний предел роста окна
    expandRate         = 50,              -- скорость роста окна в секунду (0 = не растёт)
    region             = "",              -- фильтр по региону
    requireRegion      = false,           -- если true — регион должен совпадать точно
    requiredAttributes = { mode = "ranked", lang = "en" },
    timeout            = 0,               -- 0 = без таймаута (секунды)
    progressInterval   = 0.5,             -- секунд между OnSearchProgress (0 = выкл)
    includeSelf        = true,            -- включать ли Self в выбранную команду
}
```

Кандидат проходит, если выполнены **все** условия:

1. `|candidate.skill - self.skill| <= currentSkillRange`
2. Если `requireRegion = true` и `region ~= ""`: `candidate.region == region`
3. Каждая пара ключ/значение из `requiredAttributes` присутствует и равна в `candidate.attributes`

### Схема результата

```lua
match = {
    id           = "match_3",
    mode         = "ranked",
    teamSize     = 4,
    averageSkill = 1487.5,
    finalRange   = 175.0,        -- окно скилла, на котором матч в итоге собрался
    elapsed      = 4.3,           -- секунд потрачено на поиск
    players      = {              -- длина = teamSize, отсортированы по близости к Self.skill
        { id = "me",   name = "You",   skill = 1500, region = "eu", attributes = {...} },
        { id = "p_7",  name = "Bob",   skill = 1480, region = "eu", attributes = {...} },
        { id = "p_15", name = "Cara",  skill = 1525, region = "eu", attributes = {...} },
        { id = "p_22", name = "Dale",  skill = 1445, region = "eu", attributes = {...} },
    },
}
```

### Колбэки

Все колбэки принимают `function`. Повторная регистрация заменяет предыдущий. Вызываются на Lua-потоке во время штатного тика рантайма — внутри них безопасно использовать любые другие Lua API.

| Колбэк | Сигнатура | Когда |
|---|---|---|
| `Matchmaking.OnSearchStarted(fn)` | `fn(ticketId)` | Сразу после `StartSearch`. |
| `Matchmaking.OnSearchProgress(fn)` | `fn(elapsed, candidateCount, currentSkillRange)` | Каждые `progressInterval` секунд во время поиска. |
| `Matchmaking.OnMatchFound(fn)` | `fn(match)` | Когда набралось достаточно подходящих кандидатов. |
| `Matchmaking.OnSearchCancelled(fn)` | `fn(ticketId)` | После `CancelSearch`. |
| `Matchmaking.OnSearchFailed(fn)` | `fn(ticketId, reason)` | После `Fail(reason)` или по таймауту. |
| `Matchmaking.OnSearchTimeout(fn)` | `fn(ticketId)` | Если `timeout > 0` и время вышло. Всегда сопровождается `OnSearchFailed(_, "timeout")`. |
| `Matchmaking.OnPlayerAdded(fn)` | `fn(id)` | В пул добавлен новый кандидат. |
| `Matchmaking.OnPlayerRemoved(fn)` | `fn(id)` | Кандидат удалён. |

### Полный пример: ranked-поиск с ботами и таймаутом

```lua
function StartRankedSearch()
    Matchmaking.SetSelf{ id = "me", name = "You", skill = GetMyMMR(), region = "eu" }
    Matchmaking.ClearPlayers()

    -- подгружаем живых кандидатов из бэкенда / Network
    for _, p in ipairs(Backend.FetchOnlinePlayers()) do
        Matchmaking.AddPlayer(p)
    end

    -- добиваем ботами, чтобы поиск гарантированно собрался в QA / оффлайн-режиме
    Matchmaking.AddBots(8, { minSkill = GetMyMMR() - 200, maxSkill = GetMyMMR() + 200, region = "eu" })

    Matchmaking.OnSearchProgress(function(t, candidates, range)
        UpdateUI(string.format("Поиск... %.1fс, %d кандидатов, ±%.0f MMR", t, candidates, range))
    end)

    Matchmaking.OnMatchFound(function(m)
        UpdateUI(string.format("Матч найден! средний MMR %.0f", m.averageSkill))
        -- Matchmaking только подбирает игроков; хостинг остаётся на тебе.
        -- Соглашение здесь: хост создаёт комнату с именем, равным id матча.
        Network.JoinRoom(m.id)
    end)

    Matchmaking.OnSearchTimeout(function()
        UpdateUI("Матч не найден, попробуйте ещё раз")
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

### Заметки и лучшие практики

- Матчер **детерминирован** относительно входных данных: один и тот же пул и те же параметры → один и тот же матч. Удобно для тестов.
- Используйте `requiredAttributes` для удержания пати вместе (`party = "<id>"`), для жёсткого режима (`mode = "ranked"`) или для совпадения по языку (`lang = "ru"`).
- `expandRate` и `maxSkillRange` — две главные ручки баланса между качеством матча и скоростью поиска. Маленький `expandRate` → строже подбор, дольше ожидание. Большой `maxSkillRange` → матч в итоге соберётся всегда, но качество подбора падает.
- `Matchmaking.AddBots` отлично подходит на этапе разработки и для "всегда-онлайн" PvE / коопа, где нужно несколько живых тел.
- Пул **не** синхронизируется автоматически с пирами `Network` — наполняйте его явно из своего сетевого слоя или бэкенда.
- После `OnMatchFound` состояние остаётся `"found"`. Для нового раунда вызовите `Matchmaking.Reset()` или просто новый `StartSearch`.
- Все колбэки переживают `CancelSearch` и `Fail`; снимаются только через `ClearCallbacks` / `Reset`.

---

## 60. Console — Консоль разработчика и система команд

**Консоль разработчика** — это выдвижной оверлей для отладки, чит-кодов и runtime-конфигурации. **Система команд** предоставляет фреймворк для регистрации консольных команд и переменных (CVar) как из C++, так и из Lua.

### 60.1 Console — Управление консолью

| Функция | Возвращает | Описание |
|---------|------------|----------|
| `Console.Show()` | — | Открыть консоль разработчика |
| `Console.Hide()` | — | Закрыть консоль |
| `Console.Toggle()` | — | Переключить консоль (открыть/закрыть) |
| `Console.IsOpen()` | `bool` | Открыта ли консоль |
| `Console.SetEnabled(enabled)` | — | Включить или выключить консоль |
| `Console.IsEnabled()` | `bool` | Включена ли консоль |
| `Console.SetToggleKey(scancode)` | — | Установить клавишу открытия (по умолчанию: `` ` ``) |
| `Console.GetToggleKey()` | `int` | Получить текущую клавишу (SDL scancode) |
| `Console.SetStyle(style)` | — | Настроить внешний вид консоли |

```lua
Console.Show()
Console.Toggle()
print("Консоль открыта:", Console.IsOpen())

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

### 60.2 Console — Регистрация команд

| Функция | Возвращает | Описание |
|---------|------------|----------|
| `Console.RegisterCommand(name, callback, description?, category?, usage?, opts?)` | — | Зарегистрировать консольную команду |
| `Console.UnregisterCommand(name)` | — | Удалить команду |
| `Console.HasCommand(name)` | `bool` | Проверить существование команды |
| `Console.GetAllCommands()` | `table` | Получить все зарегистрированные команды |

```lua
Console.RegisterCommand("godmode", function(args)
    local enabled = not isGodMode
    isGodMode = enabled
    Console.Print("Режим бога " .. (enabled and "ВКЛ" or "ВЫКЛ"), 3)
end, "Переключить режим бога", "Читы", "godmode")

Console.RegisterCommand("spawn", function(args)
    if #args < 1 then
        Console.Print("Использование: spawn <класс> [x] [y]", 1)
        return
    end
    local class = args[1]
    local x = tonumber(args[2]) or 0
    local y = tonumber(args[3]) or 0
    SpawnEntity(class, x, y, 0)
    Console.Print("Создан " .. class, 3)
end, "Создать сущность", "Отладка", "spawn <класс> [x] [y]")
```

**Таблица опций команды** (необязательный 6-й аргумент):

| Поле | Тип | Описание |
|------|-----|----------|
| `cheat` | `bool` | Требует включённые читы |
| `hidden` | `bool` | Скрыть из help/cvarlist/автодополнения |
| `devOnly` | `bool` | Доступна только в режиме разработчика |
| `autocomplete` | `function` | `function(partial) -> table` — строки-кандидаты для дополнения по Tab |

```lua
Console.RegisterCommand("warp", function(args)
    if #args >= 1 then LoadLevel(args[1]) end
end, "Перейти на уровень", "Отладка", "warp <уровень>", {
    devOnly = true,
    autocomplete = function(partial)
        return { "level1", "level2", "boss_arena" }
    end
})
```

### 60.3 Console — CVar (Консольные переменные)

| Функция | Возвращает | Описание |
|---------|------------|----------|
| `Console.RegisterCVar(name, defaultValue, description?, category?, opts?)` | — | Зарегистрировать консольную переменную |
| `Console.UnregisterCVar(name)` | — | Удалить CVar |
| `Console.HasCVar(name)` | `bool` | Проверить существование CVar |
| `Console.GetCVar(name)` | `any` | Получить значение (автоматический тип) |
| `Console.SetCVar(name, value)` | — | Установить значение (автоматический тип) |
| `Console.GetCVarBool(name)` | `bool` | Получить как bool |
| `Console.GetCVarInt(name)` | `int` | Получить как int |
| `Console.GetCVarFloat(name)` | `float` | Получить как float |
| `Console.GetCVarString(name)` | `string` | Получить как string |
| `Console.SetCVarBool(name, value)` | — | Установить как bool |
| `Console.SetCVarInt(name, value)` | — | Установить как int |
| `Console.SetCVarFloat(name, value)` | — | Установить как float |
| `Console.SetCVarString(name, value)` | — | Установить как string |
| `Console.GetAllCVars()` | `table` | Получить все CVar |

**Таблица опций CVar:**

| Поле | Тип | Описание |
|------|-----|----------|
| `readOnly` | `bool` | Запретить изменение из консоли |
| `hidden` | `bool` | Скрыть из help/cvarlist |
| `archive` | `bool` | Сохранять в конфиг |
| `cheat` | `bool` | Требует включённые читы |
| `devOnly` | `bool` | Доступна только в режиме разработчика (иначе скрыта/заблокирована) |
| `noLog` | `bool` | Не выводить эхо при изменении значения |
| `min` | `number` | Минимальное значение (int/float) |
| `max` | `number` | Максимальное значение (int/float) |
| `onChange` | `function` | Колбэк при изменении значения |

```lua
Console.RegisterCVar("player_speed", 200.0, "Скорость движения игрока", "Геймплей", {
    min = 10.0,
    max = 1000.0,
    archive = true,
    onChange = function(oldVal, newVal)
        Console.Print("Скорость изменена на " .. tostring(newVal))
    end
})

Console.RegisterCVar("god_mode", false, "Режим бога", "Читы", { cheat = true })
Console.RegisterCVar("game_version", "1.0.0", "Версия игры", "Инфо", { readOnly = true })

local speed = Console.GetCVarFloat("player_speed")
Console.SetCVarFloat("player_speed", speed * 1.5)
```

### 60.4 Console — Выполнение и логирование

| Функция | Возвращает | Описание |
|---------|------------|----------|
| `Console.Execute(commandLine)` | — | Выполнить строку команды |
| `Console.Print(message, level?)` | — | Вывести сообщение в консоль |
| `Console.Clear()` | — | Очистить лог консоли |
| `Console.GetLog()` | `table` | Получить все записи лога |
| `Console.GetInputHistory()` | `table` | Получить историю ввода команд |

**Уровни лога:** `0` = Инфо, `1` = Предупреждение, `2` = Ошибка, `3` = Успех

```lua
Console.Execute("god_mode true")
Console.Execute("player_speed 300")

Console.Print("Привет из Lua!", 0)
Console.Print("Что-то не так!", 1)
Console.Print("Критическая ошибка!", 2)
Console.Print("Операция успешна!", 3)
```

### 60.5 Console — Читы и алиасы

| Функция | Возвращает | Описание |
|---------|------------|----------|
| `Console.SetCheatsEnabled(enabled)` | — | Включить/выключить чит-команды |
| `Console.AreCheatsEnabled()` | `bool` | Проверить, включены ли читы |
| `Console.SetDeveloperMode(enabled)` | — | Включить/выключить команды и CVar «только для разработчика» |
| `Console.IsDeveloperMode()` | `bool` | Проверить режим разработчика (по умолчанию вкл. в debug, выкл. в release) |
| `Console.RegisterAlias(alias, target)` | — | Создать алиас команды |
| `Console.GetCategories()` | `table` | Получить все категории команд/CVar |

```lua
Console.SetCheatsEnabled(true)
Console.RegisterAlias("gm", "godmode")
Console.RegisterAlias("sp", "spawn")
```

### 60.6 Console — Сохранение настроек

| Функция | Возвращает | Описание |
|---------|------------|----------|
| `Console.ArchiveCVars(filePath)` | — | Сохранить все архивируемые CVar в JSON |
| `Console.LoadArchivedCVars(filePath)` | — | Загрузить сохранённые CVar из JSON |

```lua
function OnGameStart()
    Console.LoadArchivedCVars("Config/Console.cfg")
end

function OnGameExit()
    Console.ArchiveCVars("Config/Console.cfg")
end
```

> **Пути резолвятся относительно `Content/` и работают в песочнице.** `exec`, `ArchiveCVars` и `LoadArchivedCVars` отклоняют абсолютные пути и обход `..`; имя вроде `"Config/Console.cfg"` резолвится внутри `Content/`.

### 60.7 Встроенные команды

| Команда | Использование | Описание |
|---------|---------------|----------|
| `help` | `help [имя]` | Список всех команд или справка по конкретной |
| `clear` | `clear` | Очистить лог консоли |
| `echo` | `echo <сообщение>` | Вывести сообщение |
| `find` | `find <шаблон>` | Поиск команд и CVar по имени |
| `exec` | `exec <файл>` | Выполнить команды из конфига |
| `alias` | `alias <имя> <команда>` | Создать алиас |
| `cheats` | `cheats <on\|off>` | Включить/выключить читы |
| `developer` | `developer <on\|off>` | Включить/выключить команды и CVar «только для разработчика» |
| `cvarlist` | `cvarlist [категория]` | Список всех CVar |
| `cmdlist` | `cmdlist [категория]` | Список всех команд |
| `reset` | `reset <cvar>` | Сбросить CVar к значению по умолчанию |
| `toggle` | `toggle <cvar>` | Переключить булеву CVar |

### 60.8 Горячие клавиши консоли

| Клавиша | Действие |
|---------|----------|
| `` ` `` (тильда) | Открыть/закрыть консоль |
| `Enter` | Выполнить команду |
| `Tab` | Автодополнение |
| `Up/Down` | Навигация по истории команд |
| `PageUp/PageDown` | Прокрутка лога |
| `Колёсико мыши` | Прокрутка лога |
| `Escape` | Закрыть автодополнение / закрыть консоль |
| `Ctrl+A` | Выделить весь текст |
| `Ctrl+C` | Копировать выделенный текст |
| `Ctrl+V` | Вставить из буфера обмена |
| `Ctrl+X` | Вырезать выделенный текст |

### 60.9 Полный пример

```lua
local debugEnabled = false

function OnConstruct()
    Console.RegisterCVar("debug_overlay", false, "Показать оверлей отладки", "Отладка", {
        archive = true,
        onChange = function(old, new)
            debugEnabled = new
        end
    })

    Console.RegisterCVar("spawn_count", 10, "Количество сущностей для создания", "Отладка", {
        min = 1, max = 100
    })

    Console.RegisterCommand("spawn_wave", function(args)
        local count = Console.GetCVarInt("spawn_count")
        for i = 1, count do
            local x = math.random(-200, 200)
            local y = math.random(-200, 200)
            SpawnEntity("Enemy", x, y, 0)
        end
        Console.Print("Создано " .. count .. " врагов", 3)
    end, "Создать волну врагов", "Отладка", "spawn_wave")

    Console.RegisterCommand("fps_max", function(args)
        if #args < 1 then
            Console.Print("Использование: fps_max <значение>", 1)
            return
        end
        Console.Print("Лимит FPS установлен на " .. args[1], 3)
    end, "Установить лимит FPS", "Графика")

    Console.LoadArchivedCVars("Config/Console.cfg")
end

function OnDestroy()
    Console.ArchiveCVars("Config/Console.cfg")
end
```

---

## 61. Draw — Немедленная отрисовка (Draw / Texture / RenderTarget)

> `Draw` отправляет текстурированные квады и произвольные треугольные меши прямо в батч-рендерер, без создания сущностей.
> Всё, что отправлено за кадр, рисуется один раз и сбрасывается — вызывайте заново каждый кадр из `OnUpdate`.
>
> Это самый быстрый способ отрисовать большой объём сгенерированной геометрии: **один вызов Lua может отправить тысячи квадов**,
> тогда как управление таким же числом спрайтовых сущностей стоило бы нескольких вызовов Lua→C++ на каждую.
>
> Типичное применение: процедурная графика, кастомные частицы, внутриигровые редакторы уровней, миникарты, отладочные оверлеи,
> тайловые и колоночные рендереры, а также псевдо-3D техники (raycasting, пол в стиле Mode-7, дорога с перспективой),
> полностью остающиеся в 2D.

### Соглашения о координатах

`Draw` полностью следует соглашениям движка:

- **X+** вправо, **X-** влево.
- **Y+** вверх, **Y-** вниз.
- **Поворот** в градусах; **положительный — по часовой стрелке**, отрицательный — против (как у `SetSpriteLocalRotation`).
- **Z+** к зрителю (передний план), **Z-** от зрителя (задний план) — как у `SetSpriteOrder`.
  Геометрия в мировом пространстве проходит тест глубины вместе со спрайтами, тайлмапами и всем остальным в сцене,
  поэтому Z бесплатно даёт корректное попиксельное перекрытие.
- **Пивот** `px, py` нормализован `0..1`, причём **`py` отсчитывается сверху** — точно как у `SetSpritePivot`.
  По умолчанию пивот `0.5, 0.5` (центр), поэтому `x, y` — это центр квада, пока вы не измените пивот.
- **UV** `u, v, uw, vh` нормализованы `0..1` ровно в том же пространстве, что и `SetSpriteRegion`, делённое на размер
  текстуры: `v = 0` — это **верх** изображения, и картинка выводится не перевёрнутой, точно как у спрайта.
  Вместо них можно передать `sx, sy, sw, sh` — исходный прямоугольник **в пикселях**.
  У `Draw.Mesh` соглашение намеренно другое — там каждая вершина несёт сырую координату выборки, поэтому `v = 0`
  читает верхнюю строку изображения, а какой вершине это соответствует — решаете вы.

### Пространства

```lua
Draw.SetSpace("world")   -- по умолчанию: мировые единицы, тест глубины, движется с камерой
Draw.SetSpace("screen")  -- пиксели экрана, начало в ЛЕВОМ НИЖНЕМ углу, без теста глубины, порядок отправки
local space = Draw.GetSpace()
```

Геометрия `world` рисуется вместе со сценой (после частиц, до тумана войны и виджетов), поэтому на неё действует
пост-обработка и она участвует в буфере глубины.
Геометрия `screen` игнорирует камеру и буфер глубины и рисуется в порядке отправки.

### Состояние отрисовки

Состояние глобальное и сохраняется между вызовами: настраиваете один раз, отправляете много примитивов.

```lua
Draw.SetTexture("Content/Textures/wall.png")  -- текстура по умолчанию; nil = белая
Draw.SetColor(1, 1, 1, 1)                     -- цвет по умолчанию
Draw.SetZ(0)                                  -- глубина по умолчанию
Draw.SetPivot(0.5, 0.5)                       -- пивот по умолчанию (py сверху)
Draw.SetBlend("masked")                       -- "masked" | "additive" | "translucent" | "opaque"
Draw.SetShading("unlit")                      -- "unlit" (по умолчанию) | "lit"
Draw.SetAlphaClip(0.5)                        -- порог отсечения альфы для "masked"
Draw.SetTarget(nil)                           -- nil = экран, либо имя RenderTarget

Draw.GetTexture(); Draw.GetColor(); Draw.GetZ(); Draw.GetPivot()
Draw.GetBlend(); Draw.GetShading(); Draw.GetAlphaClip(); Draw.GetTarget()

Draw.Push()   -- сохранить всё состояние (глубина стека до 64)
Draw.Pop()    -- восстановить
Draw.Reset()  -- вернуть значения по умолчанию и очистить стек
```

### Примитивы

```lua
-- Один квад со всеми параметрами. Любое поле можно опустить.
Draw.Quad{
    x = 100, y = 200,      -- точка пивота (по умолчанию центр)
    w = 64,  h = 64,       -- размер в мировых/экранных единицах
    z = 0,                 -- глубина (Z+ = передний план)
    rot = 0,               -- градусы, по часовой стрелке положительно
    px = 0.5, py = 0.5,    -- пивот, py сверху
    u = 0, v = 0, uw = 1, vh = 1,       -- нормализованный UV-прямоугольник
    -- либо пиксельный исходный прямоугольник вместо u/v/uw/vh:
    -- sx = 0, sy = 0, sw = 16, sh = 16,
    r = 1, g = 1, b = 1, a = 1,         -- цвет
    texture = "Content/Textures/x.png", -- своя текстура для этого квада
}

-- Сплошной прямоугольник без текстуры. x, y — ЛЕВЫЙ НИЖНИЙ угол.
Draw.Rect(x, y, w, h, r, g, b, a, z)

-- Текстурированный спрайт; w/h по умолчанию равны размеру текстуры, пивот из состояния.
Draw.Sprite("Content/Textures/hero.png", x, y, w, h, rotation, z)

-- Текстурированный квад с исходным прямоугольником в пикселях.
Draw.Region("Content/Textures/atlas.png", x, y, w, h, sx, sy, sw, sh, rotation, z)

-- Линия, нарисованная как повёрнутый квад.
Draw.Line(x1, y1, x2, y2, thickness, r, g, b, a, z)
```

### Массовая отправка

```lua
-- Массив таблиц-квадов. Возвращает количество отправленных.
local n = Draw.Quads("Content/Textures/wall.png", {
    { x = 0, y = 0, w = 1, h = 100 },
    { x = 1, y = 0, w = 1, h = 120 },
})

-- То же самое, с текстурой из Draw.SetTexture:
Draw.Quads(quadList)
```

`Draw.QuadsPacked` — самый быстрый путь: **плоский массив чисел**, по 14 на квад, ровно в таком порядке:

```
x, y, w, h, z, rot, u, v, uw, vh, r, g, b, a
```

```lua
local data = {}
local i = 0
for col = 0, 319 do
    local h = ColumnHeight(col)
    data[i+1]  = col        -- x
    data[i+2]  = 180        -- y (центр)
    data[i+3]  = 1          -- w
    data[i+4]  = h          -- h
    data[i+5]  = 0          -- z
    data[i+6]  = 0          -- rot
    data[i+7]  = TexU(col)  -- u
    data[i+8]  = 0          -- v
    data[i+9]  = 1 / 64     -- uw (один столбец текселей текстуры 64 px)
    data[i+10] = 1          -- vh
    data[i+11] = 1; data[i+12] = 1; data[i+13] = 1; data[i+14] = 1  -- rgba
    i = i + 14
end
Draw.QuadsPacked("Content/Textures/wall.png", data)   -- 320 колонн, один вызов Lua
```

Необязательный третий аргумент ограничивает число читаемых квадов: `Draw.QuadsPacked(path, data, count)`.

### Меши

`Draw.Mesh` отправляет произвольный треугольный меш с UV на каждую вершину — именно это позволяет строить трапеции с
перспективой, пол в стиле Mode-7, изогнутые дороги и свободные деформации, не выходя из 2D.

```lua
Draw.Mesh(texturePath, positions, uvs, indices, options)
```

- `positions` — плоский массив `{x1, y1, x2, y2, ...}` (минимум 3 вершины, максимум 65535).
- `uvs` — плоский массив `{u1, v1, u2, v2, ...}` с тем же числом вершин. Необязателен; если опущен — все нули.
- `indices` — плоский массив индексов вершин, **нумерация с 1**, кратно 3. Необязателен; если опущен — строится веер треугольников.
- `options` — необязательная таблица: `{ r, g, b, a, z, blend, shading, alphaClip }`.

```lua
-- Текстурированная трапеция: широкая снизу, узкая сверху (сегмент дороги).
Draw.Mesh("Content/Textures/road.png",
    { -100, 0,  100, 0,  40, 60,  -40, 60 },   -- позиции
    {    0, 1,    1, 1,   1, 0,     0, 0 },    -- uv
    { 1, 2, 3,  1, 3, 4 },                     -- индексы (с 1)
    { z = -10 })
```

Идущие подряд меши с одинаковой текстурой и состоянием объединяются в один draw call.

### Лимиты и статистика

```lua
Draw.GetViewportSize()     -- { width, height } поверхности, куда рисует Draw,
                           -- в тех же единицах, что и экранное пространство.
                           -- Используйте это для вёрстки HUD, а не разрешение окна.
Draw.GetQuadCount()        -- квадов отправлено за кадр
Draw.GetCommandCount()     -- батч-команд за кадр
Draw.GetMeshVertexCount()  -- вершин мешей за кадр
Draw.DidOverflow()         -- true, если лимит превышен и геометрия была отброшена

Draw.SetMaxQuads(200000)        -- лимит квадов на кадр (по умолчанию 200000)
Draw.SetMaxMeshVertices(400000) -- лимит вершин мешей на кадр (по умолчанию 400000)
Draw.GetMaxQuads(); Draw.GetMaxMeshVertices()

Draw.Clear()  -- отбросить всё отправленное в этом кадре
```

Список очищается автоматически в начале каждого кадра и при выгрузке сцены.

### Texture — текстуры, созданные из скрипта

Такие текстуры регистрируются под своим именем, поэтому **любой API, принимающий путь к текстуре, принимает и это имя**:
`SetSpriteTexture`, `Material.SetTexture`, `PP.SetCustomMaterialTexture`, `Draw.SetTexture`, виджеты и так далее.

```lua
Texture.Create("minimap", 256, 256, {
    r = 0, g = 0, b = 0, a = 0,   -- цвет начальной заливки
    filter = "nearest",           -- "nearest" (по умолчанию) | "linear"
    wrap = "clamp",               -- "clamp" (по умолчанию) | "repeat"
})

Texture.Exists("minimap")
Texture.GetSize("minimap")            -- возвращает { width, height }
Texture.Destroy("minimap")
Texture.GetCount()

Texture.Fill("minimap", 0, 0, 0, 1)                    -- залить всю текстуру
Texture.SetPixel("minimap", x, y, r, g, b, a)          -- один пиксель, компоненты 0..1
Texture.SetPixels("minimap", x, y, w, h, floats)       -- RGBA float 0..1, w*h*4 значений
Texture.SetPixelBytes("minimap", x, y, w, h, bytes)    -- RGBA байты 0..255, w*h*4 значений

Texture.SetFilter("minimap", "linear")
Texture.SetWrap("minimap", "repeat")
Texture.GenerateMipmaps("minimap")
```

`SetPixels` и `SetPixelBytes` ожидают область построчно, по 4 компонента на пиксель, начиная с `x, y`.
Это классический путь софтверного рендерера: собрать буфер пикселей в Lua, загрузить его одним вызовом и нарисовать одним квадом.

### RenderTarget — отрисовка в текстуру

Render target — это текстура, в которую можно рисовать. Она тоже регистрируется по имени, поэтому её могут читать спрайты и материалы.

```lua
RenderTarget.Create("mirror", 512, 512, { filter = "linear", wrap = "clamp" })
RenderTarget.Exists("mirror")
RenderTarget.GetSize("mirror")
RenderTarget.Destroy("mirror")

RenderTarget.Clear("mirror", r, g, b, a, clearDepth)   -- clearDepth по умолчанию true
RenderTarget.CaptureScene("mirror")                    -- скопировать в неё отрисованную сцену этого кадра
RenderTarget.ReadPixels("mirror", x, y, w, h)          -- плоский массив байтов RGBA (медленно, тормозит GPU)

-- Направить немедленную геометрию в цель:
Draw.SetTarget("mirror")
Draw.Rect(0, 0, 512, 512, 0, 0, 0, 1)
Draw.SetTarget(nil)
```

Геометрия render target рисуется **до** основной сцены каждый кадр, поэтому цель, заполненная в этом кадре, уже может быть
прочитана спрайтами и материалами в том же кадре. В экранном пространстве используется собственный пиксельный прямоугольник
цели (начало в левом нижнем углу); в мировом — текущая проекция камеры растягивается на цель.

Буфер глубины render target **не** очищается автоматически — вызывайте `RenderTarget.Clear` каждый кадр, если рисуете туда
мировую геометрию с тестом глубины.

### Полный пример — текстурные колонны в стиле Wolfenstein

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
        local dist, texU = CastRay(px, py, rayAngle)   -- ваш DDA на чистом Lua
        local height = 12000 / math.max(dist, 0.1)
        local shade  = math.max(0.2, 1 - dist / 20)

        data[i+1]  = col; data[i+2] = 180
        data[i+3]  = 1;   data[i+4] = height
        data[i+5]  = -dist            -- Z: дальние стены уходят назад
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
