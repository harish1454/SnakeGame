# Aussie Outback Snake

A snake game with an Australian outback / bush theme, built with HTML5 Canvas, CSS, and JavaScript. Fully self-contained in a single `index.html` file with no build step and no external or network dependencies — just open the file and play.

Guide a coastal **carpet python** across the red-dirt outback, gobbling up bush tucker to grow longer without going *walkabout* into a wall or your own tail.

## How to Play

1. Open `index.html` directly in any modern web browser (works from `file://`).
2. On the **Pick Your Track** screen, choose a difficulty:
   - Click **Easy**, **Medium**, or **Hard**, or press **1**, **2**, or **3**.
3. Use the **arrow keys** to steer the python:
   - Up Arrow: Move up
   - Down Arrow: Move down
   - Left Arrow: Move left
   - Right Arrow: Move right
   (The snake can't double back directly on itself.)
4. Eat the **bush tucker** — a witchetty grub, a gum nut, or a quandong — to grow longer and raise your **Tucker** score.
5. Avoid hitting the walls or your own body.
6. When you've **gone walkabout** (game over), press **Enter** or click **Have Another Go** to restart at the same difficulty, or press **Esc** / click **Change Track** to pick a new difficulty.

## Difficulty

| Track  | Speed  | Score multiplier |
|--------|--------|------------------|
| Easy   | Slow   | 1×               |
| Medium | Normal | 2×               |
| Hard   | Fast   | 3×               |

Your displayed Tucker score is the raw count of items eaten multiplied by the track's multiplier.

## Features

- Smooth canvas-based rendering on a 20x20 grid
- Three difficulty levels (Easy / Medium / Hard) with distinct speeds and score multipliers
- Tucker score tracking with on-screen display and a difficulty label
- Carpet-python snake with a patterned body and a direction-indicating head
- Bush-tucker collectibles (witchetty grub, gum nut, quandong) with a pulsing glow
- Animated outback backdrop: warm sky, red-dirt hills, gum-tree silhouettes, and drifting dust motes
- Themed particle bursts and kicked-up dust behind the snake
- No external dependencies required

## Technical Details

- Pure HTML5, CSS, and JavaScript in a single self-contained file
- Canvas size: 400x400 pixels
- Grid: 20x20 cells (20px per cell)
- Difficulty speeds: 150 / 120 / 80 ms per tick (Easy / Medium / Hard)
- Collision detection for walls and self-intersection
