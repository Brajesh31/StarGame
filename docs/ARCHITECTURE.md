# Architecture

The project is a single C++11 executable with OpenGL rendering.

- `main.cpp`: window creation, menu/game state, input handling, frame loop, scoring, and collision orchestration.
- `core/`: base object helpers and score persistence.
- `obj/`: game entities including rocket, asteroid, star, and bullet.
- `glUtils/`: OpenGL drawing helpers, shader handling, and text rendering.
- `assets/`: textures, shaders, and font asset.
- `screenshots/`: example game screens.

The main loop transitions between MENU, HELP, and GAME states. Gameplay updates score and speed, moves entities, checks bullet/asteroid and rocket/asteroid collisions, and renders the scene.
