Windows compilation tutorial WIP
================================

My notes about trying to compile Solarus dev branch (1.6) on Windows 10.
Using QtCreator and MinGW32.

First build
-----------

Setup a build environment:

- Install QtCreator
  - open-source version
  - Qt 5.10.0
  - With MinGW 5.3.0 32 bit (check both toolchain + Qt components)
  - With QtWebEngine
- Install Git
  - From git-scm.com
- Install CMake
  - Windows installer from cmake officiel website
  - Check "Add CMake to the PATH" to help QtCreator find it

Setup Solarus build:

- Clone the repositories Solarus and Solarus Quest Editor
  - Optionally: upload public key
- Tell CMake the location of all dependencies: for instance   `C:/Users/you/solarus/libraries/win32/mingw32`
  - In QtCreator: Tools > Options > Build & Run > Kits > CMake Configuration,
    add this directory to `CMAKE_PREFIX_PATH`. For example
    `CMAKE_PREFIX_PATH:STRING=%{Qt:QT_INSTALL_PREFIX};C:/Users/you/solarus/libraries/win32/mingw32`
  - Press Apply Configuration Changes

Build Solarus
-------------

- Build from QtCreator
- Install from QtCreator
  - Check install in CMake build step
  - Set `CMAKE_INSTALL_PREFIX` to a folder that you have access to,
    for instance `C:/Users/you/solarus-install`

Build Solarus Quest editor
--------------------------

Build from QtCreator.
- Tell CMake the paths to Solarus headers and libraries:
  - Set `SOLARUS_INCLUDE_DIR` to `C:/Users/you/solarus-install/include`.
  - Set `SOLARUS_GUI_INCLUDE_DIR` to `C:/Users/you/solarus-install/include`.
- Install from QtCreator
  - Check install in CMake build step
  - Set `CMAKE_INSTALL_PREFIX` to the same folder as Solarus, for instance `C:/Users/you/solarus-install`

Run
---

- Add necessary DLLs to the install directory:
  - Add all DLL dependencies.
  - Add Qt5Core.dll, Qt5Gui.dll, Qt5Widgets.dll from Qt installation
  - Add platforms/qwindows.dll (this is located in Qt plugins folder)
  - Add libgcc_s_dw2-1.dll, libstdc++-6.dll, libwinpthread-1.dll as well
  - Add assets (for the quest editor initial quest)
