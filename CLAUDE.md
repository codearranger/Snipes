# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a modern C++ port of the classic 1982 text-mode game Snipes. The code has been reverse-engineered from the original DOS executable with 100% identical game logic. The project supports both SDL graphics and Windows console builds.

## Build Commands

### SDL Build (Default)
```bash
make
```
This creates an SDL build requiring SDL2 and SDL2_ttf libraries. The build will automatically create `config.h` from `config-sample.h` if it doesn't exist.

### Clean Build
```bash
make clean
```

### Visual Studio Build
1. Copy `config-sample.h` to `config.h`
2. Open `Snipes.sln` in Visual Studio
3. Build using the IDE (supports both SDL and Windows console configurations)

## Configuration

- **config.h**: Main configuration file (auto-created from `config-sample.h`)
- Key configuration options include:
  - `USE_SCANCODES_FOR_LETTER_KEYS`: Useful for non-QWERTY keyboards
  - `CHEAT_OMNISCIENCE`: Debug mode showing full maze
  - Font and display settings for SDL builds
  - Various bug fixes and compatibility options

## Architecture

### Core Structure
- **Snipes.cpp/h**: Main game logic and entry point
- **Platform abstraction**: Separate implementations for SDL (`sdl/`) and Windows (`windows/`)
- **Game components**: console, keyboard, sound, timer modules with platform-specific implementations

### Key Constants
- Maze dimensions: 16x20 cells (128x120 pixels)
- Each cell: 8x6 pixels
- Viewport: 40x25 characters (except in omniscience mode)

### Platform Abstraction
The game uses a clean platform abstraction layer:
- **SDL version**: Modern graphics with TTF fonts
- **Windows version**: Native console output
- Common interface through header files: `console.h`, `keyboard.h`, `sound.h`, `timer.h`

## Dependencies

### SDL Build
- SDL2
- SDL2_ttf
- Custom font file: `SnipesConsole.ttf` (loaded at startup)

### Development
- C++11 compatible compiler
- GNU Make or Visual Studio 2017+

## Replay System

The game automatically records replay files with `.SnipesGame` extension. To play back:
```bash
./snipes "2016-07-08 09.10.11.SnipesGame"
```

## macOS Status

✅ **Fully working**: The SDL version now runs correctly on macOS with all threading issues resolved.

**Features**:
- ✅ Application builds and runs successfully on macOS
- ✅ SDL window creation and event handling run properly on the main thread
- ✅ Font fallback system automatically uses system fonts (Courier.ttc, Menlo.ttc, etc.)
- ✅ Core game logic is fully functional
- ✅ All rendering and input handling work correctly

**Changes made for macOS compatibility**:
- SDL operations (window creation, event polling, rendering) now run on the main thread
- Added `SDL_MAIN_HANDLED` macro for macOS builds
- Implemented proper event processing in the main game loop
- Font loading includes automatic fallbacks to system fonts when custom font is unavailable

### Getting the Custom Font

The game expects `SnipesConsole.ttf` in the current directory. You can:
1. Download it from: http://kingbird.myphotos.cc/ee22d44076adb8a34d8e20df4be3730a/SnipesConsole.ttf
2. Or let it use system fallback fonts (Courier, Menlo, etc.)

## Build Flags

The GNUmakefile includes:
- `-std=c++11`: C++11 standard
- `-fstack-protector`: Security hardening
- Optional maintainer flags: `-Werror -Wall -Wextra` (enabled with `MAINT=1`)
- Optimized builds use `-O3`
- Debug builds can use `-Og -g -fsanitize=address -fsanitize=undefined`