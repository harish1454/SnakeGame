# Zero Gravity Snake — single-file HTML5 game

Replaces a grid-based classic Snake game with a continuous-physics, Asteroids-style zero-gravity variant — all in a single self-contained `index.html` (26 KB, no external dependencies). Movement is momentum-based with thrust from arrow keys and WASD, friction-free drift, and a speed cap. The canvas renders a neon-glowing chain of circles over a twinkling starfield with wrap-around edges, particle effects on food collection and death, Web Audio beeps, a three-state screen machine (START → PLAYING → GAME_OVER), and a DOM-based HUD with score, localStorage high score, and a speed bar. Watch for: the physics loop is not delta-time-scaled (confirmed), meaning gameplay speed varies with frame rate; food has no wrap-safe rendering so it visually pops at edges (confirmed); and the head's wrap-safe rendering omits diagonal corner ghosts (confirmed).

**Verdict**: NEEDS_CHANGES

## High-level view

The physics are per-frame fixed-step: `THRUST` and `MAX_SPEED` are in px/frame, and `updateSnake()` is called once per `requestAnimationFrame` callback regardless of how much wall time elapsed. The `dt` variable is computed but only passed to `updateParticles`; the snake itself accelerates, moves, and collides identically whether the frame took 5 ms or 30 ms. On a 120 Hz monitor the snake accelerates twice as fast and reaches double effective speed. On a background tab that resumes, the dt-clamp prevents a single giant jump but doesn't fix the per-frame coupling. This is the most impactful issue for playability on non-60 Hz hardware.

Wrap-around rendering is thorough for body segments — all eight edge/corner ghost copies are drawn — but the head only draws four cardinal ghosts and skips the four diagonal corner cases. Food draws no ghosts at all: when the orb drifts near an edge its glow is clipped and it pops into view on the opposite side. Neither is likely to cause gameplay bugs (collision math is wrap-safe), but both produce visible artifacts.

<details>
<summary>Issues (4)</summary>

1. **Physics not delta-time-scaled** (BLOCKING, confirmed) — `updateSnake()` applies constant `THRUST` and `MAX_SPEED` per call with no `dt` factor. On monitors above or below 60 Hz the game runs proportionally faster or slower. Multiply thrust and position deltas by `dt / 16.67` (or refactor to per-second units scaled by `dtSec`).
2. **Food not wrap-rendered** (MINOR, confirmed) — `drawFood()` draws the orb at its raw coordinates with no ghost copies at edges. Add the same cardinal+corner ghost pattern used for body segments.
3. **Head missing diagonal corner ghosts** (MINOR, confirmed) — `drawSnake()` draws head ghosts for the four cardinal edges but not the four corner cases. Add diagonal ghost draws matching the body segment pattern.
4. **`keys` object not cleared on restart** (MINOR, confirmed) — `transitionTo('PLAYING')` resets snake, score, and particles but does not clear the `keys` map. If a player holds a movement key while pressing SPACE to restart, thrust is applied on the very first physics tick, which feels jarring and is inconsistent with the "zero velocity" reset.

</details>

<details>
<summary>Details</summary>

## Frame-rate-coupled physics

The game loop computes `dt` and `dtSec` but only feeds them into `updateParticles(dtSec)`. The core physics in `updateSnake()` — thrust accumulation, speed capping, position integration, trail recording — all operate in per-frame units with no time scaling:

```js
// Thrust (constant per frame, not scaled by dt)
if (keys['ArrowUp'] || keys['w'] || keys['W'] || keys['KeyW']) {
  snake.vy -= THRUST;  // 0.15 px/frame
}
// ...
snake.headX += snake.vx;  // px/frame, not px/sec * dtSec
```

At 60 fps this produces 0.15 × 60 = 9 px/s² acceleration and 5 × 60 = 300 px/s max speed, matching the plan. At 120 Hz (common on modern monitors and phones) the effective acceleration doubles to 18 px/s² and max traversal speed doubles to 600 px/s, making the game dramatically faster. At 30 fps (throttled tab or slow device) it halves.

The fix is straightforward: define constants in per-second units (`THRUST_PER_SEC = 9`, `MAX_SPEED_PER_SEC = 300`), pass `dtSec` into `updateSnake`, and multiply accordingly. Trail recording needs a distance-based gate rather than per-frame push to keep segment spacing consistent across frame rates.

The collision skip count and spawn protection window (both measured in ticks) are also frame-rate-dependent. After converting to time-based physics, these should be expressed in elapsed seconds rather than tick counts.

## Wrap-safe rendering gaps

Body segments get the full eight-ghost treatment (four edges + four corners), which is correct. The head only gets four cardinal ghosts:

```js
if (snake.headX < hr) { drawHeadGhost(snake.headX + canvasW, snake.headY); }
if (snake.headX > canvasW - hr) { drawHeadGhost(snake.headX - canvasW, snake.headY); }
if (snake.headY < hr) { drawHeadGhost(snake.headX, snake.headY + canvasH); }
if (snake.headY > canvasH - hr) { drawHeadGhost(snake.headX, snake.headY - canvasH); }
// Missing: four diagonal corner cases
```

When the head sits exactly in a corner (both x and y within `HEAD_RADIUS` of their respective edges), the diagonal ghost on the opposite corner is not drawn. The visual effect is the head's glow cutting off in the corner for a frame or two.

Food rendering (`drawFood`) has no ghost logic at all. Its glow radius extends to `FOOD_RADIUS * pulse * 1.5` ≈ 15 px, so whenever the orb spawns within 15 px of an edge the radial gradient is clipped. Because `spawnFood` constrains food to `[FOOD_RADIUS, canvasW - FOOD_RADIUS]`, the core circle is always on-screen, but the outer glow still clips. More importantly, a player approaching the edge sees no hint of food on the opposite side until they wrap around, which breaks the Asteroids-style seamlessness.

## Keys not cleared across restart

`transitionTo('PLAYING')` resets `score`, `ticksSinceStart`, the snake, food, and particles. The `keys` object — which tracks currently-held keyboard state — is not reset. If a player holds ArrowRight while pressing SPACE:

1. `transitionTo('PLAYING')` fires, resetting velocity to 0.
2. On the very next frame, `updateSnake()` reads `keys['ArrowRight'] === true` and applies THRUST.
3. The snake immediately starts moving right from the first frame, contradicting the "starts at rest" experience.

The fix is to add `for (var k in keys) { delete keys[k]; }` inside the `PLAYING` branch of `transitionTo`.

## Verification coverage gaps

The harness does not exercise delta-time scaling (it calls `tick()` without varying frame intervals), wrap-safe rendering (it tests physics, not draw calls), or the keys-not-cleared scenario (`setKeys` clears the map before setting new values, masking the bug). These gaps are consistent with the issues flagged above.

</details>

<details>
<summary>File map</summary>

| File | Change |
|------|--------|
| `index.html` | Full rewrite: grid-based classic Snake → continuous-physics zero-gravity Snake with momentum, wrap-around, neon rendering, particles, Web Audio, HUD, three-state screen machine |
| `.agents/tasks/zero-gravity-snake/plan.md` | Implementation plan with constants, data structures, section layout, and verification strategy |
| `.agents/tasks/zero-gravity-snake/verification.md` | Verification report: syntax check, 16-test logic harness, HTML validity, file size, manual checklist |

Full diff: `git -C /projects/sandbox/SnakeGame diff main...feature/zero-gravity-snake`

</details>
