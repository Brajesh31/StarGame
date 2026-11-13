# StarGame, C++ / OpenGL Arcade Game

StarGame is a small 2D-style arcade game implemented in C++11 with an OpenGL rendering pipeline. The player controls a rocket, moves horizontally, fires bullets, and avoids falling asteroids. A run ends when the rocket collides with an asteroid. The game tracks a score, increases the difficulty as the score rises, and appends completed scores to a local text file.

The project is useful as a compact example of:
- A native C++ application using CMake.
- GLFW window and input handling.
- OpenGL rendering and shader-based drawing helpers.
- Object-oriented game entities.
- A real-time update/render loop.
- Local high-score persistence.

## Screenshots

The repository includes three screenshots in `screenshots/`:
- [Main screen](screenshots/SH1.png)
- [Gameplay screen 1](screenshots/SH2.png)
- [Gameplay screen 2](screenshots/SH3.png)

## Technology and dependencies

| Component | Purpose |
|---|---|
| C++11 | Application language standard |
| CMake 3.17+ | Build configuration |
| OpenGL | Graphics API |
| GLFW 3 | Window creation and keyboard input |
| GLEW | OpenGL extension loading |
| GLM | Vector and matrix mathematics |
| FreeType 2 | Font loading and text rendering |
| SOIL | Image texture loading |

The CMake configuration discovers these native libraries from the host operating system. Installing only a C++ compiler is not sufficient.

## Repository structure

```text
.
├── CMakeLists.txt
├── FindSOIL.cmake
├── build.sh
├── main.cpp
├── core/
│   ├── Object.h
│   ├── Saver.h
│   └── shortCalls.h
├── obj/
│   ├── Asteroid.h
│   ├── Bullet.h
│   ├── Rocket.h
│   └── Star.h
├── glUtils/
│   ├── GLD.h
│   ├── Shader.h
│   ├── TextRenderer.h
│   └── character.h
├── assets/
│   ├── img/
│   ├── shaders/
│   └── connection.otf
└── screenshots/
```

## Build on Linux

Install a compiler, CMake, and the development packages for OpenGL, GLFW, GLEW, GLM, FreeType, and SOIL. Package names vary by distribution.

Configure and build from the repository root:

```bash
cmake -S . -B build
cmake --build build --parallel
```

Run the executable while the current working directory is the repository root:

```bash
./build/StarGame
```

The working directory matters because the source uses relative paths such as `../assets/connection.otf` and texture paths under `../assets/img/`. Running from the repository root may not match every relative path currently used by the implementation. If assets fail to load, inspect the runtime path assumptions and run from the expected build working directory, or correct paths in a separate source-code change. The documentation does not silently change those paths.

## Build with CMake GUI or an IDE

1. Open the repository root as a CMake project.
2. Configure using a build directory such as `cmake-build-debug`.
3. Ensure CMake can locate every required native library.
4. Build the `StarGame` target.
5. Set the executable working directory to the directory expected by the asset paths.
6. Launch the application from a desktop session with a working OpenGL driver.

## Controls

| Key | Action |
|---|---|
| Left arrow | Move rocket left |
| Right arrow | Move rocket right |
| Space | Fire a bullet |
| Enter | Start a game from the menu |
| H | Open help from the menu |
| M | Return from help to the menu |
| 1, 2, 3, 4 | Select a rocket skin in the menu |
| Esc | Return to the menu during gameplay |
| Q | Quit from the menu |

## Gameplay and score

The main loop uses three states: MENU, HELP, and GAME. During gameplay, the score increases and the asteroid speed is adjusted at score thresholds. Bullets are checked against asteroids. A collision between the player rocket and an asteroid ends the run, resets the score and speed, and saves the completed score through the score helper.

Scores are appended to `save.txt` in the current working directory. This is a local file, not an online leaderboard or account-based record system.

## Validation

CI installs native development dependencies, configures CMake, and builds the executable. This verifies compilation in the GitHub runner environment. It does not verify visual rendering, keyboard interaction, texture loading, font loading, collision feel, or runtime asset paths. Those require a local desktop smoke test.

## Known limitations

- Runtime asset paths depend on the process working directory.
- The existing `build.sh` assumes a pre-existing `cmake-build-debug` directory.
- Score persistence is a plain local text file.
- There is no automated gameplay test suite.
- The project does not document cross-platform testing beyond the Linux CI build.
- Some game objects and rendering helpers are implemented in headers, so changes to shared headers can affect several entity types.
- Graphics driver behavior and window availability are not tested by headless CI.

See [architecture](docs/ARCHITECTURE.md), [build guide](docs/BUILD.md), [gameplay](docs/GAMEPLAY.md), [assets](docs/ASSETS.md), [testing](docs/TESTING.md), and [troubleshooting](docs/TROUBLESHOOTING.md).
