# IceBox Engine — Documentation

Official technical documentation for **IceBox Engine**, a modular 2D game engine built with modern C++ and open-source libraries.

The documentation is available in two languages:

| Language | Folder | Description |
| -------- | ------ | ----------- |
| 🇬🇧 English  | [`EN/`](EN) | Primary reference set. |
| 🇷🇺 Русский  | [`RU/`](RU) | Полный перевод всей документации. |

Every document is also readable **inside the editor** through the `Help → Documentation` panel, which supports full-text search, a table-of-contents sidebar, scrollable tables, clickable links, and the same font stack (FreeType) used by the rest of the ImGui editor.

---

<a id="english"></a>
## 🇬🇧 English documentation

A short guide to each document in the [`EN/`](EN) folder:

| Document | What's inside |
| -------- | ------------- |
| [Getting-Started-EN-DOC.md](EN/Getting-Started-EN-DOC.md) | Getting started — the Launcher & Updater: installing the engine, the three apps, the version scheme, activating your license key, creating & managing projects, attaching plugins & mods, and checking, downloading & installing engine updates. |
| [Editor-EN-DOC.md](EN/Editor-EN-DOC.md) | The editor & interface — every menu, the toolbar, the Viewport, Level Outliner, Properties, World Settings, Console, Preferences, Network Manager, Remote Preview, Help panels, dialogs and shortcuts. |
| [Graphics-EN-DOC.md](EN/Graphics-EN-DOC.md) | Graphics, rendering & physics — the RHI & backends, render graph, 2D batch renderer, lighting, 2D shadows, ray-traced GI, post-processing, and the Box2D physics simulation. |
| [Engine-EN-DOC.md](EN/Engine-EN-DOC.md) | Engine, multiplayer & physics — the runtime architecture and frame loop, the job system, determinism, **multiplayer** (split-screen/local and online: replication, rollback, voice, discovery, security), **advanced physics** beyond the graphics document, and the audio and input stacks. |
| [LuaAPI-EN-DOC.md](EN/LuaAPI-EN-DOC.md) | Complete Lua API reference — every module, class, and function exposed to gameplay scripts. |
| [PythonAPI-EN-DOC.md](EN/PythonAPI-EN-DOC.md) | Editor automation Python API: tooling, asset manipulation, custom build steps. |
| [Assets-EN-DOC.md](EN/Assets-EN-DOC.md) | The asset system & Content Browser — every asset type, sidecars, redirectors, importing, and editing. |
| [Profiling-And-Building-EN-DOC.md](EN/Profiling-And-Building-EN-DOC.md) | Profiling (editor & runtime) and building games for all six platforms — cooking, packing, manifests, installers, DLC. |
| [Plugins-And-Mods-EN-DOC.md](EN/Plugins-And-Mods-EN-DOC.md) | The plugin & mod systems — authoring native C++ plugins and Lua/content mods: APIs, manifests, lifecycle, the editor panel, and shipping. |

---

<a id="russian"></a>
## 🇷🇺 Русская документация

Краткий обзор документов из папки [`RU/`](RU):

| Документ | О чём |
| -------- | ----- |
| [Getting-Started-RU-DOC.md](RU/Getting-Started-RU-DOC.md) | Начало работы — Лаунчер и Апдейтер: установка движка, три приложения, схема версий, активация лицензионного ключа, создание проектов и управление ими, подключение плагинов и модов, проверка, загрузка и установка обновлений движка. |
| [Editor-RU-DOC.md](RU/Editor-RU-DOC.md) | Редактор и интерфейс — все меню, панель инструментов, Вьюпорт, Иерархия уровня, Свойства, Настройки мира, Консоль, Настройки, Менеджер сети, Удалённый предпросмотр, панели Справки, диалоги и горячие клавиши. |
| [Graphics-RU-DOC.md](RU/Graphics-RU-DOC.md) | Графика, рендеринг и физика — RHI и бэкенды, граф рендеринга, 2D-пакетный рендерер, освещение, 2D-тени, GI трассировкой лучей, пост-обработка и симуляция физики на Box2D. |
| [Engine-RU-DOC.md](RU/Engine-RU-DOC.md) | Движок, мультиплеер и физика — архитектура рантайма и цикл кадра, система задач, детерминизм, **мультиплеер** (сплит-скрин/локальный и онлайновый: репликация, rollback, голос, поиск серверов, безопасность), **продвинутая физика** сверх документа по графике, а также стеки аудио и ввода. |
| [LuaAPI-RU-DOC.md](RU/LuaAPI-RU-DOC.md) | Полный справочник Lua API — все модули, классы и функции, доступные игровым скриптам. |
| [PythonAPI-RU-DOC.md](RU/PythonAPI-RU-DOC.md) | Python API для автоматизации редактора: инструменты, работа с ассетами, кастомные шаги сборки. |
| [Assets-RU-DOC.md](RU/Assets-RU-DOC.md) | Система ассетов и Контент-браузер — все типы ассетов, сайдкары, редиректоры, импорт и редактирование. |
| [Profiling-And-Building-RU-DOC.md](RU/Profiling-And-Building-RU-DOC.md) | Профайлинг (в редакторе и рантайме) и сборка игр под все шесть платформ — подготовка, упаковка, манифесты, инсталляторы, DLC. |
| [Plugins-And-Mods-RU-DOC.md](RU/Plugins-And-Mods-RU-DOC.md) | Системы плагинов и модов — создание нативных C++-плагинов и Lua/контент-модов: API, манифесты, жизненный цикл, панель редактора и поставка. |

---

<sub>© IceBoxCrew Studio. All rights reserved. See [`LICENSE.txt`](../LICENSE.txt) for full terms.</sub>
