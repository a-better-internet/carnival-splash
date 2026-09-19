# Carnival Splash — Water Gun Gallery

A 3D water-gun shooting gallery that runs from a single HTML file. Hose down ducks,
stunt planes and golden prizes at a night-time midway, chain hits for multipliers, and
try to walk away with the six-foot teddy.

**[index.html](index.html) — open it in a browser. That's the whole game.**

No build step, no bundler, no asset pipeline. Every texture is painted into a `<canvas>`
at boot and every sound is synthesised with the Web Audio API. The only external
dependency is Three.js (r128), loaded from a CDN with a second CDN as fallback.

## Controls

| Input | Action |
| --- | --- |
| Move mouse / drag finger | Aim |
| Hold click / touch | Spray |
| Scroll wheel | Zoom |
| `Esc` or `P` | Pause / resume |
| `R` | Restart the round |
| `M` | Mute |
| `F` | Fullscreen |
| `Space` / `Enter` | Start, resume, or play again |

## How a round works

**Water pressure.** The tank drains while you hold the trigger and refills when you let
go. Run it dry and the gun sputters until it repressurises, so sustained blanket-spraying
is not the optimal strategy.

**Combos.** A hit every 2.2 seconds keeps the chain alive. The multiplier climbs
x2 → x3 → x4 → x5 → x6 at 3, 6, 9, 13 and 20 hits. An old boot snaps it instantly.

**Bullseyes.** The painted discs are worth far more than the bodywork — 150 on a duck or
tug, 200 on a speedboat, 300 on a stunt plane.

**Power-ups.** Floating crates drift across the lane. Splash one for Pressure Surge
(unlimited water), Wide Spray, Slow Motion, Double Points or Time Freeze.

**Prizes.** Score thresholds light up plushes on the shelf behind the pool, from a
goldfish in a bag at 600 up to the Midway Crown at 22,000.

## Modes and difficulty

| Mode | Length | Twist |
| --- | --- | --- |
| Classic | 60 s | The house standard |
| Frenzy | 90 s | Denser spawns, faster targets, a roomier tank, x1.25 score |
| Sharpshooter | 75 s | Only bullseyes pay, and the tank is miserly. x1.6 score |
| Zen | Untimed | No clock, no pressure worries, no records |

Easy / Normal / Hard scale target speed, spawn density, the combo window and the
score multiplier (x0.8 / x1.0 / x1.45).

## Settings

Master volume, music and SFX toggles, graphics quality (Low / Med / High — adjusts
shadows, pixel ratio, particle counts and water tessellation), screen shake, lens
droplets, camera sway, and invert-vertical. High scores, achievements and settings are
kept in `localStorage`; the game degrades gracefully when storage is unavailable.

## Implementation notes

- **Single file, ~5,100 lines**, organised into numbered sections: config, storage,
  audio, textures, scene, targets, power-ups, game flow, UI, update loop.
- **Pooled particles.** Stream droplets, splashes and confetti are `InstancedMesh`
  pools with O(1) free-lists, which keeps the whole particle system at three draw calls
  instead of several hundred.
- **Frame-rate independent.** Spawn rates, spray density, physics, ambient effects and
  the round clock are all driven by elapsed time, so the game plays the same at 30 Hz
  and 144 Hz.
- **Explicit disposal.** Targets release their geometries and materials when they leave
  the scene; shared geometry and materials are registered up front and skipped.
- Typical load: ~50k triangles and ~360 draw calls at High.
