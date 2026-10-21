<div align="center">

# 🐍 Snake Game

<img src="screenshots/03-gameplay-active.png" alt="Snake Game Screenshot" width="450" style="border-radius: 12px; box-shadow: 0 8px 24px rgba(0,0,0,0.2);">

<br><br>

[![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white)](https://developer.mozilla.org/en-US/docs/Web/HTML)
[![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white)](https://developer.mozilla.org/en-US/docs/Web/CSS)
[![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
[![No Dependencies](https://img.shields.io/badge/No%20Dependencies-4CAF50?style=for-the-badge&logo=none)](#)

<br><br>

**A classic Snake game built with HTML5 Canvas, CSS, and JavaScript.**

*Fully self-contained in a single HTML file with no external dependencies.*

<br>

[🎮 Play Now](#-how-to-play) · [✨ Features](#-features) · [🔧 Technical Details](#-technical-details) · [🌟 Quick Start](#-quick-start)

---

</div>

## 🎮 How to Play

<table>
  <tr>
    <td width="55%" valign="top">
      <h3>📋 Instructions</h3>
      <ol>
        <li>Open <code>index.html</code> in any modern web browser</li>
        <li>Click <kbd>Start Game</kbd> or press any arrow key to begin</li>
        <li>Use the <strong>arrow keys</strong> to control the snake</li>
        <li>🍎 Eat the red food to grow longer and score points</li>
        <li>💀 Avoid hitting the walls or your own tail</li>
        <li>Press <kbd>Enter</kbd> or click <kbd>Play Again</kbd> to restart</li>
      </ol>
    </td>
    <td width="45%" valign="top" align="center">
      <h3>🕹️ Controls</h3>
      <table align="center">
        <tr>
          <td align="center" colspan="3">
            <kbd>⬆️</kbd><br>
            <sub><b>Up</b></sub>
          </td>
        </tr>
        <tr>
          <td align="center">
            <kbd>⬅️</kbd><br>
            <sub><b>Left</b></sub>
          </td>
          <td align="center">
            <kbd>⬇️</kbd><br>
            <sub><b>Down</b></sub>
          </td>
          <td align="center">
            <kbd>➡️</kbd><br>
            <sub><b>Right</b></sub>
          </td>
        </tr>
      </table>
      <br>
      <p align="center">
        <sub>Use arrow keys to navigate the snake</sub>
      </p>
    </td>
  </tr>
</table>

---

## ✨ Features

<div align="center">

|         | Feature              | Description                                              |
|:------:|----------------------|----------------------------------------------------------|
| 🎨     | **Smooth Rendering** | Canvas-based rendering on a 20×20 grid                   |
| 📊     | **Score Tracking**   | Real-time on-screen score display                        |
| 🚀     | **Start Screen**     | Welcome screen with game instructions                    |
| 💀     | **Game Over Screen** | Final score display with restart option                  |
| 🌈     | **Visual Polish**    | Gradient snake body, glowing food, animated eyes         |
| ⚡     | **Zero Dependencies**| No external libraries — just pure vanilla code          |

</div>

---

## 🔧 Technical Details

<details>
<summary><b>📐 Click to expand technical specifications</b></summary>

<br>

<div align="center">

| Specification     | Value                        |
|-------------------|------------------------------|
| **Technology**    | Pure HTML5, CSS3, JavaScript |
| **Canvas Size**   | 400 × 400 pixels             |
| **Grid**          | 20 × 20 cells                |
| **Cell Size**     | 20px × 20px                  |
| **Game Speed**    | ~8 FPS (125ms interval)      |
| **Collision**     | Walls + Self-intersection    |

</div>

### 🏗️ Project Structure

```
SnakeGame/
├── index.html      # Complete game (single file!)
├── README.md       # You are here 📍
└── screenshots/    # Game screenshots
```

### 🔍 Implementation Highlights

- **Canvas API** — Efficient rendering with HTML5 Canvas
- **Game Loop** — Smooth animation via requestAnimationFrame
- **State Management** — Clean separation of game states
- **Event Handling** — Responsive keyboard controls

</details>

---

<div align="center">

## 🌟 Quick Start

```bash
# Clone the repository
git clone https://github.com/harish1454/SnakeGame.git

# Navigate to the directory
cd SnakeGame

# Open the game in your browser
# macOS
open index.html

# Linux
xdg-open index.html

# Windows
start index.html

# Or simply double-click index.html in your file browser!
```

---

<p>
  <img src="https://img.shields.io/badge/Built%20with-❤️-red?style=for-the-badge" alt="Built with love">
  <img src="https://img.shields.io/badge/Using-Vanilla%20JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black" alt="Vanilla JS">
</p>

<br>

<sub>⭐ Star this repo if you found it useful!</sub>

<br><br>

**[⬆️ Back to Top](#-snake-game)**

</div>
