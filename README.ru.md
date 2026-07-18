<p align="center">
  <img src="Tools/Helpers/logoIceBox.png" alt="IceBox Engine Logo" width="200">
</p>

<h1 align="center">🧊 IceBox Engine™</h1>

<p align="center">
  <a href="README.md">🇬🇧 English</a> &nbsp;•&nbsp; <strong>🇷🇺 Русский</strong>
</p>

<p align="center">
  <strong>Мощный модульный 2D игровой движок на современном C++ и open-source библиотеках</strong>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/C%2B%2B-26-blue?style=for-the-badge&logo=cplusplus" alt="C++26">
  <img src="https://img.shields.io/badge/CMake-4.3%2B-064F8C?style=for-the-badge&logo=cmake" alt="CMake 4.3+">
  <img src="https://img.shields.io/badge/vcpkg-Managed-purple?style=for-the-badge" alt="vcpkg">
  <img src="https://img.shields.io/badge/License-Proprietary-red?style=for-the-badge" alt="Proprietary">
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Platform-Windows%20%7C%20Linux%20%7C%20macOS-lightgrey?style=flat-square" alt="Platform">
  <img src="https://img.shields.io/badge/Runtime-Windows%20%7C%20Linux%20%7C%20macOS%20%7C%20iOS%20%7C%20Android%20%7C%20Web-green?style=flat-square" alt="Runtime">
</p>

---

## 🧊 О движке

IceBox Engine — это кроссплатформенный 2D игровой движок, предназначенный для создания игр в любом визуальном стиле — от простых проектов в пиксель-арте до 2D-игр с разрешением 4K HD и богатыми визуальными эффектами. Движок включает полнофункциональный визуальный редактор, лаунчер проектов, автоматический апдейтер и легковесный рантайм для поставки готовых игр игрокам.

**Скриптинг:**
- **Lua** — игровой скриптовый язык для игровой логики, UI, ИИ и многого другого
- **Python** — скриптинг на стороне движка для инструментов редактора и автоматизации

---

## ✨ Возможности

- **Рендеринг** — Data-driven 2D-рендерер с нодовым **редактором материалов** (инстансы и функции), пост-обработкой / **FX** и мультибэкендовым RHI: OpenGL 4.6, OpenGL ES, Vulkan, Metal (через ANGLE и MoltenVK), WebGL 2 и WebGPU.
- **Сцены и ECS** — Ядро на основе Entity-Component-System (EnTT), аутлайнер уровней, переиспользуемые классы сущностей и редактор свойств/мира.
- **2D-физика** — Твёрдые тела, коллайдеры и сочленения на базе Box2D.
- **Спрайты и тайлмапы** — Редактор спрайтов, слайсер спрайт-листов, флипбук-анимация и отдельные редакторы тайлмапов / тайлсетов.
- **Анимация** — Скелетная анимация, флипбуки и клипы по таймлайну.
- **Текст и UI** — Внутренние UI-виджеты и высококачественный текст через FreeType, HarfBuzz и FriBidi (полный Unicode-шейпинг с поддержкой письма справа налево).
- **Аудио** — Микширование и воспроизведение с поддержкой кодеков Opus / Vorbis.
- **Скриптинг** — Игровой скриптинг на Lua со встроенным отладчиком, **визуальный нодовый** редактор и Python для инструментов редактора.
- **ИИ** — Поиск пути, деревья поведения и навигация.
- **Сеть** — Надёжный UDP (ENet) плюс WebSocket-транспорт (IXWebSocket) для игры в браузере/через сервер, с криптографией через libsodium.
- **Видео** — Воспроизведение видео и редактор кат-сцен / кинематографики (FFmpeg).
- **Локализация** — 14 встроенных языков с поддержкой письма справа налево, редактируемых из панели локализации.
- **Расширяемость** — Drop-in система **плагинов** и поддержка **модов**.
- **Инструментарий** — Встроенный профилировщик Tracy, оверлеи статистики, редактор горячих клавиш и пайплайн сборки игры **Build Game** в один клик для всех поддерживаемых платформ.

---

## 🎮 Поддержка платформ

| Платформа | Разработка | Runtime |
|----------|:-----------:|:-------:|
| **Windows** | ✅ | ✅ |
| **Linux** | ✅ | ✅ |
| **macOS** | ✅ | ✅ |
| **iOS** | ❌ | ✅ |
| **Android** | ❌ | ✅ |
| **Web** | ❌ | ✅ |

---

## 🏗️ Архитектура

IceBox Engine состоит из нескольких компонентов:

