# 🐍 Abyssal Serpent — Ocean Snake Game

A classic Snake game reimagined with a deep-ocean theme, built with HTML5 Canvas, CSS, and JavaScript. Fully self-contained in a single HTML file with no external dependencies.

## 🌊 Ocean Theme Overview

The game draws inspiration from real-world **oceanic zones** to create an immersive underwater atmosphere. The visual design layers multiple ocean types and depth zones into a cohesive experience.

### Ocean Zones Represented

| Zone | Depth Range | In-Game Representation |
|------|-------------|----------------------|
| **Epipelagic (Sunlight Zone)** | 0–200m | Light rays filtering from the top of the canvas; brighter teal tones at the top of the gradient |
| **Mesopelagic (Twilight Zone)** | 200–1,000m | Mid-canvas gradient transition; fading blue-greens where light dims |
| **Bathypelagic (Midnight Zone)** | 1,000–4,000m | Deep navy-black tones at the canvas bottom; near-total darkness |
| **Abyssal Zone** | 4,000–6,000m | The game's namesake — represented by the pitch-dark background (`#021019`) and bioluminescent creatures |

### Water Gradient Layers

The background uses a three-stop vertical gradient simulating descent through ocean depths:

- **Surface waters** (`#0e8a99`) — Warm teal representing the sunlit upper ocean where photosynthesis occurs
- **Mid-depth waters** (`#063a4d`) — Dark blue-green of the twilight zone where sunlight barely penetrates
- **Abyssal depths** (`#02121d`) — Near-black representing the lightless deep ocean floor

## 🫧 Ambient Ocean Effects

### Rising Bubbles
- 22 procedurally animated bubbles drift upward with sinusoidal wobble
- Simulates hydrothermal vent activity and dissolved gas release from the deep ocean
- Each bubble has randomized size, speed, drift amplitude, and phase

### Caustic Light Rays
- 3 diagonal beams sweep slowly across the canvas
- Mimics the dappled sunlight (caustics) seen in shallow coastal waters filtering down to deeper zones
- Creates the illusion of being just below a sunlit surface layer

## 🐉 The Serpent (Player Character)

The snake is themed as a **bioluminescent deep-sea serpent**, inspired by real abyssal creatures:

| Feature | Color | Inspiration |
|---------|-------|-------------|
| Body glow | `#1fbf8f` (sea-green) | Deep-sea jellyfish bioluminescence |
| Head radiance | `#7dffd8` (bright aqua) | Anglerfish lure glow |
| Outer aura | `#39e6c8` (teal glow) | Comb jelly iridescence |
| Core highlight | `rgba(230, 255, 245, 0.5)` | Bioluminescent plankton bloom |

The serpent features:
- **Gradient body** transitioning from bright head to darker tail (simulating energy dissipation)
- **Smooth quadratic curves** between segments (organic movement like a real sea creature)
- **Direction-tracking eyes** (dark pupils that follow the heading)
- **Dual-layer rendering** — outer glow + inner bright core for a convincing bioluminescent effect

## 💎 The Pearl (Collectible)

Food is rendered as a **glowing deep-sea pearl**, representing bioluminescent organisms:

- Radial gradient from white center to teal edge (mimics light emission from within)
- Pulsing glow effect (`shadowBlur` oscillates with sine wave) — like a living organism's rhythmic luminescence
- Color palette: pure white → ice-blue (`#eafcff`) → aqua glow (`#7fdfff`) → teal fade

## 🎮 How to Play

1. Open `index.html` in any modern web browser.
2. Click **Start Game** or press any arrow key to begin.
3. Use the **arrow keys** to control the serpent:
   - ⬆️ Up Arrow: Swim up
   - ⬇️ Down Arrow: Dive down
   - ⬅️ Left Arrow: Swim left
   - ➡️ Right Arrow: Swim right
4. Devour glowing pearls to grow longer and increase your score.
5. Avoid hitting the reef walls or your own tail.
6. When the game ends, press **Enter** or click **Play Again** to restart.

## ✨ Features

- 🎨 Deep-ocean gradient background with four oceanic zone representation
- 🫧 Procedurally animated rising bubbles (22 particles with physics)
- 🔦 Drifting caustic light rays simulating underwater sunlight
- 🐍 Bioluminescent serpent with smooth curved rendering and directional eyes
- 💎 Pulsing pearl collectible with radial glow
- 📊 Score tracking with on-screen display
- 🖥️ Start screen and game over screen with restart
- 🚫 No external dependencies — single HTML file

## 🔧 Technical Details

| Spec | Value |
|------|-------|
| Canvas size | 400×400 pixels |
| Grid | 20×20 cells (20px per cell) |
| Game speed | ~8 FPS (120ms tick) |
| Render loop | `requestAnimationFrame` (60 FPS visuals) |
| Bubble count | 22 particles |
| Light rays | 3 caustic beams |
| Collision | Wall boundaries + self-intersection |
| Architecture | Single IIFE, no globals |

## 📸 Screenshots

| Start Screen | Gameplay |
|:---:|:---:|
| ![Start](screenshots/01-start-screen.png) | ![Playing](screenshots/03-gameplay-active.png) |

| Early Game | Game Over |
|:---:|:---:|
| ![Early](screenshots/02-gameplay-start.png) | ![Over](screenshots/04-game-over.png) |

## 🌐 Ocean Type Reference

For those curious about the real-world ocean science behind the theme:

- **Pelagic zones** — The open water column, divided by depth and light availability. This game traverses from the sunlit epipelagic through to the abyssal zone.
- **Bioluminescence** — Over 75% of deep-sea organisms produce their own light. The serpent and pearl draw from this phenomenon.
- **Hydrothermal vents** — The rising bubbles reference volcanic activity on the ocean floor that sustains unique ecosystems.
- **Caustic patterns** — The light-ray effect mimics how surface waves focus and defocus sunlight into dancing patterns underwater.

---

*Built with vanilla HTML5, CSS, and JavaScript. No frameworks, no dependencies — just the deep ocean and your reflexes.* 🌊
