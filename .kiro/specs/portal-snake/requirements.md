# Requirements Document

## Introduction

Portal Snake is a new variant of the classic Snake game, shipped as a standalone file (`portal-snake.html`) alongside the existing Space Snake (`index.html`). It keeps the grid-and-growth core loop but adds teleporting portals, timed bonus food, and obstacles that appear as the player progresses. Like the original, it is pure HTML5 Canvas, CSS and JavaScript with no external dependencies.

## Requirements

### Requirement 1: Core snake movement and growth

**User Story:** As a player, I want to steer a snake around a grid and eat food to grow, so that I get the familiar Snake experience.

#### Acceptance Criteria
1. WHEN the game starts THEN the system SHALL place a 3-segment snake in the grid centre moving right.
2. WHEN the player presses an arrow key or WASD THEN the system SHALL change direction on the next tick.
3. IF the requested direction is the exact reverse of the current direction THEN the system SHALL ignore it.
4. WHEN the head enters the food cell THEN the system SHALL grow the snake by one segment, add to the score, and spawn new food on an empty cell.
5. WHEN the head leaves the grid, or enters a snake segment or an obstacle THEN the system SHALL end the game.

### Requirement 2: Portals

**User Story:** As a player, I want paired portals that teleport my snake, so that movement is less predictable and more strategic.

#### Acceptance Criteria
1. WHEN a round starts THEN the system SHALL place one pair of portals (blue and orange) on distinct empty cells.
2. WHEN the head enters a portal cell THEN the system SHALL move the head to the paired portal cell and continue in the same direction on the next tick.
3. IF the cell in front of the exit portal is outside the grid, an obstacle, or a snake segment THEN the game-over rule of Requirement 1.5 SHALL apply.
4. WHEN the score reaches each multiple of 100 THEN the system SHALL relocate the portal pair to new empty cells.
5. Portals SHALL never spawn on the snake, food, obstacles, or each other.

### Requirement 3: Bonus food

**User Story:** As a player, I want occasional timed bonus food, so that I can take risks for extra points.

#### Acceptance Criteria
1. WHEN regular food has been eaten and a random chance (about 25%) succeeds THEN the system SHALL spawn golden bonus food on an empty cell.
2. The bonus food SHALL be worth 50 points and growth of one segment, and SHALL expire after 5 seconds (visible countdown ring).
3. IF bonus food is already on the board THEN the system SHALL NOT spawn another.

### Requirement 4: Obstacles

**User Story:** As a player, I want obstacles to appear over time, so that the game gets harder as I progress.

#### Acceptance Criteria
1. WHEN regular food is eaten and the total eaten is a multiple of 5 THEN the system SHALL add one obstacle on an empty cell.
2. Obstacles SHALL NOT spawn within 3 cells of the snake head or in the head's current straight path of 3 cells, nor on food, portals or the snake.
3. The system SHALL cap obstacles at 15.

### Requirement 5: Scoring, speed and high score

**User Story:** As a player, I want my score and a persistent best score, so that I can compete with myself.

#### Acceptance Criteria
1. Regular food SHALL be worth 10 points.
2. WHEN the player eats 5 regular foods THEN the system SHALL increase speed by one level (tick interval reduced by 8 ms, minimum 60 ms, starting at 140 ms).
3. The HUD SHALL display current score, best score and speed level.
4. WHEN the game ends and the score exceeds the stored best THEN the system SHALL save it to `localStorage`; IF `localStorage` is unavailable THEN the game SHALL still work using an in-memory best.

### Requirement 6: Screens and controls

**User Story:** As a player, I want clear start, pause and game-over screens, so that the game is easy to run and restart.

#### Acceptance Criteria
1. WHEN the page loads THEN the system SHALL show a start screen with instructions and a Start button.
2. WHEN the player presses Space or P during play THEN the system SHALL toggle pause.
3. WHEN the game ends THEN the system SHALL show a game-over screen with final score, best score, and a Play Again button; Enter SHALL also restart.
4. Arrow keys and Space SHALL NOT scroll the page.

### Requirement 7: Visuals

**User Story:** As a player, I want the game to look distinct from Space Snake.

#### Acceptance Criteria
1. The game SHALL use a canvas of 400x400 px with a 20x20 grid.
2. Portals SHALL be rendered as animated swirling rings in distinct colours; obstacles as stone blocks; bonus food as a golden star with a countdown ring.
3. The game SHALL run with no external assets or libraries.
