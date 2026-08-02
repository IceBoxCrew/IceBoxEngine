THIRD-PARTY LICENSES
========================================================================

This directory contains the full license text of every third-party open
source component that is redistributed in IceBox Engine and in games built
with it. It accompanies the summary in ../THIRD_PARTY_NOTICES.txt and
satisfies the attribution requirements of the licenses below.

Each file holds the verbatim license (and copyright notice) of one component.
The license texts for components obtained through vcpkg are taken from the
exact installed package revision (vcpkg "copyright" files), so they match the
versions actually shipped.

This whole directory is bundled into every platform build (Windows, Linux,
macOS, iOS, Android, Web) alongside THIRD_PARTY_NOTICES.txt.

------------------------------------------------------------------------
INVENTORY  (file  ->  component  ->  license  ->  source)
------------------------------------------------------------------------

-- Obtained via vcpkg (license text from the installed package revision) --

SDL3.txt                  SDL3                         Zlib                 vcpkg: sdl3
stb.txt                   stb                          MIT OR Unlicense     vcpkg: stb
zlib.txt                  zlib                         Zlib                 vcpkg: zlib
Box2D.txt                 Box2D                        MIT                  vcpkg: box2d
enkiTS.txt                enkiTS                       Zlib                 vcpkg: enkits       (Box2D multithreading; not on Web)
EnTT.txt                  EnTT                         MIT                  vcpkg: entt
GLM.txt                   GLM                          MIT                  vcpkg: glm
DearImGui.txt             Dear ImGui                   MIT                  vcpkg: imgui
ImGuizmo.txt              ImGuizmo                     MIT                  vcpkg: imguizmo
imgui-node-editor.txt     imgui-node-editor            MIT                  vcpkg: imgui-node-editor
spdlog.txt                spdlog                       MIT                  vcpkg: spdlog
nlohmann-json.txt         nlohmann/json                MIT                  vcpkg: nlohmann-json
ENet.txt                  ENet                         MIT                  vcpkg: enet
fmt.txt                   fmt                          MIT                  vcpkg: fmt
Lua.txt                   Lua                          MIT                  vcpkg: lua
sol2.txt                  sol2                         MIT                  vcpkg: sol2
glad.txt                  glad                         MIT                  vcpkg: glad
curl.txt                  curl                         curl (MIT/X11)       vcpkg: curl
HarfBuzz.txt              HarfBuzz                     MIT (Old MIT)        vcpkg: harfbuzz
md4c.txt                  md4c                         MIT                  vcpkg: md4c
volk.txt                  volk                         MIT                  vcpkg: volk
VulkanMemoryAllocator.txt Vulkan Memory Allocator      MIT                  vcpkg: vulkan-memory-allocator
Brotli.txt                Brotli                       MIT                  vcpkg: brotli
Tracy.txt                 Tracy                        BSD-3-Clause         vcpkg: tracy
pybind11.txt              pybind11                     BSD-3-Clause         vcpkg: pybind11
Zstandard.txt             Zstd                         BSD-3-Clause         vcpkg: zstd
Opus.txt                  Opus                         BSD-3-Clause         vcpkg: opus
libogg.txt                libogg                       BSD-3-Clause         vcpkg: libogg
libvorbis.txt             libvorbis                    BSD-3-Clause         vcpkg: libvorbis
libwebp.txt               libwebp                      BSD-3-Clause         vcpkg: libwebp
glslang.txt               glslang                      BSD-3-Clause (+MIT)  vcpkg: glslang
IXWebSocket.txt           IXWebSocket                  BSD-3-Clause         vcpkg: ixwebsocket
FreeType.txt              FreeType                     FTL (FreeType)       vcpkg: freetype
Python.txt                Python (embedded)            PSF License 2.0      vcpkg: python3
GNU-FriBidi.txt           GNU FriBidi                  LGPL-2.1-or-later    vcpkg: fribidi
shaderc.txt               shaderc                      Apache-2.0           vcpkg: shaderc
SPIRV-Tools.txt           SPIRV-Tools                  Apache-2.0           vcpkg: spirv-tools
Vulkan-Headers.txt        Vulkan-Headers               Apache-2.0 OR MIT    vcpkg: vulkan-headers
SPIRV-Cross.txt           SPIRV-Cross                  Apache-2.0           vcpkg: spirv-cross
basis_universal.txt       basis_universal              Apache-2.0           vcpkg: basisu
OpenSSL.txt               OpenSSL                      Apache-2.0           vcpkg: openssl
FFmpeg.txt                FFmpeg                       LGPL-2.1-or-later    vcpkg: ffmpeg
libsodium.txt             libsodium                    ISC                  vcpkg: libsodium
libpng.txt                libpng                       libpng (PNG Ref Lib) vcpkg: libpng
bzip2.txt                 bzip2                        bzip2 (BSD-like)     vcpkg: bzip2
libvpx.txt                libvpx (VP8/VP9)             BSD-3-Clause (+pat.) vcpkg: libvpx       (VP9 encoder via FFmpeg)
expat.txt                 Expat (XML)                  MIT                  vcpkg: expat        (inside embedded Python)
libffi.txt                libffi                       libffi (MIT-style)   vcpkg: libffi       (inside embedded Python)
liblzma-xz.txt            liblzma / xz                 0BSD / Public Domain vcpkg: liblzma      (inside embedded Python)
SQLite.txt                SQLite                       Public Domain        vcpkg: sqlite3      (inside embedded Python)
SPIRV-Headers.txt         SPIRV-Headers                MIT (Khronos)        vcpkg: spirv-headers (inside shaderc/SPIRV-*)
EGL-Registry.txt          EGL-Registry                 Apache-2.0 / SGI-B   vcpkg: egl-registry  (GL/EGL loader generation)
OpenGL-Registry.txt       OpenGL-Registry              Apache-2.0 / SGI-B   vcpkg: opengl-registry (GL loader generation)
ANGLE.txt                 ANGLE                        BSD-3-Clause         vcpkg: angle        (macOS / iOS GLES backend)

