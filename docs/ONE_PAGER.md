# Knight to Sky

## Pitch

Knight to Sky is an endless 2D platformer built with C++ and raylib. The player guides a knight between platforms while the world scrolls downward, using manual jumps and collecting score bonuses. There is no finish line: the objective is to survive and increase the run's score. Difficulty stays constant, with new platform arrangements and pickup placements generated throughout each attempt.

## Player Experience

Each run begins with the knight in midair, with two jumps available. The first jump activates downward scrolling and time-based scoring. From that point, the player must keep finding landing opportunities while deciding whether to pursue a pickup or preserve a safer route.

Landing restores both jumps. The second jump can extend an ascent or interrupt a fall, giving the player another opportunity to adjust a trajectory. Horizontal movement remains available in the air, and leaving one side of the screen brings the knight back from the opposite side.

The HUD shows score and remaining durations for timed effects. Brief control instructions introduce movement and jumping, while pickup messages identify collected rewards or penalties.

## Core Loop

1. Read the next platform positions and nearby pickups.
2. Move horizontally and time the first and second jumps.
3. Land to restore both jumps and prepare the next move.
4. Collect coins and bonuses while avoiding harmful pickups.
5. Continue surviving to earn points until a fall ends the run.
6. Review the final score, press Enter to return to the opening screen, then press Enter again to begin a new attempt.

## Key Mechanics

**Movement and jumping:** A/D or the arrow keys move the knight; Space triggers a jump. Two jumps are available between landings. Platforms can be passed through from below and landed on while descending.

**Three platform types:** Solid platforms provide landing surfaces, moving platforms shift horizontally, and breakable platforms fade and disappear after contact. New platforms replace those that descend beyond the play area.

**Score:** Survival awards 10 points per second after the first jump. Coins award 100 points each. A score multiplier doubles both sources for 15 seconds.

**Jump boost:** A spring pickup increases the next successful jump's upward velocity by 60%. The boost is consumed by that jump rather than lasting for a timed interval.

**Penalties:** Slow reduces horizontal movement speed to 60% for 12 seconds. Reverse swaps left and right input for five seconds. Different timed effects can coexist; collecting the same effect refreshes its duration without increasing its strength.

**Constant scrolling:** Platforms descend at 100 pixels per second and pickups at 125. These approved speeds remain fixed throughout a run.

## My Contribution

I worked as a designer and implemented the character and assets. My contribution covers these areas; the platform generation, pickup systems, scoring, audio systems, and scene management described above are presented as project features without assigning their authorship to me.

## Project Status

Repository inspection supports the implemented movement, platform generation, pickup effects, scoring, and scene flow. Endless play, constant difficulty, the midair opening, and current tuning are confirmed design decisions. Jump and velocity debug readouts are excluded from the intended presentation, although rendering code remains. Runtime behaviour, generated-route reachability, collision feel, and audiovisual presentation still require in-game verification. No additional future features are confirmed.
