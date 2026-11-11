# Implementation Plan

- [x] 1. Scaffold `portal-snake.html` with page layout, styles, canvas, HUD and start/pause/game-over overlays
  - _Requirements: 6.1, 6.3, 7.1, 7.3_

- [x] 2. Implement core game state and tick loop
  - [x] 2.1 Snake init, buffered input with reverse-direction guard, wall/self collision
    - _Requirements: 1.1, 1.2, 1.3, 1.5_
  - [x] 2.2 Regular food spawn via `emptyCell()` and growth/scoring
    - _Requirements: 1.4, 5.1_
  - [x] 2.3 State machine (start, playing, paused, gameover) and keyboard handling without page scroll
    - _Requirements: 6.2, 6.3, 6.4_

- [x] 3. Implement portals
  - [x] 3.1 Place pair on empty cells; teleport head to paired portal
    - _Requirements: 2.1, 2.2, 2.5_
  - [x] 3.2 Collision check after exit; relocate on every 100 points
    - _Requirements: 2.3, 2.4_

- [x] 4. Implement obstacles with safe-spawn rules and cap
  - _Requirements: 4.1, 4.2, 4.3_

- [x] 5. Implement timed bonus food with countdown and pause-aware expiry
  - _Requirements: 3.1, 3.2, 3.3_

- [x] 6. Implement speed levels and best-score persistence with `localStorage` fallback
  - _Requirements: 5.2, 5.3, 5.4_

- [x] 7. Implement rendering: grid, swirling portals, stone obstacles, golden star bonus with ring, snake
  - _Requirements: 7.1, 7.2_

- [x] 8. Verify
  - [x] 8.1 Headless smoke run of the script logic under Node with a stubbed DOM (init, reverse guard, portal teleport, obstacle cap, 200 random games + greedy bot, no exceptions)
  - [ ] 8.2 Manual in-browser visual check of start screen, portals, bonus ring and game-over (not yet done; no browser available in the build sandbox)

- [x] 9. Update README with Portal Snake section
