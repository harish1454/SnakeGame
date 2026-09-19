# Implementation Plan — Zero Gravity Snake

This plan replaces the entire contents of `/projects/sandbox/SnakeGame/index.html` with a single-file HTML/CSS/JS zero-gravity snake game. The existing file is a grid-based classic snake (~39 KB, IIFE-wrapped JS, embedded CSS, space-themed visuals). The new file reuses the same single-file architecture and space-theme colour palette (`#0a0a1a` background, `#00e5ff` accents, `#ffab40` score) but replaces every game mechanic with continuous physics.

---

## Existing code worth preserving (style only, not code)

| Aspect | Current | Carry Forward |
|---|---|---|
| Page title | `<title>Space Snake</title>` | Change to `<title>Zero Gravity Snake</title>` |
| Meta tags | charset UTF-8, viewport | Yes, keep both |
| Colour palette | `#0a0a1a`, `#00e5ff`, `#ffab40`, `#ff5252`, `#7c4dff` | Yes |
| Font stack | Segoe UI, Tahoma, Geneva, Verdana, sans-serif | Yes |
| Body layout | flex centre, overflow hidden | Yes |
| Code style | Single IIFE, `const`/`let`, no modules | Yes, same pattern |

Everything else (grid logic, difficulty system, wall collision, setInterval loop) is discarded.

---

## Chosen constants & design decisions

| Parameter | Value | Rationale |
|---|---|---|
| **Thrust acceleration** | 0.15 px/frame | Feels responsive at 60 fps; ~9 px/s² effective. Player can reverse direction in ~2 s at max speed. |
| **Max speed** | 5 px/frame | 300 px/s at 60 fps. Fast enough to be exciting; canvas crosses in ~2-3 s. |
| **Friction** | 0 (none) | Spec requires friction-free drift. |
| **Snake head radius** | 8 px | Visible but not huge on a ≥600 px canvas. |
| **Body segment radius** | Lerp from 7 → 3 px (head-adjacent → tail) | Flowing taper effect. |
| **Segment spacing** | 14 px (≈ 2 × head radius) | Segments follow a position history trail; one history entry per frame, spaced by recording every N frames or by distance. Using distance-based sampling at 14 px ensures consistent visual density regardless of speed. |
| **Collision skip count** | 5 segments nearest head | At segment spacing 14 px, the first 5 segments span 70 px — enough for a tight U-turn at max speed without false death. |
| **Collision radius sum** | `headRadius + segRadius - 2` (slight forgiveness) | Circle-to-circle overlap; subtract 2 px to allow visual touching without death. |
| **Food radius** | 10 px | Large enough to see; collection distance = `headRadius + foodRadius` = 18 px. |
| **Initial snake length** | 5 segments | Short enough to not self-collide immediately. |
| **Star count** | 150 | Dense enough for atmosphere, cheap to render (just dots). |
| **Particle burst — food** | 15 particles, speed 1–4 px/frame, life 0.5–1.0 s | Quick sparkle. |
| **Particle burst — death** | 40 particles, speed 2–6 px/frame, life 1.0–1.5 s | Dramatic explosion. |
| **Canvas size** | `window.innerWidth × window.innerHeight` (full viewport), re-created on resize | True responsive; no fixed 400×400. |
| **HUD font size** | 18 px, top-left (score, high score), top-right (speed indicator) | Clear but unobtrusive. |
| **localStorage key** | `"zeroGravitySnake_highScore"` | Avoids collision with the old game's storage. |

---

## File section layout

The new `index.html` has this structure:

