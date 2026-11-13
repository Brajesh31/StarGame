# Gameplay, Controls, and Scoring

## 1. Objective

Control the rocket, shoot incoming asteroids, and survive as the score increases. Asteroids move through the play area. A collision between the rocket and an asteroid ends the active run.

## 2. Menu state

The menu is the initial state. It presents the game title and instructions. From the menu:
- Press Enter to initialize a new asteroid field and start.
- Press H to enter the help state.
- Press Q to close the window.
- Press 1, 2, 3, or 4 to select one of the rocket skins.

The rocket skin selection is applied while the menu state is active.

## 3. Help state

The help screen displays the game's instructions. Press M to return to the menu. The help state does not run the active gameplay update.

## 4. Gameplay controls

| Input | Behavior |
|---|---|
| Left arrow | Move rocket left |
| Right arrow | Move rocket right |
| Space | Fire a bullet, subject to cooldown |
| Esc | Return to menu |

The current movement code uses a fixed speed constant multiplied by a time argument. The main loop calculates a delta-time value; inspect the exact update usage before changing movement behavior.

## 5. Shooting

The game stores a fixed number of bullet slots. Space checks a cooldown based on GLFW time before creating a new bullet. Bullets are drawn while their pointer slot is non-null and are checked against asteroids during gameplay.

When a bullet collides with an asteroid, the asteroid is repositioned and the bullet slot is cleared. Review allocation and deletion semantics before increasing the number of shots or adding longer-lived projectiles.

## 6. Asteroids and difficulty

A new run initializes a collection of asteroids at randomized positions. Their vertical speed is based on a global game-speed multiplier. As the score increases, the multiplier rises through a set of thresholds defined in `main.cpp`.

The difficulty model is score-based, not a configurable level system. There is no documented pause state, power-up system, boss system, or persistent player profile.

## 7. Game over

When the rocket collides with an asteroid:
1. The state changes back to MENU.
2. The current score is appended through the score-saving helper.
3. Score and speed are reset.
4. The asteroid collection is cleared.
5. The rocket is repositioned.
6. Bullet slots are reset.

This is the current source behavior. If adding a dedicated game-over screen, update the state machine and this guide together.

## 8. High scores

The score helper writes to `save.txt` in append mode. It reads saved values, sorts them in descending order, and returns up to five entries, padding with zeros when fewer than five scores exist.

The file is plain text. It can be edited or deleted by a local user and is not protected against tampering. There is no online leaderboard or synchronization between machines.

## 9. Gameplay smoke test

- Start from the menu.
- Verify the rocket is visible.
- Move left and right and check boundary behavior.
- Fire multiple bullets and observe the cooldown.
- Observe asteroid movement and bullet collision.
- Trigger a rocket collision.
- Confirm the menu returns and the score file is updated.
- Start a second run and verify score reset.
- Test all four skins and help/menu transitions.
