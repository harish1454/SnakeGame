# Implementation Plan — Australian Outback Snake Re-theme

Source of truth: `01-requirements.md` and `02-design.md` in `.agents/pipeline/2026-09-16_23-59-25/`.
All edits happen in this worktree: `/projects/sandbox/SnakeGame/.worktrees/australian-snake-theme/`.
Only two runtime-relevant files change: `index.html` (the whole game) and `README.md`.

Scope discipline: this is a **re-skin only**. Do NOT touch game logic — `initGame`, `spawnFood`, `update`, `gameOver`, `startGame`, `restartGame`, `selectDifficulty`, `showDifficultyScreen`, the keydown handler, button `addEventListener` wiring, `DIFFICULTY_SETTINGS`, all element IDs, and CSS class names used by JS (`hidden`, `difficulty-label easy/medium/hard`) are INVARIANT. Only visible text, colours, and canvas-drawing internals change.

Edit by **selector / element identity**, NOT by line number — line numbers below are indicative and will drift as edits land. There is no build step: "verify" means open `index.html` in a browser (`file://`) and observe, plus grep assertions where noted. All items must leave `index.html` a valid, openable page.

Locked outback palette (from design):
- Primary accent (cyan → ochre): `#00e5ff` → `#E2711D`; `rgba(0,229,255,·)` → `rgba(226,113,29,·)`
- Body background: `#0a0a1a` → `#2B1A0E`
- Canvas border indigo `#1a237e` → bark brown `#6B3F1D`
- Score / final-score only (`#ffab40`/`rgba(255,171,64,·)`) → golden wattle `#E8B84B`/`rgba(232,184,75,·)`
- Overlay subtitle + instructions text `#5c6bc0` → muted tan `#9C7A4E`
- Secondary button border `#5c6bc0`/hover `#7986cb`/glow `rgba(92,107,192,·)` → `#6B4A2A`/`#8B5E34`/`rgba(107,74,42,·)`
- Overlay paragraph / btn-secondary text `#b0bec5` → warm sand `#D8C6A8`
- Body text / btn-secondary hover text `#e0e0e0` → warm cream `#EDE3D0`
- Overlay bg radial `rgba(10,10,40,·)`/`rgba(5,5,20,·)` → `rgba(43,26,14,·)`/`rgba(25,14,7,·)`