| Компонент | Бинарник | Описание |
|-----------|--------|-------------|
| **Launcher** | `IceBoxLauncher` | Точка входа для пользователей. Управляет проектами (создание, открытие, удаление), проверяет обновления движка и запускает редактор для выбранного проекта. |
| **Editor** | `IceBoxEngine` | Основной визуальный редактор. Редактирование сцен, управление ассетами, редактор тайлмапов, инструменты анимации, рабочая область для скриптинга и пайплайн сборки игры (Tools → Build Game). |
| **Updater** | `IceBoxUpdater` | Фоновая служба обновлений. Загружает и применяет патчи движка автоматически. |
| **Runtime** | `IceBoxRuntime` | Легковесный исполняемый файл без редактора, поставляемый со собранными играми. Запускает игровой проект напрямую на целевой платформе. |

---

## 🚧 Статус проекта

**IceBox Engine находится в активной разработке.** Основные системы, инструменты редактора и архитектура непрерывно развиваются вместе с новыми функциями и улучшениями.

---

## 💻 Системные требования

### Движок и редактор (десктоп)

| | |
|-|-|
| **ОС** | Windows (x64/x86), Linux (x64/x86) или macOS 11.0+ (Apple Silicon или Intel) |
| **CPU** | Двухъядерный процессор |
| **RAM** | 4 ГБ |
| **GPU** | Совместимая с OpenGL 3.3/4.6 или Vulkan 1.1-1.4 (Windows / Linux) или GPU с поддержкой Metal (macOS, через ANGLE или MoltenVK), 512 МБ видеопамяти |
| **Диск** | 5–10 ГБ свободного места |

### Runtime — iOS

| | |
|-|-|
| **ОС** | iOS 13.0+ (iPhone и iPad, arm64) |
| **GPU** | Metal (рендеринг через MoltenVK) |

### Runtime — Android

| | |
|-|-|
| **ОС** | Android 7.0+ (API 24) |
| **GPU** | OpenGL ES 3.2/3.0 или Vulkan 1.1-1.4 |

### Runtime — Web

| | |
|-|-|
| **Браузер** | Любой современный браузер с поддержкой WebGL 2.0 или WebGPU |

---

## 📦 Требования для сборки

### Для сборки игр (Tools → Build Game)

