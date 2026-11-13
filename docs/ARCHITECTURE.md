# StarGame Architecture and Runtime

## 1. Overview

StarGame is a native C++11 desktop application. It creates a GLFW window, initializes OpenGL state, loads game assets, and enters a loop that processes input, updates game objects, checks collisions, and renders the current screen.

The project is organized into three main code areas:
- `main.cpp`: application orchestration and game state.
- `obj/` and `core/`: game entities, common object behavior, and score persistence.
- `glUtils/`: OpenGL helpers, shader management, texture and text rendering support.

## 2. Entry point and initialization

The `main` function in `main.cpp`:
1. Seeds the random number generator.
2. Initializes GLFW and handles initialization failure.
3. Requests an OpenGL 3.3 core profile.
4. Creates a 720 by 480 window.
5. Makes the window's OpenGL context current.
6. Initializes GLEW.
7. Enables blending, culling, and multisampling.
8. Reads the framebuffer dimensions and configures the viewport.
9. Configures projection state.
10. Creates the text renderer, rocket, stars, asteroid collection, and bullet slots.

The application exits if GLFW cannot initialize or the window cannot be created. The current source does not show a complete error policy for every later graphics-resource failure.

## 3. State machine

The game uses an enum with three states:

| State | Purpose | Main transitions |
|---|---|---|
| MENU | Start, choose skin, open help, quit | Enter to GAME, H to HELP, Q to close |
| HELP | Display control/help information | M to MENU |
| GAME | Run the active game simulation | Esc or collision returns to MENU |

The state variable is global in the current implementation. Input processing and rendering branch on this state inside the main loop.

## 4. Entity model

### Rocket

The rocket is controlled by the left and right arrow keys. The entity wraps a rectangle/renderable object, loads one of four texture skins, and updates its position while enforcing horizontal bounds.

### Asteroids

The asteroid collection is initialized when a new game starts. Asteroids move down the screen. Their speed is influenced by the global game-speed value. Bullet collisions reposition and reset asteroids.

### Bullets

The application maintains a fixed array of five bullet pointers. Space creates a bullet when the firing cooldown has elapsed. The main loop checks bullets against asteroids and renders active bullet objects.

### Stars

A fixed array of stars is initialized with randomized positions and speeds to create background movement.

### Shared object and rendering helpers

The `core/` and `glUtils/` directories contain common geometry, vector, shader, text, and drawing functionality. Review these headers before changing coordinate conventions or OpenGL resource ownership.

## 5. Frame loop

The loop continues until GLFW reports that the window should close. Each iteration:
1. Reads the current GLFW time.
2. Computes a delta-time value.
3. Handles state-specific keyboard input.
4. Updates score and difficulty while playing.
5. Clears the color buffer.
6. Checks bullet/asteroid collisions.
7. Updates and draws stars and asteroids.
8. Checks rocket/asteroid collision.
9. Draws bullets, rocket, and score text.
10. Draws menu or help content when those states are active.
11. Swaps buffers and processes pending window events.

The exact ordering is significant. A change to update order can affect collision timing and visual movement.

## 6. Collision and game-over behavior

Bullet collision checks use the asteroid's collision helper with the bullet rectangle. A hit moves the asteroid back above the visible play area, resets its state, and clears the bullet slot. A rocket/asteroid collision returns the state to MENU, saves the score, resets score and speed, clears asteroids, and repositions the rocket.

The code uses direct object methods and shared global state rather than a separate physics engine.

## 7. Score and difficulty

The score increases during GAME state. The global speed multiplier changes at score thresholds, causing asteroids to move faster as the run continues. The exact thresholds and multiplier values are in `main.cpp`; update this document whenever they change.

The score helper in `core/Saver.h` appends scores to a local text file and reads/sorts saved values to produce a top-five list. It is not a server-backed leaderboard.

## 8. Resource and lifetime considerations

The source uses raw pointers for bullet objects and clears slots by assigning null. Before expanding the game, review object allocation and destruction carefully. A robust future implementation should use value semantics or smart pointers, define clear ownership, and release OpenGL resources while the context is valid.

## 9. Threading and networking

No network service or background worker is documented in the current project. The game loop runs on the main thread. Do not describe the score system as cloud synchronized or multiplayer.