```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Zero Gravity Snake</title>
  <style>
    /* --- Section A: Reset & full-viewport canvas --- */
    /* --- Section B: Overlay screens (start, game-over) --- */
    /* --- Section C: HUD --- */
  </style>
</head>
<body>
  <!-- Canvas fills viewport; HUD is absolute-positioned over it -->
  <canvas id="gameCanvas"></canvas>
  <div id="hud">…</div>
  <div id="startScreen">…</div>
  <div id="gameOverScreen">…</div>

  <script>
    (function () {
      // --- Section 1: Constants & tuning ---
      // --- Section 2: State variables ---
      // --- Section 3: Utility functions (wrap, distance, clamp, vecMath) ---
      // --- Section 4: Audio (Web Audio API beeps) ---
      // --- Section 5: Stars (background) ---
      // --- Section 6: Snake data structure & physics ---
      // --- Section 7: Food spawning ---
      // --- Section 8: Particle system ---
      // --- Section 9: Collision detection ---
      // --- Section 10: Input handling (keyboard) ---
      // --- Section 11: HUD & screen state machine ---
      // --- Section 12: Rendering ---
      // --- Section 13: Game loop (requestAnimationFrame) ---
      // --- Section 14: Canvas resize handler ---
      // --- Section 15: Init & boot ---
    })();
  </script>
</body>
</html>
```

---

## Data structures

```js
// Snake
const snake = {
  headX: 0, headY: 0,       // floating-point position
  vx: 0, vy: 0,             // velocity in px/frame
  trail: [],                 // Array of {x, y} — recorded positions for body to follow
  length: 5,                 // current number of body segments (grows on food)
};

// Each body segment is derived from snake.trail at index i * SEGMENT_SPACING.
// No separate segment objects needed — just sample the trail array.

// Food
let food = { x: 0, y: 0, pulsePhase: 0 };

// Particles
let particles = [];    // { x, y, vx, vy, life, maxLife, size, color }

// Stars (generated once, re-generated on resize)
let stars = [];        // { x, y, size, brightness, twinkleOffset }

// Input
const keys = {};       // currently-held keys: keys['ArrowUp'] = true

// State machine
let gameState = 'START';  // 'START' | 'PLAYING' | 'GAME_OVER'
let score = 0;
let highScore = parseInt(localStorage.getItem('zeroGravitySnake_highScore')) || 0;
```

---

## Ordered implementation items

- [ ] 1. **HTML shell, CSS reset, canvas, overlay screens, HUD markup**
      Write the `<head>`, `<style>`, and `<body>` HTML. The canvas fills the viewport (`width: 100vw; height: 100vh; display: block;`). Two overlay `<div>`s are absolutely centred: a start screen ("ZERO GRAVITY SNAKE", instructions for arrow/WASD, "Press SPACE to start") and a game-over screen (final score, high score, "Press SPACE to restart"). A HUD `<div>` is fixed top-left/right with score, high score, and speed bar. All styled with the existing colour palette. No JS yet.
      Files: `index.html` (Sections A–C in `<style>`, body HTML)
      Verify: Open the raw HTML in a browser — the start screen overlay is visible, centred, with correct colours and text. HUD is visible in corners.

- [ ] 2. **JS scaffolding: constants, state variables, utility functions (Sections 1–3)**
      Inside the IIFE, define all tuning constants (THRUST, MAX_SPEED, HEAD_RADIUS, SEGMENT_SPACING = 14, COLLISION_SKIP = 5, FOOD_RADIUS = 10, etc.), the snake/food/particles/stars state variables, and utility functions:
      - `wrap(val, max)` → `((val % max) + max) % max` — wraps a coordinate into [0, max).
      - `wrappedDist(x1, y1, x2, y2, w, h)` → shortest toroidal distance (check 9 offsets or use `min(|dx|, w-|dx|)` trick).
      - `clamp(val, min, max)`, `vecLen(vx, vy)`, `vecNorm(vx, vy)`.
      Files: `index.html` (Sections 1–3 in `<script>`)
      Verify: Extract the JS between `<script>` tags into a temp file; run `unset NODE_OPTIONS && node --check /tmp/extracted.js` — exits 0 (no syntax errors).

- [ ] 3. **Web Audio API beeps (Section 4)**
      Create an `AudioContext` lazily on first user interaction. Two functions:
      - `playEatSound()`: short 800→1200 Hz frequency sweep, 0.1 s, sine wave.
      - `playDeathSound()`: 400→100 Hz sweep, 0.4 s, sawtooth wave.
      Gate behind a `audioCtx` null-check so the game works if audio is blocked.
      Files: `index.html` (Section 4)
      Verify: Syntax check as above.

