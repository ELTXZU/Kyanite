<p align="center">
  <img src="logo.png" alt="Bismuth logo" width="180"/>
</p>

<h1 align="center">Bismuth</h1>

<p align="center">
Rendering optimization mod with the goal of fully rewriting the rendering side
of Geometry Dash for maximum performance.
</p>

<p align="center"><b>Currently experimental</b></p>

## Hitboxes and cheat compatibility

Hitbox viewers (MegaHack's show hitboxes, GD Mega Overlay, GD's own debug draw
and hitboxes on death) draw the game's own hitbox rectangles. Bismuth keeps
rendering the level with its optimizations on while they are active: the
objects on screen are activated invisibly so the game can compute their
hitboxes, and everything else stays culled. Hitboxes line up with what you see,
including objects moved or rotated by triggers.

Hitbox viewers do not turn the renderer off - if they did, you would get
vanilla FPS while they are on.

## Frame timing

Bismuth logs an average timing breakdown of every rendered frame to the Geode
log every 5 seconds:

```
Frame timing: 1042 FPS (0.96 ms/frame) | uniforms 3 us | color channels 4 us
 | group states 6 us (alpha 3, pulse 1, upload 2) | culling 11 us (sorted batch 0, 0 regen)
 | draw 48 us | other 888 us
```

`other` is everything the renderer does not measure (game update, UI, buffer
swap). Turn it off with the *Log frame timing* setting if you don't want the
lines in your log.

## Trigger support

Group based triggers are applied on the GPU: move, rotate, follow, toggle,
alpha and pulse. Every other trigger keeps working because it doesn't change
how objects are drawn.

Per-object effects are different: the Scale trigger (2067), the Area and Enter
effects (3006+), Advanced Follow and keyframes move or resize every object
individually, which a per-group state can't describe. Bismuth keeps rendering
these levels (so you still get the speedup), but objects touched by those
triggers keep their initial transform instead of animating. Giving those
effects a per-object transform buffer is the next step (see TODO.md).

Objects that come from a sprite sheet the renderer has no texture for are
skipped by the renderer; the rest of the level is still rendered normally.

## Supported versions

The mod is built for **Geometry Dash 2.2081** with **Geode 5.11.2**. The source
also builds unchanged against Geode 6 / GD 2.2091 - set `geode` to
`6.0.0-beta.1` and `gd.win` to `2.2091` in `mod.json` and build with the 6.x
SDK and its bindings; the only version difference (`PlayLayer::init`
arguments) is handled by a `GEODE_COMP_GD_VERSION` check.

Both builds are produced from this source: use the `.geode` matching your
Geode version, Geode will refuse the other one.

## Building

The mod is built with the [Geode SDK](https://docs.geode-sdk.org/).

On Windows:

```
geode build
```

On Linux (cross-compiling for Windows):

```
geode sdk install-linux
geode build -p windows
```

If the SDK helper can't download the Windows SDK, you can wire the toolchain up
by hand (this is how the current release was built):

1. `xwin --accept-license --arch x86_64 splat --output ./xwin-splat` (xwin 0.10+)
2. LLVM/clang 19 or newer, with `clang-cl`, `lld-link`, `llvm-lib`, `llvm-mt`,
   `llvm-rc` and `llvm-nm`
3. Configure:

```
cmake -S . -B build-win -G Ninja \
  -DGEODE_TARGET_PLATFORM=Win64 \
  -DCMAKE_TOOLCHAIN_FILE=<geode-sdk>/clang-cl-msvc.cmake \
  -DSPLAT_DIR=./xwin-splat -DHOST_ARCH=x86_64 \
  -DLLVM_VER=19 -DCLANG_VER=19 -DLLVM_PATH=<llvm>/bin \
  -DCMAKE_MSVC_RUNTIME_LIBRARY=MultiThreadedDLL
cmake --build build-win
```

Pushing to this repository also builds a Windows `.geode` through the
GitHub Actions workflow in `.github/workflows/multi-platform.yml`.

You can try the latest builds from the discord server: https://discord.gg/MDbQt3w8HC

## License

Bismuth - Optimization mod for Geometry Dash

Copyright (C) 2026 IliasHDZ

If the program has been modified using AI, you CANNOT DISTRIBUTE
a binary of the modified program.

This project also CANNOT BE USED UNDER ANY CIRCUMSTANCES TO
FACILITATE OR ADVANCE GEOMETRY DASH LEVEL GENERATION OR
MODIFICATION USING GENERATIVE AI.

Except for the above clauses, the GPL v3 license applies:

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program.  If not, see <https://www.gnu.org/licenses/>.
