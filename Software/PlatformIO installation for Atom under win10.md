PlatformIO installation
=======================

For Atom text editor.

1. Download and install [Atom](https://atom.io/).
2. Download and install [MinGW](http://www.mingw.org/wiki/Getting_Started). Select components:
  * mingw-developer-toolkit-bin,
  * mingw32-base-bin,
  * mingw32-gcc-g++-bin,
  * msys-base-bin)
3. Add ```C:\MinGW\bin``` to your system PATH
4. Install Python 2.7 (for Windows go to [Anaconda](https://www.anaconda.com/distribution/) web site and choose Python 2.7 installer).
5. Add Python to your system PATH (by default it is ```C:\ProgramData\Anaconda3```).
6. Install [Clang 3.9.1 for Windows (32-bit)](http://releases.llvm.org/3.9.1/LLVM-3.9.1-win32.exe) or [Clang 3.9.1 for Windows (64-bit)](http://releases.llvm.org/3.9.1/LLVM-3.9.1-win64.exe)
7. Open console (Win+R and type cmd) and check all installed well:
  * ```python --version```
  * ```g++ --version```
  * ```clang --version```
8. Install PlatformIO. Atom Settings (Ctrl+,) -> Install -> platformio-ide.
9. Create first project, using the instructions in [Getting Started](https://docs.platformio.org/en/latest/ide/vscode.html#quick-start).

Troubleshooting
---------------
If you have errors when 'platformio' or 'pio' is not recognized as an internal or external command, add PlatformIO scripts folder (by default it is ```C:\Users\<username>\.platformio\penv\Scripts```) to your system PATH.
