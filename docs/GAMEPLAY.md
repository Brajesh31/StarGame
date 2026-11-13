# Gameplay

The game starts in a menu. Press Enter to initialize the asteroid field and begin play. The rocket moves horizontally, and Space fires bullets with a short cooldown. Avoid asteroids; a collision ends the current run, returns to the menu, and saves the score.

The score increases during gameplay. The game increases asteroid speed at score thresholds. The menu supports four rocket skins. Press H for help, M to return from help, and Q to quit.

## Persistence

Scores are appended to `save.txt` by the score helper. The file is created in the process working directory. No online leaderboard or account synchronization is implemented.
