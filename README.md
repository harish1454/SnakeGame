# Aussie Serpent 🐍🦘

An Australian outback-themed Snake game built with HTML5 Canvas, CSS, and JavaScript. Guide your serpent across the dusty outback, gobbling up classic Aussie tucker while dodging native wildlife. Fully self-contained in a single HTML file with no external dependencies.

## How to Play

1. Open `index.html` in any modern web browser.
2. **Select a difficulty level** by pressing `1`, `2`, or `3` on the keyboard, or clicking the difficulty buttons:
   - **Easy** — Slower pace, 1× score multiplier
   - **Medium** — Moderate pace, 1.5× score multiplier
   - **Hard** — Fast pace, 2× score multiplier
3. Use the **arrow keys** to control the serpent's direction:
   - Up Arrow: Move up
   - Down Arrow: Move down
   - Left Arrow: Move left
   - Right Arrow: Move right
4. Eat Australian food items to grow longer and increase your score:
   - 🥧 **Meat Pies**
   - 🍞 **Vegemite Toast**
   - 🍫 **Tim Tams**
   - 🍰 **Lamingtons**
5. As your score increases, **wildlife obstacles** appear on the board — watch out for **kangaroos** and **redback spiders**! Colliding with them ends the game.
6. Enjoy the **Australian slang popups** that appear as you eat food and hit milestones.
7. When the game ends, check your **score** and **high score**, then press **Enter** or click **Play Again** to restart.

## Features

- 🏜️ Outback-themed visuals with earthy tones and dusty colours
- 🍽️ Four Australian food types: meat pies, vegemite toast, Tim Tams, and lamingtons
- 🦘 Wildlife obstacle system — kangaroos and redback spiders appear as difficulty ramps up
- 🗣️ Australian slang popups triggered by eating food and reaching score milestones
- 🏆 Session and persistent high-score tracking
- 📊 Three difficulty levels (Easy, Medium, Hard) with score multipliers
- Smooth canvas-based rendering on a 20x20 grid
- Start screen with difficulty selection and instructions
- Game over screen with final score, high score, and restart option
- No external dependencies required

## Technical Details

- Pure HTML5, CSS, and JavaScript
- Canvas size: 400×400 pixels
- Grid: 20×20 cells (20px per cell)
- Game speed varies by difficulty:
  - Easy: 150ms per tick (~6.7 FPS)
  - Medium: 120ms per tick (~8.3 FPS)
  - Hard: 80ms per tick (~12.5 FPS)
- Collision detection for walls, self-intersection, and wildlife obstacles
- Single HTML file with no dependencies
