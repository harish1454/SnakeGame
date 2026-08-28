# Snake Game

A classic Snake game built with HTML5 Canvas, CSS, and JavaScript. Fully self-contained in a single HTML file with no external dependencies.

## How to Play

1. Open `index.html` in any modern web browser.
2. Click **Start Game** or press any arrow key to begin.
3. Use the **arrow keys** to control the snake's direction:
   - Up Arrow: Move up
   - Down Arrow: Move down
   - Left Arrow: Move left
   - Right Arrow: Move right
4. Eat the red food to grow longer and increase your score.
5. Avoid hitting the walls or your own tail.
6. When the game ends, press **Enter** or click **Play Again** to restart.

## Snake Types

The game features multiple snake types, each with unique abilities and visual styles:

| Snake Type | Description | Special Ability |
|------------|-------------|-----------------|
| 🐍 Classic | The original green snake | None — pure skill! |
| ⚡ Speed Viper | A sleek blue snake | Moves 25% faster, scores double points |
| 🔥 Fire Serpent | A red-orange blazing snake | Can pass through walls (wraps around) |
| 👻 Ghost Snake | A translucent pale snake | Can pass through its own body 3 times per game |
| 🌀 Portal Python | A purple iridescent snake | Teleports to a random safe location once every 30 seconds |
| 🧊 Frost Cobra | An icy cyan snake | Temporarily freezes time (pauses game speed) on command |

### Unlocking Snake Types

- **Classic** — Available from the start
- **Speed Viper** — Score 50+ points in a single game
- **Fire Serpent** — Score 100+ points in a single game
- **Ghost Snake** — Play 10 games
- **Portal Python** — Score 75+ points without using any power-ups
- **Frost Cobra** — Achieve a 150+ point high score

## Features

- Smooth canvas-based rendering on a 20x20 grid
- Score tracking with on-screen display
- Start screen with instructions
- Game over screen with final score and restart option
- Visual polish: gradient snake body, glowing food, subtle grid, and animated eyes
- Multiple snake types with unique abilities
- No external dependencies required

## Technical Details

- Pure HTML5, CSS, and JavaScript
- Canvas size: 400x400 pixels
- Grid: 20x20 cells (20px per cell)
- Game speed: ~8 frames per second
- Collision detection for walls and self-intersection