| Целевая платформа | Дополнительные требования |
|-----------------|----------------------|
| 🪟 **Windows** | *(ничего дополнительно — те же инструменты, что и выше)* |
| 🐧 **Linux** | WSL2 (если собирается из Windows) или нативный GCC/Clang + Ninja |
| 🍎 **macOS** | macOS-хост с Xcode 15+ Command Line Tools, vcpkg с триплетами `arm64-osx` / `x64-osx` |
| 📱 **iOS** | macOS-хост с Xcode 15+ (полная IDE, не только CLI tools), vcpkg с триплетом `arm64-ios`, аккаунт Apple Developer для деплоя на устройство |
| 🤖 **Android** | Android SDK 36+, NDK 29+, Java JDK 25+, Gradle 9.4.0 *(скачивается автоматически)* |
| 🌐 **Web** | [Emscripten SDK](https://emscripten.org/) |

---

## ⚙️ Быстрая настройка

### Windows

```bash
# Установить vcpkg
git clone https://github.com/microsoft/vcpkg C:\dev\vcpkg
C:\dev\vcpkg\bootstrap-vcpkg.bat
set VCPKG_ROOT=C:\dev\vcpkg
```

### Linux / WSL2

```bash
# 1. Установить все системные зависимости (одной командой)
sudo apt update && sudo apt install -y \
    build-essential cmake ninja-build git curl zip unzip tar pkg-config nasm xdg-utils \
    autoconf autoconf-archive automake libtool \
    python3-dev python3-venv \
    rsync gdb nsis imagemagick \
    libx11-dev libxft-dev libxext-dev libxrandr-dev libxcursor-dev libxi-dev libxfixes-dev libxss-dev libxtst-dev \
    libxkbcommon-dev libwayland-dev wayland-protocols libdecor-0-dev \
    libibus-1.0-dev \
    libgl1-mesa-dev libegl1-mesa-dev libgles2-mesa-dev \
    libasound2-dev libpulse-dev \
    libdbus-1-dev \
    libssl-dev zenity libespeak-ng-dev

# 2. Установить vcpkg
git clone https://github.com/microsoft/vcpkg ~/vcpkg
~/vcpkg/bootstrap-vcpkg.sh
echo 'export VCPKG_ROOT=~/vcpkg' >> ~/.bashrc && source ~/.bashrc
```

### Инструменты сборки macOS / iOS (требуется Apple-хост)

> **Примечание:** Цели macOS и iOS должны собираться **на macOS-хосте**. Машины с Windows / Linux не могут кросс-компилировать для платформ Apple, потому что SDK от Apple (Metal, UIKit, Cocoa) и `xcodebuild` доступны только в macOS.

```bash
# 1. Установить Xcode (полная IDE из Mac App Store) и принять лицензию
sudo xcodebuild -license accept

# 2. Установить зависимости командной строки (рекомендуется Homebrew)
brew install cmake ninja git

# 3. Установить vcpkg
git clone https://github.com/microsoft/vcpkg ~/vcpkg
~/vcpkg/bootstrap-vcpkg.sh
echo 'export VCPKG_ROOT=~/vcpkg' >> ~/.zshrc && source ~/.zshrc
```

### Инструменты сборки Android (Windows)

```bash
# 1. Установить Android SDK + NDK 29 через Android Studio или command-line tools
# 2. Установить Java JDK 25+
# 3. Установить переменные окружения:
set ANDROID_HOME=C:\Users\%USERNAME%\AppData\Local\Android\Sdk
set ANDROID_NDK_ROOT=%ANDROID_HOME%\ndk\29.0.14206865
set JAVA_HOME=C:\Program Files\Java\jdk-25

# Gradle 9.4.0 скачивается автоматически скриптом сборки, если не установлен.
```

### Инструменты сборки Android (Linux / WSL2)

```bash
# 1. Установить Java JDK 25+
sudo apt update && sudo apt install -y openjdk-25-jdk
export JAVA_HOME=/usr/lib/jvm/java-25-openjdk-amd64

# 2. Установить Android SDK command-line tools
mkdir -p ~/Android/Sdk/cmdline-tools
cd ~/Android/Sdk/cmdline-tools
curl -o tools.zip https://dl.google.com/android/repository/commandlinetools-linux-latest.zip
unzip tools.zip && mv cmdline-tools latest && rm tools.zip

# 3. Установить компоненты SDK и NDK 29
export ANDROID_HOME=~/Android/Sdk
export PATH=$PATH:$ANDROID_HOME/cmdline-tools/latest/bin
yes | sdkmanager --licenses
sdkmanager "platform-tools" "platforms;android-36" "ndk;29.0.14206865"
export ANDROID_NDK_ROOT=$ANDROID_HOME/ndk/29.0.14206865

# Gradle 9.4.0 скачивается автоматически скриптом сборки, если не установлен.

# 4. (Опционально) Сохранить переменные окружения
echo 'export JAVA_HOME=/usr/lib/jvm/java-25-openjdk-amd64' >> ~/.bashrc
echo 'export ANDROID_HOME=~/Android/Sdk' >> ~/.bashrc
echo 'export ANDROID_NDK_ROOT=$ANDROID_HOME/ndk/29.0.14206865' >> ~/.bashrc
source ~/.bashrc
```

### Инструменты сборки Web (Windows)

```bash
# Установить Emscripten SDK
git clone https://github.com/emscripten-core/emsdk.git C:\dev\emsdk
cd C:\dev\emsdk
.\emsdk install latest
.\emsdk activate latest
```

### Инструменты сборки Web (Linux / WSL2)

```bash
# 1. Установить Emscripten SDK
git clone https://github.com/emscripten-core/emsdk.git ~/emsdk
cd ~/emsdk
./emsdk install latest
./emsdk activate latest
source ./emsdk_env.sh

# 2. (Опционально) Сохранить окружение
echo 'source ~/emsdk/emsdk_env.sh' >> ~/.bashrc
```

---

## 📚 Сторонние библиотеки

IceBox Engine использует ряд сторонних open-source библиотек, каждая из которых распространяется под своей лицензией (MIT, zlib, BSD-3-Clause, Apache-2.0, ISC, FreeType/FTL, SIL OFL для встроенных шрифтов, а также LGPL-2.1 для FFmpeg и GNU FriBidi, которые подключаются динамически и могут быть свободно заменены).

Полный список библиотек и их лицензий: **[THIRD_PARTY_NOTICES.txt](THIRD_PARTY_NOTICES.txt)**

---

## 📬 Контакты

| | |
|-|-|
| 🌐 **Сайт** | [www.ice-box-crew.com](https://www.ice-box-crew.com/) |
| 📧 **Email** | [iceboxcrew057@gmail.com](mailto:iceboxcrew057@gmail.com) |
| 🐛 **Issues** | [GitHub Issues](https://github.com/IceBoxCrew/IceBoxEngine/issues) |

---

## 📄 Лицензия

**IceBox Engine** — проприетарное программное обеспечение.  
Все права защищены **IceBoxCrew Studio** © 2026.

Подробности см. в [LICENSE.txt](LICENSE.txt).

---

<p align="center">
  <strong>Сделано с ❄️ командой IceBoxCrew Studio</strong>
</p>
