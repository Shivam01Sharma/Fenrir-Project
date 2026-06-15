# Instructions

This document explains, step by step, how to build the Embergrove Tower Defense
game. Follow the fixed values exactly so the finished game matches the brief. All
numbers below are the source of truth for the build.

## 1. Project Summary

Build a single screen tower defense game that runs in a web browser. Enemies walk
a fixed path from an entry tile to an exit tile. The player places turrets on the
grass tiles beside the path to destroy enemies before they escape. The player wins
by clearing all eight waves with at least one life remaining.

## 2. Technology and Constraints

- Use only HTML, CSS, and JavaScript with the HTML5 Canvas API.
- The game must open by double clicking `index.html` in any modern browser.
- No server, no build step, no package install, no internet connection at runtime.
- Do not use any paid service, backend, database, login, or multiplayer.
- All code and assets stay inside one root folder so the game runs offline.

## 3. Folder Structure

```
Embergrove_Tower_Defense/
  index.html        entry point, loads the canvas and scripts
  styles.css        page and HUD styling
  game.js           game loop, state, and logic (may be split into more files)
  assets/           optional images (logo, background, sprites)
  README.md         how to run the game and the list of controls
```

## 4. Canvas and Grid

- Canvas size is fixed at 960 by 640 pixels.
- The field is a grid of 40 by 40 pixel tiles.
- This gives 24 columns and 16 rows of tiles.
- Tile (column, row) maps to pixels x = column times 40, y = row times 40.
- Path tiles are non buildable. Every other tile is buildable.

## 5. Path Layout

Draw one continuous path through the waypoints below, in tile coordinates. The
path enters at the left edge and exits at the right edge and bends six times.

| Order | Waypoint (col, row) | Turn        |
|-------|---------------------|-------------|
| 1     | (0, 8) entry        | start       |
| 2     | (5, 8)              | turn up     |
| 3     | (5, 3)              | turn right  |
| 4     | (12, 3)             | turn down   |
| 5     | (12, 12)            | turn right  |
| 6     | (18, 12)            | turn up     |
| 7     | (18, 6)             | turn right  |
| 8     | (23, 6) exit        | leave field |

Enemies move from waypoint to waypoint in order. Reaching the exit removes one life.

## 6. Enemies

Every enemy has health, speed in pixels per second, and a gold bounty paid when it
dies. Draw a small health bar above each enemy that shrinks as it takes damage.

| Enemy  | Health | Speed (px/s) | Bounty | Notes              |
|--------|--------|--------------|--------|--------------------|
| Sprite | 30     | 60           | 8      | basic enemy        |
| Wisp   | 20     | 120          | 12     | fast, low health   |
| Brute  | 120    | 40           | 20     | slow, high health  |

## 7. Waves

Provide exactly eight waves. Spawn enemies one at a time with about 0.8 seconds
between spawns. Later waves have more enemies and tougher types than wave 1.

| Wave | Sprites | Wisps | Brutes | Total |
|------|---------|-------|--------|-------|
| 1    | 8       | 0     | 0      | 8     |
| 2    | 12      | 0     | 0      | 12    |
| 3    | 10      | 2     | 0      | 12    |
| 4    | 14      | 0     | 3      | 17    |
| 5    | 10      | 6     | 0      | 16    |
| 6    | 12      | 0     | 5      | 17    |
| 7    | 14      | 8     | 4      | 26    |
| 8    | 16      | 6     | 8      | 30    |

Start the next wave automatically after a 3 second gap, and show the current wave
number as "Wave X / 8" on screen during play.

## 8. Turrets

Provide at least these two turret types. A turret automatically aims at the nearest
enemy inside its range and fires at its fire rate, removing damage from that enemy.

| Turret       | Cost | Range (px) | Fire rate (shots/s) | Damage |
|--------------|------|------------|---------------------|--------|
| Arbor Sentry | 50   | 100        | 3.0                 | 5      |
| Ember Cannon | 120  | 140        | 0.8                 | 30     |

## 9. Upgrades

- The player selects a placed turret and pays an upgrade cost to improve it.
- Arbor Sentry upgrade costs 40 gold: add 3 damage and 20 pixels of range.
- Ember Cannon upgrade costs 90 gold: add 15 damage and 25 pixels of range.
- At least one upgrade level per turret is required.

## 10. Economy and Lives

- The player starts with 100 gold and 20 lives.
- Show current gold and current lives on screen at all times.
- Destroying an enemy adds its bounty to gold.
- Placing a turret subtracts its cost; block placement if gold is too low.
- An enemy that reaches the exit subtracts one life.

## 11. Game States

- Start: a title screen with a clear Start button. The game begins on click.
- Playing: waves run, turrets fire, the HUD shows lives, gold, and wave number.
- Win: shown when all eight waves are cleared with at least one life remaining.
- Lose: shown when lives reach zero. Remaining enemies and waves stop.
- Win and Lose screens show a clear message and a button to restart the game
  without reloading the page.

## 12. Controls

- Click a turret in the build bar to select the type to place.
- Click a buildable grass tile to place the selected turret if gold allows.
- Click an existing turret to select it, then click Upgrade to improve it.
- Click Start to begin and click Restart on the end screen to play again.

## 13. Colors

| Use            | Hex     |
|----------------|---------|
| Background     | 1A1208  |
| Path tiles     | 6B4A2B  |
| Grass tiles    | 2E4A24  |
| Accent and fire| FF7A1A  |

## 14. Build Steps

1. Create the folder structure from section 3 and an empty `index.html`.
2. Add the 960 by 640 canvas and a fixed game loop using requestAnimationFrame.
3. Draw the tile grid, then the path tiles from the waypoints in section 5.
4. Add the HUD showing lives, gold, and the wave counter.
5. Build the enemy system: spawn, move along waypoints, draw health bars.
6. Add life loss when an enemy reaches the exit.
7. Build turret placement on grass tiles, with cost checks and gold deduction.
8. Add turret targeting and firing, and apply damage to enemies.
9. Add gold bounties when enemies die.
10. Add the upgrade action for a selected turret.
11. Build the eight wave schedule from section 7 with the 3 second gap.
12. Add Start, Win, and Lose states with a Restart that needs no page reload.
13. Apply the color palette and write README.md with run steps and controls.

## 15. Acceptance Checklist

- The game opens from `index.html` with no server and no build step.
- The canvas is 960 by 640 with a 24 by 16 tile grid.
- The path matches the waypoints and bends at least three times.
- All eight waves run, growing harder, with a visible "Wave X / 8" counter.
- Lives start at 20, gold starts at 100, and both show on screen at all times.
- At least two turret types can be placed and at least one can be upgraded.
- Turrets auto target and fire; enemies show shrinking health bars.
- A Win screen appears after wave 8 and a Lose screen appears at zero lives.
- Restart works without reloading the page.
- The fixed color palette is used and a README.md is included.

## 16. Out of Scope

Do not add a backend, database, user accounts, payment, online multiplayer, or any
external paid service. Sound effects are optional and not required. Keep everything
inside the root folder so the game runs fully offline.
