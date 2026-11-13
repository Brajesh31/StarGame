# Testing

There is no automated unit-test suite currently defined. Use the following manual smoke-test checklist after building:

- [ ] Application opens a 720x480 game window.
- [ ] Menu text, rocket, and background assets render.
- [ ] Enter starts a new run.
- [ ] Left and right arrows move the rocket within the play area.
- [ ] Space fires bullets and the cooldown behaves consistently.
- [ ] Bullets interact with asteroids.
- [ ] Rocket/asteroid collision returns to the menu and saves a score.
- [ ] H opens help and M returns to the menu.
- [ ] Number keys 1-4 change the rocket skin.
- [ ] Q closes the application from the menu.

Also test from a clean build directory and verify that the process is launched with the repository root as its working directory.
