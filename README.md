# StarGame

StarGame is a small C++11 arcade game built with OpenGL 3.3. The player moves a rocket horizontally, shoots incoming asteroids, avoids collisions, and earns a score that increases game speed. The menu supports help, four rocket skins, and a saved high-score list.

## Dependencies

- CMake 3.17+
- C++ compiler with C++11 support
- OpenGL
- GLFW 3
- GLEW
- GLM
- FreeType 2
- SOIL image loading library

The CMake configuration discovers these dependencies from the host system. Install the development packages for your operating system before building.

## Build

```bash
cmake -S . -B build
cmake --build build --parallel
```

Run from the repository root so the relative asset paths resolve:

```bash
./build/StarGame
```

The existing `build.sh` assumes a pre-existing `cmake-build-debug` directory and is not the recommended clean-build path.

## Controls

- Left / Right arrows: move the rocket
- Space: shoot
- Enter: start from the menu
- H: open help
- M: return from help to menu
- 1-4: select a rocket skin in the menu
- Esc: return to the menu during play
- Q: quit from the menu

## Runtime data

The game appends scores to `save.txt` in the current working directory. Keep the working directory at the repository root so the relative asset paths under `assets/` resolve correctly.

See [architecture](docs/ARCHITECTURE.md), [build guide](docs/BUILD.md), [controls and gameplay](docs/GAMEPLAY.md), and [testing](docs/TESTING.md).

Last updated: 2026-11-13 19:34 UTC
