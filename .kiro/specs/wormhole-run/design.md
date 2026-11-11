# Design: Wormhole Run

## Overview

A single-file HTML5 Canvas game (`wormhole-run.html`) with vanilla JS wrapped
in an IIFE. Game logic is separated from rendering so the logic can be
reasoned about (and tested) independently.

## Architecture

```
wormhole-run.html
├── <style>   Space theme (matches index.html)
├── <body>    HUD, canvas, overlays (start / paused / game over), link to classic
└── <script>
    ├── Constants      GRID=20, CELL=20, TICK_MS=110, WORMHOLE_LIFETIME=10000 ms
    ├── State          snake, dir, nextDir, food, wormholes{a,b}, asteroids[],
    │                  score, best, eaten, state (menu|running|paused|over)
    ├── Logic          step(), teleport(), spawnFood(), spawnAsteroid(),
    │                  relocateWormholes(), freeCells(), isBlocked()
    ├── Rendering      drawBackground(), drawWormhole(), drawAsteroid(),
    │                  drawFood(), drawSnake(), drawHud()
    └── Input/Loop     keydown handlers, setInterval tick + requestAnimationFrame render
```

## Key decisions

- **Two loops:** a fixed-interval logic tick (movement) and a
  `requestAnimationFrame` render loop (smooth animation of wormhole swirl).
  The tick interval is cleared before restart to avoid stacked intervals.
- **Free-cell helper:** `freeCells(excludeNearHead)` builds a list of cells not
  occupied by the snake, food, wormholes, or asteroids. All spawners pick from
  it, which guarantees no overlaps and handles the "board full" case by
  returning no cell.
- **Direction buffering:** `nextDir` is validated against the current `dir` so
  quick double key presses cannot cause a 180-degree reversal.

## Step algorithm

```
step():
  dir = nextDir
  head = snake[0] + dir
  if outside grid           -> gameOver
  if head is wormhole W:
      head = other(W)  (same direction continues next tick)
      score += 1; relocate pair (new timer)
  if head in asteroids      -> gameOver
  if head in snake body     -> gameOver   (tail excluded when not growing)
  unshift(head)
  if head == food: score++, eaten++, spawnFood();
                   if eaten % 3 == 0: spawnAsteroid()
  else: pop tail
```

Note: the teleport is resolved before the collision checks, so the exit cell
is checked for asteroids and snake body.

## Wormhole lifecycle

- `wormholeSpawnedAt` timestamp set on (re)location.
- Each tick: if `now - spawnedAt > LIFETIME` relocate; if within the last 2 s,
  the renderer flickers the wormholes as a warning.
- Time is paused/shifted with the pause state so pausing does not expire them.

## Data model

```js
cell      = { x: int, y: int }
wormholes = { a: cell, b: cell, spawnedAt: ms }
asteroid  = { x, y, rot, size }       // rot/size are cosmetic
```

## Persistence

`localStorage['wormholeRunBest']` read/written inside try/catch.

## Error handling / edge cases

- No free cell for food -> player has filled the board: treat as a win (game over screen with "Sector cleared").
- No free cell for asteroid or wormhole -> skip silently.
- localStorage blocked -> best score is session-only.

## Testing approach

Logic is deterministic given state; manual verification via Playwright /
headless browser: teleport behavior, asteroid spawn cadence, reversal block,
pause, restart.
