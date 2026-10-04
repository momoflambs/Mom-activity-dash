# Mom vs. Gamer

A fast, funny browser arcade game where a kid is trying to enjoy a handheld game while Mom keeps dropping reminders from above. Dodge the lecture bubbles, fire back with three shots, and survive as long as you can.

This project is a self-contained HTML game built with Canvas 2D and designed to run directly in the browser without a server or external assets.

## Overview

In this game, the player controls a kid at the bottom of the living room. Mom stays near the top and launches mood-lifting phrases like:

- HOMEWORK!
- DINNER!
- CLEAN UP!
- PAUSE IT!
- ENOUGH!
- GO OUTSIDE!
- BEDTIME!
- LISTEN!

Each message appears as a drifting bubble. The player can move left and right, fire upward to pop bubbles, and survive as the difficulty increases every 20 seconds.

## Gameplay

- 3 lives
- 3 shots per run
- stage-based difficulty scaling every 20 seconds
- score tracked as "Bubbles Cleared"
- pause, retry, and game-over flow
- responsive controls for keyboard and touch

The objective is simple: keep dodging, keep shooting, and last as long as possible.

## Features

- Self-contained single-file build using HTML, CSS, and JavaScript
- Canvas-rendered living room scene with cartoon characters and effects
- Randomized phrase bubbles with colorful pastel styles
- Shooting mechanic with limited ammunition
- Progressive difficulty with stage descriptions and progress bar
- Lives, status panels, score tracking, and replay flow
- Mobile-friendly controls and compact layout for smaller screens

## Run the game

Open the file directly in a modern browser:

1. Navigate to the project folder.
2. Open `game.html` in your browser.
3. Click "Let's play" or press the start action.

No installation, build step, or network dependency is required.

## Controls

### Keyboard
- Left / A: move left
- Right / D: move right
- Space: fire one shot
- P: pause/resume

### On-screen controls
- Hold Left / Hold Right
- Shoot
- Pause / Resume

## Game rules

- A bubble that hits the player removes one life.
- The player gets a short invincibility flash after taking damage.
- Each projectile can pop at most one bubble.
- Missed shots are consumed and do not refill.
- Stages advance automatically based on active play time.
- The run ends when all lives are gone.

## Files

- `game.html` — playable game
- `index.html` — landing page / menu
- `PRD.md` — detailed product requirements and acceptance criteria

## Project status

This is a browser game prototype and reference implementation designed for quick local play and iterative improvement.

## License

This project is intended for personal and educational use within this workspace. Add your own license if you plan to publish it publicly.
