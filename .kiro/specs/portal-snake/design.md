# Design Document

## Overview

A single self-contained file, `portal-snake.html`, with an inline `<style>` and an IIFE `<script>`. The game is a fixed-interval tick loop (`setTimeout`, so the interval can change with speed) plus a `requestAnimationFrame` render loop for animations (portal swirl, bonus-food countdown). Game logic is kept separate from rendering so rules can be reasoned about in isolation.

## Architecture

```
Input (keydown / buttons)
        │
        ▼
  State machine: 'start' → 'playing' ⇄ 'paused' → 'gameover' → 'playing'
        │
        ▼
  tick()  ── logic only: move, portals, collisions, food, obstacles, speed
        │
        ▼
  render() ── draws board, entities, HUD updates (rAF loop)
```

## State

| Field | Type | Notes |
|---|---|---|
| `snake` | `{x,y}[]` | index 0 is head |
| `dir`, `nextDir` | `{x,y}` | `nextDir` buffered between ticks |
| `food` | `{x,y}` | regular food |
| `bonus` | `{x,y,expires}` or null | `expires` is a `performance.now()` timestamp; paused time is added back on resume |
| `portals` | `[{x,y},{x,y}]` | pair A (blue) and B (orange) |
| `obstacles` | `{x,y}[]` | max 15 |
| `score`, `best`, `eaten`, `level` | numbers | `level = floor(eaten/5)` |
| `state` | string | state machine value |

## Key algorithms

- **`emptyCell(extraExclusions)`**: collects all grid cells not occupied by snake, food, bonus, portals, obstacles (and optional exclusion predicate), then picks one at random. Returns `null` if none, in which case spawn is skipped (food: game is won/ended).
- **Tick order**:
  1. Apply `nextDir` to `dir`.
  2. Compute `head = snake[0] + dir`.
  3. Wall check → game over.
  4. If `head` is on a portal: replace `head` with the paired portal cell. (Teleport happens within the same tick, so the snake appears at the exit; the next tick moves one cell onward.)
  5. Obstacle / self check (self check excludes the tail cell when the snake is not growing) → game over.
  6. `unshift(head)`.
  7. If head is on food: score +10, `eaten++`, spawn food, maybe add obstacle, maybe spawn bonus, check portal relocation, update speed; else if on bonus: score +50, grow; else `pop()`.
  8. Expire bonus if past `expires`.
- **Portal relocation**: after each score update, if `floor(score/100)` increased, call `placePortals()`. Placement avoids snake, food, obstacles, bonus.
- **Obstacle placement**: exclusion predicate rejects cells within Manhattan distance 3 of the head or in the next 3 cells straight ahead.
- **Speed**: `interval = max(60, 140 - 8*level)`.

## Persistence

`best` is read from and written to `localStorage['portalSnakeBest']` inside `try/catch`; fallback is an in-memory value.

## UI

- Header: title, `Score`, `Best`, `Speed`.
- Canvas 400x400 with absolutely positioned overlays for start, pause and game over (same pattern as `index.html`).
- Colour palette: dark teal background, blue/orange portals, grey-brown stone obstacles, lime snake, red food, gold bonus.

## Error handling / edge cases

- Reverse-direction input rejected against the *applied* `dir`, not the buffered one, so two quick key presses cannot cause a 180° turn.
- `keydown` handlers call `preventDefault()` for arrows and Space.
- Pausing stops the tick timer; resume shifts `bonus.expires` by the paused duration.
- No empty cell for food → treat as a win and end the game.

## Testing approach

Manual play-through checklist (see tasks) plus a headless smoke check that the script parses and runs a number of ticks without throwing.
