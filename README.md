# 🐍 Abyssal Serpent

> A deep-ocean themed Snake game built with HTML5 Canvas, CSS, and JavaScript. Fully self-contained in a single HTML file with zero external dependencies.

<p align="center">
  <img src="screenshots/03-gameplay-active.png" alt="Abyssal Serpent Gameplay" width="500"/>
</p>

---

## Table of Contents

- [Preview](#preview)
- [About](#about)
- [Features](#features)
- [Getting Started](#getting-started)
- [How to Play](#how-to-play)
- [Technical Details](#technical-details)
- [Project Structure](#project-structure)
- [License](#license)

---

## Preview

| Start Screen | Gameplay Start |
|:---:|:---:|
| ![Start Screen](screenshots/01-start-screen.png) | ![Gameplay Start](screenshots/02-gameplay-start.png) |

| Active Gameplay | Game Over |
|:---:|:---:|
| ![Active Gameplay](screenshots/03-gameplay-active.png) | ![Game Over](screenshots/04-game-over.png) |

---

## About

**Abyssal Serpent** is an ocean/underwater re-skin of the classic Snake game. Dive into the deep ocean with:

- A dark water gradient background
- Ambient rising bubbles and drifting light rays
- A glowing bioluminescent sea-serpent as the snake
- A luminous pearl as food (instead of the traditional red dot)

All core gameplay mechanics (arrow-key controls, scoring, wall and self collision, start/game-over screens, restart) remain faithful to the original Snake experience.

---

## Features

- **Bioluminescent visuals** - gradient snake body with animated glowing eyes
- **Ocean atmosphere** - rising bubbles, light rays, and deep-water color palette
- **Smooth canvas rendering** on a 20x20 grid
- **Score tracking** with on-screen display
- **Start screen** with game instructions
- **Game over screen** with final score and one-click restart
- **Zero dependencies** - runs in any modern browser with no build step
- **Single file deployment** - the entire game lives in one HTML file

---

## Getting Started

1. **Clone the repository:**

   ```bash
   git clone https://github.com/harish1454/SnakeGame.git
   cd SnakeGame
   ```

2. **Open the game:**

   Simply open `index.html` in any modern web browser. No server, no build tools, no packages needed.

   ```bash
   # macOS
   open index.html

   # Linux
   xdg-open index.html

   # Windows
   start index.html
   ```

---

## How to Play

1. Open `index.html` in any modern web browser.
2. Click **Start Game** or press any arrow key to begin.
3. Use the **arrow keys** to control the serpent:

   | Key | Direction |
   |-----|-----------|
   | `Arrow Up` | Move up |
   | `Arrow Down` | Move down |
   | `Arrow Left` | Move left |
   | `Arrow Right` | Move right |

4. Eat the glowing pearl to grow longer and increase your score.
5. Avoid hitting the walls or your own tail.
6. When the game ends, press **Enter** or click **Play Again** to restart.

---

## Technical Details

| Property | Value |
|----------|-------|
| Language | HTML5, CSS, JavaScript |
| Canvas Size | 400 x 400 pixels |
| Grid | 20 x 20 cells (20px per cell) |
| Game Speed | ~8 frames per second |
| Dependencies | None |
| Collision | Wall boundaries + self-intersection |

---

## Project Structure

```
SnakeGame/
├── index.html          # The complete game (HTML + CSS + JS)
├── README.md           # This file
└── screenshots/        # Game screenshots
    ├── 01-start-screen.png
    ├── 02-gameplay-start.png
    ├── 03-gameplay-active.png
    └── 04-game-over.png
```

---

## License

This project is open source. Feel free to fork, modify, and share.

---

<p align="center">
  Made with 🌊 by <a href="https://github.com/harish1454">harish1454</a>
</p>