- [ ] 4. **Starfield (Section 5)**
      `generateStars(canvasW, canvasH)` fills the `stars` array with 150 random `{x, y, size, brightness, twinkleOffset}`. `drawStars(ctx, time)` iterates and draws each as a filled circle with twinkling alpha (`0.3 + 0.7 * sin(time * 0.002 + offset)`). Brighter stars (size > 1.5) get a second, larger, faint glow circle.
      Files: `index.html` (Section 5)
      Verify: Syntax check.

- [ ] 5. **Snake physics & trail (Section 6)**
      `updateSnake(keys, canvasW, canvasH)`:
      1. **Thrust**: if ArrowUp/W held, `vy -= THRUST`; similarly for other directions. Multiple keys can be held simultaneously for diagonal thrust.
      2. **Speed cap**: `speed = vecLen(vx, vy); if (speed > MAX_SPEED) { vx *= MAX_SPEED/speed; vy *= MAX_SPEED/speed; }`.
      3. **Position update**: `headX += vx; headY += vy`.
      4. **Wrap-around**: `headX = wrap(headX, canvasW); headY = wrap(headY, canvasH)`.
      5. **Trail recording**: `trail.unshift({x: headX, y: headY})`. Trim trail to `length * SEGMENT_SPACING + SEGMENT_SPACING` entries (keep a buffer beyond what's rendered so the tail doesn't pop).
      Body segment positions are computed on-the-fly during rendering: segment _i_ reads `trail[i * SEGMENT_SPACING]`. No separate segment update loop.
      Files: `index.html` (Section 6)
      Verify: Syntax check. Then a **logic harness** (see Verification Plan §B) drives `updateSnake` for 120 frames with ArrowRight held and asserts: (a) headX increases, (b) speed converges to MAX_SPEED, (c) after wrapping past canvasW, headX is near 0.

- [ ] 6. **Food spawning & collection (Section 7)**
      `spawnFood(canvasW, canvasH)`: random x in [FOOD_RADIUS, canvasW - FOOD_RADIUS], same for y. Ensures food is not within 50 px of the snake head (retry up to 20 times, then accept).
      `checkFoodCollection()`: if `wrappedDist(headX, headY, food.x, food.y, canvasW, canvasH) < HEAD_RADIUS + FOOD_RADIUS`, collect: increment `score`, increase `snake.length` by 3 (smooth growth), `spawnFood()`, `playEatSound()`, emit food particles. Update high score in localStorage if score exceeds it.
      Files: `index.html` (Section 7)
      Verify: Syntax check. Logic harness: place food at a known position, move head onto it, assert score increments and snake.length grows.

- [ ] 7. **Particle system (Section 8)**
      `emitParticles(x, y, count, speedRange, lifeRange, colors)` — pushes particles into the array.
      `updateParticles()` — per particle: `x += vx; y += vy; life -= dt`. Remove dead particles. No gravity (space!). Slight drag (`v *= 0.98`) for visual spread.
      Two convenience wrappers: `emitFoodParticles(x, y)` (15 particles, cyan/gold palette) and `emitDeathParticles(x, y)` (40 particles, red/orange/white palette).
      Files: `index.html` (Section 8)
      Verify: Syntax check.

- [ ] 8. **Self-collision detection (Section 9)**
      `checkSelfCollision()`: iterate body segments starting from index `COLLISION_SKIP`. For each, compute `wrappedDist(headX, headY, seg.x, seg.y, canvasW, canvasH)`. If less than `HEAD_RADIUS + segRadius(i) - 2`, return true (dead). `segRadius(i)` linearly interpolates from 7 to 3 based on `i / snake.length`.
      **Wrap-safe distance**: uses the `min(|dx|, w-|dx|)` formula for both axes, then Euclidean of the two minimized deltas. This correctly detects collision even when head is near one edge and a body segment is near the opposite edge.
      Files: `index.html` (Section 9)
      Verify: Syntax check. Logic harness: construct a snake whose trail loops back on itself; assert collision is detected. Also: construct a straight snake and assert NO collision (especially validating the skip count).