MUST NOT change (semantic difficulty colours, shared with score's old `#ffab40`):
- Difficulty green `#69f0ae`, amber `#ffab40`, red `#ff5252` in `.difficulty-label.*`, `.difficulty-btn.*` (incl. `:hover`), and `DIFFICULTY_SETTINGS`. Only edit the `#ffab40` occurrences in `.score-display` and `.overlay .final-score`.

---

- [ ] 1. Apply the outback CSS palette per-selector in the `<style>` block of `index.html`.
      Edit each named selector to the mapped value above: `body` background `#0a0a1a`→`#2B1A0E` and body text `#e0e0e0`→`#EDE3D0`; `.title` / `.overlay h2` / all `.btn` & `.btn:hover` color+border+box-shadow+text-shadow / `canvas` glow box-shadow (cyan family → ochre `#E2711D`/`rgba(226,113,29,·)`); `canvas` border `#1a237e`→`#6B3F1D`; `.score-display` and `.overlay .final-score` color+text-shadow `#ffab40`/`rgba(255,171,64,·)`→`#E8B84B`/`rgba(232,184,75,·)` (these two selectors ONLY — leave difficulty `#ffab40` alone); `.overlay-subtitle` and `.instructions` `#5c6bc0`→`#9C7A4E`; `.btn-secondary` border `#5c6bc0`→`#6B4A2A`, its `:hover` border `#7986cb`→`#8B5E34` and glow `rgba(92,107,192,·)`→`rgba(107,74,42,·)`; `.overlay p` & `.btn-secondary` text `#b0bec5`→`#D8C6A8`; `.btn-secondary:hover` text `#e0e0e0`→`#EDE3D0`; `.overlay` radial `rgba(10,10,40,·)`/`rgba(5,5,20,·)`→`rgba(43,26,14,·)`/`rgba(25,14,7,·)`.
      Do NOT edit `.difficulty-label.*` or `.difficulty-btn.*` colours.
      Files: index.html
      Verify: open index.html in a browser — difficulty screen, header, buttons, and overlays render in warm ochre/brown/sand tones with no blue accents; the three difficulty buttons still show green/amber/red. No console errors.

- [ ] 2. Update all static theme text in the HTML body and `<title>`.
      `<title>` → `Aussie Outback Snake`; `.title` div `SPACE SNAKE`→`OUTBACK SNAKE`; difficulty `<h2>` `COSMIC SERPENT`→`OUTBACK SERPENT`; `.overlay-subtitle` `Select Mission Difficulty`→`Pick Your Track`; score label `Energy:`→`Tucker:`; game-over `<h2>` `SHIP DESTROYED`→`GONE WALKABOUT` (exactly 14 chars — do NOT use a longer heading); `.final-score` prefix `Energy Collected:`→`Tucker Bagged:`; restart button `Relaunch`→`Have Another Go`; change-difficulty button `Change Difficulty`→`Change Track`; game-over instructions `Press Enter to relaunch or Esc to change difficulty`→`Press Enter to have another go or Esc to change track`.
      Do NOT change any element IDs or the `id="score"`/`id="finalScore"` spans.
      Files: index.html
      Verify: open index.html — header reads "OUTBACK SNAKE", difficulty screen "OUTBACK SERPENT" / "Pick Your Track", score reads "Tucker:", game-over shows "GONE WALKABOUT" on one line within the 400px overlay. `grep -niE 'space snake|cosmic serpent|ship destroyed|energy:|energy collected|relaunch' index.html` returns zero hits.

- [ ] 3. Re-skin the collectibles in `drawFood()` as three bush-tucker items (keep the 3-type structure, `foodPulse` animation, glow, and `CELL_SIZE/2 - 2` bounding size).
      Type 0 (was ringed planet) → witchetty grub: cream/tan segmented crescent (`#EAD9B0` body, faint segment arcs, small darker head), `shadowColor` → `#C9A24B`. Type 1 (was 5-point star) → gum nut: woody brown capsule `#7A4B25` with a small star-shaped opening on top (reuse the existing star-path math for the cap), subtle highlight, warm `shadowColor`. Type 2 (was energy orb, incl. its `rgba(124,77,255,·)`/`rgba(0,229,255,·)` gradient) → quandong: glossy round red fruit (`#C0392B` radial gradient, white specular highlight, small stem/leaf), warm `shadowColor`. Also update the stale comment `// Draw food as space objects` and the state comment `let foodType = 0; // 0 = planet, 1 = star, 2 = energy orb` to bush-tucker wording.
      Files: index.html
      Verify: open index.html, start Easy, and confirm collectibles render as grub/gum-nut/red-fruit shapes (not planet/star/orb) with pulse+glow; eating one grows the snake and increments the displayed score.

- [ ] 4. Re-skin the snake as a coastal carpet python in `drawSnakeHead()` and `drawSnakeBody()`, visuals only (no changes to `snake` array, segment count, positions, or the head direction/rotate logic).
      `drawSnakeHead`: keep translate+rotate-by-direction; replace spaceship triangle/cockpit/engine with an elongated rounded head, olive→tan gradient (`#6E7B3D`→`#B7A55A`), two dark diamond scale marks, two white eyes with dark pupils, short forked red tongue; remove the ship outline stroke; glow `#00e5ff`→`#E2711D`; keep/ensure `ctx.shadowBlur = 0` reset after the glow fill before drawing eyes/tongue. `drawSnakeBody`: keep head→tail iteration and per-segment fade; replace cyan→purple comet gradient with alternating olive/tan circles (`#5E6B34`/`#8E7C42`) plus a darker diamond overlay per segment and a subtle ochre glow toward the tail; recolour the body-glow `shadowColor` `rgba(100,50,255,·)`→warm ochre. Keep the existing per-segment `ctx.save()`/`ctx.restore()` (that, not a `globalAlpha` reset, prevents state leaking into `drawSnakeHead`); if the fade is switched to `ctx.globalAlpha`, set AND reset it strictly inside that per-segment save/restore.
      Files: index.html
      Verify: open index.html and play — snake head is a python head that visibly points in the travel direction; body shows olive/tan patterned segments with a warm glow that fades toward the tail; long snake renders without alpha/shadow bleeding into the head.

- [ ] 5. Rewrite the `drawStarfield()` body into an outback scene, reusing the existing `stars[]` array and `starfieldOffset` drift as dust motes (keep the function name and call sites; NFR2 performance parity). **Covers the outstanding design-review finding — recolour all three currently-blue values, not just the glow line.**
      Replace base fill `#0a0a1a` (~line 573) → outback dust tone; recolour primary dust-mote fill `rgba(200,220,255,·)` (~line 596) → warm gold `rgba(220,190,120,·)`; recolour the star-glow `rgba(150,180,255,·)` (~line 603) → warm gold glow. Replace the nebula radial block with: warm sky gradient (dusty blue `#8FB3C9` → ochre `#E2A45C` → red horizon `#C0562B`), a low sun/heat radial disc, one or two red-dirt hill/mesa silhouettes (`#8E4A22`/`#A85A2C`) on a horizon baseline, a ground band (`#B5642E`→`#7A3F1C`), and a few eucalyptus/spinifex silhouettes via a new `drawGumTree(x, groundY, h, w)` helper declared inside the IIFE near the other draw helpers. Recolour the grid overlay to `rgba(90,60,30,0.12)`.
      Files: index.html
      Verify: open index.html — the background (on difficulty screen, during play, and on game-over) shows a warm outback sky/ground with drifting gold dust motes and tree silhouettes; no blue/navy background or blue-white motes remain. Animation stays smooth on all three difficulties (no new jank).

- [ ] 6. Recolour the particles to outback tones.
      In `createFoodParticles` (~line 461) replace `var colors = ['#00e5ff', '#7c4dff', '#ff4081', '#ffab40', '#69f0ae']` with `['#E2711D', '#E8B84B', '#C0392B', '#8E7C42', '#6E7B3D']` (structure/counts unchanged). In `drawParticles`, edit only the `tpColor` ternary: `tp.life > 0.5 ? 'rgba(180, 120, 70, ' + tp.life + ')' : 'rgba(140, 90, 50, ' + tp.life + ')'` (keep the two-tier `life > 0.5` structure and the surrounding save/globalAlpha/restore). Do NOT edit `createThrusterParticle` or `updateParticles`.
      Files: index.html
      Verify: open index.html, eat a collectible — the burst uses warm ochre/gold/red tones; the trail behind the snake reads as kicked-up red dust, not blue/orange thruster fire.

- [ ] 7. Full residual-blue/cyan sweep and DOM-integrity check on `index.html`.
      Run `grep -niE '#00e5ff|rgba\(0, ?229, ?255|#0a0a1a|#1a237e|rgba\(200, ?220, ?255|rgba\(150, ?180, ?255|#7c4dff|rgba\(124, ?77, ?255|rgba\(100, ?50, ?255|#5c6bc0|#7986cb' index.html` — expect ZERO hits (all space-blue/cyan/navy/purple values recoloured). Also `grep -niE 'space snake|cosmic serpent|ship destroyed|energy:|energy collected|relaunch|space objects|energy orb' index.html` → zero hits (includes the two rewritten comments). Confirm the difficulty `#ffab40`/`#69f0ae`/`#ff5252` colours are still present (they must NOT be removed).
      Files: index.html (no edits unless a stray value is found — then fix it and re-run)
      Verify: both greps return zero hits; the difficulty-colour grep still finds the three semantic colours; open index.html once more and confirm no console errors and the full flow works (see item 9).

- [ ] 8. Rewrite `README.md` to describe the Australian outback theme and correct all stale text.
      Remove/replace stale generic + space references ("Click Start Game", "Eat the red food", "click Play Again", any space wording). Describe: opening index.html directly (self-contained, no build), the outback/bush theme, carpet-python snake, bush-tucker collectibles, "Tucker" score, difficulty select via buttons or keys 1/2/3, arrow-key movement, Enter to have another go, Esc to change track.
      Files: README.md
      Verify: read README.md — it accurately matches the shipped game with no contradicting stale references (AC13).

- [ ] 9. Full manual acceptance pass and commit on the feature branch.
      Open index.html (`file://`) and verify against acceptance criteria: (AC1) no console errors, no network requests; (AC2) titles reference Australia; (AC3) python snake with patterned body + directional head; (AC4) bush-tucker collectibles, eating grows + scores; (AC5) outback palette, no space-blue; (AC6) game-over shows Aussie slang + final score; (AC7) arrows change direction, no direct reversal; (AC8) wall and self collision end the game; (AC9) all three difficulties selectable by button and 1/2/3 with distinct speed/multiplier; (AC10) Enter restarts, Esc returns to difficulty select, buttons match; (AC11) after restart, snake/score/collectible/particles reset; (AC12) raw score × multiplier applied; (AC15) still a single self-contained index.html. Screenshots (AC16/FR17) are best-effort — if no headless capture tooling is available, note the gap in the implementation notes rather than blocking.
      Then commit on `feature/australian-snake-theme` (never on main): `git add index.html README.md && git commit -m "feat: re-theme Snake to Australian outback/bush aesthetic"`.
      Files: (commit only)
      Verify: `git -C /projects/sandbox/SnakeGame/.worktrees/australian-snake-theme branch --show-current` = `feature/australian-snake-theme`; `git log main..HEAD --oneline` shows the theme commit; `git log main --oneline` shows none (AC14).

## Notes / assumptions
- Screenshots (`screenshots/`, FR17/A6) are best-effort and non-blocking per the design; skip and note if no headless browser tooling is present.
- Themed hazards/bonuses are explicitly out of scope this pass (A4).
- If "OUTBACK SNAKE" (13 ch) collides with `.header-right` in the 400px header, apply the design's scoped fallback: reduce `.title` font-size 28px→26px (or letter-spacing 2px→1px). Only touch `.title`.

---

## Verification note (implementation run)

Environment: no browser-automation tooling (no `node`/`npx`/`chromium`) and no `PIL` available in the workspace, so verification was done by **static code / DOM / canvas inspection** plus targeted greps (per the design's fallback for "no headless tooling"). Screenshots therefore NOT regenerated (best-effort per A6/FR17) — noted as a gap.

Files changed: `index.html`, `README.md` (plus this note in `plan.md`).

Checks performed and results:
- **Palette (per-selector, no global replace):** edited by selector identity. Difficulty semantics preserved — `#69f0ae` / `#ffab40` / `#ff5252` each still present (5 occurrences apiece) in `.difficulty-label.*`, `.difficulty-btn.*`, and `DIFFICULTY_SETTINGS`. Score/final-score `#ffab40` occurrences (only) recoloured to golden wattle `#E8B84B`.
- **Residual blue/cyan/navy/purple sweep** (`#00e5ff`, `rgba(0,229,255,·)`, `#0a0a1a`, `#1a237e`, `rgba(200,220,255,·)`, `rgba(150,180,255,·)`, `#7c4dff`, `rgba(124,77,255,·)`, `rgba(100,50,255,·)`, `#5c6bc0`, `#7986cb`, `rgba(92,107,192,·)`, `#00bcd4`, old orange thruster/engine `rgba(255,150/50/100,0)`, planet/star hexes): **ZERO hits**. All three starfield blues recoloured — base fill `#0a0a1a`→outback sky gradient, dust-mote fill `rgba(200,220,255)`→`rgba(220,190,120)`, glow `rgba(150,180,255)`→warm gold, plus grid `rgba(30,40,80)`→`rgba(90,60,30,0.12)`.
- **Residual space-text sweep** (user-facing): `SPACE SNAKE`, `COSMIC SERPENT`, `SHIP DESTROYED`, `Energy:`, `Energy Collected`, `Relaunch`/`relaunch`, `Draw food as space objects`, `energy orb`: **ZERO hits**. (Remaining matches for `starfield`/`thruster` are internal function/variable identifiers only, which the design says to keep.)
- **Static text updated:** title `Aussie Outback Snake`; header `OUTBACK SNAKE`; difficulty `OUTBACK SERPENT` / `Pick Your Track`; score `Tucker:`; game-over `GONE WALKABOUT` (verified length = **14 chars**, same as original `SHIP DESTROYED`, so fits the 400px overlay at 32px/ls3 without a new wrap); `Tucker Bagged:`; `Have Another Go`; `Change Track`; instructions updated.
- **Header fit:** applied the design's scoped fallback proactively (`.title` font-size 28px→26px, `.title` only) because in-browser measurement was unavailable and 13-char title + difficulty pill + score was estimated to marginally overflow 400px at 28px.
- **Snake re-skin:** `drawSnakeHead` → carpet-python head (olive→tan gradient, dark diamond scale marks, white eyes + dark pupils, forked red tongue, ochre glow, ship stroke removed, `shadowBlur=0` reset before eyes/tongue). `drawSnakeBody` → alternating olive/tan banded segments with a dark diamond overlay and warm-ochre glow fading to tail; per-segment `save`/`restore` retained (fade baked into rgba strings, no `globalAlpha` leak); `snake` array / positions / head rotate logic untouched.
- **Collectibles:** `drawFood` type 0 = witchetty grub (cream/tan segmented crescent), type 1 = gum nut (woody capsule with reused star-path opening), type 2 = quandong (glossy red fruit + specular + stem/leaf). 3-type structure, `foodPulse`, glow, and `CELL_SIZE/2-2` bounding preserved.
- **Particles:** food-burst colour array swapped to outback tones; thruster `tpColor` ternary recoloured to red dust (two-tier `life>0.5` structure kept). `createThrusterParticle`/`updateParticles` untouched.
- **DOM/JS integrity:** balanced braces/parens/brackets (103/103, 415/415, 65/65); all 11 `getElementById` IDs still exist in the DOM (none missing); all logic functions present; `draw()` order preserved (starfield→particles→food→body→head).
- **Mechanics preserved (by construction):** `git diff` touches NO core-logic lines — no changes to `update`, `gameOver`, `selectDifficulty`, `startGame`, `restartGame`, `spawnFood`, `initGame`, the keydown handler, button `addEventListener` wiring, `DIFFICULTY_SETTINGS`, `nextDirection`, `snake.unshift/pop`, or the game loop. Therefore wall/self collision, growth-on-eat, scoring×multiplier, 3 difficulties, restart/change-difficulty, and anti-reversal all behave exactly as before.

Not verified live (tooling gap): actual pixel rendering, real console-error/network checks, and animated playback require a browser that isn't available here; these were validated by code inspection instead.
