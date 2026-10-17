# Knight to Sky

An endless 2D platformer built with **C++17 and raylib**. Guide a knight across platforms scrolling downward, use double jumps to stay in the air, and collect bonuses to increase your score. Each run introduces new platform and pickup arrangements, with constant difficulty and no finish line.

## About the project

Knight to Sky is a **group project** and was our **first experience developing a game entirely in C++**. It brought together character movement, platform generation, collectible effects, scoring, and audiovisual feedback in a playable game.

## How to play

Each run begins in midair. Your first jump starts the downward scrolling and survival scoring. Watch for landing opportunities, adjust your trajectory in the air, and decide when to take a detour for a pickup. Landing restores both jumps; the second jump can extend your ascent or interrupt a fall. Moving completely beyond either side of the screen brings you back from the opposite side.

You can pass through platforms from below and land on them while descending. **Normal platforms** provide a landing surface, **moving platforms** shift horizontally, and **breakable platforms** fade away after contact once scrolling has started. New platforms appear as previous ones leave the play area.

Falling below the screen ends the run and displays your final score. Press Enter to return to the opening screen, then Enter again to start another attempt.

## Controls

| Key | Action |
|---|---|
| A / D or Left / Right arrows | Move horizontally |
| Space | Jump; up to two jumps before landing |
| Enter | Start from the opening screen; return to the menu after game over |

## Scoring and pickups

Survival awards **10 points per second** after the first jump. Pickups can help you score or make the knight harder to control.

| Pickup | Effect |
|---|---|
| Coin | +100 points |
| Score multiplier | Doubles survival and coin points for 15 seconds |
| Spring | Increases the next jump's initial upward speed by 60%; consumed on use |
| Slow | Reduces horizontal speed to 60% for 12 seconds |
| Reverse controls | Swaps left and right input for 5 seconds |

Different timed effects can coexist. Collecting the same effect again refreshes its duration without increasing its strength. The interface displays your score, remaining effect durations, and pickup messages.

## Build and run

You need **CMake 3.20 or later**, a C++17 compiler, **Ninja** for the commands below, and a **raylib** installation compatible with your compiler. Run these commands from the repository root:

```powershell
cmake -S . -B build -G Ninja -DRAYLIB_DIR="C:/raylib/w64devkit/x86_64-w64-mingw32"
cmake --build build
cd build
.\cpp-raylib-platformer.exe
```

This example targets Windows. Adjust the raylib path to match your installation and make sure the build tools are available in your PATH. If CMake already finds raylib, omit `-DRAYLIB_DIR=...`. The build copies assets next to the executable, so launch the game from that directory. Executable names and tool configuration may differ on other systems.
