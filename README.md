# 🐍 Abyssal Serpent - Ocean Snake Game

> A classic Snake game reimagined with a deep-ocean bioluminescent theme. Built with HTML5 Canvas, CSS, and JavaScript -- fully self-contained in a single HTML file with zero dependencies.

![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)

---

## 📑 Table of Contents

- [Screenshots](#-screenshots)
- [Features](#-features)
- [Quick Start](#-quick-start)
- [How to Play](#-how-to-play)
- [Technical Details](#-technical-details)
- [Project Structure](#-project-structure)
- [Contributing](#-contributing)
- [License](#-license)

---

## 📸 Screenshots

| Start Screen | Gameplay Start |
|:---:|:---:|
| ![Start Screen](screenshots/01-start-screen.png) | ![Gameplay Start](screenshots/02-gameplay-start.png) |

| Active Gameplay | Game Over |
|:---:|:---:|
| ![Active Gameplay](screenshots/03-gameplay-active.png) | ![Game Over](screenshots/04-game-over.png) |

---

## ✨ Features

- 🌊 **Ocean Theme** -- Deep-ocean water gradient background with ambient rising bubbles and drifting light rays
- 🐉 **Bioluminescent Serpent** -- Glowing sea-serpent snake with animated eyes and gradient body
- 🔮 **Glowing Pearl** -- Luminous pearl replaces the classic food item
- 🎮 **Smooth Gameplay** -- Canvas-based rendering on a 20x20 grid at ~8 FPS
- 📊 **Score Tracking** -- On-screen score display with final score on game over
- 🖥️ **Start & Game Over Screens** -- Instructional start screen and restart prompt
- 📦 **Zero Dependencies** -- Everything in a single HTML file, no build tools or packages needed

---

## 🚀 Quick Start

1. **Clone the repository**
   ```bash
   git clone https://github.com/harish1454/SnakeGame.git
   cd SnakeGame
   ```

2. **Open the game**
   ```bash
   open index.html
   ```
   Or simply double-click `index.html` in your file explorer. Any modern web browser will work.

3. **Play!** Click **Start Game** or press any arrow key to begin.

---

## 🎮 How to Play

| Key | Action |
|-----|--------|
| ⬆️ Up Arrow | Move up |
| ⬇️ Down Arrow | Move down |
| ⬅️ Left Arrow | Move left |
| ➡️ Right Arrow | Move right |

**Objective:** Guide the abyssal serpent to eat the glowing pearl. Each pearl consumed makes the serpent grow longer and increases your score.

**Game Over Conditions:**
- Hitting a wall
- Colliding with your own tail

**Restart:** Press **Enter** or click **Play Again** after game over.

---

## 🔧 Technical Details

| Property | Value |
|----------|-------|
| Canvas Size | 400 x 400 pixels |
| Grid | 20 x 20 cells |
| Cell Size | 20px |
| Game Speed | ~8 frames per second |
| Technologies | HTML5, CSS3, Vanilla JavaScript |
| Dependencies | None |

**Architecture:**
- Single `index.html` file containing all markup, styles, and game logic
- HTML5 Canvas API for rendering
- `requestAnimationFrame`-based game loop with frame-rate throttling
- Collision detection for walls and self-intersection

---

## 📁 Project Structure

```
SnakeGame/
├── index.html          # Complete game (HTML + CSS + JS)
├── README.md           # This file
└── screenshots/        # Game screenshots
    ├── 01-start-screen.png
    ├── 02-gameplay-start.png
    ├── 03-gameplay-active.png
    └── 04-game-over.png
```

---

## 🤝 Contributing

Contributions are welcome! Here are some ideas:

- Add mobile/touch controls
- Implement difficulty levels (speed increase)
- Add a high-score leaderboard (localStorage)
- Create alternative themes
- Add sound effects

To contribute:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'feat: add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

---

## 📄 License

This project is open source and available for personal and educational use.

---

<p align="center">
  Made with 🌊 and JavaScript
</p>
