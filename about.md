# Kyanite

<cr>**Alpha.** Levels that use per-object effects (Scale trigger, area/enter
effects, advanced follow, keyframes) render, but the objects those effects move
stay at their start transform.</c>

Kyanite draws level objects on the GPU. Group transforms, triggers and
visibility are resolved per group in a compute shader every frame, off-screen
objects are culled before they are drawn, and a level is no longer rebuilt into
per-object vertices each frame. Shaders, UI and particles are untouched.

Group triggers run on the GPU: move, rotate, follow, toggle, alpha and pulse.
Hitbox viewers keep working without turning the optimizations off.

Kyanite is **especially** effective with high object counts and excessive
move/rotate trigger logic. It works best on medium to high-end GPUs; on low-end
integrated GPUs, **Intel ones in particular**, the culling gain is smaller and
the mod can be **slower** than vanilla.

Levels with good results:
- Peaceful
- A daydream journey
- Departure
- Slaughterhouse

Debug tools (off by default) add **F8** to toggle the renderer in game, **F9**
for difference mode and **F7** to select every object right of the screen in the
editor. **F4** toggles overdraw view, a view for creators to see how many times
a pixel is drawn; Kyanite is unstable enough that overdraw view is best used
with the mod switched off in its settings.

Prefer the fork's issue tracker for bugs in this build. Join the Discord server
for nightly builds: https://discord.gg/B6H6ZVJx9C
