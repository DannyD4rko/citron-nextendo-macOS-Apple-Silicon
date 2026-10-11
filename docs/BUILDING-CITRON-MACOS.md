# Building Citron for macOS

The macOS nightly workflow produces separate native builds for Apple Silicon
(`arm64`) and Intel Macs (`x86_64`). The Intel build uses an Intel GitHub
Actions runner so Homebrew dependencies and Qt tools match the target
architecture.

## Building for Intel

Build on an Intel Mac with Xcode Command Line Tools and the dependencies used
by the macOS workflow installed. Check out the source with its submodules, then
configure and build:

```sh
cmake -B build -S . -G Ninja \
  -DCMAKE_OSX_ARCHITECTURES=x86_64 \
  -DCMAKE_OSX_DEPLOYMENT_TARGET=12.0 \
  -DCMAKE_BUILD_TYPE=Release \
  -DCITRON_BUILD_TYPE=Release \
  -DCITRON_USE_FASTER_LD=OFF \
  -DCITRON_USE_BUNDLED_VCPKG=OFF \
  -DENABLE_QT=ON \
  -DCITRON_ENABLE_LTO=ON \
  -DCITRON_TESTS=OFF \
  -DUSE_DISCORD_PRESENCE=ON \
  -DENABLE_WEB_SERVICE=ON \
  -DENABLE_OPENSSL=ON \
  -DENABLE_SDL2=ON \
  -DCITRON_USE_EXTERNAL_SDL2=ON
cmake --build build --config Release
```

`CMAKE_OSX_ARCHITECTURES=x86_64` is important: it makes the target explicit
and ensures CMake selects the Intel architecture even if the build setup is
later changed. Use dependencies built for x86_64; ARM-only Homebrew libraries
cannot be linked into an Intel executable.

Nightly releases contain separate DMGs named
`Citron-macOS-x86_64-nightly-<commit>.dmg` and
`Citron-macOS-arm64-nightly-<commit>.dmg`.
