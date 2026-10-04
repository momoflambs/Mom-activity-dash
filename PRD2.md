# Mom’s Activity Dash — Product Requirements

Reference for the current `game2.html` implementation, updated September 27, 2026.

## Game concept

Mom is the main character. She drives her car to pick up kids and deliver them to their matching activities while avoiding obstacles. Complete all pickups and deliveries within 20 seconds to unlock the next level.

Build the game as one self-contained HTML file named `game2.html`, with inline CSS and JavaScript. It must open directly in a modern browser without a server or downloaded assets. Link to it from the “A Mom’s Life” menu in `index.html`.

## Core gameplay

1. Begin level 1 with three kids waiting at the bottom of the map and an empty car near the center.
2. Give the player 20 seconds of active game time to collect and deliver everyone.
3. Move Mom’s car in four directions, keeping it within the map boundaries.
4. Pick up waiting kids automatically by driving close to them along the bottom curb.
5. Deliver passengers automatically by entering their matching activity’s drop-off zone.
6. Avoid roadwork barriers. Colliding with a barrier blocks movement and temporarily slows the car; drive around it to continue.
7. Complete all deliveries before time expires to display a success screen and unlock the next level.
8. If time expires first, display a retry screen. Retrying resets the same level with a fresh 20 seconds.

There is no life counter or shooting mechanic in this game. Success depends on collecting and delivering every kid before the deadline.

## Kids and pickups

- Show waiting kids along the bottom curb in outfits associated with their activities.
- Use distinct colors and outfit details: a basketball jersey, white taekwondo uniform with a belt, piano outfit with a musical note, swimming outfit, art outfit, and soccer jersey.
- Display an activity icon beside each waiting group to help players identify its destination.
- Kids sharing an activity wait at the same pickup location. Show a representative character and a count when more than one kid is waiting there.
- Driving into pickup range collects all waiting kids at that location automatically.
- The car can carry multiple kids, with no capacity limit.
- Picked-up kids disappear from the waiting area and remain in the car until delivered.
- Display a passenger count above the car.
- Track each kid in the checklist as **Waiting**, **In car**, or **Delivered**.

## Activities and delivery rules

| Activity | Identification |
| --- | --- |
| Basketball | Basketball icon and peach zone |
| Taekwondo | Martial arts uniform icon and green zone |
| Piano | Keyboard icon and lavender zone |
| Swimming | Swimming icon and blue zone |
| Art | Paint palette icon and pink zone |
| Soccer | Soccer ball icon and yellow-green zone |

Start with basketball, taekwondo, and piano. Add swimming at level 2, art at level 3, and soccer at level 4.

Only passengers already in the car can be delivered. Entering an unrelated activity does not drop off the wrong kid or apply a penalty. Deliver all onboard kids matching that zone at once. Mark an activity as delivered only after every kid assigned to it has arrived.
the drop off has to be in sequence of the pick up in order to count, otherwise the drop off won't be able to complete

## Level progression

- Kids per level: `level + 2`.
- Active activity types: `min(6, level + 2)`.
- Assign kids across the active activities in order, repeating activities when there are more kids than activity types.
- Every level starts with a fresh 20-second timer, an empty car, and all kids waiting for pickup.
- The success screen shows the completed level and remaining time. The player selects **Next level** to continue.
- Add roadwork barriers as levels increase, up to the eight positions in the current map.
- After all six activities are unlocked, later levels continue adding kids across those activities. The current implementation does not add further activity types or a final level.

## Controls and game states

| Action | Keyboard | On-screen control |
| --- | --- | --- |
| Drive left | Left arrow / A | Hold Left |
| Drive right | Right arrow / D | Hold Right |
| Drive up | Up arrow / W | Hold Up |
| Drive down | Down arrow / S | Hold Down |
| Pause or resume | P | Pause / Resume |
| Start, retry, or advance | Activate the focused button | Overlay action button |

Support diagonal driving without increasing movement speed. Opposing direction inputs cancel each other. Release movement on key release, pointer release, or pointer cancellation.

- **Ready:** Show instructions and a start button before the timer runs.
- **Playing:** Update movement, pickups, deliveries, obstacles, and remaining time.
- **Paused:** Freeze gameplay and the timer. Preserve passengers and completed deliveries. Clear held movement inputs. Automatically pause when the browser window loses focus.
- **Success:** Freeze gameplay, show remaining time, and offer the next level.
- **Timeout:** Freeze gameplay and offer a retry of the same level.

A retry or new level resets the car position, pickups, deliveries, timer, movement inputs, and obstacle slowdown.

## Visual design and interface

Match the warm “A Mom’s Life” menu theme: cream background, sage green controls, coral car, pastel destinations, rounded panels, and friendly typography.

Use Canvas 2D to draw the map, road markings, colored activity zones, roadwork barriers, waiting kids, and Mom’s car. Label the car **MOM**. Show clear icons and names inside each destination.

Include:

- A link back to `index.html`.
- The title **Mom’s Activity Dash** and a short introduction.
- Current level, remaining seconds, and delivered kids out of the total.
- A timer that changes color when five seconds or less remain.
- A checklist showing each kid’s activity and current status.
- Messages for pickups, successful drop-offs, and obstacles.
- On-screen direction buttons, pause/resume, and start/result overlays.
- Instructions explaining that pickups come before deliveries.

Keep the layout responsive for desktop and narrow screens. Provide accessible control labels, visible keyboard focus, a canvas description, and a status message region. Support device pixel ratio when sizing the canvas.

## Current implementation values

| Setting | Value |
| --- | --- |
| Logical map size | 900 × 570 units |
| Starting car position | x = 450, y = 290 |
| Normal driving speed | 300 units/second |
| Slowed driving speed | 125 units/second |
| Obstacle slowdown | 0.65 seconds, refreshed while blocked |
| Pickup curb | y = 540 |
| Pickup range | Less than 42 horizontal and 27 vertical units from a waiting group |
| Delivery range | Less than 83 horizontal and 63 vertical units from the destination center |
| Time per level | 20 active seconds |
| Starting roadwork barriers | 2 |
| Maximum roadwork barriers | 8 |

Use `requestAnimationFrame` to render. The current implementation caps each simulation step at 0.05 seconds, so the timer follows accumulated simulation time and can run slower than wall-clock time under severe frame drops. Paused time does not count. At zero remaining time, timeout takes precedence over a delivery in the same update.

## Acceptance checklist

- [ ] The landing page links to the playable `game2.html` file.
- [ ] Level 1 starts with three waiting kids, three matching activities, and 20 seconds.
- [ ] Kids wear distinguishable activity outfits and wait at the bottom curb.
- [ ] Driving near a waiting group picks up its kids and updates their checklist status.
- [ ] Visiting a destination before pickup does not deliver a waiting kid.
- [ ] Only onboard kids matching the destination are delivered.
- [ ] Wrong destinations leave passengers in the car.
- [ ] Obstacles block and slow the car while allowing the player to drive around them.
- [ ] Completing every delivery before timeout unlocks the next level.
- [ ] Later levels add kids and unlock activities up to the six available types.
- [ ] Running out of time offers a retry of the same level.
- [ ] Retry and next-level actions reset the timer, car, and all kid states.
- [ ] Pause/resume preserves the run, and losing window focus pauses automatically.
- [ ] Keyboard and on-screen controls work on desktop and narrow layouts.

This checklist describes verification criteria; it does not claim that every browser and device has been tested.
