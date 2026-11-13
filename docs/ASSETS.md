# Asset Inventory and Runtime Paths

## 1. Asset directories

The repository includes:
- `assets/connection.otf`: font asset used by text rendering.
- `assets/img/`: asteroid, bullet, rocket, and star image textures.
- `assets/img/png/`: PNG variants for selected sprites.
- `assets/shaders/`: vertex and fragment shader files.
- `screenshots/`: screenshots used in the README.

## 2. Texture roles

The image directory includes several asteroid images, bullet and enemy images, rocket skins, and a star texture. The rocket class references four rocket image paths to support the menu skin selection. Other entity classes load their own visual assets through the rendering helpers.

Do not rename or move assets without updating every source reference.

## 3. Shader files

The shader directory contains texture and text shader source files. Shader compilation and linking are handled through the rendering utilities. When changing a shader, test both startup and the relevant rendering path. A shader can compile while still producing incorrect output due to attribute, uniform, or coordinate mismatches.

## 4. Relative path behavior

The source references paths relative to the process working directory. Examples include font and texture paths prefixed with `../assets/`. This makes launch location important.

Recommended diagnostic process:
1. Record the process working directory.
2. Locate the exact path string in the relevant class.
3. Resolve the path relative to the working directory.
4. Confirm the file exists and is readable.
5. Check image loader and font loader errors.

A future source change should centralize asset root discovery or pass an explicit asset directory. This repository update documents the current behavior but does not alter source path handling.

## 5. Asset licensing

The repository includes image and font assets. Confirm that you have the right to redistribute each asset before using the project in a commercial product or republishing the assets separately. The repository's software license does not automatically establish third-party asset ownership.
