# Mom vs. Gamer — Product Requirements

Reference reconstructed from the current `game.html` implementation on September 27, 2026. This document describes the existing game and serves as instructions for rebuilding or extending it.

## Concept and deliverable

Build a playful browser arcade game named **Mom vs. Gamer**, under the **My Arcade** brand. A kid plays a handheld game at the bottom of a living room while Mom yells from the top. Her phrases travel toward the kid inside bubbles. The player moves left and right to dodge them and has three shots to pop bubbles.

Deliver the complete game as one self-contained file named `game.html`, with inline HTML, CSS, and JavaScript. It must open directly in a modern browser without installation, a server, or external assets. Draw the room, characters, bubbles, projectiles, and effects using Canvas 2D. `game.html` is the reference implementation; `index.html` is an earlier version.

## Gameplay

- Start at stage 1 with zero points, three lives, and three shots.
- Keep Mom near the top center and the kid near the bottom. The kid moves horizontally only and cannot leave the arena.
- Spawn one phrase bubble at a time near Mom, drifting downward and sideways. Bubbles bounce off the left and right boundaries.
- Select phrases randomly from: `HOMEWORK!`, `DINNER!`, `CLEAN UP!`, `PAUSE IT!`, `ENOUGH!`, `GO OUTSIDE!`, `BEDTIME!`, and `LISTEN!`.
- Give each bubble a translucent oval body, colored outline, highlight, and readable centered phrase. Randomly use peach, lavender, or yellow accents.
- A bubble hitting the kid removes one life and disappears without awarding a point. The kid flashes and receives 1.4 seconds of protection from further hits.
- Reaching zero lives ends the run. Display the total bubbles cleared and stage reached, with a retry button.
- There is no final stage or win screen; survive and score for as long as possible.

## Shooting

- Allow exactly **three shots per run**, separate from the three lives.
- Fire a projectile straight upward from the kid's current position.
- Consume a shot immediately when fired, including when it misses. Holding Space must not repeatedly fire.
- Each projectile can pop at most one bubble. Remove both on impact, show a brief burst effect, and award one point.
- Remove missed projectiles when they leave the top of the arena.
- Disable shooting before the run starts, while paused, after game over, and when no shots remain.
- Stage advancement does not refill ammunition or lives. Restarting restores both to three.

## Stages and scoring

Advance automatically after every **20 seconds of active game time**: stage 2 at 20 seconds, stage 3 at 40 seconds, and so on. No intermission is required. Preserve the score, remaining lives, ammunition, and active bubbles across stage changes.

Increase the speed and frequency of newly spawned bubbles with each stage. Existing bubbles retain their original speed. Show the current stage and a progress bar that fills over each 20-second interval, then resets for the next stage.

Award one point for each bubble that is either popped or passes completely below the arena. Label the score **Bubbles Cleared**. Hits do not award points. Display score with at least three digits and stage with at least two digits.

Use these stage descriptions:

| Stage | Description |
| --- | --- |
| 1 | Just a reminder |
| 2 | Getting serious |
| 3 | Mom means it |
| 4 and higher | Full volume! |

## Controls

| Action | Keyboard | On-screen control |
| --- | --- | --- |
| Move left | Left arrow or A | Hold Left |
| Move right | Right arrow or D | Hold Right |
| Fire one shot | Space | Click or tap Shoot |
| Pause or resume | P | Pause / Resume |
| Start or retry | Activate the focused button | Let's play / Try again |

Support simultaneous movement and shooting. Opposing movement inputs cancel each other. Release movement when its key or pointer is released or canceled.

## Game states

- **Ready:** Show the room behind an instruction overlay explaining dodging, 20-second stages, three shots, and three lives. Wait for the player to start.
- **Playing:** Update movement, bubble spawning, collisions, shots, score, and the stage timer.
- **Paused:** Freeze gameplay and the timer. Show a resume overlay and preserve the current run. Clear held movement inputs. Automatically pause when the browser window loses focus.
- **Game over:** Freeze gameplay, disable shooting and pause, and show results with a retry action.
- **Restart:** Clear bubbles, projectiles, and pop effects; reset score, stage time, and damage protection; restore three lives and three shots; center the kid.

## Screen and presentation

Use a dark navy and purple living-room setting, lime green primary actions, warm pastel bubbles, and simple cartoon characters. Mom has an angry expression and raised arm; the kid wears headphones and holds a controller.

Include:

- My Arcade branding, the title “Mom vs. Gamer.”, and a short playful introduction.
- An arena with room details, Mom above, and the kid below.
- A status indicator for ready, playing, paused, and game over.
- Score, current stage, intensity description, and stage progress.
- Three hearts representing remaining lives.
- A visible “Shots left: N / 3” counter and a Shoot button displaying the remaining count.
- Instructions, pause/resume controls, and start/retry overlays.
- Left, Shoot, and Right buttons below the arena, usable with mouse or touch.

On wider screens, place the statistics beside the arena. On screens at or below 720 CSS pixels wide, stack the arena above compact statistics panels and hide supplemental tips. Scale the canvas for its displayed size and device pixel ratio. Keep visible focus outlines, accessible button names, an arena description, and textual life/ammunition information.

## Current tuning reference

Measurements below use the canvas's logical coordinate system.

| Parameter | Current value |
| --- | --- |
| Logical arena | 720 × 570 |
| Kid starting position | x = 360, y = 490 |
| Kid horizontal bounds | x = 32 through 688 |
| Kid movement speed | 360 units/second |
| Bubble spawn position | x = 360, y = 125 |
| First bubble delay | 0.6 seconds |
| Stage duration | 20 active seconds |
| Stage calculation | `1 + floor(elapsed / 20)` |
| Bubble downward speed | `100 + min(180, stage × 16)` units/second |
| Bubble spawn interval | `max(0.27, 1.1 × 0.84^(stage - 1))` seconds |
| Bubble horizontal speed | `(random target x - 360) / 1.8` units/second |
| Bubble width | Measured phrase width at bold 16px sans-serif + 48 |
| Bubble height | 58 units |
| Projectile speed | 620 units/second upward |
| Damage protection | 1.4 seconds |
| Pop effect duration | 0.35 seconds |

Use `requestAnimationFrame` for rendering. The current implementation caps each simulation step at 0.04 seconds, so stage timing follows accumulated simulation time and can run slower than wall-clock time during severe frame drops. Paused time does not count.

## Acceptance checklist

- [ ] Opening `game.html` directly shows a playable game with no external asset requests.
- [ ] Starting a run gives stage 1, score 0, three lives, and three shots.
- [ ] Keyboard and on-screen controls move the kid left and right within the arena.
- [ ] Readable phrase bubbles descend from Mom and bounce at side boundaries.
- [ ] Surviving 20 active seconds advances a stage and increases new-bubble difficulty.
- [ ] Dodging a bubble off the bottom adds one point.
- [ ] A hit removes one life, followed by temporary flashing protection.
- [ ] Space or Shoot fires upward and consumes exactly one shot per activation.
- [ ] A projectile impact pops one bubble, awards one point, and shows a burst.
- [ ] A fourth shot cannot be fired; missed shots are not refunded.
- [ ] Advancing stages and resuming do not replenish shots or lives.
- [ ] Pausing freezes the timer and action; losing window focus automatically pauses.
- [ ] The third damaging hit ends the run and displays score and stage.
- [ ] Retry resets all run state, including ammunition and lives.
- [ ] The arena, statistics, and touch controls remain usable on narrow screens.

These are reference verification criteria, not a record that every browser or device has been tested.