- [ ] 9. **Input handling (Section 10)**
      `keydown` → `keys[e.key] = true; keys[e.code] = true`. `keyup` → delete both. Supports both Arrow keys and WASD (map W→ArrowUp etc. in the thrust section, or check both). Prevent default on game keys during PLAYING state. Space bar transitions: START→PLAYING, GAME_OVER→START (restart). On first keydown, create AudioContext.
      Files: `index.html` (Section 10)
      Verify: Syntax check.

- [ ] 10. **Screen state machine & HUD updates (Section 11)**
       Three states: `START`, `PLAYING`, `GAME_OVER`.
       - `START`: show start overlay, hide game-over overlay, run starfield animation only.
       - `PLAYING`: hide overlays, run full game loop.
       - `GAME_OVER`: show game-over overlay with final score and high score, freeze snake, run starfield + particle animation (death particles finish).
       `transitionTo(newState)` toggles overlay visibility and resets state as needed.
       HUD updates every frame: score text, high score text, speed bar (a small horizontal bar whose filled width = `vecLen(vx,vy) / MAX_SPEED * 100%`).
       Files: `index.html` (Section 11)
       Verify: Syntax check.

- [ ] 11. **Rendering (Section 12)**
       `render(ctx, canvasW, canvasH, time)`:
       1. Clear canvas + draw starfield.
       2. Draw food: glowing orb with radial gradient and pulsating size (`sin(time * 0.004) * 0.15 + 0.85` scale). Outer glow ring.
       3. Draw snake body: for each segment index `i` from `snake.length - 1` down to 1, sample `trail[i * SEGMENT_SPACING]`. Radius tapers. Colour shifts from cyan (head-end) to purple (tail-end) using HSL interpolation. Each segment is a filled circle with a soft glow (`ctx.shadowBlur`). **Wrap-safe rendering**: if a segment is within its radius of an edge, draw it again on the opposite side (ghost copy) — at most 3 extra draws per segment (corners). This avoids visual popping at edges.
       4. Draw snake head: brighter, larger circle with a white core highlight and a neon outer glow. Direction indicator: a small bright dot offset in the velocity direction.
       5. Draw particles.
       6. Draw HUD (or update DOM HUD elements — DOM is simpler and cheaper; use that).
       Files: `index.html` (Section 12)
       Verify: Syntax check.

- [ ] 12. **Game loop & resize handler (Sections 13–14)**
       `requestAnimationFrame`-based loop. Each frame:
       1. Compute `dt` (but use fixed-step: if `dt > 32 ms`, clamp to 16 ms to avoid tunneling on tab-refocus).
       2. If `PLAYING`: `updateSnake()`, `checkFoodCollection()`, `checkSelfCollision()` → if true, `transitionTo('GAME_OVER')`, `emitDeathParticles()`, `playDeathSound()`.
       3. Always: `updateParticles()`.
       4. `render()`.
       Resize handler: on `window 'resize'`, set `canvas.width = window.innerWidth; canvas.height = window.innerHeight;` and call `generateStars()`. If food is now out of bounds, respawn it.
       Files: `index.html` (Sections 13–14)
       Verify: Syntax check.

- [ ] 13. **Init & boot (Section 15)**
       On load: set canvas size, generate stars, spawn food at canvas centre, position snake head at canvas centre with zero velocity, read high score from localStorage, `transitionTo('START')`, start the game loop.
       Files: `index.html` (Section 15)
       Verify: Full syntax check of the entire extracted JS. Then run the full logic test harness (see below).

- [ ] 14. **End-to-end integration & polish**
       Review the complete file for:
       - All `ctx.save()` / `ctx.restore()` balanced.
       - No references to old grid-based variables.
       - All overlay screens show/hide correctly.
       - Wrap-around rendering has no visual seams.
       - Speed indicator updates.
       - High score persists.
       - Death particles appear, then game-over screen shows after a short delay (~1.5 s).
       - Start screen has correct title "ZERO GRAVITY SNAKE" and instructions mentioning Arrow keys/WASD, SPACE to start.
       Apply minor polish: add CSS `text-shadow` glow to title, smooth fade-in on overlays (`opacity` transition), ensure canvas `cursor: none` during play.
       Files: `index.html`
       Verify: Full verification plan (all parts A–D below).

