# Build and Toolchain Guide

## 1. Required toolchain

- CMake 3.17 or newer.
- A C++ compiler with C++11 support.
- OpenGL development headers and libraries.
- GLFW 3 development package.
- GLEW development package.
- GLM headers.
- FreeType 2 development package.
- SOIL development package and a CMake package/module discoverable by the project.

The project uses `find_package` and pkg-config to locate dependencies. Exact package names vary between Ubuntu, Debian, Fedora, Arch, and other distributions.

## 2. Linux dependency installation

On Ubuntu-based systems, the CI job uses packages equivalent to:

```bash
sudo apt-get update
sudo apt-get install -y build-essential cmake pkg-config \
  libgl1-mesa-dev libglfw3-dev libglew-dev libglm-dev \
  libfreetype-dev libsoil-dev
```

If your distribution does not package SOIL under that name, install the equivalent library and ensure CMake can locate the `SOIL` package or module. The repository includes `FindSOIL.cmake`, which should be reviewed if discovery fails.

## 3. Clean configure and build

From the repository root:

```bash
cmake -S . -B build
cmake --build build --parallel
```

CMake creates the build directory and generates a platform-specific build system. The executable target is named `StarGame`.

## 4. Run

```bash
./build/StarGame
```

The process working directory affects relative asset paths. If the font or textures fail to load, inspect the paths in the source and set the IDE's working directory to the expected location. The current code uses relative paths beginning with `../assets` in some entity and font initialization paths. Do not assume that launching from every directory works.

## 5. IDE workflow

For Visual Studio Code, CLion, or another CMake-aware IDE:
1. Open the repository root.
2. Select a compiler toolchain.
3. Configure CMake into a generated build directory.
4. Check the CMake output for missing dependencies.
5. Build the `StarGame` target.
6. Configure the executable's working directory.
7. Launch from a desktop session.

Do not commit IDE-generated build folders or local binaries.

## 6. Existing build.sh caveat

The tracked `build.sh` changes into `cmake-build-debug`, then runs CMake and Make. It assumes that directory already exists and does not create it. The clean configure commands above are more reliable for a new checkout.

## 7. Common compiler and linker failures

- CMake cannot find OpenGL: install the OpenGL development package and verify the graphics toolchain.
- GLFW package not found: install GLFW development files and verify pkg-config output.
- GLEW missing: install GLEW headers and library.
- FreeType missing: install FreeType development files.
- SOIL missing: verify the library and `FindSOIL.cmake` search paths.
- Linker cannot resolve a library: check that the development package matches the compiler architecture and that CMake reconfigured after installation.

## 8. Reconfigure after dependency changes

If you change compiler, generator, or dependency paths, remove only the generated build directory and configure again:

```bash
rm -rf build
cmake -S . -B build
cmake --build build --parallel
```

Never remove source assets or repository files as a build troubleshooting shortcut.
