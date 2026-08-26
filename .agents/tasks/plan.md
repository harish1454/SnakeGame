# Implementation Plan — Ocean / Sea-Serpent Theme for Snake

All work happens in the single file `/projects/sandbox/SnakeGame/index.html`. There is no
build system or test framework in this repo (confirmed: only `index.html`, `README.md`,
`screenshots/`, and an empty `.kiro/settings`). The project's real "test" is opening the
file in a browser and exercising the game. Every verification step below reflects that.

## Key architecture facts (so the coder need not re-read the original for understanding)

- Canvas is `400x400`, `GRID_SIZE = 20`, `CELL_SIZE = 20`, `GAME_SPEED = 120` ms. Constants
  live in an IIFE in the `<script>` at the bottom.
- The game runs on `gameLoop = setInterval(function(){ update(); if (gameRunning) draw(); }, GAME_SPEED)`.
  **Rendering is currently locked to the 120 ms logic tick**, so there is no room for
  between-move animation. This is the single most important thing to change.
- `draw()` (the only render function) currently: (1) fills `COLORS.background`, (2) strokes
  a faint grid, (3) draws the food as a glowing red circle centered in its cell, (4) loops
  `snake[]` drawing head (bright rect + direction-based eyes) and body (alpha-tapered rects
  with outline).
- Pure state/logic — **must remain byte-for-byte untouched in behavior**: `initGame`,
  `spawnFood`, `update` (wall + self + food collision, scoring, grow/shrink), the `keydown`
  handler (arrow controls + start-on-arrow + Enter-to-restart), `startGame`, `gameOver`,
  `restartGame`, and the button handlers. These reference `snake`, `food`, `direction`,
  `nextDirection`, `score`, `gameRunning`, `gameStarted`.
- Snake segments and `food` are grid coordinates (cell indices), converted to pixels via
  `* CELL_SIZE`. All new rendering must keep using these same coordinates so gameplay,
  collisions, and hitboxes are visually consistent with logic.
- The two overlays (`#startScreen`, `#gameOverScreen`) are DOM/CSS layered over the canvas;
  a standalone `draw()` call at the very end of the IIFE paints the initial background.

## Design decisions (made here; do not re-decide during implementation)

- **Decouple rendering from logic via `requestAnimationFrame`.** Keep `setInterval` driving
  `update()` at exactly 120 ms (preserves gameplay speed and control feel), but move ALL
  drawing into a continuous rAF render loop that runs whenever the page is open. This is the
  cleanest way to get "alive between moves" ambient motion without touching game timing.
  Chosen over "shorten the interval / interpolate" because that would alter gameplay cadence.
- **Food = glowing pearl** (not a fish). A radial-gradient pearl with a soft pulsing
  bioluminescent halo is simpler to draw reliably, reads clearly at 18px, and matches the
  bioluminescent serpent. Chosen over a fish sprite to minimize risk and keep it recognizable.
- **Snake = smoothed, tapered, glowing serpent body** drawn as one path through the segment
  centers (round joins/caps, width tapering head→tail, teal→bioluminescent gradient, soft
  glow), plus a rounded head and the existing direction-based animated eyes. Chosen over
  per-cell circles because a single stroked path gives the "fluid eel" look while still
  hugging the grid coordinates exactly.
- **Ambient motion = rising bubbles + a few drifting light rays.** Both are cheap, additive,
  and time-driven. Particle counts are capped and glow is used sparingly for performance.

---

# Implementation Plan

