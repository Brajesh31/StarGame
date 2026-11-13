# Testing and Quality Assurance

## 1. Automated CI scope

The GitHub Actions workflow installs native development dependencies, configures the project with CMake, and builds the executable. This catches many compile, header, linker, and dependency-discovery problems in the CI environment.

It does not run the graphical application. The runner may not provide a desktop display or the same graphics driver as a developer machine.

## 2. Clean build test

```bash
cmake -S . -B build
cmake --build build --parallel
```

Perform this from a clean checkout. Confirm that CMake finds OpenGL, GLFW, GLEW, GLM, FreeType, and SOIL.

## 3. Runtime smoke test

Run the game from the working directory expected by the relative asset paths. Confirm:
- The window opens.
- The title and menu text render.
- Rocket and background textures load.
- The help screen is readable.
- All four skins can be selected.
- A new game starts.
- The rocket responds to movement input.
- Shooting creates visible bullets.
- Asteroids move and collide with bullets.
- Rocket collision ends the run.
- Score text updates.
- A score is appended to the local save file.
- The application exits cleanly.

## 4. State transition tests

| Initial state | Input/event | Expected state |
|---|---|---|
| MENU | Enter | GAME |
| MENU | H | HELP |
| MENU | Q | Window close requested |
| HELP | M | MENU |
| GAME | Esc | MENU |
| GAME | Rocket hits asteroid | MENU and score saved |

## 5. Regression risks

Changes to `core/Object.h`, `glUtils/GLD.h`, shader utilities, or shared entity classes can affect multiple objects. After editing shared code, test all entity types and not only the object that motivated the change.

Changes to collision logic should test edge overlap, fast movement, simultaneous bullet and asteroid movement, and repeated collisions. Changes to score persistence should test missing, empty, malformed, and large save files.

## 6. Test data hygiene

The score file is local runtime output. Do not commit personal score files or generated build products. Use a clean temporary working directory for persistence tests.
