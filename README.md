<div align="center">

# 🐍 Snake Game

<img src="screenshots/03-gameplay-active.png" alt="Snake Game Screenshot" width="450" style="border-radius: 10px; box-shadow: 0 4px 8px rgba(0,0,0,0.3);">

<br><br>

[![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white)](https://developer.mozilla.org/en-US/docs/Web/HTML)
[![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white)](https://developer.mozilla.org/en-US/docs/Web/CSS)
[![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)](https://developer.mozilla.org/en-US/docs/Web/JavaScript)

**A classic Snake game built with HTML5 Canvas, CSS, and JavaScript.**<br>
*Fully self-contained in a single HTML file with no external dependencies.*

<br>

[🎮 Play Now](#how-to-play) · [✨ Features](#features) · [🔧 Technical Details](#technical-details)

---

</div>

## 🎮 How to Play

<table>
  <tr>
    <td width="60%" valign="top">
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
    <td width="40%" valign="top">
      <h3>🕹️ Controls</h3>
      <table align="center">
        <tr>
          <td align="center" colspan="3"><kbd>⬆️ ↑</kbd><br><sub>Move Up</sub></td>
        </tr>
        <tr>
          <td align="center"><kbd>⬅️ ←</kbd><br><sub>Left</sub></td>
          <td width="40"></td>
          <td align="center"><kbd>➡️ →</kbd><br><sub>Right</sub></td>
        </tr>
        <tr>
          <td align="center" colspan="3"><kbd>⬇️ ↓</kbd><br><sub>Move Down</sub></td>
        </tr>
      </table>
    </td>
  </tr>
</table>

---

## ✨ Features

<div align="center">

| | Feature | Description |
|:--:|---------|-------------|
| 🎨 | **Smooth Rendering** | Canvas-based rendering on a 20×20 grid |
| 📊 | **Score Tracking** | Real-time on-screen score display |
| 🚀 | **Start Screen** | Welcome screen with game instructions |
| 💀 | **Game Over Screen** | Final score display with restart option |
| 🌈 | **Visual Polish** | Gradient snake body, glowing food, animated eyes |
| ⚡ | **Zero Dependencies** | No external libraries — just pure vanilla code |

</div>

---

## 🔧 Technical Details

<details>
<summary><b>📐 Click to expand technical specifications</b></summary>

<br>

<div align="center">

| Specification | Value |
|--------------|-------|
| **Technology** | Pure HTML5, CSS3, JavaScript |
| **Canvas Size** | 400 × 400 pixels |
| **Grid** | 20 × 20 cells |
| **Cell Size** | 20px × 20px |
| **Game Speed** | ~8 FPS (125ms interval) |
| **Collision** | Walls + Self-intersection |

</div>

### 🏗️ Project Structure

```
SnakeGame/
├── index.html      # Complete game (single file!)
├── README.md       # You are here 📍
└── screenshots/    # Game screenshots
```

</details>

---

<div align="center">

### 🌟 Quick Start

```bash
# Clone the repository
git clone https://github.com/harish1454/SnakeGame.git

# Open the game
open SnakeGame/index.html
# or just double-click index.html in your file browser!
```

---

<sub>Built with ❤️ using vanilla JavaScript</sub>

<br>

**[⬆ Back to Top](#-snake-game)**

</div>