- [ ] 1. Retheme the CSS palette and all on-screen copy to the ocean theme (no logic/DOM-id changes).
      Update `<title>` to `Abyssal Serpent`. In `<style>`: change `body` background to a deep
      ocean color (e.g. `#021019`); recolor `.title`, `.score-display`, canvas `border` +
      `box-shadow`, `.overlay h2`, `.btn` (and `:hover`/`:active`), `.final-score`, and
      `.instructions` from the green/yellow scheme to a teal/cyan/aqua scheme (e.g. accent
      `#39e6c8`, secondary `#7fdfff`, glow `rgba(57,230,200,.35)`); change `.overlay`
      background tint to a deep-blue translucent (e.g. `rgba(3,16,25,.9)`). Update text nodes:
      header `.title` "Snake" → "Abyssal Serpent"; `#startScreen h2` "Snake Game" → e.g.
      "Abyssal Serpent"; start `<p>` reworded to same meaning ("Use the arrow keys to steer
      your serpent. Devour glowing pearls to grow and score. Avoid the reef walls and your own
      tail!"); mention pearls (not "red food"); keep the "Press any arrow key to begin" and
      "Press Enter or click to restart" instruction lines meaning-identical. Do NOT rename any
      element `id`, class used by JS, or button text logic. Also update `README.md` "red food"
      → "glowing pearl" wording to stay consistent (optional but preferred).
      Files: `/projects/sandbox/SnakeGame/index.html` (and `README.md`)
      Verify: open `index.html` in a browser (e.g. `python3 -m http.server 8000` from the
      SnakeGame dir, visit `http://localhost:8000`); confirm the start screen shows the new
      title/copy in teal/cyan and the DevTools console has zero errors.

- [ ] 2. Decouple rendering from the logic tick by introducing a continuous rAF render loop and a
      shared animation clock. Add module-scope vars near the other `let` declarations:
      `let animTime = 0;`, `let lastFrame = 0;`, `let rafId = null;`. Add a `render(now)`
      function that computes `const dt = (now - lastFrame) || 16; lastFrame = now; animTime = now;`,
      calls the (existing, soon-to-be-refactored) `draw()`, then `rafId = requestAnimationFrame(render)`.
      Start it once at the bottom of the IIFE with `requestAnimationFrame(render)` (replacing the
      current standalone `draw();` call). In `startGame`, change the interval body to
      `gameLoop = setInterval(update, GAME_SPEED);` (update only — no `draw()` inside the
      interval, and drop the `if (gameRunning) draw()` line). Leave `update`, `gameOver`,
      `clearInterval` logic otherwise unchanged. `draw()` must be safe to call in every state:
      guard the food+snake drawing blocks with `if (gameStarted && snake.length)` so nothing
      grid-related paints under the start overlay; the background/ambient always paints.
      Files: `/projects/sandbox/SnakeGame/index.html`
      Verify: reload in browser; the background is continuously repainting (confirm by adding a
      temporary `console.log` in `render` if needed, then remove). Play a full game — snake
      still moves at the same ~8 fps cadence, arrow keys steer, eating grows the snake, wall
      and self collision trigger game over, Enter and "Play Again" restart. Console clean.

- [ ] 3. Replace the flat background + grid with a deep-ocean vertical gradient and a subtle
      seabed. In `draw()`, replace the `ctx.fillStyle = COLORS.background; ctx.fillRect(...)`
      block with a cached `ctx.createLinearGradient(0, 0, 0, CANVAS_SIZE)` (build once, store
      in a module var to avoid per-frame allocation): stops ~ `0` lighter teal/cyan
      (`#0b6b7a` / `#0e8a99`), mid `#063a4d`, bottom deep navy (`#02121d`). Replace the bright
      grid loop with either a very faint teal grid (alpha ~0.05) or a soft darker gradient band
      along the bottom suggesting a seabed. Update the `COLORS` object: repoint `background`,
      `grid`, and add ocean colors (`waterTop`, `waterMid`, `waterBottom`, `serpent`,
      `serpentHead`, `serpentGlow`, `pearl`, `pearlGlow`) so all new draw code reads from
      `COLORS`. Keep `CANVAS_SIZE`/`CELL_SIZE` usage identical.
      Files: `/projects/sandbox/SnakeGame/index.html`
      Verify: reload; canvas shows a top→bottom teal→deep-navy gradient with a subtle/near-
      invisible grid, both on the start screen and during play. No console errors; framerate
      smooth (no stutter).

- [ ] 4. Add performant ambient motion: rising bubbles + drifting light rays, drawn behind the
      food/snake. Add a `bubbles` array initialized once (cap ~18–24 objects, each
      `{x, y, r, speed, drift, phase}` with `r` ~1–3px). Add an `updateAndDrawBubbles(dt)`
      helper: move each bubble up by `speed*dt`, add gentle horizontal wobble via
      `Math.sin(animTime*k + phase)`, wrap to the bottom with a new random `x` when `y < -r`;
      draw as low-alpha white/cyan circles (no per-bubble shadowBlur — use alpha only for
      performance). Add an `drawLightRays()` helper drawing 2–3 wide, low-alpha (≈0.04–0.07)
      diagonal beams whose x-offset drifts with `Math.sin(animTime * small)`; use a
      `createLinearGradient` fading to transparent. Call `drawLightRays()` then
      `updateAndDrawBubbles(dt)` in `draw()` immediately after the background/seabed and
      before food/snake. Pass `dt` from `render` into `draw(dt)` (update `render` to call
      `draw(dt)` and `draw` signature to accept it; keep default so the guard blocks still work).
      Files: `/projects/sandbox/SnakeGame/index.html`
      Verify: reload; bubbles rise and gently wobble and light rays slowly drift on both the
      start screen and during play, motion is subtle (not distracting) and stays smooth.
      Confirm CPU stays reasonable (DevTools Performance/FPS meter shows steady ~60fps) and
      console is clean.

- [ ] 5. Replace the red-square/red-circle food with a glowing pearl. In `draw()`, replace the
      food block: keep `foodX/foodY` cell-center math and `foodRadius` (~`CELL_SIZE/2 - 2`).
      Draw a pulsing halo (`ctx.shadowColor = COLORS.pearlGlow`, `shadowBlur` oscillating with
      `6 + 4*Math.sin(animTime*0.006)`) behind a `ctx.createRadialGradient` pearl: bright
      white/cyan highlight offset slightly toward top-left, mid soft cyan, edge translucent
      teal. Reset `ctx.shadowBlur = 0` after. Do not change `food` coordinates, `spawnFood`, or
      the collision check in `update` — only the visual.
      Files: `/projects/sandbox/SnakeGame/index.html`
      Verify: start a game; the food renders as a glowing, gently pulsing pearl at the correct
      cell; eating it still increments the score, grows the snake, and respawns the pearl at a
      free cell (never on the snake). Console clean.

- [ ] 6. Render the snake as a glowing sea serpent gliding through water, preserving grid
      alignment and the animated eyes. Rewrite the snake-drawing loop in `draw()`:
      (a) Build an array of pixel centers from `snake[]` (`cx = x*CELL_SIZE + CELL_SIZE/2`, etc).
      (b) Draw the body as ONE smoothed path through those centers using `moveTo` +
      `quadraticCurveTo` (midpoint smoothing), stroked with `lineJoin='round'`, `lineCap='round'`,
      a `strokeStyle` from a head→tail `createLinearGradient` (bright bioluminescent
      `COLORS.serpentHead` → teal/green `COLORS.serpent`), `shadowColor = COLORS.serpentGlow`
      with a modest `shadowBlur` (~8, applied once for the whole stroke, not per segment, for
      performance). Use a line width that tapers head→tail — either stroke once at a mid width
      then overlay a thinner brighter core, or draw the path in a few width bands; keep it
      simple and smooth. Optional subtle glide: offset each center perpendicular to travel by a
      few px via `Math.sin(animTime*0.004 + index)` — VISUAL ONLY, never write back to
      `snake[]`. (c) Draw a rounded serpent head (filled arc/gradient at `snake[0]` center) and
      re-use the EXISTING direction-based eye placement logic (the `direction.x/​.y` branches and
      two `ctx.arc` eyes) so eyes still track heading. Remove the old per-segment
      `fillRect`/`strokeRect` rectangles. Reset `shadowBlur` to 0 at the end.
      Files: `/projects/sandbox/SnakeGame/index.html`
      Verify: play a game; the snake looks like a continuous glowing teal/green serpent with
      eyes on the head pointing in the travel direction; body follows every turn tightly on the
      grid; length visibly grows on eating. Self-collision and wall-collision still end the
      game exactly as before (the visual offset must not change where collisions occur).
      Console clean.

- [ ] 7. Full regression + theme pass. Confirm the file is still one self-contained `index.html`
      with zero external assets/URLs (no `<link>`, `<img>`, `<script src>`, `@import`, remote
      fonts, or fetches — everything drawn via canvas/CSS). Exercise the complete flow:
      start-screen visible with ocean copy → press an arrow key AND click "Start Game" (both
      entry paths) → steer with all four arrows (no reverse-into-self) → eat several pearls
      (score increments, serpent grows) → trigger a wall collision and a self collision (game
      over screen shows correct final score) → restart with both Enter and the "Play Again"
      button. Confirm ambient bubbles/rays animate on every screen and there are no console
      errors or frame stutters.
      Files: `/projects/sandbox/SnakeGame/index.html`
      Verify: `grep -nE "src=|href=|@import|https?://|fetch\(" index.html` returns no external
      dependency (only in-document styles/scripts); manual browser run of the full flow above
      passes with a clean console.

## Notes / assumptions

- No automated test harness exists, so verification is manual browser testing via a local
  static server (`python3 -m http.server`) plus DevTools console — this is the project's
  actual validation method, not a substitute for missing tests.
- Gameplay constants (`GRID_SIZE`, `CELL_SIZE`, `GAME_SPEED`) and all logic functions are left
  behavior-identical; only the render pipeline, CSS, and copy change. The only structural code
  change is moving `draw()` from the `setInterval` body into a `requestAnimationFrame` loop
  (Step 2), which is required to satisfy the "alive between moves" animation requirement.
- The optional serpent glide offset (Step 6) is cosmetic and must never be written back into
  `snake[]`; if it complicates collision-visual alignment, drop it — the gradient + glow +
  smoothing already deliver the eel look.
