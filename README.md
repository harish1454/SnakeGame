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

## Features

- Smooth canvas-based rendering on a 20x20 grid
- Score tracking with on-screen display
- Start screen with instructions
- Game over screen with final score and restart option
- Visual polish: gradient snake body, glowing food, subtle grid, and animated eyes
- No external dependencies required

## Technical Details

- Pure HTML5, CSS, and JavaScript
- Canvas size: 400x400 pixels
- Grid: 20x20 cells (20px per cell)
- Game speed: ~8 frames per second
- Collision detection for walls and self-intersection