-- Vendored in-tree or bundled as assets (not from vcpkg) --

miniaudio.txt             miniaudio                    Unlicense OR MIT-0   vendored: Source/Engine/ThirdLibrary/miniaudio.h
ImGuiColorTextEdit.txt    ImGuiColorTextEdit           MIT                  vendored: Source/Engine/ThirdLibrary/ImGuiColorTextEdit
MoltenVK.txt              MoltenVK                     Apache-2.0           vendored: fetched by Tools/BuildSystem/BuildEngine/fetch_moltenvk.sh (macOS/iOS)
NotoFonts-OFL.txt         Noto Sans font family        OFL-1.1              bundled: Config/Fonts/NotoSans*.ttf
Tiny5-OFL.txt             Tiny5 (pixel font)           OFL-1.1              bundled: Content/Examples/Platformer/Widgets/F_Tiny5-Regular.ttf

------------------------------------------------------------------------
vcpkg CROSS-CHECK
------------------------------------------------------------------------

Every redistributed vcpkg dependency declared in ../vcpkg.json (and every
transitive dependency that ends up in a shipped binary) has a license file
above. This includes transitive components that are not direct dependencies
but are linked/embedded into the product:

  - expat, libffi, liblzma, sqlite3   -> pulled in by the embedded Python 3
  - spirv-headers                     -> embedded in shaderc / SPIRV-Tools / SPIRV-Cross
  - egl-registry, opengl-registry     -> used to generate the GL/EGL loaders (glad)
  - brotli, bzip2, libpng, zlib       -> pulled in by FreeType / HarfBuzz / cURL / FFmpeg
  - libvpx, opus, libvorbis, libogg   -> pulled in by FFmpeg (royalty-free VP9 + Opus + Vorbis)

The following vcpkg packages are intentionally NOT included here because they
are build-time tooling only. They are never compiled or linked into the
shipped runtime, so they carry no redistribution/attribution obligation in
distributed binaries:

  detect_compiler, pkgconf, vcpkg-cmake, vcpkg-cmake-config,
  vcpkg-cmake-get-vars, vcpkg-get-python, vcpkg-make, vcpkg-msbuild,
  vcpkg-pkgconfig-get-modules, vcpkg-tool-meson

------------------------------------------------------------------------
MOBILE PLATFORM SDKs (NOT REDISTRIBUTED HERE)
------------------------------------------------------------------------

The Android game template resolves AndroidX, the Kotlin standard library and
- when the developer enables the matching feature - Google Play services
packages (Ads/AdMob, Billing, Play Games, the UMP consent SDK, In-App Review,
Firebase) from Google's Maven repository through the developer's own Gradle
build. On iOS the Google Mobile Ads SDK is added to the generated Xcode
project by the developer. None of these are shipped inside IceBox Engine, so
they carry no license file in this directory; they are licensed directly to
the developer under Google's own terms. The resulting attribution and privacy
duties fall on the developer of the game. See THIRD_PARTY_NOTICES.txt
section 14 for the full list and the specific obligations.

------------------------------------------------------------------------
NOTES
------------------------------------------------------------------------

- FFmpeg is built under LGPL-2.1 with no GPL or non-free components and no
  external patent-encumbered encoder (no x264/x265/fdk-aac). The engine only ever
  PRODUCES royalty-free output (VP9 via libvpx, Opus, Vorbis). The editor's media
  importer additionally uses FFmpeg's standard built-in decoders to read the
  source video/audio that a developer chooses to import. See THIRD_PARTY_NOTICES.txt
  section 8.
- Cooked video uses the royalty-free VP9 video codec (libvpx, which carries the
  WebM patent grant) with Opus audio in a WebM container; see libvpx.txt. No
  patent-encumbered codec is produced by the engine.
- GNU FriBidi (LGPL-2.1) and FFmpeg (LGPL-2.1) are dynamically linked and shipped
  as separate replaceable shared libraries on every platform where they are
  included (Windows, Linux, macOS and Android), so they can be replaced/relinked
  freely without any further action. FFmpeg is not included on iOS or Web. FriBidi
  is also included on iOS and Web, where all libraries are linked statically; there
  it is shipped unmodified, so its upstream release at
  https://github.com/fribidi/fribidi is the complete corresponding source code.
  See THIRD_PARTY_NOTICES.txt sections 6 and 8.
- ANGLE and MoltenVK ship only in macOS/iOS builds; they are listed here so
  this attribution set is complete across all six target platforms.
- enkiTS is statically linked into Windows, Linux, macOS, iOS and Android builds,
  where it runs the multithreaded Box2D solver. Web builds do not link it, because
  Emscripten defaults to a single-threaded runtime; there Box2D falls back to its
  built-in serial task path.
