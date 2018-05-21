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
- Tell CMake the location of all dependencies: for instance   `C:\Users\you\solarus\libraries\win32\mingw32`
  - In QtCreator: Tools > Options > Build & Run > Kits > CMake Configuration,
    add this directory to `CMAKE_PREFIX_PATH`. For example
    `CMAKE_PREFIX_PATH:STRING=%{Qt:QT_INSTALL_PREFIX};C:\Users\you\solarus\libraries\win32\mingw32`


Build Solarus
-------------

- Build from QtCreator

Run
---

- Put built files in a folder (libsolarus.dll, solarus-run.exe, solarus.exe)
- Add all DLL dependencies.
- Add Qt5Core.dll, Qt5Gui.dll, Qt5Widgets.dll from Qt installation
- Add platforms/qwindows.dll (this is located in Qt plugins folder)
- Add libgcc_s_dw2-1.dll, libstdc++-6.dll, libwinpthread-1.dll as well
