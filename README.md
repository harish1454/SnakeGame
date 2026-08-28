# 🐍 Space Snake

A cosmic-themed Snake game built with HTML5 Canvas, CSS, and JavaScript. Fully self-contained in a single HTML file with no external dependencies. Navigate your spaceship through the cosmos, collect energy, and grow your trail!

## How to Play

1. Open `index.html` in any modern web browser.
2. Click **Launch Ship** or press any arrow key to begin.
3. Use the **arrow keys** to control the snake's direction:
   - ⬆️ Up Arrow: Move up
   - ⬇️ Down Arrow: Move down
   - ⬅️ Left Arrow: Move left
   - ➡️ Right Arrow: Move right
4. Collect cosmic energy (food) to grow longer and increase your score.
5. Avoid hitting the asteroid walls or your own energy trail.
6. When the game ends, press **Enter** or click **Relaunch** to restart.

## 🚀 Snake Types

The snake in Space Snake has a unique cosmic visual design composed of two distinct parts:

### Spaceship Head

The snake's head is rendered as a **spaceship** with the following details:

| Feature | Description |
|---------|-------------|
| Shape | Arrow/triangle pointing in the direction of travel |
| Color | Gradient from dark blue (#1a237e) to cyan (#00e5ff) |
| Cockpit | Small white semi-transparent windshield dome |
| Engine | Orange glow at the rear of the ship |
| Thruster | Emits orange/red fire particles behind the ship as it moves |
| Glow | Cyan (#00e5ff) outer glow/shadow effect |

### Comet Energy Trail (Body)

The snake's body segments form a **comet-like energy trail** that fades from head to tail:

| Feature | Description |
|---------|-------------|
| Shape | Radial gradient orbs for each segment |
| Color (near head) | Bright cyan-blue (rgb 30, 200, 255) |
| Color (near tail) | Deep purple (rgb 130, 50, 255) |
| Size | Segments shrink gradually toward the tail (30% reduction) |
| Opacity | Fades from full brightness to 30% toward the tail |
| Core | Each segment has a bright white inner core |
| Glow | Purple outer glow that fades toward the tail |

### Visual Progression

```
[🚀 Spaceship Head] → [Bright Cyan Orb] → [Blue Orb] → [Purple Orb] → [Dim Purple Orb]
         ↑                    ↑                                              ↑
    Arrow shape         Near head (bright)                           Near tail (faded)
    with cockpit        Full size                                    Smaller & dimmer
```

## 🌟 Collectible Types

Food items spawn as one of three random cosmic objects:

| Type | Visual | Color | Effect |
|------|--------|-------|--------|
| 🪐 Planet | Sphere with orbital ring | Orange/red with golden ring | +1 Energy |
| ⭐ Star | 5-pointed star shape | Golden yellow with glow | +1 Energy |
| 🔮 Energy Orb | Pulsating orb | Cyan-to-purple gradient with white core | +1 Energy |

All collectibles have a pulsating animation and glow effect. Upon collection, they burst into colorful particles (cyan, purple, pink, orange, green).

## Features

- 🌌 Animated starfield background with twinkling stars and nebula
- 🚀 Spaceship-style snake head with directional rotation
- ✨ Comet energy trail body with gradient color fade
- 🔥 Thruster particle effects behind the ship
- 💥 Particle burst explosions on food collection
- 📊 Score tracking ("Energy" counter) with on-screen display
- 🎬 Start screen with cosmic theme and instructions
- 💀 Game over screen with final score and restart option
- 🎨 Neon glow aesthetic with cyan, purple, and orange accents
- 📦 No external dependencies required

## Technical Details

- Pure HTML5, CSS, and JavaScript
- Canvas size: 400×400 pixels
- Grid: 20×20 cells (20px per cell)
- Game speed: ~8 frames per second (120ms interval)
- Collision detection for walls and self-intersection
- Independent animation loop for starfield and particles
- Space-themed UI with glowing buttons and text shadows
