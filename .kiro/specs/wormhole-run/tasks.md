# Implementation Plan: Wormhole Run

- [x] 1. Scaffold `wormhole-run.html` with space-themed layout, HUD, canvas, and overlays (start, paused, game over) _Req 4, 5_
- [x] 2. Implement core snake state, input (arrows/WASD), reverse protection, and fixed-interval tick _Req 3_
- [x] 3. Implement `freeCells` helper and food spawning _Req 3.5_
- [x] 4. Implement wormhole pair placement, teleport on entry, +1 bonus, and timed relocation with warning flicker _Req 1_
- [x] 5. Implement asteroid spawning every 3rd food and collision _Req 2_
- [x] 6. Implement rendering: starfield, swirling wormholes (A cyan / B magenta), asteroids, food, snake _Req 5.2, 5.3_
- [x] 7. Implement pause (Space), game over, restart (Enter), best score persistence _Req 4_
- [x] 8. Add cross-links between `index.html` and `wormhole-run.html`; update README _Req 4.5_
- [x] 9. Verify game logic headlessly (Node with stubbed DOM/canvas): teleport, asteroid cadence, reversal block, pause, wormhole relocation, best score. Visual rendering not checked in a real browser (none available in sandbox).
