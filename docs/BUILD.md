# Build guide

## Linux

Install a C++ compiler, CMake, OpenGL development libraries, GLFW 3, GLEW, GLM, FreeType, and SOIL development files using the package manager available for your distribution.

Configure and build:

```bash
cmake -S . -B build
cmake --build build --parallel
```

Run from the repository root:

```bash
./build/StarGame
```

Running from another working directory may break relative font and texture paths. The existing `build.sh` changes into `cmake-build-debug` and assumes that directory already exists; prefer the commands above for reproducible builds.

## Troubleshooting

- CMake cannot find a library: install its development package and rerun configuration.
- Window fails to initialize: verify a working desktop session and OpenGL driver.
- Textures or font fail to load: launch from the repository root and check asset paths.
