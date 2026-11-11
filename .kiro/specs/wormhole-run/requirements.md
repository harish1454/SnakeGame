# Requirements: Wormhole Run

## Introduction

Wormhole Run is a new variant of the Space Snake game. Instead of a static
arena, the board is dynamic: paired wormholes teleport the ship across the
map, and asteroids accumulate as the player collects energy, steadily
shrinking safe space. It ships as a separate page (`wormhole-run.html`) that
reuses the visual style of the existing game, and is linked from the original
`index.html`.

## Requirements

### Requirement 1: Wormhole teleportation

**User Story:** As a player, I want to travel through wormholes so that I can
take shortcuts across the board and plan routes creatively.

#### Acceptance Criteria
1. WHEN a game starts THEN the system SHALL place one pair of linked wormholes (A and B) on empty cells.
2. WHEN the snake head enters a wormhole cell THEN the head SHALL emerge at the linked wormhole cell and continue in the same direction.
3. WHEN the exit cell's next step is blocked it SHALL be treated like any other move (wall/asteroid/self collision ends the game).
4. WHEN the snake passes through a wormhole THEN the system SHALL award a small bonus (+1 energy).
5. WHEN 10 seconds elapse (or the snake passes through) THEN the wormhole pair SHALL relocate to new empty cells, never on the snake, food, or asteroids.
6. IF a wormhole pair is about to relocate THEN the system SHALL show a visual warning (flicker) for the last 2 seconds.

### Requirement 2: Asteroid hazards

**User Story:** As a player, I want the arena to get more dangerous as I grow so
that the game stays challenging.

#### Acceptance Criteria
1. WHEN the player collects every 3rd energy item THEN the system SHALL spawn one asteroid on an empty cell.
2. WHEN the snake head enters an asteroid cell THEN the game SHALL end.
3. Asteroids SHALL NOT spawn on the snake, food, wormholes, or within 2 cells of the snake head.
4. WHEN the board has no free cell for a spawn THEN the system SHALL skip the spawn without error.

### Requirement 3: Core snake gameplay

**User Story:** As a player, I want familiar snake controls and rules.

#### Acceptance Criteria
1. The snake SHALL move on a 20x20 grid and be controlled with arrow keys or WASD.
2. The snake SHALL NOT be able to reverse directly into itself.
3. WHEN the snake eats food THEN it SHALL grow by one segment and the score SHALL increase by 1.
4. WHEN the snake hits a wall or itself THEN the game SHALL end.
5. Food SHALL never spawn on the snake, wormholes, or asteroids.

### Requirement 4: Game flow and UI

**User Story:** As a player, I want clear screens and feedback.

#### Acceptance Criteria
1. The game SHALL show a start screen, a HUD (energy score and best score), and a game over screen.
2. WHEN the game ends THEN the system SHALL show the final score and allow restart via Enter or a button.
3. The best score SHALL persist across sessions using `localStorage` (failing gracefully if unavailable).
4. The player SHALL be able to pause and resume with the Space key.
5. The page SHALL include a link back to the classic game, and `index.html` SHALL link to Wormhole Run.

### Requirement 5: Technical constraints

#### Acceptance Criteria
1. The game SHALL be a single self-contained HTML file with no external dependencies.
2. Visuals SHALL match the existing space theme (dark background, cyan/purple glow).
3. Wormholes SHALL be rendered distinctly (A = cyan, B = magenta) with a swirling animation.