---

## Verification plan

The implementer **must** run every one of these checks before declaring done. "It looks right" is not acceptable.

### A. Syntax check (run after every item)

```bash
# Extract JS from the HTML file and syntax-check it
unset NODE_OPTIONS && sed -n '/<script>/,/<\/script>/{ /<script>/d; /<\/script>/d; p; }' \
  /projects/sandbox/SnakeGame/index.html > /tmp/zg_snake_extracted.js && \
  node --check /tmp/zg_snake_extracted.js && echo "SYNTAX OK" || echo "SYNTAX ERROR"
```

Expected: `SYNTAX OK` with exit code 0. If this fails, fix the JS before proceeding.

### B. Logic test harness

Create a temporary Node.js script `/tmp/zg_snake_test.js` that:

1. Stubs browser globals minimally: `global.window = { innerWidth: 800, innerHeight: 600, addEventListener: () => {} }; global.document = { getElementById: () => ({ classList: { add(){}, remove(){} }, textContent: '', style: {}, addEventListener: () => {} }), addEventListener: () => {}, createElement: () => ({getContext: () => mockCtx}) }; global.localStorage = { store: {}, getItem(k){ return this.store[k] || null; }, setItem(k,v){ this.store[k] = String(v); } }; global.requestAnimationFrame = () => 0; global.AudioContext = undefined; global.webkitAudioContext = undefined;` where `mockCtx` has no-op stubs for all Canvas2D methods (`fillRect(){}`, `beginPath(){}`, `arc(){}`, `fill(){}`, `stroke(){}`, `save(){}`, `restore(){}`, `clearRect(){}`, `createRadialGradient: () => ({ addColorStop(){} })`, `createLinearGradient: () => ({ addColorStop(){} })`, `measureText: () => ({width:0})`, `fillText(){}`, `moveTo(){}`, `lineTo(){}`, `closePath(){}`, and property setters for `fillStyle`, `strokeStyle`, `globalAlpha`, `shadowColor`, `shadowBlur`, `lineWidth`, `font`, `textAlign`, `textBaseline`).

2. Evaluates the extracted JS via `require('vm').runInNewContext(code, sandbox)` where `sandbox` contains the stubs above.

3. Invokes exported-to-sandbox test hooks. **The implementer must add a `window.__TEST_HOOKS__` object** at the end of the IIFE (guarded by `if (typeof window !== 'undefined')`) that exposes:
   - `getSnake()` → returns the snake state object
   - `setKeys(obj)` → sets the key-down map
   - `tick()` → runs one physics + collision frame
   - `getScore()` → returns current score
   - `getHighScore()` → returns high-score from localStorage
   - `getFood()` → returns food position
   - `setFood(x, y)` → override food position
   - `getGameState()` → returns current state string
   - `startGame()` → transitions to PLAYING
   - `getCanvasSize()` → returns {w, h}
   - `getParticles()` → returns particles array

4. Runs these test cases (assert with `console.assert` or throw on failure):

   **T1 — Thrust & speed cap**: `startGame()`, `setKeys({ArrowRight: true})`, run 200 ticks. Assert `snake.vx` is within 0.01 of `MAX_SPEED` and `snake.vy` is ~0. Assert `vecLen(snake.vx, snake.vy) <= MAX_SPEED + 0.01`.

   **T2 — Inertia / no friction**: after T1, `setKeys({})` (release all), run 60 ticks. Assert `snake.vx` is unchanged (still ~MAX_SPEED). Assert snake keeps moving (headX changes).

   **T3 — Wrap-around**: set `snake.headX = canvasW - 1`, `vx = MAX_SPEED`, run 10 ticks. Assert `snake.headX < canvasW / 2` (it wrapped to the left side).

   **T4 — Food collection**: `startGame()`, place food at `snake.headX + 5, snake.headY` (within collection range). `tick()`. Assert score increased by 1, snake.length increased by 3.

   **T5 — High score persistence**: set score to 50, collect food. Assert `localStorage.getItem('zeroGravitySnake_highScore')` ≥ 51.

   **T6 — Self-collision**: manually build a trail that loops back (head at (100,100), trail curves around and a segment at index > COLLISION_SKIP is at (100,100)). Call collision check. Assert game state transitions to GAME_OVER.

   **T7 — No false self-collision**: straight trail, 20 segments. Assert collision check returns false.

   **T8 — Particle emission on food**: collect food, assert particles array length > 0.

   **T9 — Particle emission on death**: trigger self-collision, assert particles array length > 20 (death burst).

   **T10 — State machine**: assert initial state is START. `startGame()` → PLAYING. Trigger death → GAME_OVER.

