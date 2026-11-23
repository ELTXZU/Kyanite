# 0.2.1-alpha
- Restored full-speed rendering: the hitbox compatibility fallback used to hand
  rendering back to the game while a hitbox viewer was open, which disabled the
  renderer and dropped FPS to vanilla speed. Hitboxes still line up, and Bismuth
  keeps rendering

# 0.2.0-alpha
- Added Geometry Dash 2.2081 support (Geode 5.9.0); the same source also builds for 2.2091
- Hitbox viewers keep working without costing FPS: while MegaHack's show
  hitboxes, GD Mega Overlay or the game's own debug draw is active, the objects
  on screen are activated invisibly so their hitboxes line up, and Bismuth keeps
  rendering the level
- Fixed the active renderer being reused across levels
- Fixed several crashes (missing groups, out of range group ids, invalid
  sprites, missing textures)
- Fixed rotate actions starting at the wrong angle after restarting a level
- Fixed checkpoint and timer state not being cleared on reset
- Fixed vanilla rendering not being fully restored when the renderer failed
- Fixed overdraw view leaking nodes and textures
- Fixed the profiler adding timer overhead while not taking a snapshot
- Fixed difference mode leaving a framebuffer bound when it failed
- Fixed objects with missing sprite sheet textures still taking up buffer space
- Fixed the renderer turning itself off for the whole level (so the mod did
  nothing and gave no FPS gain) when a level contained a single unsupported
  trigger or sprite sheet object
- Levels that use per-object effects (Scale trigger, area/enter effects,
  advanced follow, keyframes) keep rendering with Bismuth; the objects those
  triggers affect keep their initial transform
- Objects from an unsupported sprite sheet are skipped instead of disabling
  the renderer for the whole level
- Fixed the global renderer pointer being left dangling when initialization
  failed, which could make the game use a freed renderer
- Fixed the build on case sensitive filesystems (include paths)

# 0.1.0-alpha
- Added rotate trigger support
- Added follow trigger support
- Added shader sprite optimization
- Added seperate batch node optimization
- Added index culling optimization
- Fixed Intel (and AMD?) GPU crashes

# 0.0.1-alpha.1
- Added renderer
- Added color trigger support
- Added alpha trigger support
- Added move trigger support
- Added toggle trigger support
- Added object HSV
- Objects now rotate
- Pulsating objects now pulse to audio scale