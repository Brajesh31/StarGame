# Troubleshooting

## CMake cannot locate a dependency

Install the relevant development package, verify that the package provides headers and linkable libraries, then rerun CMake configuration. Inspect the CMake output for the exact package that failed.

## SOIL package discovery fails

The project includes `FindSOIL.cmake`. Check the module's expected include and library locations. Confirm that the installed SOIL package matches the system architecture and compiler.

## Window does not open

Confirm that GLFW initialized successfully, the machine has a working desktop session, and the graphics driver supports the requested OpenGL context. A headless server or CI shell may not provide a display.

## Black window or missing textures

Check that the asset file exists and that the process working directory makes the relative path resolve correctly. Review texture loader output and OpenGL error state. Do not assume that a successful CMake build means the runtime can find the assets.

## Font or text is missing

Verify the font path and confirm that FreeType can open the file. Check the text shader and renderer initialization. The current source references a relative font path.

## Game builds but fails to launch from another directory

Set the working directory to the location expected by the relative asset references. The current implementation does not provide a centralized asset root configuration.

## Save file is missing

The score helper writes `save.txt` relative to the process working directory. Check that the directory is writable and that a game-over collision occurred. The save file is created only when the save routine is called.

## CI passes but local build fails

Compare compiler version, CMake version, native dependency versions, and CMake package discovery paths. Reconfigure after changing system libraries. CI is a Linux build check, not proof of cross-platform compatibility.