```bash
unset NODE_OPTIONS && node /tmp/zg_snake_test.js && echo "ALL TESTS PASSED"
```

Expected: `ALL TESTS PASSED` with exit code 0.

### C. HTML validity (best-effort)

```bash
unset NODE_OPTIONS && xmllint --html --noout /projects/sandbox/SnakeGame/index.html 2>&1 | grep -c "error" | xargs -I{} test {} -eq 0 && echo "HTML OK" || echo "HTML has errors (review manually)"
```

xmllint is available in this environment. Zero errors expected (minor warnings about HTML5 tags are acceptable).

### D. File-size sanity check

```bash
wc -c /projects/sandbox/SnakeGame/index.html
```

Expected: between 15,000 and 60,000 bytes. Less than 15 KB likely means missing features; more than 60 KB suggests accidental duplication.

### E. Manual spot-check checklist (implementer reads the code and confirms each)

- [ ] Title says "ZERO GRAVITY SNAKE"
- [ ] No references to `GRID_SIZE`, `CELL_SIZE`, or grid-based movement
- [ ] `localStorage` key is `zeroGravitySnake_highScore`
- [ ] `requestAnimationFrame` is used (not `setInterval`)
- [ ] Canvas resizes on `window.resize`
- [ ] Speed is capped at `MAX_SPEED`
- [ ] No friction applied to velocity
- [ ] Wrap-around uses modular arithmetic on both X and Y
- [ ] Collision skips first `COLLISION_SKIP` body segments
- [ ] WASD and Arrow keys both work
- [ ] Start screen, playing state, and game-over screen all exist
- [ ] `window.__TEST_HOOKS__` object is present

---

## Physics recap (reference for implementer)

**Per-frame update (at 60 fps via rAF):**

```
// Thrust (only while key held)
if (keys.ArrowUp || keys.w || keys.W)    vy -= THRUST;  // 0.15
if (keys.ArrowDown || keys.s || keys.S)  vy += THRUST;
if (keys.ArrowLeft || keys.a || keys.A)  vx -= THRUST;
if (keys.ArrowRight || keys.d || keys.D) vx += THRUST;

// Speed cap
speed = sqrt(vx² + vy²);
if (speed > MAX_SPEED) { vx *= MAX_SPEED/speed; vy *= MAX_SPEED/speed; }

// No friction — velocity persists

// Position
headX = wrap(headX + vx, canvasW);
headY = wrap(headY + vy, canvasH);

// Trail
trail.unshift({x: headX, y: headY});
if (trail.length > length * SEGMENT_SPACING + SEGMENT_SPACING) trail.pop();
```

**Wrap-safe distance (for collision & food):**

```
function wrappedDist(x1, y1, x2, y2, w, h) {
  let dx = Math.abs(x1 - x2);
  let dy = Math.abs(y1 - y2);
  dx = Math.min(dx, w - dx);
  dy = Math.min(dy, h - dy);
  return Math.sqrt(dx * dx + dy * dy);
}
```

**Wrap-safe rendering:**

For each segment, if `seg.x < radius`, also draw at `seg.x + canvasW`. If `seg.x > canvasW - radius`, also draw at `seg.x - canvasW`. Same for Y. This handles edge and corner wrapping with at most 3 ghost copies per segment.
